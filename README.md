***

# AMA vs Defender Coverage Workbook

## ⚠️ This workbook assumes Microsoft Defender XDR data is ingested into Sentinel. Without ingestion, device name normalization and correlation may be inconsistent. To workaround that, copy the KQL query from the Github page and run it in Advanced Hunting in the Defender Portal.

## Overview

This Microsoft Sentinel Workbook provides visibility into Microsoft Defender for Endpoint (MDE)–managed devices and their telemetry coverage within Sentinel. It helps security and operations teams verify that devices are properly configured for comprehensive monitoring by checking:

*   **Azure Monitor Agent (AMA)** installation status
*   **SecurityEvent** log ingestion into Sentinel
*   **Last heartbeat and log timestamps** for freshness

By correlating data from **DeviceInfo**, **Heartbeat**, and **SecurityEvent** tables, the workbook identifies configuration gaps and supports remediation efforts.

***

## Key Features

*   **Coverage Analysis**  
    Detect devices that:
    *   Are onboarded to MDE but missing AMA
    *   Are not sending SecurityEvent logs despite being onboarded

*   **Filtering Options**  
    Filter by:
    *   Workspace
    *   Time range
    *   OS platform
    *   AMA status (All, Yes, No)

*   **Summary Tiles**  
    Quick overview of device counts based on AMA status

*   **Detailed Breakdown**  
    Categorizes devices as:
    *   **MDE + AMA**
    *   **MDE Only**
    *   **AMA Only**

*   **DCR Association**  
    Displays Data Collection Rules (DCRs) linked to machines for AMA configuration

*   **Merged View**  
    Combines AMA-enabled devices with associated DCRs for full visibility

***

## Important Notes

*   **Windows-Only Support**  
    This workbook is designed for **Windows endpoints** (Windows Server and Windows client OS).  
    Linux devices are not included because they do not generate SecurityEvent logs.

*   **SecurityEvent Check**  
    Queries validate Windows security log ingestion into Sentinel using the **SecurityEvent** table.

*   **OS Name Limitation**  
    Some AMA versions do not report the full OS name (e.g., only `Windows` instead of `Windows Server 2025`).  
    This can make filtering by server OS more challenging. Consider using additional metadata or naming conventions for accurate filtering.

***

## How It Works

1.  Collects data from:
    *   **DeviceInfo** (Defender onboarding status)
    *   **Heartbeat** (AMA presence and last seen timestamp)
    *   **SecurityEvent** (Windows security log ingestion)
2.  Joins and correlates AMA presence, Defender onboarding, and log ingestion.
3.  Applies filters for AMA status and OS platform.
4.  Outputs:
    *   Interactive tiles for quick insights
    *   Detailed tables for device-level analysis
    *   Export options for Excel

***

## Use Cases

*   Validate AMA deployment across Defender-managed endpoints
*   Ensure SecurityEvent logs are flowing into Sentinel
*   Identify gaps in telemetry for compliance and security posture
*   Correlate AMA coverage with DCR assignments for troubleshooting

***

## Prerequisites

*   Microsoft Sentinel workspace
*   Defender for Endpoint integration enabled
*   AMA deployed on target machines
*   Relevant tables in Log Analytics:
    *   `DeviceInfo`
    *   `Heartbeat`
    *   `SecurityEvent`

***

## Deployment

1.  Open **Microsoft Sentinel Workbooks**
2.  Click **Add Workbook → Advanced Editor**
3.  Paste the JSON from this repository
4.  Save and customize filters as needed

**Advisory:**  
Set `HasAMA = Yes` for a cleaner merged view  
Default time range is 60 days (adjustable)

***