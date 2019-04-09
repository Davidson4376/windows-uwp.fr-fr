---
ms.assetid: D677E126-C3D6-46B6-87A5-6237EBEDF1A9
description: Utilisez cette méthode de l’API de soumission au Microsoft Store pour supprimer une soumission d’extension existante.
title: Supprimer une soumission d’extension
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, API de soumission au Microsoft Store, soumission d’extension, supprimer, produit in-app, PIA
ms.localizationpriority: medium
ms.openlocfilehash: 4a694846c745fbfbc175781dd76cc983e402b676
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334917"
---
# <a name="delete-an-add-on-submission"></a>Supprimer une soumission d’extension

Utilisez cette méthode de l’API de soumission au Microsoft Store pour supprimer une extension existante (également connue sous le nom PIA ou produit in-app).

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Demande

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de requête                                                      |
|--------|------------------------------------------------------------------|
| Suppression    | `https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}` |


### <a name="request-header"></a>En-tête de requête

| Header        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | chaîne | Obligatoire. ID Windows Store de l’extension qui contient la soumission à supprimer. L’ID de Store est disponible dans le centre de partenaires.  |
| submissionId | chaîne | Obligatoire. ID de la soumission à supprimer. Cet ID est disponible dans les données de réponse des requêtes pour [créer une soumission d'extension](create-an-add-on-submission.md). Pour la soumission qui a été créée dans le centre de partenaires, cet ID est également disponible dans l’URL de la page d’envoi dans l’espace partenaires.  |


### <a name="request-body"></a>Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.


### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment supprimer une soumission d’extension.

```json
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
| 409  | La soumission spécifiée a été trouvée, mais il ne peut pas être supprimé dans son état actuel, ou le module complémentaire utilise une fonctionnalité de partenaires est [actuellement ne pas pris en charge par l’API de soumission de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |


## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obtenir une présentation du module complémentaire](get-an-add-on-submission.md)
* [Créer une soumission de module complémentaire](create-an-add-on-submission.md)
* [Valider une soumission de module complémentaire](commit-an-add-on-submission.md)
* [Mettre à jour une module complémentaire soumission](update-an-add-on-submission.md)
* [Obtenir l’état d’une soumission de module complémentaire](get-status-for-an-add-on-submission.md)
