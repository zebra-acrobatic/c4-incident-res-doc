# System Logs - Student Revision Notes

## What Are System Logs?

System logs are detailed records of events, activities, and transactions that occur within computer systems, applications, and network devices. Think of them as a digital diary that automatically records everything happening on your system - from user logins to error messages, security events, and system changes.

Logs are essential for system administrators, cybersecurity professionals, and IT support staff to monitor system health, troubleshoot problems, and investigate security incidents.

## Types of System Logs

### 1. System Logs
Record operating system events, startup/shutdown processes, hardware changes, and driver installations.
- **Example**: Service start/stop events, system boot messages, hardware failures

### 2. Application Logs
Document events from specific software applications and services.
- **Example**: Database connection errors, web server requests, email server activities

### 3. Security Logs
Track security-related events including authentication attempts, privilege changes, and access violations.
- **Example**: Failed login attempts, account lockouts, permission changes

### 4. Network Logs
Monitor network traffic, connections, and communication between devices.
- **Example**: Firewall blocks, DNS queries, network connection attempts

### 5. Audit Logs
Record user actions and system changes for compliance and accountability purposes.
- **Example**: File modifications, policy changes, administrative actions

## Common Log Sources

- **Operating Systems** (Windows Event Logs, Linux syslog)
- **Web Servers** (Apache, IIS, Nginx)
- **Databases** (MySQL, SQL Server, Oracle)
- **Firewalls and Network Devices** (Cisco, Fortinet, pfSense)
- **Applications** (Custom software, enterprise applications)
- **Antivirus Software** (Windows Defender, Symantec, McAfee)

## General Information Found in Logs

Most log entries contain standard fields that help identify and contextualise events:

- **Timestamp**: When the event occurred (date and time)
- **Source**: Which system, service, or application generated the log
- **Event ID/Type**: Specific identifier for the type of event
- **Severity Level**: Importance of the event (Critical, Error, Warning, Information)
- **User/Process**: Who or what caused the event
- **Description**: Detailed explanation of what happened
- **IP Addresses**: Source and destination network locations
- **File Paths**: Location of affected files or resources

## Windows Log Examples

### Windows Event Viewer Logs

**Security Log - Successful Login:**
```
Log Name: Security
Source: Microsoft-Windows-Security-Auditing
Date: 15/08/2024 09:23:41
Event ID: 4624
Task Category: Logon
Level: Information
User: MELBOURNE-PC\sarah.jones
Description: An account was successfully logged on.
Subject: Account Name: sarah.jones
Logon Type: 2 (Interactive)
Workstation Name: MELBOURNE-PC
Source Network Address: 192.168.1.105
```

**System Log - Service Start:**
```
Log Name: System
Source: Service Control Manager
Date: 15/08/2024 08:15:32
Event ID: 7036
Level: Information
Description: The Windows Firewall service entered the running state.
```

**Application Log - Error Example:**
```
Log Name: Application
Source: Application Error
Date: 15/08/2024 14:27:18
Event ID: 1000
Level: Error
Description: Faulting application name: outlook.exe
Faulting module name: mso.dll
Exception code: 0xc0000005
Fault offset: 0x001a2b4c
Faulting process id: 0x1234
```

## Linux Log Examples

### Syslog Examples

**Authentication Success (Ubuntu):**
```
Aug 15 09:23:41 sydney-server sshd[2847]: Accepted publickey for admin from 203.123.45.67 port 54321 ssh2: RSA SHA256:abc123def456
Aug 15 09:23:41 sydney-server systemd-logind[892]: New session 15 of user admin.
```

**System Error:**
```
Aug 15 14:15:22 brisbane-web kernel: [12345.678910] EXT4-fs error (device sda1): ext4_journal_check_start:83: comm apache2: Detected aborted journal
Aug 15 14:15:22 brisbane-web systemd[1]: apache2.service: Main process exited, code=exited, status=1/FAILURE
```

**Network Activity:**
```
Aug 15 11:30:15 perth-firewall kernel: [FIREWALL-BLOCK] IN=eth0 OUT= MAC=00:1b:21:12:34:56 SRC=118.23.45.67 DST=192.168.1.10 PROTO=TCP SPT=80 DPT=443
```

**Cron Job Execution:**
```
Aug 15 02:00:01 adelaide-backup CRON[3456]: (root) CMD (/usr/local/bin/daily-backup.sh)
Aug 15 02:05:23 adelaide-backup systemd[1]: Started Daily System Backup.
```

## Importance of Logs in Fault Finding

### 1. Problem Identification
Logs help identify exactly what went wrong and when it occurred.
- **Example**: Application crash logs show which module failed and the error code
- **Example**: System logs reveal hardware failures or driver issues

### 2. Root Cause Analysis
Sequential log entries help trace the chain of events leading to a problem.
- **Example**: Following timestamps to see what happened before a service crash
- **Example**: Identifying configuration changes that caused system instability

### 3. Performance Monitoring
Logs reveal performance bottlenecks and resource usage patterns.
- **Example**: Database query logs showing slow-running queries
- **Example**: Web server logs indicating high response times

### 4. System Health Monitoring
Regular log analysis helps identify potential issues before they become critical.
- **Example**: Increasing memory usage warnings in system logs
- **Example**: Growing number of failed authentication attempts

## Importance of Logs in Cybersecurity Investigations

### 1. Incident Detection
Security logs are often the first indicator of a cyber attack or breach.
- **Example**: Multiple failed login attempts from unusual IP addresses
- **Example**: Unusual network traffic patterns in firewall logs

### 2. Attack Timeline Reconstruction
Logs help investigators understand how an attack progressed.
- **Example**: Initial compromise through email → privilege escalation → data exfiltration
- **Example**: Following malware installation and lateral movement through network logs

### 3. Evidence Collection
Digital forensics relies heavily on log evidence for legal proceedings.
- **Example**: Proving unauthorised access through authentication logs
- **Example**: Demonstrating data theft through file access logs

### 4. Impact Assessment
Logs help determine what systems and data were affected during an incident.
- **Example**: Database access logs showing which records were viewed
- **Example**: File system logs indicating which documents were copied

### 5. Attack Attribution
Log analysis can help identify attack methods and potentially the attackers.
- **Example**: Unique malware signatures in antivirus logs
- **Example**: Specific attack patterns matching known threat groups

## Key Log Analysis Tips for Beginners

1. **Start with timestamps** - Follow the chronological order of events
2. **Focus on error and warning messages** - These often indicate problems
3. **Look for patterns** - Repeated events may indicate ongoing issues
4. **Check correlation** - Compare logs from different sources for the same timeframe
5. **Understand normal vs abnormal** - Learn what typical log entries look like
6. **Use filtering and searching** - Don't get overwhelmed by large log files
7. **Document findings** - Keep notes of significant discoveries during analysis

## Conclusion

System logs are invaluable resources for maintaining system health, troubleshooting problems, and investigating security incidents. Understanding how to read and analyse logs is a fundamental skill for anyone working in IT support, system administration, or cybersecurity. Regular log monitoring and analysis can prevent small issues from becoming major problems and provide crucial evidence during security investigations.