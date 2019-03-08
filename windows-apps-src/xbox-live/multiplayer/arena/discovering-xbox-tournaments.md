---
title: Découverte de tournois de Xbox
description: Découvrez comment créer une interface utilisateur pour la découverte de tournoi de votre jeu.
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, arena, tournoi, l’expérience utilisateur
ms.localizationpriority: medium
ms.openlocfilehash: 71a065d6d4da290f2ce3345c7da0026d4c116335
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632474"
---
# <a name="discovering-xbox-tournaments"></a>Découverte de tournois de Xbox

Les joueurs disposent de plusieurs options dans le tableau de bord Xbox ou de l’application Xbox sur un PC, pour en savoir plus sur participent ou l’observation un tournoi. Il existe également des opportunités d’accroître la découverte de tournoi à partir de votre titre.  

Passionné trouverez prenant en charge l’arène des tournois par :

* Ouverture tournois Hub d’un jeu récemment lu depuis leur domicile Xbox.
* Visite un profil de lecteur pour tournois lu, lecture en cours ou enregistrées pour.
* Répondre à une équipe de recherche de groupe (LFG) publier un club appartiennent à.
* Regarder un flux de tournoi dans l’application de Mixer, site Web ou l’espion pivot dans le jeu Hub/domaine.
* Visite un Hub jeux dans le tableau de bord Xbox.

## <a name="promoting-tournaments-in-game"></a>Promotion de tournois de jeu

Les utilisateurs regardent tournois parce qu’ils sont intéressés par le titre. Jeux Xbox ont une opportunité unique pour intercepter les joueurs à cet instant essentiel quand ils cherchent de nouvelles façons d’étendre leur expérience de jeu.

> [!TIP]
> **Recommandation de l’expérience utilisateur :** Promouvoir tournois à des moments de prise de décision.
>
> Sensibiliser pour tournois parfois lorsque les joueurs doivent décider s’il faut conserver la lecture ou de quitter (par exemple, au lancement de jeu, menu principal, multijoueur du menu ou fin de correspondance).

### <a name="1--use-a-dynamic-announcement-feature-example-message-of-the-day"></a>1.  Utiliser une fonctionnalité d’annonce dynamique (exemple : Message de la journée)

Les titres qui ont intégré une fonctionnalité d’annonce à leur interface utilisateur jeu généralement utilisent un système de gestion de contenu qui permet des mises à jour instantanées. Cela est utile pour transmettre du contenu nouveau aux joueurs sans se baser sur une mise à jour de contenu ou une version téléchargeable du contenu. Par exemple, Halo utilise une fonctionnalité de « Message du jour » dans les annonces de communauté aire de conception et de conseils. Promotion de tournoi est idéale pour ce scénario.

**Impact sur les utilisateurs**

* Les joueurs sont introduites dans une nouvelle opportunité de jeux compétitif avant de lancer une session de jeu.
* Exposition est intermittente (au lancement, combiné avec d’autres annonces).
* Pour en savoir plus, les joueurs accède à laisser le jeu et accédez au Hub Arena.
* La fonctionnalité ne nécessite pas une mise à jour de contenu pour remplir.
* Minutage de la promotion et de pertinence reste flexible.

###### <a name="ui-example-message-of-the-day"></a>Exemple d’interface utilisateur : « Message de la journée »

![Message de tournoi de la journée](../../images/arena/arena-ux-motd.png)

#### <a name="a-main-heading-ex-play-watch-learn"></a>A. Main position ex : Jouer, regarder, en savoir plus  

Informe les joueurs sur tous les tournois ou une valeur spécifique.

#### <a name="b-go-to-arena-hub"></a>B. Accédez au Hub d’Arena  

Lien ciblé pour jeux Hub/tournois - recommandées lors de la promotion de tous les tournois liés à votre jeu.  
- ou -  
Lien ciblé de jeu/tournoi/détails du Hub - recommandées quand il annonce un tournoi spécifique.

#### <a name="c-game-exit-confirmation"></a>C. Confirmation de la sortie de jeu

Éléments de l’écran A et B le conduit out le jeu au tableau de bord Xbox. Définir cette attente dans l’interface utilisateur de votre titre avant d’effectuer la gamer une décision.

###### <a name="ui-example-exit-game-confirmation"></a>Exemple d’interface utilisateur : Confirmation de jeu de sortie
![Confirmation de jeu de sortie tournoi](../../images/arena/arena-ux-exit-confirm.png)

### <a name="2--promote-tournaments-on-the-main-menu"></a>2.  Promouvoir tournois dans le Menu principal

Dans cet exemple, l’annonce est une publicité interactive pour un événement à venir. Il offre suffisamment d’informations pour inciter vos joueurs pour en savoir plus. Sur **sélectionnez**, le joueur est dirigé vers une page de détail tournois dans le Hub Arena, où ils peuvent gérer leur inscription.

###### <a name="ui-example-a-tournament-ad-displayed-alongside-the-main-menu"></a>Exemple d’interface utilisateur : Une publicité de tournoi affichée en même temps que le menu principal

![](../../images/arena/arena-ux-promo.png)

> [!TIP]
> **Recommandation de l’expérience utilisateur**  
> L’annonce doit inclure suffisamment de détails permet aux joueurs de choisir s’ils souhaitent en savoir plus. Par exemple, ***description***, ***popularité***, et ***minutage***.

## <a name="browsing-tournaments-in-game"></a>Tournois de navigation dans le jeu

Les API de Arena fournissent suffisamment de données pour votre titre créer une expérience de navigation à l’intérieur du jeu. Si vous envisagez de créer une fonctionnalité d’exploration, ajouter un **tournois** point d’entrée dans la section multijoueur.

> [!TIP]
> **Recommandations de l’expérience utilisateur**  
> Tournois présentes comme un mode de jeu multijoueur.  
> Comme Xbox Arena est une nouvelle fonctionnalité, veillez à laisser visibles au niveau du niveau 1 ou de niveau 2.

###### <a name="ui-example-tournaments-listed-as-an-additional-mode-in-the-multiplayer-section"></a>Exemple d’interface utilisateur : Tournois répertoriés comme un mode supplémentaire dans la section multijoueur

![Mode de tournoi](../../images/arena/arena-ux-tournament-mode.png)

### <a name="provide-a-comprehensive-list-of-tournaments"></a>Fournir une liste complète des tournois

Permettre aux joueurs pour vérifier rapidement l’état de tournois, dans qu'ils sont actuellement inscrits et à parcourir tournois entre les sessions de jeu.

#### <a name="user-impact"></a>Impact sur les utilisateurs

* Réduit besoin les joueurs de quitter le jeu pour vérifier l’état de tournoi dans le tableau de bord.
* Constitue une excellente solution de sauvegarde joueurs absence de notifications de toast Xbox Arena.
* Implique un coût supplémentaire pour le développeur de jeux à gérer.
* Conduit les joueurs à l’interface utilisateur de son domaine pour joindre, l’inscription, archivage, l’amorçage et l’équipe de formation.

> [!NOTE]  
> Les API Arena ne fournissent pas d’une requête pour tournois généré par l’utilisateur (club généré).


> [!TIP]
> **Recommandation de l’expérience utilisateur**  
> Créer une interface utilisateur qui peut mettre à l’échelle et fournissent des méthodes pour les joueurs gérer une longue liste de tournois.

### <a name="filters"></a>Filtres

Filtrer tournois par **tous les**, **Active** (qui couvre tous les éléments jusqu'à ce que le tournoi se termine), **Canceled**, et **terminé** États, et que tournois passionné a participeront à (par exemple, mon tournois).

###### <a name="ui-example"></a>Exemple d’interface utilisateur :

![Écran de filtres de tournoi](../../images/arena/arena-ux-filters.png)

> [!NOTE]  
> Ajout de filtres personnalisés, en dehors de ce que l’API prend en charge est possible, *mais déconseillée*.

Votre titre peut filtrer par d’autres propriétés une fois qu’il effectue la demande, mais il ne peut pas les spécifier sur le paramètre de requête. Cela rend le filtrage par d’autres propriétés non fiables pour ce type d’interface utilisateur. Les API Arena récupérer un nombre spécifié de titre de résultats à la fois, par exemple, le titre peut spécifier un **maxItems** valeur de tournois de 10. Cela permet à votre titre de gérer les performances et réduit le risque d’autant submerger l’utilisateur avec une grande liste. Toutefois, tous les filtres personnalisés fournis par votre jeu seraient appliquent uniquement à 10 éléments demandés à ce moment-là. Cela peut entraîner un nombre incohérent de résultats filtrés après chaque appel d’API. Par exemple, le premier ensemble de 10 tournois retourné peut contenir 2 associés à un filtre personnalisé, comme « Lecture ». Mais les 10 suivants peuvent afficher 9 ces articles et ainsi de suite. La seule méthode fiable pour votre titre à remplir entièrement chaque filtre personnalisé consiste à demander chaque tournoi actif et puis filtrez-les all, qui n’est pas recommandé.

#### <a name="filter-recommendations"></a>Filtrer les recommandations :

##### <a name="all"></a>ALL

Afficher toutes les **Active**, **Canceled**, et **terminé** tournois, triés par la plupart récente.

Résultats :

* État du tournoi
* Date de début de tournoi
* État du tournoi, par exemple, l’enregistrement ouvert
* Lien ciblé à la Page de détails de tournoi dans le tableau de bord

##### <a name="my-tournaments"></a>MON TOURNOIS

Tournois de vue actuellement enregistré dans un participant.

Résultats :

* Tournois qui participe à l’utilisateur actuellement connecté
* État du tournoi
* Une méthode pour saisir une correspondance de tournoi (pour l’état de « correspondance prêt »)
* Lien ciblé vers la Page de détails de tournoi dans le tableau de bord

##### <a name="active"></a>ACTIVE

Afficher tous les tournois qui ont été créés : à venir, ouvert pour l’inscription, archivage et la lecture.

Résultats :

* Tournois qui sont ouverts pour vous inscrire que l’utilisateur actuel n’a pas déjà joint
* Temps restant jusqu'à ce que la clôture des inscriptions

##### <a name="completedcanceled"></a>TERMINÉ/ANNULÉE

Afficher les résultats récents. Tournois qui vient de se clôturer peuvent ils expirent au fil du temps, au lieu de simplement disparaît une fois terminées.

Résultats :

* Lien ciblé vers la Page de détails de tournoi dans le tableau de bord
* État du tournoi
* Date/heure de fin

##### <a name="canceled"></a>ANNULÉE

Tournois de vue qui ont été annulées.

Résultats :

* Lien ciblé vers la Page de détails de tournoi dans le tableau de bord
* Date/heure de l’annulation

> [!div class="nextstepaction"]
> [Joindre un tournoi à l’aide de l’interface utilisateur de domaine](arena-ux-join-tournament.md)
