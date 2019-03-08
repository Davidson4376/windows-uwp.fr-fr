---
title: Modèles de session multijoueur
description: En savoir plus sur les modèles de session multijoueur Xbox Live.
ms.assetid: 178c9863-0fce-4e6a-9147-a928110b53a2
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox one, modèle de session multijoueurs,
ms.localizationpriority: medium
ms.openlocfilehash: 0bbe4f6a3afe2d39fb18b4d4bad13e2aa91d246e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599434"
---
# <a name="multiplayer-session-templates"></a>Modèles de session multijoueur

Cette rubrique donne une vue d’ensemble des modèles de session multijoueur et fournit plusieurs exemples de modèles que vous pouvez copier et modifier pour vos sessions multijoueurs.

Un modèle de session multijoueur est un plan qui est utilisé pour créer une session multijoueur. Toutes les sessions doivent être créées selon un modèle prédéfini. Un modèle définit des constantes qui seront les mêmes pour toute session qui est créée à partir du modèle. Une fois qu’un jeu crée une session à partir d’un modèle, le jeu peut ajouter et modifier des données supplémentaires à la session, mais pas l’une des constantes qui ont été définis dans le modèle.

 Pour plus d’informations, consultez [vue d’ensemble de la Session](../multiplayer-appendix/mpsd-session-details.md).

La liste des modèles de session qui s’appliquent à un identificateur de configuration de service (SCID), ainsi que le contenu des modèles de session spécifique, peut être récupérée à partir de l’annuaire de Session multijoueur (MPSD).


## <a name="about-session-templates"></a>À propos des modèles de session

Un modèle de session utilise le même format comme une requête HTTP PUT pour créer ou modifier une session. La différence est que le modèle est limité à des constantes (sans membres, serveurs ou propriétés). Les constantes peuvent être n’importe quel constantes de session, y compris une section personnalisée et la gamme complète des constantes de système.

### <a name="session-template-versions"></a>Versions de modèles de session

Les modèles de session définies dans cette rubrique sont construits à l’aide de la version du modèle de contrat 107. Lorsque vous les utilisez pour créer un nouveau modèle, assurez-vous que 107 est entré en tant que la version de contrat.

Si vous utilisez XSAPI et regardez les demandes qui en résulte dans le débogueur, vous pouvez remarquer que les requêtes utilisent la version du modèle de contrat 105. MPSD efficacement » met à niveau « ces demandes vers la version 107 en cours d’exécution.

> **Remarque :** Il est possible d’utiliser une version de contrat différent dans la demande à partir de ce qui est utilisé dans le modèle de session.

Si nécessaire, vous pouvez modifier un modèle de session à partir de la version 104/105 vers la version 107. Pour obtenir des instructions, suivez les instructions de migration dans [problèmes lorsque adapter votre des titres communs pour le mode multijoueur 2015](../multiplayer-appendix/common-issues-when-adapting-multiplayer.md).


## <a name="session-template-default-values"></a>Valeurs par défaut du modèle session

Chaque session créée à partir d’un modèle de session démarre en tant que copie du modèle. Les valeurs que le modèle n’inclut pas peuvent être fournies lors de la création de session. Valeurs par défaut sont fournies dans certains cas, lorsque aucune autre valeur n’est définie. Par exemple, l’ensemble de délais d’attente pour la version de contrat 107 par défaut est :

```json
    {
      "constants": {
        "system": {
          "reservedRemovalTimeout": 30000,
          "inactiveRemovalTimeout": 0,
          "readyRemovalTimeout": 180000,
          "sessionEmptyTimeout": 0
        }
      }
    }
```
Vous pouvez forcer une valeur de rester non définies en spécifiant la valeur null. Cela remplace tous les paramètres par défaut et empêche la valeur lors de la création de session. Par exemple, pour supprimer la session vide délai d’attente, ce qui permet de sessions continuer indéfiniment, même sans tous les membres, ajoutez le code suivant pour le modèle de session :
```json
    {
      "constants": {
        "system": {
          "sessionEmptyTimeout": null
        }
      }
    }
```
> **Important :** Constantes qui sont définies via un modèle ne peut pas être modifiées via écritures à MPSD. Pour modifier les valeurs, vous devez créer et envoyer un nouveau modèle avec les modifications requises.


## <a name="example-session-templates"></a>Exemples de modèles de session
Cette section montre plusieurs exemples de modèles de session pour des objectifs différents et topologies de réseau. Veuillez examiner chaque modèle avant de choisir celui adapté à votre client. Vous pouvez ensuite copier et coller le modèle dans votre configuration de service, potentiellement après les modifications requises.

### <a name="standard-lobby-session"></a>Session de salle d’attente standard
Vous pouvez utiliser le modèle suivant comme modèle starter pour créer une session d’introduction pour votre jeu :

* Modifier la `maxMembersCount` valeur pour le nombre maximal de lecteurs que vous souhaitez prendre en charge dans votre session d’introduction.  
* Si votre titre ne prend pas en charge les joueurs sur différentes plateformes (par exemple, une Xbox One et un PC) lecture ensemble, vous pouvez supprimer le `crossPlay` élément.  
* Vous pouvez modifier les autres valeurs, mais les valeurs suivantes sont les valeurs appropriées pour commencer si vous ne savez pas ce dont vous avez besoin.


```json
{
   "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 8,
            "visibility": "open",
            "capabilities": {
                "connectivity": true,
                "connectionRequiredForActiveMembers": true,
                "crossPlay": true,
                "userAuthorizationStyle": true
            },
        },
        "custom": {}
    }
}
```

### <a name="standard-game-session-without-matchmaking"></a>Session de jeu standard sans matchmaking
Vous pouvez utiliser le modèle suivant comme modèle starter pour créer une session de jeu à votre jeu, si votre jeu n’inclut pas de matchmaking anonyme et ne nécessite pas de plus de 100 membres.

Notez que seulement les nouvelles valeurs spécifiées à partir du modèle de session d’introduction standard sont les suivantes :
* `constants.system.inviteProtocol : "game"`
* `constants.system.capabilities.gameplay : true`

```json
{
   "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 8,
            "visibility": "open",
            "inviteProtocol": "game",
            "capabilities": {
                "connectivity": true,
                "connectionRequiredForActiveMembers": true,
                "gameplay" : true,
                "crossPlay": true,
                "userAuthorizationStyle": true
            }
        },
        "custom": {}
    }
}
```

### <a name="add-matchmaking-to-a-game-session-template-while-letting-the-multiplayer-service-handle-quality-of-service-checks"></a>Ajouter matchmaking à un modèle de session de jeu, tout en laissant le service multijoueur gérer la qualité des contrôles de service.

Vous pouvez ajouter les éléments suivants `memberInitialization` élément json à votre modèle de jeu afin d’ajouter la prise en charge de matchmaking.

Lorsque vous créez votre hopper SmartMatch, utilisez ce modèle en tant que le modèle de session cible pour votre hopper.

```json
{
   "constants": {
        "system": {
            "memberInitialization": {
               "joinTimeout": 20000,
               "measurementTimeout": 15000,
               "membersNeededToStart": 2
            }
        }
    }
}
```

### <a name="add-matchmaking-to-a-game-session-template-where-quality-of-service-checks-are-handled-by-a-title-managed-data-center"></a>Ajouter matchmaking à un modèle de session de jeu, où la qualité de service vérifie si sont gérées par un centre de données managées de titre.



```json
{
   "constants": {
        "system": {
            "peerToHostRequirements": {  
                "latencyMaximum": 250,
                "bandwidthDownMinimum": 256,
                "bandwidthUpMinimum": 256,
                "hostSelectionMetric": "latency"
            },
            "memberInitialization": {
               "joinTimeout": 15000,
               "measurementTimeout": 15000,
               "membersNeededToStart": 2
            }
        },
        "custom": {}
    }
}
```

### <a name="basic-session-template-for-client-server-game-session"></a>Modèle de session de base pour la session de jeu de client-serveur

Vous pouvez utiliser ce modèle pour un logiciel qui n’effectue pas de communication d’égal à égal et n’utilise pas de Xbox Live Compute, mais au lieu de cela a les clients à se connecter à un serveur hébergé par des tiers.
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 12,
          "visibility": "open",
          "inviteProtocol": "game",
          "capabilities": {
            "connectionRequiredForActiveMembers": true,
            "gameplay" : true,
          },
        },
        "custom": {}
      }
    }
```

### <a name="lobbysmartmatch-ticket-session-template-for-peer-based-networking"></a>Modèle de session d’introduction/SmartMatch ticket pour la mise en réseau homologue

Utilisez ce modèle pour la création d’une session d’introduction ou une session de ticket SmartMatch doit uniquement être utilisé pour envoyer un groupe de lecteurs dans matchmaking. Le modèle est ne pas à être utilisé pour configurer une session de jeu. Il est destiné à une utilisation par les clients à l’aide d’une topologie de réseau peer-to-peer ou hôte de l’homologue.
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 10,
          "visibility": "open",
          "capabilities": {
            "connectionRequiredForActiveMembers": true,
          },
          "memberInitialization": {
            "membersNeededToStart": 1
          },
        },
        "custom": {}
      }
    }
```

### <a name="quality-of-service-qos-templates"></a>Qualité des modèles de Service (QoS)

Si votre client est à l’aide de matchmaking et l’évaluation de qualité de service, vous devez ajouter certaines constantes pour le modèle de session pour informer MPSD pour coordonner avec le client pour gérer les utilisateurs de rejoindre la session. Cette coordination valide la qualité de l’état de connexion avant d’informer les utilisateurs le jeu est prêt à commencer. Dans le cas des jeux client-serveur, la coordination valide la qualité de la connexion avant un groupe de joueurs entre matchmaking.

#### <a name="peer-to-host-game-session-template-with-qos"></a>Modèle de session de jeu à hôte homologue avec QoS

L’exemple suivant présente un modèle de session de jeu à hôte homologue avec QoS.
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 12,
          "visibility": "open",
          "inviteProtocol": "game",
          "capabilities": {
            "connectivity": true,
            "connectionRequiredForActiveMembers": true,
            "gameplay" : true
          },
          "memberInitialization": {
            "membersNeededToStart": 2
          },
          "peerToHostRequirements": {
            "latencyMaximum": 350,
            "bandwidthDownMinimum": 1000,
            "bandwidthUpMinimum": 1000,
            "hostSelectionMetric": "latency"
          }
        },
        "custom": { }
      }
    }
```

#### <a name="peer-to-peer-game-session-template-with-qos"></a>Modèle de session de jeu de peer-to-peer avec QoS

Voici un exemple d’un modèle de session de jeu de peer-to-peer avec QoS.
```json
    {
    "constants": {
      "system": {
        "version": 1,
        "maxMembersCount": 12,
        "visibility": "open",
        "inviteProtocol": "game",
        "capabilities": {
          "connectivity": true,
          "connectionRequiredForActiveMembers": true,
          "gameplay" : true
        },
        "memberInitialization": {
          "membersNeededToStart": 2
        },
        "peerToPeerRequirements": {
          "latencyMaximum": 250,
          "bandwidthMinimum": 10000
        }
      },
      "custom": { }
     }
    }
```

#### <a name="client-server-xbox-live-compute-lobbymatchmaking-session-template-with-qos"></a>Modèle de session d’introduction/matchmaking client-serveur (Xbox Live Compute) avec QoS

Utilisez ce modèle pour créer une session d’introduction ou un équivalent à l’aide de la qualité de service. Ce modèle n’est pas destiné à être utilisé pour configurer une session de jeu.
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 12,
          "visibility": "open",
          "memberInitialization": {
            "membersNeededToStart": 1
          }
        },
        "custom": {}
      }
    }
```

#### <a name="session-template-for-crossplay-between-xbox-one-and-windows-10"></a>Modèle de session pour crossplay entre Xbox One et Windows 10

Utilisez ce modèle pour activer crossplay multijoueur entre Xbox One et Windows 10. La fonctionnalité d’userAuthorizationStyle permet d’accéder à Windows 10. La fonctionnalité facultative crossPlay signifie que votre titre prend en charge les interactions telles que les invitations et jointure-en cours d’exécution entre les plateformes.
```json
    {
      "constants": {
        "system": {
          "capabilities": {
            "crossPlay": true,
            "userAuthorizationStyle": true
          },
        },
        "custom": {}
      }
    }
```
