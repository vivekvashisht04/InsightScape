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


![Resources Setup (RG) (iii)](https://github.com/user-attachments/assets/ee3eafb7-2fa9-4e61-80a9-2a53ac29df9e)
![Resources Setup (Vnet) (ix)](https://github.com/user-attachments/assets/4b5294f0-0a01-4fb5-8c6d-5ab06e1c9386)




#### b) Virtual Machines (VMs) and Network Security Groups (NSGs)
- Created a Windows VM named **InsightScape-VM1** and a Linux VM named **InsightScape-VM2** in the **InsightScape-RG**.
  - **InsightScape-VM1** (Windows Server 2019 Datacenter) configured with an NSG allowing inbound **RDP, HTTP, and HTTPS** traffic.
  - **InsightScape-VM2** (Ubuntu 22.04) configured with an NSG allowing inbound **SSH, HTTPS, and HTTP** traffic.
- Updated NSGs:
  - Added an inbound security rule to **InsightScape-VM1-NSG** for allowing **HTTPS** traffic.
  - No changes made to **InsightScape-VM2-NSG** as default configurations were sufficient.


![Resources Setup (VM1) (vii)](https://github.com/user-attachments/assets/c719b1a9-1e5d-4141-9391-acc5b5bb0d3a)
![Resources Setup (VM2) (xi)](https://github.com/user-attachments/assets/57f1e4bb-a2f2-4bfd-8aeb-d6b64c709d6c)
![Resources Setup (NSGs) (xiv)](https://github.com/user-attachments/assets/bf9d8ad9-fdd1-4b37-989a-27a2dc662571)
![Resources Setup (NSGs) (xv)](https://github.com/user-attachments/assets/08211f1d-d903-426a-9ff8-0637b8eeefb2)



#### c) Web Application Deployment
- Created a **WebApp** named **InsightScape-WebApp** in **InsightScape-RG**.
  - Published an ASP.NET V4.8 sample application.
  - Configured **Application Insights** for monitoring.


![Resources Setup (Web App) (viii)](https://github.com/user-attachments/assets/3990a3b6-cc85-40a8-9700-b11a27125893)



#### d) Blob Storage and Logic App
- Created a **Blob Storage** account named **insightscapeblob** with a container named **files**.
- Configured a **Logic App** named **InsightScape-LogicApp** to trigger when new blobs are added to the container, using access keys for authentication.


![Resource Setup (Strg Acc) (vi)](https://github.com/user-attachments/assets/a9351dd8-abf6-4fdc-abb9-42c7c16caed3)
![Resource Setup (Strg Acc) (vii)](https://github.com/user-attachments/assets/c6ca66e6-8f97-44fd-9cc5-f09411503d4a)
![Resource Setup (Logic App) (xxi)](https://github.com/user-attachments/assets/5eb58968-5614-4285-a16a-560f821076a8)



#### e) Network Monitoring
- Configured **Network Watcher** for packet capture and connection monitoring.
- Verified packet captures by analyzing captured packets in Wireshark.


![Networking Resources (Network Watcher) (iii)](https://github.com/user-attachments/assets/55797589-d478-43bb-b70b-6704e1ee1501)
![Networking Resources (Packet Capture) (v)](https://github.com/user-attachments/assets/970065a8-cceb-4629-af4b-e545751f9f86)
![Networking Resources (Connection Monitor) (xiv)](https://github.com/user-attachments/assets/78f19655-d119-4e7a-95f0-72e1aaae304a)



#### f) Azure Backup
- Created a **Recovery Services Vault** named **InsightScape-Vault**.
- Configured **Azure Backup** for **InsightScape-VM1** and validated the recovery by successfully restoring the VM.


![Azure Backup (RSV) (v)](https://github.com/user-attachments/assets/a32e9ec2-bbad-4369-bf0f-0ad1a83bb97f)
![Azure Backup (RSV) (xii)](https://github.com/user-attachments/assets/38de5caf-a1aa-4207-a556-5028850405ac)
![Azure Backup (RSV) (xvi)](https://github.com/user-attachments/assets/85b5b826-7447-4798-ac02-9a5e0bc2ade8)



### Step 2: Azure Monitor Integration

#### 1) Monitoring Setup for Deployed Resources
- Enabled **Azure Monitor Insights** for both VMs, the WebApp, and the Logic App.
- Set up **Alerts** for **High CPU Usage** on **InsightScape-VM1**.

#### 2) Activity Logs, Metrics, and Diagnostic Settings
- Configured **Metrics** for VMs, WebApp, and Logic App.
- Installed **Diagnostics Extensions** on VMs.
- Enabled **Diagnostic Settings** for **WebApp** and **Default Log Analytics Workspace**.

#### 3) Monitor Resource Health
- **Monitored Resource Health** for all major deployed resources (VMs, WebApp, Logic App) using **Azure Monitor**.
- **Virtual Machines**: Accessed **Resource Health** under **Service Health** in Azure Monitor for **InsightScape-VM1** and **InsightScape-VM2** to verify health status.
- **WebApp**: Selected **Website** as the resource type in **Resource Health**. Resolved a **Critical Risk alert** related to Health Check failure by configuring and enabling health check.
- **Logic App**: Verified health using two methods:
  - Checked **Run History** in Development Tools to verify successful runs.
  - Monitored **Actions Failed** metric under **Monitoring** to ensure no failed actions.


![Azure Monitor Integration (VM-Analyze Data) (xi)](https://github.com/user-attachments/assets/5e1ea647-3c6d-4a8a-8603-cf603a43bce8)
![Azure Monitor Integration (WebApp-App Insights) (xxvi)](https://github.com/user-attachments/assets/7f7bb5bc-23eb-41e6-8a67-d4d2e4ea8653)
![Azure Monitor Integration (LogicApp-Diagnostics) (xxxi)](https://github.com/user-attachments/assets/bab56cce-bf06-4fe7-ad76-aab737f2738d)
![Activity Logs](https://github.com/user-attachments/assets/d0dda189-278b-4cee-8a1b-3327c946b8f5)
![VM-Metrics (vi)](https://github.com/user-attachments/assets/3ea22555-60b6-4c67-a6b9-2d97ad17860b)
![WebApp-Diagnostics (xxxii)](https://github.com/user-attachments/assets/c2d2c706-e922-4eff-a3fc-f67eb833fde9)
![VM-Resource Health (iii)](https://github.com/user-attachments/assets/a1d1c479-7f36-4954-aaf1-f394ba019930)
![WebApp-Resource Health (iv)](https://github.com/user-attachments/assets/a8d7665a-0a19-4f36-b554-7fb021eae7d7)



### Step 3: Log Analytics Workspace
- Confirmed linkage of VMs, WebApp, and Logic App to the **Log Analytics Workspace**.
- Ran **KQL Queries** to monitor:
  - **CPU Utilization**, **Memory Utilization**, **Disk Space Utilization**, **Uptime** for VMs.
  - **Slow Requests**, **Server Response Time**, **Operations** for WebApp.
  - **Workflow Trigger and Action Execution**, **Failed Runs** for Logic App.

![VM KQL Query-Memory Utilization (iii)](https://github.com/user-attachments/assets/49e35ae9-2cc5-4212-b006-4732d0a6ab21)
![WebApp KQL Query-Operations by Request Count and Duration (xiii)](https://github.com/user-attachments/assets/83b7d7f8-b64c-4f63-bcf7-a60ebbe45b33)
![LogicApp KQL Query-Total Workflow Runs Over Time (xxii)](https://github.com/user-attachments/assets/06bc067a-8bf3-48a3-8a31-bc352fb08b59)


### Step 4: Application Insights
- Monitored **InsightScape-WebApp** using **Application Insights**.
- Analyzed:
  - **Performance**, **Roles**, **Failures**, **User Interactions**, and **Live Metrics**.
- Added **Diagnostics Settings** for **Application Insights**.


![Application Insights (AppInsights Dashboard) (ii)](https://github.com/user-attachments/assets/be3e2297-5c37-49fe-a88e-6ca36f397d2d)
![Application Insights (Failures Tab-Roles) (v)](https://github.com/user-attachments/assets/1c3ff324-fc51-4b2a-a3f4-11c714a29c9e)
![Application Insights (Performance Tab-Operations) (iii)](https://github.com/user-attachments/assets/3695d641-d737-4d23-a797-61a67e13df69)



### Step 5: Network Monitoring
- Used **Network Watcher Topology** to analyze the network infrastructure.
- Conducted **NSG Diagnostics** for **InsightScape-VM1** and **InsightScape-VM2**:
  - Verified inbound and outbound rules and used packet capture to assess traffic status.


![Network Monitoring (iii)](https://github.com/user-attachments/assets/22dfdee6-d0b9-4212-9c75-2dfc5a5a5c5f)
![Network Monitoring (NSG diagnostics) (xi)](https://github.com/user-attachments/assets/7bfa9224-a0b7-4e39-928a-8200ff5e8db3)
![Network Monitoring (NSG diagnostics) (Deny Simulation) (xii)](https://github.com/user-attachments/assets/3da2e629-52d6-45e6-9c51-492603efad74)
![Network Monitoring (NSG diagnostics) (Deny Simulation) (xiii)](https://github.com/user-attachments/assets/c3bd6416-958d-4c0e-91a2-be6d2de6d50c)



### Step 6: Security & Compliance
- Reviewed security recommendations using **Azure Security Center** and **Microsoft Defender for Cloud**.
- Applied:
  - **VM Patches and Updates**.
  - **Disk Encryption**.
  - Network access restriction for storage accounts using **Virtual Network Rules**.
- Verified security alerts, ensuring no critical issues.


![Security   Compliance (i)](https://github.com/user-attachments/assets/c4c09289-d3b8-43fc-a384-f9e3278c30ca)
![Security   Compliance (Security Center) (ii)](https://github.com/user-attachments/assets/ed0df041-25a6-466b-b7ed-43771c11c9c6)
![Security   Compliance (Inventory) (iv)](https://github.com/user-attachments/assets/aa284215-8b38-4a8b-9ac1-2a7556caee25)
![Security   Compliance (Recommendations) (v)](https://github.com/user-attachments/assets/e2ad8958-fc58-4bf7-a41d-c6b1f601360e)



### Step 7: Alerts Configuration
- Created alerts for:
  1. **Failed RDP Login Attempts** on **InsightScape-VM1**.
  2. **Failed Logic App Executions**.
  3. **HTTP 5xx Errors** on **InsightScape-WebApp**.
- Tested the **Failed Logic App Alert** by deliberately failing the Logic App and verified alert trigger.

![Alerts Configurations (i)](https://github.com/user-attachments/assets/fb8fa089-d902-45f3-8499-88cfe515a8be)
![Alerts Configurations-Dashboard (xxvi)](https://github.com/user-attachments/assets/f4e80a20-3806-44a9-89d8-c04a0f4ca8f5)

### Step 8: Backup and Disaster Recovery
- Deleted **InsightScape-VM1** and successfully restored it using the **Recovery Services Vault**.
- Verified the restored VM to ensure the integrity of the recovery process.

![Backup and Disaster Recovery (vi)](https://github.com/user-attachments/assets/562bbc15-97e7-43bc-93d2-d771f7fd0415)
![Backup and Disaster Recovery (viii)](https://github.com/user-attachments/assets/e07ff8dc-9f5e-40a7-ac0e-ba36bb93fa1c)
![Backup and Disaster Recovery (xiii)](https://github.com/user-attachments/assets/dbd6d9c8-25af-4d29-979c-eb8899cd3115)
![Backup and Disaster Recovery (xiv)](https://github.com/user-attachments/assets/044e46d0-7d9e-414e-811f-2cedb5eb4700)


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
