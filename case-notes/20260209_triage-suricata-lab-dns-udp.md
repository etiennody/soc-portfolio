# Incident Ticket

## 0) Metadata
- Ticket ID: LAB-W2-J4-SURI-DNS-001
- Created (UTC): 2026-02
- Analyst: John
- Source: Wazuh (Suricata via eve.json)
- Severity: P4 (lab)
- Status: Closed
- Environment: Lab
- Asset: HOST-001 (agent.id=001)

## 1) Executive summary
Suricata-generated DNS traffic alerts were ingested into Wazuh and observed in Threat Hunting.
The activity matches controlled lab traffic (DNS queries/responses on port 53) and is used to validate the network detection pipeline.
No indicators of compromise were observed beyond expected test activity. Ticket closed as benign validation.
In production, repeated DNS anomalies or unusual domains would require enrichment and correlation.

## 2) Detection details
- Category: Network / DNS
- Detection source: Suricata EVE JSON -> Wazuh
- Rule group: suricata
- Rule description: "suricata: Alert - LAB DNS UDP ..." (sanitized)
- Time window: 2026-02
- Event volume: ~N events within ~M minutes (approx)
- Confidence: High (controlled test)
- Classification: True Positive (benign / lab)

## 3) Scope & impact
- Host: HOST-001
- Traffic: DNS UDP/53 (queries and/or responses)
- Parties: internal resolver/public resolver (anonymized)
- Data at risk: None (lab)
- Impact: None

## 4) Evidence (sanitized)
- Wazuh Threat Hunting returned events matching:
  - `rule.groups:suricata`
  - `rule.description:"LAB DNS UDP"`
- Raw evidence stored locally only (not committed).

## 5) Triage notes
- Verified time range and rule group filter in Threat Hunting.
- Verified events are consistent with expected lab DNS activity.
- No correlated alerts indicating follow-on compromise (no privilege changes, no SSH success after failures, no persistence).

## 6) Decision & next steps
- Decision: Close (P4) — authorized lab activity.
- Production next steps (if suspicious):
  - Identify unusual domains / NXDOMAIN spikes
  - Correlate with endpoint alerts, outbound connections, and user context
  - Escalate if beaconing or malware domain patterns are observed

## 7) Redaction
Hostnames, usernames, IPs and raw log lines removed. Raw evidence stored locally only.
