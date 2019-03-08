---
title: Xbox Arena
description: Découvrez comment Xbox Arena permet d’exécuter tournois pour votre jeu.
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, arena, tournoi, l’expérience utilisateur
ms.localizationpriority: medium
ms.openlocfilehash: b08da01323d05c961005d562b70667dbbdf85437
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643764"
---
# <a name="xbox-arena"></a>Xbox Arena

Xbox est une plateforme de création et en cours d’exécution en ligne tournois sur Xbox One et Windows 10 via Xbox Live, avec l’objectif consistant à placer la partie amusante du jeu de concurrence pour un public plus large.
Chaque serveur de publication a des besoins différents lorsqu’il s’agit au jeu compétitif et avec Arena nous voulons être aussi flexible que possible. Par conséquent, il existe trois façons différentes d’exécuter tournois pour vos titres sur Arena :

* Avec tournois de l’exécution de franchise quelconques, Xbox Live vous offre les outils et services pour configurer et exécuter vos propres tournois.

* Xbox a conclu un partenariat avec les organisateurs de tournoi leader du secteur (TOs), qui permet d’exécuter tournois pour le compte de votre titre Arena. (Pour une liste des organisateurs de tournoi pris en charge, contactez votre responsable de compte Microsoft.)

* Lecteurs de créer et exécuter leurs propre tournois Arena avec notre tournois généré par l’utilisateur, ou UGTs.

Plus important encore, ajout de prise en charge de l’arène à vos titres vous permet de pour tirer parti des trois. Nous fournissons des API robustes qui permettent aux titres de promouvoir des tournois de jeu et de faciliter la transition de participants vers et depuis les correspondances. Le Hub d’Arena Xbox prend en charge les tâches de gestion de tournoi, comme l’inscription, les notifications, crochets et l’amorçage et création de rapports.

> [!IMPORTANT]  
> Xbox Arena est uniquement disponible pour les partenaires gérés et ID@Xbox les développeurs. Il n’est pas disponible pour le programme Xbox Live Creators.

## <a name="a-titles-baseline-tournament-experience"></a>Expérience de tournoi de ligne de base d’un titre

Si votre titre s’intègre à un ou plusieurs organisateurs de tournoi, elle doit fournir une expérience de tournoi de ligne de base. Vous êtes libre d’intégrer plus profondément un organisateur tournoi spécifique si vous choisissez, pour fournir une expérience plus riche pour les concours. Mais vous devez toujours fournir l’expérience de ligne de base pour les autres organisateurs de tournoi et pour UGTs et franchise-run tournois si vous choisissez de les exécuter.

### <a name="baseline-requirements-for-a-title"></a>Spécifications de base pour un titre

* Contactez votre responsable de compte Microsoft pour obtenir la liste complète des exigences.

### <a name="ui-recommendations"></a>Recommandations de l’interface utilisateur

* Identifier que la correspondance est un tournoi dans l’interface utilisateur.

* Dans votre interface utilisateur d’introduction, inclure un élément d’interface utilisateur que les utilisateurs des liens à votre hub de tournoi et/ou la page de détail de tournoi dans l’interpréteur de commandes Xbox Arena l’interface utilisateur ou le tournoi organisateur application.



L’expérience utilisateur de base que le titre doit fournir est simple et suffisamment flexible pour travailler avec un grand nombre de formats de concurrence possibles. Vous êtes libre d’adapter le Guide de l’expérience utilisateur et l’expérience de configuration requise en fonction de flux de l’interface utilisateur de votre titre et pour s’assurer qu’un utilisateur sans heurts.

Exemple :

* Les informations requises n’ont doit être affiché sur l’écran principal, à condition qu’il soit disponible quelque part, comme sur une page de détails ou fenêtre contextuelle.

* Il peut y avoir de nombreuses versions de chaque écran, ou ils peuvent être combinées entre eux ou avec des écrans de jeu existants. Par exemple, de nombreux jeux incluent une correspondance après « rapport un carnage » de l’écran, ce qui peut être adapté pour satisfaire les « en attente de résultats » et « Prêt » de l’écran Configuration requise.

* Écrans n’êtes pas obligés de modifier dès que celle-ci effectue. Par exemple, si l’étape de l’équipe passe à partir de « Prêt » à « Lecture » tandis que l’utilisateur est sur un écran « Prêt », votre titre ne doit pas nécessairement passer immédiatement dans le jeu. Elle peut (et devriez probablement) Basculer le « en attente de correspondance... » indicateur à un bouton, par exemple, « prêt à lire » — afin que l’utilisateur est dans le contrôle du flux et par conséquent peuvent avoir une meilleure compréhension de celui-ci. Il est OK pour la configuration requise de l’étape « Lecture » pour être reporté à plus tard jusqu'à ce que l’utilisateur confirme la transition.


## <a name="arena-vs-title-roles"></a>Arena et le titre « rôles »

Déplacement vers et depuis le jeu, comme leur progression dans les stades du tournoi, gagne en complexité pour les utilisateurs. Lorsque le processus est différent pour chaque jeu qu'ils jouent, il est encore moins probable à mémoriser où aller et ce qui se passe.

> [!TIP]
> **Recommandation de l’expérience utilisateur**  
>
> Simplifier des rôles fonctionnels entre le jeu et de l’interface utilisateur de domaine Xbox, en les gardant nettement. Par exemple, toutes les tâches liées à la gestion sont effectuent dans son domaine, et toutes les tâches liées à la lecture jeu sont effectuées à l’intérieur du jeu.

Rôle de Xbox Arena (configuration d’un tournoi)   | Rôle du titre (jeu)
--- | ---
<ul><li>L’inscription et archivage</li><li>Notifications</li><li>Amorçage et crochets</li><li>Formation de l’équipe</li></ul> |     <ul><li>Transition de participants vers et à partir de l’interface utilisateur de domaine</li><li>Identification des correspondances de tournoi spécifiques dans l’interface utilisateur de Hall multijoueur</li><li>Promotion et/ou de navigation de tournois de jeu</li></ul>

## <a name="engineering-guidance"></a>Conseils de conception

Article | description
--- | ---
[Intégration de titre Arena](arena-title-integration.md) | Découvrez comment intégrer la prise en charge de Xbox Arena votre titre.

## <a name="operations-guidance"></a>Sur les opérations

Article | description
--- | ---
[Portail de Xbox Arena Operations](operations-portal.md) | Décrit le portail d’opérations que vous pouvez utiliser pour créer et gérer des tournois officiels pour un logiciel qui est intégré à son domaine Xbox.

## <a name="user-experience-guidance"></a>Guide de l’expérience utilisateur

Article | description
--- | ---
[Découverte de tournois de Xbox](discovering-xbox-tournaments.md) | Offre des conseils et des recommandations pour créer un utilisateur extraordinaires une expérience de découverte des tournois existants.
[Joindre un tournoi](arena-ux-join-tournament.md)  |  Offre des conseils et des recommandations pour créer un utilisateur extraordinaires une expérience de l’inscription et de joindre un tournoi.
[Engagement de correspondance](arena-ux-match-engagement.md) | Décrit les étapes d’expérience utilisateur de mise en œuvre un tournoi de joueurs.
[Métadonnées de l’interface utilisateur de l’API Arena](arena-apis-metadata.md)  | Décrit les métadonnées retournées par les API de son domaine que vous pouvez utiliser pour afficher des informations dans le jeu sur l’état actuel du tournoi.
[Notifications d’Arena](arena-notifications.md)  | Décrit les conditions lors de la Xbox Arena envoie une notification aux participants de tournoi.
[Scénarios d’utilisateur de domaine](arena-user-scenarios.md)  | Décrit les scénarios d’Arena aux joueurs selon leurs motivations courantes.
