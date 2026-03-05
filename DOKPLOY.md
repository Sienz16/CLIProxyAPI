# Dokploy Deployment Guide

This repository includes a Dokploy-ready compose file: `docker-compose.dokploy.yml`.

## Why this compose file

- Builds from your fork source (does not pull a random public image).
- Uses persistent Docker volumes for config, auth tokens, and logs.
- Bootstraps `config.yaml` automatically from `config.example.yaml` on first run.
- Exposes only the main API port (`8317`) by default.

## Deploy in Dokploy

1. Create a new project/application from your GitHub fork.
2. Select compose deployment.
3. Set compose file path to `docker-compose.dokploy.yml`.
4. Set branch (usually `main`).
5. Add environment variables:

   - `CLI_PROXY_PORT=8317`
   - `TZ=UTC`
   - `MANAGEMENT_PASSWORD=<strong-random-password>`

6. Deploy.

## Required post-deploy config

The first run creates a template config file in the `cli-proxy-config` volume at:

- `/CLIProxyAPI/config/config.yaml`

Update it with your real values:

- `api-keys` (replace placeholder values)
- provider credentials/routes you plan to use

Then redeploy/restart the service.

## Security baseline

- Keep only `8317` exposed publicly unless you explicitly need more ports.
- Always set `MANAGEMENT_PASSWORD`.
- Put Dokploy behind HTTPS with a valid domain certificate.
- Rotate management password and API keys if exposed.

## Optional

- Change `CLI_PROXY_PORT` if `8317` conflicts with another service.
- Use external backups for persistent Docker volumes.
