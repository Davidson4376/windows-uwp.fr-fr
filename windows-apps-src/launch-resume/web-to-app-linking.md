---
author: TylerMSFT
title: Support web-to-app linking with app URI handlers
description: Drive user engagement with your app by using app URI handlers.
keywords: Deep Linking Windows
translationtype: Human Translation
ms.sourcegitcommit: 3e57ca2cf0e7c788f5a8be12ebaf3e6b05a4fe51
ms.openlocfilehash: e5c815ef0c776954e5b0d7f1cb9bd5e32e10356c

---

# <a name="support-web-to-app-linking-with-app-uri-handlers"></a>Support web-to-app linking with app URI handlers

Learn how to drive user engagement with your app by supporting web-to-app linking. Web-to-app linking allows you to associate an app with a website. When users open an http or https link to your website, instead of opening the browser, your app is launched. If your app is not installed, a link is provided to open your website in the browser. Users can trust this experience because only verified content owners can register for a link.

In order to enable web to app linking you will need:
- Identify the URIs your app will handle in the manifest file
- A JSON file with the app Package Family Name at the same host root as the app manifest declaration.
- Handle the activation in the app.

## <a name="register-to-handle-http-and-https-links-in-the-app-manifest"></a>Register to handle http and https links in the app manifest

Your app needs to identify the URIs for the websites it will handle. To do so, add the **Windows.appUriHandler** extension registration to your app’s manifest file **Package.appxmanifest**.

For example, if your website’s address is “msn.com” you would make the following entry in your app’s manifest:

```xml
<Applications>
    ...
  <Extensions>
     <uap3:Extension Category="windows.appUriHandler">
      <uap3:AppUriHandler>
        <uap3:Host Name="msn.com" />
      </uap3:AppUriHandler>
    </uap3:Extension>
  </Extensions>
    ...
</Applications>
```

The declaration above registers your app to handle links from the specified host. If your website has multiple addresses (for example: m.example.com, www.example.com, and example.com) then add a separate `<uap3:Host Name=... />` entry inside of the `<uap3:AppUriHandler>` for each address.

## <a name="associate-your-app-and-website-with-a-json-file"></a>Associate your app and website with a JSON file

To ensure that only your app can open content on your website, include your app's package family name in a JSON file located in the web server root, or at the well-known directory on the domain. This signifies that your website gives consent for the listed apps to open content on your site. You can find the package family name in the Packages section in the app manifest designer.

>[!Important]
> The JSON file should not have a .json file suffix.

Create a JSON file (without the .json file extension) named **windows-app-web-link** and provide your app’s package family name. For example:

``` JSON
[{
  "packageFamilyName": "YourAppsPFN",
  "paths": [ "*" ],
  "excludePaths" : [ "/news/*", "/blog/*" ]
 }]
```

Windows will make an https connection to your website and will look for the corresponding JSON file on your web server.

### <a name="wildcards"></a>Wildcards

The JSON file example above demonstrates the use of wildcards. Wildcards allow you to support a wide variety of links with fewer lines of code. Web-to-app linking supports two types of wildcards in the JSON file:

| **Wildcard** | **Description**               |
|--------------|-------------------------------|
| *****       | Represents any substring      |
| **?**        | Represents a single character |

For instance, given `"excludePaths" : [ "/news/*", "/blog/*" ]` in the example above, your app will support all paths that start with your website’s address (e.g. msn.com), **except** those under `/news/` and `/blog/`. **msn.com/weather.html** will be supported, but not ****msn.com/news/topnews.html****.


### <a name="multiple-apps"></a>Multiple apps

If you have two apps that you would like to link to your website, list both of the application package family names in your **windows-app-web-link** JSON file. Both apps can be supported. The user will be presented with a choice of which is the default link if both are installed. If they want to change the default link later, they can change it in **Settings > Apps for Websites**. Developers can also change the JSON file at any time and see the change as early as the same day but no later than eight days after the update.

``` JSON
[{
  "packageFamilyName": "YourAppsPFN",
  "paths": [ "*" ],
  "excludePaths" : [ "/news/*", "/blog/*" ]
 },
 {
  "packageFamilyName": "Your2ndAppsPFN",
  "paths": [ "/example/*", "/links/*" ]
 }]
```

To provide the best experience for your users, use exclude paths to make sure that online-only content is excluded from the supported paths in your JSON file.

Exclude paths are checked first and if there is a match the corresponding page will be opened with the browser instead of the designated app. In the example above, ‘/news/\*’ includes any pages under that path while ‘/news\*’ (no forward slash trails 'news') includes any paths under ‘news\*’ such as ‘newslocal/’, ‘newsinternational/’, and so on.

## <a name="handle-links-on-activation-to-link-to-content"></a>Handle links on Activation to link to content

Navigate to **App.xaml.cs** in your app’s Visual Studio solution and in **OnActivated()** add handling for linked content. In the following example, the page that is opened in the app depends on the URI path:

``` CS
protected override void OnActivated(IActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame == null)
    {
        ...
    }

    // Check ActivationKind, Parse URI, and Navigate user to content
    Type deepLinkPageType = typeof(MainPage);
    if (e.Kind == ActivationKind.Protocol)
    {
        var protocolArgs = (ProtocolActivatedEventArgs)e;        
        switch (protocolArgs.Uri.AbsolutePath)
        {
            case "/":
                break;
            case "/index.html":
                break;
            case "/sports.html":
                deepLinkPageType = typeof(SportsPage);
                break;
            case "/technology.html":
                deepLinkPageType = typeof(TechnologyPage);
                break;
            case "/business.html":
                deepLinkPageType = typeof(BusinessPage);
                break;
            case "/science.html":
                deepLinkPageType = typeof(SciencePage);
                break;
        }
    }

    if (rootFrame.Content == null)
    {
        // Default navigation
        rootFrame.Navigate(deepLinkPageType, e);
    }

    // Ensure the current window is active
    Window.Current.Activate();
}
```

**Important** Make sure to replace the final `if (rootFrame.Content == null)` logic with `rootFrame.Navigate(deepLinkPageType, e);` as shown in the example above.

## <a name="test-it-out-local-validation-tool"></a>Test it out: Local validation tool

You can test the configuration of your app and website by running the App host registration verifier tool which is available in:

%windir%\\system32\\**AppHostRegistrationVerifier.exe**

Test the configuration of your app and website by running this tool with the following parameters:

**AppHostRegistrationVerifier.exe** *hostname packagefamilyname filepath*

-   Hostname: Your website (e.g. microsoft.com)
-   Package Family Name (PFN): Your app’s PFN
-   File path: The JSON file for local validation (e.g. C:\\SomeFolder\\windows-app-web-link)

## <a name="test-it-web-validation"></a>Test it: Web validation

Close your application to verify that the app is activated when you click a link. Then, copy the address of one of the supported paths in your website. For example, if your website’s address is “msn.com”, and one of the support paths is “path1”, you would use `http://msn.com/path1`

Verify that your app is closed. Press **Windows Key + R** to open the **Run** dialog box and paste the link in the window. Your app should launch instead of the web browser.

Additionally, you can test your app by launching it from another app using the [LaunchUriAsync](https://msdn.microsoft.com/en-us/library/windows/apps/hh701480.aspx) API. You can use this API to test on phones as well.

If you would like to follow the protocol activation logic, set a breakpoint in the **OnActivated** event handler.

**Note:** If you click a link in the Microsoft Edge browser, it will not launch your app but will take you to your website.

## <a name="appurihandlers-tips"></a>AppUriHandlers tips:

- Make sure to only specify links that your app can handle.

- List all of the hosts that you will support.  Note that www.example.com and example.com are different hosts.

- Users can choose which app they prefer to handle websites in Settings.

- Your JSON file must be uploaded to an https server.

- If you need to change the paths that you wish to support, you can republish your JSON file without republishing your app. Users will see the changes in 1-8 days.

- All sideloaded apps with AppUriHandlers will have validated links for the host on install. You do not need to have a JSON file uploaded to test the feature.

- This feature works whenever your app is a UWP app launched with  [LaunchUriAsync](https://msdn.microsoft.com/en-us/library/windows/apps/hh701480.aspx) or a Windows desktop app launched with  [ShellExecuteEx](https://msdn.microsoft.com/en-us/library/windows/desktop/bb762154(v=vs.85).aspx). If the URL corresponds to a registered App URI handler, the app will be launched instead of the browser.

## <a name="see-also"></a>See also

[windows.protocol registration](https://msdn.microsoft.com/en-us/library/windows/apps/br211458.aspx)

[Handle URI Activation](https://msdn.microsoft.com/en-us/windows/uwp/launch-resume/handle-uri-activation)

[Association Launching sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AssociationLaunching) illustrates how to use the LaunchUriAsync() API.



<!--HONumber=Dec16_HO1-->


