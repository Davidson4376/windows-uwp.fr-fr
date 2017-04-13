---
author: mcleanbyron
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: "Découvrez comment ajouter des valeurs d’ID d’application et d’ID d’unité publicitaire du tableau de bord du Centre de développement Windows à votre application avant de la soumettre au WindowsStore."
title: "Configurer des unités publicitaires dans votre application"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, publicités, publicité, unités publicitaires"
ms.openlocfilehash: daf0887462a4c84aa827a6261793a0eaf4d512ca
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="set-up-ad-units-in-your-app"></a>Configurer des unités publicitaires dans votre application




Lorsque vous utilisez une classe [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) ou [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) pour afficher des publicités dans votre application, vous devez spécifier un ID d’application et un ID d’unité publicitaire. Lors du développement de votre application, utilisez les [valeurs de test d’ID d’application et d’ID d’unité publicitaire](test-mode-values.md) pour voir comment votre application restitue les publicités au cours du test.

Après avoir testé votre application, et une fois que vous êtes prêt à la soumettre au Centre de développement Windows, vous devez mettre à jour son code pour qu’il utilise les valeurs desID d’application et d’unité publicitaire du [tableau de bord du Centre de développement Windows](https://msdn.microsoft.com/library/windows/apps/mt170658.aspx). Si vous essayez d’utiliser des valeurs de test dans votre application dynamique, votre application ne recevra pas de publicités dynamiques.

Pour configurer l’ID d’application et les unités publicitaires pour votre application dynamique:

1.  Dans le tableau de bord du Centre de développement Windows, sélectionnez votre application, puis cliquez sur **Monétisation&gt; Monétiser avec des publicités**.

2.  Dans la section **Publicités Microsoft Advertising** de cette page, créez une unité publicitaire. Sélectionnez le type d'unité publicitaire parmi les options suivantes:

  * Si vous utilisez un **AdControl** dans votre application pour afficher des bannières publicitaires, sélectionnez **Banner** pour le type d’unité publicitaire.

  * Si vous utilisez un **InterstitialAd** dans votre application pour afficher les spots vidéo ou les bannières publicitaires, sélectionnez **Spot** ou **Bannière** (veillez à sélectionner l’option appropriée pour le type de spot à afficher).

3.  Pour chaque unité publicitaire générée, vous verrez l’**ID de l’application** et l’**ID d’unité publicitaire** sur cette page. Pour afficher les publicités dans votre application, vous devez utiliser ces valeurs dans le code de votre application:

  * Si votre application affiche des bannières, affectez ces valeurs aux propriétés [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) et [AdUnitId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.adunitid.aspx) de votre objet **AdControl**. Pour plus d’informations, voir [AdControl en XAML et .NET](adcontrol-in-xaml-and--net.md) et [AdControl en HTML 5 et Javascript](adcontrol-in-html-5-and-javascript.md).

  * Si votre application affiche des spots publicitaires, transmettez ces valeurs à la méthode [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) de votre objet **InterstitialAd**. Pour plus d’informations, voir [Spots](interstitial-ads.md).

Pour plus d’informations sur la **monétisation des publicités** page, consultez [Monétiser les publicités](../publish/monetize-with-ads.md).

## <a name="related-topics"></a>Rubriques associées

* [Valeurs du mode test](test-mode-values.md)
* [AdControl en XAML et .NET](adcontrol-in-xaml-and--net.md)
* [AdControl en HTML5 et JavaScript](adcontrol-in-html-5-and-javascript.md)
* [Spots](interstitial-ads.md)


 

 
