# DNS Log Analysis with Splunk SIEM

## Introduction
DNS (Domain Name System) log analysis is a critical component of network security, providing insights into potential threats and anomalies in network traffic. In this project, I will upload a sample DNS log file to Splunk SIEM and analyze DNS logs to identify suspicious activities.

## Prepare Sample DNS Log File
1.	Download sample [DNS log file](https://www.secrepo.com/maccdc2012/dns.log.gz) in a suitable format (e.g., text files).
2.	Ensure the log files contain relevant DNS events, including source IP, destination IP, domain name, query type, response code, etc.
3.	Once sample log file is saved, upload the file to Splunk. (Settings > Add Data > Upload)

## Analyze DNS Log File
### 1.	Search for DNS Events
-	Open Splunk interface and navigate to the search bar.
-	Enter the following search query to retrieve DNS events
```
index=* sourcetype=<your_dns_sourcetype>
```
### 2.	Extract Relevant Fields
-	Identify key fields in DNS logs such as source IP, destination IP, domain name, query type, response code, etc.
-	Example extraction command
```
| regex _raw="(?i)\b(dns|domain|query|response|port 53)\b"
```

### 3.	Identify Anomalies
-	Search for unusual patterns or anomalies in DNS activities.
-	Example query to identify spikes
```
index=_* OR index=* sourcetype=dns_sample  | stats count by fqdn
```

### 4.	Find the top DNS Sources
-	Use the top command to count the occurrences of each query type
```
index=* sourcetype=dns_sample | top fqdn, src_ip
```

### 5.	Investigate Suspicious Domains
-	Search for domains associated with known malicious activity or suspicious behavior.
-	Utilize threat intelligence feeds or reputation databases to identify malicious domains.
-	Example search for known malicious domains
```
index=* sourcetype=dns_sample fqdn="maliciousdomain.com"
```

### 6. Examples with Screenshots

Example of Searching for DNS Events: <br/>
<img src="https://i.imgur.com/OgCv8lZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Example of Extracting Relevant Field: <br/>
<img src="https://i.imgur.com/ShhCJoK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Example of Extracting Identifying Anomalies: 
<img src="https://i.imgur.com/MnqPKK5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Example of Using Top Command: 
<img src="https://i.imgur.com/UX9lZqX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />

## Conclusion
This project underscores the power of Splunk SIEM in streamlining log analysis and fostering a proactive approach to DNS security. It provides a robust foundation for advanced network monitoring and threat mitigation strategies.







