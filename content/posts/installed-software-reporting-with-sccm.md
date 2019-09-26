---
#bigimg: [{src: "uploads/posts/2019/cover-macbook_report.jpeg"}]
title: "Installed Software Reporting with SCCM"
subtitle: "Installed software reporting made easy…"
description: "Lists installed software by computer or publisher. Supports filtering and exclusions by multiple software names using comma separated values and wildcards."
author: "Ioan Popovici"
author_link: "https://sccm.zone/"
logo: "/uploads/authors/ioan_popovici.jpg"
date: 2018-12-06T19:05:27.062Z
lastmod: 2019-09-12T12:37:41+03:00
aliases:
    - "/installed-software-reporting-with-sccm-5c70108a11c4"
tags:
    - SQL
    - SSRS
    - SCCM
    - Software
categories:
    - Reports
---

[Report release history](http://SCCM.Zone/sw-installed-software-by-user-selection-changelog)

I was sitting on this one for a while for no good reason. I built it and then completely forgot about it. A few days ago I was trying to find a part of the query, used in this report realized that I forgot to publish it.

Most software reports out there are pretty basic so I decided to build something new. The idea was to allow filtering by multiple software names and SQL wildcards. This principle applies to both inclusions and exclusions. The only drawback is that you have to use a SQL string parser for splitting the CSV strings. There is no need to re-invent the wheel, you can use the one created by [Michelle Ufford](http://hadoopsie.com).

At first, I built two reports, one displaying the data by device name and the other by publisher. In the end I ended up by merging them since it does not make sense to have to maintain two reports using the same query.

Also I’ve updated my report template since the old one was an over-designed pile of c**p, too hard to maintain and to reuse. My other reports will also get the upgrade in time.

> **Notes**
> The reports have two data sources since my CM DB in on a HA cluster. I need access to the ReportServer DB to get the report description in the `ReportDescription` dataset, so feel free to switch to one dataset.

## Prerequisites

* Download (Right click → Download linked file)
  * [SW Installed Software by User Selection.rdl](https://raw.githubusercontent.com/SCCM-Zone/sccm-zone.github.io/master/Reporting/Software/SW%20Installed%20Software%20by%20User%20Selection/SW%20Installed%20Software%20by%20User%20Selection.rdl) (SSRS Report)
* Create SQL User Defined Function (Copy/Paste → SSMS)
  * [ufn_csv_String_Parser](#sql-user-defined-function)

## Import the SSRS Report

### Upload Report to SSRS

* Start Internet Explorer and navigate to [http://&lt;YOUR_REPORT_SERVER_FQDN&gt;Reports](http://en.wikipedia.org/wiki/Fully_qualified_domain_name)
* Choose a path and upload the previously downloaded report file.

### Configure Imported Report

* [Replace the DataSource](https://joshheffner.com/how-to-import-additional-software-update-reports-in-sccm/) in the report.

## Create the SQL Support Function

The `ufn_csv_String_Parser` is needed in order to parse the csv input strings.

* Copy paste the [ufn_csv_String_Parser](#ufn-csv-string-parser) in SSMS
* Change the `<SITE_CODE>` in the `USE` statement to match your Site Code.
* Click `Execute` to add the `ufn_csv_String_Parser` function to your database.

{{<
    beautifulfigure src="/uploads/posts/2018/DB-ufn-csv_string_parser.png"
    caption="User defined support function in the CMDB."
    caption-position="bottom" caption-effect="slide"
>}}

> **Notes**
> You might need additional DB access to install the support function!

## Preview

{{< gallery >}}

{{<
    beautifulfigure src="/uploads/posts/2018/preview-sw_installed_software_by_device.png"
    caption="Installed software by device."
    caption-position="bottom" caption-effect="slide"
    hover-effect="slideup"
>}}

{{<
    beautifulfigure src="/uploads/posts/2018/preview-sw_installed_software_by_name.png"
    caption="Installed software by name."
    caption-position="bottom" caption-effect="slide"
    hover-effect="slideup"
>}}

{{< /gallery >}}

{{< gallery >}}
{{<
    beautifulfigure src="/uploads/posts/2018/preview-sw_installed_software_by_publisher_a.png"
    caption="Installed software by publisher (a)."
    caption-position="bottom" caption-effect="slide"
    hover-effect="slideup"
>}}

{{<
    beautifulfigure src="/uploads/posts/2018/preview-sw_installed_software_by_publisher_b.png"
    caption="Installed software by publisher (b)."
    caption-position="bottom" caption-effect="slide"
    hover-effect="slideup"
>}}
{{< /gallery >}}

## Code

### SQL Query

For reference only, the report includes this query.

{{% details "[Click to expand]" %}}
<script src="https://gist.github.com/Ioan-Popovici/.js"></script>
{{% /details %}}

***

{{<
    beautifulfigure src="/uploads/posts/2018/meme-mr_bean-weekly_report_done.jpeg"
>}}

***

## Support

**Please use [Github](http://SCCM.Zone/GIT) for 🐛 reporting, or 🌈 and 🦄 requests**
