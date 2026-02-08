# Incident Ticket

## 0) Metadata
- Ticket ID: LAB-W1-J5-PRIVCHANGE-001
- Created (UTC): 2026-02-08T00:00:00Z
- Analyst: John
- Source: Wazuh (Security events)
- Severity: P3 (lab: downgraded to P4 if confirmed authorized)
- Status: Closed
- Environment: Lab
- Assets impacted: HOST-001 (agent.id=001)

## 1) Executive summary
Wazuh recorded privileged account activity consistent with a local administrative change:
a new user was created and added to a privileged group (sudo/admin).
In this lab scenario, the activity was performed intentionally to validate visibility and triage workflow.
No additional indicators of compromise were observed in the same time window. Ticket closed as authorized lab test.
If this pattern occurs in production without a change request, it should be treated as potential privilege escalation.

## 2) Detection details
- Alert / Rule: Privileged change / user management activity (sanitized)
- Rule ID: 5902
- Rule level: 8
- Timestamp (UTC): 2026-02
- Log source (location): Linux auth/account logs (e.g., auth.log / user management logs)
- Confidence: High (controlled test)
- Classification: True Positive (benign / lab)

## 3) Scope & impact
- Host: HOST-001
- Account involved: john created USER-B and granted admin privileges
- Change observed:
  - New local user created
  - User added to privileged group (sudo/admin)
- Data at risk: No (lab)
- Business impact: None (lab)

## 4) Evidence & artifacts (sanitized)
- Observed in Wazuh Security events within ~2 minutes:
  - User creation event
  - Group membership modification granting privileged access
- Raw logs / full JSON: stored locally only (not committed)

## 5) Hypotheses
- H1 (benign): Authorized admin action / provisioning task. (Confirmed: lab test)
- H2 (malicious): Unauthorized privilege escalation (attacker creating backdoor admin user).
  - What would support H2:
    - Actor account not expected to administer the host
    - No change ticket / outside maintenance window
    - Other anomalies: SSH brute force, suspicious processes, persistence, outbound beacons

## 6) Containment / remediation
- Actions taken: None (lab)
- Production playbook (recommended):
  1) Validate change approval (ticket / change window / requestor)
  2) Identify actor: which account executed the change and from where
  3) If unauthorized:
     - Disable/remove newly created user
     - Remove privileged group membership
     - Rotate/reset credentials for involved accounts
     - Collect forensic artifacts (auth logs, shell history, process list)
     - Escalate to L2/IR

## 7) Lessons learned / detection improvement
- Add correlation rule:
  - “User created” + “Added to sudo/admin” within 10 minutes = raise severity
- Add enrichment in triage:
  - Asset criticality + allowed admin list + change window calendar
- Add SOC checklist entry:
  - Always confirm authorization for privilege-grant events
