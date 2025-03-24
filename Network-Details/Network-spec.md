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

[Network Interfaces window](Network-Edit.png)

**The above info and the info for this repo is intentionally generic!**  While highlighted as important, below are snippets of the areas you would change for your own setup:

The actual information used in the demo:
- Network Name: ufixu.com
- IP Range: 192.168.0.0/23
- VM Host IP: 192.168.1.119 1c:69:7a:04:3e:c3
- Base VM IP:  192.168.0.35 52:54:00:83:6f:45
- Testpoint VMs:
	- ps1u: 192.168.0.51 52:54:00:9f:0a:01
	- ps2u: 192.168.0.52 52:54:00:9f:0a:02
	- ps3u: 192.168.0.53 52:54:00:9f:0a:03
- Archive VM:
	- md-u: 192.168.0.50 52:54:00:9f:0a:05

**Updates needed during the install:**  

`curl -s https://downloads.perfsonar.net/install  | sh -s - --auto-updates --repo staging \
--security testpoint https://md-u.ufixu.com/psconfig/3by3.json`

`sed -i '/# Require ip 10.1.1.0\/24/a \ Require ip 192.168.0.0\/23\ ' \
/etc/httpd/conf.d/apache-logstash.conf`

The URL to view the Grafana dashboard:
``**https://md-u.ufixu.com/psconfig/grafana/dashboard/**``

````
{
    "archives": {
        "3by3-archive":{
           "archiver":"http",
           "data":{
              "schema":3,
              **"_url":"https://md-u.ufixu.com/logstash",**
              "verify-ssl":false,
              "op":"put",
              "_headers":{
                 "x-ps-observer":"{% scheduled_by_address %}",
                 "content-type":"application/json"
        }
      }
     }
    },
    "addresses": {
       **"host-1": { "address": "ps1u.test.net" },**
       **"host-2": { "address": "ps2u.test.net" },**
       **"host-3": { "address": "ps3u.test.net" }**
````