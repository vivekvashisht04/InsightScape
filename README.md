# InsightScape

## Project Workflow

<div align="center">
    <img src="https://github.com/user-attachments/assets/95a5c75e-dffb-4aab-a35d-be48f5b336ef" alt="Project Architecture Diagram">
</div>


## Project Overview
Welcome to **InsightScape**, a comprehensive project designed to monitor and maintain Azure resources. This project focuses on creating an integrated monitoring and maintenance framework using Azure Monitor, Log Analytics, KQL, Application Insights, Network Watcher, Security Center, and Azure Backup to ensure comprehensive visibility, security, and performance optimization across cloud environments.

- **Programming required?**: ❌ (This project relies mostly on configuration and integration, though an understanding of Kusto Query Language (KQL) is essential for custom monitoring queries.)

## Azure Services Used
- **Azure Monitor**: Monitoring solution to track metrics and logs.
- **Azure Log Analytics**: Analyzing telemetry data to gain insights.
- **Kusto Query Language (KQL)**: Used for writing custom monitoring queries.
- **Azure Application Insights**: For performance monitoring of the WebApp.
- **Azure Network Watcher**: Network monitoring and diagnostics.
- **Azure Security Center**: Evaluates and provides security recommendations.
- **Azure Backup**: Backup and recovery services for Azure VMs.
- **Azure Alerts**: Configured alerts for resource health and other monitoring activities.

## Purpose
The purpose of this project is to validate my hands-on skills in the **Monitor and Maintain Azure Resources** aspect of the **AZ-104 Certification**. The approach was to set up major and commonly used Azure resources and then work with Azure monitoring and security services.

## Steps I Did:

### Step 1: Resources Setup

#### a) Resource Group and VNet Setup
- Created a **Resource Group** named **InsightScape-RG** in the **East US** region.
- Created a **Virtual Network (VNet)** named **InsightScape-VNet** with the following subnets:
  - **VM1-Subnet**: `10.1.1.0/24`
  - **VM2-Subnet**: `10.1.2.0/24`



#### b) Virtual Machines (VMs) and Network Security Groups (NSGs)
- Created a Windows VM named **InsightScape-VM1** and a Linux VM named **InsightScape-VM2** in the **InsightScape-RG**.
  - **InsightScape-VM1** (Windows Server 2019 Datacenter) configured with an NSG allowing inbound **RDP, HTTP, and HTTPS** traffic.
  - **InsightScape-VM2** (Ubuntu 22.04) configured with an NSG allowing inbound **SSH, HTTPS, and HTTP** traffic.
- Updated NSGs:
  - Added an inbound security rule to **InsightScape-VM1-NSG** for allowing **HTTPS** traffic.
  - No changes made to **InsightScape-VM2-NSG** as default configurations were sufficient.



#### c) Web Application Deployment
- Created a **WebApp** named **InsightScape-WebApp** in **InsightScape-RG**.
  - Published an ASP.NET V4.8 sample application.
  - Configured **Application Insights** for monitoring.



#### d) Blob Storage and Logic App
- Created a **Blob Storage** account named **insightscapeblob** with a container named **files**.
- Configured a **Logic App** named **InsightScape-LogicApp** to trigger when new blobs are added to the container, using access keys for authentication.



#### e) Network Monitoring
- Configured **Network Watcher** for packet capture and connection monitoring.
- Verified packet captures by analyzing captured packets in Wireshark.



#### f) Azure Backup
- Created a **Recovery Services Vault** named **InsightScape-Vault**.
- Configured **Azure Backup** for **InsightScape-VM1** and validated the recovery by successfully restoring the VM.



### Step 2: Azure Monitor Integration

#### 1) Monitoring Setup for Deployed Resources
- Enabled **Azure Monitor Insights** for both VMs, the WebApp, and the Logic App.
- Set up **Alerts** for **High CPU Usage** on **InsightScape-VM1**.

#### 2) Activity Logs, Metrics, and Diagnostic Settings
- Configured **Metrics** for VMs, WebApp, and Logic App.
- Installed **Diagnostics Extensions** on VMs.
- Enabled **Diagnostic Settings** for **WebApp** and **Default Log Analytics Workspace**.

### Step 3: Log Analytics Workspace
- Confirmed linkage of VMs, WebApp, and Logic App to the **Log Analytics Workspace**.
- Ran **KQL Queries** to monitor:
  - **CPU Utilization**, **Memory Utilization**, **Disk Space Utilization**, **Uptime** for VMs.
  - **Slow Requests**, **Server Response Time**, **Operations** for WebApp.
  - **Workflow Trigger and Action Execution**, **Failed Runs** for Logic App.

### Step 4: Application Insights
- Monitored **InsightScape-WebApp** using **Application Insights**.
- Analyzed:
  - **Performance**, **Roles**, **Failures**, **User Interactions**, and **Live Metrics**.
- Added **Diagnostics Settings** for **Application Insights**.

### Step 5: Network Monitoring
- Used **Network Watcher Topology** to analyze the network infrastructure.
- Conducted **NSG Diagnostics** for **InsightScape-VM1** and **InsightScape-VM2**:
  - Verified inbound and outbound rules and used packet capture to assess traffic status.

### Step 6: Security & Compliance
- Reviewed security recommendations using **Azure Security Center** and **Microsoft Defender for Cloud**.
- Applied:
  - **VM Patches and Updates**.
  - **Disk Encryption**.
  - Network access restriction for storage accounts using **Virtual Network Rules**.
- Verified security alerts, ensuring no critical issues.

### Step 7: Alerts Configuration
- Created alerts for:
  1. **Failed RDP Login Attempts** on **InsightScape-VM1**.
  2. **Failed Logic App Executions**.
  3. **HTTP 5xx Errors** on **InsightScape-WebApp**.
- Tested the **Failed Logic App Alert** by deliberately failing the Logic App and verified alert trigger.



### Step 8: Backup and Disaster Recovery
- Deleted **InsightScape-VM1** and successfully restored it using the **Recovery Services Vault**.
- Verified the restored VM to ensure the integrity of the recovery process.

## Testing
The final phase involved rigorous testing of different components:
- **High CPU Alert Testing** using PowerShell to increase CPU utilization.
- **Logic App Failure Testing** to trigger alerts.
- **Packet Capture Analysis** to verify network diagnostics.
- **Disaster Recovery Testing** to restore the deleted VM.


## Conclusion
The **InsightScape** project successfully demonstrated the integration of monitoring and maintenance features in Azure using a variety of services such as **Azure Monitor**, **Log Analytics**, and **Application Insights**. It provided hands-on experience in building and maintaining a secure, high-performance cloud environment with an emphasis on **disaster recovery**, **security**, and **alert configurations**.

## Skills Demonstrated
- **Monitoring and Maintenance**:
  - Utilized **Azure Monitor**, **Log Analytics**, and **KQL** for detailed resource monitoring.
- **Network Security**:
  - Implemented **Network Watcher** diagnostics and configured **Network Security Groups (NSGs)** for secure connectivity.
- **Security and Compliance**:
  - Leveraged **Azure Security Center** for resource security posture improvements.
- **Backup and Disaster Recovery**:
  - Configured **Azure Backup** and validated the disaster recovery process for Azure VMs.
- **Alerts Configuration**:
  - Created proactive alerts for real-time resource monitoring and response.

## Repository Contents
- **Manual**: A comprehensive manual detailing each phase of the project, including step-by-step configurations, troubleshooting tips, and best practices.
- **Screenshots**: Visual documentation of key stages and configurations throughout the project, stored in the `Screenshots` folder.
- **Source Code**: Contains scripts and commands used in the project, organized in the `Source_Code` folder.

## Contact
For any questions or further information, feel free to reach out to me on LinkedIn: [LinkedIn Profile](https://www.linkedin.com/in/vivek-vashisht04/)
