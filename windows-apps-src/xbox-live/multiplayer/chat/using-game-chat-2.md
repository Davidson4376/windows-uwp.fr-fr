---
title: Utilisation de la conversation jeu 2
description: Découvrez comment Xbox Live Game Chat 2 permet d’ajouter la communication vocale à votre jeu.
ms.date: 03/20/2018
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, chat jeu 2, jeu chat, communication vocale
ms.localizationpriority: medium
ms.openlocfilehash: 43fbd7cec037df735686aa60bc6cd57217875e33
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639254"
---
# <a name="using-game-chat-2-c"></a>Utilisation de la conversation jeu 2 (C++)

Il s’agit d’une brève procédure pas à pas sur l’utilisation d’API de 2 de Chat de jeu C++. Les développeurs souhaitant accéder au jeu Chat 2 par le biais de jeux C# devrait être [utilisez Game Chat 2 WinRT Projections](using-game-chat-2-winrt.md).

1. [Conditions préalables](#prerequisites)
2. [Initialisation](#initialization)
3. [Configurer les utilisateurs](#configuring-users)
4. [Traitement des trames de données](#processing-data-frames)
5. [Traitement des modifications d’état](#processing-state-changes)
6. [Conversations textuelles](#text-chat)
7. [Accessibilité](#accessibility)
8. [UI](#ui)
9. [Désactivation](#muting)
10. [Mauvaise réputation automatique-muet](#bad-reputation-auto-mute)
11. [Privilège et confidentialité](#privilege-and-privacy)
12. [Cleanup](#cleanup)
13. [Modèle de défaillance](#failure-model)
14. [Comment configurer des scénarios courants](#how-to-configure-popular-scenarios)

## <a name="prerequisites"></a>Conditions préalables

Avant de commencer à coder avec jeu Chat 2, vous devez avoir configuré AppXManifest de votre application pour déclarer la fonctionnalité de l’appareil « microphone ». AppXManifest fonctionnalités sont décrites plus en détail dans leurs sections respectives de la documentation de la plateforme ; l’extrait suivant montre le nœud de capacité de périphérique « microphone » doit exister sous le nœud Package/fonctionnalités ou sinon conversation sera bloquée :

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <Package ...>
   <Identity ... />
   ...
   <Capabilities>
     <DeviceCapability Name="microphone" />
   </Capabilities>
 </Package>
```

Compilation jeu Chat 2 nécessite l’inclusion de l’en-tête GameChat2.h principal. Pour lier correctement, votre projet doit également inclure GameChat2Impl.h dans au moins une unité de compilation (un en-tête précompilé commun est recommandé dans la mesure où ces implémentations de fonctions stub sont petites et facilement au compilateur de générer en tant que « inline »).

L’interface de jeu Chat 2 ne nécessite pas d’un projet à choisir entre la compilation avec C++ / c++ / CX et C++ traditionnel ; Il peut être utilisé avec le. L’implémentation n’également lever des exceptions comme un moyen d’erreur non irrécupérable reporting donc vous pouvez l’utiliser facilement des projets sans exception si vous préférez. L’implémentation est le cas, toutefois, lèvent des exceptions comme moyen de rapport d’erreurs irrécupérables (consultez [modèle de défaillance](#failure) pour plus de détails).

## <a name="initialization"></a>Initialisation

Commencer à interagir avec la bibliothèque à l’initialisation de l’instance de singleton de jeu Chat 2 avec des paramètres qui s’appliquent à la durée de vie de l’initialisation du singleton.

```cpp
chat_manager::singleton_instance().initialize(...);
```

## <a name="configuring-users"></a>Configurer les utilisateurs

Une fois que l’instance est initialisée, vous devez ajouter les utilisateurs locaux à l’instance 2 de discuter de jeu. Dans cet exemple, l’utilisateur A représentera un utilisateur local.

```cpp
chat_user* chatUserA = chat_manager::singleton_instance().add_local_user(<user_a_xuid>);
```

Vous devez également ajouter les utilisateurs à distance et les identificateurs qui seront utilisés pour représenter le « point de terminaison distant » qui se trouve sur l’utilisateur. « Point de terminaison » est une instance de l’application en cours d’exécution sur un périphérique distant. Dans cet exemple, l’utilisateur B est sur le point de terminaison X, les utilisateurs C et D sont sur X de point de terminaison d’y. point de terminaison est assigné arbitrairement identificateur « 1 » et Y du point de terminaison est assigné arbitrairement identificateur « 2 ». Vous informer jeu Chat 2 des utilisateurs distants avec ces appels :

```cpp
chat_user* chatUserB = chat_manager::singleton_instance().add_remote_user(<user_b_xuid>, 1);
chat_user* chatUserC = chat_manager::singleton_instance().add_remote_user(<user_c_xuid>, 2);
chat_user* chatUserD = chat_manager::singleton_instance().add_remote_user(<user_d_xuid>, 2);
```

Maintenant, vous configurez la relation de communication entre chaque utilisateur distant et l’utilisateur local. Dans cet exemple, supposons que l’utilisateur A et B se trouvent sur la même équipe et la communication bidirectionnelle est autorisée. `c_communicationRelationshipSendAndReceiveAll` est une constante définie dans GameChat2.h pour représenter la communication bidirectionnelle. Définissez la relation de l’utilisateur A à l’utilisateur B avec :

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAndReceiveAll);
```

Supposons maintenant que les utilisateurs C et D sont « spectateurs » et doivent être autorisé à écouter à l’utilisateur A, mais pas parler. `c_communicationRelationshipSendAll` est une constante définie dans GameChat2.h pour représenter cette communication unidirectionnelle. Définir les relations avec :

```cpp
chatUserA->local()->set_communication_relationship(chatUserC, c_communicationRelationshipSendAll);
chatUserA->local()->set_communication_relationship(chatUserD, c_communicationRelationshipSendAll);
```

Si à tout moment, il existe des utilisateurs distants qui ont été ajoutés à l’instance de singleton, mais n’ont pas été configurés pour communiquer avec les utilisateurs locaux - c’est OK ! Cela est peut-être le cas dans les scénarios où les utilisateurs de déterminer les équipes ou peuvent modifier les canaux énonciation arbitrairement. Jeu 2 Chat met en cache uniquement les informations (par exemple, les relations de confidentialité et sa réputation) pour les utilisateurs qui ont été ajoutés à l’instance, ce qui est utile informer 2 de discuter du jeu de tous les utilisateurs possibles, même si elles ne peut pas parler à tous les utilisateurs locaux à un moment précis dans le temps.

Enfin, supposons que D de l’utilisateur a quitté le jeu et doivent être supprimé à partir de l’instance locale de jeu 2 Chat. Qui peut être fait avec l’appel suivant :

```cpp
chat_manager::singleton_instance().remove_user(chatUserD);
```

Appel `chat_manager::remove_user()` peut invalider l’objet utilisateur. Si vous utilisez [manipulation audio en temps réel](real-time-audio-manipulation.md), reportez-vous à la [des durées de vie utilisateur Chat](real-time-audio-manipulation.md#chat-user-lifetimes) documentation pour plus d’informations. Sinon, l’objet utilisateur est invalidé immédiatement lorsque `chat_manager::remove_user()` est appelée. Une restriction subtile sur lorsque les utilisateurs peuvent être supprimés est détaillée dans [traitement des modifications d’état](#state).

## <a name="processing-data-frames"></a>Traitement des trames de données

Jeu 2 Chat n’a pas sa propre couche de transport ; Cela doit être fourni par l’application. Ce plug-in est géré par l’application régulière, fréquentes les appels à la `chat_manager::start_processing_data_frames()` et `chat_manager::finish_processing_data_frames()` paire de méthodes. Ces méthodes sont comment jeu Chat 2 fournit des données sortantes à l’application. Ils sont conçus pour fonctionner rapidement tels qu’ils peuvent être interrogés fréquemment sur un thread dédié mise en réseau. Cela fournit un emplacement pratique pour récupérer toutes les données en file d’attente sans vous soucier du caractère imprévisible de minutage de réseau ou de la complexité de rappel multi-thread.

Lorsque `chat_manager::start_processing_data_frames()` est appelée, toutes les données en file d’attente sont signalées dans un tableau de `game_chat_data_frame` structure des pointeurs. Applications doivent effectuer une itération sur le tableau, d’inspecter la cible « points de terminaison » et utiliser la couche d’application mise en réseau pour fournir les données vers les instances d’application distant approprié. Une fois terminé avec tous les `game_chat_data_frame` structures, le tableau doit être passé au jeu Chat 2 pour libérer les ressources en appelant `chat_manager:finish_processing_data_frames()`. Exemple :

```cpp
uint32_t dataFrameCount;
game_chat_data_frame_array dataFrames;
chat_manager::singleton_instance().start_processing_data_frames(&dataFrameCount, &dataFrames);
for (uint32_t dataFrameIndex = 0; dataFrameIndex < dataFrameCount; ++dataFrameIndex)
{
    game_chat_data_frame const* dataFrame = dataFrames[dataFrameIndex];
    HandleOutgoingDataFrame(
        dataFrame->packet_byte_count,
        dataFrame->packet_buffer,
        dataFrame->target_endpoint_identifier_count,
        dataFrame->target_endpoint_identifiers,
        dataFrame->transport_requirement
        );
}
chat_manager::singleton_instance().finish_processing_data_frames(dataFrames);
```

Plus fréquemment les trames de données sont traitées, plus la latence audio apparente à l’utilisateur final. L’audio est fusionné en trames de données de 40 ms ; Il s’agit de la période d’interrogation suggérée.

## <a name="processing-state-changes"></a>Traitement des modifications d’état

Jeu 2 Chat offre des mises à jour à l’application, telles que les messages texte reçu, via des appels de l’application régulière, à intervalles réguliers à le `chat_manager::start_processing_state_changes()` et `chat_manager::finish_processing_state_changes()` paire de méthodes. Ils sont conçus pour fonctionner rapidement, telles qu’elles peuvent être appelées chaque bloc de graphique dans la boucle de rendu de l’interface utilisateur. Cela fournit un emplacement pratique pour récupérer toutes les modifications en file d’attente sans vous soucier du caractère imprévisible de minutage de réseau ou de la complexité de rappel multi-thread.

Lorsque `chat_manager::start_processing_state_changes()` est appelée, toutes les mises à jour en file d’attente sont signalés dans un tableau de `game_chat_state_change` structure des pointeurs. Les applications doivent effectuer une itération sur le tableau, inspecter la structure de base pour son type plus spécifique, effectuer un cast de la structure de base correspondant plus détaillées tapez, puis gérer cette mise à jour comme il convient. Une fois terminé avec tous les `game_chat_state_change` objets actuellement disponibles, ce tableau doit être transmis au jeu Chat 2 pour libérer les ressources en appelant `chat_manager::finish_processing_state_changes()`. Exemple :

```cpp
uint32_t stateChangeCount;
game_chat_state_change_array gameChatStateChanges;
chat_manager::singleton_instance().start_processing_state_changes(&stateChangeCount, &gameChatStateChanges);

for (uint32_t stateChangeIndex = 0; stateChangeIndex < stateChangeCount; ++stateChangeIndex)
{
    switch (gameChatStateChanges[stateChangeIndex]->state_change_type)
    {
        case game_chat_state_change_type::text_chat_received:
        {
            HandleTextChatReceived(static_cast<const game_chat_text_chat_received_state_change*>(gameChatStateChanges[stateChangeIndex]));
            break;
        }

        case Xs::game_chat_2::game_chat_state_change_type::transcribed_chat_received:
        {
            HandleTranscribedChatReceived(static_cast<const Xs::game_chat_2::game_chat_transcribed_chat_received_state_change*>(gameChatStateChanges[stateChangeIndex]));
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_state_changes(gameChatStateChanges);
```

Étant donné que `chat_manager::remove_user()` invalide immédiatement la mémoire associée à un objet utilisateur, et les changements d’état peuvent contenir des pointeurs vers les objets utilisateur, `chat_manager::remove_user()` ne doit pas être appelée lors du traitement des modifications d’état.

## <a name="text-chat"></a>Conversations textuelles

Pour envoyer les conversations textuelles, utilisez `chat_user::chat_user_local::send_chat_text()`. Exemple :

```cpp
chatUserA->local()->send_chat_text(L"Hello");
```

Jeu 2 Chat générera une trame de données contenant le message ; les points de terminaison cible pour la trame de données seront celles associées aux utilisateurs qui ont été configurés pour recevoir le texte à partir de l’utilisateur local. Lorsque les données sont traitées par les points de terminaison distants, le message sera exposé un `game_chat_text_chat_received_state_change`. À l’instar de la conversation vocale, les restrictions de privilège et de confidentialité sont respectées pour les conversations textuelles. Si une paire d’utilisateurs ont été configurés pour autoriser les conversations de texte, mais les restrictions de privilège ou de confidentialité interdisent cette communication, le message est abandonné.

Prise en charge d’entrée de conversation de texte et l’affichage est requise pour l’accessibilité (consultez [accessibilité](#access) pour plus d’informations).

## <a name="accessibility"></a>Accessibilité

Prise en charge de la conversation de texte d’entrée et l’affichage est obligatoire. Entrée de texte est nécessaire car, même sur des plateformes ou game genres qui historiquement n’ont pas eu généralisée clavier physique utiliser, les utilisateurs peuvent configurer le système pour utiliser les technologies d’assistance synthèse vocale. De même, l’affichage de texte est requis, car les utilisateurs peuvent configurer le système pour utiliser la parole-texte. Ces préférences peuvent être détectées sur les utilisateurs locaux en appelant le `chat_user::chat_user_local::text_to_speech_conversion_preference_enabled()` et `chat_user::chat_user_local::speech_to_text_conversion_preference_enabled()` méthodes respectivement, et vous pouvez souhaiter activer conditionnellement des mécanismes de texte.

### <a name="text-to-speech"></a>Conversion de texte par synthèse vocale

Lorsqu’un utilisateur a activé, de synthèse vocale `chat_user::chat_user_local::text_to_speech_conversion_preference_enabled()` renvoie « true ». Lorsque cet état est détecté, l’application doit fournir une méthode d’entrée de texte. Une fois que vous avez l’entrée de texte fournie par un clavier réel ou virtuel, transmettez la chaîne à la `chat_user::chat_user_local::synthesize_text_to_speech()` (méthode). Jeu 2 Chat détectera et synthétiser les données audio en fonction de la chaîne et les préférences de voix accessible de l’utilisateur. Exemple :

```cpp
chat_userA->local()->synthesize_text_to_speech(L"Hello");
```

L’audio synthétisé dans le cadre de cette opération vont être transporté à tous les utilisateurs qui ont été configurés pour recevoir les données audio à partir de cet utilisateur local. Si `chat_user::chat_user_local::synthesize_text_to_speech()` est appelée sur un utilisateur qui ne dispose pas de n’entreprendre aucune action de synthèse vocale sera jeu Chat 2 est activée.

### <a name="speech-to-text"></a>Parole-texte

Lorsqu’un utilisateur a activé, parole-texte `chat_user::chat_user_local::speech_to_text_conversion_preference_enabled()` retournera la valeur true. Lorsque cet état est détecté, l’application doit être prêt à fournir que l’interface utilisateur associés aux messages de conversation retranscrits. Jeu 2 Chat sera automatiquement transcription audio de chaque utilisateur distant et l’exposer via un `game_chat_transcribed_chat_received_state_change`.

> `Windows::Xbox::UI::Accessibility` une classe Xbox One est conçue spécifiquement pour fournir un rendu simple de discuter de la partie en se concentrant sur les technologies d’assistance de parole-texte.

### <a name="speech-to-text-performance-considerations"></a>Considérations sur les performances de la parole-texte

Lors de la reconnaissance vocale est activée, l’instance 2 de discuter de jeu sur chaque appareil à distance initie une connexion de socket web avec le point de terminaison de services de reconnaissance vocale. Chaque client distant de jeu Chat 2 charge audio sur le point de terminaison de services de reconnaissance vocale via ce websocket ; le point de terminaison de services de reconnaissance vocale retourne parfois un message de la transcription à l’appareil distant. L’appareil distant envoie ensuite le message de transcription (autrement dit, un message texte) sur l’appareil local, où le message retranscrits est donné par jeu Chat 2 à l’application à restituer.

Par conséquent, le coût de performances principal de la parole-texte est l’utilisation du réseau. La plupart du trafic réseau est le téléchargement de l’audio encodé. Le websocket télécharge audio qui a déjà été encodée par jeu Chat 2 dans le chemin d’accès de la conversation vocale « normal » ; l’application a un contrôle sur la vitesse de transmission via `chat_manager::set_audio_encoding_type_and_bitrate`.

## <a name="ui"></a>Interface utilisateur

Il est recommandé que n’importe où joueurs sont affichés, en particulier dans une liste d’identités comme un tableau des scores, également afficher Muet/à propos des icônes en tant que commentaires de l’utilisateur. Cela est effectué en appelant `chat_user::chat_indicator()` pour récupérer un `game_chat_user_chat_indicator` représentant l’état en cours, instantanée de conversation pour ce lecteur. L’exemple suivant illustre la récupération de la valeur d’indicateur pour un `chat_user` objet vers lequel pointe la variable « chatUserA » pour déterminer une valeur de constante icône particulière pour attribuer à une variable 'iconToShow' :

```cpp
switch (chatUserA->chat_indicator())
{
   case game_chat_user_chat_indicator::silent:
   {
       iconToShow = Icon_InactiveSpeaker;
       break;
   }

   case game_chat_user_chat_indicator::talking:
   {
       iconToShow = Icon_ActiveSpeaker;
       break;
   }

   case game_chat_user_chat_indicator::local_microphone_muted:
   {
       iconToShow = Icon_MutedSpeaker;
       break;
   }
   ...
}
```

La valeur signalée par `xim_player::chat_indicator()` est amené à changer fréquemment en tant que lecteurs start et stop parlé, par exemple. Il est conçu pour prendre en charge les applications interrogent chaque frame d’interface utilisateur en conséquence.

## <a name="muting"></a>Désactivation

Le `chat_user::chat_user_local::set_microphone_muted()` méthode peut être utilisée pour activer/désactiver l’état muet du microphone d’un utilisateur local. Lorsque le microphone est coupé, sans contenu audio à partir de ce microphone est capturé. Si l’utilisateur se trouve sur un appareil partagé, telles que Kinect, l’état muet s’applique à tous les utilisateurs.

Le `chat_user::chat_user_local::microphone_muted()` méthode peut être utilisée pour récupérer l’état muet du microphone d’un utilisateur local. Cette méthode reflète uniquement si le microphone de l’utilisateur local a été muet dans les logiciels via un appel à `chat_user::chat_user_local::set_microphone_muted()`; cette méthode ne reflète pas un désactivation du son matériel contrôlé, par exemple, via un bouton sur casque de l’utilisateur. Il n’existe aucune méthode pour récupérer l’état muet du matériel de périphérique audio d’un utilisateur à travers le jeu Chat 2.

Le `chat_user::chat_user_local::set_remote_user_muted()` méthode peut être utilisée pour activer/désactiver l’état de désactivation d’un utilisateur distant par rapport à un utilisateur local particulier. Lorsque l’utilisateur distant est coupé, l’utilisateur local ne sont pas entendre des données audio ou recevoir des messages texte à partir de l’utilisateur distant.

## <a name="bad-reputation-auto-mute"></a>Mauvaise réputation automatique-muet

En règle générale, les utilisateurs distants commenceront unmuted. Jeu 2 Chat démarre les utilisateurs dans un état muet lorsque (1) l’utilisateur distant n’est pas amis avec l’utilisateur local, et (2) l’utilisateur distant a un indicateur de mauvaise réputation. Lorsque les utilisateurs sont ignorés en raison de cette opération, `chat_user::chat_indicator()` retournera `game_chat_user_chat_indicator::reputation_restricted`. Cet état sera remplacé par le premier appel à `chat_user::chat_user_local::set_remote_user_muted()` qui inclut l’utilisateur distant en tant que l’utilisateur cible.

## <a name="privilege-and-privacy"></a>Privilège et confidentialité

En haut de la relation de communication configurée par le jeu, jeux 2 Chat applique les restrictions de privilège et de confidentialité. Jeu 2 Chat effectue des recherches de restriction de privilège et de confidentialité lors de l’ajout d’un utilisateur ; l’utilisateur `chat_user::chat_indicator()` retournera toujours `game_chat_user_chat_indicator::silent` jusqu'à ce que ces opérations terminées. Si la communication avec un utilisateur est affectée par un privilège ou confidentialité, l’utilisateur de restriction `chat_user::chat_indicator()` retournera `game_chat_user_chat_indicator::platform_restricted`. Restrictions de communication de plate-forme s’appliquent aux conversations vocales et de texte ; Il ne sera jamais y avoir une instance où conversations textuelles sont bloquée par une restriction de la plateforme, mais la conversation vocale n’est pas, ou vice versa.

`chat_user::chat_user_local::get_effective_communication_relationship()` peut être utilisé pour aider à distinguer lorsque les utilisateurs ne peuvent pas communiquer en raison de privilèges incomplet et les opérations de confidentialité. Elle retourne la relation de communication appliquée par jeu Chat 2 sous la forme de `game_chat_communication_relationship_flags` et la raison la relation ne peut pas être égale à la relation configurée sous la forme de `game_chat_communication_relationship_adjuster`. Par exemple, si les opérations de recherche sont toujours en cours, le `game_chat_communication_relationship_adjuster` sera `game_chat_communication_relationship_adjuster::intializing`. Cette méthode est censée être utilisée dans les scénarios de développement et débogage ; Il ne doit pas être utilisé pour influencer l’interface utilisateur (consultez [l’interface utilisateur](#UI)).

## <a name="cleanup"></a>Nettoyage

Lorsque l’application n’a plus besoin des communications via jeu Chat 2, vous devez appeler `chat_manager::cleanup()`. Cela permet de jeu Chat 2 libérer des ressources qui ont été alloués pour gérer les communications.

## <a name="failure-model"></a>Modèle de défaillance

L’implémentation de jeu Chat 2 ne lève des exceptions comme un moyen d’erreur non irrécupérable reporting donc vous pouvez l’utiliser facilement des projets sans exception si vous préférez. Jeu 2 Chat est le cas, toutefois, lèvent des exceptions pour informer les erreurs irrécupérables. Ces erreurs sont le résultat d’une mauvaise utilisation des API, telles que l’ajout d’un utilisateur à l’instance de conversation de jeu avant l’initialisation de l’instance ou de l’accès à un objet utilisateur après que qu’il a été supprimé de l’instance 2 de discuter de jeu. Ces erreurs sont supposés être interceptées tôt dans le développement et peuvent être corrigées en modifiant le modèle utilisé pour interagir avec le jeu Chat 2. Lorsqu’une telle erreur se produit, un Conseil au concernant ce qui a provoqué l’erreur est écrit dans le débogueur avant de l’exception est levée.

## <a name="how-to-configure-popular-scenarios"></a>Comment configurer des scénarios courants

### <a name="push-to-talk"></a>Appuyer pour parler

Appuyer pour parler doit être implémenté avec `chat_user::chat_user_local::set_microphone_muted()`. Appelez `set_microphone_muted(false)` pour autoriser la reconnaissance vocale et `set_microphone_muted(true)` afin de le restreindre. Cette méthode fournit la réponse de latence la plus basse à partir de jeu Chat 2.

### <a name="teams"></a>Équipes

Supposons que l’utilisateur A et B sont sur l’équipe Blue et utilisateur C et D de l’utilisateur se trouvent sur équipe rouge. Chaque utilisateur est dans une instance unique de l’application.

Sur l’appareil de l’utilisateur A :

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAndReceiveAll);
chatUserA->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
chatUserA->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

Sur l’appareil de l’utilisateur B :

```cpp
chatUserB->local()->set_communication_relationship(chatUserA, c_communicationRelationshipSendAndReceiveAll);
chatUserB->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
chatUserB->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

Sur l’appareil de l’utilisateur C :

```cpp
chatUserC->local()->set_communication_relationship(chatUserA, game_chat_communication_relationship_flags::none);
chatUserC->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserC->local()->set_communication_relationship(chatUserD, c_communicationRelationshipSendAndReceiveAll);
```

Sur l’appareil de l’utilisateur D :

```cpp
chatUserD->local()->set_communication_relationship(chatUserA, game_chat_communication_relationship_flags::none);
chatUserD->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserD->local()->set_communication_relationship(chatUserC, c_communicationRelationshipSendAndReceiveAll);
```

### <a name="broadcast"></a>Diffusion

Supposons que l’utilisateur A est le leader en donnant des commandes et les utilisateurs B, C et D peuvent écouter uniquement. Chaque lecteur est sur un appareil unique.

Sur l’appareil de l’utilisateur A :

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAll);
chatUserA->local()->set_communication_relationship(chatUserC, c_communicationRelationshipSendAll);
chatUserA->local()->set_communication_relationship(chatUserD, c_communicationRelationshipSendAll);
```

Sur l’appareil de l’utilisateur B :

```cpp
chatUserB->local()->set_communication_relationship(chatUserA, c_communicationRelationshipReceiveAll);
chatUserB->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
chatUserB->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

Sur l’appareil de l’utilisateur C :

```cpp
chatUserC->local()->set_communication_relationship(chatUserA, c_communicationRelationshipReceiveAll);
chatUserC->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserC->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

Sur l’appareil de l’utilisateur D :

```cpp
chatUserD->local()->set_communication_relationship(chatUserA, c_communicationRelationshipReceiveAll);
chatUserD->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserD->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
```
