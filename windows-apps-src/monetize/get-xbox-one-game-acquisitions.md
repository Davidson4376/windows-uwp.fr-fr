---
author: Xansky
ms.assetid: C1E42E8B-B97D-4B09-9326-25E968680A0F
description: Utilisez cette méthode dans l’API d’analyse du MicrosoftStore pour obtenir les données d’acquisition agrégées d’un jeu Xbox One pour une plage de dates données, et en fonction de filtres facultatifs.
title: Obtenir des acquisitions de jeu Xbox One
ms.author: mhopkins
ms.date: 03/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, services du MicrosoftStore, API d’analyse du MicrosoftStore, acquisitions de jeu Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 39d932a49e573d55a0ccb9cb69568006feede8a7
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/19/2018
ms.locfileid: "4959672"
---
# <a name="get-xbox-one-game-acquisitions"></a>Obtenir des acquisitions de jeu Xbox One

Utilisez cette méthode dans l'API d'analyse du MicrosoftStore pour obtenir des données d’acquisition agrégées au format JSON pour un jeu Xbox One intégré via le portail de développement Xbox (XDP) et disponible dans le tableau de bord du centre de développement Analyse XDP.

## <a name="prerequisites"></a>Éléments prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/acquisitions``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Requis  
|---------------|--------|---------------|------|
| applicationId | chaîne | L'ID produit du jeu Xbox One pour lequel vous récupérez des données d’acquisition. Pour obtenir l’ID produit de votre jeu, accédez à votre jeu dans le portail de développement Xbox (XDP) et récupérez l’ID produit à partir de l’URL. De même, si vous téléchargez vos données d’acquisitions à partir du rapport d'analyse du centre de développement Windows, l’ID produit sera inclus dans le fichier .tsv.  |  Oui  |
| startDate | date | Dans la plage de dates, la date de début de la récupération des données d’acquisition. La valeur par défaut est la date actuelle. |  Non  |
| endDate | date | Dans la plage de dates, la date de fin de la récupération des données d’acquisition. La valeur par défaut est la date actuelle. |  Non  |
| top | int | Le nombre de rangées de données à renvoyer. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |
| filter | chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ Nom dans le corps de la réponse et une valeur, qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. par exemple, *filter=market eq 'US' and gender eq 'm'*. <p/><p/>Vous pouvez spécifier les champs suivants dans le corps de réponse:<p/><ul><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>sandboxId</strong></li></ul> | Non   |
| aggregationLevel | chaîne | Indique la plage de temps pendant laquelle récupérer les données agrégées. Il peut s’agit des chaînes suivantes : <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Par défaut, la valeur est <strong>day</strong>. | Non |
| orderby | chaîne | Une instruction qui commande les valeurs de données de résultats pour chaque acquisition. La syntaxe est <em>orderby=field [order],field [order],...</em>. Le paramètre <em>field</em> peut être l’une des chaînes suivantes:<ul><li><strong>date</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>paymentInstrumentType</strong></li><li><strong>sandboxId</strong></li><li><strong>xboxTitleIdHex</strong></li><li><strong>purchasePriceUSDAmount</strong></li><li><strong>taxUSDAmount</strong></li></ul><p>Le paramètre facultatif <em>order</em> peut avoir la valeur <strong>asc</strong> ou <strong>desc</strong> pour spécifier l’ordre croissant ou décroissant de chaque champ. La valeur par défaut est <strong>asc</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em>: <em>orderby=date,market</em></p> |  Non  |
| groupby | chaîne | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants:<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>paymentInstrumentType</strong></li><li><strong>sandboxId</strong></li><li><strong>xboxTitleIdHex</strong></li><li><strong>purchasePriceUSDAmount</strong></li><li><strong>taxUSDAmount</strong></li></ul><p>Les lignes de données renvoyées comportent les champs spécifiés dans le paramètre <em>groupby</em>, ainsi que dans les paramètres suivants:</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p>Le paramètre <em>groupby</em> peut être utilisé avec le paramètre <em>aggregationLevel</em>. Par exemple: <em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  Non  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre plusieurs demandes d’obtention des données d’acquisition du jeu Xbox One. Remplacez la valeur *applicationId* par l’ID Store de votre jeu.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/acquisitions?applicationId=BRRT4NJ9B3D1&startDate=1/1/2017&endDate=2/1/2017&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/acquisitions?applicationId=BRRT4NJ9B3D1&startDate=8/1/2017&endDate=8/31/2017&skip=0&filter=market eq 'US' and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Valeur      | array  | Tableau d’objets contenant des données d’acquisition agrégées pour le jeu. Pour plus d’informations sur les données de chaque objet, consultez la section [Valeurs d’acquisition](#acquisition-values) ci-dessous.                                                                                                                      |
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la demande est défini sur 10000, mais que plus de 10000 lignes de données d’acquisition sont associées à la requête. |
| TotalCount | entier    | Nombre total de lignes des résultats de données pour la requête.              |


### <a name="acquisition-values"></a>Valeurs d’acquisition

Les éléments du tableau *Value* comportent les valeurs suivantes:

| Valeur               | Type   | Description                           |
|---------------------|--------|-------------------------------------------|
| date                | chaîne | Première date dans la plage de dates des données d’acquisition. Si la requête était relative à un jour unique, cette valeur correspond à la date associée. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| applicationId       | chaîne | L'ID produit du jeu Xbox One pour lequel vous récupérez des données d’acquisition. |
| applicationName     | chaîne | Le nom complet du jeu.       |
| acquisitionType     | chaîne | Une des chaînes suivantes qui indique le type d'acquisition:<ul><li><strong>Free</strong></li><li><strong>Essai</strong></li><li><strong>Payant</strong></li><li><strong>Code promotionnel</strong></li><li><strong>Iap</strong></li><li><strong>Abonnement Iap</strong></li><li><strong>Public privé</strong></li><li><strong>Ordre des versions antérieures</strong></li><li><strong>Xbox Game Pass</strong> (ou <strong>Game Pass</strong> si vous interrogez des données antérieures au 23mars2018)</li><li><strong>Disque</strong></li><li><strong>Code prépayé</strong></li><li><strong>Ordre Pre facturés</strong></li><li><strong>Commande de pré annulée</strong></li><li><strong>Ordre Pre ayant échoué</strong></li></ul>    |
| ageGroup            | chaîne | L'une des chaînes suivantes qui indique le groupe d'âge de l'utilisateur ayant effectué l’acquisition:<ul><li><strong>less than 13</strong></li><li><strong>13-17</strong></li><li><strong>18-24</strong></li><li><strong>25-34</strong></li><li><strong>35-44</strong></li><li><strong>44-55</strong></li><li><strong>greater than 55</strong></li><li><strong>Unknown</strong></li></ul>     |
| deviceType          | chaîne | L'une des chaînes suivantes qui spécifie le type d’appareil ayant effectué l'acquisition:<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Serveur</strong></li><li><strong>Tablette</strong></li><li><strong>Holographique</strong></li><li><strong>Inconnu</strong></li></ul>  |
| gender              | chaîne | L'une des chaînes suivantes qui spécifie le sexe de l'utilisateur ayant effectué l’acquisition:<ul><li><strong>m</strong></li><li><strong>f</strong></li><li><strong>Unknown</strong></li></ul>     |
| market              | chaîne | Le code pays ISO3166 du marché dans lequel l’acquisition s’est produite.  |
| osVersion           | chaîne | La version de système d’exploitation sur laquelle l’acquisition s’est produite. Pour cette méthode, cette valeur est toujours **Windows10**.</li></ul>    |
| paymentInstrumentType           | chaîne | L'une des chaînes suivantes qui indique l'instruction de paiement utilisée pour l’acquisition:<ul><li><strong>Cartes de crédit</strong></li><li><strong>Carte de débit direct</strong></li><li><strong>Achat déduit</strong></li><li><strong>Solde MS</strong></li><li><strong>Opérateur mobile</strong></li><li><strong>Virement bancaire en ligne</strong></li><li><strong>PayPal</strong></li><li><strong>Division de la transaction</strong></li><li><strong>Utilisation de jeton</strong></li><li><strong>Aucun montant payé</strong></li><li><strong>eWallet</strong></li><li><strong>Inconnue</strong></li></ul>    |
| sandboxId              | chaîne | L’ID de bac à sable créé pour le jeu. Cet élément peut comporter la valeur **RETAIL** ou l'ID de bac à sable privé.  |
| storeClient         | chaîne | Une des chaînes suivantes qui indique la version du Store sur laquelle l’acquisition s’est produite:<ul><li>**Windows Phone Store (client)**</li><li>**MicrosoftStore (client) ** (ou **Windows Store (client)** si l’interrogation pour les données a été effectuée avant le 23mars2018)</li><li>**MicrosoftStore (web) ** (ou **Windows Store (web)** si l’interrogation pour les données a été effectuée avant le 23mars2018)</li><li>**Volume purchase by organizations**</li><li>**Autres**</li></ul>                             |
| xboxTitleIdHex              | chaîne | L'ID de titre Xbox Live (représenté dans la valeur hexadécimale) attribué par le portail de développement Xbox (XDP) pour les jeux prenant en charge Xbox Live.  |
| acquisitionQuantity | nombre | Le nombre d’acquisitions qui se sont produites durant le niveau d’agrégation spécifié.     |
| purchasePriceUSDAmount | nombre | Le montant payé par le client pour l’acquisition, converti en USD, selon le taux de change mensuel.    |
| taxUSDAmount     | nombre | Le montant des taxes appliqué à l’acquisition, converti en USD. |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "date": "2017-02-01",
      "applicationId": "BRRT4NJ9B3D1 ",
      "applicationName": "Contoso Game",
      "acquisitionType": "Paid",
      "ageGroup": "35-49",
      "deviceType": "Console",
      "gender": "m",
      "market": "US",
      "osVersion": "Windows 10",
      "PaymentInstrumentType": "Credit Card ",
      "sandboxId": "RETAIL",
      "storeClient": "Windows Store (web)",
      "xboxTitleIdHex": "",
      "acquisitionQuantity": 1,
      "purchasePriceUSDAmount": 29.99,
      "taxUSDAmount": 2.99
    }
  ],
  "@nextLink": "xbox/acquisitions?applicationId=BRRT4NJ9B3D1&aggregationLevel=day&startDate=2017/02/01&endDate=2017/03/01&top=1&skip=1&orderby=date desc",
  "TotalCount": 39812
}
```

## <a name="related-topics"></a>Rubriquesassociées

* [Accéder aux données d’analyse à l’aide des services du MicrosoftStore](access-analytics-data-using-windows-store-services.md)
