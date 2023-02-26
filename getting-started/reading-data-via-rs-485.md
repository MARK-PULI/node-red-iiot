# Reading data via RS-485

#### Requirements

* Raspberry Pi, PC, or IoT gateway with RS-485 Port.
* RS-485 station ID(Slave ID), the data address table, and data format.

#### The example flow(Temperature controller with RS-485, via Advantec WISE-4051 WiFi IoT wireless I/O)

![Advantec WISE-4051 DI and RS-485 Modus/RTU](<../.gitbook/assets/Advantec WISE-4051.jpg>)

![Advantec WISE-4051 configuration](<../.gitbook/assets/Advantec WISE-4051 RS-485 configuration.jpg>)

{% hint style="info" %}
1\. Slave ID can’t duplicate, and the correct configuration of RS-485 port:\
&#x20;   Baud Rate, Data Bit, Stop Bit and Parity.

2\. Correct wiring

3\. See the RS-485 documents of the sensors or controllers, \
&#x20;   maybe you must X 10  or use IEEE-754 floating-point converter for the \
&#x20;   data conversion.  \
&#x20;   For negative values some controllers use a maximum value 65535 for \
&#x20;   example. -1 will be 65535-1=65534 ; -2 will be 65535-2=65533 ...

4\. Bit or Word, in this example, is the Word. Check WISE-4051 word status

&#x20;   have response.
{% endhint %}

```javascript
// IEEE-754 floating-point conversion Step:1
// Received data from WISE-4051 COM1(RS-485) converting to string binary 
// msg:Flow Rate, msg1:Pressure, msg2:Cumulative Flow Rate 
// pass the result to Step:2 to convert
var MCData=msg.payload.ExpWord;
var inputH=Number(MCData[1].Val);
var inputL=Number(MCData[0].Val);
var msg1={};
var msg2={};
msg1.payload={
    LWord:inputL,
    HWord:inputH
}
msg.payload = '0b'+inputH.toString(2).padStart(16,"0")+inputL.toString(2).padStart(16,"0");
inputH=Number(MCData[7].Val);
inputL=Number(MCData[6].Val);
msg1.payload = '0b'+inputH.toString(2).padStart(16,"0")+inputL.toString(2).padStart(16,"0");
inputH=Number(MCData[21].Val);
inputL=Number(MCData[20].Val);
msg2.payload = '0b'+inputH.toString(2).padStart(16,"0")+inputL.toString(2).padStart(16,"0");
return [msg,msg1,msg2];
```

```javascript
// IEEE-754 floating-point conversion Step:2
/* Converts from an number, string, buffer or array representing an 
   IEEE-754 value
 * to a javascript float.
 * The following may be given in msg.payload:
 *      A string representing a number, which may be hex or binary
 *        examples, "1735"  "0x02045789"  0b01000000010010010000111111011011
 *      An integer value
 *      A four element array or buffer of 8 bit values, most significant byte first.
*/ 
// first make a number from the given payload if necessary
var msg1={};   //result
let intValue;
if (typeof msg.payload === "number") {
    intValue = msg.payload;
} else if (typeof msg.payload === "string") {
    intValue = Number(msg.payload);
} else if (msg.payload.length == 4) {
    // four byte array or buffer
    intValue = (((((msg.payload[0] << 8) + msg.payload[1]) << 8) + msg.payload[2]) << 8) + msg.payload[3];
} else {
    node.warn("Unrecognised payload type or length");
}
msg1.payload=msg.payload;
msg.payload = Number(Int2Float32(intValue)).toFixed(3);
global.set("fuB1SF",msg.payload);
return [msg1,msg];

function Int2Float32(bytes) {
    var sign = (bytes & 0x80000000) ? -1 : 1;
    var exponent = ((bytes >> 23) & 0xFF) - 127;
    var significand = (bytes & ~(-1 << 23));

    if (exponent == 128) 
        return sign * ((significand) ? Number.NaN : Number.POSITIVE_INFINITY);

    if (exponent == -127) {
        if (significand === 0) return sign * 0.0;
        exponent = -126;
        significand /= (1 << 22);
    } else significand = (significand | (1 << 23)) / (1 << 23);

    return sign * significand * Math.pow(2, exponent);
}
```

![The flow to get Power data and Temperature data of the machine](<../.gitbook/assets/Reading data via RS-485 by WISE-4051.jpg>)

Using \[inject] node, to set interval via \[http Request]\(Restful). Reading data, ex. 5 secs. This flow can install in Raspberry Pi, NAS, or IoT Gateway.
