---
author: jnHs
Description: Your app needs to include various logos, screenshots, and images.
title: App screenshots and images
ms.assetid: D216DD2B-F43D-4D26-82EE-0CD34DB929D8
translationtype: Human Translation
ms.sourcegitcommit: 70020d3c6e0fb0fea321ce1951720803fd25f9c0
ms.openlocfilehash: eb61c9bca6b8ddfba5287482508e7d04b240d29f

---

# App screenshots and images


Your app needs to include various logos, screenshots, and images. Some of these are required, and some are optional. Keep in mind that your images are one of the main ways in which you represent your app. Well-designed images can be a big help in making your app appeal to customers.

During the [app submission process](app-submissions.md), you provide your [screenshots](#screenshots) and [promotional artwork](#promotional-artwork) in the [Store listings](create-app-store-listings.md) step. These images are used to help display your app in the Store.

The Store also uses your app's tile and other images that you include in your app's package. Run the [Windows App Certification Kit](https://msdn.microsoft.com/library/windows/apps/mt186449) to determine if you're missing any required images. For guidance and recommendations about these images, see [Tile and icon assets](../controls-and-patterns/tiles-and-notifications-app-assets.md).

> **Note**  The way images are used in the Store, on the customer's Start screen, and within the app itself may vary, depending on the customer's operating system and other factors.


## Images provided during the submission process

When entering your app's Store listing info, you have the option to provide multiple screenshots (one screenshot is required) and promotional artwork. These images are not taken from your app's package; you'll need to provide them in the **Store listing** step for each of your languages.

The following table lists the various images you can upload and explains how they are used. More details are provided in the sections below.

| Image                                                       | Pixel size                           | Usage                                                                                                                                                                           | When to include                                                                                                                                            |
|-------------------------------------------------------------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Desktop screenshots](#screenshots)                         | 1366 x 768 or larger                 | Displayed in your app's Store listing when viewed on a desktop device.                                                                                                          | Recommended for all apps, especially if your app is intended for use on desktop devices. At least one screenshot (for any device family) is required. |
| [Mobile screenshots](#screenshots)                          | 768 x 1280, 720 x 1280, or 480 x 800 | Displayed in your app's Store listing when viewed on a mobile device.                                                                                                           | Recommended for all apps, especially if your app is intended for use on mobile devices. At least one screenshot (for any device family) is required. |
| [Holographic screenshots](#screenshots)                          | 1268 x 720 or larger                 | Displayed in your app's Store listing when viewed on a holographic device.                                                                                                           | Recommended if your app is intended for use on Microsoft HoloLens. At least one screenshot (for any device family) is required. |
| [App tile icon](#app-tile-icon)                             | 300 x 300                            | Displayed as your app's icon in the Store for Windows Phone 8.1 and earlier (and, if you only have packages targeting Windows Phone 8.1 or earlier, in the Store on Windows 10) | Required for proper display in the Store if your app targets Windows Phone 8.1 or earlier.                                                                 |
| [Promotional image: Windows 10 Spotlight](#promotional-artwork) | 2400 x 1200                          | Used for promotional layouts in the Store on Windows 10.                                                                                                                        | Recommended for all apps, especially those with UWP packages targeting Windows 10 customers.                                                               |
| [Promotional images: Windows Phone 8.1 and earlier](#promotional-artwork) | 1000 x 800, 358 x 358                | Used for promotional layouts in the Store on Windows Phone 8.1 and earlier.                                                                                                     | Recommended for all apps targeting Windows Phone 8.1 or earlier.                                                                                           |
| [Promotional image: Windows 8.1 and earlier](#promotional-artwork)        | 414 x 180                            | Used for promotional layouts in the Store on Windows 8.1.                                                                                                                       | Recommended for all apps targeting Windows 8.1 or earlier.                                                                                                 |
 

## Screenshots

Screenshots are images of your app that are displayed to your customers in your app's Store listing.

You'll see multiple fields on the **Store listing** page where you have the option to provide screenshots for different device families (that will be shown when a customer views your app's Store listing on that type of device).

Only one screenshot is required for your submission, though you can provide several; up to 9 desktop screenshots and up to 8 mobile and holographic screenshots). You are not required to provide separate screenshots for each device family, but we recommend providing screenshots for every one that your app supports, so that customers will see images that resemble how the app will look on their device.

> **Note**  Microsoft Visual Studio provides a [tool to help you capture  screenshots](http://go.microsoft.com/fwlink/p/?LinkId=221135).

Each screenshot must be a .png file in either landscape or portrait orientation, and the file size can't be larger than 2 MB.

The size requirements vary depending on the device family:
- Mobile: either 768 x 1280, 720 x 1280, or 480 x 800 pixels
- Desktop: 1366 x 768 pixels or larger
- Holographic: 1268 x 720 pixels or larger

You can provide a short caption that describes each screenshot in 200 characters or less.

> **Note**  If you create Store listings for [multiple languages](supported-languages.md), you'll have a **Store listing** page for each one. You'll need to upload images for each language separately (even if you are using the same images), and provide captions to use for each language.


## App tile icon

This is not required for all submissions, but is strongly recommended if your app runs on Windows Phone 8.1 or earlier. The app tile icon is used when displaying your app's Store listing to customers on Windows Phone 8.1 and earlier. If you don't provide this image, customers on Windows Phone 8.1 or earlier will see a blank icon with your app's listing. (This also applies to customers on Windows 10, if your app only has packages targeting Windows Phone 8.1 or earlier.)

If your submission **only** includes UWP packages, you don’t need to provide this image. Note that if your submission includes UWP packages and you provide an app tile icon, it may be displayed with your app’s listing on Windows 10 in some Store layouts. To completely prevent the app tile icon for displaying to customers on Windows 10, you can create a [platform-specific listing](create-platform-specific-descriptions.md) for the earlier OS versions and only include the app tile icon there.

The app tile icon must be a .png file measuring 300 x 300 pixels.

## Promotional artwork

The Windows Store editorial team uses different images to showcase apps in the Store. Submitting promotional artwork lets the Windows Store team consider your app in promotional layouts.

> **Important**  Providing promotional images for your app doesn't guarantee that your app will be featured, but not providing them means that it can't be considered for all promotional opportunities. (See [Making your app easy to promote](make-your-app-easier-to-promote.md) for more information.)

You may want to submit promotional artwork in different sizes, depending on which OS version(s) your app targets. For all sizes, images must be in .png format.

Here are some tips to keep in mind when designing your promotional artwork:

-   Select dynamic images that relate to the app and drive recognition and differentiation. Avoid stock photography or generic visuals.
-   Don't include text (aside from your branding).
-   Minimize empty space in the image.
-   Avoid showing your app's UI, and do not use any device-specific imagery.
-   Avoid political and national themes, flags, or religious symbols.
-   Don't include images of insensitive gestures, nudity, gambling, currency, drugs, tobacco, or alcohol.
-   Don't use weapons pointing at the viewer or excessive violence and gore.

### For Windows 10: 2400 x 1200

In the Store on Windows 10, the top of the Apps and Games category pages features a rotating Spotlight image to promote content. To make your app eligible for this Spotlight placement, be sure to submit a 2400 x 1200 image.

When designing your image, keep in mind that if we use it for the Spotlight, we'll apply a gradient over the bottom third so that we can legibly display marketing text over the image. Because of this, make sure you avoid placing text and key visual elements in the bottom third. Additionally, we may crop your image, so place your app's branding and the most important details in the center.

The image below shows the key proportions to keep in mind. The "safe zone" in the center will be prominent even if we crop the image. The "dynamic text area" is where text and a gradient may appear.

![guidelines for spotlight image](images/spotlight1.jpg) The example below shows a well-designed Spotlight image which keeps these guidelines in mind. (The lines on the image are to illustrate how the artwork fits into the designated areas, and would not be included in the final image.)

![well-designed spotlight image](images/spotlight2.jpg)
### For Windows Phone 8.1 and earlier: 1000 x 800, 358 x 358

In the Store on Windows Phone 8.1 and earlier, two image sizes can be used in promotional layouts: 1000 x 800 pixels and 358 x 358 pixels. If your app runs on Windows Phone 8.1 or earlier, we recommend providing images in both of these sizes for promotional consideration.

> **Tip**   Also be sure to provide a 300 x 300 [app tile icon image](#app-tile-icon) to ensure that your app does not appear in the Store with a blank icon.

### For Windows 8.1 and earlier: 414 x 180

In the Store on Windows 8.1 and earlier, promotional layouts may use an image in the 414 x 180 pixel size. If your app runs on Windows 8.1 or earlier, we recommend providing an image in this size for promotional consideration.



<!--HONumber=Aug16_HO5-->


