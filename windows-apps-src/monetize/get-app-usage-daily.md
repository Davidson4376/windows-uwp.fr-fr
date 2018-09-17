---
author: Xansky
ms.assetid: 99DB5622-3700-4FB2-803B-DA447A1FD7B7
description: Utilisez cette méthode dans l’API d’analytique Microsoft Store pour obtenir des données d’utilisation quotidienne application pour une plage de dates données et d’autres filtres facultatifs.
title: Obtenir l’utilisation quotidienne d’application
ms.author: mhopkins
ms.date: 08/15/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, services du Windows Store, analytique du Microsoft Store, API de l’utilisation
ms.localizationpriority: medium
ms.openlocfilehash: 5060c24df7242d62e2895231d7441e904987d522
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/17/2018
ms.locfileid: "3987050"
---
# <a name="get-daily-app-usage"></a>Obtenir l’utilisation quotidienne d’application

Utilisez cette méthode dans l’API d’analytique de Microsoft Store pour obtenir les données d’utilisation agrégées (ne pas y compris Xbox multijoueur) au format JSON pour une application au cours de la plage de dates donnée (90 derniers jours uniquement) et d’autres filtres facultatifs. Ces informations sont également disponibles dans le [rapport d’utilisation](../publish/usage-report.md) dans le tableau de bord du centre de développement Windows.

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête

### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête                                                               |
|--------|---------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagedaily``` |


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
| filter        |chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ Nom dans le corps de la réponse et une valeur, qui sont associés aux opérateurs eq ou ne, et les instructions peuvent être combinées à l’aide des opérateurs and ou or. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre filter. Vous pouvez spécifier les champs suivants dans le corps de réponse: <ul><li>**marché**</li><li>**deviceType**</li><li>**packageVersion**</li></ul>                                                                                                                                              | Non         |  
| orderby       | chaîne | Une instruction commandant les valeurs des données de résultat. La syntaxe est la suivante <em>orderby=field [order],field [order],...</em>. Le paramètre <em>champ</em> peut être l'une des chaînes suivantes:<ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**market**</li><li>**packageVersion**</li><li>**deviceType**</li><li>**subscriptionName**</li><li>**dailySessionCount**</li><li>**engagementDurationMinutes**</li><li>**dailyActiveUsers**</li><li>**dailyActiveDevices**</li><li>**dailyNewUsers**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li></ul><p>Le paramètre facultatif <em>order</em> peut avoir la valeur **asc** ou **desc** pour spécifier l’ordre croissant ou décroissant de chaque champ. La valeur par défaut est **asc**.</p><p>Voici un exemple de chaîne <em>orderby</em>: <em>orderby=date,market</em></p>                                                                                                   |  Non        |
| groupby       | chaîne | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants dans le corps de réponse: <ul><li>**applicationName**</li><li>**subscriptionName**</li><li>**deviceType**</li><li>**packageVersion**</li><li>**marché**</li><li>**date**</li></ul><p>Les lignes de données renvoyées comportent les champs spécifiés dans le paramètre <em>groupby</em>, ainsi que dans les paramètres suivants :</p><ul><li>**applicationId**</li><li>**subscriptionName**</li><li>**dailySessionCount**</li><li>**engagementDurationMinutes**</li><li>**dailyActiveUsers**</li><li>**dailyActiveDevices**</li><li>**dailyNewUsers**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li></ul><p>Le paramètre <em>groupby</em> peut être utilisé avec le paramètre <em>aggregationLevel</em>. Par exemple: <em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p>                                                                                                             |  Non        |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre une demande d’obtention des données d’utilisation quotidienne application. Remplacez la valeur *applicationId* par l’ID WindowsStore de votre application.

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagedaily?applicationId=XXXXXXXXXXXX&startDate=2018-08-10&endDate=2018-08-14 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse



### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type   | Description                                                                                                                         |
|------------|--------|-------------------------------------------------------------------------------------------------------------------------------------|
| Valeur      | array  | Un tableau d’objets contenant des données d’utilisation agrégées. Pour plus d’informations sur les données incluses dans chaque objet, voir le tableau suivant. |
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour demander la page suivante. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur10000, mais que plus de10000lignes de données d’avis sont associées à la requête.                 |
| TotalCount | entier    | Nombre total de lignes dans les résultats de données de la requête.                                                                          |

 
### <a name="usage-values"></a>Valeurs d’utilisation

Les éléments du tableau *Value* ont les valeurs suivantes:

| Valeur                     | Type    | Description                                                               |
|---------------------------|---------|---------------------------------------------------------------------------|
| date                      | chaîne  | La première date dans la plage de dates pour les données d’utilisation. Si la requête spécifiait un jour unique, cette valeur est cette date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates.        |
| applicationId             | chaîne  | L’ID Windows Store de l’application pour laquelle vous récupérez les données d’utilisation.          |
| applicationName           | chaîne  | Nom d’affichage de l’application.                                              |
| deviceType                | chaîne  | L’une des chaînes suivantes qui spécifie le type d’appareil où l’utilisation s’est produite:<ul><li>**PC**</li><li>**Phone**</li><li>**Console**</li><li>**Tablette**</li><li>**IoT**</li><li>**Serveur**</li><li>**Holographic**</li><li>**Inconnu**</li></ul>                                                                                                         |
| packageVersion            | chaîne  | La version du package dans lequel l’utilisation s’est produite.                          |
| marché                    | chaîne  | Le code de pays ISO 3166 du marché dans lequel le client a utilisé votre application. |
| subscriptionName          | chaîne  | Indique si l’utilisation a été par le biais de Xbox Game Pass.                            |
| dailySessionCount         | long    | Le nombre de sessions utilisateur sur ce jour.                                  |
| engagementDurationMinutes | double  | Les minutes dans lequel les utilisateurs sont activement à l’aide de votre application mesurée en une période distincte, qui commence au lance de l’application (début du processus) et de fin lorsqu’il termine (fin du processus) ou après une période d’inactivité.             |
| dailyActiveUsers          | long    | Le nombre de clients à l’aide de l’application ce jour.                           |
| dailyActiveDevices        | long    | Nombre d’appareils utilisés quotidiennement pour interagir avec votre application par tous les utilisateurs.  |
| dailyNewUsers             | long    | Le nombre de clients ayant utilisé votre application pour la première fois ce jour.    |
| monthlyActiveUsers        | long    | Le nombre de clients à l’aide de l’application ce mois.                         |
| monthlyActiveDevices      | long    | Le nombre d’appareils exécutant votre application pour une période donnée au cours du temps, qui commence au lance de l’application (début du processus) et de fin lorsqu’il termine (fin du processus) ou après une période d’inactivité.                                      |
| monthlyNewUsers           | long    | Le nombre de clients ayant utilisé votre application pour la première fois ce mois.  |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "date": "2018-08-10",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 17718,
      "engagementDurationMinutes": 582540.2,
      "dailyActiveUsers": 7078,
      "dailyActiveDevices": 6964,
      "dailyNewUsers": 1727,
      "monthlyActiveUsers": 123609,
      "monthlyActiveDevices": 116723,
      "monthlyNewUsers": 72271
    },
    {
      "date": "2018-08-11",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 19899,
      "engagementDurationMinutes": 655379.7,
      "dailyActiveUsers": 7877,
      "dailyActiveDevices": 7789,
      "dailyNewUsers": 2062,
      "monthlyActiveUsers": 123666,
      "monthlyActiveDevices": 116770,
      "monthlyNewUsers": 72276
    },
    {
      "date": "2018-08-12",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 21349,
      "engagementDurationMinutes": 735640.5,
      "dailyActiveUsers": 8456,
      "dailyActiveDevices": 8309,
      "dailyNewUsers": 2433,
      "monthlyActiveUsers": 124241,
      "monthlyActiveDevices": 117289,
      "monthlyNewUsers": 72732
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}
```

## <a name="related-topics"></a>Rubriquesassociées

* [Accéder aux données d’analyse à l’aide des services du MicrosoftStore](access-analytics-data-using-windows-store-services.md)
* [Obtenir tous les mois ussage d’application](get-app-usage-monthly.md)
* [Obtenir des acquisitions d’applications](get-app-acquisitions.md)
* [Obtenir des acquisitions d’extensions](get-in-app-acquisitions.md)
* [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md)
