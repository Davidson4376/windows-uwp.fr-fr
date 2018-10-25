---
author: Xansky
ms.assetid: 5E722AFF-539D-456E-8C4A-ADE90CF7674A
description: Si votre application propose un vaste catalogue de produits intégrés à l’application, vous pouvez éventuellement suivre la procédure décrite dans cette rubrique pour faciliter la gestion de votre catalogue.
title: Gérer un grand catalogue de produits in-app
ms.author: mhopkins
ms.date: 08/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, achats dans l'application, FAI, extensions, catalogue, Windows.ApplicationModel.Store
ms.localizationpriority: medium
ms.openlocfilehash: fad186ed63557024fb71a6ec3c6997833afb7f4c
ms.sourcegitcommit: 2c4daa36fb9fd3e8daa83c2bd0825f3989d24be8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "5512206"
---
# <a name="manage-a-large-catalog-of-in-app-products"></a>Gérer un vaste catalogue de produits in-app

Si votre application propose un vaste catalogue de produits in-app, vous pouvez éventuellement suivre la procédure décrite dans cette rubrique pour faciliter la gestion de votre catalogue. Avant Windows10, le WindowsStore avait une limite de 200produits par compte de développeur et la procédure décrite dans cette rubrique permettait de contourner cette limite. À compter de Windows 10, le Windows Store n’a aucune limite au nombre de produits listés par compte de développeur, et la procédure décrite dans cet article n’est plus nécessaire.

> [!IMPORTANT]
> Cet article montre comment utiliser les membres de l’espace de noms [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx). Cet espace de noms n’est plus mis à jour avec de nouvelles fonctionnalités et nous vous recommandons d’utiliser l'espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) à la place. L’espace de noms **Windows.Services.Store** prend en charge les types d’extension les plus récents, comme les extensions et les abonnements consommables gérés par le Store. Il est conçu pour être compatible avec les futurs types de produits et de fonctionnalités pris en charge par le Centre de développement Windows et le Store. L'espace de noms **Windows.Services.Store** a été introduit dans Windows10, version1607 et peut être utilisé uniquement dans les projets qui ciblent **Windows10 Anniversary Edition (version10.0; build14393)** ou une version ultérieure dans Visual Studio. Pour plus d’informations, voir [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md).

Pour activer cette fonctionnalité, vous allez créer plusieurs entrées pour certaines fourchettes de prix, chacune d’elles pouvant représenter des centaines de produits dans un catalogue. Utilisez la surcharge de la méthode [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) qui spécifie une offre définie par une application et associée à un produit in-app répertorié dans le WindowsStore. En plus de spécifier une association entre une offre et un produit pendant l’appel, votre application doit transférer un objet [ProductPurchaseDisplayProperties](https://msdn.microsoft.com/library/windows/apps/dn263384) qui contient les détails de l’offre du grand catalogue. Si ces informations ne sont pas fournies, elles sont remplacées par celles du produit listé.

Le WindowsStore n’utilise que le paramètre *offerId* de la demande d’achat dans les résultats [PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392). Ce processus ne modifie pas directement les informations fournies à l’origine lors de l’[intégration du produit in-app dans le WindowsStore](../publish/add-on-submissions.md).

## <a name="prerequisites"></a>Conditions préalables

-   Cette rubrique couvre la prise en charge par le Windows Store de la représentation de plusieurs offres in-app à l’aide d’un simple produit in-app listé dans le Windows Store. Si vous ne connaissez pas les achats in-app, consultez [Activer les achats de produits in-app](enable-in-app-product-purchases.md) pour en savoir plus sur les informations de licence et pour répertorier correctement votre achat in-app dans le WindowsStore.
-   Lorsque vous codez et testez de nouvelles offres in-app pour la première fois, vous devez utiliser l’objet [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) au lieu de l’objet [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765). Cela vous permet de vérifier votre logique de licence à l’aide d’appels simulés au serveur de licences au lieu d’appels au serveur WindowsLive. Pour ce faire, vous devez personnaliser le fichier WindowsStoreProxy.xml dans %userprofile%\\AppData\\local\\packages\\&lt;nom_package&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData. Le simulateur Microsoft VisualStudio crée ce fichier quand vous exécutez votre application pour la première fois. Vous pouvez également charger un fichier personnalisé au moment de l’exécution. Pour plus d’informations, consultez [Utilisation du fichier WindowsStoreProxy.xml avec CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).
-   Cette rubrique fait également référence à des exemples de code fournis dans [Exemple WindowsStore](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store). Cet exemple représente un excellent moyen d’obtenir une expérience pratique avec les différentes options de monétisation fournies pour les applications UWP.

## <a name="make-the-purchase-request-for-the-in-app-product"></a>Effectuer une demande d’achat d’un produit in-app

La demande d’achat d’un produit spécifique dans un vaste catalogue est traitée de la même manière que n’importe quelle autre demande d’achat dans l’application. Lorsque votre application appelle la nouvelle surcharge de méthode [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync), votre application fournit à la fois un élément *OfferId* et un objet [ProductPurchaseDisplayProperties](https://msdn.microsoft.com/library/windows/apps/dn263390) contenant le nom du produit in-app.

> [!div class="tabbedCodeSnippets"]
[!code-cs[ManageCatalog](./code/InAppPurchasesAndLicenses/cs/ManageCatalog.cs#MakePurchaseRequest)]

## <a name="report-fulfillment-of-the-in-app-offer"></a>Signaler l’acquisition de l’offre in-app

Votre application devra signaler l’acquisition du produit au Store dès que l’offre aura été acquise localement. Dans le cas d’un grand catalogue, si votre application ne signale pas l’acquisition de l’offre, l’utilisateur ne pourra pas acheter d’offres dans l’application à l’aide de la même liste de produits du WindowsStore.

Comme mentionné précédemment, le WindowsStore n’utilise que les informations de l’offre pour renseigner l’élément [PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392), sans créer d’association durable entre une offre d’un vaste catalogue et la liste de produits du WindowsStore. Par conséquent, vous devez vérifier que les utilisateurs sont autorisés à accéder aux produits et fournir un contexte spécifique (comme le nom de l’article acheté ou des détails le concernant) à l’utilisateur hors de l’opération [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync).

Le code suivant illustre l’appel d’acquisition et un schéma de message d’interface utilisateur contenant les informations de l’offre. En l’absence de ces informations sur ce produit, l’exemple utilise les informations du produit [ListingInformation](https://msdn.microsoft.com/library/windows/apps/br225163).

> [!div class="tabbedCodeSnippets"]
[!code-cs[ManageCatalog](./code/InAppPurchasesAndLicenses/cs/ManageCatalog.cs#ReportFulfillment)]

## <a name="related-topics"></a>Rubriques connexes

* [Activer les achats de produits dans l’application](enable-in-app-product-purchases.md)
* [Activer l’achat de produits in-app consommables](enable-consumable-in-app-product-purchases.md)
* [Exemple du Windows Store (montre des versions d’évaluation et des achats in-app)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [RequestProductPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/dn263382)
* [ProductPurchaseDisplayProperties](https://msdn.microsoft.com/library/windows/apps/dn263384)
