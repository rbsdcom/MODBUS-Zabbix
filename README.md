# Integração Zabbix com Protocolo MODBUS

This guide provides instructions for integrating Modbus devices with Zabbix using the Modbus plugin. The plugin allows you to monitor Modbus devices seamlessly through the Zabbix Agent 2, supporting both TCP and RTU connections.

Repository to show conection from protocol MODBUS to Zabbix. This project exemple connect Generator Generac items  

This guide provides instructions for integrating Modbus devices with Zabbix using the Modbus plugin. The plugin allows you to monitor Modbus devices seamlessly through the Zabbix Agent 2, supporting both TCP and RTU connections.



# 1. Requirements

* Required Knowledge:
    - Proficiency in Zabbix server administration
    - Basic understanding of Zabbix Agent 2

*  According to <a href="https://www.zabbix.com/integrations/modbus"> Official Documentation Zabbix </a> integration Protocol Modbus + Zabbix, the following steps are required:
    - Zabbix Agent 2
        - Insert specific parameters.
    - Go >= 1.12 (only required for building from source)


# 2. Protocolo modbus

1. Concept Link/Application

- Modbus is a master/slave communication protocol, which means that there is a master device that initiates communication transactions and one or more slave devices that respond to the master's requests. The master device sends commands to the slave devices and receives data in response.



2. Communication Protocols and Parameters

- The two most common protocols are Modbus RTU (Remote Terminal Unit) and Modbus TCP (Transmission Control Protocol). Modbus RTU is a serial implementation that uses RS-485 or RS-232 connections, while Modbus TCP is based on Ethernet.

- Modbus has 8 parameters to pass: endpoint, slaveid, function, address, count, type, endianness, and offset.

    - Endpoint: This refers to the device, either tcp:destination_IP:502 or as configured in the configuration file, MB1.
    - Slaveid: Since the configuration file already exists, it is empty here.
    - Function: This value is included in the complete database address, so it is not necessary to fill it in or leave it blank.
    - Address: This is the key, the address of the item that will present the information. To find the key, you need to consult the manufacturer's documentation or PROCEED TO ITEM 3 where I present an alternative for collecting the keys. In this example, if I want to capture the fuel level of a generator that uses the DSE 855 module, we have the key 1809 here.
    - Count: How many data to obtain, the default is 1. If you want to obtain data from 40001 and the following contiguous addresses, fill in the necessary number and obtain an array. Here, we are acquiring a single value, so fill in 1.
    - Type: Fill in according to the Modbus data type. All Modbus fields in the Delta InfraSuite Manager are floating-point data, so fill in float here.
    - Endianness: Fill in according to the Modbus data type code. All Modbus fields in the Delta InfraSuite Manager are encoded in Big endian, so fill it in here.
    - Offset: This parameter is used to obtain data before or after the target address. If the data from the target address is being captured, it is not necessary to fill it in here.

- So, the format of our captured data here is: <br>
    modbus.get[MB1,,,1809,1,float,be,].



# 3. Perfmormin Modbus and collection tests

1. Parameter discovery through Modbus poll

    * Conforme informado, consulte o fabricante para verificar disponibilide das chaves com as traduções para as capturas.

    * Ou Realize coletas dos dados do dispositivo utilizando o software  <a href="https://www.modbustools.com/download.html">  Modbus poll </a>. 

    * Para conectar ao Modbus pool e usar-lo para fazer o scan acesse:
        * Connection  
            * Selecione TCP/IP, em IP Address or Node Name insira o endereço do device e clique em OK.
        * Depois, acesse Functions e 
            * clique em Address Scan.
            * Insira o valor de Start e End Address, por último clique em Scan

2. Use ZABBIX to perform data collections :
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

    - Save and close the file configuratio.


4. Restart Zabbix Agent 2: After making the configuration changes, restart the Zabbix Agent 2 service for the changes to take effect. Use the appropriate command for your operating system. For example, on Linux, you can use:
   ```
   sudo systemctl restart zabbix-agent2
   ```

5. Verify Monitoring: Once the Zabbix Agent 2 is running with the Modbus plugin enabled, navigate to your Zabbix server web interface and configure the necessary host and item settings to monitor the Modbus device. You can create triggers, graphs, and other Zabbix monitoring features based on the Modbus data collected.

Congratulations! You have successfully integrated Modbus devices with Zabbix using the Modbus plugin. You can now monitor and collect data from Modbus devices through the Zabbix monitoring system.

Now, configure the Zabbix GUI.


# 5. Configurando Zabbix GUI

* Criating a host
    
    - Host Name 
    - Host Group
    - And others Host Informations
    
 
* Criating a Modbus Item

    - Name 
    - Select type Zabbix Agent
    - Type of information related to the parameter
    - Key: modbus.get[MB1,,,EndereçoKey]
    - Insert the remaining information for data collection parameters (Units, update interval etc.)


## Contributing

If you wish to contribute to this project, please follow the guidelines outlined in the CONTRIBUTING file of this repository.

## Support

For bug reports, issues, or feature requests, please use the Issues section of this GitHub repository.

* Links extras <p>
https://www.zabbix.com/br/integrations/modbus
https://www.zabbix.com/documentation/current/pt/manual/appendix/items/modbus
https://www.bookstack.cn/read/zabbix-6.4-en/886217c3f1c358ec.md




