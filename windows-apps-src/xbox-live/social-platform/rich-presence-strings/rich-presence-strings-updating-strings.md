---
title: Présence riche la mise à jour des chaînes
description: Découvrez comment mettre à jour d’une chaîne de la présence de riches Xbox Live.
ms.assetid: eb2bb82e-8730-4d74-9b33-95d133360e44
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox, présence riche
ms.localizationpriority: medium
ms.openlocfilehash: ac4549301c60eafb935dab0ac9c5b5028452edfb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57610394"
---
# <a name="rich-presence-updating-strings"></a>Présence riche la mise à jour des chaînes

Pour mettre à jour la chaîne de présence enrichie dans votre titre, vous pouvez appeler l’URI de titre écrire avec les paramètres appropriés dans un objet JSON. Cet appel restful est également encapsulé par les API de Service Xbox. Consultez **Microsoft.Xbox.Services.Presence Namespace** pour plus d’informations sur l’API connexe.

L’URI ressemble à ceci :

          POST /users/xuid({xuid})/devices/current/titles/current

Vous trouverez ci-dessous des seuls les champs pour la définition des chaînes de présence enrichie. Il existe d’autres champs facultatifs liés à la présence d’écriture pour un logiciel non répertorié ici.

## <a name="titlerequest-object"></a>Objet de TitleRequest

Propriété | Type | Req'd | Description
---|---|---|---
Activité|ActivityRequest|N|Enregistrement qui décrit les informations de titre (présence enrichie et support info, si disponible)

## <a name="activityrequest-object"></a>Objet de ActivityRequest

Propriété | Type | Req'd | Description
---|---|---|---
richPresence|RichPresenceRequest|N|Le nom convivial de la chaîne de présence enrichie qui doit être utilisé.

## <a name="richpresencerequest-object"></a>Objet de RichPresenceRequest

Propriété | Type | Req'd | Description
---|---|---|---
Id|Chaîne|Y|Le nom convivial de la chaîne de présence enrichie qui doit être utilisé
scid|Chaîne|Y|Scid qui nous indique où les chaînes de présence enrichie sont définies.

Par exemple, si je veux mettre à jour de la présence riche pour l’utilisateur dont xuid est 12345, mon appel se présente comme suit :

          POST /users/xuid(12345)/devices/current/titles/current


Avec le corps JSON suivant :

```json
          {
            activity:
            {
              richPresence:
              {
                id:"playingMap",
                scid:"0000-0000-0000-0000-01010101"
              }
            }
          }
```

À l’aide de l’API de wrapper, ce serait un appel à **PresenceService.SetPresenceAsync (méthode)**

Si vous conservez la plateforme de données à jour, puis que vous n’êtes pas obligé de réinitialiser la chaîne de présence riche chaque fois que les données pour remplir les modifications vides. Dans l’exemple ci-dessus, nous savons que vous souhaitez utiliser l’objet map actuel. Aspect de la présence des données dans la plate-forme de données lorsqu’un utilisateur tente de lire la chaîne à remplir la valeur actuelle. Par conséquent, même si le lecteur est de commutation à partir de la carte pour carte permettant de mapper, vous n’êtes pas obligé réinitialiser la chaîne de présence enrichie dans votre jeu, que vous envoyez les événements appropriés pour la plateforme de données. N’oubliez pas qu’il peut prendre quelques secondes pour les données de son chemin via la plateforme de données.

Ensuite, quand un utilisateur tente de lire la présence de riches utilisateur de 12345, le service examiner le paramètres régionaux sont demandé et mise en forme la chaîne appropriée avant de retourner.

Dans ce cas, supposons qu’un utilisateur souhaite lire la chaîne en-US. Lecture de présence riche fonctionnerait comme suit (pour plus d’informations sur cet appel, consultez **GET (/users/xuid({xuid}))**

          GET /users/xuid(12345)?level=all

Le wrapper de pour cette API est **PresenceService.GetPresenceAsync (méthode)**

Que se passe-t-il ici est que vous demandez le PresenceRecord de l’utilisateur, dont xuid est 12345. Et vous demandez que le niveau de détail soit « tous ». Si « all » n’a pas été spécifié, présence enrichie ne serait pas retournée.

Et il retourne les éléments suivants dans la réponse JSON :

```json
          {
            xuid:"12345",
            state:"online",
            devices:
            [
              {
                type:"D",
                titles:
                [
                  {
                    id:"12345",
                    name:"Buckets are Awesome",
                    lastModified:"2012-09-17T07:15:23.4930000",
                    placement: "full",
                    state:"active",
                    activity:
                    {
                      richPresence:"Playing on map:Mountains"
                    }
                  }
                ]
              }
            ]
          }
```
