---
author: mcleanbyron
ms.assetid: 4F9657E5-1AF8-45E0-9617-45AF64E144FC
description: "Utilisez ces méthodes dans l’API de soumission du Windows Store pour gérer les extensions des applications qui sont inscrites dans votre compte du Centre de développement Windows."
title: "Gérer les modules complémentaires"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, API de soumission du Windows Store, extensions, produit dans l&quot;application, FAI
ms.openlocfilehash: b442f48b7d03f0f972882ec240dbc1f37f018fd9
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="manage-add-ons"></a>Gérer les modules complémentaires

Utilisez les méthodes suivantes dans l’API de soumission du WindowsStore pour gérer les extensions (également connues sous le nom produits in-app ou PIA) pour vos applications. Pour obtenir une présentation de l’API de soumission du WindowsStore, notamment les conditions préalables à l’utilisation de l’API, voir [Créer et gérer des soumissions à l’aide des services du WindowsStore](create-and-manage-submissions-using-windows-store-services.md).

>**Remarque**&nbsp;&nbsp;Ces méthodes ne peuvent être utilisées que pour les comptes du Centre de développement Windows qui ont reçu l’autorisation d’utiliser l’API de soumission du Windows Store. L’octroi de cette autorisation s’effectue en plusieurs étapes. Elle est accordée aux comptes de développeur, et tous les comptes n’en bénéficient pas pour le moment. Pour demander un accès anticipé, connectez-vous au tableau de bord du Centre de développement, cliquez sur **Commentaires** au bas du tableau de bord, sélectionnez **API de soumission** dans la zone de commentaires, puis soumettez votre demande. Vous recevrez un message électronique dès que cette autorisation sera accordée à votre compte.

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
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts```</td>
<td align="left">[Obtient toutes les extensions pour vos applications](get-all-add-ons.md)</td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}```</td>
<td align="left">[Obtient une extension spécifique](get-an-add-on.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts```</td>
<td align="left">[Crée une extension](create-an-add-on.md)</td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}```</td>
<td align="left">[Supprime une extension](delete-an-add-on.md)</td>
</tr>
</tbody>
</table>

## <a name="prerequisites"></a>Prérequis

Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission du WindowsStore avant d’essayer d’utiliser l’une de ces méthodes.

## <a name="data-resources"></a>Ressources de données

Les méthodes de l’API de soumission du WindowsStore pour gérer les extensions utilisent les ressources de données JSON suivantes.

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

| Valeur      | Type   | Description        |
|------------|--------|--------------|
| applications      | tableau  | Tableau qui contient une [ressource d’application](#application-object) qui représente l’application à laquelle cette extension est associée. Un seul élément est pris en charge dans ce tableau.  |
| id | chaîne  | ID WindowsStore de l’extension. Cette valeur est fournie par le Windows Store. Exemple d’ID Windows Store: 9NBLGGH4TNMP.  |
| productId | chaîne  | ID de produit de l’extension. Il s’agit de l’ID fourni par le développeur au moment de la création de l’extension. Pour plus d’informations, consultez [Définir le type et l’ID de votre produit](https://msdn.microsoft.com/windows/uwp/publish/set-your-iap-product-id). |
| productType | chaîne  | Type de produit de l’extension. Les valeurs suivantes sont prises en charge: **Durable** et **Consommable**.  |
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

| Valeur           | Type    | Description        |
|-----------------|---------|-----------|
| value            | objet  |  Objet qui contient les valeurs suivantes: <br/><br/> <ul><li>*id*. ID Windows Store de l’application. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).</li><li>*resourceLocation*. Chemin relatif à ajouter à l’URI de requête ```https://manage.devcenter.microsoft.com/v1.0/my/``` de base pour récupérer les données complètes de l’application.</li></ul>   |
| totalCount   | entier  | Nombre d’objets d’application dans le tableau *applications* du corps de la réponse.                                                                                                                                                 |

<span id="submission-object" />
### <a name="submission-resource"></a>Ressource de soumission

Cette ressource fournit des informations sur une soumission d’une extension. L’exemple suivant illustre le format de cette ressource.

```json
{
  "pendingInAppProductSubmission": {
    "id": "1152921504621243619",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
  },
}
```

Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description     |
|-----------------|---------|------------------|
| id            | chaîne  | ID de la soumission.    |
| resourceLocation   | chaîne  | Chemin relatif à ajouter à l’URI de requête ```https://manage.devcenter.microsoft.com/v1.0/my/``` de base pour récupérer les données complètes de la soumission.     |
 
<span/>
## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les soumissions d’extensions à l’aide de l’API de soumission du Windows Store](manage-add-on-submissions.md)
* [Obtenir toutes les extensions](get-all-add-ons.md)
* [Obtenir une extension](get-an-add-on.md)
* [Créer une extension](create-an-add-on.md)
* [Supprimer une extension](delete-an-add-on.md)
