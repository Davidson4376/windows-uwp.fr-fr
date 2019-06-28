---
description: Ce didacticiel montre comment ajouter des interfaces utilisateur UWP XAML, créer des packages MSIX et incorporer d’autres composants modernes dans votre application WPF.
title: Migrer le Contoso application de notes de frais vers .NET Core 3
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, uwp, WinForms, wpf, xaml (îles)
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: e718de7a22873ccf347e60c661f724ce3abdd2cf
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420134"
---
# <a name="part-1-migrate-the-contoso-expenses-app-to-net-core-3"></a>Partie 1 : Migrer le Contoso application de notes de frais vers .NET Core 3

Il s’agit de la première partie d’un didacticiel qui montre comment moderniser une application de bureau WPF exemple nommée dépenses de Contoso. Pour une vue d’ensemble du didacticiel, configuration requise et des instructions pour télécharger l’exemple d’application, consultez [didacticiel : Moderniser une application WPF](modernize-wpf-tutorial.md).
  
Dans cette partie du didacticiel, vous allez migrer toute l’application de dépenses de Contoso à partir de .NET Framework 4.7.2 [.NET Core 3](modernize-wpf-tutorial.md#net-core-3). Avant de commencer cette partie du didacticiel, assurez-vous que vous procédez comme suit :

* [Ouvrez et générez l’exemple ContosoExpenses](modernize-wpf-tutorial.md#get-the-contoso-expenses-sample-app) dans Visual Studio 2019.
* Si vous utilisez une version release de Visual Studio 2019, activer des versions préliminaires du SDK .NET Core. Dans Visual Studio, accédez à **Outils > Options**, tapez « Préversion » dans la zone de recherche, sélectionnez **utiliser des versions préliminaires du SDK .NET Core**. Si vous utilisez un [version préliminaire de Visual Studio 2019](https://visualstudio.microsoft.com/vs/preview/), vous n’avez pas besoin de sélectionner cette option, car les versions préliminaires de .NET Core sont activées par défaut.

## <a name="migrate-the-contosoexpenses-project-to-net-core-3"></a>Migrer le projet ContosoExpenses vers .NET Core 3

Dans cette section, vous allez migrer le projet ContosoExpenses dans l’application de dépenses de Contoso pour .NET Core 3. Pour ce faire, vous allez créer un nouveau fichier de projet qui contient les mêmes fichiers que le projet ContosoExpenses existant mais qu’il cible .NET Core 3 au lieu de .NET Framework 4.7.2. Cela vous permet de maintenir une solution unique avec les versions .NET Framework et .NET Core de l’application.

1. Vérifiez que les ContosoExpenses projet actuellement cible le .NET Framework 4.7.2. Dans l’Explorateur de solutions, cliquez sur le **ContosoExpenses** de projet, choisissez **propriétés**et vérifiez que le **framework cible** propriété sur le  **Application** onglet est définie sur .NET Framework 4.7.2.

    ![.NET framework version 4.7.2 pour le projet](images/wpf-modernize-tutorial/NETFramework472.png)

3. Dans l’Explorateur Windows, accédez à la **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses** dossier et créer un nouveau fichier texte nommé **ContosoExpenses.Core.csproj**.

4. Cliquez avec le bouton droit sur le fichier, choisissez **ouvrir avec**, puis ouvrez-le dans un éditeur de texte de votre choix, comme le bloc-notes, Visual Studio Code ou Visual Studio.

5. Copiez le texte suivant dans le fichier et enregistrez-le.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>netcoreapp3.0</TargetFramework>
        <UseWPF>true</UseWPF>
     </PropertyGroup>

    </Project>
    ```

6. Fermez le fichier et revenir à la **ContosoExpenses** solution dans Visual Studio.

7. Cliquez sur le **ContosoExpenses** solution et choisissez **Ajouter -> projet existant**. Sélectionnez le **ContosoExpenses.Core.csproj** fichier que vous venez de créer dans le `C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses` dossier pour l’ajouter à la solution.

Le **ContosoExpenses.Core.csproj** inclut les éléments suivants :

* Le **projet** élément spécifie une version SDK de **Microsoft.NET.Sdk.WindowsDesktop**. Cela fait référence à des applications .NET pour Windows Desktop, et il inclut des composants pour les applications WPF et Windows Forms.
* Le **PropertyGroup** élément contient des éléments enfants qui indiquent la sortie du projet est un fichier exécutable (pas une DLL), cible de .NET Core 3 et utilise WPF. Pour une application Windows Forms, vous utiliseriez un **UseWinForms** élément au lieu du **UseWPF** élément.

> [!NOTE]
> Lorsque vous travaillez avec le format .csproj introduit avec .NET Core 3.0, tous les fichiers dans le même dossier que le fichier .csproj sont considérées comme partie du projet. Par conséquent, vous n’êtes pas obligé de spécifier chaque fichier inclus dans le projet. Vous devez spécifier uniquement les fichiers pour lesquels vous souhaitez définir une action de génération personnalisée ou que vous souhaitent exclure.

## <a name="migrate-the-contosoexpensesdata-project-to-net-standard"></a>Migrer le projet ContosoExpenses.Data vers .NET Standard

Le **ContosoExpenses** solution inclut un **ContosoExpenses.Data** bibliothèque de classes qui contient des modèles et des interfaces pour les services et qu’elle cible .NET 4.7.2. Les applications .NET core 3.0 peuvent utiliser les bibliothèques .NET Framework, tant qu’ils n’utilisent pas les API qui ne sont pas disponibles dans .NET Core. Toutefois, la meilleure voie de modernisation consiste à atteindre vos bibliothèques .NET Standard. Cela permet de garantir que votre bibliothèque est entièrement pris en charge par votre application .NET Core 3.0. En outre, vous pouvez réutiliser la bibliothèque également avec d’autres plateformes, telles que web (via ASP.NET Core) et mobile (à l’aide de Xamarin).

Pour migrer le **ContosoExpenses.Data** projet vers .NET Standard :

1. Dans Visual Studio, cliquez sur le **ContosoExpenses.Data** de projet et choisissez **décharger le projet**. Cliquez à nouveau sur le projet, puis choisissez **ContosoExpenses.Data.csproj modifier**.

2. Supprimer tout le contenu du fichier projet.

3. Copiez et collez le code XML suivant et enregistrez le fichier.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
      </PropertyGroup>

    </Project>
    ```

4. Cliquez sur le **ContosoExpenses.Data** de projet et choisissez **recharger le projet**.

## <a name="configure-nuget-packages-and-dependencies"></a>Configurer les dépendances et les packages NuGet

Lorsque vous avez migré le **ContosoExpenses.Core** et **ContosoExpenses.Data** projets dans les sections précédentes, vous avez supprimé les références de package NuGet à partir des projets. Dans cette section, vous allez ajouter ces références arrière.

Pour configurer les packages NuGet pour la **ContosoExpenses.Data** projet :

1.  Dans le **ContosoExpenses.Data** de projet, développez le **dépendances** nœud. Notez que le **NuGet** section est manquante.

    ![Packages NuGet](images/wpf-modernize-tutorial/NuGetPackages.png)

    Si vous ouvrez le **Packages.config** dans le **l’Explorateur de solutions** vous trouverez les références « anciens » des packages NuGet utilisé le projet lorsqu’il était le .NET Framework complet.

    ![Dépendances et des packages](images/wpf-modernize-tutorial/Packages.png)

    Voici le contenu de la **Packages.config** fichier. Vous pouvez remarquer que tous les Packages NuGet ciblent le .NET Framework 4.7.2 :

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="Bogus" version="26.0.2" targetFramework="net472" />
      <package id="LiteDB" version="4.1.4" targetFramework="net472" />
    </packages>
    ```

2. Dans le **ContosoExpenses.Data** de projet, supprimez le **Packages.config** fichier.

4. Dans le **ContosoExpenses.Data** de projet, cliquez sur le **dépendances** nœud et choisissez **gérer les Packages NuGet**.

  ![Gérer les Packages NuGet...](images/wpf-modernize-tutorial/ManageNugetNETCORE3.png)

5. Dans le **Gestionnaire de Package NuGet** fenêtre, cliquez sur **Parcourir**. Recherchez le `Bogus` du package et l’installer.

    ![Package NuGet erroné](images/wpf-modernize-tutorial/Bogus.png)

6. Recherchez le `LiteDB` du package et l’installer.

    ![Package NuGet de LiteDB](images/wpf-modernize-tutorial/LiteDB.png)

    Vous vous demandez peut-être où ces liste de packages NuGet est stockée, dans la mesure où le projet n’a plus un fichier packages.config. Les packages NuGet référencés sont stockées directement dans le fichier .csproj. Vous pouvez le vérifier en affichant le contenu de la **ContosoExpenses.Data.csproj** fichier projet dans un éditeur de texte. Vous trouverez les lignes suivantes, ajoutées à la fin du fichier :

    ```xml
    <ItemGroup>
       <PackageReference Include="Bogus" Version="26.0.2" />
       <PackageReference Include="LiteDB" Version="4.1.4" />
    </ItemGroup>
    ```

    > [!NOTE]
    > Vous pouvez également remarquer que vous installez les packages pour ce projet .NET Core 3 en tant que celles utilisées par les projets .NET Framework 4.7.2. Les packages NuGet prend en charge le multi-ciblage. Les auteurs de bibliothèque peuvent inclure des différentes versions d’une bibliothèque dans le même package, compilé pour les plateformes et des architectures différentes. Ces packages prennent en charge le .NET Framework complet, ainsi que .NET Standard 2.0, qui est compatible avec les projets .NET Core 3. Pour plus d’informations sur les différences de .NET Framework, .NET Core et .NET Standard, consultez [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard).

Pour configurer les packages NuGet pour la **ContosoExpenses.Core** projet :

1. Dans le **ContosoExpenses.Core** projet, ouvrez le **packages.config** fichier. Notez qu’il contient actuellement les références suivantes qui ciblent le .NET Framework 4.7.2.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="CommonServiceLocator" version="2.0.2" targetFramework="net472" />
      <package id="MvvmLightLibs" version="5.4.1.1" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
      <package id="Unity" version="5.10.2" targetFramework="net472" />
    </packages>
    ```

    Dans les étapes suivantes, vous allez les versions .NET Standard de la `MvvmLightLibs` et `Unity` packages. Les deux autres sont automatiquement téléchargés par NuGet lorsque vous installez ces deux bibliothèques de dépendances.

2. Dans le **ContosoExpenses.Core** de projet, supprimez le **Packages.config** fichier.

3. Cliquez sur le **ContosoExpenses.Core** de projet et choisissez **gérer les Packages NuGet**.

4. Dans le **Gestionnaire de Package NuGet** fenêtre, cliquez sur **Parcourir**. Recherchez le `Unity` du package et l’installer.

    ![Package Unity](images/wpf-modernize-tutorial/UnityPackage.png)

5. Recherchez le `MvvmLightLibsStd10` du package et l’installer. C’est la version .NET Standard de la `MvvmLightLibs` package. Pour ce package, l’auteur a choisi pour créer un package de la version .NET Standard de la bibliothèque dans un package distinct à la version de .NET Framework.

    !MvvmLightsLibs package[](images/wpf-modernize-tutorial/MvvmLightsLibsPackage.png)

6. Dans le **ContosoExpenses.Core** de projet, cliquez sur le **dépendances** nœud et choisissez **ajouter une référence**.

7. Dans le **projets > Solution** catégorie, sélectionnez **ContosoExpenses.Data** et cliquez sur **OK**.

    ![Ajouter une référence](images/wpf-modernize-tutorial/AddReference.png)

## <a name="disable-auto-generated-assembly-attributes"></a>Désactiver les attributs de l’assembly généré automatiquement

À ce stade du processus de migration, si vous tentez de générer le **ContosoExpenses.Core** projet, vous verrez des erreurs.

![Erreurs de nouvelle génération .NET core 3](images/wpf-modernize-tutorial/NETCORE3BuildNewErrors.png)

Ce problème se produit, car le nouveau format .csproj introduite avec .NET Core 3.0 stocke les informations sur l’assembly dans le fichier projet plutôt que la **AssemblyInfo.cs** fichier. Pour corriger ces erreurs, désactiver ce comportement et de laisser le projet à continuer à utiliser le **AssemblyInfo.cs** fichier.

1. Dans Visual Studio, cliquez sur le **ContosoExpenses.Core** de projet et choisissez **décharger le projet**. Cliquez à nouveau sur le projet, puis choisissez **ContosoExpenses.Core.csproj modifier**.

1. Ajoutez l’élément suivant dans le **PropertyGroup** section et enregistrez le fichier.

    ```XML
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    Après l’ajout de cet élément, le **PropertyGroup** section doit maintenant ressembler à ceci :

    ```XML
    <PropertyGroup>
      <OutputType>WinExe</OutputType>
      <TargetFramework>netcoreapp3.0</TargetFramework>
      <UseWPF>true</UseWPF>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

3. Cliquez sur le **ContosoExpenses.Core** de projet et choisissez **recharger le projet**.

4. Cliquez sur le **ContosoExpenses.Data** de projet et choisissez **décharger le projet**. Cliquez à nouveau sur le projet, puis choisissez **ContosoExpenses.Data.csproj modifier**.

5. Ajouter la même entrée dans le **PropertyGroup** section et enregistrez le fichier.

    ```xml
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    Après l’ajout de cet élément, le **PropertyGroup** section doit maintenant ressembler à ceci :

    ```xml
    <PropertyGroup>
      <TargetFramework>netstandard2.0</TargetFramework>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

6. Cliquez sur le **ContosoExpenses.Data** de projet et choisissez **recharger le projet**.

## <a name="add-the-windows-compatibility-pack"></a>Ajouter le Pack de compatibilité Windows

Si vous essayez maintenant de compiler la **ContosoExpenses.Core** et **ContosoExpenses.Data** projets, vous verrez que les erreurs précédentes sont maintenant résolus, mais il existe toujours des erreurs dans le  **ContosoExpenses.Data** bibliothèque similaire à celles-ci.

`Services\RegistryService.cs(9,26,9,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,26,12,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,97,12,123): error CS0103: The name 'RegistryKeyPermissionCheck' does not exist in the current context`

Ces erreurs sont le résultat de la conversion du **ContosoExpenses.Data** projet à partir d’une bibliothèque .NET Framework (qui est spécifique à Windows) vers une bibliothèque .NET Standard, qui peut s’exécuter sur plusieurs plateformes, notamment Linux, Android, iOS, et bien plus encore. Le **ContosoExpenses.Data** projet contient une classe appelée **RegistryService**, qui interagit avec le Registre, un concept Windows uniquement.

Pour résoudre ces erreurs, installez le [Windows compatibilité](https://www.nuget.org/packages/Microsoft.Windows.Compatibility) package NuGet. Ce package fournit la prise en charge de nombreuses API Windows spécifique à utiliser dans une bibliothèque .NET Standard. La bibliothèque ne sera plus inter-plateformes une fois à l’aide de ce package, mais il sera toujours cibler .NET Standard. 

1. Avec le bouton droit sur le **ContosoExpenses.Data** projet.
2. Choisissez **gérer les Packages NuGet**.
3. Dans le **Gestionnaire de Package NuGet** fenêtre, cliquez sur **Parcourir**. Recherchez le `Microsoft.Windows.Compatibility` du package et l’installer. 

    ![](images/wpf-modernize-tutorial/WindowsCompatibilityPack.png)

4. À présent, réessayez compiler le projet, en cliquant avec le bouton droit sur le **ContosoExpenses.Data** projet et en choisissant **Build**.

Cette fois le processus de génération s’exécute sans erreur.

## <a name="test-and-debug-the-migration"></a>Tester et déboguer la migration

Maintenant que les projets de génération avec succès, vous êtes prêt à exécuter et tester l’application pour voir s’il existe des erreurs d’exécution.

1. Cliquez sur le **ContosoExpenses.Core** projet, puis sélectionnez **définir comme projet de démarrage**.

2. Appuyez sur F5 pour démarrer le **ContosoExpenses.Core** projet dans le débogueur. Vous verrez une exception semblable à ce qui suit.

    ![Exception affichée dans Visual Studio](images/wpf-modernize-tutorial/ExceptionNETCore3.png)

    Cette exception est déclenchée, car lorsque vous avez supprimé le contenu à partir du fichier .csproj au début de la migration, vous avez supprimé les informations sur le **action de génération** pour les fichiers image. Les étapes suivantes résoudre ce problème.

3. Arrêtez le débogueur.

4. Cliquez sur le **ContosoExpenses.Core** de projet et choisissez **décharger le projet**. Cliquez à nouveau sur le projet, puis choisissez **ContosoExpenses.Core.csproj modifier**.

5. Avant de fermer le **projet** élément, ajoutez l’entrée suivante :

    ```xml
    <ItemGroup>
      <Content Include="Images/*">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
      </Content>
    </ItemGroup>
    ```

6. Cliquez sur le **ContosoExpenses.Core** de projet et choisissez **recharger le projet**.

7. Pour affecter le Contoso.ico à l’application, cliquez sur le **ContosoExpenses.Core** de projet et choisissez **propriétés**. Dans la page ouverte, cliquez sur la liste déroulante sous **icône** et sélectionnez `Images\contoso.ico`.

    ![Icône de Contoso dans les propriétés du projet](images/wpf-modernize-tutorial/ContosoIco.png)

8. Cliquez sur **Enregistrer**.

9. Appuyez sur F5 pour démarrer le **ContosoExpenses.Core** projet dans le débogueur. Vérifiez que l’application s’exécute maintenant.

## <a name="next-steps"></a>Étapes suivantes

À ce stade dans le didacticiel, vous avez migré avec succès l’application de dépenses de Contoso pour .NET Core 3. Vous êtes maintenant prêt pour [partie 2 : Ajouter un contrôle UWP InkCanvas à l’aide de XAML îles](modernize-wpf-tutorial-2.md).

> [!NOTE]
> Si vous avez un écran haute résolution, vous pouvez remarquer que l’application est très petite. Vous allez résoudre le problème à l’étape suivante du didacticiel.
