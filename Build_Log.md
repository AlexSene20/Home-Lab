## Build Log
 
### Phase 1 - Hardware Acquisition
- Ordered UGREEN CM275 USB NIC (RTL8156BG 2.5Gb)
- Ordered TP-Link TL-SG108PE V5.0 managed PoE switch
### Phase 2 - Network Organization
- Configured XB7 DHCP range to 10.0.0.100-253
- Set Proxmox host static IP to 10.0.0.10
- Set gaming PC static IP to 10.0.0.102
- Updated port forward for Minecraft server (25565)
### Phase 3 - Hardware Setup
- Configured TP-Link switch management IP to 10.0.0.2
- Connected all devices through switch
- Verified internet connectivity through switch
- Plugged UGREEN CM275 USB NIC into Proxmox host
- Verified USB NIC detected by Proxmox
### Phase 4 - Proxmox Configuration
- Disabled enterprise repo, enabled community repo
- Updated all packages
- Added 2TB NVMe as secondary VM storage (ext4)
- Installed and configured fail2ban for brute force protection
- Configured DNS settings
### Phase 5 - Windows Server 2022 VM
- Created VM with UEFI/TPM/q35 configuration
- Installed Windows Server 2022 Standard Evaluation
- Installed VirtIO drivers and QEMU guest agent
- Set static IP 10.0.0.20
- Installed AD DS and DNS Server roles
- Promoted to domain controller
- Created forest homelab.local
- Created OUs: Users, Groups, Computers
- Created test user jdoe
- Configured daily backups retaining last 3
### Phase 6 - Windows 10 Client VM
- Created VM with UEFI/TPM/q35 configuration
- Installed Windows 10 Pro
- Installed VirtIO drivers and QEMU guest agent
- Disabled IPv6 for DNS compatibility
- Joined domain homelab.local
- Renamed to WIN10-CLIENT
- Verified domain login with jdoe
### Phase 7 - pfSense CE Installation
- Downloaded pfSense CE 2.7.2 ISO
- Created pfSense VM (VMID 102)
- Configured VM: 2 cores, 1GB RAM, 20GB disk, q35
- Passed UGREEN CM275 through via USB Vendor/Device ID
- Added second VirtIO network interface for LAN
- Assigned WAN to ue0 and LAN to vtnet0
- Installed using UFS filesystem and GPT partition scheme
- Configured LAN IP and DHCP range
- Completed setup wizard
- Set DNS to 1.1.1.1 and 9.9.9.9
- pfSense dashboard accessible and operational
### Phase 8 - Remote Desktop Setup
- Added jdoe to Remote Desktop Users group via GPO restricted groups
- Troubleshot GPO not applying to WIN10-CLIENT
- Discovered Default Domain Policy was not linked to Homelab OU
- Enabled RDP via registry command on WIN10-CLIENT
- Enabled RDP firewall rule via netsh command
- Verified RDP connection from gaming PC to WIN10-CLIENT using homelab\jdoe credentials
- RDP fully functional
