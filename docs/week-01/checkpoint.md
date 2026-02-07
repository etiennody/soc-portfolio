# Checkpoint - Wazuh up

- Location: ~/projects/wazuh-docker/single-node
- Dashboard URL: https://localhost (or https://localhost:8443)
- Status check: `docker compose ps`
- Stop: `docker compose stop`
- Resume: `docker compose start`
- Full restart (keep data): `docker compose down && docker compose up -d`
- DO NOT: `docker compose down -v`