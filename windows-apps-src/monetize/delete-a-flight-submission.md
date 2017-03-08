---
author: mcleanbyron
ms.assetid: 1A69A388-B1CC-4D2C-886B-EA07E6E60252
description: "Utilisez cette méthode de l’API de soumission du Windows Store pour supprimer une soumission de version d’évaluation du package existante."
title: "Supprimer une soumission de version d’évaluation du package à l’aide de l’API de soumission du Windows Store"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, API de soumission du Windows Store, soumission de version d’évaluation, supprimer, version d’évaluation du package"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: e720b8551949335faa0407cbc159c217d4b91343
ms.lasthandoff: 02/07/2017

---

# <a name="delete-a-package-flight-submission-using-the-windows-store-submission-api"></a>Supprimer une soumission de version d’évaluation du package à l’aide de l’API de soumission du Windows Store




Utilisez cette méthode de l’API de soumission du Windows Store pour supprimer une soumission de version d’évaluation du package existante.

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission du Windows Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

>**Remarque**&nbsp;&nbsp;Cette méthode ne peut être utilisée que pour les comptes du Centre de développement Windows qui ont reçu l’autorisation d’utiliser l’API de soumission du Windows Store. Tous les comptes ne bénéficient pas de cette autorisation.

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de requête                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/flights/{flightId}/submissions/{submissionId}``` |

<span/>
 

### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |

<span/>

### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | chaîne | Obligatoire. ID Windows Store de l’application qui contient la soumission de version d’évaluation du package à supprimer. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | chaîne | Obligatoire. ID de la version d’évaluation du package qui contient la soumission à supprimer. Cet ID est disponible dans le tableau de bord du Centre de développement et figure également dans les données de réponse aux requêtes visant à [créer une version d’évaluation du package](create-a-flight.md) ou à [obtenir des versions d’évaluation du package pour une application](get-flights-for-an-app.md).  |
| submissionId | chaîne | Obligatoire. ID de la soumission à supprimer. Cet ID est disponible dans le tableau de bord du Centre de développement et figure également dans les données de réponse aux requêtes visant à [créer une soumission de version d’évaluation du package](create-a-flight-submission.md).  |

<span/>

### <a name="request-body"></a>Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.

<span/>

### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment supprimer une soumission de version d’évaluation du package.

```
DELETE https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

En cas de succès, cette méthode retourne un corps de réponse vide.

## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description   |
|--------|------------------|
| 400  | Les paramètres de la requête ne sont pas valides. |
| 404  | La soumission spécifiée est introuvable. |
| 409  | La soumission spécifiée a été trouvée, mais elle n’a pas pu être supprimée en raison de son état actuel, ou l’application utilise une fonctionnalité du tableau de bord du Centre de développement qui n’est [actuellement pas prise en charge par l’API de soumission du Windows Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |

<span/>

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les soumissions de versions d’évaluation de package](manage-flight-submissions.md)
* [Obtenir une soumission de version d’évaluation du package](get-a-flight-submission.md)
* [Créer une soumission de version d’évaluation du package](create-a-flight-submission.md)
* [Valider une soumission de version d’évaluation de package](commit-a-flight-submission.md)
* [Mettre à jour une soumission de version d’évaluation du package](update-a-flight-submission.md)
* [Obtenir l’état d’une soumission de version d’évaluation du package](get-status-for-a-flight-submission.md)

