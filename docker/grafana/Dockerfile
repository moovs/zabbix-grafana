FROM grafana/grafana:latest

USER root

RUN apt-get update \
    && grafana-cli plugins install alexanderzobnin-zabbix-app \
    && grafana-cli plugins update alexanderzobnin-zabbix-app

ENTRYPOINT [ "/run.sh" ]

EXPOSE 3000
