---
description: >-
  This chapter will focus on how to design dashboard for the workstation, record
  the sensors data of the workstation, and how to export the data to excel for
  big data applications.
---

# Dashboard for WorkStation

In the chapter "Dashboard of Production", we design a dashboard for the production department, which emphasizes the summary information of the machines. This chapter depicts a production workstation by the flow.

### Example of the dashboard for a workstation

![example of the dashboard for workstation](<../.gitbook/assets/Dashboard of workstation.jpg>)

![The example flow of workstation status](<../.gitbook/assets/The Flow for Operation Status.jpg>)

### The function of this flow

* Production statistics and ERP information: Reading information from the MQTT of ERP or the SQL database of ERP.
* Getting the factors of the workstation status.&#x20;
* Monitor the values of sensors, and display them on the line chart or the suitable dashboard. You can arrange the dashboard by production flow and the relationship of the sensors.
* The snapshot photo of the important areas.
* Alarm or warning of the except events.
* Recording the values of sensors for the workstation.

{% hint style="info" %}
When using a line chart, the data points(depending on the received interval and the time setting of the x-axis) will influence the loading of the Node-RED system. It's recommended in 1-2 hours. Sometimes you need to split them into another tab.

If you need to monitor longer time. You can design another tab for the users. This topic will discuss in the chapter "[Time Serial Data Applications](time-serial-data-applications.md)".
{% endhint %}

Historian: Recording the values of sensors for the IIOT analytics

We can record the values of sensors with the keys of production or the status of assets (ex. workstation id, product id, lot number, employee id, or timestamp). These digital representations of the measures are usually called tags or data points. There are three methods to store the values. \
1\. To store the value for each sensor by interval(ex. 2secs). And set the relationship between the senors with the workstation or the owner. \
2\. To store the value of sensors by workstation at each interval(ex. 1min).  You can gather the data of sensors per minute or high/low value per minute. \
3\. To store the value of sensors by connection gateway at each interval. And send out via MQTT for another flow.&#x20;

{% hint style="info" %}
Because of the high volume of data, we need to plan the network and the database. Which makes them resilient against the instability of network connectivity. It's recommended to use a different subnet with ERP. And use the level 3 hubs to manage the network.&#x20;
{% endhint %}

Example of storing the values of sensors in the workstation by JSON. In this case, use Raspberry Pi as the edge computing and gather data from the controllers or gateways. Use the MongoDB run on   NAS.

```
// Tablename: IOTTC Example of storing the values of sensors in the workstation
[
{
   tcMCNo:”MC01”,
   tcLotno:”1J03001”,
   tcERPData:”ERP Item”
   tcDate:”YYYY/MM/DD”,
   tcTime:"12:37",
   tdBegin:"2022-01-12T12:37:41.407Z",
   tdEnd:"2022-01-12T12:38:42.794Z",
   tcRem:”XXX”,
   tuData01H:9,         //Highest value at tcTime
   tuData01A::8.75,     //Average value at tcTime
   tuData01L:8.5,       //Lowest value at tcTime
   tuData01S:31,        //Number of the sample
   tuData02:100,
   tuData03:2.3,
   …
},
{
   tcMCNo:”MC01”,
   tcLotno:”1J03001”,
   tcERPData:”ERP Item”
   tcDate:”YYYY/MM/DD”,
   tcTime:"12:38",
   tdBegin:"2022-01-12T12:38:42.794Z",
   tdEnd:"2022-01-12T12:39:42.977Z",
   tcRem:”XXX”,
   tuData01H:9.2,       //Highest value at tcTime
   tuData01A::8.9,      //Average value at tcTime
   tuData01L:8.8,       //Lowest value at tcTime
   tuData01S:31,        //Number of the sample
   tuData02:100,
   tuData03:2.3,
   …
},
...
]
```



The common format for the different kinds of machine data can be designed to the format below. It's easier to design the common chart or to export excel.

```
// Tablename:IOTTD common format for the sensor data of the machine
// the array series is the label of the data.
[
{
   tcMCNo:”MC01”,
   tcLotno:”1J03001”,
   tcERPData:”ERP Item”
   tcDate:”YYYY/MM/DD”,
   tcTime:"12:37",
   tdBegin:"2022-01-12T12:37:41.407Z",
   tdEnd:"2022-01-12T12:38:42.794Z",
   series:["kW","kWh","Speed","Temperature1","Temperature2","Temperature3"],
   data:[20.5,13255,105.6,201.3,202.1,201.5],
   tcRem:”XXX”   
},
...
]

```
