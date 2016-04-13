---
Ajouter du son
Au cours de cette étape, nous étudions comment l’exemple de jeu de tir crée un objet pour la lecture audio avec les API XAudio2.
ms.assetid: aa05efe2-2baa-8b9f-7418-23f5b6cd2266
---

# Ajouter du son


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Au cours de cette étape, nous étudions comment l’exemple de jeu de tir crée un objet pour la lecture audio avec les API [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813).

## Objectif


-   Ajouter une sortie audio avec [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813).

Dans l’exemple de jeu, les comportements et objets audio sont définis dans trois fichiers :

-   **Audio.h/.cpp**. Ce fichier de code définit l’objet **Audio**, qui contient les ressources XAudio2 pour la lecture audio. Il définit également une méthode pour interrompre et reprendre la lecture audio si le jeu est en pause ou désactivé.
-   **MediaReader.h/.cpp**. Ce code définit les méthodes permettant de lire des fichiers .wav audio à partir du stockage local.
-   **SoundEffect.h/.cpp**. Ce code définit un objet pour la lecture audio dans le jeu.

## Définition du moteur audio


Lorsque l’exemple de jeu démarre, il crée un objet **Audio** qui alloue les ressources audio pour le jeu. Le code qui déclare cet objet se présente comme suit :

```cpp
public:
    Audio();

    void Initialize();
    void CreateDeviceIndependentResources();
    IXAudio2* MusicEngine();
    IXAudio2* SoundEffectEngine();
    void SuspendAudio();
    void ResumeAudio();

protected:
    bool                                m_audioAvailable;
    Microsoft::WRL::ComPtr<IXAudio2>    m_musicEngine;
    Microsoft::WRL::ComPtr<IXAudio2>    m_soundEffectEngine;
    IXAudio2MasteringVoice*             m_musicMasteringVoice;
    IXAudio2MasteringVoice*             m_soundEffectMasteringVoice;
};
```

Les méthodes **Audio::MusicEngine** et **Audio::SoundEffectEngine** renvoient des références aux objets [**IXAudio2**](https://msdn.microsoft.com/library/windows/desktop/ee415908) qui définissent la voix de contrôle pour chaque type audio. Une voix de contrôle est le périphérique audio utilisé pour la lecture. Les tampons de données audio peuvent être envoyés directement aux voix de contrôle, mais les données envoyées à d’autres types de voix doivent être dirigées vers une voix de contrôle pour être entendues.

## Initialisation des ressources audio


L’exemple initialise les objets [**IXAudio2**](https://msdn.microsoft.com/library/windows/desktop/ee415908) pour les moteurs de musique et d’effets sonores avec des appels de [**XAudio2Create**](https://msdn.microsoft.com/library/windows/desktop/ee419212). Une fois que les moteurs ont été instanciés, il crée une voix de contrôle pour chacun avec des appels à [**IXAudio2::CreateMasteringVoice**](https://msdn.microsoft.com/library/windows/desktop/hh405048), comme ici :

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

Lorsqu’un fichier audio d’effet sonore ou de musique est chargé, cette méthode appelle [**IXAudio2::CreateSourceVoice**](https://msdn.microsoft.com/library/windows/desktop/ee418607) sur la voix de contrôle, ce qui crée une instance de voix source pour la lecture. Nous regarderons le code pour cela dès que nous aurons fini d’examiner la façon dont l’exemple de jeu charge les fichiers audio.

## Lecture d’un fichier audio


Dans l’exemple de jeu, le code pour la lecture des fichiers au format audio est défini dans **MediaReader.cpp**. La méthode spécifique qui lit un fichier audio .wav codé, **MediaReader::LoadMedia**, se présente comme suit :

```cpp
Platform::Array<byte>^  MediaReader::LoadMedia(_In_ Platform::String^ filename)
{
    DX::ThrowIfFailed(
        MFStartup(MF_VERSION)
        );

    ComPtr<IMFSourceReader> reader;
    DX::ThrowIfFailed(
        MFCreateSourceReaderFromURL(
            Platform::String::Concat(m_installedLocationPath, filename)->Data(),
            nullptr,
            &reader
            )
        );

    // Set the decoded output format as PCM
    // XAudio2 on Windows can process PCM and ADPCM-encoded buffers.
    // When using MediaFoundation, this sample always decodes into PCM.
    Microsoft::WRL::ComPtr<IMFMediaType> mediaType;
    DX::ThrowIfFailed(
        MFCreateMediaType(&mediaType)
        );

    DX::ThrowIfFailed(
        mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
        );

    DX::ThrowIfFailed(
        mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
        );

    DX::ThrowIfFailed(
        reader->SetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, 0, mediaType.Get())
        );

    // Get the complete WAVEFORMAT from the Media Type.
    Microsoft::WRL::ComPtr<IMFMediaType> outputMediaType;
    DX::ThrowIfFailed(
        reader->GetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, &outputMediaType)
        );

    UINT32 size = 0;
    WAVEFORMATEX* waveFormat;
    DX::ThrowIfFailed(
        MFCreateWaveFormatExFromMFMediaType(outputMediaType.Get(), &waveFormat, &size)
        );

    CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
    CoTaskMemFree(waveFormat);

    // Get the total length of the stream in bytes.
    PROPVARIANT propVariant;
    DX::ThrowIfFailed(
        reader->GetPresentationAttribute(MF_SOURCE_READER_MEDIASOURCE, MF_PD_DURATION, &propVariant)
        );
    LONGLONG duration = propVariant.uhVal.QuadPart;
    unsigned int maxStreamLengthInBytes;

    double durationInSeconds = (duration / static_cast<double>(10000 * 1000));
    maxStreamLengthInBytes = static_cast<unsigned int>(durationInSeconds * m_waveFormat.nAvgBytesPerSec);

    // Make the length a multiple of 4 bytes.
    maxStreamLengthInBytes = (maxStreamLengthInBytes + 3) / 4 * 4;

    Platform::Array<byte>^ fileData = ref new Platform::Array<byte>(maxStreamLengthInBytes);

    ComPtr<IMFSample> sample;
    ComPtr<IMFMediaBuffer> mediaBuffer;
    DWORD flags = 0;

    int positionInData = 0;
    bool done = false;
    while (!done)
    {
        DX::ThrowIfFailed(
            reader->ReadSample(MF_SOURCE_READER_FIRST_AUDIO_STREAM, 0, nullptr, &flags, nullptr, &sample)
            );

        if (sample != nullptr)
        {
            DX::ThrowIfFailed(
                sample->ConvertToContiguousBuffer(&mediaBuffer)
                );

            BYTE *audioData = nullptr;
            DWORD sampleBufferLength = 0;
            DX::ThrowIfFailed(
                mediaBuffer->Lock(&audioData, nullptr, &sampleBufferLength)
                );

            for (DWORD i = 0; i < sampleBufferLength; i++)
            {
                fileData[positionInData++] = audioData[i];
            }
        }
        if (flags & MF_SOURCE_READERF_ENDOFSTREAM)
        {
            done = true;
        }
    }

    // Fix up the array size on match the actual length.
    Platform::Array<byte>^ realfileData = ref new Platform::Array<byte>((positionInData + 3) / 4 * 4);
    memcpy(realfileData->Data, fileData->Data, positionInData);
    return realfileData;
}
```

Cette méthode utilise les API [Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197) pour la lecture du fichier audio .wav en tant que tampon de modulation par impulsions codées (PCM).

1.  Crée un objet lecteur de source multimédia ([**IMFSourceReader**](https://msdn.microsoft.com/library/windows/desktop/dd374655)) en appelant [**MFCreateSourceReaderFromURL**](https://msdn.microsoft.com/library/windows/desktop/dd388110).
2.  Crée un type de média ([**IMFMediaType**](https://msdn.microsoft.com/library/windows/desktop/ms704850)) pour le décodage du fichier audio en appelant [**MFCreateMediaType**](https://msdn.microsoft.com/library/windows/desktop/ms693861). Cette méthode spécifie que la sortie décodée est une sortie audio PCM, qui est un type audio que XAudio2 peut utiliser.
3.  Définit le type de média de sortie décodée pour le lecteur en appelant [**IMFSourceReader::SetCurrentMediaType**](https://msdn.microsoft.com/library/windows/desktop/bb970432).
4.  Crée un tampon [**WAVEFORMATEX**](https://msdn.microsoft.com/library/windows/hardware/ff538799) et copie les résultats d’un appel à [**IMFMediaType::MFCreateWaveFormatExFromMFMediaType**](https://msdn.microsoft.com/library/windows/desktop/ms702177) sur l’objet [**IMFMediaType**](https://msdn.microsoft.com/library/windows/desktop/ms704850). Cette opération formate le tampon qui contient le fichier audio après son chargement.
5.  Obtient la durée, en secondes, du flux audio en appelant [**IMFSourceReader::GetPresentationAttribute**](https://msdn.microsoft.com/library/windows/desktop/dd374662) et convertit la durée en octets.
6.  Lit le fichier audio en tant que flux en appelant [**IMFSourceReader::ReadSample**](https://msdn.microsoft.com/library/windows/desktop/dd374665).
7.  Copie le contenu de l’exemple de tampon audio dans un tableau renvoyé par la méthode.

Le point essentiel de **SoundEffect::Initialize** est la création de l’objet voix source, **m\_sourceVoice**, à partir de la voix de contrôle. Nous utilisons la voix source pour la lecture réelle du tampon de données audio obtenu à partir de **MediaReader::LoadMedia**.

L’exemple de jeu appelle cette méthode lors de l’initialisation de l’objet **SoundEffect**, comme suit :

```cpp
void SoundEffect::Initialize(
    _In_ IXAudio2 *masteringEngine,
    _In_ WAVEFORMATEX *sourceFormat,
    _In_ Platform::Array<byte>^ soundData)
{
    m_soundData = soundData;

    if (masteringEngine == nullptr)
    {
        // Audio is not available, so return.
        m_audioAvailable = false;
        return;
    }

    // Create and reuse a single source voice for the single sound effect in this sample.
    DX::ThrowIfFailed(
        masteringEngine->CreateSourceVoice(
            &m_sourceVoice,
            sourceFormat
            )
        );
    m_audioAvailable = true;
}
```

Cette méthode reçoit les résultats des appels de **Audio::SoundEffectEngine** (ou **Audio::MusicEngine**) et de **MediaReader::GetOutputWaveFormatEx**, ainsi que le tampon renvoyé par un appel de **MediaReader::LoadMedia**, comme présenté ici.

```cpp
MediaReader^ mediaReader = ref new MediaReader;
auto targetHitSound = mediaReader->LoadMedia("hit.wav");
```

```cpp
myTarget->HitSound(ref new SoundEffect());
myTarget->HitSound()->Initialize(
                m_audioController->SoundEffectEngine(),
                mediaReader->GetOutputWaveFormatEx(),
                targetHitSound);
```

**SoundEffect::Initialize** est appelé à partir de la méthode **Simple3DGame:Initialize** qui initialise l’objet jeu principal.

Maintenant que l’exemple de jeu a un fichier audio en mémoire, voyons comment il le lit pendant le jeu !

## Lecture d’un fichier audio


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

Pour lire le fichier audio, cette méthode utilise l’objet voix source **m\_sourceVoice** pour démarrer la lecture du tampon de données audio **m\_soundData**. Elle crée un [**XAUDIO2\_BUFFER**](https://msdn.microsoft.com/library/windows/desktop/ee419228), auquel elle fournit une référence au tampon de données audio, puis l’envoie par un appel de [**IXAudio2SourceVoice::SubmitSourceBuffer**](https://msdn.microsoft.com/library/windows/desktop/ee418473). Avec les données audio en file d’attente, **SoundEffect::PlaySound** démarre la lecture en appelant [**IXAudio2SourceVoice::Start**](https://msdn.microsoft.com/library/windows/desktop/ee418471).

Maintenant, chaque fois qu’une collision se produit entre les munitions et une cible, un appel de **SoundEffect::PlaySound** entraîne la lecture d’un bruit.

## Étapes suivantes


Nous avons fait un tour rapide du développement de jeux de Universal Windows Platform (UWP) DirectX ! À ce stade, vous avez une idée de ce que vous devez faire pour profiter au maximum de votre jeu sur Windows 8. Comme vous pouvez jouer sur une multitude de plateformes et d’appareils Windows 8, pensez à concevoir vos composants (graphiques, contrôles, interface utilisateur et système audio) pour autant de configurations que possible !

Pour plus d’informations sur les façons de modifier l’exemple de jeu fourni dans ces documents, voir [Extension de l’exemple de jeu](tutorial-resources.md).

## Exemple de code complet pour cette section


Audio.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

ref class Audio
{
public:
    Audio();

    void Initialize();
    void CreateDeviceIndependentResources();
    IXAudio2* MusicEngine();
    IXAudio2* SoundEffectEngine();
    void SuspendAudio();
    void ResumeAudio();

protected:
    bool                                m_audioAvailable;
    Microsoft::WRL::ComPtr<IXAudio2>    m_musicEngine;
    Microsoft::WRL::ComPtr<IXAudio2>    m_soundEffectEngine;
    IXAudio2MasteringVoice*             m_musicMasteringVoice;
    IXAudio2MasteringVoice*             m_soundEffectMasteringVoice;
};
```

Audio.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Audio.h"
#include "DirectXSample.h"

using namespace Microsoft::WRL;
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
using namespace Windows::Graphics::Display;

Audio::Audio():
    m_audioAvailable(false)
{
}

void Audio::Initialize()
{
}

void Audio::CreateDeviceIndependentResources()
{
    UINT32 flags = 0;

    DX::ThrowIfFailed(
        XAudio2Create(&m_musicEngine, flags)
        );

#if defined(_DEBUG)
    XAUDIO2_DEBUG_CONFIGURATION debugConfiguration = {0};
    debugConfiguration.BreakMask = XAUDIO2_LOG_ERRORS;
    debugConfiguration.TraceMask = XAUDIO2_LOG_ERRORS;
    m_musicEngine->SetDebugConfiguration(&debugConfiguration);
#endif


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

#if defined(_DEBUG)
    m_soundEffectEngine->SetDebugConfiguration(&debugConfiguration);
#endif

    DX::ThrowIfFailed(
        m_soundEffectEngine->CreateMasteringVoice(&m_soundEffectMasteringVoice)
        );

    m_audioAvailable = true;
}

IXAudio2* Audio::MusicEngine()
{
    return m_musicEngine.Get();
}

IXAudio2* Audio::SoundEffectEngine()
{
    return m_soundEffectEngine.Get();
}

void Audio::SuspendAudio()
{
    if (m_audioAvailable)
    {
        m_musicEngine->StopEngine();
        m_soundEffectEngine->StopEngine();
    }
}

void Audio::ResumeAudio()
{
    if (m_audioAvailable)
    {
        DX::ThrowIfFailed(m_musicEngine->StartEngine());
        DX::ThrowIfFailed(m_soundEffectEngine->StartEngine());
    }
}
```

SoundEffect.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

ref class SoundEffect
{
public:
    SoundEffect();

    void Initialize(
        _In_ IXAudio2*              masteringEngine,
        _In_ WAVEFORMATEX*          sourceFormat,
        _In_ Platform::Array<byte>^ soundData
        );

    void PlaySound(_In_ float volume);

protected:
    bool                    m_audioAvailable;
    IXAudio2SourceVoice*    m_sourceVoice;
    Platform::Array<byte>^  m_soundData;
};
```

SoundEffect.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "SoundEffect.h"
#include "DirectXSample.h"

SoundEffect::SoundEffect():
    m_audioAvailable(false)
{
}
//----------------------------------------------------------------------
void SoundEffect::Initialize(
    _In_ IXAudio2 *masteringEngine,
    _In_ WAVEFORMATEX *sourceFormat,
    _In_ Platform::Array<byte>^ soundData)
{
    m_soundData = soundData;

    if (masteringEngine == nullptr)
    {
        // Audio is not available, so return.
        m_audioAvailable = false;
        return;
    }

    // Create and reuse a single source voice for the single sound effect in this sample.
    DX::ThrowIfFailed(
        masteringEngine->CreateSourceVoice(
            &m_sourceVoice,
            sourceFormat
            )
        );
    m_audioAvailable = true;
}
//----------------------------------------------------------------------
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

 

 






<!--HONumber=Mar16_HO1-->


