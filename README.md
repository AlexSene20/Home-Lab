# Home Lab Documentation

## Overview
Personal home lab built to develop practical IT and 
networking skills in pursuit of a career in networking.
Currently pursuing an Associates in Computer Technologies.

---

## Hardware

### Network
| Device | Model | Role |
|--------|-------|------|
| Modem/router/AP | xfinity XB7 | WAN/Internet, DHCP, WiFi |
| Switch | TP-Link TL-SG108PE V5.0 | LAN switching |

NOTE: Will upgrade to dedicated modem, AP and install pfSense for routing capabilities on Proxmox VE

### Proxmox Host
| Component | Spec |
|-----------|------|
| Motherboard | Asus B550 |
| GPU | 1660 super |
| CPU | 3700x |
| RAM | 32GB |
| Storage | 1tb and 2tb nvme SSD |
| USB NIC | UGREEN CM275 RTL8156BG 2.5Gb |

---

## IP Address Plan
| Device | IP Address | Type |
|--------|------------|------|
| XB7 | 10.0.0.1 | Static |
| Switch | 10.0.0.2 | Static |
| Proxmox Host | 10.0.0.10 | Static |
| Windows Server | 10.0.0.20 | Static |
| Gaming PC | 10.0.0.102 | Reserved / Static |
| DHCP Range | 10.0.0.100-253 | Dynamic |

---

## Virtual Machines
| VM | OS | Role |
|----|-----|------|
| Windows Server | Server 2022 | Active Directory/DNS |
| Windows Client | Windows 10 | Domain client |

---

# Proxmox configuration

## Repositories
- Disabled the default Proxmox enterprise repository and enabled the no-subscription (community) repository to allow package updates without a paid subscription. The same was done for ceph-squid even though it is not utilized at the moment because I only have one node. After ran command (apt update && apt dist-upgrade) to get the latest packages.

## DNS Settings
- Verified DNS settings, 75.75.75.75 (comcast) may change to 1.1.1.1 (cloudflare) for encrypted queries and privacy.

## Storage
- Boot drive was configured during Proxmox install. Second drive (2TB NVMe) was partitioned and added as directory storage with ext4 for additional VM storage. Note: ext4 does not support Proxmox snapshots

## Security
- Installed and configured fail2ban to protect the Proxmox web interface (port 8006) and SSH against brute force attacks. A filter was created to detect failed Proxmox login attempts via the systemd journal. IPs are banned for 1 hour after 3 failed attempts within a two day window

# VM's / Containers

## Windows Server
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

## Installation
- Installed Windows Server 2022 Standard Evaluation (Desktop Experience)
- Attached VirtIO drivers ISO as secondary CD drive to install for optimized disk and network performance
- Installed QEMU guest agent for Proxmox integration
- Configured static IP 10.0.0.20 outside of DHCP range

## Active Directory & DNS
- Installed Active Directory Domain Services (AD DS) and DNS Server roles from Server Manager
- Promoted server to domain controller
- Created new forest with root domain homelab.local
- DNS Server configured and running on 10.0.0.20
- Verified AD DS and DNS listed and active in Server Manager
- Created organizational units (OU) Users, Groups, Computers

# Next Steps
- create domain user accounts in the Users OU
- created a windows 10 vm and join it to the domain
- test domain login from windows 10 client
- configure group policy
