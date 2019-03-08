---
title: Sessions de grande taille
description: En savoir plus sur l’utilisation de sessions volumineuses avec plateforme multijoueur de Xbox Live.
ms.date: 07/11/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, session multijoueur, de grande taille, lecteurs récents
ms.localizationpriority: medium
ms.openlocfilehash: dcd7b27bc0aea8b8406e7eccb420140cf32c1ed5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618544"
---
# <a name="large-sessions"></a>Sessions de grande taille

Si vous avez besoin d’une session multijoueur capable de gérer plus de 100 membres, vous devez utiliser ce que l'on appelle une session de grande taille. Ce scénario est plus courant pour les jeux (MMO) en ligne massivement multijoueurs et diffusions (où la plupart des membres est spectateurs), mais peut avoir des applications pour les autres styles de jeux ainsi.

Dans certains cas, vous pouvez également utiliser des sessions volumineuses même lorsque vous traitez des groupes plus petits de joueurs. Si vous souhaitez que plusieurs acteurs se trouver dans la même session, mais pas nécessairement connaître mutuellement s’ils ne rencontrent uns des autres dans le jeu, vous pouvez utiliser la propriété « rencontre » des sessions de grande taille.

Sessions volumineuses ne sont pas pris en charge actuellement par [Xbox intégré multijoueur (XIM)](../xbox-integrated-multiplayer.md) ou par [Manager multijoueurs (MPM)](../multiplayer-manager.md), vous devez donc utiliser les API de 2015 multijoueurs à utiliser des appels directs à la mode multijoueur Répertoire de service (MPSD).

Sessions de grande taille sont traitées légèrement différemment des sessions ordinaires :

* Contient moins d’informations que les sessions ordinaires, mais sont plus efficaces.
* Prend en charge jusqu'à 10 000 membres.
* Vous ne pouvez pas vous abonner à une session de grandes.
* Il n’existe aucune inclusion automatique dans les listes de lecteur récentes pour les membres d’une session de grande taille.

## <a name="recent-players"></a>Derniers joueurs

L’une des fonctionnalités de Xbox Live est que lors de la Xbox Live joueurs jeux multijoueurs avec de nouvelles personnes, après le jeu ils peuvent voir ces lecteurs sur leur tableau de bord dans le **derniers joueurs** liste. Si un lecteur avait une excellente expérience avec un nouveau joueur dans un jeu, ils peuvent souhaitent rejouer avec eux, ou ajoutez-les en tant qu’ami. Si elles avaient une mauvaise expérience avec un lecteur, ils souhaiteront peut-être les éviter à l’avenir, et/ou le signaler le mauvais comportement après que le jeu est terminé.

Avec les sessions régulières, Xbox Live ajoute automatiquement des lecteurs dans la même session à la liste de joueurs récents. Si vous utilisez des sessions volumineuses Toutefois, vous devez prendre des mesures supplémentaires pour vous assurer que les récentes player listes sont correctement remplies.

## <a name="set-up-a-large-session"></a>Configurer une session de grande taille

Pour configurer les sessions aussi volumineux, ajoutez `“large”: true` à la section fonctionnalités dans le modèle de session. Vous permet de définir le `maxMembersCount` jusqu'à 10 000. Un modèle de session comme le ci-dessous devrait fonctionner :

```json
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 2000,
            "visibility": "open",
            "capabilities": {
                "gameplay": true,
                "large": true
            },
            "timeouts": {
                "inactive": 0,
                "ready": 180000,
                "sessionEmpty": 0
            }
        },
        "custom": { }
    }
}
```

## <a name="working-with-large-sessions"></a>Utilisation de sessions de grande taille

Lorsque vous écrivez des sessions de grande taille à MPSD, nous vous recommandons d’effectuer afin de ne pas pour dépasser 10 écritures par seconde. Il s’agit généralement d’une session de lecteur de 1000 avec une écriture de toutes les 2 minutes en moyenne par le lecteur (par exemple, rejoindre/quitter).

Autres propriétés ne doivent pas être conservées dans les sessions de grande taille.

### <a name="associating-players-from-the-same-large-session"></a>Association des lecteurs à partir de la même session de grande taille

Lorsque vous récupérez une session volumineuse à partir de MPSD, la liste des membres ne redevient pas avec la réponse, et en fait, il n’a aucun moyen pour obtenir la liste complète. Au lieu de cela, si l’appelant est dans la session, leur enregistrement de membre sera le seul dans la collection « membres », étiquetés comme « me » (comme dans la demande).

Cela signifie que les membres de clients pourront uniquement mettre à jour leur propre entrée dans la session et seront appuie sur le serveur pour leur fournir un identificateur courant Xbox Live permet d’associer des lecteurs en lecture simultanée.

Il existe deux façons d’indiquer que les utilisateurs dans une session de lecture simultanée (pour la mise à jour sa réputation et état de joueurs récent).

#### <a name="1-persistent-groups"></a>1. Groupes persistants

Si un groupe de personnes séjourne ensemble en permanence, potentiellement avec des personnes entrants et sortants à partir de celui-ci, vous pouvez donner au groupe un nom (par exemple, un guid – suivent les mêmes règles d’affectation de noms que pour les sessions régulières).  Comme chaque membre va et vient à partir du groupe, ils doivent ajouter ou supprimer le nom du groupe de leur propre « groupes », qui est un tableau de chaînes :

```json
{
    "members": {
        "me": {
            "properties": {
                "system": {
                    "groups": [ "boffins-posse" ]
                }
            }
        }
    }
}
```

#### <a name="2-brief-encounters"></a>2. Brève rencontre

Si deux personnes ont une brève rencontre à usage unique, le jeu peut utiliser à la place du tableau « rencontre ». Donnez chaque confrontés à un nom, et après la rencontre, les deux (ou tous) participants écririez le nom à leur propre propriété « rencontre » :

```json
{
    "members": {
        "me": {
            "properties": {
                "system": {
                    "encounters": [ "trade.0c7bbbbf-1e49-40a1-a354-0a9a9e23d26a" ]
                }
            }
        }
    }
}
```

Vous pouvez utiliser le même nom pour les « groupes » et « rencontre », par exemple, si un joueur « entretient des relations avec « un groupe, les personnes dans le groupe ne sont pas besoin de faire quoi que ce soit (en supposant qu’ils précédemment ajouté le nom du groupe à leurs « groupes »), et la personne qui avait le rencontre serait u transférer le nom du groupe dans leur liste « rencontre ». Qui entraîne la personne voir tous les membres du groupe en tant que lecteurs récents et vice versa.

Nombre de rencontre comme ayant été membre du groupe pendant 30 secondes. Dans la mesure où la rencontre est considérés comme des événements uniques, le tableau « rencontre » est toujours immédiatement traité et ensuite effacé de la session.  Il n’apparaîtra jamais dans une réponse.  (Le tableau « groupes » reste collé jusqu'à ce que modifié ou supprimé, ou le membre laisse la session).
