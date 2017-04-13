---
author: mcleanbyron
ms.assetid: AE3E003F-BDEC-438B-A80A-3CE1675B369C
description: "Utilisez cette méthode dans l’API d’analyse du WindowsStore pour récupérer les données agrégées de rapport d’erreurs matérielles, pour une plage de dates données et en fonction d’autres filtres facultatifs. Cette méthode est uniquement destinées aux fabricants OEM."
title: "Obtenir les données de rapport d’erreurs matérielles de fabricant d&quot;ordinateurs (OEM)"
ms.author: mcleans
ms.date: 03/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, services du Windows Store, API d&quot;analyse du WindowsStore, erreurs
ms.openlocfilehash: d35c405ae3e8a2f5e4d7fdd70594b51cdf9941c1
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="get-oem-hardware-error-reporting-data"></a>Obtenir les données de rapport d’erreurs matérielles de fabricant d'ordinateurs (OEM)

Utilisez cette méthode dans l’API d’analyse du WindowsStore pour récupérer les données agrégées de rapport d’erreurs matérielles d'OEM, pour une plage de dates données et en fonction d’autres filtres facultatifs. Vous pouvez récupérer des informations d’erreur supplémentaires à l’aide de la méthode [obtenir des informations détaillées sur une erreur matérielle d'OEM](get-details-for-an-oem-hardware-error.md).

> [!NOTE]
> Cette méthode peut uniquement être utilisée par les comptes de développeur qui participent au [programme du centre de développement matériel Windows](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard).

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Windows Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/hardware/failurehits``` |

<span/> 

### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |

<span/> 

### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| startDate | date | Dans la plage de dates, la date de début de la récupération des données de rapport d’erreurs. La valeur par défaut est la date actuelle. |  Non  |
| endDate | date | Dans la plage de dates, la date de fin de la récupération des données de rapports d’erreurs. La valeur par défaut est la date actuelle. |  Non  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |
| filter |chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ Nom dans le corps de la réponse et une valeur, qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. Vous pouvez spécifier les champs suivants:<p/><ul><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>moduleName</strong></li><li><strong>moduleVersion</strong></li><li><strong>mode</strong></li><li><strong>architecture</strong></li><li><strong>modèle</strong></li><li><strong>carte de base</strong></li><li><strong>modelFamily</strong></li><li><strong>flightRing</strong></li></ul> | Non   |
| aggregationLevel | chaîne | Indique la plage de temps pour laquelle vous souhaitez récupérer les données agrégées. Il peut s’agit des chaînes suivantes : <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Par défaut, la valeur est <strong>day</strong>. Si vous spécifiez <strong>week</strong> ou <strong>month</strong>, les valeurs <em>failureName</em> et <em>failureHash</em> sont limitées à 1 000 compartiments. | Non |
| orderby | chaîne | Une instruction commandant les valeurs des données de résultat. Syntaxe: <em>orderby=field [order],field [order],...</em>. Vous pouvez spécifier les champs suivants dans le corps de réponse:<p/><ul><li><strong>date</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>moduleName</strong></li><li><strong>moduleVersion</strong></li><li><strong>mode</strong></li><li><strong>architecture</strong></li><li><strong>modèle</strong></li><li><strong>carte de base</strong></li><li><strong>modelFamily</strong></li><li><strong>flightRing</strong></li></ul><p>Le paramètre facultatif <em>order</em> peut avoir la valeur <strong>asc</strong> ou <strong>desc</strong> pour spécifier l’ordre croissant ou décroissant de chaque champ. La valeur par défaut est <strong>asc</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em>: <em>orderby=date,market</em></p> |  Non  |
| groupby | chaîne | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants dans le corps de réponse:<p/><ul><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>moduleName</strong></li><li><strong>moduleVersion</strong></li><li><strong>mode</strong></li><li><strong>architecture</strong></li><li><strong>modèle</strong></li><li><strong>carte de base</strong></li><li><strong>modelFamily</strong></li><li><strong>flightRing</strong></li></ul><p>Les lignes de données renvoyées comportent les champs spécifiés dans le paramètre <em>groupby</em>, ainsi que dans les paramètres suivants :</p><ul><li><strong>date</strong></li><li><strong>eventCount</strong></li></ul><p>Le paramètre <em>groupby</em> peut être utilisé avec le paramètre <em>aggregationLevel</em>. Exemple: <em>&amp;groupby=failureName,market&amp;aggregationLevel=week</em></p></p> |  Non  |

<span/>


### <a name="request-example"></a>Exemple de requête

Les exemples suivants fournissent font figurer plusieurs requêtes de récupération des données de rapport d’erreurs matérielles d'OEM.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/hardware/failurehits?startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/hardware/failurehits?startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type    | Description     |
|------------|---------|--------------|
| Valeur      | tableau   | Un tableau d’objets comportant les données agrégées de signalement d’erreurs. Pour plus d’informations sur les données incluses dans chaque objet, voir le tableau suivant.     |
| @nextLink  | chaîne  | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la demande est défini sur 10000, mais que plus de 10000lignes d’erreurs sont associées à la requête. |
| TotalCount | nombre entier | Nombre total de lignes des résultats de données pour la requête.     |

<span/>

Les éléments du tableau *Value* comportent les valeurs suivantes:

| Valeur           | Type    | Description        |
|-----------------|---------|---------------------|
| date            | chaîne  | Date de début des données d’erreur. Si la requête spécifiait un jour unique, cette valeur est cette date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| sellerId   | chaîne  | La valeur d’ID de vendeur qui est associée au compte de développeur (ce qui correspond à la valeur **ID de vendeur** dans les paramètres du compte de centre de développement). |
| failureName     | chaîne  | Le nom de l’erreur.  |
| failureHash     | chaîne  | L’identificateur unique de l’erreur.   |
| symbol          | chaîne  | Le symbole affecté à cette erreur. |
| osVersion       | chaîne  | La version du système d’exploitation en quatre parties sur laquelle l’erreur s’est produite.  |
| eventType       | chaîne  | Le type d’erreur qui s’est produite.       |
| marché          | chaîne  | Code pays ISO3166 du marché des appareils.   |
| deviceType      | chaîne  | Une des chaînes suivantes spécifiant le type d’appareil sur lequel l’erreur s’est produite:<p/><ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Inconnu</strong></li></ul>    |
| moduleName     | chaîne  | Nom unique du module associé à cette erreur.      |
| moduleVersion  | chaîne  | La version du module associé à cette erreur.   |
| architecture | chaîne |  L'architecture de l'appareil sur lequel l’erreur s’est produite.  |
| modèle | chaîne | Le nom du modèle de l'appareil sur lequel l’erreur s’est produite. |
| carte de base | chaîne | Le nom de la carte de base du périphérique sur lequel l’erreur s’est produite. |
| modelFamily | chaîne | Le nom de la famille du modèle d'appareil sur lequel l’erreur s’est produite. |
| flightRing | chaîne | Le nom de la version d'évaluation du système d'exploitation sur lequel l’erreur s’est produite. |
| mode | chaîne | Cette valeur est toujours *noyau*. |
| eventCount      | nombre entier | Le nombre d’événements affectés à cette erreur pour le niveau d’agrégation spécifié.      |

<span/> 

### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "date": "2017-02-18",
      "sellerId": "14313740",
      "mode": "Kernel",
      "failureName": "AV_wfplwfs!L2802_3ParseMacHeader",
      "failureHash": "f060c0b6-fbe6-463f-a9f1-b7ebc1cc722f",
      "symbol": "wfplwfs!L2802_3ParseMacHeader",
      "osVersion": "10.0.14393.0",
      "eventType": "System crash",
      "market": "US",
      "deviceType": "PC",
      "moduleName": "wfplwfs.sys",
      "moduleVersion": "10.0.14393.0",
      "architecture": "x64",
      "model": "Unknown",
      "baseboard": "Unknown",
      "modelFamily": "ContosoPad",
      "flightRing": "Windows 10 Retail",
      "eventCount": 2.0
    }]
}
```

## <a name="related-topics"></a>Rubriques associées

* [Obtenir des informations détaillées sur une erreur matérielle d'OEM](get-details-for-an-oem-hardware-error.md)
* [Téléchargez le fichier CAB pour une erreur matérielle d'OEM](download-the-cab-file-for-an-oem-hardware-error.md)
