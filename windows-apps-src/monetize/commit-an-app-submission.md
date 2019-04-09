---
ms.assetid: 934F2DBF-2C7E-4B77-997D-17B9B0535D51
description: Utilisez cette méthode dans l’API de soumission de Microsoft Store pour valider une soumission d’application nouvelle ou mise à jour pour les partenaires.
title: Valider une soumission d’application
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, API de soumission au Microsoft Store, valider une soumission d'applications
ms.localizationpriority: medium
ms.openlocfilehash: 9e44f5672c817f9e1ab00df341a2fd78b23f2944
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334147"
---
# <a name="commit-an-app-submission"></a>Valider une soumission d’application

Utilisez cette méthode dans l’API de soumission de Microsoft Store pour valider une soumission d’application nouvelle ou mise à jour pour les partenaires. La validation alertes partenaire centre de maintenance que les données de soumission a été chargées (y compris les packages associés et des images). En réponse, partenaires valide les modifications de données de soumission pour l’ingestion et la publication. Une fois l’opération de validation réussit, les modifications apportées à la soumission figurent dans les partenaires.

Pour plus d’informations sur la façon dont l’opération de validation s’inscrit dans le processus de soumission d’une app à l’aide de l’API de soumission au Microsoft Store, voir [Gérer les soumissions d’apps](manage-app-submissions.md).

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* [Créez une soumission d’application](create-an-app-submission.md), puis [mettez à jour cette soumission](update-an-app-submission.md) avec les éventuelles modifications nécessaires apportées aux données de soumission.

## <a name="request"></a>Demande

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de requête                                                      |
|--------|------------------------------------------------------------------|
| PUBLIER    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit` |


### <a name="request-header"></a>En-tête de requête

| Header        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | chaîne | Obligatoire. L’ID Windows Store de l’application qui contient la soumission à valider. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| submissionId | chaîne | Obligatoire. ID de la soumission à valider. Cet ID est disponible dans les données de réponse des requêtes pour [créer une soumission d’apps](create-an-app-submission.md). Pour la soumission qui a été créée dans le centre de partenaires, cet ID est également disponible dans l’URL de la page d’envoi dans l’espace partenaires.  |

### <a name="request-body"></a>Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment valider une soumission de requête.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243610/commit HTTP/1.1
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
* [Obtenir une soumission de l’application](get-an-app-submission.md)
* [Créer une soumission de l’application](create-an-app-submission.md)
* [Mise à jour une soumission de l’application](update-an-app-submission.md)
* [Supprimer une soumission de l’application](delete-an-app-submission.md)
* [Obtenir l’état d’une soumission de l’application](get-status-for-an-app-submission.md)
