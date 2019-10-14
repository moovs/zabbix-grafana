FROM zabbix/zabbix-server-mysql:ubuntu-4.0-latest

RUN apt-get update \
    && apt-get install python-pip -y \
    && pip install pyTelegramBotAPI

ENTRYPOINT ["docker-entrypoint.sh"]

EXPOSE 10051
