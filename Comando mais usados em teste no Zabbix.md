# Comando mais usados em teste no Zabbix

zabbix_server -R config_cache_reload

# Instalação do Zabbix-Agent

yum install zabbix-agent -yum

systemctl enable --now zabbix-agent

# Verificando os log do Zabbix-agent

tail -f /var/log/zabbix/zabbix_agentd.log
# Verificando porta do agent
netstat -ltpn
telnet 192.168.0.100 10050


# Reload no cahe para atualizar as configurações web
zabbix_server -R config_cache_reload