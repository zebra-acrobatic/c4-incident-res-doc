# System Logs Study Guide
## Linux and Windows Log Management for Beginners

### What are System Logs?
System logs are records of events, errors, and activities that occur on your computer system. They're essential for troubleshooting problems, monitoring system health, and understanding what's happening behind the scenes. Think of them as your system's diary - they tell you what happened, when it happened, and often why it happened.

---

## Linux System Logs

### Primary Log Locations
Most Linux distributions store logs in the `/var/log/` directory. Here are the key files you need to know:

- **`/var/log/syslog`** - General system messages (Ubuntu/Debian)
- **`/var/log/messages`** - General system messages (RedHat/CentOS)
- **`/var/log/auth.log`** - Authentication attempts and security events
- **`/var/log/kern.log`** - Kernel messages
- **`/var/log/boot.log`** - Boot process messages
- **`/var/log/cron.log`** - Scheduled task (cron job) logs
- **`/var/log/apache2/`** - Web server logs (if Apache is installed)

### Viewing Linux Logs

#### Basic Commands
```bash
# View the last 20 lines of syslog
tail -n 20 /var/log/messages

# Follow log in real-time (useful for monitoring)
tail -f /var/log/messages

# View entire log file
cat /var/log/auth.log

# View log with pagination
less /var/log/messages
```

#### Using journalctl (systemd systems)
Modern Linux systems use systemd, which provides the `journalctl` command:
```bash
# View all journal entries
journalctl

# View entries from today
journalctl --since today

# View entries for a specific service
journalctl -u ssh.service

# Follow logs in real-time
journalctl -f
```

### Filtering and Searching Linux Logs

#### Using grep for Pattern Matching
```bash
# Find all failed login attempts
grep "Failed password" /var/log/auth.log

# Find SSH connection attempts
grep "sshd" /var/log/auth.log

# Search for error messages (case-insensitive)
grep -i "error" /var/log/messages

# Show context around matches (3 lines before and after)
grep -C 3 "kernel panic" /var/log/kern.log
```

#### Advanced Filtering with awk and sed
```bash
# Extract specific time range (between 14:00 and 15:00)
awk '/14:0[0-5]/ {print}' /var/log/messages

# Show only timestamps and error messages
grep "ERROR" /var/log/messages | awk '{print $1, $2, $3, $NF}'
```

### Key Information to Look For

#### Security Events Example
```
Mar 15 14:23:45 server01 sshd[12345]: Failed password for invalid user admin from 192.168.1.100 port 22 ssh2
Mar 15 14:23:46 server01 sshd[12345]: Connection closed by 192.168.1.100 port 22 [preauth]
```
**What this tells us:** Someone tried to log in as "admin" from IP 192.168.1.100 and failed.

#### System Error Example
```
Mar 15 15:30:22 server01 kernel: [123456.789] Out of memory: Kill process 9876 (firefox) score 856 or sacrifice child
```
**What this tells us:** The system ran out of memory and killed Firefox to free up resources.

---

## Windows System Logs

### Accessing Windows Event Logs
Windows stores logs in the **Event Viewer**, accessible through:
- **Method 1:** Press `Win + R`, type `eventvwr.msc`, press Enter
- **Method 2:** Start Menu → Administrative Tools → Event Viewer
- **Method 3:** Control Panel → Administrative Tools → Event Viewer

### Key Windows Log Categories

#### Windows Logs (Primary Focus)
- **Application** - Application-specific events and errors
- **Security** - Logon/logoff events, privilege changes, security policy changes
- **System** - Hardware, drivers, and system service events
- **Setup** - Installation and update events

#### Applications and Services Logs
- Contains logs from specific applications and Windows services
- Examples: Microsoft Office, Windows PowerShell, DNS Server

### Using Windows Event Viewer

#### Filtering Events
1. Right-click on a log category (e.g., "System")
2. Select "Filter Current Log"
3. Choose criteria:
   - **Event Level:** Critical, Error, Warning, Information
   - **Event Sources:** Specific services or applications
   - **Time Range:** Last hour, 24 hours, custom range

#### Creating Custom Views
1. Click "Create Custom View" in the Actions panel
2. Set filters for multiple logs simultaneously
3. Save for future use

### PowerShell Commands for Log Management
```powershell
# Get last 10 system events
Get-EventLog -LogName System -Newest 10

# Find specific Event ID
Get-EventLog -LogName System -InstanceId 6005

# Search for events from specific source
Get-EventLog -LogName Application -Source "Application Error"

# Export events to CSV
Get-EventLog -LogName Security | Export-Csv C:\logs\security.csv
```

### Key Information to Look For

#### Successful Logon Example
```
Event ID: 4624
Source: Microsoft-Windows-Security-Auditing
Message: An account was successfully logged on.
Subject: SYSTEM
Account Name: john.smith
Logon Type: 2 (Interactive)
```
**What this tells us:** User "john.smith" successfully logged on interactively (locally).

#### System Error Example
```
Event ID: 7034
Source: Service Control Manager
Level: Error
Message: The Print Spooler service terminated unexpectedly. This is the 3rd time in 60000 milliseconds.
```
**What this tells us:** The Print Spooler service has crashed 3 times in one minute.

#### Application Crash Example
```
Event ID: 1000
Source: Application Error
Message: Faulting application name: notepad.exe
Faulting module name: ntdll.dll
Exception code: 0xc0000005
```
**What this tells us:** Notepad crashed due to an access violation in ntdll.dll.

---

## Common Event IDs to Remember

### Windows Security Events
- **4624** - Successful logon
- **4625** - Failed logon attempt
- **4634** - Account logged off
- **4648** - Logon using explicit credentials
- **4672** - Administrative privileges assigned

### Windows System Events
- **6005** - Event Log service started (system boot)
- **6006** - Event Log service stopped (system shutdown)
- **7034** - Service terminated unexpectedly
- **7035** - Service started/stopped successfully

---

## Best Practices for Log Analysis

### 1. Start with Recent Events
Always check the most recent entries first, as they're most likely related to current issues.

### 2. Look for Patterns
- Multiple failed login attempts might indicate a brute force attack
- Repeated service crashes suggest hardware or software problems
- Regular successful events help establish normal system behaviour

### 3. Cross-Reference Logs
Check multiple log sources to get a complete picture of events. For example, if you see network issues in system logs, check application logs for related errors.

### 4. Document Important Findings
Keep notes of significant events, especially security incidents or recurring problems.

### 5. Regular Monitoring
Set up automated alerts for critical events rather than waiting for problems to escalate.

---

## Quick Reference Commands

### Linux
```bash
# Real-time monitoring
tail -f /var/log/messages

# Search for specific text
grep "error" /var/log/messages

# View systemd service logs
journalctl -u servicename

# Show logs from last hour
journalctl --since "1 hour ago"
```

### Windows PowerShell
```powershell
# Get recent system errors
Get-EventLog -LogName System -EntryType Error -Newest 20

# Find specific Event ID
Get-EventLog -LogName Security -InstanceId 4625

# Clear application log (admin required)
Clear-EventLog -LogName Application
```

Remember: System logs are powerful diagnostic tools. Regular monitoring and understanding these logs will make you much more effective at troubleshooting and maintaining systems!