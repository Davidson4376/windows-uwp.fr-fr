---
title: Achievement (JSON)
assetID: d3b52f66-ddc7-e676-b419-82209caf71d6
permalink: en-us/docs/xboxlive/rest/json-achievementv2.html
description: " Achievement (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b0e20f46a0d97cba496df5c6fb9cda14fbeccccd
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628474"
---
# <a name="achievement-json"></a>Achievement (JSON)
Un objet de la prime (version 2).
<a id="ID4EN"></a>


## <a name="achievement"></a>Réalisation

L’objet de réalisation a la spécification suivante. Tous les membres sont requis.

| Membre| Type| Description|
| --- | --- | --- |
| id| chaîne| Identificateur de ressource.|
| serviceConfigId| chaîne| SCID pour cette ressource. Identifie les intitulés de voir à que ce succès est lié. |
| name| chaîne| Le nom localisé de la prime.|
| titleAssociations| tableau de [TitleAssociation](json-titleassociation.md)| Tableau de TitleAssociation.|
| progressState| **ProgressState** énumération| L’état de progression : <ul><li>non valide (0) : La progression de la prime est dans un état inconnu.</li><li>obtenu (1) : La prime a été déverrouillée.</li><li>en cours (2) : La réalisation est verrouillée, mais n’a été la progression de déverrouillage.</li><li>notStarted (3) : La réalisation est verrouillée et l’utilisateur n’a pas effectué toute progression de déverrouillage.</li></ul> | 
| progression| [Progression](json-progression.md)| Progression de l’utilisateur au sein de la prime.|
| mediaAssets| tableau de [MediaAsset](json-mediaasset.md)| Les éléments multimédias associés à la réalisation, telles que des ID de l’image. |
| Plateforme| chaîne| La plateforme la prime a été obtenue.|
| isSecret| Valeur booléenne| Indique si la prime est secret.|
| description| chaîne| Description de la prime lorsque déverrouillées.|
| lockedDescription| chaîne| Description de la prime avant qu’il soit déverrouillé.|
| productId| chaîne| La réalisation de l’ID de produit a été publiée avec.|
| achievementType| **AchievementType** énumération| Le type de réussite (pas l’identique au précédent type sur les primes hérités) : <ul><li>non valide (0) : Un type de prime inconnu et non pris en charge.</li><li>persistant (1) : Une prime aucune date de fin et peut être déverrouillés à tout moment.</li><li>défi (2) : Une prime qui a un certain laps de temps pendant lequel il peut être déverrouillé.</li></ul> |
| participationType| **ParticipationType** énumération| Le type de la participation à la réalisation. Les valeurs valides sont individu ou groupe.|
| timeWindow| TimeWindow| La fenêtre de temps pendant lequel la prime peut être déverrouillée. Seules les défis.|
| récompenses| tableau de [Reward](json-reward.md)| Collection de récompenses gagnés lorsque déverrouillé.|
| estimatedTime| TimeSpan| La durée estimée de la prime prendra de gagner.|
| lien profond| chaîne| Un lien profond dans le titre.|
| isRevoked| Valeur booléenne| La réalisation est ou non révoquée par l’application.|

<a id="ID4EIAAC"></a>


## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON


```json
{
        "id":"3",
        "serviceConfigId":"b5dd9daf-0000-0000-0000-000000000000",
        "name":"Default NameString for Microsoft Achievements Sample Achievement 3",
        "titleAssociations":
        [{
                "name":"Microsoft Achievements Sample",
                "id":3051199919,
                "version":"abc"
        }],
        "progressState":"Achieved",
        "progression":
        {
          "requirements":
          [{
            "id":"12345678-1234-1234-1234-123456789012",
            "current":null,
            "target":"100"
          }],
          "timeUnlocked":"2013-01-17T03:19:00.3087016Z",
        },
        "mediaAssets":
        [{
                "name":"Icon Name",
                "type":"Icon",
                "url":"https://www.xbox.com"
        }],
        "platform":"D",
        "isSecret":true,
        "description":"Default DescriptionString for Microsoft Achievements Sample Achievement 3",
        "lockedDescription":"Default UnachievedString for Microsoft Achievements Sample Achievement 3",
        "productId":"12345678-1234-1234-1234-123456789012",
        "achievementType":"Challenge",
        "participationType":"Individual",
        "timeWindow":
        {
                "startDate":"2013-02-01T00:00:00Z",
                "endDate":"2100-07-01T00:00:00Z"
        },
        "rewards":
        [{
                "name":null,
                "description":null,
                "value":"10",
                "type":"Gamerscore",
                "valueType":"Int"
        },
        {
                "name":"Default Name for InAppReward for Microsoft Achievements Sample Achievement 3",
                "description":"Default Description for InAppReward for Microsoft Achievements Sample Achievement 3",
                "value":"1",
                "type":"InApp",
                "valueType":"String"
        }],
        "estimatedTime":"06:12:14",
        "deeplink":"aWFtYWRlZXBsaW5r",
        "isRevoked":false
    }

```


<a id="ID4ERAAC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4ETAAC"></a>


##### <a name="parent"></a>Parent

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)
