# Changelog

All notable changes to this Ansible role will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.0] - 2025-10-18

### Added
- macOS support (10.15 through 14.x)
- macOS installation via .pkg installer
- macOS LaunchDaemon service management
- macOS validation tasks
- AGENT_DOWNLOAD.md - detailed documentation on agent download mechanisms
- ARM64 support for macOS

### Changed
- Service management updated to handle macOS LaunchDaemons
- Validation tasks extended for macOS
- Documentation updated with macOS examples

## [1.0.0] - 2025-10-17

### Added
- Initial release of Qualys Cloud Agent role
- Cross-platform support (Linux and Windows)
- Support for multiple Linux distributions:
  - Red Hat Enterprise Linux 7, 8, 9
  - CentOS 7, 8 Stream
  - Ubuntu 18.04, 20.04, 22.04, 24.04
  - Debian 10, 11, 12
  - Amazon Linux 2, 2023
  - SUSE Linux Enterprise Server 12, 15
- Cloud-agnostic deployment (AWS, Azure, GCP, on-premises)
- Support for all Qualys regional platforms
- Comprehensive validation and health checks
- Idempotent operations
- Proxy configuration support
- Automated service management
- Molecule tests for CI/CD integration
- Multiple example playbooks:
  - Basic installation
  - Advanced multi-region deployment
  - AWS-specific deployment
- Comprehensive documentation
- Ansible Vault integration for secure credential management
- Custom tagging support
- Configurable logging levels
- Automatic cleanup of installers
- Windows Firewall rule creation
- SELinux and AppArmor considerations
- Retry mechanisms for downloads and installations
- Timeout configurations
- Force reinstall capability

### Features
-  Fully automated installation and configuration
-  Works with Ansible 2.10+
-  Supports both x86_64 and ARM64 architectures (Linux)
-  Comprehensive error handling
-  Detailed logging and debug output
-  Tag-based execution control
-  Check mode support
-  Handler-based service management
-  OS-specific variable management
-  Modern Ansible best practices (FQCN)
-  Production-ready with extensive testing

### Documentation
- Complete README with usage examples
- Multiple example playbooks
- Troubleshooting guide
- Security considerations
- Contributing guidelines

### Testing
- Molecule framework integration
- Multi-distribution testing
- Idempotence testing
- Verification playbooks

## [Unreleased]

### Planned
- Azure-specific deployment examples
- GCP-specific deployment examples
- Ansible Tower/AWX integration guide
- Advanced configuration management
- Custom certificate support
- Integration with configuration management databases (CMDB)
- Automated vulnerability scanning triggers
- Integration with ServiceNow
- Support for Qualys Policy Compliance
- Enhanced monitoring and alerting
- Automated agent updates
- Bulk deployment optimization



## Version History

- **1.0.0** - Initial production release (2025-10-17)

For detailed information about each version, see the [Releases](https://github.com/yourorg/qualys_cloud_agent/releases) page.
