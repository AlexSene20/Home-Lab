# Home Lab Documentation

## Overview
Personal home lab built to develop practical IT and 
networking skills in pursuit of a career in Information Technology.
Currently pursuing an Associates in Computer Technologies and CompTIA A+ certification.

---

## Hardware

### Network
| Device | Model | Role |
|--------|-------|------|
| Modem/router/AP | xfinity XB7 | WAN/Internet, DHCP, WiFi |
| Switch | TP-Link TL-SG108PE V5.0 | LAN switching |

NOTE: Will upgrade to dedicated modem, AP and pfSense is installed waiting on cutover for routing capabilities on Proxmox VE

### Proxmox Host
| Component | Spec |
|-----------|------|
| Motherboard | Asus B550 |
| GPU | 1660 super |
| CPU | 3700x |
| RAM | 32GB |
| Storage | 1tb and 2tb nvme SSD |
| USB NIC | UGREEN CM275 RTL8156BG 2.5Gb |
| Onboard NIC | 1Gb (LAN bridge) |

---

### IP Address Plan
| Device | IP Address | Type |
|--------|------------|------|
| XB7 | 10.0.0.1 | Static |
| Switch | 10.0.0.2 | Static |
| Proxmox Host | 10.0.0.10 | Static |
| Windows Server | 10.0.0.20 | Static |
| Gaming PC | 10.0.0.102 | Reserved / Static |
| DHCP Range | 10.0.0.100-253 | Dynamic |

**Subnet:** 255.255.255.0
**DNS:** 75.75.75.75 (Comcast)
---

## Virtual Machines
| VM | OS | Role |
|----|-----|------|
| Windows Server | Server 2022 | Active Directory/DNS |
| Windows Client | Windows 10 | Domain client |


### Windows Server VM
| Setting | Value |
|--------|--------|
|VM ID | 100 |
|CPU | 2 cores (host type) |
|RAM | 6144MB (6GB) |
|Disk | 80GB raw on 2TB NVMe |
|Machine | q35 |
|BIOS | OVMF (UEFI) |
|TPM | Enabled |
|Network | vmbr0 VirtIO |
|IP Address | 10.0.0.20 (Static) |
|Role | Active Directory / DNS |
|Notes | UEFI/TPM does not support Proxmox snapshots. Daily backups configured retaining last 3|

### Windows 10 Client VM
| Setting | Value |
|------- |--------|
|VM ID | 101 |
|CPU | 2 cores (host type) |
|RAM | 4096MB (4GB) |
|Disk | 60GB raw on 2TB NVMe|
|Machine | q35|
|BIOS | OVMF (UEFI)|
|TPM | Enabled|
|Network | vmbro0 VirtIO|
|IP Address | DHCP|
|Role | Domain Client|

### pfSense CE
| Setting | Value |
|---------|-------|
| VM ID | 102 |
| CPU | 2 cores |
| RAM | 1024MB (1GB) |
| DISK | 20GB |
| MACHINE | q35 |
| WAN Interface | ue0 (UGREEN CM275 USB NIC) |
| LAN Interface | vtnet0 (VirtIO) |
| Version | 2.7.2-RELEASE |

---

## Proxmox configuration

### Repositories
- Disabled the default Proxmox enterprise repository and enabled the no-subscription (community) repository to allow package updates without a paid subscription. The same was done for ceph-squid even though it is not utilized at the moment because I only have one node. After ran command (apt update && apt dist-upgrade) to get the latest packages.

### DNS Settings
- Verified DNS settings, 75.75.75.75 (comcast) may change to 1.1.1.1 (cloudflare) for encrypted queries and privacy.

### Storage
- Boot drive was configured during Proxmox install. Second drive (2TB NVMe) was partitioned and added as directory storage with ext4 for additional VM storage. Note: ext4 does not support Proxmox snapshots

### Security
- Installed and configured fail2ban to protect the Proxmox web interface (port 8006) and SSH against brute force attacks. A filter was created to detect failed Proxmox login attempts via the systemd journal. IPs are banned for 1 hour after 3 failed attempts within a two day window

## Active Directory & DNS

### Domain Configuration 
| Setting | Value |
|---------|-------|
| Domain | homelab.local |
| Domain Controller | Windows Server 2022 |
| DNS Server | 10.0.0.20 (static) |
| Forest | homelab.local (new forest) |

### Organizational Units
- Users
- Groups
- Computers

### Domain Users
| User | Username | OU |
|------|----------|----|
| John Doe | jdoe | Users |

### Windows 10 Client Setup
- Installed Windows 10 Pro (required for domain join)
- Installed VirtIO for network and disk optimization and QEMU guest agent (Proxmox integration)
- Disabled IPv6 to reslove homelab.local on DNS 10.0.0.20
- Joined domain and renamed computer to WIN10-CLIENT
- Verified domain login with user jdoe

# Next Steps
- Configure Group Policy
- Set up Remote Desktop access to WIN10-CLIENT
- Expore Group Policy Object (GPO)
