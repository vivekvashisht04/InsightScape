Testing the High CPU Alert for VM1:

while ($true) { Start-Process calc.exe; Start-Sleep -Milliseconds 100; Stop-Process -Name calc }






Install the LinuxDiagnostic extension in InsightScape-VM2:

1.	Check the version of the waagent:
/usr/sbin/waagent -version
2.	Update and install walinuxagent:
sudo apt-get update && sudo apt-get install walinuxagent
3.	Install wget:
sudo apt-get install -y wget
4.	Install Python 2:
sudo apt-get install -y python2
5.	Set Python 2 as the default Python version:
sudo update-alternatives --install /usr/bin/python python /usr/bin/python2 1
6.	Assign a managed identity to the VM:
az vm identity assign --resource-group InsightScape-RG --name InsightScape-VM2
7.	Download the diagnostics settings JSON file:
wget https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json -O portal_public_settings.json
8.	Set the diagnostic storage account name:
my_diagnostic_storage_account="insightscapeblob"
9.	Get the resource ID of the VM:
my_vm_resource_id=$(az vm show --resource-group InsightScape-RG --name InsightScape-VM2 --query "id" -o tsv)
10.	Update the diagnostic storage account in the settings file:
sed -i "s#_DIAGNOSTIC_STORAGE_ACCOUNT_#$my_diagnostic_storage_account#g" portal_public_settings.json
11.	Update the VM resource ID in the settings file:
sed -i "s#_VM_RESOURCE_ID_#$my_vm_resource_id#g" portal_public_settings.json
12.	Generate a SAS token for the diagnostic storage account:
my_diagnostic_storage_account_sastoken=$(az storage account generate-sas --account-name $my_diagnostic_storage_account --expiry 2037-12-31T23:59:00Z --permissions wlacu --resource-types co --services b --output tsv)
13.	Create the protected settings for the extension:
my_lad_protected_settings="{\"storageAccountName\": \"$my_diagnostic_storage_account\", \"storageAccountSasToken\": \"$my_diagnostic_storage_account_sastoken\"}"
14.	Set the Linux diagnostics extension on the VM:
az vm extension set --publisher Microsoft.Azure.Diagnostics --name LinuxDiagnostic --version 4.0 --resource-group InsightScape-RG --vm-name InsightScape-VM2 --protected-settings "$my_lad_protected_settings" --settings portal_public_settings.json











Confirming Resource Linkage to Log Analytics Workspace:


For VMs:

// Availability rate
// Calculate the availability rate of each connected computer.
Heartbeat
// bin_at is used to set the time grain to 1 hour, starting exactly 24 hours ago
| summarize heartbeatPerHour = count() by bin_at(TimeGenerated, 1h, ago(24h)), Computer
| extend availablePerHour = iff(heartbeatPerHour > 0, true, false)
| summarize totalAvailableHours = countif(availablePerHour == true) by Computer
| extend availabilityRate = totalAvailableHours * 100.0 / 24

For WebApp:

// App logs for each App Service
// Breakdown of log levels for each App Service.
// To create an alert for this query, click '+ New alert rule'
AppServiceAppLogs
| project CustomLevel, _ResourceId
| summarize count() by CustomLevel, _ResourceId

For Logic App:

AzureDiagnostics
| where ResourceProvider == "MICROSOFT.LOGIC"
| where Category == "WorkflowRuntime"
| where status_s == "Failed"
| where OperationName == "workflowActionCompleted" or OperationName == "workflowTriggerCompleted"
| extend ResourceName = coalesce(resource_actionName_s, resource_triggerName_s)
| extend ResourceCategory = substring(OperationName, 34, strlen(OperationName) - 43)
| summarize dcount(resource_runId_s) by code_s, resource_workflowName_s, ResourceCategory, ResourceName, _ResourceId
| project ResourceCategory, ResourceName, FailureCount = dcount_resource_runId_s, ErrorCode = code_s, LogicAppName = resource_workflowName_s, _ResourceId
| order by FailureCount desc











KQL Queries and Monitoring Setup for Each Resource:

1. Virtual Machines:

High CPU Utilization:

InsightsMetrics
| where Origin == "vm.azm.ms"
| where Namespace == "Processor"
| where Name == "UtilizationPercentage"
| summarize avg(Val) by bin(TimeGenerated, 5m), Computer
| render timechart
•	

Explanation: This query monitors the average CPU usage of virtual machines over 5-minute intervals. It helps identify periods of high CPU utilization for VMs to manage workload performance effectively.


Memory Utilization:

InsightsMetrics
| where Origin == "vm.azm.ms"
| where Namespace == "Memory"
| where Name == "AvailableMB"
| summarize avg(Val) by bin(TimeGenerated, 5m), Computer
| render timechart
•	

Explanation: This query shows the available memory for VMs over time, helping to track memory usage trends.


Disk Space Utilization:

InsightsMetrics
| where Origin == "vm.azm.ms"
| where Namespace == "LogicalDisk"
| where Name == "FreeSpacePercentage"
| summarize avg(Val) by bin(TimeGenerated, 5m), Computer
| render timechart
•	

Explanation: This query calculates the percentage of available disk space, allowing us to see how efficiently storage is being used by VMs.


VM Uptime:

Heartbeat
| where TimeGenerated > ago(1h)
| summarize count() by Computer, bin(TimeGenerated, 5m)
| render timechart
•	

Explanation: This query monitors the uptime of VMs by counting the number of heartbeats received, useful for checking VM availability.
After running these KQL queries, I pinned each of them to my dashboard.


2. WebApp:


Top Slow Requests:

requests
| order by duration desc
| top 10 by duration
| project name, duration, url, timestamp
•	

Explanation: This query lists the top 10 slowest requests by duration, useful for identifying and optimizing slow-performing endpoints.


Server Response Time By URL:

requests
| summarize avg(duration) by bin(timestamp, 5m), url
| render timechart
•	

Explanation: This query monitors the average server response time for different URLs over time, aiding in identifying response time issues.


Operations By Request Count and Duration:

requests
| summarize RequestsCount = sum(itemCount), AverageDuration = avg(duration), percentiles(duration, 50, 95, 99) by operation_Name
| order by RequestsCount desc
•	

Explanation: This query provides insights into the number of requests and average request duration for each operation, helping to prioritize optimization efforts.


Total Successful Requests Over Time:

requests
| where success == "True"
| summarize total_success = count() by bin(timestamp, 5m)
| render timechart
•	


Explanation: This query tracks the total number of successful requests over time, helping to understand the reliability of the web app.
After running these KQL queries, I pinned each of them to my dashboard.


3. Logic App:


Workflow Trigger and Action Execution Count:

AzureDiagnostics
| where ResourceProvider == "MICROSOFT.LOGIC"
| where Category == "WorkflowRuntime"
| where OperationName has "workflowTriggerStarted" or OperationName has "workflowActionStarted"
| summarize dcount(resource_runId_s) by OperationName, resource_workflowName_s
•	


Explanation: This query shows the count of workflow trigger and action executions, helping to track the activity level of Logic Apps.


Trigger Failures:

AzureDiagnostics
| where ResourceProvider == "MICROSOFT.LOGIC"
| where Category == "WorkflowRuntime"
| where OperationName contains "workflowTriggerCompleted" and status_s == "Failed"
| summarize count() by resource_workflowName_s, resource_triggerName_s
•	


Explanation: This query lists the number of failed triggers, helping to quickly identify workflows that need attention.

Total Workflow Runs Over Time:

AzureDiagnostics
| where ResourceProvider == "MICROSOFT.LOGIC"
| where Category == "WorkflowRuntime"
| summarize totalRuns = count() by resource_workflowName_s, bin(TimeGenerated, 1h)
| render timechart
•	


Explanation: This query monitors the total number of workflow runs over time, giving an overview of Logic App activity.

Failed Runs by Error Code:

AzureDiagnostics
| where ResourceProvider == "MICROSOFT.LOGIC"
| where Category == "WorkflowRuntime"
| where status_s == "Failed"
| summarize errorCodeCount = count() by code_s
| order by errorCodeCount desc
•	


Explanation: This query lists error codes for failed Logic App runs, allowing us to prioritize error resolution.













Creating Custom Alerts


1. Alert Rule for InsightScape-VM1

o	KQL Query:

SecurityEvent
| where EventID == 4625



2. Alert Rule for InsightScape-LogicApp


o	KQL Query:

AzureDiagnostics
| where ResourceProvider == "MICROSOFT.LOGIC"
| where Category == "WorkflowRuntime"
| where status_s == "Failed"
| where OperationName has "workflowActionCompleted" or OperationName has "workflowTriggerCompleted"
| extend ResourceName = coalesce(resource_actionName_s, resource_triggerName_s)
| extend ResourceCategory = substring(OperationName, 34, strlen(OperationName) - 43)
| summarize dcount(resource_runId_s) by code_s, ResourceName, resource_workflowName_s, ResourceCategory, _ResourceId
| project ResourceCategory, ResourceName, FailureCount = dcount_resource_runId_s, ErrorCode = code_s, LogicAppName = resource_workflowName_s, _ResourceId
| order by FailureCount desc




3. Alert Rule for InsightScape-WebApp


o	KQL Query:

AppServiceHTTPLogs
| where scStatus >= 500 and scStatus < 600
| summarize FailureCount = count() by bin(TimeGenerated, 5m)
| where FailureCount > 0
