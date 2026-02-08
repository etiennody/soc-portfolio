# Incident Ticket

## 0) Metadata
- Ticket ID: LAB-W1-SUDO-001
- Created (UTC): 2026-02
- Analyst: John
- Source: Wazuh (Security events)
- Severity: P4
- Status: Closed
- Environment: Lab
- Assets impacted: HOST-001 (agent.id=001)

## 1) Executive summary
Wazuh generated an alert for repeated sudo authentication failures.
The activity was reproduced intentionally (2 wrong password attempts) to validate log collection and alerting. The source user has succeed at the third times.
No evidence of compromise beyond the failed sudo attempts. Ticket closed as benign test activity.
Next step: add a short runbook entry + repeat with an SSH failure to validate another detection path.

## 2) Detection details
- Alert / Rule: Two failed attempts to run sudo
- Rule ID: 5503
- Rule level: 5
- Timestamp (UTC): 2026-02
- Log source (location): journald
- Confidence: High (controlled test)
- FP/TP: True Positive (benign / lab test)

## 3) Scope & impact
- Host: HOST-001
- User: John
- Command / action: /usr/bin/ls /root
- Data at risk: No
- Business impact: None (lab)

## 4) Evidence & artifacts
### Event (Wazuh)
- agent.id: 001
- agent.name: HOST-001
- rule.id: 5503
- rule.level: 5
- location: journald

### Raw log excerpt
> 2026-02 Feb HOST-001 sudo[11629]: pam_unix(sudo:auth): authentication failure; logname= uid=1000 euid=0 tty=/dev/pts/5 ruser=john rhost=  user=john

## 5) Hypotheses
- H1: Legitimate user mistyped password during sudo. (Confirmed: lab test)
- H2: Brute-force or unauthorized privilege escalation attempt. (Not supported: no other indicators)

## 6) Containment / remediation
- Actions taken: None (lab validation)
- Recommendations (if this occurs in prod):
  - Confirm user intent + check for repeated failures over time window
  - If persists: reset credentials / lock account per IAM process
  - Review recent privileged actions for the account and host
  - Escalate to L2 if coupled with suspicious processes/persistence

## 7) Lessons learned / detection improvement
- Add runbook snippet: “Sudo auth failures triage”
- Add correlation idea: sudo failures + subsequent successful sudo + new user/group changes
