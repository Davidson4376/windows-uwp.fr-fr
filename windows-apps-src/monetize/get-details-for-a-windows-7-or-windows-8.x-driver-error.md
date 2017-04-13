---
author: mcleanbyron
ms.assetid: 2FBA0B73-17C6-4F25-A79D-63F2F262491A
description: "Utilisez cette méthode dans l’API d’analyse du Windows Store pour obtenir des données détaillées sur une erreur de pilote sous Windows 7 ou Windows 8.x. Cette méthode est uniquement destinées aux fabricants de matériel."
title: Obtenir des informations sur une erreur de pilote Windows7 ou Windows 8.x
ms.author: mcleans
ms.date: 03/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows10, uwp, services du Windows Store, API d&quot;analyse du WindowsStore, erreurs, détails"
ms.openlocfilehash: fbf1ee456080ff228abde061dbfef859baf79465
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="get-details-for-a-windows-7-or-windows-8x-driver-error"></a>Obtenir des informations sur une erreur de pilote Windows7 ou Windows 8.x

Utilisez cette méthode dans l’API d’analyse du WindowsStore pour obtenir les informations concernant une erreur de pilote Windows 7 ou Windows 8.x spécifique de votre application, au format JSON. Pour utiliser cette méthode, vous devez d’abord utiliser la méthode [Obtenir les données de rapport d’erreurs pour les pilotes Windows 7 et Windows 8.x](get-error-reporting-data-for-windows-7-and-windows-8.x-drivers.md) afin de récupérer l’ID de l’erreur sur laquelle vous souhaitez des informations détaillées.

> [!NOTE]
> Cette méthode peut uniquement être utilisée par les comptes de développeur qui participent au [programme du centre de développement matériel Windows](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard).

## <a name="prerequisites"></a>Prérequis


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Windows Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Récupérez l’ID de l’erreur sur laquelle vous souhaitez des informations détaillées. Pour ce faire, utilisez la méthode [Obtenir les données de rapport d’erreurs pour les pilotes Windows 7 et Windows 8.x](get-error-reporting-data-for-windows-7-and-windows-8.x-drivers.md) et utilisez la valeur **failureHash** dans le corps de la réponse de cette méthode.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/failuredetails``` |

<span/> 

### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |

<span/> 

### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| failureHash | chaîne | L'ID d’erreur unique sur laquelle vous souhaitez obtenir des informations détaillées. Pour obtenir la valeur correspondant à l’erreur qui vous intéresse, utilisez la méthode [Obtenir les données de rapport d’erreurs matérielles d'OEM](get-oem-hardware-error-reporting-data.md) et utilisez la valeur **failureHash** dans le corps de la réponse de cette méthode. |  Oui  |
| startDate | date | Date de début des données à récupérer concernant l’erreur. La valeur par défaut est de 30jours avant la date actuelle. |  Non  |
| endDate | date | Date de fin des données à récupérer concernant l’erreur. La valeur par défaut est la date actuelle |  Non  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10 et skip=0 pour obtenir les 10premières lignes de données, top=10 et skip=10 pour obtenir les 10lignes suivantes, et ainsi de suite. |  Non  |
| filter | chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ Nom dans le corps de la réponse et une valeur, qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. Vous pouvez spécifier les champs suivants dans le corps de réponse:<p/><ul><li><strong>date</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osName</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>flightRing</strong></li><li><strong>architecture</strong></li><li><strong>cabType</strong></li><li><strong>cabExpirationTime</strong></li></ul> | Non   |
| orderby | chaîne | Une instruction commandant les valeurs des données de résultat. Syntaxe: <em>orderby=field [order],field [order],...</em>. Vous pouvez spécifier les champs suivants dans le corps de réponse:<p/><ul><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>cabType</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osName</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>flightRing</strong></li><li><strong>architecture</strong></li></ul><p>Le paramètre facultatif <em>order</em> peut avoir la valeur <strong>asc</strong> ou <strong>desc</strong> pour spécifier l’ordre croissant ou décroissant de chaque champ. La valeur par défaut est <strong>asc</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em>: <em>orderby=date,market</em></p> |  Non  |

<span/> 

### <a name="request-example"></a>Exemple de requête

Les exemples suivants fournissent plusieurs requêtes permettant de récupérer des données d’erreur détaillées.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/failuredetails?failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/failuredetails?failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type    | Description    |
|------------|---------|------------|
| Valeur      | array   | Un tableau d’objets comportant des données d’erreur détaillées. Pour plus d’informations sur les données incluses dans chaque objet, voir le tableau suivant.          |
| @nextLink  | chaîne  | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour demander la page suivante. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur10, mais que plus de 10lignes d’erreur sont associées à la requête. |
| TotalCount | nombre entier | Nombre total de lignes des résultats de données pour la requête.        |

<span/>

Les éléments du tableau *Value* comportent les valeurs suivantes:

| Valeur           | Type    | Description     |
|-----------------|---------|----------------------------|
| date            | chaîne  | Date de début des données d’erreur. Si la requête spécifiait un jour unique, cette valeur est cette date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| sellerId   | chaîne  | La valeur d’ID de vendeur qui est associée au compte de développeur (ce qui correspond à la valeur **ID de vendeur** dans les paramètres du compte de centre de développement). |
| failureName     | chaîne  | Nom de l’erreur. Ce nom est celui qui figure dans la section **Échecs** du [rapport d’intégrité](../publish/health-report.md) dans le tableau de bord du Centre de développement Windows.            |
| failureHash     | chaîne  | L’identificateur unique de l’erreur.     |
| symbol     | chaîne  | Le symbole affecté à cette erreur.     |
| osVersion       | chaîne  | La version du système d’exploitation en quatre parties sur laquelle l’erreur s’est produite.    |
| osName       | chaîne  | Le nom du système d'exploitation sur lequel l’erreur s’est produite.  |
| eventType       | chaîne  | Le type d’erreur qui s’est produite.      |
| marché          | chaîne  | Code pays ISO3166 du marché des appareils.     |
| deviceType      | chaîne  | Type d’appareil sur lequel l’erreur s’est produite.     |
| driverName     | chaîne  | Nom unique du pilote associé à cette erreur.      |
| driverVersion  | chaîne  | La version du pilote associé à cette erreur.   |
| architecture | chaîne |  L'architecture de l'appareil sur lequel l’erreur s’est produite.  |
| oemName | chaîne | Le nom de l'OEM pour l'appareil sur lequel l’erreur s’est produite. |
| oemModel | chaîne | Le nom du modèle de l'appareil sur lequel l’erreur s’est produite. |
| flightRing | chaîne | Le nom de la version d'évaluation du système d'exploitation sur lequel l’erreur s’est produite. |
| clientDeviceId | chaîne | L'ID de l'appareil sur lequel l’erreur s’est produite. |
| cabIdHash         | chaîne  | L'ID unique du fichierCAB associé à cette erreur.   |
| cabType         | chaîne  | Le type du fichier CAB.   |
| cabExpirationTime  | chaîne  | La date et l'heure auxquelles le fichierCAB est arrivé à expiration et n’est plus téléchargeable au format ISO8601.   |

<span/> 

### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "sellerId": "14313740",
      "architecture": "x64",
      "cabExpirationTime": "2017-06-12 23:51:11",
      "cabIdHash": "cc1797f9-b783-44d4-b0e5-46ae7ca668bc",
      "cabType": "MINI",
      "clientDeviceId": null,
      "date": "2017-03-14 23:51:11",
      "deviceType": "PC",
      "driverName": "fastboot.sys",
      "driverVersion": "2.1.1.0",
      "failureHash": "f060c0b6-fbe6-463f-a9f1-b7ebc1cc722f",
      "failureName": "0x109_18_2_LoadImageNotify",
      "market": "US",
      "oemModel": "C-122",
      "oemName": "Contoso",
      "osName": "Windows 8.1",
      "osVersion": "6.3.9600.9600"
    }]
}
```

## <a name="related-topics"></a>Rubriques associées

* [Récupérer des données de rapport d'erreur pour les pilotes Windows7 et Windows8.x](get-error-reporting-data-for-windows-7-and-windows-8.x-drivers.md)
* [Téléchargez le fichier CAB pour une erreur de pilote Windows 7 ou Windows8.x](download-the-cab-file-for-a-windows-7-or-windows-8.x-driver-error.md)
