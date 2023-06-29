## Configurações Zabbix Agent 2

Acessar as configurações do Agent2
 nano /etc/zabbix/zabbix_agent2.conf


1.2 Configurações Zabbix Agent 2
  
    `Plugins.Modbus.Sessions.MB1.Endpoint=tcp://IP_destino:502 (ex: 192.168.0.20:502)
    Plugins.Modbus.Sessions.MB1.SlaveID=20
    Plugins.Modbus.Sessions.MB1.Timeout=2`
  