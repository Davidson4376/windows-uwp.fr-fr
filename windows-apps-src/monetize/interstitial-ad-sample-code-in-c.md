---
author: mcleanbyron
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: "Découvrez comment lancer un spot publicitaire en C#."
title: Exemple de code pour spot publicitaire en C#
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, pub, publicités, spots, c#, exemple de code"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 4c57cf4909028d5aa81c75d9e1b6f1bf28d41ad7
ms.lasthandoff: 02/07/2017

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
 

