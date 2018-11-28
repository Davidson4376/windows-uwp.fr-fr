---
ms.assetid: 16D4C3B9-FC9B-46ED-9F87-1517E1B549FA
description: Utilisez cette méthode dans l’API de soumission au Microsoft Store pour supprimer une extension pour une application inscrite dans votre compte espace partenaires.
title: Supprime une extension
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, API de soumission au MicrosoftStore, extension, supprimer, produit in-app, PIA
ms.localizationpriority: medium
ms.openlocfilehash: 837cbc19268a88be986068f4a5e60002a1eb55e2
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7850780"
---
# <a name="delete-an-add-on"></a>Supprime une extension

Utilisez cette méthode dans l’API de soumission au Microsoft Store pour supprimer une extension (également connue sous le nom dans l’application PIA, produit) pour une application inscrite dans votre compte espace partenaires.

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de la requête                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| id | chaîne | Obligatoire. ID Windows Store de l’extension à supprimer. L’ID Windows Store est disponible dans l’espace partenaires.  |


### <a name="request-body"></a>Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.


### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment supprimer une extension.

```
DELETE https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

En cas de succès, cette méthode retourne un corps de réponse vide.

## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description                                                                                                                                                                           |
|--------|------------------|
| 400  | La requête n’est pas valide. |
| 404  | L’extension spécifiée est introuvable.  |
| 409  | L’extension spécifiée a été trouvée, mais il ne peut pas être supprimé dans son état actuel, ou l’extension utilise une fonctionnalité de l’espace partenaires qui n’est [actuellement pas pris en charge par l’API de soumission au Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Rubriques associées

* [Créer et gérer des soumissions à l’aide des services du MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md)
* [Obtenir toutes les extensions](get-all-add-ons.md)
* [Obtenir une extension](get-an-add-on.md)
* [Créer une extension](create-an-add-on.md)
