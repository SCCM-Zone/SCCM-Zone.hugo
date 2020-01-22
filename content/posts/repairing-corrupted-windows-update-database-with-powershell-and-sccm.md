---
#bigimg: [{src: "uploads/posts/2018/cover-bsod_meme.jpeg"}]
title: "Repairing corrupted Windows Update Database with PowerShell and MEMCM/SCCM"
subtitle: "When a botched software update corrupts half of your fleet windows update database, this script might come in handy…"
description: "How-to repair a corrupted Windows Update Database using PowerShell."
author: "Ioan Popovici"
author_link: "https://sccm.zone/"
logo: "/uploads/authors/ioan_popovici.jpg"
date: 2018-05-17T17:43:31.100Z
lastmod: 2020-01-21T12:36:39+03:00
tags:
    - MEMCM/SCCM
    - WindowsUpdate
    - PowerShellx
    - ConfigurationItems
categories:
    - Scripts
    - HowTo
aliases:
    - "/repairing-corrupted-windows-update-database-with-sccm-bb7b25a15daa"
---

[Script release history](https://SCCM.Zone/Repair-WUDatastore-CHANGELOG)

Last month lot of complains about slowness started pouring in and an angry mob began to assemble outside of my office. Somehow the WUDatastore got corrupted on a few thousand machines causing severe slowdowns.

Without further ado let's get right into it.

## Symptoms

* Application event log full of ESENT `Event ID 623` errors, like the one below.

```text
wuaueng.dll (2160) SUS20ClientDataStore: The version store for this instance (0) has reached
its maximum size of 16Mb. It is likely that a long-running transaction is preventing cleanup
of the version store and causing it to build up in size. Updates will be rejected until the
long-running transaction has been completely committed or rolled back.
```

* Major slowdowns

## Solution

The solution was to remove and reinitialize the WUDatastore. Nuking the whole database seems a bit extreme, but then again so is being hanged by an angry mob. I could not find another solution so I added a script to a baseline with automatic remediation. This workaround should take care of any future occurrences. You can also use the run script feature if time is of the essence.

## Prerequisites

* Test environment
* Powershell script (Follow link → Copy/Download)
  * [Repair-WUDataStore.ps1](https://snippets.cacher.io/snippet/2606052d61408363f1ae)

## Script Parameters

The script supports the following parameters.

* `DetectAndRepair` - Performs detection and then performs repairs if necessary.
* `Detect` - Performs detection and returns the result.
* `Repair` - Performs repairs and flushes the specified EventLog.
* `RepairStandalone` - Performs repairs only.

> **Notes**
> Like with any other WUDatastore reinitialization the **update history will be lost**.
> The update install history however will remain untouched in `Programs and Features\View installed updates`.
> Keep in mind that the `Eventlog` used in the detection will also be **cleared** if `RepairStandalone` is not specified.

### Run using the Run Script Feature

Using the run script feature to repair the WUDatabase.

{{<
    beautifulfigure src="/uploads/posts/2018/script-Repair_WUDatastore.png"
    caption="Repair the WUDatastore with the Run Script feature" alt="Using the run script feature" caption-position="bottom" caption-effect="slide"
>}}

### Run using a Configuration Item

Using a configuration item compliance  to repair the WUDatabase.

{{<
    beautifulfigure src="/uploads/posts/2018/configuration_item-ESENT_632_error_not_compliant.png"
    caption="Repair the WUDatastore with a configuration item" alt="Using a configuration item" caption-position="bottom" caption-effect="slide"
>}}

## Code

### PowerShell Script

Can be used standalone, within a CI or with the `Run Script` feature.

{{< details "[Click to expand]" >}}
<script src="https://gist.github.com/Ioan-Popovici/f6abf4e58bb233920c74b15c9cbb1d84.js"></script>
{{< /details >}}

***

{{<
    beautifulfigure src="/uploads/posts/2018/meme-windows_update-coke_bottle_incomming.jpeg"
>}}

***

## Support

**Please use [Github](http://SCCM.Zone/GIT) for 🐛 reporting, or 🌈 and 🦄 requests**
