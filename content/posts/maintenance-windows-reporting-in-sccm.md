---
#bigimg: [{src: "uploads/posts/2019/cover-macbook_report.jpeg"}]
title: "Maintenance Windows Reporting in SCCM"
subtitle: "Sometimes you just need all maintenance windows in one place…"
description: "Lists all SCCM maintenance windows with Start Time and Duration."
author: "Ioan Popovici"
author_link: "https://sccm.zone/"
logo: "/uploads/authors/ioan_popovici.jpg"
date: 2018-08-24T16:23:26.463Z
lastmod: 2019-08-23T12:37:05+03:00
aliases:
    - "/maintenance-windows-reporting-in-sccm-2196784d0e2c"
tags:
    - SQL
    - SSRS
    - SCCM
    - MaintenanceWindows
categories:
    - Reports
---

[Report release history](https://SCCM.Zone/SIT-Maintenance-Windows-CHANGELOG)

There comes a time when you need to cleanup or view all maintenance windows for your environment. I had this for a while but realized only today that there is no builtin report that can show you all maintenance windows in SCCM. Without further ado…

## Prerequisites

* Report (Follow link → Download)
  * [SIT Maintenance Windows by Collection and Type.rdl](https://snippets.cacher.io/snippet/d049710063b6115d8747)

## Installation

### Upload Report to SSRS

* Start Internet Explorer and navigate to [`http://<YOUR_REPORT_SERVER_FQDN>/Reports`](http://en.wikipedia.org/wiki/Fully_qualified_domain_name)
* Choose a path and upload the previously downloaded report file.

> **Notes**
> This report was created with SQL 2014 Reporting Services, you might need to remove some report elements if you use an older version.

### Configure Imported Report

* Replace the [`DataSource`](https://joshheffner.com/how-to-import-additional-software-update-reports-in-sccm/) in the report.

## Preview

{{<
    beautifulfigure src="/uploads/posts/2018/preview-sit-maintenance-windows.png"
    caption="Report preview"
    caption-position="bottom" caption-effect="slide"
>}}

## Code

### SQL Query

For reference only, the report includes this query.

{{% details "[Click to expand]" %}}
<script src="https://embed.cacher.io/d9513dd35833fb42a1a211970e2c1df32a58ae17.js?a=8adc00d55d7d4b68d4c3f0806577d5b7&t=github_gist"></script>
{{% /details %}}

***

{{<
    beautifulfigure src="/uploads/posts/2018/meme-matrix-there_is_no_maintenance.jpeg"
>}}

***

## Support

**Please use [Github](http://SCCM.Zone/GIT) for 🐛 reporting, or 🌈 and 🦄 requests**
