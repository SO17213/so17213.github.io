# Correlating Okta Logins with Endpoint Activity: A SIEM Engineering Use Case

Identity and endpoint logs usually land in different indexes, with different field names. That split slows every investigation that starts with a suspicious sign in. This use case builds a reliable user to device map in Splunk by joining Okta authentication data with Windows and macOS endpoint data, so an analyst starting from a username reaches the right host in one search.

---

## Why This Matters

- Attackers move from identity to endpoint: a phished or reused password becomes a logon, then lateral movement, then persistence on a host.
- Okta shows who signed in and from where, but not what happened on a machine afterward; endpoint logs show the activity, but tying it back to an authenticated identity takes manual work.
- A maintained user to device map shortens triage, supports compromised account investigations, and gives detection rules a dependable join key.

---

## Log Sources

- **Okta** system logs in `index=okta`, with the authenticated account in `user`
- **Windows** endpoint logs in `index=win`, with logon and host activity carrying `user` and `host`
- **macOS** endpoint logs in `index=jamf`, managed through Jamf, also carrying `user` and `host`

---

## The Mapping Search

This search builds the map. It pulls the three indexes together, labels each event with its platform, and keeps only users who appear in more than one source, meaning an identity record matched at least one endpoint record.

```spl
(index=okta) OR (index=win) OR (index=jamf)
| eval user=lower(user)
| eval platform=case(index="okta", "Identity", index="jamf", "macOS", index="win", "Windows")
| stats dc(index) as source_count, values(platform) as platforms, values(host) as hosts by user
| where source_count>1
```

Run on a schedule and saved as a lookup, the map becomes shared infrastructure: any search can attach a user to their known hosts without rebuilding the join. All three sources are normalized to a common schema first, which is what makes the join hold.

---

## Use in Investigations

- **Suspicious sign in**: start from the Okta event, pull the mapped hosts starting with the most recently used one, then check those hosts for process, file, and network activity in the same window.
- **Compromised account scoping**: list every host the account touched, so containment covers all affected machines, not just the first one found.
- **Lateral movement review**: compare mapped hosts against authentication records to spot an account appearing on machines it has never used before.
- **Access review support**: confirm which devices an elevated account actually uses before approving continued access.

---

## Tuning and Data Quality

- Case differences break joins, so `user` is lowercased on every source before correlation.
- Shared and service accounts pollute the map, so known service accounts are filtered into a separate review list instead of the analyst facing map.
- Clock skew and late arriving data can drop real matches, so the map runs over a rolling window with overlap instead of a fixed slice.
- Coverage is only as good as onboarding. New log sources are added to the map only after their `user` and `host` fields pass validation against the common schema.

---

## Outcome

Analysts start identity investigations with the device already attached, removing the slowest manual step in early triage. The map also supports detections that need host context, such as flagging accounts signing in from hosts they have never used.

---

## MITRE ATT&CK Mapping

- **T1078 Valid Accounts**: spotting valid account use on unusual hosts
- **T1021 Remote Services**: tracing account movement between hosts

---

## Lessons Learned

- Correlation quality is a data engineering problem first. Normalizing identity and host fields pays off more than clever search syntax.
- A saved, scheduled map beats an ad hoc join: faster, consistent, and there when an incident starts at night.
- Mapping is never finished: new sources need onboarding into the same schema or blind spots return.
