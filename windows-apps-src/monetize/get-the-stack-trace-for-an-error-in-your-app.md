---
ms.assetid: b556a245-6359-4ddc-a4bd-76f9873ab694
description: Utilisez cette méthode dans l’API d’analyse du Microsoft Store pour obtenir la trace de pile concernant une erreur dans votre app.
title: Obtenir la trace de pile concernant une erreur dans votre application
ms.date: 06/05/2018
ms.topic: article
keywords: windows 10, uwp, services du Store, API d’analyse du Microsoft Store, trace de pile, erreur
ms.localizationpriority: medium
ms.openlocfilehash: ceffc7622f756eb17c8475208852e013df814554
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627454"
---
# <a name="get-the-stack-trace-for-an-error-in-your-app"></a>Obtenir la trace de pile concernant une erreur dans votre application

Utilisez cette méthode dans l’API d’analyse du Microsoft Store pour obtenir la trace de pile concernant une erreur dans votre app. Cette méthode ne peut télécharger que la trace de pile concernant une erreur d’application survenue dans les 30 derniers jours. Traces de pile sont également disponibles dans le **échecs** section de la [rapport d’intégrité](../publish/health-report.md) dans Partner Center.

Pour utiliser cette méthode, vous devez d’abord utiliser la méthode [Obtenir les détails sur une erreur dans votre application](get-details-for-an-error-in-your-app.md) pour récupérer l’ID du fichier CAB associé à l’erreur dont vous voulez récupérer la trace de pile.

## <a name="prerequisites"></a>Conditions préalables


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Obtenez l’ID du fichier CAB associé à l’erreur dont vous voulez récupérer la trace de pile. Pour obtenir cet ID, utilisez la méthode [Obtenir les détails d’une erreur dans votre application](get-details-for-an-error-in-your-app.md) pour récupérer les détails d’une erreur spécifique dans votre application, en spécifiant la valeur **cabId** dans le corps de la réponse.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/stacktrace``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Obligatoire  |
|---------------|--------|---------------|------|
| applicationId | chaîne | ID Windows Store de l’application dont vous souhaitez obtenir la trace de pile. L’ID de Store est disponible sur le [page identité des applications](../publish/view-app-identity-details.md) dans Partner Center. Exemple d’ID Windows Store : 9WZDNCRFJ3Q8. |  Oui  |
| cabId | chaîne | ID unique du fichier CAB associé à l’erreur dont vous voulez récupérer la trace de pile. Pour obtenir cet ID, utilisez la méthode [Obtenir les détails d’une erreur dans votre application](get-details-for-an-error-in-your-app.md) pour récupérer les détails d’une erreur spécifique dans votre application, en spécifiant la valeur **cabId** dans le corps de la réponse. |  Oui  |

 
### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment obtenir la trace de pile avec cette méthode. Remplacez la valeur *applicationId* par l’ID Windows Store de votre application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/stacktrace?applicationId=9NBLGGGZ5QDR&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type    | Description                  |
|------------|---------|--------------------------------|
| Valeur      | tableau   | Tableau d’objets qui contiennent chacun une image de données de trace de pile. Pour plus d’informations sur les données de chaque objet, consultez la section [Valeurs de la trace de pile](#stack-trace-values) ci-dessous. |
| @nextLink  | chaîne  | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur 10, mais que plus de 10 lignes d’erreur sont associées à la requête. |
| TotalCount | Entier | Nombre total de lignes dans les résultats de la requête.          |


### <a name="stack-trace-values"></a>Valeurs de la trace de pile

Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur           | Type    | Description      |
|-----------------|---------|----------------|
| level            | chaîne  |  Numéro de l’image représentant cet élément dans la pile d’appels.  |
| image   | chaîne  |   Nom du fichier exécutable ou de l’image de bibliothèque qui contient la fonction appelée dans cette image de pile.           |
| function | chaîne  |  Nom de la fonction appelée dans cette image de pile. Cette fonctionnalité n’est disponible que si votre application contient des symboles pour le fichier exécutable ou la bibliothèque.              |
| offset     | chaîne  |  Décalage d’octet de l’instruction actuelle par rapport au début de la fonction.      |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "level": "0",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.MainPage.DoWork",
      "offset": "0x25C"
    }
    {
      "level": "1",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.MainPage.Initialize",
      "offset": "0x26"
    }
    {
      "level": "2",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.Start",
      "offset": "0x66"
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}

```

## <a name="related-topics"></a>Rubriques connexes

* [Rapport d’intégrité](../publish/health-report.md)
* [Accéder aux données d’analytique à l’aide des services de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des données de signalement d’erreurs](get-error-reporting-data.md)
* [Obtenir les détails d’une erreur dans votre application](get-details-for-an-error-in-your-app.md)
* [Télécharger le fichier CAB pour une erreur dans votre application](download-the-cab-file-for-an-error-in-your-app.md)
