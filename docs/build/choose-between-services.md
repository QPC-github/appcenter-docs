---
title: Choose the right service for app builds
description: Helps user choose between Visual Studio AppCenter and Azure Pipelines for Building their mobile Apps.  
keywords: build
author: lucen-ms
ms.author: lucen
ms.date: 04/16/2020
ms.topic: article
ms.assetid: c7d77240-3a2c-4e0f-9724-20f67dbba6c6
ms.service: vs-appcenter
---

# Choose the right service for app builds
The choice on whether to use [Visual Studio App Center](https://visualstudio.microsoft.com/app-center/) or [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/) requires some thought. There are some scenarios where one service might suit your needs better than the other.  
 
The following questions should help you make an informed decision on which service works best for you. 
 
## App Center
If you answer “Yes” to these questions, App Center is a good choice for you.   

**1. Do you want to configure quick builds for your app, skip configuring build servers locally, avoid complicated configurations and code that builds on a coworker's machine but not yours?**

To [get started](./index.md),

1. Log into App Center at [https://appcenter.ms](https://appcenter.ms).
2. Select the application project you want to build.
3. Open build settings, and connect the App Center app to a cloud-hosted source control system (Azure Repos, GitHub, Bitbucket).
4. Select the repository where the app's source code is located.
5. Set up the chosen branch to be built. 
  
**2. Is your app fairly simple, without many customizations, and is the build straightforward?**

In App Center, if your app follows the usual standard layout on the respective platform and doesn't rely on many external build steps, App Center finds the app automatically in your repository and builds it right away. We take care of creating the steps/tasks that builds the app on [Cloud Build Machines](./software.md). 

All we need to know is the app you want us to build, from the repositories hosted on Azure Repos, Bitbucket, or GitHub.
 
> [!TIP]
> We still offer you a way to do some customizations during build, using [Build Scripts!](./custom/scripts/index.md) 

**3. Do you want a true/continuous Build, Test, and Distribute flow from a Single Service?**
 
App Center lets you not only build the app, but can also execute [launch tests](./build-test-integration.md) and distribute to Alpha/Beta Testers and [App Stores](./build-to-store.md) as part of the build. 
 
**4. Do you use the [App Center Diagnostics](../diagnostics/index.md) SDK in your app (especially for iOS apps?)**
 
When building your app using the App Center Build service, the corresponding debug symbol files (`dSYM` and source map `.zip` files, for iOS apps) will already be forwarded to the App Center Diagnostics service, so you don't need to manually obtain the symbol files and upload them to the diagnostics service as detailed in the [App Center Diagnostics documentation](../diagnostics/ios-symbolication.md#uploading-symbols). 
  
**5. Do you want to manage all things related to your App in one central place?**
 
App Center brings together multiple services commonly used by mobile developers into an integrated cloud solution. Developers use App Center to Build, Test, and Distribute applications. Once the app's deployed, developers monitor the status and usage of the app using the Analytics and Diagnostics services.

> [!NOTE]
> If you feel we're missing something critical from App Center Build or need help, you can always reach out and let us know by opening a support ticket. Select the help menu (?) in the upper right corner of App Center portal, then choose 'Contact support'. Our dedicated support team will respond to your questions and feedback. 

## Azure Pipelines

If you answer “Yes” to these questions, Azure Pipelines may be the best tool for you. 

**1. Did you want to Build other apps (Web apps, for instance)?**
You should stick to Azure Pipelines. App Center only supports the OS/Platforms and services as mentioned in our [Platform Service Matrix page](../general/platform-service-matrix.md)

**2. Are you ready to [create your own Build Pipeline](/azure/devops/pipelines/get-started/pipelines-get-started), create/use existing [tasks](https://github.com/Microsoft/azure-pipelines-tasks) specific to your Mobile app/Platform/Framework?**
  
Azure Pipelines will work out best for you if your app is:
- Fairly complex 
- Has many customizations 
- Uses a framework that’s not supported by App Center 
- Has requirements not currently served by App Center, like special signing considerations

  
> [!NOTE]
> If you feel App Center should be supporting a Platform/framework, don’t hesitate to reach out to us, and let us know by using the blue chat icon in the lower-right corner of every App Center page.

**3. Have you invested already in Azure Pipelines for your Build needs in your Organization?**
 
If you already have a billing plan set for Azure Pipelines, because your organization is already using it for other application needs (like Web apps) you should probably continue using Azure Pipelines for Build.  
 
Billing isn't shared between Azure Pipelines and App Center. The pipelines you purchased for Azure Pipelines can't be used with App Center. 
  
Your team might also be used to Azure Pipelines, and want to continue using the service for builds. In this case, Azure Pipelines might be better for you.   

> [!TIP]
> If you still want to use App Center Features as part of Build, you can use [Distribute](../distribution/vsts-deploy.md) and [Test](../test-cloud/vsts-plugin.md) tasks created for Azure Pipelines!
