# Detection Model

## 1. Detection Implementation Model

Detections in this lab follow a structured workflow:

1. Select MITRE ATT&CK technique
2. Identify suspicious behavioral indicators
3. Translate behavior into SPL query
4. Validate ingestion
5. Convert query into scheduled alert
6. Perform controlled execution for validation
7. Document limitations and future improvements

---

## 2. Event Types Used for Detection

Current detection cases rely primarily on the following Sysmon telemetry:

### Sysmon Event ID 1 – Process Creation

This event provides enriched visibility into process execution.

Key fields leveraged:

- Image
- CommandLine
- ParentImage
- ParentProcessId
- User
- IntegrityLevel
- ProcessId
- ComputerName

These fields allow behavioral analysis instead of static signature-based detection.
