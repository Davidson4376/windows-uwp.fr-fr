---
author: mcleanbyron
ms.assetid: 5E722AFF-539D-456E-8C4A-ADE90CF7674A
description: If your app offers a large in-app product catalog, you can optionally follow the process described in this topic to help manage your catalog.
title: Manage a large catalog of in-app products
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 529735319848fc0b8fac12e51b8536b178db0646

---

# Manage a large catalog of in-app products




>**Note**&nbsp;&nbsp;This article demonstrates how to use members of the [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) namespace. If your app targets Windows 10, version 1607, or later, we recommend that you use members of the [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) namespace to manage add-ons (also known as in-app products or IAPs) instead of the **Windows.ApplicationModel.Store** namespace. For more information, see [In-app purchases and trials](in-app-purchases-and-trials.md).

If your app offers a large in-app product catalog, you can optionally follow the process described in this topic to help manage your catalog. You will create a handful of product entries for specific price tiers, with each one able to represent hundreds of products within a catalog.

To enable this capability, use the [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382) method overload that specifies an app-defined offer associated with an in-app product listed in the Store. In addition to specifying an offer and product association during the call, your app should also pass a [**ProductPurchaseDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/dn263384) object that contains the large catalog offer details. If these details are not provided, the details for the listed product will be used instead.

The Store will only use the *offerId* from the purchase request in the resulting [**PurchaseResults**](https://msdn.microsoft.com/library/windows/apps/dn263392). This process does not directly modify the information originally provided when [listing the in-app product in the Store](https://msdn.microsoft.com/library/windows/apps/mt148551).

**Note**  Starting with Windows 10, the Store has no limit to the number of product listings per developer account. In previous releases, the Store has a limit of 200 product listings per developer account, and the process described in this topic can be used to work around that limitation.

## Prerequisites

-   This topic covers Store support for the representation of multiple in-app offers using a single in-app product listed in the Store. If you are unfamiliar with in-app purchases please review [Enable in-app product purchases](enable-in-app-product-purchases.md) to learn about license information, and how to properly list your in-app purchase in the Store.
-   When you code and test new in-app offers for the first time, you must use the [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) object instead of the [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) object. This way you can verify your license logic using simulated calls to the license server instead of calling the live server. To do this, you need to customize the file named "WindowsStoreProxy.xml" in %userprofile%\\AppData\\local\\packages\\&lt;package name&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData. The Microsoft Visual Studio simulator creates this file when you run your app for the first time—or you can also load a custom one at runtime. For more info, see **CurrentAppSimulator**.
-   This topic also references code examples provided in the [Store sample](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store). This sample is a great way to get hands-on experience with the different monetization options provided for Universal Windows Platform (UWP) apps.

## Make the purchase request for the in-app product

The purchase request for a specific product within a large catalog is handled in much the same way as any other purchase request within an app. When your app calls the new [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382) method overload, your app provides both an *OfferId* and a [**ProductPurchaseDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/dn263390) object populated with the name of the in-app product.

```CSharp
string offerId = "1234";
string displayPropertiesName = "MusicOffer1";
var displayProperties = new ProductPurchaseDisplayProperties(displayPropertiesName);

try
{
    PurchaseResults purchaseResults = await CurrentAppSimulator.RequestProductPurchaseAsync("product1", offerId, displayProperties);
    switch (purchaseResults.Status)
    {
        case ProductPurchaseStatus.Succeeded:
            // Grant the user their purchase here, and then pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
            // To indicate local fulfillment to the Windows Store.
            break;
        case ProductPurchaseStatus.NotFulfilled:
            // First check for unfulfilled purchases and grant any unfulfilled purchases from an earlier transaction.
            // Once products are fulfilled pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
            // To indicate local fulfillment to the Windows Store.
            break;
        case ProductPurchaseStatus.NotPurchased:
            // Notify user that the purchase was not completed due to cancellation or error.
            break;
    }
}
catch (Exception)
{
    //Notify the user of the purchase error.
}
```

## Report fulfillment of the in-app offer

Your app will need to report product fulfillment to the Store as soon as the offer has been fulfilled locally. In a large catalog scenario, if your app does not report offer fulfillment, the user will be unable to purchase any in-app offers using that same Store product listing.

As mentioned earlier, the Store only uses provided offer info to populate the [**PurchaseResults**](https://msdn.microsoft.com/library/windows/apps/dn263392), and does not create a persistent association between a large catalog offer and Store product listing. As a result you need to track user entitlement for products, and provide product-specific context (such as the name of the item being purchased or details about it) to the user outside of the [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382) operation.

The following code demonstrates the fulfillment call, and a UI messaging pattern in which the specific offer info is inserted. In the absence of that specific product info, the example uses info from the product [**ListingInformation**](https://msdn.microsoft.com/library/windows/apps/br225163).

```CSharp
string offerId = "1234";
product1ListingName = product1.Name;
string displayPropertiesName = "MusicOffer1";

if (String.IsNullOrEmpty(displayPropertiesName))
{
    displayPropertiesName = product1ListingName;
}
var offerIdMsg = " with offer id " + offerId;
if (String.IsNullOrEmpty(offerId))
{
    offerIdMsg = " with no offer id";
}

FulfillmentResult result = await CurrentAppSimulator.ReportConsumableFulfillmentAsync(productId, transactionId);
switch (result)
{
    case FulfillmentResult.Succeeded:
        Log("You bought and fulfilled " + displayPropertiesName + offerIdMsg, NotifyType.StatusMessage);
        break;
    case FulfillmentResult.NothingToFulfill:
        Log("There is no purchased product 1 to fulfill.", NotifyType.StatusMessage);
        break;
    case FulfillmentResult.PurchasePending:
        Log("You bought product 1. The purchase is pending so we cannot fulfill the product.", NotifyType.StatusMessage);
        break;
    case FulfillmentResult.PurchaseReverted:
        Log("You bought product 1. But your purchase has been reverted.", NotifyType.StatusMessage);
        // Since the user' s purchase was revoked, they got their money back.
        // You may want to revoke the user' s access to the consumable content that was granted.
        break;
    case FulfillmentResult.ServerError:
        Log("You bought product 1. There was an error when fulfilling.", NotifyType.StatusMessage);
        break;
}
```

## Related topics

* [Enable in-app product purchases](enable-in-app-product-purchases.md)
* [Enable consumable in-app product purchases](enable-consumable-in-app-product-purchases.md)
* [Store sample (demonstrates trials and in-app purchases)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382)
* [**ProductPurchaseDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/dn263384)



<!--HONumber=Aug16_HO5-->


