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
#### Clone this repo to your host:
```
git clone https://github.com/moovs/zabbix-grafana.git
```
#### Build and start docker-compose file:
```
cd /path/to/zabbix-grafana
```
```
docker-compose up -d --build
```
#### Move telegram script to alertscripts:
```
mv telegram alertscripts/
```
#### Set right permission to telegram script and make it executable:
```
cd alertscripts/
```
```
chown -R zabbix telegram
```
```
chmod +x telegram
```
## Step 1. Zabbix setup.
#### Login to Zabbix web interface with default 80 port:

> the default user is “admin” and the password is “zabbix”

<p align="center">
  <img width="300" height="400" src="https://github.com/moovs/zabbix-grafana/blob/master/src/zabbix-web.png">
</p>

#### Change admin password:

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

#### Update Zabbix agent config:
- after that you need to change point `Server=` in `/etc/zabbix/zabbix_agentd.conf` config to IP address your zabbix-server container `Server=172.16.238.10` and restart zabbix-agent `/etc/init.d/zabbix-agent restart`
 
#### Change Zabbix server IP address:
- and the last step, go to `Configuration` then `Hosts` click on `Zabbix server` and  change IP address to IP address your host: 

<p align="center">
  <img width="800" height="600" src="https://github.com/moovs/zabbix-grafana/blob/master/src/zabbix-host.png">
</p>

#### Check monitoring data:
- after few minutes, monitoring data will start flowing in, to check host graphs go to `Moniroting` then `Graphs`:

<p align="center">
  <img width="900" height="500" src="https://github.com/moovs/zabbix-grafana/blob/master/src/zabbix-dash.png">
</p>

## Step 2. Grafana setup.
#### Login to Grafana:
- login to Grafana web interface with default 3000 port login with default credentials and change admin password:
> default login and password is `admin`

<p align="center">
  <img width="800" height="600" src="https://github.com/moovs/zabbix-grafana/blob/master/src/grafana-web.png">
</p>

#### Enable Zabbix plugin:
- go to `Zabbix` in `Installed Apps` and enable Zabbix plugin or just click `Enable now`:

<p align="center">
  <img width="900" height="450" src="https://github.com/moovs/zabbix-grafana/blob/master/src/grafana-app.png">
</p>

#### Configure the data source as follows:
- enter a name for this new data source in the `Name` field.
- fill in the `Url` field with the full path to the Zabbix API, which will be http://your_zabbix_server_ip_address/api_jsonrpc.php.
- fill in the `Username` and `Password` fields in `Zabbix API details` block with the `username` and `password` for Zabbix.

<p align="center">
  <img width="600" height="700" src="https://github.com/moovs/zabbix-grafana/blob/master/src/data-src.png">
</p>

- click `Save & Test` to save and test your configuration. If you did everything correctly you will see the following message:

<p align="center">
  <img width="600" height="150" src="https://github.com/moovs/zabbix-grafana/blob/master/src/zabbix-api.png">
</p>

- here you may import some dashboards for vizualizations, just click to `Dashboards` and import `Zabbix Server Dashboard` and `Zabbix Template Linux Server`:

<p align="center">
  <img width="700" height="200" src="https://github.com/moovs/zabbix-grafana/blob/master/src/grafana-dash.png">
</p>

- to see them go to `Home`:

<p align="center">
  <img width="700" height="250" src="https://github.com/moovs/zabbix-grafana/blob/master/src/home-dash.png">
</p>

- and select your dashboards to see graphs:

<p align="center">
  <img width="900" height="500" src="https://github.com/moovs/zabbix-grafana/blob/master/src/dashboard.png">
</p>

> [here you find more dasboards for Zabbix](https://grafana.com/grafana/dashboards?category=zabbix)

## Step 3. Сonnecting and setting up a Telegram to Zabbix.

If you want send alert notifications to Telegram channel you need create Telegram bot.

#### Create Telegram Bot.

- firstly go to `BotFather` in your Telegram.
- in `BotFather` type `/start` see informations about what you can do or just type `/newbot` for create your bot.
- the next you will be asked to give a name and username to your bot.
>  it must end in `bot`. Like this, for example: TetrisBot or tetris_bot.
- after that you received your bot token to access the HTTP API, copy this token in you `Telegram` script in line `BOT_TOKEN`

<p align="center">
  <img width="700" height="400" src="https://github.com/moovs/zabbix-grafana/blob/master/src/telegram-bot.png">
</p>

- get the list of updates for your BOT for that follow this link and substitute your `bot token` `https://api.telegram.org/bot<YourBOTToken>/getUpdates`
- login to the Telegram web in your browser and find your bot by username and click on `Start` button.
- create your new group and add here your bot.
- now refresh this link to see you `group chat id`.
> your group chat id will be start with minus sign like this `-635324858`
- after that you can send test notification in your chat group:
```
docker exec -ti zabbix_server bash
```
```
cd /usr/lib/zabbix/alertscripts/
```
```
./telegram -635324858 test mesage
```
> if you received test message you can move on.

#### Create Media type in Zabbix.
- after everything done you need go to Zabbix web interface to `Administration` > `Media types` and choose `Create media type`: 

<p align="center">
  <img width="700" height="400" src="https://github.com/moovs/zabbix-grafana/blob/master/src/media-type.png">
</p>

- enter a name for media type in the `Name` and `Script name` fields and choose `Script` in `Type` fields.
- fill in `Script parameters` fields next parameters: `{ALERT.SENDTO}`, `{ALERT.SUBJECT}` and `{ALERT.MESSAGE}`.
- then go to `User profile: Zabbix Administrator`, click on `human icon` in the right corner and choose `Media` tab:

<p align="center">
  <img width="700" height="400" src="https://github.com/moovs/zabbix-grafana/blob/master/src/media.png">
</p>

- choose `Type` telegram in the field `Send to` substitute your group chat id `-635324858`and click `add` then `Update`.
- the next go to `Configuration` then `Actions` and choose `Host`, `equals` and name of your server `Zabbix server` and click `Add` button then `Update`:

<p align="center">
  <img width="700" height="400" src="https://github.com/moovs/zabbix-grafana/blob/master/src/action.png">
</p>

- don't forget `Enable` it on `Actions` page:

<p align="center">
  <img width="900" height="200" src="https://github.com/moovs/zabbix-grafana/blob/master/src/enable.png">
</p>
