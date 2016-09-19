---
author: TylerMSFT
title: Handle app prelaunch
description: Learn how to handle app prelaunch by overriding the OnLaunched method and calling CoreApplication.EnablePrelaunch(true).
ms.assetid: A4838AC2-22D7-46BA-9EB2-F3C248E22F52
translationtype: Human Translation
ms.sourcegitcommit: ea9aa37e15dbb6c977b0c0be4f91f77f3879e622
ms.openlocfilehash: cf7cb9f81207f4f25eb8e78283079df27f83d7dc

---

# Handle app prelaunch

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Learn how to handle app prelaunch by overriding the [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) method.

## Introduction

When available system resources allow, the startup performance of Windows Store apps on desktop device family devices is improved by proactively launching the user’s most frequently used apps in the background. A prelaunched app is put into the suspended state shortly after it is launched. Then, when the user invokes the app, the app is resumed by bringing it from the suspended state to the running state--which is faster than launching the app cold. The user's experience is that the app simply launched very quickly.

Prior to Windows 10, apps did not automatically take advantage of prelaunch. In Windows 10, version 1511, all Universal Windows Platform (UWP) apps were candidates for being prelaunched. In Windows 10, version 1607, you must opt-in to prelaunch behavior by calling [CoreApplication.EnablePrelaunch(true)](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.core.coreapplication.enableprelaunch.aspx). A good place to put this call is within `OnLaunched()` near the location that the `if (e.PrelaunchActivated == false)` check is made.

Whether an app is prelaunched depends on system resources. If the system is experiencing resource pressure, apps are not prelaunched.

Some types of apps may need to change their startup behavior to work well with prelaunch. For example, an app that plays music when its starts up, a game which assumes the user is present and displays elaborate visuals when the app starts up, a messaging app that changes the user's online visibility during startup, can identify when the app was prelaunched and can change their startup behavior as described in the sections below.

The default templates for XAML Projects (C#, VB, C++) and WinJS accommodate prelaunch in Visual Studio 2015 Update 3.

## Prelaunch and the app lifecycle

After an app is prelaunched, it will enter the suspended state. (see [Handle app suspend](suspend-an-app.md)).

## Detect and handle prelaunch

Apps receive the [**LaunchActivatedEventArgs.PrelaunchActivated**](https://msdn.microsoft.com/library/windows/apps/dn263740) flag during activation. Use this flag to run code that should only run when the user explicitly launches the app, as shown in the following excerpt from [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335).

```cs
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content - rather just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();

        rootFrame.NavigationFailed += OnNavigationFailed;

        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }

        if (!e.PrelaunchActivated)
        {
            // TODO: This is not a prelaunch activation. Perform operations which
            // assume that the user explicitly launched the app such as updating
            // the online presence of the user on a social network, updating a
            // what's new feed, etc.
        }

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }

    if (rootFrame.Content == null)
    {
        // When the navigation stack isn't restored navigate to the first page,
        // configuring the new page by passing required information as a navigation parameter
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
    }
    // Ensure the current window is active
    Window.Current.Activate();
}
```

**Tip**  If you are targeting a version of Windows 10 prior to version 1607, and you wish to opt-out of prelaunch, check the [**LaunchActivatedEventArgs.PrelaunchActivated**](https://msdn.microsoft.com/library/windows/apps/dn263740) flag. If it is set, return from OnLaunched() before doing any work to create a frame or activate the window.

## Use the VisibilityChanged event

Apps activated by prelaunch are not visible to the user. They become visible when the user switches to them. You may want to delay certain operations until your app's main window becomes visible. For example, if your app displays a list of what's new items from a feed, you could update the list during the [**VisibilityChanged**](https://msdn.microsoft.com/library/windows/apps/hh702458) event rather than use the list that was built when the app was prelaunched because it may become stale by the time the user activates the app. The following code handles the **VisibilityChanged** event for **MainPage**:

```cs
public sealed partial class MainPage : Page
{
    public MainPage()
    {
        this.InitializeComponent();

        Window.Current.VisibilityChanged += WindowVisibilityChangedEventHandler;
    }

    void WindowVisibilityChangedEventHandler(System.Object sender, Windows.UI.Core.VisibilityChangedEventArgs e)
    {
        // Perform operations that should take place when the application becomes visible rather than
        // when it is prelaunched, such as building a what's new feed
    }
}
```

## DirectX games guidance

DirectX games should generally not enable prelaunch because many DirectX games do their initialization before prelaunch can be detected. Starting with Windows 1607, Anniversary edition, your game will not be prelaunched by default.  If you do want your game to take advantage of prelaunch, call [CoreApplication.EnablePrelaunch(true)](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.core.coreapplication.enableprelaunch.aspx).

If your game targets an earlier version of Windows 10, you can handle the prelaunch condition to exit the application:

```cs
void ViewProvider::OnActivated(CoreApplicationView^ appView,IActivatedEventArgs^ args)
{
    if (args->Kind == ActivationKind::Launch)
    {
        auto launchArgs = static_cast<LaunchActivatedEventArgs^>(args);
        if (launchArgs->PrelaunchActivated)
        {
            // Opt-out of Prelaunch
            CoreApplication::Exit();
            return;
        }
    }
}
```

## WinJS app guidance

If your WinJS app targets an earlier version of Windows 10, you can handle the prelaunch condition in your [onactivated](https://msdn.microsoft.com/en-us/library/windows/apps/br212679.aspx) handler:

```js
    app.onactivated = function (args) {
        if (!args.detail.prelaunchActivated) {
            // TODO: This is not a prelaunch activation. Perform operations which
            // assume that the user explicitly launched the app such as updating
            // the online presence of the user on a social network, updating a
            // what's new feed, etc.
        }
    }
```

## General Guidance

-   Apps should not perform long running operations during prelaunch because the app will terminate if it can't be suspended quickly.
-   Apps should not initiate audio playback from [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) when the app is prelaunched because the app won't be visible and it won't be apparent why there is audio playing.
-   Apps should not perform any operations during launch which assume that the app is visible to the user, or assume that the app was explicitly launched by the user. Because an app can now be launched in the background without explicit user action, developers should consider the privacy, user experience and performance implications.
    -   An example privacy consideration is when a social app should change the user state to online. It should wait until the user switches to the app instead of changing the status when the app is prelaunched.
    -   An example user experience consideration is that if you have an app, such as a game, that displays an introductory sequence when it is launched, you might delay the introductory sequence until the user switches to the app.
    -   An example performance implication is that you might wait until the user switches to the app to retrieve the current weather information instead of loading it when the app is prelaunched and then need to load it again when the app becomes visible to ensure that the information is current.
-   If your app clears its Live Tile when launched, defer doing this until the visibility changed event.
-   Telemetry for your app should distinguish between normal tile activations and prelaunch activations to make it easier to narrow down the scenario if problems occur.
-   If you have Microsoft Visual Studio 2015 Update 1, and Windows 10, Version 1511, you can simulate prelaunch for App your app in Visual Studio 2015 by choosing **Debug** &gt; **Other Debug Targets** &gt; **Debug Windows Universal App PreLaunch**.

## Related topics

* [App lifecycle](app-lifecycle.md)
* [CoreApplication.EnablePrelaunch](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.core.coreapplication.enableprelaunch.aspx)



<!--HONumber=Aug16_HO4-->


