# n8n + PostgreSQL on Podman (supervised)

Runs n8n backed by PostgreSQL under Podman, restarted automatically
(`restart: always` + a systemd unit). Remote clients such as pclip and
agent-zero connect to n8n and launch workflows.

## Setup

```bash
cd podman
cp .env.example .env          # fill in N8N_ENCRYPTION_KEY and POSTGRES_PASSWORD
podman-compose up -d
```

Optional supervision via systemd: see `n8n-stack.service`
(`loginctl enable-linger $USER` for rootless boot start).

## Connecting clients

- Webhooks: `POST ${WEBHOOK_URL}webhook/<path>` (production) from pclip/agent-zero.
- REST API: create an API key in n8n (Settings > n8n API) and send it in the
  `X-N8N-API-KEY` header to `${WEBHOOK_URL}api/v1/...`.
- PostgreSQL is not published to the host; only n8n is reachable (port `N8N_PORT`).
  Put a TLS reverse proxy in front for remote clients.

Keep `N8N_ENCRYPTION_KEY` safe: it is required to decrypt stored credentials.
