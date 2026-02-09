# Runbook — Agent Onboarding (Wazuh)

## Objective
Enroll a new endpoint and validate telemetry + one controlled alert.

## Validate manager is up
- `docker compose ps`
- `docker compose exec wazuh.manager /var/ossec/bin/agent_control -lc`

## Enroll steps (generic)
- Install agent
- Configure manager address (1514)
- Enroll/auth (1515)
- Start agent service
- Confirm agent shows Active/Connected in manager + dashboard

## Validation test (controlled)
- Linux: generate sudo auth failures (2 fails) + success (1)
- Confirm events appear in Security events (filter by agent.name)
- Create a sanitized ticket (no raw logs committed)
