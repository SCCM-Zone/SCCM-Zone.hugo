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

## Installation

### Prerequisites

* SQL user defined functions
  * [ufn_CIDRFromIPMask](#ufn-cidrfromipmask)
  * [ufn_IsIPInRange](#ufn-isipinrange)
  * [ufn_IsIPInSubnet](#ufn-isipinsubnet)
* Downloads (Right click → _Download linked file)
  * [SIT Devices by Boundary and Network.rdl](https://raw.githubusercontent.com/SCCM-Zone/sccm-zone.github.io/master/Reporting/Site/SIT%20Device%20by%20Boundary%20and%20Network/SIT%20Devices%20by%20Boundary%20and%20Network.rdl) (SSRS Report)

### Import the SSRS Report

* Start Internet Explorer and navigate to [http://&lt;YOUR_REPORT_SERVER_FQDN&gt;Reports](http://en.wikipedia.org/wiki/Fully_qualified_domain_name)
* Choose a path and upload the previously downloaded report file.

### Configure Imported Report

* [Replace the DataSource](https://joshheffner.com/how-to-import-additional-software-update-reports-in-sccm/) in the report.

### Create the SQL Support Functions

In order to check if an IP is in range, compute subnet or CIDR, three SQL User Defined Functions need to be created.

* Copy paste the [<SQL User Defined Function>](#code) in SSMS
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

{{% details "[Click to expand - Add Code and SQL Reporting rights]" %}}
<script src="https://gist.github.com/Ioan-Popovici/144179a62e9b248328e1d6e723ee043d.js"></script>
{{% /details %}}

### ufn_IsIPInRange

Checks if the IP is in the specified IP range.

{{% details "[Click to expand  - Add Code and SQL Reporting rights]" %}}
<script src=".js"></script>
{{% /details %}}

### ufn_IsIPInSubnet

Checks if the IP is in the specified subnet using the subnet mask.

{{% details "[Click to expand  - Add Code and SQL Reporting rights]" %}}
<script src=".js"></script>
{{% /details %}}

### SQL Query

For reference only, since the report includes this query.

{{% details "[Click to expand - Add code]" %}}
<script src=".js"></script>
{{% /details %}}

***

{{<
    beautifulfigure src="/uploads/posts/2019/meme-altita-warrior_paint_blod.gif"
>}}

***

## Support

**Please use [Github](http://SCCM.Zone/GIT) for 🐛 reporting, or 🌈 and 🦄 requests**
