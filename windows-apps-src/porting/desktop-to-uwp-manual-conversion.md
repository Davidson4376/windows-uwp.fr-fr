---
author: awkoren
Description: Montre comment convertir manuellement une application de bureau Windows (Win32, WPF, Windows Forms) en une application de plateforme Windows universelle (UWP).
Search.Product: eADQiWindows 10XVcnh
title: Convertir manuellement une application de bureau Windows en application UWP
translationtype: Human Translation
ms.sourcegitcommit: b612b2c94de79f48a375ae3469c35dee6ce3939d
ms.openlocfilehash: 73f30d564fcec1b748b4d59ff545e25b62b1c719

---

# <a name="manually-convert-your-app-to-uwp-using-the-desktop-bridge"></a>Convertir manuellement votre application en application UWP à l’aide de Desktop Bridge

L’utilisation de [Desktop App Converter (DAC)](desktop-to-uwp-run-desktop-app-converter.md) est pratique et automatique. Ce module est aussi très utile en cas d’incertitude sur les opérations effectuées par votre programme d’installation. Mais si votre application est installée avec xcopy ou si vous connaissez les modifications apportées au système par le programme d’installation de votre application, vous pouvez créer manuellement un package et un manifeste d’application. Cet article décrit la procédure de prise en main. Il explique également comment ajouter des ressources sans plaque à votre application, ce que le DAC n’effectue pas. 

Voici comment faire :

## <a name="create-a-manifest-by-hand"></a>Créer un manifeste manuellement

Votre fichier _appxmanifest.xml_ doit présenter au moins le contenu suivant. Remplacez les espaces réservés de type \*\*\*CECI\*\*\* par les valeurs réelles de votre application.

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

Vous souhaitez ajouter des ressources sans plaque ? Pour savoir comment faire, consultez la section sur les [ressources sans plaque](#unplated-assets), plus loin dans cet article.

## <a name="run-the-makeappx-tool"></a>Exécuter l’outil MakeAppX

Utilisez l’[outil de création de package de l’application (MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767(v=vs.85).aspx) pour générer un AppX pour votre projet. MakeAppx.exe est inclus dans le SDK Windows 10. 

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

## <a name="sign-your-appx-package"></a>Signer votre package AppX

L’applet de commande Add-AppxPackage nécessite que le package d’application (.appx) déployé soit signé. Utilisez [SignTool.exe](https://msdn.microsoft.com/library/windows/desktop/aa387764(v=vs.85).aspx), fourni dans le SDK Microsoft Windows 10 pour signer le package .appx.

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

<span id="unplated-assets" />
## <a name="add-unplated-assets"></a>Ajouter des ressources sans plaque

Si vous le souhaitez, configurez les ressources 44 x 44 de votre application qui s’affichent sur la barre des tâches. Voici comment faire : 

1. Obtenez les images 44 x 44 correctes et copiez-les dans le dossier qui contient vos images (c’est-à-dire Assets).

2. Pour chaque image 44 x 44, créez une copie dans le même dossier et ajoutez *.targetsize-44_altform-unplated* à la fin du nom de fichier. Vous devez avoir deux copies de chaque icône, chacune ayant un nom propre. Par exemple, à l’issue du processus, votre dossier Assets peut contenir *MYAPP_44x44.png* et *MYAPP_44x44.targetsize-44_altform-unplated.png* (Remarque : la première est l’icône référencée dans le fichier appxmanifest sous l’attribut VisualElements *Square44x44Logo*). 

3.  Dans AppXManifest, définissez le paramètre BackgroundColor sur transparent pour chaque icône que vous corrigez. Cet attribut se trouve sous VisualElements pour chaque application.

4.  Ouvrez CMD, accédez au répertoire racine du package et créez un fichier priconfig.xml en exécutant la commande ```makepri createconfig /cf priconfig.xml /dq en-US```.

5.  À l’aide de CMD, dans le dossier racine du package, créez le ou les fichiers resources.pri avec la commande ```makepri new /pr <PHYSICAL_PATH_TO_FOLDER> /cf <PHYSICAL_PATH_TO_FOLDER>\priconfig.xml```. Par exemple, la commande de votre application peut ressembler à : ```makepri new /pr c:\MYAPP /cf c:\MYAPP\priconfig.xml```. 

6.  Créez un package de votre application AppX en suivant les instructions de l’étape suivante pour afficher les résultats.




<!--HONumber=Dec16_HO1-->


