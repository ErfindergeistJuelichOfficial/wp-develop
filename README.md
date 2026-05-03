# WordPress Entwicklungsumgebung

Lokale WordPress-Entwicklungsumgebung auf Basis von Podman Compose mit MariaDB, phpMyAdmin und Mailpit.

---

## Starten

Umgebungsvariablen vorbereiten (einmalig):

```bash
cp .env.example .env
```

Umgebung starten:

```bash
podman compose up -d
```

Umgebung stoppen:

```bash
podman compose down
```

Logs anzeigen:

```bash
podman compose logs -f
```

---

## Services

| Service      | URL                   | Port |
|--------------|-----------------------|------|
| WordPress    | http://localhost:8080 | 8080 |
| phpMyAdmin   | http://localhost:8081 | 8081 |
| Mailpit      | http://localhost:8025 | 8025 |

Die Ports können in der `.env`-Datei über `WP_PORT`, `PMA_PORT` und `MAIL_UI_PORT` angepasst werden.

---

## Verzeichnisstruktur

```
.
├── compose.yml             # Podman Compose Konfiguration
├── .env                    # Lokale Umgebungsvariablen (nicht in Git)
├── .env.example            # Vorlage für neue Entwickler
├── .gitignore
├── themes/                 # WordPress-Themes (direkt in Container gemounted)
├── plugins/                # WordPress-Plugins (direkt in Container gemounted)
│   └── dev-mailer/         # Hilfsplugin: leitet Mails an Mailpit weiter
└── uploads/                # Hochgeladene Medien (nicht in Git)
```

---

## Wichtige Details

**Theme- und Plugin-Entwicklung**
Änderungen in `themes/` und `plugins/` sind sofort im Browser sichtbar — kein Neustart des Containers notwendig.

**WordPress Debug-Modus**
`WP_DEBUG` ist standardmäßig aktiviert. Fehler werden in `wp-content/debug.log` geschrieben und nicht im Browser ausgegeben.

**E-Mail-Catching mit Mailpit**
Alle von WordPress versendeten E-Mails werden abgefangen und sind unter http://localhost:8025 einsehbar. Dafür muss das mitgelieferte Plugin **Dev Mailer** im WordPress-Admin unter *Plugins* aktiviert werden. Es werden keine echten E-Mails versendet.

**Datenbankzugang (phpMyAdmin)**
phpMyAdmin ist unter http://localhost:8081 erreichbar und ist bereits mit dem Root-Benutzer vorkonfiguriert. Zugangsdaten sind in der `.env`-Datei hinterlegt.

**Zugangsdaten**
Alle Standard-Zugangsdaten stehen in `.env.example`. Die lokale `.env`-Datei wird nicht in Git eingecheckt und sollte für produktive Umgebungen mit sicheren Passwörtern befüllt werden.
