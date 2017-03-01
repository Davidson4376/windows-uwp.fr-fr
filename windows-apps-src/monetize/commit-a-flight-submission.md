---
author: mcleanbyron
ms.assetid: F94AF8F6-0742-4A3F-938E-177472F96C00
description: "Utilisez cette méthode ds l’API de soum. du Windows Store pr valider une soum. de version d’éval. de pack. nouvelle ou mise à jour vers le Ctre de dév. Windows."
title: "Val. soum. de version d’éval. de pack. av l’API de soum. Windows Store"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, API de soumission du Windows Store, validation d&quot;une soumission de version d&quot;évaluation"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 305c9280b00fbbd669bee31732e831238054f177
ms.lasthandoff: 02/07/2017

---

# <a name="commit-a-package-flight-submission-using-the-windows-store-submission-api"></a>Val. soum. de version d’éval. de pack. av l’API de soum. Windows Store




Utilisez cette méthode ds l’API de soum. du Windows Store pr valider une soum. de version d’éval. de pack. nouvelle ou mise à jour vers le Ctre de dév. Windows. L’action de validation alerte le Centre de développement que les données de soumission ont été chargées sur le serveur (y compris les packages associées, le cas échéant). En réponse, le Centre de développement valide les modifications apportées aux données de soumission en vue de leur intégration et publication. Dès lors que l’opération de validation a abouti, les modifications apportées à la soumission s’affichent dans le tableau de bord du Centre de développement.

Pour plus d’informations sur la façon dont cette opération de validation s’inscrit dans le processus de création d’une soumission de version d’évaluation de package à l’aide de l’API de soumission du Windows Store, consultez [Gérer les soumissions de versions d’évaluation de package](manage-flight-submissions.md).

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission du Windows Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* [Créez une soumission de version d’évaluation de package ](create-a-flight-submission.md), puis [mettez à jour cette soumission](update-a-flight-submission.md) avec les éventuelles modifications nécessaires apportées aux données de soumission.

>**Remarque**&nbsp;&nbsp;Cette méthode ne peut être utilisée que pour les comptes du Centre de développement Windows qui ont reçu l’autorisation d’utiliser l’API de soumission du Windows Store. Tous les comptes ne bénéficient pas de cette autorisation.

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de la requête                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit``` |

<span/>
 

### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |

<span/>

### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | chaîne | Obligatoire. L’ID Windows Store de l’application qui contient la soumission de version d’évaluation de package à valider. L’ID Windows Store est disponible dans le tableau de bord du Centre de développement.  |
| flightId | chaîne | Obligatoire. ID de la version d’évaluation de package qui contient la soumission à valider. Cet ID est disponible dans le tableau de bord du Centre de développement et figure également dans les données de réponse des requêtes pour [créer une version d’évaluation de package](create-a-flight.md) ou [obtenir des versions d’évaluation de package pour une application](get-flights-for-an-app.md).  |
| submissionId | chaîne | Obligatoire. ID de la soumission à valider. Cet ID est disponible dans le tableau de bord du Centre de développement et figure également dans les données de réponse des requêtes pour [créer une soumission de version d’évaluation de package](create-a-flight-submission.md).  |

<span/>

### <a name="request-body"></a>Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment valider une soumission de version d’évaluation de package.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649/commit HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Pour plus d’informations sur les valeurs figurant dans le corps de réponse, voir les sections suivantes.

```json
{
  "status": "CommitStarted"
}
```

### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | chaîne  | État de la soumission. Les valeurs possibles sont les suivantes : <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>  |

<span/>

## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description   |
|--------|------------------|
| 400  | Les paramètres de la requête ne sont pas valides. |
| 404  | La soumission spécifiée est introuvable. |
| 409  | La soumission spécifiée a été trouvée, mais elle n’a pas pu être validée en raison de son état actuel, ou l’application utilise une fonctionnalité du tableau de bord du Centre de développement qui n’est [actuellement pas prise en charge par l’API de soumission du Windows Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |

<span/>


## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les soumissions de versions d’évaluation de package](manage-flight-submissions.md)
* [Obtenir une soumission de version d’évaluation du package](get-a-flight-submission.md)
* [Créer une soumission de version d’évaluation du package](create-a-flight-submission.md)
* [Mettre à jour une soumission de version d’évaluation de package](update-a-flight-submission.md)
* [Supprimer une soumission de version d’évaluation de package](delete-a-flight-submission.md)
* [Obtenir l’état d’une soumission de version d’évaluation du package](get-status-for-a-flight-submission.md)

