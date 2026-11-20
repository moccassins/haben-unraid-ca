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
1. Diesen Ordner als neues, öffentliches GitHub-Repository pushen.
2. In den drei templates/*.xml den Platzhalter `USERNAME/haben-unraid-ca` durch deinen echten `NAME/REPO` ersetzen:
   `sed -i 's|USERNAME/haben-unraid-ca|DEINNAME/DEINREPO|g' templates/*.xml`
3. Auf https://ca.unraid.net/submit das Repository eintragen, **Validate** und **Scan** ausführen, einreichen.

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
