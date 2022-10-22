# IIOT System Maintenance

The system maintenance for the IIOT to keep the system functional. The following are important actions:

* Issuing policies to the users, assigning roles and security responsibilities, and Training personnel.
* Implementing physical security infrastructure to protect the system. Include firewalls, antivirus, and anti-spyware. Define and control the access systems. Enhance security for the wireless communication system.
* Separate the entry point and server for the hierarchical level. For example, one Node-RED instance for the production department, and another Node-RED instance for the system management. Please reference chapter "[The I-IoT platform for SME](../getting-started/the-i-iot-platform-for-sme.md)".
* Time synchronization is very important. Include ERR/MES Server, EIP server, NAS, Edge device(ex. Raspberry pi, wireless sensing devices, DVR/NVR, IP Cam...).  A NAS can be the internal NTP server. &#x20;
* Keep the IIOT device list up to date. To avoid conflict and easy to maintain.
* As a user to run the system, to check the system is running well. Or design system dashboards for monitoring the status of the IIOT system. For example the status of gateways, and the latest time of the operation record of the workstation.
* Backup period, and prepare the backup system. And periodic maintain the IIOT database.

#### An example of  maintaining the  IIOT database(MongoDB)

![Periodic maintain the database(MongoDB)](<../.gitbook/assets/DB Maintain.jpg>)

