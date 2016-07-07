---
author: mcleanbyron
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: "Découvrez comment utiliser les bibliothèques de publicités Microsoft du Kit de développement logiciel (SDK) d’engagement et de monétisation de la BoutiqueMicrosoft pour insérer des spots publicitaires dans une application pour Windows10, Windows8.1 ou Windows Phone8.1."
title: Spots publicitaires
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 0f159409bb584aacaf66550efe8d147cd8fddd50

---

# Spots publicitaires


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Cette procédure pas à pas montre comment utiliser les bibliothèques de publicités Microsoft du Kit de développement logiciel (SDK) d’engagement et de monétisation de la BoutiqueMicrosoft pour insérer des spots publicitaires dans une application pour Windows10, Windows8.1 ou Windows Phone8.1.

Pour obtenir des exemples complets de projet qui montrent comment ajouter des spots publicitaires à des applications HTML/JavaScript et XAML en C# et C++, voir [Exemples de publicité sur GitHub](http://aka.ms/githubads).

<span id="whatareinterstitialads10"/>
## Que sont les spots publicitaires?

Contrairement aux bannières publicitaires, les spots publicitaires (ou *publicités interstitielles*) s’affichent sur la totalité de l’écran de l’application. Deuxformes de base sont fréquemment utilisées dans les jeux.

* Avec les publicités de type *Paywall*, l’utilisateur doit regarder une publicité à intervalles réguliers. Par exemple entre les niveaux de jeu:

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* Avec les publicités *basées sur les récompenses*, l’utilisateur recherche explicitement certains avantages, par exemple un conseil ou du temps supplémentaire pour terminer le niveau, et initialise la publicité vidéo par le biais de l’interface utilisateur de l’application.

    Il est important de noter que ce SDK ne gère pas les interfaces utilisateur sauf lors de la lecture vidéo. Reportez-vous aux [meilleures pratiques Spots](ui-and-user-experience-guidelines.md#interstitialbestpractices10) pour obtenir des recommandations sur les choses à faire et à ne pas faire, si vous envisagez d’intégrer des spots publicitaires dans votre application.

## Génération d’une application contenant des spots publicitaires


### Conditions préalables

1.  Installez le [Kit de développement logiciel (SDK) d’engagement et de monétisation de la BoutiqueMicrosoft](http://aka.ms/store-em-sdk) avec VisualStudio2015 ou VisualStudio2013.

2.  Dans VisualStudio, ouvrez votre projet ou créez-en un.

### Développement du code

* [Étapes pour une application XAML/.NET](#interstitialadsxaml10)

* [Étapes pour HTML/JavaScript](#interstitialadshtml10)

* [Étapes pour C++ (DirectX Interop)](#interstitialadsdirectx10)

<span id="interstitialadsxaml10"/>
### Spots publicitaires (XAML/.NET)

> **Remarque** Cette section fournit des exemples en C#, mais VisualBasic et C++ sont également pris en charge.
 
1. Ouvrez votre projet dans VisualStudio.
2. Dans **Gestionnaire de références**, sélectionnez l’une des références suivantes en fonction de votre type de projet:

    -   Pour un projet de plateforme Windows universelle (UWP): développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour XAML** (version10.0).

    -   Pour un projet Windows8.1: développez **Windows8.1**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Ad Mediator pour Windows8.1 XAML**. Cette option permet d’ajouter les bibliothèques de publicités et de médiateurs publicitaires Microsoft à votre projet, mais vous pouvez ignorer les bibliothèques de médiateurs publicitaires.

    -   Pour un projet Windows Phone8.1: développez **Windows Phone8.1**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Ad Mediator pour Windows Phone8.1 XAML**. Cette option permet d’ajouter les bibliothèques de publicités et de médiateurs publicitaires Microsoft à votre projet, mais vous pouvez ignorer les bibliothèques de médiateurs publicitaires.

3.  Dans le code de l’application, incluez la référence à l’espace de noms suivant.

    ``` syntax
    using Microsoft.Advertising.WinRT.UI;
    ```

4.  Déclarez les propriétés `MyAppId` et `MyAdUnitId`.

    ``` syntax
    var MyAppId = "<your app id for windows>";
    var MyAdUnitId = "<your adunit for windows";

    // if your code is in a universal solution and resides in the shared project
    // you can opt to use #if WINDOWS_APP or WINDOWS_PHONE_APP to override with different
    // identifiers for each
#if WINDOWS_PHONE_APP
    MyAppId = "<your app id for phone>";
    MyAdUnitId = "<your adunit for phone>";
#endif
    ```

    > **Remarque** Vous allez remplacer les valeurs de test par les valeurs dynamiques avant de soumettre votre application.

5.  Instanciez une classe [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx), connectez tous les gestionnaires d’événements et demandez une publicité.

    ``` syntax
    // instantiate an InterstitialAd
    InterstitialAd MyVideoAd = new InterstitialAd();

    // wire up all 4 events, see below for function templates
    MyVideoAd.AdReady += MyVideoAd_AdReady;
    MyVideoAd.ErrorOccurred += MyVideoAd_ErrorOccurred;
    MyVideoAd.Completed += MyVideoAd_Completed;
    MyVideoAd.Cancelled += MyVideoAd_Cancelled;

    // pre-fetch an ad 30-60 seconds before you need it
    MyVideoAd.RequestAd(AdType.Video, MyAppId, MyAdUnitId);
    ```

6.  Dans le code, à l’emplacement où vous souhaitez afficher la publicité, vérifiez qu’elle est opérationnelle, puis affichez-la.

    ``` syntax
    if ((InterstitialAdState.Ready) == (MyVideoAd.State))
    {
      MyVideoAd.Show();
    }
    ```

7.  Définissez et codez les événements.

    ``` syntax
    void MyVideoAd_AdReady(object sender, object e)
    {
      // code
    }

    void MyVideoAd_ErrorOccurred(object sender, AdErrorEventArgs e)
    {
      // code
    }

    void MyVideoAd_Completed(object sender, object e)
    {  
      // code
    }

    void MyVideoAd_Cancelled(object sender, object e)
    {
      // code
    }
    ```

8.  Affectez la propriété `MyAppId` à la valeur de test indiquée dans [Valeurs du mode test](test-mode-values.md). Cette valeur est utilisée uniquement pour les tests; vous allez la remplacer par une valeur dynamique avant de publier votre application.

    ``` syntax
    var MyAppId = "d25517cb-12d4-4699-8bdc-52040c712cab";
    ```

9.  Affectez la propriété `MyAdUnitId` à la valeur de test indiquée dans [Valeurs du mode test](test-mode-values.md). Cette valeur est utilisée uniquement pour les tests; vous allez la remplacer par une valeur dynamique avant de publier votre application.

    ``` syntax
    var MyAdUnitId = "11389925";
    ```

10.  Générez et testez votre application pour vérifier qu’elle affiche les publicités de test.

<span id="interstitialadshtml10"/>
### Spots publicitaires (HTML/JavaScript)

Cet exemple part du principe que vous avez créé un projet d’application universelle pour JavaScript dans VisualStudio2015 et que vous ciblez un processeur spécifique.

1. Ouvrez votre projet dans VisualStudio.
2.  Dans **Gestionnaire de références**, sélectionnez l’une des références suivantes en fonction de votre type de projet:

    -   Pour un projet de plateforme Windows universelle (UWP): développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour JavaScript** (version10.0).

    -   Pour un projet Windows8.1: développez **Windows8.1**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour Windows8.1 natif (JS)**.

    -   Pour un projet Windows Phone8.1: développez **Windows Phone8.1**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour Windows Phone8.1 natif (JS)**.

3.  Dans le code HTML, insérez la référence de script suivante.

    ``` syntax
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

4.  Déclarez les propriétés `myAppId` et `myAdUnitId`.

    ``` syntax
    <script>
      var myAppId = "<your app id>";
      var myAdUnitId = "<your adunit id>";
    </script>
    ```

5.  Instanciez une classe **InterstitialAd**, connectez tous les gestionnaires d’événements et demandez une publicité.

    ``` syntax
    // instantiate an InterstitialAd
    window.interstitialAd = new MicrosoftNSJS.Advertising.InterstitialAd();

    // wire up all 4 events, see below for function templates
    window.interstitialAd.onAdReady = readyHandler;
    window.interstitialAd.onErrorOccurred = errorHandler;
    window.interstitialAd.onCompleted = completeHandler;
    window.interstitialAd.onCancelled = cancelHandler;

    // pre-fetch an ad 30-60 seconds before you need it
    var myAdType = MicrosoftNSJS.Advertising.InterstitialAdType.video;
    window.interstitialAd.requestAd(myAdType, myAppId, myAdUnitId);
    ```

6.  Dans le code, à l’emplacement où vous souhaitez afficher la publicité, vérifiez qu’elle est opérationnelle, puis affichez-la.

    ``` syntax
    if ((MicrosoftNSJS.Advertising.InterstitialAdState.ready) == (window.interstitialAd.state)) {
             window.interstitialAd.show();
    }
    ```

7.  Définissez et codez les événements.

    ``` syntax
    function readyHandler(sender) {
      // code
    }

    function errorHandler(sender, args) {
      // code
    }

    function completeHandler(sender) {
      // code
    }

    function cancelHandler(sender) {
      // code
    }
    ```

7.  Affectez la propriété `MyAppId` à la valeur de test indiquée dans [Valeurs du mode test](test-mode-values.md). Cette valeur est utilisée uniquement pour les tests; vous allez la remplacer par une valeur dynamique avant de publier votre application.

    ``` syntax
    var MyAppId = "d25517cb-12d4-4699-8bdc-52040c712cab";
    ```

8.  Affectez la propriété `MyAdUnitId` à la valeur de test indiquée dans [Valeurs du mode test](test-mode-values.md). Cette valeur est utilisée uniquement pour les tests; vous allez la remplacer par une valeur dynamique avant de publier votre application.

    ``` syntax
    var MyAdUnitId = "11389925";
    ```

9.  Générez et testez votre application pour vérifier qu’elle affiche les publicités de test.

<span id="interstitialadsdirectx10"/>
### Spots publicitaires (C++ et DirectX avec XAML interop)

Cet exemple part du principe que vous avez créé un projet d’application universelle pour XAML dans VisualStudio2015 et que vous ciblez une architectureUC spécifique.

> **Important** Ce code est écrit enC++ comme le requiert DirectX.

 
1. Ouvrez votre projet dans VisualStudio.
1.  Dans **Gestionnaire de références**, sélectionnez l’une des références suivantes en fonction de votre type de projet:

    -   Pour un projet de plateforme Windows universelle (UWP): développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour XAML** (version10.0).

    -   Pour un projet Windows8.1: développez **Windows8.1**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Ad Mediator pour Windows8.1 XAML**. Cette option permet d’ajouter les bibliothèques de publicités et de médiateurs publicitaires Microsoft à votre projet, mais vous pouvez ignorer les bibliothèques de médiateurs publicitaires.

    -   Pour un projet Windows Phone8.1: développez **Windows Phone8.1**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Ad Mediator pour Windows Phone8.1 XAML**. Cette option permet d’ajouter les bibliothèques de publicités et de médiateurs publicitaires Microsoft à votre projet, mais vous pouvez ignorer les bibliothèques de médiateurs publicitaires.

2.  Dans le fichier d’en-tête approprié de votre application, déclarez l’objet spot publicitaire ainsi que les propriétés/méthodes associées.

    ``` syntax
    Microsoft::Advertising::WinRT::UI::InterstitialAd^ m_ia;
    void OnAdReady(Object^ sender, Object^ args);
    void OnAdCompleted(Object^ sender, Object^ args);
    void OnAdCancelled(Object^ sender, Object^ args);
    void OnAdError (Object^ sender,  Microsoft::Advertising::WinRT::UI::AdErrorEventArgs^ args);
    ```

3.  Déclarez les propriétés `AppId` et `AdUnitId`.

    ``` syntax
    #if WINDOWS_PHONE_APP
    static Platform::String^ IA_APPID = L"<your app id for phone>";
    static Platform::String^ IA_ADUNITID = L"<your ad unit for phone>";
    #endif

    #if WINDOWS_APP
    static Platform::String^ IA_APPID = L"<your app id for windows>";
    static Platform::String^ IA_ADUNITID = L"<your ad unit for windows>";
    #endif
    ```

4.  Dans le fichier.cpp, ajoutez une référence d’espace de noms.

    ``` syntax
    using namespace Microsoft::Advertising::WinRT::UI;
    ```

5.  Instanciez une classe **InterstitialAd**, connectez tous les gestionnaires d’événements et demandez une publicité.

    ``` syntax
    // Instantiate an InterstitialAd.
    m_ia = ref new InterstitialAd();

    // Wire up all 4 events, see below for function templates.            
    m_ia->AdReady += ref new Windows::Foundation::EventHandler<Platform::Object ^>
        (this, &Simple3DGameXaml::DirectXPage::OnAdReady);
    m_ia->Completed += ref new Windows::Foundation::EventHandler<Platform::Object ^>
        (this, &Simple3DGameXaml::DirectXPage::OnAdCompleted);
    m_ia->Cancelled += ref new Windows::Foundation::EventHandler<Platform::Object ^>
        (this, &Simple3DGameXaml::DirectXPage::OnAdCancelled);
    m_ia->ErrorOccurred += ref new
        Windows::Foundation::EventHandler<Microsoft::Advertising::WinRT::UI::AdErrorEventArgs ^>
            (this, &Simple3DGameXaml::DirectXPage::OnAdError);

    // Pre-fetch an ad 30-60 seconds before you need it.
    m_ia->RequestAd(AdType::Video, IA_APPID, IA_ADUNITID);
    ```

6.  Dans le code, à l’emplacement où vous souhaitez afficher la publicité, vérifiez qu’elle est opérationnelle, puis affichez-la.

    ``` syntax
    if ((InterstitialAdState::Ready == m_ia->State))
    {
        m_ia->Show();
    }
    ```

7.  Définissez et codez les événements.

    ``` syntax
    void DirectXPage::OnAdReady(Object^ sender, Object^ args)
    {
      // code
    }

    void DirectXPage::OnAdCompleted(Object^ sender, Object^ args)
    {
      // code
    }

    void DirectXPage::OnAdCancelled(Object^ sender, Object^ args)
    {
      // code
    }

    void DirectXPage::OnAdError
      (Object^ sender, Microsoft::Advertising::WinRT::UI::AdErrorEventArgs^ args)
    {
      // code
    }
    ```

8.  Affectez la propriété `AppId` à la valeur de test indiquée dans [Valeurs du mode test](test-mode-values.md). Cette valeur est utilisée uniquement pour les tests; vous allez la remplacer par une valeur dynamique avant de publier votre application.

    ``` syntax
    static Platform::String^ IA_APPID = L"d25517cb-12d4-4699-8bdc-52040c712cab";
    ```

9.  Affectez la propriété `AdUnitId` à la valeur de test indiquée dans [Valeurs du mode test](test-mode-values.md). Cette valeur est utilisée uniquement pour les tests; vous allez la remplacer par une valeur dynamique avant de publier votre application.

    ``` syntax
    static Platform::String^ IA_ADUNITID = L"11389925";
    ```

10. Générez et testez votre application pour vérifier qu’elle affiche les publicités de test.

### Publier l’application avec des publicités dynamiques à l’aide du Centre de développement Windows

1.  Dans le tableau de bord du Centre de développement, accédez à la page **Monétisation**&gt;**Monétiser avec des publicités** de votre application, puis [créez une unité Microsoft Advertising autonome](../publish/monetize-with-ads.md). Pour le type d’unité publicitaire, spécifiez **Spot vidéo**. Prenez note de l’ID d’unité publicitaire et de l’ID de l’application.

2.  Dans votre code, remplacez les valeurs de test de l’unité publicitaire par les valeurs dynamiques que vous avez générées dans le Centre de développement.

3.  [Soumettez votre application](../publish/app-submissions.md) au WindowsStore à l’aide du tableau de bord du Centre de développement Windows.

4.  Passez en revue vos [rapports de performances des publicités](../publish/advertising-performance-report.md) dans le tableau de bord du Centre de développement.

<span id="interstitialbestpractices10"/>
## Meilleures pratiques Spots


Pour plus d’informations sur l’utilisation efficace des spots publicitaires, voir [Recommandations en matière d’expérience utilisateur et d’interface utilisateur](ui-and-user-experience-guidelines.md).

<span id="targetplatform10"/>
## Supprimer des erreurs de référence: Cibler une plateformeUC spécifique (XAML et HTML)


Lorsque vous utilisez les bibliothèques de publicités Microsoft, vous ne pouvez pas cibler **TouteUC** dans votre projet. Si votre projet cible la plateforme **TouteUC**, vous pouvez voir un message d’avertissement dans votre projet une fois que vous avez ajouté une référence aux bibliothèques de publicités Microsoft. Pour supprimer cet avertissement, mettez à jour votre projet pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Pour plus d’informations, voir [Problèmes connus](known-issues-for-the-advertising-libraries.md).

## Rubriques connexes


* [Exemple de code pour spot publicitaire en C#](interstitial-ad-sample-code-in-c.md)
* [Exemple de code pour spot publicitaire en JavaScript](interstitial-ad-sample-code-in-javascript.md)
* [Exemples de publicité sur GitHub](http://aka.ms/githubads)

 

 



<!--HONumber=Jun16_HO4-->


