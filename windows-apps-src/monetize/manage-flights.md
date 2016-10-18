---
author: mcleanbyron
ms.assetid: 37F2C162-4910-4336-BEED-8536C88DCA65
description: "Utilisez ces méthodes dans l’API de soumission du Windows Store pour gérer les versions d’évaluation de package pour les applications inscrites dans votre compte du Centre de développement Windows."
title: "Gérer les versions d’évaluation du package à l’aide de l’API de soumission du Windows Store"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: de1c23d721cee67d813520e3a23eb553cd90b7e9

---

# Gérer les versions d’évaluation du package à l’aide de l’API de soumission du Windows Store




Utilisez les méthodes suivantes dans l’API de soumission du Windows Store pour gérer les versions d’évaluation de package pour les applications qui sont inscrites dans votre compte du Centre de développement Windows. Pour obtenir une présentation de l’API de soumission du Windows Store, notamment les conditions préalables à l’utilisation de l’API, voir [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md).

>**Remarque**&nbsp;&nbsp;Ces méthodes ne peuvent être utilisées que pour les comptes du Centre de développement Windows qui ont reçu l’autorisation d’utiliser l’API de soumission du Windows Store. Tous les comptes ne bénéficient pas de cette autorisation. Ces méthodes peuvent uniquement être utilisées pour obtenir, créer ou supprimer des versions d’évaluation de package. Pour créer des soumissions pour des versions d’évaluation de package, voir les méthodes présentées dans l’article [Gérer les soumissions de version d’évaluation de package](manage-flight-submissions.md).

| Méthode        | URI    | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` | Obtient les données relatives à une version d’évaluation du package pour une application inscrite dans votre compte du Centre de développement Windows. Pour plus d’informations, voir [Obtenir une version d’évaluation de package](get-a-flight.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights``` | Crée une version d’évaluation de package. Pour plus d’informations, voir [Créer une version d’évaluation de package](create-a-flight.md).|
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` | Supprime une version d’évaluation de package. Pour plus d’informations, voir [Supprimer une version d’évaluation de package](delete-a-flight.md). |


## Conditions préalables

Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission du Windows Store avant d’essayer d’utiliser l’une de ces méthodes.

## Rubriques connexes

* [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les soumissions de versions d’évaluation de package](manage-flight-submissions.md)



<!--HONumber=Aug16_HO5-->


