# Step 1: Secure appliance and management interface
- Turn off scripting mode (Prevents terminal from overwriting commands longer than one line)
  ~~~
  admin@PA-> set cli scripting-mode off
  ~~~
  
- Turn off mgmt interface temporarily (Red team could have access already)
  ~~~
  admin@PA> configure
  admin@PA> set deviceconfig system permitted-ip 127.0.0.1
  admin@PA# commit
  ~~~

- Turn off external data interface temp (Red team could be managing FW via Data Interface)
  ~~~
  configure
  set network interface ethernet ethernet1/1 link-state down
  commit
  ~~~
- Change Admin Password
  ~~~
  admin@PA# set mgt-config users admin password
  Enter password:
  ~~~
- Set up SSH
- Review system info
   - Displays information about your system along with  PANOS version and licenses
- Change MGMT Interface IP Address if needed
  ~~~
  >configure
  #set deviceconfig system ip-address x.x.x.x netmask x.x.x.x default-gateway x.x.x.x dns-setting servers primary x.x.x.x
  #commit
  ~~~
- Only allow Secure Protocols To Connect to MGT Interface
- Showing Admin Accounts
  ~~~
  > show admins all
  # delete mgt-config users _name_
  #commit
  ~~~
- Turn Data Interfaces Back On if turned off
- Turn Management Interface back on
  ~~~
  #set deviceconfig system permitted-ip x.x.x.x
  #commit
  ~~~
- Backup FW Using SCP
  ~~~
  >scp export configuration to user@host:/host/directory from running-config.xml
  ~~~

# Step 2: Liencse Firewall
# Step 3: 
- Setup dynamic updates to retrieve current malware sigs
# Step 4: Configure network, deploy FW, configure security policies and assign security profiles to sec policies.
Network Deployment Options: 
1, Virtual Wire
- Rapid deployment
- Con: Only provides North-South protection (can't segment internal traffic into multiple internal zones)
- Configure inbound/outbound block rule to block unknown bad urls
- Configure inbound rules for scored services
   - Be specific and allow apps and destination IP addresses
- Configure outbound rules
    - Make rules specific
    - Only allow outbound traffic from specific IP addresses that are absolutely necessary.
- Assign security profiles to all allow rules
   - FW will not block malware without Security profiles assigned to security policies
2, Layer 2
3, Layer 3
