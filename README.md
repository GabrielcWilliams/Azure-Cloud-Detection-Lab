# Azure Cloud Detection Lab

This project is a hands-on lab designed to explore how to configure and deploy a cloud-based environment in Microsoft Azure. The goal of this lab is to gain experience with Microsoft Sentinel, a Security Information and Event Management (SIEM) solution, while learning key concepts like log collection, security analysis, and detection implementation using KQL (Kusto Query Language).

---

## Overview

Microsoft Sentinel is a powerful tool that helps centralize security data, detect potential threats, and respond quickly to incidents. This lab focuses on providing practical experience with:

- Setting up Azure resources such as Virtual Machines and Log Analytics Workspace.
- Collecting and analyzing Windows Security Events to identify activity patterns.
- Writing custom detection rules to simulate real-world scenarios.
- Mapping detection techniques to the MITRE ATT&CK framework.
- Using KQL to query log data efficiently.

The lab is designed to be beginner-friendly and can be completed using Azure's free trial account. It's a great way to understand how cloud-based security tools are used in real-world scenarios.

---

## Lab Setup

### Create a Resource Group

Resource groups in Azure are used to organize resources logically. Follow these steps to create one:

1. Search for **Resource Group** in the Azure portal search bar.
2. Provide a name, select a region, and click **Create**.

![Resource Group Creation](./resource%20grou.png)

---

### Deploy a Virtual Machine

A Windows 10 Virtual Machine (VM) is created for collecting and analyzing security data:

1. Search for **Virtual Machine** in the Azure portal and click **Create**.
2. Select the previously created resource group.
3. Fill in the required fields (e.g., admin username and password).
4. Use default settings for disks, networking, and management.
5. Click **Review + Create**.

![Virtual Machine Overview](./labvm%20creation.png)

---

## Network and VM Security

### Configuring Just-in-Time (JIT) Access

To minimize security risks, enable Just-in-Time (JIT) access to the VM:

1. Go to **Microsoft Defender for Cloud** in the Azure portal.
2. Enable JIT for the VM, restricting RDP access to specific IPs for limited timeframes.
3. Verify updated rules in the Networking tab of the VM.

![Just-in-Time Access Configuration](./microsoft%20denfnder.png)

---

## Log Analytics and Sentinel Deployment

### Create a Log Analytics Workspace

The Log Analytics Workspace is where log data will be stored and analyzed:

1. Search **Log Analytics Workspace** in the Azure portal.
2. Create the workspace within the same resource group as the VM.

![Log Analytics Workspace Creation](./log%20analytics.png)

---

### Deploy Microsoft Sentinel

Microsoft Sentinel connects to the Log Analytics Workspace for managing and analyzing log data:

1. Search for **Microsoft Sentinel** in the Azure portal.
2. Add Sentinel to the previously created workspace.

![Microsoft Sentinel Deployment](./log%20analytics.png)

---

## Data Collection and Event Analysis

### Configuring Data Connectors

Data Connectors allow you to bring logs from the VM into Sentinel:

1. Navigate to the **Data Connectors** tab in Sentinel.
2. Search for "Windows Security Events via AMA."
3. Create a **Data Collection Rule** and link it to the VM.

![Data Connectors Setup](./connector%20for%20windows%20events.png)

---

### Event Analysis

Security events can be analyzed using the Event Viewer on the VM. For example:

- Event ID 4624 indicates a successful login.
- Query logs using KQL in Sentinel to find and analyze these events.

![Event Viewer Logs](./running%20a%20simple%20KQL%20query%20from%20an%20actual%20event.png)

---

## Custom Detection Rules

### Scheduled Task Monitoring

This section demonstrates how to detect the creation of scheduled tasks (Event ID 4698):

1. Enable logging for scheduled tasks in the **Local Security Policy** on the VM.
2. Create a scheduled task using **Windows Task Scheduler**.
3. Use the following KQL query in Sentinel to detect scheduled task events:
   ```kql
   SecurityEvent                             
   | where EventID == 4698
   | parse EventData with * '<Data Name="TaskName">' TaskName '</Data>'
   | project TimeGenerated, Computer, TaskName
   ```
4. Configure Sentinel to alert when these events are detected.

![Analytic Rule Creation](./creating%20a%20analytic%20rule.png)

![Custom Rule Logic](./creating%20the%20rule%20logic.png)

---

## MITRE ATT&CK Mapping

This lab ties into the **TA0003 - Persistence** tactic in the MITRE ATT&CK framework, specifically the **T1053.005 - Scheduled Task/Job** technique. It highlights how adversaries might abuse scheduled tasks to maintain persistence.

Detection strategies include:

- Monitoring Event ID 4698 to identify task creation.
- Restricting privileges to limit task creation to authorized accounts.

![MITRE ATT&CK Mapping](./accessed%20vm%20with%20remote%20desktop%20on%20mac.png)

---

## Conclusion

This project offers practical exposure to working with Microsoft Sentinel and Azure resources in a cloud-based security context. By completing this lab, you'll gain valuable skills in log analysis, detection rule creation, and leveraging SIEM tools to enhance security operations.

> **Reminder:** Always delete unused resources in Azure to avoid unnecessary costs.
