---
title: Annuaire de sessions multijoueur
description: En savoir plus sur le répertoire de Session en mode multijoueur en temps réel de Xbox (MPSD).
ms.assetid: a9b029ea-4cc1-485a-8253-e1c74184f35e
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, mpsd, annuaire de sessions multijoueurs.
ms.localizationpriority: medium
ms.openlocfilehash: 1681fe59533ebaf0db197efb95ca46b828846f5d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662544"
---
# <a name="multiplayer-session-directory-mpsd"></a>Multiplayer Session Directory (MPSD)

Cet article comprend les sections suivantes :

* Vue d’ensemble MPSD
* MPSD modifier la gestion des notifications et détection de la déconnexion
* Handles MPSD aux Sessions
* Synchronisation des mises à jour de la Session
* Appel MPSD
* Modèles de Session MPSD
* Explorateur de Session multijoueurs

## <a name="session-overview"></a>Vue d’ensemble de la session

### <a name="what-is-mpsd"></a>Qu’est MPSD ?

Annuaire de sessions multijoueur (MPSD) est un service d’exploitation dans le cloud Xbox Live qui centralise les métadonnées du système en mode multijoueur d’un jeu sur plusieurs clients. Elle est encapsulée par la **MultiplayerService classe**.

MPSD permet de titres de partager les informations de base nécessaires pour se connecter à un groupe d’utilisateurs. Elle garantit que les fonctionnalités de la session sont synchronisé et cohérente. Il coordonne avec le système d’exploitation shell et de la console de l’envoi/acceptation d’invitations et jointe par le biais de la carte de joueur.


### <a name="mpsd-sessions"></a>Sessions MPSD

Une session MPSD est représentée par le **MultiplayerSession classe** en tant que le scénario dans lequel une ou plusieurs personnes utilisent un jeu. Une session est stockée par MPSD comme un document JSON sécurisé résidant dans le cloud de Xbox Live. Plus précisément, une session MPSD présente les caractéristiques suivantes :   Est créé et géré par les titres.

-   Possède un URI unique. Pour plus d’informations, consultez **Session Directory URI**.
-   Permet la connectivité entre les utilisateurs, appelés membres de la session.
-   Stocke les données utiles pour l’activation de jeu lire, par exemple, les attributs d’un membre, paramètres de jeu, informations et des informations de serveur de jeu d’amorçage.

MPSD prend en charge plusieurs variantes de session pour une utilisation dans la configuration des jeux multijoueurs. Chaque session contient les identificateurs d’utilisateurs joueurs Xbox (XUIDs) et sécuriser les données d’adresse associations d’appareil.

-   Session de jeu, utilisée comme modèle pour le jeu. Une session de jeu peut être peer-to-peer, hôte de l’homologue, homologue-serveur ou un mélange de ces types.
-   Session de ticket, une session d’assistance utilisée pour suivre l’état d’une correspondance pendant matchmaking. Il est souvent également une session d’introduction et peut être parfois une session de jeu. Consultez [SmartMatch Matchmaking](smartmatch-matchmaking.md).
-   Session cible, une session d’assistance créée au cours de matchmaking pour représenter le jeu de mise en correspondance. Il est presque toujours également une session de jeu. Consultez [SmartMatch Matchmaking](smartmatch-matchmaking.md).
-   Session d’introduction, une session d’assistance utilisée pour prendre en compte les joueurs invités qui sont en attente pour rejoindre une session de jeu. Plusieurs titres de créent une session d’introduction et une session de jeu. Pour plus d’informations, consultez **joueurs de gestion dans votre titre**.

## <a name="mpsd-change-notification-handling-and-disconnect-detection"></a>MPSD modifier la gestion des notifications et détection de la déconnexion

MPSD permet aux clients pour se connecter à l’aide du socket web du service activité en temps réel. La connexion est utilisée pour :

-   Envoi de notifications brèves (tap épaule) en cas de modification de la session, selon les abonnements aux événements qui titres initier.
-   Détecter les déconnexions de l’utilisateur.
-   Définir des utilisateurs comme inactifs et les supprimer par la suite de la session, en fonction à la détection de déconnexion.


### <a name="making-user-connections"></a>Établissement de connexions de l’utilisateur

La bibliothèque XSAPI gère la connexion entre le client et le MPSD. Le titre appelle d’abord la **MultiplayerService.EnableMultiplayerSubscriptions méthode**. Cette méthode indique à XSAPI que le client prévoit d’utiliser une connexion de l’activité en temps réel à des fins multijoueurs. Ensuite, lorsque le titre effectue son premier appel à la **MultiplayerService.WriteSessionAsync méthode** ou le **MultiplayerService.WriteSessionByHandleAsync méthode**, avec l’utilisateur actuel est défini sur actif état, une connexion est créée et raccordé au MPSD.

| Remarque                                                                                                                         |
|-------------------------------------------------------------------------------------------------------------------------------------------|
| Pour activer les notifications de session et détecter les déconnexions, que le modèle de session a pour définir le connectionRequiredForActiveMembers sur true. |

| Remarque                                                                                                                                                                                                                                                                                                                            |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Dans les versions précédentes de XSAPI, appelé titres le **RealTimeActivityService.ConnectAsync méthode** pour créer des connexions utilisateur au service de l’activité en temps réel. Pour le mode multijoueur 2015, cette méthode ne fait rien, et la connexion est créée à la demande. |

### <a name="subscribing-for-session-changes"></a>S’abonner aux modifications de la Session

MPSD utilise un « tap épaule » comme une notification légère que quelque chose de significatif a changé. Le titre doit récupérer la ressource modifiée afin de déterminer la nature exacte de la modification. Avec les abonnements est activées, le titre peut s’abonner à épaule appuie sur les modifications de session avec un appel à la **MultiplayerSession.SetSessionChangeSubscription méthode**. Consultez [Comment : S’abonner aux Notifications de modification de Session MPSD](multiplayer-how-tos.md).


### <a name="handling-shoulder-taps"></a>Gestion des épaule appuie sur

Lorsqu’une modification apportée à une session correspond à l’abonnement du titre pour cette session, MPSD notifie le titre de la modification à l’aide de la **RealTimeActivityService.MultiplayerSessionChanged événement**. Le titre doit ensuite récupérer la session et comparer la version extraite de la session avec la vue précédente mis en cache et prendre des mesures en conséquence.


### <a name="handling-notifications-of-connection-state-changes"></a>Gestion des Notifications des changements d’état de connexion

Le titre peut également être informé sur les modifications de l’intégrité de la connexion à MPSD. Deux événements signalent ces modifications : ** RealTimeActivityService.MultiplayerSubscriptionsLost événement **--déclenché en cas de perte de connexion du titre à MPSD à l’aide du service de l’activité en temps réel. Lorsque cet événement se produit, le titre doit arrêter le mode multijoueur.
-   ** Événement de RealTimeActivityService.ConnectionStateChanged **--déclenchées lors d’une modification temporaire de l’intégrité de connexion du titre pour le service de l’activité en temps réel. Le titre n’est pas nécessaire pour entreprendre aucune action lors de la réception de cet événement, mais il peut être utile d’utiliser l’événement pour faciliter le diagnostic.


### <a name="disconnecting-clients"></a>Déconnexion des Clients

Les clients pour votre titre déconnectent MPSD lorsque le titre désactive les notifications avec un appel à la **RealTimeActivityService.DisableMultiplayerSubscriptions méthode**. Peu après cet appel, le **MultiplayerSubscriptionsLost** se déclenche l’événement, indiquant qu’un client a déconnecté de MPSD.

| Remarque                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Dans les versions antérieures multijoueurs, les titres appelé le ** RealTimeActivityService.Disconnect méthode ** pour vous déconnecter du service de l’activité en temps réel. Pour le mode multijoueur 2015, cette méthode ne fait rien. La déconnexion se produit automatiquement après **DisableMultiplayerSubscriptions** est appelée, si aucun utilisateur de la connexion de socket web, par exemple, un abonnement au service activité en temps réel de présence. |


### <a name="disconnect-detection"></a>Détection de la déconnexion

MPSD utilise son déconnecter la fonctionnalité de détection pour rapidement déterminer à quel moment un utilisateur se déconnecte de manière incorrecte. Une déconnexion sans perte de données peut se produire en cas d’échec du réseau d’un joueur, ou en cas de défaillance d’un titre. MPSD change d’état de joueur déconnecté actif à inactif et notifie les autres membres de la session de la modification comme il convient, basé sur les abonnements à des membres à la session.

## <a name="mpsd-handles-to-sessions"></a>Handles MPSD aux Sessions

Un descripteur de session MPSD est une référence abstraite et immuable à une session qui peut également contenir des données typées supplémentaires. Il est similaire à un descripteur de fichier. Tous les handles ont un handle ID (GUID) et une référence de la session complète composée de la configuration du service ID (SCID), modèle de session et le nom de session. Un handle ne peut pas être mis à jour, mais il peut être créé, lire et supprimé.

| Remarque                                                                                                                                   |
|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| Un handle peut pointer vers une session qui n’existe pas. Création d’un handle à l’aide d’un nom qui n’existe pas de session n’entraîne pas une nouvelle session doit être créé. |


### <a name="handle-types"></a>Types de handle

Mode multijoueur 2015 prend en charge les types de handle décrits dans cette section.

#### <a name="invite-handle"></a>Inviter Handle

Un handle d’invitation représente une invitation (inviter) à une personne spécifique. Les données spécifiques au type incluent source personne, personne cible et chaîne de contexte qui décrit l’invitation, par exemple, un mode de jeu spécifique.

Un handle d’invitation accorde l’accès en lecture-écriture à une session ouverte. Si la session est fermée, le handle accorde l’accès de la session en lecture seule.

| Remarque                                                |
|------------------------------------------------------------------|
| MPSD peut créer une invitation même si la session est plein ou fermé. |


#### <a name="activity-handle"></a>Handle de l’activité

Un handle d’activité indique ce que fait un utilisateur pour le moment. Activité de l’utilisateur est représentée par le **MultiplayerActivityDetails classe**.

| Remarque                                                                                                                                                                                                                      |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Un handle de l’activité peut également être explicitement supprimé, mais uniquement par le titre de propriétaire pour un utilisateur spécifique. Cette suppression est effectuée à l’aide de la **MultiplayerService.ClearActivityAsync méthode**. |


### <a name="creating-an-invite-handle"></a>Création d’un Handle d’invitation

Pour créer un handle d’invitation, les appels de titre le **MultiplayerService.SendInvitesAsync méthode**. La méthode envoie une invitation pour les utilisateurs spécifiés sous la forme d’une interface utilisateur toast que les destinataires peuvent agir sur Accepter l’invitation.


### <a name="creating-an-activity-handle"></a>Création d’un Handle de l’activité

Pour créer un handle d’activité, les appels de titre le **MultiplayerService.SetActivityAsync méthode**. MPSD définit le nouvel ID de handle en tant qu’activité dépendant du membre de la session. S’il existait une activité précédente liée, MPSD supprime le handle correspondant. Lorsque le membre actif devient inactif ou quitte la session, MPSD supprime le handle de l’activité liée.


### <a name="using-handles"></a>À l’aide de descripteurs

Le titre utilise des handles lorsqu’un utilisateur accepte une invitation (handle de l’invitation) et lorsqu’un utilisateur rejoint l’activité en cours d’un ami (handle de l’activité). Dans ces deux cas, le titre doit :

1.  Obtenir l’ID de handle dans le titre de paramètres d’activation.
2.  Créer un objet de session MPSD local et joignez-le comme actif.
3.  Écrire la session, en passant le handle approprié.

## <a name="synchronization-of-session-updates"></a>Synchronisation des mises à jour de la Session

Une session étant une ressource partagée qui peut être créée ou mis à jour par un de ses membres, il existe un risque pour les écritures en conflit. Cela peut entraîner des résultats inattendus, par exemple, un titre risque de remplacer les modifications apportées par un autre titre. De MPSD résoudre ces conflits consiste à prendre en charge l’accès concurrentiel optimiste et un modèle de lecture-modification-écriture.

Synchronisation des mises à jour de la session par MPSD utiliser deux modèles d’implémentation de haut niveau associées :

-   Arbitre met à jour les parties partagées de la session. Si votre implémentation implique un arbitre unique, vous pouvez éviter d’utiliser des mises à jour synchronisées pour la plupart des opérations d’écriture. Le titre peut éviter la synchronisation pour (1) toute mise à jour l’arbitre permet à des parties de la session, partagés, sauf si elles sont liées à la communication d’identité de l’arbitre et (2) de tout par un titre à la zone de membre dans la session.

| Remarque                                                                                                                                                                                                                                                                                                                                              |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Bien que les types de mise à jour ci-dessus n’avez pas besoin de synchronisation, il est toujours important de synchroniser les mises à jour le ** MultiplayerSessionProperties.HostDeviceToken propriété **. Cette propriété est utilisée pour communiquer l’identité de l’arbitre, par exemple, dans le cadre de la migration de l’arbitre. |

-   Tous les clients mettre à jour les parties partagées de la session. Dans ce cas, toutes les mises à jour à des parties de la session partagées doivent être synchronisées. Toutefois, les titres peuvent toujours écrire dans leurs propres domaines membre sans synchronisation.


### <a name="session-update-synchronization-using-the-multiplayer-winrt-api"></a>Synchronisation de mise à jour de session à l’aide de l’API de WinRT multijoueur

Les méthodes suivantes des API WinRT multijoueurs implémentent l’accès concurrentiel optimiste :

-   **MultiplayerService.WriteSessionAsync (méthode)**
-   **MultiplayerService.WriteSessionByHandleAsync (méthode)**
-   **MultiplayerService.TryWriteSessionAsync (méthode)**
-   **MultiplayerService.TryWriteSessionByHandleAsync (méthode)**

Chaque méthode write accepte un **MultiplayerSessionWriteMode énumération** valeur. En passant la valeur SynchronizedUpdate utilisent l’accès concurrentiel optimiste pour les mises à jour.

Autres valeurs dans l’énumération aident à résoudre les conflits potentiels lors de la création initiale d’une session. Toute écriture à une partie de la session MPSD qui peut potentiellement être écrites dans par un autre titre doit utiliser une mise à jour synchronisée. Toutefois, pas toutes les écritures doivent être protégées.

Si votre titre tente d’écrire l’objet de session locale à MPSD en utilisant l’une des méthodes de session d’écriture et reçoit un code d’état HTTP/412, il doit actualiser la copie locale en émettant un **MultiplayerService.GetCurrentSessionAsync méthode**appel pour obtenir la dernière version de serveur de la session avant de tenter à nouveau de l’écriture. Sinon, le document de session locale continue à contenir les données incorrectes et les appels à écrire la session continuent d’échouer.

| Remarque                                                                                                                                                                                  |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Si le titre est à l’aide d’un de le **TryWrite\***  méthodes, la session de mise à jour est retournée dans le cas d’un code d’état HTTP/412. Ce comportement permet d’éviter la nécessité d’appeler **GetCurrentSessionAsync**. |

Lorsque le titre appelle l’une des méthodes de session d’écriture, une version mise à jour de la session peut être retournée. Dans le cas, le titre doit basculer sa copie locale mise en cache vers cette nouvelle version de manière thread-safe.


### <a name="session-update-synchronization-using-the-multiplayer-rest-api"></a>Synchronisation de mise à jour de session à l’aide de l’API REST multijoueurs

MPSD prend en charge l’accès concurrentiel optimiste mises à jour de la session via la fonctionnalité REST à l’aide de l’en-tête « if-match » HTTP avec les paramètres de l’ETag et le modèle de lecture-modification-écriture. L’ETag passé dans la demande d’écriture doit être celui qui la MPSD retourne avec la requête de lecture précédente.

## <a name="calling-mpsd"></a>Appel MPSD

Il existe deux façons pour le titre accéder aux MPSD pour pouvoir utiliser le système multi-joueurs et matchmaking :
-   Recommandé. Utiliser l’API WinRT multijoueurs, qui contient des classes qui agissent comme des wrappers pour la fonctionnalité RESTful. Consultez **Microsoft.Xbox.Services.Multiplayer Namespace**. Pour SmartMatch matchmaking, utilisez l’équivalent API WinRT, représenté par le **Microsoft.Xbox.Services.Matchmaking Namespace**.
-   Utiliser les appels directs HTTP standards pour le mode multijoueur et matchmaking API REST, inclus dans le **Xbox Live Services RESTful référence**. Les URI applicables sont décrites dans **Session Directory URI** (pour le mode multijoueur) et **Matchmaking URI** (pour matchmaking). Les objets connexes JSON sont décrits dans le **référence d’objet JavaScript Objet Notation (JSON)**.


### <a name="using-the-multiplayer-winrt-api-to-call-mpsd"></a>À l’aide de l’API de WinRT multijoueur pour appeler MPSD

La méthode recommandée pour appeler MPSD consiste à utiliser l’API WinRT multijoueurs et le matchmaking les API WinRT.

| Remarque                                                                                             |
|---------------------------------------------------------------------------------------------------------------|
| Les exemples XDK sont écrits à l’aide du mode multijoueur et matchmaking WinRT APIs et les autres éléments de XSAPI. |

Utilisation du code wrapper pour la fonctionnalité sous-jacente de REST permet une approche plus traditionnelle à l’aide des méthodes de l’API côté client sans avoir à gérer le trafic HTTP pour chaque appel. Un fichier binaire et une source de XSAPI sont livrés avec le XDK. Vous pouvez utiliser le fichier binaire directement, ou modifier la source et incorporer dans votre titre en fonction des besoins.


### <a name="using-the-multiplayer-rest-api-to-interact-with-mpsd"></a>À l’aide de l’API REST multijoueurs pour interagir avec MPSD

Le titre, ou son service, peut utiliser des appels HTTP standard pour l’API REST multijoueurs et l’API REST de matchmaking. Lorsque vous utilisez directement des fonctionnalités REST, les problèmes de l’appelant DELETE, PUT, POST et obtenir des appels sur URI de l’annuaire de sessions pour la plupart des actions. Sur une demande PUT, le corps de la demande est fusionné dans la session existante. S’il n’y a aucune session existante, le corps de demande est utilisé pour créer une nouvelle session, ainsi que le modèle de session stocké sur [Xbox Developer Portal (XDP)](https://xdp.xboxlive.com) ou [partenaires](https://partner.microsoft.com/dashboard). Tous les champs sont facultatifs, et des deltas uniquement doivent être spécifiés. Par conséquent, {} est une requête PUT valide avec des deltas zéro.

Pour effectuer une demande PUT hypothétique qui retourne le résultat de la fusion sans affecter la copie du serveur officiel de la session, vous pouvez ajouter la chaîne de requête « ? nocommit = true » à la demande PUT.

Les demandes et réponses pour les jeux multijoueurs et les méthodes de l’API REST de matchmaking sont des documents JSON. Pour une structure de demande de session multijoueur, consultez **MultiplayerSessionRequest (JSON)**. Une structure de réponse associée **MultiplayerSession (JSON)**. La structure de réponse encadre les membres de la session en tant qu’une liste liée et renseigne les autres propriétés en lecture seule de la session et de ses membres.


### <a name="querying-for-sessions-and-session-templates-rest"></a>Interrogation des Sessions et des modèles de Session (REST)

Vos titres peuvent demander des informations de session à la configuration du service et les niveaux de modèle de session. Cette rubrique traite des requêtes qui utilisent l’API REST multijoueurs.


#### <a name="query-for-basic-session-information"></a>Demander des informations de Session de base

Vous pouvez définir des requêtes pour les informations de session de base à l’aide de l’annuaire de sessions et matchmaking URI. Le résultat d’une requête est un tableau JSON des références, avec des données de session incluses inline de session. Par défaut, une requête récupère jusqu'à 100 sessions non privée.

| Remarque                                                          |
|----------------------------------------------------------------------------|
| Chaque requête doit inclure un filtre par mots clés, un filtre XUID ou les deux. |


#### <a name="query-for-session-templates"></a>Requête pour les modèles de Session

Pour récupérer la liste des modèles de session pour le SCID, ainsi que les détails d’un modèle de session spécifique, utilisez la méthode GET pour l’un des URI suivants :
-   **/serviceconfigs/{scid}/sessiontemplates**
-   **/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}**


#### <a name="query-for-session-state"></a>Interroger l’état de Session

Pour rechercher l’état de session, utilisez la méthode GET pour l’une des ces URI :
-   **/serviceconfigs/{scid}/sessions**
-   **/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions**

## <a name="multiplayer-session-explorer"></a>Explorateur de Session multijoueurs

Mode multijoueur Session Explorer est un outil intégré à MPSD pour parcourir les sessions, les modèles de session et les chaînes de localisation. L’outil doit uniquement être utilisé pour les bacs à sable de développement.


### <a name="accessing-multiplayer-session-explorer"></a>L’accès à une Session multijoueur Explorer

| Remarque                                                                                                      |
|------------------------------------------------------------------------------------------------------------------------|
| Pour utiliser l’outil, vous devez être connecté. Votre navigation est limitée aux sessions qui ont l’utilisateur connecté en tant que membre. |

Pour accéder à l’Explorateur de Session multijoueurs, ouvrez Internet Explorer sur votre Xbox One, appuyez sur la **vue** bouton, puis entrez <https://sessiondirectory.xboxlive.com/debug> dans le **adresse** champ.

| Remarque                                                                                                                                                                                |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Vous recevrez un code d’état HTTP/404 si vous tentez d’accéder à l’outil dans le bac à sable de vente au détail. Pour plus d’informations sur ce code, consultez [Codes d’état de Session de mode multijoueur](multiplayer-session-status-codes.md). |


### <a name="using-multiplayer-session-explorer"></a>À l’aide de l’Explorateur de Session multijoueur


#### <a name="open-the-main-page"></a>Ouvrir les principaux Page

1.  Accéder à la page principale de l’outil. Il affiche le contexte de sécurité (utilisateur connecté et bac à sable) et une liste de configuration du service ID (SCIDs) dans le bac à sable.
2.  Appuyez sur la **Menu** bouton épingler cette page à l’accueil afin que vous n’êtes pas obligé de taper l’URI chaque fois.


#### <a name="display-available-sessions-and-templates"></a>Afficher les modèles et les Sessions disponibles

1.  Cliquez sur un SCID dans l’outil pour afficher une liste de sessions dans ce SCID dont l’utilisateur connecté est un membre.
2.  Sur cette même page, vous pouvez cliquer sur le SCID et afficher les modèles de session et les chaînes de localisation dans la configuration de service pour le SCID. Ces éléments sont reçus par le biais [XDP](https://xdp.xboxlive.com) ou [partenaires](https://partner.microsoft.com/dashboard).


#### <a name="display-the-full-contents-of-a-session"></a>Afficher le contenu complet d’une Session

Dans l’Explorateur de Session multijoueurs, cliquez sur un nom de session pour afficher le contenu complet de la session correspondant.

La session, comme indiqué par MPSD peut-être différer de la réponse à une méthode GET standard pour les URI de la session pour plusieurs raisons :

-   L’appel GET peut être à l’aide une ancienne version de contrat dans l’en-tête X-Xbl--Version de contrat. Session Explorer affiche toujours la session à l’aide de la toute dernière version de contrat.
-   Lorsqu’une session est demandée normalement via GET, transformations et les effets secondaires peuvent être déclenchées, par exemple, expiration des délais d’attente. Explorateur de session affiche un instantané de la session lorsqu’elles sont stockées, sans exécuter toute logique, des transformations ou des effets secondaires.
-   Étant donné que le champ objet JSON nextTimer est calculé en même temps que les effets secondaires, il n’est pas présent sur les sessions MPSD.

## <a name="see-also"></a>Voir également

[Vue d’ensemble de la session](mpsd-session-details.md)

[Codes d’état de Session multijoueurs](multiplayer-session-status-codes.md)

[Procédure : Mettre à jour d’une Session multijoueur](multiplayer-how-tos.md)

[Procédure : Rejoindre une Session MPSD à partir de l’activation d’un titre](multiplayer-how-tos.md)

[Procédure : S’abonner aux Notifications de modification de Session MPSD](multiplayer-how-tos.md)

[SmartMatch Matchmaking](smartmatch-matchmaking.md)