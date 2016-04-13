---
ms.assetid: 5E722AFF-539D-456E-8C4A-ADE90CF7674A
description: Si votre application propose un vaste catalogue de produits in-app, vous pouvez éventuellement suivre la procédure décrite dans cette rubrique pour faciliter la gestion de votre catalogue.
title: Gérer un vaste catalogue de produits in-app
---

# Gérer un vaste catalogue de produits in-app


\[ Article mis à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Si votre application propose un vaste catalogue de produits in-app, vous pouvez éventuellement suivre la procédure décrite dans cette rubrique pour faciliter la gestion de votre catalogue. Cette solution vous permet de ne créer que quelques références de produits pour des fourchettes de prix spécifiques, chacune d’elles pouvant représenter des centaines de produits dans un catalogue.

Pour activer cette fonction, utilisez la surcharge de la méthode [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382) qui spécifie une offre définie par application associée à un produit in-app listé dans le Windows Store. En plus de spécifier une association d’offre et de produit au cours de l’appel, votre application doit également transférer un objet [**ProductPurchaseDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/dn263384) qui contient les détails de l’offre du grand catalogue. Si ces détails ne sont pas fournis, ceux du produit listé seront utilisés à la place.

Le Windows Store utilisera uniquement l’*offerId* provenant de la demande d’achat, dans les [**PurchaseResults**](https://msdn.microsoft.com/library/windows/apps/dn263392) résultant de la demande. Ce processus ne modifie pas directement les informations fournies à l’origine lors de l’établissement de la [liste des produits in-app dans le Windows Store](https://msdn.microsoft.com/library/windows/apps/mt148551).

**Remarque** À partir de Windows 10, le Windows Store ne présente aucune limite au nombre de produits listés par compte de développeur. Dans les versions précédentes, le Windows Store présentait une limite de 200 produits listés par compte de développeur et la procédure décrite dans cette rubrique pouvait être utilisée pour contourner cette limite.

## Prérequis

-   Cette rubrique couvre la prise en charge par le Windows Store de la représentation de plusieurs offres in-app à l’aide d’un simple produit in-app listé dans le Windows Store. Si vous ne connaissez pas les achats in-app, passez en revue la section [Activer les achats de produits in-app](enable-in-app-product-purchases.md) pour en savoir plus sur les informations relatives aux licences et savoir comment répertorier correctement dans le Windows Store votre achat in-app.
-   Lorsque vous codez et testez de nouvelles offres intégrées à l’application pour la première fois, vous devez utiliser l’objet [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) au lieu de l’objet [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765). Cela vous permet de vérifier votre logique de licence à l’aide d’appels simulés au serveur de licences au lieu d’appels au serveur Live. Pour cela, vous devez personnaliser le fichier nommé « WindowsStoreProxy.xml » dans %userprofile%\\AppData\\local\\packages\\&lt;package name&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData. Le simulateur Microsoft Visual Studio crée ce fichier quand vous exécutez votre application pour la première fois. Vous pouvez également charger un fichier personnalisé au moment de l’exécution. Pour plus d’informations, voir **CurrentAppSimulator**.
-   Cette rubrique fait également référence à des exemples de code fournis dans l’[exemple du Windows Store](http://go.microsoft.com/fwlink/p/?LinkID=627610). Cet exemple représente un excellent moyen d’obtenir une expérience pratique avec les différentes options de monétisation fournies pour les applications UWP.

## Effectuer une demande d’achat d’un produit in-app

La demande d’achat d’un produit spécifique dans un vaste catalogue est traitée de la même manière que n’importe quelle autre demande d’achat dans l’application. Lorsque votre application appelle la nouvelle surcharge de méthode [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382), votre application fournit à la fois un élément *OfferId* et un objet [**ProductPurchaseDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/dn263390) contenant le nom du produit in-app.

```CSharp
string offerId = "1234";
string displayPropertiesName = "MusicOffer1";
var displayProperties = new ProductPurchaseDisplayProperties(displayPropertiesName);

try
{
    PurchaseResults purchaseResults = await CurrentAppSimulator.RequestProductPurchaseAsync("product1", offerId, displayProperties);
    switch (purchaseResults.Status)
    {
        case ProductPurchaseStatus.Succeeded:
            // Grant the user their purchase here, and then pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
            // To indicate local fulfillment to the Windows Store.
            break;
        case ProductPurchaseStatus.NotFulfilled:
            // First check for unfulfilled purchases and grant any unfulfilled purchases from an earlier transaction.
            // Once products are fulfilled pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
            // To indicate local fulfillment to the Windows Store.
            break;
        case ProductPurchaseStatus.NotPurchased:
            // Notify user that the purchase was not completed due to cancellation or error.
            break;
    }
}
catch (Exception)
{
    //Notify the user of the purchase error.
}
```

## Signaler l’acquisition de l’offre in-app

Votre application devra signaler l’acquisition du produit au Store dès que l’offre aura été acquise localement. Dans le cas d’un grand catalogue, si votre application ne signale pas l’acquisition de l’offre, l’utilisateur ne pourra pas acheter d’offres dans l’application à l’aide de la même liste de produits du Windows Store.

Comme mentionné précédemment, le Windows Store utilise uniquement les informations sur les offres fournies pour renseigner l’élément [**PurchaseResults**](https://msdn.microsoft.com/library/windows/apps/dn263392), sans créer d’association durable entre une offre d’un vaste catalogue et une liste de produits du Windows Store. Par conséquent, vous devez vérifier l’autorisation des utilisateurs à accéder aux produits et fournir un contexte spécifique au produit (tel que le nom de l’article acheté ou des détails le concernant) pour l’utilisateur extérieur à l’opération [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382).

Le code suivant illustre l’appel d’acquisition et un schéma de message d’interface utilisateur dans lequel sont insérées les informations sur l’offre spécifique. En l’absence de ces informations sur un produit spécifique, l’exemple utilise les informations du produit [**ListingInformation**](https://msdn.microsoft.com/library/windows/apps/br225163).

```CSharp
string offerId = "1234";
product1ListingName = product1.Name;
string displayPropertiesName = "MusicOffer1";

if (String.IsNullOrEmpty(displayPropertiesName))
{
    displayPropertiesName = product1ListingName;
}
var offerIdMsg = " with offer id " + offerId;
if (String.IsNullOrEmpty(offerId))
{
    offerIdMsg = " with no offer id";
}

FulfillmentResult result = await CurrentAppSimulator.ReportConsumableFulfillmentAsync(productId, transactionId);
switch (result)
{
    case FulfillmentResult.Succeeded:
        Log("You bought and fulfilled " + displayPropertiesName + offerIdMsg, NotifyType.StatusMessage);
        break;
    case FulfillmentResult.NothingToFulfill:
        Log("There is no purchased product 1 to fulfill.", NotifyType.StatusMessage);
        break;
    case FulfillmentResult.PurchasePending:
        Log("You bought product 1. The purchase is pending so we cannot fulfill the product.", NotifyType.StatusMessage);
        break;
    case FulfillmentResult.PurchaseReverted:
        Log("You bought product 1. But your purchase has been reverted.", NotifyType.StatusMessage);
        // Since the user' s purchase was revoked, they got their money back.
        // You may want to revoke the user' s access to the consumable content that was granted.
        break;
    case FulfillmentResult.ServerError:
        Log("You bought product 1. There was an error when fulfilling.", NotifyType.StatusMessage);
        break;
}
```

## Rubriques connexes

* [Activer les achats de produits dans l’application](enable-in-app-product-purchases.md)
* [Activer l’achat de produits in-app consommables](enable-consumable-in-app-product-purchases.md)
* [Exemple du Windows Store (montre des versions d’évaluation et des achats in-app)](http://go.microsoft.com/fwlink/p/?LinkID=627610)
* [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382)
* [**ProductPurchaseDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/dn263384)


<!--HONumber=Mar16_HO1-->


