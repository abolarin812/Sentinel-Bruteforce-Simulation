## Sentinel-Bruteforce-Simulation

## Objective

This project demonstrates hands-on experience with Microsoft Sentinel for Security Information and Event Management (SIEM). The goal was to simulate a realistic attack environment, ingest logs from multiple sources, and detect suspicious activity using alerts and queries. It highlights how SIEM can be used to monitor, analyze, and respond to potential threats in a networked environment.

## Skills Learned

- Configuring Microsoft Sentinel for log collection and monitoring.

- Writing Kusto Query Language (KQL) queries to investigate security events.

- Detecting anomalies, brute-force attempts, and unusual login patterns.

- Triage and escalation of alerts in a SOC workflow.

- Documenting and reporting security incidents effectively.

## Tools Used

- Microsoft Sentinel – SIEM for log ingestion, alerting, and incident response.

- Detection Lab – simulated network environment for generating attack telemetry.

## Steps

Here’s how the simulation was conducted:

1. Create a Windows VM on Azure

  -   Deployed a Windows virtual machine and enabled RDP for remote access.

<img width="941" height="365" alt="image" src="https://github.com/user-attachments/assets/44f519e8-28d7-4812-a116-73216b728a7d" />


2.  Set Up Log Analytics and Sentinel

  -  Created a Log Analytics Workspace.

  -  Added a Microsoft Sentinel instance to the workspace.

<img width="941" height="489" alt="image" src="https://github.com/user-attachments/assets/1c2e7d25-f6cb-45eb-bd94-b0a7b3da585f" />

Fig 1: Azure VM Configuration


3.  Connect VM to Microsoft Sentinel

  -  Configured the Windows Security Events data connector via AMA to send logs from the VM to Sentinel.

<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/ea895a34-7e96-4bd5-bcbe-a68268387b13" />


4.  Simulate Brute-Force Attack

  -  Opened RDP on the host machine.

  - Entered the VM IP and username, then intentionally entered the wrong password 10+ times to simulate a brute-force login      attempt.

<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/cd6be1d6-1b65-4703-b2d5-468127f58bf5" />


5.  Verify Logs in Sentinel

  -  Navigated to Logs in Sentinel.

 -   Ran the following KQL query to identify failed logins:<br>
&nbsp;&nbsp;&nbsp;&nbsp;SecurityEvent<br>
&nbsp;&nbsp;&nbsp;&nbsp;| where EventID == 4625

<img width="941" height="516" alt="image" src="https://github.com/user-attachments/assets/1e992245-f652-4417-a80b-f4868d750dde" />


6.  Create an Analytics Rule to Detect the Attack

  -  Navigate to Sentinel > Analytics > Create > Scheduled query rule.

 - Rule details:<br>
&nbsp;&nbsp;&nbsp;&nbsp;Name: Brute Force Detection – Failed Logins<br>
&nbsp;&nbsp;&nbsp;&nbsp;Severity: Medium<br>
&nbsp;&nbsp;&nbsp;&nbsp;Tactics: Credential Access, Initial Access<br>

- KQL Query for Rule:<br>
&nbsp;&nbsp;&nbsp;&nbsp;SecurityEvent<br>
&nbsp;&nbsp;&nbsp;&nbsp;| where EventID == 4625<br>
&nbsp;&nbsp;&nbsp;&nbsp;| project TimeGenerated, EventID, WorkstationName, Computer, Account, LogonTypeName, IpAddress<br>
&nbsp;&nbsp;&nbsp;&nbsp;| extend AccountEntity = Account<br>
&nbsp;&nbsp;&nbsp;&nbsp;| extend IPEntity = IpAddress

 - Rule Settings:<br>
&nbsp;&nbsp;&nbsp;&nbsp;Run frequency: 5 minutes<br>
&nbsp;&nbsp;&nbsp;&nbsp;Lookup period: 5 minutes<br>
&nbsp;&nbsp;&nbsp;&nbsp;Trigger alert when result count > 0

<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/e6041b76-df1e-4c67-896a-f73ed633cf55" />

<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/a00df7e5-0f07-420c-b6eb-a00948b7fb17" />


7.  Repeat Brute-Force Simulation

  -  Repeated the manual brute-force login simulation to trigger the analytics rule.

8.  View Triggered Incident

  -  Go to Sentinel > Incidents.

  - Locate the incident: Brute Force Detection – Failed Logins.

  -  Review incident details and confirm the detection worked as expected.

<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/eb78781f-7b1d-4214-85b9-2ab9841e85c3" />
 
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/015ab49c-4b2f-4706-9616-bbfac0370715" />

<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/49a83d5a-7f32-4d56-9453-93d639adb85e" />

## Lessons Learned.

This simulation helped me:

1.	Understand real-world attack simulation in a cloud environment

2.	Analyze logs in Microsoft Sentinel

3.	Configure detection rules



