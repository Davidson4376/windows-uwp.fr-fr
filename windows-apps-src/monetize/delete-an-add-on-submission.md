---
author: Xansky
ms.assetid: D677E126-C3D6-46B6-87A5-6237EBEDF1A9
description: Utilisez cette méthode de l’API de soumission au MicrosoftStore pour supprimer une soumission d’extension existante.
title: Supprime une soumission d’extension
ms.author: mhopkins
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, API de soumission au MicrosoftStore, soumission d’extension, supprimer, produit in-app, PIA
ms.localizationpriority: medium
ms.openlocfilehash: ca534cfc7c38dba9d77749e17f15dd66766de7ca
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7162981"
---
# <a name="delete-an-add-on-submission"></a>Supprime une soumission d’extension

Utilisez cette méthode de l’API de soumission au MicrosoftStore pour supprimer une extension existante (également connue sous le nom PIA ou produit in-app).

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de la requête                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | chaîne | Obligatoire. ID Windows Store de l’extension qui contient la soumission à supprimer. L’ID Windows Store est disponible dans l’espace partenaires.  |
| submissionId | chaîne | Obligatoire. ID de la soumission à supprimer. Cet ID est disponible dans les données de réponse des requêtes pour [créer une soumission d’extension](create-an-add-on-submission.md). Pour une soumission qui a été créée dans l’espace partenaires, cet ID est également disponible dans l’URL de la page de la soumission dans l’espace partenaires.  |


### <a name="request-body"></a>Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.


### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment supprimer une soumission d’extension.

```
DELETE https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621230023 HTTP/1.1
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
| 409  | La soumission spécifiée a été trouvée, mais il ne peut pas être supprimé dans son état actuel, ou l’extension utilise une fonctionnalité de l’espace partenaires qui n’est [actuellement pas pris en charge par l’API de soumission au Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |


## <a name="related-topics"></a>Rubriques associées

* [Créer et gérer des soumissions à l’aide des services du MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md)
* [Obtenir une soumission de module complémentaire](get-an-add-on-submission.md)
* [Créer une soumission d’extension](create-an-add-on-submission.md)
* [Valider une soumission d’extension](commit-an-add-on-submission.md)
* [Mettre à jour une soumission d’extension](update-an-add-on-submission.md)
* [Obtenir l’état d’une soumission d’extension](get-status-for-an-add-on-submission.md)
