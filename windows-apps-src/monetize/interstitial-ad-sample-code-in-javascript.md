---
author: mcleanbyron
ms.assetid: 646977ed-1705-4ea7-a3db-a6b9aac70703
description: Découvrez comment lancer un spot publicitaire en JavaScript/HTML.
title: Exemple de code pour spot publicitaire en JavaScript
ms.author: mcleans
ms.date: 03/22/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, pub, publicités, spots, javascript, exemple de code
ms.localizationpriority: medium
ms.openlocfilehash: f0cd50a8d3a08053f04dd0e6e8afbfafc8ef34dd
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
ms.locfileid: "1690345"
---
# <a name="interstitial-ad-sample-code-in-javascript"></a>Exemple de code pour spot publicitaire en JavaScript

Cette rubrique indique l’exemple de code complet pour une application de plateforme Windows universelle (UWP) de base en JavaScript et HTML, qui comporte un spot publicitaire. Pour connaître les instructions pas à pas pour configurer votre projet pour qu’il utilise ce code, voir [Spots publicitaires](interstitial-ads.md). Pour obtenir un exemple de projet complet, consultez les [exemples de publicité sur GitHub](http://aka.ms/githubads).

## <a name="code-example"></a>Exemple de code

Cette section affiche le contenu des fichiers HTML et JavaScript d’une application de base, qui comporte un spot publicitaire. Pour utiliser ces exemples, copiez ce code dans un projet JavaScript **Application WinJS (Windows universelle)** au sein de VisualStudio.

Cet exemple d’application utilise deuxboutons pour demander, puis lancer un spot publicitaire. Les fichiers main.js et index.html générés par VisualStudio ont été modifiés et sont présentés ci-dessous. Le fichier script.js affiché ci-dessous contient la plupart du code contenu dans l’exemple, et vous devez ajouter ce fichier dans le dossier **js** de votre projet.

Remplacez les valeurs des variables ```applicationId``` et ```adUnitId``` par les valeurs dynamiques du Centre de développement Windows avant de soumettre votre app au Store. Pour plus d’informations, voir [Configurer des unités publicitaires dans votre app](set-up-ad-units-in-your-app.md#live-ad-units).

> [!NOTE]
> Pour modifier cet exemple de sorte à afficher une bannière au lieu d’un spot vidéo, transmettez la valeur  **InterstitialAdType.display** au premier paramètre de la méthode [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) au lieu de **InterstitialAdType.video**. Pour plus d’informations, voir [Spots](interstitial-ads.md).

### <a name="indexhtml"></a>index.html

> [!div class="tabbedCodeSnippets"]
[!code-html[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/index.html#L1-L21)]

### <a name="scriptjs"></a>script.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#script)]

### <a name="mainjs"></a>main.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/main.js#main)]

## <a name="related-topics"></a>Rubriquesassociées

* [Exemples de publicité sur GitHub](http://aka.ms/githubads)

 
