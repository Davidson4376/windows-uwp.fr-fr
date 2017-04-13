---
author: jnHs
Description: "Si votre application utilise la médiation publicitaire, ou affiche des bannières ou des spots publicitaires de Microsoft Advertising, utilisez la page Monétisation &gt; Monétiser avec des publicités afin de gérer votre utilisation des publicités."
title: "Monétiser à l’aide des publicités"
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 6418fe1b47ac89e8decb135aa9a2108b3b95ef82
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="monetize-with-ads"></a>Monétiser à l’aide des publicités


Si votre application utilise un contrôle **AdMediatorControl**, **AdControl** ou **InterstitialAd** pour afficher des bannières ou des spots publicitaires, utilisez la page **Monétisation** &gt; **Monétiser avec des publicités** afin de gérer votre utilisation des publicités.

## <a name="windows-ad-mediation"></a>Médiation publicitaire Windows


Si votre application utilise la médiation publicitaire, utilisez cette section pour configurer vos paramètres de médiation et ajouter les paramètres requis pour chacun des réseaux publicitaires utilisés par votre application. Pour plus d’informations sur les options de cette section, voir [Soumettre votre application et configurer une médiation publicitaire](https://msdn.microsoft.com/library/windows/apps/mt219689).

La médiation publicitaire vous permet d’optimiser vos revenus publicitaires dans l’application par médiation des demandes de bannière publicitaire à partir de plusieurs réseaux publicitaires. Pour plus d’informations sur la médiation publicitaire, voir [Utiliser la médiation publicitaire pour optimiser les revenus](https://msdn.microsoft.com/library/windows/apps/mt219691).

## <a name="coppa-compliance"></a>Conformité avec la réglementation COPPA

Dans le cadre de la réglementation COPPA (Online Privacy Protection Act) relative à la protection de la vie privée des enfants sur Internet, vous devez avertir Microsoft si votre application s’adresse aux enfants de moins de 13 ans. Si vous utilisez le Centre de développement pour indiquer à Microsoft que votre application s’adresse aux enfants de moins de 13 ans, Microsoft prendra les mesures nécessaires pour désactiver les services de publicité comportementale lors de la diffusion de publicités dans votre application. Si votre application s’adresse aux enfants de moins de 13 ans, la réglementation COPPA vous impose certaines obligations.

Pour plus d’informations sur les obligations qui vous incombent en vertu de la réglementation COPPA, veuillez consulter [cette page](http://go.microsoft.com/fwlink/p/?linkid=536558).

## <a name="microsoft-affiliate-ads"></a>Annonces des affiliés Microsoft

Cochez la case dans cette section si vous voulez afficher les annonces des affiliés Microsoft dans votre application. Si vous activez cette case à cocher, les annonces concernant les produits disponibles dans le Store, y compris la musique, les jeux, les films, les applications, le matériel et les logiciels, sont affichées dans votre application si aucune annonce n’est disponible via les autres réseaux publicitaires. Lorsqu’un utilisateur clique sur les annonces et achète des produits au sein d’une fenêtre d’attribution donnée du Windows Store, vous touchez une commission sur les achats approuvés.

Si vous changez cette sélection, il n’est pas nécessaire de republier l’application pour que les modifications prennent effet. Pour plus d’informations sur les annonces des affiliés Microsoft, voir [À propos des annonces des affiliés](about-affiliate-ads.md).

> **Remarque**  Si votre application utilise la médiation publicitaire (autrement dit, elle utilise un contrôle **AdMediatorControl** pour afficher des annonces), elle peut seulement afficher les annonces des affiliés si vos paramètres de médiation publicitaire sont configurés pour afficher des annonces à partir de Microsoft.

## <a name="community-ads"></a>Annonces de la communauté

Cochez la case dans cette section si vous souhaitez faire la promotion croisée de votre application avec des applications d’autres développeurs. Si vous cochez cette case, puis que vous [créez une campagne d’annonces de la communauté](create-an-ad-campaign-for-your-app.md), votre application affiche les annonces d’applications publiées par d’autres développeurs ayant également créé des campagnes d’annonces de la communauté, et les annonces de leurs applications s’affichent dans votre application. Les annonces de la communauté sont gratuites et elles s’affichent seulement lorsqu’aucune annonce d’autres réseaux publicitaires n’est disponible.

Si vous changez cette sélection, il n’est pas nécessaire de republier l’application pour que les modifications prennent effet. Pour en savoir plus sur les annonces de la communauté, voir [À propos des annonces de la communauté](about-community-ads.md).

## <a name="microsoft-advertising-ad-units"></a>Unités publicitaires Microsoft Advertising

Utilisez cette section pour créer une unité publicitaire Microsoft Advertising. Il vous faudra créer des unités publicitaires uniquement dans les scénarios suivants:

-   Votre application affiche des bannières à l’aide d’un objet [AdControl](https://msdn.microsoft.com/library/mt313154.aspx).
-   Votre application affiche des spots publicitaires à l’aide d’un objet [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx).

Pour créer une unité publicitaire pour ces scénarios:

1.  Nommez l’unité publicitaire.
2.  Sélectionnez le type d’unité publicitaire (**Bannière**, **Spot vidéo** ou **Bannière spot publicitaire**).
3.  Sélectionnez le type d’appareil (**Mobile** ou **PC/tablette**).
4.  Cliquez sur **Créer une publicité**.

Vos unités publicitaires apparaissent dans un tableau situé en bas de cette section. Pour chaque unité publicitaire, vous voyez l’**ID de l’application** et l’**ID de l’unité publicitaire**. Pour afficher les publicités dans votre application, vous devez utiliser ces valeurs dans votre code :

-   Si votre application affiche des bannières, affectez ces valeurs aux propriétés [ApplicationId](https://msdn.microsoft.com/library/mt313174.aspx) et [AdUnitId](https://msdn.microsoft.com/library/mt313171.aspx) de votre objet [AdControl](https://msdn.microsoft.com/library/mt313154.aspx). Pour plus d’informations, voir [AdControl en XAML et .NET](../monetize/adcontrol-in-xaml-and--net.md) et [AdControl en HTML5 et JavaScript](../monetize/adcontrol-in-html-5-and-javascript.md).
-   Si votre application affiche des spots publicitaires, transmettez ces valeurs à la méthode [RequestAd](https://msdn.microsoft.com/library/mt313192.aspx) de votre objet [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx). Pour plus d’informations, consultez [Spots publicitaires](../monetize/interstitial-ads.md).

> **Remarque**   Si votre application utilise un objet **AdMediatorControl** pour afficher les bannières publicitaires de Microsoft Advertising, il n’est pas nécessaire de demander des unités publicitaires. Dans ce scénario, les unités publicitaires Microsoft Advertising sont automatiquement générées pour vous.

 

 

 
