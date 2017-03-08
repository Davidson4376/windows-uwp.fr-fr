---
author: awkoren
Description: "Ce guide vous explique comment configurer votre solution Visual Studio pour modifier, déboguer et empaqueter votre application Pont du bureau convertie."
Search.Product: eADQiWindows 10XVcnh
title: "Guide d’empaquetage Pont du bureau pour les applications de bureau .NET avec Visual Studio"
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 8aa68312d6ce81c809c79ddcafe7732944a628be
ms.lasthandoff: 02/08/2017

---

# <a name="desktop-bridge-packaging-guide-for-net-desktop-apps-with-visual-studio"></a>Guide d’empaquetage Pont du bureau pour les applications de bureau .NET avec Visual Studio

La Mise à jour anniversaire Windows 10 permet aux développeurs d’utiliser le kit de ressources Pont du bureau pour empaqueter des applications Win32 existantes à l’aide du nouveau modèle de package (.appx), ce qui facilite leur publication dans le Windows Store ou leur chargement indépendant. Ce guide vous explique comment configurer votre solution Visual Studio pour modifier, déboguer et empaqueter votre application. 

Pour commencer, renseignez le formulaire de la page [Transférer vos applications et jeux existants vers le Windows Store avec Pont du bureau](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge). Microsoft vous contactera pour démarrer le processus d’intégration. Une fois que votre compte a été autorisé à soumettre des applications Pont du bureau, suivez les instructions de ce document pour préparer le chargement de votre package appxupload. 

> Vous avez des commentaires ou vous rencontrez des problèmes liés à Pont du bureau ? Le meilleur endroit pour nous soumettre vos suggestions de fonctionnalités est le site [Windows Developer UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial). Si vous souhaitez nous faire part de vos questions et de vos rapports de bogues, accédez aux [forums consacrés au développement d’applications Windows universelles](https://social.msdn.microsoft.com/Forums/home?forum=wpdevelop).

## <a name="default-universal-windows-platform-packages"></a>Packages de plateforme Windows universelle par défaut

Visual Studio vous permet de générer les packages de débogage et de version qui peuvent être distribués par le biais du Windows Store ou d’un chargement indépendant des applications. Pour faciliter la création de packages, Visual Studio vous aide à créer un fichier .appxupload pouvant être soumis tel quel au Windows Store. Pour plus d’informations, consultez l’article [Création de packages d’application UWP](..\packaging\packaging-uwp-apps.md).

## <a name="desktop-bridge-packages"></a>Packages Pont du bureau

Le kit de ressources [Pont du bureau](desktop-to-uwp-root.md) autorise différentes configurations pour intégrer des fichiers binaires Win32 au package d’application (appx). L’utilisation de Pont du bureau s’effectue sous la forme d’une procédure comportant quatre grandes étapes. 

- **Étape 1 - Conversion** : empaquetez les fichiers binaires Win32 existants qui n’impliquent aucune modification de code ou seulement des modifications minimes.
- **Étape 2 - Amélioration** : insérez certaines fonctionnalités UWP de base (telles qu’une vignette dynamique) dans l’application existante en référençant Windows.winmd à partir du code Win32 existant.
- **Étape 3 - Extension** : dotez l’application existante de fonctionnalités UWP avancées (comme les tâches en arrière-plan). Si vos composants UWP et Win32 sont générés à l’aide de langages managés (tels que C# ou VB.Net), le package résultant comportera des fichiers binaires mixtes qui doivent être minutieusement traités pour garantir le traitement .NET Native approprié. 
- **Étape 4 - Migration** : vous avez effectué la migration de votre interface utilisateur vers les langages modernes XAML et C#/VB.NET, mais vous disposez encore de code Win32 hérité. Le point d’entrée est désormais un exécutable .NET UWP, mais votre package comporte encore des fichiers binaires qui utilisent certaines API Win32.

Le tableau ci-après récapitule certaines des différences pour votre application dans le cadre de chacune des quatre étapes. 

| Étape | Fichiers binaires | Point d’entrée | .NET Native | Débogage F5 |
|---|---|---|---|---|
| 1 (Conversion) | Win32 | Win32 | Non applicable | Extension VS |
| 2 (Amélioration) | Refs WinMD | Win32 | Non applicable | Extension VS |
| 3 (Extension) | Win32 + CoreCLR (*) | Win32 | Par l’utilisateur (**) | Extension VS |
| 4 (Migration)    | CoreCLR (*) + Win32 | UWP | Par l’utilisateur (**) | VS |
| 5 (UWP) | CoreCLR | UWP |Par le Windows Store | VS |

(*) [CoreCLR](https://github.com/dotnet/coreclr) fait référence au runtime .NET Core sur lequel s’appuient les composants UWP écrits dans un langage managé (C#/VB.NET). Ces composants nécessiteront également un traitement .NET Native.

(**) Dans le cadre des étapes 3 et 4, l’utilisateur doit traiter les assemblies CoreCLR pour produire les fichiers binaires .NET Native et les symboles correspondants avant de procéder à la publication dans le Windows Store.

## <a name="configure-your-visual-studio-solution"></a>Configurer votre solution Visual Studio

Visual Studio intègre les outils dont vous avez besoin pour configurer votre package d’application, tels que l’éditeur de manifeste et l’Assistant Création de package. Pour utiliser ces outils, vous devez disposer d’un projet UWP qui fera office de conteneur appx pour votre application. Bien que vous puissiez utiliser n’importe quel projet UWP (tel que C#, VB.NET, C++ ou JavaScript), il existe certains problèmes connus inhérents aux projets C#, VB.NET et C++ (voir la section [Problèmes connus](#known-issues-anchor) dans la suite de ce document). Nous utiliserons donc JavaScript pour cet exemple. 

Si vous souhaitez déboguer votre application dans le contexte du modèle d’application appx, vous devrez ajouter un autre projet qui activera le débogage F5 du modèle appx. Pour plus d’informations, consultez la section [Débogage de votre application Pont du bureau](#debugging-anchor).

Commençons par l’étape 1 de la procédure.

### <a name="step-1-convert"></a>Étape 1 : Conversion

Cette étape indique comment créer une application Pont du bureau à partir d’un projet Win32 existant. Dans cet exemple, nous allons utiliser un projet WinForms élémentaire qui effectue des opérations de lecture et d’écriture dans le Registre.

![](images/desktop-to-uwp/net-1.png)

#### <a name="add-the-uwp-project"></a>Ajouter le projet UWP 

Pour créer le package Pont du bureau, ajoutez un projet UWP en JavaScript à la même solution.

> Remarque : bien que nous utilisions un modèle UWP en JavaScript, nous n’allons écrire aucun code JavaScript. Nous nous servons uniquement de notre projet comme d’un outil.

![](images/desktop-to-uwp/net-2.png)

#### <a name="add-the-win32-binaries-to-the-win32-folder"></a>Ajouter les fichiers binaires Win32 au dossier win32

Tous les fichiers binaires Win32 seront stockés dans votre projet UWP dans un dossier nommé win32 (vous pouvez utiliser un autre nom de dossier si vous le souhaitez).

Si vous utilisez Visual Studio, vous pouvez améliorer votre workflow de développement en automatisant le projet pour qu’il copie les fichiers après chaque génération. Modifiez votre fichier projet (.csproj dans cet exemple) en y insérant une cible AfterBuild qui copiera tous les fichiers de sortie Win32 dans le dossier win32 du projet UWP, comme suit : 

```xml
  <Target Name="AfterBuild">
    <PropertyGroup>
      <TargetUWP>..\MyDesktopApp.Package\win32\</TargetUWP>
    </PropertyGroup>
     <ItemGroup>
       <Win32Binaries Include="$(TargetDir)\*" />
     </ItemGroup>
    <Copy SourceFiles="@(Win32Binaries)" DestinationFolder="$(TargetUWP)" />
  </Target>
```

Si vous utilisez un autre outil pour générer vos fichiers binaires Win32, il vous suffit de copier dans le dossier win32 tous les fichiers requis au moment de l’exécution. 

#### <a name="edit-the-app-manifest-to-enable-the-desktop-bridge-extensions"></a>Modifier le manifeste de l’application pour activer les extensions Pont du bureau

Ce modèle intègre un fichier package.appxmanifest que vous pouvez utiliser pour ajouter les extensions Pont du bureau. Pour modifier ce fichier, cliquez avec le bouton droit et sélectionnez « Afficher le code », puis ajoutez ou modifiez les éléments suivants : 

- `<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10" xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10" xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities" IgnorableNamespaces="uap rescap">`

- `<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.0" MaxVersionTested="10.0.14393.0" />`

- `<rescap:Capability Name="runFullTrust" />`

- `<Application Id="MyDesktopAppStep1" Executable="win32\MyDesktopApp.exe" EntryPoint="Windows.FullTrustApplication">`

Voici un exemple complet de fichier manifeste : 

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
        xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"

        xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
        xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
        IgnorableNamespaces="uap rescap mp">
  <Identity Name="MyDesktopAppStep1"
            ProcessorArchitecture="x64"
            Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US"
            Version="1.0.0.5" />
  <mp:PhoneIdentity PhoneProductId="6f6600a4-6da1-4d91-b493-35808d01f8de" PhonePublisherId="00000000-0000-0000-0000-000000000000" />
  <Properties>
    <DisplayName>MyDesktopAppStep1</DisplayName>
    <PublisherDisplayName>CN=Test</PublisherDisplayName>
    <Logo>Assets\SampleAppx.150x150.png</Logo>
  </Properties>
  <Resources>
    <Resource Language="en-us" />
  </Resources>
  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" 
                        MinVersion="10.0.14393.0" 
                        MaxVersionTested="10.0.14393.0" />
  </Dependencies>
  <Capabilities>
    <rescap:Capability Name="runFullTrust" />
  </Capabilities>
  <Applications>
    <Application Id="MyDesktopAppStep1" 
                 Executable="win32\MyDesktopApp.exe" 
                 EntryPoint="Windows.FullTrustApplication">
      <uap:VisualElements DisplayName="MyDesktopAppStep1" 
                          Description="MyDesktopAppStep1" 
                          BackgroundColor="#777777" 
                          Square150x150Logo="Assets\SampleAppx.150x150.png" 
                          Square44x44Logo="Assets\SampleAppx.44x44.png">
      </uap:VisualElements>
    </Application>
  </Applications>
</Package>
```

#### <a name="configure-the-win32-binaries"></a>Configurer les fichiers binaires Win32

Pour inclure les fichiers binaires requis par votre application dans le package de sortie, sélectionnez chaque fichier dans Visual Studio. Définissez ses propriétés sur « Contenu », et son comportement de génération sur « Copier si plus récent ». 

![](images/desktop-to-uwp/net-3.png)

Si vous ne souhaitez pas valider les fichiers binaires dans votre référentiel de code source, vous pouvez utiliser le fichier .gitignore afin d’exclure tous ces fichiers dans le dossier win32. 

#### <a name="optional-use-wildcards-to-specify-the-files-in-your-win32-folder"></a>Facultatif : utiliser des caractères génériques pour spécifier les fichiers figurant dans votre dossier win32

Si votre application Win32 nécessite plusieurs fichiers, vous pouvez modifier votre fichier projet en y insérant un caractère générique afin de spécifier les fichiers qui doivent être marqués en tant que « Contenu » en fonction d’une expression générique. Vous devez ouvrir le fichier .jsproj à l’aide d’un éditeur de texte et y inclure les fichiers dont vous avez besoin en procédant comme suit :

```xml
<Content Include="win32\*.dll">
  <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</Content>
<Content Include="win32\*.exe">
  <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</Content>
<Content Include="win32\*.config">
  <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</Content>
<Content Include="win32\*.pdb">
  <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</Content>
```

### <a name="step-2-enhance"></a>Étape 2 : Amélioration

Si vous voulez appeler les API UWP disponibles à partir de votre code Win32, vous devez ajouter une référence à `\Program Files (x86)\Windows Kits\10\UnionMetadata\Windows.winmd`. La liste complète des API UWP disponibles pour votre application est présentée dans l’article [API UWP prises en charge pour les applications converties avec Pont du bureau](desktop-to-uwp-supported-api.md).  

Ce fichier n’étant pas requis dans Windows 10, vous n’avez pas besoin de le distribuer. Dans les propriétés de référence, définissez la propriété « Copie locale » sur la valeur false.

![](images/desktop-to-uwp/net-4.png)

Pour ajouter les fichiers binaires Win32, suivez les mêmes instructions que dans l’étape 1. 

### <a name="step-3-extend"></a>Étape 3 : Extension 

Pour cet exemple, nous allons étendre une application Win32 avec une tâche en arrière-plan. Cette opération nécessite l’enregistrement de la tâche en arrière-plan dans le fichier package.appxmanifest de l’application UWP et l’ajout d’une référence au projet implémentant la tâche en arrière-plan, comme indiqué ci-après.

```xml
<Extensions>
  <Extension Category="windows.backgroundTasks" 
              EntryPoint="BackgroundTasks.MyBackgroundTask">
    <BackgroundTasks>
      <Task Type="timer" />
    </BackgroundTasks>
  </Extension>
</Extensions>
```

Si la tâche en arrière-plan est implémentée avec C# ou VB.NET, la sortie résultante contiendra des fichiers binaires CoreCLR qui doivent être traités par la chaîne d’outils .NET Native avant d’être soumis au Windows Store, comme décrit aux étapes 3 et 4. Créez le fichier appxupload avec des fichiers binaires mixtes.

### <a name="step-4-migrate"></a>Étape 4 : Migration

Ce scénario comporte déjà un point d’entrée UWP en C#. Il n’est donc pas nécessaire d’ajouter un autre projet UWP. Toutefois, vous devez suivre la procédure décrite à l’étape 1 pour inclure et configurer les fichiers binaires Win32.

Pour exécuter le processus Win32, utilisez les API [**FullTrustProcessLauncher**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.FullTrustProcessLauncher). Vous devrez ajouter l’extension de bureau et la fonctionnalité *fullTrustProcess* au manifeste de votre application pour utiliser les API, en procédant comme suit : 

```xml
..
xmlns:desktop=http://schemas.microsoft.com/appx/manifest/desktop/windows10
..
<desktop:Extension Category="windows.fullTrustProcess" 
                    Executable="win32\MyDesktopApp.exe" />
```

## <a name="generate-packages-for-your-desktop-bridge-app"></a>Générer des packages pour votre application Pont du bureau

Une fois que vous avez suivi les instructions ci-dessus, vous êtes en mesure de générer vos packages à l’aide de Visual Studio, comme décrit dans l’article [Création de packages d’application UWP](..\packaging\packaging-uwp-apps.md). 

### <a name="steps-1-and-2-create-appxupload-with-win32-binaries"></a>Étapes 1 et 2 : Créer un fichier appxupload avec des fichiers binaires Win32

Pour soumettre des packages avec la fonctionnalité *fullTrust*, vous devez générer un fichier appxupload incluant les symboles pour chaque plateforme dans un fichier appxsym, ainsi qu’un ensemble d’applications contenant les packages de plateforme appx.

Aux étapes 1 et 2, votre package ne contient aucun fichier binaire CoreCLR. Vous n’avez donc pas à vous soucier de la plateforme à choisir. Sélectionnez « Neutre » et « Version finale (tout processeur) », comme illustré ci-dessous.

![](images/desktop-to-uwp/net-5.png)

Une fois que vous sélectionnerez l’option « Générer les packages pour le Windows Store », l’Assistant générera le fichier appxupload pouvant être soumis au Windows Store.

### <a name="step-3-and-4-create-appxupload-with-mixed-binaries"></a>Étapes 3 et 4 : Créer un fichier appxupload avec des fichiers binaires mixtes

Il est également recommandé de générer le fichier pour la configuration Version finale. Dans ce cas, vous devez spécifier les plateformes à cibler, car .NET Native doit produire les fichiers binaires natifs pour chaque plateforme.

![](images/desktop-to-uwp/net-6.png)

Pour générer le fichier appxupload, nous allons créer une archive ZIP incluant les fichiers appxsym et appxbundle générés du dossier _Test.

Créez un fichier ZIP contenant les fichiers appxsym et appxbundle, puis renommez son extension sous la forme appxupload.

![](images/desktop-to-uwp/net-7.png)

<span id="debugging-anchor" />
## <a name="debugging-your-desktop-bridge-app"></a>Débogage de votre application Pont du bureau

Bien que vous puissiez démarrer vos projets à partir de Visual Studio sans effectuer de débogage (Ctrl + F5), un problème connu empêche Visual Studio de se rattacher automatiquement au processus en cours d’exécution. Toutefois, vous pourrez le rattacher par la suite à l’aide de l’une des méthodes suivantes :

### <a name="attach-to-the-running-app"></a>Se rattacher à l’application en cours d’exécution

#### <a name="attach-to-an-existing-process"></a>Se rattacher à un processus existant

Une fois que vous aurez lancé votre application à l’aide de Ctrl + F5, vous pourrez vous rattacher à votre processus Win32 ; toutefois, vous ne serez pas en mesure de déboguer les modules .NET Native. 

![](images/desktop-to-uwp/net-8.png)

#### <a name="attach-to-an-installed-app"></a>Se rattacher à une application installée

Vous pouvez également vous rattacher à tout package Appx existant en sélectionnant l’option Déboguer -> Autres cibles de débogage -> Déboguer le package d’application installé.

![](images/desktop-to-uwp/net-9.png)

Vous pouvez sélectionner votre ordinateur local ou vous connecter à un ordinateur distant.

![](images/desktop-to-uwp/net-10.png)

Cette option devrait vous permettre de déboguer du code .NET Native.

### <a name="use-visual-studio-extension-to-debug-your-desktop-bridge-app"></a>Utiliser l’extension Visual Studio pour déboguer votre application Pont du bureau 

Si vous préférez procéder au débogage de votre application en utilisant F5, vous devez installer l’extension Visual Studio 2017 [Projet de débogage Pont du bureau](https://marketplace.visualstudio.com/items?itemName=VisualStudioProductTeam.DesktoptoUWPPackagingProject) à partir de la galerie Visual Studio.

Ce projet vous permet de déboguer n’importe quelle application Win32 ayant fait l’objet d’une migration vers UWP à l’aide de Visual Studio (comme décrit dans ce document) ou au moyen de Desktop App Converter.

#### <a name="add-the-debugging-project-to-your-solution"></a>Ajouter le projet de débogage à votre solution

Pour commencer, ajoutez un nouveau Projet de débogage Pont du bureau à votre solution.

![](images/desktop-to-uwp/net-11.png)

Pour configurer ce projet, vous devez définir la propriété PackageLayout dans la fenêtre des propriétés pour chaque configuration/plateforme que vous souhaitez utiliser pour le débogage.
Pour sélectionner la configuration de solution Débogage/x86, nous allons définir la propriété de disposition de package sur le dossier bin\x86\debug du projet UWP à l’aide d’un chemin d’accès relatif : `..\MyDesktopApp.Package\bin\x86\Debug`. 

![](images/desktop-to-uwp/net-12.png)

Puis modifiez le fichier AppXFileLayout.xml pour spécifier votre point d’entrée :

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project ToolsVersion="14.0" 
         xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <MyProjectOutputPath>$(PackageLayout)</MyProjectOutputPath>
  </PropertyGroup>
  <ItemGroup>
    <LayoutFile Include="$(MyProjectOutputPath)\win32\MyDesktopApp.exe">
      <PackagePath>$(PackageLayout)\win32\MyDesktopApp.exe</PackagePath>
    </LayoutFile>
  </ItemGroup>
</Project>
```

Enfin, vous devez configurer les dépendances de votre solution pour vous assurer que les projets sont générés dans l’ordre approprié. 

En guise d’exemple, passons en revue la solution créée pour l’étape 3.

![](images/desktop-to-uwp/net-13.png)

Pour configurer l’ordre de génération, vous pouvez utiliser la configuration Dépendances du projet. Cliquez avec le bouton droit sur votre solution, puis sélectionnez l’option Dépendances du projet. Une fois que vous avez défini les dépendances adéquates, vous pouvez valider l’ordre de génération comme illustré ci-dessous (pour l’étape 3) :

![](images/desktop-to-uwp/net-14.png)

<span id="known-issues-anchor" />
## <a name="known-issues-with-cvbnet-and-c-uwp-projects"></a>Problèmes connus avec les projets UWP en C#/VB.NET et C++

Si vous préférez utiliser un projet C# pour empaqueter votre application, vous devez prendre note des problèmes connus ci-après. 

- **La génération de l’application dans la fonctionnalité Débogage entraîne l’erreur suivante : Microsoft.Net.CoreRuntime.targets(235,5) : erreur : Les applications avec des exécutables de point d’entrée personnalisés ne sont pas prises en charge. Vérifiez l’attribut Executable de l’élément Application dans le manifeste du package.** Pour contourner ce problème, utilisez plutôt le mode Version finale.

- **Les fichiers binaires Win32 stockés dans le dossier racine du projet UWP sont supprimés de la version finale**. Si vous n’utilisez aucun dossier pour stocker vos fichiers binaires Win32, le compilateur .NET Native supprimera ces fichiers du package final, ce qui entraînera une erreur de validation du manifeste, puisque le point d’entrée d’exécutable sera introuvable.


