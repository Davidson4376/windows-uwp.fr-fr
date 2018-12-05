---
description: Utilisez cette méthode dans l’API d’analytique Microsoft Store pour obtenir des données d’acquisition agrégées d’extensions.
title: Obtenir des acquisitions d’extensions XboxOne
ms.date: 10/18/2018
ms.topic: article
keywords: Windows 10, uwp, services du Windows Store, analytique du Microsoft Store API, Xbox One acquisitions de modules complémentaires
ms.localizationpriority: medium
ms.openlocfilehash: f102d2d692a2307c25dcb95e66d612fc561dec70
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8691010"
---
# <a name="get-xbox-one-add-on-acquisitions"></a>Obtenir des acquisitions d’extensions XboxOne

Utilisez cette méthode dans le Microsoft Store analytique API pour obtenir les données d’acquisition agrégées d’extensions au format JSON pour une console Xbox One jeu intégré via le portail de développement Xbox (XDP) et disponible dans le tableau de bord XDP Analytique l’espace partenaires.

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description          |
|---------------|--------|--------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

Le paramètre *applicationId* ou *addonProductId* est obligatoire. Pour récupérer les données d’acquisition de toutes les extensions inscrites dans l’application, spécifiez le paramètre *applicationId*. Pour récupérer des données d’acquisition d’une seule extension, spécifiez le paramètre *addonProductId* . Si vous spécifiez les deuxvaleurs, le paramètre *applicationId* est ignoré.

| Paramètre        | Type   |  Description      |  Requis  |
|---------------|--------|---------------|------|
| applicationId | chaîne | *ProductId* du jeu Xbox One pour lequel vous récupérez des données d’acquisition. Pour obtenir l' *ID du produit* de votre jeu, accédez à votre jeu dans le programme d’Analytique XDP et récupérer l' *ID du produit* à partir de l’URL. Par ailleurs, si vous téléchargez vos données d’acquisitions à partir du rapport analytique de l’espace partenaires, l' *ID du produit* est inclus dans le fichier .tsv. |  Oui  |
| addonProductId | chaîne | L' *ID du produit* de l’extension pour laquelle vous souhaitez récupérer des données d’acquisition.  | Oui  |
| startDate | date | Dans la plage de dates, date de début de la récupération des données d’acquisition. La valeur par défaut est la date du jour. |  Non  |
| endDate | date | Dans la plage de dates, date de fin de la récupération des données d’acquisition. La valeur par défaut est la date du jour. |  Non  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |
| filter |chaîne  | <p>Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ Nom dans le corps de la réponse et une valeur, qui sont associés aux opérateurs eq ou ne, et les instructions peuvent être combinées à l’aide des opérateurs and ou or. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre filter. par exemple, filter=market eq 'US' and gender eq 'm'.</p> <p>Vous pouvez spécifier les champs suivants dans le corps de réponse:</p> <ul><li><strong>acquisitionType</strong></li><li><strong>age</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>sandboxId</strong></li></ul>| Non   |
| aggregationLevel | chaîne | Indique la plage de temps pendant laquelle récupérer les données agrégées. Il peut s’agit des chaînes suivantes : <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Par défaut, la valeur est <strong>day</strong>. | Non |
| orderby | chaîne | Instruction qui commande les valeurs de données de résultats pour chaque acquisition d’extension. La syntaxe est <em>orderby = field [order], [order], de champ …</em> Le paramètre de <em>champ</em> peut être une des chaînes suivantes:<ul><li><strong>date</strong></li><li><strong>acquisitionType</strong></li><li><strong>age</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p>Le paramètre <em>order</em>, facultatif, peut comporter les valeurs <strong>asc</strong> ou <strong>desc</strong> afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est <strong>asc</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em>: <em>orderby=date,market</em></p> |  Non  |
| groupby | chaîne | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants:<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>addonProductName</strong></li><li><strong>acquisitionType</strong></li><li><strong>age</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>paymentInstrumentType</strong></li><li><strong>sandboxId</strong></li><li><strong>xboxTitleIdHex</strong></li></ul><p>Les lignes de données renvoyées comportent les champs spécifiés dans le paramètre <em>groupby</em>, ainsi que dans les paramètres suivants :</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>addonProductId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p>Le paramètre <em>groupby</em> peut être utilisé avec le paramètre <em>aggregationLevel</em>. Par exemple: <em> &amp;groupby = âge, marché&amp;aggregationLevel = week</em></p> |  Non  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre quelques requêtes d’obtention de données d’acquisition d’extensions. Remplacez les valeurs *addonProductId* et *applicationId* par l’ID Windows Store pour votre extension ou application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions?addonProductId=BRRT4NJ9B3D2&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions?applicationId=BRRT4NJ9B3D1&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions?addonProductId=BRRT4NJ9B3D2&startDate=1/1/2015&endDate=7/3/2015&top=100&skip=0&filter=market ne 'US' and gender ne 'Unknown' and gender ne 'm' and market ne 'NO' and age ne '>55' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type   | Description         |
|------------|--------|------------------|
| Valeur      | tableau  | Tableau d’objets contenant des données d’acquisition agrégées d’extensions. Pour plus d’informations sur les données incluses dans chaque objet, voir [Valeurs d’acquisition d’extensions](#add-on-acquisition-values) ci-dessous.                                                                                                              |
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête a la valeur 10000, mais qu’il existe plus de 10000lignes de données d’acquisition d’extensions pour la demande. |
| TotalCount | entier    | Nombre total de lignes dans les résultats de données de la requête.    |


<span id="add-on-acquisition-values" />

### <a name="add-on-acquisition-values"></a>Valeurs d’acquisition d’extensions

Les éléments du tableau *Value* comportent les valeurs suivantes:

| Valeur               | Type    | Description        |
|---------------------|---------|---------------------|
| date                | chaîne  | Première date dans la plage de dates des données d’acquisition. Si la requête était relative à un jour unique, cette valeur correspond à la date associée. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| addonProductId      | chaîne  | L' *ID du produit* de l’extension pour laquelle vous récupérez les données d’acquisition.                                                                                                                                                                 |
| addonProductName    | chaîne  | Nom d’affichage de l’extension. Cette valeur s’affiche uniquement dans les données de réponse si le paramètre *aggregationLevel* est défini sur le **jour**, sauf si vous spécifiez le champ **addonProductName** dans le paramètre *groupby* .                                                                                                                                                                                                            |
| applicationId       | chaîne  | L' *ID du produit* de l’application pour laquelle vous souhaitez récupérer des données d’acquisition d’extension.                                                                                                                                                           |
| applicationName     | chaîne  | Le nom complet du jeu.                                                                                                                                                                                                             |
| deviceType          | chaîne  | <p>L'une des chaînes suivantes qui spécifie le type d’appareil ayant effectué l'acquisition:</p> <ul><li>«PC»</li><li>«Téléphone»</li><li>«Console»</li><li>«IoT»</li><li>«Serveur»</li><li>«Tablette»</li><li>«HOLOGRAPHIQUE»</li><li>«Inconnu»</li></ul>                                                                                                  |
| storeClient         | chaîne  | <p>Une des chaînes suivantes qui indique la version du Store sur laquelle l’acquisition s’est produite:</p> <ul><li>«Windows Phone Store (client)»</li><li>«Microsoft Store (client)» (ou «Windows Store (client)» si l’interrogation pour les données avant le 23 mars 2018)</li><li>«Microsoft Store (web)» (ou «Windows Store (web)» si l’interrogation pour les données avant le 23 mars 2018)</li><li>«Volume purchase by organizations»</li><li>«Autre»</li></ul>                                                                                            |
| osVersion           | chaîne  | La version de système d’exploitation sur laquelle l’acquisition s’est produite. Pour cette méthode, cette valeur est toujours «Windows 10».                                                                                                   |
| market              | chaîne  | Le code pays ISO3166 du marché dans lequel l’acquisition s’est produite.                                                                                                                                                                  |
| gender              | chaîne  | <p>L'une des chaînes suivantes qui spécifie le sexe de l'utilisateur ayant effectué l’acquisition:</p> <ul><li>«m»</li><li>«f»</li><li>«Inconnu»</li></ul>                                                                                                    |
| age            | chaîne  | <p>L'une des chaînes suivantes qui indique le groupe d'âge de l'utilisateur ayant effectué l’acquisition:</p> <ul><li>«moins de 13»</li><li>«13: 17»</li><li>«18 à 24»</li><li>«25-34»</li><li>«35-44»</li><li>«44: 55»</li><li>«greater than 55»</li><li>«Inconnu»</li></ul>                                                                                                 |
| acquisitionType     | chaîne  | <p>Une des chaînes suivantes qui indique le type d'acquisition:</p> <ul><li>«Gratuitement»</li><li>«Essai»</li><li>«Payé»</li><li>«Code promotionnel»</li><li>«PIA»</li><li>«Abonnement PIA»</li><li>«Public privé»</li><li>«Pre ordre»</li><li>«Xbox Game Pass» (ou «Game Pass» si l’interrogation pour les données avant le 23 mars 2018)</li><li>«Disques»</li><li>«Code prépayé»</li><li>«Facturé Pre ordre»</li><li>«Annulé Pre ordre»</li><li>«Failed Pre ordre»</li></ul>                                                                                                    |
| acquisitionQuantity | entier | Le nombre d’acquisitions qui se sont produites.                        |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "date": "2018-10-18",
      "addonProductId": " BRRT4NJ9B3D2",
      "addonProductName": "Contoso add-on 7",
      "applicationId": "BRRT4NJ9B3D1",
      "applicationName": "Contoso Demo",
      "deviceType": "Console",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows 10",
      "market": "GB",
      "gender": "m",
      "age": "50orover",
      "acquisitionType": "iap",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "addonacquisitions?applicationId=BRRT4NJ9B3D1&addonProductId=&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 33677
}
```
