---
author: mcleanbyron
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: "Découvrez comment ajouter des valeurs d’ID d’application et d’ID d’unité publicitaire du tableau de bord du Centre de développement Windows à votre application avant de la soumettre au WindowsStore."
title: "Configurer des unités publicitaires dans votre application"
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, publicités, publicité, unités publicitaires"
ms.openlocfilehash: f96e81079764682a9f603fe93a9c123a69690507
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/27/2017
---
# <a name="set-up-ad-units-in-your-app"></a>Configurer des unités publicitaires dans votre application

Chaque bannière publicitaire, spot publicitaire ou contrôle de publicité native dans votre application possède une *unité publicitaire* correspondante qui est utilisée par nos services pour servir des publicités au contrôle. Chaque unité publicitaire se compose d'un *ID d’unité publicitaire* et d'un *ID d’application*. Lors du développement de votre application, attribuez les [valeurs de test d’ID d’application et d’ID d’unité publicitaire](test-mode-values.md) à votre contrôle pour vérifier que votre application affiche les publicités de test. Ces valeurs de test ne peuvent être utilisées que dans une version de test de votre application.

Après avoir testé votre application, et une fois que vous êtes prêt à la soumettre au Centre de développement Windows, vous devez mettre à jour son code pour qu’il utilise les valeurs desID d’application et d’unité publicitaire de la page [Monétiser avec des publicités](../publish/monetize-with-ads.md) du tableau de bord du Centre de développement Windows. Si vous essayez d’utiliser des valeurs de test dans votre application dynamique, votre application ne recevra pas de publicités dynamiques.

Pour configurer les unités publicitaires de votre application dynamique:

1.  Dans le tableau de bord du Centre de développement Windows, sélectionnez votre application, puis cliquez sur **Monétisation> Monétiser avec des publicités**.

2.  Dans la section **Créer une publicité**, indiquez un nom pour l’unité publicitaire dans le champ **Nom de la publicité**.

3. Dans la liste déroulante **Type de publicité**, sélectionnez le type d’unité publicitaire correspondant aux publicités que vous affichez dans votre contrôle:

    -   Si vous utilisez un [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) dans votre application pour afficher des bannières publicitaires, sélectionnez **Bannière**.

    -   Si vous utilisez un [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) dans votre application pour afficher les spots vidéo ou les bannières publicitaires, sélectionnez **Spot** ou **Bannière** (veillez à sélectionner l’option appropriée pour le type de spot à afficher).

    -   Si vous utilisez un [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx) dans votre application pour afficher des publicités natives, sélectionnez **Native**.
      > [!NOTE]
      > La possibilité de créer des unités publicitaires **natives** n'est actuellement disponible que pour des développeurs sélectionnés qui participent à un programme pilote, mais nous envisageons de la rendre disponible pour tous les développeurs bientôt. Si vous souhaitez rejoindre notre programme pilote, contactez-nous à l’adresse aiacare@microsoft.com.

4.  Dans la liste déroulante **Famille d’appareils**, sélectionnez la famille d’appareils ciblée par l’application dans laquelle votre publicité sera utilisée. Les options disponibles sont: **UWP (Windows10)**, **PC/tablette (Windows8.1)** ou **Mobile (Windows Phone8.x)**.

5.  Cliquez sur **Créer une publicité**. La nouvelle publicité apparaît en haut de la liste de la section **Publicités disponibles** de cette page.

6.  Pour chaque publicité générée, vous voyez un **ID de l’application** et un **ID de l’unité publicitaire**. Pour afficher les publicités dans votre application, vous devez utiliser ces valeurs dans le code de votre application:

    -   Si votre application affiche des bannières, affectez ces valeurs aux propriétés [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) et [AdUnitId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.adunitid.aspx) de votre objet **AdControl**. Pour plus d’informations, voir [AdControl en XAML et .NET](adcontrol-in-xaml-and--net.md) et [AdControl en HTML 5 et Javascript](adcontrol-in-html-5-and-javascript.md).

    -   Si votre application affiche des spots publicitaires, transmettez ces valeurs à la méthode [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) de votre objet **InterstitialAd**. Pour plus d’informations, voir [Spots](interstitial-ads.md).

    -   Si votre application affiche des publicités natives, transmettez ces valeurs aux paramètres *applicationId* et *adUnitId* du constructeur [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.nativeadsmanager.aspx). Pour plus d’informations, consultez [Publicités natives](../monetize/native-ads.md).

7. Si votre application est de type UWP pour Windows10, vous pouvez éventuellement activer la médiation publicitaire pour **AdControl** en configurant les paramètres de la section [Médiation publicitaire](../publish/monetize-with-ads.md#mediation). La médiation publicitaire vous permet d’optimiser vos revenus publicitaires et vos capacités de promotion d’application en affichant des spots issus de plusieurs réseaux publicitaires, y compris les publicités d’autres réseaux payants tels que Taboola et Smaato et les publicités des campagnes de promotion d’applications Microsoft. Par défaut, nous choisissons automatiquement les paramètres de médiation de publicité de votre application à l’aide d’algorithmes d’apprentissage machine afin de vous aider à optimiser vos revenus publicitaires sur les marchés pris en charge par votre application, mais vous pouvez éventuellement configurer manuellement vos paramètres de médiation.

<span id="manage" />
## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Gérer des unités publicitaires pour plusieurs contrôles publicitaires dans votre application

Vous pouvez utiliser plusieurs contrôles de bannière, spot et publicité native dans une seule application. Dans ce scénario, nous vous recommandons d’attribuer une unité publicitaire différente à chaque contrôle. L’utilisation de différentes unités publicitaires pour chaque contrôle vous permet de [configurer les paramètres de médiation](../publish/monetize-with-ads.md#mediation) séparément et d'obtenir des [données de rapport](../publish/advertising-performance-report.md) discrètes pour chaque contrôle. Cela permet également à nos services de mieux optimiser les publicités que nous servons à votre application.

> [!IMPORTANT]
> Vous pouvez utiliser chaque unité publicitaire dans une seule application. Si vous utilisez une unité publicitaire dans plusieurs applications, les publicités ne seront pas servies à cette unité publicitaire.

## <a name="related-topics"></a>Rubriques associées

* [Valeurs du mode test](test-mode-values.md)
* [AdControl en XAML et .NET](adcontrol-in-xaml-and--net.md)
* [AdControl en HTML5 et JavaScript](adcontrol-in-html-5-and-javascript.md)
* [Spots](interstitial-ads.md)
* [Publicités natives](../monetize/native-ads.md)


 

 
