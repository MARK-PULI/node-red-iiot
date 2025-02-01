# Survey and Study

{% hint style="info" %}


* the goals and purpose
* industrial process, logistics flow, and the information flow
* IT/OT infrastructure of the company
{% endhint %}

## Why use the I-IoT?

There are several reasons for the company to use I-IoT, and what are the priorities?&#x20;

&#x20;The following are several I-IoT use cases in the industry and their benefits:

* Operation and Logistics management: This includes all operations to increase the visibility of the supply chain, such as planning, production optimization, and quick response.
* Asset management: This includes monitoring and tracking the production, environment, energy areas, such as operation status, quality, performance, potential damage or breakdowns, bottlenecks, and so on.
* Remote monitoring and operation: This lets company can connect, monitor, and action for the operation work. Furthermore, send the messages of important information or warning to the staff and manager, to take action.
* Improve maintenance: To optimize machine availability, minimize interruption, and increase safety and security.
* Big data applications: Big data can be used to analyze the operation records, improve quality and enhance the outcomes. The applications of these analytics are as follows:
  * Diagnostic: Understanding the cause of a fault or stop.
  * Maintenance: Predicting and adjusting maintenance intervals to optimize scheduling.
  * Efficiency: Improving the performance of the production or the utilization of resources.
  * Prognostic: Providing insight to avoid faults or to maintain efficiency.
  * Optimization: Optimizing resource consumption or compliance with local government regulation.

## Industrial process, Logistics flow, and the information flow

We can study the industrial process and data flow of actives from the company. An industrial process can be defined as a set of operations that transforms, with a predetermined objective, the properties of one or more materials, types of energy, or information. The industrial process is represented in the following diagram:

![Industrial Process (Source:Hands-On Industrial Internet of Things)](<../.gitbook/assets/Industrial process.jpg>)

The industrial process is sequential. The inputs are raw materials, after the production process, the raw materials transform to WIP(Work in process).  and transportation to the next production process.  There are some reject or waste outputs. Each production process is made up of a sequence of operations: Making, Assembly, Transport and Storage, Testing, Coordination, and Control. The information will share with ERP. In the Cost Accounting view, these processes include the input of Direct Material Cost, Direct Labor Cost, and Overhead Cost, and the output of WIPs or finish products.&#x20;

The Information between these process are:

* Production plan and schedule
* Production orders&#x20;
* Production Standards or Instructions
* The Records during production, and the status or values of the control point of the machines or equipment.
* Quality Inspection or testing information
* Logistics  information
* Coordination information

This information helps us to design the I-IoT platform for the company.

## IT/OT/CT infrastructure of the company

The next task is to survey the IT/OT(Information Technology/Operation Technology) infrastructure of the company. We can use the architecture developed by Giacomo Veneri & Antonio Capasso.  Which are arranged according to the hierarchical structure in the CIM(Computer-Integrated Manufacturing) pyramid. Check the infrastructure of your company:

![I-IoT devices and protocols in the factory(Source:Hands-On Industrial Internet of Things)](<../.gitbook/assets/I-Iot in the factory.jpg>)

Level 1: Sensors, transducers, RTU(Remote terminal unit), and actuators, and the transmission of information network to Level 2.&#x20;

Level 2: PID controllers, Micro-Controller, Embedded controllers, CNCs,  PLCs, and DCSs. and what are the Fieldbus protocols? such as Modbus, Profibus, and ControlNet.

Level 3:SCADA, OPC Server and Historian. SCADA Presents information by computer or HMI(Human Machine Interface).&#x20;

Level 4:MES(Manufacturing Execution System) are computerized systems used in manufacturing to track and document the transformation of raw materials to finished goods. MES provides information that helps manufacturing decision-makers understand how current conditions on the plant floor can be optimized to improve production output.&#x20;

Level 5:ERP(Enterprise resource planning) is the integrated management of main business processes, such as accounting, marketing, procurement, project management, production, human resource management, and workflow management. often in real-time and mediated by software and technology. &#x20;

To study the Communication network in the company. And there are firewalls in the company? how are they working?

We can finish the checklist in the following:

### IT/OT infrastructure Checklist

![Infrastructure Check List](<../.gitbook/assets/I-IoT Infrastructure checklist.jpg>)

### Further Reading

"Hands-On Industrial Internet of Things: Create a powerful industrial IoT infrastructure using Industry 4.0", "Giacomo Veneri, Antonio Capasso"

![](<../.gitbook/assets/Hands-On IIOT.jpg>)

