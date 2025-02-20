# SMTP Log Analysis with Splunk SIEM

## Introduction
SMTP (Simple Mail Transfer Protocol) log files contain valuable information about email communication, including sender and recipient addresses, timestamps, email subjects, and more. Analyzing SMTP logs using Splunk SIEM enables security professionals to monitor email traffic, detect anomalies, and identify potential security threats. In this project, I will upload a sample SMTP log file to Splunk SIEM and analyze SMTP logs to identify suspicious activities.

## Prepare Sample SMTP Log File
1.	Download sample [SMTP log file](https://www.secrepo.com/maccdc2012/smtp.log.gz) in a suitable format (e.g., text files).
2.	Ensure the log files contain relevant SMTP events, including timestamps, sender and recipient addresses, email subjects, etc.
3.	Once sample log file is saved, upload the file to Splunk. (Settings > Add Data > Upload)

## Analyze SMTP Log File
### 1.	Search for SMTP Events
-	Open Splunk interface and navigate to the search bar.
-	Enter the following search query to retrieve SMTP events
```
index=* sourcetype=<your_smtp_sourcetype>
```
### 2.	Extract Relevant Fields
-	Identify key fields in SMTP logs such as timestamps, sender and recipient addresses, email subjects, etc.
-	Example extraction command
```
| rex field=_raw "<regex_pattern>"
```
### 3.	Analyze email Traffic Patterns
-	Determine the distribution of email senders.
```
index=<your_smtp_index> sourcetype=<your_smtp_sourcetype> | top limit=10 sender_address
```
- Identify top recipient addresses.
```
index=<your_smtp_index> sourcetype=<your_smtp_sourcetype> | top limit=10 recipient_address
```
### 4. Detect Anomalies
-	Search for unusual patterns in email traffic.
```
index=<your_smtp_index> sourcetype=<your_smtp_sourcetype> | timechart span=1h count by _time
```
- Investigate emails with unusual attachment types or sizes.
```
index=<your_smtp_index> sourcetype=<your_smtp_sourcetype> | search attachment_type="unusual_type" OR attachment_size > 1000000
```
### 5. Monitoring User Behavior
- Identify users with multiple failed login attempts or unauthorized access attempts to email accounts.
```
index=<your_smtp_index> sourcetype=<your_SMTP_sourcetype> | search action="login" status="failed" | stats count by user
```
- Monitor user behavior related to email communication.
```
index=<your_smtp_index> sourcetype=<your_smtp_sourcetype> | stats count by user
```
- Analyze email activity patterns and deviations from normal behavior.
```
index=<your_smtp_index> sourcetype=<your_smtp_sourcetype> | timechart span=1d count by user
```

## Conclusion
Analyzing SMTP log files with Splunk SIEM enhances network security by monitoring email traffic, detecting anomalies, and correlating data for threat detection. By leveraging Splunk's capabilities, organizations can proactively identify and respond to email-based threats, ensuring the integrity and confidentiality of their communications.
