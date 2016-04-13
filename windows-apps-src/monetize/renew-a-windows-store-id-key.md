---
ms.assetid: 3569C505-8D8C-4D85-B383-4839F13B2466
Utilisez cette méthode pour renouveler une clé du Windows Store.
Renouveler une clé d’ID du Windows Store
---

# Renouveler une clé d’ID du Windows Store


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Utilisez cette méthode pour renouveler une clé du Windows Store. Lorsque vous générez une clé d’ID du Windows Store en appelant la méthode [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674) ou [**GetCustomerPurchaseIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608675), la clé est valide pendant 90 jours. Après l’expiration de la clé, vous pouvez utiliser la clé arrivée à expiration pour en renégocier une nouvelle à l’aide de cette méthode.

## Conditions préalables


Pour utiliser cette méthode, vous devez disposer des éléments suivants :

-   un jeton d’accès Azure AD créé avec l’URI d’audience **https://onestore.microsoft.com** ;
-   une clé d’ID du Windows Store arrivée à expiration, qui a été générée en appelant la méthode [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674) ou [**GetCustomerPurchaseIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608675) à partir du code côté client de votre application.

Pour plus d’informations, voir [Afficher et octroyer des produits à partir d’un service](view-and-grant-products-from-a-service.md).

## Requête


### Syntaxe de la requête

| Type de clé    | Méthode | URI de la requête                                              |
|-------------|--------|----------------------------------------------------------|
| Collections | POST   | https://collections.mp.microsoft.com/v6.0/b2b/keys/renew |
| Achat    | POST   | https://purchase.mp.microsoft.com/v6.0/b2b/keys/renew    |

 

### En-tête de requête

| En-tête         | Type   | Description                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Host           | chaîne | Doit être défini sur la valeur **collections.mp.microsoft.com** ou **purchase.mp.microsoft.com**.           |
| Content-Length | nombre | Longueur du corps de la requête.                                                                       |
| Content-Type   | chaîne | Spécifie le type de requête et de réponse. Actuellement, la seule valeur prise en charge est **application/json**. |

 

### Corps de la requête

| Paramètre     | Type   | Description                       | Obligatoire |
|---------------|--------|-----------------------------------|----------|
| serviceTicket | chaîne | Jeton d’accès Azure AD.        | Oui      |
| key           | chaîne | Clé d’ID du Windows Store arrivée à expiration. | Non       |

 

### Exemple de requête

```
POST https://collections.mp.microsoft.com/v6.0/b2b/keys/renew HTTP/1.1
Content-Length: 2774
Content-Type: application/json
Host: collections.mp.microsoft.com

{ 
    "serviceTicket": "eyJ0eXAiOiJKV1QiLCJhb….",
    "Key": "eyJ0eXAiOiJKV1QiLCJhbG…."
}
```

## Réponse


### Corps de la réponse

| Paramètre | Type   | Description                                                                                                            | Obligatoire |
|-----------|--------|------------------------------------------------------------------------------------------------------------------------|----------|
| key       | chaîne | Clé du Windows Store actualisée qui peut être utilisée dans les futurs appels de l’API de collection ou de l’API d’achat du Windows Store. | Non       |

 

### Exemple de réponse

```
HTTP/1.1 200 OK
Content-Length: 1646
Content-Type: application/json
MS-CorrelationId: bfebe80c-ff89-4c4b-8897-67b45b916e47
MS-RequestId: 1b5fa630-d672-4971-b2c0-3713f4ea6c85
MS-CV: xu2HW6SrSkyfHyFh.0.0
MS-ServerId: 030011428
Date: Tue, 13 Sep 2015 07:31:12 GMT

{
    "key":"eyJ0eXAi….."
}
```

## Codes d’erreur


| Code | Erreur        | Code d’erreur interne           | Description                                                                                                                                                                           |
|------|--------------|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 401  | Non autorisé | AuthenticationTokenInvalid | Le jeton d’accès Azure AD n’est pas valide. Dans certains cas, les détails de l’erreur ServiceError contiennent plus d’informations, par exemple lorsque le jeton est arrivé à expiration ou que la revendication *appid* est manquante. |
| 401  | Non autorisé | InconsistentClientId       | La revendication *clientId* dans la clé d’ID du Windows Store et la revendication *appid* dans le jeton d’accès Azure AD ne correspondent pas.                                                                     |

 

## Rubriques connexes


* [Afficher et octroyer des produits à partir d’un service](view-and-grant-products-from-a-service.md)
* [Demander des produits](query-for-products.md)
* [Signaler le traitement de la commande d’un produit consommable](report-consumable-products-as-fulfilled.md)
* [Octroyer des produits gratuits](grant-free-products.md)



<!--HONumber=Mar16_HO1-->


