# Scenario

During routine monitoring of a Windows 10 endpoint, suspicious PowerShell activity is detected.

The activity involves the execution of PowerShell with command-line parameters commonly associated with script-based execution and potential abuse of built-in Windows tools (LOLBins).

## Assumptions
- The endpoint is a corporate-managed Windows workstation.
- The activity is not part of a known administrative task.
- The user executing the command does not typically run PowerShell scripts as part of their role.

## Objective
Determine whether the observed PowerShell activity represents malicious behavior, a false positive, or a misconfiguration, and decide on the appropriate response.
