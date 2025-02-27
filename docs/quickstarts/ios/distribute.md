---
title: iOS Distribute Sample Tutorials
description: Tutorial to distribute a sample iOS app to a group in App Center.
keywords: app center
author: lucen-ms
ms.author: lucen
ms.date: 06/27/2017
ms.topic: article
ms.service: vs-appcenter
ms.assetid: e4c351f6-0284-4747-a682-3e0773d3cfe1
---

# Distribute - Sample Swift (iOS) App and Tutorials
In this tutorial, you'll learn to distribute a sample Swift app to a group of users, who can install the app on their device.

If you haven't already, first follow the [getting started tutorial](getting-started.md) to set up the sample app.


### Prerequisites
- Optional: Provisioning Profile and Certificate. Go to the [Apple Developer Documentation](https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppDistributionGuide/MaintainingProfiles/MaintainingProfiles.html) to learn about creating an App ID to codesign the sample iOS app with.

## Choose a build to Distribute
There are **two ways you can distribute** the app.

If you already completed the [Build tutorial](build.md) and built the sample app:
1. From the **Distribute** service, click **Distribute new release** at the top.

2. Click **Choose branch and build** at the bottom of the page.

3. Choose the main branch.  

4. Click on the latest successful and signed build. If there isn't a build, then you haven't signed your builds. The [Build tutorial](build.md) has steps to do so.

5. Click **Next** twice. Leave the **Release notes** blank.

Another way is to upload your own .ipa file from Xcode. Skip this if you followed the steps to distribute above.
1. From the **Distribute** service, click **Distribute new release** at the top.

2. Click **Distribute new release** at the top of the page.

3. Upload your **.ipa** file.

4. Click **Next** twice. Leave the **Release notes** blank.

## Distribute the app

By creating groups of users, you can easily distribute to them all at once, and add new testers when needed:

1. Click **New distribution group**.

2. Name the group **Beta Testers** and add your email in the invitation box below.

3. Press **Create Group**.

4. Click **Next** twice and then **Distribute build**.

## Install the sample app
1. You should receive an email with a link to download the sample app.

2. Open the link and follow the instructions to register your device with App Center, and the app will begin installing.
