---
title: Migration de Chat jeu 2
description: Découvrez comment migrer le code de jeu Chat existant pour utiliser le jeu Chat 2.
ms.date: 05/02/2018
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, chat jeu 2, jeu chat, communication vocale
ms.localizationpriority: medium
ms.openlocfilehash: e963210091694a07114f10d5a3dc531a353621df
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653584"
---
# <a name="migration-from-game-chat-to-game-chat-2"></a>Migration à partir de la conversation de jeu au jeu Chat 2

Ce document décrit en détail les similitudes entre jeu Chat et 2 de discuter de jeu et comment migrer à partir de la conversation de jeu vers jeu Chat 2. Il est par conséquent, pour les titres qui ont une implémentation existante de discuter de jeu que vous souhaitent migrer vers le jeu Chat 2. Si vous ne disposez pas d’une implémentation de conversation de jeu, le point de départ suggéré est [2 de discuter de jeu à l’aide de](using-game-chat-2.md). 

## <a name="preface"></a>Préface

L’API de conversation de jeu d’origine est une API WinRT qui exposé un concept d’utilisateurs de chat et canaux de voix pour faciliter l’implémentation de scénarios de conversation de voix de jeu Xbox Live. L’API de conversation Game repose sur l’API de la conversation, qui est lui-même une API WinRT qui exposé un concept d’utilisateurs de chat et canaux de voix tout en nécessitant de bas niveau Gestion des périphériques audio. Jeu Chat 2 est le successeur de la conversation de jeu d’origine et les API de conversation : il a été conçu pour être une API plus simple pour les scénarios de conversation instantanée de base, telles que la communication d’équipe, tout en fournissant simultanément plus de souplesse pour les scénarios de conversation avancées, telles que la diffusion-style la communication et la manipulation audio en temps réel. Chat de jeu et jeu Chat 2 remplissent la niche même : les API fournissent une méthode pratique pour l’intégration de Xbox Live activé, la conversation vocale de dans le jeu, tandis que le titre fournit une couche de transport pour la transmission des paquets de données vers et depuis des instances distantes de conversation jeu ou votre jeu Conversation 2.

L’API 2 de Chat jeu présente de nombreux avantages sur le Chat du jeu d’origine et les API de conversation. Voici quelques points clés sont les suivantes :
* Un flexible orienté utilisateur API n’est pas limité au modèle « canal ».
* Améliorations de la bande passante en raison de filtrage des paquets par les sources de données et les récepteurs de données.
* Une restriction interne à 2 threads de longue durées avec l’affinité configurables par l’application.
* Une API disponible sous les formes de C++ et WinRT.
* Modèle de consommation simplifiée en alignant avec un modèle de « ne fonctionnent pas ».
* Raccordements de mémoire pour rediriger les allocations de jeu Chat 2 via un allocateur personnalisé.
* UWP identiques + en-têtes d’Application de ressource exclusif (ère) pour une expérience de développement de multiplateforme plus pratique.

Ce document décrit en détail les similitudes entre jeu Chat et 2 de discuter de jeu et comment migrer à partir de la conversation de jeu à l’API de C++ Game Chat 2. Si vous êtes intéressé par la migration à partir de la conversation de jeu à l’API de WinRT jeu Chat 2, il est recommandé que vous lire ce document pour comprendre comment mapper des concepts de discuter de jeu à jeu Chat 2 et de voir [à l’aide de jeu Chat 2 WinRT Projections](using-game-chat-2-winrt.md) pour le modèles spécifiques à WinRT. L’exemple de code pour le jeu d’origine Chat dans ce document utilise C++ / c++ / CX.

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

L’interface de jeu Chat 2 ne nécessite pas d’un projet à choisir entre la compilation avec C++ / c++ / CX et C++ traditionnel ; Il peut être utilisé avec le. L’implémentation n’également lever des exceptions comme un moyen d’erreur non irrécupérable reporting donc vous pouvez l’utiliser facilement des projets sans exception si vous préférez. L’implémentation est le cas, toutefois, lèvent des exceptions comme moyen de rapport d’erreurs irrécupérables (consultez [modèle de défaillance](#failure-model-and-debugging) pour plus de détails).

## <a name="initialization"></a>Initialisation

### <a name="game-chat"></a>Conversation de jeu

Interaction avec le Chat du jeu d’origine s’effectue via la `ChatManager` classe. L’exemple suivant montre comment construire un `ChatManager` instance à l’aide des paramètres par défaut :

```cpp
auto chatManager = ref new ChatManager();
```

### <a name="game-chat-2"></a>Game Chat 2

Toute interaction avec jeu Chat 2 s’effectue via jeu Chat 2 `chat_manager` singleton. Le singleton doit être initialisé avant toute interaction significative avec la bibliothèque peut se produire. Le singleton requiert que le nombre maximal d’utilisateurs de chat de locaux et distants simultanés soit spécifié au moment de l’initialisation. Il s’agit, car le jeu Chat 2 pré-alloue mémoire proportionnelle au nombre attendu d’utilisateurs. L’exemple suivant montre comment initialiser l’instance de singleton lorsque le nombre maximal d’utilisateurs de chat de locaux et distants simultanés seront quatre :

```cpp
chat_manager::singleton_instance().initialize(4);
```

Il existe plusieurs paramètres facultatifs qui peuvent être spécifiées lors de l’initialisation, telles que la valeur par défaut restituer le volume des utilisateurs de conversation à distance et si jeu Chat 2 doit procéder à une conversion automatique de parole-texte. Pour plus d’informations sur ces paramètres, reportez-vous à la documentation de `chat_manager::initialize()`.

## <a name="configuring-users"></a>Configurer les utilisateurs

### <a name="game-chat"></a>Conversation de jeu

Ajout d’utilisateurs locaux à l’API de discuter de jeu d’origine s’effectue via `ChatManager::AddLocalUserToChatChannelAsync()`. Ajout de l’utilisateur à plusieurs canaux de conversation nécessite plusieurs appels, chacun spécifiant un autre canal. L’exemple suivant montre comment ajouter l’utilisateur xuid « myXuid » pour le canal 0 :

```cpp
auto asyncOperation = chatManager->AddLocalUserToChatChannelAsync(0, L"myXuid");
```

Les utilisateurs distants ne sont pas ajoutés directement à l’instance. Quand un appareil distant s’affiche dans le réseau du titre, le titre crée un objet qui représente le périphérique distant et informe le jeu Chat du nouvel appareil via `ChatManager::HandleNewRemoteConsole()`. Le titre doit également fournir une méthode de comparaison les objets qui représentent les périphériques distants de discuter de jeu en implémentant `CompareUniqueConsoleIdentifiersHandler`. L’exemple suivant montre comment fournir ce délégué pour discuter de jeu, en supposant que `Platform::String` objets sont utilisés pour représenter les périphériques distants et un nouvel appareil représenté par la chaîne `L"1"` est joint au réseau du titre :

```cpp
auto token = chatManager->OnCompareUniqueConsoleIdentifiers +=
    ref new CompareUniqueConsoleIdentifiersHandler(
        [this](Platform::Object^ obj1, Platform::Object^ obj2)
    {
        return (static_cast<Platform::String^>(obj1)->Equals(static_cast<Platform::String^>(obj2)));
    });

...

Platform::String^ newDeviceIdentifier = ref new Platform::String(L"1");
chatManager->HandleNewRemoteConsole(newDeviceIdentifier);
```

Une fois que le jeu Chat a été informé de cet appareil, il génère des paquets contenant des informations sur les utilisateurs locaux et fournir ces paquets au titre pour la transmission à l’instance de jeu Chat sur le périphérique distant. De même, l’instance de jeu Chat sur le périphérique distant générera des paquets contenant des informations sur les utilisateurs sur l’appareil distant que le titre transmettra à l’instance locale de Chat du jeu. Lorsque l’instance locale reçoit des paquets contenant des informations sur les nouveaux utilisateurs à distance, les nouveaux utilisateurs à distance sont ajoutés à la liste de l’instance locale d’utilisateurs, disponibles via `ChatManager::GetUsers()`.

Suppression d’utilisateurs à partir de l’instance de jeu Chat est effectuée via des appels similaire à `ChatManager::RemoveLocalUserFromChatChannelAsync()` et `ChatManager::RemoveRemoteConsoleAsync()`

### <a name="game-chat-2"></a>Game Chat 2

Ajout d’utilisateurs locaux à jeu Chat 2 s’effectue de façon synchrone par le biais `chat_manager::add_local_user()`. Dans cet exemple, l’utilisateur A représentera un utilisateur local avec l’Id d’utilisateur de Xbox `L"myLocalXboxUserId"`:

```cpp
chat_user* chatUserA = chat_manager::singleton_instance().add_local_user(L"myLocalXboxUserId");
```

Notez que l’utilisateur n’est pas ajouté à un canal particulier - 2 de discuter de jeu utilise un concept de « relations de communication » au lieu de canaux à gérer si les utilisateurs peuvent contacter et communiquer entre eux. La méthode de configuration des relations de communication est traitée ultérieurement dans cette section.

Utilisateurs similaires en local, les utilisateurs distants sont ajoutés synchrone à la conversation jeu locale instance 2. Les utilisateurs à distance doivent simultanément être associés avec les identificateurs qui seront utilisés pour représenter le périphérique distant. Jeu 2 Chat fait référence à une instance de l’application en cours d’exécution sur un périphérique distant comme un « Endpoint ». Dans cet exemple, l’utilisateur B sera un utilisateur avec l’Id d’utilisateur de Xbox `L"remoteXboxUserId"` sur un point de terminaison représenté par l’entier `1`.

```cpp
chat_user* chatUserB = chat_manager::singleton_instance().add_remote_user(L"remoteXboxUserId", 1);
```

Une fois que les utilisateurs ont été ajoutés à l’instance, vous devez configurer la relation « communication » entre chaque utilisateur distant et chaque utilisateur local. Dans cet exemple, supposons que l’utilisateur A et B se trouvent sur la même équipe et la communication bidirectionnelle est autorisée. `c_communicationRelationshiSendAndReceiveAll` est une constante définie dans GameChat2.h pour représenter la communication bidirectionnelle. La relation peut être configurée avec :

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAndReceiveAll);
```

Enfin, supposons que l’utilisateur B a quitté le jeu et doivent être supprimé à partir de l’instance locale de discuter de jeu. Qui est exécutée de façon synchrone avec l’appel suivant :

```cpp
chat_manager::singleton_instance().remove_user(chatUserD);
```

Reportez-vous à [à l’aide de jeu Chat 2 - configuration utilisateurs](using-game-chat-2.md#configuring-users) pour plus d’exemples ou la référence pour les différentes méthodes d’API pour plus d’informations.

## <a name="processing-data"></a>Traitement des données

### <a name="game-chat"></a>Conversation de jeu

Conversation jeu n’a pas sa propre couche de transport ; Cela doit être fourni par l’application. Les paquets sortants sont gérées en vous abonnant à la `OnOutgoingChatPacketReady` événement et en inspectant les arguments pour déterminer les exigences de transport et de destination du paquet. L’exemple suivant montre comment s’abonner à l’événement et de transférer les arguments la `HandleOutgoingPacket()` méthode implémentée par le titre :

```cpp
auto token = chatManager->OnOutgoingChatPacketReady +=
    ref new Windows::Foundation::EventHandler<Microsoft::Xbox::GameChat::ChatPacketEventArgs^>(
        [this](Platform::Object^, Microsoft::Xbox::GameChat::ChatPacketEventArgs^ args)
    {
        this->HandleOutgoingPacket(args);
    });
```

Paquets entrants sont soumis à jeu Chat via `ChatManager::ProcessingIncomingChatMessage()`. La mémoire tampon de paquet brut et l’identificateur de l’appareil distant doivent être fournis. L’exemple suivant montre comment envoyer un paquet est stocké dans local `packetBuffer` et l’identificateur de périphérique distant stockées dans la variable locale `remoteIdentifier`.

```cpp
Platform::String^ remoteIdentifier = /* The identifier associated with the device that generated this packet */
Windows::Storage::Streams::IBuffer^ packetBuffer = /* The incoming chat packet */

chatManager->ProcessIncomingChatMessage(packetBuffer, remoteIdentifier);
```

### <a name="game-chat-2"></a>Game Chat 2

De même, jeu Chat 2 n’a pas sa propre couche de transport ; Cela doit être fourni par l’application. Les paquets sortants sont gérées par l’application régulière, fréquentes les appels à la `chat_manager::start_processing_data_frames()` et `chat_manager::finish_processing_data_frames()` paire de méthodes. Ces méthodes sont comment jeu Chat 2 fournit des données sortantes à l’application. Ils sont conçus pour fonctionner rapidement tels qu’ils peuvent être interrogés fréquemment sur un thread dédié mise en réseau. Cela fournit un emplacement pratique pour récupérer toutes les données en file d’attente sans vous soucier du caractère imprévisible de minutage de réseau ou de la complexité de rappel multi-thread.

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

Données entrantes sont envoyées au jeu Chat 2 via `chat_manager::processing_incoming_data()`. Le tampon de données et l’identificateur de point de terminaison distant doivent être fournis. L’exemple suivant montre comment envoyer un paquet de données qui est stocké dans la variable locale `dataFrame` et l’identificateur de point de terminaison distant stockées dans la variable locale `remoteEndpointIdentifier`:

```cpp
uin64_t remoteEndpointIdentier = /* The identifier associated with the endpoint that generated this packet */
uint32_t dataFrameSize = /* The size of the incoming data frame, in bytes */
uint8_t* dataFrame = /* A pointer to the buffer containing the incoming data */

chatManager::singleton_instance().process_incoming_data(remoteEndpointIdentifier, dataFrameSize, dataFrame);
```

## <a name="processing-events"></a>Traitement des événements

### <a name="game-chat"></a>Conversation de jeu

Conversation jeu utilise un modèle de gestion des événements pour informer l’application lorsque quelque chose de significatif se produit - telles que la réception d’un message texte ou la modification de préférence de l’accessibilité d’un utilisateur. L’application doit s’abonner à et implémenter un gestionnaire pour chaque événement de votre choix. Cet exemple montre comment s’abonner à la `OnTextMessageReceived` événement et transmet les arguments de la `OnTextChatReceived()` ou `OnTranscribedChatReceived()` méthodes implémentées par l’application :

```cpp
auto token = chatManager->OnTextMessageReceived +=
    ref new Windows::Foundation::EventHandler<Microsoft::Xbox::GameChat::TextMessageReceivedEventArgs^>(
        [this](Platform::Object^, Microsoft::Xbox::GameChat::TextMessageReceivedEventArgs^ args)
    {
        if (args->ChatTextMessageType != ChatTextMessageType::TranscribedSpeechMessage)
        {
            this->OnTextChatReceived(args);
        }
        else
        {
            this->OnTranscribedChatReceived(args);
        }
    });
```

### <a name="game-chat-2"></a>Game Chat 2

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

### <a name="game-chat"></a>Conversation de jeu

Pour envoyer les conversations textuelles avec jeu Chat, `GameChatUser::GenerateTextMessage()` peut être utilisé. L’exemple suivant montre comment envoyer un message texte avec un utilisateur de chat local représenté par le `chatUser` variable :

```cpp
chatUser->GenerateTextMessage(L"Hello", false);
```

Le deuxième paramètre boolean contrôle de synthèse vocale. Pour plus d’informations, consultez [accessibilité](#accessibility). Conversation jeu génère ensuite un paquet de conversation contenant ce message. Des instances distantes de jeu Chat êtes avertis du message texte via la `OnTextMessageReceived` événement.

### <a name="game-chat-2"></a>Game Chat 2

Pour envoyer de discuter avec jeu Chat 2, utilisez `chat_user::chat_user_local::send_chat_text()`. L’exemple suivant montre comment envoyer un message texte avec un utilisateur de chat local représenté par le `chatUser` variable :

```cpp
chatUser->local()->send_chat_text(L"Hello");
```

Jeu 2 Chat générera une trame de données contenant le message ; les points de terminaison cible pour la trame de données seront celles associées aux utilisateurs qui ont été configurés pour recevoir le texte à partir de l’utilisateur local. Lorsque les données sont traitées par les points de terminaison distants, le message sera exposé un `game_chat_text_chat_received_state_change`. À l’instar de la conversation vocale, les restrictions de privilège et de confidentialité sont respectées pour les conversations textuelles. Si une paire d’utilisateurs ont été configurés pour autoriser les conversations de texte, mais les restrictions de privilège ou de confidentialité interdisent cette communication, le message de texte est supprimé en mode silencieux.

Prise en charge d’entrée de conversation de texte et l’affichage est requise pour l’accessibilité (consultez [accessibilité](#accessibility) pour plus d’informations).

## <a name="accessibility"></a>Accessibilité

Prise en charge de la conversation de texte d’entrée et l’affichage est requis pour le jeu Chat et 2 de conversation de jeu. Entrée de texte est nécessaire car, même sur des plateformes ou game genres qui historiquement n’ont pas eu généralisée clavier physique utiliser, les utilisateurs peuvent configurer le système pour utiliser les technologies d’assistance synthèse vocale. De même, l’affichage de texte est requis, car les utilisateurs peuvent configurer le système pour utiliser la parole-texte. Chat de jeu et le jeu Chat 2 fournissent des méthodes de détection et en respectant les préférences d’accessibilité d’un utilisateur ; Vous pouvez souhaiter activer conditionnellement des mécanismes de texte basés sur ces paramètres.

### <a name="text-to-speech---game-chat"></a>Synthèse vocale-jeu de conversation

Lorsqu’un utilisateur a activé, de synthèse vocale `GameChatUser::HasRequestedSynthesizedAudio()` retournera la valeur true. Lorsque cet état est détecté, `GameChatUser::GenerateTextMessage()` génère en outre audio de synthèse vocale qui est inséré dans le flux audio associé à l’utilisateur local. L’exemple suivant montre comment envoyer un message texte avec un utilisateur local représenté par le `chatUser` variable :

```cpp
chatUser->GenerateTextMessage(L"Hello", true);
```

Le deuxième paramètre boolean est utilisé pour permettre à l’application remplacer la préférence de la parole-texte : « true » indique que Game Chat doit générer audio de synthèse vocale lors de la synthèse vocale préférence utilisateur a été activée, alors que « false » indique que jeu Conversation ne doit jamais générer audio de synthèse vocale à partir du message.

### <a name="text-to-speech---game-chat-2"></a>Synthèse vocale-conversation jeu 2

Lorsqu’un utilisateur a activé, de synthèse vocale `chat_user::chat_user_local::text_to_speech_conversion_preference_enabled()` renvoie « true ». Lorsque cet état est détecté, l’application doit fournir une méthode d’entrée de texte. Une fois que vous avez l’entrée de texte fournie par un clavier réel ou virtuel, transmettez la chaîne à la `chat_user::chat_user_local::synthesize_text_to_speech()` (méthode). Jeu 2 Chat détectera et synthétiser les données audio en fonction de la chaîne et les préférences de voix accessible de l’utilisateur. Exemple :

```cpp
chat_userA->local()->synthesize_text_to_speech(L"Hello");
```

L’audio synthétisé dans le cadre de cette opération vont être transporté à tous les utilisateurs qui ont été configurés pour recevoir les données audio à partir de cet utilisateur local. Si `chat_user::chat_user_local::synthesize_text_to_speech()` est appelée sur un utilisateur qui ne dispose pas de n’entreprendre aucune action de synthèse vocale sera jeu Chat 2 est activée.

### <a name="speech-to-text---game-chat"></a>Parole-texte - conversation jeu

Lorsqu’un utilisateur a activé, parole-texte `GameChatUser::HasRequestSynthesizedAudio()` renvoie « true ». Lorsque cet état est détecté, Chat jeu sera automatiquement transcrire le contenu audio de l’audio de chaque utilisateur à distance et l’exposer via la `OnTextMessageReceived` événement. Lorsque le `OnTextMessageReceived` événement est déclenché en raison de la réception d’un message de transcription, le `TextMessageReceivedEventArgs` indique un type de message de `ChatTextMessageType::TranscribedSpeechMessage`.

### <a name="speech-to-text---game-chat-2"></a>Parole-texte - conversation jeu 2

Lorsqu’un utilisateur a activé, parole-texte `chat_user::chat_user_local::speech_to_text_conversion_preference_enabled()` retournera la valeur true. Lorsque cet état est détecté, l’application doit être prêt à fournir que l’interface utilisateur associés aux messages de conversation retranscrits. Jeu 2 Chat sera automatiquement transcription audio de chaque utilisateur distant et l’exposer via un `game_chat_transcribed_chat_received_state_change`.

> `Windows::Xbox::UI::Accessibility` une classe Xbox One est conçue spécifiquement pour fournir un rendu simple de discuter de la partie en se concentrant sur les technologies d’assistance de parole-texte.

Reportez-vous à [2 de conversation à l’aide de jeu - considérations sur les performances de la parole-texte](using-game-chat-2.md#speech-to-text-performance-considerations) pour plus d’informations sur les considérations relatives aux performances de la parole-texte.

## <a name="ui"></a>Interface utilisateur

Il est recommandé que n’importe où joueurs sont affichés, en particulier dans une liste d’identités comme un tableau des scores, également afficher Muet/à propos des icônes en tant que commentaires de l’utilisateur. Jeu 2 Chat introduit un indicateur fusionné qui fournit un moyen plus simple de déterminer les éléments d’interface utilisateur appropriées pour afficher.

### <a name="game-chat"></a>Conversation de jeu

Un `GameChatUser` a trois propriétés qui sont couramment inspectées lors de la détermination des éléments d’interface utilisateur appropriées - `GameChatUser::TalkingMode`, `GameChatUser::IsMuted`, et `GameChatUser::RestrictionMode`. L’exemple suivant montre une heuristique possible pour déterminer une valeur de constante icône particulière pour affecter à un `iconToShow` variable à partir d’un `GameChatUser` objet vers lequel pointe la variable « chatUser ».

```cpp
if (chatUser->RestrictionMode != None)
{
    if (!chatUser->IsMuted)
    {
        if (chatUser->TalkingMode != NotTalking)
        {
            iconToShow = Icon_ActiveSpeaker;
        }
        else
        {
            iconToShow = Icon_InactiveSpeaker;
        }
    }
    else
    {
        iconToShow = Icon_MutedSpeaker;
    }
}
else
{
    iconToShow = Icon_RestrictedSpeaker;
}
```

### <a name="game-chat-2"></a>Game Chat 2

Jeu Chat 2 a un fusionnés `game_chat_user_chat_indicator` utilisé pour représenter l’état en cours, instantanée de conversation pour un lecteur. Cette valeur est récupérée en appelant `chat_user::chat_indicator()`. L’exemple suivant illustre la récupération de la valeur d’indicateur pour un `chat_user` objet vers lequel pointe la variable « chatUser » pour déterminer une valeur de constante icône particulière pour attribuer à une variable 'iconToShow' :

```cpp
switch (chatUser->chat_indicator())
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

## <a name="muting"></a>Désactivation

## <a name="game-chat"></a>Conversation de jeu

Désactivation ou réactivation d’un utilisateur dans le jeu Chat est effectuée via un appel à `ChatManager::MuteUserFromAllChannels()` ou `ChatManager::UnMuteUIserFromAllChannels()`, respectivement. L’état muet d’un utilisateur peut être récupéré en examinant `GameChatUser::IsMuted` ou `GameChatUser::IsLocalUserMuted`.

## <a name="game-chat-2"></a>Game Chat 2

Le `chat_user::chat_user_local::set_microphone_muted()` méthode peut être utilisée pour activer/désactiver l’état muet du microphone d’un utilisateur local. Lorsque le microphone est coupé, sans contenu audio à partir de ce microphone est capturé. Si l’utilisateur se trouve sur un appareil partagé, telles que Kinect, l’état muet s’applique à tous les utilisateurs.

Le `chat_user::chat_user_local::microphone_muted()` méthode peut être utilisée pour récupérer l’état muet du microphone d’un utilisateur local. Cette méthode reflète uniquement si le microphone de l’utilisateur local a été muet dans les logiciels via un appel à `chat_user::chat_user_local::set_microphone_muted()`; cette méthode ne reflète pas un désactivation du son matériel contrôlé, par exemple, via un bouton sur casque de l’utilisateur. Il n’existe aucune méthode pour récupérer l’état muet du matériel de périphérique audio d’un utilisateur à travers le jeu Chat 2.

Le `chat_user::chat_user_local::set_remote_user_muted()` méthode peut être utilisée pour activer/désactiver l’état de désactivation d’un utilisateur distant par rapport à un utilisateur local particulier. Lorsque l’utilisateur distant est coupé, l’utilisateur local ne sont pas entendre des données audio ou recevoir des messages texte à partir de l’utilisateur distant.

## <a name="bad-reputation-auto-mute"></a>Mauvaise réputation automatique-muet

En règle générale, les utilisateurs distants commenceront unmuted. Chat de jeu et le jeu Chat 2 ont une fonctionnalité « mauvaise réputation bouton Muet automatique ». Cela signifie que les utilisateurs commenceront à l’état muet lorsque (1) l’utilisateur distant n’est pas amis avec un utilisateur local, et (2) l’utilisateur distant a un indicateur de mauvaise réputation. Jeu 2 Chat fournit des commentaires lorsqu’un utilisateur est désactivé en raison de cette fonctionnalité. Reportez-vous à [2 de conversation à l’aide de jeu - mauvaise réputation automatique-muet](using-game-chat-2.md#bad-reputation-auto-mute) pour plus d’informations.

## <a name="privilege-and-privacy"></a>Privilège et confidentialité

Chat de jeu et le jeu Chat 2 appliquent des restrictions de privilège et de confidentialité Xbox Live sur les relations de communication ou de canal gérées par l’application. En outre, les jeux 2 Chat fournit des informations de diagnostic pour déterminer exactement comment la restriction est ayant un impact sur la direction de l’audio (par exemple, si audio une restriction audio est unidirectionnelle ou bidirectionnelle).

### <a name="game-chat"></a>Conversation de jeu

Conversation jeu expose des informations de privilège et de confidentialité par le biais du `RestrictionMode` propriété. Elle peut être récupérée en inspectant `GameChatUser::RestrictionMode`.

### <a name="game-chat-2"></a>Game Chat 2

Jeu 2 Chat effectue des recherches de restriction de privilège et de confidentialité lors de l’ajout d’un utilisateur ; l’utilisateur `chat_user::chat_indicator()` retournera toujours `game_chat_user_chat_indicator::silent` jusqu'à ce que ces opérations terminées. Si la communication avec un utilisateur est affectée par un privilège ou confidentialité, l’utilisateur de restriction `chat_user::chat_indicator()` retournera `game_chat_user_chat_indicator::platform_restricted`. Restrictions de communication de plate-forme s’appliquent aux conversations vocales et de texte ; Il ne sera jamais y avoir une instance où conversations textuelles sont bloquée par une restriction de la plateforme, mais la conversation vocale n’est pas, ou vice versa.

`chat_user::chat_user_local::get_effective_communication_relationship()` peut être utilisé pour aider à distinguer lorsque les utilisateurs ne peuvent pas communiquer en raison de privilèges incomplet et les opérations de confidentialité. Elle retourne la relation de communication appliquée par jeu Chat 2 sous la forme de `game_chat_communication_relationship_flags` et la raison la relation ne peut pas être égale à la relation configurée sous la forme de `game_chat_communication_relationship_adjuster`. Par exemple, si les opérations de recherche sont toujours en cours, le `game_chat_communication_relationship_adjuster` sera `game_chat_communication_relationship_adjuster::intializing`. Cette méthode est censée être utilisée dans les scénarios de développement et débogage ; Il ne doit pas être utilisé pour influencer l’interface utilisateur (consultez [l’interface utilisateur](#UI)).

## <a name="cleanup"></a>Nettoyage

Du d’origine jeu Chat `ChatManager` est une classe runtime WinRT - interface de comptabilisées une référence. Par conséquent, ses ressources de mémoire sont récupérés lorsque le dernier nombre de références atteint zéro et il nettoie. Interaction avec l’API C++ Game Chat 2 s’effectue via une instance de singleton ; Lorsque l’application n’a plus besoin des communications via jeu Chat 2, vous devez appeler `chat_manager::cleanup()`. Cela permet de jeu Chat 2 libérer immédiatement toutes les ressources qui ont été alloués pour gérer les communications. Pour plus d’informations sur les API WinRT et la gestion des ressources de jeu Chat 2, consultez [à l’aide de jeu Chat 2 WinRT Projections](using-game-chat-2-winrt.md#cleanup).

## <a name="failure-model-and-debugging"></a>Modèle de défaillance et de débogage

Le Chat du jeu d’origine est une API WinRT ; Par conséquent, il a signalé des erreurs via des exceptions. Non irrécupérable erreurs ou avertissements sont signalés via un rappel de débogage facultatifs. Jeu Chat du C++ API 2 ne lèvent des exceptions comme un moyen d’erreur non irrécupérable reporting donc vous pouvez l’utiliser facilement des projets sans exception si vous préférez. Jeu 2 Chat est le cas, toutefois, lèvent des exceptions pour informer les erreurs irrécupérables. Ces erreurs sont le résultat d’une mauvaise utilisation des API, telles que l’ajout d’un utilisateur à l’instance de conversation de jeu avant l’initialisation de l’instance ou de l’accès à un objet utilisateur après que qu’il a été supprimé de l’instance de jeu Chat. Ces erreurs sont supposés être interceptées tôt dans le développement et peuvent être corrigées en modifiant le modèle utilisé pour interagir avec le jeu Chat 2. Lorsqu’une telle erreur se produit, un Conseil au concernant ce qui a provoqué l’erreur est écrit dans le débogueur avant de l’exception est levée. API WinRT de jeu Chat du 2 suit le modèle de WinRT de signalement des erreurs via des exceptions.

## <a name="how-to-configure-popular-scenarios"></a>Comment configurer des scénarios courants

Reportez-vous à [à l’aide de jeu Chat 2 - comment configurer des scénarios courants](using-game-chat-2.md#how-to-configure-popular-scenarios) pour obtenir des exemples sur la façon de configurer des scénarios courants tels que les push pour parler, les équipes et les scénarios de communication de type de diffusion.

## <a name="pre-encode-and-post-decode-audio-manipulation"></a>Encoder au préalable et postérieures décoder manipulation audio

Le Chat du jeu d’origine autorisée pour la manipulation de l’audio en autorisant l’accès à l’audio du microphone brutes via le `OnPreEncodeAudioBuffer` et `OnPostDecodeAudioBuffers` événements. Jeu 2 Chat expose cette fonctionnalité via un modèle d’interrogation. Pour plus d’informations, consultez [manipulation audio en temps réel](real-time-audio-manipulation.md).
