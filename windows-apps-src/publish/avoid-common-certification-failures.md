---
author: jnHs
Description: Review this list to help avoid issues that frequently prevent apps from getting certified, or that might be identified during a spot check after the app is published.
title: Avoid common certification failures
ms.assetid: 9E9E3841-2F9B-42D4-B5F8-4C7C31E42E3D
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 7de2083b2a29baed5a0e6baf0a1e4c4c2f71c9e4

---

# Avoid common certification failures


Review this list to help avoid issues that frequently prevent apps from getting certified, or that might be identified during a spot check after the app is published.

> **Note**  You should also review the [Windows Store Policies](https://msdn.microsoft.com/library/windows/apps/dn764944) to ensure your app meets all of the requirements listed there.

 

-   Submit your app only when it's finished. You're welcome to use your app's description to mention upcoming features, but make sure that your app doesn't contain incomplete sections, links to web pages that are under construction, or anything else that would give a customer the impression that your app is incomplete.

-   [Test your app with the Windows App Certification Kit](https://msdn.microsoft.com/library/windows/apps/mt186449) before you submit your app.

-   Test your app on several different configurations to ensure that it's as stable as possible.

-   Make sure that your app doesn't crash without network connectivity. Even if a connection is required to actually use your app, it needs to perform appropriately when no connection is present.
-   Make sure that your app's description clearly represents what your app does. For help, see our guidance on [writing a great app description](write-a-great-app-description.md).

-   Be sure to provide complete and accurate answers to all of the questions in the [Age ratings](age-ratings.md) section.

-   If your app uses the commerce APIs from the [**Windows.ApplicationModel.Store**](https://msdn.microsoft.com/library/windows/apps/br225197) namespace, make sure to test the app and verify that it handles typical exceptions. Also, make sure that your app uses the [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) class (not the [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) class, which is for testing purposes only).

-   Don't [declare your app as accessible](app-declarations.md#this-app-has-been-tested-to-meet-accessibility-guidelines) unless you have specifically engineered and tested it for accessibility scenarios.

-   Make sure that you [provide any necessary info](notes-for-certification.md) required to use your app, such as the user name and password for a test account if your app requires users to log in to a service, or any steps required to access hidden or locked features.

 

 







<!--HONumber=Aug16_HO3-->


