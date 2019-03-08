---
title: Flux mis à jour pour les invitations de jeux multijoueurs
description: Fournit des flux de mise à jour pour l’implémentation de Xbox Live multijoueur invite.
ms.assetid: 1569588e-3bbc-47d3-8b7d-cc9774071adc
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox, 2015 multijoueurs
ms.localizationpriority: medium
ms.openlocfilehash: 1092c84521271e996db0b89630d22f7e51727bfa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596884"
---
# <a name="updated-flows-for-multiplayer-game-invites"></a>Flux mis à jour pour les invitations de jeux multijoueurs

À la suite de commentaires de version bêta de Xbox One, le flux d’expérience utilisateur pour le jeu multijoueur invite a été modifié depuis Xbox une récupération mise à jour 24, publiée le 6 novembre 2013. Il s’agit d’une modification apportée à la **-expérience utilisateur (UX) uniquement** et n’affecteront pas tout comportement ou la fonctionnalité du point de vue d’un titre de jeu. Les développeurs de titre seront inutile d’apporter des modifications de code.

## <a name="summary-of-changes"></a>Résumé des modifications

-   Le toast invitation initiale est passé de « joindre mon tiers » à «&lt;jeux titre&gt; amusons-nous » et a maintenant un bouton qui permet aux utilisateurs de lancer le jeu et lancer directement dans le jeu.

-   L’application de tiers n’est pas alignée par défaut lorsque l’utilisateur choisit le nouveau «&lt;jeux titre&gt; amusons-nous « option. Cette modification est également effectuée afin que l’utilisateur peut lancer immédiatement dans le jeu.

-   Un toast nouveau a été ajouté sur le côté de l’expéditeur indiquant que « ajout \[ *nombre* \] amis au jeu ». Cela indique clairement qu’invitations ont été envoyées lorsqu’une session de jeu est associée à des parties d’un utilisateur.

Les flux d’expérience utilisateur détaillées sont décrites dans les exemples suivants. Chaque table montre un exemple de flux pour deux utilisateurs, David et Laura. Ces flux est affichés dans chaque colonne et être exécutées en parallèle. Le <b style="background-color: #FFFF00">mis en surbrillance de texte</b> montre les ajustements qui ont été apportées entre les flux de l’expérience utilisateur précédentes.

## <a name="inviting-users-from-within-a-game"></a>Inviter des utilisateurs à partir d’au sein d’un jeu

<table>
  <tr>
    <td style="border-bottom:solid 1px #fff">
David est dans le Hall multijoueur pour un jeu, qu'il est lu.<p><br>David choisit <b>inviter un ami</b>.<p><br>David sélectionne Laura.<p><br>Toast s’affiche indiquant qu’une invitation a été envoyée. | Un autre jeu de Laura.
    </td>
    <td style="border-bottom:solid 1px #fff">
Un autre jeu de Laura.
    </td>
  </tr>
  <tr>
    <td></td>
    <td>
Toast s’affiche indiquant une invitation à partir de David, et <b style="background-color: #FFFF00">affiche le nom du jeu et l’icône</b>. (L’application tiers ne pas auto-composant logiciel enfichable.) <p><br>
Dans le centre de notifications, <b style="background-color: #FFFF00">Laura peut choisir de lancement et accepter l’invitation</b>, <b>accepter l’invitation</b>, ou <b style="background-color: #FFFF00">refuser inviter</b>.
    </td>
  </tr>
  <tr>
    <td colspan="2" style="text-align:center"><b style="background-color: #FFFF00">Cas 1 : Laura sélectionne le lancement et accepter l’invitation</b> (il s’agit d’une nouvelle option)</td>
  </tr>
  <tr>
    <td>
Toast s’affiche indiquant que Laura a joint les tiers de David.             
    <p><br>
David commence à jouer à partir de la salle d’attente multijoueur.                              
    <p><br>
    <b style="background-color: #FFFF00">Toast s’affiche indiquant qu’une invitation à un jeu a été envoyée à Laura.</b>
    </td>
    <td>
Lancement du jeu et l’application de tiers ne s’aligne pas.
    </td>
  </tr>
  <tr>
    <td colspan="2" style="text-align:center"><b>Cas 2 : Laura sélectionne Accept invitation</b></td>
  </tr>
  <tr>
    <td style="border-bottom:solid 1px #fff"></td>
    <td style="border-bottom:solid 1px #fff">Laura joint le tiers.</td>
  </tr>
  <tr>
    <td style="border-bottom:solid 1px #fff"><b style="background-color: #FFFF00">Toast s’affiche indiquant qu’une invitation à un jeu a été envoyée à Laura.</b></td>
    <td style="border-bottom:solid 1px #fff"></td>
  </tr>
  <tr>
    <td></td>
    <td>Toast s’affiche indiquant qu’un jeu a été trouvé pour le tiers.
    <p><br>
Dans le centre de notifications, Laura peut choisir : <ul>
    <li>   <b>Accepter l’invitation à un jeu :</b> Lancements de jeu.
    <li>   <b>Refuser l’invitation à un jeu :</b> Aucun jeu ne démarre. Laura est toujours dans le tiers et recevez des invitations de jeu suivantes.         
    <li>   <b style="background-color: #FFFF00">Laissez le tiers : Aucun jeu ne démarre. Laura est supprimée d’un tiers.</b>
    </ul>
    </td>
  </tr>
</table>

## <a name="in-game-invite-flow-in-a-party-and-switching-titles"></a>Dans le jeu invitation flux : Dans un tiers et de basculer des titres

<table>
  <tr>
    <td>
Jouer ensemble.
    <p><br>
David bascule vers un autre jeu et ouvre le menu multijoueur.
     <p><br>
L’interface utilisateur système de Xbox demande s’il souhaite basculer son tiers vers la nouvelle partie, à laquelle il peut répondre à David <b>Oui</b> ou <b>non</b>.
    </td>
    <td style="text-align:top">
Jouer ensemble.
    </td>
  </tr>
  <tr>
    <td colspan=2 style="text-align:center">
      <b>Cas 1 : Oui</b>
    </td>
  </tr>
  <tr>
    <td style="border-bottom:solid 1px #fff">
Tiers est fourni pour le nouveau titre.
    <p><br>
À partir de la salle d’attente multijoueur, David commence à jouer.
    <p><br>
    <b style="background-color: #FFFF00">Toast s’affiche indiquant qu’une invitation à un jeu a été envoyée à Laura.
    </td>
    <td style="border-bottom:solid 1px #fff">
    </td>
  </tr>
  <tr>
    <td></td>
    <td>Toast s’affiche indiquant qu’un jeu a été trouvé pour le tiers.
    <p><br>
À partir du centre de Notification, Laura peut choisir : <ul>
     <li><b>Accepter l’invitation à un jeu</b>: Lancements de nouveaux jeux <li><b>Refuser l’invitation à un jeu :</b> Aucun jeu ne démarre, mais est toujours dans la partie de Laura et recevra des invitations de jeu suivantes.
     <li><b style="background-color: #FFFF00"><b>Laissez le tiers :</b> Aucun jeu de lancements et Laura n’est supprimé du tiers.</b>
     </ul>
     </td>
  </tr>
  <tr>
    <td colspan=2 style="text-align:center">
      <b>Cas 2 : No</b>
    </td>
  </tr>
  <tr>
    <td>
Tiers n’est pas fourni pour le nouveau titre.
David est lue dans le mode multijoueur sans membres de son tiers.
David est toujours dans le tiers.
    </td>
    <td>
    </td>
  </tr>
</table>

## <a name="invite-flow-from-home"></a>Inviter des flux à domicile

<table>
  <tr>
    <td>
David effectue un parcours <b>accueil</b>et dans son <b>amis</b> liste, il voit que Laura est en ligne.
    <p><br>
David choisit à inviter Laura à son tiers. Toast s’affiche indiquant que l’invitation est envoyée. L’application de tiers est ancrée pour David.
    </td>
    <td>
Laura est un jeu.
    </td>
  </tr>
  <tr>
    <td></td>
    <td>
Toast s’affiche indiquant que David a invité Laura à son tiers.
    <p><br>
Dans le centre de notifications, Laura a la possibilité de <b>accepter l’invitation</b>.
    <p><br>
Lorsque Laura accepte, l’application de tiers s’aligne automatiquement.                                                                         </td>
  </tr>
  <tr>
    <td>
Toast s’affiche indiquant que Laura a joint le tiers.
    <p><br>
David et Laura discutent quel jeu de lecture. David lance le jeu et entre le mode de jeu multijoueur.
    <p><br>
Jeu attribuer soit une option pour inviter des amis ou par extraction automatique les membres de tiers.
    <p><br>
    <b style="background-color: #FFFF00">Toast s’affiche indiquant qu’une invitation à un jeu a été envoyée.</b>
    </td>
    <td>
Toast s’affiche indiquant qu’un jeu a été trouvé pour le tiers.
    <p><br>
Dans le centre de notifications, Laura peut : <ul>
    <li>   <b>Accepter l’invitation à un jeu :</b> Lancements de jeu <li>   <b>Refuser l’invitation à un jeu :</b> Aucun jeu démarre, Laura est toujours dans le tiers et recevez des invitations suivantes.
    <li>   <b style="background-color: #FFFF00">Laissez le tiers : Aucun jeu ne démarre, Laura est supprimé d’un tiers.</b>
    </ul>  
    </td>
  </tr>
</table>


## <a name="more-about-the-game-invite-sent-toast"></a>En savoir plus sur le toast jeu invitation envoyée

Le **jeux inviter envoyé** toast s’affiche uniquement la première fois une session de jeu est établie avec les membres du tiers distant. Les demandes suivantes envoyées aux membres de la partie distante ne génèrent pas cet toast. Cela empêche l’utilisateur de recevoir des messages indésirables avec répété **jeu inviter envoyé** toasts si le titre effectue plusieurs appels de **PullReservedPlayersAsync**.

**Remarque** la meilleure pratique consiste à ajouter tous les amis vous le souhaitez en tant que réservé, puis appelez **PullReservedPlayersAsync** qu’une seule fois.
