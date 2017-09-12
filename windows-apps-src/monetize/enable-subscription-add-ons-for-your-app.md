---
author: mcleanbyron
description: "Découvrez comment utiliser l’espace de noms Windows.Services.Store pour implémenter des extensions d'abonnement."
title: Activer les extensions d'abonnement de votre application
keywords: "windows10, uwp, abonnements, extensions, achats dans l’application, Windows.Services.Store"
ms.author: mcleans
ms.date: 08/01/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 16e2e8d160ad2236220dc6f19f578bbaa82c9dc0
ms.sourcegitcommit: de6bc8acec2cd5ebc36bb21b2ce1a9980c3e78b2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2017
---
# <a name="enable-subscription-add-ons-for-your-app"></a>Activer les extensions d'abonnement de votre application

> [!IMPORTANT]
> Actuellement, la possibilité de créer des extensions d'abonnement n'est disponible que pour un ensemble de comptes de développeur qui participent à un programme d’adoption anticipée. À l’avenir, nous rendrons les extensions d’abonnement disponibles pour tous les comptes de développeur et nous mettons cette documentation préliminaire à disposition dès maintenant pour donner aux développeurs un aperçu de cette fonctionnalité.

Si votre application UWP cible Windows10, version1607 ou ultérieure, vous pouvez proposer à vos clients des achats dans l'application d'extensions d'*abonnement*. Vous pouvez utiliser les abonnements pour vendre des produits numériques dans votre application (par exemple, des fonctionnalités de l’application ou du contenu numérique) avec des périodes de facturation périodiques automatisées.

## <a name="feature-highlights"></a>Fonctionnalités essentielles

Les extensions d’abonnement des applications UWP prennent en charge les fonctionnalités suivantes:

* Vous avez le choix entre les périodes d’abonnement suivantes: 1mois, 3mois, 6mois, 1an ou 2ans. Certaines applications peuvent également utiliser une période d’abonnement de 6heures uniquement à des fins de test.
* Vous pouvez ajouter des périodes d’essai gratuits d'1 semaine ou 1mois à votre abonnement.
* Le SDK Windows [fournit des API](#code-examples) que vous pouvez utiliser dans votre application pour obtenir des informations sur les extensions d’abonnement disponibles pour l’application et activer l’achat d’une extension d’abonnement. Nous fournissons également des API REST que vous pouvez appeler à partir de vos services pour [gérer les abonnements d’un utilisateur](#manage-subscriptions).
* Vous pouvez afficher des rapports analytiques qui fournissent le nombre d’acquisitions d’abonnement, les abonnés actifs et les abonnements annulés dans une période donnée.
* Les clients peuvent gérer leur abonnement sur la page [http://account.microsoft.com/services](http://account.microsoft.com/services) de leur compte Microsoft. Les clients peuvent utiliser cette page pour afficher tous les abonnements qu’ils ont acquis, annuler un abonnement et modifier le formulaire de paiement associé à leur abonnement.

> [!NOTE]
> Pour activer les achats d'extensions d’abonnement dans votre application, votre application doit cibler Windows10, version1607 ou ultérieure, et doit utiliser les API de l' espace de noms **Windows.Services.Store** pour implémenter l’expérience d’achat dans l'application au lieu de l'espace de noms **Windows.ApplicationModel.Store**. Pour plus d’informations sur les différences entre ces espaces de noms, voir [Achats dans l’application et versions d’évaluation](in-app-purchases-and-trials.md).

## <a name="steps-to-enable-a-subscription-add-on-for-your-app"></a>Étapes pour activer les extensions d'abonnement de votre application

Pour activer les achats d'extensions d’abonnement dans votre application, procédez comme suit.

1. [Créez une soumission d’extension](../publish/add-on-submissions.md) pour votre abonnement dans le tableau de bord du Centre de développement et publiez la soumission. Lorsque vous suivez le processus de soumission d'extension, faites attention aux propriétés suivantes:

  * [Type de produit](../publish/set-your-add-on-product-id.md#product-type): veillez à sélectionner **Abonnement**.

  * [Période d’abonnement](../publish/enter-add-on-properties.md#subscription-period): choisissez la période de facturation périodique de votre abonnement. Vous ne pouvez pas modifier la période d’abonnement après la publication de votre extension.

    Chaque extension d'abonnement prend en charge une période d’abonnement et une période d’évaluation uniques. Vous devez créer une extension d'abonnement différente pour chaque type d’abonnement que vous souhaitez proposer dans votre application. Par exemple, si vous souhaitez proposer un abonnement mensuel sans essai, un abonnement mensuel avec un mois d'essai, un abonnement annuel sans essai et un abonnement annuel avec un mois d'essai, vous devrez créer quatre extensions d’abonnement.
        > [!NOTE]
        > If you are in the process of implementing the in-app purchase experience for your subscription, we recommend that you create a test add-on with the **For testing only – 6 hours** subscription period to help you test the experience. You can choose this test period only if you select one of the **Hidden in the Store** [visibility options](../publish/set-add-on-pricing-and-availability.md#visibility) for your test add-on.

  * [Période d’essai](../publish/enter-add-on-properties.md#free-trial): optez pour une période d’essai d'1 semaine ou d'1mois pour votre abonnement pour que les utilisateurs puissent en essayer le contenu avant de l’acheter. Vous ne pouvez pas modifier ou supprimer la période d’essai après la publication de votre extension d'abonnement.

    Pour obtenir un essai gratuit de votre abonnement, l'utilisateur doit acheter votre abonnement via le processus d’achat dans l’application standard, y compris avec un moyen de paiement valide. Rien ne sera débité pendant la période d’essai. À la fin de la période d’essai, l’abonnement se convertit automatiquement en abonnement complet et le mode de paiement de l’utilisateur est débité pour la première période de l’abonnement payant. Si l’utilisateur choisit d’annuler son abonnement pendant la période d’essai, l’abonnement reste actif jusqu'à la fin de la période d’essai.
        > [!NOTE]
        > Some trial durations are not available for all subscription periods.

  * [Visibilité](../publish/set-add-on-pricing-and-availability.md#visibility): si vous créez une extension de test qui vous sert uniquement à tester l’expérience d’achat dans l'application pour votre abonnement, nous vous recommandons de sélectionner l'une des options **Masquée dans le Windows Store**. Sinon, vous pouvez sélectionner la meilleure option de visibilité pour votre scénario.

  * [Prix](../publish/set-add-on-pricing-and-availability.md?#pricing): choisissez le prix de votre abonnement dans cette section. Vous ne pouvez pas augmenter le prix de l’abonnement après la publication de l'extension (vous pouvez cependant le réduire par la suite).
      > [!IMPORTANT]
      > Par défaut, lorsque vous créez une extension, le prix a initialement la valeur **Gratuit**. Étant donné que vous ne pouvez pas augmenter le prix d’une extension d'abonnement une fois la soumission d’extension terminée, veillez à bien choisir le prix de votre abonnement ici.

2. Dans votre application, utilisez les API de l'espace de noms [**Windows.Services.Store**](https://docs.microsoft.com/uwp/api/windows.services.store) pour vérifier si l’utilisateur actuel a déjà acquis votre extension d'abonnement, puis la proposer à la vente à l’utilisateur sous forme d'achat dans l’application. Consultez les [exemples de code](#code-examples) de cet article pour plus d’informations.

3. Testez l’implémentation d’achat dans l’application de votre abonnement dans votre application. Vous devez télécharger votre application une seule fois à partir du WindowsStore sur votre appareil de développement pour utiliser sa licence à des fins de test. Pour plus d’informations, consultez nos [conseils de test](in-app-purchases-and-trials.md#testing) concernant les achats dans l'application.  
    > [!NOTE]
    > Si votre application a accès à la période d’abonnement **réservée aux tests de 6heures**, nous vous recommandons de créer une extension de test avec cette période pour faciliter les tests d’utilisation. Vous pouvez choisir cette période de test uniquement si vous sélectionnez l'une des **options de visibilité** [Masquée dans le Windows Store](../publish/set-add-on-pricing-and-availability.md#visibility) pour votre extension de test.

4. Créez et publiez une soumission d’applications qui inclut votre package d’application mis à jour, y compris votre code testé. Pour plus d’informations, voir l’article [Soumissions d'applications](../publish/app-submissions.md).

<span id="code-examples"/>
## <a name="code-examples"></a>Exemples de code

Les exemples de code de cette section montrent comment utiliser les API de l'espace de noms [**Windows.Services.Store**](https://docs.microsoft.com/uwp/api/windows.services.store) pour obtenir des informations sur les extensions d’abonnement de l’application en cours et solliciter l’achat d’une extension d'abonnement pour le compte de l’utilisateur actuel.

Les conditions préalables de ces exemples sont les suivantes:
* Un projet VisualStudio d’application de plateforme Windows universelle (UWP) qui cible Windows10 version1607 ou ultérieure.
* Vous avez [créé une soumission d'application](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) dans le tableau de bord du Centre de développement Windows, et cette application est publiée dans le WindowsStore. Vous pouvez éventuellement configurer l’application pour qu'elle ne soit pas détectable dans le Windows Store pendant que vous la testez. Pour plus d’informations, consultez les [conseils de test](in-app-purchases-and-trials.md#testing).
* Vous avez [créé une extension d'abonnement pour l’application](../publish/add-on-submissions.md) dans le tableau de bord du Centre de développement.

Le code de ces exemples respecte les présupposés suivants:
* Le code s’exécute dans le contexte d’une [**Page**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) qui contient un [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) nommé ```workingProgressRing``` et un [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) nommé ```textBlock```. Ces objets sont utilisés pour respectivement indiquer qu’une opération asynchrone est en cours et afficher les messages de sortie.
* Le fichier de code contient une instruction **using** pour l’espace de noms **Windows.Services.Store**.
* Cette application mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour plus d’informations, consultez [Versions d’évaluation et achats dans l'application](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Si vous disposez d’une application de bureau qui utilise [Pont du bureau](https://developer.microsoft.com/windows/bridges/desktop), vous devrez peut-être ajouter du code supplémentaire non affiché dans ces exemples pour configurer l’objet [**StoreContext**](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Pour plus d’informations, voir [Utilisation de la classe StoreContext dans une application de bureau qui utilise le Pont du bureau](in-app-purchases-and-trials.md#desktop).

### <a name="get-info-about-subscription-add-ons-for-the-current-app"></a>Obtenir des informations sur les extensions d'abonnement de l’application en cours

Cet exemple de code montre comment obtenir des informations sur les extensions d'abonnement qui sont disponibles dans votre application. Pour obtenir ces informations, utilisez tout d’abord la méthode [**GetAssociatedStoreProductsAsync**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext#Windows_Services_Store_StoreContext_GetAssociatedStoreProductsAsync_) pour obtenir la collection d'objets [**StoreProduct**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct) qui représentent chaque extension disponible pour l’application. Obtenez ensuite le [**StoreSku**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku) de chaque produit et utilisez les propriétés [**IsSubscription**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku#Windows_Services_Store_StoreSku_IsSubscription_) et [**SubscriptionInfo**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku#Windows_Services_Store_StoreSku_SubscriptionInfo_) pour accéder aux informations sur l’abonnement.

> [!div class="tabbedCodeSnippets"]
[!code-cs[Abonnements](./code/InAppPurchasesAndLicenses_RS1/cs/GetSubscriptionAddOnsPage.xaml.cs#GetSubscriptions)]

### <a name="purchase-a-subscription-add-on"></a>Acheter une extension d'abonnement

Cet exemple montre comment utiliser la méthode [**RequestPurchaseAsync**](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_RequestPurchaseAsync_) de la classe [**StoreProduct**](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) pour acheter une extension d'abonnement. Cet exemple suppose que vous connaissez déjà l'[ID Store](in-app-purchases-and-trials.md#store-ids) de l’extension d’abonnement que vous souhaitez acheter.

> [!div class="tabbedCodeSnippets"]
[!code-cs[Abonnements](./code/InAppPurchasesAndLicenses_RS1/cs/PurchaseSubscriptionAddOnPage.xaml.cs#PurchaseSubscription)]

<span id="manage-subscriptions" />
## <a name="manage-subscriptions-from-your-services"></a>Gérer les abonnements à partir de vos services

Une fois que votre application est mise à jour dans le Windows Store et que les clients peuvent acheter votre extension d’abonnement, certains scénarios peuvent nécessiter de gérer l’abonnement d'un client. Nous fournissons des API REST que vous pouvez appeler à partir de vos services pour effectuer les tâches de gestion d’abonnement suivantes:

* [Obtenir les abonnements qu'un utilisateur a le droit d'utiliser](get-subscriptions-for-a-user.md). Si votre abonnement fait partie d’un service multiplateformes, vous pouvez appeler cette API pour déterminer si l’utilisateur spécifié possède des droits pour votre abonnement et connaître l’état de son abonnement dans le contexte de votre application UWP. Vous pouvez ensuite utiliser ces informations pour mettre à jour l’état de l’abonnement sur d’autres plateformes prises en charge par votre service.

* [Modifier l’état de facturation de l’abonnement d’un utilisateur donné](change-the-billing-state-of-a-subscription-for-a-user.md). Utilisez cette API pour annuler, étendre ou désactiver le renouvellement automatique d'un abonnement.

<span id="cancellations" />
## <a name="cancellations"></a>Annulations

Les clients peuvent utiliser la page [http://account.microsoft.com/services](http://account.microsoft.com/services) de leur compte Microsoft pour afficher tous les abonnements qu’ils ont acquis, annuler un abonnement et modifier le mode de paiement associé à leur abonnement. Lorsqu’un client annule un abonnement à l’aide de cette page, il continue d’accéder à l’abonnement pour la durée du cycle de facturation en cours. Il n’obtient aucun remboursement pour une partie quelconque du cycle de facturation en cours. À la fin du cycle de facturation en cours, leur abonnement est désactivé.

Vous pouvez également annuler un abonnement pour le compte d’un utilisateur à l’aide de notre API REST qui permet de [modifier l’état de facturation d’un abonnement pour un utilisateur donné](change-the-billing-state-of-a-subscription-for-a-user.md).

## <a name="unsupported-scenarios"></a>Scénarios non pris en charge

Les scénarios suivants ne sont actuellement pas pris en charge pour les extensions d’abonnement.

* La vente d'abonnements aux clients directement via le Windows Store n’est pas prise en charge pour l’instant. Les abonnements sont disponibles uniquement pour les achats dans l'application de produits numériques.
* Les clients ne peuvent pas changer de période d'abonnement à l'aide de la page [http://account.microsoft.com/services](http://account.microsoft.com/services) de leur compte Microsoft. Pour changer de période d’abonnement, les clients doivent annuler leur abonnement actuel et acheter un abonnement avec une période d’abonnement différente à partir de votre application.
* Le changement de niveau n’est actuellement pas pris en charge pour les extensions d’abonnement (par exemple, passer un client d'un abonnement de base à un abonnement premium avec plus de fonctionnalités).
* Les codes de [vente](../publish/put-apps-and-add-ons-on-sale.md) et les [codes promotionnels](../publish/generate-promotional-codes.md) ne sont actuellement pas pris en charge pour les extensions d’abonnement.


## <a name="related-topics"></a>Rubriques associées

* [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md)
* [Obtenir les informations produit des applications et extensions](get-product-info-for-apps-and-add-ons.md)
