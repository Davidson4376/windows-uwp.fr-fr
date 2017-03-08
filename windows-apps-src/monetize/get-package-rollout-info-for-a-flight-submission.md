---
author: mcleanbyron
description: "Utilisez cette méthode dans l’API de soumission du Windows Store pour obtenir les informations de lancement du package pour une soumission de version d’évaluation du package."
title: "Obtenez les informations sur le lancement du package pour une soumission de version d’évaluation du package à l’aide de l’API de soumission du Windows Store."
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, API de soumission du Windows Store, lancement de package, soumission de version d’évaluation"
ms.assetid: 397f1b99-2be7-4f65-bcf1-9433a3d496ad
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: d47bd0a5df654ba723c1c7650ea9779ee5962993
ms.lasthandoff: 02/08/2017

---

# <a name="get-package-rollout-info-for-a-package-flight-submission-using-the-windows-store-submission-api"></a>Obtenez les informations sur le lancement du package pour une soumission de version d’évaluation du package à l’aide de l’API de soumission du Windows Store.


Appliquez cette méthode dans l’API de soumission du Windows Store pour récupérer les informations sur le [lancement du package](../publish/gradual-package-rollout.md) pour une soumission de version d’évaluation du package. Pour plus d’informations sur le processus de création d’une soumission de version d’évaluation de package à l’aide de l’API de soumission du Windows Store, voir [Gérer les soumissions de versions d’évaluation du package](manage-flight-submissions.md).

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission du Windows Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Créez une soumission de version d’évaluation du package pour une application dans votre compte du Centre de développement. Pour cela, vous pouvez utiliser le tableau de bord du Centre de développement ou la méthode [Créer une soumission de version d’évaluation du package](create-a-flight-submission.md).

>**Remarque**&nbsp;&nbsp;Cette méthode ne peut être utilisée que pour les comptes du Centre de développement Windows qui ont reçu l’autorisation d’utiliser l’API de soumission du Windows Store. Tous les comptes ne bénéficient pas de cette autorisation.

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et des paramètres de la requête.

| Méthode | URI de la requête                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/packagerollout   ``` |

<span/>
 

### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |

<span/>

### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | chaîne | Obligatoire. ID Windows Store de l’application qui contient la soumission de version d’évaluation du package avec les informations de lancement du package que vous souhaitez obtenir. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | chaîne | Obligatoire. ID de la version d’évaluation du package contenant la soumission avec les informations de lancement du package que vous souhaitez obtenir. Cet ID est disponible dans le tableau de bord du Centre de développement et figure également dans les données de réponse aux requêtes visant à [créer une version d’évaluation du package](create-a-flight.md) ou à [obtenir des versions d’évaluation du package pour une application](get-flights-for-an-app.md).  |
| submissionId | chaîne | Obligatoire. ID de la soumission avec les informations de lancement du package à obtenir. Cet ID est disponible dans le tableau de bord du Centre de développement et figure également dans les données de réponse aux requêtes visant à [créer une soumission de version d’évaluation du package](create-a-flight-submission.md).  |

<span/>

### <a name="request-body"></a>Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment obtenir les informations de lancement du package d’une soumission de version d’évaluation de package.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649/packagerollout HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

L’exemple suivant décrit le corps de la réponse JSON pour un appel réussi à cette méthode, pour une soumission de version d’évaluation du package avec le lancement progressif du package activé. Pour plus d’informations sur les valeurs figurant dans le corps de la réponse, voir la [ressource de lancement du package](manage-flight-submissions.md#package-rollout-object).

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 25,
    "packageRolloutStatus": "PackageRolloutInProgress",
    "fallbackSubmissionId": "1212922684621243058"
}
```

Si la soumission de la version d’évaluation du package ne présente pas de lancement progressif de package activé, le corps de réponse suivant sera renvoyé.

```json
{
    "isPackageRollout": false,
    "packageRolloutPercentage": 0,
    "packageRolloutStatus": "PackageRolloutNotStarted",
    "fallbackSubmissionId": "0"
}
```

## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description   |
|--------|------------------|
| 404  | La soumission de version d’évaluation du package est introuvable. |
| 409  | La soumission de version d’évaluation du package n’appartient pas à la version d’évaluation du package spécifiée, ou l’application utilise une fonctionnalité du tableau de bord du Centre de développement qui n’est [actuellement pas prise en charge par l’API de soumission du Windows Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

<span/>


## <a name="related-topics"></a>Rubriques connexes

* [Lancement de package progressif](../publish/gradual-package-rollout.md)
* [Gérer les soumissions de versions d’évaluation de package à l’aide de l’API de soumission du Windows Store](manage-flight-submissions.md)
* [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md)

