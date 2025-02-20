# Tunnel Log Traffic Analysis with Splunk SIEM

## Introduction
Tunnel log traffic from Zeek IDS (formerly known as Bro IDS) contains information about various tunneling protocols such as GRE, IPv4, IPv6, etc. Analyzing tunnel log traffic using Splunk SIEM enables security professionals to monitor tunneling activities, detect anomalies, and identify potential security threats.

## Prepare Sample Tunnel Log Files
1.	Download sample [Tunnel log file](https://www.secrepo.com/maccdc2012/tunnel.log.gz) from Zeek IDS in a suitable format (e.g., text files).
2.	Ensure the log files contain relevant tunneling events, including timestamps, source and destination IP addresses, usernames, tunneling protocols, etc.
3.	Once sample log file is saved, upload the file to Splunk. (Settings > Add Data > Upload)

## Analyze Tunnel Log Traffic
### 1.	Search for Tunnel Events
-	Open Splunk interface and navigate to the search bar.
-	Enter the following search query to retrieve tunnel events from Zeek IDS
```
index=* sourcetype=<your_tunnel_sourcetype>
```

### 2.	Extract Relevant Fields
-	Identify key fields in tunnel logs such as timestamps, source and destination IP addresses, tunneling protocols, etc.
-	Example extraction command
```
| rex field=_raw "<regex_pattern>"
```

### 3.	Analyze Tunneling Protocols
-	Determine the distribution of SSH commands executed
```
index=<your_tunnel_index> sourcetype=<your_tunnel_sourcetype> tunnel_protocol=GRE
| stats count by tunnel_protocol
```

### 4.	Detect Anomalies
-	Look for unusual patterns or anomalies in GRE tunneling activity
```
index=<your_tunnel_index> sourcetype=<your_tunnel_sourcetype> tunnel_protocol=GRE
| timechart count by _time
```

### 5.	Monitor GRE Tunneling Activity
-	Monitor GRE tunneling activity over time
```
index=<your_tunnel_index> sourcetype=<your_tunnel_sourcetype> tunnel_protocol=GRE
| timechart span=1h count by _time

```
## Conclusion
Integrating Zeek IDS tunnel logs into Splunk SIEM provides security teams with the necessary visibility and tools to proactively monitor, detect, and respond to tunneling-related security incidents, ultimately enhancing overall cybersecurity posture and resilience against emerging threats.
