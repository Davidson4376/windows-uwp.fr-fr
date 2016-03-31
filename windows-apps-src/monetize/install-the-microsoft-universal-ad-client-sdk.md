---
ms.assetid: 28C6D865-2A5C-4B64-82E3-49A862A36850
The ad mediator control and the related developer tools are available in the Microsoft Universal Ad Client SDK.
Install the Universal Ad Client SDK
---

# Install the Universal Ad Client SDK

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

The ad mediator control and the related developer tools are available in the Microsoft Universal Ad Client SDK. This SDK is an extension to Visual Studio 2013 or Visual Studio 2015. To install the SDK:

1.  Close all instances of Visual Studio 2013 or Visual Studio 2015 and uninstall any previous versions of the ad mediator extension or Microsoft Advertising SDK.
2.  Download and install the [Microsoft Universal Ad Client SDK](http://go.microsoft.com/fwlink/p/?LinkId=518026). It may take a few minutes to install. Be sure and wait until the process has finished.
3.  Restart Visual Studio.

**Note**  To install the Microsoft Universal Ad Client SDK with Visual Studio 2015, you must have version 1.1 or later of the Visual Studio Tools for Universal Windows Apps installed. For more information about this update to the Visual Studio Tools for Universal Windows Apps, see the [release notes](http://go.microsoft.com/fwlink/?LinkID=624516).

## Update an existing project to use the latest version of the SDK

Microsoft periodically releases new versions of the Microsoft Universal Ad Client SDK with new ad mediation features, such as support for additional ad networks. If you have existing projects that use an earlier version of the SDK and you want to use the latest version, follow these instructions:

1.  [Download](http://go.microsoft.com/fwlink/p/?LinkId=518026) and run the latest release of the installer.
2.  Open each of your existing project in Visual Studio, and follow the [Configure ad networks](add-and-use-the-ad-mediator-control.md#configure-ad-networks) steps again to configure each of the ad networks used by your app. This process installs the latest versions of each of the ad network assemblies.

If you have a Windows Phone 8 or Windows Phone 8.1 Silverlight project, also perform these additional steps to ensure that the correct versions of the ad mediator assemblies are used by the app:

1.  Open your project in Visual Studio.
2.  In **Solution Explorer**, expand **References**.
3.  Right-click **Microsoft.AdMediator.Core** and choose **Properties**.
4.  Set the **Specific Version** property to **False**.
5.  Repeat steps 3-4 for **Microsoft.AdMediator.WindowsPhone8**.

## Related topics

* [Select and manage your ad networks](select-and-manage-your-ad-networks.md)
* [Add and use the ad mediation control](add-and-use-the-ad-mediator-control.md)
* [Test your ad mediation implementation](test-your-ad-mediation-implementation.md)
* [Submit your app and configure ad mediation](submit-your-app-and-configure-ad-mediation.md)
* [Troubleshoot ad mediation](troubleshoot-ad-mediation.md)
 

 



<!--HONumber=Mar16_HO1-->
