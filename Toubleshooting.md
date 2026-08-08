## Troubleshooting Log
 
### Issue 1 - Switch Not Passing Internet
**Problem:** After connecting switch between XB7 and devices internet stopped working.  
**Cause:** Default VLAN configuration on TP-Link TL-SG108PE blocking traffic between ports.  
**Solution:** Accessed switch management UI and corrected port VLAN settings.
 
### Issue 2 - Gaming PC IP Conflict with XB7
**Problem:** XB7 would not allow IP reservations outside of DHCP range.  
**Cause:** XB7 only permits static reservations within its configured DHCP range.  
**Solution:** Adjusted DHCP range and gaming PC static IP to align within XB7 limitations temporarily until pfSense cutover.
 
### Issue 3 - Netgate Installer Requiring Internet
**Problem:** Netgate v1.2 installer showed "Cannot reach Netgate Servers" and would not proceed.  
**Cause:** New Netgate installer requires active WAN connection to verify with Netgate servers.  
**Solution:** Switched to pfSense CE 2.7.2 community edition which does not require Netgate server verification.
 
### Issue 4 - pfSense LAN Conflict with XB7
**Problem:** pfSense LAN set to 10.0.0.1 conflicted with XB7 gateway also at 10.0.0.1.  
**Cause:** Both XB7 and pfSense using same gateway IP while running simultaneously.  
**Solution:** Temporarily set pfSense LAN to 10.0.0.30 until MB8611 modem arrives and XB7 is retired.
 
### Issue 5 - Proxmox Console Access During pfSense Setup
**Problem:** Needed to interact with pfSense console while swapping ethernet cables losing Proxmox web UI access.  
**Cause:** Single ethernet cable serving both Proxmox management and pfSense WAN.  
**Solution:** Ordered spare ethernet cable. Long term fix allows both connections simultaneously.
 
### Issue 6 - Windows 10 DNS Resolution for homelab.local
**Problem:** Windows 10 client could not resolve homelab.local domain.  
**Cause:** IPv6 interference with DNS resolution for internal domain.  
**Solution:** Disabled IPv6 on Windows 10 client, pointed DNS to 10.0.0.20 (Windows Server).
 
### Issue 7 - GPO Not Applying to WIN10-CLIENT
**Problem:** Group Policy not applying to WIN10-CLIENT, RDP setting managed by organization but not working.  
**Cause:** Default Domain Policy was only linked to homelab.local domain root, not to the Homelab OU where WIN10-CLIENT lives. Additionally jdoe lacked local admin rights to run gpresult /scope computer.  
**Solution:** Identified GPO inheritance gap between domain root and Homelab OU. Bypassed GPO entirely and enabled RDP directly via registry (fDenyTSConnections=0) and opened firewall rule via netsh. Long term fix is to link Default Domain Policy to Homelab OU or create a dedicated GPO linked to the correct OU.
 
### Issue 8 - Access Denied Running gpresult as jdoe
**Problem:** Could not run gpresult /scope computer as jdoe — access denied error.  
**Cause:** jdoe is a standard domain user without local administrator privileges on WIN10-CLIENT.  
**Solution:** Used UAC prompt to elevate Command Prompt with domain administrator credentials (homelab\administrator) to run administrative commands.
