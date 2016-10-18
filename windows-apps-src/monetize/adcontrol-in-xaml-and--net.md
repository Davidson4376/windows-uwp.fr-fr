---
author: mcleanbyron
ms.assetid: 4e7c2388-b94e-4828-a104-14fa33f6eb2d
description: "Découvrez comment utiliser la classe AdControl pour afficher des bannières publicitaires dans une application XAML pour Windows10 (UWP), Windows8.1 ou Windows Phone8.1."
title: AdControl en XAML et .NET
translationtype: Human Translation
ms.sourcegitcommit: 2f0835638f330de0ac2d17dae28347686cc7ed97
ms.openlocfilehash: e3cc04e2c039223729a1e24224ddd19d6485d434

---

# AdControl en XAML et .NET




Cette procédure pas à pas montre comment utiliser la classe [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) pour afficher des bannières publicitaires dans une application XAML pour Windows10 (UWP), Windows8.1 ou Windows Phone8.1. Cette procédure pas à pas n’utilise ni **AdMediatorControl** ni la médiation publicitaire.

Pour un exemple de projet complet illustrant l’ajout de bannières publicitaires à une application XAML en C# et C++, voir [Exemples de publicité sur GitHub](http://aka.ms/githubads).

## Conditions préalables

* Pour les applications UWP: installez [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) avec Visual Studio2015.
* Pour les applicationsWindows8.1 ou WindowsPhone8.1: [installez le Kit de développement logiciel (SDK) Microsoft Advertising pour Windows et Windows Phone8.x](http://aka.ms/store-8-sdk) avec Visual Studio2015 ou Visual Studio2013.

## Développement du code

1. Dans Visual Studio, ouvrez votre projet ou créez-en un.

2. Si votre projet cible **Toute UC**, mettez-le à jour pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Si votre projet cible **Toute UC**, vous ne pourrez pas ajouter une référence à la bibliothèque de publicités Microsoft dans les étapes suivantes. Pour plus d’informations, voir [Erreurs de référence provoquées par le ciblage de Toute UC dans votre projet](known-issues-for-the-advertising-libraries.md#reference_errors).

1.  Dans la fenêtre **Explorateur de solutions**, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence.**

2.  Dans **Gestionnaire de références**, sélectionnez l’une des références suivantes en fonction de votre type de projet:

    -   Pour un projet de plateforme Windows universelle (UWP): développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour XAML** (version10.0).

    -   Pour un projet Windows8.1: développez **Windows8.1**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Ad Mediator pour Windows8.1 XAML**. Cette option permet d’ajouter les bibliothèques de publicités et de médiateurs publicitaires Microsoft à votre projet, mais vous pouvez ignorer les bibliothèques de médiateurs publicitaires.

    -   Pour un projet Windows Phone8.1: développez **Windows Phone8.1**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Ad Mediator pour Windows Phone8.1 XAML**. Cette option permet d’ajouter les bibliothèques de publicités et de médiateurs publicitaires Microsoft à votre projet, mais vous pouvez ignorer les bibliothèques de médiateurs publicitaires.

  ![addreferences](images/13-a84c026e-b283-44f2-8816-f950a1ef89aa.png)

    > **Remarque** Cette image correspond à la génération d’un projet UWP pour Windows10 à l’aide de Visual Studio2015. Si vous générez une application Windows8.1 ou Windows Phone8.1 à l’aide de Visual Studio2013, votre écran aura une apparence différente.

3.  Dans **Gestionnaire de références**, cliquez sur OK.
4.  Modifiez le code XAML de la page où vous incorporez des publicités pour inclure l’espace de noms **Microsoft.Advertising.WinRT.UI**. Par exemple, dans l’exemple d’application par défaut généré par Visual Studio (nommé, dans cette application, MyAdFundedWindows10AppXAML), la page XAML est **MainPage.XAML**.

    La section **Page** du fichier MainPage.xaml généré par Visual Studio contient le code suivant.

    ``` syntax
    <Page
        x:Class="MyAdFundedWindows10AppXAML.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:MyAdFundedWindows10AppXAML"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

        </Grid>
    </Page>
    ```

    Ajoutez la référence d’espace de noms **Microsoft.Advertising.WinRT.UI** pour que la section **Page** du fichier MainPage.xaml contienne le code suivant.

    ``` syntax
    <Page
        x:Class="MyAdFundedWindows10AppXAML.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:MyAdFundedWindows10AppXAML"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:UI="using:Microsoft.Advertising.WinRT.UI"
        mc:Ignorable="d">

        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

        </Grid>
    </Page>
    ```

5.  Dans la balise **Grid**, ajoutez le code correspondant à la classe **AdControl**.

    1.  Affectez les propriétés [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) et [AdUnitId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.adunitid.aspx) de la section **Page** aux valeurs de test indiquées dans [Valeurs du mode test](test-mode-values.md).

        > **Remarque** Vous allez remplacer les valeurs de test par les valeurs dynamiques avant de soumettre votre application.

    2.  Ajustez la hauteur et la largeur du contrôle afin qu’il corresponde à l’une des [tailles de publicités prises en charge pour les bannières publicitaires](supported-ad-sizes-for-banner-ads.md).

    La balise **Grid** complète ressemble à ce code.

    ``` syntax
    <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">

            <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
                          AdUnitId="10865270"
                          HorizontalAlignment="Left"
                          Height="250"
                          VerticalAlignment="Top"
                          Width="300"/>
    </Grid>
    ```

    Le code complet du fichier MainPage.xaml doit ressembler à ceci.

    ``` syntax
    <Page
        x:Class="MyAdFundedWindows10AppXAML.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:MyAdFundedWindows10AppXAML"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:UI="using:Microsoft.Advertising.WinRT.UI"
        mc:Ignorable="d">

        <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">

            <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
                          AdUnitId="10865270"
                          HorizontalAlignment="Left"
                          Height="250"
                          VerticalAlignment="Top"
                          Width="300"/>
        </Grid>
    </Page>
    ```

6.  Compilez et exécutez l’application pour la voir avec une publicité.

## Publier l’application avec des publicités dynamiques via le Centre de développement Windows


1.  Dans le tableau de bord du Centre de développement, accédez à la page **Monétisation** &gt; **Monétiser avec des publicités** de votre application, puis [créez une unité Microsoft Advertising autonome](../publish/monetize-with-ads.md). Pour le type d’unité publicitaire, spécifiez **Bannière**. Prenez note de l’ID d’unité publicitaire et de l’ID de l’application.

2.  Dans votre code, remplacez les valeurs de test de l’unité publicitaire (**ApplicationId** et **AdUnitId**) par les valeurs dynamiques que vous avez générées dans le Centre de développement.

3.  [Soumettez votre application](../publish/app-submissions.md) au Windows Store à l’aide du tableau de bord du Centre de développement.

4.  Passez en revue vos [rapports de performances des publicités](../publish/advertising-performance-report.md) dans le tableau de bord du Centre de développement.

## Remarques

C#: consultez l’article [Exemple de propriétés XAML](xaml-properties-example.md) pour voir comment affecter des gestionnaires d’événements aux événements **AdControl**. Vous pouvez ensuite consulter [Événements AdControl en C#](adcontrol-events-in-c.md) pour obtenir un exemple de code illustrant des gestionnaires d’événements écrits en C#.

Visual Basic: consultez l’article [Exemple de propriétés XAML](xaml-properties-example.md) pour voir comment affecter des gestionnaires d’événements aux événements **AdControl**.

C++: la version actuelle des bibliothèques de publicités Microsoft prend en charge C++. La classe **AdControl** charge CLR et utilise C++ managé.

Gestion des erreurs: pour en savoir plus sur la gestion des erreurs, voir [Gestion des erreurs AdControl](adcontrol-error-handling.md).

## Rubriques connexes

* [Exemples de publicité sur GitHub](http://aka.ms/githubads)

 



<!--HONumber=Sep16_HO2-->


