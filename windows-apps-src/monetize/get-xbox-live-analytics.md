---
description: Utilisez cette méthode dans l'API d'analyse du Microsoft Store pour obtenir les données d'analyse Xbox Live.
title: Obtenir des données d’analyse Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, services de Microsoft Store, API d'analyse du Microsoft Store, analyse Xbox Live
ms.localizationpriority: medium
ms.openlocfilehash: 74c898630641e8b0d53a181d1874c6df62baaa78
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637084"
---
# <a name="get-xbox-live-analytics-data"></a>Obtenir des données d’analyse Xbox Live

Utilisez cette méthode dans l'API d'analyse du Microsoft Store pour obtenir des données analytiques générales sur les 30 derniers pour les clients jouant à votre [jeu Xbox Live](../xbox-live/index.md), y compris concernant l’utilisation des accessoires de l'appareil, le type de connexion Internet, la distribution des gamerscores, les statistiques de jeu et les données sur les amis et abonnés. Ces informations sont également disponibles dans le [rapport d’analytique de Xbox](../publish/xbox-analytics-report.md) dans Partner Center.

> [!IMPORTANT]
> Cette méthode prend uniquement en charge les jeux pour Xbox et les jeux qui utilisent les services Xbox Live. Ces jeux doivent passer par le [processus d’approbation de concept](../gaming/concept-approval.md), qui inclut les jeux publiés par des [partenaires Microsoft](../xbox-live/developer-program-overview.md#microsoft-partners) et les jeux soumis via le [programme ID@Xbox](../xbox-live/developer-program-overview.md#id). Cette méthode ne prend actuellement pas en charge les jeux publiés via le [Programme Créateurs Xbox Live](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md).

Des données analytique supplémentaires pour les jeux Xbox Live sont disponibles via les méthodes suivantes :
* [Obtenir des données de primes Xbox Live](get-xbox-live-achievements-data.md)
* [Obtenir des données de santé Xbox Live](get-xbox-live-health-data.md)
* [Obtenir des données de Hub de jeu Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtenir des données de club Xbox Live](get-xbox-live-club-data.md)
* [Obtenir des données de multijoueur Xbox Live](get-xbox-live-multiplayer-data.md)
* [Obtenir des données de l’utilisation simultanée Xbox Live](get-xbox-live-concurrent-usage-data.md)

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| applicationId | chaîne | [ID Store](in-app-purchases-and-trials.md#store-ids) du jeu pour lequel vous voulez récupérer des données analytiques générales sur Xbox Live.  |  Oui  |
| metricType | chaîne | Une chaîne qui spécifie le type de données d’analytique Xbox Live à récupérer. Pour cette méthode, spécifiez la valeur **productvalues**.  |  Oui  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre une demande d’obtention de données d’analytique générale pour les clients jouant à votre jeu prenant en charge Xbox Live. Remplacez la valeur *applicationId* par l’ID Store de votre jeu.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=productvalues HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

Cette méthode renvoie un tableau *Value* contenant les objets suivants.

| Objet      | Description                  |
|-------------|---------------------------------------------------|
| ProductData   |   Contient un objet [DeviceProperties](#deviceproperties) et un objet [UserProperties](#userproperties) qui contiennent des données analytiques des appareils et des utilisateurs sur les 30 derniers jours pour votre jeu.    |  
| XboxwideData   |  Contient un objet [DeviceProperties](#deviceproperties) et un objet [UserProperties](#userproperties) qui contiennent des données analytiques sur les moyennes des appareils et des utilisateurs sur les 30 derniers jours pour tous les clients Xbox Live, indiquées sous la forme de pourcentages. Ces données permettent d'établir des comparaisons avec les données de votre jeu.   |                                           


### <a name="deviceproperties"></a>DeviceProperties

Cette ressource contient des données d’utilisation des périphériques pour votre jeu ou des données d'utilisation en moyenne des périphériques pour tous les clients Xbox Live au cours des 30 derniers jours.

| Valeur           | Type    | Description        |
|-----------------|---------|------|
|  applicationId               |    chaîne     |  L'[ID Store](in-app-purchases-and-trials.md#store-ids) du jeu dont vous souhaitez récupérer des données analytiques.   |
|  connectionTypeDistribution               |    tableau     |   Contient des objets qui indiquent le nombre de clients utilisant une connexion internet câblée par rapport à une connexion internet sans fil sur Xbox. Chaque objet comprend deux champs de chaîne : <ul><li>**conType**: Spécifie le type de connexion.</li><li>**deviceCount**: Dans le **ProductData** objet, ce champ spécifie le nombre de clients de votre jeu qui utilisent le type de connexion. Dans l'objet **XboxwideData**, ce champ indique le pourcentage de tous les clients Xbox Live qui utilisent ce type de connexion.</li></ul>   |     
|  deviceCount               |   chaîne      |  Dans l'objet **ProductData**, ce champ indique le nombre d'appareils client sur lesquels votre jeu a été lu au cours des 30 derniers jours. Dans l'objet **XboxwideData**, ce champ est toujours 1, ce qui indique un pourcentage de départ de 100 % concernant les données pour tous les clients Xbox Live.   |     
|  eliteControllerPresentDeviceCount               |   chaîne      |  Dans l'objet **ProductData**, ce champ indique le nombre de clients de votre jeu qui utilisent la manette sans fil Xbox Elite. Dans l'objet **XboxwideData**, ce champ indique le pourcentage de tous les clients Xbox Live qui utilisent la manette sans fil Xbox Elite.  |     
|  externalDrivePresentDeviceCount               |   chaîne      |  Dans l'objet **ProductData**, ce champ indique le nombre de clients de votre jeu qui utilisent un disque dur externe sur Xbox. Dans l'objet **XboxwideData**, ce champ indique le pourcentage de tous les clients Xbox Live qui utilisent un disque dur externe sur Xbox.  |


### <a name="userproperties"></a>UserProperties

Cette ressource contient des données utilisateur pour votre jeu ou des données d'utilisateur en moyenne pour tous les clients Xbox Live au cours des 30 derniers jours.

| Valeur           | Type    | Description        |
|-----------------|---------|------|
|  applicationId               |    chaîne     |   L'[ID Store](in-app-purchases-and-trials.md#store-ids) du jeu dont vous souhaitez récupérer des données analytiques.  |
|  userCount               |    chaîne     |   Dans l'objet **ProductData**, ce champ indique le nombre de clients ayant joué à votre jeu au cours des 30 derniers jours. Dans l'objet **XboxwideData**, ce champ est toujours 1, ce qui indique un pourcentage de départ de 100 % concernant les données pour tous les clients Xbox Live.   |     
|  dvrUsageCounts               |   tableau      |  Contient des objets qui indiquent le nombre de clients ayant utilisé des jeux DVR pour enregistrer et visionner l’expérience de jeu. Chaque objet comprend deux champs de chaîne : <ul><li>**dvrName**: Spécifie le jeux DVR fonctionnalité utilisée. Les valeurs possibles sont **gameClipUploads**, **gameClipViews**, **screenshotUploads** et **screenshotViews**.</li><li>**userCount**: Dans le **ProductData** objet, ce champ spécifie le nombre de clients de votre jeu qui a utilisé la fonctionnalité DVR jeu spécifiée. Dans l'objet **XboxwideData**, ce champ indique le pourcentage de tous les clients Xbox Live ayant utilisé la fonctionnalité de jeu DVR spécifié.</li></ul>   |     
|  followerCountPercentiles               |   tableau      |  Contient des objets qui fournissent des détails sur le nombre d'abonnés pour les clients. Chaque objet comprend deux champs de chaîne : <ul><li>**Pourcentage**: Actuellement, cette valeur est toujours 50, indiquant que les données SUIVEUR sont fournies comme une valeur médiane.</li><li>**Valeur**: Dans le **ProductData** de l’objet, ce champ spécifie le nombre médian d’abonnés pour les clients de votre jeu. Dans l'objet **XboxwideData**, ce champ indique le nombre moyen d'abonnés pour tous les clients Xbox Live.</li></ul>  |   
|  friendCountPercentiles               |   tableau      |  Contient des objets qui fournissent des détails sur le nombre d'amis des clients. Chaque objet comprend deux champs de chaîne : <ul><li>**Pourcentage**: Actuellement, cette valeur est toujours 50, indiquant que les données des amis sont fournies comme une valeur médiane.</li><li>**Valeur**: Dans le **ProductData** de l’objet, ce champ spécifie le nombre médian d’amis pour les clients de votre jeu. Dans l'objet **XboxwideData**, ce champ indique le nombre moyen d'amis pour tous les clients Xbox Live.</li></ul>  |     
|  gamerScoreRangeDistribution               |   tableau      |  Contient des objets qui fournissent des détails sur la distribution du gamerscore pour les clients. Chaque objet comprend deux champs de chaîne : <ul><li>**scoreRange**: La plage de score de joueur pour laquelle le champ suivant fournit des données d’utilisation. Par exemple, **10K-25K**.</li><li>**userCount**: Dans le **ProductData** objet, ce champ spécifie le nombre de clients de votre jeu ayant un score de joueur dans la plage spécifiée pour tous les jeux, ils ont été lus. Dans l'objet **XboxwideData**, ce champ indique le pourcentage de tous les clients Xbox Live dont le gamerscore est compris dans la plage spécifiée pour tous les jeux auxquels ils ont joué.</li></ul>  |
|  titleGamerScoreRangeDistribution               |   tableau      |  Contient des objets qui fournissent des détails sur la distribution du gamerscore pour votre jeu. Chaque objet comprend deux champs de chaîne : <ul><li>**scoreRange**: La plage de score de joueur pour laquelle le champ suivant fournit des données d’utilisation. Par exemple,  **100-200**.</li><li>**userCount**: Dans le **ProductData** objet, ce champ spécifie le nombre de clients de votre jeu ayant un score de joueur dans la plage spécifiée pour votre jeu. Dans l'objet **XboxwideData**, ce champ indique le pourcentage de tous les clients Xbox Live dont le gamerscore est compris dans la plage spécifiée pour votre jeu.</li></ul>   |
|  socialUsageCounts               |   tableau      |  Contient des objets qui fournissent des détails sur les interactions sociales des clients. Chaque objet comprend deux champs de chaîne : <ul><li>**scName**: Le type d’utilisation de réseaux sociaux. Par exemple, **gameInvites** et **textMessages**.</li><li>**userCount**: Dans le **ProductData** objet, ce champ spécifie le nombre de clients de votre jeu qui ont participé dans le type d’utilisation de réseaux sociaux spécifié. Dans l'objet **XboxwideData**, ce champ indique le pourcentage de tous les clients Xbox Live ayant participé au type d'interaction sociale spécifié.</li></ul>   |
|  streamingUsageCounts               |   tableau      |  Contient des objets qui fournissent des détails sur l'utilisation de la diffusion en continu pour les clients. Chaque objet comprend deux champs de chaîne : <ul><li>**stName**: Le type de plateforme de diffusion en continu. Par exemple, **youtubeUsage**, **twitchUsage**, et **mixerUsage**.</li><li>**userCount**: Dans le **ProductData** objet, ce champ spécifie le nombre de clients de votre jeu qui ont utilisé la plateforme de diffusion en continu spécifiée. Dans l'objet **XboxwideData**, ce champ indique le pourcentage de tous les clients Xbox Live ayant utilisé la plateforme de diffusion en continu spécifiée.</li></ul>  |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "ProductData": {
        "DeviceProperties": [
          {
            "applicationId": "9NBLGGGZ5QDR",
            "connectionTypeDistribution": [
              {
                "conType": "WIRED",
                "deviceCount": "43806"
              },
              {
                "conType": "WIRELESS",
                "deviceCount": "104035"
              }
            ],
            "deviceCount": "148063",
            "eliteControllerPresentDeviceCount": "10615",
            "externalDrivePresentDeviceCount": "46388"
          }
        ],
        "UserProperties": [
          {
            "applicationId": "9NBLGGGZ5QDR",
            "userCount": "142345",
            "dvrUsageCounts": [
              {
                "dvrName": "gameClipUploads",
                "userCount": "31264"
              },
              {
                "dvrName": "gameClipViews",
                "userCount": "52236"
              },
              {
                "dvrName": "screenshotUploads",
                "userCount": "27051"
              },
              {
                "dvrName": "screenshotViews",
                "userCount": "45640"
              }
            ],
            "followerCountPercentiles": [
              {
                "percentage": "50",
                "value": "11"
              }
            ],
            "friendCountPercentiles": [
              {
                "percentage": "50",
                "value": "11"
              }
            ],
            "gamerScoreRangeDistribution": [
              {
                "scoreRange": "10K-25K",
                "userCount": "30015"
              },
              {
                "scoreRange": "25K-50K",
                "userCount": "20495"
              },
              {
                "scoreRange": "3K-10K",
                "userCount": "32438"
              },
              {
                "scoreRange": "50K-100K",
                "userCount": "10608"
              },
              {
                "scoreRange": "<3K",
                "userCount": "45726"
              },
              {
                "scoreRange": ">100K",
                "userCount": "3063"
              }
            ],
            "titleGamerScoreRangeDistribution": [
              {
                "scoreRange": "400-600",
                "userCount": "133875"
              },
              {
                "scoreRange": "800-1000",
                "userCount": "45960"
              },
              {
                "scoreRange": "<100",
                "userCount": "269137"
              },
              {
                "scoreRange": "≥1K",
                "userCount": "11634"
              },
              {
                "scoreRange": "100-200",
                "userCount": "334471"
              },
              {
                "scoreRange": "600-800",
                "userCount": "123044"
              },
              {
                "scoreRange": "200-400",
                "userCount": "396725"
              }
            ],
            "socialUsageCounts": [
              {
                "scName": "gameInvites",
                "userCount": "82390"
              },
              {
                "scName": "textMessages",
                "userCount": "91880"
              },
              {
                "scName": "partySessionCount",
                "userCount": "68129"
              }
            ],
            "streamingUsageCounts": [
              {
                "stName": "youtubeUsage",
                "userCount": "74092"
              },
              {
                "stName": "twitchUsage",
                "userCount": "13401"
              }
              {
                "stName": "mixerUsage",
                "userCount": "22907"
              }
            ]
          }
        ]
      },
      "XboxwideData": {
        "DeviceProperties": [
          {
            "applicationId": "XBOXWIDE",
            "connectionTypeDistribution": [
              {
                "conType": "WIRED",
                "deviceCount": "0.213677732584786"
              },
              {
                "conType": "WIRELESS",
                "deviceCount": "0.786322267415214"
              }
            ],
            "deviceCount": "1",
            "eliteControllerPresentDeviceCount": "0.0476609278128012",
            "externalDrivePresentDeviceCount": "0.173747147416134"
          }
        ],
        "UserProperties": [
          {
            "applicationId": "XBOXWIDE",
            "userCount": "1",
            "dvrUsageCounts": [
              {
                "dvrName": "gameClipUploads",
                "userCount": "0.173210623993245"
              },
              {
                "dvrName": "gameClipViews",
                "userCount": "0.202104713778096"
              },
              {
                "dvrName": "screenshotUploads",
                "userCount": "0.136682414274251"
              },
              {
                "dvrName": "screenshotViews",
                "userCount": "0.158057895120314"
              }
            ],
            "followerCountPercentiles": [
              {
                "percentage": "50",
                "value": "5"
              }
            ],
            "friendCountPercentiles": [
              {
                "percentage": "50",
                "value": "5"
              }
            ],
            "gamerScoreRangeDistribution": [
              {
                "scoreRange": "10K-25K",
                "userCount": "0.134709282586519"
              },
              {
                "scoreRange": "25K-50K",
                "userCount": "0.0549468789343825"
              },
              {
                "scoreRange": "50K-100K",
                "userCount": "0.017301313342277"
              },
              {
                "scoreRange": "3K-10K",
                "userCount": "0.216512780268453"
              },
              {
                "scoreRange": "<3K",
                "userCount": "0.573515440094644"
              },
              {
                "scoreRange": ">100K",
                "userCount": "0.00301430477372488"
              }
            ],
            "titleGamerScoreRangeDistribution": [
              {
                "scoreRange": "100-200",
                "userCount": "0.178055695637076"
              },
              {
                "scoreRange": "200-400",
                "userCount": "0.173283639825241"
              },
              {
                "scoreRange": "400-600",
                "userCount": "0.0986865193958259"
              },
              {
                "scoreRange": "600-800",
                "userCount": "0.0506375775462092"
              },
              {
                "scoreRange": "800-1000",
                "userCount": "0.0232398822856435"
              },
              {
                "scoreRange": "<100",
                "userCount": "0.456443551582991"
              },
              {
                "scoreRange": "≥1K",
                "userCount": "0.0196531337270126"
              }
            ],
            "socialUsageCounts": [
              {
                "scName": "gameInvites",
                "userCount": "0.460375855738335"
              },
              {
                "scName": "textMessages",
                "userCount": "0.429256324070832"
              },
              {
                "scName": "partySessionCount",
                "userCount": "0.378446577751268"
              },
              {
                "scName": "gamehubViews",
                "userCount": "0.000197115778147329"
              }
            ],
            "streamingUsageCounts": [
              {
                "stName": "youtubeUsage",
                "userCount": "0.330320919178683"
              },
              {
                "stName": "twitchUsage",
                "userCount": "0.040666241835399"
              }
              {
                "stName": "mixerUsage",
                "userCount": "0.140193816053558"
              }
            ]
          }
        ]
      }
    }
  ],
  "@nextLink": null,
  "TotalCount": 4
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Accéder aux données d’analytique à l’aide des services de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des données de primes Xbox Live](get-xbox-live-achievements-data.md)
* [Obtenir des données de santé Xbox Live](get-xbox-live-health-data.md)
* [Obtenir des données de hub jeux Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtenir des données de club Xbox Live](get-xbox-live-club-data.md)
* [Obtenir des données de multijoueur Xbox Live](get-xbox-live-multiplayer-data.md)
* [Obtenir des données de l’utilisation simultanée Xbox Live](get-xbox-live-concurrent-usage-data.md)
