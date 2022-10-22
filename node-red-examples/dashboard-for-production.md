---
description: >-
  In this chapter, we will discuss developing a dashboard of the summary
  operation information from one machine to the whole production department.
---

# Dashboard for Production

### Summary operation information of the machine

In the chapter "[Survey and Study](../getting-started/survey-and-study.md)", we discovered that the IIoT lets company can connect, monitor, and action for the operation work. Furthermore, send the messages of important information or warning to the staff and manager, to take action. The information we want to monitor is some items listed following.&#x20;

* What production operations or products are produced in the workstation. We can get this information from ERP or MES.
* Operation Status(RUN/STOP/IDLING), the time accumulation of status, and the cause of STOP. The efficiency or performance of the workstation. We will discuss this in the next section.
* The outputs, speed, or important data of the workstation. We can get this information by sensors, and the information from ERP or MES if used online work report.&#x20;
* The following schedule of the workstation. This schedule comes from ERP or MES.
* The snapshot photo of the workstation. We can select the source of snapshot photos discussed in the chapter "[Get the snapshot from DVR or IP CAM](../getting-started/get-the-snapshot-from-dvr-or-ip-cam.md)".
* A RUN/STOP button signal is pressed by the operator manual.

#### Requirement

* NAS for running the Node-RED flow.
* Raspberry Pi or PC to display the dashboard.

### The example of a dashboard for the production department

![Dashboard for production department(running at NAS)](<../.gitbook/assets/Summary information for production(Dashboard).jpg>)

For matching the screen size of smartphones, the layout of the dashboard on Node-RED is recommended to 6(W)X8(H)\~6X9. The layout will be better when put together on the dashboard for the control room, and also can display on smartphones suitably.

* 6X1--The production item from ERP
* 1X1--Led of operation status;2X1--Operation status and the time accumulation;3X1--Line speed and the standard.
* 3X1--the standard basic weight;1X1--Led of basic weight status;2X1--basic weight from the BW gauge.
* 6X5--snapshot photo of the output area of the workstation.

![Summary information of the production for one workstation](<../.gitbook/assets/Summary information for production.jpg>)

### How to discriminate the operation status(RUN/STOP/IDLING) of the machine automatically, and to accumulate the time

The following is a list of several factors we can be used to discriminate the operation status.

* line speed
* electronic power
* the data read from the online gauge, basic weight gauge ex.
* web breaking sensor
* the source of tower light for the machine
* communication with PLC, SCADA, or DCS.

We can create a setting table(document name: IOTTA) for the workstations, and store it in the database. The benefit is to avoid the unpredictable shutdown or restart the Node-RED, we can restore the status by reading the setting table.

```
// IOTTA: the setting document example for each workstation
{
  "_id": ObjectID("585a4732ac11400192a60b85"),
  "DocID":"MCRun",
  "tcMCNo":"001",
  "tcStatus":"RUN",
  "tcItemdes":"Prod A",
  "tcLotno":"202112001",
  "tdChange":"2021-12-25T06:53:18.322",
  "tdBegin":"2021-12-25T23:59:59.614",
  "tcStatusDes":"",
  "tuFactor0":8,
  "tcFactor0ON":"Y",
  "tuFactor1":20,
  "tcFactor1ON:"Y",
  "tuFactor2":0,
  "tcFactor2ON":"N",
  "tcLogger":"Y",
  "tcRemark":"Factor0:Basic weight, Factor1:line speed, Factor2:kW of power"
} 
// tcStatus: RUN/STOP status of the workstation
// tdChange: the time of last status changing of the workstation.
//           The accumulation of time for the current status is calculated by
//           (TimeNow-tdChange).
// tdBegin: the time of last status changing of the workstation, including the change of date.
//          tdBegin is for calculating the distinguish time by date, it will save
//          the time at 23:59:59 for every day or the truly changing of status.
// tcStatusDes: for save the description status of STOP, "wait for material", 
//          "Equipment failure", "Changeover" ex. the message come from ERP or MES
// tuFactorx: the engineering value if less than, then STOP, if greater then RUN.
//           tuFactor0,1,2 is priority of the factors.
// tcFactorxON: Y--ON  N--OFF 
//tcLogger: save the logger or not.  
```

{% hint style="info" %}
You can reference the flow which uses the HTML template and jsGrid to design the maintain UI for the settings. "Gridview with CRUD options (MongoDB)" [https://flows.nodered.org/flow/e32f2b942bce77ef6079c0642b93c036](https://flows.nodered.org/flow/e32f2b942bce77ef6079c0642b93c036)
{% endhint %}

![The Example of Operation Parameters Setting(by jsGrid)](<../.gitbook/assets/Operation Parameters Setting.jpg>)

### The example flow for operation status of the workstation

![example flow for operation status of the workstation](<../.gitbook/assets/The Flow for Operation Status.jpg>)

In the next chapter, we will discuss how to record operation records and update the status of the workstation.

