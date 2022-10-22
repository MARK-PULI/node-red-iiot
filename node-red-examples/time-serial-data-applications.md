# Time Serial Data Applications

### Display the chart of trends and export to Excel for further analysis

#### Requirements:

* The table stored the data of sensors for the workstation. See the example in chapter "[Dashboard for Production](dashboard-for-workstation.md)". The example table name is IOTTC.&#x20;
* The [NodeRED Server](../getting-started/the-i-iot-platform-for-sme.md) installed the Excel package node. ![](../.gitbook/assets/image.png)
* In the config file of the NodeRED Server settings.js, the httpStatic directory has been set. This directory is for storing the generated excel file and for downloading the file.

### Example of the dashboard

![Example dashbord for time serial data application ](<../.gitbook/assets/Time serial data application-1.jpg>)

### &#x20;The example of download file of excel for further analysis

![Example of the Excel file](<../.gitbook/assets/Time serial data application-2.jpg>)

### The example flow

![Example flow of the time serial data application](<../.gitbook/assets/Time serial data application-3.jpg>)

{% hint style="info" %}
* The \[ExcelOK] node after generating excel finish, displays an "ok" message and set the file link. Another source is when changing the setting scope will clear the message.
* In the data series of operation factors(or parameters), you can add the control lines(the high limit, standard value, and the low limit). Of course, you must store the value for the control line in the system or from ERP/MES. In the example shown above, we have added the control lines.
* When generating the excel file, if the company implements the sensors, step by step. It is recommended to use _**try/catch**_ to add the items, to prevent errors.
{% endhint %}

```
// some example code in [IOTTCN1Excel] node add the try/catch to prevent errors 
...
CData="[";
for (i = 0; i < lnmsg; i++) {
    luBWS_Std=0.00;
    luBWS_H=0.00;
    luBWS_L=0.00;
    try {
       luBWS_Std=EData[i].tuBWS_Std;
       luBWS_H=EData[i].tuBWS_High;
       luBWS_L=EData[i].tuBWS_Low;
    }
    catch (err) {
        luBWS_Std=0.00;
        luBWS_H=0.00;
        luBWS_L=0.00;
    }
    lcData='{"M/C":"' + "N1M/C" +'",';
    lcData=lcData + '"Itemdes":"'+EData[i].tcERPData+'",';
    lcData=lcData + '"ID":"'+EData[i].tcIdno+'",';
    lcData=lcData + '"Start":"'+DateTimeS(EData[i].tdBegin)+'",';            
    lcData=lcData + '"End":"'+DateTimeS(EData[i].tdEnd)+'",';     
    lcData=lcData + '"Time":"'+DateTimeS(EData[i].tdBegin).substr(11,5)+'",';
    lcData=lcData + '"BW_STD":'+Number(luBWS_Std).toFixed(1) + ',';
    lcData=lcData + '"BW_High":'+Number(luBWS_H).toFixed(1) + ',';
    lcData=lcData + '"BW_Low":'+Number(luBWS_L).toFixed(1) + ',';    
    lcData=lcData + '"BW_AVG":'+Number(EData[i].tuBW_A).toFixed(1)+',';
    lcData=lcData + '"BW_MAX":'+Number(EData[i].tuBW_H).toFixed(1)+',';
    lcData=lcData + '"BW_MIN":'+Number(EData[i].tuBW_L).toFixed(1)+',';    
    lcData=lcData + '"BW_Count":'+EData[i].tuBW_N+',';
    ...
    // The system installed in 2020/10/20
    // This six sensors installed in 2021/12/30 
    try {     //to avoid error for searching scope are before 2021/12/30
      lcData=lcData + '"#1PMPLevel":'+Number(EData[i].tuDPT1).toFixed(2)+',';
      lcData=lcData + '"#2PMPLevel":'+Number(EData[i].tuDPT2).toFixed(2)+',';              
      lcData=lcData + '"#3PMPLevel":'+Number(EData[i].tuDPT3).toFixed(2)+',';        
      lcData=lcData + '"Touch_L":'+Number(EData[i].tuTouchPL).toFixed(2)+',';
      lcData=lcData + '"Touch_R":'+Number(EData[i].tuTouchPR).toFixed(2)+',';              
      lcData=lcData + '"Press_L":'+Number(EData[i].tuPressPL).toFixed(2)+',';
      lcData=lcData + '"Press_R":'+Number(EData[i].tuPressPR).toFixed(2)+',';                          
    }   
    catch (err) {
        
    }
    lcData=lcData + '"Status":"'+EData[i].tcToERP+'"}';      
    if (i < (lnmsg-1)) {
        CData=CData+lcData+",";
    } else {
        CData=CData+lcData;        
    }
} 
CData=CData+']';
msg1.payload=CData;
msg1.filepath="/NodeRED_Static/MCN1Log.xlsx";   //for export excel
var msg3={payload:""};
msg3.payload="/MCN1Log.xlsx";    //for download link
return [msg1,msg3]
```
