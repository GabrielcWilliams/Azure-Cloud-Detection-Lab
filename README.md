# Azure-Cloud-Detection-Lab
This project demonstrates how to configure and deploy a cloud-based lab environment in Microsoft Azure for exploring Microsoft Sentinel, a Security Information and Event Management (SIEM) solution. The lab provides hands-on experience with Azure resources, log collection, security analysis, and detection implementation using KQL (Kusto Query Language).

Overview
Microsoft Sentinel provides a centralized platform for security analytics, threat detection, and incident response. This lab focuses on:

Deploying and configuring Azure resources such as Virtual Machines and Log Analytics Workspace.
Collecting and analyzing logs from Windows Security Events.
Creating custom detection rules mapped to the MITRE ATT&CK framework.
Using KQL for querying and analyzing data.
Note: This lab utilizes a free Azure subscription and the 30-day free trial for Microsoft Sentinel. Users are encouraged to shut down resources when not in use to minimize costs.

Lab Setup
Create a Resource Group
Resource groups in Azure serve as logical containers for managing resources. The first step is to create a resource group:

Search for Resource Group in the Azure portal search bar.
Provide the required information, including the name and region, and click Create.

Deploy a Virtual Machine
A Windows 10 Virtual Machine (VM) is deployed for log collection:

Search Virtual Machine in the Azure portal and click Create.
Use the previously created resource group.
Fill in the required fields, including admin username and password.
Use the default settings for disks, networking, and management.
Click Review + Create.

Network and VM Security
Configuring Just-in-Time (JIT) Access
To reduce the attack surface and secure the VM:

Enable Just-in-Time Access through Microsoft Defender for Cloud.
Restrict RDP access to specific IPs for a limited time.
Verify updated rules in the VM's Networking tab.

Log Analytics and Sentinel Deployment
Create a Log Analytics Workspace
A Log Analytics Workspace is required to collect and store log data:

Search Log Analytics Workspace in the Azure portal.
Create the workspace within the same resource group as the VM.
Deploy Microsoft Sentinel
Microsoft Sentinel is added to the Log Analytics Workspace:

Search Microsoft Sentinel in the Azure portal.
Add Sentinel to the previously created workspace.

Data Collection and Event Analysis
Configuring Data Connectors
To bring Windows Security Events into Sentinel:

Navigate to Data Connectors in Sentinel.
Search for "Windows Security Events via AMA."
Create a Data Collection Rule and link it to the VM.

Event Analysis
Security events can be observed in the Event Viewer on the VM. For instance, Event ID 4624 represents successful logins. These events are queried and analyzed in Sentinel using KQL.

Custom Detection Rules
Scheduled Task Monitoring
This section demonstrates how to create a custom analytic rule to detect Event ID 4698 (scheduled task creation):

Enable logging for scheduled tasks in the Local Security Policy on the VM.
Create a scheduled task using Windows Task Scheduler.
Write the following KQL query in Sentinel to detect scheduled task events:
kql
SecurityEvent                             
| where EventID == 4698
| parse EventData with * '<Data Name="TaskName">' TaskName '</Data>'
| project TimeGenerated, Computer, TaskName
Configure analytic rules to generate alerts for these events.

MITRE ATT&CK Mapping
This lab aligns with the TA0003 - Persistence tactic from the MITRE ATT&CK framework, specifically focusing on the T1053.005 - Scheduled Task/Job technique. The lab also demonstrates detection and mitigation strategies, including:

Monitoring Event ID 4698 for scheduled tasks.
Restricting user account privileges to prevent unauthorized task creation.

Conclusion
This project highlights the foundational skills required for working with SIEM tools such as Microsoft Sentinel. It provides hands-on experience with Azure resources, log analysis, and custom detection rule creation. By following this guide, users can gain valuable insights into real-world security operations.

Reminder: Delete unused resources to avoid incurring additional costs.
