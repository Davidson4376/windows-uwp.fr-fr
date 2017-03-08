---
author: mcleanbyron
ms.assetid: D677E126-C3D6-46B6-87A5-6237EBEDF1A9
description: "Utilisez cette méthode de l’API de soumission du Windows Store pour supprimer une soumission d’extension existante."
title: "Supprimer une soumission d’extension à l’aide de l’API de soumission du Windows Store"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, API de soumission du Windows Store, soumission d’extension, supprimer, extension"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 117ed98dc961231cdbc7258b1097b7e0452efe35
ms.lasthandoff: 02/07/2017

---

# <a name="delete-an-add-on-submission-using-the-windows-store-submission-api"></a>Supprimer une soumission d’extension à l’aide de l’API de soumission du Windows Store




Utilisez cette méthode de l’API de soumission du Windows Store pour supprimer une extension existante (également connue sous le nom PIA ou produit in-app).

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission du Windows Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

>**Remarque**&nbsp;&nbsp;Cette méthode ne peut être utilisée que pour les comptes du Centre de développement Windows qui ont reçu l’autorisation d’utiliser l’API de soumission du Windows Store. Tous les comptes ne bénéficient pas de cette autorisation.

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de requête                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}``` |

<span/>
 

### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |

<span/>

### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | chaîne | Obligatoire. ID Windows Store de l’extension qui contient la soumission à supprimer. L’ID Windows Store est disponible dans le tableau de bord du Centre de développement.  |
| submissionId | chaîne | Obligatoire. ID de la soumission à supprimer. Cet ID est disponible dans le tableau de bord du Centre de développement et figure également dans les données de réponse aux requêtes visant à [créer une soumission d’extension](create-an-add-on-submission.md).  |

<span/>

### <a name="request-body"></a>Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.

<span/>

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
| 409  | La soumission spécifiée a été trouvée, mais elle n’a pas pu être supprimée en raison de son état actuel, ou l’extension utilise une fonctionnalité du tableau de bord du Centre de développement qui n’est [actuellement pas prise en charge par l’API de soumission du Windows Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |

<span/>

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obtenir une soumission d’extension](get-an-add-on-submission.md)
* [Créer une soumission d’extension](create-an-add-on-submission.md)
* [Valider une soumission d’extension](commit-an-add-on-submission.md)
* [Mettre à jour une soumission d’extension](update-an-add-on-submission.md)
* [Obtenir l’état d’une soumission d’extension](get-status-for-an-add-on-submission.md)

