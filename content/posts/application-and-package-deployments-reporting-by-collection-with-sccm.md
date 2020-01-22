---
#bigimg: [{src: "uploads/posts/2018/cover-bitlocker_user_chair.jpeg"}]
title: "Application and Package Deployments Reporting by Collection with MEMCM/SCCM"
subtitle: "Deployments reports for all collection members available get it while it’s hot…"
description: "Lists the application or package deployments for a MEMCM/SCCM device or user collection."
author: "Ioan Popovici"
author_link: "https://sccm.zone/"
logo: "/uploads/authors/ioan_popovici.jpg"
date: 2018-05-23T16:51:00.786Z
lastmod: 2019-08-23T12:36:41+03:00
tags:
    - SQL
    - SSRS
    - MEMCM/SCCM
    - Applications
    - Packages
    - Deployments
    - Security
categories:
    - Reports
aliases:
    - "/package-and-application-deployments-reporting-by-collection-with-sccm-64199bbcdc6c"
---

[Report release history](https://SCCM.Zone/DE-Deployments-by-Device-or-User-CHANGELOG)

No default reports exists that can display results for each collection member. There is a report that lists deployments for a specific collection but that’s it. If a collection member has a deployment on a different collection you are out of luck. Well not anymore…

I used the [`All package and program deployments`](https://docs.microsoft.com/en-us/sccm/core/servers/manage/list-of-reports#software-distribution---package-and-program-deployment) to a specified user/device as reference. One of the problems was that the `User` report has a bug and returns `NULL` for most packages. Since the views are so well documented, it took a while to get it right.

## Prerequisites

* Report (Follow link → Copy/Download)
  * [DE Software Deployments by Device or User.rdl](https://snippets.cacher.io/snippet/a8c54490242f96c2f43a)

## Installation

### Upload Report to SSRS

* Start Internet Explorer and navigate to [`http://<YOUR_REPORT_SERVER_FQDN>/Reports`](http://en.wikipedia.org/wiki/Fully_qualified_domain_name)
* Choose a path and upload the previously downloaded report file.

### Configure Imported Report

* Replace the [`DataSource`](https://joshheffner.com/how-to-import-additional-software-update-reports-in-sccm/) in the report.

## Preview

{{< gallery >}}

{{<
    beautifulfigure src="/uploads/posts/2018/preview-de-software-deployments-by-device-or-user_a.png"
    caption="User application deployments"
    caption-position="bottom" caption-effect="slide"
    hover-effect="slideup"
>}}

{{<
    beautifulfigure src="/uploads/posts/2018/preview-de-software-deployments-by-device-or-user_b.png"
    caption="Device application deployments"
    caption-position="bottom" caption-effect="slide"
    hover-effect="slideup"
>}}

{{< /gallery >}}

> **Notes**
> I did not post screenshots with package deployments because of severe laziness but you get the picture.

## Code

### SQL Queries

For reference only, reports already include these queries.

#### Application Deployments

{{< details "[Click to expand]" >}}
<script src="https://embed.cacher.io/815539d65961ad15a9ae47c50d7913a67f5aad10.js?a=05f6022fa311628003703128e475bcfb&t=github_gist"></script>
{{< /details >}}

#### Package Deployments

{{< details "[Click to expand]" >}}
<script src="https://embed.cacher.io/d7046c89083aac12a0fb13c55f2e1bf67909fd48.js?a=887e02ddfdf124481a69303f24b3a93c&t=github_gist"></script>
{{< /details >}}

***

{{<
    beautifulfigure src="/uploads/posts/2018/meme-hold-my-beer-i-got-this.jpeg"
>}}

***

## Support

**Please use [Github](http://SCCM.Zone/GIT) for 🐛 reporting, or 🌈 and 🦄 requests**
