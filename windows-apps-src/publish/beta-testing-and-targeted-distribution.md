---
author: jnHs
Description: "Le tableau de bord du Centre de développement Windows vous permet de rendre votre application accessible uniquement à des utilisateurs spécifiés. Vous pouvez ainsi demander à des testeurs de l’essayer avant de la proposer au public."
title: "Tests bêta et distribution ciblée"
ms.assetid: 38E4ED22-D6C1-40D8-9B16-6B3E51BD962E
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: a544565bf7bb82f2be1ded3e60836d5d191c6e93

---

# Tests bêta et distribution ciblée


Quelle que soit le soin avec lequel vous testez votre application, rien de vaut un test dans des conditions réelles, avec d’autres utilisateurs. Le tableau de bord du Centre de développement Windows vous permet de rendre votre application accessible uniquement à des utilisateurs spécifiés. Vous pouvez ainsi demander à des testeurs de l’essayer avant de la proposer au public. Les testeurs peuvent épingler des problèmes qui vous auraient échappés, comme des fautes d’orthographe, un flux d’application peu clair ou des erreurs pouvant entraîner un blocage de l’application. Vous pouvez ensuite résoudre ces problèmes avant de mettre à la disposition du public un produit finalement irréprochable.

Nous proposons différentes méthodes pour limiter la distribution de vos applications à vos testeurs seulement sans avoir besoin de créer une version distincte de celles-ci avec un nom et une identité de package différents. (Bien entendu, vous pouvez créer une application distincte réservée aux tests si vous le préférez. Le cas échéant, veillez à lui donner un nom différent du nom prévu pour l’application finale qui sera rendue publique.)

La méthode de distribution de votre application aux testeurs dépend des systèmes d’exploitation auxquels celle-ci est destinée. Les options ci-dessous sont disponibles pour Windows 10 et Windows Phone 8.1 et antérieur.

## Mettre votre application à la disposition de testeurs sur des appareils Windows 10

Nous proposons deux options qui vous permettent de limiter la distribution de vos applications à certaines personnes seulement sur des appareils Windows 10.

### Versions d’évaluation de package

Si vous avez déjà publié une version de votre application, vous pouvez créer des versions d’évaluation de package pour distribuer un ensemble différent de packages aux personnes que vous spécifiez. Vous pouvez créer plusieurs versions d’évaluation de package pour une même application ; vous pouvez utiliser ces versions avec différents groupes d’utilisateurs. Cela constitue un excellent moyen d’expérimenter différents packages simultanément, et vous permet de retirer des packages d’une version d’évaluation pour les intégrer à votre soumission sans version d’évaluation si vous décidez qu’ils peuvent être rendus publics.

Pour en savoir plus, voir [Versions d’évaluation de package](package-flights.md).

### Masquage de l’application dans Windows Store et utilisation des codes promotionnels

Si vous souhaitez limiter la distribution d’une application à un certain groupe de testeurs, sans d’abord publier une soumission largement disponible, vous pouvez utiliser le même [processus de soumission d’application](app-submissions.md) que pour n’importe quelle application que vous soumettez. Pour autoriser uniquement certaines personnes à télécharger gratuitement l’application (et empêcher d’autres clients de la voir ou la télécharger), procédez comme suit :

-   Dans votre soumission, sur la page **Tarification et disponibilité**, sélectionnez **Masquer cette application et empêcher l’acquisition. Les clients disposant d’un code promotionnel peuvent la télécharger sur les appareils Windows 10** dans la section [Distribution et visibilité](set-app-pricing-and-availability.md#distribution-and-visibility). Cela empêche quiconque de trouver votre application dans le Windows Store via une recherche ou une navigation.
-   Une fois l’application certifiée, vous devez [générer des codes promotionnels](generate-promotional-codes.md) pour celle-ci et les distribuer à vos testeurs. Vous pouvez générer jusqu’à 250 codes promotionnels par application sur une période de six mois. Ces codes fournissent aux testeurs un lien direct vers la description de l’application et leur permettent de télécharger celle-ci gratuitement, même si vous avez défini un prix lors de la création de votre soumission.

Une fois en possession des liens de codes promotionnels, vos testeurs peuvent télécharger votre application gratuitement, l’essayer et formuler des commentaires pour vous aider à l’améliorer. Ensuite, lorsque vous êtes prêt à rendre l’application accessible au public, vous pouvez créer une nouvelle soumission et définir l’option **Distribution et la visibilité** sur **Tout le monde peut trouver votre application sur le Windows Store** (en plus d’autres modifications que vous souhaitez faire).

Voici quelques éléments à prendre en considération lors de cette opération :

-   Vous pouvez donner à vos testeurs une version mise à jour de votre application à tout moment en créant une soumission. Veillez à ce que l’option **Distribution et visibilité** reste définie sur **Masquer cette application et empêcher l’acquisition. Les clients disposant d’un code promotionnel peuvent la télécharger sur les appareils Windows 10**. Une fois la mise à jour certifiée, les testeurs la reçoivent. Ils sont les seuls à en disposer.
-   Vos testeurs doivent avoir un appareil Windows 10 sur lequel installer l’application. (Toutefois, pour utiliser cette méthode de test, votre application ne doit pas inclure de packages Windows 10.)
-   Vous pouvez créer des [codes promotionnels](generate-promotional-codes.md) supplémentaires à distribuer à tout moment (jusqu’à 250 par période de six mois).
-   Après que vos testeurs ont téléchargé l’application, vous ne pouvez plus révoquer l’accès à celle-ci. Après avoir téléchargé l’application, les testeurs peuvent l’utiliser et recevoir les mises à jour éventuelles.
-   Vous devez déterminer le mode de collecte de leurs commentaires. Songez à fournir une adresse électronique ou un lien de site web dans l’application bêta pour que les testeurs puissent aisément vous faire part de leurs commentaires.
-   Vous avez accès à des [rapports analytiques](analytics.md) sur votre application, notamment aux évaluations ou critiques laissées par vos testeurs.
-   lorsque vous distribuez l’application aux testeurs, vous pouvez y inclure des produits in-app. Étant donné que vous ne voulez probablement pas facturer ces produits, veillez à ce qu’ils restent gratuits pendant la période de test. Ensuite, lorsque vous mettez l’application à la disposition d’autres utilisateurs, vous pouvez créer une nouvelle soumission pour chaque produit in-app en modifiant son prix.

## Autres méthodes de distribution d’applications aux testeurs

Vous pouvez également limiter la distribution de votre application uniquement à un groupe ciblé au moyen d’options supplémentaires dans la section [Distribution et visibilité](set-app-pricing-and-availability.md#distribution-and-visibility) de la page **Tarification et disponibilité** lors de la soumission d’une application. N’oubliez pas que ces options ne fonctionnent pas pour tous les systèmes d’exploitation. Les options ci-dessus sont recommandées lorsque vous testez votre application sur des appareils Windows 10.

Si vous choisissez l’une des options ci-dessus, vous pouvez toujours soumettre une mise à jour et définir l’option [Distribution et visibilité](set-app-pricing-and-availability.md#distribution-and-visibility) sur **Rendre votre application accessible dans le Windows Store** lorsque vous êtes prêt à mettre fin à la période de test et à rendre l’application disponible pour tous. Vous n’êtes pas obligé de modifier le nom de l’application et de créer une application totalement distincte (sauf si vous le souhaitez).

### Distribution ciblée à des clients via un lien d’accès à la description de l’application

Avec cette option, seules les utilisateurs disposant d’un lien direct vers la description de l’application peuvent télécharger celle-ci. Ce lien figure dans la page [Identité de l’application](view-app-identity-details.md) du tableau de bord (utilisez l’**URL pour Windows Phone** ou l’**URL pour Windows 10**). Nul ne peut trouver l’application en recherchant ou en naviguant dans le Windows Store, mais quiconque disposant du lien peut la télécharger. (Notez que le prix de votre application doit être défini sur **Gratuit** afin que les testeurs puissent la télécharger librement.)

Pour utiliser cette option, sélectionnez **Masquer cette application dans le Windows Store. Les utilisateurs accédant directement à la description de l’application peuvent malgré tout la télécharger, sauf sur Windows 8 et Windows 8.1** dans la section [Distribution et visibilité](set-app-pricing-and-availability.md#distribution-and-visibility) de la page **Tarification et disponibilité** lors de la soumission de l’application.

> **Important** Cette option ne fonctionne pas pour les testeurs sur Windows 8 ou Windows 8.1.

### Distribution ciblée à des clients disposant d’adresses de messagerie spécifiées

Cette application fournit un moyen de limiter la distribution de votre application à des fins de test sur Windows Phone 8.1. Seuls les utilisateurs dont vous spécifiez l’adresse de messagerie (associée à leur compte Microsoft) dans le champ peuvent télécharger l’application via le lien d’accès direct à sa description.

> **Important** Les utilisateurs dont vous spécifiez l’adresse de messagerie peuvent uniquement télécharger l’application sur des appareils exécutant Windows Phone 8.1 ou une version antérieure.
 
Le lien vers l’application figure dans la page [Identité de l’application](view-app-identity-details.md) du tableau de bord (utilisez **l’URL pour Windows Phone**). Aucun client ne peut trouver l’application en recherchant ou en naviguant dans le Windows Store. Même en disposant du lien d’accès à celle-ci, il est impossible de la télécharger autrement qu’en utilisant un compte Microsoft associé à une adresse de messagerie que vous avez spécifiée lors de la soumission.

> **Important** Si vous utilisez cette option, vous pouvez toujours mettre l’application à la disposition de testeurs sur des appareils Windows 10 en [générant des codes promotionnels](generate-promotional-codes.md) comme décrit ci-dessus. Tout utilisateur disposant des codes promotionnels de votre application peut télécharger celle-ci sur un appareil Windows 10, même si vous n’avez pas entré son adresse de messagerie ici.

Pour utiliser cette option, sélectionnez **Masquer cette application et la rendre disponible uniquement aux utilisateurs spécifiés ci-dessous, qui peuvent la télécharger sur les appareils Windows Phone 8.x. Un code promotionnel peut être utilisé pour télécharger cette application sur les appareils Windows 10** dans la section [Distribution et visibilité](set-app-pricing-and-availability.md#distribution-and-visibility) de la page **Tarification et disponibilité** lorsque vous soumettez votre application.

Si vous choisissez cette option, gardez à l’esprit les points suivants :

-   Vous pouvez activer cette option uniquement si vous n’avez jamais publié l’application avec l’option [Distribution et visibilité](set-app-pricing-and-availability.md#distribution-and-visibility) définie sur **Tout le monde peut trouver votre application sur le Windows Store**.
-   Le prix de votre application doit être défini sur **Gratuit** afin que les testeurs puissent la télécharger librement.
-   Vos testeurs peuvent télécharger l’application uniquement sur Windows Phone 8.1 et versions antérieures. Pour pouvoir utiliser l’application, ils doivent disposer d’un appareil Windows Phone, mais celui-ci ne doit pas nécessairement être déverrouillé ou inscrit.
-   Pour pouvoir accéder au Windows Store et télécharger votre application, ils doivent également disposer d’un compte Microsoft. Pour pouvoir ajouter un testeur ci à votre liste, vous devrez connaître l’adresse de messagerie associée à son compte Microsoft. Pour créer un compte Microsoft, les testeurs peuvent accéder à [Configuration de compte Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=618945).
-   Vous pouvez spécifier jusqu’à 10 000 adresses de messagerie dans la zone de texte.
-   Les adresses doivent être séparées par des points-virgules.
-   Si vous voulez ajouter des adresses ultérieurement, vous devez créer une autre soumission.
-   Après que vos testeurs ont téléchargé l’application, vous ne pouvez plus révoquer l’accès à celle-ci. Après avoir téléchargé l’application, les testeurs peuvent l’utiliser et recevoir les mises à jour éventuelles que vous soumettez.



<!--HONumber=Jun16_HO4-->


