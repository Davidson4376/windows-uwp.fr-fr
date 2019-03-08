---
title: Procédures multijoueurs
description: Décrit comment implémenter des tâches courantes dans Xbox Live multijoueurs 2015.
ms.assetid: 99c5b7c4-018c-4f7a-b2c9-0deed0e34097
ms.date: 08/29/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox, 2015 multijoueurs
ms.localizationpriority: medium
ms.openlocfilehash: 20bf3486491e8173ce946a3a5dc2e4bc83305c83
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57648634"
---
# <a name="multiplayer-how-tos"></a>Conseils pratiques sur le mode multijoueur

Cette rubrique contient des informations sur la façon d’implémenter des tâches spécifiques liées à l’utilisation de 2015 multijoueurs.

* S’abonner aux notifications de modification de session MPSD
* Créer une session MPSD
* Définir un arbitre pour une session MPSD
* Gérer l’Activation de titre
* Faire de l’utilisateur joignable
* Envoyer des invitations à des jeux
* Rejoindre une session de jeu à partir d’une session d’introduction
* Rejoindre une session MPSD à partir de l’activation d’un titre
* Définir l’activité actuelle de l’utilisateur
* Mettre à jour d’une session MPSD
* Laisser une session MPSD
* Remplir les emplacements de la session ouverte pendant matchmaking
* Créer un ticket de correspondance
* État de ticket de correspondance Get

## <a name="subscribe-for-mpsd-session-change-notifications"></a>S’abonner aux notifications de modification de session MPSD

| Remarque                                                                                                                                                                                                                                                                                                                                    |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| S’abonner aux modifications apportées à une session nécessite le lecteur associé doit être actif dans la session. Le champ connectionRequiredForActiveMembers doit également être défini sur true dans l’objet /constants/system/capabilities pour la session. Ce champ est généralement défini dans le modèle de session. Consultez [MPSD Session modèles](multiplayer-session-directory.md). |



Pour recevoir des notifications de modification de session MPSD, le titre peut suivre la procédure ci-dessous.

1.  Utiliser le même **XboxLiveContext classe** objet pour tous les appels par le même utilisateur. Les abonnements sont liées à la durée de vie de cet objet. S’il existe plusieurs utilisateurs locaux, recourir à un **XboxLiveContext** objet pour chaque utilisateur.
2.  Implémenter des gestionnaires d’événements pour le **RealTimeActivityService.MultiplayerSessionChanged événement** et **RealTimeActivityService.MultiplayerSubscriptionsLost événement**.
3.  Si l’abonnement à des modifications pour plusieurs utilisateurs, ajoutez le code à votre **MultiplayerSessionChanged** Gestionnaire d’événements afin d’éviter le travail inutile. Utilisez le **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch propriété** et **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber propriété**. Utilisation de ces propriétés permet le suivi de la dernière modification détectée et ignorer des modifications antérieures.
4.  Appelez le **MultiplayerService.EnableMultiplayerSubscriptions méthode** pour autoriser les abonnements.
5.  Créer un objet de session locale et une jointure cette session comme active.
6.  Effectuer des appels pour chaque utilisateur pour le **MultiplayerSession.SetSessionChangeSubscription méthode**, en passant de la session de modifier le type pour lequel doit être averti.
7.  Maintenant écrire la session à MPSD, comme décrit dans **Comment : Mettre à jour d’une Session multijoueur**.

L’organigramme suivant illustre comment démarrer Multplayer en vous abonnant aux événements décrites dans cette procédure.

![](../../images/multiplayer/Multiplayer_2015_Start_Multiplayer.png)


### <a name="parsing-duplicate-session-change-notifications"></a>L’analyse des notifications de modification de session en double

Lorsqu’il existe plusieurs utilisateurs abonnés aux notifications pour la même session, chaque modification apportée à cette session déclenchera un drainage de l’épaule pour chaque utilisateur. Toutes sauf une de ces seront les doublons. S’il est toujours recommandé qu’un titre s’abonner à tous les utilisateurs dans une session pour envoyer des notifications, un titre doit ignorer toutes les modifications qu’il est déjà été informé de ; Pour cela, en utilisant les propriétés de la branche et ChangeNumber.

Pour détecter plusieurs clics épaule, un titre doit :

-   Pour chaque **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch propriété** valeur vu, la dernière version du magasin **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber propriété**.
-   Si un drainage épaule a un degré plus élevé **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber propriété** que celle du dernier vu que **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch propriété** , traiter et mettre à jour de la dernière version **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber propriété**.
-   Si un drainage épaule n’a pas un degré plus élevé **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber propriété** pour qui **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch propriété**, ignorez-la. Cette modification a déjà été traitée.

| Remarque                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Propriété de RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber** valeurs doivent être suivis par **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch propriété**, et non par session. Il est possible pour le **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch propriété** valeur à modifier (et le **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber propriété**Réinitialiser) au sein de la durée de vie d’une session. |

## <a name="create-an-mpsd-session"></a>Créer une session MPSD


| Remarque                                                                                                                                                                                                                           |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Par défaut, une session MPSD est créée lorsque le premier membre joint. Si votre logique de titre attend le titre à exister ou non au moment de la jointure, il peut passer une valeur de mode d’écriture appropriées à la méthode write pendant la mise à jour de la session. |



Le titre doit procédez comme suit pour créer une nouvelle session :

1.  Créer un nouveau **XboxLiveContext classe** objet. Votre titre crée cet objet une fois, il stocke et réutilise en fonction des besoins tout au long du code source. En particulier lorsque vous travaillez avec des abonnements de la session, il est nécessaire d’utiliser exactement le même contexte.
2.  Créer un nouveau **MultiplayerSession classe** objet pour préparer toutes les données de session dont le MPSD a besoin pour créer une nouvelle session.
3.  Apportez les modifications requises avant d’écrire la session sur MPSD. Par exemple, lors de la jonction d’un membre à la session avec un appel à **MultiplayerSession.Join méthode**, le client ajoute des données de demande locale masqué qui indique à MPSD pour joindre lors de l’appel à la session de mise à jour.
4.  Lorsque terminé d’apporter des modifications locales, écrivez-les dans MPSD comme décrit dans **Comment : Mettre à jour d’une Session multijoueur**.
5.  Recevoir la nouvelle **MultiplayerSession** objet à partir de MPSD, avec de nombreux champs renseignés.
6.  Utiliser l’objet de la nouvelle session à l’avenir, puis annuler l’ancienne copie, qui contient une demande masquée pour créer une nouvelle session.

### <a name="example"></a>Exemple

    void Example_MultiplayerService_CreateSession()
    {
      XboxLiveContext^ xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext(User::Users->GetAt(0));

      // Values found in Xbox Developer Portal(XDP) or Partner Center configuration
      MultiplayerSessionReference^ multiplayerSessionReference = ref new MultiplayerSessionReference(
        "c83c597b-7377-4886-99e3-2b5818fa5e4f", // serviceConfigurationId
        "team-deathmatch", // sessionTemplateName
        "mySession" // sessionName
        );

      MultiplayerSession^ multiplayerSession = ref new MultiplayerSession(
        xboxLiveContext,
        multiplayerSessionReference
        );

      auto asyncOp = xboxLiveContext->MultiplayerService->WriteSessionAsync(
        multiplayerSession,
        MultiplayerSessionWriteMode::CreateNew
        );

      create_task(asyncOp)
      .then([](MultiplayerSession^ newMultiplayerSession)
      {
        // Throw away stale multiplayerSession, and use newMultiplayerSession from now on
      });
    }


## <a name="set-an-arbiter-for-an-mpsd-session"></a>Définir un arbitre pour une session MPSD




Le titre utilise la procédure suivante pour définir un arbitre pour une session qui a déjà été créée.

| Remarque                                                                                                                                       |
|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| Jetons d’appareil pour les membres (ordinateurs hôtes potentiels) ne sont pas disponibles jusqu'à ce que les membres ont rejoint la session et inclus leurs adresses sécurisée des appareils. |

1.  Récupérer les jetons d’appareil pour les candidats hôte à partir de la MPSD à l’aide de la **MultiplayerSession.Members propriété**.

| Remarque                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Si la session a été créée par SmartMatch matchmaking, vos clients peuvent utiliser les candidats hôtes disponibles à partir de MPSD via la **MultiplayerSession.HostCandidates propriété**. |

2.  Sélectionnez l’hôte requis à partir de la liste des candidats de l’hôte.
3.  Appelez le **MultiplayerSession.SetHostDeviceToken méthode** pour définir le jeton d’appareil dans le cache local de le MPSD. Si l’appel pour définir l’hôte du jeton d’appareil réussit, le jeton d’appareil local remplace le jeton de l’hôte.
4.  Si un code d’état HTTP/412 est reçu lorsque vous tentez de définir l’hôte de jeton d’appareil, interroger les données de session et voir si le jeton d’appareil hôte est de la console locale. Si elle n’est pas de la console locale, une autre console a été désignée comme l’arbitre.

| Remarque                                                                                                                                                                                                                              |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Votre client doit gérer le code d’état HTTP/412 séparément à partir d’autres codes HTTP, étant donné que HTTP/412 n’indique pas une défaillance standard. Pour plus d’informations sur le code d’état, consultez [Codes d’état de Session de mode multijoueur](multiplayer-session-status-codes.md). |

5.  Mettre à jour de la session dans MPSD, comme décrit dans **Comment : Mettre à jour d’une Session multijoueur**.

| Remarque                                                                                                                                                                                                                                           |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Si vous ne disposez d’aucun meilleur algorithme, le client peut implémenter un algorithme gourmand dans lequel chaque candidat hôte tente de se configurer lui-même en tant que l’hôte si personne d’autre ne l’a fait encore. Pour plus d’informations, consultez [Session arbitre](mpsd-session-details.md). |

## <a name="manage-title-activation"></a>Gérer l’activation de titre

Se déclenche Xbox One le **CoreApplicationView.Activated événement** pendant l’activation du protocole. Dans le contexte de l’API multijoueur, cet événement est déclenché lorsqu’un utilisateur accepte une invitation ou joint à un autre utilisateur. Ces actions qui déclenchent une activation le titre doit réagir à en apportant l’utilisateur de jonction dans le jeu avec l’utilisateur cible.

| Remarque                                                                                       |
|---------------------------------------------------------------------------------------------------------|
| Votre titre doit attendre de nouveaux arguments d’activation à tout moment et ne doit jamais être codée par rapport à la longueur. |

Le titre doit effectuer les étapes principales suivantes pour gérer l’activation de titre.

1.  Configurer un gestionnaire d’événements pour le **CoreApplicationView.Activated événement**. Ce gestionnaire se déclenche chaque fois que l’activation de protocole se produit, même si le titre est déjà en cours d’exécution.
2.  Lors de l’activation de titre, démarrer une session et s’abonner aux notifications de modification de session. Consultez **Comment : S’abonner aux Notifications de modification de Session MPSD**.
3.  Joindre l’utilisateur à la session active. Consultez **Comment : Rejoindre une Session MPSD à partir de l’activation d’un titre**.
4.  Définir la session d’introduction en tant que la session de l’activité, exposée via le profil de l’interface utilisateur. Consultez **Comment : Définir l’activité actuelle de l’utilisateur**.
5.  Joindre l’utilisateur à la session de jeu comme actif. Désormais, l’utilisateur peut se connecter à des homologues et entrez une partie ou la salle d’attente.

L’organigramme suivant illustre comment gérer l’activation de titre.

![](../../images/multiplayer/Multiplayer_2015_OnActivation.png)

## <a name="make-the-user-joinable"></a>Faire de l’utilisateur joignable

Pour rendre l’utilisateur joignable, le titre doit les opérations suivantes :

1.  Créer un objet de session et modifier les attributs en fonction des besoins.
2.  Joindre l’utilisateur à la session active. Consultez **Comment : Rejoindre une Session MPSD à partir de l’activation d’un titre**.
3.  Déterminer si l’utilisateur a été désigné comme l’arbitre de session.
4.  Si l’utilisateur n’est pas l’arbitre, passez à l’étape 7.
5.  Si l’utilisateur est l’arbitre, appelez le **MultiplayerSession.SetHostDeviceToken méthode**.
6.  Tentez d’écrire la session à l’aide d’un appel à la **MultiplayerService.TryWriteSessionAsync méthode**.
7.  Définir la session en tant que la session active. Consultez **Comment : Définir l’activité actuelle de l’utilisateur**.

L’organigramme suivant illustre les étapes à suivre pour autoriser un utilisateur à être jointes par les autres lecteurs au cours du jeu.

![](../../images/multiplayer/Multiplayer_2015_Become_Joinable.png)

## <a name="send-game-invites"></a>Envoyer des invitations à des jeux

Le titre peut activer un lecteur envoyer des invitations de jeu de plusieurs manières :

-   Envoyer les invitations pour la session d’introduction.
-   Envoyer les invitations à l’aide de la plateforme Xbox générique inviter l’interface utilisateur avec la référence de la session de jeu.

Pour envoyer des invitations de jeu pour un lecteur, le titre doit les opérations suivantes :

1.  Vérifiez le joueur invitation joignable. Consultez **Comment : Faire de l’utilisateur joignable**.
2.  Déterminer si les invitations doivent être envoyées par le biais de la session d’introduction ou à l’aide de l’interface utilisateur de l’invitation.
3.  Si vous utilisez la session d’introduction, envoyer les invitations à l’aide d’un appel à **MultiplayerService.SendInvitesAsync méthode**. Cette méthode peut nécessiter la création d’une liste de l’interface utilisateur dans le jeu à l’aide du **SystemUI.ShowPeoplePickerAsync méthode** ou **PartyChat.GetPartyChatViewAsync méthode**.
4.  Si vous utilisez l’interface utilisateur de l’invitation, appelez le **SystemUI.ShowSendGameInvitesAsync méthode** pour afficher l’interface utilisateur de l’invitation.
5.  Gérer le **RealTimeActivityService.MultiplayerSessionChanged événement** pour le lecteur local après les jointures de lecteur à distance.
6.  Pour le lecteur à distance, implémentez le code de l’activation de titre. Consultez **Comment : Gérer l’Activation de titre**.

L’organigramme suivant illustre comment envoyer des invitations.

![](../../images/multiplayer/Multiplayer_2015_Send_Invites.png)

## <a name="join-a-game-session-from-a-lobby-session"></a>Rejoindre une session de jeu à partir d’une session d’introduction

Sessions de jeu sur les appareils Windows 10 doivent avoir le `userAuthorizationStyle` fonctionnalité définie **true** s’ils ne sont pas des sessions de grande taille. Cela signifie que le `joinRestriction` propriété ne peut pas être « none », ce qui signifie que la session ne peut pas être publiquement joignable directement.

Un scénario courant consiste à créer une session d’introduction pour rassembler des joueurs et puis déplacer ces lecteurs dans une session ou matchmaking le jeu. Toutefois, si la session de jeu n’est pas joignable publiquement, les clients de jeu ne sera pas en mesure de joindre la session de jeu, sauf si elles répondent à la `joinRestriction` paramètre, qui est trop restrictive dans la plupart des cas pour ce scénario.

La solution est Utilisez un handle de transfert pour lier la session d’introduction et la session de jeu.  Le titre de ce faire, procédez comme suit :

1. Lorsque vous créez la session de jeu, utilisez la `multiplayer_service::set_transfer_handle(gameSessionRef, lobbySessionRef)` API pour créer un handle de transfert qui lie la session d’introduction et la session de jeu.
2. Store le handle de transfert GUID dans la session d’introduction au lieu de la référence de session de la session de jeu.
3. Lorsque le titre souhaite déplacer des membres à partir de la session d’introduction à la session de jeu, chaque client utilise le handle de transfert à partir de la session d’introduction à la session de jeu à l’aide de la `multiplayer_service::write_session_by_handle(multiplayerSession, multiplayerSessionWriteMode, handleId)` API.
4. MPSD recherche la session d’introduction pour vérifier que les membres qui tentent de rejoindre la session de jeu en utilisant le descripteur de transfert sont également dans la session d’introduction.
5. Si les membres sont dans la session d’introduction, ils seront en mesure d’accéder à la session de jeu.

## <a name="join-an-mpsd-session-from-a-title-activation"></a>Rejoindre une session MPSD à partir de l’activation d’un titre

Lorsqu’un utilisateur choisit joindre l’activité d’un ami ou accepter une invitation à l’aide de la Xbox shell l’interface utilisateur, le titre est activé avec des paramètres qui indiquent quelles session de l’utilisateur souhaite rejoindre. Le titre doit gérer cette activation, ajoutez l’utilisateur à la session correspondante.

Voici les étapes que le titre doit suivre :

1.  Implémenter un gestionnaire d’événements pour le **CoreApplicationView.Activated événement**. Cet événement informe d’activations pour le titre.
2.  Lorsque le gestionnaire se déclenche, examiner le **IActivatedEventArgs.Kind propriété**. Si elle est définie sur le protocole, effectuer un cast d’arguments d’événement à **ProtocolActivatedEventArgs classe**.
3.  Examiner le **ProtocolActivatedEventArgs** objet. Si l’URI indiqué dans le **ProtocolActivatedEventArgs.Uri propriété** correspond à deux inviteHandleAccept (correspondant à une invitation acceptée) ou activityHandleJoin (correspondant à une jointure par le biais de l’interface utilisateur de l’interpréteur de commandes), analyser la chaîne de requête de l’URI, qui est mise en forme comme une chaîne de requête URI normale avec les paires clé/valeur, extraire les champs suivants :
    -   Pour une invitation acceptée :
        1.  handle
        2.  invitedXuid
        3.  senderXuid
    -   Pour une jointure :
        1.  handle
        2.  joinerXuid
        3.  joineeXuid

4.  Démarrez code multijoueur du titre, qui doit inclure l’appel la **MultiplayerService.EnableMultiplayerSubscriptions méthode**.
5.  Créer une variable locale **MultiplayerSession classe** de l’objet, à l’aide de la **MultiplayerSession, constructeur (XboxLiveContext)**.
6.  Appelez le **MultiplayerSession.Join (méthode) (chaîne, booléen, booléen)** pour rejoindre la session. Utilisez les paramètres suivants pour que la jointure soit définie comme étant active :
    -   *memberCustomConstantsJson* = null
    -   *initializeRequested* = false
    -   *joinWithActiveStatus* = true

7.  Appelez le **MultiplayerSession.SetSessionChangeSubscription méthode** à drainer épaule lorsque la session change après avoir joint.
8.  Appelez le **MultiplayerService.WriteSessionByHandleAsync méthode**, à l’aide de la poignée acquise comme décrit à l’étape 3. Désormais, l’utilisateur est membre de la session et peut utiliser des données dans la session pour se connecter à la partie.

## <a name="set-the-users-current-activity"></a>Définir l’activité actuelle de l’utilisateur

L’activité l’utilisateur actuel est affichée dans des expériences utilisateur Xbox tableau de bord pour le titre. Activité d’un utilisateur peut être définie via une session ou l’activation du titre. Dans ce cas, l’utilisateur entre une session via matchmaking ou en démarrant un jeu.

| Remarque                                                                                                                                                  |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Activité définir via une session peut être supprimée avec un appel à la **MultiplayerService.ClearActivityAsync méthode**. |

Pour définir une session en tant qu’activité en cours de l’utilisateur, les appels de titre le **MultiplayerService.SetActivityAsync méthode**, en passant la référence de session pour la session.

Pour définir l’activité actuelle de l’utilisateur via l’activation de titre, consultez **Comment : Rejoindre une Session MPSD à partir de l’activation d’un titre**.

## <a name="update-an-mpsd-session"></a>Mettre à jour d’une session MPSD

| Remarque                                                                                                                                                 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lorsque le titre de la mise à jour une session existante à l’aide de l’API multijoueur, n’oubliez pas qu’il fonctionne avec une copie locale, jusqu'à ce qu’elle effectue un appel à écrire la session. |

Pour mettre à jour d’une session existante, le titre doit :

1.  Apporter des modifications à la session active en fonction des besoins, par exemple, en appelant le **MultiplayerSession.Leave méthode**.
2.  Lorsque toutes les modifications sont apportées, écrire les modifications locales à MPSD, en utilisant l’une des méthodes suivantes :

    -   **MultiplayerService.WriteSessionAsync (méthode)**
    -   **Méthode de MultiplayerService.WriteSessionByHandleAsync**.
    -   **MultiplayerService.TryWriteSessionAsync (méthode)**
    -   **MultiplayerService.TryWriteSessionByHandleAsync (méthode)**

    Définissez le mode d’écriture sur **SynchronizedUpdate** si l’écriture dans une partie partagée que d’autres titres permettre également modifier. Consultez [synchronisation des mises à jour de Session](multiplayer-session-directory.md) pour plus d’informations.

    La méthode write écrit la jointure sur le serveur et obtient la dernière session, à partir de laquelle découvrir d’autres membres de la session et les adresses de périphérique sécurisé (SDAs) de leurs consoles. Pour plus d’informations sur l’établissement d’une connexion réseau entre ces consoles, consultez **Introduction à Winsock sur Xbox One**.

3.  Supprimer l’objet de session locale ancien et utiliser l’objet de session qui vient d’être récupérées afin que les actions futures sont basées sur le dernier état de session connue.

## <a name="leave-an-mpsd-session"></a>Laisser une session MPSD

Pour autoriser un utilisateur de laisser une session, le titre procédez comme suit.

1.  Appelez le **MultiplayerSession.Leave méthode** pour la session de jeu.
2.  Mettre à jour de la session de jeu dans MPSD, comme décrit dans **Comment : Mettre à jour d’une Session multijoueur**.
3.  Si nécessaire, appelez **laisser** méthode pour la session d’introduction et mettre à jour de cette session.
4.  Si nécessaire pour la session d’introduction, arrêtez l’API en mode multijoueur en désinscrivant le **RealTimeActivityService.MultiplayerSubscriptionsLost événement** et  **Événement de RealTimeActivityService.MultiplayerSessionChanged**.

L’organigramme suivant illustre comment quitter la session et arrêter.

![](../../images/multiplayer/Multiplayer_2015_Shut_Down.png)

## <a name="fill-open-session-slots-during-matchmaking"></a>Remplir les emplacements de la session ouverte pendant matchmaking

Pour remplir les emplacements ouverts dans une session de ticket pendant matchmaking, le titre doit suivre des étapes similaires à ce qui suit :

1.  Accès le dernier état de session pour la session de ticket créée au cours de matchmaking.
2.  Ajouter des lecteurs disponibles pour le jeu à partir de la session d’introduction.
3.  Déterminer si la session de ticket est plein.
4.  Si la session est saturée, continuer le jeu.
5.  Si la session n’est pas encore complète, créez le ticket correspondance comme décrit dans **Comment : Créer un Ticket de correspondance**. Veillez à créer le ticket avec le *preserveSession* paramètre défini sur toujours.
6.  Poursuivez matchmaking. Consultez [à l’aide de SmartMatch Matchmaking](using-smartmatch-matchmaking.md).

L’organigramme suivant illustre comment remplir les emplacements de la session ouverte pendant matchmaking.

![](../../images/multiplayer/Multiplayer_2015_Fill_Open_Slots.png)

## <a name="create-a-match-ticket"></a>Créer un ticket de correspondance

Pour créer un ticket de correspondance, le scout matchmaking doit :

1.  Appelez le **MatchmakingService.CreateMatchTicketAsync méthode**, en passant une référence à la session de ticket. La méthode lit la session de ticket MPSD et démarre matchmaking pour les utilisateurs dans la session. La méthode appelle en interne la **POST (/hoppers/ /serviceconfigs/ {scid} {hoppername})**.
2.  Définir le *preserveSession* paramètre jamais si le service doit correspondre à des membres de la session dans une nouvelle session ou une autre session existante. Définir le *preserveSession* paramètre pour toujours pour autoriser le titre de réutiliser une session de jeu existante en tant qu’une session de ticket pour continuer la lecture du jeu. Le service peut puis vérifiez que la session soumise est conservée et des lecteurs sont mis en correspondance sont ajoutées à cette session.

3.  Utilisez le **CreateMatchTicketResponse.EstimatedWaitTime propriété** retournées dans le **CreateMatchTicketResponse classe** objet pour définir les attentes des utilisateurs de temps de matchmaking.
4.  Utilisez le **CreateMatchTicketResponse.MatchTicketId propriété** retournée dans l’objet de réponse pour annuler matchmaking pour la session si nécessaire, en supprimant le ticket. Ticket de suppression utilise la **MatchmakingService.DeleteMatchTicketAsync méthode**.

## <a name="get-match-ticket-status"></a>État de ticket de correspondance Get

Le titre doit effectuer la commande suivante pour récupérer l’état de ticket de correspondance :

1.  Obtenir le **MultiplayerSession classe** objet pour la session de ticket.
2.  Utilisez le **MultiplayerSession.MatchmakingServer propriété** pour accéder à la **MatchmakingServer classe** objet utilisé dans matchmaking.
3.  Vérifier le **MatchmakingServer** objet pour déterminer l’état de la procédure matchmaking, le délai d’attente par défaut pour la session et la référence de la session cible, si une correspondance a été trouvée.
