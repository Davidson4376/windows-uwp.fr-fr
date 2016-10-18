---
author: mcleanbyron
ms.assetid: 32572890-26E3-4FBB-985B-47D61FF7F387
description: "Découvrez comment activer les achats in-app et les versions d’évaluation dans les applications UWP qui ciblent les versions antérieures à Windows10 version1607."
title: "Versions d’évaluation et achats in-app utilisant l’espace de noms Windows.ApplicationModel.Store"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 649d082cddcf301fe602a5ab99637ad7bea67d49

---

# Versions d’évaluation et achats in-app utilisant l’espace de noms Windows.ApplicationModel.Store

Le SDK Windows fournit des membres dans l’espace de noms [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) que vous pouvez utiliser pour ajouter des achats in-app et la fonctionnalité d’évaluation à votre application de plateforme Windows universelle (UWP) afin de monétiser votre application et d’enrichir ses fonctionnalités. Ces API permettent également d’accéder aux informations de licence de votre application.

>**Remarque**&nbsp;&nbsp;Si votre application cible Windows10, version1607 ou ultérieure, nous vous recommandons d’utiliser les membres de l’espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) plutôt que l’espace de noms **Windows.ApplicationModel.Store**. L’espace de noms **Windows.Services.Store** prend en charge les types d’extension les plus récents, comme les extensions consommables gérées par le Windows Store. Il est conçu pour être compatible avec les futurs types de produits et de fonctionnalités pris en charge par le Centre de développement Windows et le WindowsStore. L’espace de noms **Windows.Services.Store** affiche également de meilleures performances. Pour plus d’informations, voir [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md).

Les articles de cette section indiquent des instructions détaillées et des exemples de code liés à l’utilisation des membres de l’espace de noms **Windows.ApplicationModel.Store** pour plusieurs scénarios courants. Pour obtenir une vue d’ensemble des concepts liés aux achats in-app dans les applications UWP, voir [Achats in-app et versions d’évaluation](in-app-purchases-and-trials.md).

Pour obtenir un exemple complet montrant comment implémenter des versions d’évaluation et des achats in-app à l’aide de l’espace de noms **Windows.ApplicationModel.Store**, voir [Exemple Windows Store](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store).

## Dans cette section


| Rubrique                                                                                                       | Description                 |
|-------------------------------------------------------------------------------------------------------------|-----------------------------|
| [Activer les achats de produits in-app](enable-in-app-product-purchases.md)      |  Que votre application soit gratuite ou non, vous pouvez vendre du contenu, d’autres applications ou de nouvelles fonctionnalités applicatives (par exemple le déverrouillage d’un nouveau niveau de jeu) directement dans l’application. Nous allons vous montrer comment activer ces produits dans votre application.  |
| [Activer les achats de produits consommables in-app](enable-consumable-in-app-product-purchases.md)      | Proposez des produits consommables dans l’application qui peuvent être achetés, utilisés et rachetés via la plateforme commerciale du Windows Store, afin d’offrir à vos clients une expérience d’achat à la fois solide et fiable au sein de l’application. Cette fonction est particulièrement utile pour différents aspects du jeu, comme les devises (or, pièces, etc.) susceptibles d’être achetées, puis utilisées pour acheter des améliorations spécifiques. |
| [Exclure ou limiter des fonctionnalités de la version d’évaluation](exclude-or-limit-features-in-a-trial-version-of-your-app.md) | Si vous donnez aux clients la possibilité d’utiliser votre application gratuitement pendant une période d’évaluation, vous pouvez leur donner envie de mettre à niveau vers la version complète de votre application en excluant ou en limitant certaines fonctionnalités pendant la période d’évaluation. |
| [Gérer un grand catalogue de produits in-app](manage-a-large-catalog-of-in-app-products.md)      |   Si votre application propose un vaste catalogue de produits in-app, vous pouvez éventuellement suivre la procédure décrite dans cette rubrique pour faciliter la gestion de votre catalogue.    |
| [Utiliser des reçus pour vérifier les achats de produits](use-receipts-to-verify-product-purchases.md)      |   Chaque transaction du Windows Store qui entraîne un achat de produit peut éventuellement retourner un reçu de transaction qui fournit des informations sur le produit répertorié et le coût monétaire pour le client. L’accès à ces informations permet les scénarios dans lesquels votre application doit vérifier qu’un utilisateur a acheté votre application ou qu’il a effectué des achats de produits in-app dans le Windows Store. |



<!--HONumber=Aug16_HO5-->


