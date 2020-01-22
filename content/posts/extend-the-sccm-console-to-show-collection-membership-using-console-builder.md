---
#bigimg: ""
title: "Extend the MEMCM/SCCM Console to show Collection Membership using Console Builder"
subtitle: "Let’s be frank the collection membership should be visible in the console by default. Here’s how to do it…"
description: "View Collection Membership within the MEMCM/SCCM console when selecting a device"
author: "Ioan Popovici"
author_link: "https://sccm.zone/"
logo: "/uploads/authors/ioan_popovici.jpg"
date: 2019-02-28T17:32:05.007Z
lastmod: 2019-09-12T12:38:04+03:00
tags:
    - MEMCM/SCCM
    - ConsoleBuilder
categories:
    - HowTo
    - Extension
aliases:
    - "/extend-the-sccm-console-to-show-collection-membership-using-console-builder-c6db52b408d8"
---

Using RCT to show the collection membership is slow and awkward. In my search to find a better option I stumbled onto the MEMCM/SCCM Console Builder. This is an amazing tool that is already built-in and allows a wide range of customization. I used [`Zeng Yinghua’s`](https://www.scconfigmgr.com/2017/11/09/customizing-configmgr-admin-console-add-details-tab-to-content-status/) blog post as a guide when creating this extension.

Without further ado let’s get to it!

## Recommendations

* Remember to backup your `XmlStorage` folder in the `.\AdminConsole` root.
* You can overwrite this folder on any system that needs the extension with a modified version.
* In this particular case you only need to back-up the `AssetManagementNode.xml` and `ConnectedConsole.xml` files.

## Installation

### Start the Console Builder

Start PowerShell on any machine that has the MEMCM/SCCM console installed and run the following command:

```powershell
& $env:SMS_ADMIN_UI_PATH.Replace('i386', 'AdminUI.ConsoleBuilder.exe')
```

> **Notes**
> Console builder path `<ConfigMgrInstallPath>\AdminConsole\Bin\AdminUI.ConsoleBuilder.exe`
> THE CONSOLE BUILDER HAS NO SAVE FUNCTION! ANY CHANGES WILL BE COMMITTED WHEN THE CONSOLE BUILDER IS CLOSED.

{{<
    beautifulfigure src="/uploads/posts/2019/explorer-sccm_console_XmlStorage.png"
    caption="XmlStorage folder"
    caption-position="bottom" caption-effect="slide"
>}}

### Open the Connected Console

{{<
    beautifulfigure src="/uploads/posts/2019/console_builder-connected_console.png"
    caption="Open the connected console"
    caption-position="bottom" caption-effect="slide"
>}}

### Add a New Tab to the Devices Detail Panel

{{<
    beautifulfigure src="/uploads/posts/2019/console_builder-devices-add_tab_page.png"
    caption="Add a new tab page in the Devices Detail Panel"
    caption-position="bottom" caption-effect="slide"
>}}

&nbsp;

{{<
    beautifulfigure src="/uploads/posts/2019/console_builder-devices-configure_tab_page.png"
    caption="Configure tab to run a query"
    caption-position="bottom" caption-effect="slide"
>}}

### Configure the Devices Tab Query Settings

{{<
    beautifulfigure src="/uploads/posts/2019/console_builder-devices-tab_query_a.png"
    caption="Configure query settings"
    caption-position="bottom" caption-effect="slide"
>}}

&nbsp;

{{<
    beautifulfigure src="/uploads/posts/2019/console_builder-devices-tab_query_b.png"
    caption="Add the tab query and press OK"
    caption-position="bottom" caption-effect="slide"
>}}

### Open the Navigation Alias Table

{{<
    beautifulfigure src="/uploads/posts/2019/console_builder-navigation_aliases-MembersOfCollection.png"
    caption="Edit the MemberOfCollection Alias"
    caption-position="bottom" caption-effect="slide"
>}}

&nbsp;

{{<
    beautifulfigure src="/uploads/posts/2019/console_builder-navigation_aliases-MembersOfCollection-settings.png"
    caption="Edit the Devices View"
    caption-position="bottom" caption-effect="slide"
>}}

### Add a New Tab to the Devices View Detail Panel

{{<
    beautifulfigure src="/uploads/posts/2019/console_builder-navigation_aliases-MembersOfCollection-add_tab_page.png"
    caption="Add a new tab to the devices view detail panel"
    caption-position="bottom" caption-effect="slide"
>}}

### Configure the MembersOfCollection Tab Query Settings

{{<
    beautifulfigure src="/uploads/posts/2019/console_builder-navigation_aliases-MembersOfCollection-tab_query_a.png"
    caption="Configure tab query settings"
    caption-position="bottom" caption-effect="slide"
>}}

&nbsp;

{{<
    beautifulfigure src="/uploads/posts/2019/console_builder-navigation_aliases-MembersOfCollection-tab_query_b.png"
    caption="Add a tab query and press 'OK'"
    caption-position="bottom" caption-effect="slide"
>}}

### Apply Modifications

Click Ok on all open settings, close the Console Builder and start/restart the MEMCM/SCCM Console.

> **Notes**
> THE CONSOLE BUILDER HAS NO SAVE FUNCTION! ANY CHANGES WILL BE COMMITTED WHEN THE CONSOLE BUILDER IS CLOSED.
> Remember to backup your `XmlStorage` folder in the `.\AdminConsole` root.

### Preview

{{<
    beautifulfigure src="/uploads/posts/2019/preview-show_collection_members_extension_a.png"
    caption="Device View extension preview"
    caption-position="bottom" caption-effect="slide"
>}}

&nbsp;

{{<
    beautifulfigure src="/uploads/posts/2019/preview-show_collection_members_extension_a.png"
    src="/uploads/posts/2019/preview-show_collection_members_extension_b.png"
    caption="Collection Device View extension preview"
    caption-position="bottom" caption-effect="slide"
>}}

### Official Documentation

* [https://msdn.microsoft.com/en-us/library/hh948631.aspx](https://msdn.microsoft.com/en-us/library/hh948631.aspx)

### Blogs

* Covers the subject in-depth and is really everything you need
[Extending the Configuration Manager (Current Branch) Console - Part 1](https://blog.itsdelivers.com/productive-it-insights/extending-the-configuration-manager-current-branch-console-part-1)

* A very good example by Zeng Yinghua
[Customizing ConfigMgr Admin Console - Add Details tab to Content Status](https://www.scconfigmgr.com/2017/11/09/customizing-configmgr-admin-console-add-details-tab-to-content-status/)

***

{{<
    beautifulfigure src="/uploads/posts/2019/meme-alita-kung-fu-mirror.gif"
>}}

***

## Support

**Please use [Github](http://SCCM.Zone/GIT) for 🐛 reporting, or 🌈 and 🦄 requests**
