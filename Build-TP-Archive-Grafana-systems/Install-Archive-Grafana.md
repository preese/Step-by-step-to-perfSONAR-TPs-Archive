## Specific steps to install the Archive and Grafana apps on a cloned base node:  
**Confirm you are using YOUR FQDN names and IP numbers! (see 'bold' items)**  

(the second line is very long, suggest you copy and paste it)
```
sudo -s
curl -s https://downloads.perfsonar.net/install | sh -s - --auto-updates \
--add perfsonar-grafana \
--add perfsonar-grafana-toolkit \
--add perfsonar-psconfig-hostmetrics \
--add perfsonar-psconfig-publisher \
archive https://**archive.test.net**/psconfig/**3by3.json**
```
Run:  
`psarchive troubleshoot --skip-opensearch-data`

If this runs successfully proceed, if not reinstall.

Add ip range and finish up installs:
````
sed -i '/# Require ip 10.1.1.0\/24/a \ Require ip **192.168.1.0\/24**\ ' \
/etc/httpd/conf.d/apache-logstash.conf
systemctl restart httpd
psconfig validate **3by3.json**
psconfig publish **3by3.json**
firewall-cmd --perm --add-service=https  
firewall-cmd --reload
````
