---
description: Utilisez cette méthode dans l’API de soumission au MicrosoftStore pour finaliser le lancement du package pour une soumission d’app.
title: Finalise le lancement d’une soumission d’applications
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, API de soumission au MicrosoftStore, lancement du package, soumission d'application, finaliser
ms.assetid: c7dd39e6-5162-455a-b03b-1ed76bffcf6e
ms.localizationpriority: medium
ms.openlocfilehash: c8fe211268190ac269018a6bd47acb4b824d2075
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8486818"
---
# <a name="finalize-the-rollout-for-an-app-submission"></a>Finalise le lancement d’une soumission d’applications


Utilisez cette méthode dans l’API de soumission au MicrosoftStore pour [finaliser le lancement du package](../publish/gradual-package-rollout.md#completing-the-rollout) pour une soumission d’app. Pour plus d’informations sur le processus de création d’une soumission d’apps à l’aide de l’API de soumission au MicrosoftStore, voir [Gérer les soumissions d’apps](manage-app-submissions.md).

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Créer une soumission pour une application dans votre compte espace partenaires. Vous pouvez le faire dans l’espace partenaires, ou vous pouvez le faire à l’aide de la méthode de [créer une soumission d’application](create-an-app-submission.md) .
* Autorisez un déploiement de package progressif pour la soumission. Vous pouvez effectuer cette [dans l’espace partenaires](../publish/gradual-package-rollout.md), ou vous pouvez le faire à [l’aide de l’API de soumission au Microsoft Store](manage-app-submissions.md#manage-gradual-package-rollout).

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et des paramètres de la requête.

| Méthode | URI de la requête                                                      |
|--------|------------------------------------------------------------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/finalizepackagerollout``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | chaîne | Obligatoire. ID Windows Store de l’application qui contient la soumission avec le lancementdu package que vous voulez finaliser. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| submissionId | chaîne | Obligatoire. ID de la soumission avec le lancement du package que vous voulez finaliser. Cet ID est disponible dans les données de réponse des requêtes pour [créer une soumission d’apps](create-an-app-submission.md). Pour une soumission qui a été créée dans l’espace partenaires, cet ID est également disponible dans l’URL de la page de soumission dans l’espace partenaires.  |


### <a name="request-body"></a>Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment finaliser le lancement du package d’une soumission de version d’évaluation de package.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243680/finalizepackagerollout HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Pour plus d’informations sur les valeurs figurant dans le corps de la réponse, voir la [ressource de déploiement du package](manage-app-submissions.md#package-rollout-object).

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 100.0,
    "packageRolloutStatus": "PackageRolloutComplete",
    "fallbackSubmissionId": "1212922684621243058"
}
```


## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description   |
|--------|------------------|
| 404  | La soumission est introuvable. |
| 409  | Ce code indique l’une des erreurs suivantes:<br/><br/><ul><li>La soumission n’est pas dans un état valide pour l’opération de déploiement progressif (avant d’appeler cette méthode, la soumission doit être publiée et la valeur [packageRolloutStatus](manage-app-submissions.md#package-rollout-object) doit être définie sur **PackageRolloutInProgress**).</li><li>La soumission n’appartient pas à l’application spécifiée.</li><li>L’application utilise une fonctionnalité DevPartner Center qui n’est [actuellement pas prise en charge par l’API de soumission au Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).</li></ul> |   


## <a name="related-topics"></a>Rubriques associées

* [Lancement progressif de packages](../publish/gradual-package-rollout.md)
* [Gérer les soumissions d’apps à l’aide de l’API de soumission au MicrosoftStore](manage-app-submissions.md)
* [Créer et gérer des soumissions à l’aide des services du MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md)
