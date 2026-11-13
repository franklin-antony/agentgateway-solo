# AI Workspace and AI Gateway

This repository contains two Docker Compose distributions:

- **AI Workspace** — the web workspace and its Platform API.
- **AI Gateway** — an Envoy-based gateway and its controller.

Each distribution runs independently and has its own setup and configuration.

## Prerequisites

- Docker Engine 24 or later
- Docker Compose v2
- macOS/Linux: `openssl` (and `htpasswd` or Docker for setup tasks)

The setup scripts also include PowerShell versions for Windows.

## Start the AI Workspace

From the repository root, run:

```bash
cd wso2apip-ai-workspace-1.0.0
./scripts/setup.sh
docker compose up -d
```

The setup script provisions local TLS certificates, signing/encryption keys, and admin credentials. If it prompts you to set a password, save it; if this package has already been set up, use its existing credentials.

Open **https://localhost:9643/ai-workspace** and sign in with the admin credentials. The browser may show a warning because the default certificate is self-signed.

Check service health:

```bash
curl -fk https://localhost:9243/health
curl -fk https://localhost:9643/healthz
```

## Start the AI Gateway

In another terminal, from the repository root:

```bash
cd wso2apip-ai-gateway-1.2.0
./scripts/setup.sh
docker compose up -d
```

The setup script provisions the gateway's TLS certificate, encryption key, and admin credentials. If it prompts you to set a password, save it; if this package has already been set up, use its existing credentials.

Verify the gateway controller:

```bash
curl http://localhost:9094/api/admin/v1/health
```

The gateway accepts HTTP and HTTPS traffic on **localhost:8080** and **localhost:8443**.

> The gateway starts in standalone mode. To connect it to a control plane, configure `APIP_GW_CONTROLLER_CONTROLPLANE_HOST` and `APIP_GW_CONTROLLER_CONTROLPLANE_TOKEN` in `api-platform.env` in the gateway directory before starting it.

## Stop the services

Run this in each distribution directory:

```bash
docker compose down
```

This stops the containers and keeps persisted data. To view logs while the services are running, use `docker compose logs -f`.

## Configuration and help

Edit each distribution's `configs/config.toml` to change service settings. See its README for configuration details, optional features, and production guidance:

- [AI Workspace guide](wso2apip-ai-workspace-1.0.0/README.md)
- [AI Gateway guide](wso2apip-ai-gateway-1.2.0/README.md)
