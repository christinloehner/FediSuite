# FediSuite Self-Hosting

This repository is for people who want to run FediSuite themselves with Docker Compose.

Project website: https://www.fedisuite.com

You do not need to build anything locally. You only need Docker, Docker Compose, and a `.env` file with your settings.

## Quick Start

```bash
git clone <your-repo-url>
cd fedisuite
cp .env.example .env
```

Edit `.env` and set at least these values:

- `FEDISUITE_IMAGE`
- `APP_URL`
- `JWT_SECRET`
- `DATABASE_URL`
- `POSTGRES_DB`
- `POSTGRES_USER`
- `POSTGRES_PASSWORD`
- `ADMIN_EMAIL`
- `ADMIN_PASSWORD`
- `SMTP_HOST`
- `SMTP_PORT`
- `SMTP_USER`
- `SMTP_PASS`
- `SMTP_FROM`

`ADMIN_EMAIL` and `ADMIN_PASSWORD` are required on the first start. FediSuite creates the initial admin user from these values. If `ADMIN_PASSWORD` is missing, you will not be able to log in to the app afterward.

SMTP is also required. FediSuite sends emails for user registration, password reset, and historical import related flows after Fediverse accounts are connected. Do not leave the SMTP settings empty.

If you want to prevent new user sign-ups, set `ENABLE_USER_REGISTRATION=false`. The initial admin user from `ADMIN_EMAIL` and `ADMIN_PASSWORD` is still created on first start.

Then start FediSuite:

```bash
docker compose up -d
```

Open `APP_URL` in your browser after the containers are healthy.

## Default Setup

The included [`docker-compose.yml`](https://github.com/christinloehner/fedisuite/blob/main/docker-compose.yml) starts four services:

- `db`: PostgreSQL database
- `app`: frontend and API
- `worker1`: background jobs
- `worker2`: background jobs

The workers handle scheduled posts, refresh jobs, idle reminders, and tips generation.

## Image Tags

The default `.env.example` uses this image tag:

- `christinloehner/fedisuite:latest`

If you want to pin a specific release, replace `latest` with the tag you want to run.

## Important Variables

Required:

- `FEDISUITE_IMAGE`
- `DATABASE_URL`
- `JWT_SECRET`
- `APP_URL`
- `ADMIN_EMAIL`
- `ADMIN_PASSWORD`
- `SMTP_HOST`
- `SMTP_PORT`
- `SMTP_USER`
- `SMTP_PASS`
- `SMTP_FROM`
- `POSTGRES_DB`
- `POSTGRES_USER`
- `POSTGRES_PASSWORD`

Optional:

- `APP_NAME`
- `PUBLIC_SITE_URL`
- `OUTBOUND_HTTP_USER_AGENT`
- `ENABLE_USER_REGISTRATION`

## Traefik

If you use Traefik, you have two options:

- uncomment the example labels in [`docker-compose.yml`](https://github.com/christinloehner/fedisuite/blob/main/docker-compose.yml)
- use [`docker-compose.traefik.example.yml`](https://github.com/christinloehner/fedisuite/blob/main/docker-compose.traefik.example.yml) as a reference

Adjust the hostname, cert resolver, and network name to your environment.

## Update

To update to newer image tags:

```bash
docker compose pull
docker compose up -d
```

## Verify

To check that your configuration is valid before starting:

```bash
cp .env.example .env
docker compose config
```

## License

FediSuite is licensed under the GNU GPL v3.0. See [`LICENSE`](https://github.com/christinloehner/fedisuite/blob/main/LICENSE).
