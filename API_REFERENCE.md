# Qualys Cloud Agent API Reference

## Overview

This role fully supports all platform and architecture parameters defined in the Qualys Cloud Agent API for downloading agent binaries.

## API Endpoint

```
POST <qualys_api_url>/qps/rest/1.0/download/ca/downloadbinary/
```

## Complete Platform Support

### Platform Parameter (Mandatory)

All valid platform values from the Qualys API are supported. Platform values are **case sensitive**.

| Platform Value | Description | Auto-Detection | Package Type |
|----------------|-------------|----------------|--------------|
| **WINDOWS** | Windows Server and Desktop | Windows OS | msi |
| **MACOSX** | macOS Intel (x86_64) | macOS on Intel | pkg |
| **MACOSX_M_1** | macOS Apple Silicon (M1/M2/M3/M4) | macOS on ARM64 | pkg |
| **LINUX** | Generic Linux (RHEL, CentOS, Rocky, Alma, Debian, SUSE) | Most Linux distributions | rpm, deb |
| **LINUX_UBUNTU** | Ubuntu Linux (x86_64, ARM64) | Ubuntu except s390x | deb |
| **LINUX_UBUNTU_S_390_X** | Ubuntu on IBM System z | Ubuntu on s390x | deb |
| **LINUX_AWSBR** | Amazon Linux 2 and 2023 | Amazon Linux | rpm |
| **LINUX_COREOS** | CoreOS Container Linux | CoreOS distribution | rpm |
| **LINUX_PPC_64_LE** | Linux on PowerPC 64-bit LE | ppc64le architecture | rpm |
| **LINUX_S_390_X** | Linux on IBM System z | s390x architecture | rpm |
| **GENTOO_LINUX** | Gentoo Linux | Gentoo distribution | rpm |
| **AIX** | IBM AIX | AIX operating system | aix |
| **BSD** | FreeBSD and OpenBSD | BSD variants | bsd |
| **SOLARIS_X_86** | Solaris on x86 architecture | Solaris x86 | solaris |
| **SOLARIS_SPARC** | Solaris on SPARC architecture | Solaris SPARC | solaris |

### Architecture Parameter (Mandatory)

All valid architecture values from the Qualys API are supported. Architecture values are **case sensitive**.

| Architecture Value | Description | Ansible Facts Mapping |
|--------------------|-------------|----------------------|
| **X_86_64** | x86 64-bit (AMD64/Intel 64) | x86_64, amd64 |
| **ARM_64** | ARM 64-bit (AArch64) | arm64, aarch64 |
| **X_64** | x64 (Windows 64-bit) | Windows x64 |
| **PPC_64_LE** | PowerPC 64-bit Little Endian | ppc64le |
| **S_390_X** | IBM System z (s390x) | s390x |
| **SPARC** | SPARC architecture | sparc |
| **POWER_5** | IBM POWER5 and later | power, AIX |

## API Request Format

### XML Request Body

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ServiceRequest>
  <filters>
    <Criteria field="platform" operator="EQUALS">LINUX_UBUNTU</Criteria>
    <Criteria field="architecture" operator="EQUALS">ARM_64</Criteria>
  </filters>
  <preferences>
    <preference name="packageType" value="deb"/>
  </preferences>
</ServiceRequest>
```

### Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `platform` | String | Yes | Platform type from supported list (case sensitive) |
| `architecture` | String | Yes | Architecture type from supported list (case sensitive) |
| `packageType` | String | No | Package format preference (rpm, deb, pkg, msi, etc.) |

## Automatic Platform Detection

The role automatically detects the correct platform and architecture values based on Ansible facts:

```yaml
# Detection logic in tasks/download.yml
qualys_platform_type: "{{ detected_platform }}"
qualys_architecture: "{{ detected_architecture }}"
```

### Detection Examples

#### Ubuntu 22.04 on ARM64
```yaml
Ansible Facts:
  - system: Linux
  - distribution: Ubuntu
  - architecture: arm64

Detected Values:
  - platform: LINUX_UBUNTU
  - architecture: ARM_64
  - packageType: deb
```

#### Amazon Linux 2023 on x86_64
```yaml
Ansible Facts:
  - system: Linux
  - distribution: Amazon
  - architecture: x86_64

Detected Values:
  - platform: LINUX_AWSBR
  - architecture: X_86_64
  - packageType: rpm
```

#### macOS M2 (Apple Silicon)
```yaml
Ansible Facts:
  - system: Darwin
  - architecture: arm64

Detected Values:
  - platform: MACOSX_M_1
  - architecture: ARM_64
  - packageType: pkg
```

#### RHEL 9 on IBM System z
```yaml
Ansible Facts:
  - system: Linux
  - distribution: RedHat
  - architecture: s390x

Detected Values:
  - platform: LINUX_S_390_X
  - architecture: S_390_X
  - packageType: rpm
```

#### Windows Server 2022
```yaml
Ansible Facts:
  - os_family: Windows
  - architecture: x64

Detected Values:
  - platform: WINDOWS
  - architecture: X_64
  - packageType: msi
```

#### AIX on POWER
```yaml
Ansible Facts:
  - system: AIX
  - architecture: power

Detected Values:
  - platform: AIX
  - architecture: POWER_5
  - packageType: aix
```

## Complete Platform/Architecture Matrix

This matrix shows all valid combinations supported by both the Qualys API and this Ansible role:

| Platform | X_86_64 | ARM_64 | X_64 | PPC_64_LE | S_390_X | SPARC | POWER_5 |
|----------|---------|--------|------|-----------|---------|-------|---------|
| **WINDOWS** | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ |
| **MACOSX** | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| **MACOSX_M_1** | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| **LINUX** | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ | ❌ |
| **LINUX_UBUNTU** | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| **LINUX_UBUNTU_S_390_X** | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ |
| **LINUX_AWSBR** | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| **LINUX_COREOS** | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| **LINUX_PPC_64_LE** | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ |
| **LINUX_S_390_X** | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ |
| **GENTOO_LINUX** | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| **AIX** | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |
| **BSD** | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| **SOLARIS_X_86** | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| **SOLARIS_SPARC** | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ |

## API Authentication

### Required Credentials

```yaml
qualys_api_username: "your_api_username"
qualys_api_password: "your_api_password"
```

### Regional API Endpoints

The API URL is automatically derived from the platform URL:

| Region | Platform URL | API URL |
|--------|-------------|---------|
| US Platform 1 | https://qagpublic.qg1.apps.qualys.com | https://qualysapi.qg1.apps.qualys.com |
| US Platform 2 | https://qagpublic.qg2.apps.qualys.com | https://qualysapi.qg2.apps.qualys.com |
| US Platform 3 | https://qagpublic.qg3.apps.qualys.com | https://qualysapi.qg3.apps.qualys.com |
| US Platform 4 | https://qagpublic.qg4.apps.qualys.com | https://qualysapi.qg4.apps.qualys.com |
| EU Platform 1 | https://qagpublic.qg1.apps.qualys.eu | https://qualysapi.qg1.apps.qualys.eu |
| EU Platform 2 | https://qagpublic.qg2.apps.qualys.eu | https://qualysapi.qg2.apps.qualys.eu |
| India | https://qagpublic.qg1.apps.qualys.in | https://qualysapi.qg1.apps.qualys.in |
| Canada | https://qagpublic.qg1.apps.qualys.ca | https://qualysapi.qg1.apps.qualys.ca |
| UK | https://qagpublic.qg1.apps.qualys.co.uk | https://qualysapi.qg1.apps.qualys.co.uk |
| Australia | https://qagpublic.qg1.apps.qualys.com.au | https://qualysapi.qg1.apps.qualys.com.au |

## Response Format

### Success Response (HTTP 200)

Binary file download of the appropriate agent installer.

### Error Responses

| Status Code | Description |
|-------------|-------------|
| 401 | Authentication failed - check credentials |
| 403 | Insufficient permissions - user needs Cloud Agent API access |
| 404 | Platform/architecture combination not found |
| 500 | Internal server error |

## Manual Override

If automatic detection fails, you can manually specify platform and architecture:

```yaml
- name: Manual platform specification
  ansible.builtin.set_fact:
    qualys_platform_type: "LINUX_UBUNTU"
    qualys_architecture: "ARM_64"
```

## Verification

To verify detected values before download:

```yaml
- name: Display detected platform
  ansible.builtin.debug:
    msg:
      - "Platform Type: {{ qualys_platform_type }}"
      - "Architecture: {{ qualys_architecture }}"
      - "Package Type: {{ qualys_package_type }}"
```

## Official Documentation

For the latest API documentation, refer to:
https://docs.qualys.com/en/ca/api/agents/agent_binary_download.htm

## Support Notes

### Platform-Specific Details

**MACOSX_M_1**: Covers all Apple Silicon processors (M1, M2, M3, M4)
**LINUX_AWSBR**: Specific to Amazon Linux 2 and Amazon Linux 2023
**LINUX_COREOS**: For CoreOS Container Linux, discontinued but still supported
**LINUX_PPC_64_LE**: Little Endian PowerPC 64-bit only (not Big Endian)
**POWER_5**: Covers POWER5, POWER6, POWER7, POWER8, POWER9, and later

### Case Sensitivity

Both `platform` and `architecture` parameters are **case sensitive**. The role handles this automatically, but manual overrides must use exact casing:

✅ Correct: `LINUX_UBUNTU`  
❌ Incorrect: `linux_ubuntu`, `Linux_Ubuntu`

✅ Correct: `ARM_64`  
❌ Incorrect: `arm_64`, `Arm_64`
