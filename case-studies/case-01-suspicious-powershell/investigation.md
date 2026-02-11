# Investigation

## Data Sources Reviewed
The investigation was conducted using the following data sources:
- Sysmon Event ID 1 (Process Create)
- Windows Security Event Logs
- PowerShell command-line telemetry

## Investigation Steps

### 1. Process Execution Analysis
The initial alert pointed to the execution of `powershell.exe`. The following attributes were reviewed:
- Parent process
- Command-line arguments
- Executing user
- Timestamp

The command-line included flags commonly associated with non-interactive or scripted execution, which is unusual for standard user activity.

### 2. User Context
The user account executing the process was reviewed to determine whether PowerShell usage was expected as part of normal duties.

No evidence was found to suggest that the user regularly executes PowerShell scripts as part of their role.

### 3. Parent-Child Process Relationship
The parent process was analyzed to identify how PowerShell was launched. The execution was not initiated by a known administrative or management tool, increasing the suspicion level.

### 4. Additional Activity
No immediate follow-up activity such as lateral movement, persistence mechanisms, or credential access was observed during the initial review window.

## Assessment
Based on the command-line characteristics, user context, and parent process, the activity is considered suspicious but requires containment and monitoring rather than immediate classification as confirmed malicious activity.
