---
Description: Shows how to manually convert a Windows desktop application (like Win32, WPF, and Windows Forms) to a Universal Windows Platform (UWP) app.
Search.Product: eADQiWindows 10XVcnh
title: Manually convert a Windows desktop application to a Universal Windows Platform (UWP) app
translationtype: Human Translation
ms.sourcegitcommit: 2c1a8ea38081c947f90ea835447a617c388aec08
ms.openlocfilehash: 646a5b88cb7ca97f18bf4552950979a2ceead398

---

# Manually convert your Windows desktop application to a Universal Windows Platform (UWP) app

\[Some information relates to pre-released product which may be substantially modified before it's commercially released. Microsoft makes no warranties, express or implied, with respect to the information provided here.\]

Using the Converter is convenient and automatic, and it's useful if there's any uncertainty about what your installer does. But if your app is installed by using xcopy, or if you're familiar with the changes that your app's installer makes to the system, you can opt to create an app package and manifest manually.

Here are the steps to create a package manually:

## Create a manifest by hand.

Your _appxmanifest.xml_ file needs to have the following content (at the minimum). Change placeholders that are formatted like \*\*\*THIS\*\*\* to actual values for your application.

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <Package
       xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
       xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
       xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities">
      <Identity Name="***YOUR_PACKAGE_NAME_HERE***"
        ProcessorArchitecture="x64"
        Publisher="CN=***COMPANY_NAME***, O=***ORGANIZATION_NAME***, L=***CITY***, S=***STATE***, C=***COUNTRY***"
        Version="***YOUR_PACKAGE_VERSION_HERE***" />
      <Properties>
        <DisplayName>***YOUR_PACKAGE_DISPLAY_NAME_HERE***</DisplayName>
        <PublisherDisplayName>Reserved</PublisherDisplayName>
        <Description>No description entered</Description>
        <Logo>***YOUR_PACKAGE_RELATIVE_DISPLAY_LOGO_PATH_HERE***</Logo>
      </Properties>
      <Resources>
        <Resource Language="en-us" />
      </Resources>
      <Dependencies>
        <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14316.0" MaxVersionTested="10.0.14316.0" />
      </Dependencies>
      <Capabilities>
        <rescap:Capability Name="runFullTrust"/>
      </Capabilities>
      <Applications>
        <Application Id="***YOUR_PRAID_HERE***" Executable="***YOUR_PACKAGE_RELATIVE_EXE_PATH_HERE***" EntryPoint="Windows.FullTrustApplication">
          <uap:VisualElements
           BackgroundColor="#464646"
           DisplayName="***YOUR_APP_DISPLAY_NAME_HERE***"
           Square150x150Logo="***YOUR_PACKAGE_RELATIVE_PNG_PATH_HERE***"
           Square44x44Logo="***YOUR_PACKAGE_RELATIVE_PNG_PATH_HERE***"
           Description="***YOUR_APP_DESCRIPTION_HERE***" />
        </Application>
      </Applications>
    </Package>
    ```

## Run the MakeAppX tool

Use the [App packager (MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767(v=vs.85).aspx) to generate an AppX for your project. MakeAppx.exe is included with the Windows 10 SDK. 

To run MakeAppx, first ensure you've created an manifest file as described above. 

Next, create a mapping file. The file should start with **[Files]**, then list each of your source files on disk followed by their destination path in the package. Here's an example: 

```
[Files]
"C:\MyApp\StartPage.htm"     "default.html"
"C:\MyApp\readme.txt"        "doc\readme.txt"
"\\MyServer\path\icon.png"   "icon.png"
"MyCustomManifest.xml"       "AppxManifest.xml"
```

Finally, run the following command: 

```cmd
MakeAppx.exe pack /f mapping_filepath /p filepath.appx
```

## Sign your AppX package

The Add-AppxPackage cmdlet requires that the application package (.appx) being deployed must be signed. Use [SignTool.exe](https://msdn.microsoft.com/library/windows/desktop/aa387764(v=vs.85).aspx), which ships in the Microsoft Windows 10 SDK, to sign the .appx package.

Example usage: 

```cmd
C:\> MakeCert.exe -r -h 0 -n "CN=<publisher_name>" -eku 1.3.6.1.5.5.7.3.3 -pe -sv <my.pvk> <my.cer>
C:\> pvk2pfx.exe -pvk <my.pvk> -spc <my.cer> -pfx <my.pfx>
C:\> signtool.exe sign -f <my.pfx> -fd SHA256 -v .\<outputAppX>.appx
```

When you run MakeCert.exe and you're asked to enter a password, select **none**. For more info on certificates and signing, see the following: 

- [How to: Create Temporary Certificates for Use During Development](https://msdn.microsoft.com/library/ms733813.aspx)

- [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764.aspx)

- [SignTool.exe (Sign Tool)](https://msdn.microsoft.com/library/8s9b9yaz.aspx)




<!--HONumber=Sep16_HO2-->


