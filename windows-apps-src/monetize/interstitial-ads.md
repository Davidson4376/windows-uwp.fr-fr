---
author: mcleanbyron
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: "Découvrez comment utiliser les bibliothèques de publicités Microsoft pour insérer des spots publicitaires dans une application pour Windows10, Windows8.1 ou Windows Phone8.1."
title: Spots
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows10, uwp, pub, publicité, contrôle de publicités, spot"
ms.openlocfilehash: 16cb78d9419d54a1bd56fb855ca1fad6fedf665a
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/27/2017
---
# <a name="interstitial-ads"></a>Spots

Cette procédure pas à pas montre comment insérer des spots publicitaires dans les applications de plateforme Windows universelle (UWP) et des jeux pour Windows10 et dans les applications pour Windows8.1 ou WindowsPhone8.1. Pour obtenir des exemples complets de projet qui montrent comment ajouter des spots publicitaires à des applications JavaScript/HTML et XAML en C# et C++, voir [Exemples de publicité sur GitHub](http://aka.ms/githubads).

<span id="whatareinterstitialads10"/>
## <a name="what-are-interstitial-ads"></a>Que sont les spots publicitaires?

Contrairement aux bannières standard, qui sont limitées à une partie d’une interface utilisateur dans une application ou un jeu, les spots s'affichent sur l’écran entier. Deuxformes de base sont fréquemment utilisées dans les jeux.

* Avec les publicités de type *Paywall*, l’utilisateur doit regarder une publicité à intervalles réguliers. Par exemple entre les niveaux de jeu:

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* Avec les publicités *basées sur les récompenses*, l’utilisateur recherche explicitement certains avantages, par exemple un conseil ou du temps supplémentaire pour terminer le niveau, et initialise la publicité par le biais de l’interface utilisateur de l’application.

Nous proposons deux types de spots à utiliser dans vos applications et jeux:

* **Spots vidéos**: ceux-ci sont disponibles pour les applications de plateforme Windows universelle (UWP) pour Windows10 et dans les applications pour Windows8.1 ou WindowsPhone8.1.

* **Bannières**: celles-ci sont uniquement disponibles pour les applications UWP pour Windows10.

> [!NOTE]
> L’API pour les spots ne gère pas d’interface utilisateur sauf au moment de la lecture vidéo. Reportez-vous aux [meilleures pratiques Spots](ui-and-user-experience-guidelines.md#interstitialbestpractices10) pour obtenir des recommandations sur les choses à faire et à ne pas faire, si vous envisagez d’intégrer des spots publicitaires dans votre application.

## <a name="build-an-app-with-interstitial-ads"></a>Générer une application contenant des spots publicitaires

Pour afficher des spots publicitaires dans votre application, suivez les instructions correspondant au type de projet.

* [XAML/.NET](#interstitialadsxaml10)
* [HTML/JavaScript](#interstitialadshtml10)
* [C++ (technologie interop DirectX)](#interstitialadsdirectx10)

<span/>
### <a name="prerequisites"></a>Conditions préalables

* Pour les applications UWP: installez le [SDK MicrosoftAdvertising](http://aka.ms/ads-sdk-uwp) avec Visual Studio2015 ou une version ultérieure.

* Pour les applicationsWindows8.1 ou WindowsPhone8.1: [installe Microsoft Advertising SDK pour Windows et Windows Phone8.x](http://aka.ms/store-8-sdk) avec Visual Studio2015 ou Visual Studio2013.

<span id="interstitialadsxaml10"/>
### <a name="xamlnet"></a>XAML/.NET

Cette section fournit des exemples en C#, mais VisualBasic et C++ sont également pris en charge pour les projets XAML/.NET. Pour voir un exemple de code complet en C#, consultez [Exemple de code pour spot publicitaire en C#](interstitial-ad-sample-code-in-c.md).

1. Ouvrez votre projet dans VisualStudio.

2. Dans **Gestionnaire de références**, sélectionnez l’une des références suivantes en fonction de votre type de projet:

  * Pour un projet de plateforme Windows universelle (UWP): développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour XAML** (version10.0).

  * Pour un projet Windows8.1: développez **Windows8.1**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Ad Mediator pour Windows8.1 XAML**. Cette option permet d’ajouter les bibliothèques de publicités et de médiateurs publicitaires Microsoft à votre projet, mais vous pouvez ignorer les bibliothèques de médiateurs publicitaires.

  * Pour un projet Windows Phone8.1: développez **Windows Phone8.1**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Ad Mediator pour Windows Phone8.1 XAML**. Cette option permet d’ajouter les bibliothèques de publicités et de médiateurs publicitaires Microsoft à votre projet, mais vous pouvez ignorer les bibliothèques de médiateurs publicitaires.

3.  Dans le fichier de code approprié de votre application (par exemple, dans MainPage.xaml.cs ou dans un fichier de code d’une autre page), ajoutez la référence d’espace de noms ci-dessous.

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet1)]

4.  À un emplacement approprié de votre application (par exemple, dans ```MainPage``` ou dans une autre page), déclarez un objet [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) et plusieurs champs de chaîne représentant l’ID de l’application et l’ID d’unité publicitaire de votre spot publicitaire. L’exemple de code suivant affecte les champs `myAppId` et `myAdUnitId` aux valeurs de test pour les spots vidéo indiquées dans [Valeurs du mode Test](test-mode-values.md).

  > [!NOTE]
  > Chaque **InterstitialAd** est associé à une *unité publicitaire* qui est utilisée par nos services pour servir des publicités au contrôle, et chaque unité publicitaire se compose d’un *ID d'unité publicitaire* et d'un *ID d'application*. Dans ces étapes, vous attribuez à votre contrôle des valeurs de test ID d’unité publicitaire et ID d'application. Ces valeurs de test ne peuvent être utilisées que dans une version de test de votre application. Avant de publier votre application sur le Windows Store, vous devez [remplacer ces valeurs de test par des valeurs dynamiques](#release) du Centre de développement Windows.

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet2)]

5.  Dans le code exécuté au démarrage (par exemple, dans le constructeur de la page), instanciez l’objet **InterstitialAd** et connectez les gestionnaires d’événements correspondant aux événements de l’objet.

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet3)]

6.  Si vous souhaitez afficher un *spot vidéo*: récupérez d’abord le spot publicitaire à l’aide de la méthode [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) environ 30 à 60secondes avant que vous n’en ayez besoin. Cela laisse suffisamment de temps pour demander et préparer la publicité avant qu’elle ne soit affichée. Veillez à spécifier **AdType.Video** pour le type d’annonce.

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet4)]

  Si vous souhaitez afficher une *bannière* (pour les applications UWP uniquement): récupérez d'abord le spot à l'aide de la méthode [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) environ 5 à 8 secondes avant que vous n'en ayez besoin. Cela laisse suffisamment de temps pour demander et préparer la publicité avant qu’elle ne soit affichée. Veillez à spécifier **AdType.Display** pour le type d’annonce.

  ```csharp
  myInterstitialAd.RequestAd(AdType.Display, myAppId, myAdUnitId);
  ```

6.  Dans votre code, là où vous souhaitez afficher le spot vidéo ou la bannière publicitaire, vérifiez que l’objet **InterstitialAd** est prêt à être affiché, puis affichez-le à l’aide de la méthode [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx).

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet5)]

7.  Définissez les gestionnaires d’événements pour l’objet **InterstitialAd**.

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet6)]

8.  Générez et testez votre application pour vérifier qu’elle affiche les publicités de test.

<span id="interstitialadshtml10"/>
### <a name="htmljavascript"></a>HTML/JavaScript

Les instructions ci-dessous partent du principe que vous avez créé un projet Windows universel pour JavaScript dans VisualStudio et que vous ciblez un processeur spécifique. Pour voir un exemple de code complet, consultez [Exemple de code pour spot publicitaire en JavaScript](interstitial-ad-sample-code-in-javascript.md).

1. Ouvrez votre projet dans VisualStudio.

2.  Dans **Gestionnaire de références**, sélectionnez l’une des références suivantes en fonction de votre type de projet:

  * Pour un projet de plateforme Windows universelle (UWP): développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour JavaScript** (version10.0).

  * Pour un projet Windows8.1: développez **Windows8.1**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour Windows8.1 natif (JS)**.

  * Pour un projet Windows8.1: développez **Windows Phone8.1**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) MicrosoftAdvertising pour Windows Phone8.1 natif (JS)**.

3.  Dans la section **&lt;head&gt;** du fichier HTML du projet, après les références JavaScript des fichiers default.css et default.js du projet, ajoutez la référence à ad.js. Dans un projet UWP, ajoutez la référence suivante.

  ``` HTML
  <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
  ```

  Dans un projet Windows8.1 ou Windows Phone8.1, ajoutez la référence suivante.

  ``` HTML
  <script src="/MSAdvertisingJS/ads/ad.js"></script>
  ```

4.  Dans un fichier .js de votre projet, déclarez un objet [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) et plusieurs champs contenant l’ID de l’application et l’ID d’unité publicitaire de votre spot publicitaire. L’exemple de code suivant affecte les champs `applicationId` et `adUnitId` aux valeurs de test pour les spots vidéo indiquées dans [Valeurs du mode Test](test-mode-values.md).

  > [!NOTE]
  > Chaque **InterstitialAd** est associé à une *unité publicitaire* qui est utilisée par nos services pour servir des publicités au contrôle, et chaque unité publicitaire se compose d’un *ID d'unité publicitaire* et d'un *ID d'application*. Dans ces étapes, vous attribuez à votre contrôle des valeurs de test ID d’unité publicitaire et ID d'application. Ces valeurs de test ne peuvent être utilisées que dans une version de test de votre application. Avant de publier votre application sur le Windows Store, vous devez [remplacer ces valeurs de test par des valeurs dynamiques](#release) du Centre de développement Windows.

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet1)]

5.  Dans le code exécuté au démarrage (par exemple, dans le constructeur de la page), instanciez l’objet **InterstitialAd** et connectez les gestionnaires d’événements de l’objet.

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet2)]

5. Si vous souhaitez afficher un *spot vidéo*: récupérez d’abord le spot publicitaire à l’aide de la méthode [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) environ 30 à 60secondes avant que vous n’en ayez besoin. Cela laisse suffisamment de temps pour demander et préparer la publicité avant qu’elle ne soit affichée. Veillez à spécifier **InterstitialAdType.video** pour le type d’annonce.

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet3)]

  Si vous souhaitez afficher une *bannière* (pour les applications UWP uniquement): récupérez d'abord le spot à l'aide de la méthode [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) environ 5 à 8 secondes avant que vous n'en ayez besoin. Cela laisse suffisamment de temps pour demander et préparer la publicité avant qu’elle ne soit affichée. Veillez à spécifier **InterstitialAdType.display** pour le type d’annonce.

  ```js
  if (interstitialAd) {
      interstitialAd.requestAd(MicrosoftNSJS.Advertising.InterstitialAdType.display, applicationId, adUnitId);
  }
  ```

6.  Dans votre code, là où vous souhaitez afficher la publicité, vérifiez que l’objet **InterstitialAd** est prêt à être affiché, puis affichez-le à l’aide de la méthode [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx).

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet4)]

7.  Définissez les gestionnaires d’événements pour l’objet **InterstitialAd**.

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet5)]

9.  Générez et testez votre application pour vérifier qu’elle affiche les publicités de test.

<span id="interstitialadsdirectx10"/>
### <a name="c-directx-interop"></a>C++ (technologie interop DirectX)

Cet exemple part du principe que vous avez créé un projet C++ **application DirectX et XAML (Windows universel)** dans VisualStudio et que vous ciblez une architectureUC spécifique.
 
1. Ouvrez votre projet dans Visual Studio.

1.  Dans **Gestionnaire de références**, sélectionnez l’une des références suivantes en fonction de votre type de projet:

  * Pour un projet de plateforme Windows universelle (UWP): développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour XAML** (version10.0).

  * Pour un projet Windows8.1: développez **Windows8.1**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Ad Mediator pour Windows8.1 XAML**. Cette option permet d’ajouter les bibliothèques de publicités et de médiateurs publicitaires Microsoft à votre projet, mais vous pouvez ignorer les bibliothèques de médiateurs publicitaires.

  * Pour un projet Windows Phone8.1: développez **Windows Phone8.1**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Ad Mediator pour Windows Phone8.1 XAML**. Cette option permet d’ajouter les bibliothèques de publicités et de médiateurs publicitaires Microsoft à votre projet, mais vous pouvez ignorer les bibliothèques de médiateurs publicitaires.

2.  Dans un fichier d’en-tête approprié de votre application (par exemple, DirectXPage.xaml.h), déclarez un objet [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) et les méthodes de gestionnaire d’événements associées.  

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet1)]

3.  Dans le même fichier d’en-tête, déclarez plusieurs champs de chaîne représentant l’ID de l’application et l’ID d’unité publicitaire de votre spot publicitaire. L’exemple de code suivant affecte les champs `myAppId` et `myAdUnitId` aux valeurs de test pour les spots vidéo indiquées dans [Valeurs du mode Test](test-mode-values.md).

  > [!NOTE]
  > Chaque **InterstitialAd** est associé à une *unité publicitaire* qui est utilisée par nos services pour servir des publicités au contrôle, et chaque unité publicitaire se compose d’un *ID d'unité publicitaire* et d'un *ID d'application*. Dans ces étapes, vous attribuez à votre contrôle des valeurs de test ID d’unité publicitaire et ID d'application. Ces valeurs de test ne peuvent être utilisées que dans une version de test de votre application. Avant de publier votre application sur le Windows Store, vous devez [remplacer ces valeurs de test par des valeurs dynamiques](#release) du Centre de développement Windows.

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet2)]

4.  Dans le fichier .cpp dans lequel vous voulez ajouter du code pour afficher un spot publicitaire, ajoutez la référence d’espace de noms ci-dessous. Les exemples suivants partent du principe que vous ajoutez le code dans le fichier DirectXPage.xaml.cpp de votre application.

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet3)]

6.  Dans le code exécuté au démarrage (par exemple, dans le constructeur de la page), instanciez l’objet **InterstitialAd** et connectez les gestionnaires d’événements correspondant aux événements de l’objet. Dans l’exemple suivant, ```InterstitialAdSamplesCpp``` est l’espace de noms de votre projet; modifiez ce nom au besoin pour votre code.

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet4)]

7. Si vous souhaitez afficher un *spot vidéo*: récupérez d’abord le spot à l’aide de la méthode [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) environ 30 à 60secondes avant que vous n’en ayez besoin. Cela laisse suffisamment de temps pour demander et préparer la publicité avant qu’elle ne soit affichée. Veillez à spécifier **AdType::Video** pour le type d’annonce.

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet5)]

  Si vous souhaitez afficher une *bannière* (pour les applications UWP uniquement): récupérez d'abord le spot à l'aide de la méthode [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) environ 5 à 8 secondes avant que vous n'en ayez besoin. Cela laisse suffisamment de temps pour demander et préparer la publicité avant qu’elle ne soit affichée. Veillez à spécifier **AdType::Display** pour le type d’annonce.

  ```cpp
  m_interstitialAd->RequestAd(AdType::Display, myAppId, myAdUnitId);
  ```

7.  Dans votre code, là où vous souhaitez afficher la publicité, vérifiez que l’objet **InterstitialAd** est prêt à être affiché, puis affichez-le à l’aide de la méthode [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx).

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet6)]

8.  Définissez les gestionnaires d’événements pour l’objet **InterstitialAd**.

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet7)]

9. Générez et testez votre application pour vérifier qu’elle affiche les publicités de test.

<span id="release" />
## <a name="release-your-app-with-live-ads-using-windows-dev-center"></a>Publier votre application avec des publicités dynamiques via le Centre de développement Windows

1.  Dans le tableau de bord du Centre de développement, accédez à la page [Monétiser avec des publicités](../publish/monetize-with-ads.md) de votre application, puis [créez une unité publicitaire](../monetize/set-up-ad-units-in-your-app.md). Pour le type d’unité publicitaire, choisissez **Spot vidéo** ou **Bannière**, en fonction du type de spot que vous diffusez. Prenez note de l’ID d’unité publicitaire et de l’ID de l’application.

2. Si votre application est de type UWP pour Windows10, vous pouvez éventuellement activer la médiation publicitaire pour l'objet **InterstitialAd** en configurant les paramètres dans la section [Médiation publicitaire](../publish/monetize-with-ads.md#mediation) sur la page [Monétiser avec des publicités](../publish/monetize-with-ads.md). La médiation publicitaire vous permet d’optimiser vos revenus publicitaires et vos capacités de promotion d’application en affichant des spots issus de plusieurs réseaux publicitaires, y compris les publicités d’autres réseaux payants tels que Taboola et Smaato et les publicités des campagnes de promotion d’applications Microsoft.

3.  Dans votre code, remplacez les valeurs de test de l’unité publicitaire par les valeurs dynamiques que vous avez générées dans le Centre de développement.

4.  [Soumettez votre application](../publish/app-submissions.md) au WindowsStore à l’aide du tableau de bord du Centre de développement Windows.

5.  Passez en revue vos [rapports de performances des publicités](../publish/advertising-performance-report.md) dans le tableau de bord du Centre de développement.

<span id="manage" />
## <a name="manage-ad-units-for-multiple-interstitial-ad-controls-in-your-app"></a>Gérer des unités publicitaires pour plusieurs contrôles de spot publicitaire dans votre application

Vous pouvez utiliser plusieurs contrôles **InterstitialAd** dans une seule application. Dans ce scénario, nous vous recommandons d’attribuer une unité publicitaire différente à chaque contrôle. L’utilisation de différentes unités publicitaires pour chaque contrôle vous permet de [configurer les paramètres de médiation](../publish/monetize-with-ads.md#mediation) séparément et d'obtenir des [données de rapport](../publish/advertising-performance-report.md) discrètes pour chaque contrôle. Cela permet également à nos services de mieux optimiser les publicités que nous servons à votre application.

> [!IMPORTANT]
> Vous pouvez utiliser chaque unité publicitaire dans une seule application. Si vous utilisez une unité publicitaire dans plusieurs applications, les publicités ne seront pas servies à cette unité publicitaire.

<span id="interstitialbestpractices10"/>
## <a name="interstitial-best-practices-and-policies"></a>Meilleures pratiques et politiques Spots


Pour plus d’informations sur l’utilisation efficace des spots publicitaires et les stratégies à respecter, consultez [Meilleures pratiques et politiques Spots](ui-and-user-experience-guidelines.md#interstitialbestpractices10).

<span id="targetplatform10"/>
## <a name="remove-reference-errors-target-a-specific-cpu-platform-xaml-and-html"></a>Supprimer des erreurs de référence: Cibler une plateformeUC spécifique (XAML et HTML)


Lorsque vous utilisez les bibliothèques de publicités Microsoft, vous ne pouvez pas cibler **TouteUC** dans votre projet. Si votre projet cible la plateforme **TouteUC**, vous pouvez voir un message d’avertissement dans votre projet une fois que vous avez ajouté une référence aux bibliothèques de publicités Microsoft. Pour supprimer cet avertissement, mettez à jour votre projet pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Pour plus d’informations, voir [Problèmes connus](known-issues-for-the-advertising-libraries.md).

## <a name="related-topics"></a>Rubriques associées


* [Exemple de code pour spot publicitaire en C#](interstitial-ad-sample-code-in-c.md)
* [Exemple de code pour spot publicitaire en JavaScript](interstitial-ad-sample-code-in-javascript.md)
* [Exemples de publicité sur GitHub](http://aka.ms/githubads)
* [Configurer des unités publicitaires pour votre application](../monetize/set-up-ad-units-in-your-app.md)
