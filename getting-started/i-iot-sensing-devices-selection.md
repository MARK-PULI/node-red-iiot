---
description: >-
  This chapter discusses how to select I-IoT sensing devices for the machine or
  the equipment sensors or actuators.
---

# I-IoT Sensing Devices Selection

_I-IoT sensing devices_ are the devices used to help the sensors or actuators connected to the I-IoT platform. In other books, the I-IoT sensing devices have other names ex. Industrial Adapter or IoT Adapter. This chapter list systematically the scenarios of different types of sensors, and the solutions connect to the platform.

The following table listed the recommended method and devices connecting to the I-IoT platform according to the data type or signal type.&#x20;

| Data source                                  | Data or signal type      | Recommended connector                                                  | Node-RED node                       |
| -------------------------------------------- | ------------------------ | ---------------------------------------------------------------------- | ----------------------------------- |
| RS-232  integration                          | RS-232                   | Raspberry Pi with RS-232 connector, PC or IoT gatewat with RS-232 port | RS-232 in (Read)                    |
| Meters, PID controllers support RS-485       | RS-485,       Modbus RTU | Advantech WISE-4051 or WISE-4220+S250                                  | <p>http request</p><p>(Restful)</p> |
| PLC, PID controllers support Modbus TCP      | Modbus TCP               | Raspberry Pi or NAS                                                    | Modbus Read                         |
| PLC, PID controllers support OPC UA          | OPC UA                   | Raspberry Pi or NAS                                                    | OpcUa                               |
| Control system support Telnet query commands | TCP                      | Raspberry Pi or NAS                                                    | tcp request                         |
| DHT22 temperture and humidity sensors        | GPIO i2c                 | Raspberrry Pi                                                          | rpi dht22                           |

| Data source                                  | Data or signal type                                    | Recommended connector                      | Node-RED node                       |
| -------------------------------------------- | ------------------------------------------------------ | ------------------------------------------ | ----------------------------------- |
| Inverters, sensors or meters with AI signal  | Analog Input signal                                    | Advantech WISE-4012 or WISE-4220 with S214 | <p>http request</p><p>(Restful)</p> |
| Sensors, speed or flow meters with DI signal | Digital Input signal                                   | Advantech WISE-4051 or WISE-4220 with S250 | <p>http request</p><p>(Restful)</p> |
| DVR or         IP cam                        | support snapshot API                                   | Raspberry Pi or NAS                        | http request                        |
| 3rd or other system support MQTT protocol    | MQTT                                                   | Raspberry Pi or NAS                        | mqtt subscrip                       |
| 3rd Web service support Restful              | Restful                                                | Raspberry Pi or NAS                        | http request(Restful)               |
| Database or TXT file exchange                | With NoSQL(MongoDB ex.), MS SQL, MySQL, AWS s3或TXT/CVS | NAS                                        | The match node                      |

In the following chapter, we are going to explore some examples of using the Node-RED nodes to collect data.

