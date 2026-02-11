> [!WARNING]
> **This repository has been archived and is now read-only.**  
> No further updates or pull requests will be accepted.


# 🖨️ Cups4HA – CUPS Print Server Add-on for Home Assistant

<div align="center">

[![GitHub Release](https://img.shields.io/github/v/release/TillitschScHocK/Cups4HA?style=for-the-badge&logo=github)](https://github.com/TillitschScHocK/Cups4HA/releases)
[![GitHub Stars](https://img.shields.io/github/stars/TillitschScHocK/Cups4HA?style=for-the-badge&logo=github)](https://github.com/TillitschScHocK/Cups4HA/stargazers)
[![License](https://img.shields.io/github/license/TillitschScHocK/Cups4HA?style=for-the-badge)](LICENSE)
[![Home Assistant](https://img.shields.io/badge/Home%20Assistant-Add--on-41BDF5?style=for-the-badge&logo=home-assistant)](https://www.home-assistant.io/)

**Transform your Home Assistant into a powerful network print server!**

[Features](#-features) • [Installation](#-installation) • [Configuration](#%EF%B8%8F-configuration) • [Documentation](#-documentation) • [Support](#-support)

</div>

---

## 📖 About

Cups4HA brings the power of CUPS (Common Unix Printing System) to your Home Assistant ecosystem. Manage all your printers centrally, share them across your network, and integrate printing capabilities seamlessly into your smart home automation.

Whether you have USB printers, network printers, or AirPrint devices, Cups4HA provides a unified interface to manage them all.

## ✨ Features

### 🎯 Core Capabilities
- **🖨️ Universal Printer Support** – Network, USB, IPP, LPD, and AirPrint compatible
- **🌐 Web Interface** – Full-featured CUPS admin panel at `http://<ha-ip>:631`
- **🔐 Secure Access** – Optional authentication for administrative functions
- **💾 Persistent Storage** – All configurations survive reboots and updates
- **🏗️ Multi-Architecture** – Supports aarch64, amd64, armhf, armv7, i386

### 🚀 Advanced Features
- **⚡ IPP Everywhere** – Modern driverless printing support
- **📱 AirPrint Ready** – Print from iOS and macOS devices
- **🔄 Auto-Discovery** – Detect network printers automatically
- **📊 Job Management** – Monitor and control print jobs in real-time
- **🎨 PPD Support** – Extensive printer driver database

### 🏠 Home Assistant Integration
- **🔌 Seamless Setup** – Install directly from the Add-on Store
- **📦 Backup Compatible** – Included in Home Assistant backups
- **🔔 Automation Ready** – Trigger prints from automations and scripts
- **📈 Resource Efficient** – Minimal resource footprint

## 📋 Supported Architectures

| Architecture | Supported | Badge |
|--------------|-----------|-------|
| aarch64 | ✅ | ![aarch64](https://img.shields.io/badge/aarch64-yes-green.svg) |
| amd64 | ✅ | ![amd64](https://img.shields.io/badge/amd64-yes-green.svg) |
| armhf | ✅ | ![armhf](https://img.shields.io/badge/armhf-yes-green.svg) |
| armv7 | ✅ | ![armv7](https://img.shields.io/badge/armv7-yes-green.svg) |
| i386 | ✅ | ![i386](https://img.shields.io/badge/i386-yes-green.svg) |

## 🚀 Installation

### Method 1: Add-on Store (Recommended)

1. Navigate to **Settings** → **Add-ons** → **Add-on Store** in Home Assistant
2. Click the **⋮** menu (top right) and select **Repositories**
3. Add this repository URL:
   ```
   https://github.com/TillitschScHocK/Cups4HA
   ```
4. Find **Cups4HA** in the store and click **Install**
5. Configure the add-on (see [Configuration](#%EF%B8%8F-configuration))
6. Start the add-on and check the logs

### Method 2: Manual Installation

For advanced users or custom setups:

```bash
# Clone the repository
git clone https://github.com/TillitschScHocK/Cups4HA.git

# Copy to your Home Assistant add-ons directory
scp -r Cups4HA/cups root@<ha-ip>:/addons/
```

Then add the local directory as a repository in Home Assistant.

## ⚙️ Configuration

### Basic Configuration

```yaml
admin_username: printadmin
admin_password: your_secure_password
```

| Option | Type | Required | Default | Description |
|--------|------|----------|---------|-------------|
| `admin_username` | string | Yes | `admin` | Username for CUPS admin interface |
| `admin_password` | password | Yes | `admin` | Secure password for admin access |

### 🔒 Security Best Practices

- Use a **strong, unique password**
- Change default credentials immediately after installation
- Consider network segmentation for printer access
- Regularly update the add-on to get security patches

## 📚 Documentation

### Quick Start Guide

1. **Access the Web Interface**
   - Open your browser and navigate to `http://<ha-ip>:631`
   - Log in with your configured credentials

2. **Add Your First Printer**
   - Click **Administration** → **Add Printer**
   - Select your printer from discovered devices or enter manually
   - Choose the appropriate driver/PPD file
   - Set as default if desired

3. **Configure Network Sharing**
   - Under **Administration** → **Server Settings**
   - Enable "Share printers connected to this system"
   - Allow remote administration if needed

4. **Test Printing**
   - Go to **Printers** → Select your printer
   - Click **Maintenance** → **Print Test Page**

### Printer Connection Methods

#### Network Printers (IPP/HTTP)
```
ipp://<printer-ip>/ipp/print
http://<printer-ip>:631/ipp
```

#### USB Printers
USB printers are automatically detected when connected to the Home Assistant host.

#### Windows Shared Printers (SMB)
```
smb://username:password@hostname/printer_share
```

#### AirPrint Devices
Discovered automatically via mDNS/Bonjour.

### Client Configuration

#### Windows
1. Settings → Devices → Printers & Scanners
2. Add Printer → "The printer that I want isn't listed"
3. Select "Select a shared printer by name"
4. Enter: `http://<ha-ip>:631/printers/<printer-name>`

#### macOS
1. System Preferences → Printers & Scanners
2. Click **+** to add
3. IP tab → Enter `<ha-ip>` with Protocol: **IPP**
4. Queue: `/printers/<printer-name>`

#### Linux
```bash
# Add printer via command line
lpadmin -p <printer-name> -v ipp://<ha-ip>/printers/<printer-name> -E

# Or use CUPS web interface
firefox http://localhost:631
```

#### iOS/iPadOS
AirPrint-enabled automatically. Select printer in any app's print dialog.

## 🛠️ Advanced Usage

### Persistent Data Storage

Cups4HA stores all configuration data in the Home Assistant `/data` directory:

```
/data/cups/
├── config/     # CUPS configuration files
├── cache/      # Temporary cache data
├── logs/       # Print server logs
└── state/      # Printer state information
```

This ensures that:
- ✅ Printer configurations persist across restarts
- ✅ Data is included in Home Assistant backups
- ✅ No reconfiguration needed after updates

### Home Assistant Automation Examples

#### Notify on Print Job Completion
```yaml
# Monitor CUPS logs and send notifications (requires additional setup)
automation:
  - alias: "Print Job Complete"
    trigger:
      - platform: event
        event_type: cups_job_completed
    action:
      - service: notify.mobile_app
        data:
          title: "Print Complete"
          message: "Your document has finished printing"
```

#### Scheduled Reports Printing
```yaml
automation:
  - alias: "Daily Report Print"
    trigger:
      - platform: time
        at: "08:00:00"
    action:
      - service: shell_command.print_report
        data:
          printer: "office_printer"
```

## ❓ FAQ

<details>
<summary><b>Q: Can I use multiple printers?</b></summary>

Yes! Cups4HA supports unlimited printers. Add as many network, USB, or shared printers as you need through the CUPS web interface.
</details>

<details>
<summary><b>Q: Does this work with cheap USB printers?</b></summary>

Absolutely! As long as CUPS has a driver for your printer (most common models are supported), it will work. Check the [OpenPrinting database](https://www.openprinting.org/printers) for compatibility.
</details>

<details>
<summary><b>Q: Will this interfere with my existing CUPS installation?</b></summary>

No, Cups4HA runs in an isolated Docker container. It won't conflict with other CUPS instances on different machines.
</details>

<details>
<summary><b>Q: Can I print from automations?</b></summary>

Yes! You can use shell commands or custom scripts to send print jobs to the CUPS server from Home Assistant automations.
</details>

<details>
<summary><b>Q: What about driver support?</b></summary>

Cups4HA includes common printer drivers. For additional drivers, you can:
- Download PPD files from [OpenPrinting](https://www.openprinting.org/download/PPD/)
- Use IPP Everywhere for modern driverless printers
- Add custom drivers via the CUPS web interface
</details>

## 🐛 Troubleshooting

### Web Interface Not Accessible

**Symptoms:** Cannot reach `http://<ha-ip>:631`

**Solutions:**
- ✅ Verify add-on is running (check Add-ons page)
- ✅ Check add-on logs for errors
- ✅ Ensure port 631 isn't blocked by firewall
- ✅ Try accessing from the same network as Home Assistant
- ✅ Restart the add-on

### Printer Not Detected

**Symptoms:** Printer doesn't appear in device list

**Solutions:**
- ✅ Confirm printer is powered on and network-connected
- ✅ For USB printers: check physical connection to host
- ✅ Verify printer is on the same network/VLAN
- ✅ Check CUPS logs in add-on for detection attempts
- ✅ Manually add printer using its IP address

### Authentication Issues

**Symptoms:** Login fails at CUPS web interface

**Solutions:**
- ✅ Double-check username and password in add-on config
- ✅ Ensure no extra spaces in credentials
- ✅ Restart add-on after changing configuration
- ✅ Clear browser cache and cookies
- ✅ Try incognito/private browsing mode

### Print Jobs Stuck in Queue

**Symptoms:** Jobs show "processing" but don't print

**Solutions:**
- ✅ Check printer is online and has paper/ink
- ✅ Verify correct driver is installed
- ✅ Cancel and resend the job
- ✅ Check CUPS error logs for specific issues
- ✅ Restart printer and CUPS add-on

### Driver Installation

For printers requiring specific drivers:

1. Visit [OpenPrinting PPD Repository](https://www.openprinting.org/download/PPD/)
2. Download the appropriate PPD file for your printer model
3. In CUPS web interface: Administration → Add Printer
4. Select "Provide a PPD file" and upload your downloaded PPD

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

- 🐛 **Report bugs** via [GitHub Issues](https://github.com/TillitschScHocK/Cups4HA/issues)
- 💡 **Suggest features** in [Discussions](https://github.com/TillitschScHocK/Cups4HA/discussions)
- 📝 **Improve documentation** with pull requests
- ⭐ **Star the repository** to show support
- 🔀 **Share with others** who might benefit

### Development

```bash
# Fork and clone the repository
git clone https://github.com/YOUR_USERNAME/Cups4HA.git
cd Cups4HA

# Make your changes
# Test thoroughly

# Submit a pull request
```

## 📄 License

This project is licensed under the **MIT License** – see the [LICENSE](LICENSE) file for details.

## 🙏 Credits

- Originally based on work by [Andrea Restello](https://github.com/arest)
- Maintained and enhanced by [TillitschScHocK](https://github.com/TillitschScHocK)
- Built with ❤️ for the Home Assistant community
- Powered by [CUPS](https://www.cups.org/) – the industry-standard printing system

## 🔗 Links

- [Home Assistant Add-ons Documentation](https://www.home-assistant.io/addons/)
- [CUPS Official Website](https://www.cups.org/)
- [OpenPrinting Project](https://www.openprinting.org/)
- [Report Issues](https://github.com/TillitschScHocK/Cups4HA/issues)
- [Discussions](https://github.com/TillitschScHocK/Cups4HA/discussions)

---

<div align="center">

**Made with ☕ and 🖨️**

If you find this project useful, consider giving it a ⭐!

</div>
