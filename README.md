# Python Grafana/Influx Monitoring
This is the how-to setup/troubleshooting doc for the python scripts that back the grafana dashboards both in QA and PROD.

***
### Stack
1. Python (3.X) running on the server, calling the APIs to collect data and sending to InfluxDb.
  - _TODO_ make the scripts run as windows services to be managed collectively. Like [this](https://www.coretechnologies.com/products/AlwaysUp/Apps/RunPythonScriptAsAService.html).
2. InfluxDb storing the data from the several python scripts. This runs locally on port 8600.
  - example link to verify running [here](http://localhost:8086/query?db=prod-parking&epoch=s&q=SELECT+%22value%22+FROM+%22xovis-CPS_Overall%22+WHERE+time+%3E+now%28%29+-+5m+GROUP+BY+%22region%22).
3. Grafana pulling in this data, and hosting the dashboards utilized by the business folks. This runs locally on the host server as well (port 3000). The dashboards are backed up [here](https://github.com/dkoester/dashboards). 
  - *The grafana port 3000 has to be unbloced by the firewall locally to be reached within the VPN. For production we are hoping to add a proxy rule to be externally facing.*
  - _TODO_ backups ported to this repo.
4. Users access the dashboard hosted at <server-name>:3000 using user created profiles in grafana. Longer term solution might be to figure out how to include this in windows groups/sign on.

***
### General Info
- Servers:
  - elk-d2-dev (QA): 192.168.2.155
  - elk-p2-prd (PROD): 10.128.0.111
- Dashboard Links
  - QA [here](http://192.168.2.155:3000)
  - PRD [here](http://10.128.0.111:3000)

***
### Troubleshooting
1. Make sure you have admin access to this server
2. Is Grafana up? (can you hit the dashboard links under general info)
3. Is Influx up? (can you hit this locally) ```http://localhost:8086/query?db=prod-parking&epoch=s&q=SELECT+%22value%22+FROM+%22xovis-CPS_Overall%22+WHERE+time+%3E+now%28%29+-+5m+GROUP+BY+%22region%22```
4. Are the python scripts running? 
  - _TODO_ understand how to systemically 
5. If all three of these can be verified then the system should be up and running with data. I have seen where the python scripts fail for some uncaught exception with the API they are calling and then die. We should add metrics to this and be able to catch when they encounter these types of exceptions.


