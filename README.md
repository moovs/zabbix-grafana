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
## Prerequisites:
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
## Step 1: Zabbix setup
#### login to Zabbix web interface with default 80 port:

> the default user is “admin” and the password is “zabbix”

<p align="center">
  <img width="300" height="400" src="https://github.com/moovs/zabbix-grafana/blob/master/src/zabbix-web.png">
</p>

#### Change admin password

- navigate to `Administration` then `Users` select `Admin` and click `Change password`

#### If you want to monitor your instance with Zabbix, you need to install zabbix-agent on this host:
> in this case, we install Zabbix agent of the 4.0 version:
```
wget https://repo.zabbix.com/zabbix/4.0/debian/pool/main/z/zabbix-release/zabbix-release_4.0-2+stretch_all.deb
```
```
dpkg -i zabbix-release_4.0-2+stretch_all.deb
```
```
apt-get update
```
```
apt-get install zabbix-agent
```

#### After that you need to change point `Server=` in `/etc/zabbix/zabbix_agentd.conf` config to IP address your zabbix-server container `Server=172.16.238.10` and restart zabbix-agent `/etc/init.d/zabbix-agent restart`
 
#### And the last step, go to `Configuration` then `Hosts` click on `Zabbix server` and  change IP address to IP address your host: 

<p align="center">
  <img width="800" height="600" src="https://github.com/moovs/zabbix-grafana/blob/master/src/zabbix-host.png">
</p>

#### After waiting a few minutes go to `Moniroting` then `Graphs` set your Group, Host and see 
