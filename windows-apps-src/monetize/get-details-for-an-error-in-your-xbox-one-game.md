---
description: Utilisez cette méthode dans l’API d’analytique Microsoft Store pour obtenir les informations concernant une erreur spécifique pour votre console Xbox One jeu.
title: Obtenir des détails sur une erreur dans votre console Xbox One jeu
ms.date: 11/06/2018
ms.topic: article
keywords: windows 10, uwp, services du MicrosoftStore, API d'analyse du MicrosoftStore, erreurs, détails
ms.localizationpriority: medium
ms.openlocfilehash: 6b713e3c6c2f7b82e5779e4785cc6b2e320b24f0
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8741164"
---
# <a name="get-details-for-an-error-in-your-xbox-one-game"></a>Obtenir des détails sur une erreur dans votre console Xbox One jeu

Utilisez cette méthode dans le Microsoft Store analytique API pour obtenir les informations concernant une erreur spécifique pour votre console Xbox One jeu intégré via le portail de développement Xbox (XDP) et disponible dans le tableau de bord XDP Analytique l’espace partenaires. Cette méthode ne récupère que les informations concernant les erreurs survenues dans les 30derniers jours.

Vous pouvez utiliser cette méthode, vous devez tout d’abord utiliser la méthode [get pour votre jeu Xbox One, les données de rapport d’erreurs](get-error-reporting-data-for-your-xbox-one-game.md) pour récupérer l’ID de l’erreur dont vous souhaitez obtenir des informations détaillées.

## <a name="prerequisites"></a>Conditions préalables


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Récupérez l’ID de l’erreur sur laquelle vous souhaitez des informations détaillées. Pour obtenir cet ID, utilisez la méthode [d’obtenir les données de rapport d’erreurs pour votre console Xbox One jeu](get-error-reporting-data-for-your-xbox-one-game.md) et utilisez la valeur **failureHash** dans le corps de réponse de cette méthode.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failuredetails``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Requis  
|---------------|--------|---------------|------|
| applicationId | chaîne | L’ID de produit du jeu Xbox One pour lequel vous récupérez les détails de l’erreur. Pour obtenir l’ID produit de votre jeu, accédez à votre jeu dans le portail de développement Xbox (XDP) et récupérez l’ID produit à partir de l’URL. Par ailleurs, si vous téléchargez vos données d’intégrité à partir du rapport analytique de partenaires Windows, l’ID de produit est inclus dans le fichier .tsv. |  Oui  |
| failureHash | chaîne | ID de l’erreur sur laquelle vous souhaitez des informations détaillées. Pour obtenir la valeur de l’erreur qui que vous intéresse, utilisez la méthode [obtenir les données de rapport d’erreurs pour votre console Xbox One jeu](get-error-reporting-data-for-your-xbox-one-game.md) et utilisez la valeur **failureHash** dans le corps de réponse de cette méthode. |  Oui  |
| startDate | date | Date de début des données à récupérer concernant l’erreur. La valeur par défaut est de 30jours avant la date actuelle. |  Non  |
| endDate | date | Date de fin des données à récupérer concernant l’erreur. La valeur par défaut est la date actuelle |  Non  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10 et skip=0 pour obtenir les 10premières lignes de données, top=10 et skip=10 pour obtenir les 10lignes suivantes, et ainsi de suite. |  Non  |
| filter |chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ Nom dans le corps de la réponse et une valeur, qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. Vous pouvez spécifier les champs suivants dans le corps de réponse:<p/><ul><li><strong>marché</strong></li><li><strong>date</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul> | Non   |
| orderby | chaîne | Une instruction commandant les valeurs des données de résultat. La syntaxe est la suivante <em>orderby=field [order],field [order],...</em>. Le paramètre <em>champ</em> peut être l'une des chaînes suivantes:<ul><li><strong>marché</strong></li><li><strong>date</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul><p>Le paramètre facultatif <em>order</em> peut avoir la valeur <strong>asc</strong> ou <strong>desc</strong> pour spécifier l’ordre croissant ou décroissant de chaque champ. La valeur par défaut est <strong>asc</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em>: <em>orderby=date,market</em></p> |  Non  |


### <a name="request-example"></a>Exemple de requête

Les exemples suivants illustrent plusieurs requêtes de récupération des données d’erreur pour une console Xbox One du jeu. Remplacez la valeur *applicationId* par l’ID de produit pour votre jeu.

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
| Valeur      | array   | Tableau d’objets comportant des données d’erreur détaillées. Pour plus d’informations sur les données de chaque objet, consultez la section [Valeurs des informations d’erreur](#error-detail-values) ci-dessous.          |
| @nextLink  | chaîne  | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour demander la page suivante. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur10, mais que plus de 10lignes d’erreur sont associées à la requête. |
| TotalCount | entier | Nombre total de lignes dans les résultats de données de la requête.        |


<span id="error-detail-values"/>

### <a name="error-detail-values"></a>Valeurs des informations d’erreur

Les éléments du tableau *Value* ont les valeurs suivantes:

| Valeur           | Type    | Description     |
|-----------------|---------|----------------------------|
| applicationId   | chaîne  | L’ID de produit du jeu Xbox One pour lequel vous souhaitez récupérer des données d’erreur.      |
| failureHash     | chaîne  | Identificateur unique de l’erreur.     |
| failureName     | chaîne  | Le nom de l'échec, qui se compose de quatre partie: une ou plusieurs classes de problème, un code de vérification d'exception ou de bogue, le nom de l'image où l'erreur s'est produite et le nom de la fonction associée.           |
| date            | chaîne  | Date de début des données d’erreur. Si la requête spécifiait un jour unique, cette valeur est cette date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| cabId           | chaîne  | ID unique du fichierCAB associé à cette erreur.   |
| cabExpirationTime  | chaîne  | Date et heure auxquelles le fichierCAB est arrivé à expiration et n’est plus téléchargeable au format ISO8601.   |
| marché          | chaîne  | Code pays ISO3166 du marché des appareils.     |
| osBuild         | chaîne  | Numéro de version du système d’exploitation sur lequel l’erreur s’est produite.       |
| packageVersion  | chaîne  | La version du package de jeu associé à cette erreur.    |
| deviceModel           | chaîne  | L’une des chaînes suivantes qui spécifie la console Xbox One sur lequel le jeu a été en cours d’exécution lorsque l’erreur s’est produite.<p/><ul><li><strong>Microsoft-Xbox une</strong></li><li><strong>Microsoft-Xbox One S</strong></li><li><strong>Microsoft-Xbox One X</strong></li></ul>  |
| osVersion       | chaîne  | La version de système d’exploitation sur laquelle l’erreur s’est produite. Il s’agit toujours de la valeur de **Windows 10**.    |
| osRelease       | chaîne  |  L’une des chaînes suivantes qui spécifie la version du système d’exploitation de Windows 10 ou l’anneau évaluation (comme une sous-population au sein de la version du système d’exploitation) sur lequel l’erreur s’est produite.<p/><ul><li><strong>Version 1507</strong></li><li><strong>Version 1511</strong></li><li><strong>Version 1607</strong></li><li><strong>Version 1703</strong></li><li><strong>Version 1709</strong></li><li><strong>Version 1803</strong></li><li><strong>Release Preview</strong></li><li><strong>Insider Rapides</strong></li><li><strong>Insider Lent</strong></li></ul><p>Si la version du système d’exploitation ou l'anneau de distribution de version d’évaluation est inconnu(e), ce champ comporte la valeur <strong>Inconnu</strong>.</p>    |
| deviceType      | chaîne  | Type d’appareil sur lequel l’erreur s’est produite. Il s’agit toujours de la valeur de **Console**.     |
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

## <a name="related-topics"></a>Rubriquesassociées

* [Accéder aux données d’analyse à l’aide des services du MicrosoftStore](access-analytics-data-using-windows-store-services.md)
* [Obtenir les données rapport d’erreurs pour votre console Xbox One jeu](get-error-reporting-data-for-your-xbox-one-game.md)
* [Obtenir la trace de pile concernant une erreur dans votre console Xbox One du jeu](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)
* [Télécharger le fichier CAB concernant une erreur dans votre jeu Xbox One](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)
