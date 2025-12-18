# Cupy4HA Repository Struktur

Diese Datei dokumentiert die Repository-Struktur für Entwickler und Contributor.

## Verzeichnisbaum

```
Cupy4HA/
├── .github/
│   └── REPOSITORY_STRUCTURE.md    (diese Datei)
│
├── cups_addon/                 (CUPS Print Server Addon)
│   ├── config.yaml              Home Assistant Addon Config
│   ├── Dockerfile               Container Build Definition
│   ├─┠ run.sh                   Startup Script
│   ├─┠ cupsd.conf               CUPS Server Config
│   ├─┠ build.yaml               Build Configuration
│   ├─┠ requirements.txt         Python Dependencies
│   ├─┠ .gitignore               Git Ignore Rules
│   ├─┠ README.md                User Documentation
│   ├─┠ TECHNICAL.md             Technical Docs
│   ├─┠ INTEGRATION.md           HA Integration Guide
│   └─┠ CHANGELOG.md             Release History
│
├── .gitattributes              Git Attributes
├── README.md                   Main Documentation
├── LICENSE                     MIT License
├── repository.yaml             HA Repository Definition
├── CUPS_ADDON_SETUP.md        Quick Setup Guide
├── MIGRATION.md               Migration from Fritzbox
└── .git/                       Git History
```

## Datei-Descriptions

### Root Level

| Datei | Zweck |
|-------|-------|
| `README.md` | Hauptdokumentation für Repository |
| `repository.yaml` | Home Assistant Repository Definition |
| `LICENSE` | MIT License |
| `.gitattributes` | Git Konfiguration (Line Endings, etc.) |
| `CUPS_ADDON_SETUP.md` | Schneller Überblick über Installation |
| `MIGRATION.md` | Guide von altem Fritzbox-Addon |

### cups_addon/ Directory

#### Core Files

| Datei | Zweck | Größe |
|-------|-------|---------|
| `config.yaml` | Home Assistant Addon Konfiguration | 784 B |
| `Dockerfile` | Docker Container Build | 1.6 KB |
| `run.sh` | Startup-Script (Bash) | 5.5 KB |
| `cupsd.conf` | CUPS Daemon Konfiguration | 3.0 KB |
| `build.yaml` | Multi-Arch Build Definition | 926 B |
| `requirements.txt` | Python Dependencies | 138 B |

#### Documentation Files

| Datei | Zweck |
|-------|-------|
| `README.md` | Complete User Guide (5.7 KB) |
| `TECHNICAL.md` | Architecture & Implementation (9.3 KB) |
| `INTEGRATION.md` | HA Integration Examples (5.5 KB) |
| `CHANGELOG.md` | Version History & Releases (2.6 KB) |
| `.gitignore` | Git Ignore Rules |

## Datei-Rollen

### Installation/Runtime

```
config.yaml     ← Home Assistant liest diese Datei
   ← Definiert Addon-Metadaten, Ports, Privilegien
   ← Gibt "cups" als Slug an

Dockerfile      ← Docker build-Befehl verwendet diese
   ← Baut Container mit CUPS + Dependencies
   ← Basiert auf ghcr.io/hassio-addons/debian-base

run.sh          ← Entrypoint des Containers
   ← Startet D-Bus, Avahi, CUPS
   ← Manage Persistenz zu /data/cups/

cupsd.conf      ← CUPS daemon Konfiguration
   ← Wird beim Start geladen
   ← Definiert Ports, Zugriff, Features
```

### Build Process

```
build.yaml      ← Home Assistant Build Tool
   ← Definiert Multi-Arch Support
   ← Labels für Container Metadata
```

### Dependencies

```
requirements.txt   ← pip install in Dockerfile
   ← Python Packages (minimal)
   ← Meiste Funktionalität durch System-Packages
```

### Configuration Management

```
.gitignore      ← Git Exclusions
   ← Excludes Runtime-Dateien
   ← Excludes /data/ Directory
```

## Build & Deployment Flow

```
1. User klickt "Installieren" in Home Assistant
   ↓
2. HA liest config.yaml
   ↓
3. HA liest build.yaml
   ↓
4. Docker baut Image mit Dockerfile
   ↓
5. run.sh wird als Entrypoint ausgeführt
   ↓
6. D-Bus, Avahi, CUPS werden gestartet
   ↓
7. cupsd.conf wird geladen
   ↓
8. Web-Interface verfügbar auf Port 631
```

## Git Workflow

### Commits

Jeder Commit sollte eine klare, aussagekräftige Nachricht haben:

```bash
git commit -m "cups_addon: Fix CUPS daemon startup issue"
```

### Branch Strategy

- `main` - Production-ready Code
- Feature Branches für neue Features

### .gitignore Rules

Folgende werden ignoriert:
- `/data/` - Persistent storage
- `__pycache__/` - Python cache
- `*.pyc` - Compiled Python
- `.vscode/`, `.idea/` - IDE files
- `*.log` - Log files
- `/var/`, `/tmp/` - Runtime dirs

## Development

### Local Testing

```bash
# Build Docker Image locally
cd cups_addon
docker build -t cupy4ha:latest .

# Run Container
docker run -it --rm \
  --network host \
  --privileged \
  -v $(pwd)/data:/data \
  cupy4ha:latest
```

### File Permissions

```
run.sh              755  (executable)
config.yaml         644
Dockerfile          644
cupsd.conf          644
*.md                644
```

## Documentation Standards

### Markdown

- H1: `#` Repository Title
- H2: `##` Main Sections
- H3: `###` Subsections
- Code Blocks: ```language
- Links: `[text](url)`

### Code Comments

```bash
# Use descriptive comments
# Explain WHY, not WHAT (code shows WHAT)

# D-Bus daemon initialization
# Required for printer discovery and management
dbus-daemon --system --nofork
```

## Versioning

Semantic Versioning: MAJOR.MINOR.PATCH

- MAJOR: Breaking changes
- MINOR: New features (backward compatible)
- PATCH: Bug fixes

Aktuell: `1.0.0` (Production Release)

## Contributing

### Adding Features

1. Create feature branch
2. Make changes
3. Update documentation
4. Test locally
5. Create Pull Request
6. Request review

### Bug Fixes

1. Create bugfix branch
2. Fix issue
3. Test fix
4. Create Pull Request
5. Reference issue number

## Security

- No credentials in code
- No secrets in config files
- Use Home Assistant secrets.yaml

## Performance Targets

- RAM: < 300 MB
- CPU: < 5% idle
- Startup: < 10 seconds
- Web UI: < 1s response time

## Checklist für Release

- [ ] Code reviewed
- [ ] Tests passed
- [ ] Documentation updated
- [ ] CHANGELOG.md updated
- [ ] version in config.yaml updated
- [ ] build.yaml image version updated
- [ ] All commits on main branch
- [ ] Tag created
- [ ] Release notes written

---

Für weitere Fragen oder Verbesserungen:
https://github.com/TillitschScHocK/Cupy4HA/issues
