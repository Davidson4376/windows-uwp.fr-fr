---
Description: When submitting an add-on, the options on the Pricing and availability page determine what to charge for your add-on and how it should be offered to customers.
title: Définir le prix et la disponibilité d’une extension
ms.assetid: B3D4B753-716B-460B-A3B1-ED5712ECD694
ms.date: 10/31/2018
ms.topic: article
keywords: Windows10, uwp, extensions, iap, prix
ms.localizationpriority: medium
ms.openlocfilehash: 062337c82d2567d15b0eff1767ab157618da257e
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7710724"
---
# <a name="set-add-on-pricing-and-availability"></a>Définir le prix et la disponibilité d’une extension

Quand vous soumettez un module complémentaire dans [L’espace partenaires](https://partner.microsoft.com/dashboard), les options de la page **tarification et disponibilité** déterminent la quantité de facturer les clients pour votre extension et comment elle doit être proposé aux clients.

## <a name="markets"></a>Marchés

Par défaut, votre extension sera indiquée à son prix de base dans tous les marchés possibles, y compris dans les éventuels marchés que nous ajouterons par la suite.

Toutefois, comme pour les applications, vous avez la possibilité de choisir les marchés dans lesquels vous souhaitez proposer votre extension. Dans la plupart des cas, vous sélectionnerez le même ensemble de marchés que l’application, mais vous pouvez modifier les marchés sélectionnés à votre convenance. 

Pour en savoir plus et pour obtenir la liste complète des marchés disponibles, consultez l’article [Définir la sélection du marché](define-pricing-and-market-selection.md).

## <a name="visibility"></a>Visibilité

Vous pouvez déterminer si votre extension doit être ou non proposée à l’achat aux clients. 

L’option par défaut est **Can be displayed in the parent product’s Store listing**. Laissez cette option sélectionnée pour les extensions qui seront disponibles pour tous les clients. 

Pour les extensions que vous ne voulez pas mettre à la disposition générale, sélectionnez **Masquée dans le WindowsStore** et l’une des options suivantes:

-   **Disponible à l’achat à partir du produit parent uniquement**: cette option permet à tout client d’acheter le module complémentaire à partir de votre application, mais le module complémentaire ne sera pas affiché dans la description de votre application ou détectable dans le Windows Store. Utilisez cette option uniquement si l’offre n’est pas mise à la disposition générale, par exemple lors des périodes initiales de test interne.
-   **Empêcher l’acquisition: tout client disposant d’un lien direct peut voir la description du produit dans le WindowsStore, mais ne peut le télécharger que s’il possède déjà le produit ou qu’il dispose d’un code promotionnel et utilise un appareil Windows10. This add-on is not displayed in the parent product’s listing**: cette option signifie que l’extension ne s’affiche pas dans la description de votre application, et qu’aucun nouveau client ne peut acheter cette extension. Toutefois, **cette option n’est pas pris en charge pour les clients sur Windows8.1 ou une version antérieure**. Si votre application publiée précédemment est disponible sur Windows8.1 ou une version antérieure, le module complémentaire sera toujours disponible à l’achat pour ces clients. Pour arrêter de proposer le module complémentaire aux clients Windows8.1 ou une version antérieure, vous devez mettre à jour votre application en supprimant le code proposant le module complémentaire, puis publier une nouvelle soumission pour l’application. Il est recommandé de même si votre application ne cible pas Windows8.1 ou une version antérieure; Il est une meilleure expérience pour vos clients si vous leur proposez jamais un module complémentaire que vous avez choisi de rendre indisponible.
    
 > [!NOTE] 
 > Le choix de l’option **Empêcher l’acquisition** et/ou la soumission d’une mise à jour d’application supprimant l’extension du code de votre application n’ont pas d’incidence sur les clients qui ont déjà acheté cette extension, quel que soit le système d’exploitation qu’ils utilisent.


## <a name="schedule"></a>Planification

Par défaut (sauf si vous avez sélectionné l’une des options **Masquée dans le WindowsStore** dans la section **Visibilité**), votre extension deviendra disponible pour les clients dès qu’elle aura obtenu la certification et terminé le processus de publication. Pour choisir d’autres dates, sélectionnez **Afficher les options** afin de développer cette section. 

Pour plus d’informations, voir [Configurer le calendrier de publication exact](configure-precise-release-scheduling.md).


## <a name="pricing"></a>Tarification

Vous devez sélectionner un prix de base pour votre extension (sauf si vous avez sélectionné l’option **empêcher l’acquisition** dans la section **visibilité** ). La sélection par défaut est **libre**, par conséquent, si vous souhaitez facturer pour le module complémentaire, veillez à choisir l’un des niveaux de prix disponibles (en commençant à.99 USD).

Vous pouvez également planifier des modifications de tarifs pour indiquer la date et l’heure auxquelles le prix de l’extension doit changer. En outre, vous pouvez personnaliser ces changements pour des marchés spécifiques. 

> [!TIP]
> Pour les extensions d’abonnement, vous ne pouvez pas augmenter le prix après la publication de l’extension, en sélectionnant un prix de base supérieure dans une soumission ultérieure ou en planifiant un changement de prix qui augmente le prix. Vous pouvez sélectionner un prix inférieur à l’aide d’une de ces méthodes, mais une fois que le prix est baissé, vous ne serez en mesure de déclencher supérieur à ce nouveau prix. Pour cette raison, il est particulièrement important pour vous assurer que vous sélectionnez le niveau de prix approprié pour les extensions d’abonnement. 

Pour plus d’informations, consultez l’article [Définir et planifier le prix de l’application](set-and-schedule-app-pricing.md).


## <a name="sale-pricing"></a>Prix de vente

Si vous souhaitez proposer votre extension à un prix réduit pendant une durée limitée, vous pouvez créer et planifier une vente. Pour plus d’informations, consultez l’article [Commercialiser des applications et composants additionnels](put-apps-and-add-ons-on-sale.md).



