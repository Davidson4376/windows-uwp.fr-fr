---
author: mcleanbyron
ms.assetid: 79DC7C99-70F1-499A-856B-D2A83FC6F867
description: Utilisez cette méthode dans l’API d’analyse du MicrosoftStore pour obtenir des données détaillées sur une erreur de pilote Windows 10. Cette méthode est uniquement destinées aux fabricants de matériel.
title: Obtenir des détails sur une erreur de pilote Windows10
ms.author: mcleans
ms.date: 06/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, services du Store, API d'analyse du MicrosoftStore, erreurs, détails
ms.localizationpriority: medium
ms.openlocfilehash: 66729d94f4ea8c6db42b3e573e0e9407d6295e91
ms.sourcegitcommit: cd91724c9b81c836af4773df8cd78e9f808a0bb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2018
ms.locfileid: "1989373"
---
# <a name="get-details-for-a-windows-10-driver-error"></a>Obtenir des détails sur une erreur de pilote Windows10

Utilisez cette méthode dans l’API d’analyse du MicrosoftStore pour obtenir les informations concernant une erreur de pilote Windows 10 spécifique de votre app, au format JSON. Pour utiliser cette méthode, vous devez d’abord utiliser la méthode [Obtenir les données de rapport d’erreurs pour les pilotes Windows 10](get-error-reporting-data-for-windows-10-drivers.md) afin de récupérer l’ID de l’erreur sur laquelle vous souhaitez des informations détaillées.

> [!NOTE]
> Cette méthode peut uniquement être utilisée par les comptes de développeur qui participent au [programme du centre de développement matériel Windows](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard).

## <a name="prerequisites"></a>Prérequis


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Récupérez l’ID de l’erreur sur laquelle vous souhaitez des informations détaillées. Pour ce faire, utilisez la méthode [Obtenir les données de rapport d’erreurs pour les pilotes Windows 10](get-error-reporting-data-for-windows-10-drivers.md) et utilisez la valeur **failureHash** dans le corps de la réponse de cette méthode.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/driver/failuredetails``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |
 

### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Requis  
|---------------|--------|---------------|------|
| applicationId | chaîne | La valeur d’ID de produit du pilote pour lequel vous souhaitez récupérer les données d’erreur. |  Oui  |
| failureHash | chaîne | L'ID d’erreur unique sur laquelle vous souhaitez obtenir des informations détaillées. Pour obtenir la valeur correspondant à l’erreur qui vous intéresse, utilisez la méthode [Obtenir les données de rapport d’erreurs matérielles d'OEM](get-oem-hardware-error-reporting-data.md) et utilisez la valeur **failureHash** dans le corps de la réponse de cette méthode. |  Oui  |
| startDate | date | Date de début des données à récupérer concernant l’erreur. La valeur par défaut est de 30jours avant la date actuelle. |  Non  |
| endDate | date | Date de fin des données à récupérer concernant l’erreur. La valeur par défaut est la date actuelle |  Non  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10 et skip=0 pour obtenir les 10premières lignes de données, top=10 et skip=10 pour obtenir les 10lignes suivantes, et ainsi de suite. |  Non  |
| filter |chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ Nom dans le corps de la réponse et une valeur, qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. Vous pouvez spécifier les champs suivants dans le corps de réponse:<p/><ul><li><strong>date</strong></li><li><strong>submissionId</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osName</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>architecture</strong></li><li><strong>cabIdHash</strong></li><li><strong>clientDeviceId</strong></li><li><strong>cabType</strong></li><li><strong>cabExpirationTime</strong></li></ul> | Non   |
| orderby | chaîne | Une instruction commandant les valeurs des données de résultat. La syntaxe est <em>orderby=field [order],field [order],...</em>. Vous pouvez spécifier les champs suivants à partir du corps de réponse:<p/><ul><li><strong>date</strong></li><li><strong>submissionId</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osName</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>flightRing</strong></li><li><strong>architecture</strong></li><li><strong>cabType</strong></li><li><strong>cabExpirationTime</strong></li></ul><p>Le paramètre facultatif <em>order</em> peut avoir la valeur <strong>asc</strong> ou <strong>desc</strong> pour spécifier l’ordre croissant ou décroissant de chaque champ. La valeur par défaut est <strong>asc</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em>: <em>orderby=date,market</em></p> |  Non  |


### <a name="request-example"></a>Exemple de requête

Les exemples suivants fournissent plusieurs requêtes permettant de récupérer des données d’erreur détaillées.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/driver/failuredetails?applicationId=18430592857500421&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/driver/failuredetails?applicationId=18430592857500421&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type    | Description    |
|------------|---------|------------|
| Valeur      | array   | Un tableau d’objets comportant des données d’erreur détaillées. Pour plus d’informations sur les données incluses dans chaque objet, voir le tableau suivant.          |
| @nextLink  | chaîne  | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour demander la page suivante. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur10, mais que plus de 10lignes d’erreur sont associées à la requête. |
| TotalCount | entier | Nombre total de lignes dans les résultats de données de la requête.        |


Les éléments du tableau *Value* comportent les valeurs suivantes:

| Valeur           | Type    | Description     |
|-----------------|---------|----------------------------|
| date            | chaîne  | Date de début des données d’erreur. Si la requête spécifiait un jour unique, cette valeur est cette date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| applicationId   | chaîne  | La valeur d’ID de produit du pilote pour lequel vous avez récupéré les données d’erreur. |
| submissionId   | chaîne  | L’ID de soumission associée au pilote. |
| failureName     | chaîne  |Le nom de l'échec, qui est constitué de quatre parties: une ou plusieurs classes de problème, un code de vérification d’exception/d’erreur, le nom du pilote où l’échec s’est produit et le nom de la fonction associée.             |
| failureHash     | chaîne  | L’identificateur unique de l’erreur.     |
| symbol     | chaîne  | Le symbole affecté à cette erreur.     |
| osVersion       | chaîne  | La version du système d’exploitation en quatre parties sur laquelle l’erreur s’est produite.    |
| osName       | chaîne  | Le nom du système d'exploitation sur lequel l’erreur s’est produite.  |
| eventType       | chaîne  | Le type d’erreur qui s’est produite.      |
| marché          | chaîne  | Code pays ISO3166 du marché des appareils.     |
| deviceType      | chaîne  | Une des chaînes suivantes spécifiant le type d’appareil sur lequel l’erreur s’est produite:<p/><ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Inconnu</strong></li></ul>     |
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


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {   
      "submissionId":"29989500_13736592797500435_1152921504626321174",
      "applicationId":"18430592857500421",
      "architecture": "x64",
      "cabExpirationTime": "2017-03-16 01:04:55",
      "cabIdHash": "1d7b4184-8f18-4207-88b5-36b276363eb5",
      "cabType": "MINI",
      "clientDeviceId": null,
      "date": "2017-02-14 01:04:55",
      "deviceType": "Unknown",
      "driverName": "fastboot.sys",
      "driverVersion": "2.1.1.0",
      "failureHash": "0d901479-bf1f-b842-76f2-3db7e4feaedd",
      "failureName": "AV_fastboot!unknown_function",
      "market": "US",
      "oemModel": "C-122",
      "oemName": "Contoso",
      "osName": "Windows 10",
      "osVersion": "10.0.14393.693"
    }]
}
```

## <a name="related-topics"></a>Rubriques associées

* [Obtenir les données de rapport d’erreurs pour les pilotes Windows 10](get-error-reporting-data-for-windows-10-drivers.md)
* [Téléchargez le fichier CAB pour une erreur de pilote Windows10](download-the-cab-file-for-a-windows-10-driver-error.md)
