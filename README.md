# Qualys Cloud Agent

Ansible role for deploying Qualys Cloud Agent using the agent binary download API.

## API Coverage

Supports all platform and architecture parameters from Qualys Cloud Agent API:
- 15 platform types (WINDOWS, MACOSX, MACOSX_M_1, LINUX, AIX, LINUX_UBUNTU, LINUX_PPC_64_LE, BSD, SOLARIS_X_86, SOLARIS_SPARC, LINUX_COREOS, LINUX_S_390_X, LINUX_UBUNTU_S_390_X, GENTOO_LINUX, LINUX_AWSBR)
- 7 architecture types (X_86_64, ARM_64, X_64, POWER_5, SPARC, PPC_64_LE, S_390_X)

Platform and architecture are automatically detected. See API_REFERENCE.md for complete parameter documentation.

## Requirements

- Ansible 2.10+
- For Windows: `ansible.windows` collection
- Valid Qualys API and activation credentials

## Quick Start

```yaml
- hosts: all
  become: true
  vars:
    qualys_activation_id: "{{ vault_qualys_activation_id }}"
    qualys_customer_id: "{{ vault_qualys_customer_id }}"
    qualys_api_username: "{{ vault_qualys_api_username }}"
    qualys_api_password: "{{ vault_qualys_api_password }}"
    qualys_cloud_platform: "https://qagpublic.qg1.apps.qualys.com"
  roles:
    - qualys_cloud_agent
```

## Supported Platforms

**Linux:** RHEL/CentOS/Rocky/Alma 7-9, Ubuntu 18.04-24.04, Debian 10-12, Amazon Linux 2/2023, SUSE 12-15, CoreOS, Gentoo  
**Architectures:** x86_64, ARM64, ppc64le, s390x  
**macOS:** 11+ (Intel and Apple Silicon)  
**Windows:** Server 2012 R2-2022, 10-11  
**Unix:** AIX, Solaris (x86/SPARC), FreeBSD, OpenBSD

## Variables

### Required
```yaml
qualys_activation_id: ""        # Activation ID
qualys_customer_id: ""           # Customer ID
qualys_api_username: ""          # API username for downloads
qualys_api_password: ""          # API password
qualys_cloud_platform: ""        # Platform URL
```

### Common Optional
```yaml
qualys_agent_state: "present"              # present, latest, absent
qualys_agent_proxy_url: ""                 # HTTP proxy
qualys_agent_log_level: 2                  # 0-3
qualys_agent_service_enabled: true         # Enable on boot
```

See defaults/main.yml for all variables.

## Regional Platforms

```yaml
# US
qualys_cloud_platform: "https://qagpublic.qg1.apps.qualys.com"
# EU
qualys_cloud_platform: "https://qagpublic.qg1.apps.qualys.eu"
# India
qualys_cloud_platform: "https://qagpublic.qg1.apps.qualys.in"
# Canada
qualys_cloud_platform: "https://qagpublic.qg1.apps.qualys.ca"
# UK
qualys_cloud_platform: "https://qagpublic.qg1.apps.qualys.co.uk"
# Australia
qualys_cloud_platform: "https://qagpublic.qg1.apps.qualys.com.au"
```

## Platform Detection

The role automatically detects platform and architecture:

| OS | Platform Parameter | Architecture Parameter |
|----|-------------------|----------------------|
| RHEL/CentOS/Rocky | LINUX | X_86_64, ARM_64, PPC_64_LE, S_390_X |
| Ubuntu | LINUX_UBUNTU | X_86_64, ARM_64 |
| Ubuntu s390x | LINUX_UBUNTU_S_390_X | S_390_X |
| Amazon Linux | LINUX_AWSBR | X_86_64, ARM_64 |
| macOS Intel | MACOSX | X_86_64 |
| macOS Apple Silicon | MACOSX_M_1 | ARM_64 |
| Windows | WINDOWS | X_64 |
| AIX | AIX | POWER_5 |
| Solaris x86 | SOLARIS_X_86 | X_86_64 |
| Solaris SPARC | SOLARIS_SPARC | SPARC |

Manual override available via `qualys_platform_type` and `qualys_architecture` variables.

## Tags

```bash
ansible-playbook playbook.yml --tags linux
ansible-playbook playbook.yml --tags macos
ansible-playbook playbook.yml --tags windows
```

## Troubleshooting

Agent not appearing in portal:
- Wait 10 minutes for initial check-in
- Check service status: `systemctl status qualysagent` (Linux) or Service Manager (Windows)
- Verify connectivity to platform URL
- Check logs: `/var/log/qualys/qualys-cloud-agent.log` (Linux) or `C:\ProgramData\Qualys\QualysAgent\QualysAgent.log` (Windows)

Download failures:
- Verify API credentials have Cloud Agent API access
- Check network connectivity to qualysapi domain
- Ensure platform/architecture combination is valid (see API_REFERENCE.md)

## Documentation

- API_REFERENCE.md - Complete API parameter reference and platform/architecture matrix
- CHANGELOG.md - Version history

## License

MIT
