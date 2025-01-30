# FTP Log Analysis with Splunk SIEM

## Introduction
FTP (File Transfer Protocol) log files contain valuable information about file transfers within a network. In this project, I will upload a sample FTP log file to Splunk SIEM and analyze FTP logs to identify suspicious activities.

## Prepare Sample FTP Log File
1.	Download sample [SSH log file](https://www.secrepo.com/maccdc2012/ftp.log.gz) in a suitable format (e.g., text files).
2.	Ensure the log files contain relevant FTP events, including timestamps, source IP addresses, usernames, commands, filenames, etc.
3.	Once sample log file is saved, upload the file to Splunk. (Settings > Add Data > Upload)

## Analyze FTP Log File
### 1.	Search for FTP Events
-	Open Splunk interface and navigate to the search bar.
-	Enter the following search query to retrieve FTP events
```
index=* sourcetype=<your_FTP_sourcetype>
```
### 2.	Extract Relevant Fields
-	Identify key fields in FTP logs such as timestamps, source IP addresses, usernames, commands, filename, etc.
-	Example extraction command
```
| rex field=_raw "^(?<timestamp>\d{4}-\d{2}-\d{2}\s+\d{2}:\d{2}:\d{2}).*?(?<source_ip>\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}).*?(?<username>\w+).*?(?<command>[A-Z]+).*?(?<file_path>\/[\w\/.-]+)
"
```
Explanation:
- `^`: Start of the line.
- `(?<timestamp>\d{4}-\d{2}-\d{2}\s+\d{2}:\d{2}:\d{2})`: Matches and captures the timestamp in the format "YYYY-MM-DD HH:MM:SS".
- `.*?`: Matches any character (except for line terminators) as few times as possible.
- `(?<source_ip>\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})`: Matches and captures the source IP address.
- `(?<username>\w+)`: Matches and captures the username (assuming it consists of alphanumeric characters).
- `(?<command>[A-Z]+)`: Matches and captures the FTP command (assuming it consists of uppercase letters).
- `(?<file_path>\/[\w\/.-]+)`: Matches and captures the file path (assuming it starts with "/" and can contain alphanumeric characters, "/", ".", and "-").


### 3.	Analyze File Transfer Activity
-	Determine the frequency and volume of file transfers
-	Identify top users or IP addresses involved in file transfers
-	Analyze the types of files being transferred (e.g., documents, executables, archives)
- Use stats command to calculate statistics such as count, sum, avg, etc

### 4.	Detect Anomalies
-	Look for unusual patterns in file transfer activity
-	Analyze sudden spikes or drops in file transfer volume
-	Investigate file transfer to or from suspicious IP addresses
-	Use statistical analysis or machine learning models to detect anomalies

### 5.	Monitor User Behavior
-	Monitor user behavior during file transfers
-	Identify users with multiple failed login attempts or unauthorized access attempts
-	Analyze user activity pattern and deviations from normal behavior

## Conclusion
Analyzing FTP logs with Splunk SIEM provides critical insights into file transfer activities within a network.
