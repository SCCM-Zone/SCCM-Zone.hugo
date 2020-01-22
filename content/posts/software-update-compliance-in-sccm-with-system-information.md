---
#bigimg: [{src: "uploads/posts/2019/cover-macbook_report.jpeg"}]
title: "Software Update Compliance in MEMCM/SCCM with System information"
subtitle: "Sometimes you need all the data in one place…"
description: "List software update compliance in MEMCM/SCCM with additional system information."
author: "Ioan Popovici"
author_link: "https://sccm.zone/"
logo: "/uploads/authors/ioan_popovici.jpg"
date: 2018-10-16T17:09:40.259Z
lastmod: 2019-08-23T12:37:28+03:00
tags:
    - SQL
    - SSRS
    - MEMCM/SCCM
    - Updates
categories:
    - Reports
aliases:
    - "/software-update-compliance-in-sccm-with-system-information-71881295d21a"
---

[Report release history](https://SCCM.Zone/SU-Compliance-Classification-Severity-CHANGELOG)

One of the problems I struggled with when creating this report, was the [`Pending Restart Reason`](https://secureinfra.blog/2019/01/10/understanding-and-using-the-pending-restart-feature-in-sccm-current-branch/). I found my answer in some obscure forum. It seems that the value in the DB is in fact a [`bitmask`](https://en.wikipedia.org/wiki/Mask_(computing)). The result is computet by applying the [`bitmask`](https://en.wikipedia.org/wiki/Mask_(computing)) to get all the current restart states for a machine.

Why oh why is the documentation for these features so poorly maintained?

> **Notes**
> The ufn_csv_String_Parser custom function is a prerequisite for this report.
> This report was created with SQL 2017 Reporting Services, you might need to remove some report elements if you use an older version.

## Prerequisites

* Report (Follow link → Copy/Download)
  * [SU Compliance by Computer Classification and Severity.rdl](https://snippets.cacher.io/snippet/5c0c277f2e2a567514f1)
* Create SQL User Function (Follow link → Copy/Paste → [`SSMS`](https://docs.microsoft.com/en-us/sql/ssms/sql-server-management-studio-ssms?view=sql-server-2017))
  * [ufn_csv_String_Parser.sql](https://snippets.cacher.io/snippet/22b47fd513bfb97f3dd5) (String Parser)

## Installation

### Upload Report to SSRS

* Start Internet Explorer and navigate to [`http://<YOUR_REPORT_SERVER_FQDN>/Reports`](http://en.wikipedia.org/wiki/Fully_qualified_domain_name)
* Choose a path and upload the previously downloaded report file.

### Configure Imported Report

* Replace the [`DataSource`](https://joshheffner.com/how-to-import-additional-software-update-reports-in-sccm/) in the report.

## Create the SQL User Function

The [`ufn_csv_String_Parser`](#sql-user-function) is needed in order to split the imput strings.

* Copy paste the [`ufn_csv_String_Parser`](#sql-user-function) in [`SSMS`](https://docs.microsoft.com/en-us/sql/ssms/sql-server-management-studio-ssms?view=sql-server-2017)
* Change the `<SITE_CODE>` in the `USE` statement to match your Site Code.
* Click `Execute` to add the [`ufn_csv_String_Parser`](#sql-user-function) user function to your database.

> **Notes**
> You might need additional DB access to install the support function!

{{<
    beautifulfigure src="/uploads/posts/2018/DB-ufn-csv_string_parser.png"
    caption="User defined support function in the CMDB."
    caption-position="bottom" caption-effect="slide"
>}}

## Preview

{{< gallery >}}

{{<
    beautifulfigure src="/uploads/posts/2018/preview-su_compliance_by_computer_classification_and_severity_a.png"
    caption="Report - Part 1."
    caption-position="bottom" caption-effect="slide"
    hover-effect="slideup"
>}}

{{<
    beautifulfigure src="/uploads/posts/2018/preview-su_compliance_by_computer_classification_and_severity_b.png"
    caption="Report - Part 2"
    caption-position="bottom" caption-effect="slide"
    hover-effect="slideup"
>}}

{{< /gallery >}}

> **Notes**
> The default exclusions are for antivirus definitions. Selecting `ShowInstalledUpdates` will also display installed updates.

## Code

### SQL User Function

Needs to be created as a prerequisite.

{{< details "[Click to expand]" >}}
<script src="https://embed.cacher.io/d2546a840b64fe45a8a946c558241df1285ffd44.js?a=299b685e5b395f67c54f24e4f3bef055&t=github_gist"></script>
{{< /details >}}

### SQL Query

For reference only, you can download the file in the [Prerequisites](#prerequisites) section.

{{< details "[Click to expand]" >}}
<script src="https://embed.cacher.io/83003b820f3ba845aeab1594037c19a57d5aa810.js?a=2344c7c3500c3c111ecd439031b82e32&t=github_gist"></script>
{{< /details >}}

***

{{<
    beautifulfigure src="/uploads/posts/2018/meme-joker-piechart_everyone_loses_their_minds.png"
>}}

***

## Support

**Please use [Github](http://SCCM.Zone/GIT) for 🐛 reporting, or 🌈 and 🦄 requests**
