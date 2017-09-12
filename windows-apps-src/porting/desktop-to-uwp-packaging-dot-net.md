---
author: normesta
Description: "Ce guide vous explique comment configurer votre solution VisualStudio pour modifier, déboguer et empaqueter une application de bureau pour le Pont du bureau."
Search.Product: eADQiWindows 10XVcnh
title: "Créer un package d'application à l’aide de Visual Studio (Pont du bureau)"
ms.author: normesta
ms.date: 07/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.openlocfilehash: d8919448b965f18ff7f8fdaeda325889e495ef85
ms.sourcegitcommit: f6dd9568eafa10ee5cb2b849c0d82d84a1c5fb93
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/02/2017
---
# <a name="package-an-app-by-using-visual-studio-desktop-bridge"></a>Créer un package d'application à l’aide de Visual Studio (Pont du bureau)

Vous pouvez utiliser Visual Studio pour générer un package de votre application de bureau. Vous pouvez ensuite publier ce package sur le Windows store ou le charger de manière indépendante sur un ou plusieurs ordinateurs.

Ce guide vous explique comment configurer votre solution, puis générer un package pour votre application de bureau.

## <a name="first-consider-how-youll-distribute-your-app"></a>Commencez par examiner la méthode de distribution de votre application

Si vous prévoyez de publier votre application sur le [WindowsStore](https://www.microsoft.com/store/apps), commencez par renseigner [ce formulaire](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge). Microsoft vous contactera pour démarrer le processus d’intégration. Dans le cadre de ce processus, vous devez réserver un nom dans le WindowsStore et obtenir les informations nécessaires pour créer le package de votre application.

## <a name="add-a-packaging-project-to-your-solution"></a>Ajouter un projet de création de package à votre solution

1. Dans Visual Studio, ouvrez la solution qui contient votre projet d'application du bureau.

2. Ajoutez un projet **Application vide (Windows universelle)** JavaScript à votre solution.

   Vous n’aurez pas à lui ajouter du code. Il vous sert seulement à générer un package. Nous allons désigner ce projet par l'expression «projet de création de package».

   ![Projet UWP JavaScript](images/desktop-to-uwp/javascript-uwp-project.png)

   >[!IMPORTANT]
   >En règle générale, vous devez utiliser la version JavaScript de ce projet.  Les versions C#, VB.NET et C++ présentent quelques problèmes, mais si vous souhaitez en utiliser des éléments, consultez d'abord le guide [Problèmes connus](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-known-issues#known-issues-anchor).

## <a name="add-the-desktop-application-binaries-to-the-packaging-project"></a>Ajouter les fichiers binaires d’application de bureau au projet de création de package

Ajoutez les fichiers binaires directement au projet de création de package.

1. Dans l'**Explorateur de solutions**, développez le dossier du projet de création de package, créez un sous-dossier et donnez-lui le nom de votre choix (par exemple: **win32**).

2. Cliquez avec le bouton droit sur le sous-dossier, puis choisissez **Ajouter un élément existant**.

3. Dans la boîte de dialogue **Ajouter un élément existant**, recherchez les fichiers dans le dossier de sortie de votre application de bureau, puis ajoutez-les. Cela inclut non seulement les fichiers exécutables, mais aussi tous les fichiers .dll ou .config qui se trouvent dans ce dossier.

   ![Référencer le fichier exécutable](images/desktop-to-uwp/cpp-exe-reference.png)

   Chaque fois que vous apportez une modification à votre projet d’application de bureau, vous devez copier une nouvelle version de ces fichiers dans le projet de création de package. Vous pouvez automatiser cette opération en ajoutant un événement post-build dans le fichier de projet du projet de création de package. En voici un exemple.

   ```XML
   <Target Name="PostBuildEvent">
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyWindowsFormsApplication.exe"
       DestinationFolder="win32" />
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyWindowsFormsApplication.exe.config"
       DestinationFolder="win32" />
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyWindowsFormsApplication.pdb"
       DestinationFolder="win32" />
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyBusinessLogicLibrary.dll"
       DestinationFolder="win32" />
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyBusinessLogicLibrary.pdb"
       DestinationFolder="win32" />
   </Target>
   ```

## <a name="modify-the-package-manifest"></a>Modifier le manifeste du package

Le projet de création de package contient un fichier qui décrit les paramètres de votre package. Par défaut, ce fichier décrit une application UWP. Vous devez donc le modifier afin que le système comprenne que votre package inclut une application de bureau qui s’exécute en mode de confiance totale.  

1. Dans l'**Explorateur de solutions**, développez le projet de création de package, cliquez avec le bouton droit sur le fichier **package.appxmanifest**, puis choisissez **Afficher le code**.

   ![Référencer le projet dotnet](images/desktop-to-uwp/reference-dotnet-project.png)

2. Ajoutez cet espace de noms en haut du fichier, puis ajoutez le préfixe d’espace de noms à la liste des ``IgnorableNamespaces``.

   ```XML
   xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
   ```
   Lorsque vous avez terminé, vos déclarations d’espace de noms doivent ressembler à ceci:

   ```XML
   <Package
     xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
     xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
     xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
     xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
     IgnorableNamespaces="uap mp rescap">
   ```

3. Trouvez l'élément ``TargetDeviceFamily`` et définissez l'attribut ``Name`` sur la valeur **Windows.Desktop**, l’attribut ``MinVersion`` sur la version minimale du projet de création de package et le ``MaxVersionTested`` sur la version cible du projet de création de packages.

   ```XML
   <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.10586.0" MaxVersionTested="10.0.15063.0" />
   ```

   Vous trouverez la version minimale et la version cible dans les pages de propriétés du projet de création de package.

   ![Paramètres de version minimale et cible](images/desktop-to-uwp/min-target-version-settings.png)


4. Supprimez l’attribut ``StartPage`` de l'élément ``Application``. Ensuite, ajoutez les attributs ``Executable`` et ``EntryPoint``.

   L'élément ``Application`` se présente comme suit.

   ```XML
   <Application Id="App"  Executable=" " EntryPoint=" ">
   ```

5. Attribuez à l'attribut ``Executable`` le nom du fichier exécutable de votre application de bureau. Ensuite, attribuez à l'attribut ``EntryPoint`` la valeur **Windows.FullTrustApplication**.

   L'élément ``Application`` se présente comme suit.

   ```XML
   <Application Id="App"  Executable="win32\MyWindowsFormsApplication.exe" EntryPoint="Windows.FullTrustApplication">
   ```
6. Ajoutez la fonctionnalité ``runFullTrust`` à l'élément ``Capabilities``.

   ```XML
     <rescap:Capability Name="runFullTrust"/>
   ```
   Des marques bleues ondulées peuvent apparaître sous cette déclaration, mais vous pouvez les ignorer.

   >[!IMPORTANT]
   Si votre créez un package pour une application de bureau C++, vous devez apporter quelques modifications supplémentaires à votre fichier manifeste afin de pouvoir déployer les runtimes Visual C++ avec votre application. Voir [Utilisation de runtimes Visual C++ dans un projet de Pont du bureau](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project/).

7. Générez le projet de création de package pour vous assurer qu’aucune erreur n’apparaît.

8. Si vous souhaitez tester votre package, consultez [Exécuter, déboguer et tester une application de bureau empaquetée (Pont du bureau)](desktop-to-uwp-debug.md).

   Revenez ensuite sur ce guide et consultez la section suivante pour générer votre package.

## <a name="generate-a-package"></a>Générer un package

Pour générer un package de votre application, suivez les instructions décrites dans cette rubrique: [Création de packages d’application UWP](..\packaging\packaging-uwp-apps.md).

Lorsque vous atteindrez l'écran **Sélectionner et configurer des packages**, prenez un moment pour réfléchir aux types de fichiers binaires que vous incluez dans votre package avant de sélectionner des cases à cocher.

* Si vous avez [étendu](desktop-to-uwp-extend.md) votre application de bureau en ajoutant un projet de plateforme Windows universelle en C#, C++ ou VB.NET à votre solution, sélectionnez les cases à cocher **x86** et **x64**.  

* Dans le cas contraire, choisissez la case à cocher **Neutre**.

>[!NOTE]
La raison pour laquelle vous devez choisir explicitement chaque plate-forme prise en charge, c'est parce qu'une solution étendue contient deux types de fichiers binaires: un pour le projet UWP et un pour le projet de bureau. Comme il s’agit de types de fichiers binaires différents, .NET Native doit explicitement produire des fichiers binaires natifs pour chaque plate-forme.

Si vous recevez des erreurs en tentant de générer votre package, consultez le guide [Problèmes connus](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-known-issues#known-issues-anchor) et si votre problème n’apparaît pas dans cette liste, veuillez nous faire part du problème [ici](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge).

## <a name="next-steps"></a>Étapes suivantes

**Exécutez votre application/recherchez et réparez les problèmes**

Voir [Exécuter, déboguer et tester une application de bureau empaquetée (Pont du bureau)](desktop-to-uwp-debug.md)

**Améliorer votre application de bureau en ajoutant des API UWP**

Voir [Améliorer votre application de bureau pour Windows10](desktop-to-uwp-enhance.md)

**Étendre votre application de bureau en ajoutant des composants UWP**

Voir [Étendre votre application de bureau avec des composants UWP modernes](desktop-to-uwp-extend.md).

**Distribuer votre application**

Voir [Distribuer une application de bureau empaquetée (Pont du bureau)](desktop-to-uwp-distribute.md)

**Trouver des réponses aux questions spécifiques**

Notre équipe contrôle ces [balises StackOverflow](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge).

**Envoyer vos commentaires concernant cet article**

Utilisez la section remarques ci-dessous.
