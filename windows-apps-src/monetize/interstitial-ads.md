---
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: Découvrez comment insérer des spots publicitaires dans une application UWP pour Windows10 à l’aide du SDK MicrosoftAdvertising.
title: Spots
ms.date: 03/22/2018
ms.topic: article
keywords: Windows10, uwp, pub, publicité, contrôle de publicités, spot
ms.localizationpriority: medium
ms.openlocfilehash: c1860fe51035699aaa55d014c2f76a95c7622061
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8741857"
---
# <a name="interstitial-ads"></a>Spots

Cette procédure pas à pas montre comment insérer des spots publicitaires dans les jeux et les applications de plateforme Windows universelle (UWP) pour Windows10. Pour obtenir des exemples complets de projet qui montrent comment ajouter des spots publicitaires à des applications JavaScript/HTML et XAML en C# et C++, voir [Exemples de publicité sur GitHub](http://aka.ms/githubads).

<span id="whatareinterstitialads10"/>

## <a name="what-are-interstitial-ads"></a>Que sont les spots publicitaires?

Contrairement aux bannières standard, qui sont limitées à une partie d’une interface utilisateur dans une application ou un jeu, les spots s'affichent sur l’écran entier. Deuxformes de base sont fréquemment utilisées dans les jeux.

* Avec les publicités de type *Paywall*, l’utilisateur doit regarder une publicité à intervalles réguliers. Par exemple entre les niveaux de jeu:

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* Avec les publicités *basées sur les récompenses*, l’utilisateur recherche explicitement certains avantages, par exemple un conseil ou du temps supplémentaire pour terminer le niveau, et initialise la publicité par le biais de l’interface utilisateur de l’application.

Nous proposons deux types de spots publicitaires à utiliser dans vos applications et vos jeux: **spots publicitaires vidéos** et **bannières spots publicitaires**.

> [!NOTE]
> L’API pour les spots ne gère pas d’interface utilisateur sauf au moment de la lecture vidéo. Reportez-vous aux [meilleures pratiques Spots](ui-and-user-experience-guidelines.md#interstitialbestpractices10) pour obtenir des recommandations sur les choses à faire et à ne pas faire, si vous envisagez d’intégrer des spots publicitaires dans votre application.

## <a name="prerequisites"></a>Éléments prérequis

* Installer le [SDK MicrosoftAdvertising](http://aka.ms/ads-sdk-uwp) avec VisualStudio2015 ou une version ultérieure de Visual Studio. Pour des instructions d’installation, voir [cet article](install-the-microsoft-advertising-libraries.md).

## <a name="integrate-an-interstitial-ad-into-your-app"></a>Intégrer un spot publicitaire dans votre application

Pour afficher des spots publicitaires dans votre application, suivez les instructions correspondant au type de projet:

* [XAML/.NET](#interstitialadsxaml10)
* [HTML/JavaScript](#interstitialadshtml10)
* [C++ (technologie interop DirectX)](#interstitialadsdirectx10)

<span id="interstitialadsxaml10"/>

### <a name="xamlnet"></a>XAML/.NET

Cette section fournit des exemples en C#, mais VisualBasic et C++ sont également pris en charge pour les projets XAML/.NET. Pour voir un exemple de code complet en C#, consultez [Exemple de code pour spot publicitaire en C#](interstitial-ad-sample-code-in-c.md).

1. Ouvrez votre projet dans Visual Studio.
    > [!NOTE]
    > Si vous utilisez un projet existant, ouvrez le fichier Package.appxmanifest dans votre projet et assurez-vous que la fonctionnalité **Internet (Client)** est sélectionnée. Votre application a besoin de cette fonctionnalité pour recevoir des publicités tests et des publicités dynamiques.

2. Si votre projet cible **Toute UC**, mettez-le à jour pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Si votre projet cible **Toute UC**, vous ne pourrez pas ajouter une référence à la bibliothèque de publicités Microsoft dans les étapes suivantes. Pour plus d’informations, voir [Erreurs de référence provoquées par le ciblage de Toute UC dans votre projet](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Ajoutez une référence au SDK Microsoft Advertising de votre projet:

    1. Dans la fenêtre **Explorateur de solutions**, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence...**
    2.  Dans **Gestionnaire de références**, développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **SDK Microsoft Advertising pour XAML** (version10.0).
    3.  Dans **Gestionnaire de références**, cliquez sur OK.

3.  Dans le fichier de code approprié de votre application (par exemple, dans MainPage.xaml.cs ou dans un fichier de code d’une autre page), ajoutez la référence d’espace de noms ci-dessous.

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet1)]

4.  À un emplacement approprié de votre application (par exemple, dans ```MainPage``` ou dans une autre page), déclarez un objet [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) et plusieurs champs de chaîne représentant l’ID de l’application et l’ID d’unité publicitaire de votre spot publicitaire. L’exemple de code suivant affecte les champs `myAppId` et `myAdUnitId` aux [valeurs de test](set-up-ad-units-in-your-app.md#test-ad-units) des spots publicitaires.

    > [!NOTE]
    > Chaque **InterstitialAd** est associé à une *unité publicitaire* qui est utilisée par nos services pour servir des publicités au contrôle, et chaque unité publicitaire se compose d’un *ID d'unité publicitaire* et d'un *ID d'application*. Dans ces étapes, vous attribuez à votre contrôle des valeurs de test ID d’unité publicitaire et ID d'application. Ces valeurs de test ne peuvent être utilisées que dans une version de test de votre application. Avant de publier votre application au Windows Store, vous devez [remplacer ces valeurs par des valeurs dynamiques de test](#release) à partir de l’espace partenaires.

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet2)]

5.  Dans le code exécuté au démarrage (par exemple, dans le constructeur de la page), instanciez l’objet **InterstitialAd** et connectez les gestionnaires d’événements correspondant aux événements de l’objet.

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet3)]

6.  Si vous souhaitez afficher un *spot vidéo*: récupérez d’abord le spot publicitaire à l’aide de la méthode [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) environ 30 à 60secondes avant que vous n’en ayez besoin. Cela laisse suffisamment de temps pour demander et préparer la publicité avant qu’elle ne soit affichée. Veillez à spécifier **AdType.Video** pour le type d’annonce.

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet4)]

    Si vous souhaitez afficher un *spot de bannière* publicitaire: récupérez d’abord le spot publicitaire à l’aide de la méthode [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) environ 5 à 8secondes avant que vous n’en ayez besoin. Cela laisse suffisamment de temps pour demander et préparer la publicité avant qu’elle ne soit affichée. Veillez à spécifier **AdType.Display** pour le type d’annonce.

    ```csharp
    myInterstitialAd.RequestAd(AdType.Display, myAppId, myAdUnitId);
    ```

6.  Dans votre code, là où vous souhaitez afficher le spot vidéo ou la bannière publicitaire, vérifiez que l’objet **InterstitialAd** est prêt à être affiché, puis affichez-le à l’aide de la méthode [Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show).

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet5)]

7.  Définissez les gestionnaires d’événements pour l’objet **InterstitialAd**.

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet6)]

8.  Générez et testez votre app pour vérifier qu’elle affiche les publicités de test.

<span id="interstitialadshtml10"/>

### <a name="htmljavascript"></a>HTML/JavaScript

Les instructions ci-dessous partent du principe que vous avez créé un projet Windows universel pour JavaScript dans VisualStudio et que vous ciblez un processeur spécifique. Pour voir un exemple de code complet, consultez [Exemple de code pour spot publicitaire en JavaScript](interstitial-ad-sample-code-in-javascript.md).

1. Ouvrez votre projet dans Visual Studio.

2. Si votre projet cible **Toute UC**, mettez-le à jour pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Si votre projet cible **Toute UC**, vous ne pourrez pas ajouter une référence à la bibliothèque de publicités Microsoft dans les étapes suivantes. Pour plus d’informations, voir [Erreurs de référence provoquées par le ciblage de Toute UC dans votre projet](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Ajoutez une référence au SDK Microsoft Advertising de votre projet:

    1. Dans la fenêtre **Explorateur de solutions**, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence...**
    2.  Dans **Gestionnaire de références**, développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour JavaScript** (version10.0).
    3.  Dans **Gestionnaire de références**, cliquez sur OK.

3.  Dans la section **&lt;head&gt;** du fichier HTML du projet, après les références JavaScript des fichiers default.css et default.js du projet, ajoutez la référence à ad.js.

    ``` HTML
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

4.  Dans un fichier .js de votre projet, déclarez un objet [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) et plusieurs champs contenant l’ID de l’application et l’ID d’unité publicitaire de votre spot publicitaire. L’exemple de code suivant affecte les champs `applicationId` et `adUnitId` aux [valeurs de test](set-up-ad-units-in-your-app.md#test-ad-units) des spots publicitaires.

    > [!NOTE]
    > Chaque **InterstitialAd** est associé à une *unité publicitaire* qui est utilisée par nos services pour servir des publicités au contrôle, et chaque unité publicitaire se compose d’un *ID d'unité publicitaire* et d'un *ID d'application*. Dans ces étapes, vous attribuez à votre contrôle des valeurs de test ID d’unité publicitaire et ID d'application. Ces valeurs de test ne peuvent être utilisées que dans une version de test de votre application. Avant de publier votre application au Windows Store, vous devez [remplacer ces valeurs par des valeurs dynamiques de test](#release) à partir de l’espace partenaires.

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet1)]

5.  Dans le code exécuté au démarrage (par exemple, dans le constructeur de la page), instanciez l’objet **InterstitialAd** et connectez les gestionnaires d’événements de l’objet.

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet2)]

5. Si vous souhaitez afficher un *spot vidéo*: récupérez d’abord le spot publicitaire à l’aide de la méthode [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) environ 30 à 60secondes avant que vous n’en ayez besoin. Cela laisse suffisamment de temps pour demander et préparer la publicité avant qu’elle ne soit affichée. Veillez à spécifier **InterstitialAdType.video** pour le type d’annonce.

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet3)]

    Si vous souhaitez afficher un *spot de bannière* publicitaire: récupérez d’abord le spot publicitaire à l’aide de la méthode [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) environ 5 à 8secondes avant que vous n’en ayez besoin. Cela laisse suffisamment de temps pour demander et préparer la publicité avant qu’elle ne soit affichée. Veillez à spécifier **InterstitialAdType.display** pour le type d’annonce.

    ```js
    if (interstitialAd) {
        interstitialAd.requestAd(MicrosoftNSJS.Advertising.InterstitialAdType.display, applicationId, adUnitId);
    }
    ```

6.  Dans votre code, là où vous souhaitez afficher la publicité, vérifiez que l’objet **InterstitialAd** est prêt à être affiché, puis affichez-le à l’aide de la méthode [Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show).

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet4)]

7.  Définissez les gestionnaires d’événements pour l’objet **InterstitialAd**.

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet5)]

9.  Générez et testez votre app pour vérifier qu’elle affiche les publicités de test.

<span id="interstitialadsdirectx10"/>

### <a name="c-directx-interop"></a>C++ (technologie interop DirectX)

Cet exemple part du principe que vous avez créé un projet C++ **application DirectX et XAML (Windows universel)** dans VisualStudio et que vous ciblez une architectureUC spécifique.
 
1. Ouvrez votre projet dans Visual Studio.

3. Ajoutez une référence au SDK Microsoft Advertising de votre projet:

    1. Dans la fenêtre **Explorateur de solutions**, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence...**
    2.  Dans **Gestionnaire de références**, développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **SDK Microsoft Advertising pour XAML** (version10.0).
    3.  Dans **Gestionnaire de références**, cliquez sur OK.

2.  Dans un fichier d’en-tête approprié de votre app (par exemple, DirectXPage.xaml.h), déclarez un objet [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) et les méthodes de gestionnaire d’événements associées.  

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet1)]

3.  Dans le même fichier d’en-tête, déclarez plusieurs champs de chaîne représentant l’ID de l’application et l’ID d’unité publicitaire de votre spot publicitaire. L’exemple de code suivant affecte les champs `myAppId` et `myAdUnitId` aux [valeurs de test](set-up-ad-units-in-your-app.md#test-ad-units) des spots publicitaires.

    > [!NOTE]
    > Chaque **InterstitialAd** est associé à une *unité publicitaire* qui est utilisée par nos services pour servir des publicités au contrôle, et chaque unité publicitaire se compose d’un *ID d'unité publicitaire* et d'un *ID d'application*. Dans ces étapes, vous attribuez à votre contrôle des valeurs de test ID d’unité publicitaire et ID d'application. Ces valeurs de test ne peuvent être utilisées que dans une version de test de votre application. Avant de publier votre application au Windows Store, vous devez [remplacer ces valeurs par des valeurs dynamiques de test](#release) à partir de l’espace partenaires.

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet2)]

4.  Dans le fichier .cpp dans lequel vous voulez ajouter du code pour afficher un spot publicitaire, ajoutez la référence d’espace de noms ci-dessous. Les exemples suivants partent du principe que vous ajoutez le code dans le fichier DirectXPage.xaml.cpp de votre app.

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet3)]

6.  Dans le code exécuté au démarrage (par exemple, dans le constructeur de la page), instanciez l’objet **InterstitialAd** et connectez les gestionnaires d’événements correspondant aux événements de l’objet. Dans l’exemple suivant, ```InterstitialAdSamplesCpp``` est l’espace de noms de votre projet; modifiez ce nom au besoin pour votre code.

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet4)]

7. Si vous souhaitez afficher un *spot vidéo*: récupérez d’abord le spot à l’aide de la méthode [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) environ 30 à 60secondes avant que vous n’en ayez besoin. Cela laisse suffisamment de temps pour demander et préparer la publicité avant qu’elle ne soit affichée. Veillez à spécifier **AdType::Video** pour le type d’annonce.

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet5)]

    Si vous souhaitez afficher un *spot de bannière* publicitaire: récupérez d’abord le spot publicitaire à l’aide de la méthode [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) environ 5 à 8secondes avant que vous n’en ayez besoin. Cela laisse suffisamment de temps pour demander et préparer la publicité avant qu’elle ne soit affichée. Veillez à spécifier **AdType::Display** pour le type d’annonce.

    ```cpp
    m_interstitialAd->RequestAd(AdType::Display, myAppId, myAdUnitId);
    ```

7.  Dans votre code, là où vous souhaitez afficher la publicité, vérifiez que l’objet **InterstitialAd** est prêt à être affiché, puis affichez-le à l’aide de la méthode [Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show).

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet6)]

8.  Définissez les gestionnaires d’événements pour l’objet **InterstitialAd**.

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet7)]

9. Générez et testez votre app pour vérifier qu’elle affiche les publicités de test.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Publier votre app avec des publicités dynamiques

1. Assurez-vous que votre utilisation des spots publicitaires dans votre app respecte nos [recommandations en matière de spots publicitaires](ui-and-user-experience-guidelines.md#interstitialbestpractices10).

2.  Dans l’espace partenaires, accédez à la page [publicités In-app](../publish/in-app-ads.md) et [créer une unité publicitaire](set-up-ad-units-in-your-app.md#live-ad-units). Pour le type d’unité publicitaire, choisissez **Spot vidéo** ou **Bannière**, en fonction du type de spot que vous diffusez. Prenez note de l’ID d’unité publicitaire et de l’ID de l’application.
    > [!NOTE]
    > Les valeurs d'ID d’application pour les unités publicitaires de test et les unités publicitaires dynamiques UWP ont des formats différents. Les valeurs d’ID d'application tests sont des GUID. Lorsque vous créez une unité de publicité dynamique UWP dans l’espace partenaires, la valeur de ID d’application pour l’unité publicitaire correspond toujours à l’ID Windows Store pour votre application (une valeur d’ID Windows Store exemple ressemble à 9NBLGGH4R315).

3. Vous pouvez éventuellement activer la médiation publicitaire pour l'**InterstitialAd** en configurant les paramètres de la section [Paramètres de médiation](../publish/in-app-ads.md#mediation) sur la page [Publicités In-app](../publish/in-app-ads.md). La médiation publicitaire vous permet d’optimiser vos revenus publicitaires et vos capacités de promotion d’application en affichant des spots issus de plusieurs réseaux publicitaires, y compris les publicités d’autres réseaux payants tels que Taboola et Smaato et les publicités des campagnes de promotion d’applications Microsoft.

4.  Dans votre code, remplacez les mêmes valeurs avec les valeurs dynamiques que vous avez générées dans l’espace partenaires.

5.  [Soumettre votre application](../publish/app-submissions.md) au Store à l’aide de l’espace partenaires.

6.  Passez en revue vos [rapports de performances des publicités](../publish/advertising-performance-report.md) dans l’espace partenaires.

<span id="manage" />

## <a name="manage-ad-units-for-multiple-interstitial-ad-controls-in-your-app"></a>Gérer des unités publicitaires pour plusieurs contrôles de spot publicitaire dans votre application

Vous pouvez utiliser plusieurs contrôles **InterstitialAd** dans une seule application. Dans ce scénario, nous vous recommandons d’attribuer une unité publicitaire différente à chaque contrôle. L’utilisation de différentes unités publicitaires pour chaque contrôle vous permet de [configurer les paramètres de médiation](../publish/in-app-ads.md#mediation) séparément et d’obtenir des [données de rapport](../publish/advertising-performance-report.md) discrètes pour chaque contrôle. Cela permet également à nos services de mieux optimiser les publicités que nous servons à votre application.

> [!IMPORTANT]
> Vous pouvez utiliser chaque unité publicitaire dans une seule application. Si vous utilisez une unité publicitaire dans plusieurs applications, les publicités ne seront pas servies à cette unité publicitaire.

## <a name="related-topics"></a>Rubriques associées

* [Recommandations pour les spots publicitaires](ui-and-user-experience-guidelines.md#interstitialbestpractices10)
* [Exemple de code pour spot publicitaire en C#](interstitial-ad-sample-code-in-c.md)
* [Exemple de code pour spot publicitaire en JavaScript](interstitial-ad-sample-code-in-javascript.md)
* [Exemples de publicité sur GitHub](http://aka.ms/githubads)
* [Configurer des unités publicitaires pour votre application](set-up-ad-units-in-your-app.md)
