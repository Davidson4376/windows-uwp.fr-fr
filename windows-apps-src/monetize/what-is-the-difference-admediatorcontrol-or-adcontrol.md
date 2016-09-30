---
author: mcleanbyron
ms.assetid: 9165f709-71d7-42cf-9b30-3190fe029fb4
description: "Découvrez les différences entre la classe AdControl des bibliothèques de publicités Microsoft et la classe AdMediatorControl des bibliothèques de médiation publicitaire."
title: "Quelle est la différence entre AdMediatorControl et AdControl?"
translationtype: Human Translation
ms.sourcegitcommit: 8a5b02dbc40f3f0cd9be32aa7d5184e60a3b2707
ms.openlocfilehash: 291e1c4d707e8987d29ae5840248918543d7d12a

---

# Quelle est la différence entre AdMediatorControl et AdControl?


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Utilisez les bibliothèques de publicité Microsoft pour XAML et JavaScript dans votre application lorsque vous voulez afficher des bannières publicitaires ou des spots vidéo publicitaires de Microsoft. Ces bibliothèques sont différentes des bibliothèques de médiation publicitaire, que vous utilisez pour afficher des publicités provenant de plusieurs réseaux publicitaires. Utilisez cette documentation pour les bibliothèques de publicités Microsoft (XAML et JavaScript) lorsque:

* Vous utilisez [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) ou [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) dans une application XAML ou JavaScript plutôt que **AdMediatorControl**.
* Vous recherchez des informations de référence pour l’API **AdControl** sous-jacente utilisée par la médiation publicitaire.

Les bibliothèques de publicités Microsoft et les bibliothèques de médiation publicitaire sont incluses dans le Kit de développement logiciel (SDK) d’engagement et de monétisation de la BoutiqueMicrosoft. Pour plus d’informations sur l’installation du SDK et des différentes bibliothèques de publicités Microsoft qu’il contient, voir [Installer les bibliothèques de publicités Microsoft](install-the-microsoft-advertising-libraries.md).

>**Remarque** Pour afficher les spots publicitaires, utilisez le contrôle **InterstitialAd**. Les contrôles **AdControl** et **AdMediatorControl** ne peuvent pas afficher les spots publicitaires. Pour plus d’informations, voir [Spots publicitaires](interstitial-ads.md).

 

## Ad Mediation


Pour afficher des bannières publicitaires de Microsoft (et non pas des spots publicitaires), la méthode recommandée consiste à utiliser le contrôle **AdMediatorControl** dans votre application. Le contrôle **AdMediatorControl** affiche les bannières publicitaires provenant de plusieurs réseaux publicitaires.

Lorsque vous utilisez le contrôle **AdMediatorControl** dans un projet, vous devez spécifier les réseaux publicitaires à utiliser à l’aide de la fonctionnalité **Services connectés** de Visual Studio. Visual Studio tente de récupérer les assemblys requis par programme pour certains réseaux publicitaires. Si des assemblys ne peuvent pas être récupérés automatiquement, vous devez les installer sur chaque réseau publicitaire. Pour plus d’informations sur la médiation publicitaire, voir [Utiliser la médiation publicitaire pour optimiser les revenus](use-ad-mediation-to-maximize-revenue.md).

Le contrôle **AdMediatorControl** fait abstraction de l’utilisation des ID d’unité de publicité et des ID d’application. Ces ID sont gérés par le contrôle **AdMediatorControl**, en association avec vos paramètres de médiation publicitaire dans le tableau de bord du Centre de développement Windows. Pour en savoir plus, voir [Soumettre votre application et configurer une médiation publicitaire](submit-your-app-and-configure-ad-mediation.md).

Le contrôle **AdMediatorControl** prend en charge les API de chacun des services publicitaires dont il assure la médiation à l’aide de ses propres attributs et syntaxe. Pour plus d’informations, voir [Ajouter et utiliser le contrôle de médiation publicitaire](add-and-use-the-ad-mediator-control.md).

## AdControl


Si vous souhaitez uniquement afficher des bannières publicitaires de Microsoft (et aucun autre réseau publicitaire), vous pouvez utiliser **AdControl** uniquement dans les applications XAML et JavaScript. Lors du test d’une application utilisant le contrôle **AdControl**, utilisez les [valeurs du mode test](test-mode-values.md) pour l’ID d’application et l’ID d’unité publicitaire. Après avoir testé votre application, et une fois que vous êtes prêt à la soumettre au Centre de développement Windows, utilisez le tableau de bord du Centre de développement Windows pour obtenir vos ID d’application et d’unité publicitaire dynamique et mettre à jour votre code pour qu’il utilise ces valeurs. Pour plus d’informations, voir [Configurer des unités publicitaires dans votre application](set-up-ad-units-in-your-app.md).

 

 



<!--HONumber=Jun16_HO4-->


