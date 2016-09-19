---
author: mcleanbyron
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: Learn how to include interstitial ads in a Windows 10, Windows 8.1, or Windows Phone 8.1 app using the Microsoft advertising libraries in the Microsoft Store Services SDK.
title: Interstitial ads
translationtype: Human Translation
ms.sourcegitcommit: 2f0835638f330de0ac2d17dae28347686cc7ed97
ms.openlocfilehash: 4082fdd17ba42fd2b6a7659095b019c1ad4875a0

---

# Interstitial ads




This walkthrough shows how to include interstitial ads in a Windows 10, Windows 8.1, or Windows Phone 8.1 app using the Microsoft advertising libraries in the Microsoft Store Services SDK.

For complete sample projects that demonstrate how to add interstitial ads to JavaScript/HTML apps and XAML apps using C# and C++, see the [advertising samples on GitHub](http://aka.ms/githubads).

<span id="whatareinterstitialads10"/>
## What are interstitial ads?

Unlike banner ads, interstitial ads (or *interstitials*) are shown on the entire screen of the app. Two basic forms are frequently used in games.

* With *Paywall* ads, the user must watch an ad at some regular interval. For example between game levels:

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* With *Rewards Based* ads the user is explicitly seeking some benefit, such as a hint or extra time to complete the level, and initializes the video ad through the app’s user interface.

    It is important to note that this SDK does not handle any user interface except at the time of video playback. Refer to the [interstitial best practices](ui-and-user-experience-guidelines.md#interstitialbestpractices10) for guidelines on what to do, and avoid, as you consider how to integrate interstitial ads in your app.

## Building an app with interstitial ads


### Prerequisites

* For UWP apps: install the [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) with Visual Studio 2015.
* For Windows 8.1 or Windows Phone 8.1 apps: install the [Microsoft Advertising SDK for Windows and Windows Phone 8.x](http://aka.ms/store-8-sdk) with Visual Studio 2015 or Visual Studio 2013.

### Code development

* [Steps for a XAML/.NET app](#interstitialadsxaml10)
* [Steps for HTML/JavaScript](#interstitialadshtml10)
* [Steps for C++ (DirectX Interop)](#interstitialadsdirectx10)

<span id="interstitialadsxaml10"/>
### Interstitial ads (XAML/.NET)

> **Note**   This section provides C# examples, but Visual Basic and C++ are also supported.
 
1. Open your project in Visual Studio.
2. In **Reference Manager**, select one of the following references depending on your project type:

    -   For a Universal Windows Platform (UWP) project: Expand **Universal Windows**, click **Extensions**, and then select the check box next to **Microsoft Advertising SDK for XAML** (Version 10.0).

    -   For a Windows 8.1 project: Expand **Windows 8.1**, click **Extensions**, and then select the check box next to **Ad Mediator SDK for Windows 8.1 XAML**. This option will add both the Microsoft advertising and ad mediator libraries to your project, but you can ignore the ad mediator libraries.

    -   For a Windows Phone 8.1 project: Expand **Windows Phone 8.1**, click **Extensions**, and then select the check box next to **Ad Mediator SDK for Windows Phone 8.1 XAML**. This option will add both the Microsoft advertising and ad mediator libraries to your project, but you can ignore the ad mediator libraries.

3.  In the app code, include the following namespace reference.

    ``` syntax
    using Microsoft.Advertising.WinRT.UI;
    ```

4.  Declare your `MyAppId` and `MyAdUnitId` properties.

    ``` syntax
    var MyAppId = "<your app id for windows>";
    var MyAdUnitId = "<your adunit for windows";

    // if your code is in a universal solution and resides in the shared project
    // you can opt to use #if WINDOWS_APP or WINDOWS_PHONE_APP to override with different
    // identifiers for each
#if WINDOWS_PHONE_APP
    MyAppId = "<your app id for phone>";
    MyAdUnitId = "<your adunit for phone>";
#endif
    ```

    > **Note**   You will replace the test values with live values before submitting your app for submission.

5.  Instantiate an [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx), wire up all event handlers, and request an ad.

    ``` syntax
    // instantiate an InterstitialAd
    InterstitialAd MyVideoAd = new InterstitialAd();

    // wire up all 4 events, see below for function templates
    MyVideoAd.AdReady += MyVideoAd_AdReady;
    MyVideoAd.ErrorOccurred += MyVideoAd_ErrorOccurred;
    MyVideoAd.Completed += MyVideoAd_Completed;
    MyVideoAd.Cancelled += MyVideoAd_Cancelled;

    // pre-fetch an ad 30-60 seconds before you need it
    MyVideoAd.RequestAd(AdType.Video, MyAppId, MyAdUnitId);
    ```

6.  At the point in your code where the ad is to be shown, be sure the ad is ready, and show it.

    ``` syntax
    if ((InterstitialAdState.Ready) == (MyVideoAd.State))
    {
      MyVideoAd.Show();
    }
    ```

7.  Define and code up the events.

    ``` syntax
    void MyVideoAd_AdReady(object sender, object e)
    {
      // code
    }

    void MyVideoAd_ErrorOccurred(object sender, AdErrorEventArgs e)
    {
      // code
    }

    void MyVideoAd_Completed(object sender, object e)
    {  
      // code
    }

    void MyVideoAd_Cancelled(object sender, object e)
    {
      // code
    }
    ```

8.  Assign the `MyAppId` property to the test value provided in [Test mode values](test-mode-values.md). This value is used only used for testing; you will replace it with a live value before you publish your app.

    ``` syntax
    var MyAppId = "d25517cb-12d4-4699-8bdc-52040c712cab";
    ```

9.  Assign the `MyAdUnitId` property to the test value provided in [Test mode values](test-mode-values.md). This value is used only used for testing; you will replace it with a live value before you publish your app.

    ``` syntax
    var MyAdUnitId = "11389925";
    ```

10.  Build and test your app to confirm it is showing test ads.

<span id="interstitialadshtml10"/>
### Interstitial ads (HTML/JavaScript)

This sample assumes you have created a Universal App project for JavaScript in Visual Studio 2015 and are targeting a specific CPU.

1. Open your project in Visual Studio.
2.  In **Reference Manager**, select one of the following references depending on your project type:

    -   For a Universal Windows Platform (UWP) project: Expand **Universal Windows**, click **Extensions**, and then select the check box next to **Microsoft Advertising SDK for JavaScript** (Version 10.0).

    -   For a Windows 8.1 project: Expand **Windows 8.1**, click **Extensions**, and then select the check box next to **Microsoft Advertising SDK for Windows 8.1 Native (JS)**.

    -   For a Windows 8.1 project: Expand **Windows Phone 8.1**, click **Extensions**, and then select the check box next to **Microsoft Advertising SDK for Windows Phone 8.1 Native (JS)**.

3.  In the HTML, include the following script reference.

    ``` syntax
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

4.  Declare your `myAppId` and `myAdUnitId` properties.

    ``` syntax
    <script>
      var myAppId = "<your app id>";
      var myAdUnitId = "<your adunit id>";
    </script>
    ```

5.  Instantiate an **InterstitialAd**, wire up all event handlers, and request an ad.

    ``` syntax
    // instantiate an InterstitialAd
    window.interstitialAd = new MicrosoftNSJS.Advertising.InterstitialAd();

    // wire up all 4 events, see below for function templates
    window.interstitialAd.onAdReady = readyHandler;
    window.interstitialAd.onErrorOccurred = errorHandler;
    window.interstitialAd.onCompleted = completeHandler;
    window.interstitialAd.onCancelled = cancelHandler;

    // pre-fetch an ad 30-60 seconds before you need it
    var myAdType = MicrosoftNSJS.Advertising.InterstitialAdType.video;
    window.interstitialAd.requestAd(myAdType, myAppId, myAdUnitId);
    ```

6.  At the point in your code where the ad is to be shown, be sure the ad is ready, and show it.

    ``` syntax
    if ((MicrosoftNSJS.Advertising.InterstitialAdState.ready) == (window.interstitialAd.state)) {
             window.interstitialAd.show();
    }
    ```

7.  Define and code up the events.

    ``` syntax
    function readyHandler(sender) {
      // code
    }

    function errorHandler(sender, args) {
      // code
    }

    function completeHandler(sender) {
      // code
    }

    function cancelHandler(sender) {
      // code
    }
    ```

7.  Assign the `MyAppId` property to the test value provided in [Test mode values](test-mode-values.md). This value is used only used for testing; you will replace it with a live value before you publish your app.

    ``` syntax
    var MyAppId = "d25517cb-12d4-4699-8bdc-52040c712cab";
    ```

8.  Assign the `MyAdUnitId` property to the test value provided in [Test mode values](test-mode-values.md). This value is used only used for testing; you will replace it with a live value before you publish your app.

    ``` syntax
    var MyAdUnitId = "11389925";
    ```

9.  Build and test your app to confirm it is showing test ads.

<span id="interstitialadsdirectx10"/>
### Interstitial ads (C++ and DirectX with XAML interop)

This sample assumes you have created a Universal App project for XAML in Visual Studio 2015 and are targeting a specific CPU architecture.

> **Important**   This code is written in C++ as required for DirectX.

 
1. Open your project in Visual Studio.
1.  In **Reference Manager**, select one of the following references depending on your project type:

    -   For a Universal Windows Platform (UWP) project: Expand **Universal Windows**, click **Extensions**, and then select the check box next to **Microsoft Advertising SDK for XAML** (Version 10.0).

    -   For a Windows 8.1 project: Expand **Windows 8.1**, click **Extensions**, and then select the check box next to **Ad Mediator SDK for Windows 8.1 XAML**. This option will add both the Microsoft advertising and ad mediator libraries to your project, but you can ignore the ad mediator libraries.

    -   For a Windows Phone 8.1 project: Expand **Windows Phone 8.1**, click **Extensions**, and then select the check box next to **Ad Mediator SDK for Windows Phone 8.1 XAML**. This option will add both the Microsoft advertising and ad mediator libraries to your project, but you can ignore the ad mediator libraries.

2.  In the appropriate header file for your app, declare the interstitial ad object and related properties/methods.

    ``` syntax
    Microsoft::Advertising::WinRT::UI::InterstitialAd^ m_ia;
    void OnAdReady(Object^ sender, Object^ args);
    void OnAdCompleted(Object^ sender, Object^ args);
    void OnAdCancelled(Object^ sender, Object^ args);
    void OnAdError (Object^ sender,  Microsoft::Advertising::WinRT::UI::AdErrorEventArgs^ args);
    ```

3.  Declare your `AppId` and `AdUnitId` properties.

    ``` syntax
    #if WINDOWS_PHONE_APP
    static Platform::String^ IA_APPID = L"<your app id for phone>";
    static Platform::String^ IA_ADUNITID = L"<your ad unit for phone>";
    #endif

    #if WINDOWS_APP
    static Platform::String^ IA_APPID = L"<your app id for windows>";
    static Platform::String^ IA_ADUNITID = L"<your ad unit for windows>";
    #endif
    ```

4.  In the .cpp file, add a namespace reference.

    ``` syntax
    using namespace Microsoft::Advertising::WinRT::UI;
    ```

5.  Instantiate an **InterstitialAd**, wire up all event handlers, and request an ad.

    ``` syntax
    // Instantiate an InterstitialAd.
    m_ia = ref new InterstitialAd();

    // Wire up all 4 events, see below for function templates.            
    m_ia->AdReady += ref new Windows::Foundation::EventHandler<Platform::Object ^>
        (this, &Simple3DGameXaml::DirectXPage::OnAdReady);
    m_ia->Completed += ref new Windows::Foundation::EventHandler<Platform::Object ^>
        (this, &Simple3DGameXaml::DirectXPage::OnAdCompleted);
    m_ia->Cancelled += ref new Windows::Foundation::EventHandler<Platform::Object ^>
        (this, &Simple3DGameXaml::DirectXPage::OnAdCancelled);
    m_ia->ErrorOccurred += ref new
        Windows::Foundation::EventHandler<Microsoft::Advertising::WinRT::UI::AdErrorEventArgs ^>
            (this, &Simple3DGameXaml::DirectXPage::OnAdError);

    // Pre-fetch an ad 30-60 seconds before you need it.
    m_ia->RequestAd(AdType::Video, IA_APPID, IA_ADUNITID);
    ```

6.  At the point in your code where the ad is to be shown, be sure the ad is ready, and show it.

    ``` syntax
    if ((InterstitialAdState::Ready == m_ia->State))
    {
        m_ia->Show();
    }
    ```

7.  Define and code up the events.

    ``` syntax
    void DirectXPage::OnAdReady(Object^ sender, Object^ args)
    {
      // code
    }

    void DirectXPage::OnAdCompleted(Object^ sender, Object^ args)
    {
      // code
    }

    void DirectXPage::OnAdCancelled(Object^ sender, Object^ args)
    {
      // code
    }

    void DirectXPage::OnAdError
      (Object^ sender, Microsoft::Advertising::WinRT::UI::AdErrorEventArgs^ args)
    {
      // code
    }
    ```

8.  Assign the `AppId` property to the test value provided in [Test mode values](test-mode-values.md). This value is used only used for testing; you will replace it with a live value before you publish your app.

    ``` syntax
    static Platform::String^ IA_APPID = L"d25517cb-12d4-4699-8bdc-52040c712cab";
    ```

9.  Assign the `AdUnitId` property to the test value provided in [Test mode values](test-mode-values.md). This value is used only used for testing; you will replace it with a live value before you publish your app.

    ``` syntax
    static Platform::String^ IA_ADUNITID = L"11389925";
    ```

10. Build and test your app to confirm it is showing test ads.

### Release your app with live ads using Windows Dev Center

1.  In the Dev Center dashboard, go to the **Monetization** &gt; **Monetize with ads** page for your app, and [create a standalone Microsoft Advertising unit](../publish/monetize-with-ads.md). For the ad unit type, specify **Video interstitial**. Make note of both the ad unit ID and the application ID.

2.  In your code, replace the test ad unit values with the live values you generated in Dev Center.

3.  [Submit your app](../publish/app-submissions.md) to the Store using the Windows Dev Center dashboard.

4.  Review your [advertising performance reports](../publish/advertising-performance-report.md) in the Dev Center dashboard.

<span id="interstitialbestpractices10"/>
## Interstitial best practices


For more information about how to use interstitial ads effectively, see [UI and user experience guidelines](ui-and-user-experience-guidelines.md).

<span id="targetplatform10"/>
## Remove reference errors: target a specific CPU platform (XAML and HTML)


When using the Microsoft advertising libraries, you cannot target **Any CPU** in your project. If your project targets the **Any CPU** platform, you may see a warning in your project after you add a reference to the Microsoft advertising libraries. To remove this warning, update your project to use an architecture-specific build output (for example, **x86**). For more information, see [Known issues](known-issues-for-the-advertising-libraries.md).

## Related topics


* [Interstitial ad sample code in C#](interstitial-ad-sample-code-in-c.md)
* [Interstitial ad sample code in JavaScript](interstitial-ad-sample-code-in-javascript.md)
* [Advertising samples on GitHub](http://aka.ms/githubads)

 

 



<!--HONumber=Sep16_HO2-->


