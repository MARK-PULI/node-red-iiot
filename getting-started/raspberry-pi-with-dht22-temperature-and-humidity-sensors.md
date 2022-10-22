---
description: for monitoring the eviroment of Raspberry Pi
---

# Raspberry Pi with DHT22 Temperature and Humidity sensors

#### Requirements

* Raspberry Pi wired DHT22, in this example, we use GPIO27-Pin#13 to connect DHT22, some examples are connected on GPIO4(Pin#7).

![Raspberry Pi wired DHT22(connect the data wire on GPIO27-Pin#13)](<../.gitbook/assets/Raspberry Pi with DHT22.jpg>)

#### The example flow(Get the temperature and humidity from DHT22)

![Read Temperature and Humidity from Raspberry Pi with DHT22](<../.gitbook/assets/The flow for Raspberry Pi with DHT22 .jpg>)

{% hint style="info" %}
The DHT22 outputs will be the temperature in `msg.payload` and the humidity in `msg.humidity`.&#x20;

If you have many Raspberry Pi with DHT22, you can design a dashboard, to monitor the environment around the Rasp.
{% endhint %}



