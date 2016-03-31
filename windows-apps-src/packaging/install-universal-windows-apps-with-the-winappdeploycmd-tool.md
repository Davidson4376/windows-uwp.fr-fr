---
ms.assetid: 6AA037C0-35ED-4B9C-80A3-5E144D7EE94B
Install apps with the WinAppDeployCmd.exe tool
Windows Application Deployment (WinAppDeployCmd.exe) is a command line tool that can use to deploy a Universal Windows Platform (UWP) app from a Windows 10 machine to any Windows 10 Mobile device.
---
# Install apps with the WinAppDeployCmd.exe tool

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Windows Application Deployment (WinAppDeployCmd.exe) is a command line tool that can use to deploy a Universal Windows Platform (UWP) app from a Windows 10 machine to any Windows 10 Mobile device. You can use this tool to deploy an .appx package when the Windows 10 Mobile device is connected by USB or available on the same subnet without needing Microsoft Visual Studio or the solution for that app. This article describes how to install UWP apps using this tool.

You just need the Windows 10 SDK installed to run the WinAppDeployCmd tool from a command prompt or a script file. When you install an app with WinAppDeployCmd.exe, this uses the .appx file to side-load your app onto a Windows 10 Mobile device. This command does not install the certificate required for your app. To run the app, the Windows 10 Mobile device must be in developer mode or already have the certificate installed.

Before you can deploy your app to Windows 10 Mobile devices, you must create a package first. For more information, see \[parent link here\].

The **WinAppDeployCmd.exe** tool is located here on your Windows 10 machine: **C:\\Program Files (x86)\\Windows Kits\\10\\bin\\x86\\WinAppDeployCmd.exe** (based on your installation path for the SDK). First, connect your Windows 10 Mobile device to the same subnet or connect it directly to your Windows 10 machine with a USB connection. Then use the following syntax and examples of this command later in this article to deploy your .appx package:

## WinAppDeployCmd syntax and options

Here is the possible syntax that you can use for **WinAppDeployCmd.exe**

``` syntax
WinAppDeployCmd command -option <argument> ...
    WinAppDeployCmd devices
    WinAppDeployCmd devices <x>
    WinAppDeployCmd install -file <path> -ip <address>
    WinAppDeployCmd install -file <path> -guid <address> -pin <p>
    WinAppDeployCmd install -file <path> -ip <address> -dependency <a> <b> ...
    WinAppDeployCmd install -file <path> -guid <address> -dependency <a> <b> ...
    WinAppDeployCmd uninstall -file <path>
    WinAppDeployCmd uninstall -package <name>
    WinAppDeployCmd update -file <path>
    WinAppDeployCmd list -ip <address>
    WinAppDeployCmd list -guid <address>
```

You can install or uninstall an app on the target device, or you can update an app that's already installed. To keep data or settings saved by an app that's already installed, use the **update** options instead of the **install** options.

The following table describes the commands for **WinAppDeployCmd.exe**.

|             |                                                                     |
|-------------|---------------------------------------------------------------------|
| **Command** | **Description**                                                     |
| devices     | Show the list of available network devices.                         |
| install     | Install a UWP app package to the target device.                     |
| update      | Update a UWP app that is already installed on the target device.    |
| list        | Show the list of UWP apps installed on the specified target device. |
| uninstall   | Uninstall the specified app package from the target device.         |

 

The following table describes the options for **WinAppDeployCmd.exe**

|                  |                                                                                                                                                                                                               |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Command**      | **Description**                                                                                                                                                                                               |
| -h (-help)       | Show the commands, options and arguments.                                                                                                                                                                     |
| -ip              | IP address of the target device.                                                                                                                                                                              |
| -g (-guid)       | Unique identifier of the target device.                                                                                                                                                                       |
| -d (-dependency) | (Optional) Specifies the dependency path for each of the package dependencies. If no path is specified, the tool searches for dependencies in the root directory for the app package and the SDK directories. |
| -f (-file)       | File path for the app package to install, update or uninstall.                                                                                                                                                |
| -p (-package)    | The full package name for the app package to uninstall. (You can use the list command to find the full names for packages already installed on the device.)                                                   |
| -pin             | A pin if it is required to establish a connection with the target device. (You will be prompted to retry with the -pin option if authentication is required.)                                                 |

 

The following table describes the options for **WinAppDeployCmd.exe**.

|                        |                                                                              |
|------------------------|------------------------------------------------------------------------------|
| **Argument**           | **Description**                                                              |
| &lt;x&gt;              | Timeout in seconds. (Default is 10)                                          |
| &lt;address&gt;        | IP address or unique identifier of the target device.                        |
| &lt;a&gt;&lt;b&gt; ... | Dependency path for each of the app package dependencies.                    |
| &lt;p&gt;              | An alpha-numeric pin shown in the device settings to establish a connection. |
| &lt;path&gt;           | File system path.                                                            |
| &lt;name&gt;           | Full package name for the app package to uninstall.                          |

 
## WinAppDeployCmd.exe examples

Here are some examples of how to deploy from the command-line using the sytax for **WinAppDeployCmd.exe**.

Shows the devices that are available for deployment. The command times out in 3 seconds.

``` syntax
WinAppDeployCmd devices 3
```

Installs the app from MyApp.appx package that is in your PC's Downloads directory to a Windows 10 Mobile device with an IP address of 192.168.0.1 with a PIN of A1B2C3 to establish a connection with the device

``` syntax
WinAppDeployCmd install -file "Downloads\MyApp.appx" -ip 192.168.0.1 -pin A1B2C3
```

Uninstalls the specified package (based on its full name) from a Windows 10 Mobile device with an IP address of 192.168.0.1. You can use the list command to see the full names of any packages that are installed on a device.

``` syntax
WinAppDeployCmd uninstall -package Company.MyApp_1.0.0.1_x64__qwertyuiop -ip 192.168.0.1
```

Updates the app that is already installed on the Windows 10 Mobile device with an IP address of 192.168.0.1 using the specified .appx package.

``` syntax
WinAppDeployCmd update -file "Downloads\MyApp.appx" -ip 192.168.0.1
```

<!--HONumber=Mar16_HO1-->
