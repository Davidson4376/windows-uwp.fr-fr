---
author: normesta
Description: Shows how to manually package a Windows desktop application (like Win32, WPF, and Windows Forms) for Windows 10.
Search.Product: eADQiWindows 10XVcnh
title: Créer un package d'application manuellement (Pont du bureau)
ms.author: normesta
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: e8c2a803-9803-47c5-b117-73c4af52c5b6
ms.localizationpriority: medium
ms.openlocfilehash: 81d5b9b0b52ef0f7529b277215e7fe0b95683f0a
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
ms.locfileid: "1689765"
---
# <a name="package-an-app-manually-desktop-bridge"></a>Créer un package d'application manuellement (Pont du bureau)

Cette rubrique vous explique comment créer un package de votre application sans l’aide d’outils tels que Visual Studio ou Desktop App Converter (DAC).

Pour créer manuellement un package de votre application, créez un fichier manifeste de package, puis exécutez un outil en ligne de commande pour générer un package d’application Windows.

Envisagez la création de package manuelle si vous installez votre application à l’aide de la commande xcopy ou si vous êtes familiarisé avec les modifications apportées au système par le programme d’installation de votre application et souhaitez un contrôle plus précis sur le processus.

Si vous n’êtes pas certain des modifications apportées au système par votre programme d’installation, ou si vous préférez utiliser des outils automatisés pour générer votre manifeste de package, envisager l’une de [ces](desktop-to-uwp-root.md#convert) options.

>[!IMPORTANT]
>Le Pont du bureau a été introduit dans Windows10, version1607 et peut être utilisé uniquement dans les projets qui ciblent la Mise à jour anniversaire Windows10 (version10.0; build 14393) ou une version ultérieure dans Visual Studio.

## <a name="first-consider-how-youll-distribute-your-app"></a>Commencez par examiner la méthode de distribution de votre application
Si vous prévoyez de publier votre application sur le [MicrosoftStore](https://www.microsoft.com/store/apps), commencez par remplir [ce formulaire](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge). Microsoft vous contactera pour démarrer le processus d’intégration. Dans le cadre de ce processus, vous devez réserver un nom dans le Store et obtenir les informations nécessaires pour créer le package de votre application.

## <a name="create-a-package-manifest"></a>Créer un manifeste de package

Créez un fichier, nommez-le **appxmanifest.xml**, puis ajoutez-lui ce fichier XML.

Il s’agit d’un modèle de base qui contient les éléments et attributs nécessaires pour votre package. Nous allons ajouter des valeurs à celles de la section suivante.

```XML
<?xml version="1.0" encoding="utf-8"?>
<Package
    xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities">
  <Identity Name="" Version="" Publisher="" ProcessorArchitecture="" />
    <Properties>
       <DisplayName></DisplayName>
       <PublisherDisplayName></PublisherDisplayName>
             <Description></Description>
      <Logo></Logo>
    </Properties>
    <Resources>
      <Resource Language="" />
    </Resources>
      <Dependencies>
      <TargetDeviceFamily Name="Windows.Desktop" MinVersion="" MaxVersionTested="" />
      </Dependencies>
      <Capabilities>
        <rescap:Capability Name="runFullTrust"/>
      </Capabilities>
    <Applications>
      <Application Id="" Executable="" EntryPoint="Windows.FullTrustApplication">
        <uap:VisualElements DisplayName="" Description=""   Square150x150Logo=""
                   Square44x44Logo=""   BackgroundColor="" />
      </Application>
     </Applications>
  </Package>
```

## <a name="fill-in-the-package-level-elements-of-your-file"></a>Renseignez les éléments de package de votre fichier

Remplissez ce modèle avec les informations décrivant votre package.

### <a name="identity-information"></a>Informations d’identité

Voici un exemple d’élément **identité** intégrant un emplacement réservé pour les attributs. Vous pouvez définir l’attribut ``ProcessorArchitecture`` sur ``x64`` ou ``x86``.

```XML
<Identity Name="MyCompany.MySuite.MyApp"
          Version="1.0.0.0"
          Publisher="CN=MyCompany, O=MyCompany, L=MyCity, S=MyState, C=MyCountry"
                ProcessorArchitecture="x64">
```
> [!NOTE]
> Si vous avez réservé votre nom d’application dans le Store, vous pouvez obtenir le nom et l’éditeur à l’aide du tableau de bord du centre de développement de Windows. Si vous prévoyez de charger votre application de manière indépendante sur d’autres systèmes, vous pouvez fournir vos propres noms tant que le nom de l’éditeur que vous choisissez correspond au nom indiqué sur le certificat utilisé pour signer votre application.

### <a name="properties"></a>Propriétés

L’élément [propriétés](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-properties) comprend 3éléments enfants requis. Voici un exemple de nœud **propriétés** avec texte d’emplacement réservé pour les éléments. Pour les applications téléchargées dans le Store **DisplayName** est le nom de votre application, qui peut être réservé dans le Store.

```XML
<Properties>
  <DisplayName>MyApp</DisplayName>
  <PublisherDisplayName>MyCompany</PublisherDisplayName>
  <Logo>images\icon.png</Logo>
</Properties>
```

### <a name="resources"></a>Ressources

Voici un exemple de nœud [Ressources](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-resources).

```XML
<Resources>
  <Resource Language="en-us" />
</Resources>
```
### <a name="dependencies"></a>Dépendances

Pour les applications Pont du bureau, réglez toujours l’attribut ``Name`` sur ``Windows.Desktop``.

```XML
<Dependencies>
<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14316.0" MaxVersionTested="10.0.15063.0" />
</Dependencies>
```

### <a name="capabilities"></a>Fonctionnalités
Pour les applications Pont du bureau, vous devrez ajouter la fonctionnalité ``runFullTrust``.

```XML
<Capabilities>
  <rescap:Capability Name="runFullTrust"/>
</Capabilities>
```
## <a name="fill-in-the-application-level-elements"></a>Renseignez les éléments au niveau de l’application

Remplissez ce modèle avec les informations décrivant votre application.

### <a name="application-element"></a>Élément d’application

Pour les applications Pont du bureau, l’attribut ``EntryPoint`` de l’élément d’application est toujours ``Windows.FullTrustApplication``.

```XML
<Applications>
  <Application Id="MyApp"     
        Executable="MyApp.exe" EntryPoint="Windows.FullTrustApplication">
   </Application>
</Applications>
```

### <a name="visual-elements"></a>Éléments visuels

Voici un exemple de nœud [VisualElements](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements).

```XML
<uap:VisualElements
    BackgroundColor="#464646"
    DisplayName="My App"
    Square150x150Logo="images\icon.png"
    Square44x44Logo="images\small_icon.png"
    Description="A useful description" />
```
<a id="target-based-assets" />

## <a name="optional-add-target-based-unplated-assets"></a>(Facultatif) Ajoutez des ressources sans plaque basées sur la cible

Les ressources basées sur la cible sont destinées aux icônes et vignettes qui apparaissent dans la barre des tâches Windows, dans les applications actives, dans l’affichage Alt+Tab, dans l’alignement automatique et dans le coin inférieur droit des vignettes de démarrage. Vous trouverez davantage de détails [ici](https://docs.microsoft.com/windows/uwp/shell/tiles-and-notifications/app-assets#target-based-assets).

1. Obtenez les images 44x44 correctes, puis copiez-les dans le dossier qui contient vos images (c’est-à-dire Assets).

2. Pour chaque image 44x44, créez une copie dans le même dossier et ajoutez **.targetsize-44_altform-unplated** à la fin du nom de fichier. Vous devez avoir deuxcopies de chaque icône, chacune ayant un nom propre. Par exemple, à la fin du processus, votre dossier Assets peut contenir **MYAPP_44x44.png** et **myapp_44x44.targetSize-44_altform-unplated.png**.

   > [!NOTE]
   > Dans cet exemple, l’icône nommée **MYAPP_44x44.png** est celle que vous devez référencer dans l’attribut logo ``Square44x44Logo`` de votre package d’application Windows.

3.  Dans le package d’application Windows, définissez la ``BackgroundColor`` pour chaque icône que vous rendez transparente.

4. Passez à la sous-section suivante pour générer un nouveau fichier d’Index de ressource de package.

<a id="make-pri" />

### <a name="generate-a-package-resource-index-pri-file"></a>Générer un fichier d’Index de ressource de package (IRP)

Si vous créez des ressources basées sur une cible comme décrit dans la section ci-dessus ou si vous modifiez les éléments visuels de votre application après avoir créé le package, vous devrez générer un nouveau fichier PRI.

1.  Ouvrez une **invite de commandes de développeur Visual Studio2017**.

    ![invite de commandes de développeur](images/desktop-to-uwp/developer-command-prompt.png)

2.  Accédez au répertoire racine du package et créez un fichier priconfig.xml en exécutant la commande ``makepri createconfig /cf priconfig.xml /dq en-US``.

5.  Créez les fichiers resources.pri à l’aide de la commande ``makepri new /pr <PHYSICAL_PATH_TO_FOLDER> /cf <PHYSICAL_PATH_TO_FOLDER>\priconfig.xml``.

    Par exemple, la commande de votre application peut ressembler à ceci: ``makepri new /pr c:\MYAPP /cf c:\MYAPP\priconfig.xml``.

6.  Créez un package d’application Windows en suivant les instructions de l’étape suivante.

<a id="make-appx" />

## <a name="generate-a-windows-app-package"></a>Générer un package d’application Windows

Utilisez l’outil **MakeAppx.exe** pour générer un package d’application Windows pour votre projet. Il est inclus avec le SDK Windows10 et, si vous avez installé Visual Studio, il peut être facilement accessible par le biais de l’invite de commandes de développeur de votre version de Visual Studio.

Voir [Créer un package d’application avec l’outil MakeAppx.exe](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)

## <a name="run-the-packaged-app"></a>Exécuter l’application empaquetée

Vous pouvez exécuter votre application pour la tester localement sans certificat et la signer. Exécutez simplement l’applet de commande PowerShell:

```Add-AppxPackage –Register AppxManifest.xml```

Pour mettre à jour les fichiers .exe ou .dll de votre application, remplacez les fichiers existants dans votre package par les nouveaux, augmentez le nombre de versions dans AppxManifest.xml, puis exécutez à nouveau la commande ci-dessus.

> [!NOTE]
> Une application empaquetée s’exécute toujours en tant qu’utilisateur interactif, et le lecteur sur lequel vous installez votre application empaquetée (quel qu’il soit) doit être formaté en NTFS.

## <a name="next-steps"></a>Étapes suivantes

**Trouvez des réponses à vos questions**

Des questions? Contactez-nous sur Stack Overflow. Notre équipe supervise ces [balises](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Transmettre des commentaires ou suggérer des fonctionnalités**

Consultez [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Parcourir le code/détecter et résoudre les problèmes**

Voir [Exécuter, déboguer et tester une application de bureau empaquetée (Pont du bureau)](desktop-to-uwp-debug.md)

**Signer votre application, puis la distribuer**

Consultez [Distribuer une application de bureau mise en package (Pont du bureau)](desktop-to-uwp-distribute.md)
