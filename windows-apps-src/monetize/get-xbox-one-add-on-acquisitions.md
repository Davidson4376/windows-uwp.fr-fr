---
description: Utilisez cette méthode dans l’API d’analytique Microsoft Store pour obtenir des données d’acquisition de module complémentaire agrégation.
title: Obtenir des acquisitions d’extensions Xbox One
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10 uwp, Store, aux services analytique Microsoft Store API, les acquisitions de module complémentaire Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 1387e9adc5d6ef3e7a76b6b2e898c863e8434a9b
ms.sourcegitcommit: e63fbd7a63a7e8c03c52f4219f34513f4b2bb411
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2019
ms.locfileid: "57822904"
---
# <a name="get-xbox-one-add-on-acquisitions"></a>Obtenir des acquisitions d’extensions Xbox One

Utilisez cette méthode dans le Microsoft Store analytique API d’obtenir un module complémentaire agrégation acquisition au format JSON d’une Xbox One jeu qui a été reçus par le biais du portail de développement Xbox (XDP) et disponible dans le tableau de bord XDP Analytique Partner Center.

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Demande


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions``` |


### <a name="request-header"></a>En-tête de requête

| Header        | Type   | Description          |
|---------------|--------|--------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

Le *applicationId* ou *addonProductId* paramètre est obligatoire. Pour récupérer les données d’acquisition de toutes les extensions inscrites dans l’application, spécifiez le paramètre *applicationId*. Pour récupérer les données d’acquisition d’un module complémentaire unique, spécifiez la *addonProductId* paramètre. Si vous spécifiez les deux valeurs, le paramètre *applicationId* est ignoré.

| Paramètre        | Type   |  Description      |  Obligatoire  |
|---------------|--------|---------------|------|
| applicationId | chaîne | Le *productId* du jeu Xbox One pour lequel vous récupérez des données d’acquisition. Notez que cela est l’ID de Store et pas XDP ID de produit. Pour obtenir le *productId* de votre jeu, accédez à votre jeu dans le programme d’Analytique XDP et récupérer le *productId* à partir de l’URL. Vous pouvez également, si vous téléchargez vos données d’acquisitions dans le rapport d’analytique de partenaires, les *productId* est inclus dans le fichier .tsv. |  Oui  |
| addonProductId | chaîne | Le *productId* du module complémentaire pour lequel vous souhaitez récupérer des données d’acquisition.  | Oui  |
| startDate | date | Dans la plage de dates, date de début de la récupération des données d’acquisition. La valeur par défaut est la date actuelle. |  Non  |
| endDate | date | Dans la plage de dates, date de fin de la récupération des données d’acquisition. La valeur par défaut est la date actuelle. |  Non  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |
| Filter |chaîne  | <p>Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction contient un nom de champ à partir du corps de la réponse et une valeur qui sont associés avec les opérateurs eq ou ne et instructions peuvent être combinées à l’aide d’et ou ou. Valeurs de chaîne doivent être entourées de guillemets simples dans le paramètre de filtre. Par exemple, filtrer = marché eq eq 'US' et le sexe suis ».</p> <p>Vous pouvez spécifier les champs suivants dans le corps de réponse :</p> <ul><li><strong>acquisitionType</strong></li><li><strong>age</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>sandboxId</strong></li></ul>| Non   |
| aggregationLevel | chaîne | Indique la plage de temps pendant laquelle récupérer les données agrégées. Il peut s’agit des chaînes suivantes : <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Par défaut, la valeur est <strong>day</strong>. | Non |
| orderby | chaîne | Instruction qui commande les valeurs de données de résultats pour chaque acquisition d’extension. La syntaxe est <em>orderby = champ [order], [order],...</em> Le paramètre <em>field</em> peut comporter l’une des chaînes suivantes :<ul><li><strong>date</strong></li><li><strong>acquisitionType</strong></li><li><strong>age</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p>Le paramètre <em>order</em>, facultatif, peut comporter les valeurs <strong>asc</strong> ou <strong>desc</strong> afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est <strong>asc</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em> : <em>orderby=date,market</em></p> |  Non  |
| groupby | chaîne | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants :<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>addonProductName</strong></li><li><strong>acquisitionType</strong></li><li><strong>age</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>paymentInstrumentType</strong></li><li><strong>sandboxId</strong></li><li><strong>xboxTitleIdHex</strong></li></ul><p>Les lignes de données renvoyées comportent les champs spécifiés dans le paramètre <em>groupby</em>, ainsi que dans les paramètres suivants :</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>addonProductId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p>Le paramètre <em>groupby</em> peut être utilisé avec le paramètre <em>aggregationLevel</em>. Par exemple :  <em>&amp;groupby = age, marché&amp;aggregationLevel = semaine</em></p> |  Non  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre quelques requêtes d’obtention de données d’acquisition d’extensions. Remplacez le *addonProductId* et *applicationId* valeurs avec l’ID de Store approprié pour votre application ou un module complémentaire.

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

| Value      | Type   | Description         |
|------------|--------|------------------|
| Value      | tableau  | Tableau d’objets contenant des données d’acquisition agrégées d’extensions. Pour plus d’informations sur les données incluses dans chaque objet, voir [Valeurs d’acquisition d’extensions](#add-on-acquisition-values) ci-dessous.                                                                                                              |
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête a la valeur 10000, mais qu’il existe plus de 10 000 lignes de données d’acquisition d’extensions pour la demande. |
| TotalCount | entier    | Nombre total de lignes dans les résultats de la requête.    |


<span id="add-on-acquisition-values" />

### <a name="add-on-acquisition-values"></a>Valeurs d’acquisition d’extensions

Les éléments du tableau *Value* comportent les valeurs suivantes :

| Value               | Type    | Description        |
|---------------------|---------|---------------------|
| date                | chaîne  | Première date dans la plage de dates des données d’acquisition. Si la requête spécifiait un jour précis, cette valeur correspond à la date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| addonProductId      | chaîne  | Le *productId* du module complémentaire pour lequel vous récupérez des données d’acquisition.                                                                                                                                                                 |
| addonProductName    | chaîne  | Nom d’affichage de l’extension. Cette valeur s’affiche uniquement dans les données de réponse si la *aggregationLevel* paramètre est défini sur **jour**, sauf si vous spécifiez le **addonProductName** champ dans le *groupby* paramètre.                                                                                                                                                                                                            |
| applicationId       | chaîne  | Le *productId* de l’application pour laquelle vous souhaitez récupérer des données d’acquisition de module complémentaire. Notez que cela est l’ID de Store et pas XDP ID de produit.                                                                                                                                                           |
| applicationName     | chaîne  | Le nom complet du jeu.                                                                                                                                                                                                             |
| deviceType          | chaîne  | <p>L'une des chaînes suivantes qui spécifie le type d’appareil ayant effectué l'acquisition :</p> <ul><li>« PC »</li><li>« Phone »</li><li>« Console »</li><li>« IoT »</li><li>« Serveur »</li><li>« Tablet PC »</li><li>« HOLOGRAPHIQUE »</li><li>« Inconnu »</li></ul>                                                                                                  |
| storeClient         | chaîne  | <p>Une des chaînes suivantes qui indique la version du Store sur laquelle l’acquisition s’est produite :</p> <ul><li>« Windows Phone Store (client) »</li><li>« Microsoft Store (client) » (ou « Windows Store (client) » si l’interrogation des données avant le 23 mars 2018)</li><li>« Microsoft Store (web) » (ou « Windows Store (web) » si l’interrogation des données avant le 23 mars 2018)</li><li>« Volume d’achat par les organisations »</li><li>« Autre »</li></ul>                                                                                            |
| osVersion           | chaîne  | La version de système d’exploitation sur laquelle l’acquisition s’est produite. Pour cette méthode, cette valeur est toujours « Windows 10 ».                                                                                                   |
| market              | chaîne  | Le code pays ISO 3166 du marché dans lequel l’acquisition s’est produite.                                                                                                                                                                  |
| gender              | chaîne  | <p>L'une des chaînes suivantes qui spécifie le sexe de l'utilisateur ayant effectué l’acquisition :</p> <ul><li>"m"</li><li>"f"</li><li>« Inconnu »</li></ul>                                                                                                    |
| age            | chaîne  | <p>L'une des chaînes suivantes qui indique le groupe d'âge de l'utilisateur ayant effectué l’acquisition :</p> <ul><li>« inférieur à 13 »</li><li>"13-17"</li><li>"18-24"</li><li>"25-34"</li><li>"35-44"</li><li>"44-55"</li><li>« supérieur à 55 »</li><li>« Inconnu »</li></ul>                                                                                                 |
| acquisitionType     | chaîne  | <p>Une des chaînes suivantes qui indique le type d'acquisition :</p> <ul><li>« Gratuit »</li><li>« Évaluation »</li><li>« Payante »</li><li>« Code de promotion »</li><li>« Produits »</li><li>« Produits d’abonnement »</li><li>« Audience privé »</li><li>« Pre Order »</li><li>« Xbox Game Pass » (ou « Game Pass » si l’interrogation des données avant le 23 mars 2018)</li><li>« Disque »</li><li>« Code prépayé »</li><li>« Facturé Pre ordre »</li><li>« Annulée Pre ordre »</li><li>« Échec de la commande de Pre »</li></ul>                                                                                                    |
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
