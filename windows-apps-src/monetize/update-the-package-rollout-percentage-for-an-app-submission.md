---
author: Xansky
description: Utilisez cette méthode dans l’API de soumission au Microsoft Store pour mettre à jour le pourcentage de déploiement du package pour une soumission d’applications.
title: Met à jour le pourcentage de lancement d’une soumission d'application
ms.author: mhopkins
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, API de soumission au Microsoft Store, lancement de packages, soumission d’applications, mise à jour, pourcentage
ms.assetid: 4c82d837-7a25-4f3a-997e-b7be33b521cc
ms.localizationpriority: medium
ms.openlocfilehash: ee7657a1ebd08e70e6b5dac8a9a723637539066e
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6968938"
---
# <a name="update-the-rollout-percentage-for-an-app-submission"></a>Met à jour le pourcentage de lancement d’une soumission d'application


Utilisez cette méthode dans l’API de soumission au Microsoft Store pour [mettre à jour le pourcentage de déploiement](../publish/gradual-package-rollout.md#setting-the-rollout-percentage) d’une soumission d’applications. Pour plus d’informations sur le processus de création d’une soumission d’applications à l’aide de l’API de soumission au Microsoft Store, voir [Gérer les soumissions d’applications](manage-app-submissions.md).


## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Créer une soumission pour une de vos applications. Vous pouvez le faire dans l’espace partenaires, ou vous pouvez le faire à l’aide de la méthode de [créer une soumission d’application](create-an-app-submission.md) .
* Autorisez un déploiement de package progressif pour la soumission. Vous pouvez le faire dans la zone [dans l’espace partenaires](../publish/gradual-package-rollout.md), ou vous pouvez le faire à [l’aide de l’API de soumission au Microsoft Store](manage-app-submissions.md#manage-gradual-package-rollout).

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et des paramètres de la requête.

| Méthode | URI de la requête                                                      |
|--------|------------------------------------------------------------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/updatepackagerolloutpercentage``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | chaîne | Obligatoire. ID Windows Store de l’application qui contient la soumission avec le pourcentage de déploiement du package que vous voulez mettre à jour. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| submissionId | chaîne | Obligatoire. ID de la soumission avec le pourcentage de déploiement du package que vous voulez mettre à jour. Cet ID est disponible dans les données de réponse des requêtes pour [créer une soumission d’applications](create-an-app-submission.md). Pour une soumission qui a été créée dans l’espace partenaires, cet ID est également disponible dans l’URL de la page de la soumission dans l’espace partenaires.   |
| pourcentage  |  flottant  |  Obligatoire. Le pourcentage d’utilisateurs qui recevront le package de déploiement progressif.  |


### <a name="request-body"></a>Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment mettre à jour le pourcentage de déploiement du package d’une soumission d’application.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243680/updatepackagerolloutpercentage?percentage=25 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Pour plus d’informations sur les valeurs figurant dans le corps de la réponse, voir la [ressource de déploiement du package](manage-app-submissions.md#package-rollout-object).

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 25.0,
    "packageRolloutStatus": "PackageRolloutInProgress",
    "fallbackSubmissionId": "1212922684621243058"
}
```

## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description   |
|--------|------------------|
| 404  | La soumission est introuvable. |
| 409  | Ce code indique l’une des erreurs suivantes:<br/><br/><ul><li>La soumission n’est pas dans un état valide pour l’opération de déploiement progressif (avant d’appeler cette méthode, la soumission doit être publiée et la valeur [packageRolloutStatus](manage-app-submissions.md#package-rollout-object) doit être définie sur **PackageRolloutInProgress**).</li><li>La soumission n’appartient pas à l’application spécifiée.</li><li>L’application utilise une fonctionnalité de l’espace partenaires qui n’est [actuellement pas pris en charge par l’API de soumission au Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).</li></ul> |   


## <a name="related-topics"></a>Rubriques associées

* [Lancement progressif de packages](../publish/gradual-package-rollout.md)
* [Gérer les soumissions d’apps à l’aide de l’API de soumission au MicrosoftStore](manage-app-submissions.md)
* [Créer et gérer des soumissions à l’aide des services du MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md)
