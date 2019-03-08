---
title: Guide d’intégration Arena titre
description: Découvrez comment créer un titre dans la prise en charge pour la plateforme de Xbox Live Arena.
ms.assetid: 470914df-cbb5-4580-b33a-edb353873e32
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, arena, tournoi
ms.localizationpriority: medium
ms.openlocfilehash: 891fa8da1ca6e26128e0a33d28a505a18e99662a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659884"
---
# <a name="arena-title-integration-guide"></a>Guide d’intégration Arena titre

## <a name="introduction"></a>Introduction

La plateforme Arena pour Xbox permet tournoi organisateurs (TOs) pour créer et utiliser des tournois de titres qui prennent en charge de son domaine. Arena prend en charge à la fois une Xbox Live à qui permet le serveur de publication et de tournois de l’exécution de l’utilisateur, ainsi que procédures par des tiers qui s’intègrent à son domaine. Le style du tournoi, telles que l’élimination unique ou de la Suisse, est déterminé par le.

Cette rubrique vous guide en tant que titre développeur, comment implémenter la prise en charge de l’arène votre titre. Une fois un jeu Arena activé, il fonctionnera avec n’importe quel format pris en charge tournoi sélectionné par le. L’orchestre des correspondances pour le titre en fonction de la structure du tournoi. Le titre est activé l’arène place les utilisateurs appropriés dans chaque correspondance dans le jeu, puis signale les résultats de correspondance à la ligne à.

## <a name="overview"></a>Vue d’ensemble

Xbox Arena utilise des concepts familiers au développement de jeux multijoueur Xbox. Si vous n’êtes pas familiarisé avec le système en mode multijoueur Xbox, nous vous recommandons de consulter cette documentation avant de continuer. Le processus est similaire à celle d’un utilisateur à accepter une invitation dans un jeu multijoueur. Lorsqu’il est temps pour un utilisateur de lire une correspondance dans un tournoi Arena, un toast s’affiche pour informer l’utilisateur. Accepter le toast provoque votre titre de recevoir un événement de l’activation du protocole Arena standard. Si votre titre n’est pas déjà en cours d’exécution, elle est lancée pour l’instant.

La correspondance de tournoi elle-même est coordonnée à l’aide d’une session de l’annuaire de Session multijoueur (MPSD). Dans le cas d’Arena, la session est créée par la plateforme Arena au lieu de par votre titre. Cela permet à l’aide d’un modèle de session et la configuration de jeu supplémentaire qui ont été créés par vous, en tant que le serveur de publication de titre. Les participants de correspondance du tournoi seront déjà été joint à la session. En tant que le serveur de publication de titre, vous devez configurer le modèle de session utilisé pour Arena pour prendre en charge d’arbitrage pour les résultats de la création de rapports. La configuration requise complète pour la session est incluse plus loin dans ce document. Le titre est également gratuit créer des sessions supplémentaires dont il a besoin.

Votre titre utilise la session pour définir les résultats de correspondance et de rapport. Répondre à l’activation du protocole et d’interagir avec la session MPSD sont le niveau minimal d’intégration requis par les titres de son domaine. Navigation et l’inscription pour tournois sont pris en charge par le biais de l’interface utilisateur Arena de Xbox. Si vous le souhaitez, titres peuvent obtenir des informations supplémentaires sur le tournoi à partir du service de Hub de tournois, procure une expérience de titre plus riche. Par exemple, à l’aide du Hub de tournois, votre titre peut afficher le contexte tels que les noms d’équipe et découvrir de nouvelles sessions de correspondance pour un utilisateur. Ce flux de données est représenté par les flèches vertes dans le diagramme suivant et est décrite en détail dans le [rencontrer la configuration requise et meilleures pratiques](#experience-requirements-and-best-practices) section. Les flèches estompés indiquent que la référence de l’équipe à la session change au fil du temps, comme l’utilisateur se déplace de correspondance en correspondance dans le tournoi.

![](../../images/arena/tournament-flow.png)


L’activation de protocole Arena URI contient des informations sur le tournoi, la session pour la correspondance et un lien profond que votre titre peut appeler lorsque la correspondance est positionnée. Le lien profond renvoie l’utilisateur à l’interface utilisateur Arena de Xbox. Ces composants URI sont décrits plus en détail dans le [d’activation du protocole](#1protocol-activation) section de [des exigences de base pour l’intégration de l’arène](#basic-requirements-for-arena-integration).

## <a name="basic-requirements-for-arena-integration"></a>Exigences de base pour l’intégration de l’arène

Cette section fournit des conseils techniques et des détails pour l’intégration de votre titre avec la configuration minimale requise pour prendre en charge de son domaine. Ce style d’intégration s’appuie sur le flux de données représenté par les flèches oranges dans le diagramme suivant de la vue d’ensemble.

![](../../images/arena/arena-data-flow.png)

Votre titre est activé par protocole à partir de l’interface utilisateur Arena de Xbox. Cela peut provenir d’une notification toast, la page de détails pour le tournoi, ou tout autre point d’entrée pour la correspondance. Les étapes qui se produisent quand un utilisateur participe à une correspondance sont :

1.  Votre titre est le protocole activé par l’interface utilisateur Arena de Xbox.
2.  Votre titre utilise les paramètres d’activation pour lire une correspondance unique.
3.  Lorsque la correspondance est terminée, votre titre signale les résultats à la session de jeu MPSD.
4.  Votre titre donne à l’utilisateur la possibilité pour revenir à l’interface utilisateur de domaine.

Les sections suivantes passent par chacune de ces étapes en détail.

### <a name="1--protocol-activation"></a>1.  Activation de protocole

L’interface utilisateur Arena lance choses en activant votre titre-protocole lorsqu’une correspondance est prête pour l’utilisateur. Il existe deux cas : votre titre peut être lancé pour l’instant, ou elle peut déjà être en cours d’exécution. Les deux cas sont similaires à ce qui se passe lorsqu’un titre est activé en réponse à un utilisateur qui accepte une invitation à un jeu multijoueur.

#### <a name="if-your-title-is-launched-at-the-time-of-protocol-activation"></a>Si votre titre est lancée au moment de l’activation de protocole

Le **Activated** événement est déclenché pour la première fois lorsque vous lancez votre titre. Si une activation de protocole de domaine n’est que l’activation initiale, votre titre a été lancé par un utilisateur qui tente de lire dans un tournoi. Avoir votre titre passez aussi rapidement que possible à la correspondance, en ignorant les écrans de menu et de connexion. L’ID d’utilisateur de Xbox (XUID) de l’utilisateur qui participent est fourni dans l’URI de l’activation et doit déjà être connecté.

#### <a name="if-your-title-is-already-running"></a>Si votre titre est déjà en cours d’exécution.

Le **Activated** événement peut également se déclencher avec une activation de protocole Arena, tandis que le titre est déjà en cours d’exécution. Le **Activation** événement est déclenché uniquement par une action explicite de l’utilisateur (agissant sur un toast qui indique la correspondance, ou passer à la correspondance à partir de l’interface utilisateur Arena de Xbox). Si votre titre a été en attente sur un écran de menu, faites-le passer immédiatement à la correspondance de tournoi. Si votre titre reçoit l’activation du protocole cours de jeu, l’avez aux utilisateurs la possibilité de quitter le jeu et entrez la correspondance de tournoi de la façon la plus rapide possible. Accordez à l’utilisateur une chance d’enregistrer leur jeu si nécessaire.

Activations de protocole sont remises à votre titre via la `CoreApplicationView.Activated` événement. Si le `IActivatedEventArgs.Kind` propriété des arguments d’événement est définie sur `Protocol`, l’activation est une activation de protocole et les arguments pouvant être castés en le `ProtocolActivatedEventArgs` (classe), où l’URI de l’activation du protocole est disponible via le `Uri` propriété.

Avoir le titre de vérifier le XUID sur l’activation du protocole URI. Si elle ne correspond pas le lecteur en cours, votre titre devrez également basculer entre les contextes utilisateur.

#### <a name="the-protocol-activation-uri"></a>L’URI de l’activation du protocole

Le modèle pour l’URI est :

```URI
ms-xbl-multiplayer://tournament?action=joinGame&joinerXuid={memberId}&organizer={organizerId}&tournamentId={tournamentId}&teamId={teamId}&scid={scid}&templateName={templateName}&name={name}&returnUri={returnUri}&returnPfn={returnPfn}
```

Pour les titres d’ère sur la console, le schéma d’activation est légèrement différent :

```
ms-xbl-{titleIdHex}://
```

Cela est identique à celle des invitations. À cet effet, l’ID de titre doit être huit caractères hexadécimaux, et inclut des zéros non significatifs si nécessaire.

Tout d’abord vous assurer que le `Uri.Host` (le nom qui suit immédiatement le séparateur «  :// ») est « tournoi ». Voilà comment les activations de protocole d’Arena sont distinguent de nombre d’activations d’autres fonctionnalités, telles que des invitations de jeu.

Les arguments de chaîne de requête sera URL encodée et respectent la casse. Votre titre ne doit pas dépendent de l’ordre des paramètres, et il doit ignorer les paramètres non reconnus.


Paramater(s) | Description
--- | ---
**action** | La seule action prise en charge est « joinGame ». Si le **action** est manquant ou une autre valeur est spécifiée pour celle-ci, vous pouvez ignorer l’activation.
**joinerXuid** | Le **joinerXuid** est le XUID de l’utilisateur connecté qui tente de lire dans une correspondance de tournoi. Le titre doit permettent de basculer vers ce contexte de l’utilisateur. Si le **joinerXuid** n’est pas connecté, votre titre doit inviter l’utilisateur à le faire en affichant le sélecteur de compte. XUIDs sont toujours exprimées au format décimal.
**organizerId, tournamentId** | Le **organizerId** et **tournamentId**, lorsqu’elles sont combinées, l’identificateur unique pour le tournoi associée à la correspondance de formulaire. Utiliser cet identificateur pour récupérer des informations plus détaillées sur le tournoi à partir du Hub tournois, si vous choisissez pour l’afficher dans votre titre.
**teamId** | Le **teamId** est un identificateur unique de l’équipe, dans le contexte du tournoi que l’utilisateur (spécifié par le **joinerXuid** paramètre) est un membre de. Comme le **organizerId** et **tournamentId** paramètres, vous pouvez utiliser la **teamId** pour éventuellement récupérer des informations sur l’équipe à partir du Hub de tournois.
**scid, templateName, name** | Ensemble, ils identifient la session. Voici les trois mêmes paramètres dans le chemin d’accès MPSD URI à la session :</br> </br>`https://sessiondirectory.xboxlive.com/serviceconfigs/{scid}/sessiontemplates/{templateName}/sessions{name}`.</br></br>Dans XSAPI, ils sont les trois paramètres pour le `multiplayer_session_reference `constructeur.
**returnUri, returnPfn** | Le **returnUri** est un URI de l’activation du protocole pour renvoyer l’utilisateur à l’interface utilisateur Arena de Xbox. Le **returnPfn** paramètre peuvent ou ne peuvent pas être présent. Si c’est le cas, il est le nom de famille de produit (NFP) pour l’application qui a conçu pour gérer les **returnUri**. Pour l’exemple de code qui montre comment utiliser ces valeurs, consultez [retournant à l’interface utilisateur Arena de Xbox](#4returning-to-the-xbox-arena-ui).

### <a name="2--playing-the-match"></a>2.  Lecture de la correspondance

Lors de la session MPSD est créée par l’organisateur de tournoi, l’utilisateur est définie comme un membre de cette session. Votre titre doit immédiatement la valeur est le lecteur actif à l’aide de `multiplayer_session::join()`. Cela indique à Xbox Live et les autres utilisateurs dans la correspondance de l’utilisateur est dans votre titre et prêt à lire.

L’heure de début pour la correspondance est dans la session à `/servers/arbitration/constants/system/startTime` comme une valeur de date-heure au format UTC standard (par exemple, « 2009-06-15T13:45:30.0900000Z »). (Heure de début est également disponible dans XSAPI comme `multiplayer_session_arbitration_server::arbitration_start_time()`, en commençant par le XDK 1703). LE peut créer la session dans la mesure où avant l’heure de début qu’il le souhaite (y compris en même temps que l’heure de début). Correspondance notifications sont envoyées aux participants de correspondance à l’heure de début, titres jamais être activés avant l’heure de début. L’heure de début est également la première heure à laquelle MPSD autorisera des résultats pour la correspondance.

Le titre de la recherche au niveau de chaque membre `/member/{index}/constants/system/team` propriété (`multiplayer_session_member::team_id()`) pour déterminer lequel chaque utilisateur de l’équipe est sur.

Votre titre utilise également la session pour déterminer les paramètres de correspondance, telles que map et mode. Ces paramètres spécifiques au titre peuvent être définies dans le modèle de session ou en tant que partie de la définition de tournoi en tant que constantes personnalisées. (Pour plus d’informations, consultez [configuration d’un titre pour Arena](#configuring-a-title-for-arena).) Voici un exemple :

```json
{
    "constants": {
        "custom": {
            "enableCheats": false,
            "bestOf": 3,
            "map": "winter-fall",
            "mode": "capture-the-throne"
        }
    }
}
```

Vous pouvez récupérer ces paramètres à partir de la session en tant qu’objet JavaScript Objet Notation (JSON) à l’aide de la `multiplayer_session_constants::session_custom_constants_json()` API.

En règle générale, votre titre doit traiter la session Arena la même façon qu’il traitait sa propre session MPSD. Par exemple, il peut créer des handles et abonnez-vous aux notifications de l’agrégation RTA. Mais il existe quelques différences. Par exemple, votre titre ne peut pas modifier la liste des participants de la session de jeu ou utiliser les fonctionnalités de qualité de Service (QoS) de la session, et elle doit participer à arbitrage. Les détails complets de la session sont fournies plus loin dans ce document.

### <a name="3--reporting-results"></a>3.  Résultats de la création de rapports

Les résultats de la correspondance sont signalés au domaine et le TO via la session à l’aide d’une fonctionnalité appelée arbitrage. Arbitrage est une infrastructure pour l’utilisation d’une session à lire en toute sécurité une correspondance, le rapport un résultat.

La session fournie à titre de votre à l’étape d’activation du protocole sera une session arbitrée, ce qui signifie qu’il possède une chronologie fixe qui applique le framework d’arbitrage. Ce diagramme illustre la chronologie d’arbitrage.

![](../../images/arena/arbitration-timeline.png)

Au moins un lecteur doit être actif dans la session avant l’heure acquise, qui est l’heure de début (`multiplayer_session_arbitration_server::arbitration_start_time()`) ainsi que le délai d’attente acquise. Le délai d’attente acquise est dans la session en tant que nombre de millisecondes à `/constants/system/arbitration/forfeitTimeout` (`multiplayer_session_constants::forfeit_timeout()`). Si aucune autre a rejoint la session active avant l’heure acquise, la correspondance est annulée.

Le délai d’attente d’arbitrage est exprimée en millisecondes à `/constants/system/arbitration/arbitrationTimeout` (`multiplayer_session_constants::arbitration_timeout()`). Temps d’arbitrage est l’heure de début plus la valeur de délai d’attente arbitrage et représente la durée par lequel les joueurs doivent avoir terminé la correspondance et signalé des résultats. Cette valeur est définie dans le modèle de session ou le mode de jeu par le serveur de publication. Définissez-le pour permettre aussi longtemps que votre titre a besoin pour effectuer la recherche de correspondance.

Votre titre peut signaler les résultats à tout moment entre l’heure de début et l’heure d’arbitrage. Arbitrage se produit à tout moment entre l’heure d’acquise et l’heure d’arbitrage, une fois que chaque membre actif de la session a envoyé les résultats. Par exemple, si le seul membre est actif dans la session à la fois acquise, ils peuvent (et devriez) publient un résultat et arbitrage se produira. Quel que soit le nombre de résultats est disponible au moment de l’arbitrage, arbitrage se produira si elle n’est pas déjà. Si aucun résultat n’est envoyés lors de l’heure d’arbitrage est atteinte, tous les participants de la correspondance bénéficient d’une perte.

Il est également possible pour un serveur de jeu forcer l’arbitrage à tout moment en écrivant tout simplement un résultat arbitrée.

Si un utilisateur est dans une session qui a déjà été arbitrée (soit parce que le délai d’expiration de l’arbitrage expiré, un serveur de jeu qui gère la session, ou l’utilisateur joint au plus tard), votre titre se termine la correspondance et affiche le résultat arbitrée à l’utilisateur.

Résultats de l’arbitrage toujours incluent le résultat pour chaque équipe. Lorsqu’un joueur enregistre un résultat à la session, il inclut non seulement les résultats de leur équipe, mais l’ensemble complet des résultats pour chaque équipe.

Les résultats au format JSON ressemblent à ceci.

```json
"results": {
    "red": {
        "outcome": "rank",
        "ranking": 1
    },
    "blue": {
        "outcome": "rank",
        "ranking": 2
    }
}
```

Dans cet exemple, l’ID de l’équipe est « rouge » et « bleu ». L’équipe red team a été fournie dans le premier et l’équipe blue team ensuite. Les autres valeurs possibles pour **résultat** sont :

* « win »
* « perte »
* « draw »
* « noshow »

Si **résultat** est tout élément autre que « rank », le classement est omis pour cette équipe.

Les utilisateurs individuels écrivent les résultats de correspondance dans `/members/{index}/properties/system/arbitration` (`multiplayer_session::set_current_user_member_arbitration_results()`). L’exemple suivant illustre comment un titre peut construire et envoyer un ensemble de résultats, en supposant que votre titre n’est pas l’équipe en fonction (autrement dit, tous les acteurs sont dans des équipes d’un).

```c++
void Sample::SubmitResultsForArbitration()
{
    std::unordered_map<string_t, tournament_team_result> results;

    for (auto& member : arbitratedSession->members())
    {
        tournament_team_result teamResult;
        teamResult.set_state(tournament_game_result_state::rank);
        teamResult.set_ranking(memberRank);

        results.insert(std::pair<string_t, tournament_team_result>(
            member->team_id(),
            teamResult));
    }

    arbitratedSession->set_current_user_member_arbitration_results(results);
    xboxLiveContext->multiplayer_service().write_session(
arbitratedSession,
multiplayer_session_write_mode::update_existing)
    .then([](xbox_live_result<std::shared_ptr<multiplayer_session>> sessionResult)
    {
        if (sessionResult.err())
        {
            // Handle error.
        }
        else
        {
            // Update local session cache.
        }
    });
}
```

Une fois l’arbitrage se produit, MPSD place les résultats finaux dans `/servers/arbitration/properties/system` (`multiplayer_session::arbitration_server()`), ainsi que d’autres propriétés comme indiqué ici.

```json
{
    "resultState": "completed”,
    "resultSource": "arbitration",
    "resultConfidenceLevel": 75,

    "results": { ... }
}
```

Possibilités de **resultState** incluent :

* “completed”: Tous les joueurs actives a signalé un résultat.
* « partialresults » : Certains, mais pas tous les joueurs actifs a signalé un résultat avant l’expiration d’arbitrage.
* « /noresults » : Aucun des acteurs actives a signalé un résultat avant l’expiration d’arbitrage.
* « annulée » : Aucune de joueurs actifs est arrivé avant le délai d’attente acquise.

Dans le cas « /noresults » et « canceled », les résultats sont omis.

Le **resultSource** est « arbitrage » lorsque MPSD effectue l’arbitrage en fonction des résultats signalés par les joueurs de façon individuelles. Un serveur de jeu peut écrire dans /servers/arbitration/properties/system lui-même, en contournant arbitrage. Dans ce cas, il indique un **resultSource** de « serveur ». Il est également possible pour un serveur de jeu de réécrire (autrement dit, corriger ou ajuster) les résultats, dans lequel cas il définit **resultSource** « ajustée ».

Le **resultConfidenceLevel** est un entier compris entre 0 et 100 qui indique le niveau de consensus dans le résultat. 100 signifie que tous les joueurs conviennent. 0 signifie aucune consensus est survenu, et un résultat a été choisi essentiellement au hasard à partir de celles soumises. Lorsqu’un serveur de jeu définit les résultats de l’arbitrage, il définit le **resultConfidenceLevel**-généralement à 100, sauf si pour une raison quelconque même le serveur de jeu n’est pas vraiment (par exemple, si elle signale les résultats de son propre lecteur par procédure de vote). Définir le **resultConfidenceLevel** également lorsque les résultats sont ajustés afin de refléter la confiance dans les résultats ajustées.

Lorsque le **resultSource** est « arbitrage » (et le **resultState** est « completed » ou « partialresults »), les résultats fournis sont une copie exacte d’au moins un des résultats signalés par le lecteur.

### <a name="4--returning-to-the-xbox-arena-ui"></a>4.  Retour à l’arène Xbox l’interface utilisateur

Lorsque la correspondance est sur (ou potentiellement en réponse à la demande d’un joueur d’abandonner la correspondance en cours), proposent une option à retourner à l’interface utilisateur Arena de Xbox à partir d’où ils joint la correspondance par le lecteur. Cela est possible à partir d’un écran de résultats de correspondances consécutives ou toute autre interface utilisateur de fin du jeu.

Pour revenir à l’interface utilisateur de domaine, vous devez appeler la **returnUri** qui a été passé à partir de l’URI de l’activation du protocole par à l’aide de la `Windows::System::Launcher` classe. Veillez à inclure le contexte de l’utilisateur.

L’API de lancement est exposée différemment aux jeux d’ère aux jeux de plateforme universelle Windows (UWP). La version de l’ère de l’API n’autorise pas le NFP à fournir, donc le NFP peut être ignoré même s’il est présent sur l’URI de l’activation.

Voici un exemple de code pour une ère renvoyer l’utilisateur à l’interface utilisateur de domaine.

```c++
void Sample::LaunchReturnUi(Uri ^returnUri, Windows::Xbox::System::User ^currentUser)
{
    auto options = ref new LauncherOptions();
    options->Context = currentUser;

    Launcher::LaunchUriAsync(returnUri, options);
}
```

Jeux UWP peuvent définir le **TargetApplicationPackageFamilyName** propriété sur **LauncherOptions** pour le retour NFP, si un a été fourni sur l’activation du protocole URI. Cela garantit que l’application en question est démarrée et que l’utilisateur est dirigé vers le Store si l’application n’est pas déjà installée.

Voici un exemple de code pour une application UWP renvoyer l’utilisateur à l’interface utilisateur de domaine.

```c++
void Sample::LaunchReturnUi(Uri ^returnUri, String ^returnPfn, User ^currentUser)
{
    auto options = ref new LauncherOptions();

    if (returnPfn != nullptr)
    {
        options->TargetApplicationPackageFamilyName = returnPfn;
    }

    Launcher::LaunchUriForUserAsync(currentUser, returnUri, options);
}
```

Après l’appel de l’interface utilisateur de domaine, le titre de votre exécution poursuit, éventuellement à ce même écran, en attente pour un autre événement d’activation de protocole. De cette façon, si le joueur a une autre correspondance à lire, votre titre est prêt à l’emploi. Cela accélère l’expérience utilisateur comme le joueur passe de correspondance en correspondance, basculer entre votre titre et l’interface utilisateur de domaine Xbox.

### <a name="handling-errors-and-edge-cases"></a>Gestion des erreurs et les cas ambigus

Le flux dans la section précédente décrit le chemin d’accès golden via la lecture d’une correspondance. Il existe de nombreux endroits où vous pouvez voir un comportement inattendu ou problèmes peuvent survenir. Cette section couvre un certain nombre d’erreurs potentielles ou les cas marginaux et fournit des recommandations pour les gérer dans votre titre.

#### <a name="game-session-not-found"></a>Session de jeu introuvable

Si votre titre est lancé avec une référence de session pour une correspondance, et puis, le client ne parvient pas à obtenir une erreur lors de la demande de cette session à partir de MPSD, présenter une erreur à l’utilisateur lui indiquant qu’ils ne peuvent pas joindre la correspondance.

#### <a name="user-attempts-to-join-a-match-that-has-been-played"></a>Utilisateur tente de joindre une correspondance a été lu.

Si un utilisateur tente de joindre une correspondance, une fois que les résultats ont été signalés, bloquez ce client de démarrer une nouvelle correspondance et les présenter à l’utilisateur avec une erreur. Vous pouvez vérifier si les résultats ont été signalés en effectuant une itération à travers les membres de la session pour voir si un a un `/members/{index}/arbitrationStatus` (`multiplayer_session_member::tournament_arbitration_status`) défini sur « terminé ».

#### <a name="game-clients-unable-to-establish-a-p2p-connection"></a>Impossible d’établir une connexion p2p de clients de jeu

Si votre titre utilise des connexions de personne à personne (p2p) entre les clients pour le mode multijoueur et n’a pas la prise en charge pour les relais, il peut être d’instances où les utilisateurs qui sont mis en correspondance ensemble ne parvenez pas à établir une connexion p2p.

Si le client est en mesure de récupérer la session pour la correspondance, mais ne peut pas se connecter à d’autres clients, signaler un résultat « draw » pour chaque équipe sous `/members/{index}/properties/system/arbitration/results` (`multiplayer_session::set_current_user_member_arbitration_results()`). Cela indique à Xbox Live que la correspondance n’a pas eu lieu. Il empêche également les attaques où les utilisateurs peuvent faire défiler un tournoi en forçant un échec de connexion p2p.

#### <a name="game-client-disconnects-mid-match"></a>Correspondance en milieu de déconnexion du client de jeu

Un des scénarios d’erreur plus courants est lorsque les clients se déconnectent pendant la lecture d’une correspondance. Selon l’état de la correspondance et de votre implémentation, il existe quelques options pour gérer cette situation :

* Si la correspondance de tournoi est one-VS-un, ou si tous les membres d’une équipe de déconnecter les autres clients et/ou de votre service de jeu, nous recommandons que vous signaler une « perte » pour l’équipe qui n’est plus connecté. Cela empêche le cas dans lequel les utilisateurs peuvent forcer une déconnexion d’une correspondance, qu'elles sont perdre pour empêcher que le résultat signalé.
* Si la correspondance est entre les équipes avec plusieurs membres, et un sous-ensemble des clients pour une équipe de se déconnecter en milieu de correspondance, vous pouvez choisir de terminer la correspondance à ce stade, ou lui permettre de continuer avec les autres utilisateurs. Si vous choisissez de continuer la mise en correspondance, vous pourrez également permettre aux utilisateurs déconnectés à rejoindre la correspondance.

#### <a name="full-teams-not-present"></a>Une ou plusieurs équipes complet non présent

Si votre titre prend en charge les correspondances avec les équipes de deux ou plus, vous pouvez rencontrer des cas où certains membres d’une équipe de lancer votre titre pour leur correspondance. Dans ce cas, vous pouvez choisir les membres de l’équipe pour jouer sans leur membre de l’équipe et éventuellement autoriser ce membre de l’équipe joindre la correspondance en cours d’exécution.

Ou bien, vous pouvez appliquer automatiquement un résultat. Si vous procédez ainsi, patientez jusqu'à l’heure forfeitTimeout pour permettre à tous les participants de joindre la correspondance avant d’appliquer le résultat. Cela vous permet d’ajuster la fenêtre avec les modifications apportées au mode de jeu pour le tournoi au lieu de devoir mettre à jour votre titre.

#### <a name="length-of-match-exceeds-arbitrationtimeout"></a>Longueur de correspondance dépasse arbitrationTimeout

Définir la valeur de **arbitrationTimeout** est supérieure à la longueur maximale de temps une correspondance peut éventuellement prendre. Cela étant dit, votre titre peut présenter les modes dans lesquels les longueurs de correspondances possibles sont très longs ou dans lequel il n’existe pas de valeur maximale. Dans ce cas, vous envisagez de :

* Limitation au tournoi à modes uniquement avec des longueurs fixes, nombre maximales de temps.
* Pour informer l’utilisateur de la limite de temps et les rapports d’un résultat avant la **arbitrationTimeout** selon un critère de correspondance.

Étant donné que **arbitrationTimeout** expiration déclenchera un résultat automatique pour la correspondance, n’attendez pas la version complète **arbitrationTimeout** période avant de signaler un résultat à partir du titre.  Au lieu de cela, créez dans une mémoire tampon de temps (par exemple, **arbitrationTimeout** -1 000 ms), ou utiliser une valeur séparée pour contrôler la longueur maximale de correspondance.

#### <a name="other-cases"></a>Autres cas

Pour tous les autres cas de défaillance, la recommandation générale consiste à informer l’utilisateur de l’erreur et de renvoyer l’utilisateur à l’interface utilisateur Arena de Xbox.

## <a name="experience-requirements-and-best-practices"></a>En matière d’expérience et les meilleures pratiques

Étant donné que Arena présente uniquement jamais votre titre avec des correspondances individuels, nouveaux formats peuvent être introduites au fil du temps sans devoir mettre à jour le titre de votre. Par conséquent, l’expérience utilisateur de ligne de base pour un domaine titre intégré doit être simple et suffisamment flexible pour travailler avec plusieurs concurrence met en forme.

Si vous le souhaitez, vous pouvez utiliser des données à partir du Hub de tournoi votre titre pour faire tournois une partie plus intégrée de l’expérience dans le jeu.  Cette fonctionnalité peut être trouvée sous « xbox::services::tournaments ».  À l’aide de ces API vous avez la possibilité de :
* Détails de tournoi aire de conception dans le titre de votre interface utilisateur (nom du tournoi, nom de l’équipe, etc.).
* Fournir de découverte pour tournois votre titre
* Conserver les utilisateurs dans votre titre entre les correspondances d’Arena

## <a name="configuring-a-title-for-arena"></a>Configuration d’un titre pour Arena

Pour activer un titre pour Arena, quelques étapes supplémentaires sont nécessaires lorsque vous le configurez dans le portail de développement Xbox (XDP) ou [partenaires](https://partner.microsoft.com/dashboard).

### <a name="enabling-arena-for-your-title"></a>L’activation de l’arène pour votre titre

Pour activer Arena, accédez à la page de configuration de service pour votre titre dans XDP et sélectionnez « Domaine ».

![](../../images/arena/arena-configure-xdp.png)

Ici, vous avez plusieurs options :

* **Arena activé** – Activez cette case à cocher pour activer l’arène pour ce bac à sable.
* **Fonctionnalités d’Arena** – cette section inclut des cases à cocher pour activer tournois de généré par l’utilisateur dans le bac à sable et interplate-forme play, ce qui permet aux utilisateurs sur plusieurs plateformes à participer aux mêmes tournois.
* **Plateformes d’Arena** – vous permet de sélectionner les plateformes sur lequel tournois peuvent être lus pour votre titre.
* **Ressources de tournoi** : (anciennement dans la section « Mode multijoueur et Matchmaking ».) Voici les images de tournoi pour votre titre.

Arena peut également être activée dans partenaires dans le **tournoi** menu sous le service Xbox Live.

![Menu Arena Partner Center](../../images/arena/Arena_On_WDC.JPG)

Vous devez publier la configuration du service pour que les modifications entrent en vigueur. Configuration Arena libre-service n’est actuellement pas pris en charge par le biais UDC. Si vous utilisez UDC pour la configuration du service, fonctionnent avec votre compte responsable du développement à l’intégration avec son domaine.

### <a name="setting-up-game-modes"></a>Configuration des modes de jeu

Modes de jeu sont une fonctionnalité de Xbox Live qui permet à l’éditeur préconfigurer les paramètres pour une correspondance de tournoi. Ces modes de jeu sont utilisés lors de la création de tournoi à définir les règles appliquées par votre titre pour le tournoi et de Xbox Live. En tant que le serveur de publication de titre, vous avez besoin d’au moins un mode de jeu pour activer tournois de généré par l’utilisateur pour votre titre. Éléments du mode jeu incluent :

#### <a name="supports-ugt"></a>Prend en charge UGT ?

Modes de jeu peuvent être activées pour une utilisation avec tournois généré par l’utilisateur (UGTs). Si votre titre est configuré pour prendre en charge UGT, vous pouvez marquer des modes de jeu afin que vous pouvez les sélectionner par les utilisateurs de Xbox Live lors de la création de tournois pour votre titre.

#### <a name="team-size"></a>Taille de l’équipe

Il s’agit du nombre d’utilisateurs qui peuvent participer le tournoi en tant qu’équipe. Pour un utilisateur unique titre ou le tournoi, le mode de jeu a une taille d’un de l’équipe. Arena actuellement ne prend en charge les tailles de variable team pour tournois.

#### <a name="forfeittimeout"></a>forfeitTimeout

Il s’agit du nombre de millisecondes, après l’heure, avant que la correspondance est annulée si pas afficher de lecteurs pour qu’il de début de la correspondance (autrement dit, si aucun des lecteurs avec la valeur eux-mêmes active dans la session). Définissez ce paramètre sur un laps de temps qui est au moins suffisamment long pour un lecteur à lancer votre titre et accéder à la correspondance de tournoi à partir du moment qu'ils voient la notification toast.

#### <a name="arbitrationtimeout"></a>arbitrationTimeout

Il s’agit du nombre de millisecondes, après l’heure après laquelle aucun autre résultat ne sera acceptée de début de la correspondance. Affectez-lui la valeur du délai d’attente acquise plus un laps de temps plus longue que la correspondance la plus longue peut prendre. De cette façon, même si les acteurs de commencer à lire juste avant l’heure acquise, il a toujours suffisamment de temps pour terminer la correspondance. Si aucun résultat n’est signalées par l’heure de la **arbitrationTimeout**, les participants de la correspondance toutes les une perte de réception. Xbox Arena attend également haut jusqu'à ce que le **arbitrationTimeout** pour tous les membres actifs signaler les résultats avant de commencer l’arbitrage.

Ce délai d’attente agit comme une protection au cas où une erreur survient et un résultat ne sont signalé pour un lecteur. Au lieu de faire du surplace le tournoi entière, ce délai d’attente place une limite supérieure sur la durée pendant laquelle le tournoi attendra. Pour cette raison, il ne doit pas être défini trop élevée, car elle détermine la longueur maximale de chaque tournoi round.

#### <a name="name"></a>Nom

C’est le nom visible par l’utilisateur pour le mode de jeu. Cette valeur est localisable.

#### <a name="description"></a>Description

Il s’agit de la description visibles par l’utilisateur pour le mode de jeu. Cette valeur est localisable.

#### <a name="custom"></a>Personnalisé

Le **personnalisé** section pour le mode de jeu est un jeu de propriétés où les développeurs peuvent insérer des paramètres de configuration spécifiques titre pour la correspondance du tournoi. Valeurs définies dans le cadre de la section personnalisée sont écrits dans la session MPSD correspondance sous `/properties/custom/`.

Actuellement, le programme d’installation du mode de jeu n’est pas pris en charge par le biais XDP (ou UDC). Pour créer des modes de jeu pour votre titre, contactez votre responsable de compte de développeur.

### <a name="requirements-for-the-session-template"></a>Configuration requise pour le modèle de session

En tant que le développeur de titre, vous devez fournir le modèle de session pour le champ à pour une utilisation lors de la création de sessions pour les correspondances. Il existe certaines attentes sur le contenu du modèle.

```json
{
    "version": 1,
    "inviteProtocol": "tournamentGame",
    "memberInitialization": null,

    "capabilities": {
        "gameplay": true,
        "arbitration": true,

        "large": false,
        "broadcast": false,
        "blockBadMsaReputation": false
    },

    "maxMembersCount": {maxMembersCount},
}
```

Autres propriétés non illustrées ici peuvent être définies comme vous le souhaitez.

Arena les sessions sont toujours la version 1. Définition de la **inviteProtocol** permet des notifications de correspondance-prêt à être envoyées aux participants de tournoi à « tournamentGame ». **memberInitialization** est définie sur la valeur null pour désactiver la qualité de service. La fonctionnalité de jeu doit être définie, car la session représente une correspondance, et la fonctionnalité d’arbitrage est requise pour les résultats de la création de rapports. Le **grand**, **diffusion**, et **blockBadMsaReputation** fonctionnalités doivent être désactivées, car ils pourraient gêner le fonctionnement de la session.

Votre titre peut spécifier ses propres paramètres dans la section personnalisée du modèle, pour les paramètres qui ont une valeur fixe pour tous les tournois qui utilisent le modèle. Voici un exemple.

```json
        "custom": {
            "enableCheats": false,
            "bestOf": 3
        }
```

Enfin, le **maxMembersCount** paramètre système est requis. Définissez-la sur le nombre total de lecteurs qui peuvent jouer dans une correspondance de tournoi. Le nombre de lecteurs peut être également limitée par les paramètres de mode de jeu, assurez-vous que la valeur définie dans la session représente le nombre total le plus élevé de lecteurs pris en charge pour votre titre. Par exemple, si le nombre maximal de lecteurs de que votre jeu prend en charge pour une correspondance est 5 vs. 5, définissez **maxMembersCount** à 10. Ce modèle MPSD peut être utilisé pour prendre en charge des correspondances de n’importe quel nombre de joueurs jusqu'à la **maxMembersCount**.
