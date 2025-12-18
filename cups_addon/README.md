# CUPS Print Server Addon für Home Assistant

Ein vollwertiger CUPS-Druckserver für Home Assistant mit automatischer Netzwerkdruckererkennung, AirPrint-Unterstützung und USB-Druckerzugriff.

## Funktionen

- **Vollständige CUPS-Installation**: Alle gängigen Druckertreiber sind enthalten
- **AirPrint-Unterstützung**: Drucker erscheinen automatisch in iOS/macOS
- **Netzwerkdruckererkennung**: mDNS/Bonjour-Discovery für WiFi und LAN-Drucker
- **USB-Druckerzugriff**: Lokale USB-Drucker werden erkannt und unterstützt
- **Web-Interface**: Intuitive Bedienung über http://[HA-IP]:631
- **Persistente Konfiguration**: Druckerkonfiguration bleibt nach Neustart erhalten
- **D-Bus Integration**: Vollständige Druckerverwaltung über standardisierte APIs

## Unterstützte Drucker

### Aus der Box unterstützt
- HP (mit hp-lip Treiber)
- Canon
- Epson
- Brother
- Xerox
- Ricoh
- Samsung
- Kyocera
- Konica Minolta
- OKI
- Und viele weitere...

### Verbindungstypen
- USB
- Ethernet (LAN)
- WiFi
- Bluetooth (über Host)

## Installation

1. **Repository hinzufügen**
   - Gehe zu Home Assistant → Einstellungen → Add-ons
   - Klicke auf "Add-on Store"
   - Füge folgende URL als neues Repository hinzu:
   ```
   https://github.com/TillitschScHocK/Cupy4HA
   ```

2. **Addon installieren**
   - Suche nach "CUPS Print Server"
   - Klicke auf "Installieren"

3. **Konfigurieren**
   - Keine zwingende Konfiguration nötig (Plug & Play)
   - Optional: Log Level anpassen, Samba aktivieren

4. **Starten**
   - Klicke auf "Starten"
   - Warte auf die Meldung "CUPS Server is running"

## Verwendung

### Web-Interface aufrufen

```
http://[HOME_ASSISTANT_IP]:631
```

Zum Beispiel: `http://192.168.1.100:631`

### Drucker hinzufügen

1. **Automatische Erkennung**
   - Drucker erscheinen oft automatisch im Web-Interface
   - Gehe zu "Drucker" und klicke auf den erkannten Drucker

2. **Manuelles Hinzufügen**
   - Gehe zu Admin → Drucker hinzufügen
   - Wähle den Drucker aus der Liste
   - Wähle den passenden Treiber
   - Klicke auf "Drucker hinzufügen"

3. **Netzwerkdrucker**
   - Geben Sie die IP-Adresse des Druckers ein
   - Das System erkennt den Druckertyp automatisch
   - Wählen Sie den passenden Treiber

### Auf iOS/macOS drucken (AirPrint)

1. Der Server erscheint automatisch als Drucker
2. Öffne eine App, die drucken kann (Safari, Mail, Fotos, etc.)
3. Klicke auf "Teilen" oder "Drucken"
4. Der CUPS-Server sollte in der Druckerliste erscheinen
5. Wählen Sie den Drucker und Ihre Optionen
6. Klicken Sie auf "Drucken"

### Über Netzwerk drucken (Linux/Windows)

**Linux (CUPS-Client):**
```bash
lp -h 192.168.1.100 -d Druckername datei.pdf
```

**Windows (Netzwerkdrucker):**
1. Systemsteuerung → Geräte und Drucker
2. Drucker hinzufügen → Netzwerk-, Bluetooth- oder RF-Drucker
3. Geben Sie ein: `ipp://192.168.1.100:631/`
4. Folgen Sie dem Assistenten

## Konfigurationsoptionen

```yaml
log_level: "info"        # debug, info, warning, error
enable_airprint: true    # AirPrint-Unterstützung
enable_samba: false      # Samba/SMB Druckerfreigabe
```

## Debugging

### Logs anschauen

1. Gehe zu Home Assistant → Add-ons → CUPS → Logs
2. Überprüfe die Ausgabe auf Fehler

### USB-Drucker werden nicht erkannt

1. Überprüfe, ob der Drucker physisch verbunden ist
2. Führe im Terminal aus: `lsusb`
3. Überprüfe die CUPS-Logs
4. Starte das Addon neu

### Netzwerkdrucker werden nicht erkannt

1. Überprüfe, ob der Drucker in demselben Netzwerk ist
2. Ping den Drucker: `ping [DRUCKER_IP]`
3. Der Drucker muss sich auf Port 631 oder 9100 bekannt machen
4. Manche Drucker müssen erst aktiviert werden

### CUPS-Interface lädt nicht

1. Überprüfe, ob das Addon läuft
2. Versuche, die Seite zu aktualisieren
3. Prüfe Firewall-Einstellungen
4. Stelle sicher, dass Port 631 nicht blockiert ist

## Persistente Daten

Alle Druckerkonfigurationen werden in `/data/cups/` gespeichert:

```
/data/cups/
├── ppd/              # Druckertreiber (PPD-Dateien)
├── printers.conf     # Druckerkonfiguration
├── classes.conf      # Druckerklassen
└── ssl/              # SSL-Zertifikate
```

Diese Daten bleiben auch nach Neustart oder Update erhalten.

## Performanz und Ressourcen

- **RAM**: 150-300 MB typisch
- **CPU**: Minimal (unter 5% im Idle)
- **Speicher**: ~500 MB für Base-Installation
- **Netzwerk**: Minimal (nur bei Druckaufträgen)

## Bekannte Einschränkungen

1. Einige proprietäre Drucker benötigen spezielle Firmware-Updates
2. Manche HP-Drucker brauchen zusätzliche hp-utils
3. Scan-Funktionen werden nicht unterstützt (nur Drucken)
4. Netzwerk-basierte Sicherheitsfeatures sind minimal

## Tipps & Tricks

### Standard-Drucker setzen

```bash
echo "@default PrinterName" | lpadmin -h localhost
```

### Drucker über SSH konfigurieren

```bash
ssh root@HOMEASSISTANT_IP
docker exec addon_cups_cups lpadmin -p Druckername -E
```

### Drucker aus Home Assistant automatisierung steuern

Du kannst über die Home Assistant API Druckaufträge senden.

## Troubleshooting

| Problem | Lösung |
|---------|--------|
| Addon startet nicht | Logs anschauen, Host-Network-Mode prüfen |
| Keine USB-Drucker erkannt | `privileged: true` und `/dev/bus/usb` müssen gemappt sein |
| mDNS funktioniert nicht | Avahi-Daemon überprüfen (siehe Logs) |
| CUPS-Interface antwortet nicht | Port 631 überprüfen, Addon neu starten |
| Drucker verliert Konfiguration | `/data/cups/` Zugriff überprüfen |

## Support & Kontakt

Bei Problemen bitte ein Issue auf GitHub erstellen:
https://github.com/TillitschScHocK/Cupy4HA/issues

## Lizenz

MIT License - siehe LICENSE Datei

## Credits

Basierend auf CUPS (Common Unix Printing System)
Avahi mDNS Implementation
Home Assistant Community Addons
