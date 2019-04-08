---
ms.assetid: AC74B4FA-5554-4C03-9683-86EE48546C05
description: Utilisez cette méthode dans l’API de soumission de Microsoft Store pour valider l’envoi d’un module complémentaire de nouveau ou mis à jour pour les partenaires.
title: Valider une soumission d’extension
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, API de soumission au Microsoft Store, valider une soumission d’extension, produit in-app, PIA
ms.localizationpriority: medium
ms.openlocfilehash: efab4412486566ae817eb66e78f5407533a30d5b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608214"
---
# <a name="commit-an-add-on-submission"></a>Valider une soumission d’extension

Utilisez cette méthode dans l’API de soumission de Microsoft Store pour valider l’envoi d’un module complémentaire nouveau ou mis à jour (également appelés dans l’application produit ou produits) pour les partenaires. La validation alertes partenaire centre de maintenance que les données de soumission a été chargées (y compris toutes les icônes associées). En réponse, partenaires valide les modifications de données de soumission pour l’ingestion et la publication. Une fois l’opération de validation réussit, les modifications apportées à la soumission figurent dans les partenaires.

Pour plus d’informations sur la façon dont l’opération de validation s’inscrit dans le processus de soumission d’une extension à l’aide de l’API de soumission au Microsoft Store, consultez [Gérer les soumissions d’extensions](manage-add-on-submissions.md).

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* [Créez une soumission d’extension](create-an-add-on-submission.md), puis [mettez à jour cette soumission](update-an-add-on-submission.md) avec les éventuelles modifications nécessaires apportées aux données de soumission.

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de requête                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}/commit``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | chaîne | Obligatoire. ID Windows Store de l’extension qui contient la soumission à valider. L’ID de Store est disponible dans le centre de partenaires, et il est inclus dans les données de réponse pour les demandes au [obtenir tous les modules complémentaires](get-all-add-ons.md) et [créer un module complémentaire](create-an-add-on.md). |
| submissionId | chaîne | Obligatoire. ID de la soumission à valider. Cet ID est disponible dans les données de réponse des requêtes pour [créer une soumission d'extension](create-an-add-on-submission.md). Pour la soumission qui a été créée dans le centre de partenaires, cet ID est également disponible dans l’URL de la page d’envoi dans l’espace partenaires.  |


### <a name="request-body"></a>Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment valider une soumission d’extension.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621230023/commit HTTP/1.1
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
| status           | chaîne  | État de la soumission. Les valeurs possibles sont les suivantes : <ul><li>Aucune</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publication</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>  |


## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description   |
|--------|------------------|
| 400  | Les paramètres de la requête ne sont pas valides. |
| 404  | La soumission spécifiée est introuvable. |
| 409  | La soumission spécifiée a été trouvée, mais il ne peut pas être validée dans son état actuel, ou le module complémentaire utilise une fonctionnalité de partenaires est [actuellement ne pas pris en charge par l’API de soumission de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |


## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obtenir une présentation du module complémentaire](get-an-add-on-submission.md)
* [Créer une soumission de module complémentaire](create-an-add-on-submission.md)
* [Mettre à jour une module complémentaire soumission](update-an-add-on-submission.md)
* [Supprimer un dépôt de module complémentaire](delete-an-add-on-submission.md)
* [Obtenir l’état d’une soumission de module complémentaire](get-status-for-an-add-on-submission.md)
