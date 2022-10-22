# Reading data via RS-485

#### Requirements

* Raspberry Pi, PC, or IoT gateway with RS-485 Port.
* RS-485 station ID(Slave ID), the data address table, and data format.

#### The example flow(Temperature controller with RS-485, via Advantec WISE-4051 WiFi IoT wireless I/O)

![Advantec WISE-4051 DI and RS-485 Modus/RTU](<../.gitbook/assets/Advantec WISE-4051.jpg>)

![Advantec WISE-4051 configuration](<../.gitbook/assets/Advantec WISE-4051 RS-485 configuration.jpg>)

{% hint style="info" %}
1\. Slave ID can’t duplicate

2\. Correct wiring

3\. See the menu of the sensors or controllers, maybe you must X 10

&#x20;  for the data conversion.

4\. Bit or Word, in this example, is the Word. Check WISE-4051 word status

&#x20;   have response.
{% endhint %}

![The flow to get Power data and Temperature data of the machine](<../.gitbook/assets/Reading data via RS-485 by WISE-4051.jpg>)

Using \[inject] node, to set interval via \[http Request]\(Restful). Reading data, ex. 5 secs. This flow can install in Raspberry Pi, NAS, or IoT Gateway.
