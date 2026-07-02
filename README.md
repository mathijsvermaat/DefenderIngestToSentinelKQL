# Defender Ingest To Sentinel KQL

This repository contains two KQL scripts:

1. `xdrdataingest.kql` for Microsoft Defender XDR (including selected Entra ID tables).
2. `mdcdataingest.kql` for Microsoft Defender for Cloud table ingest sizing.

### How to use 
1. Open **Defender portal** > **Advanced Hunting** (under Investigation & Response > Hunting in old portal layout)
2. Paste the contents of either [`xdrdataingest.kql`](xdrdataingest.kql) or [`mdcdataingest.kql`](mdcdataingest.kql)
3. Adjust `Price` and `LookbackDays` if needed
4. Run the query with the desired time range set in the query editor time picker or API `timespan`

The output includes a summary row (`=== TOTAL ===`) with aggregated totals across all tables.

## Output columns

| Column | Description |
|--------|-------------|
| `DefenderProduct` | Source product grouping |
| `TableName` | Log Analytics table name |
| `SizeInGB` | Total size in gigabytes |
| `SizeInMB` | Total size in megabytes |
| `DailyAvgMB` | Average daily ingestion in MB |
| `TotalEvents` | Total event count |
| `IfBillableCostUSD` | Estimated cost if data were billed at the configured price per GB |
| `FirstEvent` / `LastEvent` | Time range of events in the lookback window |


## XDR script

Use `xdrdataingest.kql` to estimate ingestion volume for Microsoft Defender XDR and selected Entra ID tables.

### Tables covered (XDR)

| Product | Tables |
|---------|--------|
| Microsoft Defender for Endpoint (MDE) | `DeviceEvents`, `DeviceFileEvents`, `DeviceLogonEvents`, `DeviceNetworkEvents`, `DeviceProcessEvents`, `DeviceRegistryEvents`, `DeviceImageLoadEvents`, `DeviceNetworkInfo`, `DeviceInfo`, `DeviceFileCertificateInfo` |
| Microsoft Defender for Identity (MDI) | `IdentityLogonEvents`, `IdentityQueryEvents`, `IdentityDirectoryEvents` |
| Microsoft Defender for Office 365 (MDO) | `EmailEvents`, `EmailUrlInfo`, `EmailAttachmentInfo`, `EmailPostDeliveryEvents` |
| Microsoft Defender for Cloud Apps (MCA) | `CloudAppEvents` |
| Microsoft 365 Defender (Alerts) | `AlertEvidence` |
| Microsoft Entra ID (Entra) | `AuditLogs`, `AADNonInteractiveUserSignInLogs`, `AADServicePrincipalSignInLogs`, `AADManagedIdentitySignInLogs`, `AADProvisioningLogs` |

### Parameters (XDR)

| Parameter | Default | Description |
|-----------|---------|-------------|
| `Price` | `3.0` | Cost per GB in USD (used for estimated billable cost) |
| `LookbackDays` | `30` | Number of days to look back |

## Defender for Cloud script

Use `mdcdataingest.kql` to estimate ingestion volume for Defender for Cloud related tables.

### Tables covered (Defender for Cloud)

| Product | Tables |
|---------|--------|
| Microsoft Defender for Cloud (MDC) | `SecurityAlert`, `SecurityBaseline`, `SecurityBaselineSummary`, `SecurityDetection`, `SecurityEvent`, `WindowsFirewall`, `ProtectionStatus`, `Update`, `UpdateSummary`, `MDCFileIntegrityMonitoringEvents`, `WindowsEvent` |

### Parameters (Defender for Cloud)

| Parameter | Default | Description |
|-----------|---------|-------------|
| `Price` | `3.0` | Cost per GB in USD (used for estimated billable cost) |
| `LookbackDays` | `30` | Number of days to look back |

