# Runbook — Alert Triage (SOC L1)

## Objective
Qualify an alert quickly, decide: close / monitor / escalate.

## Timebox
- 10 min initial triage
- 20 min max before escalation (unless severity P1)

## Inputs
- Alert payload (timestamp, rule, host/user, src/dst IP, command line if any)
- Context (asset criticality, user role, change window)

## Step 1 — Sanity checks (2 min)
- Is the timestamp correct (time drift)?
- Is the host known and in-scope?
- Is this a known maintenance/change window?

## Step 2 — Classify (3 min)
- Type: auth / malware / network / policy / vuln / integrity / other
- Likely: False positive / True positive / Unknown
- Severity draft (P1–P4)

## Step 3 — Gather minimum evidence (5 min)
### Auth
- Count failures, source IP diversity, geo/ASN (if available)
- Success after failures? New device?
### Endpoint
- Process tree + command line
- Persistence signals (scheduled task/service/cron)
### Network
- DNS patterns (DGA? NXDOMAIN spike?)
- Beaconing (regular intervals), unusual user-agent

## Step 4 — Decision
### Close as FP if
- Matches approved tool/admin behavior AND scope limited AND evidence clean
### Escalate to L2 if
- Privileged account involved
- Lateral movement suspicion
- Persistence indicators
- Data exfil suspicion
### Containment request if P1/P2
- Isolate host / disable account / block IOC (per process)

## Step 5 — Ticketing (mandatory)
Always leave:
- What you checked
- What you found
- Why you closed/escalated
- Next steps & owner
