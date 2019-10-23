<p>
  <img width="1000" height="170" align="center" src="https://github.com/moovs/zabbix-grafana/blob/master/src/ZabbixandGrafana.jpg">
</p>

***

<p align="center">
  <b>Deploy Zabbix 4.0 with Grafana dashboards in Docker-compose</b>
</p>

***

This guide is about how to deploy a Zabbix monitoring system with Grafana to display graphs.
<br>
You will need to follow the steps described below if you are not familiar with how this system works:

> clone this repo to your host:
```
git clone https://github.com/moovs/zabbix-grafana.git
```
> build and start docker-compose file:
```
cd /path/to/zabbix-grafana
```
```
docker-compose up -d --build
```
> move telegram script to alertscripts
```
mv telegram alertscripts/
```
> login to Zabbix web interface<br>
for this, you need type ip address your server, on which you installed Zabbix, in address bar your browser
```

```
