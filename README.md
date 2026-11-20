# Haben — Unraid Community Apps Templates

Docker-Vorlagen für **Haben** (selbst gehostete Buchhaltung, https://haben.app) zur Einreichung bei Unraid Community Apps.
Die Vorlagen sind unverändert aus `deploy/unraid/` des Haben-Repositories übernommen (https://github.com/Speicherstadt/haben),
nur `TemplateURL` zeigt auf dieses Repository.

## Vorlagen
- `templates/haben-db.xml` — PostgreSQL-Datenbank (zuerst installieren)
- `templates/haben.xml` — App (HTTPS über Reverse Proxy Pflicht, ELSTER nur auf x86_64)
- `templates/haben-backup.xml` — tägliches restic-Backup mit Test-Restore

Alle laufen im Docker-Netzwerk `haben`; Anleitung: https://haben.app/unraid.html

## Eigene PostgreSQL nutzen (statt des mitgelieferten Datenbank-Containers)

Die Vorlagen sind out-of-the-box auf die drei mitgelieferten Container ausgelegt,
brauchen die mitgelieferte Datenbank aber nicht: `DATABASE_URL` ist ein ganz normales
Konfigurationsfeld. Wer bereits einen PostgreSQL-Server betreibt, laesst `haben-db`
weg und biegt beide Container in zwei Schritten um:

1. **haben**: im Add-Container-Dialog Netzwerk auf `bridge` aendern und
   `DATABASE_URL` auf den eigenen Server zeigen lassen, z. B.
   `postgres://haben:PASSWORT@IP-DES-SERVERS:5432/haben` (Rolle und Datenbank
   `haben` vorher anlegen).
2. **haben-backup**: `PGHOST` auf die IP des eigenen Servers setzen (dasselbe
   Passwort wie in `DATABASE_URL`). Der Backup-Container macht `pg_dump` gegen
   beliebige erreichbare PostgreSQL-Server — die Backup- und Test-Restore-Funktion
   bleibt vollstaendig erhalten.

Ohne eigenen Server: unveraendert installieren, nichts weiter zu tun.
