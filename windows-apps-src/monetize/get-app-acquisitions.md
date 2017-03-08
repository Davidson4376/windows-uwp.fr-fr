---
author: mcleanbyron
ms.assetid: C1E42E8B-B97D-4B09-9326-25E968680A0F
description: "Utilisez cette méthode dans l’API d’analyse du Windows Store afin d’obtenir les données d’acquisition agrégées d’une application pour une plage de dates donnée et en fonction d’autres filtres facultatifs."
title: "Obtenir des acquisitions d’applications"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, services du Windows Store, API d’analyse du Windows Store, acquisitions d’application"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 9a716acb00b7a86429c444555c491b48cbc2c4b0
ms.lasthandoff: 02/07/2017

---

# <a name="get-app-acquisitions"></a>Obtenir des acquisitions d’applications


Utilisez cette méthode dans l’API d’analyse du Windows Store afin d’obtenir les données d’acquisition agrégées au format JSON d’une application pour une plage de dates donnée et en fonction d’autres filtres facultatifs. Ces informations sont également disponibles dans le [rapport Acquisitions](../publish/acquisitions-report.md) du tableau de bord du Centre de développement.

## <a name="prerequisites"></a>Prérequis


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Windows Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions``` |

<span/>

### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |

<span/> 

### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Requis  
|---------------|--------|---------------|------|
| applicationId | chaîne | L’ID Windows Store de l’application pour laquelle vous souhaitez récupérer des données d’acquisition. L’ID Windows Store est disponible dans la page [Identité de l’application](../publish/view-app-identity-details.md) du tableau de bord du Centre de développement. Exemple d’ID Windows Store : 9WZDNCRFJ3Q8. |  Oui  |
| startDate | date | Dans la plage de dates, la date de début de la récupération des données d’acquisition. La valeur par défaut est la date actuelle. |  Non  |
| endDate | date | Dans la plage de dates, la date de fin de la récupération des données d’acquisition. La valeur par défaut est la date actuelle. |  Non  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |
| filter | chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Pour plus d’informations, voir la section [Champs de filtre](#filter-fields) ci-dessous. | Non   |
| aggregationLevel | chaîne | Indique la plage de temps pendant laquelle récupérer les données agrégées. Il peut s’agit des chaînes suivantes : <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Par défaut, la valeur est <strong>day</strong>. | Non |
| orderby | chaîne | Une instruction qui commande les valeurs de données de résultats pour chaque acquisition. Syntaxe : <em>orderby=field [order],field [order],...</em>. Le paramètre <em>field</em> peut comporter l’une des chaînes suivantes :<ul><li><strong>date</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p>Le paramètre <em>order</em>, facultatif, peut comporter les valeurs <strong>asc</strong> ou <strong>desc</strong> afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est <strong>asc</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em> : <em>orderby=date,market</em></p> |  Non  |
| groupby | chaîne | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants :<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p>Les lignes de données renvoyées comportent les champs spécifiés dans le paramètre <em>groupby</em>, ainsi que dans les paramètres suivants :</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p>Le paramètre <em>groupby</em> peut être utilisé avec le paramètre <em>aggregationLevel</em>. Par exemple : <em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  Non  |

<span/>
 
### <a name="filter-fields"></a>Champs de filtrage

Le paramètre *filter* de la requête contient une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ et une valeur qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**. Voici quelques exemples de paramètres *filter* :

-   *filter=market eq ’US’ and gender eq ’m’*
-   *filter=(market ne ’US’) and (gender ne ’Unknown’) and (gender ne ’m’) and (market ne ’NO’) and (ageGroup ne ’greater than 55’ or ageGroup ne ‘less than 13’)*

Pour obtenir la liste des champs pris en charge, consultez le tableau suivant : Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*.

| Champs        |  Description        |
|---------------|-----------------|
| acquisitionType | Une des chaînes suivantes :<ul><li><strong>free</strong></li><li><strong>trial</strong></li><li><strong>paid</strong></li><li><strong>promotional code</strong></li><li><strong>iap</strong></li></ul> |
| ageGroup | Une des chaînes suivantes :<ul><li><strong>less than 13</strong></li><li><strong>13-17</strong></li><li><strong>18-24</strong></li><li><strong>25-34</strong></li><li><strong>35-44</strong></li><li><strong>44-55</strong></li><li><strong>greater than 55</strong></li><li><strong>Unknown</strong></li></ul> |
| storeClient | Une des chaînes suivantes :<ul><li><strong>Windows Phone Store (client)</strong></li><li><strong>Windows Store (client)</strong></li><li><strong>Windows Store (web)</strong></li><li><strong>Volume purchase by organizations</strong></li><li><strong>Other</strong></li></ul> |
| gender | Une des chaînes suivantes :<ul><li><strong>m</strong></li><li><strong>f</strong></li><li><strong>Unknown</strong></li></ul> |
| market | Chaîne contenant le code pays ISO 3166 du marché de l’acquisition. |
| osVersion | Une des chaînes suivantes :<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Unknown</strong></li></ul> |
| deviceType | Une des chaînes suivantes :<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul> |
| orderName | Chaîne spécifiant le nom de la commande correspondant au code promotionnel utilisé pour l’acquisition de l’application (elle s’applique uniquement si l’utilisateur a acquis l’application en utilisant un code promotionnel). |

<span/> 

### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre plusieurs demandes d’obtention des données d’acquisition d’applications. Remplacez la valeur *applicationId* par l’ID Windows Store de votre application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US'; and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Valeur      | tableau  | Tableau d’objets contenant les données de classification agrégées. Pour plus d’informations sur les données de chaque objet, consultez la section [Valeurs d’acquisition](#acquisition-values) ci-dessous.                                                                                                                      |
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la demande est défini sur 10000, mais que plus de 10 000 lignes de données d’acquisition sont associées à la requête. |
| TotalCount | entier    | Nombre total de lignes des résultats de données pour la requête.                                                                                                                                                                                                                             |

<span/>
 
### <a name="acquisition-values"></a>Valeurs d’acquisition

Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur               | Type   | Description                           |
|---------------------|--------|-------------------------------------------|
| date                | chaîne | Première date dans la plage de dates des données d’acquisition. Si la requête était relative à un jour unique, cette valeur correspond à la date associée. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| applicationId       | chaîne | L’ID Windows Store de l’application pour laquelle vous récupérez les données d’acquisition.                                                                                                                                                                 |
| applicationName     | chaîne | Nom d’affichage de l’application.                                                                                                                                                                                                             |
| deviceType          | chaîne | Le type d’appareil ayant effectué l’acquisition. Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.                                                                                                  |
| orderName           | chaîne | Le nom de la commande.                                                                                                                                                                                                                   |
| storeClient         | chaîne | La version du Store dans laquelle l’acquisition s’est produite. Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.                                                                                            |
| osVersion           | chaîne | La version de système d’exploitation sur laquelle l’acquisition s’est produite. Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.                                                                                                   |
| market              | chaîne | Le code pays ISO 3166 du marché dans lequel l’acquisition s’est produite.                                                                                                                                                                  |
| gender              | chaîne | Le sexe de l’utilisateur qui a effectué l’acquisition. Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.                                                                                                    |
| ageGroup            | chaîne | Le groupe d’âge de l’utilisateur qui a effectué l’acquisition. Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.                                                                                                 |
| acquisitionType     | chaîne | Le type d’acquisition (gratuite, payante, etc.). Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.                                                                                                    |
| acquisitionQuantity | nombre | Le nombre d’acquisitions qui se sont produites durant le niveau d’agrégation spécifié.                                                                                                                                                         |

<span/> 

### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "date": "2016-02-01",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "deviceType": "Phone",
      "orderName": "",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows Phone 8.1",
      "market": "IT",
      "gender": "m",
      "ageGroup": "0-17",
      "acquisitionType": "Free",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "appacquisitions?applicationId=9NBLGGGZ5QDR&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1&orderby=date desc",
  "TotalCount": 466766
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Rapport Acquisitions](../publish/acquisitions-report.md)
* [Accéder aux données d’analyse à l’aide des services du Windows Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des acquisitions d’extensions](get-in-app-acquisitions.md)
* [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md)
* [Obtenir les classifications des applications](get-app-ratings.md)
* [Obtenir les avis sur les applications](get-app-reviews.md)

