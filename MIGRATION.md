# Cupy4HA - Migration Guide

## Was hat sich geändert?

Das Repository wurde komplett vom alten Fritzbox-Addon auf ein neues, modernes **CUPS Print Server Addon** migriert.

## Gelöschte Dateien

folgende Dateien wurden entfernt (als Teil der Bereinigung):

```
✗ fritzmesh_addon/          (komplettes Verzeichnis)
✗ fritzmesh                 (alte Service-Datei)
✗ fritzmesh.service         (alte Systemd-Datei)
✗ FritzMesh.jpg             (Bild)
✗ make_install.sh           (altes Setup-Script)
```

## Neue Inhalte

Neues Verzeichnis mit vollständiger CUPS-Addon-Implementierung:

```
✔ cups_addon/
  ├── config.yaml           HA Addon Konfiguration
  ├── Dockerfile            Container mit CUPS + Treibern
  ├── run.sh                Startup-Script (D-Bus, Avahi, CUPS)
  ├── cupsd.conf            CUPS Server Konfiguration
  ├── build.yaml            Multi-Arch Build Definition
  ├── requirements.txt      Python Dependencies
  ├── .gitignore
  ├── README.md             Benutzerhandbuch
  ├─┠ TECHNICAL.md          Technische Dokumentation
  ├─┠ INTEGRATION.md        HA Integration
  └─┠ CHANGELOG.md

✔ Repository Root
  ├── README.md             Hauptdokumentation (aktualisiert)
  ├── repository.yaml       Addon-Repository Definition
  ├── LICENSE               MIT License
  ├── .gitattributes        Git Konfiguration
  ├── CUPS_ADDON_SETUP.md  Schnelle Anleitung
  └── MIGRATION.md          Diese Datei
```

## Installation im Home Assistant

### Repository hinzufügen

1. Gehe zu: **Home Assistant** → **Einstellungen** → **Add-ons**
2. Klicke auf **Add-on Store** (rechts unten)
3. Klicke auf die **drei Punkte** (rechts oben)
4. Wähle **Repositories**
5. Füge ein: `https://github.com/TillitschScHocK/Cupy4HA`
6. Klicke **Hinzufügen**

### Addon installieren

1. Im Add-on Store nach "CUPS Print Server" suchen
2. Klicke auf das Addon
3. Klicke **Installieren**
4. Warte auf "Addon installiert"

### Addon starten

1. Klicke **Starten**
2. Überwache die **Logs**
3. Suche nach "CUPS Server is running"

### Web-Interface aufrufen

Browser: `http://[HOME_ASSISTANT_IP]:631`

Zum Beispiel: `http://192.168.1.100:631`

## Warum diese Migration?

### Vorher (Fritzbox Addon)

- ❌ Nur Fritz!Box Router Support
- ❌ Limited zur spezifischen Hardware
- ❌ Nur Netzwerk-Mesh-Überwachung
- ❌ Nicht für Allzweck-Einsatz geeignet

### Nachher (CUPS Print Server)

- ✅ Universelle Druckerunterstützung
- ✅ Funktioniert mit allen Druckertypen
- ✅ AirPrint-Integration
- ✅ Netzwerkdrucker-Autodiscovery
- ✅ Plug & Play Erlebnis
- ✅ Production-Ready Code
- ✅ Umfassende Dokumentation

## Erste Schritte

### 1. Drucker erkunden

Nach dem Start sollten automatisch erkannte Drucker im Web-Interface auftauchen:

```
http://[HA-IP]:631 → Drucker
```

### 2. USB-Drucker hinzufügen

```
http://[HA-IP]:631 → Admin → Drucker hinzufügen
```

Wähle:
1. Deinen Drucker aus der Liste
2. Den passenden Treiber
3. Klick: "Drucker hinzufügen"

### 3. Netzwerkdrucker konfigurieren

```
http://[HA-IP]:631 → Admin → Drucker hinzufügen
```

Gib die **IP-Adresse** des Druckers ein:
1. CUPS erkennt den Typ automatisch
2. Wähle den passenden Treiber
3. Speichern

## Troubleshooting

### Problem: Repository wird nicht gefunden

**Lösung:**
- Repository URL exakt eingeben: `https://github.com/TillitschScHocK/Cupy4HA`
- Add-on Store neu laden (Seite aktualisieren)
- Cache löschen und neu versuchen

### Problem: Addon lädt nicht

**Lösung:**
- Logs anschauen auf Fehler
- Disk Space prüfen (mind. 1 GB)
- Docker neu starten: `docker restart`
- Home Assistant neu starten

### Problem: CUPS Web-Interface nicht erreichbar

**Lösung:**
- Port 631 nicht blockiert?
- Addon läuft noch? (Status prüfen)
- Firewall Rules prüfen
- Addon neu starten

### Problem: Drucker werden nicht erkannt

**Lösung:**
- Log Level auf `debug` stellen
- Logs auf Fehler anschauen
- `lsusb` (USB) oder Netzwerk-Scan (LAN) durchführen
- Siehe [README.md](cups_addon/README.md) Troubleshooting

## Dokumentation

| Datei | Zweck |
|-------|-------|
| [README.md](README.md) | Hauptdokumentation |
| [cups_addon/README.md](cups_addon/README.md) | Benutzerhandbuch |
| [cups_addon/TECHNICAL.md](cups_addon/TECHNICAL.md) | Technische Details |
| [cups_addon/INTEGRATION.md](cups_addon/INTEGRATION.md) | HA Integration |
| [CUPS_ADDON_SETUP.md](CUPS_ADDON_SETUP.md) | Schneller Überblick |

## Support

Bei Fragen oder Problemen:

1. **GitHub Issues**: https://github.com/TillitschScHocK/Cupy4HA/issues
2. **Home Assistant Community**: https://community.home-assistant.io/
3. **CUPS Dokumentation**: https://www.cups.org/doc/

## Version

- **Addon Version:** 1.0.0
- **Repository:** Cupy4HA
- **Lizenz:** MIT
- **Status:** Production Ready

---

Viel Erfolg mit deinem neuen CUPS Print Server Addon!
