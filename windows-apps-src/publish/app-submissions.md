---
author: jnHs
Description: Once you've created your app by reserving a name, you can start working on getting it published. The first step is to create a submission.
title: App submissions
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: checklist
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: ce9858da15cac0e34a2bb2c68c25ba63ec79af4c

---

# App submissions


Once you've [created your app by reserving a name](create-your-app-by-reserving-a-name.md), you can start working on getting it published. The first step is to create a **submission**.

You can start your submission when your app is complete and ready to publish, or you can start entering info even before you have written a single line of code. The submission will be saved in your dashboard, so you can work on it whenever you're ready.

After your app is published, you can publish an updated version by creating another submission in your dashboard. Creating a new submission lets you make and publish whatever changes are needed, whether you're uploading new packages or just changing details such as price or category. To create a new submission for an app, click **Update** next to the most recent submission shown on the App overview page.

> **Note**&nbsp;&nbsp;This section of the documentation describes how to create an app submission on the Dev Center dashboard. Alternatively, you can use the [Windows Store submission API](../monetize/create-and-manage-submissions-using-windows-store-services.md) to automate app submissions.

## App submission checklist


Here are the details that you can provide when creating your app submission, with links to more info.

Items that you are required to provide or specify are noted below. Some areas are optional, or have default values provided that you can change as desired.

### Pricing and availability page
| Field name                    | Notes                                       | For more info                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Base price**                | Required                                    | [Base price](set-app-pricing-and-availability.md#base-price)              |
| **Free trial**                | Default: No free trial                      | [Adding trials and in-app purchases](https://msdn.microsoft.com/library/windows/apps/jj193599)  |
| **Markets and custom prices** | Default: All possible markets, no custom pricing | [Define pricing and market selection](define-pricing-and-market-selection.md)              |
| **Sale pricing**              | Optional                                    | [Put apps and add-ons on sale](put-apps-and-add-ons-on-sale.md)                                       |
| **Distribution and visibility** | Default: Make this app available in the Store | [Distribution and visibility](set-app-pricing-and-availability.md#distribution-and-visibility) |
| **Organizational licensing**    | Default: Allow volume acquisition by organizations | [Organizational licensing options](organizational-licensing.md)                        |
| **Publish date**                | Default: Publish as soon as possible      | [Publish date](set-app-pricing-and-availability.md#publish-date)          |

<span/>

### App properties page

| Field name                    | Notes                                       | For more info                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Category and subcategory**  | Required                                    | [Category and subcategory table](category-and-subcategory-table.md)       |
| **System requirements**      | Optional                                    | [System requirements](enter-app-properties.md#system-requirements)      |
| **App declarations**          | Default: Customers can install this app to alternate drives or removable storage; Windows can include this app's data in automatic backups to OneDrive | [App declarations](app-declarations.md) |

<span/>

### Age ratings page

| Field name                    | Notes                                       | For more info                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **Age ratings**               | Required                                    | [Age ratings](age-ratings.md)          |

<span/>

### Packages page

| Field name                    | Notes                                  | For more info                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **Package upload control**    | Required (at least one package)        | [Upload app packages](upload-app-packages.md) |
| **Device family availability** | Default: based on your packages       | [Device family availability](upload-app-packages.md#device-family-availability) |
| **Gradual package rollout**   | Optional (for updates only)            | [Gradual package rollout](gradual-package-rollout.md) |
| **Mandatory update**          | Optional (for updates only)            | [Mandatory update](upload-app-packages.md#mandatory-update)

<span/>

### Store listings

You'll need all the required info for at least one of the languages that your app supports. We recommend providing [Store listings](create-app-store-listings.md) in all of the languages your app supports, and you can also [provide Store listings in additional languages](create-app-store-listings.md#store-listing-languages).

| Field name                    | Notes                                       | For more info                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Description**               | Required                                    | [Write a great app description](write-a-great-app-description.md) |
| **Release notes**             | Optional                                    | [Release notes](create-app-store-listings.md#release-notes)         |
| **Screenshots**               | Required (at least one screenshot)          | [App screenshots and images](app-screenshots-and-images.md)       |
| **App tile icon**             | Optional, but highly recommended for Windows Phone 8.1 and earlier | [App tile icon](create-app-store-listings.md#app-tile-icon) |
| **Promotional artwork**       | Optional                                    | [App screenshots and images](app-screenshots-and-images.md)       |
| **App features**              | Optional                                    | [Features](create-app-store-listings.md#app-features)               |
| **Additional system requirements**      | Optional                                    | [Additional system requirements](create-app-store-listings.md#additional-system-requirements) |
| **Keywords**                  | Optional                                    | [Keywords](create-app-store-listings.md#keywords)                   |
| **Copyright and trademark info** | Optional                                 | [Copyright and trademark info](create-app-store-listings.md#copyright-and-trademark-info) |
| **Additional license terms**  | Optional                                    | [Additional license terms](create-app-store-listings.md#additional-license-terms) |
| **Website**                   | Optional                                    | [Website](create-app-store-listings.md#website)                     |
| **Support contact info**      | Optional                                    | [Support contact info](create-app-store-listings.md)                |
| **Privacy policy**            | Required for some apps. See the [App Developer Agreement](https://msdn.microsoft.com/library/windows/apps/hh694058) and the [Windows Store Policies](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_5_1) | [Privacy policy](create-app-store-listings.md#privacy-policy) |
| **Platform-specific Store listings** | Optional                               | [Create platform-specific Store listings](create-platform-specific-store-listings.md) |

<span/>

### Notes for certification page

| Field name                    | Notes                                       | For more info                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Notes**                     | Optional                                    | [Notes for certification](notes-for-certification.md)             |

<span/>

**Note**&nbsp;&nbsp;For info about publishing line-of-business (LOB) apps directly to enterprises, see [Distribute LOB apps to enterprises](distribute-lob-apps-to-enterprises.md).



<!--HONumber=Aug16_HO5-->


