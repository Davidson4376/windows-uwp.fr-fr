---
author: mcleanbyron
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: "Découvrez comment lancer un spot publicitaire en C#."
title: Exemple de code pour spot publicitaire en C#
translationtype: Human Translation
ms.sourcegitcommit: 2b5dbf872dd7aad48373f6a6df3dffbcbaee8090
ms.openlocfilehash: c7554b94e67ce7f4b83a9ad4360819881d09f0fb

---

# <a name="interstitial-ad-sample-code-in-c"></a>Exemple de code pour spot publicitaire en C\# #  

Cette rubrique indique l’exemple de code complet pour une application de plateforme Windows universelle (UWP) de base en C# et XAML, qui comporte un spot publicitaire. Pour connaître les instructions pas à pas pour configurer votre projet pour qu’il utilise ce code, voir [Spots publicitaires](interstitial-ads.md). Pour obtenir un exemple de projet complet, consultez les [exemples de publicité sur GitHub](http://aka.ms/githubads).

## <a name="code-example"></a>Exemple de code

Cette section affiche le contenu des fichiers MainPage.xaml et MainPage.xaml.cs d’une application de base, qui comporte un spot publicitaire. Pour utiliser ces exemples, copiez le code dans un projet Visual C# **Application vide (Windows universel)** dans Visual Studio 2015.

Cet exemple d’application utilise deux boutons pour demander, puis lancer un spot publicitaire. Remplacez les valeurs des champs ```myAppId``` et ```myAdUnitId``` par les valeurs dynamiques du Centre de développement Windows avant de soumettre votre application au Windows Store. Pour plus d’informations, voir [Configurer des unités publicitaires dans votre application](set-up-ad-units-in-your-app.md).

### <a name="mainpagexaml"></a>MainPage.xaml

> [!div class="tabbedCodeSnippets"]
[!code-xml[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml#L1-L13)]

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

> [!div class="tabbedCodeSnippets"]
[!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#CompleteSample)]

 
## <a name="related-topics"></a>Rubriques connexes

* [Exemples de publicité sur GitHub](http://aka.ms/githubads)
 



<!--HONumber=Dec16_HO2-->


