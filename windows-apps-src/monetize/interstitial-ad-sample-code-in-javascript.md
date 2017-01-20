---
author: mcleanbyron
ms.assetid: 646977ed-1705-4ea7-a3db-a6b9aac70703
description: "Découvrez comment lancer un spot publicitaire en JavaScript/HTML."
title: Exemple de code pour spot publicitaire en JavaScript
translationtype: Human Translation
ms.sourcegitcommit: 2b5dbf872dd7aad48373f6a6df3dffbcbaee8090
ms.openlocfilehash: 5d7f81e16f3ecdc73fba5010cfbc6082a19cd24c


---

# <a name="interstitial-ad-sample-code-in-javascript"></a>Exemple de code pour spot publicitaire en JavaScript

Cette rubrique indique l’exemple de code complet pour une application de plateforme Windows universelle (UWP) de base en JavaScript et HTML, qui comporte un spot publicitaire. Pour connaître les instructions pas à pas pour configurer votre projet pour qu’il utilise ce code, voir [Spots publicitaires](interstitial-ads.md). Pour obtenir un exemple de projet complet, consultez les [exemples de publicité sur GitHub](http://aka.ms/githubads).

## <a name="code-example"></a>Exemple de code

Cette section affiche le contenu des fichiers HTML et JavaScript d’une application de base, qui comporte un spot publicitaire. Pour utiliser ces exemples, copiez ce code dans un projet JavaScript **Application WinJS (Windows universel)** dans Visual Studio 2015.

Cet exemple d’application utilise deux boutons pour demander, puis lancer un spot publicitaire. Les fichiers main.js et index.html générés par Visual Studio ont été modifiés et sont présentés ci-dessous. Le fichier script.js affiché ci-dessous contient la plupart du code contenu dans l’exemple, et vous devez ajouter ce fichier dans le dossier **js** de votre projet.

>**Remarque pour Windows 8.x et Windows Phone 8.1**&nbsp;&nbsp;Si votre projet cible Windows 8.1 ou Windows Phone 8.1, le fichier HTML par défaut dans votre projet s’appelle default.html au lieu de index.html, et le fichier JavaScript par défaut dans votre projet s’appelle default.js au lieu de main.js.

Remplacez les valeurs des champs ```applicationId``` et ```adUnitId``` par les valeurs dynamiques du Centre de développement Windows avant de soumettre votre application au Windows Store. Pour plus d’informations, consultez [Configurer des unités publicitaires dans votre application](set-up-ad-units-in-your-app.md).

### <a name="indexhtml"></a>index.html

> [!div class="tabbedCodeSnippets"]
[!code-html[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/index.html#L1-L21)]

<span/>
>**Remarque pour Windows 8.x et Windows Phone 8.1**&nbsp;&nbsp;Si votre projet cible Windows 8.1 ou Windows Phone 8.1, remplacez la ligne ```<script src="//Microsoft.Advertising.JavaScript/ad.js"></script>``` de l’exemple par ```<script src="/MSAdvertisingJS/ads/ad.js"></script>```.

### <a name="scriptjs"></a>script.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#script)]

### <a name="mainjs"></a>main.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/main.js#main)]

## <a name="related-topics"></a>Rubriques connexes

* [Exemples de publicité sur GitHub](http://aka.ms/githubads)

 



<!--HONumber=Dec16_HO2-->


