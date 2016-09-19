---
author: mcleanbyron
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: Learn how to launch an interstitial ad using C#.
title: Interstitial ad sample code in C#
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: 5969bfacd34bcfab5f1bebd2cfbade4fd16c5a39


---

# Interstitial ad sample code in C\# #  




This topic shows how to launch an interstitial ad using C#. For step-by-step instructions that show how to configure your project to use this code, see [Interstitial ads](interstitial-ads.md). For a complete sample project that demonstrates how to add video interstitial ads to a XAML app using C#, see the [advertising samples on GitHub](http://aka.ms/githubads).


## Code example

This code sample shows a working MainPage.xaml.cs code file that implements an interstitial ad. This code assumes that the MainPage.xaml file has a working button with a **Click** event that fires and is handled by a method named **button_Click**. This code launches the interstitial ad when the **Click** event on the button is fired.

Replace the text in the **AppID** and **AdUnitId** variables with live values before submitting your app to the Store. For more information, see [Set up ad units in your app](set-up-ad-units-in-your-app.md).

``` syntax
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Runtime.InteropServices.WindowsRuntime;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;
using Microsoft.Advertising.WinRT.UI;


namespace BasicCSharpInterstitialUWP
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        InterstitialAd MyVideoAd = new InterstitialAd();

        public MainPage()
        {
            this.InitializeComponent();
        }

        private void button_Click(object sender, RoutedEventArgs e)
        {
            // Define ApplicationId and AdUnitId.
            // Test values are shown here. Replace the test values with live values before submitting the app to the Store.
            var MyAppId = "d25517cb-12d4-4699-8bdc-52040c712cab";
            var MyAdUnitId = "11389925";

            MyVideoAd.AdReady += MyVideoAd_AdReady;
            MyVideoAd.ErrorOccurred += MyVideoAd_ErrorOccurred;
            MyVideoAd.Completed += MyVideoAd_Completed;
            MyVideoAd.Cancelled += MyVideoAd_Cancelled;

            // Pre-fetch an ad 30-60 seconds before you need it.
            MyVideoAd.RequestAd(AdType.Video, MyAppId, MyAdUnitId);
        }

        void MyVideoAd_AdReady(object sender, object e)
        {
            if ((InterstitialAdState.Ready) == (MyVideoAd.State))
            {
                MyVideoAd.Show();
            }
        }

        void MyVideoAd_ErrorOccurred(object sender, AdErrorEventArgs e)
        {
            // Add your code here.
        }

        void MyVideoAd_Completed(object sender, object e)
        {
            // Add your code here.
        }

        void MyVideoAd_Cancelled(object sender, object e)
        {
            // Add your code here.  
        }
    }
}
```

 
## Related topics

* [Advertising samples on GitHub](http://aka.ms/githubads)
 



<!--HONumber=Aug16_HO3-->


