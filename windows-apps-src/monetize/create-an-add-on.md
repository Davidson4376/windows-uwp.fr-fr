---
ms.assetid: 5BD650D2-AA26-4DE9-8243-374FDB7D932B
description: Utilisez cette méthode dans l’API de soumission de Microsoft Store pour créer un module complémentaire pour une application qui est inscrit à votre compte PartnerCenter.
title: Créer une extension
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, API de soumission au Microsoft Store, créer une extension, produit in-app, PIA
ms.localizationpriority: medium
ms.openlocfilehash: 8465dc7a42961a20fcd33ba8d43c71e2d73727ff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651034"
---
# <a name="create-an-add-on"></a>Créer une extension

Utilisez cette méthode dans l’API de soumission de Microsoft Store pour créer un module complémentaire (également appelés dans l’application produit ou produits) pour une application qui est inscrit pour votre compte espace partenaires.

> [!NOTE]
> Cette méthode permet de créer une extension sans soumission. Pour créer une soumission pour une extension, voir les méthodes décrites dans l’article [Gérer les soumissions d’extensions](manage-add-on-submissions.md).

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de requête                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-body"></a>Corps de la requête

Le corps de la requête contient les paramètres suivants.

|  Paramètre  |  Type  |  Description  |  Obligatoire  |
|------|------|------|------|
|  applicationIds  |  tableau  |  Tableau qui contient l’ID Windows Store de l’application à laquelle cette extension est associée. Un seul élément est pris en charge dans ce tableau.   |  Oui  |
|  productId  |  chaîne  |  ID de produit de l’extension. Il s’agit d’un identificateur que vous pouvez utiliser dans le code pour faire référence à l’extension. Pour plus d’informations, consultez [Définir le type et l’ID de votre produit](https://msdn.microsoft.com/windows/uwp/publish/set-your-iap-product-id).  |  Oui  |
|  productType  |  chaîne  |  Type de produit de l’extension. Les valeurs suivantes sont prises en charge : **Durable** et **consommable**.  |  Oui  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment créer une extension consommable pour une application.

```syntax
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q...
Content-Type: application/json
{
    "applicationIds": [  "9NBLGGH4R315"  ],
    "productId": "my-new-add-on",
    "productType": "Consumable",
}
```

## <a name="response"></a>Réponse

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Pour plus d’informations sur les valeurs figurant dans le corps de réponse, consultez [ressource d’extension](manage-add-ons.md#add-on-object).

```json
{
  "applications": {
    "value": [
      {
        "id": "9NBLGGH4R315",
        "resourceLocation": "applications/9NBLGGH4R315"
      }
    ],
    "totalCount": 1
  },
  "id": "9NBLGGH4TNMP",
  "productId": "my-new-add-on",
  "productType": "Consumable",
}
```

## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description                                                                                                                                                                           |
|--------|------------------|
| 400  | La requête n’est pas valide. |
| 409  | Le module complémentaire n’a pas pu être créé en raison de son état actuel, ou le module complémentaire utilise une fonctionnalité de partenaires est [actuellement ne pas pris en charge par l’API de soumission de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les envois de module complémentaire](manage-add-on-submissions.md)
* [Obtenir tous les modules complémentaires](get-all-add-ons.md)
* [Obtenir un module complémentaire](get-an-add-on.md)
* [Supprimer un module complémentaire](delete-an-add-on.md)
