---
author: mcleanbyron
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: "Découvrez comment utiliser les bibliothèques de publicités Microsoft de Microsoft Store Services SDK pour insérer des spots publicitaires dans une application pour Windows 10, Windows 8.1 ou Windows Phone 8.1."
title: Spots publicitaires
translationtype: Human Translation
ms.sourcegitcommit: 2b5dbf872dd7aad48373f6a6df3dffbcbaee8090
ms.openlocfilehash: fae0fc57eca3477bf46a6f3ac43ec35781241a6e

---

# <a name="interstitial-ads"></a>Spots publicitaires

Cette procédure pas à pas montre comment utiliser les bibliothèques de publicités Microsoft de Microsoft Store Services SDK pour insérer des spots publicitaires dans une application pour Windows 10, Windows 8.1 ou Windows Phone 8.1.

Pour obtenir des exemples complets de projet qui montrent comment ajouter des spots publicitaires à des applications JavaScript/HTML et XAML en C# et C++, voir [Exemples de publicité sur GitHub](http://aka.ms/githubads).

<span id="whatareinterstitialads10"/>
## <a name="what-are-interstitial-ads"></a>Que sont les spots publicitaires ?

Contrairement aux bannières publicitaires, les spots publicitaires (ou *publicités interstitielles*) s’affichent sur la totalité de l’écran de l’application. Deux formes de base sont fréquemment utilisées dans les jeux.

* Avec les publicités de type *Paywall*, l’utilisateur doit regarder une publicité à intervalles réguliers. Par exemple entre les niveaux de jeu :

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* Avec les publicités *basées sur les récompenses*, l’utilisateur recherche explicitement certains avantages, par exemple un conseil ou du temps supplémentaire pour terminer le niveau, et initialise la publicité vidéo par le biais de l’interface utilisateur de l’application.

    Il est important de noter que ce SDK ne gère pas les interfaces utilisateur sauf lors de la lecture vidéo. Reportez-vous aux [meilleures pratiques Spots](ui-and-user-experience-guidelines.md#interstitialbestpractices10) pour obtenir des recommandations sur les choses à faire et à ne pas faire, si vous envisagez d’intégrer des spots publicitaires dans votre application.

## <a name="build-an-app-with-interstitial-ads"></a>Générer une application contenant des spots publicitaires

Pour afficher des spots publicitaires dans votre application, suivez les instructions correspondant au type de projet.

* [XAML/.NET](#interstitialadsxaml10)
* [HTML/JavaScript](#interstitialadshtml10)
* [C++ (technologie interop DirectX)](#interstitialadsdirectx10)

<span/>
### <a name="prerequisites"></a>Prérequis

* Pour les applications UWP : installez [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) avec Visual Studio 2015.
* Pour les applications Windows 8.1 ou Windows Phone 8.1 : installez le [Kit de développement logiciel (SDK) Microsoft Advertising pour Windows et Windows Phone 8.x](http://aka.ms/store-8-sdk) avec Visual Studio 2015 ou Visual Studio 2013.

<span id="interstitialadsxaml10"/>
###<a name="xamlnet"></a>XAML/.NET

Cette section fournit des exemples en C#, mais Visual Basic et C++ sont également pris en charge pour les projets XAML/.NET. Pour voir un exemple de code complet en C#, consultez [Exemple de code pour spot publicitaire en C#](interstitial-ad-sample-code-in-c.md).

1. Ouvrez votre projet dans Visual Studio.

2. Dans **Gestionnaire de références**, sélectionnez l’une des références suivantes en fonction de votre type de projet :

  * Pour un projet de plateforme Windows universelle (UWP) : développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour XAML** (version 10.0).

  * Pour un projet Windows 8.1 : développez **Windows 8.1**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Ad Mediator pour Windows 8.1 XAML**. Cette option permet d’ajouter les bibliothèques de publicités et de médiateurs publicitaires Microsoft à votre projet, mais vous pouvez ignorer les bibliothèques de médiateurs publicitaires.

  * Pour un projet Windows Phone 8.1 : développez **Windows Phone 8.1**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Ad Mediator pour Windows Phone 8.1 XAML**. Cette option permet d’ajouter les bibliothèques de publicités et de médiateurs publicitaires Microsoft à votre projet, mais vous pouvez ignorer les bibliothèques de médiateurs publicitaires.

3.  Dans le fichier de code approprié de votre application (par exemple, dans MainPage.xaml.cs ou dans un fichier de code d’une autre page), ajoutez la référence d’espace de noms ci-dessous.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet1)]

4.  À un emplacement approprié de votre application (par exemple, dans ```MainPage``` ou dans une autre page), déclarez un objet [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) et plusieurs champs de chaîne représentant l’ID de l’application et l’ID d’unité publicitaire de votre spot publicitaire. L’exemple de code suivant affecte les champs `myAppId` et `myAdUnitId` aux valeurs de test indiquées dans [Valeurs du mode Test](test-mode-values.md). Ces valeurs sont utilisées uniquement pour les tests ; vous devez les remplacer par des valeurs dynamiques du Centre de développement Windows avant de publier votre application.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet2)]

5.  Dans le code exécuté au démarrage (par exemple, dans le constructeur de la page), instanciez l’objet **InterstitialAd** et connectez les gestionnaires d’événements correspondant aux événements de l’objet.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet3)]

6.  Récupérez d’abord le spot publicitaire à l’aide de la méthode [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) environ 30 à 60 secondes avant que vous n’en ayez besoin. Cela laisse suffisamment de temps pour demander et préparer la publicité avant qu’elle ne soit affichée.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet4)]

6.  Dans votre code, là où vous souhaitez afficher la publicité, vérifiez que l’objet **InterstitialAd** est prêt à être affiché, puis affichez-le à l’aide de la méthode [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx).

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet5)]

7.  Définissez les gestionnaires d’événements pour l’objet **InterstitialAd**.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet6)]

8.  Générez et testez votre application pour vérifier qu’elle affiche les publicités de test.

<span id="interstitialadshtml10"/>
###<a name="htmljavascript"></a>HTML/JavaScript

Les instructions ci-dessous partent du principe que vous avez créé un projet Windows universel pour JavaScript dans Visual Studio 2015 et que vous ciblez un processeur spécifique. Pour voir un exemple de code complet, consultez [Exemple de code pour spot publicitaire en JavaScript](interstitial-ad-sample-code-in-javascript.md).

1. Ouvrez votre projet dans Visual Studio.

2.  Dans **Gestionnaire de références**, sélectionnez l’une des références suivantes en fonction de votre type de projet :

  * Pour un projet de plateforme Windows universelle (UWP) : développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour JavaScript** (version 10.0).

  * Pour un projet Windows 8.1 : développez **Windows 8.1**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour Windows 8.1 natif (JS)**.

  * Pour un projet Windows 8.1 : développez **Windows Phone 8.1**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour Windows Phone 8.1 natif (JS)**.

3.  Dans la section **&lt;head&gt;** du fichier HTML du projet, après les références JavaScript des fichiers default.css et default.js du projet, ajoutez la référence à ad.js. Dans un projet UWP, ajoutez la référence suivante.

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
  ```

  Dans un projet Windows 8.1 ou Windows Phone 8.1, ajoutez la référence suivante.

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <script src="/MSAdvertisingJS/ads/ad.js"></script>
  ```

4.  Dans un fichier .js de votre projet, déclarez un objet [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) et plusieurs champs contenant l’ID de l’application et l’ID d’unité publicitaire de votre spot publicitaire. L’exemple de code suivant affecte les champs `applicationId` et `adUnitId` aux valeurs de test indiquées dans [Valeurs du mode Test](test-mode-values.md). Ces valeurs sont utilisées uniquement pour les tests ; vous devez les remplacer par des valeurs dynamiques du Centre de développement Windows avant de publier votre application.

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet1)]

5.  Dans le code exécuté au démarrage (par exemple, dans le constructeur de la page), instanciez l’objet **InterstitialAd** et connectez les gestionnaires d’événements de l’objet.

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet2)]

5. Récupérez d’abord le spot publicitaire à l’aide de la méthode [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) environ 30 à 60 secondes avant que vous n’en ayez besoin. Cela laisse suffisamment de temps pour demander et préparer la publicité avant qu’elle ne soit affichée.

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet3)]

6.  Dans votre code, là où vous souhaitez afficher la publicité, vérifiez que l’objet **InterstitialAd** est prêt à être affiché, puis affichez-le à l’aide de la méthode [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx).

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet4)]

7.  Définissez les gestionnaires d’événements pour l’objet **InterstitialAd**.

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet5)]

9.  Générez et testez votre application pour vérifier qu’elle affiche les publicités de test.

<span id="interstitialadsdirectx10"/>
###<a name="c-directx-interop"></a>C++ (technologie interop DirectX)

Cet exemple part du principe que vous avez créé un projet C++ **application DirectX et XAML (Windows universel)** dans Visual Studio 2015 et que vous ciblez une architecture UC spécifique.
 
1. Ouvrez votre projet dans Visual Studio.

1.  Dans **Gestionnaire de références**, sélectionnez l’une des références suivantes en fonction de votre type de projet :

  * Pour un projet de plateforme Windows universelle (UWP) : développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour XAML** (version 10.0).

  * Pour un projet Windows 8.1 : développez **Windows 8.1**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Ad Mediator pour Windows 8.1 XAML**. Cette option permet d’ajouter les bibliothèques de publicités et de médiateurs publicitaires Microsoft à votre projet, mais vous pouvez ignorer les bibliothèques de médiateurs publicitaires.

  * Pour un projet Windows Phone 8.1 : développez **Windows Phone 8.1**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Ad Mediator pour Windows Phone 8.1 XAML**. Cette option permet d’ajouter les bibliothèques de publicités et de médiateurs publicitaires Microsoft à votre projet, mais vous pouvez ignorer les bibliothèques de médiateurs publicitaires.

2.  Dans un fichier d’en-tête approprié de votre application (par exemple, DirectXPage.xaml.h), déclarez un objet [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) et les méthodes de gestionnaire d’événements associées.  

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet1)]

3.  Dans le même fichier d’en-tête, déclarez plusieurs champs de chaîne représentant l’ID de l’application et l’ID d’unité publicitaire de votre spot publicitaire. L’exemple de code suivant affecte les champs `myAppId` et `myAdUnitId` aux valeurs de test indiquées dans [Valeurs du mode Test](test-mode-values.md). Ces valeurs sont utilisées uniquement pour les tests ; vous devez les remplacer par des valeurs dynamiques du Centre de développement Windows avant de publier votre application.

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet2)]

4.  Dans le fichier .cpp dans lequel vous voulez ajouter du code pour afficher un spot publicitaire, ajoutez la référence d’espace de noms ci-dessous. Les exemples suivants partent du principe que vous ajoutez le code dans le fichier DirectXPage.xaml.cpp de votre application.

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet3)]

6.  Dans le code exécuté au démarrage (par exemple, dans le constructeur de la page), instanciez l’objet **InterstitialAd** et connectez les gestionnaires d’événements correspondant aux événements de l’objet. Dans l’exemple suivant, ```InterstitialAdSamplesCpp``` est l’espace de noms de votre projet ; modifiez ce nom au besoin pour votre code.

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet4)]

7. Récupérez d’abord le spot publicitaire à l’aide de la méthode [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) environ 30 à 60 secondes avant que vous n’en ayez besoin. Cela laisse suffisamment de temps pour demander et préparer la publicité avant qu’elle ne soit affichée.

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet5)]

7.  Dans votre code, là où vous souhaitez afficher la publicité, vérifiez que l’objet **InterstitialAd** est prêt à être affiché, puis affichez-le à l’aide de la méthode [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx).

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet6)]

8.  Définissez les gestionnaires d’événements pour l’objet **InterstitialAd**.

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet7)]

9. Générez et testez votre application pour vérifier qu’elle affiche les publicités de test.

<span/>
### <a name="release-your-app-with-live-ads-using-windows-dev-center"></a>Publier l’application avec des publicités dynamiques via le Centre de développement Windows

1.  Dans le tableau de bord du Centre de développement, accédez à la page **Monétisation** &gt; **Monétiser avec des publicités** de votre application, puis [créez une unité publicitaire](../publish/monetize-with-ads.md). Pour le type d’unité publicitaire, spécifiez **Spot vidéo**. Prenez note de l’ID d’unité publicitaire et de l’ID de l’application.

2.  Dans votre code, remplacez les valeurs de test de l’unité publicitaire par les valeurs dynamiques que vous avez générées dans le Centre de développement.

3.  [Soumettez votre application](../publish/app-submissions.md) au Windows Store à l’aide du tableau de bord du Centre de développement Windows.

4.  Passez en revue vos [rapports de performances des publicités](../publish/advertising-performance-report.md) dans le tableau de bord du Centre de développement.

<span id="interstitialbestpractices10"/>
## <a name="interstitial-best-practices-and-policies"></a>Meilleures pratiques et politiques Spots


Pour plus d’informations sur l’utilisation efficace des spots publicitaires et les stratégies à respecter, consultez [Meilleures pratiques et politiques Spots](ui-and-user-experience-guidelines.md#interstitialbestpractices10).

<span id="targetplatform10"/>
## <a name="remove-reference-errors-target-a-specific-cpu-platform-xaml-and-html"></a>Supprimer des erreurs de référence : Cibler une plateforme UC spécifique (XAML et HTML)


Lorsque vous utilisez les bibliothèques de publicités Microsoft, vous ne pouvez pas cibler **Toute UC** dans votre projet. Si votre projet cible la plateforme **Toute UC**, vous pouvez voir un message d’avertissement dans votre projet une fois que vous avez ajouté une référence aux bibliothèques de publicités Microsoft. Pour supprimer cet avertissement, mettez à jour votre projet pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Pour plus d’informations, voir [Problèmes connus](known-issues-for-the-advertising-libraries.md).

## <a name="related-topics"></a>Rubriques connexes


* [Exemple de code pour spot publicitaire en C#](interstitial-ad-sample-code-in-c.md)
* [Exemple de code pour spot publicitaire en JavaScript](interstitial-ad-sample-code-in-javascript.md)
* [Exemples de publicité sur GitHub](http://aka.ms/githubads)

 

 



<!--HONumber=Dec16_HO2-->


