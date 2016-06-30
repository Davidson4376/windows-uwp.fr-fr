---
author: mcleanbyron
ms.assetid: 1599605B-4243-4081-8D14-40F6F7734E25
description: "Utilisez cette méthode dans l’API d’analyse du Windows Store pour obtenir les données d’acquisition agrégées d’un produit in-app pour une plage de dates données, et en fonction de filtres facultatifs."
title: Obtenir les acquisitions de produits in-app
ms.sourcegitcommit: 02131e641cdaa76256845b38bcc50aa42d718601
ms.openlocfilehash: 21e634b1d5ab6c3ba7762c1b83c94d076d094af5

---

# Obtenir les acquisitions de produits in-app


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Utilisez cette méthode dans l’API d’analyse du Windows Store pour obtenir les données d’acquisition agrégées d’un produit in-app pour une plage de dates données, et en fonction de filtres facultatifs. Cette méthode renvoie les données au format JSON.

## Prérequis


Pour utiliser cette méthode, procédez comme suit :

-   Associez l’application Azure AD que vous utiliserez pour appeler cette méthode à votre compte du Centre de développement.

-   Obtenez un jeton d’accès Azure AD pour votre application.

Pour plus d’informations, voir [Accéder aux données d’analyse à l’aide des services du Windows Store](access-analytics-data-using-windows-store-services.md).

## Requête


### Syntaxe de la requête

| Méthode | URI de la requête                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions |

 

### En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer**&lt;*token*&gt;. |

 

### Corps de la demande

Le paramètre *applicationId* ou *inAppProductId* est requis. Pour récupérer les données d’acquisition de l’ensemble des PIA inscrits dans l’application, spécifiez le paramètre *applicationId*. Pour récupérer les données d’acquisition d’un PIA donné, spécifiez le paramètre *inAppProductId*. Si vous spécifiez les deux valeurs, le paramètre *inAppProductId* est ignoré.

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Paramètre</th>
<th align="left">Type</th>
<th align="left">Description</th>
<th align="left">Requis</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">applicationId</td>
<td align="left">chaîne</td>
<td align="left">L’ID Windows Store de l’application pour laquelle vous souhaitez récupérer les données d’acquisition de produits in-app. L’ID Windows Store est disponible dans la page [Identité de l’application](../publish/view-app-identity-details.md) du tableau de bord du Centre de développement. Exemple d’ID Windows Store : 9WZDNCRFJ3Q8.</td>
<td align="left">Oui</td>
</tr>
<tr class="even">
<td align="left">inAppProductId</td>
<td align="left">chaîne</td>
<td align="left">L’ID produit du produit in-app pour lequel vous souhaitez récupérer les données d’acquisition.</td>
<td align="left">Oui</td>
</tr>
<tr class="odd">
<td align="left">startDate</td>
<td align="left">date</td>
<td align="left">Dans la plage de dates, la date de début de la récupération des données d’acquisition des produits in-app. La valeur par défaut est la date actuelle.</td>
<td align="left">Non</td>
</tr>
<tr class="even">
<td align="left">endDate</td>
<td align="left">date</td>
<td align="left">Dans la plage de dates, la date de fin de la récupération des données d’acquisition des produits in-app. La valeur par défaut est la date actuelle.</td>
<td align="left">Non</td>
</tr>
<tr class="odd">
<td align="left">top</td>
<td align="left">entier</td>
<td align="left">Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données.</td>
<td align="left">Non</td>
</tr>
<tr class="even">
<td align="left">skip</td>
<td align="left">entier</td>
<td align="left">Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite.</td>
<td align="left">Non</td>
</tr>
<tr class="odd">
<td align="left">filter</td>
<td align="left">chaîne</td>
<td align="left">Une ou plusieurs instructions qui filtrent les lignes de la réponse. Pour plus d’informations, voir la section [Champs de filtre](#filter-fields) ci-dessous.</td>
<td align="left">Non</td>
</tr>
<tr class="even">
<td align="left">aggregationLevel</td>
<td align="left">chaîne</td>
<td align="left">Indique la plage de temps pendant laquelle récupérer les données agrégées. Il peut s’agit des chaînes suivantes : <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Par défaut, la valeur est <strong>day</strong>.</td>
<td align="left">Non</td>
</tr>
<tr class="odd">
<td align="left">orderby</td>
<td align="left">chaîne</td>
<td align="left">Une instruction qui commande les valeurs de données de résultats pour chaque acquisition de produit in-app. Syntaxe : <em>orderby=field [order],field [order],...</em>. Le paramètre <em>field</em> peut comporter l’une des chaînes suivantes :
<ul>
<li><strong>date</strong></li>
<li><strong>acquisitionType</strong></li>
<li><strong>ageGroup</strong></li>
<li><strong>storeClient</strong></li>
<li><strong>gender</strong></li>
<li><strong>market</strong></li>
<li><strong>osVersion</strong></li>
<li><strong>deviceType</strong></li>
<li><strong>orderName</strong></li>
</ul>
<p>Le paramètre <em>order</em>, facultatif, peut comporter les valeurs <strong>asc</strong> ou <strong>desc</strong> afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est <strong>asc</strong>.</p>
<p>Voici un exemple de chaîne <em>orderby</em> : <em>orderby=date,market</em></p></td>
<td align="left">Non</td>
</tr>
</tbody>
</table>

 

### Champs de filtrage

Le paramètre *filter* du corps de la demande contient une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ et une valeur qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**. Voici quelques exemples de paramètres *filter* :

-   *filter=market eq ’US’ and gender eq ’m’*
-   *filter=(market ne ’US’) and (gender ne ’Unknown’) and (gender ne ’m’) and (market ne ’NO’) and (ageGroup ne ’greater than 55’ or ageGroup ne ‘less than 13’)*

Pour obtenir la liste des champs pris en charge, consultez le tableau suivant : Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Champ</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">acquisitionType</td>
<td align="left">Une des chaînes suivantes :
<ul>
<li><strong>free</strong></li>
<li><strong>trial</strong></li>
<li><strong>paid</strong></li>
<li><strong>promotional code</strong></li>
<li><strong>iap</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">ageGroup</td>
<td align="left">Une des chaînes suivantes :
<ul>
<li><strong>less than 13</strong></li>
<li><strong>13-17</strong></li>
<li><strong>18-24</strong></li>
<li><strong>25-34</strong></li>
<li><strong>35-44</strong></li>
<li><strong>44-55</strong></li>
<li><strong>greater than 55</strong></li>
<li><strong>Unknown</strong></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">storeClient</td>
<td align="left">Une des chaînes suivantes :
<ul>
<li><strong>Windows Phone Store (client)</strong></li>
<li><strong>Windows Store (client)</strong></li>
<li><strong>Windows Store (web)</strong></li>
<li><strong>Volume purchase by organizations</strong></li>
<li><strong>Other</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">gender</td>
<td align="left">Une des chaînes suivantes :
<ul>
<li><strong>m</strong></li>
<li><strong>f</strong></li>
<li><strong>Unknown</strong></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">market</td>
<td align="left">Chaîne contenant le code pays ISO 3166 du marché de l’acquisition.</td>
</tr>
<tr class="even">
<td align="left">osVersion</td>
<td align="left">Une des chaînes suivantes :
<ul>
<li><strong>Windows Phone 7.5</strong></li>
<li><strong>Windows Phone 8</strong></li>
<li><strong>Windows Phone 8.1</strong></li>
<li><strong>Windows Phone 10</strong></li>
<li><strong>Windows 8</strong></li>
<li><strong>Windows 8.1</strong></li>
<li><strong>Windows 10</strong></li>
<li><strong>Unknown</strong></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">deviceType</td>
<td align="left">Une des chaînes suivantes :
<ul>
<li><strong>PC</strong></li>
<li><strong>Tablet</strong></li>
<li><strong>Phone</strong></li>
<li><strong>IoT</strong></li>
<li><strong>Wearable</strong></li>
<li><strong>Server</strong></li>
<li><strong>Collaborative</strong></li>
<li><strong>Other</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">orderName</td>
<td align="left">Chaîne spécifiant le nom de la commande correspondant au code promotionnel utilisé pour l’acquisition de l’application (elle s’applique uniquement si l’utilisateur a acquis l’application en utilisant un code promotionnel).</td>
</tr>
</tbody>
</table>

 

### Exemple de requête

L’exemple suivant illustre quelques requêtes de récupération de données d’acquisition de produits in-app. Remplacez les valeurs *inAppProductId* et *applicationId* par l’ID produit approprié associé à votre PIA et ID Windows Store pour votre application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?inAppProductId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?inAppProductId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=7/3/2015&top=100&skip=0&filter=market ne 'US' and gender ne 'Unknown' and gender ne 'm' and market ne 'NO' and ageGroup ne '>55' HTTP/1.1
Authorization: Bearer <your access token>
```

## Réponse


### Corps de la réponse

| Valeur      | Type   | Description                                                                                                                                                                                                                                                                                |
|------------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Valeur      | tableau  | Tableau d’objets contenant les données agrégées d’acquisition de produits in-app. Pour plus d’informations sur les données de chaque objet, consultez la section [Valeurs d’acquisition des produits in-app](#iap-acquisition-values) ci-dessous.                                                                                                              |
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la demande est défini sur 10000, mais que plus de 10 000 lignes de données d’acquisition de PIA sont associées à la requête. |
| TotalCount | entier    | Nombre total de lignes des résultats de données pour la requête.                                                                                                                                                                                                                                 |


### Valeurs d’acquisition des produits in-app

Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur               | Type    | Description                                                                                                                                                                                                                              |
|---------------------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| date                | chaîne  | Première date dans la plage de dates des données d’acquisition. Si la requête était relative à un jour unique, cette valeur correspond à la date associée. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| inAppProductId      | chaîne  | L’ID produit du produit in-app pour lequel vous récupérez les données d’acquisition.                                                                                                                                                                 |
| inAppProductName    | chaîne  | Nom d’affichage du produit in-app.                                                                                                                                                                                                             |
| applicationId       | chaîne  | L’ID Windows Store de l’application pour laquelle vous souhaitez récupérer les données d’acquisition de produits in-app.                                                                                                                                                           |
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

 

### Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "date": "2015-01-02",
      "inAppProductId": "9NBLGGH3LHKL",
      "inAppProductName": "Contoso IAP 7",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "deviceType": "Phone",
      "orderName": "",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows Phone 8.1",
      "market": "GB",
      "gender": "m",
      "ageGroup": "50orover",
      "acquisitionType": "Iap",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "inappacquisitions?applicationId=9NBLGGGZ5QDR&inAppProductId=&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 33677
}
```

## Rubriques connexes

* [Accéder aux données d’analyse à l’aide des services du Windows Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des acquisitions d’applications](get-app-acquisitions.md)
* [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md)
* [Obtenir les classifications des applications](get-app-ratings.md)
* [Obtenir les avis sur les applications](get-app-reviews.md)

 

 



<!--HONumber=Jun16_HO4-->


