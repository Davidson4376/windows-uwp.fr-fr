---
author: joannaleecy
title: Ajouter du son
description: Développer un moteur audio simple à l’aide des API XAudio2 pour la musique du jeu la lecture et des effets sonores.
ms.assetid: aa05efe2-2baa-8b9f-7418-23f5b6cd2266
ms.author: joanlee
ms.date: 10/24/2017
ms.topic: article
keywords: windows10, uwp, jeux, son
ms.localizationpriority: medium
ms.openlocfilehash: 3d1c95fe883cf2517855a3b6f1c4dfc6c9b6dd9a
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6267928"
---
# <a name="add-sound"></a>Ajouter du son

Dans cette rubrique, nous créons un moteur audio simple à l’aide de [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813) API. Si vous êtes nouveau dans __XAudio2__, nous avons inclus une présentation brève sous [les concepts de l’Audio](#audio-concepts).

>[!Note]
>Si vous n’avez pas encore téléchargé le dernier code de jeu pour cet exemple, accédez à [Exemple de jeu Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Cet exemple fait partie d’une vaste collection d’exemples de fonctionnalités UWP. Pour obtenir des instructions sur le téléchargement de l’exemple, consultez [Obtenir des exemples de la plateforme Windows universelle (UWP) à partir de GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objectif

Ajouter des sons dans l’exemple de jeu à l’aide de [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813).

## <a name="define-the-audio-engine"></a>Définir le moteur audio

Dans l’exemple de jeu, les comportements et objets audio sont définis dans trois fichiers:

* __[Audio.h](#audioh)/.cpp__: définit l’objet de __données Audio__ , qui contient les ressources __XAudio2__ pour la lecture audio. Il définit également une méthode pour interrompre et reprendre la lecture audio si le jeu est en pause ou désactivé.
* __ [MediaReader.h](#mediareaderh)/.cpp__: définit les méthodes permettant de lire des fichiers .wav audio à partir du stockage local.
* __ [SoundEffect.h](#soundeffecth)/.cpp__: définit un objet pour la lecture audio dans le jeu.

## <a name="overview"></a>Présentation

Il existe trois parties principales dans l’obtention de configurer pour la lecture audio dans votre jeu.

1. [Créez et initialisez les ressources audio](#create-and-initialize-the-audio-resources)
2. [Charger le fichier audio](#load-audio-file)
3. [Associer son objet](#associate-sound-to-object)

Elles sont toutes définies dans la méthode [Simple3DGame::Initialize](#simple3dgameinitialize-method) . Par conséquent, nous allons tout d’abord examiner cette méthode et puis pencher sur plus de détails dans chacune des sections.

Après avoir configuré, nous Découvrez comment déclencher les effets sonores à lire. Pour plus d’informations, accédez à [lire le son](#play-the-sound).

### <a name="simple3dgameinitialize-method"></a>Méthode de Simple3DGame::Initialize

Dans __Simple3DGame::Initialize__, où __m\_controller__ et __m\_renderer__ sont également initialisées, nous configurer le moteur audio et préparer la lecture de sons.

 * Créez __m\_audioController__, qui est une instance de la classe [Audio](#audioh) .
 * Créer les ressources audio nécessaires à l’aide de la méthode [Audio::CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method) . Cet exemple, deux objets __XAudio2__ &mdash; un objet de moteur de musique et un objet de moteur audio et une voix de contrôle pour chacun d’eux ont été créés. L’objet de moteur de musique peut être utilisé pour lire du contenu de la musique de fond pour votre jeu. Le moteur audio peut être utilisé pour lire des effets sonores dans votre jeu. Pour plus d’informations, voir [créer et initialiser les ressources audio](#create-and-initialize-the-audio-resources).
 * Créez __mediaReader__, qui est une instance de classe de [MediaReader](#mediareaderh) . [MediaReader](#mediareaderh), qui est une classe d’assistance pour la classe [SoundEffect](#soundeffecth) , lit les petits fichiers audio de manière synchrone à partir de l’emplacement du fichier et renvoie les données audio comme un tableau d’octets.
 * Utilisez [MediaReader::LoadMedia](#mediareaderloadmedia-method) pour charger des fichiers audio à partir de son emplacement et créez une variable __targetHitSound__ pour contenir les données audio .wav chargé. Pour plus d’informations, voir [charger le fichier audio](#load-audio). 

Les effets sonores sont associés à l’objet jeu. Par conséquent, lorsqu’une collision se produit avec cet objet de jeu, il déclenche l’effet sonore à lire. Dans cet exemple de jeu, nous avons des effets sonores pour les munitions (ce qui nous permet de tirer sur les cibles avec) et pour la cible. 
    
* Dans la classe __GameObject__ , il existe une propriété __HitSound__ qui est utilisée pour associer l’effet sonore à l’objet.
* Créer une nouvelle instance de la classe [SoundEffect](#soundeffecth) et initialiser. Pendant l’initialisation, une voix source pour l’effet sonore est créée. 
* Cette classe émet un son à l’aide d’une voix de contrôle fournie à partir de la classe [Audio](#audioh) . Données audio sont lue à partir de l’emplacement du fichier à l’aide de la classe [MediaReader](#mediareaderh) . Pour plus d’informations, voir [associer son à l’objet](#associate-sound-to-object).

>[!Note]
>Le déclencheur réel pour lire le son est déterminé par le mouvement et la collision de ces objets du jeu. Par conséquent, l’appel à ces sons réellement sont définies dans la méthode [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) . Pour plus d’informations, accédez à [lire le son](#play-the-sound).

```cpp
void Simple3DGame::Initialize(
    _In_ MoveLookController^ controller,
    _In_ GameRenderer^ renderer
    )
{
    // ...
    // Create a new Audio class object.
    m_audioController = ref new Audio;

    // Create the audio resources needed.
    // Two XAudio2 objects are created - one for music engine,
    // the other for sound engine. A mastering voice is also
    // created for each of the objects.
    m_audioController->CreateDeviceIndependentResources();

    m_ammo = std::vector<Sphere^>(GameConstants::MaxAmmo);

    // ..
    // Create a media reader which is used to read audio files from its file location.
    MediaReader^ mediaReader = ref new MediaReader;
    auto targetHitSound = mediaReader->LoadMedia("Assets\\hit.wav");

    // Instantiate the targets for use in the game.
    // Each target has a different initial position, size, and orientation.
    // But share a common set of material properties.
    for (int a = 1; a < GameConstants::MaxTargets; a++)
    {
        // ...
        // Create a new sound effect object and associate it
        // with the game object's (target) HitSound property.
        target->HitSound(ref new SoundEffect());

        // Initialize the sound effect object with
        // the sound effect engine, format of the audio wave, and audio data
        // During initialization, source voice of this sound effect is also created.
        target->HitSound()->Initialize(
            m_audioController->SoundEffectEngine(),
            mediaReader->GetOutputWaveFormatEx(),
            targetHitSound
            );
        // ...
    }

    // Instantiate a set of spheres to be used as ammunition for the game
    // and set the material properties of the spheres.
    auto ammoHitSound = mediaReader->LoadMedia("Assets\\bounce.wav");

    for (int a = 0; a < GameConstants::MaxAmmo; a++)
    {
        m_ammo[a] = ref new Sphere;
        m_ammo[a]->Radius(GameConstants::AmmoRadius);
        m_ammo[a]->HitSound(ref new SoundEffect());
        m_ammo[a]->HitSound()->Initialize(
            m_audioController->SoundEffectEngine(),
            mediaReader->GetOutputWaveFormatEx(),
            ammoHitSound
            );
        m_ammo[a]->Active(false);
        m_renderObjects.push_back(m_ammo[a]);
    }
    // ...
}
```

## <a name="create-and-initialize-the-audio-resources"></a>Créez et initialisez les ressources audio

* [XAudio2Create](https://msdn.microsoft.com/library/windows/desktop/ee419212), une API XAudio2, permet de créer deux nouveaux objets de XAudio2 qui définissent les moteurs effet musique et audio. Cette méthode retourne un pointeur d’interface [IXAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415908) de l’objet qui gère tous les États du moteur audio, le traitement, le graphique de voix et de thread plus audio.
* Après les moteurs ont été instanciés, [IXAudio2::CreateMasteringVoice](https://msdn.microsoft.com/library/windows/desktop/hh405048) permet de créer une voix de contrôle pour chacun des objets du moteur audio.

Pour plus d’informations, accédez à [Comment: initialiser les XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415779.aspx).

### <a name="audiocreatedeviceindependentresources-method"></a>Méthode de audio::CreateDeviceIndependentResources

```cpp

void Audio::CreateDeviceIndependentResources()
{
    UINT32 flags = 0;

    DX::ThrowIfFailed(
        XAudio2Create(&m_musicEngine, flags)
        );

    HRESULT hr = m_musicEngine->CreateMasteringVoice(&m_musicMasteringVoice);
    if (FAILED(hr))
    {
        // Unable to create an audio device
        m_audioAvailable = false;
        return;
    }

    DX::ThrowIfFailed(
        XAudio2Create(&m_soundEffectEngine, flags)
        );

    DX::ThrowIfFailed(
        m_soundEffectEngine->CreateMasteringVoice(&m_soundEffectMasteringVoice)
        );

    m_audioAvailable = true;
}
```

## <a name="load-audio-file"></a>Charger le fichier audio

Dans l’exemple de jeu, le code pour la lecture de fichiers au format audio est défini dans [MediaReader.h](#mediareaderh)/cpp__.  Pour lire un fichier audio .wav codé, appelez [MediaReader::LoadMedia](#mediareaderloadmedia-method), en passant le nom de fichier de le .wav en tant que paramètre d’entrée.

### <a name="mediareaderloadmedia-method"></a>Méthode de MediaReader::LoadMedia

Cette méthode utilise les API [Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197) pour la lecture du fichier audio .wav en tant que tampon de modulation par impulsions codées (PCM).

#### <a name="set-up-the-source-reader"></a>Configurer le lecteur de Source

1. [MFCreateSourceReaderFromURL](https://msdn.microsoft.com/library/windows/desktop/dd388110) permet de créer un support de lecteur de source ([IMFSourceReader](https://msdn.microsoft.com/library/windows/desktop/dd374655)).
2. [MFCreateMediaType](https://msdn.microsoft.com/library/windows/desktop/ms693861) permet de créer un objet de type ([IMFMediaType](https://msdn.microsoft.com/library/windows/desktop/ms704850)) de media (_mediaType_). Il représente une description d’un format de média. 
3. Spécifiez que de _mediaType_d' décodée est une sortie audio PCM, qui est un type audio que __XAudio2__ peut utiliser.
4. Définit le média de sortie décodée type pour le lecteur de source en appelant [IMFSourceReader::SetCurrentMediaType](https://msdn.microsoft.com/library/windows/desktop/dd374667.aspx).

Pour plus d’informations sur la raison pour laquelle nous utilisons le lecteur de Source, accédez au [Lecteur de Source](https://msdn.microsoft.com/library/windows/desktop/dd940436.aspx).

#### <a name="describe-the-data-format-of-the-audio-stream"></a>Décrire le format de données du flux audio

1. Utilisez [IMFSourceReader::GetCurrentMediaType](https://msdn.microsoft.com/library/windows/desktop/dd374660) pour obtenir le type de média en cours pour le flux.
2. Utilisez [IMFMediaType::MFCreateWaveFormatExFromMFMediaType](https://msdn.microsoft.com/library/windows/desktop/ms702177) pour convertir le type de média audio actuel dans une mémoire tampon [WAVEFORMATEX](https://msdn.microsoft.com/library/windows/hardware/ff538799) , en utilisant les résultats de l’opération antérieure comme entrée. Cette structure Spécifie le format de données du flux audio en priorité qui est utilisé après son chargement. 

Le format __WAVEFORMATEX__ peut être utilisé pour décrire le tampon PCM. Par rapport à la structure [WAVEFORMATEXTENSIBLE](https://msdn.microsoft.com/library/windows/hardware/ff538802) , il peut uniquement être utilisé pour décrire un sous-ensemble des formats d’onde audio. Pour plus d’informations sur les différences entre __WAVEFORMATEX__ et __WAVEFORMATEXTENSIBLE__, consultez [Les descripteurs de Format de priorité Extensible](https://docs.microsoft.com/windows-hardware/drivers/audio/extensible-wave-format-descriptors).

#### <a name="read-the-audio-stream"></a>Lire le flux audio

1.  Obtenir la durée, en secondes, du flux audio en appelant [IMFSourceReader::GetPresentationAttribute](https://msdn.microsoft.com/library/windows/desktop/dd374662) , puis convertit la durée en octets.
2.  Lire le fichier audio en tant que flux en appelant la méthode [IMFSourceReader::ReadSample](https://msdn.microsoft.com/library/windows/desktop/dd374665). __ReadSample__ lit l’exemple suivant à partir de la source du média.
3.  Utilisez [IMFSample::ConvertToContiguousBuffer](https://msdn.microsoft.com/library/windows/desktop/ms698917.aspx) pour copier le contenu de la mémoire tampon d’échantillon audio (_exemple_) dans un tableau (_mediaBuffer_).

```cpp
Platform::Array<byte>^ MediaReader::LoadMedia(_In_ Platform::String^ filename)
{
    DX::ThrowIfFailed(
        MFStartup(MF_VERSION)
        );

    // Creates a media source reader.
    ComPtr<IMFSourceReader> reader;
    DX::ThrowIfFailed(
        MFCreateSourceReaderFromURL(
            Platform::String::Concat(m_installedLocationPath, filename)->Data(),
            nullptr,
            &reader
            )
        );

    // Set the decoded output format as PCM.
    // XAudio2 on Windows can process PCM and ADPCM-encoded buffers.
    // When using MediaFoundation, this sample always decodes into PCM.
    Microsoft::WRL::ComPtr<IMFMediaType> mediaType;
    DX::ThrowIfFailed(
        MFCreateMediaType(&mediaType)
        );

    // Define the major category of the media as audio. For more info about major media types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa367377.aspx
    DX::ThrowIfFailed(
        mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
        );

    // Define the sub-type of the media as uncompressed PCM audio. For more info about audio sub-types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa372553.aspx
    DX::ThrowIfFailed(
        mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
        );
    
    // Sets the media type for a stream. This media type defines that format that the Source Reader 
    // produces as output. It can differ from the native format provided by the media source.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/dd374667.aspx
    DX::ThrowIfFailed(
        reader->SetCurrentMediaType(static_cast<uint32>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), 0, mediaType.Get())
        );

    // Get the current media type for the stream.
    // For more info, go to:
    // https://msdn.microsoft.com/library/windows/desktop/dd374660.aspx
        Microsoft::WRL::ComPtr<IMFMediaType> outputMediaType;
    DX::ThrowIfFailed(
        reader->GetCurrentMediaType(static_cast<uint32>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), &outputMediaType)
        );

    // Converts the current media type into the WaveFormatEx buffer structure.
    UINT32 size = 0;
    WAVEFORMATEX* waveFormat;
    DX::ThrowIfFailed(
        MFCreateWaveFormatExFromMFMediaType(outputMediaType.Get(), &waveFormat, &size)
        );

    // Copies the waveFormat's block of memory to the starting address of the m_waveFormat variable in MediaReader.
    // Then free the waveFormat memory block.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/aa366535.aspx and
    // https://msdn.microsoft.com/library/windows/desktop/ms680722.aspx
    CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
    CoTaskMemFree(waveFormat);

    PROPVARIANT propVariant;
    DX::ThrowIfFailed(
        reader->GetPresentationAttribute(static_cast<uint32>(MF_SOURCE_READER_MEDIASOURCE), MF_PD_DURATION, &propVariant)
        );

    // 'duration' is in 100ns units; convert to seconds, and round up
    // to the nearest whole byte.
    LONGLONG duration = propVariant.uhVal.QuadPart;
    unsigned int maxStreamLengthInBytes =
        static_cast<unsigned int>(
            ((duration * static_cast<ULONGLONG>(m_waveFormat.nAvgBytesPerSec)) + 10000000) /
            10000000
            );

    Platform::Array<byte>^ fileData = ref new Platform::Array<byte>(maxStreamLengthInBytes);

    ComPtr<IMFSample> sample;
    ComPtr<IMFMediaBuffer> mediaBuffer;
    DWORD flags = 0;

    int positionInData = 0;
    bool done = false;
    while (!done)
    {
        //...
        // Read audio data.
    }

    return fileData;
}
```
## <a name="associate-sound-to-object"></a>Associer son objet

Association de sons à l’objet d’a lieu lorsque le jeu initialise, dans la méthode [Simple3DGame::Initialize](#simple3dgameinitialize-method) .

Récapitulatif:
* Dans la classe __GameObject__ , il existe une propriété __HitSound__ qui est utilisée pour associer l’effet sonore à l’objet.
* Créer une nouvelle instance de l’objet de classe [SoundEffect](#soundeffecth) et l’associer à l’objet jeu. Cette classe émet un son à l’aide de __XAudio2__ API.  Il utilise une voix de contrôle fournie par la classe [Audio](#audioh) . Les données audio peuvent être lues à partir de l’emplacement du fichier à l’aide de la classe [MediaReader](#mediareaderh) .

[SoundEffect::Initialize](#soundeffectinitialize-method) est utilisé pour initialiser la __SoundEffect__ instance avec les paramètres d’entrée suivants: pointeur vers l’objet de moteur audio (IXAudio2 des objets créés dans la méthode [Audio::CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method) ), pointeur vers le format de le .wav fichier à l’aide de __MediaReader::GetOutputWaveFormatEx__et les données audio chargées à l’aide de la méthode de [MediaReader::LoadMedia](#mediareaderloadmedia-method) . Pendant l’initialisation, la voix source pour l’effet sonore est également créée.

### <a name="soundeffectinitialize-method"></a>Méthode de SoundEffect::Initialize

```cpp
void SoundEffect::Initialize(
    _In_ IXAudio2 *masteringEngine,
    _In_ WAVEFORMATEX *sourceFormat,
    _In_ Platform::Array<byte>^ soundData)
{
    m_soundData = soundData;

    if (masteringEngine == nullptr)
    {
        // Audio is not available so just return.
        m_audioAvailable = false;
        return;
    }

    // Create a source voice for this sound effect.
    DX::ThrowIfFailed(
        masteringEngine->CreateSourceVoice(
            &m_sourceVoice,
            sourceFormat
            )
        );
    m_audioAvailable = true;
}
```

## <a name="play-the-sound"></a>Lire le son

Déclencheurs d’effets sonores sont définies dans [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) méthode car il s’agit où le mouvement des objets sont mis à jour et dépend de collision entre des objets.

Dans la mesure où l’interaction entre les objets de diffère sensiblement selon le jeu, nous n’allons pas pour discuter de la dynamique des objets jeu ici. Si vous avez besoin de comprendre son implémentation, passez à la méthode de [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) .

En principe, lorsqu’une collision se produit, il déclenche l’effet sonore pour lire du contenu en appelant la méthode [SoundEffect::PlaySound]((soundeffectplaysound-method). Cette méthode arrête des effets sonores qui est en cours de lecture et de la mémoire tampon en mémoire avec les données audio souhaités en file d’attente. Il utilise des voix source pour définir le volume, l’envoi des données audio et démarrer la lecture.

### <a name="soundeffectplaysound-method"></a>Méthode de SoundEffect::PlaySound

* Utilise l' objet du voix source **m\_sourceVoice** pour démarrer la lecture de la mémoire tampon de données audio **m\_soundData**
* Crée un [XAUDIO2\_BUFFER](https://msdn.microsoft.com/library/windows/desktop/ee419228), auquel elle fournit une référence à la mémoire tampon de données audio et puis l’envoie par un appel à [IXAudio2SourceVoice::SubmitSourceBuffer](https://msdn.microsoft.com/library/windows/desktop/ee418473). 
* Avec les données audio en file d’attente, **SoundEffect::PlaySound** démarre la lecture en appelant [IXAudio2SourceVoice::Start](https://msdn.microsoft.com/library/windows/desktop/ee418471).

```cpp
void SoundEffect::PlaySound(_In_ float volume)
{
    XAUDIO2_BUFFER buffer = {0};
    XAUDIO2_VOICE_STATE state = {0};

    if (!m_audioAvailable)
    {
        // Audio is not available, so just return.
        return;
    }

    // Interrupt sound effect if currently playing.
    DX::ThrowIfFailed(
        m_sourceVoice->Stop()
        );
    DX::ThrowIfFailed(
        m_sourceVoice->FlushSourceBuffers()
        );

    // Queue in-memory buffer for playback and start the voice.
    buffer.AudioBytes = m_soundData->Length;
    buffer.pAudioData = m_soundData->Data;
    buffer.Flags = XAUDIO2_END_OF_STREAM;

    m_sourceVoice->SetVolume(volume);
    DX::ThrowIfFailed(
        m_sourceVoice->SubmitSourceBuffer(&buffer)
        );
    DX::ThrowIfFailed(
        m_sourceVoice->Start()
        );
}
```

### <a name="simple3dgameupdatedynamics-method"></a>Méthode de Simple3DGame::UpdateDynamics

La méthode __Simple3DGame::UpdateDynamics__ prend en charge l’interaction et les collisions entre les objets du jeu. Lorsque les objets entrent en collision (ou se croisent), il déclenche l’effet sonore associé à lire.

```cpp
void Simple3DGame::UpdateDynamics()
{
    // ...
    // Check for collisions between ammo.
#pragma region inter-ammo collision detection
    if (m_ammoCount > 1)
    {
       // ...
       // Check collision between instances One and Two.
       // ...
       if (distanceSquared < (GameConstants::AmmoSize * GameConstants::AmmoSize))
       {
           // The two ammo are intersecting.
           // ...
           // Start playing the sounds for the impact between the two balls.
              m_ammo[one]->PlaySound(impact, m_player->Position());
              m_ammo[two]->PlaySound(impact, m_player->Position());

       }
    }
#pragma endregion

#pragma region Ammo-Object intersections
    // Check for intersections between the ammo and the other objects in the scene.
    // ...
    // Ball is in contact with Object.
    // ...

    // Make sure that the ball is actually headed towards the object. At grazing angles there
    // could appear to be an impact when the ball is actually already hit and moving away.
    if (impact > 0.0f)
    {
        // ...
        // Play the sound associated with the Ammo hitting something.
           m_ammo[one]->PlaySound(impact, m_player->Position());

        if (m_objects[i]->Target() && !m_objects[i]->Hit())
        {
            // The object is a target and isn't currently hit, so mark it as hit and
            // play the sound associated with the impact.
             m_objects[i]->Hit(true);
             m_objects[i]->HitTime(timeTotal);
             m_totalHits++;

             m_objects[i]->PlaySound(impact, m_player->Position());
        }
        // ...
    }
#pragma endregion

#pragma region Apply Gravity and world intersection
    // Apply gravity and check for collision against enclosing volume.
    // ...
    if (position.z < limit)
    {
        // The ammo instance hit the a wall in the min Z direction.
        // Align the ammo instance to the wall, invert the Z component of the velocity and
        // play the impact sound.
           position.z = limit;
           m_ammo[i]->PlaySound(-velocity.z, m_player->Position());
           velocity.z = -velocity.z * GameConstants::Physics::GroundRestitution;
    }
    // ...
#pragma endregion
}
```
## <a name="next-steps"></a>Étapes suivantes

Nous avons couvert UWP framework, graphiques, contrôles, interface utilisateur et l’audio d’un jeu Windows 10. La partie suivante de ce didacticiel, [extension de l’exemple de jeu](tutorial-resources.md), explique les autres options qui peuvent être utilisées lorsque vous développez un jeu.

## <a name="audio-concepts"></a>Concepts audio

Pour le développement de jeux Windows 10, utilisez XAudio2 version 2.9. Cette version est fournie avec Windows 10. Pour plus d’informations, accédez aux [Versions de XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415802.aspx).

__AudioX2__ est une API de bas niveau qui fournit le traitement du signal et mixage. Pour plus d’informations, voir [Concepts principaux de XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415764.aspx).

### <a name="xaudio2-voices"></a>Voix XAudio2

Il existe trois types d’objets vocaux XAudio2: source, sous-mixée et matriçage. Les voix sont les objets XAudio2 pour lire les données audio à traiter et de manipuler. 
* Les voix source opèrent sur les données audio fournies par le client. 
* Les voix source et sous-mixées envoient leur sortie vers une ou plusieurs voix sous-mixées ou de matriçage. 
* Les voix sous-mixées et de matriçage mixent les signaux provenant de toutes les voix qui les alimentent et opèrent sur le résultat. 
* Les voix de matriçage recevoir des données à partir des voix sources et voix prémixées et envoie ces données au matériel audio.

Pour plus d’informations, accédez à la [voix XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415824.aspx).

### <a name="audio-graph"></a>Graphique audio

Graphique audio est une collection de [voix XAudio2](#xaudio2-voice-objects). Audio commence à un côté d’un graphique audio dans les voix sources, si vous le souhaitez passe par le biais d’un ou plusieurs voix prémixées et se termine à une voix de contrôle. Un graphique audio contient une voix source pour chaque son en cours de lecture, zéro ou plusieurs voix prémixées et une voix mastérisée. Le graphique audio plus simple et au minimum nécessaire pour rendre un bruit dans XAudio2, est une seule voix source sortie directement dans une voix de contrôle. Pour plus d’informations, accédez à [graphiques Audio](https://msdn.microsoft.com/library/windows/desktop/ee415739.aspx).

### <a name="additional-reading"></a>Documentation supplémentaire

* [Procédure: initialiser XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415779.aspx)
* [Procédure: charger des fichiers de données audio dans XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415781(v=vs.85).aspx)
* [Procédure: lire un son avec XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415787.aspx)

## <a name="key-audio-h-files"></a>Fichiers de clé .h audio

### <a name="audioh"></a>Audio.h

```cpp
// Audio:
// This class uses XAudio2 to provide sound output.  It creates two
// engines - one for music and the other for sound effects - each as
// a separate mastering voice.
// The SuspendAudio and ResumeAudio methods can be used to stop
// and start all audio playback.
public:
    Audio();

    void Initialize();
    void CreateDeviceIndependentResources();
    IXAudio2* MusicEngine();
    IXAudio2* SoundEffectEngine();
    void SuspendAudio();
    void ResumeAudio();

protected:
    // ...
};
```
### <a name="mediareaderh"></a>MediaReader.h

```cpp
// MediaReader:
// This is a helper class for the SoundEffect class.  It reads small audio files
// synchronously from the package installed folder and returns sound data as a
// byte array.

ref class MediaReader
{
internal:
    MediaReader();

    Platform::Array<byte>^          LoadMedia(_In_ Platform::String^ filename);
    WAVEFORMATEX*                   GetOutputWaveFormatEx();

protected private:
    Windows::Storage::StorageFolder^ m_installedLocation;
    Platform::String^               m_installedLocationPath;
    WAVEFORMATEX                    m_waveFormat;
};
```
### <a name="soundeffecth"></a>SoundEffect.h

```cpp
// SoundEffect:
// This class plays a sound using XAudio2.  It uses a mastering voice provided
// from the Audio class.  The sound data can be read from disk using the MediaReader
// class.

ref class SoundEffect
{
internal:
    SoundEffect();

    void Initialize(
        _In_ IXAudio2*              masteringEngine,
        _In_ WAVEFORMATEX*          sourceFormat,
        _In_ Platform::Array<byte>^ soundData
        );

    void PlaySound(_In_ float volume);

protected private:
    //...

};
```


