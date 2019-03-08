---
description: Utilisez cet URI REST pour obtenir des blocs de données pour une application de bureau pendant une période donnée et d’autres filtres facultatifs.
title: Obtenir les blocs de mise à niveau pour votre application de bureau
ms.date: 07/11/2018
ms.topic: article
keywords: Windows 10, blocs d’application de bureau, programme d’Application de bureau de Windows
localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 08c3265fd0ff908c210a6586f561aba62ba9155a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641104"
---
# <a name="get-upgrade-blocks-for-your-desktop-application"></a>Obtenir les blocs de mise à niveau pour votre application de bureau

Utilisez cet URI REST pour obtenir des informations sur les appareils Windows 10 sur lequel votre application de bureau bloque une mise à niveau Windows 10 à partir de l’exécution. Vous pouvez utiliser cet URI uniquement pour les applications de bureau que vous avez ajoutés à la [programme d’Application de bureau Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program). Ces informations sont également disponibles dans le [Application bloque rapport](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#application-blocks-report) pour les applications de bureau dans l’espace partenaires.

Pour obtenir plus d’informations sur les blocs de l’appareil pour un exécutable spécifique dans votre application de bureau, consultez [bloc de mise à niveau Get les détails de votre application de bureau](get-desktop-block-data-details.md).

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockhits```|


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| applicationId | chaîne | L’ID de produit de l’application de bureau pour lequel vous souhaitez récupérer des blocs de données. Pour obtenir l’ID de produit d’une application de bureau, ouvrez un [analytique de rapports pour votre application de bureau partenaires](https://msdn.microsoft.com/library/windows/desktop/mt826504) (telles que la **bloque le rapport**) et récupérer l’ID de produit dans l’URL. |  Oui  |
| startDate | date | La date de début dans la plage de dates de blocs de données à récupérer. La valeur par défaut est de 90 jours avant la date actuelle. |  Non  |
| endDate | date | La date de fin dans la plage de dates de blocs de données à récupérer. La valeur par défaut est la date actuelle. |  Non  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |
| filter | chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ Nom dans le corps de la réponse et une valeur, qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. Vous pouvez spécifier les champs suivants dans le corps de réponse :<p/><ul><li><strong>applicationVersion</strong></li><li><strong>architecture</strong></li><li><strong>blockType</strong></li><li><strong>deviceType</strong></li><li><strong>fileName</strong></li><li><strong>market</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>productName</strong></li><li><strong>targetOs</strong></li></ul> | Non   |
| orderby | chaîne | Une instruction qui trie les résultats des valeurs de données pour chaque bloc. Syntaxe : <em>orderby=field [order],field [order],...</em>. Le <em>champ</em> paramètre peut être l'un des champs suivants dans le corps de réponse :<p/><ul><li><strong>applicationVersion</strong></li><li><strong>architecture</strong></li><li><strong>blockType</strong></li><li><strong>date</strong><li><strong>deviceType</strong></li><li><strong>fileName</strong></li><li><strong>market</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>productName</strong></li><li><strong>targetOs</strong></li><li><strong>deviceCount</strong></li></ul><p>Le paramètre <em>order</em>, facultatif, peut comporter les valeurs <strong>asc</strong> ou <strong>desc</strong> afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est <strong>asc</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em> : <em>orderby=date,market</em></p> |  Non  |
| groupby | chaîne | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants dans le corps de réponse :<p/><ul><li><strong>applicationVersion</strong></li><li><strong>architecture</strong></li><li><strong>blockType</strong></li><li><strong>deviceType</strong></li><li><strong>fileName</strong></li><li><strong>market</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>targetOs</strong></li></ul><p>Les lignes de données renvoyées comportent les champs spécifiés dans le paramètre <em>groupby</em>, ainsi que dans les paramètres suivants :</p><ul><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>productName</strong></li><li><strong>deviceCount</strong></li></ul></p> |  Non  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre plusieurs demandes d’obtention de données de bloc d’application de bureau. Remplacez le *applicationId* valeur avec l’ID de produit pour votre application de bureau.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockhits?applicationId=5126873772241846776&startDate=2018-05-01&endDate=2018-06-07&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockhits?applicationId=5126873772241846776&startDate=2018-05-01&endDate=2018-06-07&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Valeur      | tableau  | Un tableau d’objets qui contiennent des données d’agrégation de bloc. Pour plus d’informations sur les données incluses dans chaque objet, voir le tableau suivant. |
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est retournée si le **haut** paramètre de la requête est définie sur 10 000, mais il y a plus de 10 000 lignes de données de bloc pour la requête. |
| TotalCount | entier    | Nombre total de lignes dans les résultats de la requête. |


Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur               | Type   | Description                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | chaîne | L’ID de produit de l’application de bureau pour lequel vous avez récupéré les blocs de données. |
| date                | chaîne | La date associée à la valeur de présences dans le bloc. |
| productName         | chaîne | Le nom complet de l’application de bureau tel que dérivé des métadonnées de ses fichiers exécutables associés. |
| fileName            | chaîne | Fichier exécutable qui a été bloquée. |
| applicationVersion  | chaîne | La version de l’exécutable de l’application qui a été bloquée. |
| osVersion           | chaîne | Une des chaînes suivantes qui spécifie la version du système d’exploitation sur lequel l’application de bureau est en cours d’exécution :<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>Inconnu</strong></li></ul>   |
| osRelease           | chaîne | Une des chaînes suivantes qui spécifie la version de système d’exploitation ou d’un anneau de distribution (comme une sous-population au sein de la version du système d’exploitation) sur lequel l’application de bureau est en cours d’exécution.<p/><p>Pour Windows 10 :</p><ul><li><strong>Version 1507</strong></li><li><strong>Version 1511</strong></li><li><strong>Version 1607</strong></li><li><strong>Version 1703</strong></li><li><strong>Version 1709</strong></li><li><strong>Release Preview</strong></li><li><strong>Insider rapide</strong></li><li><strong>Insider lente</strong></li></ul><p/><p>Pour Windows Server 1709 :</p><ul><li><strong>RTM</strong></li></ul><p>Pour Windows Server 2016 :</p><ul><li><strong>Version 1607</strong></li></ul><p>Pour Windows 8.1 :</p><ul><li><strong>Mise à jour 1</strong></li></ul><p>Pour Windows 7 :</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Si la version du système d’exploitation ou l'anneau de distribution de version d’évaluation est inconnu(e), ce champ comporte la valeur <strong>Inconnu</strong>.</p> |
| market              | chaîne | Le code de pays ISO 3166 du marché dans lequel l’application de bureau est bloquée. |
| deviceType          | chaîne | Une des chaînes suivantes qui spécifie le type d’appareil sur lequel l’application de bureau est bloquée :<p/><ul><li><strong>PC</strong></li><li><strong>Serveur</strong></li><li><strong>Tablet</strong></li><li><strong>Inconnu</strong></li></ul> |
| blockType            | chaîne | Une des chaînes suivantes qui spécifie le type de bloc trouvé sur l’appareil :<p/><ul><li><strong>Sédiment potentiel</strong></li><li><strong>Sédiment temporaire</strong></li><li><strong>Notification d’exécution</strong></li></ul><p/><p/>Pour plus d’informations sur ces types de blocs et leur signification pour les développeurs et les utilisateurs, consultez la description de la [Application bloque rapport](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#application-blocks-report).  |
| architecture        | chaîne | L’architecture de l’appareil sur lequel se trouve le bloc : <p/><ul><li><strong>ARM64</strong></li><li><strong>X86</strong></li></ul> |
| targetOs            | chaîne | Une des chaînes suivantes qui spécifie la version du système d’exploitation de Windows 10 sur lequel l’application de bureau est bloquée à partir d’en cours d’exécution : <p/><ul><li><strong>Version 1709</strong></li><li><strong>Version 1803</strong></li></ul> |
| deviceCount         | nombre | Le nombre de périphériques distincts qui contiennent des blocs au niveau de l’agrégation spécifiée. |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
     "applicationId": "10238467886765136388",
     "date": "2018-06-03",
     "productName": "Contoso Demo",
     "fileName": "contosodemo.exe",
     "applicationVersion": "2.2.2.0",
     "osVersion": "Windows 8.1",
     "osRelease": "Update 1",
     "market": "ZA",
     "deviceType": "All",
     "blockType": "Runtime Notification",
     "architecture": "X86",
     "targetOs": "RS4",
     "deviceCount": 120
    }
  ],
  "@nextLink": "desktop/blockhits?applicationId=123456789&startDate=2018-01-01&endDate=2018-02-01&top=10000&skip=10000&groupby=applicationVersion,deviceType,osVersion,osRelease",
  "TotalCount": 23012
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Programme d’Application de bureau Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)
* [Obtenir les détails de la mise à niveau bloc pour votre application de bureau](get-desktop-block-data-details.md)
