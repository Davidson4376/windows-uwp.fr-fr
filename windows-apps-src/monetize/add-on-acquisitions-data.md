---
description: Utilisez cette méthode dans l’API d’analytique Microsoft Store pour obtenir des données d’acquisition de module complémentaire agrégation au format JSON pour les applications UWP et des jeux Xbox One qui ont été reçus par le biais du portail de développement Xbox (XDP) et disponible dans le tableau de bord XDP Analytique Partner Center.
title: Obtenir des données d’acquisitions de module complémentaire pour vos applications et jeux
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10, uwp, réseau publicitaire, métadonnées d’application
ms.localizationpriority: medium
ms.openlocfilehash: 518648d52c613a3dd5f1bca0d34a7f533b59733f
ms.sourcegitcommit: df8e4143e81a1c5fe1aa5f14407b8dd5f155a12e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57829861"
---
# <a name="get-add-on-acquisitions-data-for-your-games-and-apps"></a>Obtenir des données d’acquisitions de module complémentaire pour vos applications et jeux 
Utilisez cette méthode dans l’API d’analytique Microsoft Store pour obtenir des données d’acquisition de module complémentaire agrégation au format JSON pour les applications UWP et des jeux Xbox One qui ont été reçus par le biais du portail de développement Xbox (XDP) et disponible dans le tableau de bord XDP Analytique Partner Center. 

## <a name="prerequisites"></a>Prérequis
Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes : 

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md) relatives à l’API d’analyse du Microsoft Store. 
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau. 

> [!NOTE]
> Cette API ne fournit pas de données d’agrégation quotidiennes avant le 1er octobre 2016. 

## <a name="request"></a>Demande

### <a name="request-syntax"></a>Syntaxe de la requête
| Méthode | URI de requête |
| --- | --- | 
| GET | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions` |

### <a name="request-header"></a>En-tête de requête 
| Header | Type | Description | 
| --- | --- | --- |
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** `<token>`. |

### <a name="request-parameters"></a>Paramètres de la requête
Le *applicationId* ou *addonProductId* paramètre est obligatoire. Pour récupérer les données d’acquisition de toutes les extensions inscrites dans l’application, spécifiez le paramètre *applicationId*. Pour récupérer les données d’acquisition d’un module complémentaire unique, spécifiez la *addonProductId* paramètre. Si vous spécifiez les deux valeurs, le paramètre *applicationId* est ignoré. 

| Paramètre | Type | Description | Obligatoire | 
| --- | --- | --- | --- |
| applicationId | chaîne | Le *productId* du jeu Xbox One pour lequel vous récupérez des données d’acquisition. Pour obtenir le *productId* de votre jeu, accédez à votre jeu dans le programme d’Analytique XDP et récupérer le *productId* à partir de l’URL. Vous pouvez également, si vous téléchargez vos données d’acquisitions dans le rapport d’analytique de partenaires, les *productId* est inclus dans le fichier .tsv. | Oui |
| addonProductId | chaîne | Le *productId* du module complémentaire pour lequel vous souhaitez récupérer des données d’acquisition. | Oui |
| startDate | date | Dans la plage de dates, date de début de la récupération des données d’acquisition. La valeur par défaut est la date actuelle. | Non |
| endDate | date | Dans la plage de dates, date de fin de la récupération des données d’acquisition. La valeur par défaut est la date actuelle. | Non |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. | Non |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. | Non |
| Filter | chaîne | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction contient un nom de champ à partir du corps de la réponse et une valeur qui sont associés avec les opérateurs eq ou ne et instructions peuvent être combinées à l’aide d’et ou ou. Valeurs de chaîne doivent être entourées de guillemets simples dans le paramètre de filtre. Par exemple, filtrer = marché eq eq 'US' et le sexe suis ». <br/> Vous pouvez spécifier les champs suivants dans le corps de réponse : <ul><li>**acquisitionType**</li><li>**age**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**sandboxId**</li></ul> | Non |
| aggregationLevel | chaîne | Indique la plage de temps pendant laquelle récupérer les données agrégées. Il peut s’agit des chaînes suivantes : **day**, **week** ou **month**. Par défaut, la valeur est **day**. | Non |
| orderby | chaîne | Instruction qui commande les valeurs de données de résultats pour chaque acquisition d’extension. La syntaxe est *orderby = champ [order], [order],...* Le paramètre *field* peut comporter l’une des chaînes suivantes : <ul><li>**date**</li><li>**acquisitionType**</li><li>**age**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**orderName**</li></ul> Le paramètre de commande est facultatif et peut être **asc** ou **desc** pour spécifier par ordre croissant ou décroissant pour chaque champ. La valeur par défaut est **asc**. <br/> Voici un exemple de chaîne *orderby* : *orderby=date,market* | Non |
| groupby | chaîne | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants : <ul><li>**date**</li><li>**applicationName**</li><li>**addonProductName**</li> <li>**acquisitionType**</li><li>**age**</li> <li>**storeClient**</li><li>**gender**</li> <li>**market**</li> <li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> Les lignes de données renvoyées comportent les champs spécifiés dans le paramètre *groupby*, ainsi que dans les paramètres suivants : <ul><li>**date**</li><li>**applicationId**</li><li>**addonProductId**</li><li>**acquisitionQuantity**</li></ul> Le paramètre groupby peut être utilisé avec le *aggregationLevel* paramètre. Par exemple : *& groupby = âge, marché & aggregationLevel = semaine* | Non |

### <a name="request-example"></a>Exemple de requête
L’exemple suivant illustre quelques requêtes d’obtention de données d’acquisition d’extensions. Remplacez le *addonProductId* et *applicationId* valeurs avec l’ID de Store approprié pour votre application ou un module complémentaire. 

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions?applicationId=9WZDNCRFJ314&startDate=1/1/2015&endDate=2/1/2015&skip=0 HTTP/1.1 

Authorization: Bearer <your access token> 

 

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions?applicationId=9WZDNCRFJ314&startDate=1/1/2015&endDate=2/1/2015&skip=0&filter=market eq 'GB' and gender eq 'm' HTTP/1.1 

Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

### <a name="response-body"></a>Corps de la réponse

| Value | Type | Description |
| --- | --- | --- |
| Value | tableau | Tableau d’objets contenant des données d’acquisition agrégées d’extensions. Pour plus d’informations sur les données incluses dans chaque objet, voir [Valeurs d’acquisition d’extensions](#add-on-acquisition-values) ci-dessous. |
| @nextLink | chaîne | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête a la valeur 10000, mais qu’il existe plus de 10 000 lignes de données d’acquisition d’extensions pour la demande. |
| TotalCount | entier | Nombre total de lignes dans les résultats de la requête. |

### <a name="add-on-acquisition-values"></a>Valeurs d’acquisition d’extensions
Dans le tableau de valeurs, les éléments contiennent les valeurs suivantes.

| Value | Type | Description | 
| --- | --- | --- |
| date | chaîne | Première date dans la plage de dates des données d’acquisition. Si la requête spécifiait un jour précis, cette valeur correspond à la date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| addonProductId | chaîne | Le *productId* du module complémentaire pour lequel vous récupérez des données d’acquisition. |
| addonProductName | chaîne | Nom d’affichage de l’extension. Cette valeur s’affiche uniquement dans les données de réponse si la *aggregationLevel* paramètre est défini sur **jour**, sauf si vous spécifiez le **addonProductName** champ dans le *groupby* paramètre. |
| applicationId | chaîne | Le *productId* de l’application pour laquelle vous souhaitez récupérer des données d’acquisition de module complémentaire. |
| applicationName | chaîne | Le nom complet du jeu. |
| deviceType | chaîne | L'une des chaînes suivantes qui spécifie le type d’appareil ayant effectué l'acquisition : <ul><li>« PC »</li><li>« Phone »</li><li>« Console »</li><li>« IoT »</li><li>« Serveur »</li><li>« Tablet PC »</li><li>« HOLOGRAPHIQUE »</li><li>« Inconnu »</li></ul> |
| storeClient | chaîne | Une des chaînes suivantes qui indique la version du Store sur laquelle l’acquisition s’est produite : <ul><li>« Windows Phone Store (client) »</li><li>« Microsoft Store (client) » (ou « Windows Store (client) » si l’interrogation des données avant le 23 mars 2018)</li><li>« Microsoft Store (web) » (ou « Windows Store (web) » si l’interrogation des données avant le 23 mars 2018)</li><li>« Volume d’achat par les organisations »</li><li>« Autre »</li></ul> |
| osVersion | chaîne | La version de système d’exploitation sur laquelle l’acquisition s’est produite. Pour cette méthode, cette valeur est toujours « Windows 10 ». |
| market | chaîne | Le code pays ISO 3166 du marché dans lequel l’acquisition s’est produite. |
| gender | chaîne | L'une des chaînes suivantes qui spécifie le sexe de l'utilisateur ayant effectué l’acquisition : <ul><li>"m"</li><li>"f"</li><li>« Inconnu »</li></ul> |
| age | chaîne | L'une des chaînes suivantes qui indique le groupe d'âge de l'utilisateur ayant effectué l’acquisition : <ul><li>« inférieur à 13 »</li><li>"13-17"</li><li>"18-24"</li><li>"25-34"</li><li>"35-44"</li><li>"44-55"</li><li>« supérieur à 55 »</li><li>« Inconnu »</li></ul> |
| acquisitionType | chaîne | Une des chaînes suivantes qui indique le type d'acquisition : <ul><li>« Gratuit » </li><li>« Évaluation » </li><li>« Payante »</li><li>« Code de promotion » </li><li>« Produits »</li><li>« Produits d’abonnement »</li><li>« Audience privé »</li><li>« Pre Order »</li><li>« Xbox Game Pass » (ou « Game Pass » si l’interrogation des données avant le 23 mars 2018)</li><li>« Disque »</li><li>« Code prépayé »</li><li>« Facturé Pre ordre »</li><li>« Annulée Pre ordre »</li><li>« Échec de la commande de Pre »</li></ul> |
| acquisitionQuantity | entier | Le nombre d’acquisitions qui se sont produites. |
| inAppProductId | chaîne | ID de produit du produit où ce module complémentaire est utilisé.  |
| inAppProductName | chaîne | Nom de produit du produit où ce module complémentaire est utilisé.  |
| paymentInstrumentType | chaîne | Type d’instrument de paiement utilisé pour l’acquisition.  |
| sandboxId | chaîne | L’ID de bac à sable créé pour le jeu. Cela peut être la valeur **RETAIL** ou un ID de bac à sable privé.  |
| xboxTitleId | chaîne | ID de titre Xbox du produit à partir de XDP, le cas échéant.  |
| localCurrencyCode | chaîne | Code de devise locale en fonction du pays du compte de partenaires.  |
| xboxProductId | chaîne | ID de produit Xbox du produit à partir de XDP, le cas échéant.  |
| availabilityId | chaîne | ID de la disponibilité du produit à partir de XDP, le cas échéant.  |
| skuId | chaîne | ID de référence (SKU) du produit à partir de XDP, le cas échéant.  |
| skuDisplayName | chaîne | Nom complet de référence (SKU) du produit à partir de XDP, le cas échéant.  |
| xboxParentProductId | chaîne | ID de produit Xbox Parent du produit à partir de XDP, le cas échéant.  |
| parentProductName | chaîne | Nom du produit parent du produit à partir de XDP, le cas échéant.  |
| productTypeName | chaîne | Nom du Type de produit du produit à partir de XDP, le cas échéant.  |
| purchaseTaxType | chaîne | Type de taxe d’achat du produit à partir de XDP, le cas échéant.  |
| purchasePriceUSDAmount | nombre | Le montant payé par le client pour le module complémentaire, converti en USD.  |
| purchasePriceLocalAmount | nombre | Le montant des taxes appliqué au module complémentaire.  |
| purchaseTaxUSDAmount | nombre | Le montant des taxes appliqué au module complémentaire, converti en USD.  |
| purchaseTaxLocalAmount | nombre | Acheter le montant des taxes locales du produit à partir de XDP, le cas échéant.  |

### <a name="response-example"></a>Exemple de réponse
L’exemple suivant représente un corps de réponse JSON pour cette requête. 

```JSON
{ 
  "Value": [ 
    { 
            "inAppProductId": "9NBLGGH1864K", 
            "inAppProductName": "866879", 
            "addonProductId": "9NBLGGH1864K", 
            "addonProductName": "866879", 
            "date": "2017-11-05", 
            "applicationId": "9WZDNCRFJ314", 
            "applicationName": "Tetris Blitz", 
            "acquisitionType": "Iap", 
            "age": "35-49", 
            "deviceType": "Phone", 
            "gender": "m", 
            "market": "US", 
            "osVersion": "Windows Phone 8.1", 
            "paymentInstrumentType": "Credit Card", 
            "sandboxId": "RETAIL", 
            "storeClient": "Windows Phone Store (client)", 
            "xboxTitleId": "", 
            "localCurrencyCode": "USD", 
            "xboxProductId": "00000000-0000-0000-0000-000000000000", 
            "availabilityId": "", 
            "skuId": "", 
            "skuDisplayName": "Full", 
            "xboxParentProductId": "", 
            "parentProductName": "Tetris Blitz", 
            "productTypeName": "Add-On", 
            "purchaseTaxType": "", 
            "acquisitionQuantity": 1, 
            "purchasePriceUSDAmount": 1.08, 
            "purchasePriceLocalAmount": 0.09, 
            "purchaseTaxUSDAmount": 1.08, 
            "purchaseTaxLocalAmount": 0.09 
        } 
    ], 

    "@nextLink": null, 
    
    "TotalCount": 7601 
} 
```