---
#bigimg: [{src: "uploads/posts/2019/cover-macbook_report.jpeg"}]
title: SQL Server Product and Version Reporting with SCCM
subtitle: SQL licensing is always a pain but this report should make it a little easier…
description: SCCM hardware inventory extension for SQL versioning.
author: Ioan Popovici
author_link: https://sccm.zone/
logo: "/uploads/authors/ioan_popovici.jpg"
toc: false
date: 2019-07-18T16:19:15.931Z
lastmod: 2019-09-11T12:39:20+03:00
aliases:
    - ""
tags:
    - SQL
    - SSRS
    - SCCM
    - HWI
    - vbScript
categories:
    - Reports
    - Scripts
---

[Report release history](https://SCCM.Zone/SW-SQL-Server-Products-CHANGELOG)

[Previous report version](https://sccm-zone.com/sql-version-detection-and-report-sccm-2012-r2-12f299b5e63b)

[Originaly published on sccmf12twice.com](https://SCCM.Zone/SW-SQL-Server-Products-EXT)

This is the second iteration of my SQL version report. When I look back on my previous work I always cringe and this was no exception. A while back, I received a request to add the SQL key to the report, so I began examining the old code. Horrified by the things that I found laying dormant there, I scrapped everything and started anew. The result is a brand new report with a lot more info, smaller database footprint and much better coding.

## Recommendations

* Do not modify or remove the previous extension version until you thoroughly test the new version.
* Use a test environment for validation!
* Back-up your [`configuration.mof`](https://technet.microsoft.com/en-us/library/bb680858.aspx) file before any changes!
* Use [`mofcomp.exe`](https://docs.microsoft.com/en-us/windows/win32/wmisdk/mofcomp) to validate the [`configuration.mof`](https://technet.microsoft.com/en-us/library/bb680858.aspx) on a test machine first!

> **Notes**
> This version is compatible with the previous version, they can live side by side.
> Hardware inventory extension needs to be done on the top of your hierarchy.
> **Allow** some time for the policy to be downloaded or force a policy refresh.
> **Allow** some time for the data to be gathered or force a HWI collection.
> This report was created with SQL 2017 Reporting Services, you might need to remove some report elements if you use an older version.

## Prerequisites

* Test environment
* Extension and Defintions (Follow link → Copy/Download)
  * [HWI EXT SQL Server Products.mof](https://snippets.cacher.io/snippet/b6321fe05d176fc1d5a3) (Extension)
  * [HWI DEF SQL Server Products.mof](https://snippets.cacher.io/snippet/fa0e948d7088a098b41b) (Definitions)
* Report (Follow link → Download)
  * [SW SQL Server Products.rdl](https://snippets.cacher.io/snippet/871434ea1c5a8e1fa5aa) (SSRS Report)
* Stored Procedure (Follow link → Copy/Paste → [`SSMS`](https://docs.microsoft.com/en-us/sql/ssms/sql-server-management-studio-ssms?view=sql-server-2017))
  * [usp_PivotWithDynamicColumns.sql](https://snippets.cacher.io/snippet/ae245a5955322880f1c9)

## Installation

### HWI Extension

The extension needs to be added to the `<CMInstallLocation>\Inboxes\clifiles.src\hinv\configuration.mof` file

* Insert the extension at the end of the [`configuration.mof`](https://technet.microsoft.com/en-us/library/bb680858.aspx) file between the following headers:

```text
//========================
// Added extensions start
//========================

//========================
// Added extensions end
//========================
```

* Uncomment the `Old SQL extension cleanup` section to remove the old extension classes from the clients repositories if needed.
* Use a test environment for validation as described in the [Test and Validation](#test-and-validation) section.

> **Notes**
> Always use a test environment before any changes in production!
> Never create any extensions outside of the `Added extensions start/end` headers.
> Try to have consistent formatting inside these headers.
> Never modify anything outside these headers.
> Watch for other previous extensions and use clear delimitations between them.

### Apply changes

Compiling the [`configuration.mof`](https://technet.microsoft.com/en-us/library/bb680858.aspx) file in the hinv folder on the CAS/PSS, will trigger the distribution and compilation on all machines in your environment on the next machine policy evaluation.

```cmd
mofcomp.exe <CMInstallLocation>\Inboxes\clifiles.src\hinv\Configuration.mof
```

{{<
    beautifulfigure src="/uploads/posts/2019/pwsh-mofcomp-compile_configuration.mof.png"
    caption="Compile the configuration.mof file" alt="Apply changes by compiling the configuration.mof file" caption-position="bottom" caption-effect="slide"
>}}

### HWI Definitions

You need to add the new class definitions to the Default Client Settings

* Import definitions.

{{<
    beautifulfigure src="/uploads/posts/2019/client_settings-hwi-import_sql_definitions_a.png"
    caption="Click on Import and select the HWI DEF SQL Server Products.mof file"
    caption-position="bottom" caption-effect="slide"
>}}

&nbsp;

{{<
    beautifulfigure src="/uploads/posts/2019/client_settings-hwi-import_sql_definitions_b.png"
    caption="Review the classes and click 'Import'"
    caption-position="bottom" caption-effect="slide"
>}}

&nbsp;

{{<
    beautifulfigure src="/uploads/posts/2019/client_settings-hwi-import_sql_definitions_c.png"
    caption="Make sure the new extension classes are enabled and click 'OK'"
    caption-position="bottom" caption-effect="slide"
>}}

&nbsp;

> **Notes**
> DO NOT DELETE the old extension definitions if you still want to use the old report!

## Test and Validation

### Configuration.mof

Use [`mofcomp.exe`](https://docs.microsoft.com/en-us/windows/win32/wmisdk/mofcomp) to check if [`configuration.mof`](https://technet.microsoft.com/en-us/library/bb680858.aspx) was correctly modified and implement the changes.

```powershell
## Check syntax
mofcomp.exe -check <Configuration.mof_Directory>\Configuration.mof

## Compile file
/*
Compiling the configuration.mof file in the hinv folder on the CAS/PSS,
will trigger the distribution and compilation on all machines in your
environment on the next machine policy evaluation.
*/

mofcomp.exe <Configuration.mof_Directory>\Configuration.mof
```

{{<
    beautifulfigure src="/uploads/posts/2019/pwsh-mofcomp-check_and_compile_configuration.mof.png"
    caption="Compiling the configuration.mof is done on a test environment here!"
    caption-position="bottom" caption-effect="slide"
>}}

&nbsp;

> **Notes**
> Saving and compiling the [`configuration.mof`](https://technet.microsoft.com/en-us/library/bb680858.aspx) file in the hinv folder on the CAS/PSS, will trigger the distribution and compilation on all machines in your environment on the next machine policy evaluation.

### WMI

Use PowerShell to check if the new classes have been created in WMI

```powershell
## Check if the new classes are present in WMI
/* The machine must have at least one version of SQL installed in
order for these classes to be created */

#  Get SQL 2017 class
Get-CimClass -ClassName SQL_2017_Property

#  Get SQL 2014 class
Get-CimClass -ClassName SQL_2014_Property

#  Get SQL 2012 class
Get-CimClass -ClassName SQL_2012_Property

#  Get SQL 2008 class
Get-CimClass -ClassName SQL_2008_Property

#  Get SQL Legacy class
Get-CimClass -ClassName SQL_Legacy_Property

#  Get SQL ProductID class
Get-CimClass -ClassName SQL_ProductID
```

### Database

Use [`SSMS`](https://docs.microsoft.com/en-us/sql/ssms/sql-server-management-studio-ssms?view=sql-server-2017) (SQL Server Management Studio) to check if the views are created in the CM database

{{<
    beautifulfigure src="/uploads/posts/2019/DB-hwi_sql_extension_views.png"
    caption="CMDB extension views"
    caption-position="bottom" caption-effect="slide"
>}}

## Import the SSRS Report

### Upload Report to SSRS

* Start Internet Explorer and navigate to [`http://<YOUR_REPORT_SERVER_FQDN>/Reports`](http://en.wikipedia.org/wiki/Fully_qualified_domain_name)
* Choose a path and upload the previously downloaded report file.

### Configure Imported Report

* Replace the [`DataSource`](https://joshheffner.com/how-to-import-additional-software-update-reports-in-sccm/) in the report.

## Create the SQL Stored Procedure

The [`usp_PivotWithDynamicColumns`](#sql-stored-procedure) is needed in order to maximize code reuse and have a more sane and sanitized data source.

* Copy paste the [`usp_PivotWithDynamicColumns`](#sql-stored-procedure) in [`SSMS`](https://docs.microsoft.com/en-us/sql/ssms/sql-server-management-studio-ssms?view=sql-server-2017)
* Change the `<SITE_CODE>` in the `USE` statement to match your Site Code.
* Click `Execute` to add the [`usp_PivotWithDynamicColumns`](#sql-stored-procedure) stored procedure to your database.

> **Notes**
> You might need additional DB access to install the support function!

## Preview

{{<
    beautifulfigure src="/uploads/posts/2019/preview-sw_sql_server_products.png"
    caption="Report preview"
    caption-position="bottom" caption-effect="slide"
>}}

## Code

### Extension

For reference only, you can download the file in the [Prerequisites](#prerequisites) section.

{{% details "[Click to expand]" %}}
<script src="https://embed.cacher.io/82503b820d64ff40acfe15940c7b49a67f0ef842.js?a=082e5185facf1c8324545916152f9894&t=github_gist"></script>
{{% /details %}}

### Definitions

For reference only, you can download the file in the [Prerequisites](#prerequisites) section.

{{% details "[Click to expand]" %}}
<script src="https://embed.cacher.io/860738d50536a214aeaa1c9b5b2d13af790fa813.js?a=f283c62834708e79cc125dad3b782c3b&t=github_gist"></script>
{{% /details %}}

### SQL Stored Procedure

Needs to be created as a prerequisite.

{{% details "[Click to expand]" %}}
<script src="https://embed.cacher.io/81033a840963af49acaf1791082512a77d0afa48.js?a=7630f583e8d0b8bfa6615782e3ef3a6b&t=github_gist"></script>
{{% /details %}}

### SQL Query

For reference only, the report includes this query.

{{% details "[Click to expand]" %}}
<script src="https://embed.cacher.io/865539800c36f816fcf813c6582f19a62903a912.js?a=19be45f5d31c708cce0d376811014fbd&t=github_gist"></script>
{{% /details %}}

### VB Support Function

For reference only, the report includes this function.

{{% details "[Click to expand]" %}}

<script src="https://embed.cacher.io/83576ed10a66f816abf812925f284fa17d58fc14.js?a=dac2e390b609c7b9b8c4f97ff5b0a4cc&t=github_gist"></script>
{{% /details %}}

&nbsp;

> **Notes**
> Credit to Jakob Bindslet and Chrissy LeMaire.

***

{{<
    beautifulfigure src="/uploads/posts/2019/meme-alita-hand_stand.gif"
>}}

***

## Support

**Please use [Github](http://SCCM.Zone/GIT) for 🐛 reporting, or 🌈 and 🦄 requests**
