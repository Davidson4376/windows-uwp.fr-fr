---
author: jnHs
Description: You can publish line-of-business (LOB) apps directly to enterprises for volume acquisition via the Windows Store for Business, without making the apps broadly available in the Store.
title: Distribute LOB apps to enterprises
ms.assetid: 2050126E-CE49-4DE3-AC2B-A572AC895158
translationtype: Human Translation
ms.sourcegitcommit: 44485b32a7c5580e1a7a0ca9ca7c642e0b9b29d2
ms.openlocfilehash: 76fc4e5cfa70d06d6f378409ecd9f331f64c0803

---

# Distribute LOB apps to enterprises


You can publish line-of-business (LOB) apps directly to enterprises for volume acquisition via the Windows Store for Business, without making the apps broadly available in the Store.

> **Important**  At this time, only free apps can be distributed exclusively to enterprises via the Windows Store for Business. If you submit a paid app as LOB, it will not be available to the enterprise at this time. 

## Setting up the enterprise association


The first step in publishing LOB apps exclusively to an enterprise is to establish the association between your account and the enterprise’s private store.

> **Important**  This association process must be initiated by the enterprise, and must be sent to the email address in your account’s **Contact info**. For more info, see [Working with line-of-business apps](http://go.microsoft.com/fwlink/p/?LinkId=698846).

When an enterprise chooses to invite you to publish apps for their exclusive use, you’ll get an email that includes a link to confirm the association. You can also confirm these associations by going to the **Enterprise associations** section of your **Account settings**.

To confirm the association, click **Accept**. Your account will then be able to publish apps for that enterprise’s exclusive use.

## Submitting an LOB app


Once you’re ready to publish an app for an enterprise’s exclusive use, the process is similar to the app submission process. The app goes through the same certification process, and must comply with all [Windows Store Policies](https://msdn.microsoft.com/library/windows/apps/dn764944). There are just a few parts of the process that are different.

### Distribution and visibility

After you've set up an enterprise association, every time you submit an app you’ll see a drop-down box in the **Distribution and visibility** section of the submission’s **Pricing and availability** page. By default, this is set to **Retail distribution**. To make the app exclusive to an enterprise, you’ll need to choose **Line-of-business (LOB) distribution**.

Once **Line-of-business (LOB) distribution** is selected, the usual **Distribution and visibility** options will be replaced with a list of the enterprises to which you can publish exclusive apps.

Select the enterprise(s) which should be able to get the app. No one else will be able to access it.

> **Note**  At least one enterprise must be selected in order to publish an app as line-of-business.

### Organizational licensing

By default, the box for **Store-managed (online) volume licensing** is checked when you submit an app. When publishing LOB apps, this box must be checked so that the enterprise can acquire your app in volume. This won’t make the app available to anyone outside of the enterprise(s) that you selected in the **Distribution and visibility** section.

If you’d like to make the app available to the enterprise via disconnected (offline) licensing, you can check the **Disconnected (offline) licensing** box as well.

For more info, see [Organizational licensing options](organizational-licensing.md).

### Age ratings
For LOB apps, the [age ratings](age-ratings.md) step of the submission process works the same as for retail apps, but you also have an additional option that allows you to indicate the Store age rating of your app manually rather than completing the questionnaire or importing an existing IARC rating ID. This manual rating can only be used with LOB distribution, so if you ever change the **Distribution and visibility** setting of the app to **Retail distribution**, you'll need to take the age ratings questionnaire before you can publish the submission.

### Enterprise deployment of LOB apps

After you click **Submit to the Store**, the app will go through the certification process. Once it’s ready, an admin for the enterprise must add it to their private store in the Windows Store for Business portal. The enterprise can then deploy the app to its users.

> **Note** In order to get your LOB app, the organization must be located in a [supported market](https://technet.microsoft.com/itpro/windows/whats-new/windows-store-for-business-overview#supported-markets), and you must not have excluded that market when submitting your app. 

For more info, see [Working with line-of-business apps](http://go.microsoft.com/fwlink/p/?LinkId=698846) and [Distribute apps using your private store](http://go.microsoft.com/fwlink/p/?LinkId=698847).

### Updating LOB apps

To publish updates to an app that you’ve already published as LOB, simply create a new submission. You can upload new packages or make any other changes, then click **Submit to the Store** to make the updated version available. Be sure to keep the enterprise selections in **Distribution and visibility** the same (unless you intentionally want to change them, such as selecting an additional enterprise to acquire the app, or removing one of the enterprises to which you’d previously distributed it).

If you want to stop offering an app that you’ve previously published as line-of-business, and prevent any new acquisitions, you’ll need to create a new submission. First, you’ll need to change your **Distribution and visibility** selection from **Line-of-business (LOB) distribution** to **Retail distribution**. Then, in the **Distribution and visibility** options, choose **Hide this app and prevent acquisition**. After the submission goes through the certification process, the app will no longer be available for new acquisitions (although anyone who already has it will continue to be able to use it).

> **Note** When changing an app to **Retail distribution**, you'll need to complete the [age ratings questionnaire](age-ratings.md) if you haven't done so already (even if the app will not be available for new acquisitions).

### Distributing LOB apps through sideloading

Making apps available through Store for Business ensures that the app has been signed by the Store and complies with the standard Store Policies.

In some cases, companies may not want their LOB apps to be submitted through the Windows Dev Center for various reasons (such as compliance reasons or apps that need additional capabilities). In this case, the enterprises can deploy apps directly to machines via sideloading, without using the Windows Store for Business.

For more info, see [Sideload LOB apps in Windows 10](http://go.microsoft.com/fwlink/p/?LinkId=623433).

 

 







<!--HONumber=Sep16_HO1-->


