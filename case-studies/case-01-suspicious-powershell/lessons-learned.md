# Lessons Learned

## What Worked Well
- The detection successfully identified abnormal PowerShell usage based on command-line telemetry.
- Sysmon provided sufficient detail to analyze process execution and parent-child relationships.
- A structured investigation workflow helped avoid premature conclusions.

## Areas for Improvement
- Detection logic could be refined to reduce potential false positives by incorporating user baselining.
- Additional telemetry such as PowerShell Script Block Logging would improve visibility.
- Correlation with network activity over a longer time window could strengthen the assessment.

## Future Enhancements
- Implement more granular PowerShell detections aligned with specific ATT&CK sub-techniques.
- Add automated enrichment for user context and historical behavior.
- Develop a dedicated PowerShell abuse playbook to standardize response actions.

## Final Outcome
The incident was documented and classified as suspicious activity with no confirmed malicious impact. Continued monitoring and detection improvements were recommended.
