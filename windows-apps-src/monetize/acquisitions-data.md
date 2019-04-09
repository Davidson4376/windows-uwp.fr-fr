---
description: Utilisez cette méthode dans l’API d’analytique Microsoft Store pour obtenir des données d’agrégation d’acquisition au format JSON pour les applications UWP et des jeux Xbox One qui ont été reçus par le biais du portail de développement Xbox (XDP) et disponible dans le tableau de bord XDP Analytique.
title: Obtenir des données d’acquisitions pour vos applications et jeux
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10, uwp, réseau publicitaire, métadonnées d’application
ms.localizationpriority: medium
ms.openlocfilehash: beca5620f25713e8a07e5dbaf64e955d920702a7
ms.sourcegitcommit: df8e4143e81a1c5fe1aa5f14407b8dd5f155a12e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57829862"
---
# <a name="get-acquisitions-data-for-your-games-and-apps"></a>Obtenir des données d’acquisitions pour vos applications et jeux 
Utilisez cette méthode dans l’API d’analytique Microsoft Store pour obtenir des données d’agrégation d’acquisition au format JSON pour les applications UWP et des jeux Xbox One qui ont été reçus par le biais du portail de développement Xbox (XDP) et disponible dans le tableau de bord XDP Analytique. 

> [!NOTE]
> Cette API ne fournit pas de données d’agrégation quotidiennes avant le 1er octobre 2016. 

## <a name="prerequisites"></a>Prérequis
Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes : 

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md) relatives à l’API d’analyse du Microsoft Store. 
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau. 

## <a name="request"></a>Demande
### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête |
| --- | --- |
| GET | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions` |

### <a name="request-header"></a>En-tête de requête

| Header | Type | Description |
| --- | --- | --- |
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** `<token>`. |

### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre | Type | Description | Obligatoire |
| --- | --- | --- | --- |
| applicationId | chaîne | L'ID produit du jeu Xbox One pour lequel vous récupérez des données d’acquisition. Pour obtenir l’ID de produit de votre jeu, accédez à votre jeu dans le programme d’Analytique XDP et récupérer l’ID de produit dans l’URL. Si vous téléchargez vos données acquisitions à partir du rapport d’analytique de partenaires, l’ID de produit est également incluse dans le fichier .tsv.  | Oui |
| startDate | date | Dans la plage de dates, la date de début de la récupération des données d’acquisition. La valeur par défaut est la date actuelle.  | Non |
| endDate | date | Dans la plage de dates, la date de fin de la récupération des données d’acquisition. La valeur par défaut est la date actuelle.  | Non |
| top | entier | Le nombre de rangées de données à renvoyer. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données.  | Non |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, *haut = 10000 et ignorer = 0* récupère les tout d’abord 10 000 lignes de données, *haut = 10000 et ignorer = 10000* récupère les ensuite 10 000 lignes de données et ainsi de suite.  | Non |
| Filter | chaîne | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ Nom dans le corps de la réponse et une valeur, qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**. Valeurs de chaîne doivent être entourées de guillemets simples dans le paramètre de filtre. Par exemple, *filter=market eq 'US' and gender eq 'm'*.  <br/> Vous pouvez spécifier les champs suivants dans le corps de réponse : <ul><li>**acquisitionType**</li><li>**age**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**sandboxId**</li></ul> | Non |
| aggregationLevel | chaîne | Indique la plage de temps pendant laquelle récupérer les données agrégées. Il peut s’agit des chaînes suivantes : **day**, **week** ou **month**. Par défaut, la valeur est **day**.  | Non |
| orderby | chaîne | Une instruction qui commande les valeurs de données de résultats pour chaque acquisition. La syntaxe est *orderby = champ [order], [order],...* Le paramètre *field* peut comporter l’une des chaînes suivantes : <ul><li>**date**</li><li>**acquisitionType**</li><li>**age**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> Le paramètre *order*, facultatif, peut comporter les valeurs **asc** ou **desc** afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est **asc**. Voici un exemple de chaîne *orderby* : *orderby=date,market*  | Non |
| groupby | chaîne | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants : <ul><li>**date**</li><li>**applicationName**</li><li>**acquisitionType**</li><li>**ageGroup**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> Les lignes de données renvoyées comportent les champs spécifiés dans le paramètre *groupby*, ainsi que dans les paramètres suivants : <ul><li>**date**</li><li>**applicationId**</li><li>**acquisitionQuantity**</li></ul> Le *groupby* paramètre peut être utilisé avec le paramètre aggregationLevel. Par exemple : *& groupby = ageGroup, marché & aggregationLevel = semaine*  | Non |

### <a name="request-example"></a>Exemple de requête
L’exemple suivant illustre plusieurs demandes d’obtention des données d’acquisition du jeu Xbox One. Remplacez le *applicationId* valeur avec l’ID de produit pour votre jeu.  

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions?applicationId=9WZDNCRFHXHT&startDate=1/1/2017&endDate=2/1/2019&top=10&skip=0 HTTP/1.1 
Authorization: Bearer <your access token> 

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions?applicationId=9WZDNCRFHXHT&startDate=1/1/2017&endDate=2/1/2019&skip=0&filter=market eq 'US' and gender eq 'm' HTTP/1.1 
Authorization: Bearer <your access token> 
```

## <a name="response"></a>Réponse

### <a name="response-body"></a>Corps de la réponse
| Value | Type | Description |
| --- | --- | --- |
| Value | tableau | Tableau d’objets contenant des données d’acquisition agrégées pour le jeu. Pour plus d’informations sur les données de chaque objet, consultez la section [Valeurs d’acquisition](#acquisition-values) ci-dessous. |
| @nextLink | chaîne | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la demande est défini sur 10000, mais que plus de 10 000 lignes de données d’acquisition sont associées à la requête. |
| TotalCount | entier | Nombre total de lignes dans les résultats de la requête. |

### <a name="acquisition-values"></a>Valeurs d’acquisition 
Les éléments du tableau *Value* comportent les valeurs suivantes : 

| Value | Type | Description |
| --- | --- | --- |
| date | chaîne | Première date dans la plage de dates des données d’acquisition. Si la requête spécifiait un jour précis, cette valeur correspond à la date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| applicationId | chaîne | L'ID produit du jeu Xbox One pour lequel vous récupérez des données d’acquisition. |
| applicationName | chaîne | Le nom complet du jeu. |
| acquisitionType | chaîne | Une des chaînes suivantes qui indique le type d'acquisition :  <ul><li>**Gratuit**</li><li>**Version d’évaluation**</li><li>**Paid**</li><li>**Code de promotion**</li><li>**Iap**</li><li>**Produits de l’abonnement**</li><li>**Privé public**</li><li>**Ordre de versions antérieures**</li><li>**Xbox Game Pass** (ou **Game Pass** si vous interrogez des données antérieures au 23 mars 2018)</li><li>**Disque**</li><li>**Code prépayé**</li><li>**Ordre de Pre chargée**</li><li>**Commande Pre annulée**</li><li>**Ordre de Pre ayant échoué**</li></ul> |
| age | chaîne | L'une des chaînes suivantes qui indique le groupe d'âge de l'utilisateur ayant effectué l’acquisition : <ul><li>**moins de 13**</li><li>**13-17**</li><li>**18-24**</li><li>**25-34**</li><li>**35-44**</li><li>**44-55**</li><li>**supérieur à 55**</li><li>**Inconnu**</li></ul> |
| deviceType | chaîne | L'une des chaînes suivantes qui spécifie le type d’appareil ayant effectué l'acquisition : <ul><li>**PC**</li><li>**Phone**</li><li>**Console**</li><li>**IoT**</li><li>**Serveur**</li><li>**Tablet**</li><li>**HOLOGRAPHIQUE**</li><li>**Inconnu**</li></ul> |
| gender | chaîne | L'une des chaînes suivantes qui spécifie le sexe de l'utilisateur ayant effectué l’acquisition : <ul><li>**m**</li><li>**f**</li><li>**Inconnu**</li></ul> |
| market | chaîne | Le code pays ISO 3166 du marché dans lequel l’acquisition s’est produite. |
| osVersion | chaîne | La version de système d’exploitation sur laquelle l’acquisition s’est produite. Pour cette méthode, cette valeur est toujours **Windows 10**. |
| paymentInstrumentType | chaîne | L'une des chaînes suivantes qui indique l'instruction de paiement utilisée pour l’acquisition : <ul><li>**Carte de crédit**</li><li>**Carte de débit direct**</li><li>**Achat déduit**</li><li>**Solde de MS**</li><li>**Opérateur mobile**</li><li>**Virement bancaire en ligne**</li><li>**PayPal**</li><li>**Fractionner la Transaction**</li><li>**Échange de jeton**</li><li>**Zéro montant payé**</li><li>**eWallet**</li><li>**Inconnu**</li></ul> |
| sandboxId | chaîne | L’ID de bac à sable créé pour le jeu. Cela peut être la valeur **RETAIL** ou un ID de bac à sable privé. |
| storeClient | chaîne | Une des chaînes suivantes qui indique la version du Store sur laquelle l’acquisition s’est produite : <ul><li>**Windows Phone Store (client)**</li><li>**Microsoft Store (client)**  (ou **Windows Store (client)** si l’interrogation porte sur des données antérieures au 23 mars 2018) </li><li>**Microsoft Store (web)**  (ou **Windows Store (web)** si l’interrogation porte sur des données antérieures au 23 mars 2018) </li><li>**Achat de volume par les organisations**</li><li>**Autres**</li></ul> |
| xboxTitleId | chaîne | L'ID de titre Xbox Live (représenté dans la valeur hexadécimale) attribué par le portail de développement Xbox (XDP) pour les jeux prenant en charge Xbox Live. |
| acquisitionQuantity | nombre | Le nombre d’acquisitions qui se sont produites durant le niveau d’agrégation spécifié. |
| purchasePriceUSDAmount | nombre | Le montant payé par le client pour l’acquisition, converti en USD, selon le taux de change mensuel. |
| purchaseTaxUSDAmount | nombre | Le montant des taxes appliqué à l’acquisition, converti en USD. |
| localCurrencyCode | chaîne | Code de devise locale en fonction du pays du compte de partenaires.  |
| xboxProductId | chaîne | ID de produit Xbox du produit à partir de XDP, le cas échéant.  |
| availabilityId | chaîne | ID de la disponibilité du produit à partir de XDP, le cas échéant.  |
| skuId | chaîne | ID de référence (SKU) du produit à partir de XDP, le cas échéant.  |
| skuDisplayName  | chaîne | Référence (SKU) affiche le nom du produit à partir de XDP, le cas échéant.  |
| xboxParentProductId | chaîne | ID de produit Xbox Parent du produit à partir de XDP, le cas échéant.  |
| parentProductName | chaîne | Nom du produit parent du produit à partir de XDP, le cas échéant.  |
| productTypeName | chaîne | Nom du Type de produit du produit à partir de XDP, le cas échéant.  |
| purchaseTaxType | chaîne | Type de taxe d’achat du produit à partir de XDP, le cas échéant.  |
| purchasePriceLocalAmount | nombre | Acheter montant Local du prix du produit à partir de XDP, le cas échéant.  |
| purchaseTaxLocalAmount | nombre | Acheter le montant des taxes locales du produit à partir de XDP, le cas échéant.  |

### <a name="response-example"></a>Exemple de réponse
L’exemple suivant représente un corps de réponse JSON pour cette requête. 

```JSON
{ 
    "Value": [ 
        { 
            "date": "2019-01-15T01:00:00.0000000Z", 
            "applicationId": "9WZDNCRFHXHT", 
            "applicationName": null, 
            "acquisitionType": "Paid", 
            "age": null, 
            "deviceType": "Phone", 
            "gender": null, 
            "market": "US", 
            "osVersion": "Windows 10", 
            "paymentInstrumentType": null, 
            "sandboxId": "RETAIL", 
            "storeClient": "Microsoft Store (client)", 
            "xboxTitleId": null, 
            "localCurrencyCode": "USD", 
            "xboxProductId": null, 
            "availabilityId": "B42LRTSZ2MCJ", 
            "skuId": "0010", 
            "skuDisplayName": null, 
            "xboxParentProductId": null, 
            "parentProductName": null, 
            "productTypeName": "Game", 
            "purchaseTaxType": "TaxesNotIncluded", 
            "acquisitionQuantity": 1, 
            "purchasePriceUSDAmount": 3.08, 
            "purchasePriceLocalAmount": 3.08, 
            "purchaseTaxUSDAmount": 0.09, 
            "purchaseTaxLocalAmount": 0.09 
        } 
    ], 

    "@nextLink": "acquisitions?applicationId=9WZDNCRFHXHT&aggregationLevel=day&startDate=2017-01-01T08:00:00.0000000Z&endDate=2019-01-16T08:44:15.6045249Z&top=1&skip=1", 
    
    "TotalCount": 12221 
} 
```

 
