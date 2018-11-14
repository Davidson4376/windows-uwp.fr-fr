---
author: Xansky
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: Découvrez comment lancer un spot publicitaire en C#.
title: Exemple de code pour spot publicitaire en C#
ms.author: mhopkins
ms.date: 03/22/2018
ms.topic: article
keywords: windows10, uwp, pub, publicités, spots, c#, exemple de code
ms.localizationpriority: medium
ms.openlocfilehash: 07f0927d741bbadc31aaf26f5188af245ec44790
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6662316"
---
# <a name="interstitial-ad-sample-code-in-c"></a>Exemple de code pour spot publicitaire en C\# #  

Cette rubrique indique l’exemple de code complet pour une application de plateforme Windows universelle (UWP) de base en C# et XAML, qui comporte un spot vidéo publicitaire. Pour connaître les instructions pas à pas pour configurer votre projet pour qu’il utilise ce code, voir [Spots publicitaires](interstitial-ads.md). Pour obtenir un exemple de projet complet, consultez les [exemples de publicité sur GitHub](http://aka.ms/githubads).

## <a name="code-example"></a>Exemple de code

Cette section affiche le contenu des fichiers MainPage.xaml et MainPage.xaml.cs d’une application de base, qui comporte un spot publicitaire. Pour utiliser ces exemples, copiez le code dans un projet Visual C# **Application vide (Windows universelle)** au sein de VisualStudio.

Cet exemple d’application utilise deuxboutons pour demander, puis lancer un spot publicitaire. Remplacez les valeurs de la ```myAppId``` et ```myAdUnitId``` champs avec des valeurs dynamiques à partir de l’espace partenaires avant de soumettre votre application au Windows Store. Pour plus d’informations, voir [Configurer des unités publicitaires dans votre application](set-up-ad-units-in-your-app.md#live-ad-units).

> [!NOTE]
> Pour modifier cet exemple de sorte à afficher une bannière au lieu d’un spot vidéo, transmettez la valeur **AdType.Display** au premier paramètre de la méthode [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) au lieu de **AdType.Video**. Pour plus d’informations, voir [Spots](interstitial-ads.md).

### <a name="mainpagexaml"></a>MainPage.xaml

> [!div class="tabbedCodeSnippets"]
[!code-xml[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml#L1-L13)]

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

> [!div class="tabbedCodeSnippets"]
[!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#CompleteSample)]

 
## <a name="related-topics"></a>Rubriquesassociées

* [Exemples de publicité sur GitHub](http://aka.ms/githubads)
 
