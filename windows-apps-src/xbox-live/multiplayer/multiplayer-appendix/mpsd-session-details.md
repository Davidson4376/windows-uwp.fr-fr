---
title: Détails de la session multijoueurs
description: Découvrez les détails des sessions de multijoueur Xbox Live.
ms.assetid: 5aeae339-4a97-45f4-b0e7-e669c994f249
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une multijoueur 2015, session, mpsd
ms.localizationpriority: medium
ms.openlocfilehash: 3eb92af2d478fa41150f375dd19ef347d2b6f2e9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616084"
---
# <a name="mpsd-session-details"></a>Détails de la session MPSD

Cet article comprend les sections suivantes :
* Vue d’ensemble de la session
* Fonctionnalités de la session
* Taille de la session
* États de session utilisateur
* Visibilité et Joinability
* Délais d’expiration de session
* Plusieurs utilisateurs connectés sur une Console unique
* Gestion du cycle de vie des processus
* Nettoyage des Sessions inactives
* Session arbitre

## <a name="session-overview"></a>Vue d’ensemble de la session

Une session de l’annuaire de Session multijoueur (MPSD) a un nom de session et est identifiée en tant qu’instance d’un modèle de session, qui est un document JSON qui fournit des paramètres par défaut pour la session. Le modèle fait partie d’une configuration de service avec un identificateur de configuration de service (SCID), qui est un GUID. Ce modèle se trouve sur [Xbox Developer Portal (XDP)](https://xdp.xboxlive.com) et [partenaires](https://partner.microsoft.com/dashboard). Configurations de service sont les ressources destinées aux développeurs utilisés pour l’ingestion, de gestion et de stratégie de sécurité. Lorsqu’une session est accessible via MPSD, d’autorisation du principal est effectuée sur la configuration du service selon les stratégies d’accès définie par le développeur via XDP ou partenaires. Les vérifications d’accès secondaire, telles que la validation de l’appartenance de session, sont effectuées au niveau de la session lorsque la session est chargée une fois que l’accès à la configuration du service est autorisé.

Cette rubrique suppose que votre modèle utilise la version de contrat 107, qui est la version utilisée par le MPSD actuel pour Xbox One. Si vous avez défini des modèles basés sur la version de contrat 105 (identique à 104), vous devez modifier ces pour prendre en charge la version 107. Pour obtenir des instructions, consultez [les problèmes de migration courants multijoueurs 2015](common-issues-when-adapting-multiplayer.md).

| Important                                                                                                                                                                                                                                                      |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Fonctionnalité qui est définie via un modèle ne peut pas être modifiée via écritures à la MPSD. Pour modifier les valeurs, vous devez créer et envoyer un nouveau modèle avec les modifications nécessaires. Tous les éléments qui ne sont pas définies via un modèle est modifiable via les écritures à MPSD. |

### <a name="session-reference"></a>Référence de session

Chaque session MPSD identifie de façon unique référencée par une référence de session, représentée dans l’API WinRT multijoueur par le **MultiplayerSessionReference classe**. La référence de session contient ces valeurs de chaîne :

-   Identificateur de configuration de service (SCID)
-   Nom de modèle de session
-   Nom de session

La référence de la session est mappé dans l’URI pour identifier des sessions comme indiqué ci-dessous. Dans le mappage de l’exemple suivant, « autorité » est sessiondirectory.xboxlive.com.

```HTTP
https://{authority}/serviceconfigs/{service-config-id}/sessiontemplates/{session-template-name}/sessions/{session-name}
```

### <a name="elements-of-a-session"></a>Éléments d’une Session

Chaque session contient des groupes d’éléments qui appliquent des règles de la mutabilité et la sécurité, qui varient selon l’élément de session, ainsi que des informations de ménage en lecture seule (métadonnées). Cette section décrit les groupes d’éléments de session inclus dans les fichiers JSON pour configurer votre session et dans le fichier JSON pour le modèle que vous choisissez.

| Remarque                                                                                                                                                   |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Si vous utilisez des wrappers de WinRT pour une implémentation de HTTP/REST, votre session et le modèle doivent définir des objets JSON qui reflètent précisément les fonctionnalités de WinRT. |

À l’intérieur de chacun des groupes d’éléments, il existe deux objets internes :

-   Les objets système : ces objets ont un schéma fixe qui est appliqué et interprété par MPSD. Elles sont validées et fusionnées. Dans la mesure où MPSD définit et sait que leur signification, il peut agir sur eux. Pour la définition complète de chacun des objets système, consultez les références pour le **MultiplayerSession classe** et les références pour le **Session Directory URI**.
-   Les objets personnalisés : ces objets sont facultatifs et ont pas de schéma. Ils sont utilisés pour stocker les métadonnées concernant un jeu multijoueur. Étant donné que MPSD ne peut pas interpréter ces données, il n’est pas traité. Données de jeu ou des informations enregistrées doivent être stockées dans le stockage géré par le titre (TMS). Pour plus d’informations sur les mémoires de traduction, consultez **Xbox Live titre stockage**.

Voici un exemple d’un objet JSON personnalisé :
```JSON
    "custom": {
      "myField1": true,
      "myField2": "string",
      "myField3": 5.5,
      "myField4": { "myObject": null },
      "myField5": [ "my", "array" ]
    }
```

#### <a name="session-constants"></a>Constantes de session

Constantes de session sont définies uniquement lors de la création, par le créateur ou par le modèle de session. L’objet /constants/system est utilisé pour définir des constantes pour le système en mode multijoueur tel qu’il est connu par le biais du MPSD. Le wrapper WinRT associé à cet objet est représenté par le **MultiplayerSessionConstants classe**.

L’objet /constants/system peut définir un nombre d’éléments, y compris un fonctionnalités objet, un objet de mesures, un managedInitialization (version de contrat de modèle 104/105) ou memberInitialization (version de contrat 107), un peerToPeerRequirements objet, un objet peerToHostRequirements et un objet measurementsServerAddresses.


#### <a name="session-properties"></a>Propriétés de la session

Utiliser l’objet /properties/system pour définir des propriétés de session pour MPSD. Le wrapper WinRT associé à cet objet est la **MultiplayerSessionProperties classe**. Propriétés de la session sont accessibles en écriture par les membres de la session à tout moment. Exemples de propriétés de session au format JSON sont : joinRestriction, initializationSucceeded et l’objet matchmaking. Pour obtenir un exemple d’utilisation de ce groupe d’éléments, consultez [initialisation de la Session cible et QoS](smartmatch-matchmaking.md).


#### <a name="member-constants"></a>Constantes de membre

Définissez le membre des constantes au moment de la jointure pour chaque membre de la session. L’objet JSON est /members/ {index} / constantes/système. La classe de wrapper WinRT représentant un membre de la session est la **MultiplayerSessionMember classe**.


## <a name="member-properties"></a>Propriétés de membre

Propriétés de membre sont accessibles en écriture uniquement par un membre de la session. Ils sont définis dans l’objet /members/ {index} / propriétés/système et les éléments de la **MultiplayerSessionMember classe**. Exemple :

```
    {
      // These flags control the member status and "activeTitle", and are mutually exclusive (it's an error to set both to true).
      // For each, false is the same as not present. The default status is "inactive", i.e. neither present.
      "ready": true,
      "active": false,

      // Base-64 blob, or not present. Empty string is the same as not present.
      "secureDeviceAddress": "ryY=",

      // During member initialization, if any members in the list fail, this member will also fail.
      // Can't be set on large sessions.
      "initializationGroup": [ 5 ],

      // List of the groups I'm in and the encounters I just had.
      // An encounter is a brief interaction with a group. When an encounter is reported, it counts as retroactively joining the group 30 seconds ago and just now leaving.
      // Group names use the session name validation rules (e.g. case-insensitive).
      // On large sessions, groups are used to report who played with who (rather than just session membership). Members
      // that are active in at least one group together at the same time are counted as playing together.
      // Empty lists are the same as no value specified.
      // The set of encounters is a point-in-time property, so it's immediately consumed and will never appear on a response.
      "groups": [ "team-buzz", "posse.99" ],
      "encounters": [ "CoffeeShop-757093D8-E41F-49D0-BB13-17A49B20C6B9" ],

      // Optional list of role preferences the player has specified for role-based game modes.
      // All role names have to match across all members in the session. Role weights are
      // defined from 0-100.
      "RolePreference": { "medic": 75, "sniper": 25, "assault": 50, "support": 100 },

      // QoS measurements by lower-case device token.
      // Like all fields, "measurements" must be updated as a whole. It should be set once when measurement is complete, not incrementally.
      // Metrics can be omitted if they weren't successfully measured, i.e. the peer is unreachable.
      // If a "measurements" object is set, it can't contain an entry for the member's own address.
      "measurements": {
        "e69c43a8": {
          "bandwidthDown": 19342,  // Kilobits per second
          "bandwidthUp": 944,  // Kilobits per second
          "custom": { }
        }

      // QoS measurements by game-server connection string. Like all fields, "serverMeasurements" must be updated as a whole, so it should be set once when measurement is complete.
      // If empty, it means that none of the measurements completed within the "serverMeasurementTimeout".
      "serverMeasurements": {
        "server farm a": {
          "latency": 233  // Milliseconds
        }
      },

      // Subscriptions for shoulder taps on session changes. The "profile" indicates which session changes to tap as well as other properties of the registration like the min time between taps.
      // The subscription is named with a title-generated GUID that is also sent back with the tap as a context ID.
      // Subscriptions can be added and removed individually, without affecting other subscriptions in the "subscriptions" object.
      // To remove a subscription, set its context ID to null.
      // (Like the "ready" and "active" flags, the "subscriptions" data is copied out and maintained internally, so the normal replace-all rule on system fields doesn't apply to "subscriptions".)
      // Can't be set on large sessions.
      "subscriptions": {
        "961dc162-3a8c-4982-b58b-0347ed086bc9": {
          "profile": "party",  // Or "matchmaking", "initialization", "roster", "queuehost", or "queue"
          "onBehalfOfTitleId": "3948320593",  // Optional decimal title ID of the registered channel.  If not set the title ID is taken from the token.
        },
        "709fef70-4638-4b94-905b-24cb02706eb5": null
      }
    }
```

#### <a name="server-elements"></a>Éléments de serveur

Les serveurs sont non-les utilisateurs ayant rejoint ou été invités dans une session. Les objets JSON associés sont /servers/ {nom-serveur} / constantes/système et /servers/ {nom-serveur} / Propriétés/système. Ces objets sont accessibles en écriture uniquement par les serveurs.

| Remarque                                                         |
|---------------------------------------------------------------------------|
| L’objet de système de /servers/ {nom-serveur} / constantes / n’est pas actuellement utilisé. |


### <a name="session-configuration"></a>Configuration de session

Il existe deux façons de contrôler la configuration de sessions :

-   Utiliser des modèles de session reçues par le biais XDP ou partenaires.
-   Utiliser des appels pour le mode multijoueur et matchmaking WinRT APIs ou les API REST. Vous devez toujours utiliser un modèle, mais le modèle n’a pas à contenir les valeurs que vous souhaitez configurer. Notez que le titre ne peut pas remplacer les constantes déjà définies dans le modèle.

Un document JSON distinct est fourni pour définir la session elle-même. En outre, le développeur doit implémenter les fonctionnalités du wrapper WinRT requise pour un nom particulier. Le contenu des documents JSON et tout code de wrapper WinRT doit refléter les uns des autres avec précision et doit refléter la dernière version de contrat de modèle.

Le schéma pour une session est créée avec la version de session (version principale) et la révision du protocole (version mineure). Les versions sont combinées dans l’en-tête X-Xbl--Version de contrat en tant que « 100 \* majeure + mineure ». Par exemple, un titre de la version 1.7 inclut l’en-tête suivant sur chaque demande REST, en supposant que la dernière version de contrat de modèle de 107 : X-Xbl--Version de contrat : 107.

| Remarque                                                                                              |
|----------------------------------------------------------------------------------------------------------------|
| Il est recommandé pour la plupart des titres (à l’aide de XSAPI) à utiliser la version de contrat 105 et la version de modèle de session 107. |


### <a name="session-templates"></a>Modèles de session

Chaque modèle de session est un document JSON, la partie de la configuration de service, qui définit l’infrastructure pour la session en cours de création et fournit des constantes pour la nouvelle session. Pour plus d’informations, consultez [MPSD Session modèles](../service-configuration/session-templates.md).

## <a name="session-capabilities"></a>Fonctionnalités de la session

Fonctionnalités sont des constantes dans la session MPSD configurer le comportement de la MPSD devant s’appliquer à cette session. Vous utilisez couramment XDP et partenaires pour définir des fonctionnalités dans le modèle de session. Elles sont définies dans l’objet /constants/system/capabilities. Si aucune capacité n’est nécessaires, vous pouvez utiliser un objet vide de capacités.

| Remarque                                                                                                       |
|-------------------------------------------------------------------------------------------------------------------------|
| Titres presque jamais modifient ou accéder aux fonctionnalités de session à l’aide de l’API WinRT multijoueurs ou le matchmaking les API WinRT. |

Fonctionnalités de la session sont représentées par le **MultiplayerSessionCapabilities classe**. Elles sont des valeurs booléennes qui indiquent que la session peut prendre en charge :

-   Connectivité
-   Jeu
-   Grande taille
-   Connexion requise pour les membres actifs

Le **MultiplayerSessionConstants classe** définit les propriétés suivantes qui concernent les fonctionnalités de la session :

-   **CapabilitiesConnectivity**
-   **CapabilitiesGameplay**
-   **CapabilitiesLarge**

| Remarque                                                                                                   |
|---------------------------------------------------------------------------------------------------------------------|
| Si le titre définit une fonctionnalité de session dynamique, la propriété correspondante est définie true pour les constantes de la session. |

## <a name="session-size"></a>Taille de la session

La taille d’une session MPSD est déterminée par le nombre de membres dans cette session.


### <a name="maximum-session-size"></a>Taille maximale de Session

La taille maximale d’une session est le nombre maximal de membres de session que peut accueillir. Il est représenté par le **MultiplayerSessionConstants.MaxMembersInSession propriété**. La taille maximale de membre est définie dans l’objet /constants/system.

La taille maximale de la session est entre 1 et 100 membres de session, et les valeurs par défaut à 100 si pas définie sur la création. Si la taille requise est supérieures à 100, la session est appelée une session « large » et a la valeur d’une façon particulière.

Définition d’une taille maximale pour une session peut entraîner un logement apparaisse comme complète pendant certaines déconnecter scénarios. Par exemple, si un lecteur est déconnecté en raison d’un réseau ou alimentation, le délai n'est pas répercutée immédiatement dans la session. Le membre est défini comme étant inactif à l’aide de la fonctionnalité de détection de déconnexion décrite dans [déconnecter de détection et de gestion de Notification de modification MPSD](multiplayer-session-directory.md).

En comparaison, un maillage pair à pair qui utilise une pulsation pour détecter une déconnexion connaît souvent une déconnexion dans les deux à trois secondes et pouvez ouvrir l’emplacement player immédiatement. Toutefois, l’arbitre ne peut pas supprimer d’autres membres.

### <a name="large-sessions"></a>Sessions de grande taille

Une session MPSD volumineuse peut avoir jusqu'à 1 000 membres, mais il a certaines fonctionnalités de session désactivées, telles que l’obtention d’une liste de tous les membres. Largeness de session est représenté par le **MultiplayerSessionCapabilities.Large propriété**. Cette propriété est définie sur true pour indiquer une session de grande taille, et la fonctionnalité « large » est indiquée dans l’objet /constants/system/capabilities. 

## <a name="session-user-states"></a>États de session utilisateur



MPSD définit un état de l’utilisateur en tant que l’état d’un utilisateur qui a été ajouté à une session. Les états possibles sont définis par le **MultiplayerSessionStatus énumération**. L’utilisateur est également considéré comme ayant un état de « disponible » avant d’être ajouté à une session.

Le **MultiplayerSession.SetCurrentUserStatus méthode** peut être utilisé pour modifier l’état d’utilisateur de session. Cette modification est possible pour REST par le paramètre /members/ {index} / propriétés/système correctement dans le document JSON de session de jeu.


### <a name="reserved-user-state"></a>État d’utilisateur réservé

L’utilisateur est placé dans l’état d’utilisateur réservés lors de l’arbitre a sélectionné l’utilisateur de remplissage d’un des emplacements ouvertes dans la session. Dans cet état, l’utilisateur a ne sont pas encore officiellement accepté l’invitation à la session ou rejoint la session pour commencer à se connecter avec leurs homologues.


### <a name="active-user-state"></a>Active l’état utilisateur

Lorsqu’un utilisateur est dans un état actif, le titre a rejoint la session pour le compte de l’utilisateur, et l’utilisateur participe activement à la session. L’utilisateur continue dans cet état tant qu’il est de jouer.

Une fois un titre est lancé tout d’abord, elle doit vérifier pour voir si l’utilisateur est déjà membre de toutes les sessions, généralement en vérifiant l’état de session. Si l’utilisateur est un membre de la session, le titre peut plonger directement dans le jeu, définissez tous les membres locaux qui participent à l’état de l’utilisateur actif.

Un utilisateur doit rester dans un état actif pendant la lecture dans la session. Si un utilisateur quitte la session via l’interface utilisateur dans le jeu, l’utilisateur doit être supprimé à partir de la session avec le **MultiplayerSession.Leave méthode**. Si l’utilisateur est uniquement temporairement absent du jeu, comme lorsque le titre est limité, le titre doit garder l’utilisateur dans l’état actif pour une période de temps raisonnable. Il est nécessaire pour modifier l’état utilisateur sur Inactive si l’utilisateur n’a pas renvoyé après une période titre spécifié.


### <a name="inactive-user-state"></a>État utilisateur inactif

Dans l’état inactif, l’utilisateur n’est pas actuellement engagée avec le jeu, mais dispose toujours d’un emplacement enregistré dans la session. En d’autres termes, l’utilisateur est « pas actif ».

Il est la console de l’utilisateur qui a la responsabilité de la définition de cet état d’un utilisateur à inactif dans la session. L’arbitre ne peut pas effectuer cette action. Exemples de scénarios dans lesquels un utilisateur est placé dans l’état inactif sont les suivantes :

-   Le titre reçoit un événement de suspension en cours.
-   L’utilisateur a été inactif (aucune réponse d’entrée ou contrôleur) pour une période de temps définie par le titre. Nous recommandons deux minutes pour une mode multijoueur compétitive.
-   Le titre a été en mode de contrainte pendant plus de deux minutes, ou pour une période définie par le titre. Ce délai d’attente en mode de contrainte est le montant attendu de la période pendant laquelle un utilisateur peut être en dehors du titre à l’aide d’une application ou autres expériences en lien avec le titre.
-   L’utilisateur a été déconnecté de manière incorrecte à partir de la session. Consultez [MPSD modifier la gestion des notifications et détection de la déconnexion](multiplayer-session-directory.md).

Si le titre commence et l’état de l’utilisateur pour un membre de session particulière est défini sur inactif, le titre a été suspendu ou l’utilisateur est restée inactive pendant trop longtemps dans la session. Le titre est lancé à nouveau, l’indication étant donné que l’utilisateur souhaite poursuivre la session de jeu auquel il appartient. Si l’état de l’utilisateur est actif lors du lancement de titre, cette situation est probablement en raison d’une déconnexion réseau ou un autre scénario où le titre n’a pas pu définir l’utilisateur inactif avant de s’interrompre. Dans ces deux cas, votre titre doit tenter de reconnecter l’utilisateur avec le jeu et les autres utilisateurs pour continuer la lecture, ou le supprimer à partir de la session.

### <a name="user-state-when-the-session-is-over"></a>État lors de la Session utilisateur est sur

Lorsque la session est terminée, le jeu est interrompu. Le titre doit autoriser tous les utilisateurs à se désabonner à l’aide de la **MultiplayerSession.Leave méthode**. Les activités de session associées aux utilisateurs sont automatiquement effacées lorsqu’ils quittent la session.

## <a name="visibility-and-joinability"></a>Visibilité et Joinability

Accès à la session est contrôlé au niveau du MPSD par deux paramètres : joinability de session et de visibilité de la session. Les recommandations de visibilité et joinability dans cette rubrique s’appliquent pour les scénarios courants de titre. Titres doivent suivre ces paramètres, si possible, et ils doivent utiliser une logique de titre pour effectuer cette détermination finale, faisant autoritée concernant indique si un nouveau joueur est admis dans une session.


### <a name="session-visibility"></a>Visibilité de la session

Visibilité de la session est représentée par une constante qui est définie lors de la création de session. Il est généralement défini dans le modèle de session et détermine les types d’utilisateurs ont lu et accès en écriture à une session. Les valeurs possibles pour la visibilité de la session sont définies par le **MultiplayerSessionVisibility énumération**. Les paramètres autorisés pour la constante de visibilité dans un fichier JSON sont ouverts, visibles et privé.


#### <a name="recommended-game-session-visibility-open"></a>Visibilité de la session de jeu de recommandé : ouvrir

Les sessions ouvertes ne nécessitent pas de réservations de lecteur, ce qui simplifie le processus d’invitation. L’arbitre ne réserve pas de lecteurs dans MPSD après une invitation a été envoyée, mais uniquement les pistes invité joueurs localement. Par conséquent, les joueurs peuvent immédiatement se connecter à l’arbitre et déterminer si elles doivent rejoindre une session, sont rejetés ou doivent attendre (si les lecteurs en attente sont pris en charge). L’arbitre est l’autorité ultime et répond et indique le nouveau membre soit rester dans ou laissez la session.

À l’aide de la visibilité de l’ouverture de session jeu nécessite le joueur invité lancer un titre et de se connecter à l’arbitre avant la décision finale a été effectuée. Il est acceptable pour afficher un message d’erreur à l’utilisateur si une session est saturée ou si une invitation a été rejetée.

Pour établir une connexion à l’arbitre, une adresse de périphérique sécurisé est requise. Le **MultiplayerSessionProperties.HostDeviceToken propriété** est utilisé pour déterminer quel membre de la session est l’arbitre en cours d’une session et sécuriser les adresse de l’appareil un lecteur invité doit utiliser pour la connexion.

### <a name="session-joinability"></a>Joinability de session

Joinability de session détermine quels types d’utilisateurs peuvent rejoindre une session. Elle peut être définie de manière dynamique pendant une session. Les valeurs possibles pour joinability de session sont :

-   None (valeur par défaut), il n’existe aucune restriction sur joindre à la session.
-   Locales, uniquement les utilisateurs locaux peuvent rejoindre la session.
-   Suivi--uniquement les utilisateurs locaux et les utilisateurs qui sont suivies par d’autres membres de la session peuvent rejoindre la session sans réservation.

Un arbitre session peut créer une session privée via le paramètre joinability. Joinability local ou de suivi restreint l’accès à la session et rend privé.

En outre, l’arbitre doit effectuer le suivi de session joinability afin que l’invite de session plus anciennes pouvant être rejetées au niveau de l’hôte si nécessaire. Par exemple, si des lecteurs sont invités ne sont pas arrivés pour rejoindre une session jusqu'à ce que la session est déjà complète, l’arbitre peut demander aux acteurs de jointure que la session a été verrouillée et dont ils ont besoin quitter la session automatiquement.

## <a name="session-timeouts"></a>Délais d’expiration de session

Sessions peut être modifiées par les minuteurs et autres événements externes. Délais d’expiration de session de définir les périodes pendant lequel les membres de la session peuvent rester dans des États spécifiques avant d’être automatiquement désactivés ou supprimés de la session. MPSD prend également en charge les délais d’attente pour gérer la durée de vie de session.

| Remarque                                                                                                                                                                                                                                                           |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Paramètres de délai d’attente sont faites dans /constants/system/timeouts, ou au sein de l’objet de l’initialisation managée, pour la version du modèle de contrat 104/105. 107 ou version ultérieure, les paramètres sont faites individuellement dans/constantes/système ou au sein de l’objet de l’initialisation managée. |

Quand un minuteur expire, MPSD ne pas automatiquement mise à jour de la session et informer l’arbitre à cet instant de toutes les modifications. Les États de session et de délai d’expiration sont uniquement mis à jour immédiatement avant une lecture ou écriture demande est envoyée. Mise à jour immédiate permet de s’assurer que les données retournées sont les plus récentes.

| Remarque                                                                                                          |
|----------------------------------------------------------------------------------------------------------------------------|
| Délais d’expiration de session ne sont pas empilés, et qu’un seul est appliqué pour une transition d’état par rapport à chaque membre de la session sur une mise à jour. |


### <a name="currently-defined-timeouts"></a>Délais d’attente actuellement définies

Cette section décrit les délais d’attente qui sont actuellement définies par MPSD. Tous les délais d’expiration sont spécifiés en millisecondes. La valeur 0 est autorisée et indique un délai d’expiration immédiat. Un délai d’expiration avec aucune valeur n’est considéré comme infini. Étant donné que les délais d’expiration ont des valeurs par défaut, vous devez explicitement spécifier null pour un délai d’attente infini.
#### <a name="evaluationtimeout"></a>evaluationTimeout

Ce délai d’attente indique la quantité de temps pour un membre de la session à effectuer et de charger la décision d’évaluation. Si aucune décision n’est reçue, la décision est comptabilisé comme un échec. Ce délai d’attente est placé dans l’objet de l’initialisation managée.


#### <a name="inactiveremovaltimeout"></a>inactiveRemovalTimeout

Ce délai est défini pour un membre de la session qui a joint une session, mais n’est pas actuellement engagé dans le jeu. Le membre est supprimé de la session après 2 heures, par défaut.

| Remarque                                                                      |
|----------------------------------------------------------------------------------------|
| Ce délai d’attente est désigné le délai d’attente inactive pour la version du modèle de contrat 104/105. |

Dans de nombreux cas, nous vous recommandons de définir le délai d’attente inactive sur 0, ainsi que n’importe quel utilisateur qui est défini sur l’état inactif pour être supprimée immédiatement à partir de la session et l’emplacement correspondant à effacer. Ce comportement est souhaitable pour les jeux multijoueurs plus compétitifs afin que, si un utilisateur a été inactive ou atteint un état inactif, un nouveau lecteur peut être ajouté rapidement. Pour les co-op ou autres conceptions multijoueurs, vous souhaiterez votre titre pour permettre aux utilisateurs plus de temps à se reconnecter s’ils sont déconnectés ou pas engagés dans le titre de périodes de temps. Notez qu’aucune solution unique adaptée à tous les scénarios de conception.

#### <a name="jointimeout"></a>joinTimeout

Ce délai d’attente indique le nombre de millisecondes pendant lesquelles un utilisateur peut se joindre à la session. Réservations d’utilisateurs qui ne parviennent pas à joindre sont supprimées. Ce délai d’attente est placé dans l’objet de l’initialisation managée.


#### <a name="measurementtimeout"></a>measurementTimeout

Ce délai d’attente indique la durée pendant laquelle qu'un membre de la session a pour charger des mesures. Un membre qui ne parvient pas à télécharger des mesures est marqué avec un motif de l’échec de « timeout ». Ce délai d’attente est placé dans l’objet de l’initialisation managée.

| Remarque                                                                                                                                                                              |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Au cours de matchmaking, un délai d’expiration de 45 secondes pour les mesures de qualité de service est appliquée. Par conséquent, nous recommandons l’utilisation d’un délai d’expiration de mesure est inférieure ou égale à 30 secondes au cours de matchmaking. |


#### <a name="readyremovaltimeout"></a>readyRemovalTimeout

Ce délai est défini pour un membre de la session qui a rejoint la session et essaie d’obtenir dans le jeu. Cela signifie généralement que l’interpréteur de commandes a rejoint l’utilisateur pour le compte le titre et le titre est lancé. Le membre est supprimé de la session et placé dans un état inactif après 3 minutes, par défaut.

| Remarque                                                          |
|----------------------------------------------------------------------------|
| Ce délai d’attente est désigné le délai d’attente prête pour la version de contrat 104/105. |


#### <a name="reservedremovaltimeout"></a>reservedRemovalTimeout

Ce délai est défini pour un membre de la session qui a été ajouté à la session par quelqu'un d’autre, mais n’a pas encore rejoint la session. La réservation est supprimée et le membre est considérée comme inactif lorsque le délai d’attente expire. La valeur par défaut correspond à 30 secondes.

| Remarque                                                             |
|-------------------------------------------------------------------------------|
| Ce délai d’attente est désigné le délai d’attente réservée pour la version de contrat 104/105. |


#### <a name="sessionemptytimeout"></a>sessionEmptyTimeout

Ce délai d’attente indique le nombre de millisecondes après qu’une session devienne vide lorsqu’il est supprimé. La valeur par défaut est 0.

| Remarque                                                                 |
|-----------------------------------------------------------------------------------|
| Ce délai d’attente est désigné le délai d’attente de sessionEmpty pour la version de contrat 104/105. |


### <a name="session-timeout-example"></a>Exemple de délai d’expiration de session

1.  Une session est démarrée à quatre.
2.  Deux joueurs, A et B, sont déconnectées en raison d’une panne de courant. Leur état dans le jeu reste actif.
3.  Les deux autres acteurs, C et D, quittez correctement à l’aide de la **MultiplayerSession.Leave méthode**.
4.  La session reste ouverte, avec des lecteurs A et B déconnecté, mais toujours dans un état actif.
5.  Quelques jours plus tard, le lecteur A retourne et commence à jouer.
6.  Une suite de joueurs recherche les sessions A est membre du (effectue une lecture) et recherche la session orpheline à partir d’il y a quelques jours.
7.  La session effectue une vérification de la présence sur les deux joueurs qui se trouvent toujours dans la session (A et B).
    1.  Étant donné que le lecteur A est en cours d’exécution le titre, réussit la vérification de la présence sur le lecteur A, et état actif du joueur dans la correspondance reste la même.
    2.  Le titre, le lecteur B est inactif. Par conséquent, la vérification de la présence pour le lecteur B échoue et le service définit état de l’acteur B inactif. À ce stade, le délai d’attente inactif démarre b de lecteur.

8.  Le lecteur A termine la session correctement à l’aide de la **laisser** (méthode).
9.  Le délai d’attente inactive expire pour le lecteur B, qui est supprimé de la session sur la prochaine opération de lecture ou écriture par tout le monde.
10. La session a zéro membres maintenant et est supprimée à partir du service.

Si le délai d’attente inactive pour l’exemple de session est définie sur 0, le lecteur B expire immédiatement après la vérification de la présence à l’étape 7 a et est probablement supprimées par l’écriture de la session. Dans ce cas, la fermeture de session sans avoir besoin d’un autre lire ou écrire dans la session.


## <a name="multiple-signed-in-users-on-a-single-console"></a>Plusieurs utilisateurs connectés sur une Console unique


Lorsque plusieurs utilisateurs sont connecté à la même console, il est possible que certains utilisateurs à se trouver dans une session de jeu, tandis que les autres utilisateurs ne sont pas dans la session ou ne sont pas active dans le titre actuel. Invitations jeu peuvent également être reçues et acceptées pour plusieurs utilisateurs, ayant une incidence sur l’appartenance au jeu de session. Un titre doit prendre en compte ces points pour être en mesure de gérer tous les scénarios de l’appartenance de session correctement.

Dans un scénario courant, un nouveau joueur se connecte, devienne actif dans le jeu et doit être ajouté à une session de jeu existante. Comme avec la création d’une nouvelle session de jeu, un titre doit uniquement ajouter un utilisateur quand il convient au cours du jeu.

Avec plusieurs utilisateurs connectés, un ou plusieurs utilisateurs peuvent également recevoir des invitations à une autre session de jeu. Titres n’avez pas besoin de gérer ces scénarios en aucune façon spécifique. Événements d’état et les membres de session notifient le titre de mises à jour de l’appartenance de session et d’utilisateurs jeu.

Pour gérer plusieurs utilisateurs connectés à une session en ligne, le titre s’abonne pour épaule appuie sur pour tous les utilisateurs, à l’aide d’un distinct **XboxLiveContext classe** objet pour chaque utilisateur. Le titre utilise le **MultiplayerSession.ChangeNumber propriété** pour déterminer les modifications particuliers dans la session et ignorer l’épaule en double appuie.


## <a name="process-lifecycle-management"></a>Gestion du cycle de vie des processus


Tout comme un titre non multijoueurs, un titre dans une session multijoueur peut rencontrer suspension du titre et l’arrêt du traitement des événements de cycle de vie. L’arbitre de session doit donc enregistrer l’état de session régulièrement. Dans le cas où l’arbitre est suspendue, le titre doit tenter de migration de l’arbitre, enregistrer l’état du jeu selon les besoins, afin qu’un arbitre nouvelle peut restaurer l’état de session. Il est alors possible d’une session multijoueur complète d’être suspendu et repris plus tard, tant que la session est toujours valide dans MPSD. Seul homologue désigné, généralement l’hôte du jeu, doit mettre à jour l’état global du jeu.


### <a name="storage-of-game-metadata"></a>Stockage des métadonnées de jeu

Un titre stocke les métadonnées de jeu dans la session MPSD. Métadonnées de jeu sont les informations nécessaires pour afficher des données de session et activer le titre Rechercher et rejoindre la session de jeu. Le titre stocke les métadonnées spécifiques au lecteur dans la section des propriétés personnalisées pour le membre de session, par exemple, couleur du lecteur, arme de lecteur multimédia par défaut pour la session, etc. Métadonnées de niveau session, par exemple, mappage actuel, sont stockée dans la section des propriétés personnalisées global de la session MPSD.


### <a name="storage-of-game-state"></a>Stockage de l’état de jeu

État du jeu est stocké dans le stockage géré par le titre (TMS), à l’aide de la **service de stockage du titre**. Stockage à l’aide de cet emplacement permet un titre migrer l’arbitre sans problèmes d’autorisation. Consultez [migration un arbitre](migrating-an-arbiter.md).

| Remarque                                                                                                               |
|---------------------------------------------------------------------------------------------------------------------------------|
| Le titre ne doit pas tentez d’enregistrer état du jeu de mémoires de traduction plus fréquemment que toutes les 5 minutes, sauf si elle est suspendue. |

## <a name="cleanup-of-inactive-sessions"></a>Nettoyage des Sessions inactives

Si la sessionEmptyTimeout est définie sur 0, une session MPSD est automatiquement supprimée lorsque le dernier joueur quitte la session. Pour savoir comment empêcher une session inutilisées de contenant des joueurs après incident ou de déconnexion, consultez [déconnecter de détection et de gestion de Notification de modification MPSD.](multiplayer-session-directory.md). Un traitement incorrect des sessions inutilisés après incident ou déconnectez peut entraîner des problèmes lors de l’un titre est interrogation des sessions pour un lecteur.

La méthode recommandée pour nettoyer les sessions inactives est d’avoir à la requête de titre toutes les sessions pour un utilisateur particulier en appelant le **MultiplayerService.GetSessionsAsync méthode** , puis d’évaluer les sessions. Lorsqu’il rencontre une session obsolète, le titre appelle le **MultiplayerSession.Leave méthode** de tous les joueurs locales dans la session. Cet appel supprime le nombre de membres à 0 par la suite et nettoie les sessions.

## <a name="session-arbiter"></a>Session arbitre


Certaines méthodes multijoueurs doivent uniquement être appelées par un client au sein d’une session de jeu. Ce client est une des consoles participant à la session, appelé l’arbitre, ou l’hôte. Si au moins un membre de la session est dans un jeu, la session doit avoir un arbitre pour surveiller les jointures en cours d’exécution.


### <a name="setting-the-arbiter"></a>Définition de l’arbitre

Lorsqu’il crée une session, le client désigne une seule console en tant que l’arbitre. Consultez [Comment : Définir un arbitre pour une Session MPSD](multiplayer-how-tos.md).


### <a name="saving-session-state"></a>Enregistrer l’état de Session

Comme indiqué dans **gestion du cycle de vie des processus**, l’arbitre doit enregistrer régulièrement l’état de session. Un arbitre nouveau doit être en mesure de restaurer l’état de session dans le cas de migration de l’arbitre par le titre. Pour plus d’informations, consultez [migration un arbitre](multiplayer-how-tos.md).


### <a name="managing-game-session-members-and-joins-in-progress"></a>La gestion des membres de la Session de jeu et de jointures en cours

Le rôle le plus important de l’arbitre session consiste à gérer les utilisateurs arrivant dans la session de jeu pour jouer. Cela inclut la gestion des jeux d’invitations, notifier les joueurs en attente et l’utilisation de joueurs, quittez le jeu.


#### <a name="receiving-notifications"></a>Réception de Notifications

L’arbitre doit écouter les nouveaux lecteurs qui souhaitent rejoindre la session de jeu avec le **RealTimeActivityService.MultiplayerSessionChanged événement**.


#### <a name="finding-players-to-fill-empty-game-session-slots"></a>Recherche de joueurs pour remplir les emplacements de la Session de jeu vide

L’arbitre recherche joueurs pour remplir les emplacements de la session de jeu vide à une de ces opérations :   Si votre titre utilise une session d’introduction ou un autre mécanisme pour permettre des jointures retardées, rechercher des membres de nouvelle session à l’aide de ce mécanisme.
-   Créer une autre session de ticket de correspondance.

Voir aussi [Comment : Remplir les emplacements de la Session ouverte pendant Matchmaking](multiplayer-how-tos.md).


#### <a name="handling-invited-session-members"></a>Gestion des invités des membres de la Session

L’arbitre doit surveiller les membres de la session invité et appliquer un intervalle minimum entre les invitations à un seul utilisateur. Voir aussi [Comment : Envoyer des invitations jeu](multiplayer-how-tos.md).
