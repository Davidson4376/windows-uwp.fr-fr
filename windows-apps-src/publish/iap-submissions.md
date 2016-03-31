---
IAPs are published through the Windows Dev Center dashboard.
IAP submissions
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
---

# IAP submissions


An IAP (in-app product) is a supplementary item for your app that can be purchased by customers. An IAP can be a fun new add-on feature, a new game level, or anything else you think will keep users engaged. Not only are IAPs a great way to make money, but they help to drive customer interaction and engagement.

IAPs are published through the Windows Dev Center dashboard. You'll also need to [enable the IAPs](https://msdn.microsoft.com/library/windows/apps/mt219684) in your app's code.

The first step in the IAP submission process is to create the IAP in the dashboard by [defining its product ID](set-your-iap-product-id.md). After that, you can create a submission so that your IAP can be purchased via the Windows Store. You can submit an IAP at the same time you [submit your app](app-submissions.md), or you can work on it independently. And you can make [updates](#updating-an-iap-after-submission) to IAPs after the app is in the Store without having to resubmit the app again.

## Checklist for submitting an IAP


Here is the info that you can provide when creating your IAP submission. The items that you are required to provide are noted below. Some of these are optional, or have default values already provided that you can change as desired.

### Properties page
| Field name                    | Notes                                       | For more info                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Product type**              | Required. If **Durable**, a **Product lifetime** is required. | [Enter IAP properties](enter-iap-properties.md)         |
| **Content type**              | Required                                    | [Enter IAP properties](enter-iap-properties.md)                           | 
| **Keywords**                  | Optional (up to 10 keywords, 30 character limit each) | [Enter IAP properties](enter-iap-properties.md)                 |
| **Tag**                       | Optional (3000 character limit)             | [Enter IAP properties](enter-iap-properties.md)                           |

### Pricing and availability page 
| Field name                    | Notes                                       | For more info                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Base price**                | Required                                    | [Set IAP pricing and availability](set-iap-pricing-and-availability.md)   |
| **Markets and custom pricing** | Default: available in all possible markets | [Set IAP pricing and availability](set-iap-pricing-and-availability.md)   |
| **Sale pricing**              | Optional                                    | [Set IAP pricing and availability](set-iap-pricing-and-availability.md)   |
| **Distribution and visibility** | Default: IAP can be found by customers browsing or searching the Store | [Set IAP pricing and availability](set-iap-pricing-and-availability.md) |
| **Publish date**              | Default: Publish as soon as the IAP passes certification | [Set IAP pricing and availability](set-iap-pricing-and-availability.md)   |

### Descriptions page
One description required. We recommend providing descriptions for every language your app supports.

| Field name                    | Notes                                       | For more info       |
|-------------------------------|---------------------------------------------|---------------------|
| **Title**                     | Required (100 character limit)              | [Create IAP descriptions](create-iap-descriptions.md)                     |
| **Description**               | Optional (200 character limit)              | [Create IAP descriptions](create-iap-descriptions.md)                     |
| **Icon**                      | Optional (.png, 300x300 pixels)             | [Create IAP descriptions](create-iap-descriptions.md)                     |

When you've finished entering this info, click **Submit to the Store**. In most cases, the certification process takes about an hour. After that, your IAP will be published to the Store and ready for customers to purchase!

**Note**  The IAP must also be implemented in your app's code. For more info, see [Enable in-app product purchases](https://msdn.microsoft.com/library/windows/apps/mt219684)


## Updating an IAP after publication


You can make changes to a published IAP at any time. IAP changes are submitted and published independently of your app, so you generally don't need to update the entire app in order to make changes to an IAP such as updating its price or description.

> **Important**  If your app is available to customers on Windows 8.x, you will need to create and publish a new app submission in order to make the IAP updates visible to those customers. Similarly, if you add new IAPs to an app targeting Windows 8.x after the app has been published, you'll need to update your app's code to reference those IAPs, then resubmit the app. Otherwise, the new IAPs won't be visible to customers on Windows 8.x.

To submit updates, go to the IAP's page in your dashboard and click **Update**. This will create a new submission for the IAP, using the info from your previous submission as a starting point. Change the info you'd like, and then click **Submit to the Store**.

If you'd like to remove an IAP you've previously offered, you can do this by creating a new submission and changing the [Distribution and visibility](set-iap-pricing-and-availability.md) option to **No longer available for purchase. Not displayed in your app's listing**. Be sure to update your app's code as needed to also remove references to the IAP.

<!--HONumber=Mar16_HO1-->
