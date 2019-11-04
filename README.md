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
The whole installation will be divided into 3 parts. The first step is setting up Zabbix, the second step is connecting Zabbix to Grafana, and the third step is creating Telegram bot and connect to Zabbix for notifications.
<br>
## Prerequsition:
- clone this repo to your host:
```
git clone https://github.com/moovs/zabbix-grafana.git
```
- build and start docker-compose file:
```
cd /path/to/zabbix-grafana
```
```
docker-compose up -d --build
```
- move telegram script to alertscripts:
```
mv telegram alertscripts/
```
- set right permission to telegram script and make it executable:
```
cd alertscripts/
```
```
chown -R zabbix telegram
```
```
chmod +x telegram
```
>> we will set up a notification in Telegram a little bit later, right now, we will simply prepare the ground for this so that we would not return to this later.

- login to Zabbix web interface:
<img width="250" height="40" src="https://github.com/moovs/zabbix-grafana/blob/master/src/your_host.png">

>> for this, you need type ip address your server, on which you installed Zabbix, in address bar your browse, then type default login 'admin' and password 'zabbix'
