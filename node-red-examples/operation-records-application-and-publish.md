---
description: >-
  In this chapter will focus on how to record the operation status and records
  for the workstation
---

# Operation Records Application and Publish

### Operation status and records for the workstation

In the last chapter, we can collect the operation information through the discriminate factors. The commonly used is the speed or the power(kW or A) for example. We can set a value of speed to discriminate the workstation is changing the status RUN, STOP, or IDLING. Sometimes we need to use the value of power as the 2nd factor for discriminating.&#x20;

At the same time, we can calculate the accumulation time when the status is changing. The example flows following is for storing the changing status of the workstation, posting a LINE message to the team members if necessary, and storing the summary record of the last status.

![The example flow for store the operation records of the workstation](<../.gitbook/assets/Operation records-1 (1).jpg>)

The key point of this flow&#x20;

* Read document IOTTA the last status of the workstation, and save the status to flow or global variables, please reference the chapter ["Dashboard for Production"](dashboard-for-production.md).
* Read document IOTMB the sensor's settings of the workstation, and save the settings to flow or global variables, please reference the chapter ["Signal and PV Calibration"](signal-and-pv-calibration.md).&#x20;
* Read ERP data from the MS SQL database, in this example, the 1st information is the operating content(ex. production name, worker, and the lot number), and the 2nd information is the schedule of the workstation and displays on the dashboard.
* Line speed uses the Advantec WISE-4051 DI signal to get the frequency of the signal and convert the frequency number to line speed through the setting variables from IOTMB and displays on the dashboard.
* If the line speed is greater than the IOTTA's discriminate value(ex. 10M/min), the status will swap from STOP to RUN, and vice versa. The flow will save the new status to IOTTA, and create an operation summary record of the lastest status to the operation record table or document(ex. IOTTB). And then according to the settings, send the summary message to the members through Line(or other messenger ex. Twitter ).  The operation records can be used for the members to manage and improve the operation, and also to reveal the OEE by the [Timeline chart](time-line-chart.md).

```
// IOTTB example of operation record for the workstation 
// These records can trace the Max or Min engineer value of the key sensor. 
// For other sensor values of short time will be collected in 
//    the time serial data.
[
  {
    "_id": "61de7283052c90001082c1c9",
    "DocID": "MCRunLog",
    "tcMCNo": "H809",
    "tcLotno": "J12N0868",
    "tcERPData": "Item01 1170mm E033",
    "tcStatus": "STOP",
    "tcDate": "2022/01/12",
    "tdBegin": "2022-01-12T12:37:41.407Z",
    "tdEnd": "2022-01-12T13:02:31.053Z",
    "tuF1Change": 10, 
    "tuSec": "1490",
    "tuHour": "0.41",
    "tcCause": "[S3005 Change Roll]",
    "tcDes": "0.7kW",
    "tukWh": 2, 
    "tcToERP": ""
  },
  {
    "_id": "61de7283052c90001082c1c9",
    "DocID": "MCRunLog",
    "tcMCNo": "H809",
    "tcLotno": "J12N0859",
    "tcERPData": "Item01 1170mm E033",
    "tcStatus": "RUN",
    "tcDate": "2022/01/12",
    "tdBegin": "2022-01-12T13:02:31.053Z",
    "tdEnd": "2022-01-12T14:17:39.905Z",
    "tuF1Change": 0,
    "tuSec": "4509",
    "tuHour": "1.25",
    "tcCause": "",
    "tcDes": "22.7kW",
    "tukWh": 28.375,
    "tcToERP": ""
  }
]
```

### Query form to get the operation records

The query form can be designed by Node-RED or your EIP(Enterprise Information Portal) Web service(ex. ASP.NET form). This example can use the simple "like" query string.

![Example of query form for the operation records](<../.gitbook/assets/Operation Records Mtn Form.jpg>)

You can reference the flow which uses the HTML template and jsGrid to design the maintained UI for the settings. "Gridview with CRUD options (MongoDB)" [https://flows.nodered.org/flow/e32f2b942bce77ef6079c0642b93c036](https://flows.nodered.org/flow/e32f2b942bce77ef6079c0642b93c036)&#x20;

Following is the example flow

![Example flow of Operation recrods query web service by jsGrid](<../.gitbook/assets/Operation Records Query Web Service.jpg>)

![The Output of the Operation Records query](<../.gitbook/assets/Operation Records example.jpg>)

{% hint style="info" %}
In the function of SetupFindQuery, the msg.payload from http node is the query string.

Following is the sample code to get the query string

var queryStr=msg.payload;

var lcMCNo=queryStr.StrNo;

lcMCNo=LiketoRegex(lcMCNo);   // convert to like string

...

msg.payload= {

&#x20;   "tcMCNo":lcMCNo

&#x20;  ...

}
{% endhint %}

```
// Sample code of LiketoRegex

function LiketoRegex(likestr) {
// simple convert like to $regex 
// before transfer the http query string, the char "%" must be converted to "*".
  var lcstr=likestr;
  if (likestr.replace(/^\s+|\s+$/g,"")=="*") {
      // like %    likestr.trim  
      lcstr={$gte:"0"};
  }
  else {
      if (likestr.substr(likestr.length-1,1)=="*") {
        // like %ABC
       lcstr={$regex: "^"+likestr.replace("*",""),$options:"i"};  
       }
      else {
        if (likestr.substr(0,1)=="*") {
          // like ABC%
          lcstr={$regex: likestr.replace("*","")+"$",$options:"i"};  
         }
        else {
          if (likestr.indexOf(",")>0) {
            // like ABC%
            lcstr={$in: [likestr]};  
          }
        }
      }
  } 
  return lcstr;
}
```

More applications for the operation records of the workstation will be discussed in the chapter "[Time Serial Data Applications](time-serial-data-applications.md)".



