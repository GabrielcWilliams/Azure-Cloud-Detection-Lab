# Azure Cloud Detection Lab

This project demonstrates how to configure and deploy a cloud-based lab environment in Microsoft Azure for exploring Microsoft Sentinel, a Security Information and Event Management (SIEM) solution. The lab provides hands-on experience with Azure resources, log collection, security analysis, and detection implementation using KQL (Kusto Query Language).

---

## Overview

Microsoft Sentinel provides a centralized platform for security analytics, threat detection, and incident response. This lab focuses on:

- Deploying and configuring Azure resources such as Virtual Machines and Log Analytics Workspace.
- Collecting and analyzing logs from Windows Security Events.
- Creating custom detection rules mapped to the MITRE ATT&CK framework.
- Using KQL for querying and analyzing data.

> **Note:** This lab utilizes a free Azure subscription and the 30-day free trial for Microsoft Sentinel. Users are encouraged to shut down resources when not in use to minimize costs.

---

## Lab Setup

### Create a Resource Group

Resource groups in Azure serve as logical containers for managing resources. The first step is to create a resource group:

1. Search for **Resource Group** in the Azure portal search bar.
2. Provide the required information, including the name and region, and click **Create**.

![Resource Group Creation](./images/resource grou.png)

---

### Deploy a Virtual Machine

A Windows 10 Virtual Machine (VM) is deployed for log collection:

1. Search **Virtual Machine** in the Azure portal and click **Create**.
2. Use the previously created resource group.
3. Fill in the required fields, including admin username and password.
4. Use the default settings for disks, networking, and management.
5. Click **Review + Create**.

![Virtual Machine Overview](./images/labvm creation.png)

---

## Network and VM Security

### Configuring Just-in-Time (JIT) Access

To reduce the attack surface and secure the VM:

1. Enable **Just-in-Time Access** through Microsoft Defender for Cloud.
2. Restrict RDP access to specific IPs for a limited time.
3. Verify updated rules in the VM's Networking tab.

![Just-in-Time Access Configuration](./images/microsoft denfnder.png)

---

## Log Analytics and Sentinel Deployment

### Create a Log Analytics Workspace

A Log Analytics Workspace is required to collect and store log data:

1. Search **Log Analytics Workspace** in the Azure portal.
2. Create the workspace within the same resource group as the VM.

---

### Deploy Microsoft Sentinel

Microsoft Sentinel is added to the Log Analytics Workspace:

1. Search **Microsoft Sentinel** in the Azure portal.
2. Add Sentinel to the previously created workspace.

![Sentinel Deployment](./images/log analytics.png)

---

## Data Collection and Event Analysis

### Configuring Data Connectors

To bring Windows Security Events into Sentinel:

1. Navigate to **Data Connectors** in Sentinel.
2. Search for "Windows Security Events via AMA."
3. Create a **Data Collection Rule** and link it to the VM.

![Data Connectors Setup](./images/connector for windows events.png)

---

### Event Analysis

Security events can be observed in the **Event Viewer** on the VM. For instance, Event ID 4624 represents successful logins. These events are queried and analyzed in Sentinel using KQL.

![Event Viewer Logs](./images/running a simple KQL query from an actual event.png)

---

## Custom Detection Rules

### Scheduled Task Monitoring

This section demonstrates how to create a custom analytic rule to detect Event ID 4698 (scheduled task creation):

1. Enable logging for scheduled tasks in the **Local Security Policy** on the VM.
2. Create a scheduled task using **Windows Task Scheduler**.
3. Write the following KQL query in Sentinel to detect scheduled task events:
   ```kql
   SecurityEvent                             
   | where EventID == 4698
   | parse EventData with * '<Data Name="TaskName">' TaskName '</Data>'
   | project TimeGenerated, Computer, TaskName
   ```
4. Configure analytic rules to generate alerts for these events.

![Analytic Rule Creation](./images/creating a analytic rule.png)

![Custom Rule Logic](./images/creating the rule logic.png)

---

## MITRE ATT&CK Mapping

This lab aligns with the **TA0003 - Persistence** tactic from the MITRE ATT&CK framework, specifically focusing on the **T1053.005 - Scheduled Task/Job** technique. The lab also demonstrates detection and mitigation strategies, including:

- Monitoring Event ID 4698 for scheduled tasks.
- Restricting user account privileges to prevent unauthorized task creation.

![MITRE ATT&CK Mapping](./images/accessed vm with remote desktop on mac.png)

---

## Conclusion

This project highlights the foundational skills required for working with SIEM tools such as Microsoft Sentinel. It provides hands-on experience with Azure resources, log analysis, and custom detection rule creation. By following this guide, users can gain valuable insights into real-world security operations.

> **Reminder:** Delete unused resources to avoid incurring additional costs.

---

### Next Steps:

1. Clone this repository to follow along with the guide.
2. Upload the images to the `images` folder in the repository to ensure they display correctly.
3. Test each step to familiarize yourself with the lab environment.
