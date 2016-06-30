---
author: mcleanbyron
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: "Le SDK d’engagement et de monétisation de la Boutique Microsoft vous permet de monétiser votre application de plusieurs manières en insérant des annonces publicitaires."
title: "Afficher des publicités dans votre application"
translationtype: Human Translation
ms.sourcegitcommit: 8a5b02dbc40f3f0cd9be32aa7d5184e60a3b2707
ms.openlocfilehash: c79ba96908cc7b52afefbe44c3f56ce009c87f16

---

# Afficher des publicités dans votre application


\[ Article mis à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Le [Kit de développement logiciel (SDK) d’engagement et de monétisation de la Boutique Microsoft](monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md) vous permet de monétiser votre application de plusieurs manières en insérant des annonces publicitaires.

## Afficher des bannières et des spots vidéo publicitaires à l’aide des bibliothèques de publicités Microsoft

Gagnez davantage d’argent avec vos applications Windows en insérant des bannières et des spots vidéo publicitaires. Les publicités s’affichent dans les applications Windows pour PC, tablettes et téléphones. Vous pouvez surveiller la performance des publicités en temps réel à l’aide du tableau de bord du Centre de développement Windows.

Pour insérer des publicités dans vos applications, utilisez les contrôles **AdControl** et **InterstitialAd** des bibliothèques de publicités qui sont distribuées dans le Kit de développement logiciel (SDK) d’engagement et de monétisation de la Boutique Microsoft. Vous pouvez utiliser ces contrôles pour afficher des bannières et spots vidéo publicitaires de Microsoft dans vos applications XAML ou HTML/JavaScript pour Windows 10, Windows 8.1, Windows Phone 8.1 et Windows Phone 8.

Pour plus d’informations, voir [Afficher des publicités à l’aide des bibliothèques de publicités Microsoft](display-ads-using-the-microsoft-advertising-libraries.md). Après avoir publié une application contenant des publicités, utilisez le [rapport sur les performances des publicités](../publish/advertising-performance-report.md) pour suivre ces performances.                                           

## Servez-vous de la médiation publicitaire pour les bannières publicitaires provenant de plusieurs réseaux publicitaires.

Vous pouvez utiliser la classe **AdMediatorControl** dans vos applications XAML pour optimiser vos recettes publicitaires en affichant des bannières publicitaires provenant de plusieurs réseaux publicitaires. Une fois que vous avez ajouté ce contrôle à votre application, vous pouvez configurer vos paramètres de médiation publicitaire dans le tableau de bord du Centre de développement Windows, et nous nous chargeons de la médiation des demandes de bannières publicitaires des réseaux publicitaires que vous choisissez. Pour plus d’informations, voir [Utiliser la médiation publicitaire pour optimiser les recettes publicitaires](use-ad-mediation-to-maximize-revenue.md).

## Différences entre les bibliothèques de publicités Microsoft et la médiation publicitaire

Le Kit de développement logiciel (SDK) d’engagement et de monétisation de la Boutique Microsoft inclut les bibliothèques de publicités Microsoft et de médiation publicitaire. Toutefois, ces bibliothèques fournissent différentes classes et ciblent des objectifs différents.

* Utilisez les classes **AdControl** et **InterstitialAd** des bibliothèques de publicités Microsoft si vous voulez afficher des bannières ou des spots vidéo publicitaires dans une application XAML ou JavaScript.
* Utilisez la classe **AdMediatorControl** des bibliothèques de médiation publicitaire si vous voulez afficher des bannières publicitaires provenant de plusieurs réseaux publicitaires dans une application XAML.

Pour plus d’informations, voir [Quelle est la différence entre AdMediatorControl et AdControl ?](what-is-the-difference-admediatorcontrol-or-adcontrol.md).

## Rubriques connexes

* [Kit de développement logiciel (SDK) d’engagement et de monétisation de la Boutique Microsoft](monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md)
* [Monétiser votre application en insérant des annonces publicitaires]( http://go.microsoft.com/fwlink/p/?LinkId=699559)



<!--HONumber=Jun16_HO4-->


