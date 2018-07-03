---
author: laurenhughes
title: Packages facultatifs avec code exécutable
description: Découvrez comment utiliser Visual Studio pour créer un package facultatif avec du code exécutable.
ms.author: lahugh
ms.date: 5/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, programme d’installation d’application, AppInstaller, charger une version test, ensemble connexe, packages facultatifs
ms.localizationpriority: medium
ms.openlocfilehash: 9745fa168ba337979a628ca9c4e799b07ad9c7ab
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843793"
---
# <a name="optional-packages-with-executable-code"></a>Packages facultatifs avec code exécutable
 
Les packages facultatifs avec code exécutable sont utiles pour diviser une application complexe ou volumineuse, ou pour ajouter une extension à une application déjà publiée. Avec Visual Studio2017, version15.7 et .NET Native2.1, vous pouvez charger un code exécutable à partir des packages facultatifs C++ et C#.

## <a name="prerequisites"></a>Éléments prérequis
- Visual Studio2017, version15.7
- Windows10, version1709
- SDK Windows10, version1709

Pour obtenir les outils de développement les plus récents, consultez [Téléchargements et outils pour Windows10](https://developer.microsoft.com/windows/downloads). 

> [!NOTE]
> Pour soumettre une application qui utilise des packages facultatifs et/ou des ensembles connexes dans le Store, il vous faudra une autorisation. S’ils ne sont pas soumis au Store, les packages facultatifs et ensembles connexes peuvent servir pour les applications métier (LOB) ou de l’entreprise sans autorisation du centre de développement. Voir [Support technique pour les développeurs Windows](https://developer.microsoft.com/windows/support) pour obtenir l’autorisation de soumettre une application qui utilise des packages facultatifs et ensembles connexes.

## <a name="c-optional-packages-with-executable-code"></a>Packages facultatifs C++ avec code exécutable

Pour charger du code à partir d’un package facultatif C++, voir le référentiel [OptionalPackageSample](https://github.com/AppInstaller/OptionalPackageSample) sur GitHub. [OptionalPackageDLL](https://github.com/AppInstaller/OptionalPackageSample/tree/master/OptionalPackageDLL) montre comment créer un projet avec du code qui peut être exécuté à partir du package principal. Le projet MyMainApp illustre comment [charger du code](https://github.com/AppInstaller/OptionalPackageSample/blob/bf6b4915ff1f3b8abfdaacb1ad9e77184c49fe18/MyMainApp/MainPage.xaml.cpp#L182) à partir du fichier OptionalPackageDLL.dll.

## <a name="c-optional-packages-with-executable-code"></a>Packages facultatifs C# avec code exécutable

Pour commencer à créer un package de code facultatif en C#, suivez les étapes ci-dessous pour configurer votre solution:

1. Créez une application UWP avec la version minimale définie sur le SDK Windows10 Fall Creators Update (Build16299) ou une version ultérieure.

2. Ajoutez un nouveau projet **Package de code facultatif (Windows universel)** à la solution. Vérifiez que la **Version minimale** et la **Version cible** correspondent à celle de votre application principale.

3. Si vous prévoyez de soumettre vos applications au Store, cliquez avec le bouton droit sur les deux projets et sélectionnez **Store-> Associer l'application au Store...**

4. Ouvrez le fichier `Package.appxmanifest` de l’application principale et recherchez la valeur `Identity Name`. Prenez également note de cette valeur pour l’étape suivante.

5. Ouvrez le fichier `Package.appxmanifest` du package d’application facultatif et recherchez la valeur `uap3:MainAppPackageDependency Name`. Mettez à jour la valeur `uap3:MainAppPackageDependency Name` pour la faire correspondre avec la valeur `Identity Name` du package d'application principal de l’étape précédente. 

    Voici un exemple d'`Identity` de l’application principale `Package.appxmanifest`.
    ```XML
    <Identity Name="12345.MainAppProject" Publisher="CN=PublisherName" Version="1.0.0.0" />
    ```

    Le package d’application facultatif `uap3:MainPackageDependency` doit être mis à jour pour correspondre à l'`Identity` de l’application principale.
    ```XML
    <uap3:MainPackageDependency Name="12345.MainAppProjectTest" />
    ```

6. Ajoutez un fichier `Bundle.mapping.txt` à l’application principale. Suivez les étapes de cette section [Ensembles connexes](https://docs.microsoft.com/windows/uwp/packaging/optional-packages#related-sets) pour créer un ensemble contenant les deux applications. 

7. Générez le projet de package facultatif, puis accédez au dossier de référence du package dans la sortie de la build située dans `..\[PathToOptionalPackageProject]\bin\[architecture]\[configuration]\Reference`. Notez que vous pouvez choisir n’importe quelle architecture dans le chemin d’accès au dossier de référence, car le fichier `.winmd` (étape8) est indépendant de l'architecture.

8. Ajoutez une référence du projet d'application principale au fichier `.winmd` qui se trouve dans ce dossier. Chaque fois que vous modifiez la surface d’exposition d’API dans le projet de package facultatif, ce fichier `.winmd` **doit** être mis à jour. Cette référence fournit le projet d’application principale avec les informations nécessaires pour compiler.

9. Dans le projet d’application principale, accédez aux propriétés de la génération de projet et sélectionnez **Compiler avec la chaîne d’outils .NET Native**. Seul le débogage dans.NET Native est actuellement pris en charge pour la création de package de code facultatif en C#. Accédez aux propriétés de débogage du projet et sélectionnez **Déployer des packages facultatifs**. Cela permet de garantir que les deux packages sont synchronisés chaque fois que vous déployez le projet d’application principale.

Une fois que vous avez terminé ces étapes, vous pouvez ajouter du code au projet de package facultatif comme s’il s’agissait d’un projet de composant WinRT géré. Pour accéder au code dans le projet d’application principale, appelez les méthodes publiques exposées dans le projet de package facultatif.