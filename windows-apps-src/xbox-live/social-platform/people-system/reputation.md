---
title: Réputation
description: Découvrez comment la Xbox Live utilise le service de réputation d’encourager jeu positif.
ms.assetid: f8966184-5db7-4cab-93ca-9a0250a6077d
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, réputation, plate-forme sociale
ms.localizationpriority: medium
ms.openlocfilehash: 4e7763294458f19509d5b797a6fa10e03678abee
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641814"
---
# <a name="reputation"></a>Réputation

Réputation est une statistique de l’utilisateur, tout comme les autres et est incluse dans les appels pour récupérer toutes les statistiques d’un utilisateur et de les utiliser dans la création de rapports et de suivi. La réputation lui-même est représentée par le **réputation classe**. Le **ReputationService classe**représente le service de réputation. Les URI correspondant sont décrites dans **réputation URI**.

> [!IMPORTANT]
> Statistiques de réputation sont globales pour le service, ne pas lié à un logiciel spécifique. La configuration du service ID (SCID) pour le service est le SCID globale permettant d’accéder aux statistiques de réputation.


## <a name="features-of-the-reputation-service"></a>Fonctionnalités du Service de réputation

Le service de réputation :

-   Gère des commentaires et des plaintes de la même façon. Entités envoyer des commentaires sur un utilisateur, et ces commentaires affecte la réputation de l’utilisateur. Les commentaires peuvent ensuite être transférés à l’équipe de mise en œuvre pour d’autres actions.
-   Permet aux utilisateurs d’envoyer des commentaires sur les autres utilisateurs. Titres peuvent envoyer des commentaires automatiquement.
-   Autorise les titres un accès direct à l’API. Un utilisateur peut envoyer des commentaires directement au sein d’un jeu et depuis le contexte de la zone de jeu où l’utilisateur est actuellement.
-   Handles de faible réputation comme influant sur les utilisateurs qui sont en mesure d’effectuer sur Xbox Live et dans les titres. Par conséquent, les utilisateurs doivent garder un œil sur sa réputation et l’acte de plus convenablement à le lors de la lecture en ligne.
-   Autorise les commentaires positifs, ainsi que des commentaires négatifs. Les utilisateurs qui aident à la Communauté Xbox ou la Communauté d’un titre peuvent être récompensés pour leurs efforts, et ils peuvent même avec des privilèges spéciaux.
-   Dérive une seule réputation, représentée par le **Reputation.OverallReputation propriété**. Elle est dérivée parmi les types suivants de la réputation :

    -   Équitable play. Représenté par le **Reputation.FairplayReputation propriété**.
    -   Communications. Représenté par le **Reputation.CommunicationsReputation propriété**.
    -   Contenu généré par l’utilisateur (UGC). Représenté par le **Reputation.UserGeneratedContentReputation propriété**.

Consultez **ResetReputation (JSON)** pour plus d’informations.


## <a name="usage-flow-for-the-reputation-service"></a>Flux d’utilisation pour le Service de réputation

Le flux global pour le service de réputation comprend deux phases : reporting matchmaking filtré à la réputation et sa réputation.


## <a name="reporting-reputation"></a>Réputation de création de rapports

Lorsque suffisamment de commentaires négatifs a été signalé pour un utilisateur spécifique, le titre définit le **Reputation.OverallReputation propriété** pour indiquer une mauvaise réputation pour cet utilisateur (attribut JSON OverallReputationIsBad). Moment sans plaintes, améliore lentement la réputation de l’utilisateur, et il est possible pour un utilisateur avec une seule fois mauvaise réputation obtenir une bonne réputation à nouveau.


## <a name="reputation-filtered-matchmaking"></a>Matchmaking filtré de réputation

Pour matchmaking filtré de réputation, spécifié par exigence pour le Xbox (XR), le titre récupère le lecteur **Reputation.OverallReputation propriété**. Cette valeur est un score qui indique l’état de la réputation du joueur.

> [!NOTE]
> Si le titre de la recherche pour l’attribut OverallReputationIsBad dans un fichier JSON et ne le trouve pas, il est logique de supposer que l’utilisateur dispose d’une bonne réputation. Cela peut se produire avec les nouveaux comptes, jusqu'à ce que des commentaires a été traité pour l’utilisateur. Les joueurs avec des commentaires d’autres utilisateurs ont toujours des statistiques de réputation et une valeur pour le **Reputation.OverallReputation** propriété.


## <a name="reputation-as-an-indicator-of-behavior"></a>Réputation en tant qu’indicateur de comportement

Réputation fournit un indicateur du comporte de l’utilisateur sur le système. Par exemple, soit la personne un lecteur convivial pas ? Commentaires des autres membres de session détermine la réputation d’un joueur. Chaque utilisateur a un score de réputation qui transite avec cette personne partout sur Xbox One. Il est exposé en évidence dans des emplacements réseau sociales amis peuvent voir afin qu’ils peuvent s’appliquer à la pression d’homologue à un lecteur se comportent mieux. Exemple :

-   Il se trouve sur la carte de visite de chaque utilisateur. Tout le monde dans le système peut examiner le profil utilisateur avec un seul clic.
-   Elle est affichée dans les jeux multijoueurs. Lorsque les utilisateurs jouent ensemble en ligne, la réputation du groupe est égale à la réputation du lecteur avec la plus faible réputation dans le groupe. Le groupe est mis en correspondance uniquement avec d’autres utilisateurs avec réputation similaire. Autres intervenants permet de décider de type de joueurs se trouvent dans ce type de jeu et décident s’ils souhaitent participer à la partie réputation.


## <a name="key-elements-of-reputation"></a>Éléments clés de réputation

Un titre peut déterminer si un utilisateur a atteint un Évitez Me niveau équitable play, les communications ou les catégories de contenu généré par l’utilisateur (UGC). L’utilisateur a été atteinte m’éviter pour la catégorie associée si l’une des propriétés suivantes est définie sur 1 :

-   **Propriété de Reputation.OverallReputation**. L’attribut JSON associé est OverallReputationIsBad.
-   **Propriété de Reputation.FairplayReputation**. L’attribut JSON associé est FairplayReputationIsBad.
-   **Propriété de Reputation.CommunicationsReputation**. L’attribut JSON associé est CommsReputationIsBad.
-   **Propriété de Reputation.UserGeneratedContentReputation**. L’attribut JSON associé est UserContentReputationIsBad.


## <a name="good-game-reports"></a>Rapport de jeu efficace

En plus du signalement des utilisateurs pour les actions incorrectes, les utilisateurs peuvent également envoyer eux bonne rapports jeu, qui peuvent être créés dans les titres comme des votes pour le joueur plus précieux.


## <a name="feedback-history"></a>Historique des commentaires

L’historique des commentaires fournit des informations telles que le moment où l’utilisateur a été signalé dernier et quelle était la personne signalée pour, par exemple, « récemment reçu des commentaires sur le style de Communication. »


## <a name="xbox-requirement-for-reputation"></a>Exigence de Xbox pour réputation

Exigence (XR Xbox) 068, Matchmaking filtrage par réputation, requiert la séparation des acteurs de faible réputation de ceux avec haute réputation. Cela en examinant la **Reputation.OverallReputation** valeur d’un joueur et attribut de OverallReputationIsBad du joueur dans les statistiques de l’utilisateur.

Le titre de la retrouver réputation de l’utilisateur, en passant « OverallReputation » à la *statisticName* paramètre de la **UserStatisticsService.GetSingleUserStatisticsAsync méthode (String, String, String)**. Les appels de méthode WinRT **obtenir (/users/xuid({xuid})/scids/{scid}/stats)** comme indiqué dans le corps JSON suivant.

```json
    {
      "requestedusers": [
        "2533274792693551"
      ],
      "requestedscids": [
        {
          "scid": "7492baca-c1b4-440d-a391-b7ef364a8d40",
          "requestedstats": [
            "OverallReputationIsBad",
            "FairplayReputationIsBad",
            "CommsReputationIsBad",
            "UserContentReputationIsBad"
          ]
        }
      ]
    }
```

Lorsque les commentaires d’un utilisateur à partir d’autres joueurs atteint un niveau faible, OverallReputationIsBad est défini sur 1 pour l’utilisateur. Les personnes dont **Reputation.OverallReputation** est 1 doit être mis en correspondance uniquement avec d’autres personnes ayant **OverallReputation** défini sur 1. Par défaut, lorsque les utilisateurs entrent une correspondance, ils généralement n’ont à gérer avec des personnes de faible réputation. Titres peuvent éventuellement permettre à un lecteur avec une bonne réputation participer à faire correspondre avec les lecteurs de réputation de faible.

Pour la version la plus récente de XRs qui s’appliquent à la réputation, consultez [Xbox exigences](https://developer.xboxlive.com/en-us/platform/certification/requirements/Pages/home.aspx) sur Game Developer Network (GDN).


## <a name="creating-users-with-bad-overall-reputation-for-testing"></a>Création d’utilisateurs avec une mauvaise réputation globale pour le test

Pour le test, les utilisateurs avec une très mauvaise réputation permet de configurer pour tester la mise en œuvre pour un logiciel de filtrage de correspondance. Pour définir une valeur spécifique pour un utilisateur, le titre de test ou l’outil appelle **POST (/users/xuid({xuid})/resetreputation)** avec un fichier JSON qui définit des valeurs spécifiques de réputation de l’utilisateur. Consultez **ResetReputation (JSON)** pour plus d’informations.

Par exemple, le corps JSON suivant définit la réputation d’équitable play d’un utilisateur à une valeur très faible :

```json
    {
      "fairplayReputation": 5,
      "commsReputation": 75,
      "userContentReputation": 75
    }
```

Partenaires peuvent effectuer cet appel pour tous les bacs à sable à l’exception de vente au détail. Cette requête définit les scores de réputation de base d’un utilisateur, et les pondérations du lecteur pour les commentaires positifs seront tous être remis à zéro. Réputation réelle de l’utilisateur après cet appel se compose de ces scores de base plus Ambassadeur bonus du joueur, et bonus SUIVEUR du joueur. Ce mécanisme crée un utilisateur avec un faible score et **Reputation.OverallReputation** défini sur 1 pour tester le XR de filtre de correspondance. Le titre peut obtenir le score de réputation d’utilisateur pour l’utilisateur à partir du service de statistiques utilisateur, comme décrit dans la section « Spécification de réputation Xbox » de cette rubrique.


## <a name="resetting-a-users-reputation-to-the-defaults"></a>La réinitialisation de la réputation de l’utilisateur pour les valeurs par défaut

Le titre définit l’attribut OverallReputationIsBad pour indiquer que l’utilisateur a une bonne réputation. Il appelle **POST (/ utilisateurs/me/resetreputation)** et définit toutes les valeurs à 75. Un seul appel à **/users/xuid({xuid})/deleteuserdata** peut être utilisée pour réinitialiser plusieurs ID d’utilisateur de Xbox. Le corps de demande est :

```json
    {
      "xuids": [
        "2814659110958830"
      ]
    }
```

## <a name="sending-feedback-from-titles"></a>Envoi de commentaires à partir de titres

Titres envoyer des commentaires d’équitable play sur des lecteurs à partir de correspondances. Pour cela directement depuis le titre ou à partir du service de titre par lots. Le titre peut utiliser le **ReputationService.SubmitReputationFeedbackAsync méthode** ou les méthodes REST de directes de ce qui suit :

|                      |                                                             |
|----------------------|-------------------------------------------------------------|
| Client POST          | https://reputation.xboxlive.com/users/xuid{XUID}/feedback |
| Service (lot) POST | https://reputation.xboxlive.com:10443/users/batchfeedback   |

Consultez **commentaires (JSON)** pour un exemple de corps JSON pour l’envoi et pour les définitions des champs autorisés.


## <a name="types-of-feedback-allowed"></a>Types de commentaires autorisé

Les catégories de commentaires qui peuvent être publiées sont définies dans **commentaires (JSON)**.


## <a name="when-titles-should-send-feedback"></a>Lorsque les titres doivent envoyer des commentaires

Un titre doit envoyer des commentaires négatifs lorsqu’un événement affecte négativement l’expérience de joueur dans un jeu. En règle générale, le titre doit envoyer des commentaires qu’une seule par type, par arrondi lu. Par exemple, le titre doit :

1.  Envoyer uniquement un commentaire FairPlayKillsTeammates pour une personne par tourniquet mettre fin à des collègues 3 ou plus, au lieu d’envoyer un événement chaque fois que la personne qui met fin à un collègue.
2.  Envoyer le commentaire FairplayQuitter lorsqu’une personne quitte volontairement une correspondance au début.
3.  Envoyer le commentaire FairplayUnsporting, une fois par concurrence, lorsqu’un utilisateur dirige vers l’arrière dans un jeu de course.
