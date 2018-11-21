---
author: Xansky
ms.assetid: 4E4CB1E3-D213-4324-91E4-7D4A0EA19C53
description: Utilisez cette méthode dans l’API d’analytique Microsoft Store pour obtenir des données d’utilisation mensuelle application pour une plage de dates données et d’autres filtres facultatifs.
title: Obtenir l’utilisation d’applications mensuelles
ms.author: mhopkins
ms.date: 08/15/2018
ms.topic: article
keywords: Windows 10, uwp, services du Windows Store, analytique du Microsoft Store, API de l’utilisation
ms.localizationpriority: medium
ms.openlocfilehash: 585e44a884bc90c5c7e69458ad5d024d7f26a79f
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7430655"
---
# <a name="get-monthly-app-usage"></a>Obtenir l’utilisation d’applications mensuelles

Utilisez cette méthode dans l’API d’analytique Microsoft Store pour obtenir les données agrégées de l’utilisation (ne pas y compris Xbox en mode multijoueur) au format JSON pour une application au cours de la plage de dates donnée (90 derniers jours uniquement) et d’autres filtres facultatifs. Ces informations sont également disponibles dans le [rapport d’utilisation](../publish/usage-report.md) dans l’espace partenaires.

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête

### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête                                                                 |
|--------|-----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagemonthly``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre     | Type   |  Description                                                                                                    |  Requis  |
|---------------|--------|-----------------------------------------------------------------------------------------------------------------|------------|
| applicationId | chaîne | L'[ID Store](in-app-purchases-and-trials.md#store-ids) pour l’app pour laquelle vous souhaitez récupérer les avis. |  Oui       |
| startDate     | date   | Dans la plage de dates, la date de début de la récupération des avis. La valeur par défaut est la date actuelle.                   |  Non        |
| endDate       | date   | Dans la plage de dates, la date de début de la récupération des avis. La valeur par défaut est la date actuelle.                     |  Non        |
| top           | entier    | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données.                          |  Non        |
| skip          | entier    | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite.                         |  Non        |  
| filter        |chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ Nom dans le corps de la réponse et une valeur, qui sont associés aux opérateurs eq ou ne, et les instructions peuvent être combinées à l’aide des opérateurs and ou or. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre filter. Vous pouvez spécifier les champs suivants dans le corps de réponse: <ul><li>**market**</li><li>**deviceType**</li><li>**packageVersion**</li></ul>                                                                                                                                              | Non         |  
| orderby       | chaîne | Une instruction commandant les valeurs des données de résultat. La syntaxe est la suivante <em>orderby=field [order],field [order],...</em>. Le paramètre <em>champ</em> peut être l'une des chaînes suivantes:<ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**market**</li><li>**packageVersion**</li><li>**deviceType**</li><li>**subscriptionName**</li><li>**monthlySessionCount**</li><li>**engagementDurationMinutes**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li><li>**averageDailyActiveUsers**</li><li>**averageDailyActiveDevices**</li></ul><p>Le paramètre facultatif <em>order</em> peut avoir la valeur **asc** ou **desc** pour spécifier l’ordre croissant ou décroissant de chaque champ. La valeur par défaut est **asc**.</p><p>Voici un exemple de chaîne <em>orderby</em>: <em>orderby=date,market</em></p> |  Non        |
| groupby       | chaîne | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants dans le corps de réponse:<ul><li>**applicationName**</li><li>**subscriptionName**</li><li>**deviceType**</li><li>**packageVersion**</li><li>**marché**</li><li>**date**</li></ul><p>Les lignes de données renvoyées comportent les champs spécifiés dans le paramètre <em>groupby</em>, ainsi que dans les paramètres suivants :</p><ul><li>**applicationId**</li><li>**subscriptionName**</li><li>**monthlySessionCount**</li><li>**engagementDurationMinutes**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li><li>**averageDailyActiveUsers**</li><li>**averageDailyActiveDevices**</li></ul><p>Le paramètre <em>groupby</em> peut être utilisé avec le paramètre <em>aggregationLevel</em>. Par exemple: <em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  Non        |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre une demande d’obtention des données d’utilisation application mensuelle. Remplacez la valeur *applicationId* par l’ID WindowsStore de votre application.

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagemonthly?applicationId=XXXXXXXXXXXX&startDate=2018-06-01&endDate=2018-07-01 HTTP/1.1  
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type   | Description                                                                                                                         |
|------------|--------|-------------------------------------------------------------------------------------------------------------------------------------|
| Valeur      | tableau  | Un tableau d’objets contenant des données d’utilisation agrégées. Pour plus d’informations sur les données incluses dans chaque objet, voir le tableau suivant. |
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour demander la page suivante. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur10000, mais que plus de10000lignes de données d’avis sont associées à la requête.                 |
| TotalCount | entier    | Nombre total de lignes dans les résultats de données de la requête.                                                                          |

 
### <a name="usage-values"></a>Valeurs d’utilisation

Les éléments du tableau *Value* ont les valeurs suivantes:

| Valeur                     | Type    | Description                                                                                 |
|---------------------------|---------|---------------------------------------------------------------------------------------------|
| date                      | chaîne  | La première date dans la plage de dates pour les données d’utilisation. Si la requête spécifiait un jour unique, cette valeur est cette date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates.                          |
| applicationId             | chaîne  | L’ID Windows Store de l’application pour laquelle vous récupérez les données d’utilisation.                            |
| applicationName           | chaîne  | Nom d’affichage de l’application.                                                                |
| market                    | chaîne  | Le code de pays ISO 3166 du marché dans lequel le client a utilisé votre application.                   |
| packageVersion            | chaîne  | La version du package où l’utilisation s’est produite.                                            |
| deviceType                | chaîne  | L’une des chaînes suivantes qui spécifie le type d’appareil où l’utilisation s’est produite:<ul><li>**PC**</li><li>**Phone**</li><li>**Console**</li><li>**Tablette**</li><li>**IoT**</li><li>**Serveur**</li><li>**Holographic**</li><li>**Inconnu**</li></ul>                                                                                                                           |
| subscriptionName          | chaîne  | Indique si l’utilisation a été par le biais de Xbox Game Pass.                                              |
| monthlySessionCount       | long    | Le nombre de sessions utilisateur au cours du mois.                                              |
| engagementDurationMinutes | double  | Les minutes dans lequel les utilisateurs sont activement à l’aide de votre application exprimée par une période distincte, qui commence au lance de l’application (début du processus) et de fin lorsqu’il termine (fin du processus) ou après une période d’inactivité.                               |
| monthlyActiveUsers        | long    | Le nombre de clients à l’aide de l’application ce mois.                                           |
| monthlyActiveDevices      | long    | Le nombre d’appareils exécutant votre application pour une période donnée au cours du temps, qui commence au lance de l’application (début du processus) et de fin lorsqu’il termine (fin du processus) ou après une période d’inactivité.                                                        |
| monthlyNewUsers           | long    | Le nombre de clients ayant utilisé votre application pour la première fois ce mois.                    |
| averageDailyActiveUsers   | double  | Le nombre moyen de clients à l’aide de l’application sur tous les jours.                             |
| averageDailyActiveDevices | double  | Le nombre moyen d’appareils utilisés par tous les utilisateurs sur tous les jours pour interagir avec votre application. |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```http
{
  "Value": [
    {
      "date": "2018-06-01",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "monthlySessionCount": 582973,
      "engagementDurationMinutes": 16941418.7,
      "monthlyActiveUsers": 139604,
      "monthlyActiveDevices": 132296,
      "monthlyNewUsers": 88127,
      "averageDailyActiveUsers": 9099.23,
      "averageDailyActiveDevices": 8999.0
    },
    {
      "date": "2018-07-01",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "monthlySessionCount": 681460,
      "engagementDurationMinutes": 21656645.3,
      "monthlyActiveUsers": 130481,
      "monthlyActiveDevices": 123583,
      "monthlyNewUsers": 78465,
      "averageDailyActiveUsers": 8257.55,
      "averageDailyActiveDevices": 8170.58
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>Rubriquesassociées

* [Accéder aux données d’analyse à l’aide des services du MicrosoftStore](access-analytics-data-using-windows-store-services.md)
* [Obtenir quotidiennes ussage d’application](get-app-usage-daily.md)
* [Obtenir des acquisitions d’applications](get-app-acquisitions.md)
* [Obtenir des acquisitions d’extensions](get-in-app-acquisitions.md)
* [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md)
