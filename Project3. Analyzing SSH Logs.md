# SSH Log Analysis with Splunk SIEM

## Introduction
SSH (Secure Shell) log files contain valuable information about remote access to servers, including login attempts, commands executed, and session details. In this project, I will upload a sample SSH log file to Splunk SIEM and analyze SSH logs to identify suspicious activities.

## Prepare Sample SSH Log File
1.	Download sample [SSH log file](https://www.secrepo.com/maccdc2012/ssh.log.gz) in a suitable format (e.g., text files).
2.	Ensure the log files contain relevant SSH events, including timestamps, source IP addresses, usernames, actions (login, logout), etc.
3.	Once sample log file is saved, upload the file to Splunk. (Settings > Add Data > Upload)

## Analyze SSH Log File
### 1.	Search for SSH Events
-	Open Splunk interface and navigate to the search bar.
-	Enter the following search query to retrieve SSH events
```
index=* sourcetype=<your_ssh_sourcetype>
```
### 2.	Extract Relevant Fields
-	Identify key fields in SSH logs such as timestamps, source IP addresses, usernames, actions, etc.
-	Example extraction command
```
| rex field=_raw "<regex_pattern>"
```

### 3.	Analyze SSH Activity Patterns
-	Determine the distribution of SSH commands executed
```
index=* sourcetype=<your_ssh_sourcetype> | stats count by command
```
-	Identify top source IP addresses accessing the SSH server
```
index=* sourcetype=<your_ssh_sourcetype> | top limit=10 src_ip
```
-	Analyze SSH login attempts action
```
index=* sourcetype=<your_ssh_sourcetype> | stats count by action
```

### 4.	Detect Anomalies
-	Look for unusual patterns in SSH activity (e.g., sudden spikes in login attempts)
```
index=* sourcetype=<your_ssh_sourcetype> | timechart span=1h count by time
```
-	Analyze failed login attempts
```
index=* sourcetype=<your_ssh_sourcetype> | search action="failure"
```
-	Investigate SSH sessions from unusual or suspicious source IP addresses
```
index=* sourcetype=<your_ssh_sourcetype> | search src_ip="suspicious_ip"
```

### 5.	Monitor User Behavior
-	Identify users with multiple failed login attempts
```
index=* sourcetype=<your_ssh_sourcetype> | search action="failed" | stats count by user
```
-	Analyze user session durations
```
index=* sourcetype=<your_ssh_sourcetype> | stats range(time) as session_duration by session_id | stats avg(session_duration) as avg_session_duration by user
```

### 6. Examples with Screenshots

Example of Searching for SSH Events: <br/>
<img src="https://i.imgur.com/XGsSEmI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Example of Analyzing SSH login attempts action:
<img src="https://i.imgur.com/tsk1r1A.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Example of Analyzing failed login attempts:
<img src="https://i.imgur.com/qtkNuLv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Example of Investigating SSH sessions from unusual or suspicious source IP addresses:
<img src="https://i.imgur.com/nf4fSLZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />

## Conclusion
Analyzing SSH logs with Splunk SIEM provides critical insights into remote access activities, helping to detect anomalies, monitor user behavior, and identify potential security threats. 
