---
ms.assetid: 25B18BA5-E584-4537-9F19-BB2C8C52DFE1
title: App capability declarations
description: Capabilities must be declared in your Universal Windows Platform (UWP) app's package manifest to access certain API or resources like pictures, music, or devices like the camera or the microphone.
---
# App capability declarations

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Capabilities must be declared in your Universal Windows Platform (UWP) app's [package manifest](https://msdn.microsoft.com/library/windows/apps/BR211474) to access certain API or resources like pictures, music, or devices like the camera or the microphone.

You request access to specific resources or API by declaring capabilities in your app's [package manifest](https://msdn.microsoft.com/library/windows/apps/BR211474). You can declare general capabilities by using the [Manifest Designer](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/br230259.aspx) in Microsoft Visual Studio, or you can add them manually. For more information, see [How to specify capabilities in a package manifest](https://msdn.microsoft.com/library/windows/apps/BR211477). It is important to know that when customers get your app from the Store, they're notified of all the capabilities that the app declares. Avoid declaring capabilities that your app doesn't need.

Some capabilities provide apps with access to a *sensitive resource*. These resources are considered sensitive because they can access the user's personal data or cost the user money. Privacy settings, managed by the Settings app, let the user dynamically control access to sensitive resources. Thus, it's important that your app doesn't assume a sensitive resource is always available. For more info about accessing sensitive resources, see [Guidelines for privacy-aware apps](https://msdn.microsoft.com/library/windows/apps/Hh768223). Capabilities that provide apps with access to a *sensitive resource* are annotated by an asterisk (\*) next to the capability scenario.

This article reviews four categories of capabilities described below.

-   General-use capabilities that apply to most common app scenarios.

-   Device capabilities that allow your app to access peripheral and internal devices.

-   Special-use capabilities that require a special company account for submission to the Store to use them. For more info about company accounts, see [Account types, locations, and fees](https://msdn.microsoft.com/library/windows/apps/JJ863494).

-   Restricted capabilities that are only available to Microsoft and its partners.

## General-use capabilities

General-use capabilities apply to the most common app scenarios.

<table>
        <thead>
            <tr>
                <th>Capability scenario</th>
                <th>Capability usage</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>**Music***</td>
                <td>
                    The **musicLibrary** capability provides programmatic access to the user's Music, allowing the app to enumerate and access all files in the library without user interaction. This capability is typically used in jukebox apps that make use of the entire Music library.

                    The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app. Declare the **musicLibrary** capability only when the scenarios for your app require programmatic access and can't be realized by using the **file picker**.

                    The **musicLibrary** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="musicLibrary"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Pictures***</td>
                <td>
                    The **picturesLibrary** capability provides programmatic access to the user's Pictures, allowing the app to enumerate and access all files in the library without user interaction. This capability is typically used in photo apps that make use of the entire Pictures library.

                    The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app. Declare the **picturesLibrary** capability only when the scenarios for your app require programmatic access and can't be realized them by using the **file picker**.

                    The **picturesLibrary** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="picturesLibrary"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Videos***</td>
                <td>
                    The **videosLibrary** capability provides programmatic access to the user's Videos, allowing the app to enumerate and access all files in the library without user interaction. This capability is typically used in movie-playback apps that make use of the entire Videos library.

                    The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app. Declare the **videosLibrary** capability only when the scenarios for your app require programmatic access and can't be realized by using the **file picker**.

                    The **videosLibrary** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="videosLibrary"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Removable Storage**</td>
                <td>
                    The **removableStorage** capability provides programmatic access to files on removable storage, like USB keys and external hard drives, filtered to the file-type associations declared in the package manifest. For example, if a document-reader app declares a .doc file-type association, it can open .doc files on the removable storage device, but not other types of files. Be careful when you declare this capability, because users may include a variety of info in their removable storage devices, and will expect your app to provide a valid justification for programmatic access to the removable storage for all files of the declared type.

                    Users will expect your app to handle any file associations that you declare. So don't declare file associations that your app cannot handle responsibly. The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app.

                    Declare the **removableStorage** capability only when the scenarios for your app require programmatic access and can't be realized by using the [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847).

                    The **removableStorage** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="removableStorage"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Internet and public networks***</td>
                <td>
                    There are two capabilities that provide different levels of access to the Internet and public networks.

                    The **internetClient** capability indicates that apps can receive incoming data from the Internet. Cannot act as a server. No local network access.

                    The **internetClientServer** capability indicates that apps can receive incoming data from the Internet. Can act as a server. No local network access.

                    Most apps that have a web service component will use **internetClient**. Apps that enable peer-to-peer (P2P) scenarios where the app needs to listen for incoming network connections should use **internetClientServer**. The **internetClientServer** capability includes the access that the **internetClient** capability provides, so you don't need to specify **internetClient** when you specify **internetClientServer**.
                </td>
            </tr>
            <tr>
                <td>**Homes and work networks***</td>
                <td>
                    The **privateNetworkClientServer** capability provides inbound and outbound access to home and work networks through the firewall. This capability is typically used for games that communicate across the local area network (LAN), and for apps that share data across a variety of local devices. If your app specifies **musicLibrary**, **picturesLibrary**, or **videosLibrary**, you don't need to use this capability to access the corresponding library in a Home Group. On Windows, this capability does not provide access to the Internet.
                </td>
            </tr>
            <tr>
                <td>**Appointments**</td>
                <td>
                    The **appointments** capability provides access to the user’s appointment store. This capability allows read access to appointments obtained from the synced network accounts and to other apps that write to the appointment store. With this capability, your app can create new calendars and write appointments to calendars that it creates.

                    The **appointments** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="appointments"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Contacts***</td>
                <td>
                    The **contacts** capability provides access to the aggregated view of the contacts from various contacts stores. This capability gives the app limited access (network permitting rules apply) to contacts that were synced from various networks and the local contact store.

                    The **contacts** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="contacts"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Code generation**</td>
                <td>
                    The **codeGeneration** capability allows apps to access the following functions which provide JIT capabilities to apps.

                    - [**VirtualProtectFromApp**](https://msdn.microsoft.com/library/windows/desktop/Mt169846)
                    - [**CreateFileMappingFromApp**](https://msdn.microsoft.com/library/windows/desktop/Hh994453)
                    - [**OpenFileMappingFromApp**](https://msdn.microsoft.com/library/windows/desktop/Mt169844)
                    - [**MapViewOfFileFromApp**](https://msdn.microsoft.com/library/windows/desktop/Hh994454)
                </td>
            </tr>
            <tr>
                <td>**AllJoyn**</td>
                <td>
                    The **allJoyn** capability allows AllJoyn-enabled apps and devices on a network to discover and interact with each other.

                    All apps that access APIs in the [**Windows.Devices.AllJoyn**](https://msdn.microsoft.com/library/windows/apps/Dn894971) namespace must use this capability.
                </td>
            </tr>
            <tr>
                <td>**Phone calls**</td>
                <td>
                    The **phoneCall** capability allows apps to access all of the phone lines on the device and perform the following functions.

                    - Place a call on the phone line and show the system dialer without prompting the user.
                    - Access line-related metadata.
                    - Access line-related triggers.
                    - Allows the user-selected spam filter app to set and check block list and call origin information.

                    The **phoneCall** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="phoneCall"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                    The **phoneCallHistoryPublic** capability allows apps to read cellular and some VOIP call history information on the device. This capability also allows the app to write VOIP call history entries. This capability is required to access all members of the [**PhoneCallHistoryStore**](https://msdn.microsoft.com/library/windows/apps/Dn705931) class.
                </td>
            </tr>
            <tr>
                <td>**Recorded Calls Folder***</td>
                <td>
                    <p>The **recordedCallsFolder** device capability allows apps to access the recorded calls folder.</p>
                    <p>The **recordedCallsFolder** capability must include the **mobile** namespace when you declare it in your app's package manifest as shown below.</p>
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;mobile:Capability Name="recordedCallsFolder"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**User Account Information***</td>
                <td>
                    <p>The **userAccountInformation** capability gives apps the ability to access the user's name and picture.</p>
                    <p>This capability is required to access some APIs in the Windows.System.User namespace.</p>
                    <p>The **userAccountInformation** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="userAccountInformation"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**VOIP calling**</td>
                <td>
                    <p>The **voipCall** capability allows apps to access the VOIP calling APIs in the [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266) namespace.</p>
                    <p>The **voipCall** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="voipCall"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**3D Objects**</td>
                <td>
                    <p>The **objects3D** capability allows apps to have programmatic access to the 3D object files. This capability is typically used in 3D apps and games that need access to the entire 3D objects library.</p>
                    <p>This capability is required to access the folder that contains the 3D objects using APIs in the [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346) namespace.</p>
                    <p>The **objects3D** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="objects3d"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Read Blocked Messages***</td>
                <td>
                    <p>The **blockedChatMessages** capability allows apps to read SMS and MMS messages that have been blocked by the Spam Filter app.</p>
                    <p>This capability is required to access the blocked messages using APIs in the [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) namespace.</p>
                    <p>The **blockedChatMessages** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="blockedChatMessages"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Chat Message Access**</td>
                <td>
                    <p>The **chat** capability allows apps to read and delete Text Messages. This capability also allows apps to store chat messages in the system data store.</p>
                    <p>This capability is required to use some APIs in the [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) namespace.</p>
                    <p>The **chat** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="chat"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**IoT Low-level Bus Hardware**</td>
                <td>
                    <p>The **lowLevelDevices** capability allows apps that run on IoT devices to access low-level bus hardware such as GPIO, I2C, SPI, ADC, and PWM.</p>
                    <p>This capability is required to access some APIs in the [**Windows.Devices.Spi**](https://msdn.microsoft.com/library/windows/apps/Dn708178) namespaces.</p>
                    <p>The **lowLevelDevices** capability must include the **iot** namespace when you declare it in your app's package manifest as shown below.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;iot:Capability Name="lowLevelDevices"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**IoT System Administration**</td>
                <td>
                    <p>The **systemManagement** capability allows apps to have basic system administration privileges such as shutting down or rebooting, locale, and timezone.</p>
                    <p>This capability is required to access some of the APIs in the [**Windows.System**](https://msdn.microsoft.com/library/windows/apps/BR241814) namespace.</p>
                    <p>The **systemManagement** capability must include the **iot** namespace when you declare it in your app's package manifest as shown below.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;iot:Capability Name="systemManagement"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
        </tbody>
</table>

 
## Device capabilities

Device capabilities allow your app to access peripheral and internal devices. Device capabilities are specified by using the **DeviceCapability** element in your app package manifest. This element may require additional child elements and some device capabilities need to be added to the package manifest manually. For more info, see [How to specify device capabilities in a package manifest](https://msdn.microsoft.com/library/windows/apps/Dn263092) and [**DeviceCapability Schema reference**](https://msdn.microsoft.com/library/windows/apps/BR211430).

| Capability scenario | Capability usage |
|---------------------|------------------|
| **Location**\* | The **location** capability provides access to location functionality that is retrieved from dedicated hardware like a GPS sensor in the PC or is derived from available network info. Apps must handle the case in which the user has disabled location services from the **Settings** charm. |
| **Microphone** | The **microphone** capability provides access to the microphone’s audio feed, which allows the app to record audio from connected microphones. Apps must handle the case in which the user has disabled the microphone from the **Settings** charm. |
| **Proximity** | The **proximity** capability enables multiple devices in close proximity to communicate with one another. This capability is typically used in casual multi-player games and in apps that exchange information. Devices attempt to use the communication technology that provides the best possible connection, including Bluetooth, Wi-Fi, and the Internet. This capability is used only to initiate communication between the devices. |
| **Webcam** | The **webcam** capability provides access to the video feed of a built-in camera or external webcam, which allows the app to capture photos and videos. On Windows, apps must handle the case in which the user has disabled the camera from the **Settings** charm.<br/>The **webcam** capability only grants access to the video stream. In order to grant access to the audio stream as well, the **microphone** capability must be added. | 
| **USB** | The **usb** device capability enables access to APIs in the [Updating the app manifest package for a USB device](http://go.microsoft.com/fwlink/p/?LinkId=302259). | 
| **Human interface device (HID)** | The **humaninterfacedevice** device capability enables access to APIs in the [How to specify device capabilities for HID](https://msdn.microsoft.com/library/windows/apps/Dn263091). |
| **Point of Sevice (POS)** | The **pointOfService** device capability enables access to APIs in the [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071) namespace. This namespace lets your app access Point of Service (POS) barcode scanners and magnetic stripe readers. The namespace provides a vendor-neutral interface for accessing POS devices from various manufacturers from a Windows Store app. |
| **Bluetooth** | The **bluetooth** device capability allows apps to communicate with already paired bluetooth devices over both Generic Attribute (GATT) or Classic Basic Rate (RFCOMM) protocol.<br/>This capability is required to use some APIs in the [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413) namespace. |
| **Wi-Fi Networking** | The **wiFiControl** device capability allows apps to scan and connect to Wi-Fi networks.<br/>This capability is required to use some APIs in the [**Windows.Devices.WiFi**](https://msdn.microsoft.com/library/windows/apps/Dn975224) namespace. |
| **Radio state** | The **radios** device capability allows apps to toggle the Wi-Fi and Bluetooth radios.<br/>This capability is required to use the APIs in the [**Windows.Devices.Radios**](https://msdn.microsoft.com/library/windows/apps/Dn996447) namespace.  |
| **Optical disc** | The **optical** device capability allows apps to access functions on optical disk drives such as CD, DVD, and Blu-ray.<br/>This capability is required to use some APIs in the [**Windows.Devices.Custom**](https://msdn.microsoft.com/library/windows/apps/Dn263667) namespace. |
| **Motion activity** | The **activity** device capability allows apps to detect the current motion of the device.<br/>This capability is required to use some APIs in the [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408) namespace. |

## Special and restricted capabilities

**Important**  
Special and restricted capabilities are intended for very specific scenarios. The use of these capabilities is highly restricted and subject to additional Store onboarding policy and review.

There are cases where such capabilities are necessary and appropriate, such as banking with two-factor authentication, where users provide a smart card with a digital certificate that confirms their identity. Other apps may be designed primarily for enterprise customers and may need access to corporate resources that cannot be accessed without the user’s domain credentials.

Apps that apply the special-use capabilities require a company account to submit them to the Store. In contrast, restricted capabilities do not require a special company account for the Store, they are not available for developers to use. Restricted capabilities are available only to apps that are developed by Microsoft and its partners. For more information about company accounts, see [Account types, locations, and fees](https://msdn.microsoft.com/library/windows/apps/JJ863494).

All restricted capabilities must include the **rescap** namespace when you declare them in your app's package manifest differently than other capabilities. The following example shows you how to declare the **appCaptureSettings** capability.

```xml
<Capabilities>
    <rescap:Capability Name="appCaptureSettings"/>
</Capabilities>
```

You must also add the **xmlns:rescap** namespace declaration in the top of the Package.appxmanifest file as shown below.

```xml
<Package 
    xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10" 
    xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest" 
    xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10" 
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities" 
    IgnorableNamespaces="uap mp wincap rescap"> 
```

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Capability scenario</th>
<th align="left">Capability usage</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">**Enterprise**</td>
<td align="left"><p>Windows domain credentials enable a user to log into remote resources using their credentials, and act as if a user provided their user name and password. The **enterpriseAuthentication** special capability is typically used in line-of-business apps that connect to servers within an enterprise.</p>
<p>You don't need this capability for generic communication across the Internet.</p>

<p>The **enterpriseAuthentication** special capability is intended to support common line-of-business apps. Don't declare it in apps that don't need to access corporate resources. The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that enables users to open files on a network share for use with an app. Declare the **enterpriseAuthentication** special capability only when the scenarios for your app require programmatic access, and you cannot realize them by using the **file picker**.</p>
<p>The **enterpriseAuthentication** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.</p>
<div class="code">
<span codelanguage="XML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="enterpriseAuthentication"/&gt;
&lt;/Capabilities&gt;</code></pre></td>
</tr>
</tbody>
</table>
</div>
<p>The **enterpriseDataPolicy** capability allows apps to define and use enterprise-specific policies for the device. This capability is required to use all members of the following classes.</p>
<ul>
<li>[**FileProtectionManager**](https://msdn.microsoft.com/library/windows/apps/Dn705151)</li>
<li>[**DataProtectionManager**](https://msdn.microsoft.com/library/windows/apps/Dn706017)</li>
<li>[**ProtectionPolicyManager**](https://msdn.microsoft.com/library/windows/apps/Dn705170)</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">**Shared user certificates**</td>
<td align="left"><p>The **sharedUserCertificates** special capability enables an app to add and access software and hardware-based certificates in the Shared User store, such as certificates stored on a smart card. This capability is typically used for financial or enterprise apps that require a smart card for authentication.</p>
<p>The **sharedUserCertificates** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.</p>
<div class="code">
<span codelanguage="XML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="sharedUserCertificates"/&gt;
&lt;/Capabilities&gt;</code></pre></td>
</tr>
</tbody>
</table>
</div></td>
</tr>
<tr class="odd">
<td align="left">**Documents***</td>
<td align="left"><p>The **documentsLibrary** special capability provides programmatic access to the user's Documents, filtered to the file type associations declared in the package manifest, to support offline access to OneDrive. For example, if a DOC reader app declared a .doc file type association, it can open .doc files in Documents, but not other types of files.</p>
<p>Apps that declare the **documentsLibrary** special capability can't access Documents on Home Group computers. The [file picker](https://msdn.microsoft.com/library/windows/apps/Hh465174) provides a robust UI mechanism that enables users to open files for use with an app. Declare the **documentsLibrary** special capability only when you cannot use the file picker.</p>
<p>To use the **documentsLibrary** special capability, an app must:</p>
<ul>
<li>Facilitate cross-platform offline access to specific OneDrive content using valid OneDrive URLs or Resource IDs</li>
<li>Save open files to the user’s OneDrive automatically while offline</li>
</ul>
<p>Apps that use the **documentsLibrary** special capability for these two purposes may also optionally use the capability to open embedded content within another document. Only the above uses of the **documentsLibrary** special capability are accepted.</p>
<ul>
<li><p>Your app can't access the Documents library in the phone's internal storage. If another app creates a Documents folder on the optional SD card, however, your app can see that folder.</p></li>
</ul>
<p>The **documentsLibrary** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.</p>
<div class="code">
<span codelanguage="XML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="documentsLibrary"/&gt;
&lt;/Capabilities&gt;</code></pre></td>
</tr>
</tbody>
</table>
</div></td>
</tr>
<tr class="even">
<td align="left">**Game DVR Settings**</td>
<td align="left"><p>The **appCaptureSettings** restricted capability allows apps to control the user settings for the Game DVR.</p>
<p>This capability is required to use some APIs in the [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/BR226738) namespace.</p></td>
</tr>
<tr class="odd">
<td align="left">**Cellular**</td>
<td align="left"><p>The **cellularDeviceControl** restricted capability allows apps to have control over the cellular device.</p>
<p>The **cellularDeviceIdentity** capability allows apps to access cellular identification data.</p>
<p>The **cellularMessaging** capability allows apps to make use of SMS and RCS.</p>
<p>These capabilities are required to use some APIs in the [**Windows.Devices.Sms**](https://msdn.microsoft.com/library/windows/apps/BR206567) namespaces.</p>
<p>Starting in Windows 10, apps calling [**AppIDList**](https://msdn.microsoft.com/library/windows/apps/Dn393996)).</p></td>
</tr>
<tr class="even">
<td align="left">**Device Unlock**</td>
<td align="left"><p>The **deviceUnlock** restricted capability allows apps to unlock a device for developer and enterprise sideloading scenarios.</p></td>
</tr>
<tr class="odd">
<td align="left">**Dual SIM Tiles**</td>
<td align="left"><p>The **dualSimTiles** restricted capability allows apps to create an additional app list entry on devices that have multiple SIMs.</p>
<p>This capability is required to use some APIs in the [**Windows.UI.StartScreen**](https://msdn.microsoft.com/library/windows/apps/BR242235) namespace.</p></td>
</tr>
<tr class="even">
<td align="left">**Enterprise Shared Storage**</td>
<td align="left"><p>The **enterpriseDeviceLockdown** restricted capability allows apps to use the device lock down API and access the enterprise shared storage folders.</p></td>
</tr>
<tr class="odd">
<td align="left">**System Input Injection**</td>
<td align="left"><p>The **inputInjection** restricted capability allows apps to inject various forms of input such as HID, touch, pen, keyboard or mouse into the system programmatically. This capability is typically used for collaboration apps that can take control of the system.</p>
<div class="alert">
**Note**  For a PC, input injection from an app that has this capability will only be received by processes in the same App Container.
</div>
</td>
</tr>
<tr class="even">
<td align="left">**Observe Input***</td>
<td align="left"><p>The **inputObservation** restricted capability allows apps to observe various forms of raw input such as HID, touch, pen, keyboard, or mouse being received by the system regardless of its final destination.</p></td>
</tr>
<tr class="odd">
<td align="left">**Suppress Input**</td>
<td align="left"><p>The **inputSuppression** restricted capability allows apps to suppress various forms of raw input such as HID, touch, pen, keyboard, or mouse from being received by the system.</p></td>
</tr>
<tr class="even">
<td align="left">**VPN App**</td>
<td align="left"><p>The **networkingVpnProvider** restricted capability allows apps to have full access to VPN features, including the ability to manage connections and provide VPN Plugin functionality.</p>
<p>This capability is required to use some APIs in the [**Windows.Networking.Vpn**](https://msdn.microsoft.com/library/windows/apps/Dn434040) namespace.</p></td>
</tr>
<tr class="odd">
<td align="left">**Other App Mangement**</td>
<td align="left"><p>The **packageManagement** restricted capability allows apps to manage other apps directly.</p>
<p>The **packageQuery** device capability allows apps to gather information about other apps.</p>
<p>These capabilities are required to access some methods and properties in the [**PackageManager**](https://msdn.microsoft.com/library/windows/apps/BR240960) class.</p></td>
</tr>
<tr class="even">
<td align="left">**Screen Projection**</td>
<td align="left"><p>The **screenDuplication** restricted capability allows apps to project the screen on another device.</p>
<p>This capability is required to use APIs in the DirectX namespace.</p></td>
</tr>
<tr class="odd">
<td align="left">**User Principal Name**</td>
<td align="left"><p>The **userPrincipalName** restricted capability allows apps to modify and access the thumbnail cache from photos.</p>
<p>This capability is required to call the [**GetUserNameEx**](https://msdn.microsoft.com/library/windows/desktop/ms724435) function.</p></td>
</tr>
<tr class="even">
<td align="left">**Wallet**</td>
<td align="left"><p>The **walletSystem** restricted capability allows apps to have full access to the stored wallet cards.</p>
<p>This capability is required to use APIs in the [**Windows.ApplicationModel.Wallet.System**](https://msdn.microsoft.com/library/windows/apps/Mt171610) namespace.</p></td>
</tr>
<tr class="odd">
<td align="left">**Location History**</td>
<td align="left"><p>The **locationHistory** restricted capability allows apps to access the location history of the device.</p>
<p>This capability is required to use APIs in the [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/BR225603) namespace.</p></td>
</tr>
<tr class="even">
<td align="left">**App Close Confirmation**</td>
<td align="left"><p>The **confirmAppClose** restricted capability allows apps to close themselves, their own windows, and delay the closing of their app.</p>
<p>This capability is required to use APIs in the [**Windows.UI.ViewManagement**](https://msdn.microsoft.com/library/windows/apps/BR242295) namespace.</p></td>
</tr>
<tr class="odd">
<td align="left">**Call History***</td>
<td align="left"><p>The **phoneCallHistory** restricted capability allows apps to read the call history and to delete entries in the history.</p>
<p>This capability is required to use APIs in the [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) namespace.</p></td>
</tr>
<tr class="even">
<td align="left">**System Level Appointment Access**</td>
<td align="left"><p>The **appointmentsSystem** restricted capability allows apps to read and modify all appointments on the user's calendar.</p>
<p>This capability is required to use APIs in the [**Windows.ApplicationModel.Appointment**](https://msdn.microsoft.com/library/windows/apps/Dn263359) namespace.</p></td>
</tr>
<tr class="odd">
<td align="left">**System Level Chat Message Access***</td>
<td align="left"><p>The **chatSystem** restricted capability allows apps to read and write all SMS and MMS messages.</p>
<p>This capability is required to use APIs in the [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) namespace.</p></td>
</tr>
<tr class="even">
<td align="left">**System Level Contact Access**</td>
<td align="left"><p>The **contactsSystem** restricted capability allows apps to read contact information that has been designated as restricted or sensitive and modify existing contact information.</p>
<p>This capability is required to use APIs in the [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) namespace.</p></td>
</tr>
<tr class="odd">
<td align="left">**Email Access***</td>
<td align="left"><p>The **email** restricted capability allows apps to read, triage, and send user emails.</p>
<p>This capability is required to use APIs in the [**Windows.ApplicationModel.Email**](https://msdn.microsoft.com/library/windows/apps/Dn631285) namespace.</p></td>
</tr>
<tr class="even">
<td align="left">**System Level Email Access**</td>
<td align="left"><p>The **emailSystem** restricted capability allows apps to read, triage, and send user restricted or sensitive emails.</p>
<p>This capability is required to use APIs in the [**Windows.ApplicationModel.Email**](https://msdn.microsoft.com/library/windows/apps/Dn631285) namespace.</p></td>
</tr>
<tr class="odd">
<td align="left">**System Level Call History Access**</td>
<td align="left"><p>The **phoneCallHistorySystem** restricted capability allows apps to fully modify the call history by changing existing entries and writing new ones.</p>
<p>This capability is required to use APIs in the [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266) namespace.</p></td>
</tr>
<tr class="even">
<td align="left">**Send Text Messages***</td>
<td align="left"><p>The **smsSend** restricted capability allows apps to send SMS and MMS messages.</p>
<p>This capability is required to use APIs in the [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) namespace.</p></td>
</tr>
<tr class="odd">
<td align="left">**System Level Access to All User Data**</td>
<td align="left"><p>The **userDataSystem** restricted capability allows apps to access the user data system datastore.</p></td>
</tr>
<tr class="even">
<td align="left">**Store Preview Features**</td>
<td align="left"><p>The **previewStore** restricted capability allows apps to retrieve and purchase SKUs of in-app products.</p>
<p>This capability is required to use certain APIs in the [**Windows.ApplicationModel.Store.Preview**](https://msdn.microsoft.com/library/windows/apps/Mt185546) namespace.</p></td>
</tr>
<tr class="odd">
<td align="left">**First-Time Sign-in Settings**</td>
<td align="left"><p>The **firstSignInSettings** restricted capability allows apps to access user settings that were set when the user first signed in to their device.</p></td>
</tr>
<tr class="even">
<td align="left">**Windows Team Experience**</td>
<td align="left"><p>The **teamEditionExperience** restricted capability allows apps to access internal APIs that control many experiential aspects of a Windows Team session. A Windows Team session is likely to be running on a team device such as a Microsoft Surface Hub.</p></td>
</tr>
<tr class="odd">
<td align="left">**Remote Unlock**</td>
<td align="left"><p>The **remotePassportAuthentication** restricted capability allows apps to access credentials that can be used to unlock a remote PC.</p></td>
</tr>
<tr class="even">
<td align="left">**Preview Composition**</td>
<td align="left"><p>The **previewUiComposition** restricted capability allows apps to preview the [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Dn706878) namespace for their user interface so they can provide feedback on the API before it is completed. Please contact wincomposition@microsoft.com for more information.</p></td>
</tr>
<tr class="odd">
<td align="left">**Secure Assessment Lockdown**</td>
<td align="left"><p>The **secureAssessment** restricted capability allows apps to lockdown Windows into a single app mode for secure assessments.</p></td>
</tr>
<tr class="even">
<td align="left">**Connection Manager Provisioning**</td>
<td align="left"><p>The **networkConnectionManagerProvisioning** restricted capability allows apps to define the policies that connect the device with WWAN and WLAN interfaces. Apps that use this capability are created by Mobile Operators to govern the devices that connect to their mobile network.</p></td>
</tr>
<tr class="odd">
<td align="left">**Data Plan Provisioning**</td>
<td align="left"><p>The **networkDataPlanProvisioning** restricted capability allows apps to gather information about data plans on the device and read network usage. Apps that use this capability are created by Mobile Operators to integrate their customers’ actual data usage into the OS Data usage setting.</p></td>
</tr>
<tr class="even">
<td align="left">**Software Licensing**</td>
<td align="left"><p>The **slapiQueryLicenseValue** restricted capability allows apps to query software licensing policies.</p></td>
</tr>
<tr class="odd">
<td align="left">**Extended Execution**</td>
<td align="left"><p>The **extendedExecutionBackgroundAudio** restricted capability allows apps to play audio when the app is not in the foreground.</p>
<p>The **extendedExecutionCritical** restricted capability allows apps to begin a critical extended execution session.</p>
<p>The **extendedExecutionUnconstrained** restricted capability allows apps to begin an unconstrained extended execution session.</p></td>
</tr>
<tr class="even">
<td align="left">**Mobile Device Management**</td>
<td align="left"><p>The **deviceManagementDmAccount** restricted capability allows apps to provision and configure Mobile Operator Open Mobile Alliance - Device Management (MO OMA-DM) accounts.</p>
<p>The **deviceManagementFoundation** restricted capability allows apps to have basic access to the Mobile Device Management (MDM) configuration service provider (CSP) infrastructure on the device. Note that other capabilities are needed to access specific CSPs.</p>
<p>The **deviceManagementWapSecurityPolicies** restricted capability allows apps to configure Wireless Application Protocol (WAP)-based services such as MMs, Service Indication/Service Loading (SI/SL), and Open Mobile Alliance - Client Provisioning (OMA-CP).</p>
<p>The **deviceManagementEmailAccount** restricted capability allows apps created by Mobile Operators to add and manage an email account on devices they provision to users.</p></td>
</tr>
<tr class="odd">
<td align="left">**Package Policy Control**</td>
<td align="left"><p>The **packagePolicySystem** restricted capability allows apps to have control of system policies related to apps that are installed on the device.</p></td>
</tr>
<tr class="even">
<td align="left">**Games List**</td>
<td align="left"><p>The **gameList** restricted capability allows apps to get a list of known games installed on the system.</p></td>
</tr>
<tr class="odd">
<td align="left">**Xbox Accessory**</td>
<td align="left"><p>The **xboxAccessoryManagement** restricted capability allows apps to directly manage Xbox devices that conform to the Xbox hardware specification.</p></td>
</tr>
<tr class="even">
<td align="left">**Speech Recognition for Accessories**</td>
<td align="left"><p>The **cortanaSpeechAccessory** restricted capability allows apps to invoke and pass commands to Cortana.</p></td>
</tr>
<tr class="odd">
<td align="left">**Accessory Management**</td>
<td align="left"><p>The **accessoryManager** restricted capability allows apps to register as an accessory app and opt-in to specific app notifications so that they may be forwarded to accessories and display to the user.</p></td>
</tr>
<tr class="even">
<td align="left">**Driver access**</td>
<td align="left"><p>The **interopServices** restricted capability allows apps to interact directly with drivers.</p></td>
</tr>
<tr class="odd">
<td align="left">**Foreground observation**</td>
<td align="left"><p>The **inputForegroundObservation** restricted capability allows apps in the foreground to intercept keyboard input and byasses all non-app keyboard input processing. SAS combinations cannot be intercepted by this capability. This capability is required to access members of the [**KeyboardDeliveryInterceptor**](https://msdn.microsoft.com/library/windows/apps/Mt608395) class.</p></td>
</tr>
<tr class="even">
<td align="left">**OEM and MO Partner apps**</td>
<td align="left"><p>The **oemDeployment** restricted capability allows apps that are created by Microsoft partners to install new apps and query currently installed apps on the device.</p>
<p>The **oemPublicDirectory** restricted capability allows apps that are created by Microsoft partners to have access to the shared app folder.</p></td>
</tr>
</tbody>
</table>

**Note**  
This article is for Windows 10 developers writing UWP apps. If you’re developing for Windows 8.x or Windows Phone 8.x, see the [archived documentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

## Related topics

* [Manifest Designer](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/br230259.aspx)
* [Guidelines for privacy-aware apps](https://msdn.microsoft.com/library/windows/apps/Hh768223)
* [How to specify capabilities in a package manifest](https://msdn.microsoft.com/library/windows/apps/BR211477)
* [How to specify device capabilities in a package manifest](https://msdn.microsoft.com/library/windows/apps/Dn263092)
 

<!--HONumber=Mar16_HO1-->
