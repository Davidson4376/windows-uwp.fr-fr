---
title: Envoi de commentaires de lecteur à partir de votre titre
description: Découvrez comment votre titre peut aider à promouvoir des expériences multijoueurs positif en envoyant des commentaires de lecteur pour le service de réputation Xbox Live.
ms.assetid: 49f8eb44-1e31-4248-b645-9123df6f8689
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, réputation, les commentaires de lecteur
ms.localizationpriority: medium
ms.openlocfilehash: 3e8d82fc9b195f174bf7a46a8d21fb20b2df9551
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592234"
---
# <a name="sending-player-feedback-from-your-title"></a>Envoi de commentaires de lecteur à partir de votre titre
La majorité des membres de Xbox Live est extraordinaire, mais il existe un petit pourcentage de « Pommes incorrecte » qui nuisent expériences de jeux d’autres personnes. Nous identifier ces petits pourcentages des utilisateurs via utilisateur et le titre des commentaires envoyés. Nous aider à protéger le reste de la population en garantissant que ces « incorrectes pommes » une expérience multijoueur limitée où ils ne peuvent pas interférer avec les jeux de bonne joueurs. Xbox s’appuie fortement sur les utilisateurs à d’autres utilisateurs pour assurer l’exactitude du système de rapport, mais les titres dans Xbox One peuvent participer directement et considérablement améliorer la précision de la population de réputation d’utilisateur.

## <a name="steps-to-submit-feedback-from-title-or-title-service"></a>Étapes pour envoyer vos commentaires à partir de titre ou Service de titre
1. Ajouter des instants de commentaires à titre ou service de titre
2. Déterminer le type de commentaire correct
3. Appeler des API de commentaires de réputation avec commentaires

### <a name="adding-feedback-moments-to-title-or-title-service"></a>Ajout de commentaires instants à titre ou Service de titre
Tous les joueurs ont eu des mauvaises expériences avec des coéquipiers qui saboter leurs propres côté, les joueurs qui simplement autour au lieu d’en cours d’exécution ou tricheurs qui endommager leurs jeux. Xbox Live permet aux utilisateurs de signaler ces acteurs problématiques directement, mais les commentaires des utilisateurs n’est pas parfait. Titres peuvent facilement déterminer les choses simples telles que des joueurs ralenti jeu et quitter tôt et parfois même déterminer quand une personne est de la triche. Votre titre peut envoyer des commentaires dans un large éventail d’instants des commentaires, ce qui vous aideront à améliorer l’expérience de tous les acteurs de bonne.

### <a name="determining-the-correct-feedback-type"></a>Détermination du Type Correct de commentaires
Le système de réputation a de nombreux types de commentaires qui permettent de capturer les différentes méthodes qu’un utilisateur peut justifier vos commentaires. Ils sont entièrement répertoriés dans le tableau 1 ci-dessous. La plupart d'entre eux est négatif, mais il est possible d’améliorer la réputation d’un utilisateur avec des commentaires positifs également.

Interface utilisateur du système Xbox offre un moyen pour les joueurs à envoyer vos commentaires sur les autres utilisateurs dans le jeu. Ces commentaires utilisateur et l’utilisateur ne comporte pas de pondération, étant donné que les utilisateurs sont susceptibles d’engendrer des griefing eux lorsqu’ils perdent. Titres peuvent compléter ce système de l’interface utilisateur, en fournissant l’interface utilisateur pour les utilisateurs pour envoyer directement des commentaires sur un autre, mais au lieu de cela, nous préférons que les titres envoyer des commentaires pour le compte le titre lui-même à l’aide des partenaires. Des partenaires de confiance sont élevé.

## <a name="example-partner-feedback-usage-scenarios"></a>Exemples de scénarios d’utilisation des commentaires partenaire

### <a name="user-quitting-a-game-in-the-middle-of-a-match"></a>Utilisateur de quitter un jeu au milieu d’une correspondance
Un lecteur est perdu à un jeu et utilise les menus du jeu pour quitter le jeu, abandonner ses coéquipiers. Lorsqu’un titre détecte ce comportement, ils peuvent signaler le comportement à l’aide **FairPlayQuitter.**

### <a name="idling-after-match-found-in-game"></a>Correspondance trouvée dans le jeu, consécutive
Un lecteur obtient correspond à d’autres joueurs à jouer, mais régulièrement signifie inactif dans le jeu au lieu d’aider l’équipe. Le titre peut signaler le comportement de lecteur à l’aide de **FairplayIdler.**

### <a name="killing-teammates-in-game"></a>Suppression des collègues dans le jeu
Un lecteur dans un jeu est constamment tuer des collègues pour vous amuser. Lorsqu’un titre détecte qu’un utilisateur est systématiquement team-mort, il peut signaler le lecteur à l’aide de **FairPlayKillsTeammates.**

### <a name="title-has-community-kickvote-feature"></a>Titre a Communauté Kick/Vote fonctionnalité
Un lecteur a été voté par les joueurs à arrondir à supprimer de la session pour le mauvais comportement. Si le titre supprime ce lecteur à partir de la session, il peut signaler à l’utilisateur **FairPlayKicked.**

### <a name="helpful-player-community-vote"></a>Utile Player Vote de la Communauté
Après quelques essais d’un jeu, le titre propose une option pour choisir une personne qui a aidé le meilleur parti. Lorsqu’un titre voit cette action, il peut indiquer le comportement à l’aide **PositiveHelpfulPlayer.**

### <a name="high-quality-ugc-user-generated-content"></a>Haute qualité par (contenu généré par l’utilisateur)
Un titre a une scène dans laquelle un lecteur peut choisir la meilleure conception pour un véhicule. Lorsqu’un titre voit cette action, il peut indiquer le comportement à l’aide **PositiveHighQualityUGC.**

### <a name="skilled-player"></a>Lecteur compétent
Après quelques essais d’un jeu, le titre propose une option pour choisir une personne qui est le motif MVP qui a été le joueur meilleures. Quand un titre voit qu’un consistenly player gagne statut de MVP il peut signaler le comportement à l’aide **PositiveSkilledPlayer.**

### <a name="user-reports-questionable-ugc-in-title"></a>Utilisateur des rapports UGC douteux dans le titre
Un titre a une scène dans lequel un lecteur peut afficher des conceptions de véhicule. Un lecteur Remarque une conception offensante et souhaite le signaler. Lorsqu’un titre voit cette action, il peut signaler le coupable à l’aide de **UserContentInappropriateUGC.**

### <a name="title-wants-to-request-an-xbl-ban-review-of-a-player-in-their-title"></a>Titre souhaite demander une révision d’interdiction XBL d’un lecteur dans leur titre.
Responsable d’un titre de la Communauté a remarqué un lecteur de réputation de faible constamment à l’origine des problèmes de leur Match. Un titre peut demander une stratégie de XBL et la vérification de l’équipe mise en œuvre à l’aide **FairPlayUserBanRequest.**

## <a name="complete-behavior-feedback-options"></a>Terminer les Options de commentaires de comportement
Le tableau ci-dessous répertorie les types de commentaires que vous pouvez utiliser pour envoyer des commentaires des utilisateurs pour le compte de votre titre. Le service de réputation est flexible et peut ajouter facilement de nouveaux types de commentaires si vous pensez qu’il ne convient pas de votre titre. Veuillez indiquer votre responsable de compte si vous aimeriez voir un nouveau type de commentaires ajouté.

Tableau 1 : Les commentaires de partenaire divers types le prend en charge du service de réputation.

**Types de commentaires de FairPlay**               | **Description**
----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------
FairplayKillsTeammates                    | Signaler un lecteur qui est tuer intentionnellement leurs collègues
FairplayCheater                           | Rapport un lecteur qui est certainement de la triche
FairplayTampering                         | Signaler un lecteur qui a certainement meddled avec le disque de jeu ou a falsifié sinon jeu logiciel ou du matériel
FairplayUserBanRequest                    | Signaler un joueur qui vous pensez a gagné une suspension
FairplayConsoleBanRequest                 | Une console que vous pensez de rapport doit être exclue de se connecter à Xbox Live
FairplayUnsporting                        | Signaler un lecteur qui exerce certainement conduite unsportsmanlike
FairplayIdler                             | Un lecteur qui entre en mode multijoueur correspond, mais il ne lit pas activement de rapports
FairplayLeaderboardCheater                | Signaler un lecteur qui a triché certainement apparaisse élevée sur un classement
**Types de commentaires de communications**         |
CommsInappropriateVideo                   | Signaler un lecteur qui est inapproprié dans la conversation vidéo
**Types de commentaires sur le contenu généré par l’utilisateur** |
UserContentInappropriateUGC               | Une partie inapproprié de contenu un lecteur crée votre titre de rapport
UserContentReviewRequest                  | Signaler un morceau de contenu proactive afin qu’elle sera examinée par l’équipe XBLPET
UserContentReviewRequestBroadcast         | Signaler une diffusion de façon proactive afin qu’elle sera examinée par l’équipe XBLPET
UserContentReviewRequestGameDVR           | Signaler un clip GameDVR de manière proactive afin qu’elle sera examinée par l’équipe XBLPET
UserContentReviewRequestScreenshot        | Signaler une capture d’écran de manière proactive afin qu’elle sera examinée par l’équipe XBLPET
**Commentaires positifs**                     |
PositiveSkilledPlayer                     | Si les utilisateurs peuvent voter pour déterminer un MVP, signaler un lecteur compétent lorsque certains que le joueur mérite commentaires positifs
PositiveHelpfulPlayer                     | Si un jeu fournit l’interface utilisateur pour un lecteur signaler qu’une autre était utile, signaler le joueur utile
PositiveHighQualityUGC                    | Si un jeu fournit l’interface utilisateur pour un lecteur compléter le contenu d’un autre utilisateur, rapport positivement le contenu

## <a name="feedback-api-calls"></a>Appels d’API de commentaires
Titres peuvent utiliser deux stratégies pour appeler le service de réputation. La méthode recommandée consiste à des utilisateurs de rapports dans l’agrégat à partir d’un service partenaire à l’aide d’un jeton de service pour l’authentification. Titres peuvent également signaler les utilisateurs directement à partir du client. L’API cliente a technologie antifraude intégrée qui nécessite plusieurs rapports sur un utilisateur à être considéré comme valide. Ces deux API par lots et peut signaler plusieurs utilisateurs simultanément.

Le titre peut utiliser les API suivantes Xbox Live pour envoyer des commentaires de réputation de lecteur :

Langue | API
-------- | --------------------------------------------------------------------------------------------
WinRT    | Microsoft::Xbox::Services::Social::ReputationService.SubmitBatchReputationFeedbackAsync(...)
C++      | xbox::services::social::reputation_service.submit_batch_reputation_feedback(...)

Le titre pouvez également utiliser les méthodes REST directes suivantes :

API          | URL                                                      | Exigences d’authentification
------------ | -------------------------------------------------------- | ---------------------------------------
Service POST | https://reputation.xboxlive.com/users/batchfeedback      | S-jeton avec des revendications de partenaire et bac à sable
Client POST  | https://reputation.xboxlive.com/users/batchtitlefeedback | Xtoken avec les revendications de titre et le bac à sable

## <a name="feedback-object"></a>Objet de commentaires
L’objet de commentaires a la spécification suivante pour la version la plus récente, 101. Ces deux API attendre un lot d’objets suivants.

Membre       | Type   | Description
------------ | ------ | -----------------------------------------------------------------------------------------------------------------------------------------------------------------
sessionRef   | objet | Objet décrivant la session MPSD ces commentaires concerne, ou null.
feedbackType | chaîne | Le type de commentaires. Les valeurs possibles sont définies dans l’énumération ReputationFeedbackType
textReason   | chaîne | Texte fourni par l’utilisateur que l’expéditeur est ajouté pour expliquer la raison pour laquelle le commentaire a été envoyé. Ceci est très utile pour notre équipe de mise en œuvre de stratégie.
evidenceId   | chaîne | L’ID d’une ressource qui peut être utilisée en tant que preuve des commentaires en cours d’envoi, par exemple, un fichier vidéo enregistrées au cours de jeu, ou un élément de flux d’activité.
titleID      | Chaîne | L’ID de titre du titre joué ; obligatoire uniquement si elle est absente dans le jeton.
targetXuid   | Chaîne | XUID du lecteur que vous signalez.

Exemple :

```json
POST https://reputation.xboxlive.com/users/batchtitlefeedback
{
    "items" :
    [
        {
            "targetXuid": "33445566778899",
            "titleId" : null,
            "sessionRef": {
  "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
  "templateName": "CaptureFlag5",
  "name": "Title56932",
           },
            "feedbackType": "FairPlayKillsTeammates",
            "textReason": "Title detected this player killing team members 19 times",
            "evidenceId": null
        }
    ]
}
```

## <a name="feedback-qa"></a>Commentaires Q & r

### <a name="q-can-i-send-a-hint-to-the-system-to-help-with-humans-that-might-be-looking-at-the-player-report"></a>Q : Puis-je envoyer un indicateur dans le système d’aide avec des personnes susceptibles de consulter le rapport de lecteur ?
R : Oui, et il est très utile ! Utilisez le paramètre « textReason » pour aider au Enforcer qui sera finalement consulter les commentaires laissés. Par exemple, pour un fou, vous pouvez ajouter une raison de texte indiquant « Nous avons ne reçu aucune entrée d’utilisateur de ce lecteur après les cinq premières secondes du jeu ». C’est pourquoi de texte peut s’avérer très utile pour les agents de mise en œuvre XBLPET, assurez-vous que la raison de texte est utile et descriptif.

### <a name="q-should-i-worry-about-how-often-i-send-in-feedback-on-a-user"></a>Q : Dois-je inquiétez sur la fréquence à laquelle envoyer des commentaires sur un utilisateur ?
R : Titres doivent appeler le service de réputation lorsqu’ils sont convaincus qu’un utilisateur a reçu des commentaires. Le système a plusieurs captures de sécurité pour empêcher que des titres et les utilisateurs soient en mesure d’impact excessive pour les utilisateurs.

### <a name="q-can-i-adjust-the-weight-of-the-feedback-being-sent"></a>Q : Puis-je ajuster le poids des commentaires envoyés ?
R : Non, le service de réputation détermine l’épaisseur des commentaires. Nous sommes toujours ajustement des pondérations pour améliorer le système.

### <a name="q-can-i-undo-feedback-ive-sent-on-a-user"></a>Q : Puis-je annuler les commentaires que j’ai envoyées sur un utilisateur ?
R : Non, les commentaires sont finale. Si vous pensez que votre titre comporte un bogue et envoie des commentaires erronés, faites-le nous savoir et nous allons mettre sur liste rouge le titre de la corriger le bogue.

### <a name="q-can-i-see-the-feedback-sent-for-my-title-from-users"></a>Q : Puis-je voir les commentaires envoyés pour mon titre des utilisateurs ?
R : Ces informations ne sont actuellement pas disponible en libre-service. Vous pouvez demander à votre compte de gestionnaire et nous ont de données par titre, nous pouvons partager. Bientôt, nous aimerions rendre disponibles directement à vous et également afficher combinaison d’utilisateurs avec une faible réputation à l’aide de votre titre.

### <a name="q-when-i-send-in-console-or-user-ban-review-request-how-do-i-know-what-happened"></a>Q : Lorsque j’envoie dans la console ou demande de révision exclure l’utilisateur comment je sais que s’est-il passé ?
R : Actuellement les informations pour la révision sont envoyées à l’équipe Stratégie de XBL et mise en œuvre, mais actuellement vous ne pas mis à jour sur la révision de l’interdiction d’accès.

### <a name="q-is-there-a-reputation-score-per-title"></a>Q : Existe-t-il un score de réputation par titre ?
R : Non. Il existe actuellement des sous scores pour réputation pour fairplay, communications et du contenu généré par l’utilisateur, mais pas par titre. Nous pouvons ajouter que cette fonctionnalité à l’avenir s’il existe suffisamment à la demande, par conséquent, informer votre responsable de compte si vous souhaitez demander cette fonctionnalité.
