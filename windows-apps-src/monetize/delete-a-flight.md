---
author: mcleanbyron
ms.assetid: AD80F9B3-CED0-40BD-A199-AB81CDAE466C
description: "Utilisez cette méthode dans l’API de soumission du Windows Store pour supprimer une version d’évaluation du package pour une application inscrite dans votre compte du Centre de développement Windows."
title: "Supprimer une version d’évaluation du package à l’aide de l’API de soumission du Windows Store"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: eae25f9a19523b928e30baaf0ffe0eec6e779ed2

---

# Supprimer une version d’évaluation du package à l’aide de l’API de soumission du Windows Store




Utilisez cette méthode dans l’API de soumission du Windows Store pour supprimer une version d’évaluation du package pour une application inscrite dans votre compte du Centre de développement Windows.


## Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission du Windows Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

>**Remarque**  Cette méthode ne peut être utilisée que pour les comptes du Centre de développement Windows qui ont reçu l’autorisation d’utiliser l’API de soumission du Windows Store. Tous les comptes ne bénéficient pas de cette autorisation.

## Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de requête                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` |

<span/>
 

### En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |

<span/>

### Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | chaîne | Obligatoire. ID Windows Store de l’application qui contient la version d’évaluation du package à supprimer. L’ID Windows Store de l’application est disponible dans le tableau de bord du Centre de développement.  |
| flightId | chaîne | Obligatoire. ID de la version d’évaluation du package à supprimer. Cet ID est disponible dans le tableau de bord du Centre de développement et figure également dans les données de réponse aux requêtes visant à [créer une version d’évaluation du package](create-a-flight.md) ou à [obtenir des versions d’évaluation du package pour une application](get-flights-for-an-app.md).  |

<span/>

### Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.

<span/>

### Exemple de requête

L’exemple suivant montre comment supprimer une version d’évaluation du package.

```
DELETE https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd HTTP/1.1
Authorization: Bearer <your access token>
```

## Réponse

En cas de succès, cette méthode retourne un corps de réponse vide.

## Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description                                                                                                                                                                           |
|--------|------------------|
| 400  | Les paramètres de la requête ne sont pas valides. |
| 404  | La version d’évaluation du package spécifiée est introuvable.  |
| 409  | La version d’évaluation du package spécifiée a été trouvée, mais elle n’a pas pu être supprimée en raison de son état actuel, ou l’application utilise une fonctionnalité du tableau de bord du Centre de développement qui n’est [actuellement pas prise en charge par l’API de soumission du Windows Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

<span/>

## Rubriques connexes

* [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Créer une version d’évaluation du package](create-a-flight.md)
* [Obtenir une version d’évaluation du package](get-a-flight.md)



<!--HONumber=Aug16_HO5-->


