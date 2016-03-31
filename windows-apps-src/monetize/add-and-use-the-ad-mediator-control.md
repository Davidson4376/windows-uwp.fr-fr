---
ms.assetid: 3C03FDD8-FA61-4E7B-BDCA-3C29DFEA20E4
description: After you install the Microsoft Universal Ad Client SDK, follow the instructions in this topic to use the ad mediator control in your app.
title: Add and use the ad mediator control
---

# Add and use the ad mediator control


\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


After you [install the Microsoft Universal Ad Client SDK](install-the-microsoft-universal-ad-client-sdk.md), follow the instructions in this topic to use the ad mediator control in your app. For a list of the ad networks and project types currently supported by ad mediation, see [Select and manage your ad networks](select-and-manage-your-ad-networks.md).

## Add the ad mediator control to your project


To add an instance of the ad mediator control to your project:

1.  In Visual Studio, open your project.
2.  If you're adding ad mediation to an app which is already monetizing with any of the [ad networks](select-and-manage-your-ad-networks.md) supported by ad mediation, remove the existing ad implementation and all of its references before proceeding.
3.  In **Solution Explorer**, locate the page in your app where you want to show ads and then double-click the page to open it in the designer.
4.  From the **Toolbox**, drag a new **AdMediatorControl** into the designer (be sure to drag the control into the designer, not into your XAML code). Position the control in the location where you'd like your ads to display. You can add multiple controls if you want to display ads in more than one area of your app.

    The **AdMediatorControl** is located in the following **Toolbox** locations:

    -   In a Universal Windows Platform (UWP) project, use the **AdMediatorControl** under the **AdMediator Universal** section.
    -   In a Windows 8.1 or Windows Phone 8.1 project using C# or Visual Basic with XAML, use the **AdMediatorControl** under the **AdMediator** section.
    -   In a Windows Phone Silverlight project, use the **AdMediatorControl** under the **All Windows Phone Controls** section.

    **Note**  When you drag the **AdMediatorControl** control to the designer for the first time in a UWP, Windows 8.1, or Windows Phone 8.1 project using C# or Visual Basic with XAML, Visual Studio adds the required ad mediator assembly reference to your project, but the control isn't added to the designer yet. To add the control, click OK in the message displayed by Visual Studio, wait several seconds for the designer to refresh, and then drag the control back to the designer again. If you still can't successfully add the control to the designer, make sure your project targets the applicable processor architecture for your app (for example, **x86**) rather than **Any CPU**. The control cannot be added to the designer if the project targets **Any CPU** for the build platform.

5.  Visual Studio adds an ad mediator assembly reference to your project and inserts XAML for the ad mediator control into the current page, including a unique ID and a name for the control. The assembly reference and the XAML varies depending on your target platform. For example, for a Universal Windows Platform (UWP) app the assembly name is **Microsoft.AdMediator.Universal**, and the generated XAML is similar to the following example.

    ```xml
    // Code that gets added to the XAML page header
        xmlns:Universal="using:Microsoft.AdMediator.Universal"

        // Code that gets added for the ad mediator control
        <Universal:AdMediatorControl x:Name="AdMediator_3D4884" 
         Grid.ColumnSpan="2" HorizontalAlignment="Left" Height="250" 
         Id="AdMediator-Id-D1FDFDA7-EABB-474C-940C-ECA7FBCFF143" Margin="121,175,0,0" 
         VerticalAlignment="Top" Width="300"/>
    ```

    The **Name** element helps you to identify the specific control in your app when you configure your ad mediation. You can change this to whatever you’d like, but be sure not to change or duplicate the **Id** element. This **Id** must be unique for each control within your app.

6.  Adjust the size and position of the control as necessary. For more information, see [Adjust size and position](#adjust-size-and-position).

## Configure ad networks

After you’ve added all the controls you’d like, you’re ready to configure the ad networks through Connected Services.

**Important**  If you add an additional AdMediatorControl later, you’ll need to configure it through Connected Services again. Otherwise, the new control will not be able to use ad mediation.

To configure the ad networks:

1.  Right-click the name of the project in Solution Explorer, click **Add**, and then click **Connected Service…** to launch the **Add Connected Service** window (Visual Studio 2015) or **Services Manager** window (Visual Studio 2013).
2.  If you are using Visual Studio 2015, click **Ad Mediator** and then click **Configure** to open the **Ad Mediator** window. If you are using Visual Studio 2013, simply click **Ad Mediator** in the left pane of the **Services Manager**.

    The AdMediator.config file is added to your project. This file is where initial ad network configuration settings are saved locally in your project.

3.  In the **Ad Mediator** (Visual Studio 2015) or **Services Manager** (Visual Studio 2013) window, click **Select ad networks**, select the ad networks you want to use, and click **OK** in the **Select ad networks** window.

    **Tip**  It’s a good idea to add all the networks that you have accounts with, even if you don’t plan to use all of them in your app right away. After the app is published, you'll be able configure how often each network is used in Dev Center (or start to use a network you hadn't before) without having to make code changes and resubmit the app.

    Visual Studio fetches the required assemblies for the selected ad networks and adds those assembly references to your project. After this process is complete, click **OK** in the **Fetching Status** dialog box.

4.  In the **Ad Mediator** (Visual Studio 2015) or **Services Manager** (Visual Studio 2013) window, optionally select each network and click **Configure** to enter the configuration information for each network to use while testing the app. This information is saved to the AdMediator.config file in your project. You'll be able to modify this information when you configure ad network behavior on the Windows Dev Center dashboard. For more information, see [Submitting your app and configuring ad mediation](submit-your-app-and-configure-ad-mediation.md).
    **Note**  If you do not enter configuration information during this step, ad mediation will automatically use test configuration values when you run your app on your development computer (for UWP and Windows 8.1 XAML apps) or on the emulator or device (for Windows Phone apps).

5.  In the **Ad Mediator** (Visual Studio 2015) or **Services Manager** (Visual Studio 2013) window, confirm that each ad network you have selected shows **Fetched**. Click **OK** to submit the changes to your project.

**Note**   If you later upgrade to a newer version of the Microsoft Universal Ad Client SDK, you’ll need to launch **Connected Services** again to ensure any automatically fetched ad network DLLs are correctly updated.

### Declare required capabilities

Each ad network may require certain app capabilities. These are shown by each provider in the **Ad Mediator** (for Visual Studio 2015) or **Services Manager** (for Visual Studio 2013) window. Be sure to declare all of the required capabilities in your app's manifest so that the ads are properly displayed.

The following screenshot shows the required capabilities for several ad networks in a Windows 8.1 or Windows Phone 8.1 XAML app.

![services manager showing all references fetched](images/ad-med-8.jpg)

The following screenshot shows the required capabilities for several ad networks in a Windows Phone 8.1 Silverlight app.

![services manager showing all references fetched](images/ad-med-6.jpg)
### Fetch ad network DLLs manually

In some cases, you may see that certain DLLs were not fetched. In this case, you'll need to add them manually. For links to download individual assemblies, see [Selecting and managing your ad networks](select-and-manage-your-ad-networks.md).

**Note**  When adding DLLs manually, you may get an error message saying "A reference to a higher version or incompatible assembly cannot be added to the project." To resolve this error, right-click the DLL in Explorer and then select **Properties**. In the Security section, click **Unblock**.

![unblock button for resolving error message](images/ad-med-4.png)
## Adjust size and position

You can configure the size and position of the ad mediator control in the designer or in your XAML code. Make sure that this size will be large enough to fit all the ads you'll be displaying from your ad networks. Some ad networks may not serve an ad if they detect that the canvas size isn't large enough for the ad to display in full. If any of your ad units will be larger than the default size, you can adjust the canvas size to accommodate your largest ad.

When you drag the control to the designer, the default control sizes are:

-   UWP and Windows 8.1 XAML: 300 width x 250 height.
-   Windows Phone 8.1 XAML: 400 width x 67 height.
-   Windows Phone 8 and Windows Phone 8.1 Silverlight: 480 width x 80 height.

You can override the default ad sizes using the **Width** and **Height** optional parameters, as shown below.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Width"] = 400;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Height"] = 80;
```

You can also specify how each control is positioned to accommodate a variety of sizes and placements depending on your app's needs. For some ad networks, you can use optional parameters to make adjustments. For example, to align ads from Microsoft Advertising to the bottom left:

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["HorizontalAlignment"] = HorizontalAlignment.Left;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["VerticalAlignment"] = VerticalAlignment.Bottom;
```

### Supported ad sizes for Microsoft Advertising

Microsoft Advertising only supports ads in the following standard sizes recommended by the Interactive Advertising Bureau (IAB) for apps running on the following platforms.

-   Windows 10 and Windows 8.1:
    -   160 x 600
    -   250 x 250 (Note that very few ads are available in this size. We recommend using other sizes to maximize the fill rate and eCPM.)
    -   300 x 250
    -   300 x 600
    -   728 x 90
-   Windows 10 Mobile, Windows Phone 8.1 and Windows Phone 8:
    -   300 x 50
    -   320 x 50
    -   480 x 80
    -   640 x 100

You might want to specify an ad mediator control size that doesn't match one of the ad sizes supported by Microsoft Advertising (for example, you might want to do this if a different size is a better fit for your app's UI, or if you are also targeting other ad networks that support other ad sizes). To do this, specify the exact control size you want in the designer or in your XAML code, and then assign the **Width** and **Height** optional parameters for Microsoft Advertising to the closest supported size that will fit within the bounds of the control. The control will display to the exact size you specify in the designer, but Microsoft Advertising will serve ads that match the size you specify using the **Width** and **Height** optional parameters.

For example, if you have a UWP app and you want your ad mediator control to display at 300 x 300, set the control to 300 x 300 in the designer or in your XAML code. Then, assign the **Width** optional parameter to 300 and the **Height** optional parameter to 250 for Microsoft Advertising, as shown in the following code.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Width"] = 300;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Height"] = 250;
```

To verify that your size settings are compatible with Microsoft Advertising, [test your ad mediation implementation](test-your-ad-mediation-implementation.md) and make sure that test ads from Microsoft Advertising are displayed.

## Pause, resume, and disable ad mediation

If you’d like to pause ad mediation for any given amount of time during the app runtime, use the AdMediatorControl.Pause() method. Note that when you do this, the most recent ad will continue showing until you resume mediation by calling the AdMediatorControl.Resume() method.

To disable the ad mediator completely, use the AdMediatorControl.Disable() method. This will remove any ads that are showing, and it will minimize the memory footprint of the mediator. You can call AdMediatorControl.Resume() to resume mediation, but note that startup time will be slower than usual after ad mediation has been disabled.

## Set timeouts

You can specify the number of seconds (from 2-60) for which ad mediation should wait after requesting an ad from that ad network before abandoning that request and making a request to another network instead. By default, the timeout for all ad networks is 15 seconds.

The code below shows how to specify a timeout duration for Microsoft Advertising. You can modify the durations and networks as needed.

```CSharp
myAdMediatorControl.AdSdkTimeouts[AdSdkNames.MicrosoftAdvertising] = TimeSpan.FromSeconds(10); 
```

**Note**  You can alternatively set the timeout value in the **Monetize with ads** page on the Dev Center dashboard. If you set the timeout in code and in the dashboard, that value you set in code overrides the dashboard value.

## Event handling

Adding code to log events and capture ad mediation errors can help with troubleshooting. The code sample below adds event handlers for specific events from a control.

```CSharp
// add this during initialization of your app

    AdMediator_Bottom.AdSdkError += AdMediator_Bottom_AdError;
    AdMediator_Bottom.AdMediatorFilled += AdMediator_Bottom_AdFilled;
    AdMediator_Bottom.AdMediatorError += AdMediator_Bottom_AdMediatorError;
    AdMediator_Bottom.AdSdkEvent += AdMediator_Bottom_AdSdkEvent;

// and then add these functions

void AdMediator_Bottom_AdSdkEvent(object sender, Microsoft.AdMediator.Core.Events.AdSdkEventArgs e)
{
    Debug.WriteLine("AdSdk event {0} by {1}", e.EventName, e.Name);}

void AdMediator_Bottom_AdMediatorError(object sender, Microsoft.AdMediator.Core.Events.AdMediatorFailedEventArgs e)
{
    Debug.WriteLine("AdMediatorError:" + e.Error + " " + e.ErrorCode );
    // if (e.ErrorCode == AdMediatorErrorCode.NoAdAvailable)
    // AdMediator will not show an ad for this mediation cycle
}

void AdMediator_Bottom_AdFilled(object sender, Microsoft.AdMediator.Core.Events.AdSdkEventArgs e)
{
    Debug.WriteLine("AdFilled:" + e.Name);
}

void AdMediator_Bottom_AdError(object sender, Microsoft.AdMediator.Core.Events.AdFailedEventArgs e)
{
    Debug.WriteLine("AdSdkError by {0} ErrorCode: {1} ErrorDescription: {2} Error: {3}", e.Name, e.ErrorCode, e.ErrorDescription, e.Error);
}
```

## Handle unhandled exceptions from ad networks

**Note**  As part of our testing, we've identified a number of unhandled exceptions from specific ad networks that must be handled within the app to avoid app crashes related to these exceptions. We highly recommend that you copy and paste the code sample below to your App.xaml.cs file.

Code to use for a UWP, Windows 8.1 or a Windows Phone app using C# and XAML

```CSharp
// In App.xaml.cs file, register with the UnhandledException event handler.
UnhandledException += App_UnhandledException;

void App_UnhandledException(object sender, UnhandledExceptionEventArgs e)
   {
      if (e != null)
      {
         Exception exception = e.Exception;
         if (exception is NullReferenceException &amp;&amp; exception.ToString().ToUpper().Contains("SOMA"))
         {
            Debug.WriteLine("Handled Smaato null reference exception {0}", exception);
            e.Handled = true;
            return;
         }
      }
// APP SPECIFIC HANDLING HERE

   if (Debugger.IsAttached)
      {
         // An unhandled exception has occurred; break into the debugger
         Debugger.Break();
      }
   }
```

Code to use for a Windows Phone Silverlight app

```CSharp
// In App.xaml.cs file, register with the UnhandledException event handler.
UnhandledException += Application_UnhandledException;

// Code to execute on unhandled exceptions
private void Application_UnhandledException(object sender, ApplicationUnhandledExceptionEventArgs e)
{
    if (e != null)
   {
       Exception exception = e.ExceptionObject;
       if ((exception is XmlException || exception is NullReferenceException) &amp;&amp; exception.ToString().ToUpper().Contains("INNERACTIVE"))
       {
           Debug.WriteLine("Handled Inneractive exception {0}", exception);
           e.Handled = true;
           return;
       }
       else if (exception is NullReferenceException &amp;&amp; exception.ToString().ToUpper().Contains("SOMA"))
       {
           Debug.WriteLine("Handled Smaato null reference exception {0}", exception);
           e.Handled = true;
           return;
       }
       else if ((exception is System.IO.IOException || exception is NullReferenceException) &amp;&amp; exception.ToString().ToUpper().Contains("GOOGLE"))
      {
          Debug.WriteLine("Handled Google exception {0}", exception);
          e.Handled = true;
          return;
       }
       else if ((exception is NullReferenceException || exception is XamlParseException) &amp;&amp; exception.ToString().ToUpper().Contains("MICROSOFT.ADVERTISING"))
       {
           Debug.WriteLine("Handled Microsoft.Advertising exception {0}", exception);
           e.Handled = true;
           return;
       }
              
   }
// APP SPECIFIC HANDLING HERE

if (Debugger.IsAttached)
   {
       // An unhandled exception has occurred; break into the debugger
       Debugger.Break();
   }
   //e.Handled = true;
}
```

## Related topics

* [Select and manage your ad networks](select-and-manage-your-ad-networks.md)
* [Test your ad mediation implementation](test-your-ad-mediation-implementation.md)
* [Submit your app and configure ad mediation](submit-your-app-and-configure-ad-mediation.md)
* [Troubleshoot ad mediation](troubleshoot-ad-mediation.md)
 

 



<!--HONumber=Mar16_HO1-->
