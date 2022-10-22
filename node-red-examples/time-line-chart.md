---
description: >-
  In the previous chapters, we had been collecting the data from the
  workstations. This chapter will reveal the applications of these data.
---

# Time Line Chart

### Display the TimeLine Chart for the workstations

Requirements:

* The collected operation records from workstations, discuss in the chapter of [Operation Records Application and Publish](operation-records-application-and-publish.md).  In this sample, we use the table IOTTA and IOTTB to display the timeline chart.
* In the Node-RED install the package node "node-red-contrib-ui-timelines-chart". ![](<../.gitbook/assets/image (1).png>)

The Timeline chart sample will display below. Select the machine group(by the production process). The right-hand displays the OEE of the machine. Move the mouse on the specified period can display the operation status and the time span.&#x20;

![Example of Time line chart](<../.gitbook/assets/Timeline Chart-1.jpg>)

This package node supports the "Zoom" function. Click the "Reset Zoom" will come back to the original chart.

![The "Zoom" function of the timeline chart](<../.gitbook/assets/Timeline Chart-2.jpg>)

### The example flow of Timeline Chart

![Example flow of timeline chart](<../.gitbook/assets/Timeline Chart-3.jpg>)

* Store the DateTime setting search scope by the user to the global variables. We can design auto-refresh the chart by 15mins.&#x20;
* Get M/C Group data: Get the machine group data from [IOTTA](dashboard-for-production.md).

```
// Get M/C Group data from IOTTA
function LiketoRegex(likestr) {
// simple convert like to $regex
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

var lcMCNo;
var lcBegin,lcEnd;
lcMCNo=LiketoRegex("MCStatus_A*");  
lcBegin=global.get("fcDate_Bgn") + "T" + global.get("fcTime_Bgn") +":00.001Z";
lcEnd=global.get("fcDate_End") + "T" + global.get("fcTime_End") +":00.001Z";
msg.payload=[
        {
         "DocID":"MCRun",
         "TimeStamp":lcMCNo
        },{sort:{"TimeStamp":1}}
];
return msg;
```

* Get the M/C operation records from [IOTTB](operation-records-application-and-publish.md)

```
// Get the M/C operation records from IOTTB
function LiketoRegex(likestr) {
// simple convert like to $regex
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

var lcMCNo;
var lcBegin,lcEnd;
var lcMCStatus={};
lcMCStatus=msg.payload;
global.set("MCStatus",lcMCStatus);    //Save for lastest record
lcMCNo=LiketoRegex("A*");  
lcBegin=global.get("fcDate_Bgn") + "T" + global.get("fcTime_Bgn") +":00.001Z";
lcEnd=global.get("fcDate_End") + "T" + global.get("fcTime_End") +":00.001Z";
global.set("fdEnd",new Date(lcEnd));
msg.payload=[
        {
         "$and":[{
         "tcMCNo":lcMCNo,
         "$or":[{"tdBegin":{"$gte":new Date(lcBegin),"$lte":new Date(lcEnd)}},{"tdEnd":{"$gte":new Date(lcBegin),"$lte":new Date(lcEnd)}}]
           }]
        },{sort:{"tcMCNo":1,"tdBegin":1}}
];
return msg;
```

* Prepare the JSON data for the Timeline chart

```
// Example code of the JSON data for timeline chart
// Tips: caution about the begin and end of the time scope.
function MCStatus(fcCause) {
  //Convert the stop cause ID from IOTTB(data source ERP), 
  //return the display caption
  var lcCause;
  lcCause="2Stop";  //default
  try {
     if (fcCause.indexOf("S3004")>=0 || fcCause.indexOf("S3002")>=0) {
         lcCause="8Clean";   
     }
     if (fcCause.indexOf("S3005")>=0) {
         lcCause="3Change";   
     }
     if (fcCause.indexOf("S1001")>=0) {
         lcCause="5Hday";   
     }
     if (fcCause.indexOf("S3001")>=0) {
         lcCause="6Wait";   
     }
     if (fcCause.indexOf("S2007")>=0) {
         lcCause="7QC";   
     }
     if ((fcCause.indexOf("S2001")>=0) || (fcCause.indexOf("S2004")>=0)) {
         lcCause="4Fault";
     }
  } catch(err) {
      
  }
  
  return lcCause;
}

function CheckFD(paData,pcBegin,pcEnd) {
    //Check Datetime
   var laData={};
   var ldBegin,ldEnd;
   var lnBegin,lnEnd;
   var ldBeginC,ldEndC;
   var lnEndC;
   ldBegin=new Date(pcBegin);
   ldEnd=new Date(pcEnd);
   lnBegin=ldBegin.getTime();
   lnEnd=ldEnd.getTime();
   laData=paData;
   ldBeginC=new Date(laData.tdBegin);   
   ldEndC=new Date(laData.tdEnd);
   if ((ldBeginC.getTime()<=lnBegin) && (ldEndC.getTime()>lnBegin)) {
       laData.tdBegin=pcBegin;    //Set start time 
       laData.tuSecS=(ldEndC.getTime()-lnBegin)/1000;    //Reset begin time
   }
   
   return laData;
}


var Data=msg.payload;
var Datanow=global.get("MCStatus");   //lastest record
var msg1={};
var msg2={};
var msg3={};
var msg5={};
var msg6={};
var msgN1={};
var CData1,CData2,CData3,CData5,CData6,CDataN1;
var lcStatus,lcData;
var ldBegin,ldEnd;
var lnBegin,lnEnd;
var lcBegin,lcEnd;
var luRun1,luStop1;
var luRun2,luStop2;
var luRun3,luStop3;
var luRun5,luStop5;
var luRun6,luStop6;
var luRunN1,luStopN1;
var j1,j2,j3,j5,j6,jN1;
lcBegin=global.get("fcDate_Bgn") + "T" + global.get("fcTime_Bgn") +":00.001Z";
lcEnd=global.get("fcDate_End") + "T" + global.get("fcTime_End") +":00.001Z";
CData1=[];
CData2=[];
CData3=[];
CData5=[];
CData6=[];
CDataN1=[];
luRun1=0;
luRun2=0;
luRun3=0;
luRun5=0;
luRun6=0;
luRunN1=0;
luStop1=0;
luStop2=0;
luStop3=0;
luStop5=0;
luStop6=0;
luStopN1=0;
var i;
j1=0;
j2=0;
j3=0;
j5=0;
j6=0;
jN1=0;
for (i=0;i<Data.length;i++) {
   ldBegin=new Date(Data[i].tdBegin);
   ldEnd=new Date(Data[i].tdEnd);  
   ldBegin=ldBegin.getTime()-8*3600*1000;
   ldEnd=ldEnd.getTime()-8*3600*1000;   
   switch (Data[i].tcMCNo) {
       case "A01": 
           if (j1===0) {
               //Process first record
               Data[i]=CheckFD(Data[i],lcBegin,lcEnd);
               ldBegin=new Date(Data[i].tdBegin);
               ldEnd=new Date(Data[i].tdEnd);  
               ldBegin=ldBegin.getTime()-8*3600*1000;  //convert to UTC timestamp 
               ldEnd=ldEnd.getTime()-8*3600*1000;               
           }
          switch (Data[i].tcStatus) {
            case "RUN": 
               lcStatus="1RUN";
               luRun1=luRun1+parseInt(Data[i].tuSecS);
               break;
            case "STOP": 
               lcStatus=MCStatus(Data[i].tcCause);
               luStop1=luStop1+parseInt(Data[i].tuSecS);               
               break;
          }
          lcData={"timeRange":[ldBegin,ldEnd],'val':lcStatus};           
          CData1[j1]=lcData;
           j1++;
           break;
       case "A02":
           if (j2===0) {
               //Process first record
               Data[i]=CheckFD(Data[i],lcBegin,lcEnd);
               ldBegin=new Date(Data[i].tdBegin);
               ldEnd=new Date(Data[i].tdEnd);  
               ldBegin=ldBegin.getTime()-8*3600*1000;
               ldEnd=ldEnd.getTime()-8*3600*1000;               
           }
          switch (Data[i].tcStatus) {
            case "RUN": 
               lcStatus="1RUN";
               luRun2=luRun2+parseInt(Data[i].tuSecS);
               break;
            case "STOP": 
               lcStatus=MCStatus(Data[i].tcCause);
               luStop2=luStop2+parseInt(Data[i].tuSecS);               
               break;
          }
          lcData={"timeRange":[ldBegin,ldEnd],'val':lcStatus};           
          CData2[j2]=lcData;
           j2++;
           break;
       case "A03": 
           if (j3===0) {
               //Process first record
               Data[i]=CheckFD(Data[i],lcBegin,lcEnd);
               ldBegin=new Date(Data[i].tdBegin);
               ldEnd=new Date(Data[i].tdEnd);  
               ldBegin=ldBegin.getTime()-8*3600*1000;
               ldEnd=ldEnd.getTime()-8*3600*1000;               
           }
          switch (Data[i].tcStatus) {
            case "RUN": 
               lcStatus="1RUN";
               luRun3=luRun3+parseInt(Data[i].tuSecS);
               break;
            case "STOP": 
               lcStatus=MCStatus(Data[i].tcCause);
               luStop3=luStop3+parseInt(Data[i].tuSecS);               
               break;
          }
          lcData={"timeRange":[ldBegin,ldEnd],'val':lcStatus};           
          CData3[j3]=lcData;
           j3++;
           break;
       case "A05": 
           if (j5===0) {
               //Process first record
               Data[i]=CheckFD(Data[i],lcBegin,lcEnd);
               ldBegin=new Date(Data[i].tdBegin);
               ldEnd=new Date(Data[i].tdEnd);  
               ldBegin=ldBegin.getTime()-8*3600*1000;
               ldEnd=ldEnd.getTime()-8*3600*1000;               
           }
       
          switch (Data[i].tcStatus) {
            case "RUN": 
               lcStatus="1RUN";
               luRun5=luRun5+parseInt(Data[i].tuSecS);
               break;
            case "STOP": 
               lcStatus=MCStatus(Data[i].tcCause);
               luStop5=luStop5+parseInt(Data[i].tuSecS);               
               break;
          }
          lcData={"timeRange":[ldBegin,ldEnd],'val':lcStatus};           
          CData5[j5]=lcData;
           j5++;
           break;
       case "A06": 
           if (j6===0) {
               //Process first record
               Data[i]=CheckFD(Data[i],lcBegin,lcEnd);
               ldBegin=new Date(Data[i].tdBegin);
               ldEnd=new Date(Data[i].tdEnd);  
               ldBegin=ldBegin.getTime()-8*3600*1000;
               ldEnd=ldEnd.getTime()-8*3600*1000;               
           }
           
          switch (Data[i].tcStatus) {
            case "RUN": 
                lcStatus="1RUN";
               luRun6=luRun6+parseInt(Data[i].tuSecS);
               break;
            case "STOP": 
               lcStatus=MCStatus(Data[i].tcCause);
               luStop6=luStop6+parseInt(Data[i].tuSecS);               
               break;
          }
          lcData={"timeRange":[ldBegin,ldEnd],'val':lcStatus};           
          CData6[j6]=lcData;
           j6++;
           break;
       case "AN1": 
           if (jN1===0) {
               //Process first record
               Data[i]=CheckFD(Data[i],lcBegin,lcEnd);
               ldBegin=new Date(Data[i].tdBegin);
               ldEnd=new Date(Data[i].tdEnd);  
               ldBegin=ldBegin.getTime()-8*3600*1000;
               ldEnd=ldEnd.getTime()-8*3600*1000;               
           }
          switch (Data[i].tcStatus) {
            case "RUN": 
               lcStatus="1RUN";
               luRunN1=luRunN1+parseInt(Data[i].tuSecS);
               break;
            case "STOP": 
               lcStatus=MCStatus(Data[i].tcCause);
               luStopN1=luStopN1+parseInt(Data[i].tuSecS);               
               break;
          }
          lcData={"timeRange":[ldBegin,ldEnd],'val':lcStatus};           
          CDataN1[jN1]=lcData;
           jN1++;
           break;
   }
}
//Process lastest status (IOTTA)
var Today=new Date();
var lnCheck;
var ldCheck;
ldEnd=new Date(lcEnd);
if ((ldEnd.getTime())>=Today.getTime()) {
    ldEnd=Today;
}
i=0;
for (i=0;i<Datanow.length;i++) {
       ldBegin=new Date(Datanow[i].MCStatus.tdBegin);   
       lnBegin=ldBegin.getTime()-8*3600*1000;
       lnEnd=ldEnd.getTime();
       luSecs=(lnEnd-lnBegin)/1000;
       switch (Datanow[i].TimeStamp) {
           case "MCStatus_A01":
              if (luSecs>0) {
                switch (Datanow[i].MCStatus.tcStatus) {
                   case "RUN": 
                     lcStatus="1RUN";
                     luRun1=luRun1+parseInt(luSecs);
                     break;
                case "STOP": 
                     lcStatus=MCStatus(Datanow[i].MCStatus.tcCause);
                     luStop1=luStop1+parseInt(luSecs);               
                     break;
                }
                lcData={"timeRange":[lnBegin,lnEnd],'val':lcStatus};           
                CData1[j1]=lcData;
                j1++;                 
               }
               break;
           case "MCStatus_A02":
              if (luSecs>0) {
                switch (Datanow[i].MCStatus.tcStatus) {
                   case "RUN": 
                     lcStatus="1RUN";
                     luRun2=luRun2+parseInt(luSecs);
                     break;
                case "STOP": 
                     lcStatus=MCStatus(Datanow[i].MCStatus.tcCause);
                     luStop2=luStop2+parseInt(luSecs);               
                     break;
                }
                lcData={"timeRange":[lnBegin,lnEnd],'val':lcStatus};           
                CData2[j2]=lcData;
                j2++;                 
               }
               break;
           case "MCStatus_A03":
              if (luSecs>0) {
                switch (Datanow[i].MCStatus.tcStatus) {
                   case "RUN": 
                     lcStatus="1RUN";
                     luRun3=luRun3+parseInt(luSecs);
                     break;
                case "STOP": 
                     lcStatus=MCStatus(Datanow[i].MCStatus.tcCause);
                     luStop3=luStop3+parseInt(luSecs);               
                     break;
                }
                lcData={"timeRange":[lnBegin,lnEnd],'val':lcStatus};           
                CData3[j3]=lcData;
                j3++;                 
               }
               break;
           case "MCStatus_A05":
              if (luSecs>0) {
                switch (Datanow[i].MCStatus.tcStatus) {
                   case "RUN": 
                     lcStatus="1RUN";
                     luRun5=luRun5+parseInt(luSecs);
                     break;
                case "STOP": 
                     lcStatus=MCStatus(Datanow[i].MCStatus.tcCause);
                     luStop5=luStop5+parseInt(luSecs);               
                     break;
                }
                lcData={"timeRange":[lnBegin,lnEnd],'val':lcStatus};           
                CData5[j5]=lcData;
                j5++;                 
               }
               break;
           case "MCStatus_A06":
              if (luSecs>0) {
                switch (Datanow[i].MCStatus.tcStatus) {
                   case "RUN": 
                     lcStatus="1RUN";
                     luRun6=luRun6+parseInt(luSecs);
                     break;
                case "STOP": 
                     lcStatus=MCStatus(Datanow[i].MCStatus.tcCause);
                     luStop6=luStop6+parseInt(luSecs);               
                     break;
                }
                lcData={"timeRange":[lnBegin,lnEnd],'val':lcStatus};           
                CData6[j6]=lcData;
                j6++;                 
               }
               break;
           case "MCStatus_AN1":
              if (luSecs>0) {
                switch (Datanow[i].MCStatus.tcStatus) {
                   case "RUN": 
                     lcStatus="1RUN";
                     luRunN1=luRunN1+parseInt(luSecs);
                     break;
                case "STOP": 
                     lcStatus=MCStatus(Datanow[i].MCStatus.tcCause);
                     luStopN1=luStopN1+parseInt(luSecs);               
                     break;
                }
                lcData={"timeRange":[lnBegin,lnEnd],'val':lcStatus};           
                CDataN1[jN1]=lcData;
                jN1++;                 
               }
               break;
       }
}
//Caculate OEE
var lcEF1,lcEF2,lcEF3,lcEF5,lcEF6,lcEFN1;
try {
    lcEF1=(Number(((luRun1)/(luRun1+luStop1))*100).toFixed(1))+"%";
} catch(err) {
    lcEF1="NA";
}
try {
    lcEF2=(Number(((luRun2)/(luRun2+luStop2))*100).toFixed(1))+"%";
} catch(err) {
    lcEF2="NA";
}
try {
    lcEF3=(Number(((luRun3)/(luRun3+luStop3))*100).toFixed(1))+"%";
} catch(err) {
    lcEF3="NA";
}
try {
    lcEF5=(Number(((luRun5)/(luRun5+luStop5))*100).toFixed(1))+"%";
} catch(err) {
    lcEF5="NA";
}
try {
    lcEF6=(Number(((luRun6)/(luRun6+luStop6))*100).toFixed(1))+"%";
} catch(err) {
    lcEF6="NA";
}
try {
    lcEFN1=(Number(((luRunN1)/(luRunN1+luStopN1))*100).toFixed(1))+"%";
} catch(err) {
    lcEFN1="NA";
}
var lcDateTime;
var luSecs;
ldBegin=new Date(lcBegin);
ldEnd=new Date(lcEnd);
lnBegin=ldBegin.getTime();
lnEnd=ldEnd.getTime();
if (((lnEnd-lnBegin)/3600/1000)<24 ) {
    lcDateTime="HH:mm";
} else {
    lcDateTime="MM-DD";    
}
// join the JSON format
msg1.payload={ 
    dataItems:[{
        group:"1M/C",
        data:[{label:lcEF1,data:CData1}]
    },
    {
        group:"2M/C",
        data:[{label:lcEF2,data:CData2}]
    },
    {
        group:"3M/C",
        data:[{label:lcEF3,data:CData3}]
    },
    {
        group:"5M/C",
        data:[{label:lcEF5,data:CData5}]
    },
    {
        group:"6M/C",
        data:[{label:lcEF6,data:CData6}]
    },
    {
        group:"N1M/C",
        data:[{label:lcEFN1,data:CDataN1}]
    }
    ],
    settings:{
        xAxis:{
            tickFormat:"MM-DD",            
            startDateTime:new Date(lcBegin),
            endDateTime: new Date(lcEnd),
        }    
    }
};
msg.payload=ldBegin;
return [msg,msg1];
```

* The Timeline chart settings. The legends value must be the same as the data sent to the Timeline chart.

![Sent data must be match the value of lengends](<../.gitbook/assets/Timeline Chart-4.jpg>)

### Display trend of the operation data and output to Excel for further analysis







