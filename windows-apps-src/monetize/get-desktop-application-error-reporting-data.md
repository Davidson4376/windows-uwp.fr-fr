---
description: Utilisez cette méthode dans l’API d’analyse du Microsoft Store pour récupérer les données agrégées de rapport d’erreurs pour une application de bureau, pour une plage de dates données et en fonction d’autres filtres facultatifs.
title: Obtenir les données de signalement d’erreurs pour votre application de bureau
ms.date: 09/04/2018
ms.topic: article
keywords: windows 10, uwp, services du Microsoft Store, API d'analyse du Microsoft Store, erreurs, application de bureau
ms.localizationpriority: medium
ms.openlocfilehash: 0a628df7f557070a077cd0c4ec328bc49ffaf5bd
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372114"
---
# <a name="get-error-reporting-data-for-your-desktop-application"></a>Obtenir les données de signalement d’erreurs pour votre application de bureau

Utilisez cette méthode dans l'API d'analyse du Microsoft Store pour obtenir des données de rapport d'erreur agrégées pour une application de bureau que vous avez ajoutée au [Du programme d’application de bureau Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program). Cette méthode peut récupérer uniquement les erreurs qui se sont produites au cours des 30 derniers jours. Ces informations sont également disponibles dans le [rapport d’intégrité](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program) pour les applications de bureau dans l’espace partenaires.

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Demande


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits``` |


### <a name="request-header"></a>En-tête de requête

| Header        | type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | type   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| applicationId | chaîne | L’ID produit d'une application de bureau pour laquelle vous souhaitez récupérer les données de rapport d’erreur. Pour obtenir l’ID de produit d’une application de bureau, ouvrez un [analytique de rapports pour votre application de bureau partenaires](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program) (telles que la **rapport d’intégrité**) et récupérer l’ID de produit dans l’URL. |  Oui  |
| startDate | date | Dans la plage de dates, la date de début de la récupération des données de rapport d’erreurs, au format ```mm/dd/yyyy```. La valeur par défaut est la date actuelle.<p/><p/>**Remarque :** &nbsp;&nbsp;cette méthode peut récupérer uniquement les erreurs qui se sont produites au cours des 30 derniers jours.  |  Non  |
| endDate | date | Dans la plage de dates, la date de fin de la récupération des données de rapport d’erreurs, au format ```mm/dd/yyyy```. La valeur par défaut est la date actuelle.   |  Non  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |
| Filter |chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ Nom dans le corps de la réponse et une valeur, qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. Vous pouvez spécifier les champs suivants dans le corps de réponse :<p/><ul><li><strong>fileName</strong></li><li><strong>applicationVersion</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osBuild</strong></li><li><strong>osRelease</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>productName</strong></li><li><strong>date</strong></li></ul> | Non   |
| aggregationLevel | chaîne | Indique la plage de temps pendant laquelle récupérer les données agrégées. Il peut s’agit des chaînes suivantes : **day**, **week** ou **month**. Par défaut, la valeur est **day**. Si vous spécifiez **week** ou **month**, les valeurs *failureName* et *failureHash* sont limitées à 1 000 compartiments.<p/>  | Non |
| orderby | chaîne | Instruction commandant les valeurs des données de résultats. Syntaxe : *orderby=field [order],field [order],...* . Le paramètre *field* peut comporter l’une des chaînes suivantes :<ul><li><strong>fileName</strong></li><li><strong>applicationVersion</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osBuild</strong></li><li><strong>osRelease</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>productName</strong></li><li><strong>date</strong></li></ul>Le paramètre *order*, facultatif, peut comporter les valeurs **asc** ou **desc** afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est **asc**.</p><p>Voici un exemple de chaîne *orderby* : *orderby=date,market*</p> |  Non  |
| groupby | chaîne | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants :<ul><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**eventType**</li><li>**market**</li><li>**deviceType**</li></ul><p>Les lignes de données renvoyées comportent les champs spécifiés dans le paramètre *groupby*, ainsi que dans les paramètres suivants :</p><ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**eventCount**</li></ul><p>Le paramètre *groupby* peut être utilisé avec le paramètre *aggregationLevel*. Exemple : *&amp;groupby=failureName,market&amp;aggregationLevel=week*</p></p> |  Non  |


### <a name="request-example"></a>Exemple de requête

Les exemples suivants fournissent font figurer plusieurs requêtes de récupération des données de rapport d’erreurs. Remplacez la valeur *applicationId* par l'ID produit pour votre application de bureau.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits?applicationId=10238467886765136388&startDate=1/1/2018&endDate=2/1/2018&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits?applicationId=10238467886765136388&startDate=8/1/2017&endDate=8/31/2017&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Value      | type    | Description     |
|------------|---------|--------------|
| Value      | tableau   | Tableau d’objets comportant les données agrégées de rapport d’erreurs. Pour plus d’informations sur les données de chaque objet, consultez la section [Valeurs des erreurs](#error-values) ci-dessous.     |
| @nextLink  | chaîne  | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la demande est défini sur 10000, mais que plus de 10 000 lignes d’erreurs sont associées à la requête. |
| TotalCount | entier | Nombre total de lignes dans les résultats de la requête.     |


### <a name="error-values"></a>Valeurs des erreurs

Les éléments du tableau *Value* comportent les valeurs suivantes :

| Value           | type    | Description        |
|-----------------|---------|---------------------|
| date            | chaîne  | Date de début des données d’erreur, au format ```yyyy-mm-dd```. Si la requête spécifie un jour unique, cette valeur est cette date. Si la requête spécifie une plage de dates plus étendue, cette valeur correspond à la première date de la plage de dates. Pour les demandes qui spécifient un *aggregationLevel* en **hour**, cette valeur inclut également une valeur d’heure au format ```hh:mm:ss```.  |
| applicationId   | chaîne  | L'ID produit de l’application de bureau dont vous souhaitez récupérer des données d’erreur.    |
| productName | chaîne  | Le nom complet de l’application de bureau tel que dérivé des métadonnées de ses fichiers exécutables associés.   |
| appName | chaîne  |  À déterminer  |
| fileName | chaîne  | Le nom du fichier exécutable de l’application de bureau.   |
| failureName     | chaîne  | Le nom de l'échec, qui est constitué de quatre parties : une ou plusieurs classes de problème, un code de vérification d’exception/d’erreur, le nom de l'image où l’échec s’est produit et le nom de la fonction associée.  |
| failureHash     | chaîne  | Identificateur unique de l’erreur.   |
| symbol          | chaîne  | Le symbole affecté à cette erreur. |
| osBuild       | chaîne  | Numéro de version en quatre parties du système d’exploitation sur lequel l’erreur s’est produite.  |
| osVersion       | chaîne  | Une des chaînes suivantes qui spécifie la version du système d'exploitation sur laquelle l’application de bureau est installée :<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>Inconnu</strong></li></ul>   |   
| osRelease | chaîne  | L'une des chaînes suivantes qui spécifie la version du système d’exploitation ou l'anneau de distribution de version d’évaluation (comme une sous-population dans la version du système d’exploitation) sur laquelle/lequel l'erreur s'est produite.<p/><p>Pour Windows 10 :</p><ul><li><strong>Version 1507</strong></li><li><strong>Version 1511</strong></li><li><strong>Version 1607</strong></li><li><strong>Version 1703</strong></li><li><strong>Version 1709</strong></li><li><strong>Version 1803</strong></li><li><strong>Release Preview</strong></li><li><strong>Insider rapide</strong></li><li><strong>Insider lente</strong></li></ul><p/><p>Pour Windows Server 1709 :</p><ul><li><strong>RTM</strong></li></ul><p>Pour Windows Server 2016 :</p><ul><li><strong>Version 1607</strong></li></ul><p>Pour Windows 8.1 :</p><ul><li><strong>Mise à jour 1</strong></li></ul><p>Pour Windows 7 :</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Si la version du système d’exploitation ou l'anneau de distribution de version d’évaluation est inconnu(e), ce champ comporte la valeur <strong>Inconnu</strong>.</p> |
| eventType       | chaîne  | L'une des chaînes suivantes qui indique le type d'événement d'erreur :<ul><li>**crash**</li><li>**hang**</li><li>**memory**</li><li>**jse**</li></ul>       |
| market          | chaîne  | Code pays ISO 3166 du marché des appareils.   |
| deviceType      | chaîne  | Une des chaînes suivantes spécifiant le type d’appareil sur lequel l’erreur s’est produite :<p/><ul><li><strong>PC</strong></li><li><strong>Serveur</strong></li><li><strong>Tablet</strong></li><li><strong>Inconnu</strong></li></ul>    |
| applicationVersion     | chaîne  |   La version du fichier exécutable de l’application dans laquelle l’erreur s’est produite.    |
| eventCount      | nombre | Le nombre d’événements affectés à cette erreur pour le niveau d’agrégation spécifié.      |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "date": "2018-02-01",
      "applicationId": "10238467886765136388",
      "productName": "Contoso Demo",
      "appName": "Contoso Demo",
      "fileName": "contosodemo.exe",
      "failureName": "SVCHOSTGROUP_localservice_IN_PAGE_ERROR_c0000006_hardware_disk!Unknown",
      "failureHash": "11242ef3-ebd8-d525-838d-b5497b225695",
      "symbol": "hardware_disk!Unknown",
      "osBuild": "10.0.15063.850",
      "osVersion": "Windows 10",
      "osRelease": "Version 1703",
      "eventType": "crash",
      "market": "US",
      "deviceType": "PC",
      "applicationVersion": "2.2.2.0",
      "eventCount": 0.0012422360248447205
    }
  ],
  "@nextLink": "desktop/failurehits?applicationId=10238467886765136388&aggregationLevel=week&startDate=2018/02/01&endDate2018/02/08&top=1&skip=1",
  "TotalCount": 21
}

```

## <a name="related-topics"></a>Rubriques connexes

* [Rapport d’intégrité](../publish/health-report.md)
* [Accéder aux données d’analytique à l’aide des services de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir les détails d’une erreur dans votre application de bureau](get-details-for-an-error-in-your-desktop-application.md)
* [Obtenir la trace de pile pour une erreur dans votre application de bureau](get-the-stack-trace-for-an-error-in-your-desktop-application.md)
* [Télécharger le fichier CAB pour une erreur dans votre application de bureau](download-the-cab-file-for-an-error-in-your-desktop-application.md)
