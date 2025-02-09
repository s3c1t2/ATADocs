---
title: Plan capacity for deployment | Microsoft Defender for Identity
description: Plan your deployment and decide how many Microsoft Defender for Identity servers are needed to support your network.
ms.date: 08/27/2023
ms.topic: how-to
---

# Plan capacity for Microsoft Defender for Identity deployment

This article describes how to use the Microsoft Defender for Identity sizing tool to determine whether your domain controller servers have enough resources for a Microsoft Defender for Identity sensor.

While domain controller performance may not be affected if the server doesn't have required resources, the Defender for Identity sensor may not operate as expected. For more information, see [Microsoft Defender for Identity prerequisites](prerequisites.md).

The sizing tool measures the capacity needed for domain controllers only. There is no need to run it against AD FS / AD CS servers, as the performance impact on AD FS / AD CS servers is extremely minimal to not existent.

> [!TIP]
> By default, Defender for Identity supports up to 350 sensors. To install more sensors, contact Defender for Identity support.
>

## Prerequisites

- Download the [Defender for Identity sizing tool](<https://aka.ms/mdi/sizingtool>).
- Review the [Defender for Identity architecture](../architecture.md) article.
- Review the [Defender for Identity prerequisites](prerequisites.md) article.

To ensure accurate results, only run the sizing tool *before* you've installed any Defender for Identity sensors in your environment.

## Use the sizing tool

1. Run the Defender for Identity sizing tool, **TriSizingTool.exe**, from the zip file you downloaded.

1. When the tool finishes running, open the Excel file results.

1. In the Excel file, locate and select the **Azure ATP Summary** sheet, and then check the **Sensor Supported** column for results that indicate whether your server is supported.

    For example:

    :::image type="content" source="../media/capacity-tool.png" alt-text="Screenshot of a sample capacity planning tool." lightbox="../media/capacity-tool.png":::

    > [!NOTE]
    > The other sheet in the file is used for [Advanced Threat Analytics (ATA)](/advanced-threat-analytics/what-is-ata) planning and isn't needed for Defender for Identity.
    >

The sizing tool determines whether your server is supported based on the **Busy Packets/Second** value, which is calculated based on the 15 busiest minutes over a 24 hour period.

Common results include:

|Result  |Description  |
|---------|---------|
|**Yes**     |   The sensor is supported on your server      |
|**Yes, but additional resources required** | The sensor is supported on your server as long you add any specified missing resources.  |
|**Maybe**     |     The current **Busy Packets/Second** value may be significantly higher at that point than average. Check the timestamps to understand the processes running at that time, and whether you can limit the bandwidth for those processes under normal circumstances.     |
|**Maybe, but additional resources required** |The sensor may be supported on your server as long you add any specified missing resources, or the **Busy packets / Second** may be above 60K |
|**No**     |    The sensor isn't supported on your server. <br><br>The current **Busy Packets/Second** value may be significantly higher at that point than average. Check the timestamps to understand the processes running at that time, and whether you can limit the bandwidth for those processes under normal circumstances.    |
|**Missing OS Data**     |  There was an issue reading the operating system data.  Make sure the connection to your server is able to query WMI remotely.   |
|**Missing Traffic Data**     | There was an issue reading the traffic data.    Make sure the connection to your server is able to query performance counters remotely.        |
|**Missing RAM data**     |    There was an issue reading the RAM data.  Make sure the connection to your server is able to query WMI remotely.       |
|**Missing core data**     |   There was an issue reading the core data. Make sure the connection to your server is able to query WMI remotely.          |

For example, the following image shows a set of results where the **Maybe** indicates that the **Busy Packets/Second** value is significantly higher at that point than average.  Note that the **Display DC Times as UTC/Local** is set to *Local DC Time*. This setting helps highlight the fact that the values were taken at around 3:30 AM.

:::image type="content" source="../media/capacity-tool-maybe.png" alt-text="Screenshot of a capacity tool results showing Maybe values." lightbox="../media/capacity-tool-maybe.png":::

<a name="sizing"></a>
## Defender for Identity sensor estimated sizing

The following table shows the estimated CPU and RAM capacity needed for a Defender for Identity sensor, based on the typical amount of network traffic generated by a domain controller.

**This table is an estimate. The final amount that the sensor parses is dependent on the amount of traffic and the distribution of traffic.**

|Busy packets / second|CPU (physical cores)|RAM (GB)|
|----|----|-----|
|0-1k|0.25|2.50|
|1k-5k|0.75|6.00|
|5k-10k|1.00|6.50|
|10k-20k|2.00|9.00|
|20k-50k|3.50|9.50|
|50k-75k |5.50|11.50|
|75k-100k|7.50|13.50|

In this table:

- CPU and RAM capacity refers to the **sensor's own consumption**, not the domain controller capacity.

- CPU capacity doesn't include hyper-threaded cores. We recommend that you don't work with hyper-threaded cores, which can result in health issues in the Defender for Identity sensor. 

When determining sizing, keep in mind the total number of cores and total amount of memory that will be used by the sensor service.

For more information, see [Resource limitations](../architecture.md#resource-limitations).

## Next step


> [!div class="step-by-step"]
> [Configure endpoint proxy and internet connectivity settings »](configure-proxy.md)
