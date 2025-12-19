# Cups4HA – CUPS Print Server Add-on for Home Assistant

[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)](https://github.com/TillitschScHocK/Cups4HA)
[![Supports aarch64 Architecture](https://img.shields.io/badge/aarch64-yes-green.svg)](https://github.com/TillitschScHocK/Cups4HA)
[![Supports amd64 Architecture](https://img.shields.io/badge/amd64-yes-green.svg)](https://github.com/TillitschScHocK/Cups4HA)
[![Supports armhf Architecture](https://img.shields.io/badge/armhf-yes-green.svg)](https://github.com/TillitschScHocK/Cups4HA)
[![Supports armv7 Architecture](https://img.shields.io/badge/armv7-yes-green.svg)](https://github.com/TillitschScHocK/Cups4HA)
[![Supports i386 Architecture](https://img.shields.io/badge/i386-yes-green.svg)](https://github.com/TillitschScHocK/Cups4HA)

Cups4HA ist ein Home Assistant Add-on, das einen CUPS (Common Unix Printing System) Printserver bereitstellt, damit Drucker zentral im Smart Home verwaltet und im Netzwerk freigegeben werden können.

## Funktionen

- Netzwerkdruck mit CUPS für dein Home Assistant System
- Weboberfläche unter `http://<ha-ip>:631` zur Druckerverwaltung
- Optionale Authentifizierung für das Admin-Interface
- Unterstützung für Netzwerk- und USB-Drucker
- Schlanke Basis auf Alpine Linux
- Persistente Speicherung der CUPS-Konfiguration unter `/data`

## Installation

### Add-on Repository in Home Assistant einbinden

1. In Home Assistant zu **Einstellungen** → **Add-ons** → **Add-on Store** wechseln.
2. Oben rechts auf das Drei-Punkte-Menü klicken und **Repositories** auswählen.
3. Folgende URL hinzufügen:
   ```
   https://github.com/TillitschScHocK/Cups4HA
   ```
4. Nach dem Hinzufügen nach **Cups4HA** im Add-on Store suchen und das Add-on installieren.

### Manuelle Installation (optional)

1. Repository klonen:
   ```bash
   git clone https://github.com/TillitschScHocK/Cups4HA.git
   ```
2. Den `cups` Ordner auf dein Home Assistant System kopieren, zum Beispiel:
   ```bash
   scp -r Cups4HA/cups root@<ha-ip>:/addons/
   ```
3. In Home Assistant unter **Einstellungen** → **Add-ons** → **Add-on Store** das Drei-Punkte-Menü öffnen und das lokale Verzeichnis als Repository hinzufügen (je nach Setup z. B. `/addons`).
4. Danach sollte **Cups4HA** als Add-on im Store erscheinen und kann installiert werden.

## Konfiguration

Beispielkonfiguration des Add-ons in Home Assistant:

```yaml
admin_username: printadmin
admin_password: dein_sicheres_passwort
```

- **admin_username**: Benutzername für das CUPS Admin-Interface
- **admin_password**: Passwort für das CUPS Admin-Interface

Nach dem Speichern der Konfiguration:

1. Add-on auf der Infoseite starten.
2. Im **Log** prüfen, ob der Container fehlerfrei läuft.
3. Die CUPS Weboberfläche unter `http://<ha-ip>:631` aufrufen.

## Nutzung

### Zugriff auf die Weboberfläche

- Browser öffnen und `http://<ha-ip>:631` aufrufen.
- Mit den konfigurierten Zugangsdaten am CUPS Admin-Interface anmelden (falls aktiviert).

### Drucker hinzufügen

1. In CUPS auf **Administration** gehen.
2. **Add Printer** auswählen und den Anweisungen folgen.
3. Den passenden Treiber für dein Druckermodell wählen.

### Vom Client drucken

- Auf PCs, Laptops oder anderen Geräten den Drucker mit der URL deines Home Assistant Systems (`ipp://<ha-ip>/printers/<druckername>` bzw. `http://<ha-ip>:631`) einrichten.

## Unterstützte Druckertypen

- Netzwerkdrucker (z. B. IPP, LPD)
- USB-Drucker am Home Assistant Host
- Freigegebene Windows-Drucker (Samba)
- AirPrint-fähige Geräte

## Fehlerbehebung

### Weboberfläche nicht erreichbar

- Prüfen, ob das Add-on läuft und keine Fehler im Log angezeigt werden.
- Sicherstellen, dass Port 631 im Netzwerk nicht blockiert wird.
- Netzwerk- und Firewallregeln des Home Assistant Hosts prüfen.

### Drucker wird nicht erkannt

- Prüfen, ob der Drucker eingeschaltet und im Netzwerk erreichbar ist.
- Bei USB-Druckern kontrollieren, ob das Gerät korrekt an den Host angebunden ist und ggf. USB-Passthrough für den Container konfiguriert werden muss.
- CUPS-Logs im Add-on Logfenster prüfen.

### Probleme mit Treibern

- Für viele Modelle finden sich PPDs und Treiber unter:
  - https://www.openprinting.org/download/PPD/
  - https://www.openprinting.org/drivers/

### Authentifizierungsprobleme

- Benutzername und Passwort mit der Add-on Konfiguration abgleichen.
- Bei vergessenem Passwort die Add-on Konfiguration anpassen und das Add-on neu starten.

## Datenpersistenz

Cups4HA speichert alle relevanten CUPS-Daten im Home Assistant `/data` Verzeichnis, insbesondere:

- `/data/cups/config` für Konfigurationsdateien
- `/data/cups/cache` für Cache-Daten
- `/data/cups/logs` für Logfiles
- `/data/cups/state` für Statusinformationen

Damit bleiben Drucker, Einstellungen und Logs über Neustarts, Systemreboots und Add-on Updates hinweg erhalten und sind in Home Assistant Backups enthalten.

## Lizenz und Credits

- Ursprünglich basierend auf der Arbeit von [Andrea Restello](https://github.com/arest) und dessen CUPS Add-on.
- Cups4HA wird von **TillitschScHocK** gepflegt: https://github.com/TillitschScHocK
- Lizenz: MIT License (siehe `LICENSE`).
