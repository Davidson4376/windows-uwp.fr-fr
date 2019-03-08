---
Description: Offrent des produits dans l’application consommables &\#8212 ; les éléments qui peuvent être achetées, utilisé et achetées à nouveau &\#8212 ; via le Store plateforme de commerce pour fournir l’expérience de vos clients avec un achat, qui est à la fois robuste et fiable.
title: Activer les achats de produits consommables in-app
ms.assetid: F79EE369-ACFC-4156-AF6A-72D1C7D3BDA4
keywords: uwp, consommables, extensions, achats dans l'application, PIA, Windows.ApplicationModel.Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5588558eff3e9c9b2954f0726995765a2862c43b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655644"
---
# <a name="enable-consumable-in-app-product-purchases"></a>Activer les achats de produits consommables in-app

Proposez des produits consommables dans l’application qui peuvent être achetés, utilisés et rachetés via la plateforme commerciale du Windows Store, afin d’offrir à vos clients une expérience d’achat à la fois solide et fiable au sein de l’application. Cette fonction est particulièrement utile pour différents aspects du jeu, comme les devises (or, pièces, etc.) susceptibles d’être achetées, puis utilisées pour acheter certaines améliorations.

> [!IMPORTANT]
> Cet article explique comment utiliser des membres de l’espace de noms [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) pour autoriser les achats de produits consommables dans l’application. Cet espace de noms n’est plus mis à jour avec de nouvelles fonctionnalités et nous vous recommandons d’utiliser l'espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) à la place. Le **Windows.Services.Store** espace de noms prend en charge les types de module complémentaire dernière, telles que géré par le Store de modules complémentaires consommables et des abonnements et est conçu pour être compatible avec des types futurs des produits et fonctionnalités pris en charge par le partenaire Le centre et le Store. L'espace de noms **Windows.Services.Store** a été introduit dans Windows 10, version 1607 et peut être utilisé uniquement dans les projets qui ciblent **Windows 10 Anniversary Edition (version 10.0 ; build 14393)** ou une version ultérieure dans Visual Studio. Pour plus d’informations sur l'autorisation d'acheter des produits consommables dans l’application à l'aide de l'espace de noms **Windows.Services.Store**, consultez [cet article](enable-consumable-add-on-purchases.md).

## <a name="prerequisites"></a>Conditions préalables

-   Cette rubrique porte sur les rapports relatifs aux achats et acquisitions de produits in-app consommables. Si vous ne connaissez pas les produits in-app, consultez [Activer les achats de produits in-app](enable-in-app-product-purchases.md) pour en savoir plus sur les licences et obtenir la liste des produits in-app dans le Windows Store.
-   Lorsque vous codez et testez de nouveaux produits in-app pour la première fois, vous devez utiliser l’objet [CurrentAppSimulator](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) au lieu de l’objet [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp). Cela vous permet de vérifier votre logique de licence à l’aide d’appels simulés au serveur de licences au lieu d’appels au serveur Windows Live. Pour ce faire, vous devez personnaliser le fichier nommé WindowsStoreProxy.xml dans % UserProfile%\\AppData\\local\\packages\\&lt;nom_package&gt;\\LocalState\\ Microsoft\\Windows Store\\ApiData. Le simulateur Microsoft Visual Studio crée ce fichier quand vous exécutez votre application pour la première fois. Vous pouvez également charger un fichier personnalisé au moment de l’exécution. Pour plus d’informations, consultez [Utilisation du fichier WindowsStoreProxy.xml avec CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).
-   Cette rubrique fait également référence à des exemples de code fournis dans [Exemple Windows Store](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store). Cet exemple représente un excellent moyen d’obtenir une expérience pratique avec les différentes options de monétisation fournies pour les applications UWP.

## <a name="step-1-making-the-purchase-request"></a>Étape 1 : La demande d’achat

La demande d’achat initiale est effectuée avec le paramètre [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync), comme tout autre achat effectué dans le Windows Store. En ce qui concerne les produits in-app, la différence réside dans le fait qu’après avoir effectué un achat, un client ne peut pas acheter le même produit tant que l’application n’a pas averti le Windows Store que l’achat précédent a été correctement effectué. Il revient à votre application d’acquérir les consommables achetés et d’avertir le Windows Store de l’opération.

L’exemple suivant représente une demande d’achat de produits consommables dans l’application. Vous noterez la présence de commentaires de code indiquant le moment où votre application doit effectuer l’acquisition locale du produit in-app consommable, selon deux scénarios différents : lorsque la demande aboutit ou lorsqu’elle échoue suite à un achat non finalisé du même produit.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#MakePurchaseRequest)]

## <a name="step-2-tracking-local-fulfillment-of-the-consumable"></a>Étape 2 : Suivi d’exécution locale de la consommables

Quand vous accordez à votre client un accès au produit in-app consommable, il est important de garder une trace du produit acquis (*productId*) et de chaque transaction associée à cette acquisition (*transactionId*).

> [!IMPORTANT]
> Votre application doit signaler correctement la finalisation de l’acquisition au Windows Store. Cette étape est essentielle pour assurer une expérience d’achat fiable et juste à vos clients.

L’exemple suivant illustre l’utilisation des propriétés [PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392) de l’appel [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) à l’étape précédente, pour identifier le produit acheté à acquérir. Un tableau est utilisé pour stocker les informations du produit à un emplacement référençable ultérieurement et permettant de confirmer que l’acquisition locale a abouti.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#GrantFeatureLocally)]

L’exemple qui suit montre comment utiliser le tableau de l’exemple précédent pour accéder à l’ID du produit et à l’ID de transaction, deux informations qui sont utilisées plus tard pour signaler la finalisation de l’opération au Windows Store.

> [!IMPORTANT]
> Quelle que soit la méthodologie utilisée pour suivre et confirmer l’acquisition, votre application doit faire le maximum pour garantir que les clients ne paient pas des articles qu’ils n’ont pas reçus.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#IsLocallyFulfilled)]

## <a name="step-3-reporting-product-fulfillment-to-the-store"></a>Étape 3 : Fourniture de produits de création de rapports pour le Store

Une fois l’acquisition locale effectuée, votre application doit passer un appel [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync) incluant l’élément *productId* et la transaction comprenant l’achat du produit.

> [!IMPORTANT]
> Tant que vous ne signalez pas au Windows Store l’acquisition des produits in-app consommables, l’utilisateur ne peut pas racheter ce produit.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#ReportFulfillment)]

## <a name="step-4-identifying-unfulfilled-purchases"></a>Étape 4 : Identification des achats non satisfaites

Votre application peut utiliser la méthode [GetUnfulfilledConsumablesAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getunfulfilledconsumablesasync) pour rechercher les produits in-app consommables non acquis dans l’application et ce, à tout moment. Cette méthode doit être appelée régulièrement pour rechercher les consommables non acquis suite à des événements imprévus de l’application (interruption de la connectivité réseau, arrêt de l’application, etc.).

L’exemple suivant montre comment la méthode [GetUnfulfilledConsumablesAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getunfulfilledconsumablesasync) permet d’énumérer les consommables non acquis et comment votre application peut parcourir cette liste pour effectuer l’acquisition locale.

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#GetUnfulfilledConsumables)]

## <a name="related-topics"></a>Rubriques connexes

* [Activer les achats de produits in-app](enable-in-app-product-purchases.md)
* [Exemple de Store (illustre les versions d’évaluation et les achats dans l’application)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/br225197)
 

 
