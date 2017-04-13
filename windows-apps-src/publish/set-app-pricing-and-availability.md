---
author: jnHs
Description: "La page Tarification et disponibilité du processus de soumission d’application vous permet de déterminer le prix de votre application, la mise à disposition ou non d’une version d’évaluation gratuite, ainsi que le mode, la date et l’emplacement d’accessibilité de votre application auprès des clients."
title: "Définir la tarification et la disponibilité d’une application"
ms.assetid: 37BE7C25-AA74-43CD-8969-CBA3BD481575
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: cf781345d0fe089f779db9b42fcf9eb56f229028
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="set-app-pricing-and-availability"></a>Définir la tarification et la disponibilité d’une application


La page **Tarification et disponibilité** du [processus de soumission d’application](app-submissions.md) vous permet de déterminer le prix de votre application, la mise à disposition ou non d’une version d’évaluation gratuite, ainsi que le mode, la date et l’emplacement d’accessibilité de votre application auprès des clients. Cet article décrit les options disponibles sur cette page et les éléments à prendre en compte pour la spécification de ces informations.

## <a name="base-price"></a>Prix de base


Le premier élément de cette page vous permet de sélectionner un prix de base pour votre application. Vous pouvez choisir de mettre à disposition votre application gratuitement ou sélectionner l'un des niveaux de prix disponibles. Vous devez définir un prix de base pour être en mesure de soumettre votre application.

Pour plus d’informations, voir l’article [Définition des prix et sélection du marché](define-pricing-and-market-selection.md).

## <a name="free-trial"></a>Version d’évaluation gratuite


De nombreux développeurs offrent aux utilisateurs la possibilité d’essayer gratuitement leur application à l’aide de la fonctionnalité d’essai fournie par le Windows Store. Par défaut, une application n’est pas disponible sous forme de version d’évaluation gratuite ; si vous souhaitez en proposer une, sélectionnez une valeur dans la liste déroulante **Évaluation gratuite**.

Choisissez **La version d’évaluation n’expire jamais** pour permettre aux clients d’accéder à votre application gratuitement pendant une période indéfinie. Si vous voulez les inciter à acheter la version complète ultérieurement, veillez à ajouter du code pour [exclure ou limiter des fonctionnalités de la version d’évaluation](../monetize/in-app-purchases-and-trials.md).

Vous avez également la possibilité de sélectionner une évaluation limitée dans le temps, d’**1 jour**, de **7 jours**, de **15 jours** ou de **30 jours**. Vous pouvez toujours limiter certaines fonctionnalités ou donner accès à toutes les fonctionnalités pendant la période d’évaluation.

> **Remarque** Les versions d’évaluation à durée limitée ne sont pas proposées aux clients qui utilisent Windows Phone 8.1 ou une version antérieure.

## <a name="markets-and-custom-prices"></a>Marchés et prix personnalisés


Par défaut, votre application sera indiquée à son prix de base dans tous les marchés possibles. Vous pouvez modifier ce paramétrage dans la section **Marchés et prix personnalisés** en incluant ou excluant des marchés spécifiques et en changeant le prix de l’application pour n’importe quel marché dans lequel vous proposez l’application. Pour plus d’informations, voir l’article [Définition des prix et sélection du marché](define-pricing-and-market-selection.md).

## <a name="sale-pricing"></a>Prix de vente


Si vous souhaitez proposer votre application à un prix réduit pendant une durée limitée, vous pouvez créer et planifier une vente. Pour plus d’informations, voir [Vendre des applications et des modules complémentaires à prix réduit](put-apps-and-add-ons-on-sale.md).

## <a name="distribution-and-visibility"></a>Distribution et visibilité


La section **Distribution et visibilité** vous permet de définir des restrictions relatives aux modes de découverte et d’acquisition de votre application.

Le paramètre par défaut est **Rendre votre application accessible dans le Windows Store**. Cala signifie que dans le Store, votre application sera mise à disposition des clients qui y auront accès via un lien direct ou par le biais d’autres méthodes, comme la recherche, la navigation et l’intégration dans des listes organisées.

Si vous souhaitez masquer votre application dans le Windows Store tout en la rendant accessible à certains utilisateurs, utilisez l’une des options ci-après pour limiter sa disponibilité. Notez qu’aucun client sur Windows8 ou Windows8.1 n’aura accès à l’application si vous sélectionnez l’une de ces options.

-   **Masquer cette application et empêcher l’acquisition. Les clients disposant d’un code promotionnel peuvent la télécharger sur les appareils Windows 10** : aucun client ne peut rechercher votre application ou y accéder dans le Windows Store, mais vous pouvez [générer des codes promotionnels](generate-promotional-codes.md) afin de la rendre disponible pour certains utilisateurs sur Windows 10. Ceux-ci peuvent utiliser le lien et le code pour obtenir votre application gratuitement, même si vous ne l’offrez à aucun autre client.
-   **Masquer cette application dans le Windows Store. Les clients accédant directement à la description de l’application peuvent malgré tout la télécharger, sauf sur Windows 8 et Windows 8.1** : aucun client ne peut rechercher votre application ou y accéder dans le Windows Store, mais tous les clients cliquant sur le lien direct vers la description de votre application peuvent télécharger l’application sur les appareils exécutant Windows 10 ou Windows Phone 8.1 et les versions antérieures.
-   **Masquer cette application et la rendre disponible uniquement aux utilisateurs spécifiés ci-dessous, qui peuvent la télécharger sur les appareils Windows Phone 8.x. Un code promotionnel peut être utilisé pour télécharger cette application sur les appareils Windows 10** : aucun client ne peut rechercher votre application ou y accéder sur le Windows Store, et seuls les clients Windows Phone 8.x dont vous saisissez les adresses de messagerie (associées à leurs comptes Microsoft) dans la zone (en les séparant par des points-virgules) peuvent la télécharger en cliquant sur lien direct vers la description. Vous pouvez également [générer des codes promotionnels](generate-promotional-codes.md) afin de la rendre disponible à certains utilisateurs sur Windows 10. Cette option est souvent utilisée pour le [test bêta](beta-testing-and-targeted-distribution.md) sur Windows Phone 8.1 et versions antérieures. Notez que vous pouvez activer cette option uniquement si vous n’avez jamais publié l’application avec l’option **Distribution et visibilité** définie sur **Tout le monde peut trouver votre application sur le Windows Store**.

> **Remarque** Pour arrêter d’offrir une application aux nouveaux clients, cliquez sur **Rendre votre application indisponible**, sur la page Vue d’ensemble de l’application. Quelques heures après que vous avez confirmé vouloir la rendre indisponible, votre application disparaît du Windows Store. Dès lors, aucun nouveau client ne pourra y accéder, quelle que soit la méthode. Cette action remplacera les options définies ici: aucun nouveau client ne disposera d’un accès. Si vous décidez de la remettre à disposition des clients, vous pouvez à tout moment cliquer sur **Rendre votre application disponible** sur la page Vue d’ensemble de l’application. Pour en savoir plus, consultez l’article [Suppression d’une application du Windows Store](guidance-for-app-package-management.md#removing-an-app-from-the-store).

## <a name="windows-10-device-families"></a>Familles d’appareils Windows10

la disponibilité de la famille d’appareils est désormais gérée sur la page **Packages** de votre soumission. Pour plus d’informations, consultez la section [Disponibilité de la famille d’appareils](upload-app-packages.md#device-family-availability).

## <a name="organizational-licensing"></a>Gestion des licences organisationnelles


Par défaut, votre application peut être proposée aux entreprises avec une formule d’achat en volume. Vous pouvez indiquer si et comment votre application peut être proposée dans cette section.

Pour plus d’informations, voir [Options de gestion des licences organisationnelles](organizational-licensing.md).

## <a name="publish-date"></a>Date de publication


Vous pouvez indiquer le moment de la publication de votre application (ou de la mise à jour) en choisissant une option dans la section **Date de publication**.

-   Pour que votre soumission devienne accessible dans le Windows Store le plus tôt possible, choisissez l’option **Publier cette soumission dès qu’elle aura obtenu la certification**.
-   Pour choisir la date à laquelle votre soumission doit être publiée, sélectionnez l’option **Publier cette soumission manuellement**. Vous pourrez alors effectuer cette opération à partir de la page de degré de certification en cliquant sur **Publier maintenant** ou en sélectionnant une date spécifique, comme décrit ci-après.
-   Pour vous assurer que la soumission ne sera pas publiée avant une date donnée, choisissez l’option **Pas avant le \[date\]**. Avec cette option, votre soumission sera publiée aussitôt que possible à la date spécifiée ou après. La date doit être postérieure de 24 heures au moins. En parallèle de la date, vous pouvez également définir l’heure à laquelle la publication de la soumission doit démarrer.

   > **Remarque** Des retards lors de la certification ou de la publication peuvent faire que la sortie réelle intervient plus tard que la date demandée. Le Windows Store ne peut pas garantir que votre application (ou mise à jour) sera disponible à une date spécifique.

Vous pouvez également modifier la date de sortie après avoir soumis votre application, à condition que l’étape **Publier** n’ait pas encore commencé pour l’application.
 

 
