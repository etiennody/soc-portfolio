# Redaction Policy (Portfolio Safe)

## Never commit
- Raw logs: full_log/message, JSON exports, PCAP, EVTX, HAR, dumps
- Real hostnames, usernames, internal IPs/subnets, domains, URLs
- Secrets: tokens, passwords, private keys, cert bundles

## Allowed in public tickets
- Sanitized host/user: HOST-001, John
- Approx timestamps: date
- Counts and patterns: "3 failed attempts in ~2 minutes"
- Rule description (generic) + severity + decision + next steps

## Storage
- Raw evidence is stored locally only under:
  - case-notes/private/
  - evidence/
