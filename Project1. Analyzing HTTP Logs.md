# HTTP Log Analysis with Splunk SIEM

## Introduction
HTTP (Hypertext Transfer Protocol) log files contain valuable information about web server activity, including requests, responses, user agents, and more. Analyzing HTTP logs using Splunk SIEM enables security professionals to monitor web traffic, detect anomalies, and identify potential security threats. In this project, I will upload a sample HTTP log file to Splunk SIEM and analyze HTTP logs to identify suspicious activities.

## Prepare Sample HTTP Log File
1.	Download sample [HTTP log file](https://www.secrepo.com/maccdc2012/http.log.gz) in a suitable format (e.g., text files).
2.	Ensure the log files contain relevant HTTP events, including timestamps, request methods, URLs, response codes, user agents, etc.
3.	Once sample log file is saved, upload the file to Splunk. (Settings > Add Data > Upload)

## Analyze HTTP Log File
### 1.	Search for HTTP Events
-	Open Splunk interface and navigate to the search bar.
-	Enter the following search query to retrieve HTTP events
```
index=<your_http_index> sourcetype=<your_http_sourcetype>
```
### 2.	Extract Relevant Fields
-	Identify key fields in HTTP logs such as timestamps, request methods, URLs, response codes, user agents, etc.
-	Example extraction command
```
| rex field=_raw "<regex_pattern>"
```
### 3.	Analyze Web Traffic Patterns
-	Determine the distribution of request methods (GET, POST, etc.) to understand web traffic patterns.
```
index=<your_http_index> sourcetype=<your_http_sourcetype> | stats count by method
```
- Identify top URLs or endpoints accessed by users.
```
index=<your_http_index> sourcetype=<your_http_sourcetype> | top limit=10 <url or uri>
```
- Analyze response codes to identify errors or successful requests.
```
index=<your_http_index> sourcetype=<your_http_sourcetype> | stats count by status
```
### 4. Detect Anomalies
-	Search for unusual patterns or anomalies in file transfer activities.
```
index=<your_http_index> sourcetype=<your_http_sourcetype> | timechart span=1h count by _time
```
- Analyze high volumes of error responses
```
index=<your_http_index> sourcetype=<your_http_sourcetype> | stats count by status | where status >= 400
```
- Investigate file transfers to or from suspicious IP addresses.
```
index=<your_http_index> sourcetype=<your_http_sourcetype> | search src_ip="suspicious_ip"
```
### 5. Monitoring User Behavior
- Identify users with multiple failed login attempts or unauthorized access attempts.
```
index=<your_http_index> sourcetype=<your_http_sourcetype> | search action="login" status="failed" | stats count by user
```
- Analyze user session durations and access patterns.
```
index=<your_http_index> sourcetype=<your_http_sourcetype> | stats range(_time) as session_duration by session_id | stats avg(session_duration) as avg_session_duration by user
```

### 6. Examples with Screenshots

Example of Extracting Relevant Field: <br/>
<img src="https://i.imgur.com/DYDRLwi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Creating Table to See Network Traffic Communication (Showing only unique destination ip addresses): <br/>
<img src="https://i.imgur.com/6vn4JdO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Example of Using top Command to Detect Malicious Traffic (dst_ip **192.168.229.101** is suspicious): <br/>
<img src="https://i.imgur.com/1qBE0ns.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Finding Top 10 Most Communicating Source ip Addresses with dst_ip **192.168.229.101**: <br/>
<img src="https://i.imgur.com/5qjKlvy.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Example of Analysing High Volumes of Error Responses: <br/>
<img src="https://i.imgur.com/lJjB4HT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />

## Conclusion
Analyzing HTTP logs using Splunk SIEM provides invaluable insights into web server activity and user behavior, enabling proactive detection of potential security threats.
