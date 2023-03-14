# Reading data via RS-232 Port

#### Requirements

* Raspberry  Pi or PC or IoT gateway with RS-232 port, and installed Node-RED.
* The RS-232 transfer protocol and the data format of your device.

The following are two examples:\
1.An early NDC measurement system used for online basic weight measurement.\
2.An electronic scale with RS-232 transmission port.

#### The example flow(the early NDC measurement system for the online basic weight  measurement, the Node-RED flow installed in Raspberry Pi)

![Reading data via RS-232 port](<../.gitbook/assets/Reading data via RS-232.jpg>)

Setting the RS-232 Baud Rate, Data Bits, Parity, and Stop Bits. And the input or output setting you need.

Using \[debug] node to test received data stream, And using \[change] or \[function] node to get the value

Further processing ex.\
1\. \[MQ\_BW] Publish to MQTT SERVER\
2\. \[SetRS232MCX-1]  for dashboard\
&#x20;  dynamic to change the color, or\
3\. \[fgLED\_RunMCX] for Run/Stop/idling Status by setting color



#### The example flow(Electronic Scale with RS-232 Port). Output to MQTT for ERP working form based on ASP.NET

![Reading data via RS-232 for Electronic Scale](<../.gitbook/assets/Reading Data via RS-232 Electronic Scale.jpg>)

```
// codes in function node [GetTRNW]
var data=msg.payload;
var luWeight=0;
var msg1={};
if (data.indexOf("-")>=0) {
    //Get Tare
    //ST,NT,-  3.350KG
    //01234567890123456    
    try {
      luWeight=parseFloat(data.substr(7,5))
      global.set("fuTR",Number(luWeight).toFixed(1));        
    } catch (err) {

    }
} else {
    //Get Net Weight
    //US,NT,  16.565KG
    //01234567890123456    
    try {
      luWeight=parseFloat(data.substr(7,5))
      global.set("fuNW",Number(luWeight).toFixed(1));        
    } catch (err) {

    }
}
try {
  msg.payload=global.get("fuTR");    
} catch (err) {
  msg.payload=-1;  
}
try {
  msg1.payload=global.get("fuNW");    
} catch (err) {
  msg1.payload=-1;  
}
return [msg,msg1];
```

On the ASP.NET, receiving MQTT by MQTTnet(The codes are based on VB.NET), to solve the problem of the transfer RS-232 port data from the client side.

```
'VB.NET 
'Received data by MQTT protocol
Imports System.Threading
Imports System.Threading.Tasks
Imports MQTTnet
Imports MQTTnet.Client
Imports MQTTnet.Client.Options
Imports MQTTnet.Protocol
Imports MQTTnet.Server
Imports MQTTnet.Exceptions
Imports MQTTnet.Client.Receiving
Imports MQTTnet.Client.Disconnecting
Imports MQTTnet.Client.Connecting
Imports MQTTnet.Formatter

Public Class MOCTG2_Form_code
    Inherits System.Web.UI.Page
    Dim mqClient As MqttClient
    Dim mqFactory As New MqttFactory
    Dim mqType As Integer
    ...
   Private Sub Page_Load(ByVal sender As System.Object, ByVal e As EventArgs)
    ...
   End Sub
   
    Sub BtnGetTR_onclick(ByVal sender As Object, ByVal e As EventArgs) Handles BtnGetTR.Click
        'Get Tare
        mqType = 1
        Call P_MQTTService()
    End Sub

    Sub BtnGetWeight_onclick(ByVal sender As Object, ByVal e As EventArgs) Handles BtnGetWeight.Click
        'Get Net Weight
        mqType = 2    
        Call P_MQTTService()
    End Sub


    Sub P_MQTTService()
        Dim MQTTClientID As String
        Dim MQTTServer As New MQTTService("MQTTServer1")
        
        'Setting only for the IPs having Electronic Scale
        If InStr("192.168.60.1/192.168.60.2/", Trim(Session("strUserIP"))) = 0 Then
            Exit Sub
        End If

        Try
            MQTTClientID = Guid.NewGuid.ToString()
            Connect(MQTTClientID, MQTTServer.ServerIP, MQTTServer.Act, MQTTServer.Pwd).Wait()
        Catch ex As Exception
            js = "alert('Err：Can not connect MQTT SERVER!" & Replace(Err.Description, "'", "") & "');"
            JavaScript.AddJavaScript(Me.Page, js)
        End Try

        Try
            Select Case mqType
                Case 1
                    Select Case Trim(SessionClass.item("strUserIP"))
                        Case "192.168.60.1"
                            Subscribe("B2/Scale/01/TR").Wait()
                        Case "192.168.60.219"
                            Subscribe("B1/Scale/01/TR").Wait()
                    End Select
                Case 2, 3
                    Select Case Trim(SessionClass.item("strUserIP"))
                        Case "192.168.60.1"
                            Subscribe("Wks1/Scale/01/NW").Wait()
                        Case "192.168.60.2"
                            Subscribe("Wks2/Scale/01/NW").Wait()
                    End Select
            End Select
        Catch ex As Exception
            js = "alert('Error：Can not subscribe MQTT SERVER!" & Replace(Err.Description, "'", "") & "');"
            JavaScript.AddJavaScript(Me.Page, js)
        End Try
    End Sub

    Async Function Connect(Client As String, Server As String, User As String, Password As String) As Task
        mqClient = CType(mqFactory.CreateMqttClient(), MqttClient)
        mqClient.UseApplicationMessageReceivedHandler(AddressOf MessageRecieved)
        mqClient.UseDisconnectedHandler(AddressOf ConnectionClosed)
        mqClient.UseConnectedHandler(AddressOf ConnectionOpened)

        Dim Options As New Options.MqttClientOptionsBuilder
        Options.WithClientId(Client).WithTcpServer(Server, 1883).WithCredentials(User, Password).WithCleanSession().WithProtocolVersion(MqttProtocolVersion.V311)

        Await mqClient.ConnectAsync(Options.Build).ConfigureAwait(False)
    End Function

    Async Function Disconnect() As Task
        Await mqClient.DisconnectAsync().ConfigureAwait(False)
    End Function

    Async Function Publish(Topic As String, Payload As String) As Task
        Await mqClient.PublishAsync(Topic, Payload).ConfigureAwait(False)
    End Function

    Async Function Unsubscribe(Topic As String) As Task
        Await mqClient.UnsubscribeAsync(Topic).ConfigureAwait(False)
    End Function

    Async Function Subscribe(Topic As String) As Task
        Await mqClient.SubscribeAsync(Topic, MqttQualityOfServiceLevel.AtMostOnce).ConfigureAwait(False)
        'Me.MQTTStatus.Text = "Subscribing:" & Topic
        'delay for suitable time
        Await Task.Delay(1000)   
    End Function


    Private Sub MessageRecieved(e As MqttApplicationMessageReceivedEventArgs)
        Dim luTare As Double
        'Set the initial value
        If InStr("KG,KGS,G", UCase(Trim(Me.fcUnit.Text))) > 0 Then
            luTare = 1
        Else
            luTare = 30    'Pallet add TARE
        End If
        Select Case mqType
            Case 1
                'Get Tare
                Me.fuTare.Text = Text.Encoding.UTF8.GetString(e.ApplicationMessage.Payload)
            Case 2
                Try
                    luTare = CDbl(Me.fuTare.Text)
                Catch ex As Exception

                End Try
                'Get 
                If InStr("KG,KGS,G", UCase(Trim(Me.fcUnit.Text))) > 0 Then
                    Me.fuQty.Text = Text.Encoding.UTF8.GetString(e.ApplicationMessage.Payload)
                    Me.fuNW.Text = Me.fuQty.Text
                Else
                    Me.fuNW.Text = Text.Encoding.UTF8.GetString(e.ApplicationMessage.Payload)
                End If
                Try
                    Me.fuGW.Text = Format(CDbl(Me.fuQty.Text) + luTare, "0.0")
                Catch ex As Exception

                End Try
        End Select
        Disconnect().Wait()
    End Sub

    Private Sub ConnectionClosed(e As Disconnecting.MqttClientDisconnectedEventArgs)
        'Me.MQTTStatus.Text = "Disconnected"
    End Sub

    Private Sub ConnectionOpened(e As Connecting.MqttClientConnectedEventArgs)
        'Me.MQTTStatus.Text = "Connecting"
    End Sub   
   
    ...

End Class
```

