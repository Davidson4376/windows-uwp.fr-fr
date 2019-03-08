---
title: FAQ et résolution des problèmes 2015 relatifs au mode multijoueur
description: Forum aux questions sur Xbox Live multijoueurs 2015 et le dépannage.
ms.assetid: 75823f10-b342-4e20-b885-e5ad4392bc3d
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, mode multijoueur
ms.localizationpriority: medium
ms.openlocfilehash: 171d80f4fc925d95d80043f40bb387b045a4fe23
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57625524"
---
# <a name="multiplayer-2015-faq-and-troubleshooting"></a>FAQ et résolution des problèmes 2015 relatifs au mode multijoueur

-   Je développe un nouveau titre. Les éléments d’API multijoueurs dois-je utiliser ?
-   Comment puis-je accéder à la nouvelle API multijoueur à partir d’un service ?
-   Mon titre instantanés pour plus d’une session ?
-   Est un utilisateur supprimé immédiatement s’il perd une connexion réseau ou désactive la console ?
-   Comment trouver les SCID, le modèle de session et le bac à sable à utiliser ?
-   Existe-t-il un exemple d’un corps de demande puis-je comparer à celui de mon titre ?
-   J’obtiens un code HTTP/403 lors de l’appel MPSD.
-   J’obtiens un code HTTP/404 lors de l’appel MPSD.
-   Pourquoi un code HTTP/404 lors de l’appel **MultiplayerService.WriteSessionByHandleAsync** ou **MultiplayerService.GetCurrentSessionByHandleAsync**?
-   J’obtiens un code HTTP/412 lors de l’appel MPSD.
-   J’obtiens un HTTP/400, 405, 409, 503, code etc. lors de l’appel MPSD.
-   Ce qui peut ou dois-je modifier dans les modèles de session pour mon titre ?
-   J’obtiens une erreur qui dit à que ma session n’est pas initialisée.
-   Ma session n’est pas créée et j’obtiens un code HTTP/204.
-   Quand dois-je interroger MPSD ?
-   Que se passe-t-il si un lecteur qui a été réservé ou invité à la session ne joint pas ?
-   Pourquoi une session créée pas trouvée en matchmaking ?
-   Qu’est la principale différence entre la façon dont parties sont correctement utilisées en mode multijoueur 2015 et la façon dont ils ont été utilisées en mode multijoueur 2014 ?
-   Si une session de jeu dispose ouvrir le lecteur emplacements et prend en charge de la jointure en cours, pourquoi les utilisateurs pas serait en mesure de trouver la session une fois qu’il a démarré ?
-   Si une session de jeu est ouverte, permettre un utilisateur qui vient juste de rejoindre un jeu simplement rejoindre la session et commencer à lire sans avoir à attendre la réservation ?
-   Lorsque vous lisez des sessions de jeu volumineuses dans Mon titre, pourquoi ne sont pas tous les membres de la session de voir le toast d’invitation à un jeu ?
-   Je vois un comportement incohérent jeu et ont reçu les informations sur l’activation de protocole faisant référence à des fonctionnalités de jeux tiers.
-   Pourquoi vois-je la syntaxe pour les documents de session v105 dans Mes traces bien que j’ai configuré un modèle de session v107 ?


### <a name="i-am-developing-a-new-title-which-multiplayer-api-elements-should-i-use"></a>Je développe un nouveau titre. Les éléments d’API multijoueurs dois-je utiliser ?

Fonctionnalité multijoueur 2014 continuent à s’appliquer pour les titres existants, mais les éléments d’API associés probables être déconseillé. Nous recommandons l’utilisation du mode multijoueur 2015 lorsque vous préparez vos clients pour la mise en production en 2015.


### <a name="how-can-i-access-the-new-multiplayer-api-from-a-service"></a>Comment puis-je accéder à la nouvelle API multijoueur à partir d’un service ?

Consultez [appelant MPSD](multiplayer-session-directory.md).


### <a name="can-my-title-subscribe-to-changes-for-more-than-one-session"></a>Mon titre instantanés pour plus d’une session ?

Oui, un titre peut s’abonner pour recevoir des modifications sur les sessions jusqu'à 10 par connexion.


### <a name="will-a-user-be-removed-immediately-if-heshe-loses-a-network-connection-or-turns-off-the-console"></a>Est un utilisateur supprimé immédiatement s’il perd une connexion réseau ou désactive la console ?

La connexion de socket web permet MPSD rapidement détecter la déconnexion du client et la valeur du client inactif. Membres de la session sont supprimés dès que le délai de suppression inactive expire. Pour plus d’informations, consultez [des délais d’expiration de Session](mpsd-session-details.md).


### <a name="how-do-i-find-out-what-scid-session-template-and-sandbox-to-use"></a>Comment trouver les SCID, le modèle de session et le bac à sable à utiliser ?

Si vous n’avez pas participé à l’inscription initiale de votre titre, lors de la création de ces informations, vous n’avez pas accès à ces informations. Contactez votre responsable de compte de développeur, qui peut obtenir ces données pour vous.


### <a name="is-there-an-example-of-a-request-body-that-i-can-compare-to-the-one-for-my-title"></a>Existe-t-il un exemple d’un corps de demande puis-je comparer à celui de mon titre ?

Voir la structure de la demande dans **MultiplayerSessionRequest (JSON)**


### <a name="i-am-getting-an-http403-code-when-calling-mpsd"></a>J’obtiens un code HTTP/403 lors de l’appel MPSD.

Il s’agit généralement des autorisations d’un ou de problème de l’étendue. Collecter traces Fiddler pour obtenir plus d’informations, puis vérifiez les messages retournés en tant que partie intégrante du corps HttpResponse pour les 403 messages courantes :

-   La configuration de service demandé n’est pas accessible.
    1.  Vérifiez que vous utilisez un compte qui a accès à la sandbox.
    2.  Vérifiez que vous êtes dans le bac à sable correct.
     3.  Si vous utilisez l’authentification par certificat et que vous obtenez cette erreur, contactez votre responsable de compte de développeur.   La session demandée n’est pas accessible. L’utilisateur appelant doit avoir le privilège multijoueur et sessions privées peuvent uniquement être lus par les membres de la session.

    Vous n’avez pas la possibilité de voir la session, probablement parce que vous tentez d’accéder à une session qui a une visibilité de privé. Si la visibilité est définie sur privé, seuls les membres de cette session peuvent lire le document de la session.

| Remarque                                                                                                                                                  |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Un utilisateur doit disposer d’un compte de Gold pour inscrire une nouvelle session. Sans privilèges de compte Gold pour un utilisateur en cours de lecture, les demandes d’inscription d’une nouvelle session retournent HTTP/403. |

-   Le corps de la demande ne peut pas contenir les références de membre existantes, sauf si l’authentification du principal inclut un serveur.

    Vous ne pouvez pas joindre un autre utilisateur à une session pour le compte d’un utilisateur. Vous pouvez uniquement inviter. L’index de la valeur « réserver\_&lt;nombre&gt;» pour inviter un lecteur.


### <a name="i-am-getting-an-http404-code-when-calling-mpsd"></a>J’obtiens un code HTTP/404 lors de l’appel MPSD.

Collecter les traces Fiddler pour obtenir plus d’informations et puis procédez comme suit :

1.  Vérifiez le message retourné en tant que partie intégrante du corps HttpResponse pour les 404 messages courantes :
    -   La configuration de service demandé est non valide ou n’est pas configuré pour les sessions. Assurez-vous que le SCID utilisée est correcte.
    -   La session demandée est introuvable. Assurez-vous que la session est créée et que le modèle de session est correct avant de les récupérer. Vous pouvez créer une session avec un appel PUT.

2.  Vérifiez l’URI que vous utilisez.
3.  Redémarrez la console ou essayez avec un nouvel utilisateur.
4.  Consultez les Forums de développeurs de jeux pour le code d’erreur ou d’autres solutions.
5.  Vérifiez que la session n’a pas été supprimée car il est vide. Sessions peut devenir vides, car le délai d’attente des utilisateurs. Dans ce cas, il est fréquemment, car tous les membres de la session sont dans un état auquel un délai d’expiration est appliqué, telles que « prêt » ou « inactif ». Consultez [États de Session utilisateur](mpsd-session-details.md) pour les détails de l’état utilisateur.


### <a name="why-am-i-getting-an-http404-code-when-calling-multiplayerservicewritesessionbyhandleasync-or-multiplayerservicegetcurrentsessionbyhandleasync"></a>Pourquoi un code HTTP/404 lors de l’appel **MultiplayerService.WriteSessionByHandleAsync** ou **MultiplayerService.GetCurrentSessionByHandleAsync**?

Si votre titre accède à une session MPSD handle en réponse à une activation du protocole de jointure contenant un ID de handle, l’ID de handle dans l’activation de protocole peut-être être obsolète pour l’une des raisons suivantes :

-   La vue de l’interface utilisateur dans l’interpréteur de commandes Xbox à partir duquel le titre a initié la jointure est peut-être obsolète. Certaines vues de l’interface utilisateur, par exemple, carte de visite de l’utilisateur, n’actualisent pas automatiquement les handles de jointure quand ils sont ouverts. Pour éviter de recevoir le code HTTP/404, le titre doit fermer et rouvrir la vue pour actualiser les données avant de rejoindre.
-   L’utilisateur qui rejoint le titre a pu changer activité sessions après que le titre sélectionné de l’opération de jointure à partir de l’interface utilisateur d’interpréteur de commandes Xbox. C’est pourquoi le code HTTP/404 doit être rare.

Dans les deux cas, votre code de titre doit afficher l’utilisateur de joindre un message d’erreur indiquant que la jointure a échoué.


### <a name="i-am-getting-an-http412-code-when-calling-mpsd"></a>J’obtiens un code HTTP/412 lors de l’appel MPSD.

La requête suivante renvoie HTTP/412 si la session existe déjà :

    PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/foo HTTP/1.1
    Content-Type: application/json
    If-None-Match: *


La requête suivante renvoie HTTP/412 si l’etag de la session ne correspond pas à l’en-tête If-Match :

    PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/foo HTTP/1.1
    Content-Type: application/json
    If-Match: 9555A7DE-8B91-40E4-8CFB-0629312C9C7D


Consultez [synchronisation des mises à jour de Session](multiplayer-session-directory.md) pour plus d’informations.


### <a name="i-am-getting-an-http400-405-409-503-etc-code-when-calling-mpsd"></a>J’obtiens un HTTP/400, 405, 409, 503, code etc. lors de l’appel MPSD.

Collecter les traces Fiddler pour obtenir plus d’informations, et puis vérifiez les messages retournés en tant que partie intégrante du corps HttpResponse. Cela devrait vous donner suffisamment d’informations pour identifier et résoudre l’erreur, ou vous pouvez rechercher les forums de développeurs pour les résolutions.

Vous pouvez également obtenir des corps de la réponse si vous utilisez XSAPI, comme décrit dans [résolution des problèmes de l’API de Services Xbox Live](../../using-xbox-live/troubleshooting/troubleshooting-the-xbox-live-services-api.md). Vous pouvez également votre code peut utiliser le **XboxLiveContextSettings.EnableServiceCallRoutedEvents** propriété pour envoyer la sortie pour le titre de votre interface utilisateur.


### <a name="what-can-or-should-i-change-in-the-session-templates-for-my-title"></a>Ce qui peut ou dois-je modifier dans les modèles de session pour mon titre ?

Modèles de session sont des modèles pour vos sessions, et vous ne pouvez pas remplacer les constantes déjà définis dans les modèles. Toutefois, vous pouvez ajouter de nouvelles propriétés aux modèles. Consultez [MPSD Session modèles](multiplayer-session-directory.md) pour plus de détails.


### <a name="im-getting-an-error-that-is-saying-my-session-isnt-initialized"></a>J’obtiens une erreur qui dit à que ma session n’est pas initialisée.

Erreur de réponse d’exemple :

400 - \[ResponseBody\]: Cette session est configurée pour l’initialisation managée nécessitant des membres au moins 2 Démarrer.

La session n’est pas suffisant réservations de membre de session avec le champ « initialiser » défini sur true sont incluses dans la demande ne peut pas être créée. Votre code peut définir ce champ pour un membre à l’aide de la *initializeRequested* paramètre pour le **MultiplayerSession.AddMemberReservation** méthode ou le **MultiplayerSession.Join** (méthode).

Lorsque l’initialisation est spécifiée dans votre modèle de session, assurez-vous que « initialisation » : « true » est configuré pour un nombre suffisant de réservations de membre pour passer de matchmaking QoS. Pour plus d’informations, consultez [initialisation de la Session cible et QoS](smartmatch-matchmaking.md).


### <a name="my-session-is-not-being-created-and-im-getting-an-http204-code"></a>Ma session n’est pas créée et j’obtiens un code HTTP/204.

Ce code d’état indique qu’aucun utilisateur ont été ajoutés à la session lors de leur création. S’il n’y a aucun utilisateur dans la session lorsqu’il est créé et le délai d’attente vide de session est zéro (valeur par défaut), la session n’est pas créée.

Veillez à inclure au moins un lecteur lors de la création d’une session. Pour les scénarios de serveur dédié, obtenir un lecteur qui essaie de créer une correspondance ou qui a besoin de créer une correspondance et le lecteur que le lecteur initial dans la correspondance. Vous pouvez également modifier ou supprimer le délai d’attente vide de session. Pour plus d’informations, consultez [des délais d’expiration de Session](mpsd-session-details.md).


### <a name="when-should-i-poll-mpsd"></a>Quand dois-je interroger MPSD ?

Les titres doivent éviter d’interrogation MPSD. Si un titre doit découvrir les modifications apportées aux sessions MPSD, il doit s’abonner pour les événements de changement de session. Pour plus d'informations, consultez [Procédure : S’abonner aux Notifications de modification de Session MPSD](multiplayer-how-tos.md).


### <a name="what-happens-if-a-player-who-was-reserved-or-invited-to-the-session-does-not-join-it"></a>Que se passe-t-il si un lecteur qui a été réservé ou invité à la session ne joint pas ?

Cela dépend si le titre est en cours d’exécution lorsque le lecteur est informé que la session de jeu est prêt. Si le lecteur est dans le titre et n’accepte pas la notification de la session de jeu à partir de l’interface utilisateur de titre, c’est à titre à supprimer le lecteur à partir de la session de jeu avec le **MultiplayerSession.Leave** (méthode).

Si le titre est limité ou ne pas en cours d’exécution, l’interpréteur de commandes fournit une notification qui informe le lecteur qu’un emplacement est disponible. Si le joueur refuse ou ignore la notification du système, MPSD supprime cette personne de la session de jeu.


### <a name="why-would-a-created-session-not-be-found-by-matchmaking"></a>Pourquoi une session créée pas trouvée en matchmaking ?

Sur Xbox One, se contentant de créer une session n’est pas suffisant pour matchmaking rechercher la nouvelle session. Vous devez créer un ticket de correspondance pour mettent à la session pour le service. Consultez [SmartMatch Matchmaking](smartmatch-matchmaking.md).


### <a name="what-is-the-key-difference-between-the-way-parties-are-properly-used-by-2015-multiplayer-and-the-way-they-were-used-in-2014-multiplayer"></a>Qu’est la principale différence entre la façon dont parties sont correctement utilisées en mode multijoueur 2015 et la façon dont ils ont été utilisées en mode multijoueur 2014 ?

En mode multijoueur 2015, l’API multijoueur ne définit aucun tiers de jeux au niveau du système, les seules parties de conversation. Au lieu d’utiliser des parties de jeu, le titre utilise des sessions pour contrôler la jointure invite et fonctionnalités associées. Pour le mode multijoueur 2014, l’API multijoueur sur Xbox One utilisé en premier le concept de jeux tiers (**tiers** classe), qui implémente efficacement une salle d’attente de jointure au niveau du système au lieu de jeu invite.


### <a name="if-a-game-session-has-open-player-slots-and-supports-join-in-progress-why-would-users-not-be-able-to-find-the-session-once-it-has-started"></a>Si une session de jeu dispose ouvrir le lecteur emplacements et prend en charge de la jointure en cours, pourquoi les utilisateurs pas serait en mesure de trouver la session une fois qu’il a démarré ?

Lorsqu’une session de jeu démarre, il n’est plus publié sur le service. Si le lecteur emplacements se libèrent dans la session et l’arbitre (hôte) souhaite attirer de nouveaux lecteurs, l’arbitre doit créer un nouveau ticket de correspondance pour la session en cours d’exécution avec le **MatchmakingService.CreateMatchTicketAsync** méthode. Le ticket d’annonce à nouveau la session et détecte plus de joueurs. Consultez [SmartMatch Matchmaking](smartmatch-matchmaking.md).


### <a name="if-a-game-session-is-open-can-a-user-who-has-just-joined-a-game-simply-join-the-session-and-start-playing-without-having-to-wait-for-the-reservation"></a>Si une session de jeu est ouverte, permettre un utilisateur qui vient juste de rejoindre un jeu simplement rejoindre la session et commencer à lire sans avoir à attendre la réservation ?

Oui. Cela est particulièrement utile dans les cas où votre titre utilise plusieurs sessions pour effectuer le suivi des sous-groupes de joueurs au sein d’une session de jeu. L’utilisateur de jointure peut rejoindre la session représentant de son groupe et vous devez ensuite joindre la session de jeu plus volumineuse.


### <a name="when-large-game-sessions-are-playing-in-my-title-why-arent-all-session-members-seeing-the-game-invite-toast"></a>Lorsque vous lisez des sessions de jeu volumineuses dans Mon titre, pourquoi ne sont pas tous les membres de la session de voir le toast d’invitation à un jeu ?

Quand votre titre ajoute un utilisateur à la session à rejoindre, le titre définit toujours la **MultiplayerSessionMember.InitializeRequested** true à la propriété. Ce code indique à MPSD d’attente pour le reste des membres de la session avant de passer le jeu en dehors de l’étape d’initialisation. Sinon, les utilisateurs ont une fenêtre très courte dans laquelle joindre et peut manquer des toasts et les notifications de modifications de la session.


### <a name="i-am-seeing-inconsistent-game-behavior-and-have-received-protocol-activation-information-referencing-game-party-features"></a>Je vois un comportement incohérent jeu et ont reçu les informations sur l’activation de protocole faisant référence à des fonctionnalités de jeux tiers.

Cela indique que vous utilisez plusieurs 2014 multijoueurs et des fonctionnalités multijoueur 2015. L’API pour le mode multijoueur 2015 ne doit jamais être utilisée avec le code écrit pour le mode multijoueur 2014.


### <a name="why-am-i-seeing-the-syntax-for-v105-session-documents-in-my-traces-although-i-have-configured-a-v107-session-template"></a>Pourquoi vois-je la syntaxe pour les documents de session v105 dans Mes traces bien que j’ai configuré un modèle de session v107 ?

Le MPSD effectue une conversion automatique entre les versions de document de session basée sur la demande du client. Actuellement, toutes les API de Service Xbox utiliser v105 pour les demandes pour le MPSD. Cela peut entraîner une syntaxe différente entre les modèles de session v107 et v105 effectue le suivi, mais n’a aucun autre impact fonctionnel. Modèles de session doivent être configurés comme v107.

© 2015 Microsoft Corporation. Tous droits réservés.
Envoyer des commentaires sur <https://forums.xboxlive.com/spaces/22/index.html>.
Version : 2.0.90731.0
