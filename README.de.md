# FediSuite Self-Hosting

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](LICENSE)
[![Docker Image Version](https://img.shields.io/docker/v/christinloehner/fedisuite/latest)](https://hub.docker.com/r/christinloehner/fedisuite)
[![Docker Image Size](https://img.shields.io/docker/image-size/christinloehner/fedisuite/latest)](https://hub.docker.com/r/christinloehner/fedisuite)
[![Docker Pulls](https://img.shields.io/docker/pulls/christinloehner/fedisuite)](https://hub.docker.com/r/christinloehner/fedisuite)
[![GitHub issues](https://img.shields.io/github/issues/christinloehner/FediSuite-Docker-Image)](https://github.com/christinloehner/FediSuite-Docker-Image/issues)
[![Last Commit](https://img.shields.io/github/last-commit/christinloehner/FediSuite-Docker-Image)](https://github.com/christinloehner/FediSuite-Docker-Image/commits/main)

---

**Dieses Repository richtet sich an alle, die FediSuite selbst mit Docker Compose betreiben möchten.**

Projektwebsite: <https://www.fedisuite.com>

Du musst nichts lokal bauen. Du brauchst nur Docker, Docker Compose und eine `.env`-Datei mit deinen Einstellungen.

## Was du wofür nutzen solltest

FediSuite hat mehrere wichtige öffentliche URLs mit unterschiedlichen Zwecken.

- Projektwebsite / Landingpage:
  https://www.fedisuite.com
- Self-Hosting-Repository:
  https://github.com/christinloehner/FediSuite
- Quellcode, Fehlerberichte und Beiträge:
  https://github.com/christinloehner/FediSuite-Docker-Image
- Veröffentlichtes Docker-Image für Self-Hoster:
  https://hub.docker.com/r/christinloehner/fedisuite

Kurz gesagt:

- Wenn du mehr über FediSuite erfahren möchtest, starte auf der Website.
- Wenn du FediSuite selbst hosten möchtest, nutze dieses Repository mit den Deployment-Dateien.
- Wenn du Fehler melden oder an der Anwendung selbst mitarbeiten möchtest, nutze das `FediSuite-Docker-Image`-Repository.
- Wenn du das fertige Container-Image brauchst, findest du es auf Docker Hub.

## Fehler melden und Mitarbeit

**Wenn du einen Fehler gefunden hast oder mitarbeiten möchtest, meld dich entweder mit einem Bug-Report oder lies die CONTRIBUTING.md hier:**

---> https://github.com/christinloehner/FediSuite-Docker-Image


---

## Schnellstart

```bash
git clone <your-repo-url>
cd fedisuite
cp .env.example .env
```

Bearbeite `.env` und setze mindestens diese Werte:

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

`ADMIN_EMAIL` und `ADMIN_PASSWORD` sind beim ersten Start erforderlich. FediSuite legt den initialen Admin-Benutzer anhand dieser Werte an. Fehlt `ADMIN_PASSWORD`, kannst du dich danach nicht in der App anmelden.

SMTP ist ebenfalls erforderlich. FediSuite verschickt E-Mails für die Benutzerregistrierung, den Passwort-Reset und Abläufe rund um den historischen Import nach dem Verbinden von Fediverse-Konten. Lass die SMTP-Einstellungen nicht leer.

Wenn du verhindern möchtest, dass sich neue Nutzer registrieren können, setze `ENABLE_USER_REGISTRATION=false`. Der initiale Admin-Benutzer aus `ADMIN_EMAIL` und `ADMIN_PASSWORD` wird beim ersten Start trotzdem angelegt.

Dann FediSuite starten:

```bash
docker compose up -d
```

Öffne `APP_URL` im Browser, sobald die Container healthy sind.

## Standard-Setup

Die mitgelieferte [`docker-compose.yml`](https://github.com/christinloehner/fedisuite/blob/main/docker-compose.yml) startet vier Services:

- `db`: PostgreSQL-Datenbank
- `app`: Frontend und API
- `worker1`: Hintergrundjobs
- `worker2`: Hintergrundjobs

Die Worker kümmern sich um geplante Posts, Refresh-Jobs, Inaktivitätserinnerungen und die Generierung von Tipps.

## Image-Tags

Die Standard-`.env.example` verwendet dieses Image-Tag:

- `christinloehner/fedisuite:latest`

Wenn du eine bestimmte Version festnageln möchtest, ersetze `latest` durch den gewünschten Tag.

## Wichtige Variablen

Erforderlich:

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

Wenn du Traefik verwendest, hast du zwei Möglichkeiten:

- Die Beispiel-Labels in der [`docker-compose.yml`](https://github.com/christinloehner/fedisuite/blob/main/docker-compose.yml) auskommentieren
- Die [`docker-compose.traefik.example.yml`](https://github.com/christinloehner/fedisuite/blob/main/docker-compose.traefik.example.yml) als Referenz verwenden

Passe Hostname, Cert-Resolver und Netzwerkname an deine Umgebung an.

## Update

Um auf neuere Image-Tags zu aktualisieren:

```bash
docker compose pull
docker compose up -d
```

## Konfiguration prüfen

Um zu prüfen, ob deine Konfiguration vor dem Start gültig ist:

```bash
cp .env.example .env
docker compose config
```

## Lizenz

FediSuite steht unter der GNU GPL v3.0. Siehe [`LICENSE`](https://github.com/christinloehner/fedisuite/blob/main/LICENSE).
