# MODBUS-Zabbix
 Repository to show conection from protocol MODBUS to Zabbix. This project exemple connect Generator Generac items  

This guide provides instructions for integrating Modbus devices with Zabbix using the Modbus plugin. The plugin allows you to monitor Modbus devices seamlessly through the Zabbix Agent 2, supporting both TCP and RTU connections.

Conhecimento necessario:
    Dominio do Zabbix server, 
    Conhecimento basico Zabbix agent 2

## 1 Requisitos Zabbix

1.1 De acordo com a <a href="https://www.zabbix.com/integrations/modbus"> Documentação Oficial </a> de integração Protocol Modbus + Zabbix, é necessário:

    1.1.1 Zabbix Server 

    1.1.2 Zabbix Agent 2 
        1.1.2.1 Adicionar 3 parametros especificos



## 2 Requisitos Modbus

2.1 Modbus poll

-Para realizar as coletas dos dados do dispositivo, geralmente uso o software <a href="https://www.modbustools.com/download.html">  Modbus poll </a>. O fabricante pode disponibilizar as chaves com as traducoes para as capturas.

-Para conectar ao Modbus pool e usar para fazer o scan, use este tutorial

-Como usar o ZABBIX para realizar as coletas:
*   Depois do ITEM validado e comparado com o dispositivo, segue:

## 3 Config Zabbix Server GUI
1 Crie um host
    -adicione um nome <br>
    -adicione a fonte de coleta > *para realizar esta coleta, um dispositivo na rede sera usado para coletar e enviar, no nosso caso usamos uma instancia Zabbix Proxy, mas pode ser qualquer servidor com Zabbix Agent2. <br>
    -Adicione os templates padroes e os grupos de hosts <br>


2 Crie um Item
    -Adcione um nome<br>
    -Selecione Agente passivo <br>
    -adicione a chave > aqui adicione a seguinte sequencia ...
    -







