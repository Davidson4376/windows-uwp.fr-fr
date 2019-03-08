---
title: Multiplayer Session Directory Xbox One
description: En savoir plus sur la création de sessions en mode multijoueur en utilisant le service Xbox Live Mutliplayer Session Directory (MPSD).
ms.assetid: 70da1be3-5f39-4eed-b62d-9cdd47e413d2
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 52b363492d1cd17ae54ae5d23d04371c5adf73ed
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606814"
---
# <a name="xbox-one-multiplayer-session-directory"></a>Multiplayer Session Directory Xbox One

Cette rubrique fournit une vue d’ensemble de la création d’une session multijoueur à l’aide du nouveau service Xbox un multijoueur Session Directory (MPSD). Le livre s’adressent principalement les développeurs de titre Xbox One qui soumettent leurs modèles de session directement à Xbox développement Portal (XDP). Le service MPSD peut aussi être configuré avec des partenaires, mais n’est pas consacré dans cet article. Il vise à se familiariser avec les termes et concepts de MPSD configuration, l’utilisation et le dépannage de sessions multijoueurs.

## <a name="revision-summary"></a>Résumé de la révision

Les bibliothèques XSAPI (API de Services Xbox) côté client utilisent actuellement la version de contrat 104 lors de la communication aux services MPSD. Cette version du document met à jour le schéma de modèle de session de contrat version 107, qui est la dernière version de contrat pris en charge par les services MPSD. Les modifications entre la version 104 et 107 sont résumées dans la section intitulée [mise à jour du schéma de contrat](#_Contract_schema_update).

Notez que les modèles qui ont été écrites pour la version de contrat 104 doit être mis à jour si elles sont republiés en tant que 107. Toutefois, les services présentent une compatibilité descendante pour pouvoir continuer à utiliser les bibliothèques actuellement, ce qui mettra à jour dans une prochaine version XDK.

La révision précédente de ce document mis à jour des informations sur les sessions de serveur et ajouté une nouvelle section sur les API de Service Xbox Live et les appels de service RESTful. En outre, le tableau dans la requête pour la section des informations d’état de Session a été mis à jour et la section modèles de qualité de Service (QoS) a été modifiée.

## <a name="introduction"></a>Introduction

Sur Xbox One, une session multijoueur est un document sécurisé qui se trouve dans le cloud sur le répertoire de Session multijoueur (MPSD), et ce document représente un groupe de personnes de jeu. Plus précisément, les sessions multijoueurs ont les caractéristiques suivantes :

-   Chaque session possède un URI unique.

-   Le document de session contient les membres actuels, les paramètres de jeu, l’amorçage des données et les informations de serveur de jeu.

-   Titres de créer et gérer leurs propres sessions.

-   Une session permet une connectivité entre ses membres.

L’annuaire de sessions multijoueur centralise les métadonnées de la session de jeu sur tous les clients. Il fournit les informations de base nécessaires sur une session pour aider à configurer les associations appareil sécurisé entre les clients. Il fournit également des métadonnées de premier démarrage du système de base pour un client se connecte à la partie avant de passer des données de jeu plus spécifiques. Avec la gestion de durée de vie de processus (PLM) et la nature de basculement de tâche des applications sur Xbox One, le MPSD permet de s’assurer que les clients ont les informations correctes pour contacter des homologues et les serveurs qui sont identifiés comme faisant partie de la session active de jeu et les coordonnées avec le shell et la console de système d’exploitation réserve, activer et gérer la durée de vie de lecteur pour une session de jeu.

## <a name="terminology-used-in-this-document"></a>Terminologie utilisée dans ce document

| Terme                 | Définition                                                                                                                                                                                                                                                                                  |
|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Session multijoueur  | Un document sécurisé qui se trouve dans le cloud Xbox Live et représente un groupe d’utilisateurs qui sont (ou sera) connectées ensemble pendant la lecture d’un titre sur Xbox One. Tous les aspects de mode multijoueur, tels que matchmaking, parties, jointure-en cours d’exécution et ainsi de suite : tirer parti de la session multijoueur. |
| Session de jeu         | Il s’agit de la session de jeu réelle, exposée dans le MPSD, dans lequel les utilisateurs jouent ensemble. Tous les scénarios multijoueurs finissent dans une session de jeu.                                                                                                                               |
| Correspond à la session de ticket | Il s’agit d’une session utilisée pour effectuer le suivi de la soumission de ticket de correspondance au cours de matchmaking.                                                                                                                                                                                                                 |
| Acteur inactif      | Un lecteur qui a été défini sur l’état inactif au sein de la session. Le titre définit un utilisateur sur le Inactive état lorsque le jeu est limité, suspendu, ou sinon inactif tel que défini par le titre.                                                                                     |

## <a name="the-multiplayer-session-directory"></a>L’annuaire de sessions multijoueur

Le MPSD permet et facilite les titres à coordonner les informations de session entre joueurs en ligne. Il peut y avoir différents types de sessions créées pour réaliser différentes tâches d’un jeu multijoueur. Le tableau suivant présente les différences dans la façon dont ces tâches ont été réalisés sur Xbox 360 par rapport à la façon dont elles sont réalisées sur Xbox One.

| Fonction ou une tâche                     | Xbox 360                                                                                                        | Xbox One                                                                                                   |
|--------------------------------------|-----------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| **Obtenir des informations de session de jeu**     | **XSessionGetDetails**, **XSessionSearchByID**, ou suivre vous-même.                                              | Demander les informations de session à partir du service.                                                          |
| **Migrer l’ordinateur hôte**                     | Si nécessaire, le titre appelle **XSessionMigrateHost.**                                                           | Sans devoir créer une nouvelle session suffit d’attribuer un nouvel hôte pour SessionID.                                  |
| **Gérer plusieurs sessions de lecteur**  | Il est difficile de gérer plusieurs sessions à la fois. Par exemple, **XNetReplaceKey** et **XNetUnregisterKey**. | Session basée sur le service facilite la gestion d’un session nettoyeur et le rend faciles à gérer plusieurs sessions.    |
| **Gérer les déploiements de connexion et se déconnecte** | Être amené à gérer se déconnecte et de déconnexion différemment, avec **XCloseHandle** ou **XSessionDelete**, respectivement. | Service centralisé simplifie les déploiements de connexion et déconnexion de gestion des, et des délais d’expiration sont définies dans la configuration de jeu. |

### <a name="session-patterns"></a>Modèles de session

-   Sessions de jeu

    -   Session avec les joueurs XUIDs sécurisée des appareils adresse et les données des États de propriété. Cela est considéré comme la session de jeu réel.

    -   Peut être peer-to-peer, hôte de l’homologue, homologue-serveur ou hybride.

-   Sessions de ticket de correspondance

    -   Une session qui est soumise à matchmaking pour rechercher d’autres joueurs à jouer avec. Notez qu’une session de jeu peut également une session de ticket, si le jeu est recherchez plus de joueurs.

-   Sessions de serveur

    -   Sessions de jeu créé et utilisé via le traitement de Xbox Live Compute.

La figure 1 illustre les utilisations de sessions MPSD, où les cases bleues représentent les sessions MPSD et les zones rouges sont les services qui les utilisent.

Figure 1. Utilisation de session MPSD.

## <a name="mpsd-sessions"></a>Sessions MPSD

Sessions maintenir deux listes d’entités associées :

1.  Membres (utilisateurs) qui ont rejoint ou été invités dans une session.

2.  Serveurs (serveurs cloud ou des serveurs dédiés) qui ont rejoint la session.

Chaque entité a :

1.  Valeurs constantes définies au moment de la création.

2.  Propriétés mutables.

3.  Métadonnées en lecture seule.

### <a name="xbox-live-service-apis-and-restful-service-calls"></a>API de Service Xbox Live et les appels de service RESTful

Il existe deux façons pour interagir avec les Sessions une Xbox et les services de Matchmaking. La première consiste à utiliser des appels HTTPS standard pour les URI de Services Live Xbox RESTful. Cela permet une grande souplesse titres appelant et interagissant avec ces services en fonction de leur serveur et les configurations de jeu. Vous trouverez une liste de ces URI dans le [documentation de Xbox un développement Kit (XDK)](https://developer.xboxlive.com/en-us/platform/development/documentation/software/Pages/home.aspx) sous « Xbox Live référence RESTful de Services. » [1]

La deuxième méthode consiste à utiliser la Xbox Live API de Service, qui agissent comme des wrappers pour l’URI de service RESTful. Celles-ci permettent d’adopter une approche plus traditionnelle à l’aide des API dans le code sans avoir à gérer le trafic HTTPS pour chaque appel. Le code source pour les API de Service Xbox Live est livré avec le Kit de développement Xbox (XDK) et peut être modifié et intégré dans votre titre en fonction des besoins. Les exemples sont écrits à l’aide de l’API de Service Xbox Live. Vous trouverez plus d’informations sur les API des Services Xbox Live dans Xbox One [documentation du XDK](https://developer.xboxlive.com/en-us/platform/development/documentation/software/Pages/home.aspx) sous « Xbox Live Services Reference ». [2]

### <a name="mpsd-sessions-and-templates"></a>Modèles et les sessions MPSD

Les sessions MPSD sont créées avec des modèles reçues via le portail de développement Xbox (XDP). Les modèles sont des documents JSON qui définissent l’infrastructure pour la session en cours de création. Le modèle fournit des constantes pour la nouvelle session.

L’extrait suivant à partir de la [exemple de code de lecteur Rendezvous](https://developer.xboxlive.com/en-us/platform/development/education/Pages/Samples.aspx) est un exemple de modèle de configuration.

```json
// Used for creating the session that you then pass into your match ticket request. This *should* not have any QoS requirements.
MatchTicketSession (Contract Version: 107)
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {},
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            }
        },
        "custom": {}
    }
}

// This is the new game session that is returned after you’ve been matched.
// Note: This is used for in-game QoS. For out-of-game QoS, you will need P2P/HTP requirements.
GameSession (Contract Version:107)
{
    "constants": {
        "system": {
            "maxMembersCount": 12,
            "capabilities": {"connectivity": true }
        },
        "memberInitialization": {
         "joinTimeout": 20000,
         "measurementTimeout": 15000,
         "externalEvaluation": false,
         "membersNeededToStart": 4
         },

   "custom": {}
  }
}
```

La session de ticket de correspondance doit être utilisée avec un modèle de session de jeu configuré avec des valeurs de délai d’expiration de qualité de service dans son **memberInitialization** objet.

Figure 2. Hopper d’exemple.

![](../../images/whitepapers/mpsd_image1.png)

L’extrait de code qui suit est un exemple d’un modèle de session de jeu de peer-to-peer (géré par le titre de la qualité de service).

```
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {
                "connectivity": true
            },
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 2
            }
        },
        "custom": {}
    }
}

```

Il s’agit d’un exemple d’une session de serveur de l’homologue et le modèle RAW.

```
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {}
        },
        "custom": {}
    }
}
```

Le code suivant montre un exemple d’un modèle de session de ticket de correspondance, qui est utilisé pour correspondre actives.

```
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {},
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            }
        },
        "custom": {}
    }
}

```

### <a name="checking-which-templates-are-configured-for-your-scid"></a>Vérifier les modèles sont configurés pour votre SCID

#### <a name="session-templates"></a>Modèles de session

La liste des modèles de session qui existent au sein d’un SCID, ainsi que les détails d’un modèle de session spécifique et peuvent être récupérées à partir de MPSD :

```
GET /serviceconfigs/{scid}/sessiontemplates

GET /serviceconfigs/{scid}/sessiontemplates/{session-template-name}
```

#### <a name="query-for-session-state-information"></a>Demander des informations d’état de session

Sessions peuvent être interrogées de niveaux de service config et session de modèle :

```
GET /serviceconfigs/{scid}/sessions

GET /serviceconfigs/{scid}/sessiontemplates/{session-template-name}/sessions
```

Par défaut, cela renverra un maximum de 100 sessions non privée. La requête peut être modifiée à l’aide de ces paramètres de chaîne de requête :

| Chaîne de requête             | Effet                                                                                                         | Description                                                                                         |
|--------------------------|----------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| keyword=foo              | Inclure uniquement les sessions qui ont le mot clé « foo ».                                                             |                                                                                                     |
| XUID=123                 | Inclure uniquement les sessions auxquelles l’utilisateur « 123 » dans.                                                               | Par défaut, l’utilisateur doit être actif dans la session à inclure.                                  |
| *reservations*=**true** | Inclure des sessions pour lequel l’utilisateur est défini comme un lecteur réservé, mais n’a pas joint pour qu’il devienne un lecteur actif. | Uniquement lors de l’interrogation de vos propres sessions, ou lors de l’interrogation des sessions de serveur à serveur d’un utilisateur spécifique. |
| *inactive*=**true**      | Inclure les sessions de l’utilisateur a accepté mais n’est pas en cours d’exécution.                                     | Le nombre de sessions dans lesquelles l’utilisateur est « prêt » mais pas « actif » comme étant inactives.                           |
| *private*=**true**       | Inclure les sessions privées.                                                                                      | Uniquement lors de l’interrogation de vos propres sessions, ou lors de l’interrogation de serveur à serveur.                            |
| *visibilité*= ouvert        | Inclure uniquement les sessions qui sont « ouverts ».                                                                         | Si défini sur « privé », le « privé = true » (directive) doit également être définie.                                 |
| *take*=5                 | Retourner jusqu'à cinq sessions.                                                                                    | Doit être comprise entre 0 et 100.                                                                          |

Le résultat est un tableau JSON des références de session. Certaines données de session sont incluses inline.

**Remarque** chaque requête doit inclure un filtre par mots clés, un filtre XUID ou les deux.

Paramètre *privé* (qui retourne les sessions privées) ou *réservations* (qui retourne des sessions de l’utilisateur n’a pas joint) à **true** nécessite l’appelant doit être accès au niveau du serveur à la session, ou pour XUID l’appelant prétendent correspondent au filtre XUID dans l’URI. Sinon, 403 Interdit est retournée (ou non des ces sessions existent réellement).

L’extrait de code suivant montre un exemple d’une réponse de requête.

```
{
"results": [ {
"xuid": "9876",  // If the session was found from a xuid, that xuid.
"startTime": "2009-06-15T13:45:30.0900000Z",
"sessionRef": {
  "scid": "foo",
  "templateName": "bar",
  "name": "session-seven"
},
"accepted": 4,        // Approximate number of non-reserved members.
"status": "active",   // or "reserved" or "inactive". This is the state of the user in the session, not the session itself. Only present if the session was found using a xuid.
"visibility": "open", // or "private", "visible", or "full"
"myTurn": true,       // Not present is the same as 'false'. Only present if the session was found using a xuid.
"keywords": [ "one", "two" ]
} ]
}

```

## <a name="session-template-attributes"></a>Attributs des modèles de session

<div id="_Contract_schema_update"/>

## <a name="contract-schema-update"></a>Mise à jour du schéma de contrat

Comme mentionné au début de ce document, la dernière version de contrat de modèle de session est 107, qui introduit des modifications au schéma à partir de la version précédente de 104. Les modèles qui ont été écrites pour la version de contrat 104 seront doivent être mis à jour si elles sont republiés en tant que 107. Voici un résumé des modifications.

-   **/constants/System/managedInitialization** a été renommée en **/constants/system/memberInitialization**.

    -   Le **autoEvaluate** champ a été renommé en **externalEvaluation** et ses modifications polarité, sauf que **false** reste la valeur par défaut.

    -   La valeur par défaut **membersNeededToStart** passe de 2 à 1.

    -   La valeur par défaut **joinTimeout** passe de 5 secondes à 10 secondes.

    -   Le **measurementTimeout** par défaut passe de 5 secondes à 30 secondes.

-   **/constants/System/Timeouts** est supprimé, et les délais d’expiration sont renommés et déplacés sous **/constantes/système**.

    -   Le **réservé** délai d’attente devient **reservedRemovalTimeout**.

    -   Le **inactive** délai d’attente devient **inactiveRemovalTimeout** et sa nouvelle valeur par défaut est 0 au lieu de 2 heures.

    -   Le **prêt** délai d’attente devient **readyRemovalTimeout**.

    -   Le **sessionEmpty** délai d’attente devient **sessionEmptyTimeout**.

-   **/constants/System/Capabilities/gameplay** doit être spécifié en tant que **true** sur les sessions qui représentent le jeu réel (par opposition à des sessions d’assistance telles que les sessions de correspondance et de la salle d’attente) afin de permettre aux joueurs récentes et Création de rapports de réputation.

### <a name="system-objects"></a>Objets système

Chacun des objets système dans le document de la session a un schéma fixe qui est appliqué et interprété par MPSD.

Dans le corps des requêtes PUT, les objets système sont validées et fusionnées tout comme les objets personnalisés. Mais contrairement aux objets personnalisés, une fois qu’elles sont fusionnées le système d’objets sont encore validées et réactions en fonction de ces schémas.

**/constants/system**

```json
{
"version": 1,  //Document Version (FAL release version 1, service contract 107)
"maxMembersCount": 100,  // Defaults to 100 if not set on creation. Must be between 1 and 100.
"visibility": "private",  // Or "visible" or "open", defaults to "open" if not set on creation.

"initiators": [ "1234" ],  // If specified on a new session, the creators xuid must be in the list (or the creator must be a server).
"inviteProtocol": "party",  // Optional URI scheme of the launch URI for invite toasts.

"reservedRemovalTimeout": 10000, // Default is 30 seconds. Member is removed from the session.
"inactiveRemovalTimeout": 0, // Default is zero. Member is removed from the session.
"readyRemovalTimeout": 60000, // Default is three minutes. Member is removed from the session.
"sessionEmptyTimeout": 0, // Default is zero. Session is deleted.

// Capabilities are boolean values that are optionally set in the session template. If no capabilities are needed, an empty "capabilities" object should be in the template in order to prevent capabilities from being specified on session creation, unless the title desires dynamic session capabilities.
"capabilities": {
"clientMatchmaking": true,
"connectivity": true, // Cannot be set if ‘large’ is specified.
     "suppressPresenceActivityCheck": false,
     "gameplay": false,
     "large": false
},
/* If a "memberInitialization" object is set, the session expects the client system or title to perform initialization following session creation and/or as new members join the session. The timeouts and initialization stages are automatically tracked by the session, including QoS measurements if any metrics are set. These timeouts override the session's reservation and ready timeouts for members that have 'initializationEpisode' set. */
"memberInitialization": {
"joinTimeout": 20000,  // Milliseconds. Unspecified counts as 10 seconds.
"externalEvaluation": false,
"membersNeededToStart": 2  // Unspecified counts as 1. Must be between 0 and maxMembersCount. Only applies to episode 1. If 00 and the session is created with no members to initialize, episode 1 is skipped..
},
```

**/properties/system**

```
{
// Optional array of case-insensitive strings. Cannot be set if the session's visibility is "private".
"keywords": [ "hello" ],

// Array of integer indices of members whose turn it is. Defaults to empty. Can't be set (and doesn't appear) on large sessions.
"turn": [ 0 ],

/* Restricts who can join "open" sessions. (Has no effect on reservations, which means it has no impact on "private" and "visible" sessions.) Defaults to "none". On large sessions, "none" is the only valid value.
If "local", only users whose token's DeviceId matches someone else already in the session and "active": true.
If "followed", only local users (as defined above) and users who are followed by an existing (not reserved) member of the session can join without a reservation. */
"joinRestriction": "none",

// Device token of the host. Must match the "deviceToken" of at least one member, otherwise this field is deleted.
// If "peerToHostRequirements" is set and "host" is set, the measurement stage assumes the given host is the correct host and only measures metrics to that host.
// Can't be set on large sessions.
"host": "99e4c701",

// Can only be set while "initializing/stage" is "evaluating". True indicates success, and false indicates failure. Once set, "initializing/stage" is immediately updated, and this field is removed.
"initializationSucceeded": true,

/* The ordered list of case-insensitive connection strings that the session could use to connect to a game server. Generally titles should use the first on the list, but sophisticated titles could use a custom mechanism (e.g. Thunderhead) for choosing one of the others (e.g. based on load). */
"serverConnectionStringCandidates": [ "datacenter b", "serverfarm a" ],

"matchmaking": {
    "targetSessionConstants": { },
    // Force a specific connection string to be used (useful in preserveSession=always cases).
    "serverConnectionString": "datacenter b",
},

// True if the match that was found didn't work out and needs to be resubmitted. Set to false to signal that the match did work, and the matchmaking service can release the session.
"matchmakingResubmit": true
}

```

### <a name="timeouts"></a>Timeouts

Sessions peut être modifiées par les minuteurs et autres événements externes. Le **des délais d’expiration** objet de MPSD fournit un mécanisme de base pour gérer la durée de vie de session et des membres.

Le **nextTimer** champ de la session donne le temps de la minuterie suivante. Les clients peuvent utiliser ces informations pour choisir un bon intervalle pour l’interrogation suivante. Cette valeur est également retournée dans le **expiration** en-tête HTTP.

Délais d’expiration sont spécifiés en millisecondes. Zéro est autorisé et signifie que le délai d’expiration doit être immédiat. Si un délai donné n’est pas spécifié, il est considéré comme infini. Étant donné que les délais d’expiration ont des valeurs par défaut, le modèle de session doit spécifier explicitement « null » pour un délai d’attente infini.

#### <a name="sessionemptytimeout"></a>SessionEmptyTimeout

Le **/constants/system/sessionEmptyTimeout** valeur configure le nombre de millisecondes après une session devienne vide qu’il sera supprimé. La valeur par défaut est zéro, ce qui signifie que la session est immédiatement supprimée. Si la valeur n’est pas spécifiée, les sessions vides seront trouvera indéfiniment.

#### <a name="member-timeouts"></a>Membre des délais d’attente

Les trois autres délais d’expiration dans **/constantes/système** contrôler la quantité de temps un membre peut rester dans un état particulier :

-   **reservedRemovalTimeout**

    -   La réservation est supprimée lorsque le délai d’attente expire. La valeur par défaut est de 30 secondes.

-   **inactiveRemovalTimeout**

    -   Un membre est supprimé de la session après deux heures par défaut.

-   **readyRemovalTimeout**

    -   Les membres qui sont « prêts » revenir à l’état inactif après trois minutes par défaut.

<div id="_Member_initialization_in"/>

## <a name="member-initialization-in-session-documents"></a>Initialisation d’un membre dans les documents de la session

### <a name="member-initialization"></a>Initialisation de membre


Le **memberInitialization** objet contrôle l’initialisation des étapes après la création de session et/ou de nouveaux membres rejoindre la session. Les étapes des délais d’expiration et d’initialisation sont automatiquement suivies par la session, y compris les mesures de qualité de service si toutes les mesures sont définies.

Ces délais d’attente remplacent la réservation et les délais d’expiration de prêt de la session pour les membres qui ont le **initializationEpisode** jeu de propriétés.

Exemple :

```
"memberInitialization": {
        "joinTimeout": 5000,
        "measurementTimeout": 5000,
        "evaluationTimeout": 5000,  // only specify for external evaluation
        "externalEvaluation": true,
       "membersNeededToStart": 2
    },
```


![](../../images/whitepapers/mpsd_image2.png)

**Figure 3. Flux de l’initialisation de membre.**

Chacune des trois étapes d’initialisation de membre peuvent expirer :

1.  **joiningTimeout**

    -   La durée, en millisecondes, que les utilisateurs doivent rejoindre la session. Les réservations de membres qui ne parviennent pas à joindre sont supprimées.

2.  **measuringTimeout**

    -   La durée pendant laquelle les membres ont à charger leurs mesures. Les membres qui ne parviennent pas à télécharger des mesures sont marquées avec un *failureReason* de « timeout ».

3.  **evaluationTimeout**

    -   La quantité de temps pour un membre à effectuer et de charger la décision d’évaluation. Si aucune décision n’est reçue, elle est considérée comme un échec.

**externalEvaluation**

-   MPSD faire une évaluation automatique de QoS basée sur les spécifications fournies dans le modèle de session. L’évaluation est effectuée lorsque externalEvaluation est définie sur false. **externalEvaluation** doit être **true** lorsqu’un **evaluationTimeout** est défini. S’il existe deux exigences peer-to-peer ou pair-à-host, vous devez toujours définir **externalEvaluation** à **false** afin d’avoir à la session de fin automatiquement de l’initialisation.

**membersNeededToStart**

-   Il s’agit du nombre minimal de réservations de membres doivent exister avec « initialisation » : « true » et transmettez la qualité de service pour l’évaluation automatique réussisse.

### <a name="initialization-episodes"></a>Épisodes de l’initialisation


Lorsque le **memberInitialization** objet est défini, MPSD progresseront pour les phases de l’initialisation par épisode. Le premier épisode est démarré lorsque la session est créée et inclura toutes les phases définies dans le modèle.

Tous les nouveaux membres invités ou jointe pendant un épisode est déjà en cours d’exécution seront marquées pour le prochain épisode. L’état d’un épisode ou **memberInitialization** en général pouvez récupérer à partir de la **initialisation** objet de la session.

**Remarque** n’oubliez pas, cet objet est supprimé après l’initialisation est terminée.

Exemple :

```
"initializing": {
    "stage": "measuring",
    "stageStartTime": "2009-06-15T13:45:30.0900000Z",
    "episode": 1
},

```

L’étape va joindre à mesure de l’évaluation. Si un épisode échoue, étape a la valeur *échec* et la session ne peut pas être initialisée. Sinon, lorsqu’un épisode de l’initialisation est terminée, le **initialisation** objet est supprimé.

Échecs d’initialisation peuvent également être suivis pour chaque membre. Elles sont définies lors de la transition en dehors de la phase de jointure ou mesure si ce membre ne passe pas.

Exemple :
```
"initializationFailure": "latency",
```
Dans l’ordre de priorité, la valeur de cet attribut peut être définie *délai d’attente, latence, bandwidthdown, bandwidthup, réseau, groupe*, ou *épisode*. La valeur de réseau signifie que la configuration du réseau et/ou les conditions (telles que la traduction d’adresse réseau en conflit \[NAT\]) a empêché les mesures de qualité de service à mesurer. La seule valeur possible à la fin de la jointure est *groupe*. (Sur le délai d’attente à partir de la jointure, la réservation est supprimée.)

Si **memberInitialization** a la valeur et le membre a été ajouté par le « initialisation » : true, il est défini sur l’épisode d’initialisation qui participera au membre. Une valeur de 1 est utilisée pour les membres ajoutés à une nouvelle session au moment de que sa création, et il est supprimé lorsque l’épisode de l’initialisation se termine.
```
"initializationEpisode": 1,
```

## <a name="match-ticket-session"></a>Correspond à la session de ticket

Quand une session MPSD est utilisée comme une session de ticket de correspondance, certaines propriétés de la session et les constantes sont utilisées.

**/members/{index}/constants/system**

```json
{

  {
  "xuid": "12345678",

  "initialize": "false", // Run initialization for this user (if “memberInitialization” is set). Defaults to false.

```

Lorsque le service ajoute des utilisateurs à une session, il fournit un contexte autour comment et pourquoi ils ont été mis en correspondance dans la session, dans le **matchmakingResult** champ.

```
"matchmakingResult": {
```

Il s’agit d’une copie de l’utilisateur **serverMeasurements** à partir de la session de matchmaking.

```json
"serverMeasurements": {
    "east.thunderhead.azure.com": {
        "latency": 233  // Milliseconds
      }
    }
  }
}

```

## <a name="quality-of-service-qos-templates"></a>Qualité des modèles de Service (QoS)

Pour les modèles de session de jeu, les valeurs peuvent être ajoutées qui informent MPSD coordonner vos efforts avec la couche réseau et la plateforme de réseaux sociaux de console. L’objectif de cette coordination consiste à valider la qualité de l’état de connexion avant qu’une session est créée et avant que l’utilisateur est informé que le jeu est prêt à joindre.

L’extrait de code suivant est un exemple d’un modèle de session de jeu à hôte homologue avec QoS :

```json
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 20,
            "visibility": "private",
            "capabilities": {
                "connectivity": true
            },
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            },
            "peerToHostRequirements": {
                "latencyMaximum": 350,
                "bandwidthDownMinimum": 1000,
                "bandwidthUpMinimum": 100,
                "hostSelectionMetric": "latency"
            }
        },
        "custom": {}
    }
}
```

Cet extrait de code est un exemple d’un modèle de session de jeu de peer-to-peer avec QoS :

```json
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 20,
            "visibility": "private",
            "capabilities": {
                "connectivity": true
            },
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            },
            "peerToPeerRequirements": {
                "latencyMaximum": 250,
                "bandwidthMinimum": 10000
            }
        },
        "custom": {}
    }
}

```

### <a name="qos-session-template-attributes"></a>Attributs de modèle de session de qualité de service

Si un **memberInitialization** objet est défini, la session attend le système client ou le titre pour effectuer une initialisation après la création de session et/ou en tant que nouveaux membres rejoindre la session.

Les étapes des délais d’expiration et d’initialisation sont automatiquement suivies par la session, y compris les mesures de qualité de service si toutes les mesures sont définies.

Ces délais d’attente remplacent la réservation et les délais d’expiration de prêt de la session pour les membres qui ont le **initializationEpisode** jeu de propriétés.

```json
"memberInitialization": {
"joinTimeout": 5000,  // Milliseconds. Unspecified counts as 10 seconds.
"measurementTimeout": 5000,  // Milliseconds. Unspecified counts as 30 seconds.
"evaluationTimeout": 5000,  // Milliseconds. Can only be set if 'externalEvaluation' is true. Unspecified counts as 5 seconds.
"externalEvaluation": true,
"membersNeededToStart": 2  // Unspecified counts as 1. Must be between 0 and maxMembersCount. Only applies to episode 1. If 0 and the session is created with no members to initialize, episode 1 is skipped.
},

```

Une session de jeu avec QoS requiert la fonctionnalité de connectivité. Si aucune mesure n’est spécifiée, leur valeur par défaut pour ce qui est nécessaire pour répondre aux exigences de qualité de service. S’ils sont spécifiés, ils doivent être suffisantes pour répondre aux exigences de qualité de service.

```json
"metrics": {
    "latency": true,
    "bandwidthDown": true,
    "bandwidthUp": true,
    "custom": true

```

Les seuils suivants s’appliquent à chaque connexion par paire pour tous les membres dans une session :

```json
"peerToPeerRequirements": {
    "latencyMaximum": 250,  // Milliseconds
    "bandwidthMinimum": 10000  // Kilobits per second
},

```

Les seuils suivants s’appliquent à chaque connexion à partir d’un candidat d’hôte :

```json
"peerToHostRequirements": {
    "latencyMaximum": 250,  // Milliseconds
    "bandwidthDownMinimum": 100000,  // Kilobits per second
    "bandwidthUpMinimum": 1000,  // Kilobits per second
    "hostSelectionMetric": "bandwidthup"  // Or "bandwidthdown" or "latency". Not specified is the same as "latency".
},

```

La connexion au serveur potentielles suivantes les chaînes doivent être évalués (Notez que les chaînes de connexion doivent être en minuscules) :

```json
"measurementServerAddresses": {
        "east.thunderhead.azure.com": {
            "secureDeviceAddress": "r5Y="  // Base-64 encoded secure-device-address
        },
        "west.thunderhead.azure.com": {
            "secureDeviceAddress": "rwY="
        }
    }
}

```

**members/{index}/properties/system**

Ces indicateurs contrôlent l’état de membre et **activeTitle**, et ils s’excluent mutuellement (c’est une erreur à la valeur à la fois **true**). Pour chacune, **false** est identique à « absent ». L’état par défaut est « inactif », autrement dit, qu'aucun n’est présent.

```json
"ready": true,
"active": false,

// Base-64 blob, or not present. Empty-string is the same as not present.
// 'capabilities/connectivity' must be enabled in order for this to be set.
"secureDeviceAddress": "ryY=",

// List of members in my group, by index. Each element must be an integer 0 <= n < 'membersInfo/next'.  
// During member initialization, if any members in the list fail, this member will also fail.
"group": [ 5 ],

// QoS measurements by lower-case device token.
// Like all fields, "measurements" must be updated as a whole. It should be set once when measurement is complete, not incrementally.
// Metrics can me omitted if they weren't successfully measured, i.e. the peer is unreachable.
// If a "measurements" object is set, it can't contain an entry for the member's own address.
"measurements": {
"e69c43a8": {
"latency": 5953,  // Milliseconds
"bandwidthDown": 19342,  // Kilobits per second
"bandwidthUp": 944,  // Kilobits per second
"custom": { }
     }
},

// QoS measurements by game-server connection string. Like all fields, "serverMeasurements" must be updated as a whole, so it should be set once when measurement is complete.
// If empty, it means that none of the measurements completed within the "serverMeasurementTimeout".
    "serverMeasurements": {
        "east.thunderhead.azure.com": {
            "latency": 233  // Milliseconds
        }
    },

// Subscriptions for shoulder taps on session changes. The 'profile' indicates which session changes to tap as well as other properties of the registration like the min time between taps.
// The subscription is named with a client-generated GUID that is also sent back with the tap as a context ID.
// Subscriptions can be added and removed individually, without affecting other subscriptions in the "subscriptions" object.
// To remove a subscription, set its context ID to null.
// (Like the "ready" and "active" flags, the "subscriptions" data is copied out and maintained internally, so the normal replace-all rule on system fields doesn't apply to "subscriptions".)
"subscriptions": {
"961dc162-3a8c-4982-b58b-0347ed086bc9": {
"profile": "party",  // Or "matchmaking", "initialization", "roster", "queueHost", or "queue"
"onBehalfOfTitleId": "3948320593",  // Optional decimal title ID of the registered channel.  If not set the title ID is taken from the token.
},
"709fef70-4638-4b94-905b-24cb02706eb5": null
}
}

```

## <a name="qos-phase-and-session-initialization-details"></a>Détails de l’initialisation de session et de la phase de qualité de service

MPSD suit et résultats de qualité de service pour la création de jeux de rapports une fois le modèle terminé l’initialisation de membre. La progression de cette opération est représentée par un *initialisation* de l’objet dans le document de session comme décrit dans la [l’initialisation de membre](#_Member_initialization_in) section ci-dessus. Le *initialisation* objet possède un *étape* attribut, qui représente l’étape en cours d’initialisation. Faire évoluer la scène *rejoindre* à *mesure* à *l’évaluation*.

-   Si l’initialisation épisode \#1 échoue, l’étape est définie sur *échec* et la session ne peut pas être initialisée. Sinon, lorsqu’un épisode de l’initialisation est terminée, l’objet « initialisation » est supprimé. Si **externalEvaluation** a la valeur **false**, l’évaluation d’étape est ignorée. Si ni **métriques** ni **measurementServerAddresses** est défini, la phase de mesure est ignorée.

```json
"initializing": {
    "stage": "measuring",
    "stageStartTime": "2009-06-15T13:45:30.0900000Z",
    "episode": 1
},

```

-   Candidats de l’hôte sont les jetons de périphérique répertoriés par ordre de préférence. Elles sont définies après le *mesure* stade de l’épisode de l’initialisation \#1 si **peerToHostRequirements** est définie et **/properties/system/host** n’est pas définie. Elle est également effacée après un **/properties/system/host** objet est défini.

```json
"hostCandidates": [ "ab90a362", "99582e67" ],

"constants": { /* Property Bag */ },
"properties": { /* Property Bag */ },
"members": {
    "1": {
        "constants": { /* Property Bag */ },
        "properties": { /* Property Bag */ },

```

-   Le *gamertag* attribut n’est défini que si le membre a accepté et si une revendication d’identité a été trouvée pour ce membre.

```json
"gamertag": "stacy",
```

-   Le *deviceToken* attribut est défini lorsque le membre télécharge une adresse sécurisée des appareils. C’est une chaîne de non-respect de la casse qui peut être utilisée pour les comparaisons d’égalité.

```json
"deviceToken": "9f4032ba7",
```

-   Le *réservé* valeur est supprimée une fois que l’utilisateur exécute sa première PUT demande vers le document de la session. Lorsque les lecteurs sont réservés, cela signifie qu’ils ont été invités à la session de jeu mais n’ont ni accepté ni avaient leurs connexions évaluées.

```json
"reserved": true,
```

-   Si le membre est actif, *activeTitleId* est le titre dans lequel le membre est actif, au format décimal.

```json
"activeTitleId": "8397267",
```

-   Cet attribut correspond à la durée que l’utilisateur rejoint la session. Si *réservé* est **true**, puis *joinTime* sera l’heure à laquelle la réservation a été effectuée.

```json
"joinTime": "2009-06-15T13:45:30.0900000Z",
```

-   Présenter si ce membre n’est dans le tableau de propriétés/système/turn, sinon pas.

```json
"turn": true,
```

-   Le *initializationFailure* attribut est défini sur l’objet de membre lors de la phase de mesure si le membre n’a pas réussi à l’étape ou de transition en dehors de la jointure. Dans l’ordre de priorité, elle peut être définie *délai d’attente*, *latence*, *bandwidthdown*, *bandwidthup*, *réseau* , *groupe*, ou *épisode*.
    Le *réseau* valeur signifie que la configuration du réseau et/ou les conditions (telles que des traductions d’adresses réseau en conflit \[NAT\]) a empêché les mesures de qualité de service à mesurer. La seule valeur possible à la fin de la jointure est *groupe*. (Sur le délai d’attente à partir de la jointure, la réservation est supprimée.) Le *épisode* a la valeur après une étape « évaluation » ayant échoué sur tous les membres de l’épisode de l’initialisation n’a pas échoué au cours de jointure ou de mesure.

```json
"initializationFailure": "latency",
```

-   Si **memberInitialization** a la valeur et le membre a été ajouté par le « initialisation » : true, il est défini sur l’épisode d’initialisation qui participera au membre. Une valeur de 1 est utilisée pour les membres ajoutés à une nouvelle session au temps de création. Supprimé lorsque l’épisode de l’initialisation se termine.

```json
"initializationEpisode": 1,
```

-   Le *suivant* attribut représente la valeur d’index du membre suivant dans la session. Il sera la même valeur que la *suivant* d’attribut dans le **membersInfo** si aucun membre de plus pour ajouter l’objet ci-dessous.

```json
            "next": 4
        },
        "4": { "next": 5 /* etc */ }
    },
    "membersInfo": {
        "first": 1,  // The first member's index.
        "next": 5,  // The index that the next member added will get.
        "count": 2,  // The number of members.
        "accepted": 1  // The number of members that are no longer 'pending'.
    },
    "servers": {
        "name": {
            "constants": { /* Property Bag */ },
            "properties": { /* Property Bag */ }
        }
    }
}

```

## <a name="xbox-cloud-compute-session"></a>Session de calcul Cloud Xbox

Une session de calcul Cloud Xbox contient la liste ordonnée, non-respect de la casse des chaînes de connexion que la session peut utiliser pour se connecter à un serveur de jeu. En règle générale, titres doivent utiliser la première chaîne de la liste, mais les titres sophistiqués peuvent utiliser un mécanisme personnalisé (par exemple, Xbox Live Compute) pour choisir l’une des autres (par exemple, en fonction de chargement).

```json
    "serverConnectionStringCandidates": [ "west.thunderhead.azure.com", "east.thunderhead.azure.com" ],

    "matchmaking": {
        "clientResult": {  // Requires the clientMatchmaking property.
            "status": "searching",  // Or "expired", "found", "failed", or "canceled".
            "statusDetails": "Description",  // Default is empty string.
            "typicalWait": 30,  // The expected number of seconds waiting as a non-negative integer.
            "targetSessionRef": {
                "scid": "1ECFDB89-36EB-4E59-8901-11F7393689AE",
                "templateName": "capture-the-flag",
                "name": "2D58F65F-0E3C-4F1F-8277-2BC9873FDB23"
            }
        },

        "targetSessionConstants": { },

        // Force a specific connection string to be used (useful in preserveSession=always cases).
        "serverConnectionString": "west.thunderhead.azure.com",

        // True if the match that was found didn't work out and needs to be resubmitted. Set to false
        // to signal that the match did work, and the matchmaking service can release the session.
        "resubmit": true
    }
}

```

** / serveurs / {nom-serveur} / Propriétés/système **

```json
{
    "lockId": "opaque56789",  // If set, a matchmaking service is servicing this session.
    "status": "searching",  // Or "expired", "found", "failed", or "canceled". Optional.
    "statusDetails": "Description",  // Optional free-form text. Default is empty string.
    "typicalWait": 30,  // Optional. The expected number of seconds waiting as a non-negative integer.
    "targetSessionRef": {  // Optional.
        "scid": "1ECFDB89-36EB-4E59-8901-11F7393689AE",
        "templateName": "capture-the-flag",
        "name": "2D58F65F-0E3C-4F1F-8277-2BC9873FDB23"
    }
}

```

## <a name="raw-session-and-custom-title-properties"></a>Session brutes et les propriétés du titre personnalisé

Une session peut être utilisée pour stocker les informations des tâches de nettoyage personnalisé (métadonnées) autour d’un jeu multijoueur. Données de jeu ou des informations enregistrées doivent être stockées dans TMS ++.

### <a name="property-bags"></a>Conteneurs de propriétés

Chacun des objets ci-dessus marqué comme un conteneur de propriétés se compose de deux objets internes facultatifs, système et personnalisées.

Les objets personnalisés peuvent contenir n’importe quel JSON.

```json
"custom": {
    "myField1": true,
    "myField2": "string",
    "myField3": 5.5,
    "myField4": { "myObject": null },
    "myField5": [ "my", "array" ]
}

```

## <a name="active-member-decay"></a>Atténuation de membre actif

Membres actifs sont automatiquement marqués inactifs lorsque MPSD détecte que l’utilisateur est activé n’est plus dans le titre. Cela peut se produire, par exemple, si la présence expire à l’enregistrement de l’utilisateur. Ce mécanisme est simplement une protection ; Autrement dit, les titres sont toujours requis pour marquer les membres comme étant inactive (ou de façon proactive les supprimer de la session) lorsque les membres laisseront ou basculement vers un autre titre, sign out ou sinon devienne désengagés.

## <a name="faq-and-troubleshooting"></a>FAQ et résolution des problèmes

### <a name="how-do-i-call-mpsd-"></a>Comment appeler les MPSD ?

À l’aide de l’authentification par certificat : sessiondirectory.xboxlive.com de client

Exemple :

```
PUT https://client-sessiondirectory-stress.xboxlive.com/serviceconfigs/8cvda84-2606-4bab-8eda-d12313e65143/sessiontemplates/teamDeathmatch/sessions/3baafc35-816d-49cd-9656-5772506c988a
```

À l’aide de l’authentification XToken : sessiondirectory.xboxlive.com

Exemple :

```
PUT https://sessiondirectory-stress.xboxlive.com/serviceconfigs/8cvda84-2606-4bab-8eda-d12313e65143/sessiontemplates/teamDeathmatch/sessions/3baafc35-816d-49cd-9656-5772506c988a
```

### <a name="how-do-i-find-out-what-scid-session-template-and-sandbox-to-use"></a>Comment trouver les SCID, le modèle de session et le bac à sable à utiliser ?

Vous trouverez ces informations sur le site XDP pour votre titre. Si vous n’avez pas encore accès à XDP contacter votre responsable de compte de développeur, qui peut vous aider dans l’obtention des informations pour vous.

### <a name="is-there-an-example-of-a-request-body-that-i-can-compare-my-own-to"></a>Existe-t-il un exemple d’un corps de demande puis-je comparer mon propre à ?


### <a name="i-am-getting-a-404-error-when-calling-mpsd"></a>J’obtiens une erreur 404 lors de l’appel MPSD.

Collecter les traces Fiddler pour aider à obtenir des informations supplémentaires et puis procédez comme suit :

1.  Vérifiez le message retourné en tant que partie intégrante du corps HttpResponse pour les 404 messages courantes :

    -   *La configuration de service demandé est non valide ou n’est pas configuré pour les sessions*. Vérifiez que le SCID utilisée est correcte.

    -   *La session demandée n’a pas été trouvée*. Vérifiez que la session est créée et le modèle de session est correct avant de les récupérer. Vous pouvez créer une session avec un appel PUT.

2.  Vérifiez l’URI que vous utilisez.

3.  Redémarrez la console et/ou réessayez avec un nouvel utilisateur.

4.  Recherche le [Forums des développeurs de divertissement](https://developer.xboxlive.com/en-us/platform/community/forums/Pages/home.aspx) pour le code d’erreur ou d’autres solutions potentielles.

### <a name="i-am-getting-a-403-error-when-calling-mpsd"></a>J’obtiens une erreur 403 lors de l’appel MPSD.

Il s’agit généralement des autorisations d’un ou de problème de l’étendue. Collecter les traces Fiddler pour aider à obtenir des informations supplémentaires et puis procédez comme suit :

1.  Vérifier les messages retournés en tant que partie intégrante du corps HttpResponse pour les 403 messages courantes :

-   * La configuration de service demandé n’est pas accessible. *

    -   Vérifiez que vous utilisez un compte qui a accès à la sandbox.

    -   Vérifiez que vous êtes dans le bac à sable correct.

    -   Si vous utilisez l’authentification par certificat et que vous obtenez cette erreur, contactez votre mère.

-   *La session demandée n’est pas accessible. Les Sessions privé peuvent uniquement être lus par les membres de la session.*

    -   Vous tentez d’accéder à une session qui a une visibilité de « privé ». Seuls les membres dans la session peuvent lire le document de la session.

-   *Le corps de la demande ne peut pas contenir les références de membre existantes, sauf si l’authentification du principal inclut un serveur.*

    -   Vous ne pouvez pas joindre un autre utilisateur à une session pour le compte d’un utilisateur. Vous pouvez uniquement inviter. L’index de la valeur « réserver\_&lt;nombre&gt;» pour inviter un lecteur.

### <a name="i-am-getting-a-412-precondition-failed-error"></a>J’obtiens une erreur 412 Échec de la précondition.

Ceci renverra 412 Échec de la précondition si la session existe déjà :

> PUT serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/rapide/sessions/foo HTTP/1.1 Content-Type : application/json If-None-Match : \*

Ceci renverra 412 Échec de la précondition si l’etag de la session ne correspond pas à l’en-tête If-Match :

> PUT serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/rapide/sessions/foo HTTP/1.1 Content-Type : application/json If - Match : 9555A7DE-8B91-40E4-8CFB-0629312C9C7D

### <a name="i-am-getting-errors-such-as-405-409-503-and-400when-calling-mpsd"></a>J’obtiens des erreurs, 405, 409, 503 et MPSD appelant 400when.

Collecter traces Fiddler pour aider à obtenir plus d’informations, puis vérifiez les messages retournés en tant que partie intégrante du corps HttpResponse. Cela devrait vous donner suffisamment d’informations pour identifier et corriger l’erreur ou pour rechercher le [Forums des développeurs de divertissement](https://developer.xboxlive.com/en-us/platform/community/forums/Pages/home.aspx) pour les solutions possibles.

Vous pouvez également obtenir le corps de réponse si vous utilisez les API de Service Xbox Live en définissant le **DiagnosticsTraceLevel** propriété erreur, les informations contenues dans la sortie de débogage de sortie qui vous, ou vous pouvez utiliser le  **XboxLiveContextSettings.ServiceCallRouted** événement, comme illustré dans plusieurs exemples de sortie pour le titre de votre interface utilisateur.

### <a name="what-can-or-should-i-change-in-the-templates-for-my-title"></a>Ce qui peut ou dois-je modifier dans les modèles pour mon titre ?

Modèles de session ne sont pas des valeurs par défaut, mais sont plus de moule. Toutefois, vous ne peut pas remplacer les constantes déjà définis dans les modèles, bien que vous pouvez y ajouter.

### <a name="im-getting-an-error-that-is-saying-my-session-isnt-initialized"></a>J’obtiens une erreur qui dit à que ma session n’est pas initialisée.

Lors de l’initialisation d’un membre est présente dans votre modèle (jeu, tiers et des scénarios de ticket correspondance, généralement), vous devez vous assurer que « initialiser = true » est défini pour un nombre suffisant de réservations de membre (les membres requis pour démarrer) pour passer de QoS pour résoudre ce problème.

### <a name="my-session-is-not-being-created-and-im-getting-an-http-204-error"></a>Ma session n’est pas créée et j’obtiens une erreur HTTP 204.

Cela indique qu’il n’y avait aucun utilisateur ajouté à la session lors de leur création. S’il n’existe aucun utilisateur pour une session au moment de la création, la session ne sera pas créée. Assurez-vous que vous ajoutez au moins un lecteur à une session sur la création. Pour les scénarios de serveur dédié, obtenir un lecteur qui essaie de créer une correspondance, ou qui a besoin créer une correspondance et qu’il devienne le lecteur initial dans cette correspondance. Vous pouvez également modifier ou supprimer la **sessionEmptyTimeout**.

### <a name="when-should-i-poll-the-mpsd"></a>Quand dois-je pour interroger le MPSD ?

Vous devez éviter d’interrogation des sessions MPSD. À un niveau élevé, cela en concevant votre code d’une manière qui utilise la session MPSD uniquement pour les établissement initial de la connectivité réseau pour chaque client et pour rétablir l’état du réseau pour un client ou les clients qui ont perdu la connectivité. Il est également important de tirer parti des primitives de synchronisation basé sur l’etag de MPSD pour éliminer la nécessité d’actualiser l’état de session pour résoudre les conditions de concurrence.

Une application courante de ces principes est lorsque vous avez un ensemble de N clients qui doivent tous se connecter entre eux et de lire dans un maillage pair à pair. Plutôt que d’interroger régulièrement la session pour les nouveaux membres, chaque membre permettre rejoindre la session, connectez-vous aux membres déjà dans la session et supposent que les souscriptions ultérieures effectue la même. Consultez les exemples de conversation et le lecteur Rendezvous pour obtenir des exemples montrant comment implémenter cela.

Il existe certains cas rares où l’interrogation peut être nécessaire pendant de courtes périodes ; Contactez votre responsable de compte de développeur si vous pensez que vous avez besoin pour ce faire.
