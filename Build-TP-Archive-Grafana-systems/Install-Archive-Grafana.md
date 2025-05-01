## Specific steps to install the Archive and Grafana apps on a cloned base node:  
**Confirm you are using YOUR FQDN names and IP numbers!**  

Copy the  **_[3by3.json](../3by3.json)_** file to this VM, adjust FQDMs in the file as needed.

(Suggest you copy and paste the whole block)
```
sudo -s
curl -s https://downloads.perfsonar.net/install | sh -s - --auto-updates \
--add perfsonar-grafana \
--add perfsonar-grafana-toolkit \
--add perfsonar-psconfig-hostmetrics \
--add perfsonar-psconfig-publisher \
archive https://archive.ufixu.com/psconfig/3by3.json
sed -i '/# Require ip 10.1.1.0\/24/a \ Require ip 192.168.0.0\/23\ ' \
    /etc/httpd/conf.d/apache-logstash.conf
systemctl restart httpd
psconfig publish 3by3.json
firewall-cmd --perm --add-service=https
firewall-cmd --reload
```
Run:  
`psarchive troubleshoot --skip-opensearch-data`

If this runs successfully proceed, if not reinstall.

