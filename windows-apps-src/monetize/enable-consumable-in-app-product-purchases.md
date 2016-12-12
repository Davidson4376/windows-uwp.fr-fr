---
author: mcleanbyron
Description: Offer consumable in-app products&\#8212;items that can be purchased, used, and purchased again&\#8212;through the Store commerce platform to provide your customers with a purchase experience that is both robust and reliable.
title: Enable consumable in-app product purchases
ms.assetid: F79EE369-ACFC-4156-AF6A-72D1C7D3BDA4
keywords: in-app offer code sample
translationtype: Human Translation
ms.sourcegitcommit: ffda100344b1264c18b93f096d8061570dd8edee
ms.openlocfilehash: acb7218bed287f430950d4f8d3621831b269ae18

---

# <a name="enable-consumable-in-app-product-purchases"></a>Enable consumable in-app product purchases


>**Note**&nbsp;&nbsp;This article demonstrates how to use members of the [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) namespace. If your app targets Windows 10, version 1607, or later, we recommend that you use members of the [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) namespace to manage add-ons (also known as in-app products or IAPs) instead of the **Windows.ApplicationModel.Store** namespace. For more information, see [In-app purchases and trials](in-app-purchases-and-trials.md).

Offer consumable in-app products—items that can be purchased, used, and purchased again—through the Store commerce platform to provide your customers with a purchase experience that is both robust and reliable. This is especially useful for things like in-game currency (gold, coins, etc.) that can be purchased and then used to purchase specific power-ups.

## <a name="prerequisites"></a>Prerequisites

-   This topic covers the purchase and fulfillment reporting of consumable in-app products. If you are unfamiliar with in-app products, please review [Enable in-app product purchases](enable-in-app-product-purchases.md) to learn about license information, and how to properly list in-app products in the Store.
-   When you code and test new in-app products for the first time, you must use the [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) object instead of the [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) object. This way you can verify your license logic using simulated calls to the license server instead of calling the live server. To do this, you need to customize the file named WindowsStoreProxy.xml in %userprofile%\\AppData\\local\\packages\\&lt;package name&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData. The Microsoft Visual Studio simulator creates this file when you run your app for the first time—or you can also load a custom one at runtime. For more info, see [Using the WindowsStoreProxy.xml file with CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).
-   This topic also references code examples provided in the [Store sample](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store). This sample is a great way to get hands-on experience with the different monetization options provided for Universal Windows Platform (UWP) apps.

## <a name="step-1-making-the-purchase-request"></a>Step 1: Making the purchase request

The initial purchase request is made with [RequestProductPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/dn263381) like any other purchase made through the Store. The difference for consumable in-app products is that after a successful purchase, a customer cannot purchase the same product again until the app has notified the Store that the previous purchase was successfully fulfilled. It's your app's responsibility to fulfill purchased consumables and notify the Store of the fulfillment.

The following example shows a consumable in-app product purchase request. You'll notice code comments indicating when your app should conduct its local fulfillment of the consumable in-app product for two different scenarios—when the request is successful, and when the request is not successful because of an unfulfilled purchase of that same product.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#MakePurchaseRequest)]

## <a name="step-2-tracking-local-fulfillment-of-the-consumable"></a>Step 2: Tracking local fulfillment of the consumable

When granting your customer access to the consumable in-app product, it's important to keep track of which product is fulfilled (*productId*), and which transaction that fulfillment is associated with (*transactionId*).

>**Important**&nbsp;&nbsp;Your app is responsible for the accurately reporting fulfillment to the Store. This step is essential to maintaining a fair and reliable purchase experience for your customers.

The following example demonstrates use of the [PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392) properties from the [RequestProductPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/dn263381) call in the previous step to identify the purchased product for fulfillment. A collection is used to store the product information in a location that can later be referenced to confirm that local fulfillment was successful.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#GrantFeatureLocally)]

This next example shows how to use the array from the previous example to access product ID/transaction ID pairs that are later used when reporting fulfillment to the Store.

>**Important**&nbsp;&nbsp;Whatever methodology your app uses to track and confirm fulfillment, your app must demonstrate due diligence to ensure that your customers are not charged for items they haven't received.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#IsLocallyFulfilled)]

## <a name="step-3-reporting-product-fulfillment-to-the-store"></a>Step 3: Reporting product fulfillment to the Store

After local fulfillment is completed, your app must make a [ReportConsumableFulfillmentAsync](https://msdn.microsoft.com/library/windows/apps/dn263380) call that includes the *productId* and the transaction the product purchase is included in.

>**Important**&nbsp;&nbsp;Failure to report fulfilled consumable in-app products to the Store will result in the user being unable to purchase that product again until fulfillment for the previous purchase is reported.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#ReportFulfillment)]

## <a name="step-4-identifying-unfulfilled-purchases"></a>Step 4: Identifying unfulfilled purchases

Your app can use the [GetUnfulfilledConsumablesAsync](https://msdn.microsoft.com/library/windows/apps/dn263379) method to check for unfulfilled consumable in-app products at any time. This method should be called on a regular basis to check for unfulfilled consumables that exist due to unanticipated app events like an interruption in network connectivity or app termination.

The following example demonstrates how [GetUnfulfilledConsumablesAsync](https://msdn.microsoft.com/library/windows/apps/dn263379) can be used to enumerate unfulfilled consumables, and how your app can iterate through this list to complete local fulfillment.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#GetUnfulfilledConsumables)]

## <a name="related-topics"></a>Related topics

* [Enable in-app product purchases](enable-in-app-product-purchases.md)
* [Store sample (demonstrates trials and in-app purchases)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/br225197)
 

 



<!--HONumber=Dec16_HO1-->


