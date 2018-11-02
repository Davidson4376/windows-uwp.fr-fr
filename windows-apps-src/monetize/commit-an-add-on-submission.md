---
author: Xansky
ms.assetid: AC74B4FA-5554-4C03-9683-86EE48546C05
description: Utilisez cette méthode dans l’API de soumission au MicrosoftStore pour valider une soumission d’extension nouvelle ou mise à jour à destination du Centre de développement Windows.
title: Valider une soumission d’extension
ms.author: mhopkins
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, API de soumission au MicrosoftStore, valider une soumission d’extension, produit in-app, PIA
ms.localizationpriority: medium
ms.openlocfilehash: 52dce19410741c0ac7b006b14d572ec7280a5e2c
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5926246"
---
# <a name="commit-an-add-on-submission"></a>Valider une soumission d’extension

Utilisez cette méthode dans l’API de soumission au MicrosoftStore pour valider une soumission d’extension (également connue sous le nom PIA, produit in-app) nouvelle ou mise à jour à destination du Centre de développement Windows. L’action de validation alerte le Centre de développement que les données de soumission ont été chargées sur le serveur (y compris les icônes associées, le cas échéant). En réponse, le Centre de développement valide les modifications apportées aux données de soumission en vue de leur intégration et publication. Dès lors que l’opération de validation a abouti, les modifications apportées à la soumission s’affichent dans le tableau de bord du Centre de développement.

Pour plus d’informations sur la façon dont l’opération de validation s’inscrit dans le processus de soumission d’une extension à l’aide de l’API de soumission au MicrosoftStore, consultez [Gérer les soumissions d’extensions](manage-add-on-submissions.md).

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* [Créez une soumission d’extension](create-an-add-on-submission.md), puis [mettez à jour cette soumission](update-an-add-on-submission.md) avec les éventuelles modifications nécessaires apportées aux données de soumission.

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de la requête                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}/commit``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | chaîne | Obligatoire. ID Windows Store de l’extension qui contient la soumission à valider. L’ID Windows Store est disponible dans le tableau de bord du Centre de développement et figure également dans les données de réponse des requêtes pour [obtenir toutes les extensions](get-all-add-ons.md) et [créer une extension](create-an-add-on.md). |
| submissionId | chaîne | Obligatoire. ID de la soumission à valider. Cet ID est disponible dans les données de réponse des requêtes pour [créer une soumission d’extension](create-an-add-on-submission.md). Concernant une soumission qui a été créée dans le tableau de bord du centre de développement, cet ID est également disponible dans l’URL de la page de la soumission dans le tableau de bord.  |


### <a name="request-body"></a>Corps de requête

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
| status           | chaîne  | État de la soumission. Les valeurs possibles sont les suivantes: <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>  |


## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description   |
|--------|------------------|
| 400  | Les paramètres de la requête ne sont pas valides. |
| 404  | La soumission spécifiée est introuvable. |
| 409  | La soumission spécifiée a été trouvée, mais elle n’a pas pu être validée en raison de son état actuel, ou l’extension utilise une fonctionnalité du tableau de bord du Centre de développement qui n’est [actuellement pas prise en charge par l’API de soumission au MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md#not_supported). |


## <a name="related-topics"></a>Rubriques associées

* [Créer et gérer des soumissions à l’aide des services du MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md)
* [Obtenir une soumission de module complémentaire](get-an-add-on-submission.md)
* [Créer une soumission d’extension](create-an-add-on-submission.md)
* [Mettre à jour une soumission d’extension](update-an-add-on-submission.md)
* [Supprimer une soumission d’extension](delete-an-add-on-submission.md)
* [Obtenir l’état d’une soumission d’extension](get-status-for-an-add-on-submission.md)
