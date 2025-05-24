<p align="center">
  <a href="https://github.com/Samuel-Cavada" target="_blank">
    <img src="https://img.shields.io/badge/Back_to_Main_Page-000000?style=for-the-badge&logo=github&logoColor=white" alt="Back to Main Page"/>
  </a>
</p>

<h1 align="center">Azure Activity Logs</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Azure-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white" alt="Cloud Platform" />
  <img src="https://img.shields.io/badge/OS-N/A-0078D6?style=for-the-badge&logo=windows&logoColor=white" alt="OS" />
  <img src="https://img.shields.io/badge/Tool-Azure%20Monitor-00B388?style=for-the-badge&logo=microsoftazure&logoColor=white" alt="Tool" />
  <img src="https://img.shields.io/badge/Tool-PowerShell-2C5EA8?style=for-the-badge&logo=powershell&logoColor=white" alt="Tool" />
  <img src="https://img.shields.io/badge/Focus-Activity%20Log%20Monitoring-orange?style=for-the-badge" alt="Focus Area" />
</p>

---

## 📌 Project Objective
> Monitor and analyze Azure Activity Logs to detect potentially unauthorized or bulk resource deletions using Kusto Query Language (KQL). This project reinforces log analysis and operational visibility in Azure.

---

## 🧰 Tools & Technologies
- **Platform:** Azure
- **OS:** N/A
- **Tools:** Azure Monitor, Log Analytics, PowerShell
- **Languages/Scripts:** KQL

---

## 🧠 Skills Gained / Focus Areas
- Queried Azure Activity Logs with KQL
- Identified users performing high-volume DELETE operations
- Applied time-based filters to detect patterns of behavior
- Practiced threshold-based alerts and summarizations

---

## 🧪 Environment Setup
> Azure Activity Logs were accessed via the Azure Portal → Monitor → Logs section. Logs were queried using KQL in a connected Log Analytics Workspace.

![Environment Setup](assets/images/setup.jpg)

---

## 🛠️ Walkthrough
1. [Step 1: Access Activity Logs](#step-1-access-activity-logs)
2. [Step 2: Run Query](#step-2-run-query)
3. [Step 3: Analyze and Modify](#step-3-analyze-and-modify)

---

### ✅ Step 1: Access Activity Logs
> - Opened Azure Monitor  
> - Selected "Logs" and used the `AzureActivity` table  
> - Set workspace context and query scope for recent events

![Step 1](assets/images/step1.jpg)

---

### ✅ Step 2: Run Query
> Reviewed the following query to detect large-scale deletions:
```kql
// Someone deleting more than 10 resources
let resource_threshold = 10;
let time_threshold_in_minutes = 30m;
AzureActivity
| where TimeGenerated > ago(time_threshold_in_minutes) 
| where OperationNameValue endswith "DELETE" and ActivityStatusValue == "Success"
| summarize number_of_records = count() by Caller, ActivityStatusValue
| where number_of_records > resource_threshold
```

> **Purpose:**
> - Monitor successful DELETE operations  
> - Count deletions by user within a 30-minute window  
> - Identify users who deleted more than 10 resources

---

### ✅ Step 3: Analyze and Modify
> - Changed `resource_threshold` and `time_threshold_in_minutes` to simulate other scenarios  
> - Used `OperationNameValue startswith` for other operation types  
> - Practiced extending queries to include affected resources or IPs

![Step 3](assets/images/step3.jpg)

---

## 📝 Outcomes and Lessons Learned
- **Technical Insight:** Activity Logs can detect anomalous or risky admin behavior when paired with flexible query logic.
- **Configuration Skills:** Gained experience filtering and summarizing high-value audit events.
- **Troubleshooting:** Verified query results against known deletions in test subscriptions.
- **Takeaway:** KQL empowers security and operations teams to create targeted alerts and dashboards for governance enforcement.

---

## 📎 References
- [Azure Activity Logs Overview](https://learn.microsoft.com/en-us/azure/azure-monitor/platform/activity-log)
- [Kusto Query Language Reference](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/)
- [Azure Monitor Logs](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/log-query-overview)
