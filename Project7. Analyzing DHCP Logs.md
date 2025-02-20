# DHCP Log Analysis with Splunk SIEM

## Introduction
DHCP (Dynamic Host Configuration Protocol) log files contain valuable information about IP address assignments, lease durations, client requests, and server responses. Analyzing DHCP logs using Splunk SIEM enables network administrators to monitor IP address usage, detect anomalies, and troubleshoot network issues effectively. In this project, I will upload a sample DHCP log file to Splunk SIEM and analyze DHCP logs to identify suspicious activities.

## Prepare Sample DHCP Log File
1.	Download sample [DHCP log file](https://www.secrepo.com/maccdc2012/dhcp.log.gz) in a suitable format.
2.	Ensure the log files contain relevant DHCP events, including timestamps, IP address assignments, lease durations, client identifiers, etc.
3.	Once sample log file is saved, upload the file to Splunk. (Settings > Add Data > Upload)

## Analyze DHCP Log File
### 1.	Search for DHCP Events
-	Open Splunk interface and navigate to the search bar.
-	Enter the following search query to retrieve DHCP events
```
index=* sourcetype=<your_dhcp_sourcetype>
```
### 2.	Extract Relevant Fields
-	Identify key fields in DHCP logs such as timestamps, IP addresses, lease durations, client identifiers, etc.
-	Example extraction command
```
| rex field=_raw "<regex_pattern>"
```
### 3.	Analyze email Traffic Patterns
-	Determine the distribution of IP address assignments.
```
index=<your_dhcp_index> sourcetype=<your_dhcp_sourcetype> | stats count by leased_ip
```
- Identify top IP addresses leased by the DHCP server.
```
index=<your_dhcp_index> sourcetype=<your_dhcp_sourcetype> | top limit=10 leased_ip
```
### 4. Detect Anomalies
-	Search for unusual patterns in IP address assignments.
```
index=<your_dhcp_index> sourcetype=<your_dhcp_sourcetype> | timechart span=1h count by _time
```
- Analyze DHCP requests from unauthorized or unknown clients.
```
index=<your_dhcp_index> sourcetype=<your_dhcp_sourcetype> | search NOT client_identifier="authorized_identifier"
```
### 5. Monitoring IP Address Usage
- Monitor IP address usage over time.
```
index=<your_dhcp_index> sourcetype=<your_dhcp_sourcetype> | timechart span=1h count by leased_ip
```
- Identify IP addresses with multiple lease renewals or changes.
```
index=<your_dhcp_index> sourcetype=<your_dhcp_sourcetype> | stats count by leased_ip, lease_renewal | where count > 1 AND lease_renewal="true"
```
- Analyze DHCP traffic pattern and deviations from normal behavior.
```
index=<your_dhcp_index> sourcetype=<your_dhcp_sourcetype> | timechart span=1d count by leased_ip
```

## Conclusion
Analyzing DHCP log files using Splunk SIEM provides valuable insights into IP address assignment within a network. By monitoring DHCP events, detecting anomalies, and correlating with other logs, organizations can enhance their network management capabilities, troubleshoot issues, and improve overall network security.
