# Haben — Unraid Community Apps Templates

Docker-Vorlagen für **Haben** (selbst gehostete Buchhaltung, https://haben.app) zur Einreichung bei Unraid Community Apps.
Die Vorlagen sind unverändert aus `deploy/unraid/` des Haben-Repositories übernommen (https://github.com/Speicherstadt/haben),
nur `TemplateURL` zeigt auf dieses Repository.

## Vorlagen
- `templates/haben-db.xml` — PostgreSQL-Datenbank (zuerst installieren)
- `templates/haben.xml` — App (HTTPS über Reverse Proxy Pflicht, ELSTER nur auf x86_64)
- `templates/haben-backup.xml` — tägliches restic-Backup mit Test-Restore

Alle laufen im Docker-Netzwerk `haben`; Anleitung: https://haben.app/unraid.html

## Einreichung bei Community Apps
Dieses Repository ist einreichfertig: Die TemplateURL-Einträge in `templates/*.xml` zeigen auf
`moccassins/haben-unraid-ca`. Schritt: Auf https://ca.unraid.net/submit das Repository eintragen,
**Validate** und **Scan** ausführen, einreichen.

Für ein eigenes Fork-Repo: Platzhalter ersetzen mit
`sed -i 's|moccassins/haben-unraid-ca|DEINNAME/DEINREPO|g' templates/*.xml`

## Nur lokal benutzen (ohne Einreichung)
Im Unraid-Terminal:
```sh
docker network create haben
mkdir -p /mnt/user/appdata/haben/belege /mnt/user/appdata/haben/eric /mnt/user/appdata/haben/eric-log
chown -R 1000:1000 /mnt/user/appdata/haben/belege /mnt/user/appdata/haben/eric /mnt/user/appdata/haben/eric-log
for t in haben-db haben haben-backup; do
  wget -qO /boot/config/plugins/dockerMan/templates-user/my-$t.xml \
    https://raw.githubusercontent.com/USERNAME/haben-unraid-ca/main/templates/$t.xml
done
```
Danach unter Docker › Add Container die Vorlagen in der Reihenfolge haben-db → haben → haben-backup anlegen.
