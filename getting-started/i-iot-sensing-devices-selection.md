---
description: >-
  This chapter discusses how to select I-IoT sensing devices for the machine or
  the equipment sensors or actuators.
---

# I-IoT Sensing Devices Selection

_I-IoT sensing devices_ are the devices used to help the sensors or actuators connected to the I-IoT platform. In other books, the I-IoT sensing devices have other names ex. Industrial Adapter or IoT Adapter. This chapter list systematically the scenarios of different types of sensors, and the solutions connect to the platform.

The following table listed the recommended method and devices connecting to the I-IoT platform according to the data type or signal type.&#x20;

<table><thead><tr><th>Data source</th><th width="177">Data or signal type</th><th width="212">Recommended connector</th><th>Node-RED node</th></tr></thead><tbody><tr><td>RS-232  integration</td><td>RS-232</td><td>Raspberry Pi with RS-232 connector, PC or IoT <mark style="color:yellow;">gatewat</mark> with RS-232 port</td><td>RS-232 in (Read)</td></tr><tr><td>Meters, PID controllers support RS-485</td><td>RS-485,       Modbus RTU</td><td>Advantech WISE-4051 or WISE-4220+S250 </td><td><p>http request</p><p>(Restful)</p></td></tr><tr><td>PLC, PID controllers support Modbus TCP</td><td>Modbus TCP</td><td>Raspberry Pi or NAS</td><td>Modbus Read</td></tr><tr><td>PLC, PID controllers support OPC UA</td><td>OPC UA</td><td>Raspberry Pi or NAS</td><td>OpcUa</td></tr><tr><td>Control system support Telnet query commands</td><td>TCP</td><td>Raspberry Pi or NAS</td><td>tcp request</td></tr><tr><td>DHT22 temperture and humidity sensors</td><td>GPIO i2c</td><td>Raspberrry Pi</td><td>rpi dht22</td></tr></tbody></table>

<table><thead><tr><th width="158">Data source</th><th width="178.20433017591338">Data or signal type</th><th>Recommended connector</th><th>Node-RED node</th></tr></thead><tbody><tr><td>Inverters, sensors or meters with AI signal</td><td>Analog Input signal</td><td>Advantech WISE-4012 or WISE-4220 with S214</td><td><p>http request</p><p>(Restful)</p></td></tr><tr><td>Sensors, speed or flow meters with DI signal</td><td>Digital Input signal</td><td>Advantech WISE-4051 or WISE-4220 with S250</td><td><p>http request</p><p>(Restful)</p></td></tr><tr><td>DVR or         IP cam</td><td>support snapshot API</td><td>Raspberry Pi or NAS</td><td>http request</td></tr><tr><td>3rd or other system support MQTT protocol</td><td>MQTT</td><td>Raspberry Pi or NAS</td><td>mqtt subscribe</td></tr><tr><td>3rd Web service support Restful</td><td>Restful</td><td>Raspberry Pi or NAS</td><td>http request(Restful)</td></tr><tr><td>Database or TXT file exchange</td><td>With NoSQL(MongoDB ex.), MS SQL, MySQL, AWS s3或TXT/CVS</td><td>NAS</td><td>The match node</td></tr></tbody></table>

In the following chapter, we are going to explore some examples of using the Node-RED nodes to collect data.

