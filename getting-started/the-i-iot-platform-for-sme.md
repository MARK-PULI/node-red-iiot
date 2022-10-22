---
description: >-
  This chapter discussing how  to implement I-IOT patform for Small and
  medium-sized enterprise
---

# The I-IoT platform for SME

{% hint style="info" %}
After our surveys and study for the company.  There are limited resources for SMEs(Small and medium-sized enterprises). Need some economic programs to create a private cloud of I-IoT.  In this chapter, we will cover the following topics:

* NAS(Network Attached Storage)  as the Node-RED server,  Mosquitto MQTT server(Message Queuing Telemetry Transport), and DB server(we install MongoDB).
* Raspberry pi installs Node-RED as the gateway and for edge computing.
* The functions of NAS and Raspberry pi
{% endhint %}

The following diagram depicts the proposed framework of the I-IoT platform for SMEs.&#x20;

There is much wireless technology developed.  In our case, we used WIFI Wireless IoT sensing devices to connect to sensors and actuators in the factory. The examples In the diagram depicts above are the Advantech WISE-4000 series.&#x20;

The MQTT and RESTful API are our IT communication protocols.  The Modbus, OPC, and RS-232 are the main OT communication protocols.

NAS(Network Attached Storage)  as the Node-RED server,  MQTT server, and Mongo DB server. Raspberry pi is the gateway and for edge computing.

And used NAS to communicate with ERP and MES(maybe there no MES in the company, but I suggested there is ERP at least).



![The Proposed framework of the private cloud I-IoT planform for SMEs](<../.gitbook/assets/The framework of I-IoT.jpg>)

### The benefits of using NAS

We can find the NASs used in many companies. And the functions progress over time. We can very easy to use Docker to install The Node-RED server, MQTT server, and MongoDB. The benefits of using NAS are:

* More storage spaces
* Private cloud storage
* The simple server setup procedure
* Automated backup
* Data protection

### The Roles of NASs and  Raspberry Pi

The following diagram depicts the relationship between NASs and Raspberry Pi

![The rols of NASs and Raspberry Pi](<../.gitbook/assets/The roles of NASs and Raspberry pi-1.jpg>)



The detail of the functions outlined in the following diagram:

![The rols of NASs and Raspberry Pi](<../.gitbook/assets/The roles of NASs and Raspberry pi-2.jpg>)

{% hint style="info" %}
* For the ethernet configuration, we suggest 3  network segments at least, and they are separated by the firewall. Please see the following table.
* Each NAS in addition to designing many tabs for the dashboard page. It can install 2 instants of Node-RED, like 1880 port or 1887 port. You can use these features
* Raspberry Pi can add the extension box UNO-220 of Advantech. There are more benefits from the box. You can get more information from Advantech.
{% endhint %}

### The ethernet configuration for ERP/MES Server, NASs, and Raspberry Pi

Subnets 1, 2, and 3 in the following table, mean 192.168.1.x, 192.168.2.x, 192.168.3.x for example. You can configure them to 192.168.11.x, 192.168.21.x, 192.168.31.x also. Some DSCs, PLCs have different subnets.

|                            |       1st Lan Port      |       2nd Lan Port      |
| -------------------------- | :---------------------: | :---------------------: |
| ERP/MES                    |         Subnet 1        |            NA           |
| NAS-1                      |         Subnet 1        |         Subnet 2        |
| NAS-2                      |         Subnet 1        |         Subnet 2        |
| Raspberry Pi for Dashboard |         Subnet 2        |            NA           |
| IoT Sensing devices        |         Subnet 2        |            NA           |
| Raspberry Pi for Edge      | <p>Subnet 2<br>Wifi</p> |  <p>Subnet 3<br>Lan</p> |
| DSC, PLC  or controller    |         Subnet 3        |            NA           |
| Your Computer              |  <p>Subnet 1<br>Lan</p> | <p>Subnet 2<br>Wifi</p> |

### The Service installed by Docker for NAS

The following diagram depicts the installation of the proposed services by Docker for NAS. We will install Eclipse Mosquitto MQTT data broker("eclipse-mosquitto1"), Node-RED platform(2 instances "nodered-node-red1" and "nodered-node-red7"), and MongoDB("mongoIoT206" in this example) in the Synology NAS.&#x20;

![The examples install service by the Docker for Synology NAS](<../.gitbook/assets/The Services Installed by Docker for NAS.jpg>)

### The services install to Raspberry Pi

* mosquitto MQTT
* Node-RED

The install procedure and settings you can find more information on the internet.

### The nodes install for Node-RED of NAS and Raspberry Pi

After you finished installing the NAS and Raspberry Pi, and their services. In this book, we install the nodes for Node-RED of NAS and Raspberry Pi as follows, please use the \[Manage palette] of the menu in Node-RED.

![The recommanded nodes install for Node-RED](<../.gitbook/assets/The nodes install for Node-RED.jpg>)
