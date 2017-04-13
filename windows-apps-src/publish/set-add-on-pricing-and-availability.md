---
author: jnHs
Description: "Lors de la soumission d’un module complémentaire, les options de la page Tarification et disponibilité déterminent le prix et les conditions de disponibilité."
title: "Définir le prix et la disponibilité d’un module complémentaire"
ms.assetid: B3D4B753-716B-460B-A3B1-ED5712ECD694
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 2902b81e777662e0d71bf10553ed82087c5cee9a
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="set-add-on-pricing-and-availability"></a>Définir le prix et la disponibilité d’un module complémentaire


Quand vous soumettez un module complémentaire, les options de la page **Tarification et disponibilité** déterminent le prix auquel facturer ce module et la façon dont ce dernier doit être proposé aux clients.

## <a name="base-price"></a>Prix de base


Vous devez sélectionner un prix de base pour votre module complémentaire. Ces niveaux de prix sont identiques à ceux des applications et commencent à 0,99USD. Vous pouvez également mettre gratuitement à disposition votre module complémentaire.

## <a name="markets-and-custom-prices"></a>Marchés et prix personnalisés


Par défaut, votre module complémentaire sera indiqué à son prix de base dans tous les marchés possibles, y compris dans les éventuels marchés que nous ajouterons par la suite.

Toutefois, comme pour les applications, vous avez la possibilité de choisir les marchés dans lesquels vous souhaitez proposer votre module complémentaire. Dans la plupart des cas, vous sélectionnerez le même ensemble de marchés que l’application, mais vous pouvez modifier les marchés sélectionnés à votre convenance. En outre, vous pouvez définir des prix personnalisés afin d’être en mesure de facturer des prix différents pour votre module complémentaire selon les marchés.

Pour en savoir plus et pour obtenir la liste complète des marchés disponibles, voir [Définition des prix et sélection du marché](define-pricing-and-market-selection.md).

## <a name="sale-pricing"></a>Prix de vente


Si vous souhaitez proposer votre module complémentaire à un prix réduit pendant une durée limitée, vous pouvez créer et planifier une vente. Pour plus d’informations, voir [Vendre des applications et des modules complémentaires à prix réduit](put-apps-and-add-ons-on-sale.md).

## <a name="distribution-and-visibility"></a>Distribution et visibilité


Vous pouvez déterminer si votre module complémentaire doit être proposé à l’achat aux clients. Choisissez l’une des options suivantes:

-   **Disponible à l’achat et affichable dans la description de votre application** : cette option constitue la sélection par défaut et le choix recommandé, sauf si vous voulez restreindre l’accès à votre module complémentaire. Laissez cette option sélectionnée pour les modules complémentaires qui seront disponibles pour tous les clients.
-   **Disponible à l’achat, mais non affiché dans la description de votre application**: cette option permet aux clients d’acheter le module complémentaire à partir de votre application, mais n’affichera pas ce module dans la description de votre application dans le WindowsStore. Utilisez cette option uniquement si l’offre n’est pas largement disponible, par exemple lors des périodes initiales de test interne.
-   **Plus disponible à l’achat et non affiché dans la description de votre application.** Le choix de cette option signifie que le module complémentaire ne s’affichera pas dans la description de votre application et qu’aucun nouveau client ne pourra l’acheter. Toutefois, **cette option n’est pas prise en charge pour les clients qui utilisent Windows8.1 ou une version antérieure**. Si votre application n’est pas disponible sur Windows8.1 ou une version antérieure, le module complémentaire restera disponible à l’achat pour ces clients. Pour arrêter de proposer ce module complémentaire aux clients qui utilisent Windows8.1 ou une version antérieure, vous devrez mettre à jour votre application en supprimant le code proposant le module complémentaire, puis publier une nouvelle soumission de l’application. Nous vous recommandons de suivre cette procédure, même si votre application ne cible pas Windows8.1 ou une version antérieure; en effet, vos clients bénéficieront d’une meilleure expérience si vous ne leur proposez jamais un module complémentaire dont vous avez décidé d’arrêter la mise à disposition.
    
 > **Remarque** Le choix de ce paramétrage et/ou la soumission d’une mise à jour d’application supprimant le module complémentaire du code de votre application n’ont pas d’incidence sur les clients qui ont déjà acheté ce module complémentaire, quel que soit le système d’exploitation qu’ils utilisent.


## <a name="publish-date"></a>Date de publication

Vous pouvez indiquer la date de publication de votre soumission d’application en choisissant une option dans la section **Date de publication**.

-   Pour que votre soumission devienne accessible dans le WindowsStore le plus tôt possible, choisissez l’option **Publier mon module complémentaire dès qu’il obtient une certification**.
-   Pour choisir la date à laquelle votre soumission doit être publiée, sélectionnez l’option **Publier ce module complémentaire manuellement**. Vous pourrez alors effectuer cette opération à partir de la page de degré de certification en cliquant sur **Publier maintenant** ou en sélectionnant une date spécifique, comme décrit ci-après.
-   Pour vous assurer que la soumission ne sera pas publiée avant une date donnée, choisissez l’option **Pas avant le \[date\]**. Avec cette option, votre soumission sera publiée aussitôt que possible à la date spécifiée ou après. La date doit être postérieure de 24 heures au moins. En parallèle de la date, vous pouvez également définir l’heure à laquelle la publication de la soumission doit démarrer.

 > **Remarque** Des retards lors de la certification ou de la publication peuvent faire que la sortie réelle intervient plus tard que la date demandée. Le WindowsStore ne peut pas garantir que votre module complémentaire (ou mise à jour) sera disponible à une date spécifique.
 

 




