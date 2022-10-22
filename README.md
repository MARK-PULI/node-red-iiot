---
description: 'I-IoT:Industrial Internet of Things  Node-RED:'
---

# I-IoT with Node-RED Platform

{% hint style="info" %}
This document introduces how to use the Node-RED platform, build the  I-IoT applications for your company in a private cloud. The key IoT technologies in this document are

1. NAS(Network Attached Storage) as the Node-RED server, MQTT server(Message Queuing Telemetry Transport), and DB server(we install MongoDB).
2. Raspberry pi installs Node-RED as the gateway and for edge computing.
3. Advantec WISE-4000 as the IoT sensing device for AI/DI or Modbus devices.
4. The main functions of the I-IoT platform.
5. System maintenance.

Here we only focus on the concept of designing the I-IoT platform, and not on coding. You can get many information and examples on the internet by using the search engine.


{% endhint %}

### Node-RED

On the official website, [https://nodered.org/](https://nodered.org/)  describes Node-RED as a flow-based development tool for visual programming developed originally by IBM for wiring together hardware devices, APIs, and online services as part of the Internet of Things. the features are

* Browser-based flow editing: makes it easy to wire together flows using the wide range of nodes in the palette. Flows can be then deployed to the runtime in a single click.
* Built on Node.js: The light-weight runtime is built on Node.js, taking full advantage of its event-driven, non-blocking model. This makes it ideal to run at the edge of the network on low-cost hardware such as the Raspberry Pi as well as in the cloud. With over 225,000 modules in Node's package repository, it is easy to extend the range of palette nodes to add new capabilities.
* Social Development: The flows created in Node-RED are stored using JSON which can be easily imported and exported for sharing with others. An online flow library allows you to share your best flows with the world.



## MQTT

On the official website, [https://mqtt.org/](https://mqtt.org/) describes MQTT as an OASIS standard messaging protocol for the Internet of Things (IoT). It is designed as an extremely lightweight publish/subscribe messaging transport that is ideal for connecting remote devices with a small code footprint and minimal network bandwidth. MQTT today is used in a wide variety of industries, such as automotive, manufacturing, telecommunications, oil and gas, etc.

## Contents

{% hint style="info" %}
### GETTING STARTED
{% endhint %}

{% content-ref url="getting-started/survey-and-study.md" %}
[survey-and-study.md](getting-started/survey-and-study.md)
{% endcontent-ref %}

{% content-ref url="getting-started/the-i-iot-platform-for-sme.md" %}
[the-i-iot-platform-for-sme.md](getting-started/the-i-iot-platform-for-sme.md)
{% endcontent-ref %}

{% content-ref url="getting-started/i-iot-sensing-devices-selection.md" %}
[i-iot-sensing-devices-selection.md](getting-started/i-iot-sensing-devices-selection.md)
{% endcontent-ref %}

{% content-ref url="getting-started/i-iot-sensing-device-connection-examples.md" %}
[i-iot-sensing-device-connection-examples.md](getting-started/i-iot-sensing-device-connection-examples.md)
{% endcontent-ref %}

{% hint style="info" %}
### Node-RED I-IoT Application Examples
{% endhint %}

{% content-ref url="node-red-examples/signal-and-pv-calibration.md" %}
[signal-and-pv-calibration.md](node-red-examples/signal-and-pv-calibration.md)
{% endcontent-ref %}

{% content-ref url="node-red-examples/dashboard-for-production.md" %}
[dashboard-for-production.md](node-red-examples/dashboard-for-production.md)
{% endcontent-ref %}

{% content-ref url="node-red-examples/operation-records-application-and-publish.md" %}
[operation-records-application-and-publish.md](node-red-examples/operation-records-application-and-publish.md)
{% endcontent-ref %}

{% content-ref url="node-red-examples/dashboard-for-workstation.md" %}
[dashboard-for-workstation.md](node-red-examples/dashboard-for-workstation.md)
{% endcontent-ref %}

{% content-ref url="node-red-examples/time-line-chart.md" %}
[time-line-chart.md](node-red-examples/time-line-chart.md)
{% endcontent-ref %}

{% content-ref url="node-red-examples/time-serial-data-applications.md" %}
[time-serial-data-applications.md](node-red-examples/time-serial-data-applications.md)
{% endcontent-ref %}

{% content-ref url="node-red-examples/alarm-record-and-publish.md" %}
[alarm-record-and-publish.md](node-red-examples/alarm-record-and-publish.md)
{% endcontent-ref %}

{% content-ref url="node-red-examples/iiot-system-maintenance.md" %}
[iiot-system-maintenance.md](node-red-examples/iiot-system-maintenance.md)
{% endcontent-ref %}

Contact:  s0215501@gmail.com  MARK TSAI
