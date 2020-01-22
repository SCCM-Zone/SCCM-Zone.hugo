---
#bigimg: [{src: "uploads/posts/2019/cover-macbook_report.jpeg"}]
title: "MEMCM/SCCM Application Detection Method using in Console Application Version"
subtitle: "Say no to application version hard-coded in the detection script."
description: "How-to use the MEMCM/SCCM console application version as a detection method."
author: "Ioan Popovici"
author_link: "https://sccm.zone/"
logo: "/uploads/authors/ioan_popovici.jpg"
date: 2019-04-01T16:19:15.931Z
lastmod: 2019-09-11T12:39:20+03:00
tags:
    - MEMCM/SCCM
    - Application
    - Powershell
    - DetectionMethod
categories:
    - HowTo
    - Scripts
aliases:
    - "/sccm-application-detection-method-using-in-console-application-version-59a755995942"
---

This method is valid for any MEMCM/SCCM application. In this case I will use the configuration manager client upgrade application. You might ask yourself, why not use the automatic client upgrade? Well, because the client upgrade can only run during a maintenance window. I won’t go into details, but unless this behavior changes, I cannot use this feature with my current environment.

Without further ado, here is how to do it…

## Create the Application

I wanted to make this as simple as possible so I don’t have to tinker with it every time there’s a new update.

### Set the configuration manager application version

The version will be used by the discovery method, make sure you use the correct one.

### Set the application source

This path will remain the same, so there is no need to touch this ever again.

`<ConfigMgrServer>\<ConfigMgrPath>\Client`

### Create the installation script

In order to be able to specify the local installation source we need to use a script. Useful in low bandwidth environments.

<script src="https://embed.cacher.io/d6033dd3053bad41faf813905b2b1da67e5aaa47.js?a=16ec5caa3f5d4abf745efff14fe500c8&t=github_gist"></script>

### Set the discovery method

<script src="https://embed.cacher.io/d0566d835960fe16ada9419b0c2848af2c0faa47.js?a=416f41347cef57be422b9dbaadfadb76&t=github_gist"></script>

> **Notes**
> Remember to change the application name accordingly.

### Set the Install String

```powershell
Powershell.exe -File Install-CMClient.ps1 -ExecutionPolicy ‘Bypass’ -WindowStyle ‘Hidden’
```

### Set the uninstall string

```cmd
CCMSETUP.EXE /Uninstall
```

## Deploy the application

The only thing to do now is to deploy the newly created application. If a new version of the client becomes available you just need to change the application version and update the content.

> **Notes**
> The production version of the configuration manager client is always used, so make sure you promote the pilot to production if you want to use the latest version.

***

{{<
    beautifulfigure src="/uploads/posts/2019/meme-scripting_sccm_enhanced_detection_methods.jpeg"
>}}

***

## Support

**Please use [Github](http://SCCM.Zone/GIT) for 🐛 reporting, or 🌈 and 🦄 requests**
