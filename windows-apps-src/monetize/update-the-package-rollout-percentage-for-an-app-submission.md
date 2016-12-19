---
author: mcleanbyron
description: "Utilisez cette méthode dans l’API de soumission du Windows Store pour mettre à jour le pourcentage de déploiement du package pour une soumission d’application."
title: "Mettre à jour le pourcentage de déploiement du package pour une soumission d’application à l’aide de l’API de soumission du Windows Store"
translationtype: Human Translation
ms.sourcegitcommit: 9b76a11adfab838b21713cb384cdf31eada3286e
ms.openlocfilehash: bae825e0fecda32df770a1f48038c2eb652a05cc

---

# Mettre à jour le pourcentage de déploiement du package pour une soumission d’application à l’aide de l’API de soumission du Windows Store


Utilisez cette méthode dans l’API de soumission du Windows Store pour [mettre à jour le pourcentage de déploiement](../publish/gradual-package-rollout.md#setting-the-rollout-percentage) d’une soumission d’application. Pour plus d’informations sur le processus de création d’une soumission d’application à l’aide de l’API de soumission du Windows Store, voir [Gérer les soumissions d’applications](manage-app-submissions.md).


## Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission du Windows Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Créez une soumission pour une application dans votre compte du Centre de développement. Pour cela, vous pouvez utiliser le tableau de bord du Centre de développement ou la méthode [Créer une soumission d’application](create-an-app-submission.md).
* Autorisez un déploiement de package progressif pour la soumission. Pour cela, vous pouvez utiliser le [tableau de bord du Centre de développement](manage-app-submissions.md#manage-gradual-package-rollout) ou [l’API de soumission du Windows Store](../publish/gradual-package-rollout.md).

>**Remarque**  Cette méthode ne peut être utilisée que pour les comptes du Centre de développement Windows qui ont reçu l’autorisation d’utiliser l’API de soumission du Windows Store. Tous les comptes ne bénéficient pas de cette autorisation.

## Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et des paramètres de la requête.

| Méthode | URI de la requête                                                      |
|--------|------------------------------------------------------------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/updatepackagerolloutpercentage``` |

<span/>
 

### En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |

<span/>

### Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | chaîne | Obligatoire. ID Windows Store de l’application qui contient la soumission avec le pourcentage de déploiement du package que vous voulez mettre à jour. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| submissionId | chaîne | Obligatoire. ID de la soumission avec le pourcentage de déploiement du package que vous voulez mettre à jour. Cet ID est disponible dans le tableau de bord du Centre de développement et figure également dans les données de réponse aux requêtes visant à [créer une soumission d’application](create-an-app-submission.md).  |
| percentage  |  flottant  |  Obligatoire. Le pourcentage d’utilisateurs qui recevront le package de déploiement progressif.  |

<span/>

### Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.

### Exemple de requête

L’exemple suivant montre comment mettre à jour le pourcentage de déploiement du package d’une soumission d’application.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243680/updatepackagerolloutpercentage?percentage=25 HTTP/1.1
Authorization: Bearer <your access token>
```

## Réponse

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Pour plus d’informations sur les valeurs figurant dans le corps de la réponse, voir la [ressource de déploiement du package](manage-app-submissions.md#package-rollout-object).

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 25,
    "packageRolloutStatus": "PackageRolloutInProgress",
    "fallbackSubmissionId": "1212922684621243058"
}
```

## Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description   |
|--------|------------------|
| 404  | La soumission est introuvable. |
| 409  | Ce code indique l’une des erreurs suivantes :<br/><br/><ul><li>La soumission n’est pas dans un état valide pour l’opération de déploiement progressif (avant d’appeler cette méthode, la soumission doit être publiée et la valeur [packageRolloutStatus](manage-app-submissions.md#package-rollout-object) doit être définie sur **PackageRolloutInProgress**).</li><li>La soumission n’appartient pas à l’application spécifiée.</li><li>L’application utilise une fonctionnalité du tableau de bord du Centre de développement qui n’est [actuellement pas prise en charge par l’API de soumission du Windows Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).</li></ul> |   

<span/>


## Rubriques connexes

* [Déploiement de package progressif](../publish/gradual-package-rollout.md)
* [Gérer les soumissions d’applications à l’aide de l’API de soumission du Windows Store](manage-app-submissions.md)
* [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md)



<!--HONumber=Nov16_HO1-->


