---
author: mcleanbyron
ms.assetid: 8C1E9E36-13AF-4386-9D0F-F9CB320F02F5
description: "Utilisez cette méth. dans l’API de soum. du Windows Store pour créer une version d’éval. de package pour une app. inscrite dans le cpte du Ctre de dév. Windows."
title: "Créer une version d’éval. de pack. av l’API de soum. du Windows Store"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 35823bd1fd0c059ebc9b2107c31400a7ad788a1e

---

# Créer une version d’éval. de pack. av l’API de soum. du Windows Store




Utilisez cette méth. dans l’API de soum. du Windows Store pour créer une version d’éval. de package pour une app. inscrite dans le cpte du Ctre de dév. Windows.

>**Remarque**&nbsp;&nbsp;Cette méthode permet de créer une version d’évaluation de package sans soumission. Pour créer une soumission pour une version d’évaluation de package, voir les méthodes présentées dans l’article [Gérer les soumissions de version d’évaluation de package](manage-flight-submissions.md).

## Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission du Windows Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous disposez de 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

>**Remarque**&nbsp;&nbsp;Cette méthode ne peut être utilisée que pour les comptes du Centre de développement Windows qui ont reçu l’autorisation d’utiliser l’API de soumission du Windows Store. Tous les comptes ne bénéficient pas de cette autorisation.

## Requête

Cette méthode présente la syntaxe suivante. Voir les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de la requête                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights``` |

<span/>
 

### En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |

<span/>

### Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | chaîne | Obligatoire. ID Windows Store de l’application pour laquelle vous voulez créer une version d’évaluation de package. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |

<span/>

### Corps de la requête

Le corps de la requête contient les paramètres suivants.
 
|  Paramètre  |  Type  |  Description  |  Obligatoire  |
|------|------|------|------|
|  friendlyName  |  chaîne  |  Nom de la version d’évaluation du package, tel que spécifié par le développeur.  |  Non  |
|  groupIds  |  tableau  |  Tableau de chaînes qui contiennent les ID des groupes de versions d’évaluation associés à la version d’évaluation de package. Pour plus d’informations sur les groupes de versions d’évaluation, voir [Versions d’évaluation de package](https://msdn.microsoft.com/windows/uwp/publish/package-flights).  |  Non  |
|  rankHigherThan  |  chaîne  |  Nom convivial de la version d’évaluation de package qui est classée juste en dessous de la version d’évaluation de package actuelle. Si vous ne définissez pas ce paramètre, la nouvelle version d’évaluation de package est classée au-dessus de toutes les autres. Pour plus d’informations sur le classement des groupes de versions d’évaluation, voir [Versions d’évaluation de package](https://msdn.microsoft.com/windows/uwp/publish/package-flights).    |  Non  |

<span/>

### Exemple de requête

L’exemple suivant montre comment créer une nouvelle version d’évaluation de package pour une application ayant l’ID Windows Store 9WZDNCRD911W.

```syntax
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q...
Content-Type: application/json
{
  "friendlyName": "myflight",
  "groupIds": [
    0
  ],
  "rankHigherThan": null
}

```

## Réponse

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Pour plus d’informations sur les valeurs figurant dans le corps de réponse, voir les sections suivantes.

```json
{
  "flightId": "43e448df-97c9-4a43-a0bc-2a445e736bcd",
  "friendlyName": "myflight",
  "groupIds": [
    "0"
  ],
  "rankHigherThan": "671c2857-725e-4faf-9e9e-ea1191ef879c"
}
```

### Corps de la réponse

| Valeur      | Type   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightId            | chaîne  | ID de la version d’évaluation du package. Cette valeur est fournie par le Centre de développement.  |
| friendlyName           | chaîne  | Nom de la version d’évaluation du package, tel que spécifié dans la requête.   |  
| groupIds           | tableau  | Tableau de chaînes qui contiennent les ID des groupes de versions d’évaluation associés à la version d’évaluation de package, comme spécifié dans la requête. Pour plus d’informations sur les groupes de versions d’évaluation, voir [Versions d’évaluation de package](https://msdn.microsoft.com/windows/uwp/publish/package-flights).   |
| rankHigherThan           | chaîne  | Nom convivial de la version d’évaluation de package qui est classée juste en dessous de la version d’évaluation de package actuelle, comme spécifié dans la requête. Pour plus d’informations sur le classement des groupes de versions d’évaluation, voir [Versions d’évaluation de package](https://msdn.microsoft.com/windows/uwp/publish/package-flights).  |

<span/>

## Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Code d’erreur |  Description   |
|--------|------------------|
| 400  | La requête n’est pas valide. |
| 409  | La version d’évaluation de package n’a pas pu être créée en raison de son état actuel, ou l’application utilise une fonctionnalité du tableau de bord du Centre de développement qui n’est [actuellement pas prise en charge par l’API de soumission du Windows Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   
<span/>

## Rubriques connexes

* [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obtenir une version d’évaluation de package](get-a-flight.md)
* [Supprimer une version d’évaluation de package](delete-a-flight.md)



<!--HONumber=Aug16_HO5-->


