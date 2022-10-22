# Reading data from 3rd Web Service

#### Requirements

* 3rd party, edge device, or another system provides Web Service.

#### The example flow(Reading data from Central Weather Bureau Taiwan)

![Get the Rainfall data of 埔里(Puli,Taiwan) from CWB](<../.gitbook/assets/Reading data via Web Service.jpg>)

![Setting the URL of CWBrainData\_埔里](<../.gitbook/assets/Reading data from Web Service(CWB)  .jpg>)

{% hint style="info" %}
The API method of URL from the Government Open Data Platform can get from the website [https://data.gov.tw/en](https://data.gov.tw/en)&#x20;
{% endhint %}

```
// code in GetData
var CWBData=msg.payload;
var locationdata=CWBData.records.location;
var data=locationdata[0].weatherElement;
var msg1={};
var msg2={};
var msg3={};
var q;
try {
  q=parseInt(data[1].elementValue);    
}
catch (err) {
  q=0;  
}

if (q<0) {
    q=0;
}
msg.payload=Number(q).toFixed(1);
msg.topic="hour Rainfall";
try {
  q=parseInt(data[3].elementValue);    
}
catch (err) {
  q=0;  
}

if (q<0) {
    q=0;
}
msg1.payload=Number(q).toFixed(1);
msg1.topic="3hr Rainfall";
try {
  q=parseInt(data[7].elementValue);    
}
catch (err) {
  q=0;  
}

if (q<0) {
    q=0;
}
msg2.payload=Number(q).toFixed(1);
msg2.topic="Total Today";
msg3.payload=locationdata[0].time.obsTime;
msg3.topic="CWB Time:";
return [msg,msg1,msg2,msg3];
```

