---
Description: Lors de la soumission d’un module complémentaire, les options de la page Tarification et disponibilité déterminent le prix et les conditions de disponibilité.
title: Définir le prix et la disponibilité d’un module complémentaire
ms.assetid: B3D4B753-716B-460B-A3B1-ED5712ECD694
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, extensions, iap, prix
ms.localizationpriority: medium
ms.openlocfilehash: 062337c82d2567d15b0eff1767ab157618da257e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597804"
---
# <a name="set-add-on-pricing-and-availability"></a>Définir le prix et la disponibilité d’un module complémentaire

Lors de l’envoi d’un module complémentaire dans [partenaires](https://partner.microsoft.com/dashboard), les options sur la **tarification et disponibilité** page déterminer combien de facturer les clients pour votre module complémentaire et comment il doit être disponible pour les clients.

## <a name="markets"></a>Marchés

Par défaut, votre module complémentaire sera indiqué à son prix de base dans tous les marchés possibles, y compris dans les éventuels marchés que nous ajouterons par la suite.

Toutefois, comme pour les applications, vous avez la possibilité de choisir les marchés dans lesquels vous souhaitez proposer votre module complémentaire. Dans la plupart des cas, vous sélectionnerez le même ensemble de marchés que l’application, mais vous pouvez modifier les marchés sélectionnés à votre convenance. 

Pour en savoir plus et pour obtenir la liste complète des marchés disponibles, consultez l’article [Définir la sélection du marché](define-pricing-and-market-selection.md).

## <a name="visibility"></a>Visibilité

Vous pouvez déterminer si votre module complémentaire doit être proposé à l’achat aux clients. 

L’option par défaut est **Can be displayed in the parent product’s Store listing**. Laissez cette option sélectionnée pour les modules complémentaires qui seront disponibles pour tous les clients. 

Pour les extensions que vous ne voulez pas mettre à la disposition générale, sélectionnez **Masquée dans le Windows Store** et l’une des options suivantes :

-   **Achat d’au sein du produit parent uniquement**: Cette option permet à tous les clients à acheter le module complémentaire à partir de votre application, mais le module complémentaire ne sera pas affiché dans la liste de votre application Store ou détectables dans le Store. Utilisez cette option uniquement si l’offre n’est pas largement disponible, par exemple lors des périodes initiales de test interne.
-   **Acquisition de s’arrêter : Tous les clients avec un lien direct peuvent voir Store du produit liste, mais ils peuvent uniquement télécharger si le propriétaire du produit avant, ou un code promotionnel et utilisez un appareil Windows 10. Ce module complémentaire n’est pas affiché dans la liste du produit parent**: Le choix de cette option signifie que le module complémentaire ne s’affichera pas dans la description de votre application et qu’aucun nouveau client ne pourra l’acheter. Toutefois, **cette option n’est pas pris en charge pour les clients sur Windows 8.1 ou version antérieure**. Si votre application précédemment publiée est disponible sur Windows 8.1 ou une version antérieure, le module complémentaire sera toujours disponible à l’achat aux clients. Pour arrêter l’offre le module complémentaire aux clients sur Windows 8.1 ou version antérieure, vous devrez mettre à jour votre application pour supprimer le code qui offre le module complémentaire, puis publiez une nouvelle soumission de l’application. Il est recommandé de même si votre application ne cible pas Windows 8.1 ou une version antérieure ; Il est une meilleure expérience pour vos clients si vous leur offrez jamais un module complémentaire que vous avez choisi pour le rendre indisponible.
    
 > [!NOTE] 
 > Le choix de l’option **Empêcher l’acquisition** et/ou la soumission d’une mise à jour d’application supprimant l’extension du code de votre application n’ont pas d’incidence sur les clients qui ont déjà acheté cette extension, quel que soit le système d’exploitation qu’ils utilisent.


## <a name="schedule"></a>Planification

Par défaut (sauf si vous avez sélectionné l’une des options **Masquée dans le Windows Store** dans la section **Visibilité**), votre extension deviendra disponible pour les clients dès qu’elle aura obtenu la certification et terminé le processus de publication. Pour choisir d’autres dates, sélectionnez **Afficher les options** pour développer cette section. 

Pour plus d’informations, voir [Configurer le calendrier de publication exact](configure-precise-release-scheduling.md).


## <a name="pricing"></a>Tarification

Vous devez sélectionner un prix de base pour votre module complémentaire (sauf si vous avez sélectionné le **arrêter acquisition** option dans le **visibilité** section). La sélection par défaut est **gratuit**, par conséquent, si vous souhaitez payer pour le module complémentaire, veillez à choisir un des niveaux de prix disponibles (à partir de.99 USD).

Vous pouvez également planifier des modifications de tarifs pour indiquer la date et l’heure auxquelles le prix de l’extension doit changer. En outre, vous pouvez personnaliser ces changements pour des marchés spécifiques. 

> [!TIP]
> Pour les modules complémentaires d’abonnement, vous ne pouvez pas augmenter son prix après avoir publié le module complémentaire, en sélectionnant un prix de base plus élevé dans un envoi ultérieur ou en planifiant un changement de prix qui augmente le prix. Vous pouvez sélectionner un prix inférieur à l’aide de ces méthodes, mais une fois que le prix est plus bas vous ne pourrez déclencher supérieure à ce nouveau prix. Pour cette raison, il est particulièrement important pour être sûr que vous sélectionnez le niveau de prix approprié pour les modules complémentaires d’abonnement. 

Pour plus d’informations, voir [Définir et planifier le prix de l’application](set-and-schedule-app-pricing.md).


## <a name="sale-pricing"></a>Prix de vente

Si vous souhaitez proposer votre module complémentaire à un prix réduit pendant une durée limitée, vous pouvez créer et planifier une vente. Pour plus d’informations, voir [Vendre des applications et des composants additionnels à prix réduit](put-apps-and-add-ons-on-sale.md).



