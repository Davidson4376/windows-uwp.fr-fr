---
description: Utilisez cette méthode dans l’API d’analytique Microsoft Store pour obtenir des données d’acquisition d’un abonnement de module complémentaire pendant une période donnée et d’autres filtres facultatifs.
title: Obtenir des acquisitions d’extensions d'inscription
ms.date: 01/25/18
ms.topic: article
keywords: Windows 10 uwp, Store, aux services analytique Microsoft Store API, abonnements
ms.openlocfilehash: e33a3ded219fb4d223137b40ebe871f66589addf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594774"
---
# <a name="get-subscription-add-on-acquisitions"></a>Obtenir des acquisitions d’extensions d'inscription

Utilisez cette méthode dans l’API d’analytique Microsoft Store pour obtenir des données d’agrégation d’acquisition pour les abonnements aux modules complémentaires pour votre application pendant une période donnée et d’autres filtres facultatifs.

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/subscriptions``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description          |
|---------------|--------|--------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| applicationId | chaîne | Le [Store ID](in-app-purchases-and-trials.md#store-ids) de l’application pour laquelle vous souhaitez récupérer les données d’abonnement complémentaire acquisition. |  Oui  |
| subscriptionProductId  | chaîne | Le [Store ID](in-app-purchases-and-trials.md#store-ids) du module complémentaire d’abonnement pour lequel vous souhaitez récupérer des données d’acquisition. Si vous ne spécifiez pas cette valeur, cette méthode retourne les données d’acquisition pour tous les modules complémentaires d’abonnement pour l’application spécifiée.  | Non  |
| startDate | date | La date de début dans la plage de dates des données d’acquisition de module complémentaire d’abonnement à récupérer. La valeur par défaut est la date actuelle. |  Non  |
| endDate | date | La date de fin dans la plage de dates de données d’abonnement complémentaire acquisition à récupérer. La valeur par défaut est la date actuelle. |  Non  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut si non spécifié est 100. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=100 et skip=0 pour obtenir les 100 premières lignes de données, top=100 et skip=100 pour obtenir les 100 lignes suivantes, et ainsi de suite. |  Non  |
| filter | chaîne  | Une ou plusieurs instructions qui filtrent le corps de la réponse. Chaque instruction peut utiliser les opérateurs **eq** ou **ne** ; les instructions peuvent être combinées à l’aide de **and** ou **or**. Vous pouvez spécifier les chaînes suivantes dans les instructions de filtre (ils correspondent aux [valeurs dans le corps de réponse](#subscription-acquisition-values)) : <ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li></ul><p>Voici un exemple *filtre* paramètre : <em>filtre = date eq ' 2017-07-08'</em>.</p> | Non   |
| aggregationLevel | chaîne | Indique la plage de temps pendant laquelle récupérer les données agrégées. Il peut s’agit des chaînes suivantes : <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Par défaut, la valeur est <strong>day</strong>. | Non |
| orderby | chaîne | Une instruction qui trie les résultats de chaque acquisition de module complémentaire d’abonnement, les valeurs de données. Syntaxe : <em>orderby=field [order],field [order],...</em>. Le paramètre <em>field</em> peut comporter l’une des chaînes suivantes :<ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li></ul><p>Le paramètre <em>order</em>, facultatif, peut comporter les valeurs <strong>asc</strong> ou <strong>desc</strong> afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est <strong>asc</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em> : <em>orderby=date,market</em></p> |  Non  |
| groupby | chaîne | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants :<ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li></ul><p>Le paramètre <em>groupby</em> peut être utilisé avec le paramètre <em>aggregationLevel</em>. Par exemple : <em>groupby = marché&amp;aggregationLevel = semaine</em></p> |  Non  |


### <a name="request-example"></a>Exemple de requête

Les exemples suivants montre comment obtenir des données d’abonnement complémentaire acquisition. Remplacez le *applicationId* valeur avec l’ID de Store approprié pour votre application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/subscriptions?applicationId=9NBLGGGZ5QDR&startDate=2017-07-07&endDate=2017-07-08 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type   | Description         |
|------------|--------|------------------|
| Valeur      | tableau  | Tableau d’objets qui contiennent des données d’agrégation abonnement complémentaire acquisition. Pour plus d’informations sur les données de chaque objet, consultez le [valeurs d’acquisition des abonnements](#subscription-acquisition-values) section ci-dessous.             |
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est retournée si le **haut** paramètre de la demande est défini sur 100, mais il y a plus de 100 lignes de données d’acquisition de module complémentaire abonnement pour la requête. |
| TotalCount | entier    | Nombre total de lignes dans les résultats de la requête.       |


<span id="subscription-acquisition-values" />

### <a name="subscription-acquisition-values"></a>Valeurs d’acquisition des abonnements

Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur               | Type    | Description        |
|---------------------|---------|---------------------|
| date                | chaîne  | Première date dans la plage de dates des données d’acquisition. Si la requête spécifiait un jour précis, cette valeur correspond à la date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| subscriptionProductId      | chaîne  | Le [Store ID](in-app-purchases-and-trials.md#store-ids) du module complémentaire d’abonnement pour lequel vous récupérez des données d’acquisition.    |
| subscriptionProductName    | chaîne  | Nom complet du module complémentaire d’abonnement.         |
| applicationId       | chaîne  | Le [Store ID](in-app-purchases-and-trials.md#store-ids) de l’application pour laquelle vous récupérez des données d’abonnement complémentaire acquisition.   |
| applicationName     | chaîne  | Nom d’affichage de l’application.     |
| skuId     | chaîne  | L’ID de la [référence (SKU)](in-app-purchases-and-trials.md#products-skus) du module complémentaire d’abonnement pour lequel vous récupérez des données d’acquisition.     |
| deviceType          | chaîne  |  L'une des chaînes suivantes qui spécifie le type d’appareil ayant effectué l'acquisition :<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>HOLOGRAPHIQUE</strong></li><li><strong>Inconnu</strong></li></ul>       |
| market           | chaîne  | Le code pays ISO 3166 du marché dans lequel l’acquisition s’est produite.     |
| currencyCode         | chaîne  | Le code de devise au format ISO 4217 pour le chiffre d’affaires brut avant impôts.       |
| grossSalesBeforeTax           | Entier  | Le chiffre d’affaires brut dans la devise locale spécifié par le *currencyCode* valeur.     |
| totalActiveCount             | Entier  | Le nombre de total abonnements actifs pendant la période spécifiée. Cela équivaut à la somme de la *goodStandingActiveCount*, *pendingGraceActiveCount*, *graceActiveCount*, et *lockedActiveCount* valeurs.  |
| totalChurnCount              | Entier  | Le nombre total d’abonnements qui ont été désactivés pendant la période spécifiée. Cela équivaut à la somme de la *billingChurnCount*, *nonRenewalChurnCount*, *refundChurnCount*, *chargebackChurnCount*, *earlyChurnCount*, et *otherChurnCount* valeurs.   |
| newCount            | Entier  | Le nombre de nouvelles acquisitions d’abonnement pendant la période de temps spécifié, y compris les essais.    |
| renewCount     | Entier  | Le nombre de renouvellements d’abonnement pendant la période spécifiée, y compris les renouvellements initiée par l’utilisateur et le renouvellement automatique.        |
| goodStandingActiveCount | Entier | Le nombre d’abonnements qui ont été actives pendant la période de temps et où la date d’expiration est > = la *endDate* valeur pour la requête.    |
| pendingGraceActiveCount | Entier | Le nombre d’abonnements qui étaient actives pendant la période spécifiée, mais une défaillance de facturation, et où la date d’expiration de l’abonnement est > = la *endDate* valeur pour la requête.     |
| graceActiveCount | Entier | Le nombre d’abonnements qui étaient actives pendant la période spécifiée, mais une défaillance dans Facturation, et où :<ul><li>La date d’expiration < le *endDate* valeur pour la requête.</li><li>La fin de la période de grâce est > = la *endDate* valeur.</li></ul>        |
| lockedActiveCount | Entier | Le nombre d’abonnements qui se trouvaient dans *dunning* (autrement dit, l’abonnement est arrivent à expiration et Microsoft essaie d’acquérir des fonds pour renouveler automatiquement l’abonnement) au cours de l’heure et des périodes où :<ul><li>La date d’expiration < le *endDate* valeur pour la requête.</li><li>La fin de la période de grâce est < = le *endDate* valeur.</li></ul>        |
| billingChurnCount | Entier | Le nombre d’abonnements qui ont été désactivés pendant la période spécifiée en raison d’une défaillance pour traiter une charge de facturation et où les abonnements ont été précédemment dans dunning.        |
| nonRenewalChurnCount | Entier | Le nombre d’abonnements qui ont été désactivés pendant la période spécifiée, car ils n’étaient pas renouvelés.        |
| refundChurnCount | Entier | Le nombre d’abonnements qui ont été désactivés pendant la période spécifiée, car ils ont été remboursés.        |
| chargebackChurnCount | Entier | Le nombre d’abonnements qui ont été désactivés pendant la période spécifiée en raison d’une facturation interne.        |
| earlyChurnCount | Entier | Le nombre d’abonnements qui ont été désactivés pendant la période spécifiée alors qu’elles étaient d’arriérés.        |
| otherChurnCount | Entier | Le nombre d’abonnements qui ont été désactivés pendant la période de temps spécifié pour d’autres raisons.        |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "date": "2017-07-08",
      "subscriptionProductId": "9KDLGHH6R365",
      "subscriptionProductName": "Contoso App Subscription with One Month Free Trial",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso App",
      "skuId": "0020",
      "market": "Unknown",
      "deviceType": "PC",
      "currencyCode": "USD",
      "grossSalesBeforeTax": 0.0,
      "totalActiveCount": 1,
      "totalChurnCount": 0,
      "newCount": 0,
      "renewCount": 0,
      "goodStandingActiveCount": 1,
      "pendingGraceActiveCount": 0,
      "graceActiveCount": 0,
      "lockedActiveCount": 0,
      "billingChurnCount": 0,
      "nonRenewalChurnCount": 0,
      "refundChurnCount": 0,
      "chargebackChurnCount": 0,
      "earlyChurnCount": 0,
      "otherChurnCount": 0
    },
    {
      "date": "2017-07-08",
      "subscriptionProductId": "9JJFDHG4R478",
      "subscriptionProductName": "Contoso App Monthly Subscription",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso App",
      "skuId": "0020",
      "market": "US",
      "deviceType": "PC",
      "currencyCode": "USD",
      "grossSalesBeforeTax": 0.0,
      "totalActiveCount": 1,
      "totalChurnCount": 0,
      "newCount": 0,
      "renewCount": 0,
      "goodStandingActiveCount": 1,
      "pendingGraceActiveCount": 0,
      "graceActiveCount": 0,
      "lockedActiveCount": 0,
      "billingChurnCount": 0,
      "nonRenewalChurnCount": 0,
      "refundChurnCount": 0,
      "chargebackChurnCount": 0,
      "earlyChurnCount": 0,
      "otherChurnCount": 0
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Rapport sur les acquisitions d’extensions](../publish/add-on-acquisitions-report.md)
* [Accéder aux données d’analytique à l’aide des services de Microsoft Store](access-analytics-data-using-windows-store-services.md)

 

 
