## Network Setup and IP Configuration

For consistency, the following network configuration will be used in this guide:  
**_(Be sure you adjust these settings, sprinkled through the files, based on your setup!)_**

- Network Name: test.net
- IP Range: 192.168.1.0/24
- VM Host IP: 192.168.1.198
- Base VM IP: 192.168.1.199
- Testpoint VMs:
	- ps01: 192.168.1.200
	- ps02: 192.168.1.201
	- ps03: 192.168.1.202
- Archive VM:
	- archive: 192.168.1.203
    
Suggest you use 'Static DHCP' addressing for the nodes. 

Within Cockpit's view of a VM, there is an option- Network interfaces, click edit.  The image below shows the key areas for edits.

[Network Interfaces window](Network-Edit2.png)