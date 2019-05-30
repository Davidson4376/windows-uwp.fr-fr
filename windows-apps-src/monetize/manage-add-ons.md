---
ms.assetid: 4F9657E5-1AF8-45E0-9617-45AF64E144FC
description: Utilisez ces méthodes dans l’API de soumission de Microsoft Store pour gérer les modules complémentaires pour les applications qui sont inscrits dans votre compte espace partenaires.
title: Gérer les extensions
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, API de soumission au Microsoft Store, extensions, produit dans l'app, FAI
ms.localizationpriority: medium
ms.openlocfilehash: 8e06f8e915466f116692c63df5c53c2a0f97447f
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372494"
---
# <a name="manage-add-ons"></a>Gérer les extensions

Utilisez les méthodes suivantes dans l’API de soumission au Microsoft Store pour gérer les extensions de vos apps. Pour obtenir une présentation de l’API de soumission au Microsoft Store, notamment les conditions préalables à l’utilisation de l’API, voir [Créer et gérer des soumissions à l’aide des services au Microsoft Store](create-and-manage-submissions-using-windows-store-services.md).

Ces méthodes peuvent uniquement être utilisées pour obtenir, créer ou supprimer des extensions. Pour créer des soumissions pour des extensions, voir les méthodes indiquées dans [Gérer les soumissions d’extensions](manage-add-on-submissions.md).

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Méthode</th>
<th align="left">URI</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts</td>
<td align="left"><a href="get-all-add-ons.md">Obtenir tous les modules complémentaires pour vos applications</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}</td>
<td align="left"><a href="get-an-add-on.md">Obtenir un module complémentaire spécifique</a></td>
</tr>
<tr>
<td align="left">PUBLIER</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts</td>
<td align="left"><a href="create-an-add-on.md">Créer un module complémentaire</a></td>
</tr>
<tr>
<td align="left">Suppression</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}</td>
<td align="left"><a href="delete-an-add-on.md">Supprimer un module complémentaire</a></td>
</tr>
</tbody>
</table>

## <a name="prerequisites"></a>Prérequis

Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au Microsoft Store avant d’essayer d’utiliser l’une de ces méthodes.

## <a name="data-resources"></a>Ressources de données

Les méthodes de l’API de soumission au Microsoft Store pour gérer les extensions utilisent les ressources de données JSON suivantes.

<span id="add-on-object" />

### <a name="add-on-resource"></a>Ressource d’extension

Cette ressource décrit une extension.

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
  "productId": "TestAddOn",
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

Cette ressource a les valeurs suivantes.

| Value      | type   | Description        |
|------------|--------|--------------|
| applications      | tableau  | Tableau qui contient une [ressource d’application](#application-object) qui représente l’application à laquelle cette extension est associée. Un seul élément est pris en charge dans ce tableau.  |
| id | chaîne  | ID Windows Store de l’extension. Cette valeur est fournie par le Windows Store. Exemple d’ID Windows Store : 9NBLGGH4TNMP.  |
| productId | chaîne  | ID de produit de l’extension. Il s’agit de l’ID fourni par le développeur au moment de la création de l’extension. Pour plus d’informations, consultez [Définir le type et l’ID de votre produit](https://docs.microsoft.com/windows/uwp/publish/set-your-iap-product-id). |
| productType | chaîne  | Type de produit de l’extension. Les valeurs suivantes sont prises en charge : **Durable** et **consommable**.  |
| lastPublishedInAppProductSubmission       | objet | [Ressource de soumission](#submission-object) qui fournit des informations sur la dernière soumission publiée de l’extension.         |
| pendingInAppProductSubmission        | objet  |  [Ressource de soumission](#submission-object) qui fournit des informations sur la soumission actuellement en attente pour l’extension.  |   |

<span id="application-object" />

### <a name="application-resource"></a>Ressource d’application

Cette ressource décrit l’application à laquelle une extension est associée. L’exemple suivant illustre le format de cette ressource.

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
}
```

Cette ressource a les valeurs suivantes.

| Value           | type    | Description        |
|-----------------|---------|-----------|
| valeur            | objet  |  Objet qui contient les valeurs suivantes : <br/><br/> <ul><li>*id*. ID Windows Store de l’application. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details).</li><li>*resourceLocation*. Chemin relatif à ajouter à l’URI de requête ```https://manage.devcenter.microsoft.com/v1.0/my/``` de base pour récupérer les données complètes de l’application.</li></ul>   |
| totalCount   | entier  | Nombre d’objets d’application dans le tableau *applications* du corps de la réponse.                                                                                                                                                 |

<span id="submission-object" />

### <a name="submission-resource"></a>Ressource de soumission

Cette ressource fournit des informations sur une soumission pour une extension. L’exemple suivant illustre le format de cette ressource.

```json
{
  "pendingInAppProductSubmission": {
    "id": "1152921504621243619",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
  },
}
```

Cette ressource a les valeurs suivantes.

| Value           | type    | Description     |
|-----------------|---------|------------------|
| id            | chaîne  | ID de la soumission.    |
| resourceLocation   | chaîne  | Chemin relatif à ajouter à l’URI de requête ```https://manage.devcenter.microsoft.com/v1.0/my/``` de base pour récupérer les données complètes de la soumission.     |
 
<span/>

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les envois de module complémentaire à l’aide de l’API de soumission de Microsoft Store](manage-add-on-submissions.md)
* [Obtenir tous les modules complémentaires](get-all-add-ons.md)
* [Obtenir un module complémentaire](get-an-add-on.md)
* [Créer un module complémentaire](create-an-add-on.md)
* [Supprimer un module complémentaire](delete-an-add-on.md)
