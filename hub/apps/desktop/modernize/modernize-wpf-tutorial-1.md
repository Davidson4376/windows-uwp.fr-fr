---
description: Ce didacticiel montre comment ajouter des interfaces utilisateur en XAML UWP, créer des packages MSIX et intégrer d’autres composants modernes à votre application WPF.
title: Effectuer une migration de l'application Contoso Expenses vers .NET Core 3
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, îlots XAML
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 6a52e12f9d60ee4abb4b1aed3043a69c25845267
ms.sourcegitcommit: f34deba1d4460d85ed08fe9648999fe03ff6a3dd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317106"
---
# <a name="part-1-migrate-the-contoso-expenses-app-to-net-core-3"></a>Partie 1 : Effectuer une migration de l'application Contoso Expenses vers .NET Core 3

Il s’agit de la première partie d’un didacticiel qui montre comment moderniser un exemple d’application de bureau WPF nommée Contoso depenses. Pour obtenir une vue d’ensemble du didacticiel, des conditions préalables et des instructions pour télécharger l’exemple [d’application, consultez le didacticiel: Moderniser une application](modernize-wpf-tutorial.md)WPF.
  
Dans cette partie du didacticiel, vous allez migrer l’intégralité de l’application Contoso depenses à partir du .NET Framework 4.7.2 vers [.net Core 3](modernize-wpf-tutorial.md#net-core-3). Avant de commencer cette partie du didacticiel, assurez-vous d' [ouvrir et de générer l’exemple ContosoExpenses](modernize-wpf-tutorial.md#get-the-contoso-expenses-sample-app) dans Visual Studio 2019.

> [!NOTE]
> Pour plus d’informations sur la migration d’une application WPF de l' .NET Framework vers .NET Core 3, consultez [cette série de blogs](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/).

## <a name="migrate-the-contosoexpenses-project-to-net-core-3"></a>Migrer le projet ContosoExpenses vers .NET Core 3

Dans cette section, vous allez migrer le projet ContosoExpenses dans l’application Contoso depenses vers .NET Core 3. Pour ce faire, vous devez créer un nouveau fichier projet qui contient les mêmes fichiers que le projet ContosoExpenses existant, mais qui cible .NET Core 3 au lieu du 4.7.2 .NET Framework. Cela vous permet de gérer une solution unique avec les versions .NET Framework et .NET Core de l’application.

1. Vérifiez que le projet ContosoExpenses cible actuellement le 4.7.2 .NET Framework. Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet **ContosoExpenses** , choisissez **Propriétés**, puis vérifiez que la propriété **Framework cible** de l’onglet **application** est définie sur le .NET Framework 4.7.2.

    ![.NET Framework de la version 4.7.2 pour le projet](images/wpf-modernize-tutorial/NETFramework472.png)

3. Dans l’Explorateur Windows, accédez au dossier **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses** et créez un nouveau fichier texte nommé **ContosoExpenses. Core. csproj**.

4. Cliquez avec le bouton droit sur le fichier, choisissez **Ouvrir avec**, puis ouvrez-le dans un éditeur de texte de votre choix, tel que le bloc-notes, Visual Studio code ou Visual Studio.

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

6. Fermez le fichier et revenez à la solution **ContosoExpenses** dans Visual Studio.

7. Cliquez avec le bouton droit sur la solution **ContosoExpenses** et choisissez **Ajouter-> projet existant**. Sélectionnez le fichier **ContosoExpenses. Core. csproj** que vous venez de créer `C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses` dans le dossier pour l’ajouter à la solution.

**ContosoExpenses. Core. csproj** comprend les éléments suivants :

* L’élément **Project** spécifie une version SDK de **Microsoft. net. Sdk. WindowsDesktop**. Cela fait référence aux applications .NET pour Windows Desktop et comprend des composants pour les applications WPF et Windows Forms.
* L’élément **PropertyGroup** contient des éléments enfants qui indiquent que la sortie du projet est un fichier exécutable (et non une dll), qui cible .net Core 3 et qui utilise WPF. Pour une application Windows Forms, vous devez utiliser un élément **UseWinForms** au lieu de l’élément **UseWPF** .

> [!NOTE]
> Lorsque vous utilisez le format. csproj introduit avec .NET Core 3,0, tous les fichiers dans le même dossier que le fichier. csproj sont considérés comme faisant partie du projet. Par conséquent, vous n’êtes pas obligé de spécifier chaque fichier inclus dans le projet. Vous devez spécifier uniquement les fichiers pour lesquels vous souhaitez définir une action de génération personnalisée ou que vous souhaitez exclure.

## <a name="migrate-the-contosoexpensesdata-project-to-net-standard"></a>Migrer le projet ContosoExpenses. Data vers .NET Standard

La solution **ContosoExpenses** inclut une bibliothèque de classes **ContosoExpenses. Data** qui contient des modèles et des interfaces pour les services et les cibles .net 4.7.2. Les applications .NET Core 3,0 peuvent utiliser des bibliothèques de .NET Framework, à condition qu’elles n’utilisent pas les API qui ne sont pas disponibles dans .NET Core. Toutefois, le meilleur chemin de modernisation consiste à déplacer vos bibliothèques vers .NET Standard. Cela permet de s’assurer que votre bibliothèque est entièrement prise en charge par votre application .NET Core 3,0. En outre, vous pouvez réutiliser la bibliothèque également avec d’autres plateformes, telles que le Web (via ASP.NET Core) et mobile (via Xamarin).

Pour migrer le projet **ContosoExpenses. Data** vers .NET standard :

1. Dans Visual Studio, cliquez avec le bouton droit sur le projet **ContosoExpenses. Data** , puis choisissez **décharger le projet**. Cliquez à nouveau avec le bouton droit sur le projet, puis choisissez **modifier ContosoExpenses. Data. csproj**.

2. Supprimez tout le contenu du fichier projet.

3. Copiez et collez le code XML suivant, puis enregistrez le fichier.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
      </PropertyGroup>

    </Project>
    ```

4. Cliquez avec le bouton droit sur le projet **ContosoExpenses. Data** , puis choisissez **recharger le projet**.

## <a name="configure-nuget-packages-and-dependencies"></a>Configurer des packages et des dépendances NuGet

Lorsque vous avez migré les projets **ContosoExpenses. Core** et **ContosoExpenses. Data** dans les sections précédentes, vous avez supprimé les références de package NuGet des projets. Dans cette section, vous allez rajouter ces références.

Pour configurer des packages NuGet pour le projet **ContosoExpenses. Data** :

1. Dans le projet **ContosoExpenses. Data** , développez le nœud **dépendances** . Notez que la section **NuGet** est manquante.

    ![Packages NuGet](images/wpf-modernize-tutorial/NuGetPackages.png)

    Si vous ouvrez le **fichier Packages. config** dans la **Explorateur de solutions** vous trouverez les références anciennes des packages NuGet qui utilisaient le projet lorsqu’il utilisait la .NET Framework complète.

    ![Dépendances et packages](images/wpf-modernize-tutorial/Packages.png)

    Voici le contenu du fichier **packages. config** . Vous remarquerez que tous les packages NuGet ciblent la .NET Framework complète 4.7.2 :

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="Bogus" version="26.0.2" targetFramework="net472" />
      <package id="LiteDB" version="4.1.4" targetFramework="net472" />
    </packages>
    ```

2. Dans le projet **ContosoExpenses. Data** , supprimez le fichier **packages. config** .

4. Dans le projet **ContosoExpenses. Data** , cliquez avec le bouton droit sur le nœud **dépendances** , puis choisissez **gérer les packages NuGet**.

  ![Gérer les packages NuGet...](images/wpf-modernize-tutorial/ManageNugetNETCORE3.png)

5. Dans la fenêtre **Gestionnaire de package NuGet** , cliquez sur **Parcourir**. Recherchez le `Bogus` package et installez la dernière version stable.

    ![Bogue (package NuGet)](images/wpf-modernize-tutorial/Bogus.png)

6. Recherchez le `LiteDB` package et installez la dernière version stable.

    ![Package NuGet LiteDB](images/wpf-modernize-tutorial/LiteDB.png)

    Vous vous demandez peut-être où la liste des packages NuGet est stockée, car le projet n’a plus de fichier Packages. config. Les packages NuGet référencés sont stockés directement dans le fichier. csproj. Vous pouvez vérifier cela en affichant le contenu du fichier projet **ContosoExpenses. Data. csproj** dans un éditeur de texte. Les lignes suivantes sont ajoutées à la fin du fichier :

    ```xml
    <ItemGroup>
       <PackageReference Include="Bogus" Version="26.0.2" />
       <PackageReference Include="LiteDB" Version="4.1.4" />
    </ItemGroup>
    ```

    > [!NOTE]
    > Vous pouvez également remarquer que vous installez les mêmes packages pour ce projet .NET Core 3 que ceux utilisés par .NET Framework projets 4.7.2. Les packages NuGet prennent en charge le multi-ciblage. Les auteurs de bibliothèques peuvent inclure différentes versions d’une bibliothèque dans le même package, compilées pour différentes architectures et plateformes. Ces packages prennent en charge la .NET Framework complète, ainsi que .NET Standard 2,0, qui est compatible avec les projets .NET Core 3. Pour plus d’informations sur les différences .NET Framework, .NET Core et .NET Standard, consultez [.NET standard](https://docs.microsoft.com/dotnet/standard/net-standard).

Pour configurer des packages NuGet pour le projet **ContosoExpenses. Core** :

1. Dans le projet **ContosoExpenses. Core** , ouvrez le fichier **packages. config** . Notez qu’il contient actuellement les références suivantes qui ciblent le .NET Framework 4.7.2.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="CommonServiceLocator" version="2.0.2" targetFramework="net472" />
      <package id="MvvmLightLibs" version="5.4.1.1" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
      <package id="Unity" version="5.10.2" targetFramework="net472" />
    </packages>
    ```

    Dans les étapes suivantes, vous allez .NET standard versions des `MvvmLightLibs` packages `Unity` et. Les deux autres sont des dépendances automatiquement téléchargées par NuGet lors de l’installation de ces deux bibliothèques.

2. Dans le projet **ContosoExpenses. Core** , supprimez le fichier **packages. config** .

3. Cliquez avec le bouton droit sur le projet **ContosoExpenses. Core** , puis choisissez **gérer les packages NuGet**.

4. Dans la fenêtre **Gestionnaire de package NuGet** , cliquez sur **Parcourir**. Recherchez le `Unity` package et installez la dernière version stable.

    ![Package Unity](images/wpf-modernize-tutorial/UnityPackage.png)

5. Recherchez le `MvvmLightLibsStd10` package et installez la dernière version stable. Il s’agit de la version .NET standard `MvvmLightLibs` du package. Pour ce package, l’auteur a choisi d’empaqueter la version .NET Standard de la bibliothèque dans un package distinct de celui de la version de .NET Framework.

    ![Package MvvmLightsLibs](images/wpf-modernize-tutorial/MvvmLightsLibsPackage.png)

6. Dans le projet **ContosoExpenses. Core** , cliquez avec le bouton droit sur le nœud **dépendances** , puis choisissez **Ajouter une référence**.

7. Dans la catégorie **projets > solution** , sélectionnez **ContosoExpenses. Data** , puis cliquez sur **OK**.

    ![Ajouter une référence](images/wpf-modernize-tutorial/AddReference.png)

## <a name="disable-auto-generated-assembly-attributes"></a>Désactiver les attributs d’assembly générés automatiquement

À ce stade du processus de migration, si vous essayez de générer le projet **ContosoExpenses. Core** , vous verrez des erreurs.

![.NET Core 3 génère de nouvelles Erreurs](images/wpf-modernize-tutorial/NETCORE3BuildNewErrors.png)

Ce problème est dû au fait que le nouveau format. csproj introduit avec .NET Core 3,0 stocke les informations de l’assembly dans le fichier projet plutôt que dans le fichier **AssemblyInfo.cs** . Pour corriger ces erreurs, désactivez ce comportement et laissez le projet continuer à utiliser le fichier **AssemblyInfo.cs** .

1. Dans Visual Studio, cliquez avec le bouton droit sur le projet **ContosoExpenses. Core** , puis choisissez **décharger le projet**. Cliquez à nouveau avec le bouton droit sur le projet, puis choisissez **modifier ContosoExpenses. Core. csproj**.

1. Ajoutez l’élément suivant dans la section **PropertyGroup** et enregistrez le fichier.

    ```XML
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    Après avoir ajouté cet élément, la section **PropertyGroup** doit maintenant ressembler à ceci :

    ```XML
    <PropertyGroup>
      <OutputType>WinExe</OutputType>
      <TargetFramework>netcoreapp3.0</TargetFramework>
      <UseWPF>true</UseWPF>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

3. Cliquez avec le bouton droit sur le projet **ContosoExpenses. Core** , puis choisissez **recharger le projet**.

4. Cliquez avec le bouton droit sur le projet **ContosoExpenses. Data** et choisissez **décharger le projet**. Cliquez à nouveau avec le bouton droit sur le projet, puis choisissez **modifier ContosoExpenses. Data. csproj**.

5. Ajoutez la même entrée dans la section **PropertyGroup** et enregistrez le fichier.

    ```xml
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    Après avoir ajouté cet élément, la section **PropertyGroup** doit maintenant ressembler à ceci :

    ```xml
    <PropertyGroup>
      <TargetFramework>netstandard2.0</TargetFramework>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

6. Cliquez avec le bouton droit sur le projet **ContosoExpenses. Data** , puis choisissez **recharger le projet**.

## <a name="add-the-windows-compatibility-pack"></a>Ajouter le Pack de compatibilité Windows

Si vous essayez maintenant de compiler les projets **ContosoExpenses. Core** et **ContosoExpenses. Data** , vous verrez que les erreurs précédentes sont maintenant résolues, mais il existe toujours des erreurs dans la bibliothèque **ContosoExpenses. Data** , comme celles-ci.

`Services\RegistryService.cs(9,26,9,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,26,12,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,97,12,123): error CS0103: The name 'RegistryKeyPermissionCheck' does not exist in the current context`

Ces erreurs sont le résultat de la conversion du projet **ContosoExpenses. Data** d’une bibliothèque .NET Framework (qui est spécifique pour Windows) en bibliothèque de .NET standard, qui peut s’exécuter sur plusieurs plateformes, notamment Linux, Android, iOS, etc. Le projet **ContosoExpenses. Data** contient une classe appelée **RegistryService**, qui interagit avec le registre, un concept Windows uniquement.

Pour résoudre ces erreurs, installez le package NuGet de [Compatibilité Windows](https://www.nuget.org/packages/Microsoft.Windows.Compatibility) . Ce package fournit la prise en charge de nombreuses API spécifiques à Windows à utiliser dans une bibliothèque de .NET Standard. La bibliothèque ne sera plus multiplateforme après l’utilisation de ce package, mais elle sera toujours ciblée .NET Standard. 

1. Cliquez avec le bouton droit sur le projet **ContosoExpenses. Data** .
2. Choisissez **gérer les packages NuGet**.
3. Dans la fenêtre **Gestionnaire de package NuGet** , cliquez sur **Parcourir**. Recherchez le `Microsoft.Windows.Compatibility` package et installez la dernière version stable.

    ![](images/wpf-modernize-tutorial/WindowsCompatibilityPack.png)

4. Réessayez maintenant de compiler le projet, en cliquant avec le bouton droit sur le projet **ContosoExpenses. Data** et en choisissant **générer**.

Cette fois, le processus de génération se termine sans erreurs.

## <a name="test-and-debug-the-migration"></a>Tester et déboguer la migration

Maintenant que les projets sont générés avec succès, vous êtes prêt à exécuter et à tester l’application pour voir s’il existe des erreurs d’exécution.

1. Cliquez avec le bouton droit sur le projet **ContosoExpenses. Core** , puis choisissez **définir comme projet de démarrage**.

2. Appuyez sur F5 pour démarrer le projet **ContosoExpenses. Core** dans le débogueur. Une exception semblable à la suivante s’affiche.

    ![Exception affichée dans Visual Studio](images/wpf-modernize-tutorial/ExceptionNETCore3.png)

    Cette exception est levée car lorsque vous supprimez le contenu du fichier. csproj au début de la migration, vous avez supprimé les informations sur l' **action de génération** pour les fichiers image. Les étapes suivantes résolvent ce problème.

3. Arrêtez le débogueur.

4. Cliquez avec le bouton droit sur le projet **ContosoExpenses. Core** , puis choisissez **décharger le projet**. Cliquez à nouveau avec le bouton droit sur le projet, puis choisissez **modifier ContosoExpenses. Core. csproj**.

5. Avant l’élément de fermeture du **projet** , ajoutez l’entrée suivante :

    ```xml
    <ItemGroup>
      <Content Include="Images/*">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
      </Content>
    </ItemGroup>
    ```

6. Cliquez avec le bouton droit sur le projet **ContosoExpenses. Core** , puis choisissez **recharger le projet**.

7. Pour affecter le contoso. ico à l’application, cliquez avec le bouton droit sur le projet **ContosoExpenses. Core** , puis choisissez **Propriétés**. Dans la page ouverte, cliquez sur liste déroulante sous **icône** et sélectionnez `Images\contoso.ico`.

    ![Icône Contoso dans les propriétés du projet](images/wpf-modernize-tutorial/ContosoIco.png)

8. Cliquez sur **Enregistrer**.

9. Appuyez sur F5 pour démarrer le projet **ContosoExpenses. Core** dans le débogueur. Vérifiez que l’application s’exécute maintenant.

## <a name="next-steps"></a>Étapes suivantes

À ce stade du didacticiel, vous avez réussi à migrer l’application Contoso depenses vers .NET Core 3. Vous êtes maintenant prêt pour [la partie 2 : Ajoutez un contrôle UWP InkCanvas à l’aide](modernize-wpf-tutorial-2.md)des îlots XAML.

> [!NOTE]
> Si vous avez un écran haute résolution, vous remarquerez peut-être que l’application semble très petite. Vous allez résoudre ce problème à l’étape suivante du didacticiel.
