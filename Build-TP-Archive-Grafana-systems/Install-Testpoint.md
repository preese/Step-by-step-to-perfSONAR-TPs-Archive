## Specific steps to install a Testpoint app, once, on a cloned base node:
```
sudo -s  
curl -s https://downloads.perfsonar.net/install  | sh -s - \  
--auto-updates --security testpoint \  
https://archive.test.net/psconfig/3by3.json
````
(wait awhile)

`pscheduler troubleshoot`

If this returns successfully proceed, if not reinstall.

`poweroff`
