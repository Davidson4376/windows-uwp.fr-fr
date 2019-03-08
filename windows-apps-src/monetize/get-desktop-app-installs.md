---
description: Utilisez cet URI REST pour obtenir des données d’agrégation d’installation pour une application de bureau pendant une période donnée et d’autres filtres facultatifs.
title: Obtenir des installations d’application de bureau
ms.date: 03/01/2018
ms.topic: article
keywords: Windows 10, application de bureau s’installe, programme d’Application de bureau de Windows
localizationpriority: medium
ms.openlocfilehash: f9beb73dafa96c489b8a4c9dddecf59d8a047109
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612974"
---
# <a name="get-desktop-application-installs"></a>Obtenir des installations d’application de bureau


Cet URI REST permet d’obtenir des données d’agrégation d’installation au format JSON pour une application de bureau que vous avez ajoutés à la [programme d’Application de bureau Windows](https://msdn.microsoft.com/library/windows/desktop/mt826504). Cet URI permet d’obtenir des données de l’installation pendant une période donnée et d’autres filtres facultatifs. Ces informations sont également disponibles dans le [installe rapport](https://msdn.microsoft.com/library/windows/desktop/mt826504) pour les applications de bureau dans l’espace partenaires.

## <a name="prerequisites"></a>Conditions préalables


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/installbasedaily```|


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| applicationId | chaîne | L’ID de produit de l’application de bureau pour lequel vous souhaitez récupérer des données de l’installation. Pour obtenir l’ID de produit d’une application de bureau, ouvrez un [analytique de rapports pour votre application de bureau partenaires](https://msdn.microsoft.com/library/windows/desktop/mt826504) (telles que la **installe le rapport**) et récupérer l’ID de produit dans l’URL. |  Oui  |
| startDate | date | Dans la plage de dates, la date de début de la récupération des données d'installation. La valeur par défaut est de 90 jours avant la date actuelle. |  Non  |
| endDate | date | Dans la plage de dates, la date de fin de la récupération des données d'installation. La valeur par défaut est la date actuelle. |  Non  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |
| filter | chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ Nom dans le corps de la réponse et une valeur, qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. Vous pouvez spécifier les champs suivants dans le corps de réponse :<p/><ul><li><strong>applicationVersion</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li></ul> | Non   |
| orderby | chaîne | Une instruction qui commande les valeurs de données de résultats pour chaque installation. Syntaxe : <em>orderby=field [order],field [order],...</em>. Le <em>champ</em> paramètre peut être l'un des champs suivants dans le corps de réponse :<p/><ul><li><strong>productName</strong></li><li><strong>date</strong><li><strong>applicationVersion</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>installBase</strong></li></ul><p>Le paramètre <em>order</em>, facultatif, peut comporter les valeurs <strong>asc</strong> ou <strong>desc</strong> afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est <strong>asc</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em> : <em>orderby=date,market</em></p> |  Non  |
| groupby | chaîne | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants dans le corps de réponse :<p/><ul><li><strong>applicationVersion</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li></ul><p>Les lignes de données renvoyées comportent les champs spécifiés dans le paramètre <em>groupby</em>, ainsi que dans les paramètres suivants :</p><ul><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>productName</strong></li><li><strong>installBase</strong></li></ul></p> |  Non  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre plusieurs demandes pour la mise en application de bureau installer des données. Remplacez le *applicationId* valeur avec l’ID de produit pour votre application de bureau.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/installbasedaily?applicationId=1234567890&startDate=2018-01-01&endDate=2018-02-01&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/installbasedaily?applicationId=1234567890&startDate=2018-01-01&endDate=2018-02-01&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Valeur      | tableau  | Tableau d’objets contenant les données d'installation agrégées. Pour plus d’informations sur les données incluses dans chaque objet, voir le tableau suivant. |
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur 10000, mais que plus de 10000 lignes de données d’installation sont associées à la requête. |
| TotalCount | entier    | Nombre total de lignes dans les résultats de la requête. |


Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur               | Type   | Description                           |
|---------------------|--------|-------------------------------------------|
| date                | chaîne | La date associée à la valeur de base d’installation. |
| applicationId       | chaîne | Installer l’ID de produit de l’application de bureau pour lequel vous avez récupéré des données. |
| productName         | chaîne | Le nom complet de l’application de bureau tel que dérivé des métadonnées de ses fichiers exécutables associés. |
| applicationVersion  | chaîne | La version de l’exécutable de l’application qui a été installé. |
| deviceType          | chaîne | Une des chaînes suivantes qui spécifie le type d’appareil sur lequel est installée l’application de bureau :<p/><ul><li><strong>PC</strong></li><li><strong>Serveur</strong></li><li><strong>Tablet</strong></li><li><strong>Inconnu</strong></li></ul> |
| market              | chaîne | Le code de pays ISO 3166 du marché dans lequel est installée l’application de bureau. |
| osVersion           | chaîne | Une des chaînes suivantes qui spécifie la version du système d'exploitation sur laquelle l’application de bureau est installée :<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>Inconnu</strong></li></ul>   |
| osRelease           | chaîne | L'une des chaînes suivantes qui spécifie la version du système d’exploitation ou l'anneau de distribution de version d’évaluation (comme une sous-population dans la version du système d’exploitation) sur laquelle/lequel est installée l’application de bureau.<p/><p>Pour Windows 10 :</p><ul><li><strong>Version 1507</strong></li><li><strong>Version 1511</strong></li><li><strong>Version 1607</strong></li><li><strong>Version 1703</strong></li><li><strong>Version 1709</strong></li><li><strong>Release Preview</strong></li><li><strong>Insider rapide</strong></li><li><strong>Insider lente</strong></li></ul><p/><p>Pour Windows Server 1709 :</p><ul><li><strong>RTM</strong></li></ul><p>Pour Windows Server 2016 :</p><ul><li><strong>Version 1607</strong></li></ul><p>Pour Windows 8.1 :</p><ul><li><strong>Mise à jour 1</strong></li></ul><p>Pour Windows 7 :</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Si la version du système d’exploitation ou l'anneau de distribution de version d’évaluation est inconnu(e), ce champ comporte la valeur <strong>Inconnu</strong>.</p> |
| installBase         | nombre | Le nombre de périphériques distincts ayant produit installé au niveau de l’agrégation spécifiée. |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "date": "2018-01-24",
      "applicationId": "123456789",
      "productName": "Contoso Demo",
      "applicationVersion": "1.0.0.0",
      "deviceType": "PC",
      "market": "All",
      "osVersion": "Windows 10",
      "osRelease": "Version 1709",
      "installBase": 348218.0
    }
  ],
  "@nextLink": "desktop/installbasedaily?applicationId=123456789&startDate=2018-01-01&endDate=2018-02-01&top=10000&skip=10000&groupby=applicationVersion,deviceType,osVersion,osRelease",
  "TotalCount": 23012
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Programme d’Application de bureau Windows](https://msdn.microsoft.com/library/windows/desktop/mt826504)
