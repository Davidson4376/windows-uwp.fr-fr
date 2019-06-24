---
title: Ajouter du son
description: Développer un moteur de son simple à l’aide de XAudio2 API musique la lecture du jeu et les effets sonores.
ms.assetid: aa05efe2-2baa-8b9f-7418-23f5b6cd2266
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, jeux, son
ms.localizationpriority: medium
ms.openlocfilehash: 06c06e1ffe52cae37a000f748076d78ebf6afff4
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318861"
---
# <a name="add-sound"></a>Ajouter du son

Dans cette rubrique, nous créons un simple moteur audio à l’aide [XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-introduction) API. Si vous ne connaissez pas __XAudio2__, nous avons inclus une courte présentation sous [concepts de l’Audio](#audio-concepts).

>[!Note]
>Si vous n’avez pas encore téléchargé le dernier code de jeu pour cet exemple, accédez à [Exemple de jeu Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Cet exemple fait partie d’une vaste collection d’exemples de fonctionnalités UWP. Pour obtenir des instructions sur le téléchargement de l’exemple, consultez [Obtenir des exemples de la plateforme Windows universelle (UWP) à partir de GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objectif

Ajouter des sons dans l’exemple qui utilise jeu [XAudio2](/windows/desktop/xaudio2/xaudio2-introduction).

## <a name="define-the-audio-engine"></a>Définir le moteur audio

Dans l’exemple de jeu, les comportements et objets audio sont définis dans trois fichiers :

* __[Audio.h](#audioh)/.cpp__: Définit le __Audio__ objet qui contient le __XAudio2__ ressources lecture audio. Il définit également une méthode pour interrompre et reprendre la lecture audio si le jeu est en pause ou désactivé.
* __[MediaReader.h](#mediareaderh)/.cpp__: Définit les méthodes pour la lecture des fichiers audio .wav à partir du stockage local.
* __[SoundEffect.h](#soundeffecth)/.cpp__: Définit un objet pour une lecture dans le jeu.

## <a name="overview"></a>Vue d'ensemble

Il existe trois parties principales dans sa configuration pour la lecture audio dans votre jeu.

1. [Créer et initialiser les ressources audio](#create-and-initialize-the-audio-resources)
2. [Charger le fichier audio](#load-audio-file)
3. [Associer son objet](#associate-sound-to-object)

Elles sont définies dans le [Simple3DGame::Initialize](#simple3dgameinitialize-method) (méthode). Par conséquent, nous allons tout d’abord examiner cette méthode et avant d’étudier plus en détail dans chacune des sections.

Après avoir configuré, nous comment déclencher les effets de son à lire. Pour plus d’informations, accédez à [le signal sonore](#play-the-sound).

### <a name="simple3dgameinitialize-method"></a>Simple3DGame::Initialize (méthode)

Dans __Simple3DGame::Initialize__, où __m\_contrôleur__ et __m\_convertisseur__ sont également initialisés, nous configurer le moteur audio et obtenez-le prêt à émettre des sons.

 * Créer __m\_audioController__, qui est une instance de la [Audio](#audioh) classe.
 * Créer les ressources audio nécessaires à l’aide de la [Audio::CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method) (méthode). Ici, deux __XAudio2__ objets &mdash; un objet de moteur de musique et un objet de son moteur et une maîtrise voix pour chacun d'entre eux ont été créés. L’objet de moteur de musique peut être utilisé pour lire la musique de fond pour votre jeu. Le moteur audio peut être utilisé pour lire des effets sonores dans votre jeu. Pour plus d’informations, consultez [créer et initialiser les ressources audio](#create-and-initialize-the-audio-resources).
 * Créer __mediaReader__, qui est une instance de [MediaReader](#mediareaderh) classe. [MediaReader](#mediareaderh), qui est une classe d’assistance pour la [SoundEffect](#soundeffecth) classe, lectures audio de petits fichiers de façon synchrone à partir de l’emplacement du fichier et retourne les données audio en tant que tableau d’octets.
 * Utilisez [MediaReader::LoadMedia](#mediareaderloadmedia-method) pour charger des fichiers audio à partir de son emplacement et créer un __targetHitSound__ variable pour contenir les données audio .wav chargé. Pour plus d’informations, consultez [charger le fichier audio](#load-audio-file). 

Effets sonores sont associées à l’objet de jeu. Par conséquent, lorsqu’une collision se produit avec cet objet de jeu, il déclenche l’effet audio à lire. Dans cet exemple de jeu, nous avons des effets sonores pour la munitions (ce qui nous permet de dépanner des cibles avec) et pour la cible. 
    
* Dans le __GameObject__ de classe, il existe un __HitSound__ propriété qui est utilisée pour associer l’effet audio à l’objet.
* Créer une nouvelle instance de la [SoundEffect](#soundeffecth) classe et l’initialiser. Pendant l’initialisation, une voix source pour l’effet sonore est créée. 
* Cette classe lit un son à l’aide d’une voix maîtrise fournie à partir de la [Audio](#audioh) classe. Les données audio sont lues à partir de l’emplacement du fichier à l’aide du [MediaReader](#mediareaderh) classe. Pour plus d’informations, consultez [associer son objet](#associate-sound-to-object).

>[!Note]
>Le déclencheur réels pour lire le son est déterminé par le déplacement et la collision de ces objets de jeu. Par conséquent, l’appel à ces sons réellement sont définies dans le [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) (méthode). Pour plus d’informations, accédez à [le signal sonore](#play-the-sound).

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

## <a name="create-and-initialize-the-audio-resources"></a>Créer et initialiser les ressources audio

* Utilisez [XAudio2Create](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create), une API XAudio2, pour créer deux objets XAudio2 qui définissent les moteurs d’effet de musique et son. Cette méthode retourne un pointeur vers l’objet [IXAudio2](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) interface qui gère le moteur audio de tous les États, l’audio de traitement de thread, le graphique de voix et bien plus encore.
* Une fois que les moteurs ont été instanciés, utiliser [IXAudio2::CreateMasteringVoice](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createmasteringvoice) pour créer une voix maîtrise pour chacun des objets de son moteur.

Pour plus d’informations, accédez à [Comment : Initialiser XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/how-to--initialize-xaudio2).

### <a name="audiocreatedeviceindependentresources-method"></a>Audio::CreateDeviceIndependentResources (méthode)

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

Dans l’exemple de jeu, le code pour la lecture des fichiers de format audio est défini dans [MediaReader.h](#mediareaderh)/cpp__.  Pour lire un fichier audio .wav encodé, appelez [MediaReader::LoadMedia](#mediareaderloadmedia-method), en passant le nom de fichier de la .wav comme paramètre d’entrée.

### <a name="mediareaderloadmedia-method"></a>MediaReader::LoadMedia method

Cette méthode utilise les API [Media Foundation](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk) pour la lecture du fichier audio .wav en tant que tampon de modulation par impulsions codées (PCM).

#### <a name="set-up-the-source-reader"></a>Configurer le lecteur Source

1. Utilisez [MFCreateSourceReaderFromURL](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-mfcreatesourcereaderfromurl) pour créer un support de lecteur de la source ([IMFSourceReader](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nn-mfreadwrite-imfsourcereader)).
2. Utilisez [MFCreateMediaType](https://docs.microsoft.com/windows/desktop/api/mfapi/nf-mfapi-mfcreatemediatype) pour créer un type de média ([IMFMediaType](https://docs.microsoft.com/windows/desktop/api/mfobjects/nn-mfobjects-imfmediatype)) objet (_mediaType_). Il représente une description d’un format de média. 
3. Spécifier que le _mediaType_de décodé sortie est audio PCM, qui est son type de __XAudio2__ peut utiliser.
4. Définit le type de média sortie décodé pour le lecteur source en appelant [IMFSourceReader::SetCurrentMediaType](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentmediatype).

Pour plus d’informations sur la raison pour laquelle nous utilisons le lecteur Source, accédez à [lecteur Source](https://docs.microsoft.com/windows/desktop/medfound/source-reader).

#### <a name="describe-the-data-format-of-the-audio-stream"></a>Décrire le format de données du flux audio

1. Utilisez [IMFSourceReader::GetCurrentMediaType](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getcurrentmediatype) pour obtenir le type de média actuel pour le flux.
2. Utilisez [IMFMediaType::MFCreateWaveFormatExFromMFMediaType](https://docs.microsoft.com/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype) pour convertir les type de média audio actuel à un [WAVEFORMATEX](https://docs.microsoft.com/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) mettre en mémoire tampon, l’utilisation des résultats de l’opération précédente en tant qu’entrée. Cette structure Spécifie le format de données du flux audio wave qui est utilisé une fois que l’audio est chargé. 

Le __WAVEFORMATEX__ format peut être utilisé pour décrire la mémoire tampon PCM. Par rapport à la [WAVEFORMATEXTENSIBLE](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/ksmedia/ns-ksmedia-waveformatextensible) structure, il peut uniquement être utilisé pour décrire un sous-ensemble de formats audio wave. Pour plus d’informations sur les différences entre __WAVEFORMATEX__ et __WAVEFORMATEXTENSIBLE__, consultez [Extensible au Format Wave descripteurs](https://docs.microsoft.com/windows-hardware/drivers/audio/extensible-wave-format-descriptors).

#### <a name="read-the-audio-stream"></a>Lire le flux audio

1.  Obtenir la durée, en secondes, du flux audio en appelant [IMFSourceReader::GetPresentationAttribute](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getpresentationattribute) et convertit ensuite la durée en octets.
2.  Lire le fichier audio dans en tant que flux en appelant [IMFSourceReader::ReadSample](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-readsample). __ReadSample__ lit l’exemple suivant à partir de la source du média.
3.  Utilisez [IMFSample::ConvertToContiguousBuffer](https://docs.microsoft.com/windows/desktop/api/mfobjects/nf-mfobjects-imfsample-converttocontiguousbuffer) pour copier le contenu de la mémoire tampon échantillon audio (_exemple_) dans un tableau (_mediaBuffer_).

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

Associer des sons à l’objet a lieu lors de l’initialisation du jeu, dans le [Simple3DGame::Initialize](#simple3dgameinitialize-method) (méthode).

Récapitulatif :
* Dans le __GameObject__ de classe, il existe un __HitSound__ propriété qui est utilisée pour associer l’effet audio à l’objet.
* Créer une nouvelle instance de la [SoundEffect](#soundeffecth) classe objet et l’associer à l’objet de jeu. Cette classe joue un son à l’aide __XAudio2__ API.  Il utilise une voix maîtrise fournie par le [Audio](#audioh) classe. Les données audio peuvent être lues à partir de l’emplacement du fichier à l’aide du [MediaReader](#mediareaderh) classe.

[SoundEffect::Initialize](#soundeffectinitialize-method) est utilisé pour initialiser le __SoundEffect__ instance avec les paramètres d’entrée suivants : pointeur vers l’objet de son moteur (objets IXAudio2 créés dans le [Audio :: CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method) méthode), pointeur vers le format du fichier .wav en utilisant __MediaReader::GetOutputWaveFormatEx__, et les données audio chargées à l’aide de [MediaReader::LoadMedia ](#mediareaderloadmedia-method) (méthode). Pendant l’initialisation, la voix source pour l’effet audio est également créée.

### <a name="soundeffectinitialize-method"></a>SoundEffect::Initialize (méthode)

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

Les déclencheurs à effets sonores sont définis dans [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) méthode comme il s’agit où le déplacement des objets sont mis à jour et la collision entre des objets est déterminée.

Étant donné que l’interaction d’entre les objets diffère considérablement, selon le jeu, nous n’allons pas aborder la dynamique des objets de jeu. Si vous vous intéressez à comprendre son implémentation, accédez à [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) (méthode).

En principe, lorsqu’une collision se produit, il déclenche l’effet audio à lire en appelant **SoundEffect::PlaySound**. Cette méthode permet d’arrêter les effets sonores qui est en cours de lecture et de files d’attente de la mémoire tampon en mémoire avec les données audio souhaitées. Il utilise la voix source pour définir le volume, envoyer des données audio et démarrer la lecture.

### <a name="soundeffectplaysound-method"></a>SoundEffect::PlaySound (méthode)

* Utilise l’objet source de la voix **m\_sourceVoice** pour démarrer la lecture de la mémoire tampon de données audio **m\_soundData**
* Crée un [XAUDIO2\_tampon](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_buffer), dans lequel il fournit une référence à la mémoire tampon de données audio et il soumet ensuite avec un appel à [IXAudio2SourceVoice::SubmitSourceBuffer](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-submitsourcebuffer). 
* Avec les données audio la file d’attente, **SoundEffect::PlaySound** démarre lire en appelant [IXAudio2SourceVoice::Start](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start).

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

### <a name="simple3dgameupdatedynamics-method"></a>Simple3DGame::UpdateDynamics (méthode)

Le __Simple3DGame::UpdateDynamics__ méthode prend en charge l’interaction et une collision entre des objets de jeu. Lorsque des objets sont en conflit (ou se croisent), il déclenche l’effet audio associé à lire.

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

Nous avons abordé UWP framework, graphiques, contrôles, interface utilisateur et l’audio d’un jeu de Windows 10. La partie suivante de ce didacticiel, [extension de l’exemple de jeu](tutorial-resources.md), décrit les autres options qui peuvent être utilisées lors du développement d’un jeu.

## <a name="audio-concepts"></a>Concepts de l’audio

Pour le développement de jeux Windows 10, utilisez XAudio2 version 2.9. Cette version est livrée avec Windows 10. Pour plus d’informations, accédez à [XAudio2 Versions](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-versions).

__AudioX2__ est une API de bas niveau qui fournit le traitement des signaux et en combinant foundation. Pour plus d’informations, consultez [XAudio2 Key Concepts](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-key-concepts).

### <a name="xaudio2-voices"></a>XAudio2 voices

Il existe trois types d’objets de voix XAudio2 : source, le mixage secondaire et la maîtrise de voix. Voix est que les objets XAudio2 utilisent pour traiter, manipuler et lire des données audio. 
* Les voix source opèrent sur les données audio fournies par le client. 
* Les voix source et sous-mixées envoient leur sortie vers une ou plusieurs voix sous-mixées ou de matriçage. 
* Les voix sous-mixées et de matriçage mixent les signaux provenant de toutes les voix qui les alimentent et opèrent sur le résultat. 
* Voix maîtrise recevoir des données à partir de la voix de la source et de voix de mixage secondaire et les envoie au matériel audio.

Pour plus d’informations, accédez à [XAudio2 voix](/windows/desktop/xaudio2/xaudio2-voices).

### <a name="audio-graph"></a>Graphique audio

Graphique de l’audio est une collection de [XAudio2 voix](/windows/desktop/xaudio2/xaudio2-voices). Audio commence à un côté d’un graphique d’audio dans les voix source éventuellement traverse un ou plusieurs voix de mixage secondaire et se termine à une maîtrise voix. Un graphique d’audio contiendra une voix source pour chaque lecture du son actuellement, zéro ou plusieurs voix de mixage secondaire et une seule des voix maîtrise. Le graphique audio la plus simple et la valeur minimale nécessaire pour rendre un bruit de XAudio2, est une voix source unique sortie directement dans une maîtrise voix. Pour plus d’informations, accédez à [graphiques de l’Audio](https://docs.microsoft.com/windows/desktop/xaudio2/audio-graphs).

### <a name="additional-reading"></a>Lecture supplémentaire

* [Procédure : Initialiser XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/how-to--initialize-xaudio2)
* [Procédure : Charger des fichiers de données Audio dans XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/how-to--load-audio-data-files-in-xaudio2)
* [Procédure : Émettre un signal sonore avec XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/how-to--play-a-sound-with-xaudio2)

## <a name="key-audio-h-files"></a>Fichiers de clés .h audio

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


