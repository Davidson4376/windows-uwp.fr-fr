---
title: Joinability et visibilité de la session de jeu
description: Décrit la session de jeu Xbox Live et visibilité de jeux tiers et joinability.
ms.assetid: 39b6dac1-0c6b-4dc1-9fe0-3cb7c471fbab
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 65da0d60080cd790b84ce46f8598c2f6f638ff05
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57619714"
---
# <a name="game-session-and-game-party-visibility-and-joinability"></a>Session de jeu et visibilité de jeux tiers et joinability

Sur Xbox One, les *visibilité* et *joinability* paramètres pour les sessions de jeu et les parties de jeu, respectivement, contrôler l’accès à des expériences multijoueurs. Pour fournir une expérience utilisateur satisfaisante sur la jonction de la session et pour les joueurs invitation dans les parties et les sessions de jeu, les développeurs de titre doivent comprendre ces paramètres. Ce livre blanc présente les différences entre la visibilité et joinability, et il décrit les paramètres spécifiques, nous recommandons d’utilisent des titres de donner leurs consommateurs le flux mieux multijoueur utilisateur.

## <a name="game-session-visibility"></a>Visibilité de la session de jeu


Accès à une session multijoueur Session Directory (MPSD) est contrôlé par deux paramètres : session *visibilité* et session *joinability*. Ces paramètres peuvent être utilisés pour limiter l’accès de session au niveau du MPSD.

Le premier aspect du contrôle d’accès est la visibilité de la session. Visibilité de la session est une constante qui est définie au moment de la création de la session. Il est généralement défini dans le modèle de session et détermine les types d’utilisateurs ont lu et accès en écriture à une session. Les valeurs pour le paramètre de visibilité sont :

-   *Ouvert —* tous les utilisateurs peuvent lire et écrire à la session.

-   *Visible :* tous les utilisateurs peuvent lire, mais seulement joint et réservé joueurs peuvent écrire à la session.

-   *Privé :* uniquement joint et réservé joueurs capable de lire ou écrire dans la session.

> **Remarque :** Définition de la visibilité sur privé peut nécessiter de nouvelles tentatives sur le **join()** appeler par le joueur invité. Si une invitation est envoyée via la plateforme de l’interface utilisateur, il peut être reçu par un autre lecteur avant que le lecteur a été réservé dans la session. Cette condition de concurrence peut provoquer un **join()** appeler pour un lecteur invité échouer, car il est privées ou visibles sessions nécessitent une réservation pour le joueur de jointure.
>
> Si aucune réservation n’existe, une erreur d’accès sur la session (HTTP/403) est déclenchée. Le joueur invité devra puis recommencez la **join()** opération, mais les nouvelles tentatives ne doivent pas être tentée plus fréquemment que toutes les cinq secondes. Nous recommandons de définir une visibilité de session pour ouvrir pour éviter des conditions de concurrence pendant le flux de jointure pour un lecteur de titres.

## <a name="game-session-joinability"></a>Joinability de session de jeu


Le second aspect de contrôle d’accès est joinability. Il peut être définie de manière dynamique pendant la durée de vie de session et détermine les types d’utilisateurs peuvent rejoindre une session. Les valeurs pour joinability de session sont :

-   *Aucun* (valeur par défaut), il n’existe aucune restriction sur joindre à la session.

-   *Local*: seuls les utilisateurs locaux peuvent rejoindre la session.

-   *Suivi :* uniquement les utilisateurs locaux et les utilisateurs qui sont suivies par d’autres membres de la session peuvent rejoindre la session sans réservation.

Un hôte de session ou d’un arbitre peut créer une session privée via le paramètre joinability : joinability soit local ou suivi sera restreindre l’accès à une session de jeu et rendez-le privé.

En outre, l’hôte de session ou d’un arbitre doit effectuer le suivi de la joinability d’une session, afin que les anciennes session invitations pouvant être rejetées au niveau de l’hôte si nécessaire. Par exemple, si des lecteurs sont invités ne sont pas arrivés pour rejoindre une session et/ou de tiers jusqu'à ce que la session est déjà complète, l’hôte ou un arbitre peut demander à des lecteurs de jointure sont que la session a été verrouillée et dont ils ont besoin quitter la session et/ou tiers automatiquement.

**Remarque :** Joinability de session de jeu n’est pas reflétée dans l’application de partie interface utilisateur. Même si joinability est limité, l’application de tiers inviter l’interface utilisateur et **ShowSendInvitesAsync** autoriser les invitations de session de jeu.

## <a name="game-party-joinability"></a>Joinability de jeux tiers


Outre le contrôle de la session MPSD, le titre a également contrôler l’état de joinability de jeux tiers. Ce paramètre peut être utilisé pour limiter l’accès de jeux tiers au niveau du tiers.

Joinability de tiers peut être modifiée de manière dynamique une fois que le jeu tiers est créé. L’état de la joinability d’un tiers de jeu est reflétée dans la partie inviter l’interface utilisateur. Elle peut être définie sur :

-   *Une invitation —* ce paramètre nécessite une invitation à rejoindre le jeu tiers.

-   *Joignable par vos amis* (valeur par défaut)*—* ce paramètre requiert une relation de friend pour un lecteur à rejoindre le jeu tiers (pour joindre un tiers, un membre de tiers doit suivre le joueur connecté).

Joinability peut être utilisé pour créer un tiers de jeu une invitation. Pour restreindre l’accès à un tiers et exiger que les joueurs ont reçu une invitation à rejoindre, joinability doit être défini sur « inviter uniquement ».

**Remarque :** Joinability de tiers n’influence pas joinability de session, mais le joinability tiers est reflété dans l’application de partie interface utilisateur. Lecteurs peuvent modifier le joinability tiers dans l’application de tiers manuellement en dehors d’un titre.

## <a name="recommendations"></a>Recommandations


Les recommandations suivantes s’appliquent pour les scénarios courants de titre. Titres doivent suivre ces paramètres, si possible, et ils doivent utiliser une logique de titre pour effectuer cette détermination finale, faisant autoritée concernant indique si un nouveau lecteur y est admis dans la session ou non.

### <a name="recommended-game-session-visibility-open"></a>Visibilité de la session de jeu de recommandé : ouvrir

Les sessions ouvertes ne nécessitent pas de réservations de lecteur, simplifiant ainsi le flux d’invitations. L’hôte de session ou d’un arbitre ne réserve pas joueurs dans le document de session après qu’une invitation a été envoyée. Au lieu de cela, l’hôte de session ou d’un arbitre uniquement effectue le suivi des joueurs invités localement. Cela permet aux joueurs d’immédiatement se connecter à l’hôte et déterminer si elles doivent rejoindre une session, sont rejetés ou doivent attendre (si les lecteurs en attente sont pris en charge). L’arbitre ou l’hôte est l’autorité ultime et répond et indique le nouveau membre soit rester dans ou laissez la session.

**Remarque :** Cette opération exige le joueur invité lancer un titre et le connecter à l’hôte ou d’un arbitre avant la décision finale a été effectuée. Il est acceptable pour afficher un message d’erreur à l’utilisateur si une session est plein ou une invitation a été rejetée.

> **Remarque :** Pour établir une connexion à l’hôte ou d’un arbitre, une adresse de l’appareil sécurisé est requise. Le **HostDeviceToken** dans la session doit être utilisé pour indiquer quel membre de la session est l’hôte actuel ou un arbitre d’une session et qui adresse du périphérique de sécuriser un lecteur invité doit se connecter.

### <a name="recommended-game-session-joinability-default-not-set"></a>Recommandé joinability de session de jeu : par défaut (*nenastaveno*)

Actuellement joinability de session de jeu n’influence pas l’application de partie interface utilisateur et n’est pas présenté à l’utilisateur. Pour simplifier le flux de titre, les jeux joinability doit rester à la valeur par défaut, et toute l’autorité jointure doit être gérée par l’intermédiaire de l’arbitre ou l’hôte.

### <a name="recommended-game-party-joinability"></a>Recommandé joinability de jeux tiers

Joinability de jeux tiers est dépendant du type de session qui tente d’un titre pour fournir à un utilisateur. Les deux scénarios sont :

-   Ouvrez le jeu, un jeu ouvert, le joinability de jeux tiers doit conserver la valeur par défaut : Joignable par vos amis. Cela permet à vos amis à participer à la partie tiers (et par extension de la session de jeu) sans une invitation.

-   Partie privée, pour un jeu privé ou fermé, le joinability de jeux tiers doit être définie sur invitation uniquement. Ce paramètre restreint autres joueurs de rejoindre le tiers (et par extension de la session de jeu) sans une invitation.

**Remarque** : Lecteurs peuvent modifier manuellement le joinability d’une session de jeu dans l’application de tiers.

Pour les deux types de jeux, l’arbitre ou l’hôte doit rester toujours de l’autorité ultime pour déterminer qui est accepté dans une session. Cela répond à des conditions de concurrence peuvent se produire à partir de la commutation de flux des jeux d’open privé. Si l’arbitre/hôte rejette un lecteur, l’instance de titre rejetés doit supprimer le lecteur à partir de la session et afficher une interface utilisateur dans le jeu autorise également un lecteur de laisser le tiers.

### <a name="maximum-session-size"></a>Taille maximale de session

La taille maximale de membre pour une session peut être utilisée pour limiter le nombre de joueurs rejoindre une session de jeu. Toutefois, cette limite peut entraîner un emplacement ouvert pour toujours signalé comme étant complète pendant certaines déconnecter scénarios. Par exemple, si un lecteur est déconnecté en raison d’un réseau ou alimentation, le délai n'est pas répercutée immédiatement dans la session. Le membre est défini comme étant inactif dès que le service de présence détecte la déconnexion (jusqu'à 20 secondes) et le lecteur sera supprimé après que le délai d’inactivité a expiré.

En comparaison, un maillage pair à pair qui utilise une pulsation pour détecter une déconnexion connaît souvent une déconnexion dans les deux à trois secondes et peut ouvrir l’emplacement player immédiatement. Toutefois, l’arbitre ou l’hôte ne peut pas supprimer d’autres membres.

Pour résoudre ce problème, la taille maximale de membre d’une session doit toujours être définie plus haut que le nombre maximal de lecteurs pris en charge par un titre. Par exemple, si le nombre de lecteur est 8, le titre doit définir la taille maximale d’une session à 12. De cette façon nouvelle joueurs peuvent joindre même si d’anciens lecteurs n’ont pas encore expiré. L’arbitre ou l’hôte détermine si la session est plein et définit une propriété de session personnalisé qui sera détermine si les nouveaux joueurs puissent être joints (**IsFull** : « true »). Cela permet une vérification rapide de rejoindre les joueurs si la session est joignable.

L’arbitre ou l’hôte doit également conserver une propriété personnalisée qui indique quels membres, par index, ont expiré (par exemple, **TimedOutPlayers** : “3, 5, 9”). Cela permet de nouveaux lecteurs identifier correctement les membres de la session active.

### <a name="notifying-waiting-players"></a>Notifier les lecteurs en attente

Un titre peut prendre en charge les lecteurs en attente en gérant une liste de lecteur en file d’attente en plus de jeux tiers. Lorsqu’un joueur se connecte à l’hôte et la session de jeu est plein, le lecteur est ajouté à la liste d’attente interne sur l’ordinateur hôte. Le lecteur en file d’attente ne joint pas la session de jeu, mais reste dans le jeu tiers. Pour réduire le trafic réseau, le joueur en attente déconnecte l’ordinateur hôte à ce stade.

Quand un emplacement dans la session de jeu s’ouvre pour l’acteur en attente, l’arbitre ou l’hôte ajoute une réservation pour le lecteur en appelant le **AddMemberReservation** (méthode). À ce stade l’acteur en attente n’est pas encore prenant en charge de cette réservation, par conséquent l’arbitre ou l’hôte doit appeler la **PullReservedPlayersAsync** (méthode). Cela entraîne une interface utilisateur toast ou **GameSessionReady** notification de tous les joueurs réservées, selon la configuration de toast et l’état de focus de titre. Nouvel acteur se reconnecte à l’hôte quand cette notification est reçue et rejoint la session.

Sinon, un lecteur peut rester connecté à l’hôte et rejoindre la session lorsque l’hôte avertit le lecteur qu’un emplacement est disponible. Toutefois, dans ce flux un toast de l’interface utilisateur en dehors du titre ne peut pas être affiché.

**Remarque :** Joueurs seront automatiquement supprimés du jeu tiers si le titre est suspendu et/ou s’est arrêté et recevez aucune autre notifications.

<a name="client-multiplayer-flow"></a>Flux multijoueur du client
-----------------------
![](../../images/whitepapers/gamesessionvisibility_image1.png)
![](../../images/whitepapers/gamesessionvisibility_image2.png)
