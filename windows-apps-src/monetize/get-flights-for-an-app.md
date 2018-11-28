---
ms.assetid: B0AD0B8E-867E-4403-9CF6-43C81F3C30CA
description: Utilisez cette méthode dans l’API de soumission au Microsoft Store pour récupérer des informations de version d’évaluation du package pour une application inscrite dans votre compte espace partenaires.
title: Obtenir des versions d’évaluation du package pour une application
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp, API de soumission au MicrosoftStore, versions d’évaluation, versions d’évaluation de package
ms.localizationpriority: medium
ms.openlocfilehash: c7e7ab4db7690cee86b76e39caa30b3c0fb25618
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7826282"
---
# <a name="get-package-flights-for-an-app"></a>Obtenir des versions d’évaluation du package pour une application

Utilisez cette méthode dans l’API de soumission au Microsoft Store pour répertorier les versions d’évaluation de package pour une application inscrite dans votre compte espace partenaires. Pour plus d’informations sur les versions d’évaluation du package, voir [Versions d’évaluation du package](https://msdn.microsoft.com/windows/uwp/publish/package-flights).

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de la requête                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

|  Nom  |  Type  |  Description  |  Requis  |
|------|------|------|------|
|  applicationId  |  chaîne  |  ID WindowsStore de l’application pour laquelle vous voulez récupérer les versions d’évaluation du package. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |  Oui  |
|  top  |  entier  |  Nombre d’éléments à retourner dans la requête (autrement dit, nombre de versions d’évaluation du package à retourner). Si votre compte comporte plus de versions d’évaluation du package que la valeur que vous spécifiez dans la requête, le corps de la réponse comprend un chemin d’URI relatif que vous pouvez ajouter à l’URI de la méthode pour solliciter la page suivante de données.  |  Non  |
|  skip  |  entier  |  Nombre d’éléments à ignorer dans la requête avant de retourner les éléments restants. Utilisez ce paramètre pour parcourir des ensembles de données. Par exemple, top=10 et skip=0 permettent de récupérer les éléments 1 à10, top=10 et skip=10 permettent de récupérer les éléments 11 à20, et ainsi de suite.  |  Non  |


### <a name="request-body"></a>Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-examples"></a>Exemples de requête

L’exemple suivant montre comment répertorier toutes les versions d’évaluation du package pour une application.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listflights HTTP/1.1
Authorization: Bearer <your access token>
```

L’exemple suivant montre comment répertorier la première version d’évaluation du package pour une application.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listflights?top=1 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

L’exemple suivant illustre le corps de réponse JSON renvoyé par une requête réussie de la première version d’évaluation du package pour une application qui en comporte trois au total. Pour plus d’informations sur les valeurs figurant dans le corps de réponse, voir la section suivante.

```json
{
  "value": [
    {
      "flightId": "7bfc11d5-f710-47c5-8a98-e04bb5aad310",
      "friendlyName": "myflight",
      "lastPublishedFlightSubmission": {
        "id": "1152921504621086517",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621086517"
      },
      "pendingFlightSubmission": {
        "id": "1152921504621215786",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621215786"
      },
      "groupIds": [
        "1152921504606962205"
      ],
      "rankHigherThan": "Non-flighted submission"
    }
  ],
  "totalCount": 3
}
```

### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type   | Description       |
|------------|--------|---------------------|
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne contient un chemin relatif que vous pouvez ajouter à l’URI de requête ```https://manage.devcenter.microsoft.com/v1.0/my/``` de base pour solliciter la page suivante de données. Par exemple, si le paramètre *top* du corps de requête initial a la valeur 2, mais qu’il existe 4versions d’évaluation du package pour l’application, le corps de réponse comprendra une valeur @nextLink```applications/{applicationid}/listflights/?skip=2&top=2```, ce qui indique que vous pouvez appeler ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/listflights/?skip=2&top=2``` pour solliciter les 2versions d’évaluation du package suivantes. |
| value      | tableau  | Tableau d’objets qui fournissent des informations sur les versions d’évaluation du package pour l’application spécifiée. Pour plus d’informations sur les données incluses dans chaque objet, voir [Ressource de version d’évaluation du package ](get-app-data.md#flight-object).               |
| totalCount | entier    | Nombre total de lignes dans les résultats de données pour la requête (autrement dit, nombre total de versions d’évaluation du package pour l’application spécifiée).   |


## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description   |
|--------|------------------|
| 404  | Aucune version d’évaluation du package n’a été trouvée. |
| 409  | L’application utilise une fonctionnalité de l’espace partenaires qui n’est [actuellement pas pris en charge par l’API de soumission au Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>Rubriquesassociées

* [Créer et gérer des soumissions à l’aide des services du MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md)
* [Obtenir toutes les apps](get-all-apps.md)
* [Obtenir une application](get-an-app.md)
* [Obtenir des extensions pour une application](get-add-ons-for-an-app.md)
