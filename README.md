# M365 XDR Data Ingest

This KQL query shows all M365 XDR tables (Defender XDR) with their size in GB, event count, daily average, and estimated cost if they were billable. Use it to quantify the cost of sending the raw data to Sentinel Analytics or Data Lake tier.

The main query in [xdrdataingest.kql](xdrdataingest.kql) includes both Defender XDR and Entra ID tables.

## Tables covered

| Product | Tables |
|---------|--------|
| Microsoft Defender for Endpoint (MDE) | `DeviceEvents`, `DeviceFileEvents`, `DeviceLogonEvents`, `DeviceNetworkEvents`, `DeviceProcessEvents`, `DeviceRegistryEvents`, `DeviceImageLoadEvents`, `DeviceNetworkInfo`, `DeviceInfo`, `DeviceFileCertificateInfo` |
| Microsoft Defender for Identity (MDI) | `IdentityLogonEvents`, `IdentityQueryEvents`, `IdentityDirectoryEvents` |
| Microsoft Defender for Office 365 (MDO) | `EmailEvents`, `EmailUrlInfo`, `EmailAttachmentInfo`, `EmailPostDeliveryEvents` |
| Microsoft Defender for Cloud Apps (MCA) | `CloudAppEvents` |
| Microsoft 365 Defender (Alerts) | `AlertEvidence` |
| Microsoft Entra ID (Entra) | `AuditLogs`, `AADNonInteractiveUserSignInLogs`, `AADServicePrincipalSignInLogs`, `AADManagedIdentitySignInLogs`, `AADProvisioningLogs` |

## Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `Price` | `3.0` | Cost per GB in USD (used for estimated billable cost) |
| `LookbackDays` | `30` | Number of days to look back |

## How to use

1. Open **Defender portal** > **Advanced Hunting** (under Investigation & Response > Hunting in old portal layout)
2. Paste the contents of [`xdrdataingest.kql`](xdrdataingest.kql)
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
