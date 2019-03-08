---
title: À l’aide de XIM (C++)
description: Découvrez comment utiliser Xbox intégré multijoueur (XIM) avec C++.
ms.date: 04/24/2018
ms.topic: article
keywords: Xbox live, xbox, jeux, xbox une, mode multijoueur intégré xbox
ms.localizationpriority: medium
ms.openlocfilehash: 798d1a1bc738cbdc7bb2b3fb34076f0897fc76ff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644054"
---
# <a name="using-xim-c"></a>À l’aide de XIM (C++)

> [!div class="op_single_selector" title1="Language"]
> - [C++](using-xim.md)
> - [C#](using-xim-cs.md)

Il s’agit d’une brève procédure pas à pas sur l’utilisation C++ API de XIM. Les développeurs qui souhaitent accéder XIM par le biais de jeux C# devrait être [à l’aide de XIM (C#)](using-xim-cs.md).

- [À l’aide de XIM (C++)](#using-xim-c)
    - [Conditions préalables](#prerequisites)
    - [L’initialisation et démarrage](#initialization-and-startup)
    - [Opérations asynchrones et les modifications d’état de traitement](#asynchronous-operations-and-processing-state-changes)
    - [Gestion des objets de joueur XIM](#handling-xim-player-objects)
    - [L’activation de vos amis à joindre et de les inviter](#enabling-friends-to-join-and-inviting-them)
    - [Base matchmaking et le déplacement vers un autre réseau XIM avec d’autres utilisateurs](#basic-matchmaking-and-moving-to-another-xim-network-with-others)
    - [Désactivation de la règle NAT matchmaking à des fins de débogage](#disabling-matchmaking-nat-rule-for-debugging-purposes)
    - [En laissant un réseau XIM et le nettoyage](#leaving-a-xim-network-and-cleaning-up)
    - [Envoi et réception de messages](#sending-and-receiving-messages)
    - [Évaluation de la qualité de chemin d’accès de données](#assessing-data-pathway-quality)
    - [Partage des données à l’aide des propriétés personnalisées du lecteur](#sharing-data-using-player-custom-properties)
    - [Partage des données à l’aide des propriétés personnalisées de réseau](#sharing-data-using-network-custom-properties)
    - [Équivalent à l’aide de la compétence de par le lecteur](#matchmaking-using-per-player-skill)
    - [Équivalent à l’aide du rôle de par le lecteur](#matchmaking-using-per-player-role)
    - [Comment XIM fonctionne avec les équipes de lecteur](#how-xim-works-with-player-teams)
    - [Utilisation de conversation](#working-with-chat)
    - [Désactivation des acteurs](#muting-players)
    - [Configuration des cibles de conversation à l’aide des équipes de lecteur](#configuring-chat-targets-using-player-teams)
    - [Remplissage d’arrière-plan automatique d’emplacements de lecteur (« renvoi » matchmaking)](#automatic-background-filling-of-player-slots-backfill-matchmaking)
    - [Interrogation des réseaux joignables](#querying-joinable-networks)

## <a name="prerequisites"></a>Conditions préalables

Avant de commencer à coder avec XIM, il existe deux conditions préalables.

1. Vous devez avoir configuré les AppXManifest de votre application avec des fonctionnalités de mise en réseau multijoueur standards et vous devez avoir configuré la partie de manifeste de réseau pour déclarer des modèles de modèle utilisés par XIM le trafic nécessaire.

    AppXManifest fonctionnalités et les manifestes de réseau sont décrits plus en détail dans leurs sections respectives de la documentation de la plateforme ; le standard XML XIM spécifiques pour coller est fournie au [Configuration de projet XIM](xim-manifest.md).

1. Vous devez disposer de deux éléments application des informations d’identité disponibles :

    * Le titre attribué Xbox Live ID.
    * L’ID de configuration de service fourni en tant que partie de l’approvisionnement de votre application pour l’accès au service Xbox Live.

    Consultez votre représentant Microsoft pour plus d’informations sur l’acquisition de ceux-ci. Ces éléments d’information seront utilisés lors de l’initialisation.

Compilation XIM nécessite y compris la principale `XboxIntegratedMultiplayer.h` en-tête. Pour lier correctement, votre projet doit également inclure `XboxIntegratedMultiplayerImpl.h` dans au moins une compilation unit (un en-tête précompilé commun est recommandé dans la mesure où ces implémentations de fonctions stub sont petites et facilement au compilateur de générer en tant que « inline »).

Comme indiqué dans le [vue d’ensemble de XIM](../xbox-integrated-multiplayer.md), l’interface XIM ne nécessite pas d’un projet à choisir entre la compilation avec C++ / c++ / CX et C++ traditionnel ; il peut être utilisé avec. L’implémentation n’également lever des exceptions comme un moyen d’erreur non irrécupérable reporting consommer facilement des projets sans exception, si vous préférez.

## <a name="initialization-and-startup"></a>L’initialisation et démarrage

Commencer à interagir avec la bibliothèque en initialisant le singleton d’objet XIM avec votre Xbox Live service configuration ID titre et la chaîne de numéro d’identification. Si vous utilisez également l’API de Services Xbox, il peut s’avérer plus commode de récupérer à partir de `xbox::services::xbox_live_app_config`. L’exemple suivant suppose que les valeurs se trouvent déjà dans 'myServiceConfigurationId' et 'myTitleId' variables respectivement :

```cpp
xim::singleton_instance().initialize(myServiceConfigurationId, myTitleId);
```

Une fois initialisé, l’application doit récupérer les chaînes d’ID d’utilisateur Xbox pour tous les utilisateurs actuellement sur l’appareil local qui participera et les passer à un `xim::set_intended_local_xbox_user_ids()` appeler. L’exemple de code suivant suppose un seul utilisateur a appuyé sur un bouton de contrôleur exprimer l’intention de lire et de la chaîne d’ID d’utilisateur Xbox associée à l’utilisateur a déjà été récupérée dans une variable 'myXuid' :

```cpp
xim::singleton_instance().set_intended_local_xbox_user_ids(1, &myXuid);
```

Un appel à `xim::set_intended_local_xbox_user_ids()` immédiatement définit les ID d’utilisateur de Xbox associée aux utilisateurs locaux qui doivent être ajoutés au réseau XIM. Cette liste Xbox d’ID d’utilisateur sera utilisée dans toutes les opérations de futurs du réseau jusqu'à ce que les modifications de liste via un autre appel à `xim::set_intended_local_xbox_user_ids()`.

Dans le cas où aucun réseau XIM n’existe tout encore, la première étape consiste à déplacer vers un réseau XIM afin de lancer le processus démarré. Si l’utilisateur n’a pas encore un réseau XIM spécifique à l’esprit, la meilleure solution consiste simplement à déplacer vers un réseau vide. Ainsi, les amis de l’utilisateur joindre le réseau comme une sorte de « Hall » à partir duquel ils peuvent collaborer pour sélectionner leur activité multijoueur suivante (par exemple, la saisie de matchmaking).

Voici un exemple montrant comment déplacer simplement les utilisateurs locaux ajoutés précédemment avec `xim::set_intended_local_xbox_user_ids()` à un réseau local pour le nombre total de lecteurs jusqu'à 8 XIM vide :

```cpp
xim::singleton_instance().move_to_new_network(8, xim_players_to_move::bring_only_local_players);
```

L’appel à `xim::move_to_new_network()` initialise la communication asynchrone l’opération de déplacement qui se termine par un événement de changement d’état que les développeurs doivent traiter régulièrement pour.

## <a name="asynchronous-operations-and-processing-state-changes"></a>Opérations asynchrones et les modifications d’état de traitement

Le cœur de XIM est l’application régulière, fréquentes les appels à la `xim::start_processing_state_changes()` et `xim::finish_processing_state_changes()` paire de méthodes. Ces méthodes sont comment XIM est informé que l’application est prête à gérer les mises à jour à l’état multijoueur, et comment XIM fournit ces mises à jour. Ils sont conçus pour fonctionner rapidement, telles qu’elles peuvent être appelées chaque bloc de graphique dans la boucle de rendu de l’interface utilisateur. Cela fournit un emplacement pratique pour récupérer toutes les modifications en file d’attente sans vous soucier du caractère imprévisible des durées de réseau ou de la complexité de rappel multi-thread. L’API XIM est réellement optimisé pour ce modèle de thread unique. Elle garantit que son état reste inchangé en dehors de ces deux fonctions, vous pouvez l’utiliser directement et plus efficacement.

Pour la même raison, tous les objets retournés par l’API XIM doivent *pas* être considéré comme thread-safe. La bibliothèque a protection multithreading interne, mais vous devrez toujours implémenter votre propre verrouillage si vous avez besoin d’un thread arbitraire d’accès à toutes les valeurs de XIM. Par exemple, dans le cas d’avoir un seul thread à remonter le `xim::players()` liste tandis que peut appeler un autre thread soit `xim::start_processing_state_changes()` ou `xim::finish_processing_state_changes()` et modification de la mémoire associée à cette liste de lecteur.

Lorsque `xim::start_processing_state_changes()` est appelée, toutes les mises à jour en file d’attente sont signalés dans un tableau de `xim_state_change` structure des pointeurs.

Les applications doivent :

1. Itérer au sein du tableau.
1. Examinez la structure de base pour son type plus spécifique.
1. Un cast du type de structure de base correspondant plus type.
1. Gérer cette mise à jour comme il convient.

Une fois terminé avec tous les `xim_state_change` structures actuellement disponibles, ce tableau doit être retransmis à XIM pour libérer les ressources en appelant `xim::finish_processing_state_changes()`. Exemple :

```cpp
uint32_t stateChangeCount;
xim_state_change_array stateChanges;
xim::singleton_instance().start_processing_state_changes(&stateChangeCount, &stateChanges);
for (uint32_t stateChangeIndex = 0; stateChangeIndex < stateChangeCount; stateChangeIndex++)
{
   const xim_state_change * stateChange = stateChanges[stateChangeIndex];
   switch (stateChange->state_change_type)
   {
       case xim_state_change_type::player_joined:
       {
           MyHandlePlayerJoined(static_cast<const xim_player_joined_state_change*>(stateChange));
           break;
        }

       case xim_state_change_type::player_left:
       {
           MyHandlePlayerLeft(static_cast<const xim_player_left_state_change*>(stateChange));
           break;
       }

       ...
    }
 }
 xim::singleton_instance().finish_processing_state_changes(stateChanges);
```

Maintenant que vous avez votre boucle de traitement de base, vous pouvez gérer les modifications d’état associées à la première `xim::move_to_new_network()` opération. Chaque opération de déplacement XIM réseau commence par un `xim_move_to_network_starting_state_change`. Si le déplacement échoue pour une raison quelconque, votre application sera fournie une `xim_network_exited_state_change`, qui est l’erreur courante mécanisme pour les erreurs asynchrones qui vous empêche de déplacer vers un réseau XIM ou vous déconnecte du réseau XIM en cours de gestion. Sinon, le déplacement se termine avec un `xim_move_to_network_succeeded_state_change` une fois que tous les États a été finalisées et que tous les joueurs ont été ajoutés correctement au réseau XIM.

Lorsqu’un utilisateur ne dispose pas du privilège de la plateforme pour jouer avec d’autres utilisateurs dans une session multijoueur, XIM envoie à un périphérique qui tente de créer ou joindre un réseau XIM un `xim_network_exited_state_change` avec la raison `xim_network_exit_reason::user_multiplayer_restricted`. Cela se produit quand un utilisateur local définie avec `xim::set_intended_local_xbox_user_ids()` ont une restriction de multijoueur avec Xbox Live. Applications Xbox ère à l’aide de XIM doivent appeler soit `Store::Product::CheckPrivilegeAsync` lecteurs local avant de tenter de les déplacer dans un réseau XIM avec attemptResolution défini sur true, ou faire en réponse à la réception `xim_network_exit_reason::user_multiplayer_restricted` comme un motif d’une `xim_network_exited_state_change`.

## <a name="handling-xim-player-objects"></a>Gestion des objets de joueur XIM

En supposant que l’exemple de déplacement d’un utilisateur local à un nouveau réseau XIM a réussi, votre application sera fournie une `xim_player_joined_state_change` pour une variable locale `xim_player` objet. Ce pointeur d’objet reste valide tant que l’instance d’acteur proprement dit n’est valide. L’instance d’acteur devienne non valide lorsque le correspondant `xim_player_left_state_change` pour que le lecteur instance a été retournée via un appel à `xim::finish_processing_state_changes()`. Votre application sera toujours fournie un `xim_player_left_state_change` pour chaque `xim_player_joined_state_change`.

Vous pouvez également récupérer un tableau de tous les `xim_player` objets dans le XIM réseau à tout moment à l’aide de `xim::get_players()`.

Le `xim_player` objet possède de nombreuses méthodes utiles, telles que `xim_player::gamertag()` pour récupérer la chaîne de Xbox Live Gamertag actuelle associée du lecteur pour l’affichage. Si le `xim_player` est local sur l’appareil, puis il signale également une valeur non null `xim_player::xim_local` pointeur d’objet à partir de `xim_player::local()`, qui propose des méthodes supplémentaires disponibles uniquement pour les lecteurs locaux.

Bien sûr, l’état plus importantes pour les joueurs n’est pas capable de XIM informations courantes, mais ce que votre application spécifique veut effectuer le suivi. Étant donné que vous avez probablement votre propre construction pour ces informations de suivi, vous souhaitez lier le `xim_player` de l’objet à votre propre objet de construction player afin que les rapports XIM à tout moment un `xim_player`, vous pouvez récupérer rapidement votre état sans avoir à remplir et Rechercher un pointeur de contexte du lecteur personnalisé. L’exemple suivant suppose un pointeur vers votre état privé est dans la variable « myPlayerStateObject » et récemment ajouté `xim_player` objet se trouve dans la variable « newXimPlayer » :

```cpp
newXimPlayer->set_custom_player_context(myPlayerStateObject);
```

Cette opération enregistre la valeur de pointeur spécifié avec l’objet player localement (il est jamais transférée sur le réseau pour les appareils distants où la mémoire ne serait pas valide). Vous serez en mesure de toujours revenir à votre objet en récupérant le contexte personnalisé et en castant vers votre objet comme dans l’exemple suivant :

```cpp
myPlayerStateObject = reinterpret_cast<MyPlayerState *>(newXimPlayer->custom_player_context());
```

Vous pouvez modifier le pointeur de contexte de lecteur personnalisé affecté à un `xim_player` à tout moment.

## <a name="enabling-friends-to-join-and-inviting-them"></a>L’activation de vos amis à joindre et de les inviter

Sécurité et confidentialité, tous les nouveaux réseaux XIM sont automatiquement configurés par défaut pour être uniquement joignable par les lecteurs locaux, et c’est à l’application pour autoriser explicitement d’autres utilisateurs une fois qu’il est prêt. L’exemple suivant montre comment utiliser `xim::network_configuration()` pour récupérer la configuration réseau actuelle et de mettre à jour à l’aide de joinability `xim::set_network_configuration()` pour commencer à autoriser les nouveaux utilisateurs locaux à joindre en tant que lecteurs, ainsi que d’autres utilisateurs qui ont été invités ou qui sont en cours » suivi » (une relation de réseaux sociaux Xbox Live) par les lecteurs déjà dans le réseau XIM :

```cpp
xim_network_configuration networkConfiguration = *xim::singleton_instance.network_configuration();
networkConfiguration.allowed_player_joins = xim_allowed_player_joins::local | xim_allowed_player_joins::invited | xim_allowed_player_joins::followed;
xim::singleton_instance().set_network_configuration(&networkConfiguration);
```

`xim::set_network_configuration()` exécute asynchronoulsy. Une fois l’appel d’exemple de code précédent est terminée, un `xim_network_configuration_changed_state_change` est fourni pour notifier l’application que la valeur joinability a changé à partir de sa valeur par défaut `xim_allowed_player_joins::none`. Vous pouvez ensuite interroger pour la nouvelle valeur en vérifiant la `allowed_player_joins` propriété de la `xim_network_configuration` retourné par `xim::network_configuration()`. 

Le `allowed_player_joins` peut être vérifié alors que celui-ci se trouve dans un réseau XIM pour déterminer le joinability du réseau.

Si un des lecteurs locaux souhaitez envoyer des invitations aux utilisateurs distants de se connecter à ce réseau XIM, l’application peut appeler `xim_player::xim_local::show_invite_ui()` pour lancer l’interface utilisateur d’invitation système. Ici, l’utilisateur local peut sélectionner des personnes qu’il souhaite inviter et envoyer des invitations. L’exemple suivant montre comment cela fonctionne et suppose que la variable 'ximPlayer' pointe vers une variable locale valide `xim_player`:

```cpp
ximPlayer->local()->show_invite_ui();
```

Après l’exécution de l’instruction ci-dessus, l’invitation système l’interface utilisateur affiche. Une fois que l’utilisateur a envoyé l’invitation (ou sinon ignoré l’interface utilisateur), un `xim_show_invite_ui_completed_state_change` sera fourni lors du prochain appel à `xim::start_processing_state_changes()`. Vous pouvez également, votre application peut envoyer des invitations directement à l’aide `xim_player::xim_local::invite_users()` sans afficher l’interface utilisateur d’invitation système.

Dans les deux cas, les utilisateurs distants messages s’affichent Xbox Live invitation partout où ils sont connectés, laquelle ils peuvent choisir d’accepter ou refuser. S’il accepte, l’activation d’un protocole lance votre application sur ces appareils si elle n’est pas déjà en cours d’exécution, et l’activation du protocole est remise à votre application avec ses arguments d’événement qui peut être utilisé pour déplacer vers le réseau XIM correspondant.

Consultez la documentation de la plateforme pour plus d’informations sur l’activation de protocole lui-même.

L’exemple suivant montre comment un appel à `xim::extract_protocol_activation_information()` détermine si les arguments d’événement sont applicables à XIM. Cela suppose que vous avez récupérés à partir de la chaîne d’URI brute `Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs` à une variable 'uriString' :

```cpp
xim_protocol_activation_information activationInfo;
bool isXimActivation;
isXimActivation = xim::singleton_instance().extract_protocol_activation_information(uriString, &activationInfo);
```

Si elle est une activation XIM, vous devez vous assurer de l’utilisateur local est identifié dans le champ « local_xbox_user_id » de rempli `xim_protocol_activation_information` structure n’est connectée et qu’il est entre les utilisateurs spécifiés à `xim::set_intended_local_xbox_user_ids()`. Vous pouvez lancer déplacement vers le réseau XIM spécifié avec un appel à `xim::move_to_network_using_protocol_activated_event_args()` à l’aide de la même chaîne URI. Exemple :

```cpp
xim::singleton_instance().move_to_network_using_protocol_activated_event_args(uriString);
```

Également, notez que « suivi » les utilisateurs distants permettre naviguer à la carte de lecteur de l’utilisateur local dans l’interface utilisateur du système et lancer une tentative de jointure eux-mêmes sans invitation (en supposant que vous avez autorisé ce type de jointure lecteur comme indiqué ci-dessus). Cette action sera également protocole activer votre application seulement comme invitations et sont gérées de la même façon.

Déplacement vers un réseau XIM à l’aide de l’activation de protocole est identique à un déplacement vers un nouveau réseau XIM comme indiqué dans [d’initialisation et de démarrage](#initialization-and-startup). La seule différence est que lorsque le déplacement réussit, l’appareil mobile sera fourni le lecteur local et distant `xim_player_joined_state_change` structures représentant tous les joueurs applicables. Naturellement, l’appareil d’origine qui était déjà dans le réseau XIM ne se déplacent, mais s’affiche aux utilisateurs de l’appareil de nouveau être ajouté en tant que lecteurs via supplémentaires `xim_player_joined_state_change` structures.

Une fois que les joueurs sur différents appareils sont joints dans un réseau XIM, ils peuvent initier des scénarios multijoueurs, communiquer automatiquement sur les fonctionnalités vocales et de texte et envoyer des messages spécifiques de l’application.

## <a name="basic-matchmaking-and-moving-to-another-xim-network-with-others"></a>Base matchmaking et le déplacement vers un autre réseau XIM avec d’autres utilisateurs

Vous pouvez développer davantage l’expérience de mise en réseau pour un groupe d’amis en déplaçant les joueurs à un réseau XIM qui possède également des inconnus. Inconnus sont des adversaires du monde qui sont affichent à l’aide de service Xbox Live matchmaking en fonction des intérêts similaires.

Un moyen simple de lancer base matchmaking avec XIM est en appelant `xim::move_to_network_using_matchmaking()` sur l’un des appareils avec un remplis `xim_matchmaking_configuration` structure, en prenant les joueurs à partir du réseau XIM actuels en même temps.

L’exemple suivant lance un déplacement à l’aide d’une configuration de matchmaking configurée pour rechercher un total de 8 joueurs pour une correspondance de librement non-équipes. Si le nombre total de 8 lecteurs est introuvables, le système peut toujours correspondre à 2 à 7 joueurs ensemble. Cet exemple utilise une constante définie par l’application, de type uint64_t, nommé MYGAMEMODE_DEATHMATCH, qui représente le mode de jeu pour filtrer issu de. Matchmaking de XIM correspondra uniquement à ce réseau avec d’autres joueurs en spécifiant que même valeur et apporte le long de tout socialement joints à un lecteurs à partir du réseau XIM actuel lorsque vous fournissez le deuxième paramètre `xim_players_to_move::bring_existing_social_players` à `xim::move_to_network_using_matchmaking()`:

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 1;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 2;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 8;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;

xim::singleton_instance().move_to_network_using_matchmaking(matchmakingConfiguration, xim_players_to_move::bring_existing_social_players);
```

Comme les déplacements antérieures, ce processus matchmaking fournira une initiale `xim_move_to_network_starting_state_change` sur tous les appareils et un `xim_move_to_network_succeeded_state_change` une fois le déplacement terminé. Dans la mesure où il s’agit d’un déplacement d’un réseau XIM vers un autre, la différence est qui existent déjà `xim_player` objets ajoutés pour les utilisateurs locaux et distants, et ils resteront de tous les joueurs qui migrent ensemble vers le nouveau réseau XIM. Conversation et les données de communication entre les lecteurs continueront à fonctionner sans interruption pendant que matchmaking est en cours d’exécution.

Matchmaking peut prendre du temps en fonction du nombre de joueurs potentiels dans le pool de matchmaking qui ont également appelé `xim::move_to_network_using_matchmaking()`.

Un `xim_matchmaking_progress_updated_state_change` sera fourni régulièrement tout au long de l’opération pour que vos utilisateurs informés de l’état actuel et vous. Lorsque la correspondance a été trouvée, les joueurs supplémentaires sont ajoutés au réseau XIM classique `xim_player_joined_state_change` et le déplacement se termine.

Une fois que vous avez terminé l’expérience multijoueur avec cet ensemble de lecteurs de « matchmade », vous pouvez répéter le processus pour déplacer vers un autre réseau XIM avec une autre série de matchmaking. Vous verrez chaque joueur joint par le biais de l’avant `xim::move_to_network_using_matchmaking()` opération fournissent un `xim_player_left_state_change` pour indiquer que leurs `xim_player` objets ne sont plus dans le même réseau XIM.

Si lorsque vous choisissez de le lancer à nouveau à l’aide de matchmaking `xim::move_to_network_using_matchmaking()`, vous spécifiez `xim_players_to_move::bring_existing_social_players`, les joueurs qui joint par le biais de points d’entrée de réseaux sociaux (`xim::move_to_network_using_protocol_activated_event_args()` ou `xim::move_to_network_using_joinable_xbox_user_id()`) reste, tandis que le nouveau matchmaking a lieu. Vous pouvez également spécifier `xim_players_to_move::bring_only_local_players` se déconnecte de lecteurs socialement connectés à distance, en laissant uniquement les lecteurs locales pour qu’ils restent. Dans les deux cas, un ensemble différent d’étrangers figurera à l’issue de la deuxième opération de déplacement.

Également, vous avez le choix pour déplacer les joueurs de non matchmade (ou simplement locales joueurs) à un réseau XIM inédits moment du choix entre l’activité de configuration/mode multijoueur matchmaking suivante. L’exemple suivant montre comment un appareil appellerait `xim::move_to_new_network()` pour un réseau XIM avec un maximum de 8 joueurs, mais cette fois en prenant les socialement appartenant à un des lecteurs existants ainsi :

```cpp
xim::singleton_instance().move_to_new_network(8, xim_players_to_move::bring_existing_social_players);
```

Un `xim_move_to_network_starting_state_change` et `xim_move_to_network_succeeded_state_change` permettent tous les appareils participants, avec un `xim_player_left_state_change` pour les joueurs matchmade qui ont été conservés. Sur ces appareils, ils verront même un `xim_player_left_state_change` pour chaque joueur est déplacé en dehors du réseau.

Vous pouvez continuer la migration à partir du réseau XIM vers le réseau XIM de la manière décrite ci-dessus autant de fois comme vous le souhaitez.

Pour des performances, le service Xbox Live pas tente de faire correspondre les groupes de joueurs sur les appareils qui sont peu susceptibles d’être en mesure d’établir toutes les connexions directes peer-to-peer. Si vous développez dans un environnement réseau qui n’est pas correctement configuré pour prendre en charge le standard Xbox Live multijoueurs, le passage au nouveau réseau à l’aide d’opération de matchmaking peut continuer indéfiniment sans mise en correspondance réussit même lorsque vous êtes certain de que disposer de suffisantes joueurs répondant aux critères de matchmaking qui sont tout déplacement et à l’aide de tous les appareils dans le même environnement local. Veillez à exécuter le test de connectivité multijoueur dans les paramètres de réseau application de zone/Xbox et suivez ses recommandations si elle signale des problèmes, en particulier concernant une NAT « Strict ».

## <a name="disabling-matchmaking-nat-rule-for-debugging-purposes"></a>Désactivation de la règle NAT matchmaking à des fins de débogage

Si votre administrateur réseau ne peut pas apporter les modifications d’environnement nécessaires pour prendre en charge les règles NAT de XIM, vous pouvez débloquer votre matchmaking test sur les kits de développement Xbox One en configurant XIM pour autorise la correspondance de périphériques « NAT Strict » sans au moins un « Open Périphérique NAT ». Cela en plaçant un fichier appelé « xim_disable_matchmaking_nat_rule » (contenu n’a pas d’importance) à la racine du lecteur de « zéro title » sur toutes les consoles Xbox One. Un exemple pour faire qui consiste en exécutant ce qui suit à partir d’une invite de commandes XDK avant de lancer votre application, en remplaçant l’espace réservé « {console_name_or_ip_address} » pour chaque console comme il convient :

```bat
echo.>%TEMP%\emptyfile.txt
copy %TEMP%\emptyfile.txt \\{console_name_or_ip_address}\TitleScratch\xim_disable_matchmaking_nat_rule
del %TEMP%\emptyfile.txt
```

> [!NOTE]
> Cette solution de contournement de développement est actuellement disponible uniquement pour les applications de ressources exclusives Xbox One et n’est pas pris en charge pour les Applications Windows universelles. Également Remarque consoles qui sont à l’aide de ce paramètre ne correspondront jamais avec les appareils qui n’ont également ce fichier, quelle que soit l’environnement réseau, par conséquent, veillez à ajouter et supprimer le fichier partout.

## <a name="leaving-a-xim-network-and-cleaning-up"></a>En laissant un réseau XIM et le nettoyage

Lorsque les utilisateurs locaux sont effectuées participant à un réseau XIM, souvent qu'ils seront simplement déplacés dans un nouveau réseau XIM qui permet aux utilisateurs locaux, les utilisateurs invités et « suivi » aux utilisateurs de rejoindre afin qu’ils peuvent continuer de coordination avec leurs amis dans recherche de l’activité suivante. Toutefois, si l’utilisateur est terminé avec toutes les expériences multijoueurs, votre application peut souhaitez commencer en laissant le XIM réseau complètement et revenir à l’état comme si seule `xim::initialize()` et `xim::set_intended_local_xbox_user_ids()` avait été appelée. Cette opération est effectuée à l’aide de la `xim::leave_network()` méthode :

```cpp
xim::singleton_instance().leave_network();
```

Cette méthode commence le processus de déconnexion asynchrone à partir d’autres participants normalement. Ainsi, les appareils à distance à fournir un `xim_player_left_state_change` pour chaque lecteur local déconnecté. L’appareil local sont être fournis un `xim_player_left_state_change` pour chaque joueur locaux et distant qui a été déconnecté. Lorsque tous les déconnecter opérations terminées, une dernière `xim_network_exited_state_change` sera fourni. L’application peut ensuite appeler ' xim::cleanup()'' pour libérer toutes les ressources et revenir à l’état non initialisé :

```cpp
xim::singleton_instance().cleanup();
```

Appel `xim::leave_network()` et attend le `xim_network_exited_state_change` afin de quitter un XIM réseau normalement est toujours recommandé quand un `xim_network_exited_state_change` n’a pas déjà été fourni.

Si `xim::cleanup()` est appelé directement sans `xim::leave_network()` est appelée, des problèmes de performances de communication pour les autres participants se produira dans la mesure où ils seront forcés à envoyer des messages non remis à l’appareil simplement « disparu » par l’appel `xim::cleanup()`.

## <a name="sending-and-receiving-messages"></a>Envoi et réception de messages

XIM et ses composants sous-jacents effectuent tout le travail fastidieux de l’établissement des canaux de communication sécurisée via Internet sans que vous ayez à vous soucier de problèmes de connectivité ou d’atteindre certains, mais pas tous les joueurs. S’il existe des problèmes de connectivité de peer-to-peer fondamentaux, déplacement vers un réseau XIM ne réussira pas.

Lorsqu’un réseau XIM est formé et vérifié avec `xim_move_to_network_succeeded_state_change`, toutes les instances de votre application sur tous les appareils joints sont garanties pour être informé de chaque `xim_player` connecté au réseau XIM et envoyer des messages à un d’eux.

Nous allons vous montrer comment envoyer des messages sur le réseau XIM. L’exemple suivant suppose que la variable 'sendingPlayer' est un pointeur vers un objet de lecteur local valide. 'sendingPlayer' appelle `local()` et appelle `send_data_to_other_players()` pour envoyer la structure du message 'msgData' à tous les joueurs avec remise garantie, séquentiel (locales et distants). Veuillez noter qu’il conduit à tous les acteurs, car une liste de joueurs spécifiques n’a pas été transmise dans la méthode :

```cpp
sendingPlayer->local()->send_data_to_other_players(sizeof(msgData), &msgData, 0, nullptr, xim_send_type::guaranteed_and_sequential);
```

Messages peuvent être remis à tous les joueurs à l’aide de `send_data_to_other_players()` en passant ne pas un tableau des lecteurs spécifiques à la méthode.

Tous les destinataires du message sont fournis un `xim_player_to_player_data_received_state_change` qui inclut un pointeur vers une copie des données, ainsi que des pointeurs vers les xim_player correspondante de l’objet qui a été envoyé et reçoivent localement.

Bien sûr, une livraison garantie et séquentielle est pratique, mais il peut également être un type d’envoi inefficace, dans la mesure où XIM doit retransmettre ou différer les messages si les paquets sont supprimées ou TROUBLES par Internet. Veillez à envisager d’utiliser les autres types d’envoi pour que votre application peut tolérer une perte ou d’avoir des messages arrivent dans le désordre. Dans la mesure où les données de message proviennent d’un ordinateur distant, la meilleure pratique consiste à avoir défini clairement les formats de données, telles que la compression des valeurs de plusieurs octets dans un ordre d’octet particulier (« endianness »). L’application doit également valider les données avant d’agir dessus.

XIM fournit une sécurité au niveau du réseau, il est donc inutile pour les schémas de chiffrement ou signature supplémentaires. Toutefois, nous recommandons de toujours employer « défense en profondeur » afin de protéger contre les bogues de l’application accidentelle et être en mesure de gérer différentes versions de votre application protocole coexistence normalement (au cours de développement, les mises à jour de contenu, etc.). Les connexions d’utilisateurs Internet peuvent être des ressources limitées et en constante évolution. Nous recommandons fortement l’utilisation efficace des messages formats de données et en évitant les conceptions qui envoient chaque frame d’interface utilisateur.

## <a name="assessing-data-pathway-quality"></a>Évaluation de la qualité de chemin d’accès de données

Pour en savoir plus sur la qualité de la voie de données entre deux joueurs, appelez le `xim_player::network_path_information()` (méthode). L’exemple suivant récupère un pointeur vers le `xim_network_path_information` de structure pour un `xim_player` pointeur contenue dans la variable « remotePlayer » :

```cpp
 const xim_network_path_information * networkPathInfo = remotePlayer->network_path_information();
```

La structure retournée inclut la latence aller-retour estimé et le nombre de messages est toujours en attente localement pour la transmission dans le cas que la connexion ne peut pas prendre en charge transmettant plus de données à cet instant.

Si vous voyez que les files d’attente sauvegardez vos données pour un 'xim_player' particulier, vous devez réduire la vitesse à laquelle vous envoyez des données.

Le `xim_network_path_information::round_trip_latency_in_milliseconds` champ représente la latence du réseau sous-jacent et latence estimée de XIM sans queuing. Latence efficace augmente à mesure que `xim_network_path_information::send_queue_size_in_messages` augmente et XIM fonctionne via la file d’attente.

Choisissez un point raisonnable pour démarrer la limitation des appels à `send_data_to_other_players` selon l’utilisation et la configuration requise du jeu. Tous les messages dans la file d’attente d’envoi représente également une augmentation de la latence du réseau efficace.

Une valeur proche de la limite maximale de XIM (actuellement des 3500 messages) est trop élevée pour la plupart des jeux et représente probablement plusieurs secondes de données en attente d’être envoyées en fonction de la vitesse de l’appel `send_data_to_other_players` et la taille est de chaque charge utile de données. Au lieu de cela, choisissez un nombre qui tient compte des exigences de latence du jeu, ainsi que comment instable du jeu `send_data_to_other_players` est du modèle d’appel.

## <a name="sharing-data-using-player-custom-properties"></a>Partage des données à l’aide des propriétés personnalisées du lecteur

La plupart des échanges de données d’application se produisent avec le `xim_player::xim_local::send_data_to_other_players()` (méthode), car elle permet la plus grande maîtrise qui reçoit les données, lorsque qu’il doit recevoir ces données, et comment le système doit traiter les pertes de paquets. Toutefois il existe des fois quand il serait intéressant pour les joueurs partager l’état de base, changent rarement sur elles-mêmes, avec d’autres utilisateurs avec un minimum d’efforts. Par exemple, chaque joueur peut avoir une chaîne fixe représentant le modèle de caractère sélectionné avant d’entrer en mode multijoueur que tous les joueurs utilisent pour rendre leur représentation dans le jeu.

Pour les données qui ne changent pas très souvent sur un lecteur, XIM propose lecteur de propriétés personnalisées. Ces propriétés se composent d’un nom défini par l’application et une valeur, qui sont des paires de chaîne se terminant par null qui peut être appliqué au lecteur local et qui sont automatiquement propagées à tous les périphériques chaque fois qu’elles sont modifiées.

Propriétés du lecteur personnalisé ont plus de synchronisation interne surcharge que la `xim_player::xim_local::send_data_to_other_players()` (méthode), donc pour modifier rapidement des données (par exemple, position de l’acteur), vous devriez toujours utiliser envoie direct.

Paires clé-valeur de propriétés personnalisées Player ont leurs valeurs actuelles fournies automatiquement à de nouveaux appareils concernés quand ces appareils joindre un réseau XIM et voir le lecteur ajouté. Valeurs peuvent être définies en appelant `xim_player::xim_local::set_player_custom_property()` avec des chaînes de nom et la valeur. Dans l’exemple suivant, nous avons défini une propriété nommée « model » pour avoir la valeur « brute » sur une variable locale `xim_player` objet vers lequel pointe la variable « localPlayer » :

```cpp
localPlayer->local()->set_player_custom_property(L"model", L"brute");
```

Entraînent des modifications apportées aux propriétés du lecteur une `xim_player_custom_properties_changed_state_change` doivent être fournies à tous les appareils, les alertes pour les noms des propriétés qui ont été modifiés. La valeur pour un nom donné peut être récupérée sur n’importe quel lecteur, sur local ou distant, avec `xim_player::get_player_custom_property()`. L’exemple suivant récupère la valeur pour une propriété nommée « model » à partir d’un `xim_player` vers lequel pointe la variable « ximPlayer » :

```cpp
PCWSTR modelName = ximPlayer->get_player_custom_property(L"model");
```

La définition d’une nouvelle valeur pour un nom de propriété donné remplace toute valeur existante. Définition d’un nom de propriété à une valeur d’un pointeur de chaîne de valeur null est traitée identique à une chaîne de valeur vide, ce qui est identique à la propriété ne pas après avoir été encore précisée. Noms et les valeurs ne sont pas interprétées par XIM, par conséquent, est conservé sur l’application pour valider le contenu de chaîne en fonction des besoins.

Propriétés du lecteur personnalisé sont toujours réinitialisées lors du déplacement d’un réseau XIM vers un autre. Toutefois, nouveaux joueurs qui rejoignent le réseau XIM recevra des propriétés actuelles de lecteur personnalisé de tous les joueurs disposant d’un jeu.

## <a name="sharing-data-using-network-custom-properties"></a>Partage des données à l’aide des propriétés personnalisées de réseau

Propriétés personnalisées du réseau visent commodité de synchronisation des données spécifiques au réseau XIM qui ne change pas fréquemment. Travail de propriétés personnalisées du réseau identique à [propriétés personnalisées du lecteur](#sharing-data-using-player-custom-properties), sauf qu’elles sont définies sur l’objet de singleton XIM avec `xim::set_network_custom_property()`. L’exemple suivant définit une propriété « mappage » de la valeur « stronghold » :

```cpp
xim::singleton_instance().set_network_custom_property(L"map", L"stronghold");
```

Modifications apportées aux propriétés du réseau entraîne un `xim_network_custom_properties_changed_state_change` doit être fourni à tous les appareils, les alertes pour les noms des propriétés qui ont été modifiés. La valeur pour un nom donné peut être récupérée avec `xim::get_network_custom_property()`, comme dans l’exemple suivant qui Récupère la valeur pour une propriété nommée « carte » :

```cpp
PCWSTR mapName = xim::singleton_instance().get_network_custom_property(L"map");
```

Tout comme [propriétés personnalisées du lecteur](#sharing-data-using-player-custom-properties), définir une nouvelle valeur pour un nom de propriété donné remplace toute valeur existante. Définition d’un nom de propriété à une valeur d’un pointeur de chaîne de valeur null est traitée identique à une chaîne de valeur vide, ce qui est identique à la propriété ne pas après avoir été encore précisée. Noms et les valeurs ne sont pas interprétées par XIM, par conséquent, est conservé sur l’application pour valider le contenu de chaîne en fonction des besoins.

Qui vient d’être créé XIM réseaux toujours commencent par aucun jeu de propriétés personnalisées de réseau. Toutefois, nouveaux joueurs qui rejoignent un réseau XIM existant recevront les valeurs actuelles des propriétés personnalisées du réseau défini pour ce réseau XIM.

## <a name="matchmaking-using-per-player-skill"></a>Équivalent à l’aide de la compétence de par le lecteur

Correspondance des joueurs par un intérêt commun dans un mode particulier de jeu spécifié par l’application est une bonne stratégie de base. Comme le pool de lecteurs disponibles augmente, vous devez envisager également correspondance joueurs en fonction de leur expérience avec votre jeu ou les compétences personnelles afin que les lecteurs expérimentés peuvent bénéficier au défi de la concurrence intègre avec d’autres utilisateurs expérimentés, tandis que les lecteurs plus récents peuvent augmenter de face à d’autres avec des capacités similaires.

Pour ce faire, commencez par fournir le niveau de compétence de tous les joueurs locales dans leurs rôles de par le lecteur et de la structure de configuration de compétence spécifiées dans les appels à `xim_player::xim_local::set_roles_and_skill_configuration()` avant de commencer à déplacer vers un XIM réseau à l’aide de matchmaking. Niveau de compétence est un concept spécifique à l’application et le nombre n’est pas interprété par XIM, à ceci près que matchmaking tout d’abord essayer de trouver les joueurs avec la même valeur de compétences et régulièrement s’étendre sa recherche par incréments de +/-10 pour essayer de trouver d’autres joueurs déclarant compétence valeurs dans une plage autour de cette compétence. L’exemple suivant suppose que l’ordinateur local `xim_player` objet, dont le pointeur est « localPlayer », a une valeur de compétence d’associé spécifiques de l’application uint32_t récupérée à partir de stockage local ou Xbox Live dans une variable appelée « playerSkillValue » :

```cpp
xim_player_roles_and_skill_configuration playerRolesAndSkillConfiguration = { 0 };
playerRolesAndSkillConfiguration.skill = playerSkillValue;

localPlayer->local()->set_roles_and_skill_configuration(&playerRolesAndSkillConfiguration);
```

Lorsque cette opération terminée, tous les participants recevront un `xim_player_roles_and_skill_configuration_changed_state_change` indiquant que ce `xim_player` a modifié son les rôles par le lecteur et la configuration de compétence. La nouvelle valeur peut être récupérée en appelant `xim_player::roles_and_skill_configuration()`. Lorsque tous les joueurs ont des rôles non nulle et configuration de compétence appliquée, vous pouvez déplacer vers un réseau XIM à l’aide de matchmaking avec la valeur true pour le `require_player_roles_and_skill_configuration` champ la `xim_matchmaking_configuration` structure spécifiée à `xim::move_to_network_using_matchmaking()`. L’exemple suivant remplit une configuration de matchmaking qui regroupe un total de 2 à 8 lecteurs pour un librement non-équipes, à l’aide d’un uint64_t constante de mode de jeu spécifiques de l’application définie par la valeur MYGAMEMODE_DEATHMATCH correspondra uniquement à d’autres joueurs spécifiant la même valeur et qui nécessite les rôles par le lecteur et la configuration de la compétence :

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 1;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 2;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 8;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;
matchmakingConfiguration.require_player_roles_and_skill_configuration = true;
```

Lorsque cette structure est fournie pour `xim::move_to_network_using_matchmaking()`, l’opération de déplacement démarre normalement à condition que le déplacement des joueurs ont appelé `xim_player::xim_local::set_roles_and_skill_configuration()` avec une valeur non null `xim_player_roles_and_skill_configuration` pointeur. Si n’importe quel lecteur n’a pas encore, puis le processus de matchmaking seront suspendus et tous les participants recevront un `xim_matchmaking_progress_updated_state_change` avec un `xim_matchmaking_status::waiting_for_player_roles_and_skill_configuration` valeur. Cela inclut les lecteurs que par la suite de joindre le réseau XIM via une invitation envoyée précédemment ou par d’autres moyens (par exemple, un appel à `xim::move_to_network_using_joinable_xbox_user_id()`) avant la fin de matchmaking. Une fois que tous les joueurs ont entré leurs `xim_player_roles_and_skill_configuration` structures, matchmaking reprendra.

Équivalent à l’aide de la compétence de par le lecteur peut aussi être combinée avec le rôle par le lecteur utilisateur matchmaking, comme expliqué dans la section suivante. Si seul un est souhaité, vous pouvez spécifier une valeur de 0 pour l’autre. Il s’agit, car tous les acteurs de déclarer qu’ils ont un `xim_player_roles_and_skill_configuration` compétence valeur 0 correspondra toujours à l’autre.

Une fois le `xim::move_to_network_using_matchmaking()` ou n’importe quel autre réseau XIM déplacer l’opération est terminée, tous les joueurs `xim_player_roles_and_skill_configuration` structures seront automatiquement effacés à un pointeur null (avec accompagné d’un `xim_player_roles_and_skill_configuration_changed_state_change` notification). Si vous envisagez de déplacer vers un autre réseau XIM à l’aide de matchmaking qui nécessite une configuration par le lecteur, vous devrez appeler `xim_player::xim_local::set_roles_and_skill_configuration()` à nouveau avec un nouveau pointeur de structure contenant les informations les plus récentes.

## <a name="matchmaking-using-per-player-role"></a>Équivalent à l’aide du rôle de par le lecteur

Une autre méthode de l’utilisation des rôles par le lecteur et la configuration de la compétence pour améliorer l’expérience des utilisateurs matchmaking est via l’utilisation des rôles de lecteur requis. Cela convient mieux aux jeux qui fournissent des types de caractère sélectionnables qui encouragent des styles différents play coopérative ; Autrement dit, les types qui ne simplement modifiant la représentation sous forme de graphique dans le jeu, mais complémentaires, affectant des attributs tels que défensives » healers » et offensive de fermeture dans « mêlée » et « range » éloigné de contrôler attaquent prise en charge. Les personnalités utilisateurs signifient que préfère participer à une spécialisation particulier. Mais si votre jeu est conçu tel qu’il soit fonctionnellement pas possible de terminer les objectifs au moins une personne qui remplissent chaque rôle, il est parfois préférable faire correspondre ces lecteurs premier ensemble que pour correspondent à des lecteurs sont ensemble puis lui demander à négocier lire les styles entre eux une fois collectées. Ce faire, vous pouvez définir un indicateur de bit unique qui représente chaque rôle doit être spécifié dans un lecteur de donnée `xim_player_roles_and_skill_configuration` structure.

L’exemple suivant définit une valeur de rôle spécifiques de l’application MYROLEBITFLAG_HEALER uint8_t pour l’objet local xim_player, dont le pointeur est « localPlayer » :

```cpp
xim_player_roles_and_skill_configuration playerRolesAndSkillConfiguration = { 0 };
playerRolesAndSkillConfiguration.roles = MYROLEBITFLAG_HEALER;

localPlayer->local()->set_roles_and_skill_configuration(&playerRolesAndSkillConfiguration);
```

Lorsque cette opération terminée, tous les participants recevront un `xim_player_roles_and_skill_configuration_changed_state_change` indiquant que ce `xim_player` a changé sa configuration de rôle par le lecteur. La nouvelle valeur peut être récupérée en appelant `xim_player::roles_and_skill_configuration()`.

Global `xim_matchmaking_configuration` structure spécifiée à `xim::move_to_network_using_matchmaking()` doivent avoir tous les indicateurs de rôles requis combinées à l’aide de bits OR et la valeur true pour le champ require_player_matchmaking_configuration.

L’exemple suivant remplit une configuration de matchmaking qui regroupe un total de 3 lecteurs pour un librement non-équipes. En outre, cet exemple utilise une constante définie par l’application, qui est de type uint64_t et nommé MYGAMEMODE_COOPERATIVE, qui représente le mode de jeu pour filtrer issu de. En outre, la configuration est configurée pour exiger des rôles par le lecteur et configuration de compétence où au moins un lecteur remplit chaque rôles propres à l’application uint8_t qui étaient au niveau du bit OR avaient ensemble et placé dans la configuration (MYROLEBITFLAG_HEALER, MYROLEBITFLAG_ MÊLÉE et MYROLEBITFLAG_RANGE) :

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 1;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 3;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 3;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_COOPERATIVE;
matchmakingConfiguration.required_roles = MYROLEBITFLAG_HEALER | MYROLEBITFLAG_MELEE | MYROLEBITFLAG_RANGE;
matchmakingConfiguration.require_player_roles_and_skill_configuration = true;
```

Lorsque cette structure est fournie à `xim::move_to_network_using_matchmaking()`, l’opération de déplacement démarre comme décrit ci-dessus.

Équivalent à l’aide du rôle de par le lecteur peut également être combinée avec compétence de matchmaking utilisateur par le lecteur. Si seul un est souhaité, spécifiez la valeur 0 pour l’autre. Il s’agit, car tous les acteurs de déclarer qu’ils ont un `xim_player_roles_and_skill_configuration` compétence valeur 0 correspondra toujours à l’autre ; et, si tous les bits sont égales à zéro dans le `xim_matchmaking_configuration` required_roles champ, alors aucun bit de rôle n’est nécessaires pour faire correspondre.

Une fois le `xim::move_to_network_using_matchmaking()` ou n’importe quel autre réseau XIM déplacer l’opération est terminée, tous les joueurs `xim_player_roles_and_skill_configuration` structures seront automatiquement effacés à un pointeur null (avec accompagné d’un `xim_player_roles_and_skill_configuration_changed_state_change` notification). Si vous envisagez de déplacer vers un autre réseau XIM à l’aide de matchmaking qui nécessite une configuration par le lecteur, vous devrez appeler `xim_player::xim_local::set_roles_and_skill_configuration()` à nouveau avec un nouveau pointeur de structure contenant les informations les plus récentes.

## <a name="how-xim-works-with-player-teams"></a>Comment XIM fonctionne avec les équipes de lecteur

Mode multijoueur jeux souvent implique des joueurs organisés sur opposées aux équipes. Rend XIM il est facile d’affecter équipes lorsque matchmaking en définissant `xim_team_configuration`. L’exemple suivant lance un déplacement à l’aide de matchmaking configurée pour rechercher un total de 8 joueurs à placer sur deux équipes égales de 4 (bien que si 4 ne sont pas trouvées, les joueurs de 1 à 3 sont également acceptables), à l’aide d’un uint64_t constante de mode de jeu spécifiques de l’application définie par la valeur MYGAMEMODE_CAPTURETHEFLAG qui correspondra uniquement à d’autres joueurs en spécifiant cette même valeur et les mettre tous les acteurs socialement joint à partir du réseau XIM actuel :

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 2;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 1;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 4;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_CAPTURETHEFLAG;

xim::singleton_instance().move_to_network_using_matchmaking(matchmakingConfiguration, xim_players_to_move::bring_existing_social_players);
```

Lorsque ce une opération de déplacement de réseau XIM est terminée, les joueurs recevront une équipe valeur d’index 1 à {n} correspondant aux équipes {n} demandés. La vraie signification de n’importe quelle valeur d’index équipe particulier est à l’application. Valeur d’index du joueur team est récupéré `xim_player::team_index()`. Lorsque vous utilisez un `xim_team_configuration` avec deux ou plusieurs équipes, joueurs jamais recevront une équipe valeur d’index zéro par l’appel à `xim::move_to_network_using_matchmaking()`. Il s’agit en revanche pour les lecteurs qui sont ajoutés au réseau XIM avec tout autre configuration ou un autre type d’opération de déplacement (comme une activation de protocole résultant d’une invitation acceptée), qui aura toujours un index de base zéro team. Il peut être utile traiter les index de l’équipe 0 comme une équipe de « non attribuée » spéciale.

L’exemple suivant récupère l’index de l’équipe pour un objet xim_player dont le pointeur est dans la variable « ximPlayer » :

```cpp
uint8_t playerTeamIndex = ximPlayer->team_index();
```

Pour une expérience utilisateur par défaut (à ne pas mention réduite l’opportunité pour le comportement des joueurs négatif), le service Xbox Live n’est jamais les joueurs de fractionnement qui migrent vers un réseau XIM ensemble sur les différentes équipes.

La valeur d’index équipe assignée initialement par matchmaking est uniquement une recommandation et l’application peut le modifier pour les joueurs locales à tout moment `xim_player::xim_local::set_team_index()`. Il peut également être appelé dans les réseaux XIM qui n’utilisent pas du tout matchmaking. L’exemple suivant configure un pointeur de lecteur « localPlayer » pour avoir une nouvelle valeur d’index équipe d’un :

```cpp
localPlayer->local()->set_team_index(1);
```

Tous les appareils sont informés du fait que le joueur a une nouvelle valeur d’index team en vigueur quand elles sont proposées un `xim_player_team_index_changed_state_change` pour que le lecteur. Vous pouvez appeler `xim_player::xim_local::set_team_index()` à tout moment.

Matchmaking évalue les rôles de lecteur requise indépendamment des équipes. Par conséquent, il a déconseillé d’utiliser des équipes et rôles requis en tant que critères de configuration simultanées matchmaking étant donné que les équipes sont équilibrées par nombre de lecteur, pas par les rôles de lecteur remplie.

## <a name="working-with-chat"></a>Utilisation de conversation

Communication de conversations vocales et de texte sont automatiquement activés entre acteurs dans un réseau XIM. XIM gère l’interaction avec tout matériel casque et le microphone de voix pour vous. Votre application n’a pas besoin effectuer une grande pour la conversation, mais elle ne possède pas une exigence concernant les conversations textuelles : prise en charge d’entrée et l’affichage. Entrée de texte est nécessaire car, même sur des plateformes ou game genres qui historiquement n’ont pas eu généralisée clavier physique utiliser, lecteurs peuvent configurer le système pour utiliser les technologies d’assistance synthèse vocale. De même, affichage de texte est requis, car les lecteurs peuvent configurer le système pour utiliser la reconnaissance vocale.

Ces préférences peuvent être détectées sur les lecteurs locaux en appelant `xim_player::xim_local::chat_text_to_speech_conversion_preference_enabled()` et `xim_player::xim_local::chat_speech_to_text_conversion_preference_enabled()`, et vous pouvez souhaiter activer conditionnellement des mécanismes de texte. Toutefois, nous vous recommandons d’envisager rendant le texte d’entrée et afficher les options qui sont toujours disponibles.

 `Windows::Xbox::UI::Accessability` une classe Xbox One est conçue spécifiquement pour fournir un rendu simple de discuter de la partie en se concentrant sur les technologies d’assistance de parole-texte.

Une fois que vous avez entrée de texte fournie par un clavier réel ou virtuel, transmettez la chaîne à la `xim_player::xim_local::send_chat_text()` (méthode). Le code suivant illustre l’envoi d’un exemple de chaîne codée en dur à partir d’une variable locale `xim_player` objet vers lequel pointe la variable « localPlayer » :

```cpp
localPlayer->local()->send_chat_text(L"Example chat text");
```

Ce texte de la conversation est remis à tous les acteurs dans le réseau XIM qui peut recevoir les communications de conversation à partir du lecteur local d’origine. Il peut être synthétisée audio de reconnaissance vocale et elle peut être fournie comme un `xim_chat_text_received_state_change`.

Votre application doit effectuer une copie d’une chaîne de texte reçue et afficher ainsi que certains d’identification de l’acteur d’origine pour une quantité appropriée de temps (ou dans une fenêtre de défilement).

Il existe également quelques meilleures pratiques concernant la conversation. Il est recommandé que n’importe où joueurs sont affichés, en particulier dans une liste d’identités comme un tableau des scores, également afficher Muet/à propos des icônes en tant que commentaires de l’utilisateur. Cela est effectué en appelant `xim_player::chat_indicator()` pour récupérer un `xim_player_chat_indicator` représentant l’état en cours, instantanée de conversation pour ce lecteur. L’exemple suivant illustre la récupération de la valeur d’indicateur pour un `xim_player` objet vers lequel pointe la variable « ximPlayer » pour déterminer une valeur de constante icône particulière pour attribuer à une variable 'iconToShow' :

```cpp
switch (ximPlayer->chat_indicator())
{
   case xim_player_chat_indicator::silent:
   {
       iconToShow = Icon_InactiveSpeaker;
       break;
   }

   case xim_player_chat_indicator::talking:
   {
       iconToShow = Icon_ActiveSpeaker;
       break;
   }

   case xim_player_chat_indicator::muted:
   {
       iconToShow = Icon_MutedSpeaker;
       break;
   }
   ...
}
```

La valeur signalée par `xim_player::chat_indicator()` est amené à changer fréquemment en tant que lecteurs start et stop parlé, par exemple. Il est conçu pour prendre en charge les applications interrogent chaque frame d’interface utilisateur en conséquence.

> [!NOTE]
> Si un utilisateur local ne dispose pas des privilèges suffisants de communications en raison de leurs paramètres d’appareil, `xim_player::chat_indicator()` retournera `xim_player_chat_indicator::platform_restricted`. L’attente pour répondre aux exigences pour la plateforme est que l’application afficher iconographie indiquant une restriction de la plateforme pour la discussion vocale ou de messagerie et d’un message à l’utilisateur indiquant le problème. Un exemple de message que nous recommandons est : « Nous sommes désolés, vous n'êtes pas autorisé à démarrer une conversation vers la droite. »

## <a name="muting-players"></a>Désactivation des acteurs

Une autre pratique recommandée consiste à prendre en charge des lecteurs en sourdine. XIM gère automatiquement le système sourdine initiées par les utilisateurs via les cartes du joueur, mais les applications doivent prendre en charge le jeu spécifique temporaires sourdine qui peuvent être effectuées dans l’interface utilisateur jeu via le `xim_player::set_chat_muted()` (méthode). L’exemple suivant commence la désactivation à distance `xim_player` objet vers lequel pointe la variable « remotePlayer » afin qu’il y a aucune conversation vocale et aucune conversation texte n’est reçue à partir de celui-ci :

```cpp
remotePlayer->set_chat_muted(true);
```

La désactivation immédiatement en vigueur et qu’il existe aucune `xim_state_change` associé. Il peut être annulée en appelant `xim_player::set_chat_muted()` à nouveau avec la valeur false. L’exemple suivant unmutes à distance `xim_player` objet vers lequel pointe la variable « remotePlayer » :

```cpp
remotePlayer->set_chat_muted(false);
```

Désactive le reste en vigueur pour tant que le `xim_player` existe, y compris lors du déplacement vers un nouveau réseau XIM avec le lecteur. Il n’est pas rendu persistant si le joueur quitte et rejoint le même utilisateur (en tant que nouvelle `xim_player` instance).

Lecteurs démarrent généralement en l’état unmuted. Si votre application souhaite démarrer un lecteur dans l’état muet pour des raisons de jeu, elle peut appeler `xim_player::set_chat_muted()` sur le `xim_player` objet avant la fin de traitement associé `xim_player_joined_state_change`, et garantit XIM il n’y aura aucune période de temps où vocal audio le joueur peut se faire entendre.

Une vérification muet automatique basée sur la réputation de joueur se produit lorsqu’un lecteur distant joint le réseau XIM. Si le joueur a un indicateur de mauvaise réputation, le joueur est automatiquement désactivé. Désactivation affecte uniquement état local et par conséquent persiste si un lecteur se déplace entre les réseaux. La vérification muet réputation automatique est effectuée une seule fois et pas réévaluée à nouveau pour tant que le `xim_player` reste valide.

## <a name="configuring-chat-targets-using-player-teams"></a>Configuration des cibles de conversation à l’aide des équipes de lecteur

Comme indiqué dans le [XIM comment fonctionne avec les équipes de lecteur](#how-xim-works-with-player-teams) section de ce document, la vraie signification de n’importe quelle valeur d’index équipe particulier est à l’application. XIM n’interprète pas les à l’exception des comparaisons d’égalité en ce qui concerne la configuration de la conversation cible. Si la configuration de cible de conversation signalées par `xim::chat_targets()` est actuellement `xim_chat_targets::same_team_index_only`, puis n’importe quel lecteur donné sera échanger uniquement de communication de conversation avec un autre si les deux ont la même valeur que signalée par `xim_player::team_index()` (et de confidentialité / stratégie également autoriser).

Pour être prudent et prise en charge des scénarios de concurrence, qui vient d’être créé des réseaux XIM sont automatiquement configurés pour utiliser par défaut `xim_chat_targets::same_team_index_only`. Toutefois, discuter avec des adversaires vanquished sur l’autre équipe peut être souhaitable, par exemple, dans un jeu après « entrée ». Vous pouvez demander à XIM pour autoriser tout le monde de communiquer avec tout le monde (où politique de confidentialité et autorisent) en appelant `xim::set_chat_targets()`. L’exemple suivant commence la configuration de tous les participants dans le réseau XIM à utiliser un `xim_chat_targets::all_players` valeur :

```cpp
xim::singleton_instance().set_chat_targets(xim_chat_targets::all_players);
```

Tous les participants sont informés qu’un nouveau paramètre de cible est en vigueur quand elles sont proposées un `xim_chat_targets_changed_state_change`.

Comme indiqué précédemment, la plupart des types de déplacement de réseau XIM initialement affectera tous les acteurs de la valeur d’index équipe égale à zéro. Cela signifie une configuration de `xim_chat_targets::same_team_index_only` est susceptible de se distinguent pas des `xim_chat_targets::all_players` par défaut. Toutefois, lecteurs déplacement vers un réseau XIM à l’aide de matchmaking auront des différentes valeurs d’index de l’équipe si la configuration de matchmaking `xim_team_matchmaking_mode` valeur déclarée deux ou plusieurs équipes. Vous pouvez également appeler `xim_player::xim_local::set_team_index()` à tout moment, comme indiqué ci-dessus. Si votre application est à l’aide de valeurs d’index non nulle équipe par le biais de ces méthodes, n’oubliez pas de gérer les cibles de conversation en cours définissant de manière appropriée.

## <a name="automatic-background-filling-of-player-slots-backfill-matchmaking"></a>Remplissage d’arrière-plan automatique d’emplacements de lecteur (« renvoi » matchmaking)

Des groupes distincts de joueurs appelant `xim::move_to_network_using_matchmaking()` en même temps donne le service Xbox Live, la plus grande flexibilité pour les organiser dans réseaux XIM optimales nouveau rapidement. Toutefois, certains scénarios de jeu souhaite conserver un réseau XIM intact et trouver un fabricant correspondant uniquement les lecteurs supplémentaires de remplissage des emplacements de lecteur vacants. Prend en charge XIM configuration matchmaking de fonctionner dans un automatique en arrière-plan remplissant mode ou « renvoi », en appelant `xim::set_network_configuration()` avec un `xim_network_configuration` qui a `xim_allowed_player_joins::matchmade` l’indicateur est défini sur son `xim_network_configuration::allowed_player_joins` propriété.

L’exemple suivant configure matchmaking de renvoi pour essayer de trouver un total de 8 joueurs pour un librement non-équipes (bien qu’il est si 8 sont introuvables, lecteurs de 2 à 7 sont également acceptables), à l’aide d’un uint64_t constante de mode de jeu spécifiques de l’application définie par la valeur MYGAMEMODE_ DEATHMATCH qui correspondra uniquement à d’autres joueurs en spécifiant cette même valeur :

```cpp
xim_network_configuration networkConfiguration = *xim::singleton_instance().network_configuration();
networkConfiguration.allowed_player_joins |= xim_allowed_player_joins::matchmade;
networkConfiguration.team_configuration.team_count = 1;
networkConfiguration.team_configuration.min_player_count_per_team = 2;
networkConfiguration.team_configuration.max_player_count_per_team = 8;
networkConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;

xim::singleton_instance().set_network_configuration(&networkConfiguration);
```

Cela rend le XIM existant réseau disponibles aux appareils appelant `xim::move_to_network_using_matchmaking()` de manière normale. Ces appareils ne voir aucun comportement à modifier. Les participants dans le réseau XIM de renvoi ne déplacera pas, mais il sera fournis une `xim_network_configuration_changed_state_change` signifiant renvoi sous tension, ainsi que plusieurs `xim_matchmaking_progress_updated_state_change` notifications le cas échéant. N’importe quel lecteur matchmade sera ajouté au réseau XIM à l’aide de la normale `xim_player_joined_state_change`.

Par défaut, renvoi matchmaking est toujours en cours indéfiniment, bien qu’il ne tente pas d’ajouter des joueurs si le réseau XIM possède déjà le nombre maximal de lecteurs spécifiée par le paramètre xim_team_configuration. Renvoi peut être désactivé en xim_allowed_player_joins de paramètre pour ne pas autoriser matchmade. L’exemple suivant désactive le renvoi en désactivant l’indicateur xim_allowed_player_joins::matchmade tout en conservant tous les autres indicateurs existants et les paramètres de configuration réseau.

```cpp
xim_network_configuration networkConfiguration = *xim::singleton_instance().network_configuration();
networkConfiguration.allowed_player_joins &= ~xim_allowed_player_joins::matchmade;
xim::singleton_instance().set_network_configuration(&networkConfiguration);
```

Correspondante `xim_network_configuration_changed_state_change` sera fourni à tous les appareils, et une fois ce processus asynchrone terminée, une dernière `xim_matchmaking_progress_updated_state_change` doivent être fournies avec `xim_matchmaking_status::none` pour indiquer qu’aucune autres joueurs matchmade ne seront ajoutés au réseau XIM.

Lors de l’activation de matchmaking renvoi avec un `xim_team_configuration` qui déclare deux équipes ou plus, tous les lecteurs existants doivent avoir un index d’équipe valide est comprise entre 1 et le nombre d’équipes. Cela inclut les joueurs qui ont appelé `xim_player::xim_local::set_team_index()` pour spécifier une valeur personnalisée ou qui ont rejoint à l’aide d’une invitation ou via d’autres sociale signifie (par exemple, un appel à `xim::move_to_network_using_joinable_xbox_user_id`) et ont été ajoutés avec une valeur d’index équipe par défaut de 0. Si n’importe quel lecteur n’a pas un index valide d’équipe, puis le processus de matchmaking seront suspendus et tous les participants recevront un `xim_matchmaking_progress_updated_state_change` avec un `xim_matchmaking_status::waiting_for_player_team_index` valeur. Une fois que tous les joueurs ont fourni ou leurs valeurs d’index équipe avec corrigées `xim_player::xim_local::set_team_index()`, renvoi matchmaking reprendra. Vous trouverez plus d’informations dans le [XIM comment fonctionne avec les équipes de lecteur](#how-xim-works-with-player-teams) section de ce document.

De même, lors de l’activation de matchmaking renvoi avec un `xim_network_configuration` structure avec le `require_player_roles_and_skill_configuration` champ défini sur true pour les rôles ou des compétences, tous les joueurs doivent avoir spécifié une configuration de matchmaking non null par lecteur. Si n’importe quel lecteur n’a pas encore, puis le processus de matchmaking seront suspendus et tous les participants recevront un `xim_matchmaking_progress_updated_state_change` avec un `xim_matchmaking_status::waiting_for_player_roles_and_skill_configuration` valeur. Une fois que tous les joueurs ont entré leurs `xim_player_roles_and_skill_configuration` structures, renvoi matchmaking reprendra. Vous trouverez plus d’informations dans le [Matchmaking à l’aide des compétences par lecteur](#matchmaking-using-per-player-skill) et [Matchmaking à l’aide du rôle de par le lecteur](#matchmaking-using-per-player-role) sections de ce document.

## <a name="querying-joinable-networks"></a>Interrogation des réseaux joignables

Tandis que matchmaking est un excellent moyen interconnecter les joueurs rapidement, il est parfois préférable d’autoriser les lecteurs pour la détection des réseaux joignables à l’aide de critères de recherche personnalisé, puis sélectionnez le réseau qu’ils souhaitent rejoindre. Cela peut être particulièrement avantageux lorsqu’une session de jeu peut avoir un grand ensemble de règles de jeu configurables et les préférences de lecteur. Pour ce faire, un réseau existant doit être rendu peut être interrogé par l’activation de `xim_allowed_player_joins::queried` joinability et la configuration des informations réseau disponibles à d’autres personnes en dehors du réseau via un appel à `xim::set_network_configuration()`.

L’exemple suivant active `xim_allowed_player_joins::queried` joinability, définit le réseau configuration avec une configuration de l’équipe qui permet un total de 1 à 8 lecteurs en 1 équipe, un uint64_t constante de mode de jeu spécifiques de l’application définie par la valeur GAME_MODE_BRAWL, une description « cat et conversion boxing de mouton correspondent à », un uint32_t constante index de mappage de spécifiques de l’application définie par la valeur MAP_KITCHEN et inclut des balises « chatrequired », « facile », « spectatorallowed » :

```cpp
PCWSTR tags[] = { L"chatrequired", L"easy", L"spectatorallowed" };
xim_network_configuration networkConfiguration = *xim::singleton_instance().network_configuration();
networkConfiguration.allowed_player_joins |= xim_allowed_player_joins::queried;
networkConfiguration.team_configuration.team_count = 1;
networkConfiguration.team_configuration.min_player_count_per_team = 1;
networkConfiguration.team_configuration.max_player_count_per_team = 8;
networkConfiguration.custom_game_mode = GAME_MODE_BRAWL;
networkConfiguration.description = L"cat and sheep's boxing match";
networkConfiguration.map_index = MAP_KITCHEN;
networkConfiguration.tag_count = _countof(tags);
networkConfiguration.tags = tags;

xim::set_network_configuration(&networkConfiguration);
```

Autres intervenants en dehors du réseau peuvent rechercher ensuite le réseau en appelant `xim::start_joinable_network_query()` avec un ensemble de filtres qui correspondent aux informations réseau dans le précédent `xim::set_network_configuration()` appeler. L’exemple suivant démarre une requête réseau joignable avec l’option de filtre de mode de jeu interrogera uniquement pour les réseaux en utilisant le mode de jeux spécifiques de l’application défini par la valeur GAME_MODE_BRAWL :

```cpp
xim_joinable_network_query_filters queryFilters = { 0 };
queryFilters.custom_game_mode_filter = GAME_MODE_BRAWL;

xim::start_joinable_network_query(queryFilters);
```

Voici un autre exemple qui utilise l’option de filtres de balise à interroger pour les réseaux de balise « facile » et « spectatorallowed » dans leur configuration peut être interrogée publique :

```cpp
PCWSTR tagFilters[] = { L"easy", L"spectatorallowed" };
xim_joinable_network_query_filters queryFilters = { 0 };
queryFilters.tag_filter_count = _countof(tagFilters);
queryFilters.tag_filters = tagFilters;

xim::start_joinable_network_query(queryFilters);
```

Options de filtrage différents peuvent également être combinées. L’exemple suivant qui utilise l’option de filtre de mode de jeu et la balise de filtre permet de démarrer une requête pour les réseaux que les deux ont la constante de mode de jeu spécifiques de l’application GAME_MODE_BRAWL et la balise « facile » :

```cpp
PCWSTR tagFilters[] = { L"easy" };
xim_joinable_network_query_filters queryFilters = { 0 };
queryFilters.custom_game_mode_filter = GAME_MODE_BRAWL;
queryFilters.tag_filter_count = _countof(tagFilters);
queryFilters.tag_filters = tagFilters;

xim::start_joinable_network_query(queryFilters);
```

Si l’opération de requête réussit, l’application reçoit un `xim_start_joinable_network_query_completed_state_change` à partir de laquelle l’application peut récupérer la liste des réseaux joignables. L’application recevra également continuellement `xim_joinable_network_query_updated_state_change` pour réseaux joignables supplémentaires ou de toutes les modifications qui se produisent à la liste des réseaux joignables renvoyée jusqu'à ce qu’elle soit arrêtée manuellement ou automatiquement. La requête en cours d’exécution peut être arrêtée manuellement en appelant `xim::stop_joinable_network_query()`. Il est automatiquement arrêtée lors de l’appel `xim::start_joinable_network_query()` pour démarrer une nouvelle requête.

L’application peut essayer de joindre à un réseau dans la liste des réseaux joignables en appelant `xim::move_to_network_using_joinable_network_information()`. L’exemple suivant suppose que vous essayez de joindre un `xim_joinable_network_information` référencée par le pointeur 'selectedNetwork' n’est pas sécurisé par un code secret (ce qui nous nullptr au second paramètre) :

```cpp
xim::move_to_network_using_joinable_network_information(selectedNetwork, nullptr);
```

Lorsque vous activez la requête réseau avec un xim_team_configuration qui déclare deux ou plusieurs équipes, les joueurs joint en appelant `xim::move_to_network_using_joinable_network_information()` aura une valeur d’index équipe par défaut de 0.

> [!NOTE]
> Si l’application a spécifié à plusieurs utilisateurs locales et doit être jointe à un réseau ayant moins d’espace que le nombre d’utilisateurs locales, la jointure peut réussir. Mais uniquement le nombre autorisé d’utilisateurs locaux peut-être se connecter au réseau.