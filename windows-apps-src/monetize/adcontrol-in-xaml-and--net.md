---
author: mcleanbyron
ms.assetid: 4e7c2388-b94e-4828-a104-14fa33f6eb2d
description: "Découvrez comment utiliser la classe AdControl pour afficher des bannières publicitaires dans une application XAML pour Windows10 (UWP), Windows8.1 ou Windows Phone8.1."
title: AdControl en XAML et .NET
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows10, uwp, annonces, publicité, AdControl, contrôle de publicité, XAML, .net, procédure pas à pas"
ms.openlocfilehash: be273ca4c17edb4affa5e0abb4b3317b03893280
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/27/2017
---
# <a name="adcontrol-in-xaml-and-net"></a>AdControl en XAML et .NET


Cette procédure pas à pas montre comment utiliser la classe [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) pour afficher des bannières publicitaires dans une application XAML pour Windows10 (UWP), Windows8.1 ou Windows Phone8.1.

Pour un exemple de projet complet illustrant l’ajout de bannières publicitaires à une application XAML en C# et C++, voir [Exemples de publicité sur GitHub](http://aka.ms/githubads).

## <a name="prerequisites"></a>Conditions préalables

* Pour les applications UWP: installez le [SDK MicrosoftAdvertising](http://aka.ms/ads-sdk-uwp) avec VisualStudio2015 ou version ultérieure.
* Pour les applicationsWindows8.1 ou WindowsPhone8.1: [installe Microsoft Advertising SDK pour Windows et Windows Phone8.x](http://aka.ms/store-8-sdk) avec Visual Studio2015 ou Visual Studio2013.

## <a name="code-development"></a>Développement du code

1. Dans Visual Studio, ouvrez votre projet ou créez-en un.

2. Si votre projet cible **Toute UC**, mettez-le à jour pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Si votre projet cible **Toute UC**, vous ne pourrez pas ajouter une référence à la bibliothèque de publicités Microsoft dans les étapes suivantes. Pour plus d’informations, voir [Erreurs de référence provoquées par le ciblage de Toute UC dans votre projet](known-issues-for-the-advertising-libraries.md#reference_errors).

1.  Dans la fenêtre **Explorateur de solutions**, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence.**

2.  Dans **Gestionnaire de références**, sélectionnez l’une des références suivantes en fonction de votre type de projet:

    -   Pour un projet de plateforme Windows universelle (UWP): développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour XAML** (version10.0).

    -   Pour un projet Windows8.1: développez **Windows8.1**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Ad Mediator pour Windows8.1 XAML**. Cette option permet d’ajouter les bibliothèques de publicités et de médiateurs publicitaires Microsoft à votre projet, mais vous pouvez ignorer les bibliothèques de médiateurs publicitaires.

    -   Pour un projet Windows Phone8.1: développez **Windows Phone8.1**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Ad Mediator pour Windows Phone8.1 XAML**. Cette option permet d’ajouter les bibliothèques de publicités et de médiateurs publicitaires Microsoft à votre projet, mais vous pouvez ignorer les bibliothèques de médiateurs publicitaires.

    ![addreferences](images/13-a84c026e-b283-44f2-8816-f950a1ef89aa.png)

3.  Dans **Gestionnaire de références**, cliquez sur OK.

4.  Modifiez le code XAML de la page où vous incorporez des publicités pour inclure l’espace de noms **Microsoft.Advertising.WinRT.UI**. Par exemple, dans l’exemple d’application par défaut généré par Visual Studio (nommé, dans cette application, MyAdFundedWindows10AppXAML), la page XAML est **MainPage.XAML**.

    La section **Page** du fichier MainPage.xaml généré par Visual Studio contient le code suivant.

    ``` xml
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

    ``` xml
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

5. Dans la balise **Grid**, ajoutez le code correspondant à la classe **AdControl**. Affectez les propriétés [AdUnitId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.adunitid.aspx) et [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) de la section **Page** aux valeurs de test indiquées dans [Valeurs du mode test](test-mode-values.md). Ajustez également la hauteur et la largeur de la commande pour qu’elle corresponde à l’une des [tailles de publicités prises en charge pour les bannières publicitaires](supported-ad-sizes-for-banner-ads.md).

    > [!NOTE]
    > Chaque **AdControl** est associé à une *unité publicitaire* qui est utilisée par nos services pour servir des publicités au contrôle, et chaque unité publicitaire se compose d’un *ID d'unité publicitaire* et d'un *ID d'application*. Dans ces étapes, vous attribuez à votre contrôle des valeurs de test ID d’unité publicitaire et ID d'application. Ces valeurs de test ne peuvent être utilisées que dans une version de test de votre application. Avant de publier votre application sur le Windows Store, vous devez [remplacer ces valeurs de test par des valeurs dynamiques](#release) du Centre de développement Windows.

    La balise **Grid** complète ressemble à ce code.

    ``` xml
    <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
            AdUnitId="test"
            HorizontalAlignment="Left"
            Height="250"
            VerticalAlignment="Top"
            Width="300"/>
    </Grid>
    ```

    Le code complet du fichier MainPage.xaml doit ressembler à ceci.

    ``` xml
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
                  AdUnitId="test"
                  HorizontalAlignment="Left"
                  Height="250"
                  VerticalAlignment="Top"
                  Width="300"/>
      </Grid>
    </Page>
    ```

6.  Compilez et exécutez l’application pour la voir avec une publicité.

<span id="release" />
## <a name="release-your-app-with-live-ads-using-windows-dev-center"></a>Publier l’application avec des publicités dynamiques via le Centre de développement Windows

1.  Dans le tableau de bord du Centre de développement, accédez à la page [Monétiser avec des publicités](../publish/monetize-with-ads.md) de votre application, puis [créez une unité publicitaire](../monetize/set-up-ad-units-in-your-app.md). Pour le type d’unité publicitaire, spécifiez **Bannière**. Prenez note de l’ID d’unité publicitaire et de l’ID de l’application.

2. Si votre application est de type UWP pour Windows10, vous pouvez éventuellement activer la médiation publicitaire pour la commande **AdControl** en configurant les paramètres dans la section [Médiation publicitaire](../publish/monetize-with-ads.md#mediation) sur la page [Monétiser avec des publicités](../publish/monetize-with-ads.md). La médiation publicitaire vous permet d’optimiser vos revenus publicitaires et vos capacités de promotion d’application en affichant des spots issus de plusieurs réseaux publicitaires, y compris les publicités d’autres réseaux payants tels que Taboola et Smaato et les publicités des campagnes de promotion d’applications Microsoft.

3.  Dans votre code, remplacez les valeurs de test de l’unité publicitaire (**ApplicationId** et **AdUnitId**) par les valeurs dynamiques que vous avez générées dans le Centre de développement.

4.  [Soumettez votre application](../publish/app-submissions.md) au Windows Store à l’aide du tableau de bord du Centre de développement.

5.  Passez en revue vos [rapports de performances des publicités](../publish/advertising-performance-report.md) dans le tableau de bord du Centre de développement.

<span id="manage" />
## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Gérer des unités publicitaires pour plusieurs contrôles publicitaires dans votre application

Vous pouvez utiliser plusieurs objets **AdControl** dans une seule application (par exemple, chaque page de votre application peut héberger un objet **AdControl** différent). Dans ce scénario, nous vous recommandons d’attribuer une unité publicitaire différente à chaque contrôle. L’utilisation de différentes unités publicitaires pour chaque contrôle vous permet de [configurer les paramètres de médiation](../publish/monetize-with-ads.md#mediation) séparément et d'obtenir des [données de rapport](../publish/advertising-performance-report.md) discrètes pour chaque contrôle. Cela permet également à nos services de mieux optimiser les publicités que nous servons à votre application.

> [!IMPORTANT]
> Vous pouvez utiliser chaque unité publicitaire dans une seule application. Si vous utilisez une unité publicitaire dans plusieurs applications, les publicités ne seront pas servies à cette unité publicitaire.

## <a name="notes"></a>Remarques

* C#: consultez l’article [Exemple de propriétés XAML](xaml-properties-example.md) pour voir comment affecter des gestionnaires d’événements aux événements **AdControl**. Vous pouvez ensuite consulter [Événements AdControl en C#](adcontrol-events-in-c.md) pour obtenir un exemple de code illustrant des gestionnaires d’événements écrits en C#.

* C++: la version actuelle des bibliothèques de publicités Microsoft prend en charge C++. La classe **AdControl** est implémentée dans le code C++ natif et ne charge pas .NET CLR. Pour obtenir des exemples de code qui illustrent l’utilisation de **AdControl** en C++, voir [Exemples de publicité sur GitHub](http://aka.ms/githubads).

* Visual Basic: consultez l’article [Exemple de propriétés XAML](xaml-properties-example.md) pour voir comment affecter des gestionnaires d’événements aux événements **AdControl**.

* Gestion des erreurs: pour en savoir plus sur la gestion des erreurs, voir [Gestion des erreurs AdControl](adcontrol-error-handling.md).

## <a name="related-topics"></a>Rubriques connexes

* [Exemples de publicité sur GitHub](http://aka.ms/githubads)
* [Configurer des unités publicitaires pour votre application](../monetize/set-up-ad-units-in-your-app.md)
