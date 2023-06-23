# MODBUS-Zabbix
 Repository to show conection from protocol MODBUS to Zabbix. This project exemple connect Generator Generac items  

This guide provides instructions for integrating Modbus devices with Zabbix using the Modbus plugin. The plugin allows you to monitor Modbus devices seamlessly through the Zabbix Agent 2, supporting both TCP and RTU connections.

Conhecimento necessario, dominio do Zabbix server, conhecimento basico Zabbix agent 2

## 1 Requisitos Zabbix

1.1 De acordo com a documentação oficial Zabbix é necessário:

1.1.1 Zabbix Server

1.1.2 Zabbix Agent 2

Clique aqui para acessar as configurações necessarias para o funcionamento do modulo Modbus nativamente.


## 2 Requisitos Modbus

2.1 Modbus poll

-Para verificar as coletas dos dados do dispositivo, usa-se o software Modbus poll no link, ou procure no site do fabricante as chaves com as traducoes para as capturas.

-Para conectar ao Modbus pool e usar para fazer o scan, use este tutorial

-Como usar o ZABBIX para realizar as coletas:
*   Depois do ITEM validado e comparado com o dispositivo, segue passo a passo:

## Config Zabbix Server GUI
1 Crie um host
    -adicione um nome
    -adicione a fonte de coleta > *para realizar esta coleta, um dispositivo na rede sera usado para coletar e enviar, no nosso caso usamos uma instancia Zabbix Proxy, mas pode ser qualquer servidor com Zabbix Agent2.
    -Adicione os templates padroes e os grupos de hosts


2 Crie um Item
    -Adcione um nome
    -Selecione Agente passivo
    -adicione a chave > aqui adicione a seguinte sequencia ...
    -

* FUNCIONAMENTO MODBUS

Mode Slave


## Estudo de caso Gerador Generac DepSea 855

Neste estudo de caso, foi usado para monitorar 2 geradores da marca generac com o modulo de monitoramento Depsea 855. Portanto segue documentação das descobertas



