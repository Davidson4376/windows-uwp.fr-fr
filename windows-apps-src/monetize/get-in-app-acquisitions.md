---
author: mcleanbyron
ms.assetid: 1599605B-4243-4081-8D14-40F6F7734E25
description: "Utilisez cette méthode dans l’API d’analyse du Windows Store pour obtenir les données d’acquisition agrégées d’une extension pour une plage de dates données, et en fonction de filtres facultatifs."
title: "Obtenir des acquisitions d’extensions"
translationtype: Human Translation
ms.sourcegitcommit: 7d05c8953f1f50be0b388a044fe996f345d45006
ms.openlocfilehash: a79cd324d57151318445df0dedd35a98d9c9f915

---

# <a name="get-add-on-acquisitions"></a>Obtenir des acquisitions d’extensions




Utilisez cette méthode dans l’API d’analyse du Windows Store pour obtenir les données d’acquisition agrégées des extensions (également connue sous le nom produit in-app ou PIA) au format JSON pour votre application, pour une plage de dates données, et en fonction de filtres facultatifs. Ces informations sont également disponibles dans le [rapport sur les acquisitions des extensions](../publish/add-on-acquisitions-report.md) du tableau de bord du Centre de développement.

## <a name="prerequisites"></a>Prérequis


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Windows Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions``` |

<span/> 

### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description          |
|---------------|--------|--------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |

<span/> 

### <a name="request-parameters"></a>Paramètres de la requête

Le paramètre *applicationId* ou *inAppProductId* est obligatoire. Pour récupérer les données d’acquisition de toutes les extensions inscrites dans l’application, spécifiez le paramètre *applicationId*. Pour récupérer les données d’acquisition d’une seule extension, spécifiez le paramètre *inAppProductId*. Si vous spécifiez les deux valeurs, le paramètre *applicationId* est ignoré.

| Paramètre        | Type   |  Description      |  Requis  
|---------------|--------|---------------|------|
| applicationId | chaîne | ID Windows Store de l’application pour laquelle vous voulez récupérer des données d’acquisition d’extension. L’ID Windows Store est disponible dans la [page Identité de l’application](../publish/view-app-identity-details.md) du tableau de bord du Centre de développement. Exemple d’ID Windows Store : 9WZDNCRFJ3Q8. |  Oui  |
| inAppProductId | chaîne | ID Windows Store de l’extension pour laquelle vous voulez récupérer des données d’acquisition. L’ID Windows Store est disponible dans l’URL de la page de la vue d’ensemble de l’extension dans le tableau de bord du Centre de développement Windows. Par exemple, si l’URL de la page du tableau de bord d’une extension est ```https://developer.microsoft.com/en-us/dashboard/iaps/9NBLGGH4SCZS?appId=9NBLGGH29DM8```, l’ID Windows Store de cette extension est la chaîne 9NBLGGH4SCZS. | Oui  |
| startDate | date | Dans la plage de dates, date de début de la récupération des données d’acquisition. La valeur par défaut est la date du jour. |  Non  |
| endDate | date | Dans la plage de dates, date de fin de la récupération des données d’acquisition. La valeur par défaut est la date du jour. |  Non  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |
| filter |chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Pour plus d’informations, voir la section [Champs de filtre](#filter-fields) ci-dessous. | Non   |
| aggregationLevel | chaîne | Indique la plage de temps pendant laquelle récupérer les données agrégées. Il peut s’agit des chaînes suivantes : <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Par défaut, la valeur est <strong>day</strong>. | Non |
| orderby | chaîne | Instruction qui commande les valeurs de données de résultats pour chaque acquisition d’extension. Syntaxe : <em>orderby=field [order],field [order],...</em>. Le paramètre <em>field</em> peut comporter l’une des chaînes suivantes :<ul><li><strong>date</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p>Le paramètre <em>order</em>, facultatif, peut comporter les valeurs <strong>asc</strong> ou <strong>desc</strong> afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est <strong>asc</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em> : <em>orderby=date,market</em></p> |  Non  |
| groupby | chaîne | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants :<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>inAppProductName</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p>Les lignes de données renvoyées comportent les champs spécifiés dans le paramètre <em>groupby</em>, ainsi que dans les paramètres suivants :</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>inAppProductId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p>Le paramètre <em>groupby</em> peut être utilisé avec le paramètre <em>aggregationLevel</em>. Par exemple : <em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  Non  |

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
| orderName | Chaîne spécifiant le nom de la commande correspondant au code promotionnel utilisé pour l’acquisition du module complémentaire (elle s’applique uniquement si l’utilisateur a acquis le module complémentaire à l’aide d’un code promotionnel). |

<span/> 

### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre quelques requêtes d’obtention de données d’acquisition d’extensions. Remplacez les valeurs *inAppProductId* et *applicationId* par l’ID Windows Store qui correspond à votre extension ou application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?inAppProductId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?inAppProductId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=7/3/2015&top=100&skip=0&filter=market ne 'US' and gender ne 'Unknown' and gender ne 'm' and market ne 'NO' and ageGroup ne '>55' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type   | Description         |
|------------|--------|------------------|
| Valeur      | tableau  | Tableau d’objets contenant des données d’acquisition agrégées d’extensions. Pour plus d’informations sur les données incluses dans chaque objet, voir [Valeurs d’acquisition d’extensions](#add-on-acquisition-values) ci-dessous.                                                                                                              |
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête a la valeur 10000, mais qu’il existe plus de 10 000 lignes de données d’acquisition d’extensions pour la demande. |
| TotalCount | entier    | Nombre total de lignes dans les résultats de données de la requête.                                                                                                                                                                                                                                 |

<span/>

<span id="add-on-acquisition-values" />
### <a name="add-on-acquisition-values"></a>Valeurs d’acquisition d’extensions

Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur               | Type    | Description        |
|---------------------|---------|---------------------|
| date                | chaîne  | Première date dans la plage de dates des données d’acquisition. Si la requête était relative à un jour unique, cette valeur correspond à la date associée. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| inAppProductId      | chaîne  | ID Windows Store de l’extension pour laquelle vous récupérez des données d’acquisition.                                                                                                                                                                 |
| inAppProductName    | chaîne  | Nom d’affichage de l’extension. Cette valeur apparaît dans les données de la réponse uniquement si le paramètre *aggregationLevel* est défini sur **day**, sauf si vous définissez le champ **inAppProductName** dans le paramètre *groupby*.                                                                                                                                                                                                            |
| applicationId       | chaîne  | ID Windows Store de l’application pour laquelle vous voulez récupérer des données d’acquisition d’extension.                                                                                                                                                           |
| applicationName     | chaîne  | Nom d’affichage de l’application.                                                                                                                                                                                                             |
| deviceType          | chaîne  | Le type d’appareil ayant effectué l’acquisition. Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.                                                                                                  |
| orderName           | chaîne  | Le nom de la commande.                                                                                                                                                                                                                   |
| storeClient         | chaîne  | La version du Store dans laquelle l’acquisition s’est produite. Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.                                                                                            |
| osVersion           | chaîne  | La version de système d’exploitation sur laquelle l’acquisition s’est produite. Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.                                                                                                   |
| market              | chaîne  | Le code pays ISO 3166 du marché dans lequel l’acquisition s’est produite.                                                                                                                                                                  |
| gender              | chaîne  | Le sexe de l’utilisateur qui a effectué l’acquisition. Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.                                                                                                    |
| ageGroup            | chaîne  | Le groupe d’âge de l’utilisateur qui a effectué l’acquisition. Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.                                                                                                 |
| acquisitionType     | chaîne  | Le type d’acquisition (gratuite, payante, etc.). Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.                                                                                                    |
| acquisitionQuantity | nombre entier | Le nombre d’acquisitions qui se sont produites.                                                                                                                                                                                                |

<span/> 

### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "date": "2015-01-02",
      "inAppProductId": "9NBLGGH3LHKL",
      "inAppProductName": "Contoso add-on 7",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "deviceType": "Phone",
      "orderName": "",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows Phone 8.1",
      "market": "GB",
      "gender": "m",
      "ageGroup": "50orover",
      "acquisitionType": "iap",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "inappacquisitions?applicationId=9NBLGGGZ5QDR&inAppProductId=&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 33677
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Rapport sur les acquisitions des extensions](../publish/add-on-acquisitions-report.md)
* [Accéder aux données d’analyse à l’aide des services du Windows Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des acquisitions d’applications](get-app-acquisitions.md)
* [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md)
* [Obtenir les classifications des applications](get-app-ratings.md)
* [Obtenir les avis sur les applications](get-app-reviews.md)

 

 



<!--HONumber=Dec16_HO1-->


