---
author: TylerMSFT
title: Launch the Windows Settings app
description: Learn how to launch the Windows Settings app from your app. This topic describes the ms-settings URI scheme. Use this URI scheme to launch the Windows Settings app to specific settings pages.
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
translationtype: Human Translation
ms.sourcegitcommit: f90ba930db60f338ee0ebcc80934281363de01ee
ms.openlocfilehash: 249e485f74364475ff96a8256ee88bdb79749259

---

# Launch the Windows Settings app


\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Important APIs**

-   [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-   [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
-   [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

Learn how to launch the Windows Settings app from your app. This topic describes the **ms-settings:** URI scheme. Use this URI scheme to launch the Windows Settings app to specific settings pages.

Launching to the Settings app is an important part of writing a privacy-aware app. If your app can't access a sensitive resource, we recommend providing the user a convenient link to the privacy settings for that resource. For more info, see [Guidelines for privacy-aware apps](https://msdn.microsoft.com/library/windows/apps/hh768223).

## How to launch the Settings app

To launch the **Settings** app, use the `ms-settings:` URI scheme as shown in the following examples.

In this example, a Hyperlink XAML control is used to launch the privacy settings page for the microphone using the `ms-settings:privacy-microphone` URI.

```xml
<!--Set Visibility to Visible when access to the microphone is denied -->  
<TextBlock x:Name="LocationDisabledMessage" FontStyle="Italic"
                 Visibility="Collapsed" Margin="0,15,0,0" TextWrapping="Wrap" >
          <Run Text="This app is not able to access the microphone. Go to " />
              <Hyperlink NavigateUri="ms-settings:privacy-microphone">
                  <Run Text="Settings" />
              </Hyperlink>
          <Run Text=" to check the microphone privacy settings."/>
</TextBlock>
```

Alternatively, your app can call the [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) method to launch the **Settings** app from code. This example shows how to launch to the privacy settings page for the camera using the `ms-settings:privacy-webcam` URI.

```cs
using Windows.System;
...
bool result = await Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```

The code above launches the privacy settings page for the camera:

![camera privacy settings.](images/privacyawarenesssettingsapp.png)



For more info about launching URIs, see [Launch the default app for a URI](launch-default-app.md).

## ms-settings: URI scheme reference


Use the following URIs to open various pages of the Settings app. Note that the Supported SKUs column indicates whether the settings page exists in Windows 10 for desktop editions (Home, Pro, Enterprise, and Education), Windows 10 Mobile, or both.

| Category           | Settings page                          | Supported SKUs | URI                                       |
|--------------------|----------------------------------------|----------------|-------------------------------------------|
| Home Page          | Landing page for Settings              | Both           | ms-settings:                              |
| System             | Display                                | Both           | ms-settings:screenrotation                |
|                    | Notifications & actions                | Both           | ms-settings:notifications                 |
|                    | Phone                                  | Mobile only    | ms-settings:phone                         |
|                    | Messaging                              | Mobile only    | ms-settings:messaging                     |
|                    | Battery Saver                          | Mobile and desktop on devices with a battery such as a tablet | ms-settings:batterysaver                  |
|                    | Battery Saver / Battery saver settings | Mobile and desktop on devices with a battery such as a tablet | ms-settings:batterysaver-settings         |
|                    | Battery Saver / Battery use            | Mobile and desktop on devices with a battery such as a tablet | ms-settings:batterysaver-usagedetails     |
|                    | Power & sleep                          | Desktop only   | ms-settings:powersleep                    |
|                    | Desktop: About                         | Both           | ms-settings:deviceencryption              |
|                    |                                        |                |                                           |
|                    | Mobile: Device encryption              |                |                                           |
|                    | Offline Maps                           | Both           | ms-settings:maps                          |
|                    | About                                  | Both           | ms-settings:about                         |
| Devices            | Default camera                         | Mobile only    | ms-settings:camera                        |
|                    | Bluetooth                              | Desktop only   | ms-settings:bluetooth                     |
|                    | Mouse & touchpad                       | Both           | ms-settings:mousetouchpad                 |
|                    | NFC                                    | Both           | ms-settings:nfctransactions               |
| Network & Wireless | Wi-Fi                                  | Both           | ms-settings:network-wifi                  |
|                    | Airplane mode                          | Both           | ms-settings:network-airplanemode          |
| Network & Internet | Data usage                             | Both           | ms-settings:datausage                     |
|                    | Cellular & SIM                         | Both           | ms-settings:network-cellular              |
|                    | Mobile hotspot                         | Both           | ms-settings:network-mobilehotspot         |
|                    | Proxy                                  | Both           | ms-settings:network-proxy                 |
|                    | Status                                 | Desktop only   | ms-settings:network-status                |
| Personalization    | Personalization (category)             | Both           | ms-settings:personalization               |
|                    | Background                             | Desktop only   | ms-settings:personalization-background    |
|                    | Colors                                 | Both           | ms-settings:personalization-colors        |
|                    | Sounds                                 | Mobile only    | ms-settings:sounds                        |
|                    | Lock screen                            | Both           | ms-settings:lockscreen                    |
| Accounts           | Access work or school                  | Both           | ms-settings:workplace                     |
|                    | Email & app accounts                   | Both           | ms-settings:emailandaccounts              |
|                    | Family & other people                  | Both           | ms-settings:otherusers                    |
|                    | Sign-in options                        | Both           | ms-settings:signinoptions                 |
|                    | Sync your settings                     | Both           | ms-settings:sync                          |
|                    | Other people                           | Both           | ms-settings:otherusers                    |
|                    | Your info                              | Both           | ms-settings:yourinfo                      |
| Time and language  | Date & time                            | Both           | ms-settings:dateandtime                   |
|                    | Region & language                      | Desktop only   | ms-settings:regionlanguage                |
| Ease of Access     | Narrator                               | Both           | ms-settings:easeofaccess-narrator         |
|                    | Magnifier                              | Both           | ms-settings:easeofaccess-magnifier        |
|                    | High contrast                          | Both           | ms-settings:easeofaccess-highcontrast     |
|                    | Closed captions                        | Both           | ms-settings:easeofaccess-closedcaptioning |
|                    | Keyboard                               | Both           | ms-settings:easeofaccess-keyboard         |
|                    | Mouse                                  | Both           | ms-settings:easeofaccess-mouse            |
|                    | Other options                          | Both           | ms-settings:easeofaccess-otheroptions     |
| Privacy            | Location                               | Both           | ms-settings:privacy-location              |
|                    | Camera                                 | Both           | ms-settings:privacy-webcam                |
|                    | Microphone                             | Both           | ms-settings:privacy-microphone            |
|                    | Motion                                 | Both           | ms-settings:privacy-motion                |
|                    | Speech, inking & typing                | Both           | ms-settings:privacy-speechtyping          |
|                    | Account info                           | Both           | ms-settings:privacy-accountinfo           |
|                    | Contacts                               | Both           | ms-settings:privacy-contacts              |
|                    | Calendar                               | Both           | ms-settings:privacy-calendar              |
|                    | Call history                           | Both           | ms-settings:privacy-callhistory           |
|                    | Email                                  | Both           | ms-settings:privacy-email                 |
|                    | Messaging                              | Both           | ms-settings:privacy-messaging             |
|                    | Radios                                 | Both           | ms-settings:privacy-radios                |
|                    | Background Apps                        | Both           | ms-settings:privacy-backgroundapps        |
|                    | Other devices                          | Both           | ms-settings:privacy-customdevices         |
|                    | Feedback & diagnostics                 | Both           | ms-settings:privacy-feedback              |
| Update & security  | For developers                         | Both           | ms-settings:developers                    |
 



<!--HONumber=Aug16_HO4-->


