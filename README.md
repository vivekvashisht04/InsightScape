# InsightScape

## Project Workflow

<div align="center">
    <img src="![Project Architecture Diagram ](https://github.com/user-attachments/assets/14df6833-6b5c-4585-9bcb-e304390b998d)">

</div>


## Project Overview
Welcome to **InsightScape**, a comprehensive project designed to monitor and maintain Azure resources. This project focuses on creating an integrated framework using Azure Monitor, Log Analytics, KQL, Application Insights, Network Watcher, Security Center, and Azure Backup to ensure comprehensive visibility, security, and performance optimization across cloud environments.

## Features
- **Resource Monitoring and Insights:** Set up detailed monitoring for Azure resources including VMs, WebApps, and Logic Apps.
- **Alerts Configuration:** Configure alerts for proactive resource health monitoring.
- **Network Security and Monitoring:** Implement Network Watcher to monitor and troubleshoot network resources.
- **Backup and Disaster Recovery:** Ensure availability and recovery of critical workloads with Azure Backup.
- **Application Insights for Performance Tracking:** Utilize Application Insights to monitor web application performance and diagnose issues.

## Azure Services Used
- **Azure Monitor**: Monitoring solution to track metrics and logs.
- **Azure Log Analytics**: Analyzing telemetry data to gain insights.
- **Kusto Query Language (KQL)**: Used for writing custom monitoring queries.
- **Azure Application Insights**: For performance monitoring of the WebApp.
- **Azure Network Watcher**: Network monitoring and diagnostics.
- **Azure Security Center**: Evaluates and provides security recommendations.
- **Azure Backup**: Backup and recovery services for Azure VMs.
- **Azure Alerts**: Configured alerts for resource health and other monitoring activities.

## Steps I Did:

### Step 1: Resources Setup
- **Resource Group and Virtual Network (VNet) Setup**:
  Created a Resource Group named **InsightScape-RG** in East US and deployed a Virtual Network **InsightScape-VNet** with multiple subnets (`VM1-Subnet` and `VM2-Subnet`).



### Step 2: VMs and NSGs
- **Windows and Linux VMs**:
  Deployed two virtual machines, **InsightScape-VM1** (Windows) and **InsightScape-VM2** (Linux), each in separate subnets. Configured Network Security Groups (NSGs) for both.



### Step 3: Web Application Deployment
- **App Service Setup**:
  Created a WebApp named **InsightScape-WebApp** to host an ASP.NET sample application, deployed via GitHub.



### Step 4: Blob Storage and Logic App
- **Blob Storage Setup**:
  Created **insightscapeblob** storage with a container named **files**.
- **Logic App**:
  Configured a Logic App to trigger when new blobs are added, using access keys for authentication.



### Step 5: Network Monitoring
- **Network Watcher**:
  Configured Packet Capture and Connection Monitor between VMs to monitor connectivity and network health.



### Step 6: Security & Compliance
- **Microsoft Defender for Cloud**:
  Reviewed security recommendations and implemented VM patches, disk encryption, and network security rules to enhance the overall security posture.



### Step 7: Alerts Configuration
- **Azure Alerts**:
  Created alerts for failed login attempts, Logic App failures, and HTTP 5xx errors for the WebApp.



### Step 8: Backup and Disaster Recovery
- **Azure Backup**:
  Configured backup for **InsightScape-VM1** and validated disaster recovery by restoring the VM successfully.



## Conclusion
The **InsightScape** project successfully showcased the ability to integrate monitoring and maintenance features in Azure. It provided hands-on experience in using a wide range of Azure services to build a secure, high-performance cloud environment, which was thoroughly monitored and backed up for disaster recovery.

## Skills Demonstrated
- **Monitoring and Maintenance**: Azure Monitor, Log Analytics, and KQL for in-depth resource monitoring.
- **Network Security**: Network Watcher diagnostics and NSGs to manage secure connectivity.
- **Security and Compliance**: Enhanced resource security posture with Azure Security Center.
- **Backup and Disaster Recovery**: Configured Azure Backup and validated disaster recovery using restored VMs.
- **Alerts Configuration**: Configured custom alerts for various Azure resources.

## Repository Contents
- **Manual**: A comprehensive manual detailing each phase of the project, including step-by-step configurations, screenshots, and monitoring tips.
- **Screenshots**: Visual documentation of key stages and configurations in the `Screenshots` folder.
- **Source Code**: Contains the source code for commands, scripts, and KQL queries used throughout the project, organized in the `Source_Code` folder.

## Contact
For any questions or further information, feel free to reach out to me on LinkedIn: [LinkedIn Profile](https://www.linkedin.com/in/vivek-vashisht04/)
