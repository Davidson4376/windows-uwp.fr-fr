---
Description: La page Tarification et disponibilité du processus de soumission d’application vous permet de déterminer le prix de votre application, la mise à disposition ou non d’une version d’évaluation gratuite, ainsi que le mode, la date et l’emplacement d’accessibilité de votre application auprès des clients.
title: Définir la tarification et la disponibilité d’une application
ms.assetid: 37BE7C25-AA74-43CD-8969-CBA3BD481575
---

# Définir la tarification et la disponibilité d’une application


La page **Tarification et disponibilité** du [processus de soumission d’application](app-submissions.md) vous permet de déterminer le prix de votre application, la mise à disposition ou non d’une version d’évaluation gratuite, ainsi que le mode, la date et l’emplacement d’accessibilité de votre application auprès des clients. Cet article décrit les options disponibles sur cette page et les éléments à prendre en compte pour la spécification de ces informations.

## Prix de base


Le premier élément de cette page vous permet de sélectionner un prix de base pour votre application. Vous pouvez choisir de mettre à disposition votre application gratuitement ou sélectionner l’un des niveaux de prix disponibles. Vous devez définir un prix de base pour être en mesure de soumettre votre application.

Pour plus d’informations, voir l’article [Définition des prix et sélection du marché](define-pricing-and-market-selection.md).

## Version d’évaluation gratuite


De nombreux développeurs offrent aux utilisateurs la possibilité d’essayer gratuitement leur application à l’aide de la fonctionnalité d’essai fournie par le Windows Store. Par défaut, une application n’est pas disponible sous forme de version d’évaluation gratuite ; si vous souhaitez en proposer une, sélectionnez une valeur dans la liste déroulante **Évaluation gratuite**.

Choisissez **La version d’évaluation n’expire jamais** pour permettre aux clients d’accéder à votre application gratuitement pendant une période indéfinie. Si vous voulez les inciter à acheter la version complète ultérieurement, veillez à ajouter du code pour [exclure ou limiter des fonctionnalités de la version d’évaluation](https://msdn.microsoft.com/library/windows/apps/mt219685).

Vous avez également la possibilité de sélectionner une évaluation limitée dans le temps, d’**1 jour**, de **7 jours**, de **15 jours** ou de **30 jours**. Vous pouvez toujours limiter certaines fonctionnalités ou donner accès à toutes les fonctionnalités pendant la période d’évaluation.

> **Remarque** Les versions d’évaluation à durée limitée ne sont pas proposées aux clients qui utilisent Windows Phone 8.1 ou une version antérieure.

## Marchés et prix personnalisés


Par défaut, votre application sera indiquée à son prix de base dans tous les marchés possibles. Vous pouvez modifier ce paramétrage dans la section **Marchés et prix personnalisés** en incluant ou excluant des marchés spécifiques et en changeant le prix de l’application pour n’importe quel marché dans lequel vous proposez l’application. Pour plus d’informations, voir l’article [Définition des prix et sélection du marché](define-pricing-and-market-selection.md).

## Prix de vente


Si vous souhaitez proposer votre application à un prix réduit pendant une durée limitée, vous pouvez créer et planifier une vente. Pour plus d’informations, voir [Vendre des applications et des produits in-app à prix réduit](put-apps-and-iaps-on-sale.md).

## Distribution et visibilité


La section **Distribution et visibilité** vous permet de définir des restrictions relatives aux modes de découverte et d’acquisition de votre application.

Le paramètre par défaut est **Rendre votre application accessible dans le Windows Store**. Cala signifie que dans le Store, votre application sera mise à disposition des clients qui y auront accès via un lien direct ou par le biais d’autres méthodes, comme la recherche, la navigation et l’intégration dans des listes organisées.

Si vous souhaitez masquer votre application dans le Windows Store tout en la rendant accessible à certains utilisateurs, utilisez l’une des options ci-après pour limiter sa disponibilité. Notez qu’aucun client sur Windows 8 ou Windows 8.1 n’aura accès à l’application si vous sélectionnez l’une de ces options.

-   **Masquer cette application et empêcher l’acquisition. Les clients disposant d’un code promotionnel peuvent la télécharger sur les appareils Windows 10** : aucun client ne peut rechercher votre application ou y accéder dans le Windows Store, mais vous pouvez [générer des codes promotionnels](generate-promotional-codes.md) afin de la rendre disponible pour certains utilisateurs sur Windows 10. Ceux-ci peuvent utiliser le lien et le code pour obtenir votre application gratuitement, même si vous ne l’offrez à aucun autre client.
-   **Masquer cette application dans le Windows Store. Les clients accédant directement à la description de l’application peuvent malgré tout la télécharger, sauf sur Windows 8 et Windows 8.1** : aucun client ne peut rechercher votre application ou y accéder dans le Windows Store, mais tous les clients cliquant sur le lien direct vers la description de votre application peuvent télécharger l’application sur les appareils exécutant Windows 10 ou Windows Phone 8.1 et les versions antérieures.
-   **Masquer cette application et la rendre disponible uniquement aux utilisateurs spécifiés ci-dessous, qui peuvent la télécharger sur les appareils Windows Phone 8.x. Un code promotionnel peut être utilisé pour télécharger cette application sur les appareils Windows 10** : aucun client ne peut rechercher votre application ou y accéder sur le Windows Store, et seuls les clients Windows Phone 8.x dont vous saisissez les adresses de messagerie (associées à leurs comptes Microsoft) dans la zone (en les séparant par des points-virgules) peuvent la télécharger en cliquant sur lien direct vers la description. Vous pouvez également [générer des codes promotionnels](generate-promotional-codes.md) afin de la rendre disponible à certains utilisateurs sur Windows 10. Cette option est souvent utilisée pour le [test bêta](beta-testing-and-targeted-distribution.md) sur Windows Phone 8.1 et versions antérieures. Notez que vous pouvez activer cette option uniquement si vous n’avez jamais publié l’application avec l’option **Distribution et visibilité** définie sur **Tout le monde peut trouver votre application sur le Windows Store**.

> **Remarque** Pour arrêter d’offrir une application aux nouveaux clients, cliquez sur **Rendre votre application indisponible**, sur la page Vue d’ensemble de l’application. Quelques heures après que vous avez confirmé vouloir la rendre indisponible, votre application disparaît du Windows Store. Dès lors, aucun nouveau client ne pourra y accéder, quelle que soit la méthode. Cette action remplacera les options définies ici : aucun nouveau client ne disposera d’un accès. Si vous décidez de la remettre à disposition des clients, vous pouvez à tout moment cliquer sur **Rendre votre application disponible** sur la page Vue d’ensemble de l’application. Pour en savoir plus, consultez l’article [Suppression d’une application du Windows Store](guidance-for-app-package-management.md#removing-an-app-from-the-store).

## Familles d’appareils Windows 10

Cette section vous permet d’indiquer quels types d’appareils Windows 10 les clients peuvent utiliser pour acquérir l’application. (Si votre package ne fonctionne pas sur un certain type d’appareil, nous ne le proposons pas en téléchargement pour ce type d’appareil.)

> **Important** Pour empêcher complètement une certaine famille d’appareils Windows 10 d’obtenir votre application, vous devez mettre à jour l’élément [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903) dans votre manifeste appx pour cibler uniquement la famille d’appareils que vous voulez prendre en charge (c’est-à-dire, **Windows.Mobile** ou **Windows.Desktop**), au lieu de laisser la valeur **Windows.Universal** (pour la famille d’appareils universelle) que Microsoft Visual Studio inclut dans le manifeste appx par défaut.

Par défaut, les cases **Mobile** et **Bureau** sont cochées. Nous vous recommandons de laisser les cases à cocher activées, sauf si vous avez une raison spécifique de limiter les types d’appareils Windows 10 pouvant acquérir votre application. Par exemple, vous avez peut-être créé des packages Windows universels, mais savez que vous devez encore tester certaines fonctionnalités de l’application sur des appareils mobiles. Dans ce cas, pour empêcher de nouveaux clients de télécharger l’application sur des appareils mobiles Windows 10, vous pouvez désactiver la case à cocher **Mobile**. Si vous estimez ultérieurement être prêt à proposer l’application aux clients sur appareils mobiles Windows 10, vous pouvez créer une soumission en activant la case à cocher **Mobile**.

Si vous avez testé votre application pour vous assurer qu’elle s’exécute correctement sur Microsoft HoloLens, vous pouvez également cocher la case **Holographique** pour proposer l’application aux clients HoloLens. Pour plus d’informations sur la création, le test et la publication d’applications holographiques, voir la [Vue d’ensemble du développement holographique Windows](http://dev.windows.com/holographic/development_overview).

Notez que les sélections effectuées dans cette section s’appliquent à l’ensemble de vos packages d’application, indépendamment de la version de système d’exploitation ciblée (Windows 10, Windows 8.x, Windows Phone 8.x, etc.). Toutefois, elles affectent la disponibilité uniquement pour les clients qui utilisent des appareils Windows 10 (non pas Windows 8.x ou Windows Phone 8.x).

Il est également important de savoir que les sélections que vous opérez ici s’appliquent uniquement aux nouvelles acquisitions. Quiconque disposant déjà de votre application peut continuer à l’utiliser et obtenir les mises à jour que vous soumettez, même si vous supprimez cette famille d’appareils ici. Cela s’applique même aux clients ayant acquis votre application avant la mise à niveau vers Windows 10. Par exemple, si vous avez une application publiée avec des packages Windows Phone 8.1, puis ajoutez un package Windows 10 (UWP) à la même application, qui cible la famille d’appareils universelle, les clients mobiles Windows 10 qui disposaient de votre package Windows Phone 8.1 recevront une mise à jour vers ce package Windows 10 (UWP), même si vous avez désactivé la case à cocher **Mobile** (car il ne s’agit pas d’une nouvelle acquisition, mais d’une mise à jour). En revanche, si vous ne fournissez pas de package Windows 10 (UWP) ciblant la famille d’appareils universelle ou d’appareils mobiles, vos clients mobiles Windows 10 resteront avec le package Windows Phone 8.1.

Pour plus d’informations sur les familles d’appareils, voir le [Guide des applications pour la plateforme Windows universelle (UWP)](https://msdn.microsoft.com/library/windows/apps/dn894631) et [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903).

> **Remarque** Vous verrez également une case à cocher permettant d’indiquer si vous souhaitez permettre à Microsoft de rendre l’application disponible pour de futures familles d’appareils Windows 10. Nous recommandons de conserver cette case à cocher activée pour que votre application puisse être disponible pour davantage de clients potentiels à mesure que de nouvelles familles d’appareils seront introduites.

## Gestion des licences organisationnelles


Par défaut, votre application peut être proposée aux entreprises avec une formule d’achat en volume. Vous pouvez indiquer si et comment votre application peut être proposée dans cette section.

Pour plus d’informations, voir [Options de gestion des licences organisationnelles](organizational-licensing.md).

## Date de publication


Vous pouvez indiquer le moment de la publication de votre application (ou de la mise à jour) en choisissant une option dans la section **Date de publication**.

-   Pour que votre soumission devienne accessible dans le Windows Store le plus tôt possible, choisissez l’option **Publier cette soumission dès qu’elle aura obtenu la certification**.
-   Pour choisir la date à laquelle votre soumission doit être publiée, sélectionnez l’option **Publier cette soumission manuellement**. Vous pourrez alors effectuer cette opération à partir de la page de degré de certification en cliquant sur **Publier maintenant** ou en sélectionnant une date spécifique, comme décrit ci-après.
-   Pour vous assurer que la soumission ne sera pas publiée avant une date donnée, choisissez l’option **Pas avant le \[date\]**. Avec cette option, votre soumission sera publiée aussitôt que possible à la date spécifiée ou après. La date doit être postérieure de 24 heures au moins. En parallèle de la date, vous pouvez également définir l’heure à laquelle la publication de la soumission doit démarrer.
    > **Remarque** Des retards lors de la certification ou de la publication peuvent faire que la sortie réelle intervient plus tard que la date demandée. Le Windows Store ne peut pas garantir que votre application (ou mise à jour) sera disponible à une date spécifique.

Vous pouvez également modifier la date de sortie après avoir soumis votre application, à condition que l’étape **Publier** n’ait pas encore commencé pour l’application.
 

 






<!--HONumber=Mar16_HO5-->


