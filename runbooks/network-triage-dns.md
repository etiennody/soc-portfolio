# Runbook — Network triage (DNS)

## Goal
Triage DNS-related alerts/events and decide: close, monitor, or escalate.

## Fast filters (Threat Hunting)
- Suricata DNS lab:
  - `rule.groups:suricata AND rule.description:"DNS"`
- Narrow by agent:
  - `rule.groups:suricata AND agent.name:"<asset>"`

## What to capture (sanitized)
- time window (approx)
- asset alias
- pattern count (N events / M minutes)
- suspected domain category (benign / unknown / high-risk)
- decision + rationale

## Escalate if
- high volume across many hosts/users
- external resolver anomalies or rare domains
- repeated NXDOMAIN spikes
- correlated endpoint alerts or suspicious processes
- signs of beaconing (periodic DNS queries)
