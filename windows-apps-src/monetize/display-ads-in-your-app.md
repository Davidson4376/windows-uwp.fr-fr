---
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: Le SDK Microsoft Advertising vous offre plusieurs moyens de monétiser votre application grâce aux publicités.
title: Afficher des publicités dans votre application avec le SDK Microsoft Advertising
ms.date: 06/20/2018
ms.topic: article
keywords: windows 10, uwp, publicités, publicité, bannière, contrôle de publicité, spot
ms.localizationpriority: medium
ms.openlocfilehash: 0ef3050e2583674bf6cd5a601dbde1500f6b457e
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372550"
---
# <a name="display-ads-in-your-app-with-the-microsoft-advertising-sdk"></a>Afficher des publicités dans votre application avec le SDK Microsoft Advertising

Augmentez vos opportunités de revenus en plaçant des publicités dans votre application pour plateforme Windows universelle (UWP) sous Windows 10 à l’aide du SDK Microsoft Advertising. Notre plateforme de monétisation ad offre une variété de formats publicitaires qui peuvent être intégrées en toute transparence dans votre médiation prend en charge et les applications avec de nombreux réseaux ad populaires. Notre plateforme est conforme aux normes OpenRTB, VAST 2.x, MRAID 2, et VPAID 3 et est compatible avec MOAT et IAS. 

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
    <a href="https://aka.ms/ads-sdk-uwp">Installer le Kit de développement Microsoft Advertising SDK</a>
</td>
<td align="left"><img src="images/write-code.png" alt="Develop icon" /></td>
<td align="left"><b>Guides du développeur</b><br/><br/>
    <a href="banner-ads.md">Annonces de bannière</a>
    <br/>
    <a href="interstitial-ads.md">Publicités INTERSTITIELLES</a>
    <br/>
    <a href="native-ads.md">Publicités natives</a>
    </td>
<td align="left"><img src="images/api-reference.png" alt="API ref icon" /></td>
<td align="left"><b>Autres ressources</b><br/><br/>
    <a href="set-up-ad-units-in-your-app.md">Paramétrez ad dans votre application</a>
    <br/>
    <a href="best-practices-for-ads-in-apps.md">Meilleures pratiques</a>
    <br/>
    <a href="https://docs.microsoft.com/uwp/api/overview/advertising">Référence de l’API</a>
    </td>
</tr>
</tbody>
</table>

## <a name="step-1-install-the-microsoft-advertising-sdk"></a>Étape 1 : Installer le SDK Microsoft Advertising

Pour commencer, installez le [kit SDK Microsoft Advertising](https://aka.ms/ads-sdk-uwp) sur l’ordinateur de développement que vous utilisez pour créer votre application. Pour des instructions d’installation, voir [cet article](install-the-microsoft-advertising-libraries.md).

## <a name="step-2-implement-ads-in-your-app"></a>Étape 2 : Intégrer des publicités dans votre application

Le SDK Microsoft Advertising fournit plusieurs types de contrôles de publicités différents que vous pouvez utiliser dans votre application. Choisissez les types de publicités qui conviennent le mieux à votre scénario, puis ajoutez du code à votre application pour afficher ces publicités. Pendant cette étape, vous allez utiliser une unité publicitaire de test pour voir comment votre application restitue les publicités au cours du test.

### <a name="banner-ads"></a>Bannières publicitaires

Il s'agit de publicités à affichage statique qui utilisent une partie rectangulaire d’une page de votre application pour afficher du contenu publicitaire. Ces publicités peuvent s'actualiser automatiquement à intervalles réguliers. Il s’agit d’un bon point de départ si vous débutez avec les publicités dans votre application.

Pour obtenir des instructions et des exemples de code, voir [cet article](adcontrol-in-xaml-and--net.md).

![addreferences](images/banner-ad.png)

### <a name="interstitial-video-and-interstitial-banner-ads"></a>Spots vidéo et spots de bannières publicitaires

Ce sont des publicités plein écran qui obligent en général l’utilisateur à visionner une vidéo ou à cliquer dessus pour poursuivre sa progression dans l'application ou dans le jeu. Nous prenons en charge deux types de spots publicitaires : bannières et vidéo.

Pour obtenir des instructions et des exemples de code, voir [cet article](interstitial-ads.md).

![addreferences](images/interstitial-ad.png)

### <a name="native-ads"></a>Publicités natives

Il s’agit de publicités basées sur un composant. Chaque élément de l'annonce publicitaire (par exemple, le titre, l’image, la description et le texte de l’appel à l’action) est fourni à votre application sous forme d'élément individuel que vous pouvez intégrer dans votre application en utilisant vos propres polices, couleurs et autres composants d’interface utilisateur.

Pour obtenir des instructions et des exemples de code, voir [cet article](native-ads.md).

![addreferences](images/native-ad.png)

<span id="ad-mediation"/>

## <a name="step-3-create-an-ad-unit-and-configure-mediation"></a>Étape 3 : Créer une unité d’ad et configurer la médiation

Une fois que vous avez fini de tester votre application et vous êtes prêt à soumettre au Store, créer une unité d’ad sur le [les publicités dans l’application](../publish/in-app-ads.md) page dans l’espace partenaires. Ensuite, mettez à jour le code de votre application pour utiliser cette unité publicitaire afin que votre application reçoit des publicités dynamiques. Pour plus d’informations, voir [Configurer des unités publicitaires dans votre application](set-up-ad-units-in-your-app.md#live-ad-units).

Par défaut, votre application affiche des publicités issues du réseau Microsoft pour les publicités payées. Pour optimiser vos revenus publicitaires, vous pouvez activer la [médiation publicitaire](ad-mediation-service.md) pour votre unité publicitaire afin d'afficher des publicités à partir de réseaux de publicités payées supplémentaires, tels que Taboola et Smaato. Vous pouvez également augmenter vos capacités de promotion d'application en affichant des publicités provenant des campagnes de promotion d'applications Microsoft.

Pour commencer à utiliser la médiation publicitaire dans votre application UWP, [configurez les paramètres de médiation publicitaire](../publish/in-app-ads.md#mediation-settings) de votre unité publicitaire. Par défaut, nous configurons automatiquement les paramètres de médiation à l’aide d’algorithmes d’apprentissage automatique pour vous aider à optimiser vos revenus publicitaires sur les marchés pris en charge par votre application. Cependant, vous avez également la possibilité de sélectionner manuellement les réseaux que vous souhaitez utiliser. Dans tous les cas, les paramètres de médiation sont entièrement configurés sur nos serveurs, par conséquent vous n'avez pas besoin de modifier le code dans votre app.    

## <a name="step-4-submit-your-app-and-review-performance"></a>Étape 4 : Soumettez votre application et examinez les performances

Une fois que vous avez terminé de développer votre application grâce à ads, vous pouvez [soumettre votre application mise à jour](https://docs.microsoft.com/windows/uwp/publish/app-submissions) dans partenaires pour la rendre disponible dans le Store. Les apps qui affichent des publicités doivent respecter les exigences supplémentaires qui sont spécifiées dans la [section 10.10 des politiques du Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) et dans l'[Annexe E du Contrat du développeur d'application](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement).

Une fois que votre application est publiée et disponible dans le Store, vous pouvez consulter votre [les rapports de performances de publicité](../publish/advertising-performance-report.md) centre partenaires et continuer à apporter des modifications à vos paramètres de médiation pour optimiser les performances de vos annonces. Vos revenus publicitaires sont inclus dans votre [résumé du paiement](../publish/payout-summary.md).

<span id="additional-help" />

## <a name="additional-help"></a>Aide supplémentaire

Pour obtenir une aide supplémentaire sur l'utilisation du SDK Microsoft Advertising, utilisez les ressources suivantes.

|  Tâche    | Resource |               
|----------|-------|
| Signaler un bogue ou obtenir un support assisté en matière de publicité     | Visitez la [page de support](https://developer.microsoft.com/en-us/windows/support) et choisissez **Publicités dans l’app**.        |
| Bénéficier du support de la communauté     | Consultez le [forum](https://go.microsoft.com/fwlink/p/?LinkId=401266).       |
| Télécharger des exemples de projet qui montrent comment ajouter des bannières et des spots publicitaires aux applications.     | Voir [Exemples de publicité sur GitHub](https://aka.ms/githubads).       |
| Découvrir les dernières opportunités de monétisation pour les applications Windows     | Visitez [Monétiser vos applications](https://developer.microsoft.com/store/monetize).        |

## <a name="windows-81-and-windows-phone-8x-apps"></a>Applications Windows 8.1 et Windows Phone 8.x

Pour les applications Windows 8.1 et Windows Phone 8.x, nous fournissons le [SDK Microsoft Advertising pour Windows et Windows Phone 8.x](https://aka.ms/store-8-sdk). Pour plus d’informations concernant l’utilisation de ce SDK pour afficher des publicités dans des applications Windows 8.1 et Windows Phone 8.x, consultez [cet article](https://docs.microsoft.com/en-us/previous-versions/windows/apps/dn792120(v=win.10)).

## <a name="related-topics"></a>Rubriques connexes

* [SDK Microsoft Advertising](https://aka.ms/ads-sdk-uwp)
* [Rapport sur les performances publicitaires](../publish/advertising-performance-report.md)
* [Programme de serveurs de publication Windows Premium Ads](windows-premium-ads-publishers-program.md)
