---
ms.assetid: 252C44DF-A2B8-4F4F-9D47-33E423F48584
description: Utilisez cette méthode dans l’API d’analyse du Microsoft Store pour récupérer les données agrégées de rapport d’erreurs, pour une plage de dates données et en fonction d’autres filtres facultatifs.
title: Obtenir les données de signalement d’erreurs pour votre application
ms.date: 09/04/2018
ms.topic: article
keywords: windows 10, uwp, services du Microsoft Store, API d'analyse du Microsoft Store, erreurs
ms.localizationpriority: medium
ms.openlocfilehash: 69504fd8d670158a35e5afd5dcd035acd296f084
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590594"
---
# <a name="get-error-reporting-data-for-your-app"></a>Obtenir les données de signalement d’erreurs pour votre application

Utilisez cette méthode dans l’API d’analyse du Microsoft Store pour récupérer le rapport complet des erreurs de votre app au format JSON, correspondant à une période donnée et à d’autres filtres facultatifs. Cette méthode peut récupérer uniquement les erreurs qui se sont produites au cours des 30 derniers jours. Ces informations sont également disponibles dans le **échecs** section de la [rapport d’intégrité](../publish/health-report.md) dans Partner Center.

Vous pouvez récupérer des informations d’erreur supplémentaires à l’aide des méthodes [obtenir des informations les erreurs](get-details-for-an-error-in-your-app.md), [obtenir la trace de la pile](get-the-stack-trace-for-an-error-in-your-app.md) et [télécharger le fichier CAB](download-the-cab-file-for-an-error-in-your-app.md).

## <a name="prerequisites"></a>Conditions préalables


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| applicationId | chaîne | L’ID Windows Store de l’application pour laquelle vous souhaitez récupérer les données de rapport d’erreur. L’ID de Store est disponible sur le [page identité des applications](../publish/view-app-identity-details.md) dans Partner Center. Exemple d’ID Windows Store : 9WZDNCRFJ3Q8. |  Oui  |
| startDate | date | Dans la plage de dates, la date de début de la récupération des données de rapport d’erreurs. La valeur par défaut est la date actuelle. Si *aggregationLevel* is **day**, **week** ou **month**, ce paramètre spécifiera une date dans le format ```mm/dd/yyyy```. Si *aggregationLevel* est **hour**, ce paramètre peut spécifier une date au format ```mm/dd/yyyy```ou la date et l'heure au format ```yyyy-mm-dd hh:mm:ss```.<p/><p/>**Remarque :**&nbsp;&nbsp;cette méthode peut récupérer uniquement les erreurs qui se sont produites au cours des 30 derniers jours.  |  Non  |
| endDate | date | Dans la plage de dates, la date de fin de la récupération des données de rapports d’erreurs. La valeur par défaut est la date actuelle. Si *aggregationLevel* is **day**, **week** ou **month**, ce paramètre spécifiera une date dans le format ```mm/dd/yyyy```. Si *aggregationLevel* est **hour**, ce paramètre peut spécifier une date au format ```mm/dd/yyyy```ou la date et l'heure au format ```yyyy-mm-dd hh:mm:ss```. |  Non  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |
| filter |chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ Nom dans le corps de la réponse et une valeur, qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. Vous pouvez spécifier les champs suivants dans le corps de réponse :<p/><ul><li>**applicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**osRelease**</li><li>**eventType**</li><li>**market**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li><li>**date**</li></ul> | Non   |
| aggregationLevel | chaîne | Indique la plage de temps pendant laquelle récupérer les données agrégées. Il peut s’agit des chaînes suivantes : **hour**, **day**, **week** ou **month**. Par défaut, la valeur est **day**. Si vous spécifiez **week** ou **month**, les valeurs *failureName* et *failureHash* sont limitées à 1 000 compartiments.<p/><p/>**Remarque :**&nbsp;&nbsp;si vous spécifiez **hour**, vous pouvez uniquement récupérer des données d’erreur survenues dans les dernières 72 heures. Pour récupérer les données d’erreur datant de plus de 72 heures, spécifiez **day** ou l’un des autres niveaux d’agrégation.  | Non |
| orderby | chaîne | Instruction commandant les valeurs des données de résultats. Syntaxe : *orderby=field [order],field [order],...*. Le paramètre *field* peut comporter l’une des chaînes suivantes :<ul><li>**applicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**osRelease**</li><li>**eventType**</li><li>**market**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li><li>**date**</li></ul><p>Le paramètre *order*, facultatif, peut comporter les valeurs **asc** ou **desc** afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est **asc**.</p><p>Voici un exemple de chaîne *orderby* : *orderby=date,market*</p> |  Non  |
| groupby | chaîne | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants :<ul><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**eventType**</li><li>**market**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li></ul><p>Les lignes de données renvoyées comportent les champs spécifiés dans le paramètre *groupby*, ainsi que dans les paramètres suivants :</p><ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**deviceCount**</li><li>**eventCount**</li></ul><p>Le paramètre *groupby* peut être utilisé avec le paramètre *aggregationLevel*. Exemple : *&amp;groupby=failureName,market&amp;aggregationLevel=week*</p></p> |  Non  |


### <a name="request-example"></a>Exemple de requête

Les exemples suivants fournissent font figurer plusieurs requêtes de récupération des données de rapport d’erreurs. Remplacez la valeur *applicationId* par l’ID Windows Store de votre application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'phone' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type    | Description     |
|------------|---------|--------------|
| Valeur      | tableau   | Tableau d’objets comportant les données agrégées de rapport d’erreurs. Pour plus d’informations sur les données de chaque objet, consultez la section [Valeurs des erreurs](#error-values) ci-dessous.     |
| @nextLink  | chaîne  | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la demande est défini sur 10000, mais que plus de 10 000 lignes d’erreurs sont associées à la requête. |
| TotalCount | Entier | Nombre total de lignes dans les résultats de la requête.     |


### <a name="error-values"></a>Valeurs des erreurs

Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur           | Type    | Description        |
|-----------------|---------|---------------------|
| date            | chaîne  | Date de début des données d’erreur, au format ```yyyy-mm-dd```. Si la requête spécifie un jour unique, cette valeur est cette date. Si la requête spécifie une plage de dates plus étendue, cette valeur correspond à la première date de la plage de dates. Pour les demandes qui spécifient un *aggregationLevel* en **hour**, cette valeur inclut également une valeur d’heure au format ```hh:mm:ss```.  |
| applicationId   | chaîne  | ID Windows Store de l’application dont vous souhaitez récupérer les données d’erreur.   |
| applicationName | chaîne  | Nom d’affichage de l’application.   |
| failureName     | chaîne  | Le nom de l'échec, qui est constitué de quatre parties : une ou plusieurs classes de problème, un code de vérification d’exception/d’erreur, le nom de l'image où l’échec s’est produit et le nom de la fonction associée.  |
| failureHash     | chaîne  | Identificateur unique de l’erreur.   |
| symbol          | chaîne  | Le symbole affecté à cette erreur. |
| osVersion       | chaîne  | L'une des chaînes suivantes qui spécifie la version du système d’exploitation sur lequel l'erreur s’est produite :<ul><li>**Windows Phone 7.5**</li><li>**Windows Phone 8**</li><li>**Windows Phone 8.1**</li><li>**Windows Phone 10**</li><li>**Windows 8**</li><li>**Windows 8.1**</li><li>**Windows 10**</li><li>**Inconnu**</li></ul>  |
| osRelease       | chaîne  |  L'une des chaînes suivantes qui spécifie la version du système d’exploitation ou l'anneau de distribution de version d’évaluation (comme une sous-population dans la version du système d’exploitation) sur laquelle/lequel l'erreur s'est produite.<p/><p>Pour Windows 10 :</p><ul><li><strong>Version 1507</strong></li><li><strong>Version 1511</strong></li><li><strong>Version 1607</strong></li><li><strong>Version 1703</strong></li><li><strong>Version 1709</strong></li><li><strong>Version 1803</strong></li><li><strong>Release Preview</strong></li><li><strong>Insider rapide</strong></li><li><strong>Insider lente</strong></li></ul><p/><p>Pour Windows Server 1709 :</p><ul><li><strong>RTM</strong></li></ul><p>Pour Windows Server 2016 :</p><ul><li><strong>Version 1607</strong></li></ul><p>Pour Windows 8.1 :</p><ul><li><strong>Mise à jour 1</strong></li></ul><p>Pour Windows 7 :</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Si la version du système d’exploitation ou l'anneau de distribution de version d’évaluation est inconnu(e), ce champ comporte la valeur <strong>Inconnu</strong>.</p>    |
| eventType       | chaîne  | Une des chaînes suivantes :<ul><li>**crash**</li><li>**hang**</li><li>**memory**</li><li>**jse**</li></ul>      |
| market          | chaîne  | Code pays ISO 3166 du marché des appareils.   |
| deviceType      | chaîne  | Une des chaînes suivantes indiquant le type d’appareil sur lequel l’erreur s’est produite :<ul><li>**PC**</li><li>**Phone**</li><li>**Console**</li><li>**IoT**</li><li>**HOLOGRAPHIQUE**</li><li>**Inconnu**</li></ul>    |
| packageName     | chaîne  | Nom unique du package applicatif associé à cette erreur.      |
| packageVersion  | chaîne  | Version du package applicatif associé à cette erreur.   |
| deviceCount     | nombre | Le nombre d’appareils uniques correspondant à cette erreur pour le niveau d’agrégation spécifié.  |
| eventCount      | nombre | Le nombre d’événements affectés à cette erreur pour le niveau d’agrégation spécifié.      |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "date": "2017-03-09",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "failureName": "APPLICATION_FAULT_8013150a_StoreWrapper.ni.DLL!70475e55",
      "failureHash": "5a6b2170-1661-ed47-24d7-230fed0077af",
      "symbol": "storewrapper_ni!70475e55",
      "osVersion": "Windows 10",
      "osRelease": "Version 1507",
      "eventType": "crash",
      "market": "US",
      "deviceType": "PC",
      "packageName": "",
      "packageVersion": "0.0.0.0",
      "deviceCount": 0.0,
      "eventCount": 1.0
    }
  ],
  "@nextLink": "failurehits?applicationId=9NBLGGGZ5QDR&aggregationLevel=week&startDate=2017/03/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 21
}

```

## <a name="related-topics"></a>Rubriques connexes

* [Rapport d’intégrité](../publish/health-report.md)
* [Obtenir les détails d’une erreur dans votre application](get-details-for-an-error-in-your-app.md)
* [Obtenir la trace de pile pour une erreur dans votre application](get-the-stack-trace-for-an-error-in-your-app.md)
* [Télécharger le fichier CAB pour une erreur dans votre application](download-the-cab-file-for-an-error-in-your-app.md)
* [Accéder aux données d’analytique à l’aide des services de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des acquisitions d’applications](get-app-acquisitions.md)
* [Obtenir les acquisitions de module complémentaire](get-in-app-acquisitions.md)
* [Obtenez les évaluations de l’application](get-app-ratings.md)
* [Obtenir les révisions d’application](get-app-reviews.md)
