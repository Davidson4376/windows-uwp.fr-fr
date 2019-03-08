---
description: Utilisez cette méthode dans l’API d’analytique Microsoft Store pour obtenir des données détaillées pour une erreur spécifique pour votre Xbox One jeu.
title: Obtenir les détails d’une erreur dans votre jeu Xbox One
ms.date: 11/06/2018
ms.topic: article
keywords: windows 10, uwp, services du Store, API d'analyse du Microsoft Store, erreurs, détails
ms.localizationpriority: medium
ms.openlocfilehash: da3252c42a0c2e2bd02465985737125cc053a616
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589964"
---
# <a name="get-details-for-an-error-in-your-xbox-one-game"></a>Obtenir les détails d’une erreur dans votre jeu Xbox One

Utilisez cette méthode dans le Microsoft Store analytique API pour obtenir des données détaillées pour une erreur spécifique pour votre Xbox One jeu qui a été reçus par le biais du portail de développement Xbox (XDP) et disponible dans le tableau de bord XDP Analytique Partner Center. Cette méthode ne récupère que les informations concernant les erreurs survenues dans les 30 derniers jours.

Avant de pouvoir utiliser cette méthode, vous devez d’abord utiliser le [obtenir des rapports d’erreurs données pour votre Xbox One jeu](get-error-reporting-data-for-your-xbox-one-game.md) méthode pour récupérer l’ID de l’erreur pour laquelle vous souhaitez obtenir des informations détaillées.

## <a name="prerequisites"></a>Conditions préalables


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Récupérez l’ID de l’erreur sur laquelle vous souhaitez des informations détaillées. Pour obtenir cet ID, utilisez le [obtenir des rapports d’erreurs données pour votre Xbox One jeu](get-error-reporting-data-for-your-xbox-one-game.md) méthode et l’utilisation du **failureHash** valeur dans le corps de réponse de cette méthode.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failuredetails``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| applicationId | chaîne | Le **Store ID** du jeu Xbox One pour lequel vous récupérez des détails de l’erreur. Le **Store ID** est disponible sur la page identité des applications dans le centre de partenaires. Un exemple **Store ID** est 9WZDNCRFJ3Q8. |  Oui  |
| failureHash | chaîne | ID de l’erreur sur laquelle vous souhaitez des informations détaillées. Pour obtenir cette valeur pour l’erreur que vous êtes intéressé, utilisez le [obtenir des rapports d’erreurs données pour votre Xbox One jeu](get-error-reporting-data-for-your-xbox-one-game.md) méthode et l’utilisation du **failureHash** valeur dans le corps de réponse de cette méthode. |  Oui  |
| startDate | date | Date de début des données à récupérer concernant l’erreur. La valeur par défaut est de 30 jours avant la date actuelle. |  Non  |
| endDate | date | Date de fin des données à récupérer concernant l’erreur. La valeur par défaut est la date actuelle. |  Non  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10 et skip=0 pour obtenir les 10 premières lignes de données, top=10 et skip=10 pour obtenir les 10 lignes suivantes, et ainsi de suite. |  Non  |
| filter |chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ Nom dans le corps de la réponse et une valeur, qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. Vous pouvez spécifier les champs suivants dans le corps de réponse :<p/><ul><li><strong>market</strong></li><li><strong>date</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul> | Non   |
| orderby | chaîne | Instruction commandant les valeurs des données de résultats. Syntaxe : <em>orderby=field [order],field [order],...</em>. Le paramètre <em>field</em> peut comporter l’une des chaînes suivantes :<ul><li><strong>market</strong></li><li><strong>date</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul><p>Le paramètre <em>order</em>, facultatif, peut comporter les valeurs <strong>asc</strong> ou <strong>desc</strong> afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est <strong>asc</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em> : <em>orderby=date,market</em></p> |  Non  |


### <a name="request-example"></a>Exemple de requête

Les exemples suivants illustrent plusieurs demandes d’obtention des données d’erreur détaillées pour une Xbox One jeu. Remplacez le *applicationId* valeur avec le **Store ID** pour votre jeu.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failuredetails?applicationId=BRRT4NJ9B3D1&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failuredetails?applicationId=BRRT4NJ9B3D1&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'Windows.Desktop' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type    | Description    |
|------------|---------|------------|
| Valeur      | tableau   | Tableau d’objets comportant des données d’erreur détaillées. Pour plus d’informations sur les données de chaque objet, consultez la section [Valeurs des informations d’erreur](#error-detail-values) ci-dessous.          |
| @nextLink  | chaîne  | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur 10, mais que plus de 10 lignes d’erreur sont associées à la requête. |
| TotalCount | Entier | Nombre total de lignes dans les résultats de la requête.        |


<span id="error-detail-values"/>

### <a name="error-detail-values"></a>Valeurs des informations d’erreur

Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur           | Type    | Description     |
|-----------------|---------|----------------------------|
| applicationId   | chaîne  | Le **Store ID** du jeu Xbox One pour lequel vous avez récupéré les données d’erreur détaillé.      |
| failureHash     | chaîne  | Identificateur unique de l’erreur.     |
| failureName     | chaîne  | Le nom de l'échec, qui est constitué de quatre parties : une ou plusieurs classes de problème, un code de vérification d’exception/d’erreur, le nom de l'image où l’échec s’est produit et le nom de la fonction associée.           |
| date            | chaîne  | Date de début des données d’erreur. Si la requête spécifiait un jour précis, cette valeur correspond à la date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| cabId           | chaîne  | ID unique du fichier CAB associé à cette erreur.   |
| cabExpirationTime  | chaîne  | Date et heure auxquelles le fichier CAB est arrivé à expiration et n’est plus téléchargeable au format ISO 8601.   |
| market          | chaîne  | Code pays ISO 3166 du marché des appareils.     |
| osBuild         | chaîne  | Numéro de version du système d’exploitation sur lequel l’erreur s’est produite.       |
| packageVersion  | chaîne  | La version du package de jeu qui est associé à cette erreur.    |
| deviceModel           | chaîne  | Une des chaînes suivantes qui spécifie la console Xbox One sur lequel le jeu était en cours d’exécution lorsque l’erreur s’est produite.<p/><ul><li><strong>Microsoft-Xbox One</strong></li><li><strong>Microsoft-Xbox One S</strong></li><li><strong>Microsoft Xbox One X</strong></li></ul>  |
| osVersion       | chaîne  | Version de système d’exploitation sur laquelle l’erreur s’est produite. Il s’agit toujours de la valeur **Windows 10**.    |
| osRelease       | chaîne  |  Une des chaînes suivantes qui spécifie la version du système d’exploitation de Windows 10 ou un anneau de distribution (comme une sous-population au sein de la version du système d’exploitation) sur laquelle l’erreur s’est produite.<p/><ul><li><strong>Version 1507</strong></li><li><strong>Version 1511</strong></li><li><strong>Version 1607</strong></li><li><strong>Version 1703</strong></li><li><strong>Version 1709</strong></li><li><strong>Version 1803</strong></li><li><strong>Release Preview</strong></li><li><strong>Insider rapide</strong></li><li><strong>Insider lente</strong></li></ul><p>Si la version du système d’exploitation ou l'anneau de distribution de version d’évaluation est inconnu(e), ce champ comporte la valeur <strong>Inconnu</strong>.</p>    |
| deviceType      | chaîne  | Type d’appareil sur lequel l’erreur s’est produite. Il s’agit toujours de la valeur **Console**.     |
| cabDownloadable           | Booléen  | Indique si le fichier CAB est téléchargeable par cet utilisateur.   |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "applicationId": "BRRT4NJ9B3D1",
      "failureHash": "012345-5dbc9-b12f-c124-9d9810f05d8b",
      "failureName": "STOWED_EXCEPTION_System.UriFormatException_exe!ContosoSports.GroupedItems+_ItemView_ItemClick_d__9.MoveNext",
      "date": "2018-02-05 09:11:25",
      "cabId": "133637331323",
      "cabExpirationTime": "2016-12-05 09:11:25",
      "market": "US",
      "osBuild": "10.0.17134",
      "packageVersion": "1.0.2.6",
      "deviceModel": "Microsoft-Xbox One",
      "osVersion": "Windows 10",
      "osRelease": "Version 1803",
      "deviceType": "Console",
      "cabDownloadable": false
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Accéder aux données d’analytique à l’aide des services de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des rapports d’erreurs données pour votre Xbox One jeu](get-error-reporting-data-for-your-xbox-one-game.md)
* [Obtenir la trace de pile pour une erreur dans votre Xbox One jeu](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)
* [Télécharger le fichier CAB pour une erreur dans votre jeu Xbox One](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)
