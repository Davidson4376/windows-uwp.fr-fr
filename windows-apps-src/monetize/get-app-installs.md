---
ms.assetid: FAD033C7-F887-4217-A385-089F09242827
description: Utilisez cette méthode dans l’API d’analyse du Microsoft Store pour obtenir les données d’installation agrégées d’une application pour une plage de dates données, et en fonction de filtres facultatifs.
title: Obtenir les installations d’applications
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, services du Microsoft Store, API d'analyse du Microsoft Store, installations d'app
ms.localizationpriority: medium
ms.openlocfilehash: e2dc604e15180296e84d6177aa3d9b71a0bf82dc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594794"
---
# <a name="get-app-installs"></a>Obtenir les installations d’applications


Utilisez cette méthode dans l’API d’analyse du Microsoft Store pour obtenir les données d’installation agrégées au format JSON d’une application pour une plage de dates données, et en fonction de filtres facultatifs. Ces informations sont également disponibles dans le [rapport d’Acquisitions](../publish/acquisitions-report.md) dans Partner Center.

## <a name="prerequisites"></a>Conditions préalables


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/installs``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| applicationId | chaîne | L’[ID Store](in-app-purchases-and-trials.md#store-ids) de l’app pour laquelle vous souhaitez récupérer des données d’installation.  |  Oui  |
| startDate | date | Dans la plage de dates, la date de début de la récupération des données d'installation. La valeur par défaut est la date actuelle. |  Non  |
| endDate | date | Dans la plage de dates, la date de fin de la récupération des données d'installation. La valeur par défaut est la date actuelle. |  Non  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |
| filter | chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ Nom dans le corps de la réponse et une valeur, qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. Vous pouvez spécifier les champs suivants dans le corps de réponse :<p/><ul><li><strong>sur le marché</strong></li><li><strong>osVersion</strong></li><li><strong>type d’appareil</strong></li><li><strong>PackageVersion</strong></li></ul> | Non   |
| aggregationLevel | chaîne | Indique la plage de temps pendant laquelle récupérer les données agrégées. Il peut s’agit des chaînes suivantes : <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Par défaut, la valeur est <strong>day</strong>. | Non |
| orderby | chaîne | Une instruction qui commande les valeurs de données de résultats pour chaque installation. Syntaxe : <em>orderby=field [order],field [order],...</em>. Le <em>champ</em> paramètre peut être l'un des champs suivants dans le corps de réponse :<p/><ul><li><strong>ApplicationName</strong></li><li><strong>Date</strong><li><strong>type d’appareil</strong></li><li><strong>sur le marché</strong></li><li><strong>osVersion</strong></li><li><strong>PackageVersion</strong></li><li><strong>successfulInstallCount</strong></li></ul><p>Le paramètre <em>order</em>, facultatif, peut comporter les valeurs <strong>asc</strong> ou <strong>desc</strong> afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est <strong>asc</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em> : <em>orderby=date,market</em></p> |  Non  |
| groupby | chaîne | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants dans le corps de réponse :<p/><ul><li><strong>ApplicationName</strong></li><li><strong>Date</strong><li><strong>type d’appareil</strong></li><li><strong>sur le marché</strong></li><li><strong>osVersion</strong></li><li><strong>PackageVersion</strong></li></ul><p>Les lignes de données renvoyées comportent les champs spécifiés dans le paramètre <em>groupby</em>, ainsi que dans les paramètres suivants :</p><ul><li><strong>Date</strong></li><li><strong>applicationId</strong></li><li><strong>successfulInstallCount</strong></li></ul><p>Le paramètre <em>groupby</em> peut être utilisé avec le paramètre <em>aggregationLevel</em>. Par exemple : <em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  Non  |

 
### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre plusieurs demandes d’obtention des données d’installation d’applications. Remplacez la valeur *applicationId* par l’ID Windows Store de votre application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/installs?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/installs?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Valeur      | tableau  | Tableau d’objets contenant les données d'installation agrégées. Pour plus d’informations sur les données incluses dans chaque objet, voir le tableau suivant.                                                                                                                      |
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur 10000, mais que plus de 10000 lignes de données d’installation sont associées à la requête. |
| TotalCount | entier    | Nombre total de lignes dans les résultats de la requête.    |


Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur               | Type   | Description                           |
|---------------------|--------|-------------------------------------------|
| date                | chaîne | Première date dans la plage de dates d'installation. Si la requête spécifiait un jour précis, cette valeur correspond à la date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| applicationId       | chaîne | L’ID Windows Store de l’application pour laquelle vous récupérez les données d'installation.     |
| applicationName     | chaîne | Nom d’affichage de l’application.     |
| deviceType          | chaîne | L'une des chaînes suivantes qui spécifie le type d’appareil ayant effectué l’installation :<p/><ul><li><strong>PC</strong></li><li><strong>Téléphone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>HOLOGRAPHIQUE</strong></li><li><strong>Inconnu</strong></li></ul>  |
| packageVersion           | chaîne | La version du package qui a été installé.  |
| osVersion           | chaîne | L'une des chaînes suivantes qui spécifie la version du système d’exploitation sur lequel l’installation s’est produite :<p/><ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Inconnu</strong></li></ul>   |
| market              | chaîne | Le code pays ISO 3166 du marché dans lequel l’installation s’est produite.    |
| successfulInstallCount | nombre | Le nombre d’installations réussies qui se sont produites durant le niveau d’agrégation spécifié.     |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "date": "2016-02-01",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "packageVersion": "1.0.0.0",
      "deviceType": "Phone",
      "market": "IT",
      "osVersion": "Windows Phone 8.1",
      "successfulInstallCount": 1
    }
  ],
  "@nextLink": "installs?applicationId=9NBLGGGZ5QDR&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1&orderby=date desc",
  "TotalCount":23012
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Rapports d’installations](../publish/installs-report.md)
* [Accéder aux données d’analytique à l’aide des services de Microsoft Store](access-analytics-data-using-windows-store-services.md)
