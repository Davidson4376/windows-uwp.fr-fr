---
author: jnHs
Description: "Lors de la soumission d’un module complémentaire, les options de la page Tarification et disponibilité déterminent le prix et les conditions de disponibilité."
title: "Définir le prix et la disponibilité d’un module complémentaire"
ms.assetid: B3D4B753-716B-460B-A3B1-ED5712ECD694
ms.author: wdg-dev-content
ms.date: 08/03/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 09671148e670acbfdbc944558a2738712f848dd5
ms.sourcegitcommit: de6bc8acec2cd5ebc36bb21b2ce1a9980c3e78b2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2017
---
# <a name="set-add-on-pricing-and-availability"></a>Définir le prix et la disponibilité d’un module complémentaire


Quand vous soumettez un module complémentaire, les options de la page **Tarification et disponibilité** déterminent le prix auquel facturer ce module et la façon dont ce dernier doit être proposé aux clients.

> [!NOTE]
> Nous avons récemment mis à jour les options disponibles sur cette page. Si vous disposiez de soumissions en cours avant que ces options ne deviennent disponibles, il est possible que ces soumissions continuent d’afficher les anciennes options. Vous pouvez supprimer ces soumissions, puis en créer une autre si vous souhaitez utiliser les dernières options. Dans le cas contraire, les dernières options deviendront disponibles avec la prochaine mise à jour une fois que vous aurez publié vos soumissions en cours.

## <a name="markets"></a>Marchés

Par défaut, votre module complémentaire sera indiqué à son prix de base dans tous les marchés possibles, y compris dans les éventuels marchés que nous ajouterons par la suite.

Toutefois, comme pour les applications, vous avez la possibilité de choisir les marchés dans lesquels vous souhaitez proposer votre module complémentaire. Dans la plupart des cas, vous sélectionnerez le même ensemble de marchés que l’application, mais vous pouvez modifier les marchés sélectionnés à votre convenance. 

Pour en savoir plus et pour obtenir la liste complète des marchés disponibles, consultez l’article [Définir la sélection du marché](define-pricing-and-market-selection.md).

## <a name="visibility"></a>Visibilité

Vous pouvez déterminer si votre extension doit être ou non proposée à l’achat aux clients. 

L’option par défaut est **Can be displayed in the parent product’s Store listing**. Laissez cette option sélectionnée pour les extensions qui seront disponibles pour tous les clients. 

Pour les extensions que vous ne voulez pas mettre à la disposition générale, sélectionnez **Masquée dans le WindowsStore** et l’une des options suivantes:

-   **Available for purchase from within the parent product only**: cette option permet à tout client d’acheter l’extension à partir de votre application, mais n’affichera pas cette extension dans la description de votre application dans le WindowsStore. Utilisez cette option uniquement si l’offre n’est pas mise à la disposition générale, par exemple lors des périodes initiales de test interne.
-   **Empêcher l’acquisition: tout client disposant d’un lien direct peut voir la description du produit dans le WindowsStore, mais ne peut le télécharger que s’il possède déjà le produit ou qu’il dispose d’un code promotionnel et utilise un appareil Windows10. This add-on is not displayed in the parent product’s listing**: cette option signifie que l’extension ne s’affiche pas dans la description de votre application, et qu’aucun nouveau client ne peut acheter cette extension. Toutefois, **cette option n’est pas prise en charge pour les clients qui utilisent Windows8.1 ou une version antérieure**. Si votre application n’est pas disponible sur Windows8.1 ou une version antérieure, le module complémentaire restera disponible à l’achat pour ces clients. Pour arrêter de proposer ce module complémentaire aux clients qui utilisent Windows8.1 ou une version antérieure, vous devrez mettre à jour votre application en supprimant le code proposant le module complémentaire, puis publier une nouvelle soumission de l’application. Nous vous recommandons de suivre cette procédure, même si votre application ne cible pas Windows8.1 ou une version antérieure; en effet, vos clients bénéficieront d’une meilleure expérience si vous ne leur proposez jamais un module complémentaire dont vous avez décidé d’arrêter la mise à disposition.
    
 > [!NOTE] 
 > Le choix de l’option **Empêcher l’acquisition** et/ou la soumission d’une mise à jour d’application supprimant l’extension du code de votre application n’ont pas d’incidence sur les clients qui ont déjà acheté cette extension, quel que soit le système d’exploitation qu’ils utilisent.


## <a name="schedule"></a>Planification

Par défaut (sauf si vous avez sélectionné l’une des options **Masquée dans le WindowsStore** dans la section **Visibilité**), votre extension deviendra disponible pour les clients dès qu’elle aura obtenu la certification et terminé le processus de publication. Pour choisir d’autres dates, sélectionnez **Afficher les options** afin de développer cette section. 

Pour plus d’informations, consultez l’article [Configurer une planification précise de la publication](configure-precise-release-scheduling.md).


## <a name="pricing"></a>Tarification

Vous devez sélectionner un prix de base pour votre extension (sauf si vous avez sélectionné l’option **Empêcher l’acquisition** dans la section **Visibilité**), en choisissant soit **Gratuit**, soit l’un des niveaux de prix disponibles (qui commencent à 0,99USD).

Vous pouvez également planifier des modifications de tarifs pour indiquer la date et l’heure auxquelles le prix de l’extension doit changer. En outre, vous pouvez personnaliser ces changements pour des marchés spécifiques. 

Pour plus d’informations, consultez l’article [Définir et planifier le prix de l’application](set-and-schedule-app-pricing.md).


## <a name="sale-pricing"></a>Prix de vente

Si vous souhaitez proposer votre module complémentaire à un prix réduit pendant une durée limitée, vous pouvez créer et planifier une vente. Pour plus d’informations, consultez l’article [Commercialiser des applications et composants additionnels](put-apps-and-add-ons-on-sale.md).


## <a name="publish-date"></a>Date de publication

Par défaut, votre soumission déclenche le processus de publication dès qu’elle a obtenu sa certification, sauf si vous avez configuré des dates dans la [**section Planification**](#schedule) décrite ci-dessus. 

Pour contrôler la date à laquelle votre extension doit être publiée dans le WindowsStore, utilisez la section **Planification**. Pour la plupart des soumissions, vous devez utiliser cette section afin de planifier la publication et laisser la section **Date de publication** définie sur l’option par défaut, **Publier cette soumission dès qu’elle a obtenu la certification**. Cela n’a pas pour conséquence la publication de la soumission avant les dates que vous avez définies dans la section **Planifier**. Les dates que vous avez sélectionnées dans la section **Planification** déterminent le moment où votre extension devient disponible pour les clients dans le WindowsStore.

Si vous ne voulez pas encore définir de date de publication et que vous préférez que votre soumission reste non publiée jusqu’à ce que vous décidiez de déclencher manuellement le processus de publication, vous pouvez choisir **Publier cette soumission manuellement**. La sélection de cette option signifie que votre sélection ne sera pas publiée tant vous n’aurez pas indiqué qu’elle doit l’être. Une fois votre extension certifiée, vous pouvez la publier en sélectionnant **Publier maintenant** sur la page d’état de certification ou en sélectionnant une date spécifique, comme décrit ci-dessous.

Pour vous assurer que la soumission ne sera pas publiée avant une date donnée, choisissez l’option **Pas avant le \[date\]**. Avec cette option, votre soumission sera publiée aussitôt que possible à la date spécifiée ou après. La date doit être postérieure de 24 heures au moins. Parallèlement à la date, vous pouvez également définir l’heure à laquelle la publication de la soumission doit démarrer.
 
> [!NOTE]
> Des retards lors de la certification ou de la publication peuvent différer la sortie réelle par rapport à la date demandée. Le WindowsStore ne peut pas garantir que votre module complémentaire (ou mise à jour) sera disponible à une date spécifique.  



 




