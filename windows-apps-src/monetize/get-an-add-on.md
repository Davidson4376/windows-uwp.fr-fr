---
author: mcleanbyron
ms.assetid: 78278741-09A4-4406-A112-9AF3C73F5C16
description: "Utilisez cette méthode de l’API de soumission du Windows Store pour récupérer des informations sur une extension pour une application inscrite dans votre compte du Centre de développement Windows."
title: Obtenir une extension
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, API de soumission du Windows Store, extension, produit dans l&quot;application, FAI
ms.openlocfilehash: a79c93c1ed7d38b4de1da2bbd05456fd0884692e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="get-an-add-on"></a>Obtenir une extension




Utilisez cette méthode dans l’API de soumission du Windows Store pour récupérer des informations sur une extension (également connue sous le nom PIA ou produit in-app) pour une application inscrite dans votre compte du Centre de développement Windows.

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission du Windows Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

>**Remarque**&nbsp;&nbsp;Cette méthode ne peut être utilisée que pour les comptes du Centre de développement Windows qui ont reçu l’autorisation d’utiliser l’API de soumission du Windows Store. Tous les comptes ne bénéficient pas de cette autorisation.

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de la requête                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}``` |

<span/>
 

### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |

<span/>

### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| id | chaîne | Obligatoire. ID Windows Store de l’extension à récupérer. L’ID Windows Store est disponible dans le tableau de bord du Centre de développement.  |

<span/>

### <a name="request-body"></a>Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.

<span/>

### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment récupérer des informations sur une extension.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP HTTP/1.1
Authorization: Bearer <your access token>
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
  "productId": "Test-add-on",
  "productType": "Durable",
  "pendingInAppProductSubmission": {
    "id": "1152921504621243619",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
  },
  "lastPublishedInAppProductSubmission": {
    "id": "1152921504621243705",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243705"
  }
}
```

## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description   |
|--------|------------------|
| 404  | L’extension spécifiée est introuvable. |
| 409  | L’extension utilise une fonctionnalité du tableau de bord du Centre de développement qui n’est [actuellement pas prise en charge par l’API de soumission du Windows Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |

<span/>

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les soumissions d’extensions](manage-add-on-submissions.md)
* [Obtenir toutes les extensions](get-all-add-ons.md)
* [Créer une extension](create-an-add-on.md)
* [Supprimer une extension](delete-an-add-on.md)
