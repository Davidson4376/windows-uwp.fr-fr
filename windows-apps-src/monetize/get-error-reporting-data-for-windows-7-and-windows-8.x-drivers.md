---
author: mcleanbyron
ms.assetid: 2F30E68B-B643-4387-9430-793D08AAF0E7
description: Utilisez cette méthode dans l’API d’analyse du MicrosoftStore pour récupérer les données agrégées de rapport d’erreurs pour les pilotes Windows 7 et Windows 8.x, pour une plage de dates données et en fonction d’autres filtres facultatifs. Cette méthode est uniquement destinées aux fabricants de matériel.
title: Récupérer des données de rapport d'erreur pour les pilotes Windows7 et Windows8.x
ms.author: mcleans
ms.date: 06/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, services du MicrosoftStore, API d'analyse du MicrosoftStore, erreurs
ms.localizationpriority: medium
ms.openlocfilehash: 75a1b16e8882e961ccd0a10e99e94038948f59fd
ms.sourcegitcommit: cd91724c9b81c836af4773df8cd78e9f808a0bb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2018
ms.locfileid: "1989593"
---
# <a name="get-error-reporting-data-for-windows-7-and-windows-8x-drivers"></a>Récupérer des données de rapport d'erreur pour les pilotes Windows7 et Windows8.x

Utilisez cette méthode dans l’API d’analyse du MicrosoftStore pour récupérer les données agrégées de rapport d’erreurs pour les erreurs de pilote Windows 7 et Windows 8.x, pour une plage de dates données et en fonction d’autres filtres facultatifs. Vous pouvez récupérer des informations supplémentaires sur les erreurs à l’aide de la méthode [obtenir des détails sur une erreur de pilote Windows7 ou Windows 8.x](get-details-for-a-windows-7-or-windows-8.x-driver-error.md).

> [!NOTE]
> Cette méthode peut uniquement être utilisée par les comptes de développeur qui participent au [programme du centre de développement matériel Windows](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard).

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/failurehits``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| startDate | date | Dans la plage de dates, la date de début de la récupération des données de rapport d’erreurs. La valeur par défaut est la date actuelle. |  Non  |
| endDate | date | Dans la plage de dates, la date de fin de la récupération des données de rapports d’erreurs. La valeur par défaut est la date du jour. |  Non  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |
| filter | chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ Nom dans le corps de la réponse et une valeur, qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. Vous pouvez spécifier les champs suivants dans le corps de réponse:<p/><ul><li><strong>date</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>flightRing</strong></li><li><strong>architecture</strong></li></ul> | Non   |
| aggregationLevel | chaîne | Indique la plage de temps pendant laquelle récupérer les données agrégées. Il peut s’agit des chaînes suivantes : <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Par défaut, la valeur est <strong>day</strong>. Si vous spécifiez <strong>week</strong> ou <strong>month</strong>, les valeurs <em>failureName</em> et <em>failureHash</em> sont limitées à 1 000 compartiments. | Non |
| orderby | chaîne | Une instruction commandant les valeurs des données de résultat. La syntaxe est <em>orderby=field [order],field [order],...</em>. Vous pouvez spécifier les champs suivants à partir du corps de réponse:<p/><ul><li><strong>date</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osName</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>flightRing</strong></li><li><strong>architecture</strong></li></ul><p>Le paramètre facultatif <em>order</em> peut avoir la valeur <strong>asc</strong> ou <strong>desc</strong> pour spécifier l’ordre croissant ou décroissant de chaque champ. La valeur par défaut est <strong>asc</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em>: <em>orderby=date,market</em></p> |  Non  |
| groupby | chaîne | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants dans le corps de réponse:<p/><ul><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>flightRing</strong></li><li><strong>architecture</strong></li></ul><p>Les lignes de données renvoyées comportent les champs spécifiés dans le paramètre <em>groupby</em>, ainsi que dans les paramètres suivants :</p><ul><li><strong>date</strong></li><li><strong>eventCount</strong></li></ul><p>Le paramètre <em>groupby</em> peut être utilisé avec le paramètre <em>aggregationLevel</em>. Exemple: <em>&amp;groupby=failureName,market&amp;aggregationLevel=week</em></p></p> |  Non  |


### <a name="request-example"></a>Exemple de requête

Les exemples suivants fournissent font figurer plusieurs requêtes de récupération des données de rapport d’erreurs matérielles d'OEM.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/failurehits?startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/failurehits?startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type    | Description     |
|------------|---------|--------------|
| Valeur      | tableau   | Un tableau d’objets comportant les données agrégées de signalement d’erreurs. Pour plus d’informations sur les données incluses dans chaque objet, voir le tableau suivant.     |
| @nextLink  | chaîne  | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la demande est défini sur 10000, mais que plus de 10000lignes d’erreurs sont associées à la requête. |
| TotalCount | entier | Nombre total de lignes dans les résultats de données de la requête.     |


Les éléments du tableau *Value* comportent les valeurs suivantes:

| Valeur           | Type    | Description        |
|-----------------|---------|---------------------|
| date            | chaîne  | Date de début des données d’erreur. Si la requête spécifiait un jour unique, cette valeur est cette date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| sellerId   | chaîne  | La valeur d’ID de vendeur qui est associée au compte de développeur (ce qui correspond à la valeur **ID de vendeur** dans les paramètres du compte de centre de développement). |
| failureName     | chaîne  | Le nom de l'échec, qui est constitué de quatre parties: une ou plusieurs classes de problème, un code de vérification d’exception/d’erreur, le nom du pilote où l’échec s’est produit et le nom de la fonction associée.  |
| failureHash     | chaîne  | L’identificateur unique de l’erreur.   |
| symbol          | chaîne  | Le symbole affecté à cette erreur. |
| osVersion       | chaîne  | La version du système d’exploitation en quatre parties sur laquelle l’erreur s’est produite.  |
| osName       | chaîne  | Le nom du système d'exploitation sur lequel l’erreur s’est produite.  |
| eventType       | chaîne  | Le type d’erreur qui s’est produite.      |
| marché          | chaîne  | Code pays ISO3166 du marché des appareils.   |
| deviceType      | chaîne  | Type d’appareil sur lequel l’erreur s’est produite.    |
| driverName     | chaîne  | Nom unique du pilote associé à cette erreur.      |
| driverVersion  | chaîne  | La version du pilote associé à cette erreur.   |
| architecture | chaîne |  L'architecture de l'appareil sur lequel l’erreur s’est produite.  |
| oemName | chaîne | Le nom de l'OEM pour l'appareil sur lequel l’erreur s’est produite. |
| oemModel | chaîne | Le nom du modèle de l'appareil sur lequel l’erreur s’est produite. |
| flightRing | chaîne | Le nom de la version d'évaluation du système d'exploitation sur lequel l’erreur s’est produite. |
| eventCount      | entier | Le nombre d’événements affectés à cette erreur pour le niveau d’agrégation spécifié.      |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "date": "2017-02-26",
      "sellerId":"14313740",
      "driverName": "fastboot.sys",
      "osVersion": "6.3.9600.9600",
      "osName": "Windows 8.1",
      "flightRing": "Unknown",
      "oemName": "Contoso",
      "oemModel": "C-122",
      "market": "US",
      "symbol": "fastboot",
      "failureName": "AV_fastboot!unknown_function",
      "failureHash": "f060c0b6-fbe6-463f-a9f1-b7ebc1cc722f",
      "architecture": "x64",
      "driverVersion": "2.1.1.0",
      "deviceType": "Unknown",
      "eventType": "System crash",
      "deviceCount": 1.0,
      "eventCount": 1.0
    }]
}
```

## <a name="related-topics"></a>Rubriques associées

* [Obtenir des informations sur une erreur de pilote Windows7 ou Windows 8.x](get-details-for-a-windows-7-or-windows-8.x-driver-error.md)
* [Téléchargez le fichier CAB pour une erreur de pilote Windows 7 ou Windows8.x](download-the-cab-file-for-a-windows-7-or-windows-8.x-driver-error.md)
