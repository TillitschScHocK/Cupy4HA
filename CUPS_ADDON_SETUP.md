# CUPS Print Server Setup Guide

## Quick Start

This guide will help you set up the CUPS Print Server addon for Home Assistant.

## Prerequisites

- Home Assistant with Docker support
- At least 300 MB RAM available
- Ports 631/tcp, 631/udp available
- Port 5353/udp for mDNS (optional but recommended)
- Local network with mDNS support

## Installation Steps

### 1. Add Repository

1. Open Home Assistant
2. Go to **Settings** → **Add-ons & automations** → **Add-on store**
3. Click the menu (three dots) in the top right
4. Select **Repositories**
5. Add this URL:
   ```
   https://github.com/TillitschScHocK/Cupy4HA
   ```
6. Click **Create**

### 2. Install the Addon

1. Search for "CUPS Print Server" in the Add-on store
2. Click on the addon
3. Click **Install**
4. Wait for the installation to complete

### 3. Start the Addon

1. Click **Start**
2. Check the logs for the message "CUPS Server is running"
3. The addon should now be running

### 4. Access the Web Interface

Open your browser and navigate to:
```
http://[YOUR_HOMEASSISTANT_IP]:631
```

For example: `http://192.168.1.100:631`

## Adding Printers

### USB Printers

1. Connect the USB printer to your Home Assistant system
2. Open the CUPS web interface
3. Go to **Administration** → **Add Printer**
4. The printer should appear in the list
5. Select it and click **Continue**
6. Choose the appropriate driver
7. Click **Add Printer**

### Network Printers

1. Ensure the printer is on the same network
2. Open the CUPS web interface
3. Go to **Administration** → **Add Printer**
4. The printer should be automatically discovered via mDNS
5. If not, select "Other Network Printers"
6. Enter the printer's IP address or hostname
7. Choose the appropriate driver
8. Click **Add Printer**

### Manual Configuration

1. Go to **Administration** → **Manage Printers**
2. Click on your printer
3. Configure print settings as needed
4. Click **Set as Default Printer** if desired

## Using AirPrint on iOS/macOS

1. Open an app that supports printing (Safari, Mail, Photos, etc.)
2. Tap the Share button or select Print
3. Look for your CUPS server in the printer list
4. Select it and configure print options
5. Tap Print

## Printing from Windows

1. Open **Control Panel** → **Devices and Printers**
2. Click **Add a printer**
3. Select "Add a network, wireless or Bluetooth printer"
4. Enter: `ipp://[YOUR_HOMEASSISTANT_IP]:631/`
5. Follow the wizard

## Printing from Linux

### Using CUPS Client

```bash
lp -h [YOUR_HOMEASSISTANT_IP] -d [PRINTER_NAME] file.pdf
```

### Using `lpstat`

```bash
lpstat -h [YOUR_HOMEASSISTANT_IP] -p
```

## Configuration Options

### Log Level

Choose the verbosity of logging:
- `debug` - Very detailed logging
- `info` - Standard logging (recommended)
- `warning` - Only warnings and errors
- `error` - Only errors

### AirPrint Support

Enable or disable AirPrint support (enabled by default).

### Samba/SMB Sharing

Enable SMB/CIFS printer sharing (experimental, disabled by default).

## Troubleshooting

### Addon Won't Start

1. Check the logs for error messages
2. Ensure Host Network is enabled
3. Verify port 631 is not used by another service

### USB Printer Not Detected

1. Check if the printer is physically connected
2. Verify the addon has privileged access
3. Check if `/dev/bus/usb` is properly mapped
4. Restart the addon

### Network Printer Not Found

1. Ping the printer to verify network connectivity
2. Check if Avahi daemon is running (see logs)
3. Ensure the printer supports mDNS
4. Try adding manually with the printer's IP address

### Web Interface Not Responding

1. Check if the addon is running
2. Verify port 631 is not blocked by firewall
3. Try opening in a different browser
4. Clear browser cache and cookies
5. Restart the addon

## Performance Tips

- Keep only necessary printers configured
- Disable unused services (Samba if not needed)
- Check logs regularly for errors
- Restart the addon if it seems sluggish

## Security Considerations

⚠️ **Important Security Notes:**

- The addon runs in host network mode (required for mDNS)
- Runs with root privileges (required for USB access)
- Only use in trusted local networks
- Do not expose port 631 to the internet
- Use a firewall to restrict access

## Support

For issues or questions, please create an issue on GitHub:
https://github.com/TillitschScHocK/Cupy4HA/issues

## Additional Resources

- [CUPS Official Documentation](https://www.cups.org/)
- [Home Assistant Add-ons Documentation](https://developers.home-assistant.io/docs/add-ons)
- [Avahi mDNS Documentation](https://avahi.org/)
