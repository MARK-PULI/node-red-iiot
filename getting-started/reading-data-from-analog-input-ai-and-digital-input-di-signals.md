---
description: >-
  In general, AI/DI signals use RS485 to transfer values. In this chapter, we
  introduce alternative methods by Advantec IoT Wifi solution of WISE-4000
  serial which supported Restful protocol.
---

# Reading data from Analog Input(AI) and Digital Input(DI) signals

#### Requirements

* Advantec WISE-4012 or WISE-4220 with S214 for AI/DI/DO.\
  WISE-4051 or WISE-4220 with S251 for DI and RS-485.
* If the AI signal is used already, you can use Programmable DC Transducer for one input to two outputs signal.

#### The example flow(Monitoring the inverter frequency of the pump, tank level, and the flow rate for the recycled water)

![Reading data from AI/DI via Advantec WISE-4012](<../.gitbook/assets/Reading data from AI or DI signal via Advantec WISE-4012.jpg>)

#### Wiring of Advantec WISE-4012(AI/DI/DO)

![](<../.gitbook/assets/Adventec WISE-4012\_4051 DI REST (1).jpg>)

#### Wiring of WISE-4051(DI)

![](<../.gitbook/assets/Advantec WISE-4051 wiring(DI).jpg>)

#### WISE-4051 RS-485 wiring

![](<../.gitbook/assets/Advantec WISE-4051 wiring(RS-485).jpg>)

#### WISE-4012 AI REST format

![](<../.gitbook/assets/Advantec WISE-4012 AI REST.jpg>)

#### WISE-4012, WISE-4051 DI REST format

![](<../.gitbook/assets/Adventec WISE-4012\_4051 DI REST.jpg>)

#### Advantec WISE-4000 series REST URL and method

<figure><img src="../.gitbook/assets/Advantec WISE-4000 serial REST format.jpg" alt=""><figcaption></figcaption></figure>

There are alternative options for reading the singnal from the devices. Use WISE-4051 RS-485 Port with ADAM-4000 Modbus I/O Module. Which the DI port of WISE-4051 monitoring the speed of machine, and ADAM-4017+ monitoring another analog singnal equipments. The following figure is the  application scenario.

<figure><img src="../.gitbook/assets/Advantec WISE-4051 with ADAM-4017plus.jpg" alt=""><figcaption><p>Application Scenario of WISE-4051 RS-485 Port with ADAM-4000 Modbus I/O Module</p></figcaption></figure>

And use the ADAM-6700 series (ADAM-6717 for AI/DI/DO or ADAM-6750 for DI/DO) are Intelligent I/O Gateway which Node-RED platform included. In the Node-RED of ADAM-6700, there are package nodes for contact to the ADAM-6700. Because of the outputs of nodes are the value of the signal, 4-20mA for example. So you can convert to 0-65535 digital number for precisely calibration before MQTT out.

<figure><img src="../.gitbook/assets/Advantec ADAM-6700 series Node-RED.jpg" alt=""><figcaption><p>Node-RED example of  ADAM-6717</p></figcaption></figure>

The Node-RED example have these features

* Receive the AI/DI signal from ADAM6717.
* Convert AI/DI signal to 0-65535 digital number.
* Record the MAX/MIN value of this session.
* MQTT out the values of PV/MAX/MIN values.
* Display chart of the PV and MAX/MIN values.
* Can be reset and to restart collect the MAX/MIN value and clean the chart.&#x20;

For more WISE-4000 series information please reference from Advantech web site.
