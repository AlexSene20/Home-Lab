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
| Modem/router/AP | xfinity XB7 | WAN/Internet |
| Switch | TP-Link TL-SG108PE V5.0 | LAN switching |

### Proxmox Host
| Component | Spec |
|-----------|------|
| Motherboard | Asus B550 |
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
| Gaming PC | 10.0.0.100 | Reserved |
| DHCP Range | 10.0.0.100-253 | Dynamic |

---

## Virtual Machines
| VM | OS | Role |
|----|-----|------|
| Windows Server | Server 2022 | Active Directory/DNS |
| Windows Client | Windows 10 | Domain client |

---
