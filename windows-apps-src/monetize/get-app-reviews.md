---
author: mcleanbyron
ms.assetid: 2967C757-9D8A-4B37-8AA4-A325F7A060C5
description: "Utilisez cette méthode dans l’API d’analyse du WindowsStore pour obtenir les avis relatifs à une plage de dates donnée, et suivant d’autres filtres facultatifs."
title: Obtenir les avis sur les applications
translationtype: Human Translation
ms.sourcegitcommit: f7e67a4ff6cb900fb90c5d5643e2ddc46cbe4dd2
ms.openlocfilehash: 6e7e8c6a1b870031fb7055bf09c8ebbaa7dc13a5

---

# Obtenir les avis sur les applications


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Utilisez cette méthode dans l’API d’analyse du WindowsStore pour obtenir les avis relatifs à une plage de dates donnée, et suivant d’autres filtres facultatifs. Cette méthode renvoie les données au format JSON.

## Prérequis


Pour utiliser cette méthode, procédez comme suit:

-   Associez l’application Azure AD que vous utiliserez pour appeler cette méthode à votre compte du Centre de développement.

-   Obtenez un jeton d’accès Azure AD pour votre application.

Pour plus d’informations, voir [Accéder aux données d’analyse à l’aide des services du WindowsStore](access-analytics-data-using-windows-store-services.md).

## Requête


### Syntaxe de la requête

| Méthode | URI de la requête                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews``` |

<span/> 

### En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer**&lt;*token*&gt;. |

<span/> 

### Paramètres de la requête

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
<td align="left">L’ID WindowsStore pour l’application pour laquelle vous souhaitez récupérer les avis. L’ID WindowsStore est disponible dans la page [Identité de l’application](../publish/view-app-identity-details.md) du tableau de bord du Centre de développement. Exemple d’ID WindowsStore: 9WZDNCRFJ3Q8.</td>
<td align="left">Oui</td>
</tr>
<tr class="even">
<td align="left">startDate</td>
<td align="left">date</td>
<td align="left">Dans la plage de dates, la date de début de la récupération des avis. La valeur par défaut est la date actuelle.</td>
<td align="left">Non</td>
</tr>
<tr class="odd">
<td align="left">endDate</td>
<td align="left">date</td>
<td align="left">Dans la plage de dates, la date de début de la récupération des avis. La valeur par défaut est la date actuelle.</td>
<td align="left">Non</td>
</tr>
<tr class="even">
<td align="left">top</td>
<td align="left">entier</td>
<td align="left">Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données.</td>
<td align="left">Non</td>
</tr>
<tr class="odd">
<td align="left">skip</td>
<td align="left">entier</td>
<td align="left">Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite.</td>
<td align="left">Non</td>
</tr>
<tr class="even">
<td align="left">filter</td>
<td align="left">chaîne</td>
<td align="left">Une ou plusieurs instructions qui filtrent les lignes de la réponse. Pour plus d’informations, voir la section [Champs de filtre](#filter-fields) ci-dessous.</td>
<td align="left">Non</td>
</tr>
<tr class="odd">
<td align="left">orderby</td>
<td align="left">chaîne</td>
<td align="left">Une instruction qui commande les valeurs de données de résultats pour chaque avis. Syntaxe : <em>orderby=field [order],field [order],...</em>. Le paramètre <em>field</em> peut comporter l’une des chaînes suivantes :
<ul>
<li><strong>date</strong></li>
<li><strong>osVersion</strong></li>
<li><strong>market</strong></li>
<li><strong>deviceType</strong></li>
<li><strong>isRevised</strong></li>
<li><strong>packageVersion</strong></li>
<li><strong>deviceModel</strong></li>
<li><strong>productFamily</strong></li>
<li><strong>deviceScreenResolution</strong></li>
<li><strong>isTouchEnabled</strong></li>
<li><strong>reviewerName</strong></li>
<li><strong>reviewTitle</strong></li>
<li><strong>reviewText</strong></li>
<li><strong>helpfulCount</strong></li>
<li><strong>notHelpfulCount</strong></li>
<li><strong>responseDate</strong></li>
<li><strong>responseText</strong></li>
<li><strong>deviceRAM</strong></li>
<li><strong>deviceStorageCapacity</strong></li>
<li><strong>rating</strong></li>
</ul>
<p>Le paramètre <em>order</em>, facultatif, peut comporter les valeurs <strong>asc</strong> ou <strong>desc</strong> afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est <strong>asc</strong>.</p>
<p>Voici un exemple de chaîne <em>orderby</em> : <em>orderby=date,market</em></p></td>
<td align="left">Non</td>
</tr>
</tbody>
</table>

<span/>
 
### Champs de filtrage

Le paramètre *filter* de la requête contient une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ et une valeur qui sont associés aux opérateurs **eq** ou **ne**, et certains champs prennent également en charge les opérateurs **contains**, **gt**, **lt**, **ge** et **le**. Les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**.

Voici un exemple de chaîne *filter*: *filter=contains(reviewText,’great’) and contains(reviewText,’ads’) and deviceRAM lt 2048 and market eq ’US’*

Pour obtenir une liste des champs pris en charge et des opérateurs associés à chacun d’entre eux, consultez le tableau suivant. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Champs</th>
<th align="left">Opérateurs pris en charge</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">market</td>
<td align="left">eq, ne</td>
<td align="left">Chaîne contenant le code pays ISO3166 du marché des appareils.</td>
</tr>
<tr class="even">
<td align="left">osVersion</td>
<td align="left">eq, ne</td>
<td align="left">Une des chaînes suivantes:
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
<td align="left">eq, ne</td>
<td align="left">Une des chaînes suivantes:
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
<td align="left">isRevised</td>
<td align="left">eq, ne</td>
<td align="left">Spécifiez la valeur <strong>true</strong> pour filtrer les avis révisés ; sinon, définissez la valeur <strong>false</strong>.</td>
</tr>
<tr class="odd">
<td align="left">packageVersion</td>
<td align="left">eq, ne</td>
<td align="left">La version du package d’application qui a été passée en revue.</td>
</tr>
<tr class="even">
<td align="left">deviceModel</td>
<td align="left">eq, ne</td>
<td align="left">Le type d’appareil sur lequel l’application a été révisée.</td>
</tr>
<tr class="odd">
<td align="left">productFamily</td>
<td align="left">eq, ne</td>
<td align="left">Une des chaînes suivantes:
<ul>
<li><strong>PC</strong></li>
<li><strong>Tablet</strong></li>
<li><strong>Phone</strong></li>
<li><strong>Wearable</strong></li>
<li><strong>Server</strong></li>
<li><strong>Collaborative</strong></li>
<li><strong>Other</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">deviceScreenResolution</td>
<td align="left">eq, ne</td>
<td align="left">Résolution de l’écran de l’appareil, au format &quot;<em>largeur</em> x <em>hauteur</em>&quot;.</td>
</tr>
<tr class="odd">
<td align="left">isTouchEnabled</td>
<td align="left">eq, ne</td>
<td align="left">Spécifiez la valeur <strong>true</strong> pour filtrer les appareils tactiles ; sinon, définissez la valeur <strong>false</strong>.</td>
</tr>
<tr class="even">
<td align="left">reviewerName</td>
<td align="left">eq, ne</td>
<td align="left">Le nom de réviseur.</td>
</tr>
<tr class="odd">
<td align="left">helpfulCount</td>
<td align="left">eq, ne</td>
<td align="left">Le nombre d’occurrences où l’avis a été marqué comme utile.</td>
</tr>
<tr class="even">
<td align="left">notHelpfulCount</td>
<td align="left">eq, ne</td>
<td align="left">Le nombre d’occurrences où l’avis a été marqué comme inutile.</td>
</tr>
<tr class="odd">
<td align="left">reviewTitle</td>
<td align="left">eq, ne, contains</td>
<td align="left">Le titre de l’avis.</td>
</tr>
<tr class="even">
<td align="left">reviewText</td>
<td align="left">eq, ne, contains</td>
<td align="left">Le contenu textuel de l’avis.</td>
</tr>
<tr class="odd">
<td align="left">responseText</td>
<td align="left">eq, ne, contains</td>
<td align="left">Le contenu textuel de la réponse.</td>
</tr>
<tr class="even">
<td align="left">responseDate</td>
<td align="left">eq, ne</td>
<td align="left">La date à laquelle la réponse a été soumise.</td>
</tr>
<tr class="odd">
<td align="left">deviceRAM</td>
<td align="left">eq, ne, gt, lt, ge, le</td>
<td align="left">La mémoireRAM physique, en Mo.</td>
</tr>
<tr class="even">
<td align="left">deviceStorageCapacity</td>
<td align="left">eq, ne, gt, lt, ge, le</td>
<td align="left">La capacité du disque de stockage principal, en Go.</td>
</tr>
<tr class="odd">
<td align="left">rating</td>
<td align="left">eq, ne, gt, lt, ge, le</td>
<td align="left">L’évaluation de l’application, en étoiles.</td>
</tr>
</tbody>
</table>

<span/> 

### Exemple de requête

Les exemples suivants illustrent plusieurs requêtes de récupération des avis. Remplacez la valeur *applicationId* par l’ID WindowsStore de votre application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US' HTTP/1.1
Authorization: Bearer <your access token>
```

## Réponse


### Corps de la réponse

| Valeur      | Type   | Description                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Valeur      | tableau  | Un tableau d’objets comportant des avis. Pour plus d’informations sur les données de chaque objet, consultez la section [Valeurs d’avis](#review-values) ci-dessous.                                                                                                                                      |
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la demande est défini sur 10000, mais que plus de 10000 lignes de données d’acquisition sont associées à la requête. |
| TotalCount | entier    | Nombre total de lignes des résultats de données pour la requête.                                                                                                                                                                                                                             |

<span/>
 
### Valeurs d’avis

Les éléments du tableau *Value* comportent les valeurs suivantes:

| Valeur                  | Type    | Description                                                                                                                                                                                                                          |
|------------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| date                   | chaîne  | Première date dans la plage de dates de classification. Si la requête était relative à un jour unique, cette valeur correspond à la date associée. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| applicationId          | chaîne  | L’ID WindowsStore de l’application pour laquelle vous récupérez les données de classification.                                                                                                                                                                 |
| applicationName        | chaîne  | Nom d’affichage de l’application.                                                                                                                                                                                                         |
| market                 | chaîne  | Le code pays ISO3166 du marché dans lequel la classification a été soumise.                                                                                                                                                              |
| osVersion              | chaîne  | La version du système d’exploitation sur lequel la classification a été soumise. Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.                                                                                               |
| deviceType             | chaîne  | Le type d’appareil sur lequel la classification a été soumise. Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.                                                                                           |
| isRevised              | Booléen | La valeur **true** indique que l’avis a été révisé; sinon, la valeur **false** est affichée.                                                                                                                                                       |
| packageVersion         | chaîne  | La version du package d’application qui a été passée en revue.                                                                                                                                                                                    |
| deviceModel            | chaîne  | Le type d’appareil sur lequel l’application a été révisée.                                                                                                                                                                                    |
| productFamily          | chaîne  | Le nom de la famille d’appareils. Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.                                                                                                                         |
| deviceScreenResolution | chaîne  | Résolution de l’écran de l’appareil, au format «*largeur*x*hauteur*».                                                                                                                                                                     |
| isTouchEnabled         | Booléen | La valeur **true** indique l’activation de l’interaction tactile; sinon, la valeur **false** est affichée.                                                                                                                                                             |
| reviewerName           | chaîne  | Le nom de réviseur.                                                                                                                                                                                                                   |
| helpfulCount           | nombre  | Le nombre d’occurrences où l’avis a été marqué comme utile.                                                                                                                                                                                   |
| notHelpfulCount        | nombre  | Le nombre d’occurrences où l’avis a été marqué comme inutile.                                                                                                                                                                               |
| reviewTitle            | chaîne  | Le titre de l’avis.                                                                                                                                                                                                             |
| reviewText             | chaîne  | Le contenu textuel de l’avis.                                                                                                                                                                                                     |
| responseText           | chaîne  | Le contenu textuel de la réponse.                                                                                                                                                                                                   |
| responseDate           | chaîne  | La date de soumission d’une réponse.                                                                                                                                                                                                   |
| deviceRAM              | nombre  | La mémoireRAM physique, en Mo.                                                                                                                                                                                                             |
| deviceStorageCapacity  | nombre  | La capacité du disque de stockage principal, en Go.                                                                                                                                                                                     |
| rating                 | nombre  | L’évaluation de l’application, en étoiles.                                                                                                                                                                                                            |

<span/> 

### Exemple de réponse

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

## Rubriques connexes

* [Accéder aux données d’analyse à l’aide des services du Windows Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des acquisitions d’applications](get-app-acquisitions.md)
* [Obtenir les acquisitions de produits in-app](get-in-app-acquisitions.md)
* [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md)
* [Obtenir les classifications des applications](get-app-ratings.md)



<!--HONumber=Jul16_HO1-->


