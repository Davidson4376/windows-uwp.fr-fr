---
author: mcleanbyron
description: Utilisez cette méthode dans l’API d’analyse du MicrosoftStore pour obtenir les informations concernant une erreur spécifique de votre application de bureau.
title: Obtenir les informations sur une erreur de votre application de bureau
ms.author: mcleans
ms.date: 06/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, services du MicrosoftStore, API d'analyse du MicrosoftStore, erreurs, détails, application de bureau
ms.localizationpriority: medium
ms.openlocfilehash: 31b7684878eb0f8921d81c8fda9d262a21e12132
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/03/2018
ms.locfileid: "4320782"
---
# <a name="get-details-for-an-error-in-your-desktop-application"></a>Obtenir les informations sur une erreur de votre application de bureau

Utilisez cette méthode dans l’API d’analyse du MicrosoftStore pour obtenir les informations concernant une erreur spécifique de votre app, au format JSON. Cette méthode ne récupère que les informations concernant les erreurs survenues dans les 30derniers jours. Ces informations sont également disponibles dans le [Rapport d’intégrité](https://msdn.microsoft.com/library/windows/desktop/mt826504) pour les applications de bureau dans le tableau de bord du Centre de développement Windows.

Pour utiliser cette méthode, vous devez d’abord utiliser la méthode [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md) afin de récupérer l’ID de l’erreur sur laquelle vous souhaitez des informations détaillées.

## <a name="prerequisites"></a>Conditions préalables


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Récupérez l’ID de l’erreur sur laquelle vous souhaitez des informations détaillées. Pour ce faire, utilisez la méthode [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md) et utilisez la valeur **failureHash** dans le corps de la réponse de cette méthode.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failuredetails``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Requis  
|---------------|--------|---------------|------|
| applicationId | chaîne | L’ID produit d'une application de bureau pour laquelle vous souhaitez récupérer les détails de l'erreur. Pour obtenir l’ID de produit d’une application de bureau, ouvrez un [rapport d'analyse du centre de développement relatif à votre application de bureau](https://msdn.microsoft.com/library/windows/desktop/mt826504) (comme le **rapport d’intégrité**) et récupérez l’ID de produit à partir de l’URL. |  Oui  |
| failureHash | chaîne | ID de l’erreur sur laquelle vous souhaitez des informations détaillées. Pour obtenir la valeur correspondant à l’erreur qui vous intéresse, utilisez la méthode [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md) et utilisez la valeur **failureHash** dans le corps de la réponse de cette méthode. |  Oui  |
| startDate | date | Date de début des données à récupérer concernant l’erreur. La valeur par défaut est de 30jours avant la date actuelle.<p/><p/>**Remarque:**&nbsp;&nbsp;cette méthode ne peut récupérer les informations concernant les erreurs qui se sont produites au cours des 30 derniers jours. |  Non  |
| endDate | date | Date de fin des données à récupérer concernant l’erreur. La valeur par défaut est la date actuelle |  Non  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10 et skip=0 pour obtenir les 10premières lignes de données, top=10 et skip=10 pour obtenir les 10lignes suivantes, et ainsi de suite. |  Non  |
| filter |chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ Nom dans le corps de la réponse et une valeur, qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. Vous pouvez spécifier les champs suivants dans le corps de réponse:<p/><ul><li><strong>marché</strong></li><li><strong>date</strong></li><li><strong>cabIdHash</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>applicationVersion</strong></li><li><strong>osBuild</strong></li><li><strong>fileName</strong></li></ul> | Non   |
| orderby | chaîne | Une instruction commandant les valeurs des données de résultat. La syntaxe est la suivante <em>orderby=field [order],field [order],...</em>. Le paramètre <em>champ</em> peut être l'une des chaînes suivantes:<ul><li><strong>marché</strong></li><li><strong>date</strong></li><li><strong>cabIdHash</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>applicationVersion</strong></li><li><strong>osBuild</strong></li><li><strong>fileName</strong></li></ul><p>Le paramètre facultatif <em>order</em> peut avoir la valeur <strong>asc</strong> ou <strong>desc</strong> pour spécifier l’ordre croissant ou décroissant de chaque champ. La valeur par défaut est <strong>asc</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em>: <em>orderby=date,market</em></p> |  Non  |


### <a name="request-example"></a>Exemple de requête

Les exemples suivants fournissent plusieurs requêtes permettant de récupérer des données d’erreur. Remplacez la valeur *applicationId* par l'ID produit pour votre application de bureau.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failuredetails?applicationId=10238467886765136388&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failuredetails?applicationId=10238467886765136388&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
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
| applicationId   | chaîne  | L'ID produit de l’application de bureau dont vous souhaitez récupérer des détails de l’erreur.      |
| failureHash     | chaîne  | Identificateur unique de l’erreur.     |
| failureName     | chaîne  | Le nom de l'échec, qui se compose de quatre partie: une ou plusieurs classes de problème, un code de vérification d'exception ou de bogue, le nom de l'image où l'erreur s'est produite et le nom de la fonction associée.           |
| date            | chaîne  | Date de début des données d’erreur. Si la requête spécifiait un jour unique, cette valeur est cette date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| cabIdHash           | chaîne  | Le hachage de l'ID unique du fichierCAB associé à cette erreur.   |
| cabExpirationTime  | chaîne  | Date et heure auxquelles le fichierCAB est arrivé à expiration et n’est plus téléchargeable au format ISO8601.   |
| marché          | chaîne  | Code pays ISO3166 du marché des appareils.     |
| osBuild         | chaîne  | Numéro de version du système d’exploitation sur lequel l’erreur s’est produite.       |
| applicationVersion         | chaîne  |   La version du fichier exécutable de l’application dans laquelle l’erreur s’est produite.     |
| deviceModel           | chaîne  | Chaîne identifiant le modèle d’appareil sur lequel l’app s’exécutait lorsque l’erreur s’est produite.   |
| osVersion       | chaîne  | Une des chaînes suivantes qui spécifie la version du système d'exploitation sur laquelle l’application de bureau est installée:<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows8.1</strong></li><li><strong>Windows10</strong></li><li><strong>Windows Server2016</strong></li><li><strong>Windows Server1709</strong></li><li><strong>Inconnu</strong></li></ul>    |
| osRelease       | chaîne  |  L'une des chaînes suivantes qui spécifie la version du système d’exploitation ou l'anneau de distribution de version d’évaluation (comme une sous-population dans la version du système d’exploitation) sur laquelle/lequel l'erreur s'est produite.<p/><p>Pour Windows10:</p><ul><li><strong>Version 1507</strong></li><li><strong>Version 1511</strong></li><li><strong>Version 1607</strong></li><li><strong>Version 1703</strong></li><li><strong>Version 1709</strong></li><li><strong>Version 1803</strong></li><li><strong>Release Preview</strong></li><li><strong>Insider Rapides</strong></li><li><strong>Insider Lent</strong></li></ul><p/><p>Pour WindowsServer1709:</p><ul><li><strong>RTM</strong></li></ul><p>Pour WindowsServer2016:</p><ul><li><strong>Version 1607</strong></li></ul><p>Pour Windows8.1:</p><ul><li><strong>Mise à jour 1</strong></li></ul><p>Pour Windows7:</p><ul><li><strong>Service Pack1</strong></li></ul><p>Si la version du système d’exploitation ou l'anneau de distribution de version d’évaluation est inconnu(e), ce champ comporte la valeur <strong>Inconnu</strong>.</p>    |
| deviceType      | chaîne  | Une des chaînes suivantes indiquant le type d’appareil sur lequel l’erreur s’est produite: <p/><ul><li><strong>PC</strong></li><li><strong>Serveur</strong></li><li><strong>Inconnu</strong></li></ul>     |
| cabDownloadable           | Booléen  | Indique si le fichier CAB est téléchargeable par cet utilisateur.   |
| fileName           | chaîne  | Le nom du fichier exécutable de l’application de bureau dont vous souhaitez récupérer des détails de l’erreur.  |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "applicationId": "10238467886765136388",
      "failureHash": "012345-5dbc9-b12f-c124-9d9810f05d8b",
      "failureName": "NULL_CLASS_PTR_WRITE_c0000005_contoso.exe!unknown_error_in_process",
      "date": "2018-01-28 23:55:29",
      "cabIdHash": "54ffb83a-e159-41d2-8158-f36f306cc01e",
      "cabExpirationTime": "2018-02-27 23:55:29",
      "market": "US",
      "osBuild": "10.0.10240",
      "applicationVersion": "2.2.2.0",
      "deviceModel": "Contoso All-in-one",
      "osVersion": "Windows 10",
      "osRelease": "Version 1703",
      "deviceType": "PC",
      "cabDownloadable": false,
      "fileName": "contosodemo.exe"
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Rubriques associées

* [Rapport d’intégrité](../publish/health-report.md)
* [Accéder aux données d’analyse à l’aide des services du MicrosoftStore](access-analytics-data-using-windows-store-services.md)
* [Obtenir des données de rapport d'erreur pour votre application de bureau](get-desktop-application-error-reporting-data.md)
* [Obtenir la trace de pile concernant une erreur dans votre application de bureau](get-the-stack-trace-for-an-error-in-your-desktop-application.md)
* [Télécharger le fichier CAB pour une erreur dans votre application de bureau](download-the-cab-file-for-an-error-in-your-desktop-application.md)
