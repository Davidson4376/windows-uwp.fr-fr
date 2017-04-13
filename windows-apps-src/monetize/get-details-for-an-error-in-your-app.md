---
author: mcleanbyron
ms.assetid: f0c0325e-ad61-4238-a096-c37802db3d3b
description: "Utilisez cette méthode dans l’API d’analyse du WindowsStore pour obtenir les informations concernant une erreur spécifique de votre application."
title: Obtenir les informations sur une erreur de votre application
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows10, uwp, services du WindowsStore, API d’analyse du WindowsStore, erreurs, détails"
ms.openlocfilehash: cfab1c8f5149d4c6d02a9fa94287a4e204a11a7f
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="get-details-for-an-error-in-your-app"></a>Obtenir les informations sur une erreur de votre application

Utilisez cette méthode dans l’API d’analyse du WindowsStore pour obtenir les informations concernant une erreur spécifique de votre application, au format JSON. Cette méthode ne récupère que les informations concernant les erreurs survenues dans les 30derniers jours. Ces informations sont également disponibles dans la section **Échecs** du [rapport d’intégrité](../publish/health-report.md) dans le tableau de bord du Centre de développement Windows.

Pour utiliser cette méthode, vous devez d’abord utiliser la méthode [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md) afin de récupérer l’ID de l’erreur sur laquelle vous souhaitez des informations détaillées.

## <a name="prerequisites"></a>Conditions préalables


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Windows Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Récupérez l’ID de l’erreur sur laquelle vous souhaitez des informations détaillées. Pour ce faire, utilisez la méthode [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md) et utilisez la valeur **failureHash** dans le corps de la réponse de cette méthode.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails``` |

<span/> 

### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |

<span/> 

### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Requis  
|---------------|--------|---------------|------|
| applicationId | chaîne | ID WindowsStore de l’application dont vous souhaitez récupérer des données d’erreur. L’ID WindowsStore est disponible dans la page [Identité de l’application](../publish/view-app-identity-details.md) du tableau de bord du Centre de développement. Exemple d’ID WindowsStore: 9WZDNCRFJ3Q8. |  Oui  |
| failureHash | chaîne | ID de l’erreur sur laquelle vous souhaitez des informations détaillées. Pour obtenir la valeur correspondant à l’erreur qui vous intéresse, utilisez la méthode [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md) et utilisez la valeur **failureHash** dans le corps de la réponse de cette méthode. |  Oui  |
| startDate | date | Date de début des données à récupérer concernant l’erreur. La valeur par défaut est de 30jours avant la date actuelle. |  Non  |
| endDate | date | Date de fin des données à récupérer concernant l’erreur. La valeur par défaut est la date actuelle |  Non  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10 et skip=0 pour obtenir les 10premières lignes de données, top=10 et skip=10 pour obtenir les 10lignes suivantes, et ainsi de suite. |  Non  |
| filter |chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Pour plus d’informations, voir la section [Champs de filtre](#filter-fields) ci-dessous. | Non   |
| orderby | chaîne | Instruction commandant les valeurs des données de résultats. Syntaxe: <em>orderby=field [order],field [order],...</em>. Le paramètre <em>field</em> peut comporter l’une des chaînes suivantes :<ul><li><strong>date</strong></li><li><strong>market</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul><p>Le paramètre facultatif <em>order</em> peut avoir la valeur <strong>asc</strong> ou <strong>desc</strong> pour spécifier l’ordre croissant ou décroissant de chaque champ. La valeur par défaut est <strong>asc</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em>: <em>orderby=date,market</em></p> |  Non  |

<span/>
 
### <a name="filter-fields"></a>Champs de filtrage

Le paramètre *filter* de la requête contient une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ et une valeur qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**. Voici quelques exemples de paramètres *filter*:

-   *filter=market eq 'US' and osVersion eq 'Windows 10'*
-   *filter=market ne 'US' and osVersion ne 'Windows 8'*

Pour obtenir la liste des champs pris en charge, consultez le tableau suivant: Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*.

| Champs        |  Description        |
|---------------|-----------------|
| osVersion | Une des chaînes suivantes:<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows8</strong></li><li><strong>Windows8.1</strong></li><li><strong>Windows10</strong></li><li><strong>Unknown</strong></li></ul> |
| osBuild | Numéro de version du système d’exploitation sur lequel l’application s’exécutait lorsque l’erreur s’est produite. |
| market | Chaîne contenant le code de pays ISO3166 du marché de l’appareil sur lequel l’application s’exécutait lorsque l’erreur s’est produite. |
| deviceType | Une des chaînes suivantes qui spécifie le type de l’appareil sur lequel l’application s’exécutait lorsque l’erreur s’est produite:<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul> |
| deviceModel | Chaîne identifiant le modèle d’appareil sur lequel l’application s’exécutait lorsque l’erreur s’est produite. |
| cabId | ID unique du fichierCAB associé à cette erreur. |
| cabExpirationTime | Date et heure auxquelles le fichierCAB est arrivé à expiration et n’est plus téléchargeable au format ISO8601. |
| packageVersion | Version du package applicatif associé à cette erreur. |

<span/> 

### <a name="request-example"></a>Exemple de requête

Les exemples suivants fournissent plusieurs requêtes permettant de récupérer des données d’erreur. Remplacez la valeur *applicationId* par l’ID WindowsStore de votre application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails?applicationId=9NBLGGGZ5QDR&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails?applicationId=9NBLGGGZ5QDR&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'Windows.Desktop' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type    | Description    |
|------------|---------|------------|
| Valeur      | array   | Tableau d’objets comportant des données d’erreur détaillées. Pour plus d’informations sur les données de chaque objet, consultez la section [Valeurs des informations d’erreur](#error-detail-values) ci-dessous.          |
| @nextLink  | chaîne  | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour demander la page suivante. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur10, mais que plus de 10lignes d’erreur sont associées à la requête. |
| TotalCount | nombre entier | Nombre total de lignes dans les résultats de la requête.        |

<span id="error-detail-values"/>
### <a name="error-detail-values"></a>Valeurs des informations d’erreur

Les éléments du tableau *Value* ont les valeurs suivantes:

| Valeur           | Type    | Description     |
|-----------------|---------|----------------------------|
| date            | chaîne  | Date de début des données d’erreur. Si la requête spécifiait un jour unique, cette valeur est cette date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| applicationId   | chaîne  | ID WindowsStore de l’application dont vous souhaitez récupérer des données d’erreur.      |
| failureName     | chaîne  | Nom de l’erreur. Ce nom est celui qui figure dans la section **Échecs** du [rapport d’intégrité](../publish/health-report.md) dans le tableau de bord du Centre de développement Windows.            |
| failureHash     | chaîne  | Identificateur unique de l’erreur.     |
| osVersion       | chaîne  | Version de système d’exploitation sur laquelle l’erreur s’est produite.    |
| market          | chaîne  | Code pays ISO3166 du marché des appareils.     |
| deviceType      | chaîne  | Type d’appareil sur lequel l’erreur s’est produite.     |
| packageVersion  | chaîne  | Version du package applicatif associé à cette erreur.    |
| osBuild         | chaîne  | Numéro de version du système d’exploitation sur lequel l’erreur s’est produite.       |
| cabId           | chaîne  | ID unique du fichierCAB associé à cette erreur.   |
| cabExpirationTime  | chaîne  | Date et heure auxquelles le fichierCAB est arrivé à expiration et n’est plus téléchargeable au format ISO8601.   |
| deviceModel           | chaîne  | Chaîne identifiant le modèle d’appareil sur lequel l’application s’exécutait lorsque l’erreur s’est produite.   |
| cabDownloadable           | Booléen  | Indique si le fichier CAB est téléchargeable par cet utilisateur.   |

<span/> 

### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR ",
      "failureHash": "012345-5dbc9-b12f-c124-9d9810f05d8b",
      "failureName": "STOWED_EXCEPTION_System.UriFormatException_exe!ContosoGame.GroupedItems+_ItemView_ItemClick_d__9.MoveNext",
      "date": "2015-02-05 09:11:25",
      "cabId": "133637331323",
      "cabExpirationTime": "2016-12-05 09:11:25",
      "market": "US",
      "osBuild": "10.0.10240",
      "packageVersion": "1.0.2.6",
      "deviceModel": "Contoso Computer",
      "osVersion": "Windows 10",
      "deviceType": "Windows.Desktop",
      "cabDownloadable": false
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Rapport d’intégrité](../publish/health-report.md)
* [Accéder aux données d’analyse à l’aide des services du Windows Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md)
* [Obtenir la trace de pile concernant une erreur dans votre application](get-the-stack-trace-for-an-error-in-your-app.md)
