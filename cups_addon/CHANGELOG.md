# Changelog

## [1.0.1] - 2025-01-18

### Fixed
- Fixed container image naming from `cupy-print-server` to `cups-print-server`
- Fixed addon store recognition by standardizing naming conventions
- Fixed config.yaml version number
- Updated repository metadata for proper visibility in Home Assistant Add-on Store

### Changed
- Renamed repository branding from "Cupy4HA" to "CUPS Print Server" in documentation
- Updated all image references to use correct `cups-print-server` naming
- Improved documentation clarity and consistency

## [1.0.0] - 2025-01-01

### Added
- Initial release of CUPS Print Server addon for Home Assistant
- Full CUPS installation with all common printer drivers
- AirPrint support for iOS/macOS devices
- Automatic network printer discovery via mDNS/Bonjour
- USB printer access with automatic detection
- Web interface for intuitive management
- Persistent configuration storage
- D-Bus integration for advanced printer management
- Multi-architecture support (ARM, ARM64, AMD64, i386)
- Docker support with multiple base images
