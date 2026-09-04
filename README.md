# M365 XDR Data Ingest

> [!NOTE]
> **Part of the [Sentinel Maturity Model](https://github.com/mathijsvermaat/Sentinel-Maturity)** — tiered guidance for Microsoft Sentinel data-connector onboarding, retention and detection coverage. This query backs the [XDR Data Volume Insights walkthrough](https://github.com/mathijsvermaat/Sentinel-Maturity/blob/main/procedures/xdr-data-volume-insights.md); use it to decide Analytics vs Data Lake tier per table and record the result in the [assessment checklist](https://mathijsvermaat.github.io/sentinel-maturity-assessment.html).

This KQL query shows all M365 XDR tables (Defender XDR) with their size in GB, event count, daily average, and estimated cost if they were billable. Use it to quantify the cost of sending the raw data to Sentinel Analytics or Data Lake tier.

> **Note:** Entra ID tables (`SigninLogs`, `AuditLogs`, `AAD*`, `ADFSSignInLogs`) are excluded — they are ingested via the Entra ID connector in Sentinel, not the Defender XDR connector.

## Tables covered

| Product | Tables |
|---------|--------|
| Microsoft Defender for Endpoint (MDE) | `DeviceEvents`, `DeviceFileEvents`, `DeviceLogonEvents`, `DeviceNetworkEvents`, `DeviceProcessEvents`, `DeviceRegistryEvents`, `DeviceImageLoadEvents`, `DeviceNetworkInfo`, `DeviceInfo`, `DeviceFileCertificateInfo` |
| Microsoft Defender for Identity (MDI) | `IdentityLogonEvents`, `IdentityQueryEvents`, `IdentityDirectoryEvents` |
| Microsoft Defender for Office 365 (MDO) | `EmailEvents`, `EmailUrlInfo`, `EmailAttachmentInfo`, `EmailPostDeliveryEvents` |
| Microsoft Defender for Cloud Apps (MCA) | `CloudAppEvents` |
| Microsoft 365 Defender (Alerts) | `AlertEvidence` |

## Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `Price` | `3.0` | Cost per GB in USD (used for estimated billable cost) |
| `LookbackDays` | `30` | Number of days to look back |

## How to use

1. Open **Defender portal** > **Advanced Hunting** (under Investigation & Response > Hunting in old portal layout)
2. Paste the contents of [`xdrdataingest.kql`](xdrdataingest.kql)
3. Adjust `Price` and `LookbackDays` if needed
4. Run the query

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

## Related

- **[Sentinel Maturity Model](https://github.com/mathijsvermaat/Sentinel-Maturity)** — the tiered connector guidance model this query belongs to.
- **[XDR Data Volume Insights walkthrough](https://github.com/mathijsvermaat/Sentinel-Maturity/blob/main/procedures/xdr-data-volume-insights.md)** — how to run this query and turn the output into an Analytics vs Data Lake tier decision.
- **[Microsoft Defender XDR connector guidance](https://github.com/mathijsvermaat/Sentinel-Maturity/blob/main/connectors/microsoft-defender-xdr.md)** — per-table retention recommendations, forensic value and detection rationale.
- **[XDR Ingestion Calculator](https://github.com/mathijsvermaat/DefenderIngestToSentinel)** — the PowerShell equivalent, for estimating before the connector is enabled.
- **[Assessment checklist](https://mathijsvermaat.github.io/sentinel-maturity-assessment.html)** — record the per-table GB/day and tier choice.
