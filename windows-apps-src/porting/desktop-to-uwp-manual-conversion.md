---
Description: Montre comment convertir manuellement une application de bureau Windows (Win32, WPF, Windows Forms) en une application de plateforme Windows universelle (UWP).
Search.Product: eADQiWindows 10XVcnh
title: Convertir manuellement une application de bureau Windows en appli UWP
---

# Convertir manuellement votre application de bureau Windows en une application de plateforme Windows universelle (UWP)

\[Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.\]

L’utilisation du convertisseur est pratique et automatique ; celui-ci est très utile en cas d’incertitude sur ce que fait votre programme d’installation. Toutefois, si votre application est installée à l’aide de xcopy, ou si vous connaissez les modifications que le programme d’installation de votre application apporte au système, vous pouvez choisir de créer manuellement un package d’application et un manifeste.

Voici la procédure pour créer un package Centennial manuellement :

## Créez un manifeste manuellement.

Votre fichier _appxmanifest.xml_ doit présenter le contenu suivant (au minimum). Modifiez les espaces réservés mis en forme comme \*\*\*CECI\*\*\* en valeurs réelles pour votre application.

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

## Exécutez l’outil MakeAppX.

Utilisez l’[outil de création de package de l’application (MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767(v=vs.85).aspx) pour générer un AppX pour votre projet. MakeAppx.exe est inclus dans le SDK Windows 10. 

Pour exécuter MakeAppx, assurez-vous d’abord que vous avez créé un fichier manifeste, tel que décrit ci-dessus. 

Ensuite, créez un fichier de mappage. Le fichier doit commencer par **[Files]**, puis répertorier chacun de vos fichiers sources sur le disque, suivis de leur chemin de destination dans le package. Voici un exemple : 

```
[Files]
"C:\MyApp\StartPage.htm"     "default.html"
"C:\MyApp\readme.txt"        "doc\readme.txt"
"\\MyServer\path\icon.png"   "icon.png"
"MyCustomManifest.xml"       "AppxManifest.xml"
```

Enfin, exécutez la commande suivante : 

```cmd
MakeAppx.exe pack /f mapping_filepath /p filepath.appx
```

## Signer votre package AppX

L’applet de commande Add-AppxPackage nécessite que le package d’application (.appx) déployé soit signé. Utilisez [SignTool.exe](https://msdn.microsoft.com/library/windows/desktop/aa387764(v=vs.85).aspx), fourni dans le SDK Microsoft Windows 10 pour signer le package .appx.

Exemple d’utilisation : 

```cmd
C:\> MakeCert.exe -r -h 0 -n "CN=<publisher_name>" -eku 1.3.6.1.5.5.7.3.3 -pe -sv <my.pvk> <my.cer>
C:\> pvk2pfx.exe -pvk <my.pvk> -spc <my.cer> -pfx <my.pfx>
C:\> signtool.exe sign -f <my.pfx> -fd SHA256 -v .\<outputAppX>.appx
```

Lorsque vous exécutez MakeCert.exe et que vous êtes invité à entrer un mot de passe, sélectionnez **Aucun**. Pour plus d’informations sur les certificats et la signature, consultez les rubriques suivantes : 

- [Procédure : Créer des certificats temporaires à utiliser pendant le développement](https://msdn.microsoft.com/library/ms733813.aspx)

- [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764.aspx)

- [SignTool.exe (Sign Tool)](https://msdn.microsoft.com/library/8s9b9yaz.aspx)

<!--HONumber=May16_HO2-->


