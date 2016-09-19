---
author: jnHs
Description: When working with an app in the Windows Dev Center dashboard, you can view details related to the unique identity assigned to it by the Windows Store and get a link to your app's Store listing.
title: View app identity details
ms.assetid: 86F05A79-EFBC-4705-9A71-3A056323AC65
translationtype: Human Translation
ms.sourcegitcommit: a25d87556bb85718f818af5b586f54e6985aaaa4
ms.openlocfilehash: dc61971865a05e1de17cdcf55ab495fee4917b74

---

# View app identity details


When working with an app in the Windows Dev Center dashboard, you can view details related to the unique identity assigned to it by the Windows Store and get a link to your app's Store listing.

To find this info, navigate to one of your apps, then expand **App management** in the left navigation menu. Click **App identity** to view these details.

> **Note**  You need to have a [reserved name](create-your-app-by-reserving-a-name.md) for your app in order to see most of these identity details.

## Values to include in your appx manifest


The following values must be included in your appx manifest. If you are using Microsoft Visual Studio to build your packages, and are signed in with the same Microsoft account that you have associated with your developer account, these details are included automatically. If you're building your package manually, you'll need to add these in.

-   **Package/Identity/Name**
-   **Package/Identity/Publisher**

For more info, see [**Identity**](https://msdn.microsoft.com/library/windows/apps/br211441) in the [package manifest schema reference](https://msdn.microsoft.com/library/windows/apps/br211473).

Together, these elements declare the identity of your app, establishing the "package family" to which all of its packages belong. Individual packages will have additional details, such as architecture and version.

## Additional values for package family


The following values are additional values that refer to your app's package family, but are not included in your manifest.

-   **Package Family Name (PFN)**: This value is used with certain Windows APIs.
-   **Package SID**: You'll need this value to send WNS notifications to your app. For more info, see [Windows Push Notification Services (WNS) overview](https://msdn.microsoft.com/library/windows/apps/mt187203).

## Link to your app's listing

The link to your app's page can be shared to help your customers find the app in the Store. This link is in the format **`https://www.microsoft.com/store/apps/<your app's Store ID>`**.

> **Note**  This URL will work for any OS version on which your app is available. You may also see additional links for Windows 8.1 and earlier and/or Windows Phone 8.1 and earlier, which will work only on the specified OS versions.

When a customer clicks this link, it will open the web-based listing page for your app. If your app is available for the customer's Windows device, the Store app will also launch and display your app's listing.

Your app's **Store ID** is also shown in this section. This Store ID can be used to [generate Store badges](http://go.microsoft.com/fwlink/p/?LinkId=534236) or otherwise identify your app.

 

 







<!--HONumber=Aug16_HO3-->


