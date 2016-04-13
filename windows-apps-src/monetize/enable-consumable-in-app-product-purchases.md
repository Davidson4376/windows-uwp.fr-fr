---
Description: Proposez des produits consommables dans l’application&\#8212; qui peuvent être achetés, utilisés et rachetés&\#8212;via la plateforme commerciale du Windows Store, afin d’offrir à vos clients une expérience d’achat à la fois solide et fiable au sein de l’application.
title: Activer les achats de produits consommables in-app
ms.assetid: F79EE369-ACFC-4156-AF6A-72D1C7D3BDA4
mots-clés : offre dans l’application
mots-clés : consommables
mots-clés : achat dans l’application
mots-clés : produit in-app
mots-clés : comment prendre en charge les achats dans l’application
mots-clés : exemple de code d’un achat dans l’application
mots-clés : exemple de code d’une offre intégrée à l’application
---

# Activer les achats de produits consommables in-app


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Proposez des produits consommables dans l’application qui peuvent être achetés, utilisés et rachetés via la plateforme commerciale du Windows Store, afin d’offrir à vos clients une expérience d’achat à la fois solide et fiable au sein de l’application. Cette fonction est particulièrement utile pour différents aspects du jeu, comme les devises (or, pièces, etc.) susceptibles d’être achetées, puis utilisées pour acheter des améliorations spécifiques.

## Prérequis

-   Cette rubrique porte sur les rapports relatifs aux achats et acquisitions de produits in-app consommables. Si vous ne connaissez pas les produits dans l’application, passez en revue la section [Activer les achats de produits dans l’application](enable-in-app-product-purchases.md) pour en savoir plus sur les informations relatives aux licences et savoir comment répertorier correctement les produits de l’application dans le Windows Store.
-   Lorsque vous codez et testez de nouveaux produits dans l’application pour la première fois, vous devez utiliser l’objet [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) au lieu de l’objet [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765). Cela vous permet de vérifier votre logique de licence à l’aide d’appels simulés au serveur de licences au lieu d’appels au serveur Windows Live. Pour cela, vous devez personnaliser le fichier nommé « WindowsStoreProxy.xml » dans %userprofile%\\AppData\\local\\packages\\&lt;package name&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData. Le simulateur Microsoft Visual Studio crée ce fichier quand vous exécutez votre application pour la première fois. Vous pouvez également charger un fichier personnalisé au moment de l’exécution. Pour plus d’informations, voir **CurrentAppSimulator**.
-   Cette rubrique fait également référence à des exemples de code fournis dans l’[exemple du Windows Store](http://go.microsoft.com/fwlink/p/?LinkID=627610). Cet exemple représente un excellent moyen d’obtenir une expérience pratique avec les différentes options de monétisation fournies pour les applications UWP.

## Étape 1 : Établissement de la demande d’achat

La demande d’achat initiale est effectuée via le paramètre [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263381), comme tout autre achat effectué via le Windows Store. En ce qui concerne les produits dans l’application, la différence réside dans le fait qu’après avoir effectué un achat un client ne peut pas acheter le même produit tant que l’application n’a pas averti le Windows Store que l’achat précédent a été correctement effectué. Il revient à votre application d’acquérir les consommables achetés et d’avertir le Windows Store de l’opération.

L’exemple suivant représente une demande d’achat de produits consommables dans l’application. Vous noterez la présence de commentaires de code indiquant le moment auquel votre application doit effectuer l’acquisition locale du produit in-app consommable, selon deux scénarios différents : lorsque la demande aboutit ou lorsqu’elle échoue suite à un achat non effectué du même produit.

```CSharp
PurchaseResults purchaseResults = await CurrentAppSimulator.RequestProductPurchaseAsync("product1");
switch (purchaseResults.Status)
{
    case ProductPurchaseStatus.Succeeded:
        product1TempTransactionId = purchaseResults.TransactionId;

        // Grant the user their purchase here, and then pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
        // To indicate local fulfillment to the Windows Store.
        break;

    case ProductPurchaseStatus.NotFulfilled:
        product1TempTransactionId = purchaseResults.TransactionId;

        // First check for unfulfilled purchases and grant any unfulfilled purchases from an earlier transaction.
        // Once products are fulfilled pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
        // To indicate local fulfillment to the Windows Store.
        break;
}
```

## Étape 2 : Suivi de l’acquisition locale du consommable

Quand vous accordez à votre client un accès au produit in-app consommable, il est important de garder une trace des produits acquis (*productId*) et de chaque transaction associée à cette acquisition (*transactionId*).

**Important** Votre application est chargée de signaler correctement l’acquisition au Store. Cette étape est essentielle pour assurer une expérience d’achat juste et fiable à vos clients.

L’exemple suivant illustre l’utilisation des propriétés [**PurchaseResults**](https://msdn.microsoft.com/library/windows/apps/dn263392) à partir de l’appel [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263381) de l’étape précédente pour identifier le produit acheté à acquérir. Un tableau est utilisé pour stocker les informations sur le produit à un emplacement pouvant être ultérieurement référencé afin de confirmer que l’acquisition locale a abouti.

```CSharp
private void GrantFeatureLocally(string productId, Guid transactionId)
{
    if (!grantedConsumableTransactionIds.ContainsKey(productId))
    {
        grantedConsumableTransactionIds.Add(productId, new List<Guid>());
    }
    grantedConsumableTransactionIds[productId].Add(transactionId);

    // Grant the user their content. You will likely increase some kind of gold/coins/some other asset count.
}
```

L’exemple qui suit montre comment utiliser le tableau de l’exemple précédent pour accéder aux associations ID produit/ID de transaction, qui sont utilisées plus tard quand une acquisition est signalée au Store.

**Important** Quelle que soit la méthodologie utilisée par votre application pour suivre et confirmer l’acquisition, elle doit faire preuve de la diligence appropriée pour garantir que les clients n’ont pas à payer des articles qu’ils n’ont pas reçus.

```CSharp
private Boolean IsLocallyFulfilled(string productId, Guid transactionId)
{
    return grantedConsumableTransactionIds.ContainsKey(productId) &amp;&amp; grantedConsumableTransactionIds[productId].Contains(transactionId);
}
```

## Étape 3 : Signalement de l’acquisition de produits au Windows Store

Une fois l’acquisition locale effectuée, votre application doit passer un appel [**ReportConsumableFulfillmentAsync**](https://msdn.microsoft.com/library/windows/apps/dn263380) incluant l’élément *productId* et la transaction dans laquelle l’achat de produit est inclus.

**Important** Tant que vous ne signalerez pas les produits consommables acquis dans l’application au Windows Store, l’utilisateur ne pourra pas racheter ce produit.

```CSharp
FulfillmentResult result = await CurrentAppSimulator.ReportConsumableFulfillmentAsync("product2", product2TempTransactionId);
```

## Étape 4 : Identification des achats non acquis

Votre application peut utiliser la méthode [**GetUnfulfilledConsumablesAsync**](https://msdn.microsoft.com/library/windows/apps/dn263379) pour rechercher les produits in-app consommables non acquis et ce, à tout moment. Cette méthode doit être appelée régulièrement pour rechercher les consommables non acquis suite à des événements imprévus de l’application (interruption de la connectivité réseau, arrêt de l’application, etc.)

L’exemple suivant montre comment la méthode [**GetUnfulfilledConsumablesAsync**](https://msdn.microsoft.com/library/windows/apps/dn263379) peut être utilisée pour énumérer les consommables non fournis et comment votre application peut parcourir cette liste pour effectuer l’acquisition locale.

```CSharp
private async void GetUnfulfilledConsumables()
{
    products = await CurrentApp.GetUnfulfilledConsumablesAsync();

    foreach (UnfulfilledConsumable product in products)
    {
        logMessage += "\nProduct Id: " + product.ProductId + " Transaction Id: " + product.TransactionId;
        // This is where you would pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
    // To indicate local fulfillment to the Windows Store.
    }
}
```

## Rubriques connexes

* [Activer les achats de produits dans l’application](enable-in-app-product-purchases.md)
* [Exemple du Windows Store (montre des versions d’évaluation et des achats in-app)](http://go.microsoft.com/fwlink/p/?LinkID=627610)
* [**Windows.ApplicationModel.Store**](https://msdn.microsoft.com/library/windows/apps/br225197)
 

 






<!--HONumber=Mar16_HO1-->


