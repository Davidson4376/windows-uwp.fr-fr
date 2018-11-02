---
author: laurenhughes
title: Installer un ensemble connexe à l’aide d’un fichier du Programme d’installation d’application
description: Dans cette section, nous allons examiner les étapes à suivre pour autoriser l’installation d’un ensemble connexe via le Programme d’installation d’application. Nous effectuerons également les étapes nécessaires pour créer un fichier *.appinstaller qui définira votre ensemble connexe.
ms.author: lahugh
ms.date: 1/4/2018
ms.topic: article
keywords: windows10, uwp, programme d’installation d’application, appinstaller, charger une version test, ensemble connexe, packages facultatifs
ms.localizationpriority: medium
ms.openlocfilehash: 4caf4333bb3d442779aedac2028b0996cbd17645
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5942866"
---
# <a name="install-a-related-set-using-an-app-installer-file"></a>Installer un ensemble connexe à l’aide d’un fichier du Programme d’installation d’application

Si vous commencez juste à utiliser des packages facultatifs UWP ou des ensembles connexes, les articles suivants constituent de bonnes ressources pour commencer. 

1.  [Étendre votre application à l’aide de packages facultatifs](https://blogs.msdn.microsoft.com/appinstaller/2017/04/05/uwpoptionalpackages/)
2.  [Créer votre premier package facultatif](https://blogs.msdn.microsoft.com/appinstaller/2017/05/09/build-your-first-optional-package/)
3.  [Code de chargement à partir d’un package facultatif](https://blogs.msdn.microsoft.com/appinstaller/2017/05/11/loading-code-from-an-optional-package/)
4.  [Outils pour créer un ensemble connexe](https://blogs.msdn.microsoft.com/appinstaller/2017/05/12/tooling-to-create-a-related-set/)
5.  [Packages facultatifs et création d’ensembles connexes](https://docs.microsoft.com/windows/uwp/packaging/optional-packages)

Avec Windows10FallCreatorsUpdate, les ensembles connexes peuvent désormais être installés via le Programme d’installation d’application. Cela permet la distribution et le déploiement de packages d’application d'ensembles connexes pour les utilisateurs. 

## <a name="how-to-install-a-related-set"></a>Comment installer un ensemble connexe 
Un ensemble connexe n’est pas une entité, mais plutôt une combinaison d’un package principal et de packages facultatifs. Pour être en mesure d’installer un ensemble connexe comme une seule entité, nous devons être en mesure de spécifier le package principal et le package facultatif comme un seul package. Pour ce faire, nous devons créer un fichier XML avec une extension «.appinstaller» pour définir un ensemble connexe. Le Programme d’installation d’application utilise le fichier *.appinstaller et autorise l’utilisateur à installer tous les packages définis d’un seul clic. 

Avant d’entrer dans les détails, voici un exemple complet de fichier *.appinstaller:

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

</AppInstaller>
```

Au cours du déploiement, le fichier du Programme d’installation d’application est validé par rapport aux packages de l’application référencés dans l’élément `Uri`. Par conséquent, `Name`, `Publisher` et `Version` doivent correspondre à l’élément [Package/Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) dans le manifeste du package de l’application. 

## <a name="how-to-create-an-app-installer-file"></a>Comment créer un fichier du Programme d’installation d’application 

Pour distribuer votre ensemble connexe comme une seule entité, vous devez créer un fichier du Programme d’installation d’application qui contient les éléments requis par ce [schéma appinstaller](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file).

### <a name="step-1-create-the-appinstaller-file"></a>Étape1: Créer le fichier *.appinstaller
À l’aide d’un éditeur de texte, créez un fichier (qui contiendra le code XML) et nommez-le &lt;nomdefichier&gt;.appinstaller 

### <a name="step-2-add-the-basic-template"></a>Étape2: Ajouter le modèle de base
Le modèle de base comprend les informations du fichier du Programme d’installation d’application. 
```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
</AppInstaller>
```

### <a name="step-3-add-the-main-package-information"></a>Étape3: Ajouter les informations du package principal 
Si le package principal de l’application est un fichier .appxbundle ou .msixbundle, puis utilisez le `<MainBundle>` illustré ci-dessous. Si le package principal de l’application est un fichier .appx ou .msix, utilisez alors `<MainPackage>` à la place de `<MainBundle>` dans l’extrait de code. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />

</AppInstaller>
```
Les informations contenues dans l’attribut `<MainBundle>` ou `<MainPackage>` doivent correspondre à l’élément [Package/Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) dans le manifeste de l'ensemble d'applications ou dans le manifeste du package de l’application. 

### <a name="step-4-add-the-optional-packages"></a>Étape4: Ajouter les packages facultatifs 
Similaire à l’attribut du package principal de l’application, si le package facultatif peut être soit un package de l’application, soit un ensemble d’applications, l’élément enfant au sein de l’attribut `<OptionalPackages>` doit être `<Package>` ou `<Bundle>` respectivement. Les informations du package dans les éléments enfants doivent correspondre à l’élément d’identité dans le manifeste de l’ensemble d’applications ou du package. 

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

</AppInstaller>
```

### <a name="step-5-add-dependencies"></a>Étape5: Ajouter des dépendances 
Dans l’élément de dépendances, vous pouvez spécifier les packages d’infrastructure requis pour le package principal ou les packages facultatifs. 

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x86"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

    <Dependencies>
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x86" Uri="http://foobarbaz.com/fwkx86.appx" />
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x64" Uri="http://foobarbaz.com/fwkx64.appx" />
    </Dependencies>

</AppInstaller>
```

### <a name="step-6-add-update-setting"></a>Étape6: Ajouter le paramètre de mise à jour 
Le fichier du Programme d'installation d'application permet également de spécifier le paramètre de mise à jour afin que les ensembles connexes puissent être automatiquement mis à jour lorsqu’un fichier du Programme d’installation d’application plus récent est publié. **<UpdateSettings>** est un élément facultatif. Dans **<UpdateSettings>**, l'option OnLaunch spécifie que les vérifications de mise à jour doivent être appliquées au lancement de l'application, et l'élément HoursBetweenUpdateChecks="12" spécifique que la vérification de mise à jour doit être réalisée toutes les 12heures. Si l'élément HoursBetweenUpdateChecks n’est pas spécifié, l’intervalle par défaut spécifie que les mises à jour doivent être vérifiées toutes les 24heures.
``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x86"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

    <Dependencies>
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x86" Uri="http://foobarbaz.com/fwkx86.appx" />
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x64" Uri="http://foobarbaz.com/fwkx64.appx" />
    </Dependencies>
    
    <UpdateSettings>
        <OnLaunch HoursBetweenUpdateChecks="12" />
    </UpdateSettings>

</AppInstaller>
```

Pour obtenir tous les détails sur le schéma XML, voir [Informations de référence sur le fichier du Programme d’installation d’application](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file).

> [!NOTE]
> 
> Le type du fichier du Programme d’installation d’application est nouveau dans Windows10 FallCreatorsUpdate. Il n’existe aucune prise en charge pour le déploiement des applicationsUWP utilisant un fichier du Programme d’installation d’application sur les versions précédentes de Windows10.
> Il faut également noter que **HoursBetweenUpdateChecks** est un nouvel élément de la mise à jour majeure suivante de Windows10.
