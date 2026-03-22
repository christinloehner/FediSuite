# FediSuite Self-Hosting

Dieses Repository richtet sich an alle, die FediSuite mit Docker Compose selbst betreiben möchten.

Projektwebseite: https://www.fedisuite.com

Es muss nichts lokal gebaut werden. Du benötigst nur Docker, Docker Compose und eine `.env`-Datei mit deinen Einstellungen.

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

`ADMIN_EMAIL` und `ADMIN_PASSWORD` sind beim ersten Start erforderlich. FediSuite erstellt den initialen Admin-Benutzer aus diesen Werten. Wenn `ADMIN_PASSWORD` fehlt, ist danach keine Anmeldung in der App möglich.

SMTP ist ebenfalls erforderlich. FediSuite versendet E-Mails für Benutzerregistrierung, Passwort-Zurücksetzen und historische Import-Abläufe, nachdem Fediverse-Konten verbunden wurden. Lass die SMTP-Einstellungen nicht leer.

Wenn du neue Benutzerregistrierungen verhindern möchtest, setze `ENABLE_USER_REGISTRATION=false`. Der initiale Admin-Benutzer aus `ADMIN_EMAIL` und `ADMIN_PASSWORD` wird beim ersten Start trotzdem angelegt.

Starte FediSuite anschließend:

```bash
docker compose up -d
```

Öffne `APP_URL` in deinem Browser, sobald die Container bereit sind.

## Standard-Setup

Die enthaltene [`docker-compose.yml`](https://github.com/christinloehner/fedisuite/blob/main/docker-compose.yml) startet vier Services:

- `db`: PostgreSQL-Datenbank
- `app`: Frontend und API
- `worker1`: Hintergrundjobs
- `worker2`: Hintergrundjobs

Die Worker übernehmen geplante Beiträge, Aktualisierungs-Jobs, Inaktivitätserinnerungen und die Generierung von Tipps.

## Image-Tags

Die Standard-`.env.example` verwendet diesen Image-Tag:

- `christinloehner/fedisuite:latest`

Wenn du einen bestimmten Release festlegen möchtest, ersetze `latest` durch den gewünschten Tag.

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

- Kommentiere die Beispiel-Labels in der [`docker-compose.yml`](https://github.com/christinloehner/fedisuite/blob/main/docker-compose.yml) ein
- Nutze [`docker-compose.traefik.example.yml`](https://github.com/christinloehner/fedisuite/blob/main/docker-compose.traefik.example.yml) als Referenz

Passe Hostname, Cert-Resolver und Netzwerkname an deine Umgebung an.

## Update

Um auf neuere Image-Tags zu aktualisieren:

```bash
docker compose pull
docker compose up -d
```

## Überprüfen

Um die Konfiguration vor dem Start auf Gültigkeit zu prüfen:

```bash
cp .env.example .env
docker compose config
```

## Lizenz

FediSuite steht unter der GNU GPL v3.0. Siehe [`LICENSE`](https://github.com/christinloehner/fedisuite/blob/main/LICENSE).
