FROM grafana/grafana:latest

USER root

RUN grafana-cli plugins install alexanderzobnin-zabbix-app \
    && grafana-cli plugins update alexanderzobnin-zabbix-app

ENTRYPOINT [ "/run.sh" ]

EXPOSE 3000
