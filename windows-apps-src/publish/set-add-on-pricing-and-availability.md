---
author: jnHs
Description: When submitting an add-on, the options on the Pricing and availability page determine what to charge for your add-on and how it should be offered to customers.
title: Définir le prix et la disponibilité d’une extension
ms.assetid: B3D4B753-716B-460B-A3B1-ED5712ECD694
ms.author: wdg-dev-content
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, uwp, extensions, iap, prix
ms.localizationpriority: medium
ms.openlocfilehash: b5b7a6424fea3d62849e992f56b0b40ab72a55f5
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2018
ms.locfileid: "2814031"
---
# <a name="set-add-on-pricing-and-availability"></a>Définir le prix et la disponibilité d’une extension


Quand vous soumettez une extension, les options de la page **Tarification et disponibilité** déterminent le prix auquel facturer cette extension et la façon dont elle doit être proposée aux clients.

## <a name="markets"></a>Marchés

Par défaut, votre extension sera indiquée à son prix de base dans tous les marchés possibles, y compris dans les éventuels marchés que nous ajouterons par la suite.

Toutefois, comme pour les applications, vous avez la possibilité de choisir les marchés dans lesquels vous souhaitez proposer votre extension. Dans la plupart des cas, vous sélectionnerez le même ensemble de marchés que l’application, mais vous pouvez modifier les marchés sélectionnés à votre convenance. 

Pour en savoir plus et pour obtenir la liste complète des marchés disponibles, consultez l’article [Définir la sélection du marché](define-pricing-and-market-selection.md).

## <a name="visibility"></a>Visibilité

Vous pouvez déterminer si votre extension doit être ou non proposée à l’achat aux clients. 

L’option par défaut est **Can be displayed in the parent product’s Store listing**. Laissez cette option sélectionnée pour les extensions qui seront disponibles pour tous les clients. 

Pour les extensions que vous ne voulez pas mettre à la disposition générale, sélectionnez **Masquée dans le WindowsStore** et l’une des options suivantes:

-   **Available for purchase from within the parent product only**: cette option permet à tout client d’acheter l’extension à partir de votre application, mais n’affichera pas cette extension dans la description de votre application dans le WindowsStore. Utilisez cette option uniquement si l’offre n’est pas mise à la disposition générale, par exemple lors des périodes initiales de test interne.
-   **Empêcher l’acquisition: tout client disposant d’un lien direct peut voir la description du produit dans le WindowsStore, mais ne peut le télécharger que s’il possède déjà le produit ou qu’il dispose d’un code promotionnel et utilise un appareil Windows10. This add-on is not displayed in the parent product’s listing**: cette option signifie que l’extension ne s’affiche pas dans la description de votre application, et qu’aucun nouveau client ne peut acheter cette extension. Toutefois, **cette option n’est pas prise en charge pour les clients qui utilisent Windows8.1 ou une version antérieure**. Si votre application n’est pas disponible sur Windows8.1 ou une version antérieure, l'extension restera disponible à l’achat pour ces clients. Pour arrêter de proposer cette extension aux clients qui utilisent Windows8.1 ou une version antérieure, vous devrez mettre à jour votre application en supprimant le code proposant l'extension, puis publier une nouvelle soumission de l’application. Nous vous recommandons de suivre cette procédure, même si votre application ne cible pas Windows8.1 ou une version antérieure; en effet, vos clients bénéficieront d’une meilleure expérience si vous ne leur proposez jamais une extension dont vous avez décidé d’arrêter la mise à disposition.
    
 > [!NOTE] 
 > Le choix de l’option **Empêcher l’acquisition** et/ou la soumission d’une mise à jour d’application supprimant l’extension du code de votre application n’ont pas d’incidence sur les clients qui ont déjà acheté cette extension, quel que soit le système d’exploitation qu’ils utilisent.


## <a name="schedule"></a>Planification

Par défaut (sauf si vous avez sélectionné l’une des options **Masquée dans le WindowsStore** dans la section **Visibilité**), votre extension deviendra disponible pour les clients dès qu’elle aura obtenu la certification et terminé le processus de publication. Pour choisir d’autres dates, sélectionnez **Afficher les options** afin de développer cette section. 

Pour plus d’informations, voir [Configurer le calendrier de publication exact](configure-precise-release-scheduling.md).


## <a name="pricing"></a>Tarification

Vous devez sélectionner un prix de base pour votre module complémentaire (sauf si vous avez sélectionné l’option **Arrêter d’acquisition** dans la section de **visibilité** ). La sélection par défaut est **disponible**, si vous souhaitez payer pour le module complémentaire, veillez donc à choisir une des couches de prix disponibles (commençant à.99 EUR).

Vous pouvez également planifier des modifications de tarifs pour indiquer la date et l’heure auxquelles le prix de l’extension doit changer. En outre, vous pouvez personnaliser ces changements pour des marchés spécifiques. 

> [!TIP]
> Des modules complémentaires d’abonnement, vous ne pouvez pas déclencher le prix après avoir publié le module complémentaire, en sélectionnant un prix de base supérieur dans un envoi ultérieur ou en planifiant une modification de prix qui augmente le prix. Vous pouvez sélectionner un prix inférieur à l’aide d’une de ces méthodes, mais diminue le prix vous ne serez en mesure de déclencher supérieure à ce nouveau prix. Pour cette raison, il est particulièrement important pour vous assurer que vous sélectionnez le niveau de prix approprié des modules complémentaires d’abonnement. 

Pour plus d’informations, consultez l’article [Définir et planifier le prix de l’application](set-and-schedule-app-pricing.md).


## <a name="sale-pricing"></a>Prix de vente

Si vous souhaitez proposer votre extension à un prix réduit pendant une durée limitée, vous pouvez créer et planifier une vente. Pour plus d’informations, consultez l’article [Commercialiser des applications et composants additionnels](put-apps-and-add-ons-on-sale.md).



