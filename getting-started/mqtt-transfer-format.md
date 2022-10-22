# MQTT transfer format



### MQTT transfer format

The recommended format depicts the following

```
// set data package of the machine 
msg.payload=[{
    tcMCID:”A01”,
    tcTagID:”A01*”,
    tcPath:”MCData/101/ALL”,
    tcTime:“yyyy-MM-dd hh:mm:ss”,
    toData:{
          tuSpeed: x,
          tuBW: y,
          tukW: z,
          …
   }
}];
// or only single data field
msg.payload=[{
    tcMCID:”A01”,
    tcTagID:”A01_BW”,
    tcPath:”MCData/101/BW”,
    tcTime:“yyyy-MM-dd hh:mm:ss”,
    tuValue: x
}];
// using the timestamp for transfer the data time for the subscriber
```

{% hint style="info" %}
MQTT publisher, avoid duplicates for one data source.&#x20;

QoS is set to 0(at most one) unless you need to confirm each data all transfer to the MQTT Server.
{% endhint %}
