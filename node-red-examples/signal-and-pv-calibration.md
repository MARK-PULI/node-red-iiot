---
description: This chapter discussing how to carlibration the AI/DI signal and the PV.
---

# Signal and PV Calibration

### The Principle of calibration AI signal with PV

#### Requirement

* AI signal connect to IoT sensing device Advantech WISE-4012, WISE-4022+S214, or alternative devices.&#x20;
* Reading data from the IoT sensing device by Raspberry Pi, NAS, or IoT Gateway.

Using Liner calibration the formula is listed following

$$
Y=a+bX
$$

Where Y is the reading value read from WISE(0-65535), a parameter is the intercept or offset of the reading value. X is the engineering value (ex. flow rate, pressure, level...). b parameter is the convert multiplier.

After getting the a and b parameters, we can read the value from WISE, and use the following formula to get the engineering value.

$$
X=(Y-a)/b
$$

{% hint style="info" %}
Advantech WISE-4000 serials convert the AI signal(4-20ma, 0-10V, 0-5V...) to 0-65535.\
Please see the chapter on [Reading data from Analogy Input(AI) signal](../getting-started/reading-data-from-analog-input-ai-and-digital-input-di-signals.md).

Advantech WISE-4000 serials are provided to monitor the value of MIN/MAX reading value(RV). Then we can record the engineering value(EV) of MIN and MAX.

&#x20;       _b=(RV.MAX-RV.MIN)/(EV.MAX-EV.MIN)_&#x20;

&#x20;       that is the reading value per unit of engineering value

&#x20;       _a=RV.MAX-(b ＊ EV.MAX)_    get the offset

We can save the values of a and b for each sensor in the database.
{% endhint %}

![WISE-4012 configuration and monitoring](<../.gitbook/assets/WISE-4012 AI signal monitor.jpg>)

### The Principle of calibration DI(Pulse) signal

#### Requirement

* AI signal connect to IoT sensing device Advantech WISE-4012, WISE-4051, WISE-4022+S214, or alternative devices.&#x20;
* Reading data from the IoT sensing device by Raspberry Pi, NAS, or IoT Gateway.

Using the same Liner calibration of the formula for AI signal is listed above. And the parameter a will be 0.

{% hint style="info" %}
_Y_ is the reading value depending on the _**frequency**_ setting on the WISE-4051.

If you need more precision, set it to 0.01Hz.

As the application of line speed signal from the limit switch or driver. Reading value is the frequency value.

&#x20;   _b=(RV.MAX)/(MAX speed)_&#x20;

&#x20;   that is the reading value per unit of line speed (ex. M/min or rpm)

We can convert the reading value Y from WISE-4051 to line speed by

&#x20;    _Line speed(X) = Y / b_   &#x20;
{% endhint %}

![Configuration of WISE-4051](<../.gitbook/assets/WISE-4051 DI signal-1.jpg>)

![The sampe of reading value from WISE-4051](<../.gitbook/assets/WISE-4051 DI signal-2.jpg>)

### Design the UI for managing the sensors and calibration

&#x20;The following diagram is an example of UI for managing the sensors and calibration, and saving the result to the database(ex. MongoDB document name: IOTMB).

![Example of the setting UI for sensors(running at NAS)](<../.gitbook/assets/Sensor setting.jpg>)

1. After setting the basic information of the sensor, use the SensorAddr to read the value from WISE IoT Device(WISE-4012 in this example).
2. Input the Signal value(from WISE-4012), and the represent engineering value, including the MAX and the MIN value. In the example shown above, the MAX engineering value 4kg/cm2 the WISE-4012 convert to 34255 signal value.&#x20;
3. Click the \[CALCULATE PARAMETERS(a,b)] button will get the value of a=12699, and b=5389.
4. Click the \[SAVE PARAMETER(a,b)] button will save to the database.

```
// IOTMB Example of JSON for settings of the sensors
{
  "_id": ObjectID(“5d2863ff95ba9022a4ce3fd9”),
  "DocID": "MCNode",
  "tcMCNo": "Q01",
  "tcMCName": "MC01",
  "toSensors": [
    {
      "tcTagId": "Temp01",
      "tcSensorName": "Temperature of Raspberry",
      "tcSensorType": "DHT22",
      "tcCtrl": "Raspberry Pi",
      "tcSensorAddr": "GPIO27",
      "tu_a": 0,
      "tu_b": 0,
      "tcDispUnit": "C",
      "tcMQTT": "192.168.60.101",
      "tcTopic": "Machine/Q01/Temp",
      "tcRemark": ""
    },
    {
      "tcTagId": "Humi01",
      "tcSensorName": "Humidity of Raspberry",
      "tcSensorType": "DHT22",
      "tcCtrl": "Raspberry Pi",
      "tcSensorAddr": "GPIO27",
      "tu_a": 0,
      "tu_b": 0,
      "tcDispUnit": "%RH",
      "tcMQTT": "192.168.60.101",
      "tcTopic": "Machine/Q01/Humi",
      "tcRemark": ""
    },
    {
      "tcTagId": "Sensor01",
      "tcSensorName": "Pressure of TR",
      "tcSensorType": "0-5V",
      "tcCtrl": "WISE-4012",
      "tcSensorAddr": "192.168.60.172/ai_value/slot_0/ch_3",
      "tu_a": 0,
      "tu_b": 1326.6194,
      "tcDispUnit": "kg/cm2",
      "tcMQTT": "192.168.60.101",
      "tcTopic": "Machine/Q01/TR",
      "tcRemark": ""
    },
    {
      "tcTagId": "Sensor02",
      "tcSensorName": "Pressure of DR",
      "tcSensorType": "0-5V",
      "tcCtrl": "WISE-4012",
      "tcSensorAddr": "192.168.60.172/ai_value/slot_0/ch_4",
      "tu_a": 0,
      "tu_b": 1326.6194,
      "tcDispUnit": "kg/cm2",
      "tcMQTT": "192.168.60.101",
      "tcTopic": "Machine/Q01/DR",
      "tcRemark": ""
    }
  ]
}

```



