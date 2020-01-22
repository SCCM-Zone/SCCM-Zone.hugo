---
title: "BIOS version Reporting by Manufacturer and Model with MEMCM/SCCM"
subtitle: "With the Meltdown and Spectre looming on the horizon you might need to have a BIOS version report ready…"
description: "List BIOS version and model with MEMCM/SCCM."
author: "Ioan Popovici"
author_link: "https://sccm.zone/"
logo: "/uploads/authors/ioan_popovici.jpg"
date: 2018-01-25T11:35:30.718Z
lastmod: 2030-02-23T12:36:11+03:00
tags:
    - MEMCM/SCCM
    - BIOS
    - Security
categories:
    - Reports
aliases:
    - "/bios-version-reporting-by-manufacturer-and-model-with-sccm-83f14dcd7225"
---

[Report release history](https://SCCM.Zone/HW-BIOS-Manufacturer-CHANGELOG)

A few days ago I was asked to provide a BIOS version report for our Lenovo and Dell machines. Since you guys might find it useful I decided to publish it.

## Recommendations

* Use a test environment for validation!

> **Notes**
> This report was created with SQL 2014 Reporting Services, you might need to remove some report elements if you use an older version.

## Prerequisites

* Test environment
* Report (Follow link → Download)
  * [HW BIOS by Manufacturer and Model for Collection.rdl](https://snippets.cacher.io/snippet/c9b7bd7231b4797422c8)

## Installation

### Upload Report to SSRS

* Start Internet Explorer and navigate to [`http://<YOUR_REPORT_SERVER_FQDN>/Reports`](http://en.wikipedia.org/wiki/Fully_qualified_domain_name)
* Choose a path and upload the previously downloaded report file.

### Configure Imported Report

* Replace the [`DataSource`](https://joshheffner.com/how-to-import-additional-software-update-reports-in-sccm/) in the report.

## Preview

{{<
    beautifulfigure src="/uploads/posts/2018/preview-hw-bios-by_manufacturer_and_model_for_collection.png"
    caption="Report preview"
    caption-position="bottom" caption-effect="slide"
>}}

## Code

### SQL Query

For reference only, the report includes this query.

{{< details "[Click to expand]" >}}
<script src="https://embed.cacher.io/d7526ad10d3aab14fbaa419a5c7e1fa62802ff17.js?a=eb1315bbf72fb1684cb1a3fd57e671b1&t=github_gist"></script>
{{< /details >}}

***

{{<
    beautifulfigure src="/uploads/posts/2018/meme-mr_bean-access_bios.gif"
>}}

***

## Support

**Please use [Github](http://SCCM.Zone/GIT) for 🐛 reporting, or 🌈 and 🦄 requests**
