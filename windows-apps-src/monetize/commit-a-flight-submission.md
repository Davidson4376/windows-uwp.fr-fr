---
ms.assetid: F94AF8F6-0742-4A3F-938E-177472F96C00
description: Utilisez cette méthode dans l’API de soumission de Microsoft Store pour valider une soumission de vol de package nouveau ou mis à jour pour les partenaires.
title: Valider une soumission de version d’évaluation de package
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, API de soumission au Microsoft Store, valider une soumission de version d’évaluation
ms.localizationpriority: medium
ms.openlocfilehash: 0cf1abcb1575252456383fd8fe187962567a6096
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334407"
---
# <a name="commit-a-package-flight-submission"></a>Valider une soumission de version d’évaluation de package

Utilisez cette méthode dans l’API de soumission de Microsoft Store pour valider une soumission de vol de package nouveau ou mis à jour pour les partenaires. La validation alertes partenaire centre de maintenance que les données de soumission a été chargées (y compris tous les packages associés). En réponse, partenaires valide les modifications de données de soumission pour l’ingestion et la publication. Une fois l’opération de validation réussit, les modifications apportées à la soumission figurent dans les partenaires.

Pour plus d’informations sur la façon dont cette opération de validation s’inscrit dans le processus de création d’une soumission de version d’évaluation de package à l’aide de l’API de soumission au Microsoft Store, consultez [Gérer les soumissions de versions d’évaluation de package](manage-flight-submissions.md).

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* [Créez une soumission de version d’évaluation de package ](create-a-flight-submission.md), puis [mettez à jour cette soumission](update-a-flight-submission.md) avec les éventuelles modifications nécessaires apportées aux données de soumission.

## <a name="request"></a>Demande

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de requête                                                      |
|--------|------------------------------------------------------------------|
| PUBLIER    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit` |

### <a name="request-header"></a>En-tête de requête

| Header        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |

### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | chaîne | Obligatoire. L’ID Windows Store de l’application qui contient la soumission de version d’évaluation de package à valider. L’ID de Store pour l’application est disponible dans le centre de partenaires.  |
| flightId | chaîne | Obligatoire. ID de la version d’évaluation de package qui contient la soumission à valider. Cet ID est disponible dans les données de réponse des requêtes pour [créer une version d’évaluation du package](create-a-flight.md) ou [obtenir des versions d’évaluation du package pour une application](get-flights-for-an-app.md). Pour un vol a été créé dans le centre de partenaires, cet ID est également disponible dans l’URL de la page de vol de partenaires.  |
| submissionId | chaîne | Obligatoire. ID de la soumission à valider. Cet ID est disponible dans les données de réponse des requêtes pour [créer une soumission de version d’évaluation de package](create-a-flight-submission.md). Pour la soumission qui a été créée dans le centre de partenaires, cet ID est également disponible dans l’URL de la page d’envoi dans l’espace partenaires.  |

### <a name="request-body"></a>Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment valider une soumission de version d’évaluation de package.

```json
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

| Value      | Type   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | chaîne  | État de la soumission. Les valeurs possibles sont les suivantes : <ul><li>Aucune</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publication</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>  |

## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description   |
|--------|------------------|
| 400  | Les paramètres de la requête ne sont pas valides. |
| 404  | La soumission spécifiée est introuvable. |
| 409  | La soumission spécifiée a été trouvée, mais il ne peut pas être validée dans son état actuel, ou l’application utilise une fonctionnalité de partenaires est [actuellement ne pas pris en charge par l’API de soumission de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les envois de vol de package](manage-flight-submissions.md)
* [Obtenir une soumission de vol de package](get-a-flight-submission.md)
* [Créer une soumission de vol de package](create-a-flight-submission.md)
* [Mettre à jour une soumission de vol de package](update-a-flight-submission.md)
* [Supprimer l’envoi de vol d’un package](delete-a-flight-submission.md)
* [Obtenir l’état de l’envoi de vol d’un package](get-status-for-a-flight-submission.md)
