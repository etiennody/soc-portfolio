# Incident Ticket

## 0) Metadata
- Ticket ID: LAB-W1-SUDO-001
- Created (UTC): 2026-02
- Analyst: John
- Source: Wazuh (Security events)
- Severity: P3  (lab: downgraded to P4 if confirmed authorized)
- Status: Closed
- Environment: Lab
- Assets impacted: HOST-001 (agent.id=001)

## 1) Executive summary
Wazuh generated an alert for repeated ssh authentication failures.
The activity was reproduced intentionally (3 wrong password attempts) to validate log collection and alerting. The source user has succeed at the third times.
No evidence of compromise beyond the failed ssh attempts. Ticket closed as benign test activity.
Next step: add a short runbook entry + repeat with an SSH failure to validate another detection path.

## 2) Detection details
- Alert / Rule: SSH authentication failure
- Rule ID: 5503
- Rule level: 5
- Timestamp (UTC): 2026-02
- Log source (location): sshd/auth logs
- Confidence: High (controlled test)
- FP/TP: True Positive (benign / lab test)

## 3) Scope & impact
- Host: HOST-001
- User: John
- Command / action: ~3 failed password attempts (time-bounded)
- Data at risk: No
- Business impact: None (lab)

## 4) Evidence & artifacts
### Event (Wazuh)
- Observed in Wazuh Security events:
  - sshd failed password events
  - consistent time proximity (burst pattern)

## 5) Hypotheses
- H1 (benign): User mistyped password / wrong auth method. (Confirmed: lab test)
- H2 (malicious): Password guessing / brute force.
  - What would support H2:
    - High volume across many usernames
    - Source IP external / unusual ASN/geo
    - Followed by a successful login
    - Lateral movement signs or privilege changes

## 6) Containment / remediation
- Actions taken: None (lab validation)
- Production playbook (recommended):
  1) Confirm source (internal vs external) and attempt volume
  2) Check for “success after failures” on same user/host
  3) If external or high-volume:
     - Block/denylist source at firewall/IDS
     - Enable rate limiting (Fail2ban / sshd config)
     - Force password reset / rotate keys
     - Escalate to L2 if success observed or persistence suspected

## 7) Lessons learned / detection improvement
- Correlate: “multiple failed SSH logins” + “successful SSH login” within 10 minutes -> raise severity
- Add thresholding: N failures/minute per source or per host -> alert
- Prefer key-based auth; restrict SSH exposure and enforce MFA where possible
