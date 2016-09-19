---
author: jnHs
Description: When submitting an add-on, the options on the Properties page help determine the behavior of your add-on when offered to customers.
title: Enter add-on properties
ms.assetid: 26D2139F-66FD-479E-940B-7491238ADCAE
translationtype: Human Translation
ms.sourcegitcommit: e3bc74fab0ae75c35581e33323100376ad755e9c
ms.openlocfilehash: 1c030f7d79de37e20620cf56a30b1c1570e9b90b

---

# Enter add-on properties


When submitting an add-on, the options on the **Properties** page help determine the behavior of your add-on when offered to customers.

## Product type

Your product type is selected when you first [create the add-on](set-your-add-on-product-id.md). The product type you selected is displayed here, but you can't change it.

> **Note**  If you haven't published the add-on. you can delete the submission and start again if you need to choose a different product type. 

Depending on the product type you selected, you may see one of the following fields:

### Product lifetime
If you selected **Durable** for your product type, the **Product lifetime** is shown here. The default **Product lifetime** for a durable add-on is **Forever**, which means the add-on never expires. If you prefer, you can set the **Product lifetime** so that the add-on expires after a set duration (with options from 1-365 days). 

### Quantity
If you selected **Store-managed consumable** for your product type, the **Quantity** is shown here. You'll need to enter a number between 1 and 1000000. This quantity will be granted to the customer when they acquire your add-on, and the Store will track the balance as the app reports the customer’s consumption of the add-on.

## Content type

Regardless of your add-on's product type, you'll also need to indicate the type of content you're offering. For most add-ons, the content type should be **Electronic software download**. If another option from the list seems to describe your add-on better (for example, if you are offering a music download or an e-book), select that option instead. 

These are the possible options for an add-on's content type:

-   Electronic software download
-   Electronic books
-   Electronic magazine single issue
-   Electronic newspaper single issue
-   Music download
-   Music streaming
-   Online data storage/services
-   Video download
-   Video streaming
-   Software as a service

## Keywords

You have the option to provide up to ten keywords of up to 30 characters each for each add-on you submit. Your app can then query for add-ons that match these words. This feature lets you build screens in your app that can load add-ons without you having to directly specify the product ID in your app's code. You can then change the add-on's keywords anytime, without having to make code changes in your app or submit the app again.

> **Note**  Keywords are not available for use in packages targeting Windows 8 and Windows 8.1.

## Custom developer data

You can enter up to 3000 characters into the **Custom developer data** field to provide extra context for your in-app product.

> **Note** This field was formerly called **Tag**.

For example, let’s say you have a game, and you’re selling a bag of gold coins as an add-on. Using the **Custom developer data** field, the app can query for this bag of gold. You can adjust the value at any time (in this case, the number of coins in your bag) by updating the info in the add-on's **Custom developer data** field, without having to make code changes in your app or submit the app again.

> **Note**  The **Custom developer data** field is not available for use in packages targeting Windows 8 and Windows 8.1.

 

 

 







<!--HONumber=Aug16_HO5-->


