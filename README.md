
 # Quick Start Guide:
 ## Setting Up perfSONAR Testpoints, Archive, and Grafana Hosts
 
Originally getting a MaDDash site up and running was quite challenging. Andy Lake’s **MaDDash Quick Install Guide** proved to be very helpful. However, it still left out an essential detail: what exactly should the .json file look like?  While it's thoroughly documented, I didn't want to keep dozens of tabs open in my browser to reference all the details.

With perfSONAR V5, the introduction of Testpoints and Archive sites made things more approachable. Still, understanding the .json file structure was a missing piece.

In this guide, I’ll walk you through setting up a set of virtual machines (VMs) with perfSONAR, along with the central Archive and Grafana components. I'll also provide the working .json file for your convenience. 

If you're only interested in the .json file, feel free to grab the 3by3.json file and skip the rest.

### Prerequisites
- This guide assumes a moderate level of Linux, OS and Grafana experience.
- Familiarity with Cockpit for system configuration and VM management is helpful.
- The guide includes instructions for using Cockpit, but other tools or bare-metal setups can be used as well.
- If you're new to Linux or Cockpit, I suggest checking out the provided links at the end for more in-depth guides.

If you're just starting out and need more guidance, you might want to begin by setting up Testpoints and Archive hosts on bare metal systems. You can then use the 3by3.json file to quickly get a working Grafana grid.

1. **Overview of the Setup Process**
Here's a high-level outline of the steps we’ll follow:
1. Create a base VM with a supported OS (from the perfSONAR list).
2. Clone the base VM to create one Testpoint node. Ensure it's fully configured and tested.
3. Clone the known good Testpoint VM two more times, for a total of three Testpoint VMs.
4. Use the Base VM to create a new VM for the Archive and Grafana setup.
5. Configure the Archive node and Grafana, then add the 3by3.json file to all nodes.
6. Verify the system and ensure the Grafana dashboard starts populating.

The following sections will provide step-by-step instructions for setting up a working perfSONAR grid using Cockpit and virtual machines.

2. **Detailed Setup: Using Cockpit to Set Up the Testpoints and Archive**

**System Requirements**
- A host system with at least 4 cores, 32 GB of RAM, and 300 GB of available storage (after OS installation) is recommended.
- The system should be connected to a network with internet access for downloading dependencies.
- A 1G NIC should suffice.
- Install your preferred, modern, up-to-date OS.
- Install Cockpit and its necessary components (cockpit-machines) to manage virtual machines

### Set Up the Virtual Machines
Here’s how to configure your system to build a working 3x3 perfSONAR grid, using Cockpit:
A. Create a "Base" VM on your host machine. The base VM will run a supported OS and should have at least 4 GB of RAM allocated.  Configure networking for Direct Attachment in Cockpit, ensuring that each VM can communicate with others on the same host. This is critical for Testpoints to interact with the Archive and Grafana VMs.
B. Clone the Base VM to create a Testpoint node. This will be your first Testpoint (ps01).
- Once cloned, install the Testpoint package.
- Run pscheduler troubleshoot to confirm that the Testpoint is functioning correctly.
        
(See below for the minimal command necessary to get the Testpoint installed)

Clone the Testpoint VM (ps01) two more times to create ps02 and ps03.
- Ensure each cloned VM is configured with the appropriate MAC address for each instance to enable static DHCP IP allocation. This will allow the VMs to retain consistent IP addresses.
- Re-check the networking configuration to use Direct Attachment for all cloned VMs.
Clone the Base VM again to create the Archive/Grafana VM.
- Increase the allocated RAM to 8GB for the Archive VM.
- Configure the networking with Direct Attachment to ensure communication with the other VMs.
Install the Archive and Grafana components on the Archive/Grafana VM

(See below for the minimal command necessary to get the Testpoint installed)

Once completed, visit the Grafana dashboard at `https://archive.test.net/grafana/dashboards`. If everything is set up correctly, you should begin seeing data populate into the dashboard.

-----
Detailed commands to use for the Testpoint and Archive/Grafana server installs.  Suggest you copy and paste the lines, as they are long.

Testpoint install steps:
```
sudo -s
curl -s https://downloads.perfsonar.net/install  | sh -s - --auto-updates --security testpoint https://archive.test.net/psconfig/3by3.json
```
Test the install with:

`pscheduler troubleshoot`

Archive and Grafana install steps:
``` sudo -s
curl -s https://downloads.perfsonar.net/install | sh -s - --auto-updates \
--add perfsonar-grafana \
--add perfsonar-grafana-toolkit \
--add perfsonar-psconfig-hostmetrics \
--add perfsonar-psconfig-publisher \
archive https://archive.test.net/psconfig/3by3.json
```
Test this install with:

`psarchive troubleshoot --skip-opensearch-data`

```
sed -i '/# Require ip 10.1.1.1 10.1.1.2/a \ Require ip 192.168.0.0/23\ ' /etc/httpd/conf.d/apache-logstash.conf
systemctl restart httpd
```

(develop .json file <xxxx.json> which has been done for this install, 3by3.json)

`psconfig validate 3by3.json`
`psconfig publish 3by3.json`

open firewall port:  
```
firewall-cmd --perm --add-service=https
firewall-cmd --reload
```

