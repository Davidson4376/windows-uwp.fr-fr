---
author: mcleanbyron
ms.assetid: 2967C757-9D8A-4B37-8AA4-A325F7A060C5
description: "Utilisez cette méthode dans l’API d’analyse du Windows&nbsp;Store pour obtenir les avis relatifs à une plage de dates donnée, et suivant d’autres filtres facultatifs."
title: Obtenir les avis sur les applications
translationtype: Human Translation
ms.sourcegitcommit: 7d05c8953f1f50be0b388a044fe996f345d45006
ms.openlocfilehash: 49d3f3cb608f3207306af443c67b684a0ae9f319

---

# <a name="get-app-reviews"></a>Obtenir les avis sur les applications


Utilisez cette méthode dans l’API d’analyse du Windows&nbsp;Store pour obtenir, au format&nbsp;JSON, les avis relatifs à une plage de dates donnée, et suivant d’autres filtres facultatifs. Ces informations sont également disponibles dans le [rapport Avis](../publish/reviews-report.md) du tableau de bord du Centre de développement Windows.

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes&nbsp;:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Windows Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60&nbsp;minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête

### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews``` |

<span/> 

### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|---------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |

<span/> 

### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Requis  
|---------------|--------|---------------|------|
| applicationId | chaîne | L’ID Windows&nbsp;Store pour l’application pour laquelle vous souhaitez récupérer les avis. L’ID Windows&nbsp;Store est disponible dans la page [Identité de l’application](../publish/view-app-identity-details.md) du tableau de bord du Centre de développement. Exemple d’ID Windows&nbsp;Store&nbsp;: 9WZDNCRFJ3Q8. |  Oui  |
| startDate | date | Dans la plage de dates, la date de début de la récupération des avis. La valeur par défaut est la date actuelle. |  Non  |
| endDate | date | Dans la plage de dates, la date de début de la récupération des avis. La valeur par défaut est la date actuelle. |  Non  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |
| filter |chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Pour plus d’informations, voir la section [Champs de filtre](#filter-fields) ci-dessous. | Non   |
| orderby | chaîne | Instruction commandant les valeurs des données de résultats. Syntaxe&nbsp;: <em>orderby=field [order],field [order],...</em>. Le paramètre <em>field</em> peut comporter l’une des chaînes suivantes :<ul><li><strong>date</strong></li><li><strong>osVersion</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>isRevised</strong></li><li><strong>packageVersion</strong></li><li><strong>deviceModel</strong></li><li><strong>productFamily</strong></li><li><strong>deviceScreenResolution</strong></li><li><strong>isTouchEnabled</strong></li><li><strong>reviewerName</strong></li><li><strong>reviewTitle</strong></li><li><strong>reviewText</strong></li><li><strong>helpfulCount</strong></li><li><strong>notHelpfulCount</strong></li><li><strong>responseDate</strong></li><li><strong>responseText</strong></li><li><strong>deviceRAM</strong></li><li><strong>deviceStorageCapacity</strong></li><li><strong>rating</strong></li></ul><p>Le paramètre <em>order</em>, facultatif, peut comporter les valeurs <strong>asc</strong> ou <strong>desc</strong> afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est <strong>asc</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em> : <em>orderby=date,market</em></p> |  Non  |

<span/>
 
### <a name="filter-fields"></a>Champs de filtrage

Le paramètre *filter* de la requête contient une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ et une valeur qui sont associés aux opérateurs **eq** ou **ne**, et certains champs prennent également en charge les opérateurs **contains**, **gt**, **lt**, **ge** et **le**. Les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**.

Voici un exemple de chaîne *filter*&nbsp;: *filter=contains(reviewText,’great’) and contains(reviewText,’ads’) and deviceRAM lt 2048 and market eq ’US’*

Pour obtenir une liste des champs pris en charge et des opérateurs associés à chacun d’entre eux, consultez le tableau suivant. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*.

| Champs        | Opérateurs pris en charge   |  Description        |
|---------------|--------|-----------------|
| market | eq, ne | Chaîne contenant le code pays ISO&nbsp;3166 du marché des appareils. |
| osVersion  | eq, ne  | Une des chaînes suivantes&nbsp;:<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows&nbsp;8</strong></li><li><strong>Windows&nbsp;8.1</strong></li><li><strong>Windows&nbsp;10</strong></li><li><strong>Unknown</strong></li></ul>  |
| deviceType  | eq, ne  | Une des chaînes suivantes&nbsp;:<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul>  |
| isRevised  | eq, ne  | Spécifiez la valeur <strong>true</strong> pour filtrer les avis révisés ; sinon, définissez la valeur <strong>false</strong>.  |
| packageVersion  | eq, ne  | La version du package d’application qui a été passée en revue.  |
| deviceModel  | eq, ne  | Le type d’appareil sur lequel l’application a été révisée.  |
| productFamily  | eq, ne  | Une des chaînes suivantes&nbsp;:<ul><li><strong>PC</strong></li><li><strong>Tablet</strong></li><li><strong>Phone</strong></li><li><strong>Wearable</strong></li><li><strong>Server</strong></li><li><strong>Collaborative</strong></li><li><strong>Other</strong></li></ul>  |
| deviceRAM  | eq, ne, gt, lt, ge, le  | Mémoire&nbsp;RAM physique, en Mo.  |
| deviceScreenResolution  | eq, ne  | Résolution de l’écran de l’appareil, au format &quot;<em>largeur</em>&nbsp;x&nbsp;<em>hauteur</em>&quot;.   |
| deviceStorageCapacity  | eq, ne, gt, lt, ge, le   | Capacité du disque de stockage principal, en Go.  |
| isTouchEnabled  | eq, ne  | Spécifiez la valeur <strong>true</strong> pour filtrer les appareils tactiles ; sinon, définissez la valeur <strong>false</strong>.   |
| reviewerName  | eq, ne  |  Nom de réviseur. |
| rating  | eq, ne, gt, lt, ge, le  | Classification de l’application, en étoiles.  |
| reviewTitle  | eq, ne, contains  | Le titre de l’avis.  |
| reviewText  | eq, ne, contains  |  Texte de l’avis. |
| helpfulCount  | eq, ne  |  Le nombre d’occurrences où l’avis a été marqué comme utile. |
| notHelpfulCount  | eq, ne  | Nombre d’occurrences où l’avis a été signalé comme inutile.  |
| responseDate  | eq, ne  | Date à laquelle la réponse a été soumise.  |
| responseText  | eq, ne, contains  | Texte de la réponse.  |


<span/> 

### <a name="request-example"></a>Exemple de requête

Les exemples suivants illustrent plusieurs requêtes de récupération des avis. Remplacez la valeur *applicationId* par l’ID Windows&nbsp;Store de votre application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type   | Description                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Valeur      | tableau  | Un tableau d’objets comportant des avis. Pour plus d’informations sur les données de chaque objet, consultez la section [Valeurs d’avis](#review-values) ci-dessous.                                                                                                                                      |
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour demander la page suivante. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur&nbsp;10000, mais que plus de&nbsp;10000&nbsp;lignes de données d’avis sont associées à la requête. |
| TotalCount | entier    | Nombre total de lignes des résultats de données pour la requête.                                                                                                                                                                                                                             |

<span/>
 
### <a name="review-values"></a>Valeurs d’avis

Les éléments du tableau *Value* comportent les valeurs suivantes&nbsp;:

| Valeur                  | Type    | Description                                                                                                                                                                                                                          |
|------------------------|---------|---------------------|
| date                   | chaîne  | Première date dans la plage de dates des données d’avis. Si la requête spécifiait un jour unique, cette valeur est cette date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates.  |
| applicationId          | chaîne  | ID Windows&nbsp;Store de l’application dont vous récupérez les données d’avis.        |
| applicationName        | chaîne  | Nom d’affichage de l’application.   |
| market                 | chaîne  | Code pays ISO&nbsp;3166 du marché où l’avis a été soumis.       |
| osVersion              | chaîne  | Version du système d’exploitation sur lequel l’avis a été soumis. Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.         |
| deviceType             | chaîne  | Type d’appareil sur lequel l’avis a été soumis. Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.      |
| isRevised              | Booléen | La valeur **true** indique que l’avis a été révisé&nbsp;; sinon, la valeur **false** est affichée.         |
| packageVersion         | chaîne  | La version du package d’application qui a été passée en revue.   |
| deviceModel            | chaîne  | Le type d’appareil sur lequel l’application a été révisée.      |
| productFamily          | chaîne  | Le nom de la famille d’appareils. Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.  |
| deviceRAM              | nombre  | Mémoire&nbsp;RAM physique, en Mo.        |
| deviceScreenResolution | chaîne  | Résolution de l’écran de l’appareil, au format *largeur*&nbsp;x&nbsp;*hauteur*.        |
| deviceStorageCapacity  | nombre  | Capacité du disque de stockage principal, en Go.   |
| isTouchEnabled         | Booléen | La valeur **true** indique l’activation de l’interaction tactile&nbsp;; sinon, la valeur **false** est affichée.      |
| reviewerName           | chaîne  | Nom de réviseur.      |
| rating                 | nombre  | Classification de l’application, en étoiles.         |
| reviewTitle            | chaîne  | Le titre de l’avis.       |
| reviewText             | chaîne  | Texte de l’avis.     |
| helpfulCount           | nombre  | Le nombre d’occurrences où l’avis a été marqué comme utile.     |
| notHelpfulCount        | nombre  | Nombre d’occurrences où l’avis a été signalé comme inutile.               |
| responseDate           | chaîne  | Date de soumission d’une réponse.                 |
| responseText           | chaîne  | Texte de la réponse.        |

<span/> 

### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "date": "2015-07-29",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso demo",
      "market": "US",
      "osVersion": "10.0.10240.16410",
      "deviceType": "PC",
      "isRevised": true,
      "packageVersion": "",
      "deviceModel": "Microsoft Corporation-Virtual Machine",
      "productFamily": "PC",
      "deviceRAM": -1,
      "deviceScreenResolution": "1024 x 768",
      "deviceStorageCapacity": 51200,
      "isTouchEnabled": false,
      "reviewerName": "ALeksandra",
      "rating": 5,
      "reviewTitle": "Love it",
      "reviewText": "Great app for demos and great dev response.",
      "helpfulCount": 0,
      "notHelpfulCount": 0,
      "responseDate": "2015-08-07T01:50:22.9874488Z",
      "responseText": "1"
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Rapport Avis](../publish/reviews-report.md)
* [Accéder aux données d’analyse à l’aide des services du Windows Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des acquisitions d’applications](get-app-acquisitions.md)
* [Obtenir des acquisitions d’extensions](get-in-app-acquisitions.md)
* [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md)
* [Obtenir les classifications des applications](get-app-ratings.md)



<!--HONumber=Dec16_HO1-->


