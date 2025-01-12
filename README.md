# Azure Cloud Detection Lab

This project is a hands-on lab designed to explore how to configure and deploy a cloud-based environment in Microsoft Azure. The goal of this lab is to gain experience with Microsoft Sentinel, a Security Information and Event Management (SIEM) solution, while learning key concepts like log collection, security analysis, and detection implementation using KQL (Kusto Query Language).

---

## Overview

Microsoft Sentinel is a powerful cloud-based SIEM solution that centralizes security data, detects potential threats, and enables swift incident response. This lab is designed to provide practical experience with:

- Setting up and configuring Azure resources such as Virtual Machines and Log Analytics Workspaces.
- Collecting and analyzing Windows Security Events to understand activity patterns and detect suspicious behavior.
- Writing custom detection rules to simulate real-world scenarios.
- Mapping detection techniques to the MITRE ATT&CK framework for better situational awareness.
- Using KQL (Kusto Query Language) for efficient querying and log data analysis.

This lab is beginner-friendly and can be completed using Azure's free trial account, making it accessible for those new to cloud-based security tools.

---

## Lab Setup

### Create a Resource Group

Resource groups in Azure are logical containers that help organize and manage resources. Follow these steps to create one:

1. Navigate to the **Azure Portal**.
2. Search for **Resource Group** in the search bar.
3. Provide the required details, such as the resource group name and region.
4. Click **Create** to finalize the setup.

![Resource Group Creation](./resource%20grou.png)

---

### Deploy a Virtual Machine

Virtual Machines (VMs) are essential for collecting and analyzing security data. Here’s how to deploy a Windows 10 VM:

1. Search for **Virtual Machine** in the Azure portal and click **Create**.
2. Use the resource group you created earlier.
3. Fill in the required fields, such as the admin username and password.
4. Use default settings for disks, networking, and management.
5. Click **Review + Create** and then **Create** to deploy the VM.

![Virtual Machine Overview](./labvm%20creation.png)

---

## Network and VM Security

### Configuring Just-in-Time (JIT) Access

Just-in-Time (JIT) access minimizes the attack surface by restricting RDP access to specific IPs and limited timeframes. Follow these steps to enable JIT access:

1. Go to **Microsoft Defender for Cloud** in the Azure portal.
2. Enable JIT for your VM by selecting it and configuring access rules.
3. Restrict RDP access to your IP address and set a time limit for access.
4. Verify the updated rules in the Networking tab of your VM.

![MITRE ATT&CK Mapping](./accessed%20vm%20with%20remote%20desktop%20on%20mac.png)
![Just-in-Time Access Configuration](./microsoft%20denfnder.png)

---

## Log Analytics and Sentinel Deployment

### Create a Log Analytics Workspace

The Log Analytics Workspace serves as a repository for log data collected from your Azure resources. To set it up:

1. Search for **Log Analytics Workspace** in the Azure portal.
2. Create a new workspace within the same resource group as your VM.
3. Complete the configuration by clicking **Review + Create**.

![Log Analytics Workspace Creation](./log%20analytics.png)

---

### Deploy Microsoft Sentinel

Microsoft Sentinel integrates with your Log Analytics Workspace to provide advanced security insights. To deploy Sentinel:

1. Search for **Microsoft Sentinel** in the Azure portal.
2. Add Sentinel to the Log Analytics Workspace you created earlier.
3. Complete the setup to start using Sentinel for log analysis and threat detection.

![Microsoft Sentinel Deployment](./log%20analytics.png)

---

## Data Collection and Event Analysis

### Configuring Data Connectors

Data Connectors allow you to ingest logs from your VM into Sentinel for analysis. Here’s how to set up a connector:

1. Navigate to the **Data Connectors** tab in Sentinel.
2. Search for **Windows Security Events via AMA** and select it.
3. Create a **Data Collection Rule** to link the connector to your VM.

![Data Connectors Setup](./connector%20for%20windows%20events.png)

---

### Event Analysis

Once data is ingested, you can analyze security events using the Event Viewer on your VM. For example:

- Event ID **4624** indicates a successful login.
- These events can be queried and analyzed in Sentinel using KQL to uncover patterns or anomalies.

![Event Viewer Logs](./running%20a%20simple%20KQL%20query%20from%20an%20actual%20event.png)

---

## Custom Detection Rules

### Scheduled Task Monitoring

This section demonstrates how to create a custom rule in Sentinel to detect the creation of scheduled tasks (Event ID 4698):

1. Enable logging for scheduled tasks by configuring the **Local Security Policy** on your VM.
2. Use **Windows Task Scheduler** to create a task.
3. Write the following KQL query in Sentinel to detect scheduled task events:
   ```kql
   SecurityEvent                             
   | where EventID == 4698
   | parse EventData with * '<Data Name="TaskName">' TaskName '</Data>'
   | project TimeGenerated, Computer, TaskName
   ```
4. Configure Sentinel to generate alerts when these events are detected.

![Analytic Rule Creation](./creating%20a%20analytic%20rule.png)

![Custom Rule Logic](./creating%20the%20rule%20logic.png)

---

## MITRE ATT&CK Mapping

This lab aligns with the **TA0003 - Persistence** tactic in the MITRE ATT&CK framework, specifically focusing on the **T1053.005 - Scheduled Task/Job** technique. Adversaries may abuse scheduled tasks to maintain persistence in an environment.

Detection strategies include:

- Monitoring Event ID 4698 for scheduled task creation.
- Restricting privileges to authorized accounts to prevent unauthorized task creation.


---

## Detection

As observed, monitoring and logging specific Windows Event IDs were used to detect this activity. The following image highlights the detection of scheduled tasks in Sentinel:

![New Scheduled Task](./new%20scheuled%20task.png)

---

## Conclusion

This lab provides hands-on experience with Azure and Microsoft Sentinel, offering insights into modern SIEM solutions and practical skills in log analysis, detection rule creation, and threat hunting. By completing this project, you’ll develop foundational skills for cloud-based security operations.

> **Reminder:** Always delete unused resources in Azure to avoid unnecessary costs.
````

