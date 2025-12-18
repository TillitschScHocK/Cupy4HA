# CUPS Print Server für Home Assistant

[![Home Assistant Add-on Repository](https://img.shields.io/badge/home%20assistant-add%20on%20repository-41BDF5.svg)](https://github.com/TillitschScHocK/Cupy4HA)

## Überblick

Ein vollwertiger CUPS-Druckserver für Home Assistant mit automatischer Netzwerkdruckererkennung, AirPrint-Unterstützung und umfassender USB-Drucker-Kompatibilität.

## Funktionen

- ✅ **Vollständige CUPS-Installation** mit allen gängigen Druckertreibern
- ✅ **AirPrint-Unterstützung** für iOS/macOS Geräte
- ✅ **Automatische Netzwerkdruckererkennung** via mDNS/Bonjour
- ✅ **USB-Druckerzugriff** mit automatischer Erkennung
- ✅ **Web-Interface** für intuitive Verwaltung
- ✅ **Persistente Konfiguration** - Drucker bleiben nach Neustart
- ✅ **Multi-Architektur-Support** (ARM, ARM64, AMD64, i386)
- ✅ **D-Bus Integration** für erweiterte Druckerverwaltung

## Supported Drucker

**Out of the Box unterstützt:**
- HP (LaserJet, OfficeJet, Photosmart)
- Canon (PIXMA, imageCLASS)
- Epson (WorkForce, Expression)
- Brother (HL, MFC)
- Xerox, Ricoh, Samsung, Kyocera, Konica Minolta, OKI und viele mehr

**Verbindungstypen:**
- USB
- Ethernet (LAN)
- WiFi
- Über Home Assistant Host-Netzwerk

## Installation

### 1. Repository hinzufügen

Gehe zu **Home Assistant** → **Einstellungen** → **Add-ons** → **Add-on Store**

Klicke auf die drei Punkte oben rechts und wähle **Repositories**

Füge folgende URL ein:
```
https://github.com/TillitschScHocK/Cupy4HA
```

### 2. Addon installieren

Suche nach "CUPS Print Server" und klicke auf **Installieren**

### 3. Addon starten

Nach der Installation klickst du auf **Starten**

### 4. Web-Interface aufrufen

```
http://[HOME_ASSISTANT_IP]:631
```

Zum Beispiel: `http://192.168.1.100:631`

## Schnellstart

### Drucker hinzufügen

1. Öffne das CUPS Web-Interface auf Port 631
2. Gehe zu **Admin** → **Drucker hinzufügen**
3. Wähle deinen Drucker aus der Liste
4. Wähle den passenden Treiber
5. Klicke auf **Drucker hinzufügen**

### Auf iOS/macOS drucken (AirPrint)

1. Öffne eine App, die drucken kann (Safari, Mail, Fotos, etc.)
2. Klicke auf **Teilen** oder **Drucken**
3. Der CUPS-Server sollte in der Druckerliste erscheinen
4. Wähle den Drucker und klicke auf **Drucken**

### Auf Windows/Linux drucken

**Windows:**
- Systemsteuerung → Geräte und Drucker
- Drucker hinzufügen → Netzwerkdrucker
- URL eingeben: `ipp://[HA-IP]:631/`

**Linux:**
```bash
lp -h [HA-IP] -d PrinterName datei.pdf
```

## Konfiguration

```yaml
log_level: "info"        # debug, info, warning, error
enable_airprint: true    # AirPrint-Unterstützung
enable_samba: false      # Samba/SMB Druckerfreigabe (zukünftig)
```

## Dokumentation

- **[Benutzerhandbuch](cups_addon/README.md)** - Detaillierte Anleitung und FAQ
- **[Technische Dokumentation](cups_addon/TECHNICAL.md)** - Architektur und Konfiguration
- **[Integration Guide](cups_addon/INTEGRATION.md)** - Home Assistant Integration
- **[Changelog](cups_addon/CHANGELOG.md)** - Release Notes
- **[Setup Guide](CUPS_ADDON_SETUP.md)** - Schneller Überblick

## Support & Debugging

### Logs anschauen

Home Assistant → Add-ons → CUPS Print Server → **Logs**

Verstelle den **Log Level** auf `debug` für detaillierte Ausgaben

### Häufige Probleme

**Q: Addon startet nicht**
- Logs anschauen auf Fehler
- Host Network Mode ist aktiviert?
- Port 631 nicht durch anderes Addon blockiert?

**Q: USB-Drucker nicht erkannt**
- `privileged: true` in config.yaml?
- `/dev/bus/usb` Device Mapping aktiv?
- Drucker physisch verbunden?

**Q: Netzwerkdrucker nicht erkannt**
- Drucker im Netzwerk erreichbar? (`ping`)
- Avahi Daemon läuft? (siehe Logs)
- Host Network Mode aktiviert?

**Q: CUPS Web-Interface antwortet nicht**
- Port 631 nicht blockiert?
- Addon läuft noch? (Status prüfen)
- Seite aktualisieren und Cookies löschen

## Ressourcenverbrauch

- **RAM:** 150-300 MB typisch
- **CPU:** <5% im Idle-Zustand
- **Speicher:** ~500 MB für Installation
- **Netzwerk:** Minimal (nur bei Druckaufträgen)

## Sicherheit

⚠️ **Wichtig:**

- Addon läuft im Host-Network-Modus (erforderlich für mDNS)
- Wird mit Root-Rechten ausgeführt (für USB-Zugriff)
- Nur im lokalen, vertrauenswürdigen Netzwerk verwenden
- Port 631 nicht nach außen exposen

## Voraussetzungen

- Home Assistant mit Docker-Unterstützung
- Mindestens 300 MB RAM verfügbar
- Port 631/tcp und 631/udp (CUPS)
- Port 5353/udp (mDNS/Avahi)
- Lokales Netzwerk mit mDNS-Support

## Unterstützte Architekturen

- amd64 (Intel/AMD 64-bit)
- aarch64 (ARM 64-bit, z.B. Raspberry Pi 4)
- armv7 (ARM 32-bit v7)
- armhf (ARM 32-bit Hard Float)
- i386 (Intel 32-bit)

## Versionshistorie

Siehe [CHANGELOG.md](cups_addon/CHANGELOG.md)

## Lizenz

MIT License - siehe [LICENSE](LICENSE)

## Credits

- Basierend auf [CUPS](https://www.cups.org/)
- [Avahi](https://avahi.org/) für mDNS/Bonjour
- [Home Assistant Community Addons](https://github.com/hassio-addons)

## Support

Bei Fragen oder Problemen bitte ein Issue auf GitHub erstellen:

[GitHub Issues](https://github.com/TillitschScHocK/Cupy4HA/issues)

---

**Status:** Production Ready  
**Version:** 1.0.1  
**Last Updated:** 2025-01-18
