---
title: Meilleures pratiques pour appeler Xbox Live
description: En savoir plus sur les meilleures pratiques pour l’appel d’API de Xbox Live.
ms.assetid: f4c7156b-7736-41e5-9b3d-e87cc8dd2531
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox, meilleures pratiques
ms.localizationpriority: medium
ms.openlocfilehash: 55e05ef7de2e2981f9f5af86623a8d8413ce2c99
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613704"
---
# <a name="best-practices-for-calling-xbox-live"></a>Meilleures pratiques pour appeler Xbox Live

Les services Xbox Live peuvent être appelées à partir de deux manières : à l’aide de l’API de Services Xbox (XSAPI) ou en appelant les points de terminaison REST directement. Quelle que soit la façon dont votre code appelle Xbox Live, il est important d’avoir des modèles d’appel appropriées et la logique de nouvelle tentative.

Pour comprendre comment écrire une logique de nouvelle tentative appropriée, il est nécessaire de savoir sur les deux types de points de terminaison REST - **idempotent** et **non idempotent**. Nous aborderons chacun d'entre eux ci-dessous
 
## <a name="non-idempotent-endpoints"></a>Les points de terminaison non-idempotent

Les méthodes HTTP qui ont des effets secondaires lors de répéter les appels sont considérés comme **non idempotent**. Cela signifie que si un client devait appeler le point de terminaison et un délai d’expiration réseau se produit, il n’est pas possible de relancer la méthode, car la ressource peut avoir été mis à jour, mais le réseau n’a pas pu notifier l’appelant qu’elle a réussi. Cas d’erreur, au lieu d’une nouvelle tentative, le client doit tout d’abord interroger pour voir si l’appel a réussi. Uniquement si l’appel n’a pas réussi doit il recommencez.

Dans l’API de Services Xbox, certaines API sont marqués en interne en tant que l’appel de points de terminaison non idempotent. Cela signifie que si les échecs se produisent lors de l’appel de ces points de terminaison, les API pas réessaiera automatiquement le point de terminaison.

La liste complète des API de non idempotent sont :

* game\_server\_platform\_service::allocate\_cluster()
<br>
* game\_server\_platform\_service::allocate\_cluster\_inline()
<br>
* game\_server\_platform\_service::allocate\_session\_host()
<br>
* matchmaking\_service::create\_match\_ticket()
<br>
* multiplayer\_service::write\_session()
<br>
* multiplayer\_service::write\_session\_by\_handle()
<br>
* multiplayer\_service::send\_invites()
<br>
* reputation\_service::submit\_batch\_reputation\_feedback()
<br>
* reputation\_service::submit\_reputation\_feedback()
 

## <a name="idempotent-methods"></a>Méthodes idempotent

**Idempotent** méthodes HTTP quant à eux ne laissent pas d’effets secondaires. À son tour, cela signifie qu’ils sont sécurisés à renouveler. Dans l’API de Services Xbox, toutes les méthodes idempotentes sont automatiquement retentées sous certaines conditions.

La liste complète des idempotent API sont toutes les API qui n’étaient pas répertoriés ci-dessus comme étant non idempotent.


## <a name="retry-logic-best-practices"></a>Meilleures pratiques de logique de nouvelle tentative

Pour les appels idempotent ces conditions doivent être retentées automatiquement :

* Toutes les erreurs de réseau
* 401 : Non autorisé
* 408 : RequestTimeout
* 429: Trop de demandes
* 500 : InternalError
* 502: BadGateway
* 503 : ServiceUnavailable
* 504: GatewayTimeout


Sur UWP, 401 : Non autorisé est traité spécial. Cette propriété indique le jeton d’authentification Xbox Live a expiré, afin que l’API de Services Xbox effectue des appels dans le système d’exploitation pour actualiser le jeton et effectue ensuite comme une seule nouvelle tentative.

Lorsqu’une nouvelle tentative est effectuée, il est recommandé de ne pas appeler le service jusqu'à ce que l’heure de l’en-tête « Retry-After » a été atteint. XSAPI implémente désormais cette meilleure pratique. Si un code d’état HTTP erreur et l’en-tête « Retry-After » a été retourné pour n’importe quelle API, les appels supplémentaires à la même API avant l’heure Retry-After seront retournée immédiatement avec l’erreur d’origine sans s’appuyer sur le service.

Lorsque vous renouvelez un appel, il est recommandé d’effectuer la temporisation exponentielle avec une instabilité aléatoire pour répartir la charge pour le service. XSAPI commence par un délai par défaut de 2 secondes, qui est contrôlé à l’aide de la xbox\_live\_contexte\_settings::set\_http\_de nouvelle tentative\_delay(). Cela signifie par défaut pour chaque nouvelle tentative effectue une temporisation exponentielle de 2, 4, 8, les secondes etc. et il jitters le délai entre la valeur actuelle et suivant temporisation basé sur le temps de réponse pour continuer à propager en charge pour l’ensemble des périphériques d’essayer la nouvelle tentative.

Titres doivent se trouver dans le contrôle de la durée pendant laquelle passent une nouvelle tentative d’un appel. À l’aide de XSAPI, les développeurs ont un contrôle direct de ce à l’aide de la fonction xbox\_live\_contexte\_settings::set\_http\_délai d’attente\_fenêtre(). Par défaut, il est défini sur 20 secondes. Ce paramètre sur 0 seconde s’éteint efficacement logique de nouvelle tentative. XSAPI maintenant également ajuste dynamiquement le délai d’attente HTTP interne en fonction de la quantité reste gauche de temps dans le http\_délai d’attente\_fenêtre(). Les contrôles de délai d’attente HTTP internes la durée pendant laquelle le système d’exploitation est occupé à effectuer l’opération de réseau HTTP avant d’abandonner. L’appel ne sera pas retentée, sauf si il reste au moins 5 secondes restant dans le protocole http\_délai d’attente\_fenêtre() afin de donner un délai assez raisonnable pour la fin de l’appel. Cette règle ne s’applique pas pour le premier appel, affectant ainsi l’http\_délai d’attente\_fenêtre() 0 est acceptable et entraîne un appel unique. Cette logique a pour effet que le protocole http\_délai d’attente\_fenêtre() est beaucoup plus déterministe lors de l’appel d’API retournera. Si un en-tête « Retry-After » a été retourné, aucun reties ne sera jusqu’après que l’heure « Retry-After » a été atteint. Si l’heure « Retry-After » après le http\_délai d’attente\_fenêtre(), puis l’appel de retourne à la fin du http\_délai d’attente\_fenêtre().


## <a name="error-handling"></a>Gestion des erreurs

Les développeurs de titre doivent **toujours** utiliser approprié gestion des erreurs pour **chaque** appel de service, dont ils ont besoin pour vous assurer qu’ils sont correctement gestion des réponses en échec.
 
Peut entraîner une demande à la Xbox Live pour retourner des codes d’échec, tel que de nombreuses conditions réelles

1.  Réseau n’est pas disponible. Par exemple, l’appareil perdu 4G, perdu Wi-Fi, ou le réseau s’est arrêté.
2.  Charge trop importante sur les services de charge (503)
3.  Une défaillance s’est produite sur le service (500)
4.  Trop de demandes envoyées au service (429)
5.  Conflit d’opération (412) d’écriture. Par exemple, un autre lecteur dans une session multijoueur soumise à une modification tout d’abord
6.  L’utilisateur a été exclue ou n’est pas autorisé
7.  Utilisateur a déconnecté

Gestionnaire d’erreur approprié est essentiel pour vous assurer que le jeu fonctionne correctement dans ces conditions.

XSAPI a deux types de modèles de gestion des erreurs. Un modèle en utilisant les APIs WinRT à partir de C++ / c++ / CX, C\#, ou Javascript et un autre modèle lorsque vous utilisez les nouvelles APIs C++. Détails complets sur les meilleures pratiques de gestion des erreurs, voir la page de document Xbox Live « Gestion des erreurs » et pour une vidéo qui couvre cet aspect, reportez-vous à la discussion dans [ *Xfest 2015 vidéos* ](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2015.aspx) appelée *XSAPI : C++, aucune exception !*


## <a name="best-calling-patterns"></a>Meilleurs modèles d’appel

### <a name="usebatching-requests"></a>Utiliser des demandes de traitement par lot

Certains points de terminaison prend en charge le traitement par lot ou d’agrégation d’un ensemble de requêtes en un seul appel. Par exemple, avec le service de profil Xbox Live, vous pouvez demander pour le profil d’un utilisateur unique ou un ensemble de profils d’utilisateurs. Par conséquent, si vous avez besoin d’un profil d’utilisateur pour un ensemble d’utilisateurs, il serait très inefficace d’appeler le point de terminaison ou l’API une à la fois pour chaque profil utilisateur. Chaque appel ajoute un grand nombre de surcharge de l’authentification. Passer à la place, tous les utilisateurs que vous souhaitez que les informations à la fois pour l’API, afin que le point de terminaison permettre traiter tous les profils utilisateur en même temps et renvoyer une réponse unique.

### <a name="use-the-real-time-activity-rta-service-instead-of-polling"></a>Utiliser le service de l’activité en temps réel (RTA) au lieu d’interrogation

Une autre pratique recommandée est utiliser l’interrogation périodique à la place du service activité en temps réel (RTA). Ce service expose un websocket qui envoie une notification aux clients lorsque les ressources cibles sont modifiées sur le service. Le service d’agrégation RTA donne des notifications sur les modifications de présence, les modifications des statistiques, les modifications de document multijoueur et les modifications de la relation de réseaux sociaux. Pour savoir ce que le client d’informations est intéressé, le client doit s’abonner à l’élément sur le websocket. Cela évite d’interrogation du service pour détecter les modifications apportées dans la mesure où vous serez informé exactement quand l’élément change.

Expose XSAPI le service d’agrégation RTA comme un ensemble de s’abonner à des API qui permet aux clients. Chacune de ces API correspondre \* \_modifié\_Gestionnaire d’API qui prennent dans une fonction de rappel qui sera appelée quand un élément change.

* presence\_service::subscribe\_to\_device\_presence\_change
<br>
* presence\_service::subscribe\_to\_title\_presence\_change
<br>
* user\_statistics\_service::subscribe\_to\_statistic\_change
<br>
* social\_service::subscribe\_to\_social\_relationship\_change<br>
 

## <a name="use-xbox-live-client-side-managers"></a>Utilisation des gestionnaires de côté client de Xbox Live

Nouveau dans XSAPI nous disposons désormais un ensemble de gestionnaires qui agissent en tant que cache et l’état des ordinateurs qui effectuent tous les camions soulever pour certains scénarios.


### <a name="social-manager"></a>Gestionnaire sociale

Le Gestionnaire de sociale ne le gros autour des listes d’amis et de profils. Il conserve vos amis liste, leurs profils et leur présence de données à jour à l’aide du service d’agrégation RTA. Le gestionnaire expose une API synchrone qui est le moteur de jeu très convivial. Jeux peuvent appeler ses API fréquemment car il maintient un cache en mémoire les dernières informations à partir du service. Pour plus d’informations, consultez la page de documentation Xbox Live « Introduction à Manager Social »

### <a name="multiplayer-manager"></a>Gestionnaire de mode multijoueur

Pour la gestion de session multijoueur, le Gestionnaire de mode multijoueur est une solution dans le désordre pour les jeux multijoueurs traditionnels. L’API du Gestionnaire de mode multijoueur inclut la gestion de session et une liste de lecteur, gère les invitations de jeu, jointure en cours, matchmaking et s’intègre à votre solution de mise en réseau existante. Il effectue le gros concernant l’implémentation des flux multijoueurs traditionnels. Pour plus d’informations, consultez la page de documentation Xbox Live « Introduction à mode multijoueur Manager »


## <a name="throttling-fine-grained-rate-limiting"></a>La limitation (limitation du débit de granularité fine)

Les services Xbox Live ont la limitation en place pour empêcher le placement de charge extrême sur le service de n’importe quel appareil unique. Il est important de savoir quand votre titre a été limitée. Vous pouvez savoir si votre titre a été limitée de plusieurs façons différentes :


### <a name="monitor-for-http-status-code-429"></a>Analyse de Code d’état HTTP 429

Vous pouvez utiliser Fiddler et regardez si un Code d’état HTTP 429 est retournée. La réponse JSON contiendra des détails sur la façon dont le point de terminaison a été limitée. Exemple :

```json
{
  "version":1,
  "currentRequests":13,
  "maxRequests":10,
  "periodInSeconds":120,
  "limitType":"Rate"
}
```

Si vous utilisez XSAPI, les API retourneront un http\_état\_429\_trop\_nombreux\_erreur de demande et définir le message d’erreur pour être en détail comment l’API a été limitée.

### <a name="debug-asserts"></a>Assertions de débogage

Lorsque vous utilisez XSAPI, si l’appel est limité dans un bac à sable de développement et à l’aide d’une version debug du titre, il vérifie immédiatement permettent au développeur de savoir si une limitation s’est produite. Il s’agit afin d’éviter l’erreur de limitation 429 involontairement manquants en raison du code mal écrite. Si vous souhaitez désactiver ces assertions pour continuer à travailler sans corriger le code incriminé, vous pouvez appeler cette API :


> xboxLiveContext -&gt;settings() -&gt;désactiver\_déclare\_pour\_xbox\_live\_limitation\_dans\_dev\_ les bacs à sable (xbox\_live\_contexte\_limitation\_setting::this\_code\_doit\_à\_être\_modifié\_à\_éviter\_limitation) ;

Notez toutefois que cette API n’empêchera pas votre titre de limitée. Votre titre sera toujours limité. Cela désactive simplement les assertions dans les bacs à sable de développement lors de l’utilisation d’une version debug. 

### <a name="xbox-live-trace-analyzer-tool"></a>Outil Analyseur de Trace Xbox Live

Une autre option consiste à enregistrer une trace des appels Xbox Live et les analyser cette trace à l’aide du [outil Analyseur de Trace Xbox Live.](https://docs.microsoft.com/windows/uwp/xbox-live/tools/analyze-service-calls)

Pour enregistrer une trace, vous pouvez utiliser Fiddler pour enregistrer un. Fichier SAZ, ou à l’aide de la journalisation de suivi intégré des XSAPI. Pour plus d’informations, comment utiliser activer le suivi dans XSAPI reportez-vous à la page de documentation Xbox Live « Analyser les appels aux Services Live Xbox ». Une fois que vous avez une trace, l’analyseur Xbox Live Trace outil avertira lors de la détection limitée des appels.

## <a name="is-xbox-live-up"></a>Xbox Live est ?

Xbox Live est une collection de microservices qui exposent des fonctionnalités Xbox Live tels que le profil, vos amis et présence, statistiques, leaderboards, achievements, multijoueurs et matchmaking. Il n’est pas un serveur unique ou un point de terminaison qui définit si Xbox Live est opérationnel. Si un seul serveur tombe en panne, le reste des microservices Xbox Live sont en grande partie indépendant et doit être opérationnels.

Si un seul service subit une panne temporaire, il est important de savoir si cet appel de service est vitale pour votre jeu. Essayez de fournir une expérience raisonnable tandis que les problèmes de service ou de réseau intermittents. Par exemple, si le service de présence retourne échec qui appellent probable n’est pas les aspects stratégiques pour votre jeu. Il vous suffit de signaler à l’utilisateur la présence dernière connue au lieu de la création de rapports que Xbox Live est arrêté.

Xbox Live suit également le modèle de cohérence de la cohérence éventuelle. Cela signifie que si aucune nouvelle mise à jour n’est effectuées, que par la suite toutes les demandes pour cette ressource signale la dernière valeur de mise à jour. En effet, cela signifie qu’une petite période où les informations sont obsolètes en tant que les données se propage.
