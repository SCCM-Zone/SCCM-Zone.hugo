---
#bigimg: [{src: "uploads/posts/2018/cover-bitlocker_user_chair.jpeg"}]
title: "BitLocker Compliance and Policy Reporting with SCCM"
subtitle: "If you are looking for a comprehensive BitLocker report, look no more…"
description: ""
author: "Ioan Popovici"
author_link: "https://sccm.zone/"
logo: "/uploads/authors/ioan_popovici.jpg"
date: 2018-11-02T13:03:15.290Z
lastmod: 2019-08-23T12:37:33+03:00
tags:
    - SQL
    - SSRS
    - SCCM
    - Bitlocker
    - Security
categories:
    - Reports
aliases:
    - "/bitlocker-compliance-and-policy-reporting-with-sccm-86539b6940a6"
---

[Report release history](https://SCCM.Zone/SEC-BitLocker-Compliance-And-Policy-CHANGELOG)

My vague promises of publishing a BitLocker report based on HWI seem to have come true. This is a complete report that also displays BitLocker GPO settings. In order to get the BitLocker and Policy data, you need to extend the SCCM Hardware Inventory. If you don’t want to do that you can use my [`BitLocker Configuration Baseline`](https://SCCM.Zone/Get-BitlockerStatus) together with the [`Baseline Report with Actual Values`](https://SCCM.Zone/CB-Configuration-Baseline-Compliance) report.

## Recommendations

* Use a test environment for validation!
* Back-up your [`configuration.mof`](https://technet.microsoft.com/en-us/library/bb680858.aspx) file before any changes!
* Use [`mofcomp.exe`](https://docs.microsoft.com/en-us/windows/win32/wmisdk/mofcomp) to validate the [`configuration.mof`](https://technet.microsoft.com/en-us/library/bb680858.aspx) on a test machine first!

> **Notes**
> Hardware inventory extension needs to be done on the top of your hierarchy.
> **Allow** some time for the policy to be downloaded or force a policy refresh.
> **Allow** some time for the data to be gathered or force a HWI collection.
> This report was created with SQL 2017 Reporting Services, you might need to remove some report elements if you use an older version.

## Prerequisites

* Test environment
* Extensions (Follow link → Copy/Paste)
  * [HWI EXT Win32_EncryptableVolume_Ext.mof](https://snippets.cacher.io/snippet/dd3ad984e4dada9fbec5)
  * [HWI EXT Win32Reg_BitlockerPolicy.mof](https://snippets.cacher.io/snippet/5d7adc9bc208a6bcad95)
* Definitions (Follow link → Download)
  * [HWI DEF Win32_EncryptableVolume_Ext.mof](https://snippets.cacher.io/snippet/f3d44f4475a2e79490f7)
  * [HWI DEF Win32Reg_BitlockerPolicy.mof](https://snippets.cacher.io/snippet/f3d44f4475a2e79490f7)
* Reports (Follow link → Download)
  * [SEC Bitlocker Compliance and Policy.rdl](https://snippets.cacher.io/snippet/5c82da245acc0824786f) (SSRS Report)
  * [SR Display Formatted Text.rdl](https://snippets.cacher.io/snippet/d02f2115aae43a153809) (SSRS Sub-Report)

## Installation

### HWI Extension

The previously downloaded extensions need to be added to the `<CMInstallLocation>\Inboxes\clifiles.src\hinv\configuration.mof` file

* Insert the extensions at the end of the [`configuration.mof`](https://technet.microsoft.com/en-us/library/bb680858.aspx) file between the following headers:

```text
//========================
// Added extensions start
//========================

//========================
// Added extensions end
//========================
```

* Use a test environment for validation as described in the [Test and Validation](#test-and-validation) section.

> **Notes**
> Always use a test environment before any changes in production!
> Never create any extensions outside of the `Added extensions start/end` headers.
> Try to have consistent formatting inside these headers.
> Never modify anything outside these headers.
> Watch for other previous extensions and use clear delimitations between them.

### Apply changes

Compiling the configuration.mof file in the hinv folder on the CAS/PSS, will trigger the distribution and compilation on all machines in your environment on the next machine policy evaluation.

```cmd
mofcomp.exe <CMInstallLocation>\Inboxes\clifiles.src\hinv\Configuration.mof
```

{{<
    beautifulfigure src="/uploads/posts/2018/pwsh-mofcomp-compile_configuration.mof.png"
    caption="Compile the configuration.mof file" alt="Apply changes by compiling the configuration.mof file" caption-position="bottom" caption-effect="slide"
>}}

### HWI Definitions

You need to add the new class definitions to the Default Client Settings

* Import definitions.

{{<
    beautifulfigure src="/uploads/posts/2018/client_settings-import_Win32_EncryptableVolume_Ext_definitions_a.png"
    caption="Click on Import and select the HWI DEF Win32_EncryptableVolume_Ext.mof file"
    caption-position="bottom" caption-effect="slide"
>}}

&nbsp;

{{<
    beautifulfigure src="/uploads/posts/2018/client_settings-import_Win32_EncryptableVolume_Ext_definitions_b.png"
    caption="Review the class and click 'Import'"
    caption-position="bottom" caption-effect="slide"
>}}

&nbsp;

{{<
    beautifulfigure src="/uploads/posts/2018/client_settings-import_Win32Reg_BitlockerPolicy_definitions_a.png"
    caption="Click on Import and select the HWI DEF Win32Reg_BitlockerPolicy.mof file"
    caption-position="bottom" caption-effect="slide"
>}}

&nbsp;

{{<
    beautifulfigure src="/uploads/posts/2018/client_settings-import_Win32Reg_BitlockerPolicy_definitions_b.png"
    caption="Review the class and click 'Import'"
    caption-position="bottom" caption-effect="slide"
>}}

&nbsp;

{{<
    beautifulfigure src="/uploads/posts/2018/client_settings-import_bitlocker_definitions.png"
    caption="Make sure the new extension classes are enabled and click 'OK'"
    caption-position="bottom" caption-effect="slide"
>}}

&nbsp;

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
    beautifulfigure src="/uploads/posts/2018/pwsh-mofcomp-check_and_compile_configuration.mof.png"
    caption="Compiling the configuration.mof is done on a test environment here!"
    caption-position="bottom" caption-effect="slide"
>}}

&nbsp;

> **Notes**
> Saving and compiling the configuration.mof file in the hinv folder on the CAS/PSS, will trigger the distribution and compilation on all machines in your environment on the next machine policy evaluation.

### WMI

Use PowerShell to check if the new classes have been created in WMI

```powershell
## Check if the new classes are present in WMI
/* The machine must have the configuration.mof compiled in order for these
classes to show up*/

#  Get the EncryptableVolume extended class
Get-CimClass -ClassName Win32_EncryptableVolume_Ext

#  Get the BitlockerPolicy class
Get-CimClass -ClassName Win32Reg_BitlockerPolicy
```

{{<
    beautifulfigure src="/uploads/posts/2018/pwsh-wmi-bitlocker_classes.png"
    caption="CMDB extension views"
    caption-position="bottom" caption-effect="slide"
>}}

### Database

Use [`SSMS`](https://docs.microsoft.com/en-us/sql/ssms/sql-server-management-studio-ssms?view=sql-server-2017) (SQL Server Management Studio) to check if the views are created in the CM database

{{<
    beautifulfigure src="/uploads/posts/2018/DB-hwi_bitlocker_extension_views.png"
    caption="CMDB extension views"
    caption-position="bottom" caption-effect="slide"
>}}

## Import the SSRS Report

The report is comprised of a report and a sub-report. If you rename the sub-report you will have to change the hard coded value in the main report. Report and sub-report need to be placed in the same folder.

### Upload Report to SSRS

* Start Internet Explorer and navigate to [`http://<YOUR_REPORT_SERVER_FQDN>/Reports`](http://en.wikipedia.org/wiki/Fully_qualified_domain_name)
* Choose a path and upload the previously downloaded report files.

### Configure Imported Report

* Replace the [`DataSource`](https://joshheffner.com/how-to-import-additional-software-update-reports-in-sccm/) in the report.

## Preview

{{< gallery >}}

{{<
    beautifulfigure src="/uploads/posts/2018/preview_sec-bitlocker_compliance_and_policy_a.png"
    caption="Bitlocker Compliance and Policy report."
    caption-position="bottom" caption-effect="slide"
    hover-effect="slideup"
>}}

{{<
    beautifulfigure src="/uploads/posts/2018/preview_sec-bitlocker_compliance_and_policy_b.png"
    caption="BitLocker Policy sub-report (Settings without values are filtered out automatically)."
    caption-position="bottom" caption-effect="slide"
    hover-effect="slideup"
>}}

{{< /gallery >}}

## Code

### Extensions

For reference only, you can download the files in the [Prerequisites](#prerequisites) section.

#### HWI EXT Win32_EncryptableVolume_Ext

{{< details "[Click to expand]" >}}
<script src="https://embed.cacher.io/84023bd1583ba244fcae40c25e7c13f1795efa44.js?a=068ee8a301be70303d2ba01094d20cb1&t=github_gist"></script>
{{< /details >}}

#### HWI EXT Win32Reg_BitlockerPolicy

{{< details "[Click to expand]" >}}
<script src="https://embed.cacher.io/d5023fd15861a312faa8149b5b2b48f47a5fa044.js?a=ff436a1d0f93f585e3971eeb965bfbf2&t=github_gist"></script>
{{< /details >}}

### Definitions

For reference only, you can download the files in the [Prerequisites](#prerequisites) section.

#### HWI DEF Win32_EncryptableVolume_Ext

{{< details "[Click to expand]" >}}
<script src="https://embed.cacher.io/86556c840864ae44aeaf45915f2a13a3220bff46.js?a=0d0c2d11f28c1825f27f1dd36cd03e84&t=github_gist"></script>
{{< /details >}}

#### HWI DEF Win32Reg_BitlockerPolicy

{{< details "[Click to expand]" >}}
<script src="https://embed.cacher.io/d20530815e61a214aeab1cc00e244fa02a5ef843.js?a=919483b5e59734b34f318e05e85ee9df&t=github_gist"></script>
{{< /details >}}

### SQL Query

For reference only, the report includes this query.

{{< details "[Click to expand]" >}}
<script src="https://embed.cacher.io/815e39850433f841ffaf419b082a1ef12258a144.js?a=befad5d1a8f8d5bf079895fca36bfcc7&t=github_gist"></script>
{{< /details >}}

***

{{<
    beautifulfigure src="/uploads/posts/2018/meme-matrix-codes_everywhere.jpeg"
>}}

***

## Support

**Please use [Github](http://SCCM.Zone/GIT) for 🐛 reporting, or 🌈 and 🦄 requests**
