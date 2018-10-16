---
author: Xansky
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: Le SDK MicrosoftAdvertising vous offre plusieurs moyens de monétiser votre application grâce aux publicités.
title: Afficher des publicités dans votre application avec le SDK MicrosoftAdvertising
ms.author: mhopkins
ms.date: 06/20/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, publicités, publicité, bannière, contrôle de publicité, spot
ms.localizationpriority: medium
ms.openlocfilehash: c0dde67e3f7ab43734ffb0bf2a5826cc54691e17
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4685023"
---
# <a name="display-ads-in-your-app-with-the-microsoft-advertising-sdk"></a>Afficher des publicités dans votre application avec le SDK MicrosoftAdvertising

Augmentez vos opportunités de revenus en plaçant des publicités dans votre application pour plateforme Windows universelle (UWP) sous Windows10 à l’aide du SDK MicrosoftAdvertising. Notre plateforme de monétisation des publicités offre une variété de formats de publicité qui peut être intégré en toute transparence dans vos applications et prend en charge médiation avec de nombreux réseaux publicitaires populaires. Notre plateforme est conforme à la OpenRTB, 2.x une grande, Definitions 2 et 3 VPAID normes et est compatible avec MOAT et IAS. 

<br/>

<table style="border: none !important;">
<colgroup>
<col width="10%" />
<col width="23%" />
<col width="10%" />
<col width="23%" />
<col width="10%" />
<col width="23%" />
</colgroup>
<tbody>
<tr>
<td align="left"><img src="images/install-sdk.png" alt="Install SDK icon" /></td>
<td align="left"><b>Prise en main</b><br/><br/>
    <a href="http://aka.ms/ads-sdk-uwp">Installer le SDK MicrosoftAdvertising</a>
</td>
<td align="left"><img src="images/write-code.png" alt="Develop icon" /></td>
<td align="left"><b>Guides pour développeurs</b><br/><br/>
    <a href="banner-ads.md">Bannières publicitaires</a>
    <br/>
    <a href="interstitial-ads.md">Spots</a>
    <br/>
    <a href="native-ads.md">Publicités natives</a>
    </td>
<td align="left"><img src="images/api-reference.png" alt="API ref icon" /></td>
<td align="left"><b>Autres ressources</b><br/><br/>
    <a href="set-up-ad-units-in-your-app.md">Configurer des unités publicitaires dans votre application</a>
    <br/>
    <a href="best-practices-for-ads-in-apps.md">Bonnes pratiques</a>
    <br/>
    <a href="https://msdn.microsoft.com/en-us/library/windows/apps/mt691884.aspx">Informations de référence de l’API</a>
    </td>
</tr>
</tbody>
</table>

## <a name="step-1-install-the-microsoft-advertising-sdk"></a>Étape1: Installer le SDK Microsoft Advertising

Pour commencer, installez le [kit SDK MicrosoftAdvertising](http://aka.ms/ads-sdk-uwp) sur l’ordinateur de développement que vous utilisez pour créer votre application. Pour des instructions d’installation, voir [cet article](install-the-microsoft-advertising-libraries.md).

## <a name="step-2-implement-ads-in-your-app"></a>Étape2: Intégrer des publicités dans votre application

Le SDK MicrosoftAdvertising fournit plusieurs types de contrôles de publicités différents que vous pouvez utiliser dans votre application. Choisissez les types de publicités qui conviennent le mieux à votre scénario, puis ajoutez du code à votre application pour afficher ces publicités. Pendant cette étape, vous allez utiliser une unité publicitaire de test pour voir comment votre application restitue les publicités au cours du test.

### <a name="banner-ads"></a>Bannières publicitaires

Il s'agit de publicités à affichage statique qui utilisent une partie rectangulaire d’une page de votre application pour afficher du contenu publicitaire. Ces publicités peuvent s'actualiser automatiquement à intervalles réguliers. Il s’agit d’un bon point de départ si vous débutez avec les publicités dans votre application.

Pour obtenir des instructions et des exemples de code, voir [cet article](adcontrol-in-xaml-and--net.md).

![addreferences](images/banner-ad.png)

### <a name="interstitial-video-and-interstitial-banner-ads"></a>Spots vidéo et spots de bannières publicitaires

Ce sont des publicités plein écran qui obligent en général l’utilisateur à visionner une vidéo ou à cliquer dessus pour poursuivre sa progression dans l'application ou dans le jeu. Nous prenons en charge deux types de spots publicitaires: bannières et vidéo.

Pour obtenir des instructions et des exemples de code, voir [cet article](interstitial-ads.md).

![addreferences](images/interstitial-ad.png)

### <a name="native-ads"></a>Publicités natives

Il s’agit de publicités basées sur un composant. Chaque élément de l'annonce publicitaire (par exemple, le titre, l’image, la description et le texte de l’appel à l’action) est fourni à votre application sous forme d'élément individuel que vous pouvez intégrer dans votre application en utilisant vos propres polices, couleurs et autres composants d’interface utilisateur.

Pour obtenir des instructions et des exemples de code, voir [cet article](native-ads.md).

![addreferences](images/native-ad.png)

<span id="ad-mediation"/>

## <a name="step-3-create-an-ad-unit-and-configure-mediation"></a>Étape3: Créer une unité publicitaire et configurer une médiation

Après avoir testé votre app, et une fois que vous êtes prêt à la soumettre au MicrosoftStore, créez une unité publicitaire sur la page [Publicités dans l'app](../publish/in-app-ads.md) dans le tableau de bord du Centre de développement Windows. Ensuite, mettez à jour le code de votre app pour utiliser cette unité publicitaire de sorte que votre app reçoive des publicités dynamiques. Pour plus d’informations, voir [Configurer des unités publicitaires dans votre application](set-up-ad-units-in-your-app.md#live-ad-units).

Par défaut, votre application affiche des publicités issues du réseau Microsoft pour les publicités payées. Pour optimiser vos revenus publicitaires, vous pouvez activer la [médiation publicitaire](ad-mediation-service.md) pour votre unité publicitaire afin d'afficher des publicités à partir de réseaux de publicités payées supplémentaires, tels que Taboola et Smaato. Vous pouvez également augmenter vos capacités de promotion d'application en affichant des publicités provenant des campagnes de promotion d'applications Microsoft.

Pour commencer à utiliser la médiation publicitaire dans votre application UWP, [configurez les paramètres de médiation publicitaire](../publish/in-app-ads.md#mediation-settings) de votre unité publicitaire. Par défaut, nous configurons automatiquement les paramètres de médiation à l’aide d’algorithmes d’apprentissage automatique pour vous aider à optimiser vos revenus publicitaires sur les marchés pris en charge par votre application. Cependant, vous avez également la possibilité de sélectionner manuellement les réseaux que vous souhaitez utiliser. Dans tous les cas, les paramètres de médiation sont entièrement configurés sur nos serveurs, par conséquent vous n'avez pas besoin de modifier le code dans votre app.    

## <a name="step-4-submit-your-app-and-review-performance"></a>Étape 4: Soumettre votre app et évaluer ses performances

Une fois que vous avez développé votre application contenant des publicités, vous pouvez [soumettre votre application mise à jour](https://docs.microsoft.com/windows/uwp/publish/app-submissions) dans le tableau de bord du Centre de développement pour qu’elle soit disponible dans le Windows Store. Les apps qui affichent des publicités doivent respecter les exigences supplémentaires qui sont spécifiées dans la [section 10.10 des politiques du MicrosoftStore](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) et dans l'[Annexe E du Contrat du développeur d'application](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement).

Une fois que votre application est publiée et disponible dans le Windows Store, vous pouvez examiner vos [rapports de performances des publicités](../publish/advertising-performance-report.md) dans le tableau de bord et continuer à apporter des modifications à vos paramètres de médiation afin d'optimiser les performances de vos annonces. Vos revenus publicitaires sont inclus dans votre [résumé du paiement](../publish/payout-summary.md).

<span id="additional-help" />

## <a name="additional-help"></a>Aide supplémentaire

Pour obtenir une aide supplémentaire sur l'utilisation du SDK MicrosoftAdvertising, utilisez les ressources suivantes.

|  Tâche    | Ressource |               
|----------|-------|
| Signaler un bogue ou obtenir un support assisté en matière de publicité     | Visitez la [page de support](https://developer.microsoft.com/en-us/windows/support) et choisissez **Publicités dans l’app**.        |
| Bénéficier du support de la communauté     | Consultez le [forum](http://go.microsoft.com/fwlink/p/?LinkId=401266).       |
| Télécharger des exemples de projet qui montrent comment ajouter des bannières et des spots publicitaires aux applications.     | Voir [Exemples de publicité sur GitHub](http://aka.ms/githubads).       |
| Découvrir les dernières opportunités de monétisation pour les applications Windows     | Consultez [Monétiser vos applications](https://developer.microsoft.com/store/monetize).        |

## <a name="windows-81-and-windows-phone-8x-apps"></a>Applications Windows8.1 et WindowsPhone8.x

Pour les applicationsWindows8.1 et WindowsPhone8.x, nous fournissons le [SDK Microsoft Advertising pour Windows et Windows Phone8.x](http://aka.ms/store-8-sdk). Pour plus d’informations concernant l’utilisation de ce SDK pour afficher des publicités dans des applications Windows8.1 et WindowsPhone8.x, consultez [cet article](https://docs.microsoft.com/en-us/previous-versions/windows/apps/dn792120(v=win.10)).

## <a name="related-topics"></a>Rubriques associées

* [SDK Microsoft Advertising](http://aka.ms/ads-sdk-uwp)
* [Rapport sur les performances publicitaires](../publish/advertising-performance-report.md)
* [Programme Éditeurs de publicités Premium Windows](windows-premium-ads-publishers-program.md)
