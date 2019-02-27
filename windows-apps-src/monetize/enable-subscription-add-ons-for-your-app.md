---
description: Découvrez comment utiliser l’espace de noms Windows.Services.Store pour implémenter des extensions d'abonnement.
title: Activer les extensions d'abonnement de votre application
keywords: windows10, uwp, abonnements, extensions, achats dans l’application, Windows.Services.Store
ms.date: 12/06/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cda22488f613c508b2c753c6b530b2b34b10909d
ms.sourcegitcommit: 175d0fc32db60017705ab58136552aee31407412
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2019
ms.locfileid: "9114475"
---
# <a name="enable-subscription-add-ons-for-your-app"></a>Activer les extensions d'abonnement de votre application

Votre app de plateforme Windows universelle (UWP) peut offrir des achats in-app d'extension d'*abonnement* à vos clients. Vous pouvez utiliser les abonnements pour vendre des produits numériques dans votre application (par exemple, des fonctionnalités de l’application ou du contenu numérique) avec des périodes de facturation périodiques automatisées.

> [!NOTE]
> Pour activer les achats d'extensions d’abonnement dans votre application, votre projet doit cibler **Windows10 Anniversary Edition (version10.0; build14393)** ou une version ultérieure dans Visual Studio (ce qui correspond à Windows10, version1607). Il doit en outre utiliser les API dans l'espace de noms **Windows.Services.Store** à la place de l'espace de noms **Windows.ApplicationModel.Store** pour offrir une expérience d’achat dans l’application. Pour plus d’informations sur les différences entre ces espaces de noms, voir [Achats dans l’application et versions d’évaluation](in-app-purchases-and-trials.md).

## <a name="feature-highlights"></a>Fonctionnalités essentielles

Les extensions d’abonnement des applications UWP prennent en charge les fonctionnalités suivantes:

* Vous avez le choix entre les périodes d’abonnement suivantes: 1mois, 3mois, 6mois, 1an ou 2ans.
* Vous pouvez ajouter des périodes d’essai gratuits d'une semaine ou d'unmois à votre abonnement.
* Le SDK Windows [fournit des API](#code-examples) que vous pouvez utiliser dans votre application pour obtenir des informations sur les extensions d’abonnement disponibles pour l’application et activer l’achat d’une extension d’abonnement. Nous fournissons également des API REST que vous pouvez appeler à partir de vos services pour [gérer les abonnements d’un utilisateur](#manage-subscriptions).
* Vous pouvez afficher des rapports d'analyse qui indiquent le nombre d’acquisitions d’abonnement, les abonnés actifs et les abonnements annulés au cours d'une période donnée.
* Les clients peuvent gérer leur abonnement sur la page [https://account.microsoft.com/services](https://account.microsoft.com/services) de leur compte Microsoft. Les clients peuvent utiliser cette page pour afficher tous les abonnements qu’ils ont acquis, annuler un abonnement et modifier le formulaire de paiement associé à leur abonnement.

## <a name="steps-to-enable-a-subscription-add-on-for-your-app"></a>Étapes pour activer les extensions d'abonnement de votre application

Pour activer les achats d'extensions d’abonnement dans votre application, procédez comme suit.

1. [Créer une soumission d’extension](../publish/add-on-submissions.md) pour votre abonnement dans l’espace partenaires et publier la soumission. Lorsque vous suivez le processus de soumission d'extension, faites attention aux propriétés suivantes:

    * [Type de produit](../publish/set-your-add-on-product-id.md#product-type): veillez à sélectionner **Abonnement**.

    * [Période d’abonnement](../publish/enter-add-on-properties.md#subscription-period): choisissez la période de facturation périodique de votre abonnement. Vous ne pouvez pas modifier la période d’abonnement après la publication de votre extension.

        Chaque extension d'abonnement prend en charge une période d’abonnement et une période d’évaluation uniques. Vous devez créer une extension d'abonnement différente pour chaque type d’abonnement que vous souhaitez proposer dans votre application. Par exemple, si vous souhaitez proposer un abonnement mensuel sans essai, un abonnement mensuel avec un mois d'essai, un abonnement annuel sans essai et un abonnement annuel avec un mois d'essai, vous devrez créer quatre extensions d’abonnement.

    * [Période d’essai](../publish/enter-add-on-properties.md#free-trial): optez pour une période d’essai d'1 semaine ou d'1mois pour votre abonnement pour que les utilisateurs puissent en essayer le contenu avant de l’acheter. Vous ne pouvez pas modifier ou supprimer la période d’essai après la publication de votre extension d'abonnement.

        Pour obtenir un essai gratuit de votre abonnement, l'utilisateur doit acheter votre abonnement via le processus d’achat dans l’application standard, y compris avec un moyen de paiement valide. Rien ne sera débité pendant la période d’essai. À la fin de la période d’essai, l’abonnement se convertit automatiquement en abonnement complet et le mode de paiement de l’utilisateur est débité pour la première période de l’abonnement payant. Si l’utilisateur choisit d’annuler son abonnement durant la période d’évaluation, celui-ci reste actif jusqu'à la fin de la période d’évaluation. Certaines périodes d’évaluation ne sont pas disponibles pour toutes les périodes d’abonnement.

        > [!NOTE]
        > Chaque client peut obtenir un essai gratuit d'extension d'abonnement une seule fois. Une fois que le client a obtenu un essai gratuit pour un abonnement, le Store empêchera ce client d'acquérir à nouveau le même essai gratuit d'abonnement.

    * [Visibilité](../publish/set-add-on-pricing-and-availability.md#visibility): si vous créez une extension de test que vous n'utiliserez que pour tester l'expérience d'achats in-app de votre abonnement, nous vous recommandons de sélectionner l'une des options **Masqué dans le Store**. Sinon, vous pouvez sélectionner la meilleure option de visibilité pour votre scénario.

    * [Prix](../publish/set-add-on-pricing-and-availability.md?#pricing): choisissez le prix de votre abonnement dans cette section. Vous ne pouvez pas augmenter le prix de l’abonnement après la publication de l'extension. Toutefois, vous pouvez réduire ce prix ultérieurement.
        > [!IMPORTANT]
        > Par défaut, lorsque vous créez une extension, le prix a initialement pour valeur **Gratuit**. Étant donné que vous ne pouvez pas augmenter le prix d’une extension d'abonnement une fois la soumission d’extension terminée, veillez à bien choisir le prix de votre abonnement ici.

2. Dans votre app, utilisez les API dans l'espace de noms [**Windows.Services.Store**](https://docs.microsoft.com/uwp/api/windows.services.store) afin de déterminer si l'utilisateur actuel a déjà fait l'acquisition de votre extension d'abonnement, puis la lui proposer à la vente en tant qu'achat in-app. Consultez les [exemples de code](#code-examples) de cet article pour plus d’informations.

3. Testez l’implémentation d’achat dans l’application de votre abonnement dans votre application. Vous devez télécharger votre application une seule fois à partir du WindowsStore sur votre appareil de développement pour utiliser sa licence à des fins de test. Pour plus d’informations, consultez nos [conseils de test](in-app-purchases-and-trials.md#testing) concernant les achats in-app.  

4. Créer et publier une soumission d’app qui inclut votre package d’app mis à jour, y compris votre code testé. Pour plus d’informations, voir l’article [Soumissions d'applications](../publish/app-submissions.md).

<span id="code-examples"/>

## <a name="code-examples"></a>Exemples de code

Les exemples de code de cette section montrent comment utiliser les API de l'espace de noms [**Windows.Services.Store**](https://docs.microsoft.com/uwp/api/windows.services.store) pour obtenir des informations sur les extensions d’abonnement de l’application en cours et solliciter l’achat d’une extension d'abonnement pour le compte de l’utilisateur actuel.

Les conditions préalables de ces exemples sont les suivantes:
* Un projet Visual Studio pour une application de plateforme Windows universelle (UWP) qui cible **Windows10 Anniversary Edition (version10.0; build14393)** ou une version ultérieure.
* Vous avez [créé une soumission d’application](https://docs.microsoft.com/windows/uwp/publish/app-submissions) dans l’espace partenaires et que cette application est publiée dans le Windows Store. Vous pouvez éventuellement configurer l’application pour qu'elle ne soit pas détectable dans le Windows Store pendant que vous la testez. Pour plus d’informations, consultez les [conseils de test](in-app-purchases-and-trials.md#testing).
* Vous avez [créé une extension d’abonnement de l’application](../publish/add-on-submissions.md) dans l’espace partenaires.

Le code de ces exemples respecte les présupposés suivants:
* Le fichier de code contient des instructions **using** pour les espaces de noms **Windows.Services.Store** et **System.Threading.Tasks**.
* Cette app mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour plus d’informations, consultez [Versions d’évaluation et achats dans l'application](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Si vous disposez d’une application de bureau qui utilise [Pont du bureau](https://developer.microsoft.com/windows/bridges/desktop), vous devrez peut-être ajouter du code supplémentaire non affiché dans ces exemples pour configurer l’objet [**StoreContext**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext). Pour plus d’informations, voir [Utilisation de la classe StoreContext dans une application de bureau qui utilise le Pont du bureau](in-app-purchases-and-trials.md#desktop).

### <a name="purchase-a-subscription-add-on"></a>Acheter une extension d'abonnement

Cet exemple montre comment demander l’achat d’une extension d’abonnement connue pour votre app pour le compte du client actuel. Cet exemple montre également comment gérer une situation dans laquelle l’abonnement comporte une période d’évaluation.

1. Le code détermine d’abord si le client possède déjà une licence active pour l’abonnement. Si le client possède déjà une licence active, votre code doit déverrouiller les fonctionnalités d’abonnement selon des besoins (il s'agit d'éléments propriétaires de votre app, par conséquent cela est identifié avec un commentaire dans l’exemple).
2. Ensuite, le code obtient l'objet [**StoreProduct**](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) qui représente l’abonnement que vous souhaitez acheter pour le compte du client. Le code suppose que vous connaissez déjà l'[ID Store](in-app-purchases-and-trials.md#store-ids) de l’extension d’abonnement que vous souhaitez acheter, et que vous avez assigné cette valeur à la variable *subscriptionStoreId*.
3. Le code détermine ensuite si une version d’évaluation est disponible pour l’abonnement. Si vous le souhaitez, votre app peut utiliser ces informations pour afficher plus d’informations sur l’abonnement d’évaluation ou complet disponible pour le client.
4. Enfin, le code appelle la méthode [**RequestPurchaseAsync**](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.RequestPurchaseAsync) pour demander l'achat de l'abonnement. Si une version d’évaluation est disponible pour l’abonnement, celle-ci sera proposée au client à l’achat. Dans le cas contraire, l’abonnement complet sera proposé à l’achat.

> [!div class="tabbedCodeSnippets"]
[!code-cs[Subscriptions](./code/InAppPurchasesAndLicenses_RS1/cs/PurchaseSubscriptionAddOnTrialPage.xaml.cs#PurchaseTrialSubscription)]

### <a name="get-info-about-subscription-add-ons-for-the-current-app"></a>Obtenir des informations sur les extensions d'abonnement de l'app actuelle

Cet exemple de code démontre comment obtenir des informations sur toutes les extensions d'abonnement disponibles dans votre app. Pour obtenir ces informations, utilisez tout d’abord la méthode [**GetAssociatedStoreProductsAsync**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext.GetAssociatedStoreProductsAsync) pour obtenir la collection d'objets [**StoreProduct**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct) qui représentent chaque extension disponible pour l’application. Obtenez ensuite le [**StoreSku**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku) pour chaque produit et utilisez les propriétés [**IsSubscription**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.IsSubscription) et [**SubscriptionInfo**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.SubscriptionInfo) pour accéder aux informations de l’abonnement.

> [!div class="tabbedCodeSnippets"]
[!code-cs[Subscriptions](./code/InAppPurchasesAndLicenses_RS1/cs/GetSubscriptionAddOnsPage.xaml.cs#GetSubscriptions)]

<span id="manage-subscriptions" />

## <a name="manage-subscriptions-from-your-services"></a>Gérer les abonnements depuis vos services

Une fois que votre application est mise à jour dans le Windows Store et que les clients peuvent acheter votre extension d’abonnement, certains scénarios peuvent nécessiter de gérer l’abonnement d'un client. Nous fournissons des API REST que vous pouvez appeler à partir de vos services pour effectuer les tâches de gestion d’abonnement suivantes:

* [Obtenir les abonnements qu'un utilisateur a le droit d'utiliser](get-subscriptions-for-a-user.md). Si votre abonnement fait partie d’un service multiplateformes, vous pouvez appeler cette API pour déterminer si l’utilisateur spécifié possède des droits pour votre abonnement et connaître l’état de son abonnement dans le contexte de votre application UWP. Vous pouvez ensuite utiliser ces informations pour mettre à jour l’état de l’abonnement sur d’autres plateformes prises en charge par votre service.

* [Modifier l’état de facturation de l’abonnement d’un utilisateur donné](change-the-billing-state-of-a-subscription-for-a-user.md). Utilisez cette API pour annuler, étendre ou désactiver le renouvellement automatique d'un abonnement.

## <a name="cancellations"></a>Annulations

Les clients peuvent utiliser la page [https://account.microsoft.com/services](https://account.microsoft.com/services) de leur compte Microsoft afin d'afficher tous les abonnements qu'ils ont acquis, annuler un abonnement et modifier le moyen de paiement associé à leur abonnement. Si un client annule un abonnement via cette page, il continuera d'avoir accès à ce dernier pendant toute la durée de la période de facturation en cours. Aucun remboursement ne sera émis pour une partie de la période de facturation actuelle. Son abonnement sera désactivé au terme de la période de facturation en cours.

Vous pouvez également annuler un abonnement pour le compte d’un utilisateur à l’aide de notre API REST qui permet de [modifier l’état de facturation d’un abonnement pour un utilisateur donné](change-the-billing-state-of-a-subscription-for-a-user.md).

## <a name="subscription-renewals-and-grace-periods"></a>Renouvellement d’abonnement et périodes de grâce

Au cours de chaque période de facturation, nous tenterons à un moment donné de facturer la carte bancaire du client pour la période de facturation suivante. En cas d’échec du paiement, l'abonnement du client passera à l'état *relance*. Cela signifie que son abonnement est toujours actif pour le reste de la période de facturation en cours, mais nous tenterons périodiquement de facturer sa carte bancaire afin de renouveler automatiquement l'abonnement. Cet état peut demeurer ainsi pendant deux semaines maximum, jusqu'à la fin de la période de facturation en cours et la date de renouvellement de la prochaine période de facturation.

Nous n’offrons pas de périodes de grâce pour la facturation de l’abonnement. Si nous ne parvenons pas à débiter la carte bancaire du client à la fin de la période de facturation en cours, l’abonnement sera annulé et le client n’aura plus accès à l’abonnement au terme de la période de facturation en cours.

## <a name="unsupported-scenarios"></a>Scénarios non pris en charge

Les scénarios suivants ne sont actuellement pas pris en charge pour les extensions d’abonnement.

* La vente d'abonnements aux clients directement via le Windows Store n’est pas prise en charge pour l’instant. Les abonnements sont disponibles pour les achats in-app de produits numériques uniquement.
* Les clients ne peuvent pas modifier les périodes d'abonnement via la page [https://account.microsoft.com/services](https://account.microsoft.com/services) de leur compte Microsoft. Pour basculer vers la période d’abonnement, les clients doivent annuler leur abonnement actuel et acheter un abonnement avec une période d’abonnement différente à partir de votre application.
* Le changement de niveau n’est actuellement pas pris en charge pour les extensions d’abonnement (par exemple, passer un client d'un abonnement de base à un abonnement premium avec plus de fonctionnalités).
* Les codes de [vente](../publish/put-apps-and-add-ons-on-sale.md) et les [codes promotionnels](../publish/generate-promotional-codes.md) ne sont actuellement pas pris en charge pour les extensions d’abonnement.


## <a name="related-topics"></a>Rubriques associées

* [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md)
* [Obtenir les informations produit des applications et extensions](get-product-info-for-apps-and-add-ons.md)
