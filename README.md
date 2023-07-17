# Integração Zabbix com Protocolo MODBUS

This guide provides instructions for integrating Modbus devices with Zabbix using the Modbus plugin. The plugin allows you to monitor Modbus devices seamlessly through the Zabbix Agent 2, supporting both TCP and RTU connections.

 Repository to show conection from protocol MODBUS to Zabbix. This project exemple connect Generator Generac items  

This guide provides instructions for integrating Modbus devices with Zabbix using the Modbus plugin. The plugin allows you to monitor Modbus devices seamlessly through the Zabbix Agent 2, supporting both TCP and RTU connections.

Conhecimento necessario:
    Dominio do Zabbix server, 
    Conhecimento basico Zabbix agent 2

# 1. Requirements

*  De acordo com a <a href="https://www.zabbix.com/integrations/modbus"> Documentação Oficial </a> de integração Protocol Modbus + Zabbix, é necessário:
    - Zabbix Agent 2
        - Inserir parametros especificos
    - Go >= 1.12 (only required for building from source)


# 2. Protocolo modbus

1. Conceito Enlace/Aplicação

- O Modbus é um protocolo de comunicação mestre/escravo, o que significa que há um dispositivo mestre que inicia as transações de comunicação e um ou mais dispositivos escravos que respondem às solicitações do mestre. O dispositivo mestre envia comandos para os dispositivos escravos e recebe dados em resposta.



2. Protocolos de comunicação e Parametros

- Os dois protocolos mais comuns são o Modbus RTU (Unidade de Terminal Remota) e o Modbus TCP (Protocolo de Controle de Transmissão). O Modbus RTU é uma implementação serial que utiliza uma conexão RS-485 ou RS-232, enquanto o Modbus TCP é baseado em Ethernet.

- O modbus tem 8 parâmetros para passar: endpoint, slaveid, function, address, count, type, endianness e offset.

    * Endpoint: aqui é referente ao dispositivo, tcp:IP_destino:502, ou conforme configurado no arquivo de configuração,  MB1.
    - Slaveid: Como o arquivo de configuração já existe, ele está vazio aqui.
    - Função: Este valor está incluído no endereço completo do banco de dados, não sendo necessário preenchê-lo ou deixar-lo em branco.
    - Endereço: Esta é a chave, o endereço do item que apresentará a informação. Para descobrir a chave é necessário consultar as documentações do fabricante ou **SIGA PARA O ITEM 3** que apresento alternativa para coleta das chaves. Neste exemplo se eu quiser capturar o nível de combustivel de um gerador que usa o modulo DSE 855, temos a chave 1809 aqui.
    - Count: quantos dados obter, o padrão é 1, se você deseja obter dados de 40001 e dos seguintes endereços respiratórios, preencha o número necessário e obtenha uma matriz. Aqui somos uma aquisição única, então pre encha 1.
    - Digite: Preencha aqui de acordo com o tipo de dados modbus Todos os campos do modbus no Delta InfraSuite Manager são dados de ponto flutuante, então preencha float aqui.
    - Endianness: Aqui também é preenchido de acordo com o código do tipo de dados modbus Todos os campos do modbus do Delta InfraSuite Manager são codificados em Big endian, então preencha aqui.
    - Offset: Este parâmetro é usado para obter os dados antes ou depois do endereço de destino, se os dados do endereço de destino foram capturados, não é necessário preencher aqui.

- Então o formato de nossos dados capturados aqui é: <br>
    modbus.get[MB1,,,1809,1,float,be,].



# 3. Fazendo coletas e testes Modbus

1. Descoberta dos parametros através do Modbus poll

    * Conforme informado, consulte o fabricante para verificar disponibilide das chaves com as traduções para as capturas.

    * Ou Realize coletas dos dados do dispositivo utilizando o software  <a href="https://www.modbustools.com/download.html">  Modbus poll </a>. 

    * Para conectar ao Modbus pool e usar-lo para fazer o scan acesse:
        * Connection  
            * Selecione TCP/IP, em IP Address or Node Name insira o endereço do device e clique em OK.
        * Depois, acesse Functions e 
            * clique em Address Scan.
            * Insira o valor de Start e End Address, por último clique em Scan

2. Usar o ZABBIX para realizar as coletas:
*   Depois do ITEM validado e comparado com as informações do dispositivo realize os procedimentos abaixoÇ=.  
    
* Teste de coleta via linha de comando:
    
    ```
    zabbix_get 127.0.0.1 -p 10050 -k modbus.get[tcp:ip_destino:502,,3,id_modbus]
    
    Ex:
    zabbix_get -s 127.0.0.1 -p 10050 -k modbus.get[MB1,,3,1027]
    ```
![zabbix_get to modbus.get](https://raw.githubusercontent.com/rbsdcom/MODBUS-Zabbix/main/img/zabbix_get%20to%20modbus.get.png)


The Modbus plugin is included with the Zabbix Agent 2 and doesn't require any additional installation steps. Follow the steps below to ensure a Modbus device is available for connection and configure monitoring.

* Install Zabbix Agent 2: Follow the official Zabbix documentation to install and configure the Zabbix Agent 2 on your target system.
    - <a href="https://www.zabbix.com/download">Download Zabbix Agent2</a>

* Connect the Modbus Device: Ensure that the Modbus device you want to monitor is accessible and properly connected to your network or system. Make note of the device's IP address or serial port details, depending on the connection type (TCP or RTU).

* Configure Zabbix Agent 2: Open the Zabbix Agent 2 configuration file (typically located at `nano /etc/zabbix/zabbix_agent2.conf`) and make the following changes:

   - Enable the Modbus plugin by adding/modifying the following line:
     ```
     LoadModulePath=/path/to/modbus.so
     ```

   - Configure the Modbus items by adding the necessary configuration blocks for each Modbus device you want to monitor. Here's an example configuration for a TCP connection:
     ```
        Plugins.Modbus.Sessions.MB1.Endpoint=tcp://IP_destino:502 
        Plugins.Modbus.Sessions.MB1.SlaveID=20
        Plugins.Modbus.Sessions.MB1.Timeout=2
     ```
    - If you have two instances: "MB1" and "MB2", the following options have to be added to the agent configuration:
    ```
        Plugins.Modbus.Sessions.MB2.Endpoint=tcp://IP_destino:502 
        Plugins.Modbus.Sessions.MB2.SlaveID=20
        Plugins.Modbus.Sessions.MB2.Timeout=2
     ```

    Replace `<IP_destino>`, with the appropriate values for your Modbus device.

    - Salvar e fechar o arquivo de configuração


4. Restart Zabbix Agent 2: After making the configuration changes, restart the Zabbix Agent 2 service for the changes to take effect. Use the appropriate command for your operating system. For example, on Linux, you can use:
   ```
   sudo systemctl restart zabbix-agent2
   ```

5. Verify Monitoring: Once the Zabbix Agent 2 is running with the Modbus plugin enabled, navigate to your Zabbix server web interface and configure the necessary host and item settings to monitor the Modbus device. You can create triggers, graphs, and other Zabbix monitoring features based on the Modbus data collected.

Congratulations! You have successfully integrated Modbus devices with Zabbix using the Modbus plugin. You can now monitor and collect data from Modbus devices through the Zabbix monitoring system.

Agora, configure o Zabbix GUI.


# 5. Configurando Zabbix GUI

* Criar um host
    
    -adicione um nome <br>
    -adicione a fonte de coleta > *para realizar esta coleta, um dispositivo na rede sera usado para coletar e enviar, no nosso caso usamos uma instancia Zabbix Proxy, mas pode ser qualquer servidor com Zabbix Agent2. <br>
    -Adicione os templates padroes e os grupos de hosts <br>


* Crie um Item

    -Adcione um nome<br>
    -Selecione Agente passivo <br>
    -adicione a chave > aqui adicione a seguinte sequencia ...





## Contributing

If you wish to contribute to this project, please follow the guidelines outlined in the CONTRIBUTING file of this repository.

## Support

For bug reports, issues, or feature requests, please use the Issues section of this GitHub repository.

* Links extras <p>
https://www.zabbix.com/br/integrations/modbus
https://www.zabbix.com/documentation/current/pt/manual/appendix/items/modbus
https://www.bookstack.cn/read/zabbix-6.4-en/886217c3f1c358ec.md




