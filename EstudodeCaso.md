## Estudo de caso Gerador Generac DepSea 855

Neste estudo de caso, foi utilizado para monitorar o Gerador PY100 da Generac, este modelo especifico possui o modulo de monitoramento Depsea 855. 
Segue documentação das descobertas:

1- Configuração do Zabbix Agent

2- Descoberta dos parametros através do Modbus poll
    2.1 Teste de coleta
        zabbix_get 127.0.0.1 -p 10050 -k modbus.get[tcp:ip_destino:502,,3,id_modbus]

        zabbix_get -s 192.168.1.11 -p 10050 -k modbus.get[MB1,,3,1027]

3- Criação do Host para monitoramento

4- Criação dos itens de monitoramento

4- Criação das triggers

5- Criação dos graficos 