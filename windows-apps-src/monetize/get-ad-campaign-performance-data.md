---
author: mcleanbyron
ms.assetid: A26A287C-B4B0-49E9-BB28-6F02472AE1BA
description: "Utilisez cette méthode dans l’API d’analyse du WindowsStore pour obtenir les données agrégées de performances de la campagne publicitaire de l’application considérée pour une plage de dates données, et en fonction de filtres facultatifs."
title: "Obtenir les données relatives aux performances de la campagne publicitaire"
translationtype: Human Translation
ms.sourcegitcommit: 67845c76448ed13fd458cb3ee9eb2b75430faade
ms.openlocfilehash: 8859658675d31deccc3403e4862c47a54ca25b1a

---

# Obtenir les données relatives aux performances de la campagne publicitaire


Utilisez cette méthode dans l’API d’analyse du WindowsStore pour obtenir une synthèse agrégée des données de performances de la campagne publicitaire de vos applications pour une plage de dates données, et en fonction de filtres facultatifs. Cette méthode renvoie les données au format JSON.

Cette méthode renvoie les données fournies par le [Rapport de publicité sur l’installation d’applications](../publish/app-install-ads-reports.md) sur le tableau de bord du Centre de développement Windows. Pour plus d’informations sur les campagnes publicitaires, consultez la page [Création d’une campagne de publicité pour votre application](https://msdn.microsoft.com/windows/uwp/publish/create-an-ad-campaign-for-your-app).

Pour retrouver tous les détails des campagnes publicitaires créées pour votre compte du Centre de développement, vous pouvez appliquer la [méthode de récupération des détails pour l’ensemble des campagnes publicitaires](#get-details-for-all-ad-campaigns) qui est décrite plus loin dans cet article.

## Prérequis


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Windows Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## Requête


### Syntaxe de la requête

| Méthode | URI de la requête                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion``` |

<span />

### En-tête de requête

| En-tête        | Type   | Description                |
|---------------|--------|---------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |

<span />

### Paramètres de la requête

Pour récupérer les données de performances des campagnes publicitaires d’une application spécifique, utilisez le paramètre *applicationId*. Pour récupérer les données de performances publicitaires de l’ensemble des applications associées à votre compte de développeur, omettez le paramètre *applicationId*.

| Paramètre     | Type   | Description     | Requis |
|---------------|--------|-----------------|----------|
| applicationId   | chaîne    | L’ID WindowsStore de l’application pour laquelle vous souhaitez récupérer les données de performances des campagnes publicitaires. L’ID WindowsStore est disponible dans la page [Identité de l’application](../publish/view-app-identity-details.md) du tableau de bord du Centre de développement. Exemple d’ID Windows Store: 9NBLGGH4R315. |    Non      |
|  startDate  |  date   |  La date de début dans la plage de dates des données de performances des campagnes publicitaires à récupérer, au format AAAA/MM/JJ. La valeur par défaut correspond à la date antérieure de 30jours à la date actuelle.   |   Non    |
| endDate   |  date   |  La date de fin dans la plage de dates des données de performances des campagnes publicitaires à récupérer, au format AAAA/MM/JJ. La valeur par défaut correspond à la date antérieure d’unjour à la date actuelle.   |   Non    |
| top   |  entier   |  Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données.   |   Non    |
| skip   | entier    |  Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite.   |   Non    |
| filter   |  chaîne   |  Une ou plusieurs instructions qui filtrent les lignes de la réponse. Le seul filtre pris en charge est **campaignId**. Chaque instruction peut utiliser les opérateurs **eq** ou **ne**; les instructions peuvent être combinées à l’aide de **and** ou **or**.  Voici un exemple de paramètre *filter*: ```filter=campaignId eq '100023'```.   |   Non    |
|  aggregationLevel  |  chaîne   | Indique la plage de temps pendant laquelle récupérer les données agrégées. Il peut s’agit des chaînes suivantes : <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Par défaut, la valeur est <strong>day</strong>.    |   Non    |
| orderby   |  chaîne   |  <p>Une instruction qui commande les valeurs des données de performances des campagnes publicitaires. Syntaxe: <em>orderby=field [order],field [order],...</em>. Le paramètre <em>field</em> peut comporter l’une des chaînes suivantes :</p><ul><li><strong>date</strong></li><li><strong>campaignId</strong></li></ul><p>Le paramètre <em>order</em>, facultatif, peut comporter les valeurs <strong>asc</strong> ou <strong>desc</strong> afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est <strong>asc</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em>: <em>orderby=date,campaignId</em></p>   |   Non    |
|  groupby  |  chaîne   |  <p>Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants:</p><ul><li><strong>campaignId</strong></li><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>currencyCode</strong></li></ul><p>Le paramètre <em>groupby</em> peut être utilisé avec le paramètre <em>aggregationLevel</em>. Par exemple: <em>&amp;groupby=applicationId&amp;aggregationLevel=week</em></p>   |   Non    |


<span />
 

### Exemple de requête

L’exemple suivant illustre plusieurs requêtes de récupération des données de performances des campagnes publicitaires.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion?aggregationLevel=week&groupby=applicationId,campaignId,date  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion?applicationId=9NBLGGH0XK8Z&startDate=2015/1/20&endDate=2016/8/31&skip=0&filter=campaignId eq '31007388' HTTP/1.1
Authorization: Bearer <your access token>
```

## Réponse


### Corps de la réponse

| Valeur      | Type   | Description                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Valeur      | tableau  | Un tableau d’objets qui comporte les données agrégées de performances des campagnes publicitaires. Pour plus d’informations sur les données de chaque objet, consultez la section [Objet de performances des campagnes](#campaign-performance-object) ci-dessous.                                                                                                                      |
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur 5 mais que la requête présente plus de 5éléments de données. |
| TotalCount | entier    | Nombre total de lignes des résultats de données pour la requête.                                                                                                                                                                                                                             |

<span id="campaign-performance-object" />
### Objet de performances des campagnes

Les éléments du tableau *Value* comportent les valeurs suivantes:

| Valeur               | Type   | Description            |
|---------------------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| date                | chaîne | La première date dans la plage de dates des données de performances des campagnes publicitaires. Si la requête spécifiait un jour précis, cette valeur correspond à la date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| applicationId       | chaîne | L’ID WindowsStore de l’application pour laquelle vous récupérez les données de performances des campagnes publicitaires.                     |
| campaignId     | chaîne | L’ID de la campagne publicitaire.           |
| currencyCode              | chaîne | Le code de devise du budget de la campagne.              |
| spend          | chaîne |  Le volume budgétaire consacré à la campagne publicitaire.     |
| impressions           | longue | Le nombre d’expositions publicitaires pour la campagne.        |
| installs              | longue | Le nombre d’installations d’applications associées à la campagne.   |
| clicks            | longue | Le nombre de clics sur les publicités de la campagne.      |

<span />

### Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "date": "2015-04-12",
      "applicationId": "9WZDNCRFJ31Q",
      "campaignId": "4568",
      "currencyCode": "USD",
      "spend": 700.6,
      "impressions": 200,
      "installs": 30,
      "clicks": 8
    },
    {
      "date": "2015-05-12",
      "applicationId": "9WZDNCRFJ31Q",
      "campaignId": "1234",
      "currencyCode": "USD",
      "spend": 325.3,
      "impressions": 20,
      "installs": 2,
      "clicks": 5
    }
  ],
  "@nextLink": "promotion?applicationId=9NBLGGGZ5QDR&aggregationLevel=day& startDate=2015/1/20&endDate=2016/8/31&top=2&skip=2",
  "TotalCount": 1917
}
```

<span id="get-details-for-all-ad-campaigns" />
## Obtenez des détails pour l’ensemble des campagnes publicitaires.


Appliquez cette méthode pour récupérer des détails sur l’ensemble des campagnes publicitaires créées pour les applications enregistrées sur votre compte du Centre de développement Windows. Cette méthode renvoie les données au format JSON. Pour plus d’informations sur les campagnes publicitaires, consultez la page [Création d’une campagne de publicité pour votre application](https://msdn.microsoft.com/windows/uwp/publish/create-an-ad-campaign-for-your-app).

### Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Windows Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

### Requête


#### Syntaxe de la requête

| Méthode | URI de la requête                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/adscampaign``` |

<span />

#### En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |

<span />

#### Paramètres de la requête

| Paramètre     | Type   | Description     | Obligatoire |
|---------------|--------|-----------------|----------|
| top           | entier    | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 1000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |    Non      |
| skip           | entier    | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=100 et skip=0 pour obtenir les 100 premières lignes de données, top=100 et skip=100 pour obtenir les 100 lignes suivantes, et ainsi de suite. |    Non      |


<span />
 

#### Exemple de requête

L’exemple suivant représente plusieurs requêtes de récupération des détails de l’ensemble de vos campagnes publicitaires.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/adscampaign?top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>
```

### Réponse


#### Corps de la réponse

| Valeur      | Type   | Description  |
|------------|--------|--------------|
| Valeur      | tableau  | Tableau d’objets contenant les détails de vos campagnes publicitaires. Pour plus d’informations sur les données de chaque objet, consultez la section [Objet de campagne](#campaign-object) ci-dessous.                        |
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur 5 mais que la requête présente plus de 5éléments de données. |                                   |

<span id="campaign-object" />
#### Objet de campagne

Les éléments du tableau *Value* comportent les valeurs suivantes:

| Valeur               | Type   | Description          |
|---------------------|--------|------------------|
| id     | chaîne | L’ID de la campagne publicitaire. |
| name     | chaîne | Nom de la campagne publicitaire. |
| createDate     | chaîne | Date de création de la campagne publicitaire, dans le fuseau horaireUTC. |
| startDate     | chaîne | Date de début de la campagne publicitaire, dans le fuseau horaire UTC. |
| endDate     | chaîne | Date de fin de la campagne publicitaire, dans le fuseau horaire UTC. |
| applicationId       | chaîne | IDWindowsStore de l’application à laquelle est associée la campagne publicitaire. L’ID WindowsStore est disponible dans la page [Identité de l’application](../publish/view-app-identity-details.md) du tableau de bord du Centre de développement. Exemple d’ID Windows Store: 9NBLGGH4R315.                    |
| budget     | nombre | Budget de la campagne publicitaire.           |
| budgetType    | chaîne | Type de budget pour la campagne publicitaire. Il peut s’agir des chaînes suivantes: **Monthly** ou **Total**.           |
| currencyCode              | chaîne | Le code de devise du budget de la campagne.              |
| type              | chaîne | Type de campagne publicitaire. Il peut s’agir des chaînes suivantes: **Paid**, **House** ou **Community**.             |
| status              | chaîne | Statut de la campagne publicitaire. Il peut s’agir des chaînes suivantes: **Active**, **UserPaused**, **Pending**, **Ended** ou **SystemPaused**.             |
| targetingType              | chaîne | Type de ciblage de la campagne publicitaire. Il peut s’agir des chaînes suivantes: **Auto**, **Manual** ou **Segment**.             |
| target              | dictionnaire | Dictionnaire de paires clé/valeur comportant les données de ciblage de la campagne. Pour plus d’informations sur cet objet, consultez la section [Objet cible](#target-object) ci-dessous.             |

<span />

<span id="target-object" />
#### Objet cible

Cette ressource est un dictionnaire des paires clé/valeur suivantes.

| Clé               | Type   | Valeur          |
|---------------------|--------|------------------|
| Country     | tableau |   Une ou plusieurs chaînes contenant les codesISO 3166 alpha-2 des pays ou régions ciblés par la campagne publicitaire. |
| OsVersion     | tableau | Une ou plusieurs des chaînes suivantes spécifiant les versions de systèmes d’exploitation ciblées par la campagne publicitaire: **10** ou **8.x**.  |
| Age     |  tableau |  Une ou plusieurs des chaînes suivantes spécifiant la plage d’âges ciblée: **Age13To17**, **Age18To24**, **Age25To34**, **Age35To49** ou **Age50AndAbove**. |
| Gender     |  tableau |  Une ou plusieurs des chaînes suivantes spécifiant le sexe ciblé: **Female** ou **Male**. |
| DeviceType       |  tableau  |  Une ou plusieurs des chaînes suivantes spécifiant le type d’appareil ciblé: **PC/Tablet** ou **Phone**.                  |
| Segment     |  tableau |    Une ou plusieurs chaînes comportant les ID des segments de clientèle ciblés.       |


<span />

#### Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
   "Value": [
      {
         "id":31010026,
         "name": "Contoso App 1 Campaign",
         "createDate":"2016-07-14T10:01:21Z",
         "startDate":"2016-07-21T11:23:36Z",
         "applicationId":"9NBLGGH0XK8Z",
         "budget": 100,
         "budgetType":"Monthly",
         "currencyCode": "USD",
         "type": "Paid",
         "status": "Active",
         "targetingType": "Auto",
         "target": null
      },
      {
         "id":31010025,
         "name":"Contoso App 2 Campaign",
         "createDate":"2016-07-14T10:02:21Z",
         "startDate":"2016-07-21T11:18:44Z",
         "endDate":"2016-08-21T11:18:44Z",
         "applicationId":"9WZDNCRDX48S",
         "budget": 50,
         "budgetType":"Total",
         "currencyCode": "USD",
         "type": "Paid",
         "status": "Ended",
         "targetingType": "Manual",
         "target": {
            "Country": [
               "US",
               "BR",
               "FR",
               "IN",
               "IT"
            ],
            "OsVersion": [
               "8.X"
            ],
            "Age": [
               "Age18To24",
               "Age25To34",
               "Age35To49",
               "Age50AndAbove"
            ],
            "Gender": [
               "Male"
            ],
            "DeviceType": [
               "PC/Tablet",
               "Phone"
            ]
         }
      }
   ]
}
```

## Rubriques connexes

* [Rapport de publicité sur l’installation d’applications](../publish/app-install-ads-reports.md)
* [Création d’une campagne de publicité pour votre application](https://msdn.microsoft.com/windows/uwp/publish/create-an-ad-campaign-for-your-app)
* [Accéder aux données d’analyse à l’aide des services du Windows Store](access-analytics-data-using-windows-store-services.md)



<!--HONumber=Nov16_HO1-->


