---
#bigimg: [{src: "uploads/posts/2019/cover-macbook_report.jpeg"}]
title: "Devices by Boundary and Network Information in SCCM"
subtitle: "Got to have this report for boundaries review :)"
description: ""
author: "Ioan Popovici"
author_link: "https://sccm.zone/"
logo: "/uploads/authors/ioan_popovici.jpg"
date: 2019-03-13T18:42:18.921Z
lastmod: 2019-09-11T12:39:14+03:00
aliases:
    - "/devices-by-boundary-and-network-information-in-sccm-45323b50b080"
tags:
    - SQL
    - SSRS
    - SCCM
    - Boundaries
categories:
    - Reports
---

[Report release history](https://SCCM.Zone/SIT-Devices-by-Boundary-and-Network-CHANGELOG)

This all started with a simple boundary review when I figured It might be handy to have a boundary report. After some research It started to dawn on me that this would not be an easy task.

In the SCCM DB there is no correlation between boundaries and IPs so there goes the easy way. After a lot of banging my head on the desk this is what I came up with. It’s not pretty but I did my best considering my limited SQL knowledge.

Here goes nothing…

## Prerequisites

* Create SQL User Functions (Follow link → Copy/Paste → [`SSMS`](https://docs.microsoft.com/en-us/sql/ssms/sql-server-management-studio-ssms?view=sql-server-2017))
  * [ufn_CIDRFromIPMask.sql](https://snippets.cacher.io/snippet/04acae7ec8247cf0fbb3)
  * [ufn_IsIPInRange.sql](https://snippets.cacher.io/snippet/96ad40077a1d195958fb)
  * [ufn_IsIPInSubnet.sql](https://snippets.cacher.io/snippet/d152a3fe860948796395)
* Report (Follow link → Download)
  * [SIT Devices by Boundary and Network.rdl](https://snippets.cacher.io/snippet/5c0c277f2e2a567514f1)

## Installation

### Import the SSRS Report

* Start Internet Explorer and navigate to [`http://<YOUR_REPORT_SERVER_FQDN>/Reports`](http://en.wikipedia.org/wiki/Fully_qualified_domain_name)
* Choose a path and upload the previously downloaded report file.

### Configure Imported Report

* Replace the [`DataSource`](https://joshheffner.com/how-to-import-additional-software-update-reports-in-sccm/) in the report.

### Create the SQL Support Functions

In order to check if an IP is in range, compute subnet or CIDR, three SQL User Defined Functions need to be created.

* Copy paste the [`<SQL User Defined Function>`](#code) in [`SSMS`](https://docs.microsoft.com/en-us/sql/ssms/sql-server-management-studio-ssms?view=sql-server-2017)
* Change the `<SITE_CODE>` in the `USE` statement to match your Site Code.
* Click `Execute` to add the `ufn_<Support_Function_Name>` to your database.
* Repeat the steps for all three functions.

{{<
    beautifulfigure src="/uploads/posts/2019/DB-ufn-network_computation_functions.png"
    caption="User defined support functions in the CMDB."
    caption-position="bottom" caption-effect="slide"
>}}

## Preview - ADD high resolution screenshoot

{{<
    beautifulfigure src="/uploads/posts/2019/preview-sit_devices_by_boundary_and_network_information.png"
    caption="The collection selection is not shown here. It’s also kind of scrubbed…"
    caption-position="bottom" caption-effect="slide"
>}}

## Code

### ufn_CIDRFromIPMask

Gets the CIDR (‘/’) from a IP Subnet Mask.

{{% details "[Click to expand]" %}}
<script src="https://embed.cacher.io/d05269d35d67ad15faa216970d7e4ca77d59fb42.js?a=4cd86b487fdce0cf633ebc8c2297ceb5&t=github_gist"></script>
{{% /details %}}

### ufn_IsIPInRange

Checks if the IP is in the specified IP range.

{{% details "[Click to expand]" %}}
<script src="https://embed.cacher.io/d95069d40832aa47aefb15c70b241fae2e03ff13.js?a=ca86032df616ad71693f19f645a5a40b&t=github_gist"></script>
{{% /details %}}

### ufn_IsIPInSubnet

Checks if the IP is in the specified subnet using the subnet mask.

{{% details "[Click to expand]" %}}
<script src="https://embed.cacher.io/84573d825d31fc15a1ac149a0e251dae2d08a044.js?a=3d8c251b93e2b820bb7bdf3a42c66c06&t=github_gist"></script>
{{% /details %}}

### SQL Query

For reference only, since the report includes this query.

{{% details "[Click to expand]" %}}
<script src="https://embed.cacher.io/82503e890460a841a9a91d910a2b49a22e08a012.js?a=789306ddd3ff82c26522f1a23b158d9c&t=github_gist"></script>
{{% /details %}}

***

{{<
    beautifulfigure src="/uploads/posts/2019/meme-altita-warrior_paint_blod.gif"
>}}

***

## Support

**Please use [Github](http://SCCM.Zone/GIT) for 🐛 reporting, or 🌈 and 🦄 requests**
