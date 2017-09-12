---
author: jnHs
Description: "La page Tarification et disponibilité du processus de soumission d’application vous permet de déterminer le prix de votre application, la mise à disposition ou non d’une version d’évaluation gratuite, ainsi que le mode, la date et l’emplacement d’accessibilité de votre application auprès des clients."
title: "Définir la tarification et la disponibilité d’une application"
ms.assetid: 37BE7C25-AA74-43CD-8969-CBA3BD481575
ms.author: wdg-dev-content
ms.date: 08/03/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 9686876bf0e5e3d89ce527eb07535f1c0a704f2a
ms.sourcegitcommit: 968187e803a866b60cda0528718a3d31f07dc54c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/03/2017
---
# <a name="set-app-pricing-and-availability"></a>Définir la tarification et la disponibilité d’une application

> [!NOTE]
> Nous avons récemment mis à jour les options disponibles sur cette page. Si vous disposiez de soumissions en cours avant que ces options ne deviennent disponibles, il est possible que ces soumissions continuent d’afficher les anciennes options. Si vous souhaitez utiliser les dernières options pour cette application, vous pouvez supprimer ces soumissions, puis en créer d’autres. Dans le cas contraire, les dernières options deviendront disponibles avec la prochaine mise à jour une fois que vous aurez publié vos soumissions en cours.

La page **Tarification et disponibilité** du [processus de soumission d’application](app-submissions.md) vous permet de déterminer le prix de votre application, la mise à disposition ou non d’une version d’évaluation gratuite, ainsi que le mode, la date et l’emplacement d’accessibilité de votre application auprès des clients. Cet article décrit les options disponibles sur cette page et les éléments à prendre en compte pour la spécification de ces informations.


## <a name="markets"></a>Marchés

Le Windows Store touche des clients dans plus de 200pays et régions dans le monde. Par défaut, nous allons proposer votre application sur tous les marchés possibles. Si vous préférez, vous pouvez choisir les marchés spécifiques sur lesquels vous souhaitez proposer votre application. 

Pour plus d’informations, voir l’article [Définir la sélection du marché](define-pricing-and-market-selection.md).


## <a name="visibility"></a>Visibilité

La section **Visibilité** vous permet de définir des restrictions relatives aux modes de découverte et d’acquisition de votre application.

Le paramètre par défaut est **Rendre votre produit accessible et détectable dans le Windows Store**. Cala signifie que dans le Store, votre application sera mise à disposition des clients qui y auront accès via un lien direct ou par le biais d’autres méthodes, comme la recherche, la navigation et l’intégration dans des listes organisées. 

Si vous souhaitez masquer votre application dans le Windows Store, mais en la rendant accessible à certaines personnes, sélectionnez **Afficher les options** pour développer la section, puis **Rendre ce produit disponible mais pas détectable dans le Windows Store**. Cela signifie qu’aucun client ne pourra trouver votre application dans le Windows Store via une recherche ou en naviguant, quelle que soit sa version du système d’exploitation. Vous devez également choisir l'une des versions suivantes pour déterminer comment votre application peut être obtenue.

>[!IMPORTANT]
> Chacune de ces options limite les versions des systèmes d’exploitation sur lesquels les clients peuvent acquérir votre application. Lisez attentivement les descriptions pour vous assurer que vous savez quelles versions des systèmes d’exploitation sont prises en charge. Notez que les clients sous Windows8 ou Windows8.1 ne pourront pas du tout obtenir l’application si vous choisissez l'une des options figurant sous **Rendre ce produit disponible mais pas détectable dans le Windows Store**. 

- **Lien direct uniquement: n’importe quel client ayant un lien direct vers la description du produit peut télécharger celui-ci, sauf sous Windows8.x.** Tout client parvenant à la description de votre application via un lien direct peut la télécharger sur un appareil exécutant Windows10 ou WindowsPhone8.1 et les versions antérieures. Avec cette option, les clients sous Windows8.x ne peuvent pas du tout obtenir l’application.
- **Arrêter l’acquisition: n’importe quel client ayant un lien direct peut voir la description du produit dans le Windows Store, mais ne peut télécharger ce produit que s'il en est déjà propriétaire ou s'il possède un code promotionnel et utilise un appareil Windows10.** Même si un client dispose d’un lien direct, il ne peut pas télécharger l’application, sauf s'il possède un [code promotionnel](generate-promotional-codes.md) et utilise un appareil Windows10. Lorsque vous fournissez un code à un client, celui-ci peut utiliser ce lien et le code pour obtenir gratuitement votre application (sous Windows10 uniquement), même si vous ne l’offrez à aucun autre client. En dehors d'un code promotionnel, il n’existe aucun moyen pour obtenir votre application.
- **Utilisateurs sous Windows Phone8.x uniquement: seuls les utilisateurs spécifiés ci-dessous peuvent télécharger ce produit sur un appareil Windows Phone8.x. Toute personne ayant un lien direct et un code promotionnel peut télécharger le produit sur un appareil Windows10.** Cette option peut ne pas apparaître pour toutes les soumissions. Elle ne s’applique que si vous disposez de packages pouvant s’exécuter sur Windows Phone8.x. Seuls les clients dont vous spécifiez l’adresse de messagerie (associée à leur compteMicrosoft) dans le champ (séparées par des points-virgules) peuvent télécharger l’application sur Windows Phone8.x via le lien d’accès direct à sa description. Vous pouvez également générer des codes promotionnels afin de la rendre disponible à certains utilisateurs sur Windows10, comme expliqué plus haut. 

> [!TIP]
> Pour arrêter d’offrir une application aux nouveaux clients, cliquez sur **Rendre votre application indisponible** sur la page Vue d’ensemble de l’application. Quelques heures après que vous avez confirmé vouloir la rendre indisponible, votre application disparaît du Windows Store. Dès lors, aucun nouveau client ne pourra y accéder, quelle que soit la méthode. Cette action remplacera les options définies ici: aucun nouveau client ne disposera d’un accès. Si vous décidez de la remettre à disposition des clients, vous pouvez à tout moment cliquer sur **Rendre votre application disponible** sur la page Vue d’ensemble de l’application. Pour en savoir plus, consultez l’article [Suppression d’une application du Windows Store](guidance-for-app-package-management.md#removing-an-app-from-the-store).

## <a name="schedule"></a>Planifier

Par défaut (sauf si vous avez sélectionné l'une des options **Rendre votre application disponible, mais pas détectable dans le Windows Store** dans la section **Visibilité**), votre application sera disponible pour les clients dès qu’elle aura obtenu la certification et terminé le processus de publication. Pour choisir d’autres dates, sélectionnez **Afficher les options** pour développer cette section. 

Pour plus d’informations, voir [Configurer le calendrier de publication exact](configure-precise-release-scheduling.md).

## <a name="display-release-date"></a>Afficher la date de publication

Par défaut, la date de publication de votre application sera la date à laquelle elle apparaît dans le Windows Store. Si vous souhaitez la remplacer et fournir une date de sortie personnalisée, vous pouvez cochez la case dans cette section, puis saisir la date de publication. Notez que la date de publication n’est pas toujours affichée dans les descriptions du Windows Store.

## <a name="pricing"></a>Tarification

Vous devez sélectionner un prix de base pour votre application (sauf si vous avez sélectionné l'une des options **Rendre votre application disponible, mais pas détectable dans le Windows Store** dans la section [Visibilité](#visibility)), en choisissant soit **Gratuit**, soit l’un des niveaux de prix disponibles. Vous pouvez également programmer des modifications de tarifs pour indiquer la date et l'heure à laquelle le prix de votre application doit changer. Vous pouvez aussi personnaliser ces changements pour des marchés spécifiques. 

Pour plus d’informations, voir [Fixer et planifier le prix des applications](set-and-schedule-app-pricing.md).

## <a name="free-trial"></a>Essai gratuit

De nombreux développeurs offrent aux utilisateurs la possibilité d’essayer gratuitement leur application à l’aide de la fonctionnalité d’essai fournie par le Windows Store. Par défaut, **Aucune version d’essai gratuit** est sélectionné. Il n’y aura alors aucune version d’essai de votre application. Si vous souhaitez en proposer une version d’essai, vous pouvez sélectionner une valeur à partir de la liste déroulante **Version d’essai gratuite**.

Vous pouvez choisir parmi deux types de version d’essai et vous avez la possibilité de configurer la date et l’heure à laquelle cette version d’essai doit démarrer et arrêter d'être proposée.

### <a name="time-limited"></a>Durée limitée

Choisissez **Durée limitée** pour permettre aux clients d’essayer votre application gratuitement pendant un certain nombre de jours: **1 jour**, **7jours**, **15jours** ou **30jours**. Vous pouvez limiter les fonctionnalités en ajoutant du code pour [exclure ou limiter des fonctionnalités de la version d’essai](../monetize/in-app-purchases-and-trials.md)ou vous pouvez donner accès à toutes les fonctionnalités pendant cette durée. 
> [!NOTE]
> Les versions d’essai limitées dans le temps ne sont pas proposées aux clients sous Windows10 build10.0.10586 ou versions antérieures, ni aux clients sous WindowsPhone8.1 et versions antérieures.

### <a name="unlimited"></a>Illimité

Choisissez **Illimité** pour permettre aux clients d’accéder à votre application gratuitement pendant une période indéfinie. Si vous voulez les inciter à acheter la version complète ultérieurement, veillez à ajouter du code pour [exclure ou limiter des fonctionnalités de la version d’évaluation](../monetize/in-app-purchases-and-trials.md).

### <a name="start-and-end-dates"></a>Dates de début et de fin

Par défaut, votre version d’essai sera disponible dès que votre application aura été publiée et elle sera proposée indéfiniment. Si vous le souhaitez, vous pouvez spécifier la date et l’heure à laquelle votre version d’évaluation doit commencer à être proposée et le moment où cette proposition doit s’arrêter. 

>[!NOTE]
> Ces dates ne s’appliquent qu'aux clients sous Windows10. Si votre version d’essai s’applique à des clients utilisant des versions antérieures du système d’exploitation, elle sera proposée aussi longtemps que votre produit sera disponible. 

Pour définir les dates auxquelles votre version d’essai doit être proposée, changez les paramètres **Commence le** et/ou **Se termine le** sur la liste déroulante en **au**, puis choisissez la date et l’heure. Si vous procédez ainsi, vous pouvez choisir **UTC**, afin que l’heure que vous sélectionnez s'exprime en temps universel coordonné (UTC), ou **Local**, afin que ces dates soient utilisées dans chaque fuseau horaire associé à un marché. (Notez que pour les marchés qui incluent plusieurs fuseaux horaires, un seul fuseau horaire de ces marchés sera utilisé. Pour les États-Unis, le fuseau horaire utilisé est celui de l'Est.) 

>[!NOTE]
> Contrairement à ce qui se fait dans la section [Planifier](configure-precise-release-scheduling.md), les dates que vous sélectionnez pour votre **version d’essai gratuite** ne peuvent pas être personnalisées pour des marchés spécifiques. 



## <a name="sale-pricing"></a>Prix de vente

Si vous souhaitez proposer votre application à un prix réduit pendant une durée limitée, vous pouvez créer et planifier une vente.

Pour plus d’informations, voir [Vendre des applications et des extensions à prix réduit](put-apps-and-add-ons-on-sale.md).


## <a name="organizational-licensing"></a>Gestion des licences organisationnelles

Par défaut, votre application peut être proposée aux entreprises avec une formule d’achat en volume. Vous pouvez indiquer si et comment votre application peut être proposée dans cette section.

Pour plus d’informations, voir [Options de gestion des licences organisationnelles](organizational-licensing.md).

## <a name="publish-date"></a>Date de publication

Par défaut, votre soumission déclenche le processus de publication dès qu’elle a obtenu sa certification, sauf si vous avez configuré des dates dans la section [**Planifier**](#schedule) décrite ci-dessus. 

Pour contrôler la date à laquelle votre application doit être publiée dans le Windows Store, utilisez la section **Planifier**. Pour la plupart des soumissions, vous devez utiliser cette section pour planifier la publication de votre application et laisser la section **Date de publication** sur l’option par défaut, **Publier cette soumission dès qu’elle a obtenu la certification**. Cela n’a pas pour conséquence la publication de la soumission avant les dates que vous avez définies dans la section **Planifier**. Les dates que vous avez sélectionnées dans la section **Planifier** déterminent le moment auquel votre application est disponible pour les clients dans le Windows Store.

Si vous ne voulez pas encore définir de date de publication et que vous préférez que votre soumission reste non publiée jusqu'à ce que vous décidiez de déclencher manuellement le processus de publication, vous pouvez choisir **Publier cette soumission manuellement.** La sélection de cette option signifie que votre sélection ne sera pas publiée tant vous n'aurez pas indiqué qu’elle doit l'être. Une fois votre application certifiée, vous pouvez la publier en sélectionnant **Publier maintenant** sur la page d’état de certification ou en sélectionnant une date spécifique, comme décrit ci-dessous.

Pour vous assurer que la soumission ne sera pas publiée avant une date donnée, choisissez l’option **Pas avant le \[date\]**. Avec cette option, votre soumission sera publiée aussitôt que possible à la date spécifiée ou après. La date doit être postérieure de 24 heures au moins. Parallèlement à la date, vous pouvez également définir l’heure à laquelle la publication de la soumission doit démarrer.
 
> [!NOTE]
> Des retards lors de la certification ou de la publication peuvent faire que la sortie réelle intervienne plus tard que la date demandée. Le Windows Store ne peut pas garantir que votre application (ou mise à jour) sera disponible à une date spécifique.  

Vous pouvez également modifier la date de sortie après avoir soumis votre application, à condition que l’étape Publier n’ait pas encore commencé pour l’application. 

