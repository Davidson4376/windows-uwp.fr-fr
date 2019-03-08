---
title: Problèmes courants de migration multijoueurs 2015
description: En savoir plus sur les problèmes courants que vous pouvez rencontrer lorsque vous adaptez votre titre 2014 multijoueurs pour le mode multijoueur 2015.
ms.assetid: 206f8fe4-c7aa-44b8-923b-18f679d8439f
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a04370ee2f45534c88467700b9523c5a4ad11094
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57615814"
---
# <a name="common-issues-when-adapting-your-multiplayer-2014-title-to-multiplayer-2015"></a>Problèmes courants lorsque vous adaptez votre titre 2014 multijoueur à 2015 multijoueurs

Cette rubrique décrit les problèmes que vous devez prendre en compte lorsque vous adaptez vos titres multijoueur 2014 pour le mode multijoueur 2015.


## <a name="configuration-changes-to-make-for-2015-multiplayer"></a>Modifications de configuration à prendre pour le mode multijoueur 2015

Cette section décrit les modifications à connaître lors de la configuration de vos sessions et les modèles pour le mode multijoueur 2015. Pour obtenir des exemples des éléments spécifiques décrits, consultez [MPSD Session modèles](multiplayer-session-directory.md).

### <a name="add-a-capability-for-active-member-connection"></a>Ajouter une fonctionnalité de connexion de membre actif

La fonctionnalité connectionRequiredForActiveMembers permet la détection de déconnexion et fonctionnalités d’abonnement de mode multijoueur 2015 de changement de session. Ajouter cette fonctionnalité à l’objet /constants/system/capabilities pour tous les modèles de session.


### <a name="add-a-system-constant-for-invite-protocol"></a>Ajoutez une constante de système pour le protocole de l’invitation

La constante de système inviteProtocol permettant au destinataire d’une invitation pour recevoir un toast lorsque le titre de l’expéditeur appelle le **MultiplayerService.SendInvitesAsync méthode** ou **SystemUI.ShowSendGameInvitesAsync Méthode (IUser, IMultiplayerSessionReference, String)**. Ajoutez cette constante, la valeur est « game », à l’objet /constants/system pour tous les modèles pour les sessions à laquelle le titre invite les joueurs.


## <a name="runtime-considerations-for-2015-multiplayer"></a>Considérations sur le runtime pour le mode multijoueur 2015

Titres pour 2015 multijoueurs doit :   Appelez toujours le **MultiplayerService.EnableMultiplayerSubscriptions méthode** avant l’entrée dans la zone multijoueur du code de titre. Cet appel permet les deux abonnements aux modifications de la session et la détection de la déconnexion.
-   Veillez à utiliser le même **XboxLiveContext classe** objet pour tous les appels par le même utilisateur. Le contexte contient l’état lié à la gestion de la connexion utilisée pour les abonnements multijoueurs et détection de la déconnexion.
-   S’il existe plusieurs utilisateurs locaux, recourir à un **XboxLiveContext** objet pour chaque utilisateur.


## <a name="migrating-a-session-template-from-contract-version-104105-to-107"></a>Migration d’un modèle de Session à partir de la Version de contrat/105 104 à 107

La dernière version de contrat de modèle de session est 107, avec certaines modifications du schéma utilisé pour MPSD. Les modèles que vous avez écrit pour la version de contrat de modèle de session 104/105 doivent être mis à jour si elles sont publiées à nouveau pour utiliser la version 107. Cette rubrique récapitule les changements à effectuer lors de la migration de vos modèles vers la dernière version. Les modèles eux-mêmes sont décrites dans [MPSD Session modèles](multiplayer-session-directory.md).

| Important                                                                                                                                                                                                                                                      |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Fonctionnalité qui est définie via un modèle ne peut pas être modifiée via écritures à MPSD. Pour modifier les valeurs, vous devez créer et envoyer un nouveau modèle avec les modifications nécessaires. Tous les éléments qui ne sont pas définies via un modèle est modifiable via les écritures vers la MPSD. |


### <a name="changes-to-the-constantssystemmanagedinitialization-object"></a>Modifications apportées à l’objet /constants/system/managedInitialization

L’objet /constants/system/managedInitialization a été renommé en /constants/system/memberInitialization. Voici les modifications à apporter aux paires nom/valeur pour cet objet : autoEvaluate externalEvaluation est renommé. Modifications apportées à son polarité, sauf que false reste la valeur par défaut.
-   La valeur par défaut membersNeededToStart passe de 2 à 1.
-   La valeur par défaut joinTimeout passe de 5 secondes à 10 secondes.
-   La valeur par défaut measurementTimeout passe de 5 secondes à 30 secondes.


### <a name="changes-to-the-constantssystemtimeouts-object"></a>Modifications apportées à l’objet /constants/system/timeouts

L’objet /constants/system/timeouts est supprimé, et les délais d’expiration sont renommés et déplacés sous /constants/system. Voici les modifications à apporter aux paires nom/valeur pour cet objet :   Le délai d’attente réservé devient reservedRemovalTimeout.
-   Le délai d’attente inactive devient inactiveRemovalTimeout. Sa nouvelle valeur par défaut est 0 (heures).
-   Le délai d’attente prêt devient readyRemovalTimeout.
-   Le délai d’attente sessionEmpty devient sessionEmptyTimeout.

Détails les délais d’attente sont présentés dans [des délais d’expiration de Session](mpsd-session-details.md).


### <a name="change-to-the-game-play-capability"></a>Remplacez par la fonctionnalité de jeu

Le jeu joue fonctionnalité permet aux derniers joueurs et la réputation reporting. Vous devez maintenant spécifier le champ /constants/system/capabilities/gameplay comme true sur les sessions qui représentent la partie réelle, et non les sessions d’assistance, par exemple, correspondent et sessions de la salle d’attente.


## <a name="see-also"></a>Voir également

[Modèles de Session MPSD](mpsd-session-details.md)
