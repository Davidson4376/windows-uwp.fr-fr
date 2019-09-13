---
description: Cet article explique comment héberger un contrôle UWP personnalisé dans une application WPF à l’aide des îlots XAML.
title: Héberger un contrôle UWP personnalisé dans une application WPF à l’aide des îlots XAML
ms.date: 08/20/2019
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, îlots XAML, contrôles personnalisés, contrôles utilisateur, contrôles hôtes
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: ab9bff69ac9ac0eaf1f02c943229829e726a0b9d
ms.sourcegitcommit: 8cbc9ec62a318294d5acfea3dab24e5258e28c52
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70911569"
---
# <a name="host-a-custom-uwp-control-in-a-wpf-app-using-xaml-islands"></a>Héberger un contrôle UWP personnalisé dans une application WPF à l’aide des îlots XAML

Cet article montre comment utiliser le contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) dans la boîte à outils de la communauté Windows pour héberger un contrôle UWP personnalisé dans une application WPF qui cible .net Core 3. Le contrôle personnalisé contient plusieurs contrôles UWP internes du SDK Windows et lie une propriété de l’un des contrôles UWP à une chaîne dans l’application WPF. Cet article montre également comment héberger un contrôle UWP de premier tiers à partir de la [bibliothèque WinUI](https://docs.microsoft.com/uwp/toolkits/winui/).

Bien que cet article montre comment effectuer cette opération dans une application WPF, le processus est similaire pour une application Windows Forms. Pour obtenir une vue d’ensemble de l’hébergement de contrôles UWP dans des applications WPF et Windows Forms, consultez [cet article](xaml-islands.md#wpf-and-windows-forms-applications).

## <a name="overview"></a>Vue d'ensemble

Pour héberger un contrôle UWP personnalisé dans une application WPF, vous avez besoin des composants suivants. Cet article fournit des instructions sur la création de chacun de ces composants.

* **Le projet et le code source pour votre application WPF**. L’utilisation du contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) pour héberger des contrôles UWP personnalisés est prise en charge uniquement dans les applications WPF et Windows Forms qui ciblent .net Core 3. Ce scénario n’est pas pris en charge dans les applications qui ciblent le .NET Framework.

* **Contrôle UWP personnalisé**. Vous aurez besoin du code source du contrôle UWP personnalisé que vous souhaitez héberger pour pouvoir le compiler avec votre application. En règle générale, le contrôle personnalisé est défini dans un projet de bibliothèque de classes UWP que vous référencez dans la même solution que votre projet WPF (ou Windows Forms).

* **Projet d’application UWP qui définit un objet XamlApplication**. Votre projet WPF (ou Windows Forms) doit avoir accès à une instance de la `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` classe fournie par le kit de pratiques de la communauté Windows. Cet objet joue le rôle de fournisseur de métadonnées racine pour le chargement des métadonnées des types XAML UWP personnalisés dans les assemblys du répertoire actif de votre application. La méthode recommandée consiste à ajouter un projet **application vide (Windows universel)** à la même solution que votre projet WPF (ou Windows Forms) et à modifier la classe par défaut `App` de ce projet.
  > [!NOTE]
  > Votre solution ne peut contenir qu’un seul projet qui `XamlApplication` définit un objet. Tous les contrôles UWP personnalisés de votre application partagent le `XamlApplication` même objet. Le projet qui définit l' `XamlApplication` objet doit inclure des références à toutes les autres bibliothèques et projets UWP utilisés pour héberger les contrôles UWP dans l’îlot XAML.

## <a name="create-a-wpf-project"></a>Créer un projet WPF

Avant de commencer, suivez ces instructions pour créer un projet WPF et le configurer pour héberger des îlots XAML. Si vous avez un projet WPF existant, vous pouvez adapter ces étapes et des exemples de code pour votre projet.

> [!NOTE]
> Si vous avez un projet existant qui cible le .NET Framework, vous devez migrer votre projet vers .NET Core 3. Pour plus d’informations, consultez [cette série de blogs](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/).

1. Si vous ne l’avez pas déjà fait, installez la dernière version préliminaire disponible du [SDK .net Core 3 Preview](https://dotnet.microsoft.com/download/dotnet-core/3.0).

2. Dans Visual Studio 2019, créez un nouveau projet d' **application WPF (.net Core)** .

3. Vérifiez que les [références de package](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) sont activées :

    1. Dans Visual Studio, cliquez sur **Outils-> gestionnaire de package NuGet-> paramètres du gestionnaire de package**.
    2. Assurez-vous que **PackageReference** est sélectionné pour le **format de gestion des packages par défaut**.

4. Dans **Explorateur de solutions** , cliquez avec le bouton droit sur votre projet WPF, puis choisissez **gérer les packages NuGet**.

5. Dans la fenêtre **Gestionnaire de package NuGet** , assurez-vous que l’option **inclure la version préliminaire** est sélectionnée.

6. Sélectionnez l’onglet **Parcourir** , recherchez le package [Microsoft. Toolkit. WPF. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) (version v 6.0.0-preview7 ou version ultérieure) et installez le package. Ce package fournit tout ce dont vous avez besoin pour utiliser le contrôle **WindowsXamlHost** pour héberger un contrôle UWP, y compris d’autres packages NuGet associés.
    > [!NOTE]
    > Windows Forms applications doivent utiliser le package [Microsoft. Toolkit. Forms. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost) (version v 6.0.0-preview7 ou version ultérieure).

7. Configurez votre solution pour cibler une plateforme spécifique, telle que x86 ou x64. Les contrôles UWP personnalisés ne sont pas pris en charge dans les projets qui ciblent **n’importe quel processeur**.

    1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de la solution, puis sélectionnez **Propriétés** -> propriétés de**configuration** -> **Configuration Manager**. 
    2. Sous **plateforme de la solution active**, sélectionnez **nouveau**. 
    3. Dans la boîte de dialogue **nouvelle plateforme de solution** , sélectionnez **x64** ou **x86** , puis cliquez sur **OK**. 
    4. Fermez les boîtes de dialogue ouvertes.

## <a name="create-a-xamlapplication-object-in-a-uwp-app-project"></a>Créer un objet XamlApplication dans un projet d’application UWP

Ensuite, ajoutez un projet d’application UWP à la même solution que votre projet WPF. Vous allez modifier la classe par `App` défaut de ce projet pour qu’elle dérive de la `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` classe fournie par le kit de pratiques de la communauté Windows. L’objet **WindowsXamlHost** dans votre application WPF a besoin `XamlApplication` de cet objet pour héberger des contrôles UWP personnalisés.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de la solution et sélectionnez **Ajouter** -> **nouveau projet**.
2. Ajoutez un projet **Application vide (Windows universelle)** à votre solution. Assurez-vous que la version cible et la version minimale sont toutes deux définies sur **Windows 10, version 1903** ou ultérieure.
3. Dans le projet d’application UWP, installez le package NuGet [Microsoft. Toolkit. Win32. UI. XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) (version v 6.0.0-preview7 ou version ultérieure).
4. Ouvrez le fichier **app. Xaml** et remplacez le contenu de ce fichier par le code XAML suivant. Remplacez `MyUWPApp` par l’espace de noms de votre projet d’application UWP.

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
    </xaml:XamlApplication>
    ```

5. Ouvrez le fichier **app.Xaml.cs** et remplacez le contenu de ce fichier par le code suivant. Remplacez `MyUWPApp` par l’espace de noms de votre projet d’application UWP.

    ```csharp
    namespace MyUWPApp
    {
        sealed partial class App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
        {
            public App()
            {
                this.Initialize();
            }
        }
    }
    ```

6. Supprimez le fichier **MainPage. Xaml** du projet d’application UWP.
7. Générez le projet d’application UWP.
8. Dans votre projet WPF, cliquez avec le bouton droit sur le nœud **dépendances** et ajoutez une référence à votre projet d’application UWP.

## <a name="create-a-custom-uwp-control"></a>Créer un contrôle UWP personnalisé

Pour héberger un contrôle UWP personnalisé dans votre application WPF, vous devez disposer du code source du contrôle afin de pouvoir le compiler avec votre application. En général, les contrôles personnalisés sont définis dans un projet de bibliothèque de classes UWP pour faciliter la portabilité.

Dans cette section, vous allez définir un contrôle UWP personnalisé simple dans un nouveau projet de bibliothèque de classes. Vous pouvez également définir le contrôle UWP personnalisé dans le projet d’application UWP que vous avez créé dans la section précédente. Toutefois, ces étapes sont effectuées dans un projet de bibliothèque de classes distinct à des fins d’illustration, car il s’agit généralement de la façon dont les contrôles personnalisés sont implémentés pour la portabilité.

Si vous disposez déjà d’un contrôle personnalisé, vous pouvez l’utiliser à la place du contrôle présenté ici. Toutefois, vous devrez toujours configurer le projet qui contient le contrôle comme indiqué dans ces étapes.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de la solution et sélectionnez **Ajouter** -> **nouveau projet**.
2. Ajoutez un projet de **bibliothèque de classes (Windows universel)** à votre solution. Assurez-vous que la version cible et la version minimale sont toutes deux définies sur **Windows 10, version 1903** ou ultérieure.
3. Cliquez avec le bouton droit sur le fichier projet et sélectionnez **décharger le projet**. Cliquez à nouveau avec le bouton droit sur le fichier projet et sélectionnez **modifier**.
4. Avant l’élément `</Project>` de fermeture, ajoutez le code XML suivant pour désactiver plusieurs propriétés, puis enregistrez le fichier projet. Ces propriétés doivent être activées pour héberger le contrôle UWP personnalisé dans une application WPF (ou Windows Forms).

    ```xml
    <PropertyGroup>
      <EnableTypeInfoReflection>false</EnableTypeInfoReflection>
      <EnableXBindDiagnostics>false</EnableXBindDiagnostics>
    </PropertyGroup>
    ```

5. Cliquez avec le bouton droit sur le fichier projet et sélectionnez **recharger le projet**.
6. Supprimez le fichier **Class1.cs** par défaut et ajoutez un nouvel élément de **contrôle utilisateur** au projet.
7. Dans le fichier XAML du contrôle utilisateur, ajoutez ce qui suit `StackPanel` en tant qu’enfant de la `Grid`valeur par défaut. Cet exemple ajoute un ``TextBlock`` contrôle, puis lie l' ``Text`` attribut ``XamlIslandMessage`` de ce contrôle au champ.

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom UWP control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
    </StackPanel>
    ```

8. Dans le fichier code-behind du contrôle utilisateur, ajoutez le `XamlIslandMessage` champ à la classe de contrôle utilisateur, comme indiqué ci-dessous.

    ```csharp
    public sealed partial class MyUserControl : UserControl
    {
        public string XamlIslandMessage { get; set; }

        public MyUserControl()
        {
            this.InitializeComponent();
        }
    }
    ```

9. Générez le projet de bibliothèque de classes UWP.
10. Dans votre projet WPF, cliquez avec le bouton droit sur le nœud **dépendances** et ajoutez une référence au projet de bibliothèque de classes UWP.
11. Dans le projet d’application UWP que vous avez configuré précédemment, cliquez avec le bouton droit sur le nœud **références** et ajoutez une référence au projet Bibliothèque de classes UWP.
12. Régénérez l’ensemble de la solution et assurez-vous que tous les projets sont générés avec succès.

## <a name="host-the-custom-uwp-control-in-your-wpf-app"></a>Héberger le contrôle UWP personnalisé dans votre application WPF

1. Dans **Explorateur de solutions**, développez le projet WPF et ouvrez le fichier MainWindow. XAML ou une autre fenêtre dans laquelle vous souhaitez héberger le contrôle personnalisé.
2. Dans le fichier XAML, ajoutez la déclaration d’espace de noms `<Window>` suivante à l’élément.

    ```xml
    xmlns:xaml="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

3. Dans le même fichier, ajoutez le contrôle suivant à l' `<Grid>` élément. Remplacez l' `InitialTypeName` attribut par le nom qualifié complet du contrôle utilisateur dans votre projet de bibliothèque de classes UWP.

    ```xml
    <xaml:WindowsXamlHost InitialTypeName="UWPClassLibrary.MyUserControl" ChildChanged="WindowsXamlHost_ChildChanged" />
    ```

4. Ouvrez le fichier code-behind et ajoutez le code suivant à la `Window` classe. Ce code définit un `ChildChanged` gestionnaire `WPFMessage` d’événements qui assigne la valeur ``XamlIslandMessage`` du champ du contrôle personnalisé UWP à la valeur du champ dans l’application WPF. Remplacez `UWPClassLibrary.MyUserControl` par le nom qualifié complet du contrôle utilisateur dans votre projet de bibliothèque de classes UWP.

    ```csharp
    private void WindowsXamlHost_ChildChanged(object sender, EventArgs e)
    {
        // Hook up x:Bind source.
        global::Microsoft.Toolkit.Wpf.UI.XamlHost.WindowsXamlHost windowsXamlHost =
            sender as global::Microsoft.Toolkit.Wpf.UI.XamlHost.WindowsXamlHost;
        global::UWPClassLibrary.MyUserControl userControl =
            windowsXamlHost.GetUwpInternalObject() as global::UWPClassLibrary.MyUserControl;

        if (userControl != null)
        {
            userControl.XamlIslandMessage = this.WPFMessage;
        }
    }

    public string WPFMessage
    {
        get
        {
            return "Binding from WPF to UWP XAML";
        }
    }
    ```

6. Générez et exécutez votre application et vérifiez que le contrôle utilisateur UWP s’affiche comme prévu.

## <a name="add-a-control-from-the-winui-library-to-the-custom-control"></a>Ajouter un contrôle à partir de la bibliothèque WinUI au contrôle personnalisé

Traditionnellement, les contrôles UWP ont été publiés dans le cadre du système d’exploitation Windows 10 et mis à la disposition des développeurs par le biais du SDK Windows. La [bibliothèque WinUI](https://docs.microsoft.com/uwp/toolkits/winui/) est une approche alternative, dans laquelle les versions mises à jour des contrôles UWP internes du SDK Windows sont distribuées dans un package NuGet qui n’est pas lié aux versions de SDK Windows. Cette bibliothèque comprend également de nouveaux contrôles qui ne font pas partie du SDK Windows et de la plateforme UWP par défaut. Pour plus d’informations, consultez notre feuille de route de la [bibliothèque WinUI](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md) .

Cette section montre comment ajouter un contrôle UWP de la bibliothèque WinUI à votre contrôle utilisateur afin que vous puissiez héberger ce contrôle dans votre application WPF. 

1. Dans le projet d’application UWP, installez la version préliminaire la plus récente du package NuGet [Microsoft. UI. Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) .
    > [!NOTE]
    > Veillez à installer la dernière version *préliminaire* . Actuellement, seules les versions préliminaires de ce package fonctionnent si vous choisissez de créer un package pour votre application dans un [package MSIX](https://docs.microsoft.com/windows/msix) pour le déploiement.

2. Dans le fichier app. XAML de ce projet, ajoutez l’élément enfant suivant à l' `<xaml:Application>` élément.

    ```xml
    <Application.Resources>
        <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
    </Application.Resources>
    ```

    Après avoir ajouté cet élément, le contenu de ce fichier doit maintenant ressembler à ce qui suit.

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
        <Application.Resources>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
        </Application.Resources>
    </xaml:XamlApplication>
    ```

3. Dans le projet de bibliothèque de classes UWP, installez la version préliminaire la plus récente du package NuGet [Microsoft. UI. Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) (la même version que celle que vous avez installée dans le projet d’application UWP).

4. Dans le même projet, ouvrez le fichier XAML pour le contrôle utilisateur et ajoutez la déclaration d’espace de noms `<UserControl>` suivante à l’élément.

    ```xml
    xmlns:winui="using:Microsoft.UI.Xaml.Controls"
    ```

5. Dans le même fichier, ajoutez un `<winui:RatingControl />` élément en tant qu’enfant `<StackPanel>`de. Cet élément ajoute une instance de la classe [RatingControl](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.ratingcontrol?view=winui-2.2) à partir de la bibliothèque WinUI. Après avoir ajouté cet élément, `<StackPanel>` le doit maintenant ressembler à ce qui suit.

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom UWP control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
        <winui:RatingControl />
    </StackPanel>
    ```

6. Générez et exécutez votre application et vérifiez que le nouveau contrôle d’évaluation s’affiche comme prévu.

## <a name="package-the-app"></a>Empaqueter l’application

Vous pouvez éventuellement empaqueter l’application WPF dans un [package MSIX](https://docs.microsoft.com/windows/msix) pour le déploiement. MSIX est la technologie d’empaquetage d’applications moderne pour Windows et est basée sur une combinaison de technologies d’installation MSI, APPX, App-V et ClickOnce.

Les instructions suivantes vous montrent comment empaqueter tous les composants de la solution dans un package MSIX à l’aide du [projet de packaging des applications Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) dans Visual Studio 2019. Ces étapes sont nécessaires uniquement si vous souhaitez empaqueter l’application WPF dans un package MSIX. Notez que ces étapes incluent actuellement des solutions de contournement spécifiques au scénario d’hébergement de contrôles UWP personnalisés.

1. Ajoutez un nouveau [projet d’empaquetage d’applications Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) à votre solution. Au fur et à mesure que vous créez le projet, sélectionnez **Windows 10, version 1903 (10,0 ; Build 18362)** pour la **version cible** et la **version minimale**.

2. Dans le projet d’empaquetage, cliquez avec le bouton droit sur le nœud **applications** et choisissez **Ajouter une référence**. Dans la liste des projets, sélectionnez le projet WPF dans votre solution, puis cliquez sur **OK**.

3. Modifiez le fichier de projet d’empaquetage. Ces modifications sont actuellement requises pour l’empaquetage d’applications WPF qui ciblent .NET Core 3 et qui hébergent des îlots XAML.

    1. Dans Explorateur de solutions, cliquez avec le bouton droit sur le nœud du projet Packaging et sélectionnez **modifier le fichier projet**.
    2. Recherchez l’élément `<Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />` dans le fichier. Remplacez cet élément par le code XML suivant. Ces modifications sont actuellement requises pour l’empaquetage d’applications WPF qui ciblent .NET Core 3 et qui hébergent des contrôles UWP.

        ``` xml
        <ItemGroup>
            <SDKReference Include="Microsoft.VCLibs,Version=14.0">
            <TargetedSDKConfiguration Condition="'$(Configuration)'!='Debug'">Retail</TargetedSDKConfiguration>
            <TargetedSDKConfiguration Condition="'$(Configuration)'=='Debug'">Debug</TargetedSDKConfiguration>
            <TargetedSDKArchitecture>$(PlatformShortName)</TargetedSDKArchitecture>
            <Implicit>true</Implicit>
            </SDKReference>
        </ItemGroup>
        <Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />
        <Target Name="_StompSourceProjectForWapProject" BeforeTargets="_ConvertItems">
            <ItemGroup>
            <_TemporaryFilteredWapProjOutput Include="@(_FilteredNonWapProjProjectOutput)" />
            <_FilteredNonWapProjProjectOutput Remove="@(_TemporaryFilteredWapProjOutput)" />
            <_FilteredNonWapProjProjectOutput Include="@(_TemporaryFilteredWapProjOutput)">
                <SourceProject></SourceProject>
                <TargetPath Condition="'%(FileName)%(Extension)'=='resources.pri'">app_resources.pri</TargetPath>
            </_FilteredNonWapProjProjectOutput>
            </ItemGroup>
        </Target>
        ```

    3. Enregistrez le fichier projet et fermez-le.

4. Modifiez le manifeste du package pour faire référence à l’image d’écran de démarrage par défaut appropriée. Cette solution de contournement est actuellement requise pour l’empaquetage d’applications WPF qui hébergent des contrôles UWP personnalisés.

    1. Dans le projet d’empaquetage, cliquez avec le bouton droit sur le fichier **Package. appxmanifest** , puis cliquez sur **afficher le code**.
    2. Localisez l’élément suivant dans le fichier.

        ```<uap:SplashScreen Image="Images\SplashScreen.png" />```

    3. Remplacez cet élément par :

        ```<uap:SplashScreen Image="Images\SplashScreen.scale-200.png" />```

    4. Enregistrez le fichier **Package. appxmanifest** et fermez-le.

5. Modifiez le fichier projet WPF. Ces modifications sont actuellement requises pour l’empaquetage d’applications WPF qui hébergent des contrôles UWP personnalisés.

    1. Dans Explorateur de solutions, cliquez avec le bouton droit sur le nœud de projet WPF et sélectionnez **décharger le projet**.
    2. Cliquez avec le bouton droit sur le nœud de projet WPF et sélectionnez **modifier**.
    3. Recherchez la dernière `</PropertyGroup>` balise de fermeture dans le fichier, puis ajoutez le code XML suivant immédiatement après cette balise.

        ``` xml
        <PropertyGroup>
          <AssetTargetFallback>uap10.0.18362</AssetTargetFallback>
        </PropertyGroup>
        ```

    4. Enregistrez le fichier projet et fermez-le.
    5. Cliquez avec le bouton droit sur le nœud de projet WPF et choisissez **recharger le projet**.

6. Générez et exécutez le projet de Packaging. Vérifiez que WPF s’exécute et que le contrôle personnalisé UWP s’affiche comme prévu.

## <a name="related-topics"></a>Rubriques connexes

* [Contrôles UWP dans les applications de bureau](xaml-islands.md)
* [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)
