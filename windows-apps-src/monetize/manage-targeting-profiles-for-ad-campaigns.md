---
author: mcleanbyron
ms.assetid: d305746a-d370-4404-8cde-c85765bf3578
description: Utilisez cette méthode dans l’API des promotions du MicrosoftStore pour gérer les profils de ciblage dans le cadre de campagnes publicitaires.
title: Gérer les profils de ciblage
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, API de promotions du MicrosoftStore, campagnes de publicité
ms.localizationpriority: medium
ms.openlocfilehash: 692da5c2cc45e64d3feeab6136c1e50c72a7b0b0
ms.sourcegitcommit: 1773bec0f46906d7b4d71451ba03f47017a87fec
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/17/2018
ms.locfileid: "1664189"
---
# <a name="manage-targeting-profiles"></a>Gérer les profils de ciblage


Utilisez ces méthodes dans l’API des promotions du MicrosoftStore pour sélectionner les utilisateurs, les zones géographiques et les types d’inventaire que vous souhaitez cibler pour chaque ligne de livraison d’une campagne publicitaire promotionnelle. Les profils de ciblage peuvent être créés et réutilisés sur plusieurs chaînes de livraison.

Pour plus d’informations sur la relation entre les profils de ciblage et les campagnes de publicité, les lignes de livraison et les contenus créatifs, voir [Exécuter des campagnes publicitaires à l’aide des services du MicrosoftStore](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

## <a name="prerequisites"></a>Prérequis

Pour utiliser ces méthodes, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](run-ad-campaigns-using-windows-store-services.md#prerequisites) relatives à l’API de promotions du MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de ces méthodes. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête

Ces méthodes présentent les URI suivants.

| Type de méthode | URI de la requête                                                      |  Description  |
|--------|------------------------------------------------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile``` |  Crée un profil de ciblage.  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile/{targetingProfileId}``` |  Remplace le profil de ciblage spécifié par la valeur *targetingProfileId*.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile/{targetingProfileId}``` |  Récupère le profil de ciblage défini par la valeur *targetingProfileId*.  |


### <a name="header"></a>En-tête

| En-tête        | Type   | Description         |
|---------------|--------|---------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès AzureAD sous la forme **Bearer** &lt;*jeton*&gt;. |
| ID de suivi   | GUID   | Facultatif. ID de suivi du flux d’appels.                                  |


### <a name="request-body"></a>Corps de la requête

Les méthodes POST et PUT nécessitent un corps de requêteJSON avec les champs requis d’un objet de [profil de ciblage](#targeting-profile) et tous les champs que vous souhaitez définir ou modifier.


### <a name="request-examples"></a>Exemples de requête

L’exemple suivant vous explique comment appeler la méthode POST pour créer un profil de ciblage.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile HTTP/1.1
Authorization: Bearer <your access token>

{
    "name": "Contoso App Campaign - Targeting Profile 1",
    "targetingType": "Manual",
    "age": [
      651,
      652],
    "gender": [
      700
    ],
    "country": [
      11,
      12
    ],
    "osVersion": [
      504
    ],
    "deviceType": [
      710
    ],
    "supplyType": [
      11470
    ]
}
```

L’exemple suivant vous explique comment appeler la méthode GET pour récupérer un profil de ciblage.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile/310023951  HTTP/1.1
Authorization: Bearer <your access token>
```

<span/>

## <a name="response"></a>Réponse

Ces méthodes renvoient un corps de réponseJSON avec un objet de [profil de ciblage](#targeting-profile) comportant des informations sur le profil de ciblage créé, mis à jour ou récupéré. L’exemple suivant représente un corps de réponse associé à ces méthodes.

```json
{
  "Data": {
    "id": 310021746,
    "name": "Contoso App Campaign - Targeting Profile 1",
    "targetingType": "Manual",
    "age": [
      651,
      652
    ],
    "gender": [
      700
    ],
    "country": [
      6,
      13,
      29
    ],
    "osVersion": [
      504,
      505,
      506,
      507,
      508
    ],
    "deviceType": [
      710,
      711
    ],
    "supplyType": [
      11470
    ]
  }
}
```

<span id="targeting-profile"/>

## <a name="targeting-profile-object"></a>Objet de profil de ciblage

Les corps de requête et de réponse associés à ces méthodes comportent les champs suivants. Ce tableau signale les champs en lecture seule (qui ne peuvent être modifiés dans la méthodePUT) et les champs requis dans le corps de requête associé à la méthodePOST.

| Champ        | Type   |  Description      |  Lecture seule  | Valeur par défaut  | Requis pour la méthode POST |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  entier   |  ID du profil de ciblage.     |   Oui    |       |   Non      |       
|  name   |  chaîne   |   Nom du profil de ciblage.    |    Non   |      |  Oui     |       
|  targetingType   |  chaîne   |  Utilisez l’une des valeurs suivantes: <ul><li>**Auto**: définissez cette valeur pour autoriser Microsoft à sélectionner le profil de ciblage en fonction des paramètres de votre application dans le Centre de développement.</li><li>**Manual**: définissez cette valeur pour définir votre propre profil de ciblage.</li></ul>     |  Non     |  Auto    |   Oui    |       
|  age   |  tableau   |   Un ou plusieurs nombres entiers identifiant les tranches d’âge des utilisateurs à cibler. Pour obtenir une liste complète des entiers, consultez la section [Valeurs d’âge](#age-values) de cet article.    |    Non    |  null    |     Non    |       
|  gender   |  tableau   |  Un ou plusieurs nombres entiers identifiant les genres des utilisateurs à cibler. Pour obtenir une liste complète des entiers, consultez la section [Valeurs de sexe](#gender-values) de cet article.       |  Non    |  null    |     Non    |       
|  country   |  tableau   |  Un ou plusieurs nombres entiers identifiant les codes pays des utilisateurs à cibler. Pour obtenir une liste complète des entiers, consultez la section [Valeurs de codes pays](#country-code-values) de cet article.    |  Non    |  null   |      Non   |       
|  osVersion   |  tableau   |   Un ou plusieurs nombres entiers identifiant les versions de systèmes d’exploitation des utilisateurs à cibler. Pour obtenir une liste complète des entiers, consultez la section [Valeurs des versions de systèmes d’exploitation](#osversion-values) de cet article.     |   Non    |  null   |     Non    |       
|  deviceType   | tableau    |  Un ou plusieurs nombres entiers identifiant les types d’appareil des utilisateurs à cibler. Pour obtenir une liste complète des entiers, consultez la section [Valeurs des types d’appareil](#devicetype-values) de cet article.       |   Non    |  null    |    Non     |       
|  supplyType   |  tableau   |  Un ou plusieurs nombres entiers identifiant le type de stock pour lequel les campagnes publicitaires s’affichent. Pour obtenir une liste complète des nombres entiers, consultez la section [Valeurs de type d’approvisionnement](#supplytype-values) de cet article.      |   Non    |  null   |     Non    |   |  


<span id="age-values"/>

### <a name="age-values"></a>Valeurs d’âge

Le champ *âge* de l'objet [TargetingProfile](#targeting-profile) contient un ou plusieurs des entiers suivants qui identifient les tranches d’âge des utilisateurs à cibler.

|  Valeur entière pour le champ *âge*  |  Tranche d’âge correspondante  |  
|---------------------------------|---------------------------|
|     651     |            13 à 17             |
|     652     |           18 à 24             |
|     653     |            25 à 34             |
|     654     |            35 à 49             |
|     655     |            50 et plus             |

Pour obtenir les valeurs prises en charge pour le champ *âge* de manière programmée, vous pouvez utiliser la méthode GET suivante.  Pour l’en-tête ```Authorization```, communiquez votre jeton d’accès AzureAD, sous la forme **Bearer** &lt;*jeton*&gt;.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/age
Authorization: Bearer <your access token>
```

L’exemple suivant représente le corps de réponse pour cette méthode.

```json
{
  "Data": {
    "Age": {
      "651": "Age13To17",
      "652": "Age18To24",
      "653": "Age25To34",
      "654": "Age35To49",
      "655": "Age50AndAbove"
    }
  }
}
```

<span id="gender-values"/>

### <a name="gender-values"></a>Valeurs de sexe

Le champ *gender* de l’objet de [TargetingProfile](#targeting-profile) comporte un ou plusieurs des entiers suivants identifiant le sexe des utilisateurs à cibler.

|  Valeur entière pour le champ *gender*  |  Sexe correspondant  |  
|---------------------------------|---------------------------|
|     700     |            Homme             |
|     701     |           Femme             |

Pour obtenir les valeurs prises en charge pour le champ *gender* automatiquement, vous pouvez appeler la méthode GET suivante.  Pour l’en-tête ```Authorization```, communiquez votre jeton d’accès AzureAD, sous la forme **Bearer** &lt;*jeton*&gt;.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/gender
Authorization: Bearer <your access token>
```

L’exemple suivant représente le corps de réponse pour cette méthode.

```json
{
  "Data": {
    "Gender": {
      "700": "Male",
      "701": "Female"
    }
  }
}
```


<span id="osversion-values"/>

### <a name="os-version-values"></a>Valeurs des versions de systèmes d’exploitation

Le champ *osVersion* de l’objet [TargetingProfile](#targeting-profile) comporte un ou plusieurs des entiers suivants identifiant les versions de système d’exploitation des utilisateurs à cibler.

|  Valeur entière pour le champ*osVersion*  |  Version correspondante de système d’exploitation  |  
|---------------------------------|---------------------------|
|     500     |            Windows Phone7             |
|     501     |           Windows Phone 7.1             |
|     502     |           Windows Phone 7.5             |
|     503     |           Windows Phone 7.8             |
|     504     |           Windows Phone 8.0             |
|     505     |           Windows Phone 8.1             |
|     506     |           Windows8.0             |
|     507     |           Windows8.1             |
|     508     |           Windows10             |
|     509     |           Windows10 Mobile             |

Pour obtenir les valeurs prises en charge pour le champ *osVersion* automatiquement, vous pouvez appeler la méthodeGET suivante.  Pour l’en-tête ```Authorization```, communiquez votre jeton d’accès AzureAD, sous la forme **Bearer** &lt;*jeton*&gt;.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/osversion
Authorization: Bearer <your access token>
```

L’exemple suivant représente le corps de réponse pour cette méthode.

```json
{
  "Data": {
    "OsVersion": {
      "500": "WindowsPhone70",
      "501": "WindowsPhone71",
      "502": "WindowsPhone75",
      "503": "WindowsPhone78",
      "504": "WindowsPhone80",
      "505": "WindowsPhone81",
      "506": "Windows80",
      "507": "Windows81",
      "508": "Windows10",
      "509": "WindowsPhone10"
    }
  }
}
```


<span id="devicetype-values"/>

### <a name="device-type-values"></a>Valeurs des types d’appareil

Le champ *deviceType* de l’objet de [TargetingProfile](#targeting-profile) comporte un ou plusieurs des entiers suivants identifiant les types d’appareil des utilisateurs à cibler.

|  Valeur entière pour le champ *deviceType*  |  Type d’appareil correspondant  |  Description  |
|---------------------------------|---------------------------|---------------------------|
|     710     |  Windows   |  Cette valeur représente les appareils exécutant une version bureau de Windows10 ou de Windows8.x.  |
|     711     |  Téléphone     |  Cette valeur représente les appareils exécutant Windows 10 Mobile, Windows Phone 8.x ou Windows Phone 7.x.

Pour obtenir les valeurs prises en charge pour le champ *deviceType* automatiquement, vous pouvez appeler la méthodeGET suivante.  Pour l’en-tête ```Authorization```, communiquez votre jeton d’accès AzureAD, sous la forme **Bearer** &lt;*jeton*&gt;.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/devicetype
Authorization: Bearer <your access token>
```

L’exemple suivant représente le corps de réponse pour cette méthode.

```json
{
  "Data": {
    "DeviceType": {
      "710": "Windows",
      "711": "Phone"
    }
  }
}
```


<span id="supplytype-values"/>

### <a name="supply-type-values"></a>Valeurs de type d’approvisionnement

Le champ *supplyType* de l’objet de [TargetingProfile](#targeting-profile)  comporte un ou plusieurs des entiers suivants identifiant le type de stock pour lequel s’affichent les campagnes publicitaires.

|  Valeur entière pour le champ *supplyType*  |  Type d’approvisionnement correspondant  |  Description  |
|---------------------------------|---------------------------|---------------------------|
|     11470     |  Applications        |  Cette valeur fait référence aux publicités apparaissant uniquement dans les applications.  |
|     11471     |  Universel        |  Cette valeur fait référence aux publicités apparaissant dans les applications, sur le web et sur d’autres surfaces d’affichage.  |

Pour obtenir les valeurs prises en charge pour le champ *supplyType* automatiquement, vous pouvez appeler la méthodeGET suivante.  Pour l’en-tête ```Authorization```, communiquez votre jeton d’accès AzureAD, sous la forme **Bearer** &lt;*jeton*&gt;.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/supplytype
Authorization: Bearer <your access token>
```

L’exemple suivant représente le corps de réponse pour cette méthode.

```json
{
  "Data": {
    "SupplyType": {
      "11470": "App",
      "11471": "Universal"
    }
  }
}
```

<span id="country-code-values"/>

### <a name="country-code-values"></a>Valeurs de codes pays

Le champ *country* de l’objet de [TargetingProfile](#targeting-profile) comporte un ou plusieurs des entiers suivants identifiant les codes pays [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) des utilisateurs à cibler.

|  Valeur entière pour le champ *country*  |  Code pays correspondant  |  
|-------------------------------------|------------------------------|
|     1      |            US                  |
|     2      |            AU                  |
|     3      |            AT                  |
|     4      |            BE                  |
|     5      |            BR                  |
|     6      |            CA                  |
|     7      |            DK                  |
|     8      |            FI                  |
|     9      |            FR                  |
|     10      |            DE                  |
|     11      |            GR                  |
|     12      |            HK                  |
|     13      |            IN                  |
|     14      |            IE                  |
|     15      |            IT                  |
|     16      |            JP                  |
|     17      |            LU                  |
|     18      |            MX                  |
|     19      |            NL                  |
|     20      |            NZ                  |
|     21      |            NO                  |
|     22      |            PL                  |
|     23      |            PT                  |
|     24      |            SG                  |
|     25      |            ES                  |
|     26      |            SE                  |
|     27      |            CH                  |
|     28      |            TW                  |
|     29      |            GB                  |
|     30      |            RU                  |
|     31      |            CL                  |
|     32      |            CO                  |
|     33      |            CZ                  |
|     34      |            HU                  |
|     35      |            ZA                  |
|     36      |            KR                  |
|     37      |            CN                  |
|     38      |            RO                  |
|     39      |            TR                  |
|     40      |            SK                  |
|     41      |            IL                  |
|     42      |            ID                  |
|     43      |            AR                  |
|     44      |            MY                  |
|     45      |            PH                  |
|     46      |            PE                  |
|     47      |            UA                  |
|     48      |            AE                  |
|     49      |            TH                  |
|     50      |            IQ                  |
|     51      |            VN                  |
|     52      |            CR                  |
|     53      |            VE                  |
|     54      |            QA                  |
|     55      |            SI                  |
|     56      |            BG                  |
|     57      |            LT                  |
|     58      |            RS                  |
|     59      |            HR                  |
|     60      |            HR                  |
|     61      |            LV                  |
|     62      |            EE                  |
|     63      |            IS                  |
|     64      |            KZ                  |
|     65      |            SA                  |
|     67      |            AL                  |
|     68      |            DZ                  |
|     70      |            AO                  |
|     72      |            AM                  |
|     73      |            AZ                  |
|     74      |            BS                  |
|     75      |            BD                  |
|     76      |            BB                  |
|     77      |            BY                  |
|     81      |            BO                  |
|     82      |            BA                  |
|     83      |            BW                  |
|     87      |            KH                  |
|     88      |            CM                  |
|     94      |            CD                  |
|     95      |            CI                  |
|     96      |            CY                  |
|     99      |            DO                  |
|     100      |            EC                  |
|     101      |            EG                  |
|     102      |            SV                  |
|     107      |            FJ                  |
|     108      |            GA                  |
|     110      |            GE                  |
|     111      |            GH                  |
|     114      |            GT                  |
|     118      |            HT                  |
|     119      |            HN                  |
|     120      |            JM                  |
|     121      |            JO                  |
|     122      |            KE                  |
|     124      |            KW                  |
|     125      |            KG                  |
|     126      |            LA                  |
|     127      |            LB                  |
|     133      |            MK                  |
|     135      |            MW                  |
|     138      |            MT                  |
|     141      |            MU                  |
|     145      |            ME                  |
|     146      |            MA                  |
|     147      |            MZ                  |
|     148      |            NA                  |
|     150      |            NP                  |
|     151      |            NI                  |
|     153      |            NG                  |
|     154      |            OM                  |
|     155      |            PK                  |
|     157      |            AF                  |
|     159      |            PY                  |
|     167      |            SN                  |
|     172      |            LK                  |
|     176      |            TZ                  |
|     180      |            TT                  |
|     181      |            TN                  |
|     184      |            UG                  |
|     185      |            UY                  |
|     186      |            UZ                  |
|     189      |            ZM                  |
|     190      |            ZW                  |
|     219      |            MD                  |
|     224      |            PS                  |
|     225      |            RE                  |
|     246      |            PR                  |

Pour obtenir les valeurs prises en charge pour le champ *country* automatiquement, vous pouvez appeler la méthode GET suivante.  Pour l’en-tête ```Authorization```, communiquez votre jeton d’accès AzureAD, sous la forme **Bearer** &lt;*jeton*&gt;.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/country
Authorization: Bearer <your access token>
```

L’exemple suivant représente le corps de réponse pour cette méthode.

```json
{
  "Data": {
    "Country": {
      "1": "US",
      "2": "AU",
      "3": "AT",
      "4": "BE",
      "5": "BR",
      "6": "CA",
      "7": "DK",
      "8": "FI",
      "9": "FR",
      "10": "DE",
      "11": "GR",
      "12": "HK",
      "13": "IN",
      "14": "IE",
      "15": "IT",
      "16": "JP",
      "17": "LU",
      "18": "MX",
      "19": "NL",
      "20": "NZ",
      "21": "NO",
      "22": "PL",
      "23": "PT",
      "24": "SG",
      "25": "ES",
      "26": "SE",
      "27": "CH",
      "28": "TW",
      "29": "GB",
      "30": "RU",
      "31": "CL",
      "32": "CO",
      "33": "CZ",
      "34": "HU",
      "35": "ZA",
      "36": "KR",
      "37": "CN",
      "38": "RO",
      "39": "TR",
      "40": "SK",
      "41": "IL",
      "42": "ID",
      "43": "AR",
      "44": "MY",
      "45": "PH",
      "46": "PE",
      "47": "UA",
      "48": "AE",
      "49": "TH",
      "50": "IQ",
      "51": "VN",
      "52": "CR",
      "53": "VE",
      "54": "QA",
      "55": "SI",
      "56": "BG",
      "57": "LT",
      "58": "RS",
      "59": "HR",
      "60": "BH",
      "61": "LV",
      "62": "EE",
      "63": "IS",
      "64": "KZ",
      "65": "SA",
      "67": "AL",
      "68": "DZ",
      "70": "AO",
      "72": "AM",
      "73": "AZ",
      "74": "BS",
      "75": "BD",
      "76": "BB",
      "77": "BY",
      "81": "BO",
      "82": "BA",
      "83": "BW",
      "87": "KH",
      "88": "CM",
      "94": "CD",
      "95": "CI",
      "96": "CY",
      "99": "DO",
      "100": "EC",
      "101": "EG",
      "102": "SV",
      "107": "FJ",
      "108": "GA",
      "110": "GE",
      "111": "GH",
      "114": "GT",
      "118": "HT",
      "119": "HN",
      "120": "JM",
      "121": "JO",
      "122": "KE",
      "124": "KW",
      "125": "KG",
      "126": "LA",
      "127": "LB",
      "133": "MK",
      "135": "MW",
      "138": "MT",
      "141": "MU",
      "145": "ME",
      "146": "MA",
      "147": "MZ",
      "148": "NA",
      "150": "NP",
      "151": "NI",
      "153": "NG",
      "154": "OM",
      "155": "PK",
      "157": "PA",
      "159": "PY",
      "167": "SN",
      "172": "LK",
      "176": "TZ",
      "180": "TT",
      "181": "TN",
      "184": "UG",
      "185": "UY",
      "186": "UZ",
      "189": "ZM",
      "190": "ZW",
      "219": "MD",
      "224": "PS",
      "225": "RE",
      "246": "PR"
    }
  }
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Gérer des campagnes publicitaires à l’aide des services du MicrosoftStore](run-ad-campaigns-using-windows-store-services.md)
* [Gérer les campagnes publicitaires](manage-ad-campaigns.md)
* [Gérer les chaînes de distribution des campagnes publicitaires](manage-delivery-lines-for-ad-campaigns.md)
* [Gérer les contenus des campagnes publicitaires](manage-creatives-for-ad-campaigns.md)
* [Obtenir les données relatives aux performances des campagnes publicitaires](get-ad-campaign-performance-data.md)
