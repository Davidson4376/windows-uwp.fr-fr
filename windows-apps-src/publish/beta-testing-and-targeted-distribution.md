---
author: jnHs
Description: "Le tableau de bord du Centre de développementWindows vous permet de rendre votre application accessible uniquement à des utilisateurs spécifiés. Vous pouvez ainsi demander à des testeurs de l’essayer avant de la proposer au public."
title: "Tests bêta et distribution ciblée"
ms.assetid: 38E4ED22-D6C1-40D8-9B16-6B3E51BD962E
ms.author: wdg-dev-content
ms.date: 08/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 7dd0e346e6be147935503eeb0b685568bfce726c
ms.sourcegitcommit: 6c6f3c265498d7651fcc4081c04c41fafcbaa5e7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2017
---
# <a name="beta-testing-and-targeted-distribution"></a>Tests bêta et distribution ciblée

Quelle que soit le soin avec lequel vous testez votre application, rien de vaut un test dans des conditions réelles, avec d’autres utilisateurs. Le tableau de bord du Centre de développementWindows vous permet de rendre une soumission d'application accessible uniquement à des utilisateurs spécifiés. Vous pouvez ainsi demander à des testeurs de l’essayer avant de la proposer au public. Les testeurs peuvent épingler des problèmes qui vous auraient échappés, comme des fautes d’orthographe, un flux d’application peu clair ou des erreurs pouvant entraîner un blocage de l’application. Vous pouvez ensuite résoudre ces problèmes avant de mettre à la disposition du public un produit finalement irréprochable.

Nous proposons différentes méthodes pour limiter la distribution de vos applications à vos testeurs seulement sans avoir besoin de créer une version distincte de celles-ci avec un nom et une identité de package différents. (Bien entendu, vous pouvez créer une application distincte réservée aux tests si vous le préférez. Le cas échéant, veillez à lui donner un nom différent du nom prévu pour l’application finale qui sera rendue publique.)

La méthode de distribution de votre application aux testeurs dépend des systèmes d’exploitation auxquels celle-ci est destinée. Les options ci-dessous sont disponibles pour Windows10 et Windows Phone8.1 et antérieur.

## <a name="making-your-app-available-to-testers-on-windows-10-devices"></a>Mettre votre application à la disposition de testeurs sur des appareils Windows10

Nous proposons deux options qui vous permettent de limiter la distribution de vos applications à certaines personnes seulement sur des appareils Windows10.

### <a name="package-flights"></a>Versions d’évaluation de package

Si vous avez déjà publié votre application, vous pouvez créer des versions d’évaluation de package pour distribuer un ensemble différent de packages aux personnes que vous spécifiez. Vous pouvez même créer plusieurs versions d’évaluation de package pour une même application et utiliser ces versions avec différents groupes d’utilisateurs. Cela constitue un excellent moyen d’expérimenter différents packages simultanément, et vous pouvez retirer des packages d’une version d’évaluation pour les intégrer à votre soumission sans version d’évaluation si vous décidez qu’ils peuvent être rendus publics.

Pour en savoir plus, consultez [Versions d’évaluation de package](package-flights.md).

> [!NOTE]
> Pour distribuer des packages spécifiques à un pourcentage défini de clients Windows10 sélectionnés aléatoirement, plutôt qu’à un groupe désigné de clients spécifiques, vous pouvez utiliser le [lancement progressif de packages](gradual-package-rollout.md). Vous pouvez également combiner le déploiement avec vos versions d’évaluation de package si vous souhaitez distribuer progressivement une mise à jour à l’un de vos groupes de versions d’évaluation.

<span id="hide" />
### <a name="hiding-the-app-in-the-store-and-using-promotional-codes"></a>Masquage de l’application dans WindowsStore et utilisation des codes promotionnels

Si vous souhaitez limiter la distribution d’une application à un certain groupe de testeurs, **sans** d’abord publier une soumission largement disponible, vous pouvez utiliser le même [processus de soumission d’application](app-submissions.md) que pour n’importe quelle application que vous soumettez. Pour autoriser uniquement certaines personnes à télécharger gratuitement l’application (et empêcher d’autres clients de la voir ou la télécharger), procédez comme suit:

-   Dans votre soumission, dans la section [Visibilité](set-app-pricing-and-availability.md#visibility) de la page **Tarification et disponibilité**, sélectionnez **Rendre ce produit disponible mais non détectable dans le Windows Store**.  Choisissez l’option pour **Empêcher l'acquisition: tout client disposant d'un lien direct peut voir la description du produit dans le Windows Store, mais ne peut le télécharger que s'il possède déjà le produit ou dispose d'un code promotionnel et utilise un appareil Windows10**. Cela empêche quiconque de trouver votre application dans le WindowsStore via une recherche ou une navigation.
-   Une fois l’application certifiée, vous devez [générer des codes promotionnels](generate-promotional-codes.md) pour celle-ci et les distribuer à vos testeurs. Vous pouvez générer des codes pour permettre jusqu’à 1600échanges par application sur une période de six mois. Ces codes fournissent aux testeurs un lien direct vers la description de l’application et leur permettent de télécharger celle-ci gratuitement, même si vous avez défini un prix lors de la création de votre soumission.

Une fois en possession des liens de codes promotionnels, vos testeurs peuvent essayer votre application et formuler des commentaires pour vous aider à l’améliorer. Ensuite, lorsque vous êtes prêt à rendre l’application accessible au public, créez une soumission et redéfinissez l’option **Visibilité** sur **Rendre cette application accessible et détectable dans le WindowsStore** (en plus d’autres modifications que vous souhaitez apporter).

Voici quelques éléments à prendre en considération lors de cette opération:

-   Vous pouvez donner à vos testeurs une version mise à jour de votre application à tout moment en créant une soumission. Veillez à conserver l'option **Visibilité** définie sur **Rendre ce produit disponible mais non détectable dans le Windows Store** avec l'option **Empêcher l'acquisition**. Une fois la mise à jour certifiée, les testeurs la reçoivent. Ils sont les seuls à en disposer.
-   Vos testeurs doivent avoir un appareil Windows 10 sur lequel installer l’application. (Toutefois, pour utiliser cette méthode de test, votre application ne doit pas inclure de packages Windows10.)
-   Vous pouvez créer des [codes promotionnels](generate-promotional-codes.md) supplémentaires à distribuer à tout moment (jusqu’à 1600échanges par application par période de six mois).
-   Après que vos testeurs ont téléchargé l’application, vous ne pouvez plus révoquer l’accès à cette dernière. Après avoir téléchargé l’application, les testeurs peuvent l’utiliser et recevoir les mises à jour éventuelles.
-   Vous devez déterminer le mode de collecte des commentaires de vos testeurs. Envisagez de fournir dans l’application bêta un lien qui permet à vos testeurs de communiquer facilement leurs commentaires par e-mail ou par le biais de l’application [Hub de commentaires](../monetize/launch-feedback-hub-from-your-app.md).
-   Vous pouvez consulter des [rapports d’analyse](analytics.md) concernant votre application, notamment des rapports d’utilisation et d’intégrité, ainsi que des évaluations ou avis laissés par vos testeurs.
-   Lorsque vous distribuez l’application aux testeurs, vous pouvez y inclure des extensions. Étant donné que vous ne voulez probablement pas facturer ces modules, veillez à ce qu’ils restent **gratuits** pendant la période de test. Ensuite, lorsque vous mettez l’application à la disposition d’autres utilisateurs, vous pouvez créer une nouvelle soumission pour chaque module complémentaire en modifiant son prix.


## <a name="other-methods-for-distributing-apps-to-testers"></a>Autres méthodes de distribution d’applications aux testeurs

Vous pouvez également limiter la distribution de votre application uniquement à un groupe ciblé au moyen d’options supplémentaires dans la section [Visibilité](set-app-pricing-and-availability.md#visibility) de la page **Tarification et disponibilité** lors de la soumission d’une application. N’oubliez pas que ces options ne fonctionnent pas pour les clients sous certaines versions du système d’exploitation. Plus précisément, aucune d'elles ne fonctionne pour les clients sous Windows8 ou Windows8.1.

### <a name="targeted-distribution-to-customers-with-a-link-to-your-apps-listing"></a>Distribution ciblée à des clients via un lien d’accès à la description de l’application

Avec cette option, seules les utilisateurs disposant d’un lien direct vers la description de l’application peuvent télécharger celle-ci. Vous pouvez trouver cette **URL** sur la page [Identité des applications](view-app-identity-details.md) dans le tableau de bord. Nul ne peut trouver l’application en recherchant ou en naviguant dans le Windows Store, mais quiconque disposant du lien peut la télécharger sur un appareil exécutant WindowsPhone8.1 ou versions antérieures ou Windows10. 

> [!NOTE]
> Pour que vos testeurs puissent télécharger l'application librement, vous devez définir son prix sur **Gratuit**.

Pour utiliser cette option, sélectionnez **Rendre ce produit disponible mais non détectable dans le Windows Store** dans la section [Visibilité](set-app-pricing-and-availability.md#visibility) de la page **Tarification et disponibilité**. Choisissez ensuite l’option **Lien direct uniquement: tout client accédant directement à la description du produit peut le télécharger, sauf sous Windows8.x.**.  


### <a name="targeted-distribution-to-customers-with-specified-email-addresses"></a>Distribution ciblée à des clients disposant d’adresses de messagerie spécifiées

Cette application fournit un moyen de limiter la distribution de votre application à des fins de test sur **Windows Phone8.1 et les versions antérieures seulement**. Seuls les utilisateurs dont vous spécifiez l’adresse de messagerie (associée à leur compteMicrosoft) dans le champ peuvent télécharger l’application via le lien d’accès direct à sa description.

> [!IMPORTANT]
> Les utilisateurs dont vous spécifiez l’adresse de messagerie peuvent uniquement télécharger l’application sur des appareils exécutant Windows Phone8.1 ou une version antérieure.
 
Le lien vers l’application figure dans la page [Identité de l’application](view-app-identity-details.md) du tableau de bord. Aucun client ne peut trouver l’application en recherchant ou en naviguant dans le Windows Store. Même en disposant du lien d’accès à celle-ci, il est impossible de la télécharger autrement qu’en utilisant un compte Microsoft associé à une adresse de messagerie que vous avez spécifiée lors de la soumission.

> [!NOTE]
Si vous utilisez cette option, vous pouvez toujours mettre l’application à la disposition de testeurs sur des appareils Windows10 en [générant des codes promotionnels](generate-promotional-codes.md) comme décrit ci-dessus. Tout utilisateur disposant des codes promotionnels de votre application peut télécharger celle-ci sur un appareil Windows 10, même si vous n’avez pas entré son adresse de messagerie ici.

Pour utiliser cette option, sélectionnez **Rendre ce produit disponible mais non détectable dans le Windows Store** dans la section [Visibilité](set-app-pricing-and-availability.md#visibility) de la page **Tarification et disponibilité**. Puis choisissez l’option **Utilisateurs sur Windows Phone8.x uniquement: seuls les utilisateurs spécifiés ci-dessous peuvent télécharger ce produit sur un appareil Windows Phone8.x. Les utilisateurs disposant d’un lien direct et d'un code promotionnel peuvent télécharger le produit sur un appareil Windows10.** 

Si vous choisissez cette option, gardez à l’esprit les points suivants:

-   Vous pouvez activer cette option uniquement si vous n’avez jamais publié l’application avec l’option [Visibilité](set-app-pricing-and-availability.md#visibility) définie sur **Rendre l’application disponible dans le Windows Store**.
-   Le prix de votre application doit être défini sur **Gratuit** afin que les testeurs puissent la télécharger librement.
-   Vos testeurs peuvent télécharger l’application uniquement sur Windows Phone 8.1 et versions antérieures. Pour pouvoir utiliser l’application, ils doivent disposer d’un appareil Windows Phone, mais celui-ci ne doit pas nécessairement être déverrouillé ou inscrit.
-   Pour pouvoir accéder au Windows Store et télécharger votre application, ils doivent également disposer d’un compte Microsoft. Pour pouvoir ajouter un testeur ci à votre liste, vous devrez connaître l’adresse de messagerie associée à son compte Microsoft. Pour créer un compte Microsoft, les testeurs peuvent accéder à [Configuration de compte Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=618945).
-   Vous pouvez spécifier jusqu’à 10 000 adresses de messagerie dans la zone de texte.
-   Les adresses doivent être séparées par des points-virgules.
-   Si vous voulez ajouter des adresses ultérieurement, vous devez créer une autre soumission.
-   Après que vos testeurs ont téléchargé l’application, vous ne pouvez plus révoquer l’accès à celle-ci. Après avoir téléchargé l’application, les testeurs peuvent l’utiliser et recevoir les mises à jour éventuelles que vous soumettez.
