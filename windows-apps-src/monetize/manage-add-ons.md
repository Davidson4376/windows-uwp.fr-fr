---
author: mcleanbyron
ms.assetid: 4F9657E5-1AF8-45E0-9617-45AF64E144FC
description: "Utilisez ces méthodes dans l’API de soumission du Windows Store pour gérer les extensions des applications qui sont inscrites dans votre compte du Centre de développement Windows."
title: "Gérer les extensions à l’aide de l’API de soumission du Windows Store"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 75548ee4689fd31d734c570f8e3eca5d33a6181f

---

# Gérer les extensions à l’aide de l’API de soumission du Windows Store




Utilisez les méthodes suivantes dans l’API de soumission du Windows Store pour gérer les extensions (également connues sous le nom produits in-app ou PIA) pour les applications inscrites dans votre compte du Centre de développement Windows. Pour obtenir une présentation de l’API de soumission du Windows Store, voir [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md).

>**Remarque**&nbsp;&nbsp;Ces méthodes ne peuvent être utilisées que pour les comptes du Centre de développement Windows qui ont reçu l’autorisation d’utiliser l’API de soumission du Windows Store. Tous les comptes ne bénéficient pas de cette autorisation. Ces méthodes peuvent uniquement être utilisées pour obtenir, créer ou supprimer des extensions. Pour créer des soumissions pour des extensions, voir les méthodes indiquées dans [Gérer les soumissions d’extensions](manage-add-on-submissions.md).

| Méthode        | URI    | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` | Obtient les données de toutes les extensions de toutes les applications inscrites dans votre compte du Centre de développement Windows. Pour plus d’informations, voir [Obtenir toutes les extensions](get-all-add-ons.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId} ``` | Obtient les données d’une extension spécifique. Pour plus d’informations, voir [Obtenir une extension](get-an-add-on.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` | Crée une extension. Pour plus d’informations, voir [Créer une extension](create-an-add-on.md).  |
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}``` | Supprime une extension. Pour plus d’informations, voir [Supprimer une extension](delete-an-add-on.md). |

## Conditions préalables

Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission du Windows Store avant d’essayer d’utiliser l’une de ces méthodes.

## Ressources

Ces méthodes utilisent les ressources suivantes pour formater les données.

<span id="add-on-object" />
### Extension

Cette ressource représente une extension. L’exemple suivant illustre le format de cette ressource.

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

| Valeur      | Type   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applications      | tableau  | Tableau qui contient un objet qui représente l’application à laquelle est associée cette extension. Pour plus d’informations sur les données incluses dans cet objet, voir la section [Objet d’application](#application-object) ci-dessous. Un seul élément est pris en charge dans ce tableau.  |
| id | chaîne  | ID Windows Store de l’extension. Cette valeur est fournie par le Windows Store. Exemple d’ID Windows Store: 9NBLGGH4TNMP.  |
| productId | chaîne  | ID de produit de l’extension. Il s’agit de l’ID fourni par le développeur au moment de la création de l’extension. Pour plus d’informations, consultez [Définir le type et l’ID de votre produit](https://msdn.microsoft.com/windows/uwp/publish/set-your-iap-product-id). |
| productType | chaîne  | Type de produit de l’extension. Les valeurs suivantes sont prises en charge: **Durable** et **Consommable**.  |
| lastPublishedInAppProductSubmission       | objet | Objet qui fournit des informations sur la dernière soumission publiée de l’extension. Pour plus d’informations, voir la section [Soumission](#submission-object) ci-dessous.                                                                                                                                                          |
| pendingInAppProductSubmission        | objet  |  Objet qui fournit des informations sur la soumission actuellement en attente pour l’extension. Pour plus d’informations, voir la section [Soumission](#submission-object) ci-dessous.  |   |

<span id="application-object" />
### Application

Cette ressource représente une application à laquelle est associée à une extension. L’exemple suivant illustre le format de cette ressource.

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

| Valeur           | Type    | Description                                                                                                                                                                                                                          |
|-----------------|---------|-----------|
| value            | objet  |  Objet qui contient les valeurs suivantes: <br/><br/> <ul><li>*id*. ID Windows Store de l’application. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).</li><li>*resourceLocation*. Chemin relatif à ajouter à l’URI de requête ```https://manage.devcenter.microsoft.com/v1.0/my/``` de base pour récupérer les données complètes de l’application.</li></ul>   |
| totalCount   | entier  | Nombre d’objets d’application dans le tableau *applications* du corps de la réponse.                                                                                                                                                 |

<span id="submission-object" />
### Soumission

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

| Valeur           | Type    | Description                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | chaîne  | ID de la soumission.    |
| resourceLocation   | chaîne  | Chemin relatif à ajouter à l’URI de requête ```https://manage.devcenter.microsoft.com/v1.0/my/``` de base pour récupérer les données complètes de la soumission.                                                                                                                                               |
 
<span/>

## Rubriques connexes

* [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les soumissions d’extensions à l’aide de l’API de soumission du Windows Store](manage-add-on-submissions.md)
* [Obtenir toutes les extensions](get-all-add-ons.md)
* [Obtenir une extension](get-an-add-on.md)
* [Créer une extension](create-an-add-on.md)
* [Supprimer une extension](delete-an-add-on.md)



<!--HONumber=Aug16_HO5-->


