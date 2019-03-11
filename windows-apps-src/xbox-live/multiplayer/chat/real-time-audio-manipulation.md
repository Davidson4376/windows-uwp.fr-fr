---
title: Manipulation audio en temps réel
description: Découvrez comment manipuler et de traiter l’audio de conversation capturée par jeu Chat 2.
ms.date: 05/10/2018
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, chat jeu 2, jeu chat, communication vocale, manipulation de mémoire tampon, manipulation audio
ms.localizationpriority: medium
ms.openlocfilehash: 7746080ea8a9698993a679b425f41442e4a6d943
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643894"
---
# <a name="real-time-audio-manipulation"></a>Manipulation audio en temps réel

Jeu Chat 2 permet aux développeurs s’insérer dans le pipeline de conversation audio pour inspecter et manipuler des données audio de conversation les joueurs. Cela peut être utile pour appliquer intéressant les effets audio de voix de joueurs dans le jeu. Il s’agit d’un pipeline de manipulation audio jeu Chat du 2 interagir avec via des objets de flux audio qui peuvent être interrogés pour les données audio. Par opposition à l’aide de rappels, ce modèle permet aux développeurs d’inspecter ou de manipuler audio sur le thread de traitement est la plus commode pour eux.

Une brève procédure pas à pas des manipulations audio en temps réel est décrit ci-dessous, qui contient les rubriques suivantes :

1. [L’initialisation du pipeline de manipulation audio](#initializing-the-audio-manipulation-pipeline)
2. [Traitement des modifications d’état de flux audio](#processing-audio-stream-state-changes)
3. [Manipulation encoder au préalable audio de conversation](#manipulating-pre-encode-chat-audio-data)
4. [Manipulation de décoder postérieures audio de conversation](#manipulating-post-decode-chat-audio-data)
5. [Durées de vie utilisateur Chat](#chat-user-lifetimes)

## <a name="initializing-the-audio-manipulation-pipeline"></a>L’initialisation du pipeline de manipulation audio

Jeu 2 Chat par défaut n’active pas la manipulation audio en temps réel. Pour activer la manipulation audio en temps réel, l’application doit spécifier les formes de manipulation audio il aimerait qu’activée dans `chat_manager::initialize()` en définissant le paramètre audioManipulationMode.

Actuellement, les formes de manipulation audio suivantes sont prises en charge :

* `game_chat_audio_manipulation_mode_flags::none` -Désactive la manipulation de l’audio. Il s'agit de la configuration par défaut. Dans ce mode, l’audio de conversation circule sans interruption.
* `game_chat_audio_manipulation_mode_flags::pre_encode_stream_manipulation` -Permet d’encode au préalable manipulation audio. Dans ce mode, tous les fichiers audio de conversation générées par les utilisateurs locaux seront alimentées à travers le pipeline de manipulation audio avant d’être codée. Même si l’application est uniquement les données audio de conversation et manipulez ne pas, il incombe toujours de l’application pour envoyer les tampons audio non modifiés au jeu Chat 2 afin qu’ils peuvent être encodés et transmis.
* `game_chat_audio_manipulation_mode_flags::post_decode_stream_manipulation` -Permet de décoder postérieures manipulation audio. Dans ce mode, tous les fichiers audio de conversation reçus des utilisateurs à distance seront alimentées à travers le pipeline de manipulation audio une fois qu’il est décodé par le récepteur, mais avant son rendu. Même si l’application est uniquement les données audio de conversation et manipulez ne pas, il incombe toujours de l’application pour combiner et de soumettre les tampons audio non modifiés au jeu Chat 2 afin qu’elles puissent être rendues.

## <a name="processing-audio-stream-state-changes"></a>Traitement des modifications d’état de flux audio

Jeu 2 Chat offre des mises à jour à l’état de flux audio via `game_chat_stream_state_change` structures. Ces mises à jour stockent des informations sur les flux de données a été mis à jour et comment il a été mis à jour. Ces mises à jour peuvent être interrogés par des appels à la `chat_manager::start_processing_stream_state_changes()` et `chat_manager::finish_processing_stream_state_changes()` paire de méthodes. Cette paire de méthodes fournit toutes du flux audio plus récente, en file d’attente de mises à jour de l’état sous forme de tableau de `game_chat_stream_state_change` structure des pointeurs. Les applications doivent effectuer une itération sur le tableau et gérer chaque mise à jour de façon appropriée. Une fois tous disponibles `game_chat_stream_state_change` mises à jour ont été traités, ce tableau doit être passé au jeu Chat 2 à `chat_manager::finish_processing_stream_state_changes()`. Exemple :

```cpp
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);

for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        case game_chat_stream_state_change_type::pre_encode_audio_stream_created:
        {
            HandlePreEncodeAudioStreamCreated(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        case Xs::game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_closed:
        {
            HandlePreEncodeAudioStreamClosed(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);
```

## <a name="manipulating-pre-encode-chat-audio-data"></a>Manipulation encoder au préalable les données audio de conversation

Jeu Chat 2 fournit un accès pour encoder au préalable les données audio de conversation pour les utilisateurs locaux via la `pre_encode_audio_stream` classe.

### <a name="stream-lifetime"></a>Durée de vie de Stream
Lorsqu’un nouveau `pre_encode_audio_stream` instance est prête pour l’application à utiliser, il est remis via un `game_chat_stream_state_change` structure avec ses `state_change_type` champ la valeur `game_chat_stream_state_change_type::pre_encode_audio_stream_created`. Une fois que ce changement d’état de flux de données est renvoyé au jeu Chat 2, le flux audio sont disponible pour la manipulation audio encoder au préalable.

Quand un existant `pre_encode_audio_stream` devient indisponible à utiliser pour la manipulation de l’audio, l’application sera notifiée via un `game_chat_stream_state_change` structure avec ses `state_change_type` champ la valeur `game_chat_stream_state_change_type::pre_encode_audio_stream_closed`. Il s’agit d’opportunité de l’application pour démarrer le nettoyage des ressources associées à ce flux audio. Une fois que ce changement d’état de flux de données est renvoyé au jeu Chat 2, le flux audio deviennent indisponible pour encoder au préalable manipulation audio.

Quand un fermé `pre_encode_audio_stream` a toutes ses ressources retourné, le flux sera détruit et l’application est informée par une `game_chat_stream_state_change` structure avec ses `state_change_type` champ la valeur `game_chat_stream_state_change_type::pre_encode_audio_stream_destroyed`. Des références ou des pointeurs vers ce flux de données doivent être nettoyées. Une fois que ce changement d’état de flux de données est renvoyé au jeu Chat 2, la mémoire de flux audio deviendront non valide.

### <a name="stream-users"></a>Utilisateurs de Stream
La liste des utilisateurs associés à un flux de données peut être inspectée à l’aide de `pre_encode_audio_stream::get_users()`.

### <a name="audio-formats"></a>Formats audio
Le format audio des mémoires tampons de l’application récupère à partir de jeu Chat 2 peut être inspecté à l’aide de `pre_encode_audio_stream::get_pre_processed_format()`. Le format audio prétraitée sera toujours mono. L’application doit attendre à gérer les données représentées sous forme de valeurs en virgule flottante 32 bits, des entiers 16 bits et des entiers 32 bits.

L’application doit informer le jeu Chat 2 du format audio des mémoires tampons manipulés qui sont soumises à ce dernier pour l’encodage et la transmission à l’aide de `pre_encode_audio_stream::set_processed_format()`. Formats traités pour encodent au préalable audio flux doivent respecter ces conditions préalables :

* Le format doit être mono.
* Le format doit être float 32 bits PCM, l’entier 32 bits PCM ou formats PCM entier 16 bits.
* Taux d’échantillonnage du format doit respecter les conditions préalables en fonction de sa plateforme. Xbox One ère prend en charge le taux d’échantillonnage 8kHz, 12kHz, 16kHz et 24kHz. UWP pour Xbox One et PC prend en charge 8kHz, 12kHz, kHz 16, 24kHz, 32kHz, 44,1 kHz et taux d’échantillonnage de 48 kHz.

### <a name="retrieving-and-submitting-audio"></a>Extraire et envoyer des données Audio
Applications peuvent interroger encoder au préalable les flux audio pour le nombre de tampons disponibles pour traiter à l’aide de `pre_encode_audio_stream::get_available_buffer_count()`. Ces informations peuvent être utilisées si l’application souhaite retarder le traitement audio jusqu'à ce qu’un nombre minimal de mémoires tampons est disponible. Mémoires tampons uniquement 10 seront en file d’attente sur chaque encoder au préalable les flux audio et des retards audio introduira une latence dans le pipeline audio, il est donc recommandé que les applications drainer leurs encoder au préalable les flux audio avant leur file d’attente de mémoires tampons plus de 4.

Applications peuvent récupérer des tampons audio à partir d’un encoder au préalable à l’aide du flux audio `pre_encode_audio_stream::get_next_buffer()`. Nouvelles mémoires tampons audio sera disponibles en moyenne, une seule fois chaque 40 MS. Mémoires tampons retournés par cette méthode doivent être libérées à `pre_encode_audio_stream::return_buffer()` lorsqu’elles sont effectuées utilisé. En file d’attente d’un maximum de 10 ou mémoires tampons ont pas été restitués peuvent exister à un moment donné pour un flux audio encoder au préalable. Une fois cette limite est atteinte, nouvelles mémoires tampon capturées à partir de la source audio du lecteur est supprimés jusqu'à ce que certaines des mémoires tampons en attente sont retournés.

Applications peuvent envoyer leurs inspectés et manipulés les mémoires tampons au jeu Chat 2 pour l’encodage et à l’aide de la transmission `pre_encode_audio_stream::submit_buffer()`. Jeu 2 Chat prend en charge manipulation audio sur place et hors de place, les mémoires tampons soumis à `pre_encode_audio_stream::submit_buffer()` n’ont pas nécessairement être récupérées à partir de mémoires tampons mêmes `pre_encode_audio_stream::get_next_buffer()`. Confidentialité/privilège pour ces mémoires tampons soumis sera appliquée selon les utilisateurs associés à ce flux. Chaque 40 MS, les 40 MS suivant des données audio à partir de ce flux seront encodés et transmis. Pour éviter les interruptions audio, les mémoires tampons pour l’audio entendre en continu doivent être soumises dans ce flux à une vitesse constante.

### <a name="stream-contexts"></a>Contextes de Stream
Applications peuvent gérer les valeurs de contexte de taille d’un pointeur personnalisé d’encoder au préalable les flux audio à l’aide de `pre_encode_audio_stream::set_custom_stream_context()` et `pre_encode_audio_stream::custom_stream_context()`. Les contextes de flux personnalisés sont utiles pour créer des mappages entre les du jeux Chat 2 flux audio et les données auxiliaire : diffuser les métadonnées, état du jeu, etc.

### <a name="example"></a>Exemple
Voici un exemple de bout en bout simplifié pour savoir comment utiliser encoder au préalable les flux audio dans le cadre d’un traitement audio :

```cpp
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);

for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        case game_chat_stream_state_change_type::pre_encode_audio_stream_created:
        {
            pre_encode_audio_stream* stream = streamStateChanges[streamStateChangeIndex]->pre_encode_audio_stream;
            stream->set_processed_audio_format(...);
            stream->set_custom_stream_context(...);
            HandlePreEncodeAudioStreamCreated(stream);
            break;
        }

        case game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_closed:
        {
            HandlePreEncodeAudioStreamClosed(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        case game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_destroyed:
        {
            HandlePreEncodeAudioStreamDestroyed(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);

uint32_t preEncodeAudioStreamCount;
pre_encode_audio_stream_array preEncodeAudioStreams;
chat_manager::singleton_instance().get_pre_encode_audio_streams(&preEncodeAudioStreamCount, &preEncodeAudioStreams);
for (uint32_t preEncodeAudioStreamIndex = 0; preEncodeAudioStreamIndex < preEncodeAudioStreamCount; ++preEncodeAudioStreamIndex)
{
    pre_encode_audio_stream* stream = preEncodeAudioStreams[preEncodeAudioStreamIndex];
    StreamContext* context = reinterpret_cast<StreamContext*>(stream->custom_stream_context());

    game_chat_audio_format audio_format = stream->get_pre_processed_format();

    uint32_t preProcessedBufferByteCount;
    void* preProcessedBuffer;
    stream->get_next_buffer(&preProcessedBufferByteCount, &preProcessedBuffer);

    while (preProcessedBuffer != nullptr)
    {
        void* processedBuffer = nullptr;
        switch (audio_format.bits_per_sample)
        {
            case 16:
            {
                assert (audio_format.sample_type == game_chat_sample_type::integer);
                processedBuffer = ManipulateChatBuffer<int16_t>(preProcessedBufferByteCount, preProcessedBuffer, context);
                break;
            }

            case 32:
            {
                switch (audio_format.sample_type)
                {
                    case game_chat_sample_type::integer:
                    {
                        processedBuffer = ManipulateChatBuffer<int32_t>(preProcessedBufferByteCount, preProcessedBuffer, context);
                        break;
                    }

                    case game_chat_sample_type::ieee_float:
                    {
                        processedBuffer = ManipulateChatBuffer<float>(preProcessedBufferByteCount, preProcessedBuffer, context);
                        break;
                    }

                    default:
                    {
                        assert(false);
                        break;
                    }
                }
                break;
            }

            default:
            {
                assert(false);
                break;
            }
        }
        // processedBuffer can be the same as preProcessedBuffer (in-place manipulation) or it can be a buffer of
        // memory not managed by Game Chat 2 (out-of-place manipulation).
        stream->submit_buffer(processedBuffer);
        // Only return buffers retrieved from Game Chat 2. Do not return foreign memory to return_buffer.
        stream->return_buffer(preProcessedBuffer);
        stream->get_next_buffer(&preProcessedBufferByteCount, &preProcessedBuffer);
    }
}

Sleep(audioProcessingPeriodInMilliseconds);
```

## <a name="manipulating-post-decode-chat-audio-data"></a>Manipulation après décoder les données audio de conversation

Jeu Chat 2 fournit un accès pour décoder après les données audio de conversation via le `post_decode_audio_source_stream` et `post_decode_audio_sink_stream` des classes, afin que les utilisateurs peuvent manipuler audio d’utilisateurs distants identifie de façon unique pour chaque récepteur local de l’audio de la conversation.

### <a name="sources-and-sinks"></a>Sources et récepteurs
Contrairement au pipeline pre-encode, le modèle pour traiter les décoder après des données audio est réparti sur deux classes : `post_decode_audio_source_stream` et `post_decode_audio_sink_stream`. Audio décodé d’utilisateurs distants peut être récupérée à partir de `post_decode_audio_source_stream` objets, manipulé et envoyées à `post_decode_audio_sink_stream` objets pour le rendu. Ainsi, pour l’intégration entre jeu Chat 2 décoder postérieurs au pipeline de traitement audio et des intergiciels (middleware) audio utile.

### <a name="stream-lifetime"></a>Durée de vie de Stream
Lorsqu’un nouveau `post_decode_audio_source_stream` ou `post_decode_audio_sink_stream` instance est prête pour l’application à utiliser, il est remis via un `game_chat_stream_state_change` structure avec ses `state_change_type` champ la valeur `game_chat_stream_state_change_type::post_decode_audio_source_stream_created` ou `game_chat_stream_state_change_type::post_decode_audio_sink_stream_created`, respectivement. Une fois que ce changement d’état de flux de données est renvoyé au jeu Chat 2, le flux audio sont disponible pour décoder postérieures manipulation audio.

Quand un existant `post_decode_audio_source_stream` ou `post_decode_audio_sink_stream` devient indisponible à utiliser pour la manipulation de l’audio, l’application sera notifiée via un `game_chat_stream_state_change` structure avec ses `state_change_type` champ la valeur `game_chat_stream_state_change_type::post_decode_audio_source_stream_closed` ou `game_chat_stream_state_change_type::post_decode_audio_sink_stream`, respectivement. Il s’agit d’opportunité de l’application pour démarrer le nettoyage des ressources associées à ce flux audio. Une fois que ce changement d’état de flux de données est renvoyé au jeu Chat 2, le flux audio n’est plus disponible pour décoder postérieures manipulation audio. Pour les flux de la source, cela signifie qu’aucune mémoire tampon plus n’être mise en attente pour la manipulation. Pour les flux de récepteur, cela signifie qui soumis mémoires tampons n’est plus seront restitués.

Quand un fermé `post_decode_audio_source_stream` ou `post_decode_audio_sink_stream` a toutes ses ressources retourné, le flux sera détruit et l’application est informée par une `game_chat_stream_state_change` structure avec ses `state_change_type` champ la valeur `game_chat_stream_state_change_type::post_decode_audio_source_stream_destroyed` ou `game_chat_stream_state_change_type::post_decode_audio_sink_stream_destroyed`, respectivement. Des références ou des pointeurs vers ce flux de données doivent être nettoyées. Une fois que ce changement d’état de flux de données est renvoyé au jeu Chat 2, la mémoire de flux audio deviendront non valide.

### <a name="stream-users"></a>Utilisateurs de Stream
La liste des utilisateurs à distance associé à un flux de données source post-decode peut être inspectée à l’aide de `post_decode_audio_source_stream::get_users()`. La liste d’utilisateurs locales associé à un flux de données du récepteur post-decode peut être inspectée à l’aide de `post_decode_audio_sink_stream::get_users()`.

### <a name="audio-formats"></a>Formats audio
Le format audio des mémoires tampons de l’application récupère à partir de jeu Chat 2 peut être inspecté à l’aide de `post_decode_audio_source_stream::get_pre_processed_format()`. Le format audio prétraitée sera toujours mono 16 bits entier PCM.

L’application doit informer le jeu Chat 2 du format audio des mémoires tampons manipulés qui sont soumises à ce dernier pour le rendu à l’aide de `post_decode_audio_sink_stream::set_processed_format()`. Formats traités pour décoder postérieures audio récepteur de flux de données doit respecter ces conditions préalables :

* Le format doit avoir moins de 64 canaux.
* Le format doit être entier 16 bits PCM (optimale), 20 bits entier PCM (dans un conteneur de 24 bits), 24 bits entier PCM, l’entier 32 bits PCM ou float 32 bits PCM (format préféré après l’entier 16 bits PCM). 
* Taux d’échantillonnage du format doit comprendre entre 1 000 et 200000 échantillons par seconde.

### <a name="retrieving-and-submitting-audio"></a>Extraire et envoyer des données Audio
Applications peuvent interroger postérieures décoder flux source audio pour le nombre de tampons disponibles pour traiter à l’aide de `post_decode_audio_source_stream::get_available_buffer_count()`. Ces informations peuvent être utilisées si l’application souhaite retarder le traitement audio jusqu'à ce qu’un nombre minimal de mémoires tampons est disponible. Mémoires tampons uniquement 10 seront en file d’attente sur chaque postérieures décoder le flux source audio et des retards audio introduira une latence dans le pipeline audio, il est donc recommandé que les applications drainer leurs postérieures décoder les flux audio avant leur file d’attente de mémoires tampons plus de 4.

Applications peuvent récupérer des tampons audio à partir d’un post-décoder le flux source audio à l’aide `post_decode_audio_source_stream::get_next_buffer()`. Nouvelles mémoires tampons audio sera disponibles en moyenne, une seule fois chaque 40 MS. Mémoires tampons retournés par cette méthode doivent être libérées à `post_decode_audio_source_stream::return_buffer()` lorsqu’elles sont effectuées utilisé. En file d’attente d’un maximum de 10 ou mémoires tampons ont pas été restitués peuvent exister à un moment donné pour un post-décoder le flux source audio. Une fois cette limite est atteinte, nouvelle tampons décodés à partir du lecteur distant sont supprimés jusqu'à ce que certaines des mémoires tampons en attente sont retournés.

Les applications peuvent envoyer leurs inspectés et manipulés les mémoires tampons au jeu Chat 2 à décodent postérieurs au flux audio récepteur pour le rendu à l’aide `post_decode_audio_sink_stream::submit_mixed_buffer()`. Jeu 2 Chat prend en charge manipulation audio sur place et hors de place, les mémoires tampons soumis à `post_decode_audio_sink_stream::submit_mixed_buffer()` n’ont pas nécessairement être récupérées à partir de mémoires tampons mêmes `post_decode_audio_source_stream::get_next_buffer()`. Chaque 40 MS, les 40 MS suivant des données audio à partir de ce flux s’affichera. Pour éviter les interruptions audio, les mémoires tampons pour l’audio entendre en continu doivent être soumises dans ce flux à une vitesse constante.

### <a name="privacy-and-mixing"></a>Confidentialité et mélange
En raison du modèle de récepteur de la source du pipeline post-decode, il incombe de l’application de mélanger les mémoires tampons extraites `post_decode_audio_source_stream` objets et soumettre les mémoires tampons mixtes à `post_decode_audio_sink_stream` objets pour le rendu. Cela signifie également qu’il est responsable de l’application pour effectuer la combinaison avec confidentialité appropriée et de privilèges appliquées. Fournit des jeux 2 Chat `post_decode_audio_sink_stream::can_receive_audio_from_source_stream()` pour simplifier l’interrogation de ces informations simples et efficaces.

### <a name="chat-indicators"></a>Indicateurs de conversation

Décoder postérieures audio manipulation pas aura un effet sur l’état d’indicateur de conversation pour chaque utilisateur. Par exemple, lorsqu’un utilisateur distant est désactivé, l’audio doivent être fournie à l’application, mais l’indicateur de conversation pour cet utilisateur distant indique toujours en sourdine. Lorsqu’un utilisateur distant parle, leur audio doivent être fournie, mais l’indicateur de conversation indique communique avec quel que soit l’indique si l’application fournit un mixage audio contenant des données audio à partir de cet utilisateur. Pour plus d’informations sur l’interface utilisateur et l’indicateur de conversation, consultez [2 de conversation de jeu à l’aide de](using-game-chat-2.md#ui). Si les restrictions supplémentaires propres à l’application sont utilisées pour déterminer quels utilisateurs sont présents dans un mixage audio, il incombe de l’application à prendre en compte ces mêmes restrictions lors de la lecture les indicateurs de conversation fournies par le jeu Chat 2.

### <a name="stream-contexts"></a>Contextes de Stream
Applications peuvent gérer les valeurs de contexte de taille d’un pointeur personnalisé sur postérieures de décoder les flux audio à l’aide de la `set_custom_stream_context()` et `custom_stream_context()` méthodes. Les contextes de flux personnalisés sont utiles pour créer des mappages entre les du jeux Chat 2 flux audio et les données auxiliaire : diffuser les métadonnées, état du jeu, etc.

### <a name="example"></a>Exemple
Voici un exemple de bout en bout simplifié pour savoir comment utiliser postérieures décoder les flux audio dans le cadre d’un traitement audio :

```cpp
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);

for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        case game_chat_stream_state_change_type::post_decode_audio_source_stream_created:
        {
            post_decode_audio_source_stream* stream = streamStateChanges[streamStateChangeIndex]->post_decode_audio_source_stream;
            stream->set_custom_stream_context(...);
            HandlePostDecodeAudioSourceStreamCreated(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_source_stream_closed:
        {
            HandlePostDecodeAudioSourceStreamClosed(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_source_stream_destroyed:
        {
            HandlePostDecodeAudioSourceStreamDestroyed(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_sink_stream_created:
        {
            post_decode_audio_sink_stream* stream = streamStateChanges[streamStateChangeIndex]->post_decode_audio_sink_stream;
            stream->set_custom_stream_context(...);
            stream->set_processed_format(...);
            HandlePostDecodeAudioSinkStreamCreated(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_sink_stream_closed:
        {
            HandlePostDecodeAudioSinkStreamClosed(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_sink_stream_destroyed:
        {
            HandlePostDecodeAudioSinkStreamDestroyed(stream);
            break;
        }

        ...
    }
}

chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);

uint32_t sourceStreamCount;
post_decode_audio_source_stream_array sourceStreams;
chatManager::singleton_instance().get_post_decode_audio_source_streams(&sourceStreamCount, &sourceStreams);

uint32_t sinkStreamCount;
post_decode_audio_sink_stream_array sinkStreams;
chatManager::singleton_instance().get_post_decode_audio_sink_streams(&sinkStreamCount, &sinkStreams);

//
// MixBuffer is a custom type defined as:
// struct MixBuffer
// {
//     uint32_t bufferByteCount;
//     void* buffer;
// };
//
std::vector<std::pair<post_decode_audio_source_stream*, MixBuffer>> cachedSourceBuffers;

for (uint32_t sourceStreamIndex = 0; sourceStreamIndex < sourceStreamCount; ++sourceStreamIndex)
{
    post_decode_audio_source_stream* sourceStream = sourceStreams[sourceStreamIndex];

    MixBuffer mixBuffer;
    sourceStream->get_next_buffer(&mixBuffer.bufferByteCount, &mixBuffer.buffer);
    if (buffer != nullptr)
    {
        // Stash the buffer to return after we are done with mixing. If this program was using audio middleware, now
        // would be an appropriate time to plumb the buffer through the middleware
        cachedSourceBuffer.push_back(std::pair<post_decode_audio_source_stream*, MixBuffer>{sourceStream, mixBuffer});
    }
}

// Loop over each sink stream, perform mixing, and submit
for (uint32_t sinkStreamIndex = 0; sinkStreamIndex < sinkStreamCount; ++sinkStreamIndex)
{
    post_decode_audio_sink_stream* sinkStream = sinkStreams[sinkStreamIndex];

    if (sinkStream->is_open())
    {
        std::vector<std::pair<MixBuffer, float>> buffersToMixForThisStream;

        for (const std::pair<post_decode_audio_source_stream, MixBuffer>& sourceBufferPair : cachedSourceBuffers)
        {
            float volume;
            if (sinkStream->can_receive_audio_from_source_stream(sourceBufferPair.first, &volume))
            {
                buffersToMixForThisStream.push_back(std::pair<MixBuffer, float>{sourceBufferPair.second, volume});
            }
        }

        if (buffersToMixForThisStream.size() > 0)
        {
            uint32_t mixedBufferByteCount;
            uint8_t* mixedBuffer;
            MixPostDecodeBuffers(buffersToMixForThisStream, &mixedBufferByteCount, &mixedBuffer);
            sinkStream->submit_mixed_buffer(mixedBufferByteCount, mixedBuffer);
        }
    }
}

// Return buffers after mix and submission
for (const std::pair<post_decode_audio_source_stream*, MixBuffer>& cachedSourceBuffer : cachedSourceBuffers)
{
    post_decode_audio_source_stream* sourceStream = cachedSourceBuffer.first;
    void* bufferToReturn = cachedSourceBuffer.second.buffer;
    sourceStream->return_buffer(bufferToReturn);
}

Sleep(audioProcessingPeriodInMilliseconds);
```

## <a name="chat-user-lifetimes"></a>Durées de vie utilisateur Chat

L’activation de manipulation audio en temps réel affecte les durées de vie des utilisateurs de la conversation. Si `chat_manager::remove_user(chatUserX)` est appelée, le chat_user objet vers lequel pointé chatUserX restera valide jusqu'à ce que tous les flux audio qui font référence à chatUserX ont été détruits. Considérez le scénario suivant :

```cpp
// At somepoint a chat user, chatUserX, leaves the game session.
chat_manager::singleton_instance().remove_user(chatUserX);

// chatUserX is still valid, but to avoid further synchronization, prevent non-audio-stream use of chatUserX
chatUserX = nullptr;

// On the audio processing thread...
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);
for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        ...

        // All of the streams associated with chatUserX will close.
        case Xs::game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_closed:
        {
            CleanupPreEncodeAudioStreamResources(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);

// The next time the app processes stream state changes...
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);
for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        ...

        case Xs::game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_destroyed:
        {
            uint32_t chatUserCount;
            Xs::game_chat_2::chat_user_array chatUsers;
            streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream->get_users(&chatUserCount, &chatUsers);
            assert(chatUserCount != 0);
            for (uint32_t chatUserIndex = 0; chatUserIndex < chatUserCount; ++chatUserIndex)
            {
                // chat_user objects such as chatUserX will still be valid while the destroyed state change is being processed.
                Log(chatUsers[chatUserIndex]->xbox_user_id());
            }
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);
// Once the all destroyed state changes have been processed for all streams associated with chatUserX, it's memory will be invalidated.
// Do not call methods on chatUserX, e.g. chatUserX->xbox_user_id()
```
