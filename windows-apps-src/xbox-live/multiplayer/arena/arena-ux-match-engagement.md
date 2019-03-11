---
title: Engagement de correspondance
description: Décrit les étapes de l’expérience utilisateur de joueurs progresse, grâce à un tournoi.
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, arena, tournoi, l’expérience utilisateur
ms.localizationpriority: medium
ms.openlocfilehash: edc81ff7c692c4789855d341917c54acf4d24594
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598354"
---
# <a name="match-engagement"></a>Engagement de correspondance

Une fois passionné a enregistré et archivé pour un tournoi, qu’elles soient paramétrées à lire. Un participant peut être à un des quatre phases. Chaque étape contient un ensemble de critères de guider les utilisateurs grâce à l’expérience de jeu tournoi unique. Les quatre étapes sont :

1.  Prêt (Arena l’interface utilisateur ou la partie)
2.  Lecture (jeu)
3.  Résultats (Arena l’interface utilisateur ou la partie)
4.  Fin (Arena l’interface utilisateur ou la partie)

> [!NOTE]  
> La lecture est la seule étape des quatre qui ne sont pas pris en charge dans l’interface utilisateur de domaine. Au minimum, votre titre doit rediriger des participants à l’interface utilisateur de domaine à la fin de chaque correspondance (lors de la phase de jouer est moyenne), pour pouvoir visualiser les résultats.

###### <a name="diagram-the-four-stages-of-participant-progression-after-check-in"></a>Diagramme : Les quatre étapes de progression participante, après l’archivage.

![Diagramme de flux de correspondance engagement](../../images/arena/arena-ux-match-flow.png)

![Barre de flux engagement de correspondance](../../images/arena/arena-ux-match-flow-bar.png)

## <a name="1-ready-waiting-for-match"></a>1. Prêt (« en attente de correspondance »)

![En attente de diagramme de correspondance](../../images/arena/arena-ux-flow-ready.png)

### <a name="user-impact"></a>Impact sur les utilisateurs

À ce stade, le joueur est censé jouer dans une correspondance, mais l’adversaire n’est pas encore connu. Cette étape est déclenchée lorsqu’un tournoi a démarré et le participant est en attente de leur session de correspondance commencer. Ce délai d’attente peut signifier que :

* La période d’archivage est toujours ouverte.
* Le lot suivant n’a pas démarré.
* Le participant a un bye en cours à arrondir.

### <a name="when-a-bye-is-needed"></a>Quand un bye est nécessaire

Un bye peut se produire dans n’importe quel round d’un tournoi, même si nous avons réparti pour essayer d’éviter l’amorçage.

À tout moment une élimination d’unique ou double-élimination tournoi implique un nombre de lecteurs ou les équipes qui n’est pas une puissance de 2 (autrement dit, 4, 8, 16, 32, 64, 128 ou 256), octets doivent être attribués. Un bye signifie simplement qu’une équipe n’a pas de participer à la première série du tournoi et obtient à la place un laissez-passer gratuit pour la deuxième série.

### <a name="discovery"></a>Découverte

![Capture d’écran de la découverte Team](../../images/arena/arena-ux-team-discovery.png)

Dans la phase de prête, les participants apprendront sur leur état dans ces emplacements :

* L’interface utilisateur de domaine : Page de détails de tournoi
* Dans le jeu : Liste de tournoi, Promotion, salle d’attente de correspondance

> [!TIP]  
> **Recommandations de l’expérience utilisateur**  
> Indique que le lecteur est en attente de leur correspondance suivante dans l’événement. Exemples : « Faire correspondre en attente » ou « Vous avez reçu un bye » de notification.  
>
> Si cette étape est pris en charge, fournissent une méthode pour revenir à la page de détails du tournoi.  
>
> Donner le contexte de l’événement : Nom du tournoi, état, Style, description, démarrer temps et de fin, etc.


###### <a name="ui-example-in-game-multiplayer-lobby"></a>Exemple d’interface utilisateur : Lobby multijoueur dans le jeu

![Capture d’écran de tournoi d’introduction](../../images/arena/arena-ux-tournament-lobby.png)

Présentent toujours l’état du tournoi et la raison dans une position cohérente, partout où une correspondance de tournoi est promue ou correspondance détails sont présentés.

Il s’agit d’instructions essentielles pour les joueurs lorsqu’ils doivent décider :

* Pour laisser le jeu pour en savoir plus.
* Les étapes suivantes sont nécessaires pour passer dans un tournoi.

### <a name="under-the-hood"></a>Sous le capot
Si cette étape est pris en charge par le titre, passez à l’étape de jouer lorsque la correspondance est prête.

 
## <a name="2-playing"></a>2. Lecture

![Lire le schéma de correspondance](../../images/arena/arena-ux-flow-play.png)

### <a name="user-impact"></a>Impact sur les utilisateurs

Suivant correspondance à distance le joueur est connue et la session multijoueur a été créée. Durant cette phase, le titre déplace les joueurs par le biais du flux de jeu normal (écrans d’installation de la correspondance, salle de jeu et ainsi de suite). Il s’agit de la phase d’atteindre les utilisateurs lorsque la correspondance est prête.

### <a name="discovery"></a>Découverte

Il existe deux façons d’alerter un participant au tournoi et leur permettre de joindre une correspondance de tournoi à partir en dehors d’un jeu :

* **Notification de toast Xbox** : lors de la notification s’affiche, le participant peut contenir le ![bouton de nexus](../../images/xbox-nexus.png) (bouton Nexus) pour lancer le jeu et entrez une correspondance.

  ![Toast prêt de correspondance](../../images/arena/arena-ux-match-ready-toast.png)

* Page de détails Arena tournoi : le participant peut sélectionnez **lire** pour lancer le jeu et entrez une correspondance
 
> [!TIP]  
> **Recommandation de l’expérience utilisateur :** Vérifiez l’entrée de correspondance.  
> Afficher l’interface utilisateur qui informe et/ou confirme que le participant est sur le point d’entrer directement dans une salle d’attente de correspondance.

###### <a name="ui-example-match-entry-confirmation-dialog-box"></a>Exemple d’interface utilisateur : boîte de dialogue de confirmation entrée correspond à

![Boîte de dialogue de confirmation de correspondance entrée](../../images/arena/arena-ux-confirm-match-entry.png)

Afficher cette interface utilisateur une fois que le participant choisit d’entrer un tournoi à partir d’un emplacement en dehors du jeu. Pour cela une fois un jeu a lancé et avant d’entrer la salle d’attente de correspondance.

> [!TIP]  
> **Recommandation de l’expérience utilisateur :** Proposer une option pour annuler.

Les participants peuvent décider d’autres tâches que dont ils ont besoin pour effectuer pour préparer le tournoi. Il est important de leur donner la possibilité de changer leur avis. Votre titre peut offrir des options supplémentaires :

* Restez dans le jeu. (Le participant est dirigé vers le menu principal.)
* Revenir à l’interface utilisateur de son domaine.
* Si le participant choisit de quitter le jeu, nous recommandons que votre titre informer le participant de la durée restante avant la limite de délai d’attente acquise a été atteint.

###### <a name="ui-example-match-entry-confirmation--leave-game-warning"></a>Exemple d’interface utilisateur : correspond à la confirmation de l’entrée : laissez avertissement jeu

![Boîte de dialogue de confirmation à acquise correspondance entrée](../../images/arena/arena-ux-confirm-match-cancel.png)

### <a name="playing-a-match"></a>Lecture d’une correspondance

L’heure de début pour la correspondance est défini dans le modèle de session. Il est défini comme une date-heure au format UTC standard — par exemple, `2009-06-15T13:45:30.0900000Z`. L’organisateur de tournoi peut créer la session dans la mesure où avant l’heure de début qu’il le souhaite, y compris en même temps que l’heure de début. Correspondance notifications sont envoyées aux participants de correspondance à l’heure de début, titres jamais être activés avant l’heure de début. En général, traiter la session de domaine de la même façon que vous traiteriez votre session multijoueur du titre. Toutefois, il existe quelques différences :

* Votre titre ne peut pas modifier la liste des participants de la session de jeu.
* Il n’est pas le processus d’analyse habituels des joueurs avec des vitesses de connexion problématique.
* Votre titre doit participer d’arbitrage.

### <a name="match-lobby-details"></a>Détails de salle d’attente de correspondance

Une salle d’attente de la correspondance tournoi doit inclure des détails qui il apparaît clairement qu’il s’agit d’un tournoi, et qu’il est différent d’une correspondance multijoueur standard. Exemples de tels détails sont minutage, dépendance de l’équipe et arrondit consécutifs et étapes. Lorsque la correspondance est terminée, passez à l’étape 3 (résultats en attente) ou retourner le lecteur à l’interface utilisateur de domaine.

###### <a name="ui-example-match-lobby-details"></a>Exemple d’interface utilisateur : Détails de salle d’attente de correspondance

![Écran de détails de correspondance d’introduction](../../images/arena/arena-ux-match-lobby-details.png)

**Un** -renforcer qu’il s’agit d’une correspondance « Tournoi » ou « Domaine ».  
**B** -détail du tournoi  
   1. Nom du tournoi (128 caractères maximum)  
   2. Style, Mode de jeu  
   3. Round ou numéro d’étape  

**C** - prêt inscrire / bouton lecture – Participants peuvent avoir entré un tournoi, mais nécessitent un temps supplémentaire avant de commencer la session de correspondance. Par exemple, une correspondance peut nécessiter de configuration, comme la sélection de caractère. Cet intuitif peut informer les autres membres de l’équipe lorsqu’ils sont prêts.  
**D** -compte à rebours
  * La minuterie est basée sur le **limite de délai d’attente acquise** (défini par le jeu).
  * Cela vous avertit d’équipes que si la configuration requise minimale d’équipe n’est pas remplie à ce stade, le jeu est perdu et le service wins équipe actif.  

**E** - diffusion rappel  
**F** -nom de l’équipe + participants

 
### <a name="under-the-hood"></a>Sous le capot

#### <a name="protocol-activation"></a>Activation de protocole

L’interface utilisateur Arena lance choses en envoyant des participants directement dans un titre lorsqu’un utilisateur est prêt à jouer une correspondance, par exemple, quand acceptant le toast « correspondance prêt » envoyé par le domaine. L’activation de protocole peut se produire à deux reprises : comme le titre est lancé ou quand il est déjà en cours d’exécution. Dans les deux cas, l’activation est similaire à ce qui se passe lorsqu’un titre est activé en réponse à un utilisateur qui accepte une invitation au jeu.

* **Si le titre est lancé** -- **Activated** événement est déclenché pour la première fois lorsque le titre est lancé. Si cette activation initiale est une activation de protocole Arena, cela signifie que le titre a été lancé par un utilisateur qui tente de lire dans un tournoi. Le titre doit ignorer aussi rapidement que possible à la correspondance, en contournant le menu principal, les écrans d’inscription. L’ID d’utilisateur de Xbox (XUID) de l’utilisateur qui participent est fourni dans l’URI de l’activation et doit être déjà connecté.

* **Si le titre a été déjà en cours d’exécution** -- **Activation** événement peut également se déclencher avec une activation de protocole Arena, tandis que le titre est déjà en cours d’exécution. Si le titre est inactif, lorsqu’il reçoit l’activation de protocole, il doit accéder immédiatement à la correspondance de tournoi. Il est possible pour ce faire, car l’événement d’activation est déclenché uniquement par une action explicite de l’utilisateur (agissant sur un toast qui mène à la correspondance, ou passer à la correspondance à partir de l’interface utilisateur de domaine). Si le titre reçoit l’activation du protocole cours de jeu, il doit donner à l’utilisateur la possibilité de laisser le jeu (enregistrant d’abord, si nécessaire) et entrez la correspondance de tournoi de la manière la plus rapide possible.

* **Si un participant contourne la notification de toast Xbox** --nous recommandons qu’un titre compatible Arena interrogez toujours Arena au démarrage pour voir si l’utilisateur est dans une correspondance active. Si par conséquent, le titre doit présenter « confirmer la correspondance entrée » interface utilisateur, pour indiquer à l’utilisateur, ils ont une correspondance et demander s’ils souhaitent à le saisir.  

  Cela garantit que participants effectuer la transition vers une correspondance de tournoi correctement, au cas où ils manquer le toast de correspondance et lancent simplement le jeu (comptez obtenir dans leur correspondance). Ou, dans le cas le jeu se bloque et le participant de redémarrage simplement le titre revenir dans leur correspondance.

> [!NOTE]  
> Le titre doit vérifier le XUID sur l’activation du protocole URI. Si elle ne correspond pas au lecteur actuel du titre, le titre doit également basculer les contextes d’utilisateur.

#### <a name="forfeit-time-out"></a>Délai d’attente acquise

Au moins un lecteur doit être actif dans la session avant l’heure acquise, qui est l’heure de début ainsi que le délai d’attente acquise (voir le diagramme lecture). Si aucune autre a rejoint la session active avant l’heure acquise, la correspondance est annulée et les deux équipes bénéficient d’une perte via l’arbitrage de son domaine.
 
## <a name="3-results"></a>3. Résultats

![Résultats du diagramme de correspondance](../../images/arena/arena-ux-flow-results.png)

Il s’agit de la phase lorsqu’un lecteur a terminé la correspondance et les résultats ont été signalés à être arbitrée. À ce stade, il ne sont pas encore été dépend si l’équipe est toujours dans le tournoi. En attendant, l’expérience d’écran de « game fin » doit continuer signale les résultats de la session (comme dans n’importe quelle correspondance multijoueur).

Les résultats du tournoi, aucun concours (aucun jeu), Win, perte, Draw, rang ou non afficher — suivront.

Il est possible pour cette étape doit être ignorée si le résultat est immédiatement disponible.

### <a name="reporting-results"></a>Résultats de la création de rapports

Les résultats de la correspondance sont signalés au domaine et l’organisateur de tournoi via la session à l’aide d’une fonctionnalité appelée *arbitrage*. Arbitrage est une infrastructure pour l’utilisation d’une session de correspondance à lire en toute sécurité une correspondance, le rapport un résultat.

Lorsqu’une session de correspondance commence, elle est considérée comme une session arbitrée. Il a une chronologie fixe qui applique le framework d’arbitrage.
 
### <a name="arbitration-time-out"></a>Délai d’attente de l’arbitrage

Le délai d’attente d’arbitrage est la quantité maximale de temps, après l’heure, qui ont des participants à lire et afficher des résultats de début de la correspondance. Cette valeur est définie dans le modèle de session pour les événements d’exécution de la bibliothèque multimédia tournoi et dans le Mode de jeu multijoueur Arena pour UGT et les événements de l’exécution de franchise quelconques, afin d’en faire autant de temps que les besoins votre titre.

Le titre peut signaler les résultats à tout moment entre l’heure de début et l’heure d’arbitrage. Arbitrage se produit à tout moment entre l’heure d’acquise et l’heure d’arbitrage, une fois que chaque membre actif de la session a envoyé les résultats. Par exemple, si le seul membre est actif dans la session à la fois acquise, ils peuvent (et devriez) publient un résultat et arbitrage se produira. Quel que soit le nombre de résultats est disponible au moment de l’arbitrage, arbitrage se produira si elle n’est pas déjà.

Si un utilisateur se trouve dans une session qui a déjà été arbitrée (étant donné que le délai d’expiration de l’arbitrage a expiré, un serveur de jeu qui gère la session ou l’utilisateur joint plus tard), votre titre doit terminer la correspondance et afficher le résultat arbitrée à l’utilisateur.

Résultats de l’arbitrage toujours incluent le résultat pour chaque équipe. Lorsque le score d’un joueur individuels est signalée, il inclut non seulement les résultats de leur équipe, mais l’ensemble de résultats pour chaque équipe.
Arbitrage commence automatiquement dès que le premier client signale les résultats à la Xbox Live. Elle se termine dès que tous les clients signalent les résultats. Il peut arriver que les participants Lisez plus longtemps que la durée d’arbitrage spécifiée dans le modèle de session. Cela entraîne un risque qu’une correspondance est obligée de s’arrêter avant que les participants sont prêtes.

> [!TIP]  
> **Recommandation de l’expérience utilisateur :** Fournir un laps de temps fixe de session de correspondance.
>
> Les limites de temps prédéfinies sur les sessions de correspondance permet de réduire les risques de problèmes qui se produisent à partir de délai d’attente de l’arbitrage.

Il existe deux options pour empêcher une correspondance de se terminer avant que les participants sont effectuées en cours de lecture :

* Définir un intervalle de temps important pour le délai d’attente de l’arbitrage.
* Si un laps de temps fixe est en conflit avec le style de jeu de votre titre, essayez la solution suivante :
   1.   Déterminer les résultats n’ont pas été signalés, et que le délai d’attente d’arbitrage est arrive à expiration (par exemple, cinq minutes sont gauche).
   2.   Afficher l’interface utilisateur qui avertit les participants à leur jeu est sur le point de se terminer par X minutes. Cette option peut être implémentée pour tous les tournois et si le délai d’attente d’arbitrage est important, la plupart des participants ne le voit jamais.

### <a name="returning-to-the-arena-ui-by-using-a-dedicated-a-button-press"></a>Retour à l’interface utilisateur de domaine à l’aide un dédié une touche

Si votre titre ne commencent à manifester résultats du tournoi ou progression étape dans le jeu, nous recommandons que le participant de disposer d’un moyen pour revenir à l’interface utilisateur qui offre ces informations. Il peut s’agir de l’interface utilisateur de domaine ou une application de la bibliothèque multimédia par tournoi par des tiers. Dans l’idéal, cet intuitif apparaît une fois la correspondance sur ou potentiellement en réponse à la demande d’un joueur d’abandonner la correspondance en cours d’exécution. Cela peut être effectué à partir d’un écran de rapport de fin de jeu.

> [!TIP]  
> **Recommandation de l’expérience utilisateur**  
> Dédiez un bouton de contrôleur comme un moyen rapide pour les joueurs à retourner au Hub Arena dans le tableau de bord Xbox.  
>
> ![Bouton du contrôleur de domaine du concentrateur](../../images/arena/arena-ux-arena-hub-button.png)

### <a name="redirect-participants-at-the-end-of-a-match"></a>Rediriger les participants à la fin d’une correspondance

Lorsque les participants de la correspondance complète, il est important que votre titre donne clairement orienté sur les étapes suivantes. Cela inclut une méthode pour passer en revue le tournoi round ou les résultats de l’étape (si ce n’est pas présenté dans le jeu).

> [!TIP]  
> **Recommandation de l’expérience utilisateur :** Utiliser un segment de recouvrement contextuelle.
>
> Cette interface utilisateur s’affiche lorsque les résultats du tournoi se trouvent dans l’expérience de jeu est terminé.

### <a name="recommended-ui-data"></a>Données de l’interface utilisateur recommandées :

* Nom du tournoi
* Crochet suivant
* Détails de round ou de la phase suivantes
* Permanent de l’équipe
* Résultats arbitrées
* Lien ciblé vers les détails de tournoi
* Option permettant de rester dans le jeu

### <a name="under-the-hood"></a>Sous le capot

Après avoir appelé l’interface utilisateur de domaine, le titre doit se poursuivre en cours d’exécution, peut-être à ce même écran, en attente d’un autre événement d’activation du protocole. Ensuite, si le joueur a une autre correspondance de jouer, le titre sera prêt à l’emploi. Elle peut accélérer l’expérience utilisateur comme le joueur passe de correspondance en correspondance, basculer entre le titre et l’interface utilisateur de domaine.

###### <a name="ui-example-tournament-arbitrated-resultswinning-team"></a>Exemple d’interface utilisateur : Tournoi arbitrée résultats — gagnante d’équipe

![Vous avez gagné écran](../../images/arena/arena-ux-won-game-redirect.png)

###### <a name="ui-example-end-of-tournament-resultsmatch-ended"></a>Exemple d’interface utilisateur : Résultats de la fin du tournoi, correspondance s’est terminée 

![Vous avez perdu écran](../../images/arena/arena-ux-lost-game-redirect.png)

## <a name="4-end"></a>4. Fin

![Fin du diagramme de correspondance](../../images/arena/arena-ux-flow-end.png)

L’étape de fin d’un flux de tournoi se produit lorsque le tournoi lui-même s’est terminée et les résultats finaux sont signalés.

Les participants sont informés que le joueur a été supprimé ou que le tournoi est terminé.

### <a name="discovery"></a>Découverte

L’interface utilisateur de domaine affiche les résultats et célèbre les gagnants finales :

* Page Détails du tournoi
* Résultats sont partagés par les participants dans leur flux d’activités performant de club ou de lecteur
* Résultats du tournoi validées au profil d’un joueur

Le titre de la surface de résultats du tournoi dans le jeu :

* Après le démarrage de la fin du jeu pour la dernière évaluation effectué le participant.
* Répertoriés sous **tournois Mes**, dans une fonctionnalité tournoi Parcourir.


###### <a name="ui-example-end-of-tournament-resultstournament-ended"></a>Exemple d’interface utilisateur : Fin des résultats du tournoi, tournoi s’est terminée

![Tournoi sur l’écran](../../images/arena/arena-ux-tournament-completed.png)

* Si cette étape est pris en charge, fournissent une option pour revenir à l’interface utilisateur de domaine ou d’une application de bibliothèque multimédia tournoi.
* Le nom complet de l’événement.
* Afficher le nom d’équipe, si l’équipe a plusieurs membres.
* Afficher l’équipe permanent dans l’événement, s’il est disponible. Si aucune capacité n’est disponible (ou si vous le souhaitez, en plus de la position), votre titre doit indiquer que la participation du joueur dans l’événement a pris fin.
* Votre titre suggère des étapes suivantes (par exemple, « Espion de flux live », « Trouver une autre tournoi, », « rester dans le jeu »).
* Votre titre fournit une raison pourquoi l’engagement de l’équipe est terminée :
  * Rejeté : Échec de l’équipe à se conformer aux qualifications permettant d’entrer le tournoi.
  * Éliminé, l’équipe a perdu la correspondance de tournoi.
  * Supprimé, l’équipe a été supprimée à partir d’un tournoi actif.
  * Terminé, l’équipe effectué round finale du tournoi.
