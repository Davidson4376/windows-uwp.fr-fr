---
author: jnHs
Description: Here’s some important info you’ll need to ensure that you receive payment for your apps, in-app products (IAPs), and advertising earnings.
title: Rémunération
ms.assetid: 37D1EF45-C4A8-4849-8819-3D4A4898215C
ms.author: wdg-dev-content
ms.date: 02/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, paiements, ventes d’applications, revenu de l’application, revenu, frais d’utilisation du Store, mise en attente des paiements, pourcentage
ms.localizationpriority: medium
ms.openlocfilehash: 0c128bedd1c889f4c2dcf0565c7c10575eb75013
ms.sourcegitcommit: 00d27738325d6db5b5e481911ae7fac0711b05eb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/07/2018
ms.locfileid: "3661395"
---
# <a name="getting-paid"></a>Rémunération
Voici quelques informations importantes dont vous aurez besoin pour vérifier que vous avez reçu le paiement pour vos applications et extensions, ainsi que vos revenus publicitaires.

> [!IMPORTANT]
> Pour recevoir l’argent des ventes d’applications dans le MicrosoftStore, vous devez [configurer votre compte de paiement et remplir les déclarations fiscales appropriées](setting-up-your-payout-account-and-tax-forms.md).

## <a name="store-fee"></a>Frais d’utilisation du Windows Store

Quand vous [créez un compte de développeur](http://go.microsoft.com/fwlink/p/?LinkID=615100), vous acceptez le [Contrat développeur d’applications](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement). Ce contrat détaille la relation qui vous unit à Microsoft en ce qui concerne la vente d'applications dans le Microsoft Store, notamment les frais facturés par Microsoft pour chaque vente réalisée par le biais du Store.

Dans la plupart des cas, les frais du Windows Store sont de 30 %. Ces frais sont officiellement définis dans le [Contrat développeur d’applications](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement). Pour toute question, reportez-vous à ce document.

Les frais d’utilisation du Store s’appliquent à toutes les ventes d’application collectées par le Microsoft Store, y compris aux ventes d'extensions.


## <a name="price-tiers"></a>Niveaux de prix

Le ou les niveaux de prix que vous sélectionnez fixent le [prix de vente](set-and-schedule-app-pricing.md#base-price) dans tous les pays où vous souhaitez distribuer votre application. Vous pouvez également utiliser des fonctionnalités de tarification supplémentaires, telles que la [fixation de prix différents pour des marchés distincts](set-and-schedule-app-pricing.md#override-base-price-for-specific-markets) ou la [mise en vente de votre application](put-apps-and-add-ons-on-sale.md).

Vous pouvez proposer votre application gratuitement, ou vous pouvez choisir un prix que les clients devront verser pour l’acquérir. Les niveaux de prix commencent à 0,99USD, avec des incréments supplémentaires (1,09USD, 1,19USD, etc.). Les incréments entre les niveaux de prix augmentent en même temps que le prix.

> [!NOTE] 
> Ces niveaux de prix s’appliquent également à toutes les extensions que vous proposez dans votre application.

Chaque niveau de prix a une valeur correspondante dans chacune des devises du WindowsStore. Ces valeurs vous aident à vendre vos applications à un prix comparable dans le monde entier. Cependant, en raison des fluctuations des taux de change, le montant exact des ventes peut varier légèrement d’une devise à l’autre.

Vous avez également la possibilité d’entrer le prix de format libre de votre choix, dans la devise locale d’un marché spécifique. Lorsque vous effectuez cette opération, le prix ne sera pas ajusté (même si le taux de conversion change), sauf si vous soumettez une mise à jour avec un nouveau prix. 

N'oubliez pas que le prix que vous sélectionnez peut inclure la taxe de vente ou la taxe sur la valeur ajoutée que vos clients doivent payer. Pour plus d'informations, consultez l'article [Informations fiscales pour les applications payantes](tax-details-for-paid-apps.md).


## <a name="payout-reporting"></a>Rapports sur les paiements

Vous pouvez accéder à vos informations de paiement et télécharger des rapports dans la page **Synthèse des paiements** dans le tableau de bord du Centre de développement Windows. Pour plus d'informations sur les données figurant sur cette page et sur la façon dont nous classons vos revenus, consultez l'article [Synthèse des paiements](payout-summary.md).


## <a name="payout-timeframe"></a>Période de paiement

Les paiements sont effectués sur une base mensuelle (à condition que le seuil de paiement applicable ait été atteint et que vous n’ayez pas mis le paiement en attente comme décrit ci-dessous). Généralement, nous transférons les paiements dus pour un mois donné vers le 15 de ce mois. Notez qu’un délai de 3 à 10jours ouvrés supplémentaires est nécessaire pour l’enregistrement des montants sur votre compte de paiement. Pour plus d’informations, consultez la page [Seuils de paiement, méthodes et délais](payment-thresholds-methods-and-timeframes.md).


##  <a name="payout-hold-status"></a>État de mise en attente des paiements

Par défaut, nous envoyons les paiements sur une base mensuelle comme décrit ci-dessus. Toutefois, vous avez la possibilité de mettre vos paiements en attente, auquel cas nous ne pouvons pas envoyer des paiements vers votre compte. Si vous choisissez de mettre vos paiements en attente, nous continuons à enregistrer les revenus que vous gagnez et à en fournir les détails dans votre **Résumé du paiement**. Toutefois, nous n’envoyons pas de paiement vers votre compte tant que vous n’avez pas annulé la mise en attente. 

Pour mettre des paiements en attente, accédez à **Paramètres du compte**. Sous **Détails financiers**, dans la section **État de mise en attente des paiements**, basculez le curseur sur **Activé**. Vous pouvez modifier l’état de mise en attente du paiement à tout moment, mais n’oubliez pas que votre décision a un impact sur le paiement mensuel suivant. Par exemple, si vous voulez mettre en attente le paiement du mois d’avril, configurez l’état de mise en attente des paiements sur **Activé** avant la fin du mois de mars.

Une fois que vous avez configuré l’état de mise en attente des paiements sur **Activé**, tous les paiements sont mis en attente jusqu’à ce que vous basculiez à nouveau le curseur sur **Désactivé**. Lorsque vous procédez ainsi, vous êtes inclus dans le cycle de paiement mensuel suivant (à condition que les seuils de paiement applicables aient été atteints). Par exemple, si vous avez mis vos paiements en attente, mais que vous souhaitez qu’un paiement soit généré en juin, vous devez configurer l’état de mise en attente des paiements sur **Désactivé** avant la fin du mois de mai.

> [!NOTE]
> La valeur que vous sélectionnez dans **État de mise en attente des paiements** s’applique à **toutes** les sources de revenus payées par le biais du Centre de développement Windows (MicrosoftStore, publicité, Place de marché MicrosoftAzure, etc.). Vous ne pouvez pas sélectionner des états de mise en attente différents pour chaque source de revenus.


 

 




