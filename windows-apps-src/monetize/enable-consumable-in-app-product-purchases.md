---
author: mcleanbyron
Description: "Proposez des produits consommables dans l’application&amp;\\#8212; qui peuvent être achetés, utilisés et rachetés&amp;\\#8212;via la plateforme commerciale du Windows Store, afin d’offrir à vos clients une expérience d’achat à la fois solide et fiable au sein de l’application."
title: "Activer les achats d’extensions consommables dans l’application"
ms.assetid: F79EE369-ACFC-4156-AF6A-72D1C7D3BDA4
keywords: uwp, consommables, extensions, achats dans l&quot;application, PIA, Windows.ApplicationModel.Store
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: e2ecaf364c581e82406c76831dd3e33c82594601
ms.sourcegitcommit: d053f28b127e39bf2aee616aa52bb5612194dc53
translationtype: HT
---
# <a name="enable-consumable-in-app-product-purchases"></a>Activer l’achat de produits dans l'application consommables


> [!NOTE]
> Cet article montre comment utiliser les membres de l’espace de noms  [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx). Si votre application cible Windows10, version1607 ou ultérieure, nous vous recommandons d’utiliser des membres de l’espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) pour gérer les extensions (également appelées produits in-app ou PIA) plutôt que l’espace de noms **Windows.ApplicationModel.Store**. Pour plus d’informations, voir [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md).

Proposez des produits consommables dans l’application qui peuvent être achetés, utilisés et rachetés via la plateforme commerciale du Windows Store, afin d’offrir à vos clients une expérience d’achat à la fois solide et fiable au sein de l’application. Cette fonction est particulièrement utile pour différents aspects du jeu, comme les devises (or, pièces, etc.) susceptibles d’être achetées, puis utilisées pour acheter des améliorations spécifiques.

## <a name="prerequisites"></a>Prérequis

-   Cette rubrique porte sur les rapports relatifs aux achats et acquisitions de produits in-app consommables. Si vous ne connaissez pas les produits in-app, consultez [Activer les achats de produits in-app](enable-in-app-product-purchases.md) pour en savoir plus sur les licences et obtenir la liste des produits in-app dans le WindowsStore.
-   Lorsque vous codez et testez de nouveaux produits in-app pour la première fois, vous devez utiliser l’objet [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) au lieu de l’objet [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765). Cela vous permet de vérifier votre logique de licence à l’aide d’appels simulés au serveur de licences au lieu d’appels au serveur WindowsLive. Pour ce faire, vous devez personnaliser le fichier WindowsStoreProxy.xml dans %userprofile%\\AppData\\local\\packages\\&lt;nom_package&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData. Le simulateur Microsoft VisualStudio crée ce fichier quand vous exécutez votre application pour la première fois. Vous pouvez également charger un fichier personnalisé au moment de l’exécution. Pour plus d’informations, consultez [Utilisation du fichier WindowsStoreProxy.xml avec CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).
-   Cette rubrique fait également référence à des exemples de code fournis dans [Exemple WindowsStore](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store). Cet exemple représente un excellent moyen d’obtenir une expérience pratique avec les différentes options de monétisation fournies pour les applications UWP.

## <a name="step-1-making-the-purchase-request"></a>Étape1: Établissement de la demande d’achat

La demande d’achat initiale est effectuée avec le paramètre [RequestProductPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/dn263381), comme tout autre achat effectué dans le WindowsStore. En ce qui concerne les produits in-app, la différence réside dans le fait qu’après avoir effectué un achat, un client ne peut pas acheter le même produit tant que l’application n’a pas averti le WindowsStore que l’achat précédent a été correctement effectué. Il revient à votre application d’acquérir les consommables achetés et d’avertir le Windows Store de l’opération.

L’exemple suivant représente une demande d’achat de produits consommables dans l’application. Vous noterez la présence de commentaires de code indiquant le moment où votre application doit effectuer l’acquisition locale du produit in-app consommable, selon deux scénarios différents: lorsque la demande aboutit ou lorsqu’elle échoue suite à un achat non finalisé du même produit.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#MakePurchaseRequest)]

## <a name="step-2-tracking-local-fulfillment-of-the-consumable"></a>Étape2: Suivi de l’acquisition locale du consommable

Quand vous accordez à votre client un accès au produit in-app consommable, il est important de garder une trace du produit acquis (*productId*) et de chaque transaction associée à cette acquisition (*transactionId*).

> [!IMPORTANT]
> Votre application doit signaler correctement la finalisation de l’acquisition au WindowsStore. Cette étape est essentielle pour assurer une expérience d’achat fiable et juste à vos clients.

L’exemple suivant illustre l’utilisation des propriétés [PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392) de l’appel [RequestProductPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/dn263381) à l’étape précédente, pour identifier le produit acheté à acquérir. Un tableau est utilisé pour stocker les informations du produit à un emplacement référençable ultérieurement et permettant de confirmer que l’acquisition locale a abouti.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#GrantFeatureLocally)]

L’exemple qui suit montre comment utiliser le tableau de l’exemple précédent pour accéder à l’ID du produit et à l’ID de transaction, deuxinformations qui sont utilisées plus tard pour signaler la finalisation de l’opération au WindowsStore.

> [!IMPORTANT]
> Quelle que soit la méthodologie utilisée pour suivre et confirmer l’acquisition, votre application doit faire le maximum pour garantir que les clients ne paient pas des articles qu’ils n’ont pas reçus.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#IsLocallyFulfilled)]

## <a name="step-3-reporting-product-fulfillment-to-the-store"></a>Étape3: Signalement de l’acquisition de produits au WindowsStore

Une fois l’acquisition locale effectuée, votre application doit passer un appel [ReportConsumableFulfillmentAsync](https://msdn.microsoft.com/library/windows/apps/dn263380) incluant l’élément *productId* et la transaction comprenant l’achat du produit.

> [!IMPORTANT]
> Tant que vous ne signalez pas au WindowsStore l’acquisition des produits in-app consommables, l’utilisateur ne peut pas racheter ce produit.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#ReportFulfillment)]

## <a name="step-4-identifying-unfulfilled-purchases"></a>Étape4: Identification des achats non acquis

Votre application peut utiliser la méthode [GetUnfulfilledConsumablesAsync](https://msdn.microsoft.com/library/windows/apps/dn263379) pour rechercher les produits in-app consommables non acquis dans l’application et ce, à tout moment. Cette méthode doit être appelée régulièrement pour rechercher les consommables non acquis suite à des événements imprévus de l’application (interruption de la connectivité réseau, arrêt de l’application, etc.).

L’exemple suivant montre comment la méthode [GetUnfulfilledConsumablesAsync](https://msdn.microsoft.com/library/windows/apps/dn263379) permet d’énumérer les consommables non acquis et comment votre application peut parcourir cette liste pour effectuer l’acquisition locale.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#GetUnfulfilledConsumables)]

## <a name="related-topics"></a>Rubriques connexes

* [Activer les achats de produits dans l’application](enable-in-app-product-purchases.md)
* [Exemple du Windows Store (montre des versions d’évaluation et des achats in-app)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/br225197)
 

 
