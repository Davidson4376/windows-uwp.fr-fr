---
author: mcleanbyron
ms.assetid: B356C442-998F-4B2C-B550-70070C5E4487
description: Learn how to use the Windows.Services.Store namespace to purchase an app or one of its add-ons.
title: Enable in-app purchases of apps and add-ons
keywords: in-app offer code sample
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 0347c3a72ccdf26ddd885a5bad944ae36e09a190

---

# Enable in-app purchases of apps and add-ons

Apps that target Windows 10, version 1607, or later can use members in the [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) namespace to request the purchase the current app or one of its add-ons (also known as in-app products or IAPs) for the user. For example, if the user currently has a trial version of the app, you can use this process to purchase a full license for the user. Alternatively, you can use this process to purchase an add-on, such as a new game level for the user.

To request the purchase of an app or add-on, the [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) provides several different methods:
* If you know the [Store ID](in-app-purchases-and-trials.md#store_ids) of the app or add-on, you can use the [RequestPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.requestpurchaseasync.aspx) method of the [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) class.
* If you already have a [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx), [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) or [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) object that represents the app or add-on, you can use the **RequestPurchaseAsync** methods of these objects.

Each method presents a standard purchase UI to the user and then completes asynchronously after the transaction is complete. The method returns an object that indicates whether the transaction was successful.

>**Note**&nbsp;&nbsp;This article is applicable to apps that target Windows 10, version 1607, or later. If your app targets an earlier version of Windows 10, you must use the [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) namespace instead of the **Windows.Services.Store** namespace. For more information, see [In-app purchases and trials using the Windows.ApplicationModel.Store namespace](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## Prerequisites

This example has the following prerequisites:
* A Visual Studio project for a Universal Windows Platform (UWP) app that targets Windows 10, version 1607, or later.
* You have created an app in the Windows Dev Center dashboard, and this app is published and available in the Store. This can be an app that you want to release to customers, or it can be a basic app that meets minimum [Windows App Certification Kit](https://developer.microsoft.com/windows/develop/app-certification-kit) requirements that you are using for testing purposes only. For more information, see the [testing guidance](in-app-purchases-and-trials.md#testing).

The code in this example assumes:
* The code runs in the context of a [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) that contains a [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) named ```workingProgressRing``` and a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) named ```textBlock```. These objects are used to indicate that an asynchronous operation is occurring and to display output messages, respectively.
* The code file has a **using** statement for the **Windows.Services.Store** namespace.
* The app is a single-user app that runs only in the context of the user that launched the app. For more information, see [In-app purchases and trials](in-app-purchases-and-trials.md#api_intro).

## Code example

This example demonstrates how to use the [RequestPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.requestpurchaseasync.aspx) method of the [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) class to purchase an app or add-on with a known [Store ID](in-app-purchases-and-trials.md#store_ids).

```csharp
private StoreContext context = null;

public async void PurchaseAddOn(string storeId)
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    workingProgressRing.IsActive = true;
    StorePurchaseResult result = await context.RequestPurchaseAsync(storeId);
    workingProgressRing.IsActive = false;

    if (result.ExtendedError != null)
    {
        // The user may be offline or there might be some other server failure.
        textBlock.Text = $"ExtendedError: {result.ExtendedError.Message}";
        return;
    }

    switch (result.Status)
    {
        case StorePurchaseStatus.AlreadyPurchased:
            textBlock.Text = "The user has already purchased the product.";
            break;

        case StorePurchaseStatus.Succeeded:
            textBlock.Text = "The purchase was successful.";
            break;

        case StorePurchaseStatus.NotPurchased:
            textBlock.Text = "The purchase did not complete. " +
                "The user may have cancelled the purchase.";
            break;

        case StorePurchaseStatus.NetworkError:
            textBlock.Text = "The purchase was unsuccessful due to a network error.";
            break;

        case StorePurchaseStatus.ServerError:
            textBlock.Text = "The purchase was unsuccessful due to a server error.";
            break;

        default:
            textBlock.Text = "The purchase was unsuccessful due to an unknown error.";
            break;
    }
}
```

For a complete sample application, see the [Store sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

## Related topics

* [In-app purchases and trials](in-app-purchases-and-trials.md)
* [Get product info for apps and add-ons](get-product-info-for-apps-and-add-ons.md)
* [Get license info for apps and add-ons](get-license-info-for-apps-and-add-ons.md)
* [Enable consumable add-on purchases](enable-consumable-add-on-purchases.md)
* [Implement a trial version of your app](implement-a-trial-version-of-your-app.md)
* [Store sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)



<!--HONumber=Aug16_HO5-->


