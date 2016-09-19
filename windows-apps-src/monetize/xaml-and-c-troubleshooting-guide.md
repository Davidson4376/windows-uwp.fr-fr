---
author: mcleanbyron
ms.assetid: 141900dd-f1d3-4432-ac8b-b98eaa0b0da2
description: Learn about solutions to common development issues with the Microsoft advertising libraries in XAML apps.
title: XAML and C# troubleshooting guide
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: 0688ca6e0c88628803ad4e9a55b9285bd39351b4

---

# XAML and C# troubleshooting guide



This topic contains solutions to common development issues with the Microsoft advertising libraries in XAML apps.

-   [XAML](#xaml)

    -   [AdControl not appearing](#xaml-notappearing)

    -   [Black box blinks and disappears](#xaml-blackboxblinksdisappears)

    -   [Ads not refreshing](#xaml-adsnotrefreshing)

-   [C#](#csharp)

    -   [AdControl not appearing](#csharp-adcontrolnotappearing)

    -   [Black box blinks and disappears](#csharp-blackboxblinksdisappears)

    -   [Ads bot refreshing](#csharp-adsnotrefreshing)

<span id="xaml"/>
## XAML

<span id="xaml-notappearing"/>
### AdControl not appearing

1.  Ensure that the **Internet (Client)** capability is selected in Package.appxmanifest.

2.  Check the application ID and ad unit ID. These IDs must match the application ID and ad unit ID that you obtained in Windows Dev Center. For more information, see [Set up ad units in your app](set-up-ad-units-in-your-app.md).

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}" ApplicationId="{ApplicationID}"
                  Width="728" Height="90" />
    ```

3.  Check the **Height** and **Width** properties. These must be set to one of the [Supported ad sizes for banner ads](supported-ad-sizes-for-banner-ads.md).

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90" />
    ```

4.  Check the element position. The [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) must be inside the viewable area.

5.  Check the **Visibility** property. The optional **Visibility** property must not be set to collapsed or hidden. This property can be set inline (as shown below) or in an external style sheet.

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Visibility="Visible"
                  Width="728" Height="90" />
    ```

6.  Check the **IsEnabled** property. The optional `IsEnabled` property must be set to `True`.

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  IsEnabled="True"
                  Width="728" Height="90" />
    ```

7.  Check the parent of the **AdControl**. If the **AdControl** element resides in a parent element, the parent must be active and visible.

    ``` syntax
    <StackPanel>
        <UI:AdControl AdUnitId="{AdUnitID}"
                      ApplicationId="{ApplicationID}"
                      Width="728" Height="90" />
    </StackPanel>
    ```

8.  Ensure the **AdControl** is not hidden from the viewport. The **AdControl** must be visible for ads to display properly.

9.  Live values for **ApplicationId** and **AdUnitId** should not be tested in the emulator. To ensure the **AdControl** is functioning as expected, use the test IDs for both **ApplicationId** and **AdUnitId** found in [Test mode values](test-mode-values.md).

<span id="xaml-blackboxblinksdisappears"/>
### Black box blinks and disappears

1.  Double-check all steps in the previous [AdControl not appearing](#xaml-notappearing) section.

2.  Handle the **ErrorOccurred** event, and use the message that is passed to the event handler to determine whether an error occurred and what type of error was thrown. See [Error handling in XAML/C# walkthrough](error-handling-in-xamlc-walkthrough.md) for more information.

    This example demonstrates an **ErrorOccurred** event handler. The first snippet is the XAML UI markup.

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  ErrorOccurred="adControl_ErrorOccurred" />

    <TextBlock x:Name="TextBlock1" TextWrapping="Wrap" Width="500" Height="250" />
    ```

    This example demonstrates of the the corresponding code.

    ``` syntax
    private void adControl_ErrorOccurred(object sender,               
        Microsoft.Advertising.WinRT.UI.AdErrorEventArgs e)
    {
        TextBlock1.Text = e.Error.Message;
    }
    ```

    The most common error that causes a black box is “No ad available.” This error means there is no ad available to return from the request.

3.  The [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) is behaving normally.

    By default, the **AdControl** will collapse when it cannot display an ad. If other elements are children of the same parent they may move to fill the gap of the collapsed **AdControl** and expand when the next request is made.

<span id="xaml-adsnotrefreshing"/>
### Ads not refreshing

1.  Check the [IsAutoRefreshEnabled](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled.aspx) property. By default, this optional property is set to **True**. When set to **False**, the [Refresh](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.refresh.aspx) method must be used to retrieve another ad.

    ``` syntax
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  IsAutoRefreshEnabled="True" />
    ```

2.  Check calls to the **Refresh** method. When using automatic refresh, **Refresh** cannot be used to retrieve another ad. When using manual refresh, **Refresh** should be called only after a minimum of 30 to 60 seconds depending on the device’s current data connection.

    The following code snippets show an example of how to use the **Refresh** method. The first snippet is the XAML UI markup.

    ``` syntax
    <UI:AdControl x:Name="adControl1"
                  AdUnitId="{AdUnit_ID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  IsAutoRefreshEnabled="False" />
    ```

    This code snippet shows an example of the C# code behind the UI markup.

    ``` syntax
    public Ads()
    {
        var timer = new DispatcherTimer() { Interval = TimeSpan.FromSeconds(60) };
        timer.Tick += (s, e) => adControl1.Refresh();
        timer.Start();
    }
    ```

3.  The **AdControl** is behaving normally. Sometimes the same ad will appear more than once in a row giving the appearance that ads are not refreshing.

<span id="csharp"/>
## C\# #

<span id="csharp-adcontrolnotappearing"/>
### AdControl not appearing

1.  Ensure that the **Internet (Client)** capability is selected in Package.appxmanifest.

2.  Ensure the **AdControl** is instantiated. If the **AdControl** is not instantiated it will not be available.

    ``` syntax
    using Microsoft.Advertising.WinRT.UI;

    namespace App1
    {
        public sealed partial class MainPage : Page
        {
            AdControl adControl;

            public MainPage()
            {
                this.InitializeComponent();

                adControl = new AdControl()
                {
                    ApplicationId = "{ApplicationID}",
                    AdUnitId = "{AdUnitID}",
                    Height = 90,
                    Width = 728
                };
            }
        }
    }
    ```

3.  Check the application ID and ad unit ID. These IDs must match the application ID and ad unit ID that you obtained in Windows Dev Center. For more information, see [Set up ad units in your app](set-up-ad-units-in-your-app.md).

    ``` syntax
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    ```

4.  Check the **Height** and **Width** parameters. These must be set to one of the [supported ad sizes for banner ads](supported-ad-sizes-for-banner-ads.md).

    ``` syntax
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;adControl.Width = 728;
    ```

5.  Ensure the **AdControl** is added to a parent element. To display, the **AdControl** must be added as a child to a parent control (for example, a **StackPanel** or **Grid**).

    ``` syntax
    ContentPanel.Children.Add(adControl);
    ```

6.  Check the **Margin** parameter. The **AdControl** must be inside the viewable area.

7.  Check the **Visibility** property. The optional **Visibility** property must be set to **Visible**.

    ``` syntax
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    adControl.Visibility = System.Windows.Visibility.Visible;
    ```

8.  Check the **IsEnabled** property. The optional **IsEnabled** property must be set to **True**.

    ``` syntax
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    adControl.IsEnabled = True;
    ```

9.  Check the parent of the **AdControl**. The parent must be active and visible.

10. Live values for **ApplicationId** and **AdUnitId** should not be tested in the emulator. To ensure the **AdControl** is functioning as expected, use the test IDs for both **ApplicationId** and **AdUnitId** found in [Test mode values](test-mode-values.md).

<span id="csharp-blackboxblinksdisappears"/>
### Black box blinks and disappears

1.  Double-check all steps in the [AdControl not appearing](#csharp-adcontrolnotappearing) section above.

2.  Handle the **ErrorOccurred** event, and use the message that is passed to the event handler to determine whether an error occurred and what type of error was thrown. See [Error handling in XAML/C# walkthrough](error-handling-in-xamlc-walkthrough.md) for more information.

    The following examples show the basic code needed to implement an error call. This XAML code defines a **TextBlock** that is used to display the error message.

    ``` syntax
    <TextBlock x:Name="TextBlock1" TextWrapping="Wrap" Width="500" Height="250" />
    ```

    This C# code retrieves the error message and displays it in the **TextBlock**.

    ``` syntax
    using Microsoft.Advertising.WinRT.UI;

    namespace App1
    {
        public partial class MainPage : Page
        {
            AdControl adControl;

            public MainPage()
            {
                this.InitializeComponent();

                adControl = new AdControl();
                adControl.ApplicationId = "{ApplicationID}";
                adControl.AdUnitId = "{AdUnitID}";
                adControl.Height = 90;
                adControl.Width = 728;
                adControl.ErrorOccurred += (s,e) =>
                {
                    TextBlock1.Text = e.Error.Message;
                };
            }
        }
    }
    ```

    The most common error that causes a black box is “No ad available.” This error means there is no ad available to return from the request.

3.  **AdControl** is behaving normally. Sometimes the same ad will appear more than once in a row giving the appearance that ads are not refreshing.

<span id="csharp-adsnotrefreshing"/>
### Ads not refreshing

1.  Check the **IsAutoRefreshEnabled** property. By default, this optional property is set to **True**. When set to **False**, the **Refresh** method must be used to retrieve another ad.

    The following example demonstrates how to use the **IsAutoRefreshEnabled** property.

    ``` syntax
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    adControl.IsAutoRefreshEnabled = true;
    ```

2.  Check calls to the **Refresh** method. When using automatic refresh, **Refresh** cannot be used to retrieve another ad. When using manual refresh, **Refresh** should be called only after a minimum of 30 to 60 seconds depending on the device’s current data connection.

    The following example demonstrates how to call the **Refresh** method.

    ``` syntax
    public MainPage()
    {
        InitializeComponent();

        adControl = new AdControl();
        adControl.ApplicationId = "{ApplicationID}";
        adControl.AdUnitId = "{AdUnitID}";
        adControl.Height = 90;
        adControl.Width = 728;
        adControl.IsAutoRefreshEnabled = false;

        ContentPanel.Children.Add(adControl);

        var timer = new DispatcherTimer() { Interval = TimeSpan.FromSeconds(60) };
        timer.Tick += (s, e) => adControl.Refresh();
        timer.Start();
    }
    ```

3.  The **AdControl** is behaving normally. Sometimes the same ad will appear more than once in a row giving the appearance that ads are not refreshing.

 

 



<!--HONumber=Aug16_HO3-->


