---
Ajout de son à l’exemple Marble Maze
Ce document décrit les pratiques clés à prendre en compte quand vous utilisez du son et comment appliquer ces pratiques à l’exemple Marble Maze.
ms.assetid: 77c23d0a-af6d-17b5-d69e-51d9885b0d44
---

# Ajout de son à l’exemple Marble Maze


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Ce document décrit les pratiques clés à prendre en compte quand vous utilisez du son et comment appliquer ces pratiques à l’exemple Marble Maze. Marble Maze utilise Microsoft Media Foundation pour charger des ressources audio à partir de fichiers, et XAudio2 pour mixer et lire l’audio et pour appliquer des effets à l’audio.

Marble Maze joue de la musique en arrière-plan et utilise également des sons dans le jeu pour indiquer des événements, par exemple quand la bille touche un mur. Dans son implémentation, Marble Maze utilise un effet de réverbération, ou écho, pour reproduire le son de la bille quand elle rebondit. L’implémentation de l’effet de réverbération permet à l’écho de s’entendre plus rapidement et plus fort dans des petites pièces et d’être plus sourd et de s’entendre moins fort dans des pièces de grandes dimensions.

> **Remarque** L’exemple de code correspondant à ce document est disponible dans l’[exemple de jeu Marble Maze en DirectX](http://go.microsoft.com/fwlink/?LinkId=624011).

Voici quelques éléments clés présentés dans ce document que vous devez prendre en compte quand vous utilisez du son dans votre jeu :

-   Utilisez Media Foundation pour décoder les éléments audio et XAudio2 pour lire l’audio. Cependant, si vous disposez déjà d’un mécanisme de chargement d’éléments audio qui fonctionne dans une application de la plateforme Windows universelle (UWP), vous pouvez l’utiliser.
-   Un graphique audio contient une voix source pour chaque son actif, zéro ou plusieurs voix prémixées et une voix mastérisée. Les voix sources peuvent être utilisées dans les voix prémixées et/ou dans la voix mastérisée. Les voix prémixées peuvent être utilisées dans d’autres voix prémixées ou dans la voix mastérisée.
-   Si la taille de vos fichiers de musique de fond est importante, diffusez la musique en continu dans des mémoires tampons de plus petites tailles afin d’économiser la mémoire.
-   Vous pouvez également décider de suspendre l’audio quand l’application perd le focus, n’est plus visible ou est suspendue. Relancez l’audio quand l’application retrouve le focus, est de nouveau visible ou est relancée.
-   Définissez les catégories audio de sorte qu’elles reflètent le rôle de chaque son. Par exemple, vous utilisez généralement **AudioCategory_GameMedia** pour le fond sonore du jeu et **AudioCategory\_GameEffects** pour les effets sonores.
-   Gérez les changements de périphériques, notamment les casques, en libérant et en recréant toutes les ressources et interfaces audio.
-   Pensez à compresser les fichiers audio quand vous devez économiser l’espace disque et réduire les coûts de diffusion. Dans le cas contraire, ne compressez pas l’audio pour un chargement plus rapide.

## Présentation de XAudio2 et Microsoft Media Foundation


XAudio2 est une bibliothèque audio de bas niveau pour Windows qui prend en charge tout particulièrement l’audio de jeu. Il fournit un moteur de traitement du signal numérique (DSP) et de graphique audio pour les jeux. XAudio2 se base sur ses prédécesseurs, DirectSound et XAudio, pour prendre en charge les tendances informatiques telles que les architectures à virgule flottante SIMD et l’audio HD. Il prend également en charge les demandes de traitement audio plus complexes des jeux actuels.

Le document [Concepts principaux de XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415764) explique les concepts clés de l’utilisation de XAudio2. En résumé, ces concepts sont les suivants :

-   L’interface [**IXAudio2**](https://msdn.microsoft.com/library/windows/desktop/ee415908) est au cœur du moteur XAudio2. Marble Maze utilise cette interface pour créer des voix et recevoir une notification quand le périphérique de sortie change ou ne fonctionne pas.
-   Une voix traite, ajuste et lit les données audio.
-   Une voix source est une collection de canaux audio (mono, 5.1, etc.) et représente un flux de données audio. Dans XAudio2, le traitement de l’audio commence au niveau de la voix source. En général, les données sonores sont chargées à partir d’une source externe, par exemple un fichier ou un réseau, puis sont envoyées à une voix source. Marble Maze utilise [Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197) pour charger les données audio à partir de fichiers. Media Foundation est présenté plus loin dans ce document.
-   Une voix prémixée traite les données audio. Ce traitement peut inclure la modification du flux audio ou la combinaison de plusieurs flux en un seul. Marble Maze utilise les prémixages pour créer l’effet de réverbération.
-   Une voix mastérisée combine les données des voix sources et prémixées et envoie ces données au matériel audio.
-   Un graphique audio contient une voix source pour chaque son actif, zéro ou plusieurs voix prémixées et une seule voix mastérisée.
-   Un rappel informe le code client qu’un événement s’est produit dans une voix ou dans un objet de moteur. En utilisant des rappels, vous pouvez réutiliser la mémoire quand XAudio2 a terminé d’utiliser une mémoire tampon, vous pouvez réagir quand le périphérique audio change (par exemple, quand vous connectez ou déconnectez un casque), etc. La section [Gestion des casques et des changements de périphériques](#phones), plus loin dans ce document, explique comment Marble Maze utilise ce mécanisme pour gérer les changements de périphériques.

Marble Maze utilise deux moteurs audio (c’est-à-dire, deux objets [**IXAudio2**](https://msdn.microsoft.com/library/windows/desktop/ee415908)) pour traiter l’audio. Un moteur traite la musique de fond et l’autre moteur traite les sons dans le jeu.

Marble Maze doit également créer une voix mastérisée pour chaque moteur. N’oubliez pas qu’un moteur mastérisé associe les flux audio en un seul flux et envoie ce flux au matériel audio. Le flux de musique de fond, une voix source, envoie les données à une voix mastérisée et à deux voix prémixées. Les voix prémixées réalisent l’effet de réverbération.

Media Foundation est une bibliothèque multimédia qui prend en charge de nombreux formats audio et vidéo. XAudio2 et Media Foundation se complètent. Marble Maze utilise Media Foundation pour charger les éléments audio à partir de fichiers et utilise XAudio2 pour lire l’audio. Vous n’avez pas besoin d’utiliser Media Foundation pour charger les éléments audio. Si vous disposez déjà d’un mécanisme de chargement d’éléments audio qui fonctionne dans les applications de la plateforme Windows universelle (UWP), vous pouvez l’utiliser.

Pour plus d’informations sur XAudio2, voir le [Guide de programmation](https://msdn.microsoft.com/library/windows/desktop/ee415737). Pour plus d’informations sur Media Foundation, voir [Microsoft Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197).

## Initialisation des ressources audio


Marble Mazes utilise un fichier Windows Media Audio (.wma) pour la musique de fond et des fichiers WAV (.wav) pour les sons du jeu. Ces formats sont pris en charge par Media Foundation. Même si le format de fichier .wav est pris en charge en mode natif par XAudio2, un jeu doit analyser le format de fichier manuellement pour remplir les structures de données XAudio2 appropriées. Marble Maze utilise Media Foundation pour traiter plus facilement les fichiers .wav. Pour obtenir une liste complète des formats multimédias pris en charge par Media Foundation, voir [Formats multimédias pris en charge dans Media Foundation](https://msdn.microsoft.com/library/windows/desktop/dd757927). Marble Maze n’utilise pas des formats audio distincts au moment de la conception et de l’exécution. Il n’utilise pas non plus la prise en charge de la compression ADPCM XAudio2. Pour plus d’informations sur la compression ADPCM dans XAudio2, voir [Vue d’ensemble d’ADPCM](https://msdn.microsoft.com/library/windows/desktop/ee415711).

La méthode **Audio::CreateResources**, qui est appelée à partir de **MarbleMaze::CreateDeviceIndependentResources**, charge les flux audio à partir d’un fichier, initialise les objets du moteur XAudio2 et crée les voix sources, prémixées et mastérisées.

###  Création des moteurs XAudio2

N’oubliez pas que Marble Maze crée un objet [**IXAudio2**](https://msdn.microsoft.com/library/windows/desktop/ee415908) pour représenter chaque moteur audio qu’il utilise. Pour créer un moteur audio, appelez la fonction [**XAudio2Create**](https://msdn.microsoft.com/library/windows/desktop/ee419212). L’exemple suivant montre comment Marble Maze crée le moteur audio qui traite la musique de fond.

```cpp
DX::ThrowIfFailed(
    XAudio2Create(&m_musicEngine)
    );
```

Marble Maze procède de la même manière pour créer le moteur audio qui lit les sons dans le jeu.

L’utilisation de l’interface [**IXAudio2**](https://msdn.microsoft.com/library/windows/desktop/ee415908) dans une application pour UWP diffère de son utilisation dans une application de bureau de deux façons. En premier lieu, vous ne devez pas appeler **CoInitializeEx** avant d’appeler [**XAudio2Create**](https://msdn.microsoft.com/library/windows/desktop/ee419212). De plus, **IXAudio2** ne prend plus en charge l’énumération de périphériques. Pour plus d’informations sur la façon d’énumérer des périphériques audio, voir [Énumération de périphériques](https://msdn.microsoft.com/library/windows/apps/hh464977).

###  Création des voix mastérisées

L’exemple suivant montre comment la méthode **Audio::CreateResources** crée la voix mastérisée pour la musique de fond. L’appel à [**IXAudio2::CreateMasteringVoice**](https://msdn.microsoft.com/library/windows/desktop/hh405048) spécifie deux canaux d’entrée. Cela simplifie la logique de l’effet de réverbération. La spécification **XAUDIO2\_DEFAULT\_SAMPLERATE** indique au moteur audio d’utiliser le taux d’échantillonnage spécifié dans l’onglet Sons du Panneau de configuration. Dans cet exemple, **m\_musicMasteringVoice** est un objet [**IXAudio2MasteringVoice**](https://msdn.microsoft.com/library/windows/desktop/ee415912).

```cpp
// This sample plays the equivalent of background music, which we tag on the  
// mastering voice as AudioCategory_GameMedia. In ordinary usage, if we were  
// playing the music track with no effects, we could route it entirely through 
// Media Foundation. Here, we are using XAudio2 to apply a reverb effect to the 
// music, so we use Media Foundation to decode the data then we feed it through 
// the XAudio2 pipeline as a separate Mastering Voice, so that we can tag it 
// as Game Media. We default the mastering voice to 2 channels to simplify  
// the reverb logic.
DX::ThrowIfFailed(
    m_musicEngine->CreateMasteringVoice(
        &m_musicMasteringVoice, 
        2, 
        48000, 
        0, 
        nullptr, 
        nullptr, 
        AudioCategory_GameMedia
        )
);
```

La méthode **Audio::CreateResources** procède de la même façon pour créer la voix mastérisée pour les sons du jeu, mais elle indique **AudioCategory\_GameEffects** pour le paramètre *StreamCategory*, qui est la valeur par défaut. Marble Maze spécifie **AudioCategory_GameMedia** pour la musique de fond afin que les utilisateurs puissent écouter une musique d’une autre application pendant qu’ils jouent. Quand une application de musique est en cours d’exécution, Windows désactive le son des voix créées par l’option **AudioCategory_GameMedia**. L’utilisateur entend toujours les sons du jeu, car ils sont créés par l’option **AudioCategory_GameEffects**. Pour plus d’informations sur les catégories audio, voir la rubrique Énumération [**AUDIO\_STREAM\_CATEGORY**](https://msdn.microsoft.com/library/windows/desktop/hh404178).

###  Création de l’effet de réverbération

Pour chaque voix, vous pouvez utiliser XAudio2 pour créer des séquences d’effets qui traitent l’audio. Ces séquences sont appelées « chaînes d’effets ». Utilisez les chaînes d’effets quand vous voulez appliquer un ou plusieurs effets à une voix. Les chaînes d’effets peuvent être destructrices. Autrement dit, chaque effet de la chaîne peut remplacer la mémoire tampon audio. Cette propriété est importante, car XAudio2 ne garantit pas l’initialisation des mémoires tampons de sortie en mode silence. Les objets d’effets sont représentés dans XAudio2 par des objets de traitement de l’audio entre plateformes (XAPO). Pour plus d’informations sur les objets XAPO, voir la rubrique présentant une [Vue d’ensemble des objets XAPO](o:microsoft.directx_sdk.xapo.audio_overview_xapo).

Quand vous créez une chaîne d’effets, procédez ainsi :

1.  Créez l’objet d’effet.
2.  Remplissez une structure [**XAUDIO2\_EFFECT\_DESCRIPTOR**](https://msdn.microsoft.com/library/windows/desktop/ee419236) avec des données d’effets.
3.  Remplissez une structure [**XAUDIO2\_EFFECT\_CHAIN**](https://msdn.microsoft.com/library/windows/desktop/ee419235) avec des données.
4.  Appliquez la chaîne d’effets à une voix.
5.  Remplissez une structure de paramètre d’effet et appliquez-la à l’effet.
6.  Désactivez ou activez l’effet en fonction des besoins.

La classe **Audio** définit la méthode **CreateReverb** pour créer la chaîne d’effets qui implémente la réverbération. Cette méthode appelle la fonction [**XAudio2CreateReverb**](https://msdn.microsoft.com/library/windows/desktop/ee419213) pour créer un objet [**IXAudio2SubmixVoice**](https://msdn.microsoft.com/library/windows/desktop/ee415915), qui sert de voix prémixée pour l’effet de réverbération.

```cpp
DX::ThrowIfFailed(
    XAudio2CreateReverb(&soundEffectXAPO)
    );
```

La structure [**XAUDIO2\_EFFECT\_DESCRIPTOR**](https://msdn.microsoft.com/library/windows/desktop/ee419236) contient des informations sur un objet XAPO à utiliser dans une chaîne d’effets, par exemple le nombre cible de canaux de sortie. La méthode **Audio::CreateReverb** crée un objet **XAUDIO2\_EFFECT\_DESCRIPTOR** qui est défini à l’état désactivé, utilise deux canaux de sortie et fait référence à l’objet [**IXAudio2SubmixVoice**](https://msdn.microsoft.com/library/windows/desktop/ee415915) pour l’effet de réverbération. L’objet **XAUDIO2\_EFFECT\_DESCRIPTOR** démarre en mode désactivé, car le jeu doit définir des paramètres avant que l’effet commence à modifier les sons du jeu. Marble Maze utilise deux canaux de sortie pour simplifier la logique de l’effet de réverbération.

```cpp
soundEffectdescriptor.InitialState = false;
soundEffectdescriptor.OutputChannels = 2;
soundEffectdescriptor.pEffect = soundEffectXAPO.Get();
```

Si votre chaîne d’effets comporte plusieurs effets, chaque effet nécessite un objet. La structure [**XAUDIO2\_EFFECT\_CHAIN**](https://msdn.microsoft.com/library/windows/desktop/ee419235) contient le tableau d’objets [**XAUDIO2\_EFFECT\_DESCRIPTOR**](https://msdn.microsoft.com/library/windows/desktop/ee419236) qui participent à l’effet. L’exemple suivant montre comment la méthode **Audio::CreateReverb** spécifie l’effet permettant d’implémenter la réverbération.

```cpp
soundEffectChain.EffectCount = 1;
soundEffectChain.pEffectDescriptors = &soundEffectdescriptor;
```

La méthode **Audio::CreateReverb** appelle la méthode [**IXAudio2::CreateSubmixVoice**](https://msdn.microsoft.com/library/windows/desktop/ee418608) pour créer la voix prémixée pour l’effet. Elle spécifie l’objet [**XAUDIO2\_EFFECT\_CHAIN**](https://msdn.microsoft.com/library/windows/desktop/ee419235) pour le paramètre *pEffectChain* afin d’associer la chaîne d’effets à la voix. Marble Maze spécifie également deux canaux de sortie et un taux d’échantillonnage de 48 kilohertz. Nous avons choisi ce taux d’échantillonnage car il représente un bon équilibre entre la qualité audio et la quantité de traitement d’UC nécessaire. Un taux d’échantillonnage supérieur aurait requis davantage de traitement de l’UC sans amélioration notable de la qualité.

```cpp
DX::ThrowIfFailed(
    engine->CreateSubmixVoice(newSubmix, 2, 48000, 0, 0, nullptr, &soundEffectChain)
    );
```

> **Conseil**Si vous voulez attacher une chaîne d’effets existante à une voix prémixée existante, ou remplacer la chaîne d’effets actuelle, utilisez la méthode [**IXAudio2Voice::SetEffectChain**](https://msdn.microsoft.com/library/windows/desktop/ee418594).

 

La méthode [**Audio::XAudio2CreateReverb**](https://msdn.microsoft.com/library/windows/desktop/ee419213) appelle [**IXAudio2Voice::SetEffectParameters**](https://msdn.microsoft.com/library/windows/desktop/ee418595) pour définir des paramètres supplémentaires associés à l’effet. Cette méthode utilise une structure de paramètre spécifique à l’effet. Un objet [**XAUDIO2FX\_REVERB\_PARAMETERS**](https://msdn.microsoft.com/library/windows/desktop/ee419224), qui contient les paramètres d’effet de la réverbération, est initialisé dans la méthode **Audio::Initialize**, car chaque effet de réverbération partage les mêmes paramètres. L’exemple suivant montre comment la méthode **Audio::Initialize** initialise les paramètres de réverbération en champ proche.

```cpp
m_reverbParametersSmall.ReflectionsDelay = XAUDIO2FX_REVERB_DEFAULT_REFLECTIONS_DELAY;
m_reverbParametersSmall.ReverbDelay = XAUDIO2FX_REVERB_DEFAULT_REVERB_DELAY;
m_reverbParametersSmall.RearDelay = XAUDIO2FX_REVERB_DEFAULT_REAR_DELAY;
m_reverbParametersSmall.PositionLeft = XAUDIO2FX_REVERB_DEFAULT_POSITION;
m_reverbParametersSmall.PositionRight = XAUDIO2FX_REVERB_DEFAULT_POSITION;
m_reverbParametersSmall.PositionMatrixLeft = XAUDIO2FX_REVERB_DEFAULT_POSITION_MATRIX;
m_reverbParametersSmall.PositionMatrixRight = XAUDIO2FX_REVERB_DEFAULT_POSITION_MATRIX;
m_reverbParametersSmall.EarlyDiffusion = 4;
m_reverbParametersSmall.LateDiffusion = 15;
m_reverbParametersSmall.LowEQGain = XAUDIO2FX_REVERB_DEFAULT_LOW_EQ_GAIN;
m_reverbParametersSmall.LowEQCutoff = XAUDIO2FX_REVERB_DEFAULT_LOW_EQ_CUTOFF;
m_reverbParametersSmall.HighEQGain = XAUDIO2FX_REVERB_DEFAULT_HIGH_EQ_GAIN;
m_reverbParametersSmall.HighEQCutoff = XAUDIO2FX_REVERB_DEFAULT_HIGH_EQ_CUTOFF;
m_reverbParametersSmall.RoomFilterFreq = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_FREQ;
m_reverbParametersSmall.RoomFilterMain = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_MAIN;
m_reverbParametersSmall.RoomFilterHF = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_HF;
m_reverbParametersSmall.ReflectionsGain = XAUDIO2FX_REVERB_DEFAULT_REFLECTIONS_GAIN;
m_reverbParametersSmall.ReverbGain = XAUDIO2FX_REVERB_DEFAULT_REVERB_GAIN;
m_reverbParametersSmall.DecayTime = XAUDIO2FX_REVERB_DEFAULT_DECAY_TIME;
m_reverbParametersSmall.Density = XAUDIO2FX_REVERB_DEFAULT_DENSITY;
m_reverbParametersSmall.RoomSize = XAUDIO2FX_REVERB_DEFAULT_ROOM_SIZE;
m_reverbParametersSmall.WetDryMix = XAUDIO2FX_REVERB_DEFAULT_WET_DRY_MIX;
m_reverbParametersSmall.DisableLateField = TRUE;
```

Cet exemple utilise les valeurs par défaut pour la plupart des paramètres de réverbération, mais il affecte à **DisableLateField** la valeur TRUE pour spécifier la réverbération en champ proche, à **EarlyDiffusion** la valeur 4 pour simuler des surfaces proches planes et à **LateDiffusion** la valeur 15 pour simuler des surfaces éloignées très diffuses. Les surfaces proches planes permettent aux échos de vous atteindre plus rapidement et plus fort. Les surfaces éloignées diffuses permettent aux échos d’être plus silencieux et de vous atteindre plus lentement. Vous pouvez essayer différentes valeurs de réverbération pour obtenir l’effet souhaité dans votre jeu ou utiliser la fonction **ReverbConvertI3DL2ToNative** pour utiliser les paramètres I3DL2 (Interactive 3D Audio Rendering Guidelines Level 2.0) standard.

L’exemple suivant montre comment **Audio::CreateReverb** définit les paramètres de réverbération. Le paramètre parameters est un objet [**XAUDIO2FX\_REVERB\_PARAMETERS**](https://msdn.microsoft.com/library/windows/desktop/ee419224).

```cpp
DX::ThrowIfFailed(
    (*newSubmix)->SetEffectParameters(0, parameters, sizeof(m_reverbParametersSmall))
    );
```

La méthode **Audio::CreateReverb** se termine par l’activation de l’effet si l’indicateur **enableEffect** est défini et par la définition de son volume et sa matrice de sortie. Cette partie définit le volume sur complet (1.0), puis définit la matrice de volume en mode silence pour les entrées droite et gauche et les haut-parleurs de sortie droit et gauche. Nous procédons ainsi car un autre code se fond entre les deux réverbérations (en simulant la transition entre le fait d’être près d’un mur et celui d’être dans une grande salle) ou désactive le son des deux réverbérations si nécessaire. Quand le chemin de réverbération est ensuite restauré, le jeu définit une matrice {1.0f, 0.0f, 0.0f, 1.0f} pour acheminer la sortie de réverbération gauche vers l’entrée gauche de la voix mastérisée et la sortie de réverbération droite vers l’entrée droite de la voix mastérisée.

```cpp
if (enableEffect)
{
    DX::ThrowIfFailed(
        (*newSubmix)->EnableEffect(0)
        );    
}

DX::ThrowIfFailed(
    (*newSubmix)->SetVolume (1.0f)
    );

float outputMatrix[4] = {0, 0, 0, 0};
DX::ThrowIfFailed(
    (*newSubmix)->SetOutputMatrix(masteringVoice, 2, 2, outputMatrix)
    );
```

Marble Maze appelle la méthode **CreateReverb** quatre fois : deux fois pour la musique de fond et deux fois pour les sons du jeu. L’exemple suivant montre comment Marble Maze appelle la méthode **CreateReverb** pour la musique de fond.

```cpp
CreateReverb(
    m_musicEngine, 
    m_musicMasteringVoice, 
    &m_reverbParametersSmall, 
    &m_musicReverbVoiceSmallRoom, 
    true
    );
CreateReverb(
    m_musicEngine, 
    m_musicMasteringVoice, 
    &m_reverbParametersLarge, 
    &m_musicReverbVoiceLargeRoom, 
    true
    );
```

Pour obtenir la liste des sources d’effets possibles utilisables avec XAudio2, voir [Effets audio XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415756).

### Chargement des données audio à partir d’un fichier

Marble Maze définit la classe **MediaStreamer**, qui utilise Media Foundation pour charger les ressources audio à partir d’un fichier. Marble Maze utilise un objet **MediaStreamer** pour charger chaque fichier audio.

Marble Maze appelle la méthode **MediaStreamer::Initialize** pour initialiser chaque flux audio. Voici comment la méthode **Audio::CreateResources** appelle **MediaStreamer::Initialize** pour initialiser le flux audio pour la musique de fond :

```cpp
// Media Foundation is a convenient way to get both file I/O and format decode for 
// audio assets. You can replace the streamer in this sample with your own file I/O 
// and decode routines.
m_musicStreamer.Initialize(L"Media\\Audio\\background.wma");
```

La méthode **MediaStreamer::Initialize** commence par appeler la fonction [**MFStartup**](https://msdn.microsoft.com/library/windows/desktop/ms702238) pour initialiser Media Foundation.

```cpp
DX::ThrowIfFailed(
    MFStartup(MF_VERSION)
    );
```

**MediaStreamer::Initialize** appelle ensuite [**MFCreateSourceReaderFromURL**](https://msdn.microsoft.com/library/windows/desktop/dd388110) pour créer un objet [**IMFSourceReader**](https://msdn.microsoft.com/library/windows/desktop/dd374655). Un objet **IMFSourceReader** lit les données multimédias du fichier spécifié par l’URL.

```cpp
DX::ThrowIfFailed(
    MFCreateSourceReaderFromURL(url, nullptr, &m_reader)
    );
```

La méthode **MediaStreamer::Initialize** crée ensuite un objet [**IMFMediaType**](https://msdn.microsoft.com/library/windows/desktop/ms704850) pour décrire le format du flux audio. Un format audio a deux types : un type principal et un sous-type. Le type principal définit le format général des données multimédias, par exemple vidéo, audio, script, etc. Le sous-type définit le format, par exemple PCM, ADPCM ou WMA. La méthode **MediaStreamer::Initialize** utilise la méthode [**IMFMediaType::SetGUID**](https://msdn.microsoft.com/library/windows/desktop/bb970530) pour spécifier audio (**MFMediaType\_Audio**) comme type principal et audio PCM non compressé (**MFAudioFormat\_PCM**) comme type secondaire. La méthode [**IMFSourceReader::SetCurrentMediaType**](https://msdn.microsoft.com/library/windows/desktop/bb970432) associe le type multimédia au lecteur de flux.

```cpp
// Set the decoded output format as PCM. 
// XAudio2 on Windows can process PCM and ADPCM-encoded buffers. 
// When this sample uses Media Foundation, it always decodes into PCM.

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
    m_reader->SetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, 0, mediaType.Get())
    );
```

La méthode **MediaStreamer::Initialize** obtient alors le format multimédia de sortie complet de Media Foundation et appelle la fonction [**MFCreateWaveFormatExFromMFMediaType**](https://msdn.microsoft.com/library/windows/desktop/ms702177) pour convertir le type multimédia audio Media Foundation en structure [**WAVEFORMATEX**](https://msdn.microsoft.com/library/windows/hardware/ff538799). La structure **WAVEFORMATEX** définit le format des données audio .wav. Marble Maze utilise cette structure pour créer les voix sources et appliquer le filtre passe-bas au son de roulement de la bille.

```cpp
// Get the complete WAVEFORMAT from the Media Type.
DX::ThrowIfFailed(
    m_reader->GetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, &outputMediaType)
    );

uint32 formatSize = 0;
WAVEFORMATEX* waveFormat;
DX::ThrowIfFailed(
    MFCreateWaveFormatExFromMFMediaType(outputMediaType.Get(), &waveFormat, &formatSize)
    );
CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
CoTaskMemFree(waveFormat);
```

> **Important** La fonction [**MFCreateWaveFormatExFromMFMediaType**](https://msdn.microsoft.com/library/windows/desktop/ms702177) utilise **CoTaskMemAlloc** pour allouer l’objet [**WAVEFORMATEX**](https://msdn.microsoft.com/library/windows/hardware/ff538799). Par conséquent, veillez à appeler **CoTaskMemFree** quand vous avez terminé d’utiliser cet objet.

 

La méthode **MediaStreamer::Initialize** se termine par le calcul de la longueur du flux, m\_*maxStreamLengthInBytes*, en octets. Pour ce faire, elle appelle la méthode [**IMFSourceReader::IMFSourceReader::GetPresentationAttribute**](https://msdn.microsoft.com/library/windows/desktop/dd374662) pour obtenir la durée du flux audio en unités de 100 nanosecondes, convertit la durée en sections, puis multiplie par le taux de transfert de données moyen en octets par seconde. Marble Maze utilise ensuite cette valeur pour allouer la mémoire tampon qui contient chaque son du jeu.

```cpp
// Get the total length of the stream, in bytes.
PROPVARIANT var;
DX::ThrowIfFailed(
    m_reader->GetPresentationAttribute(MF_SOURCE_READER_MEDIASOURCE, MF_PD_DURATION, &var)
    );
LONGLONG duration = var.uhVal.QuadPart;
// The duration is in 100ns units; convert the value to seconds. 
double durationInSeconds = (duration / static_cast<double>(10000000)); 
m_maxStreamLengthInBytes = 
    static_cast<unsigned int>(durationInSeconds * m_waveFormat.nAvgBytesPerSec);

// Round up the buffer size to the nearest four bytes.
m_maxStreamLengthInBytes = (m_maxStreamLengthInBytes + 3) / 4 * 4;
```

### Création des voix sources

Marble Maze crée des voix sources XAudio2 pour lire chacun de ses sons et sa musique de jeu dans les voix sources. La classe **Audio** définit un objet [**IXAudio2SourceVoice**](https://msdn.microsoft.com/library/windows/desktop/ee415914) pour la musique de fond et un tableau d’objets **SoundEffectData** pour contenir les sons de jeu. La structure **SoundEffectData** contient l’objet **IXAudio2SourceVoice** pour un effet et définit également d’autres données liées à l’effet, telles que la mémoire tampon audio. Audio.h définit l’énumération **SoundEvent**. Marble Maze utilise cette énumération pour identifier chaque son du jeu. La classe Audio utilise également cette énumération pour indexer le tableau d’objets **SoundEffectData**.

```cpp
enum SoundEvent
{
    RollingEvent        = 0,
    FallingEvent        = 1,
    CollisionEvent      = 2,
    CheckpointEvent     = 3,
    MenuChangeEvent     = 4,
    MenuSelectedEvent   = 5,
    LastSoundEvent,
};
```

Le tableau suivant montre la relation entre chacune de ces valeurs, le fichier qui contient les données de son associées et une courte description de ce que chaque son représente. Les fichiers audio se trouvent dans le dossier \Media\Audio.

| Valeur SoundEvent  | Nom de fichier      | Description                                              |
|-------------------|----------------|----------------------------------------------------------|
| RollingEvent      | MarbleRoll.wav | Lu quand la bille roule.                              |
| FallingEvent      | MarbleFall.wav | Lu quand la bille sort (tombe) du labyrinthe.               |
| CollisionEvent    | MarbleHit.wav  | Lu quand la bille entre en collision avec le labyrinthe.           |
| CheckpointEvent   | Checkpoint.wav | Lu quand la bille passe par un point de contrôle.         |
| MenuChangeEvent   | MenuChange.wav | Lu quand l’utilisateur change d’élément de menu. |
| MenuSelectedEvent | MenuSelect.wav | Lu quand l’utilisateur sélectionne un élément de menu.           |

 

L’exemple suivant montre comment la méthode **Audio::CreateResources** crée la voix source pour la musique de fond. La méthode [**IXAudio2::CreateSourceVoice**](https://msdn.microsoft.com/library/windows/desktop/ee418607) crée et configure une voix source. Elle prend une structure [**WAVEFORMATEX**](https://msdn.microsoft.com/library/windows/hardware/ff538799) qui définit le format des mémoires tampons audio envoyées à la voix. Comme indiqué précédemment, Marble Maze utilise le format PCM. La structure [**XAUDIO2\_SEND\_DESCRIPTOR**](https://msdn.microsoft.com/library/windows/desktop/ee419244) définit la voix de destination cible à partir d’une autre voix et spécifie si un filtre doit être utilisé. Marble Maze appelle la fonction **Audio::SetSoundEffectFilter** pour utiliser les filtres afin de modifier le son de la bille quand elle roule. La structure [**XAUDIO2\_VOICE\_SENDS**](https://msdn.microsoft.com/library/windows/desktop/ee419246) définit l’ensemble des voix pour recevoir les données à partir d’une seule voix de sortie. Marble Maze envoie les données de la voix source à la voix mastérisée (pour la partie inchangée d’un son en cours de lecture) et aux deux voix prémixées qui implémentent la partie réverbérante d’un son en cours de lecture.

```cpp
XAUDIO2_SEND_DESCRIPTOR descriptors[3];
descriptors[0].pOutputVoice = m_soundEffectMasteringVoice;
descriptors[0].Flags = 0;
descriptors[1].pOutputVoice = m_soundEffectReverbVoiceSmallRoom;
descriptors[1].Flags = 0;
descriptors[2].pOutputVoice = m_soundEffectReverbVoiceLargeRoom;
descriptors[2].Flags = 0;
XAUDIO2_VOICE_SENDS sends = {0};
sends.SendCount = 3;
sends.pSends = descriptors;

// The rolling sound can have pitch shifting and a low-pass filter. 
if (sound == RollingEvent)
{
    DX::ThrowIfFailed(
        m_soundEffectEngine->CreateSourceVoice(
            &m_soundEffects[sound].m_soundEffectSourceVoice,
            &(soundEffectStream.GetOutputWaveFormatEx()),
            XAUDIO2_VOICE_USEFILTER,
            2.0f,
            &m_voiceContext,
            &sends)
        );
}
else
{
    DX::ThrowIfFailed(
        m_soundEffectEngine->CreateSourceVoice(
            &m_soundEffects[sound].m_soundEffectSourceVoice,
            &(soundEffectStream.GetOutputWaveFormatEx()),
            0,
            1.0f,
            &m_voiceContext,
            &sends)
        );
}
```

## Lecture de la musique de fond


Une voix source est créée dans l’état arrêté. Marble Maze démarre la musique de fond dans la boucle de jeu. Le premier appel à **MarbleMaze::Update** appelle **Audio::Start** pour démarrer la musique de fond.

```cpp
if (!m_audio.m_isAudioStarted)
{
    m_audio.Start();
}
```

La méthode **Audio::Start** appelle [**IXAudio2SourceVoice::Start**](https://msdn.microsoft.com/library/windows/desktop/ee418471) pour démarrer le traitement de la voix source pour la musique de fond.

```cpp
void Audio::Start()
{     
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    HRESULT hr = m_musicSourceVoice->Start(0);

    if SUCCEEDED(hr) {
        m_isAudioStarted = true;
    }
    else
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

La voix source passe ces données audio à l’étape suivante du graphique audio. Dans le cas de Marble Maze, l’étape suivante contient deux voix prémixées qui appliquent les deux effets de réverbération à l’audio. Une voix prémixée applique une réverbération proche à champ tardif. La seconde applique une réverbération lointaine à champ tardif. La quantité selon laquelle chaque voix prémixée contribue à la combinaison finale est déterminée par la taille et la forme de la salle. La réverbération en champ proche est plus importante quand la bille est près d’un mur ou dans une petite salle. La réverbération à champ tardif est plus importante quand la bille se trouve dans un grand espace. Cette technique produit un effet d’écho plus réaliste quand la bille se déplace dans le labyrinthe. Pour en savoir plus sur la façon dont Marble Maze implémente cet effet, voir **Audio::SetRoomSize** et **Physics::CalculateCurrentRoomSize** dans le code source de Marble Maze.

> **Remarque** Dans un jeu dans lequel la taille des pièces est à peu près la même, vous pouvez utiliser un modèle de réverbération de base. Par exemple, vous pouvez utiliser un paramètre de réverbération pour toutes les pièces ou vous pouvez créer un paramètre de réverbération prédéfini pour chaque pièce.

 

La méthode **Audio::CreateResources** utilise Media Foundation pour charger la musique de fond. Cependant, à ce stade, la voix source ne dispose pas de données audio à utiliser. De plus, comme la musique de fond passe en boucle, la voix source doit être régulièrement mise à jour avec les données pour que la musique puisse continuer à être jouée. Pour que la voix source dispose de ces données, la boucle du jeu met à jour les mémoires tampons audio à chaque image. La méthode **MarbleMaze::Render** appelle **Audio::Render** pour traiter la mémoire tampon audio de la musique de fond. **Audio::Render** définit un tableau de trois mémoires tampons audio, **m\_audioBuffers**. Chaque mémoire tampon contient 64 Ko (65 536 octets) de données. La boucle lit les données à partir de l’objet Media Foundation et écrit ces données dans la voix source jusqu’à ce que cette dernière ait trois mémoires tampons mises en file d’attente.

> **Attention** Bien que Marble Maze utilise une mémoire tampon de 64 Ko pour les données de musique, il est possible que vous ayez besoin d’une mémoire tampon plus petite ou plus grande. La taille dépend des exigences du jeu.

 

```cpp
void Audio::Render()
{
    if (m_engineExperiencedCriticalError)
    {
        m_engineExperiencedCriticalError = false;
        ReleaseResources();
        Initialize();
        CreateResources();
        Start();
        if (m_engineExperiencedCriticalError)
        {
            return;
        }
    }

    try
    {
        bool streamComplete;
        XAUDIO2_VOICE_STATE state;
        uint32 bufferLength;
        XAUDIO2_BUFFER buf = {0};

        // Use MediaStreamer to stream the buffers.
        m_musicSourceVoice->GetState(&state);
        while (state.BuffersQueued <= MAX_BUFFER_COUNT - 1)
        {
            streamComplete = m_musicStreamer.GetNextBuffer(
                m_audioBuffers[m_currentBuffer], 
                STREAMING_BUFFER_SIZE, 
                &bufferLength
                );

            if (bufferLength > 0)
            {
                buf.AudioBytes = bufferLength;
                buf.pAudioData = m_audioBuffers[m_currentBuffer];
                buf.Flags = (streamComplete) ? XAUDIO2_END_OF_STREAM : 0;
                buf.pContext = 0;
                DX::ThrowIfFailed(
                    m_musicSourceVoice->SubmitSourceBuffer(&buf)
                    );

                m_currentBuffer++;
                m_currentBuffer %= MAX_BUFFER_COUNT;
            }

            if (streamComplete)
            {
                // Loop the stream.
                m_musicStreamer.Restart();
                break;
            }

            m_musicSourceVoice->GetState(&state);
        }
    }
    catch(...)
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

La boucle gère également le moment où l’objet Media Foundation atteint la fin du flux. Dans ce cas, la méthode [**MediaStreamer::OnClockRestart**](https://msdn.microsoft.com/library/windows/desktop/ms697215) est appelée pour réinitialiser la position de la source audio.

```cpp
void MediaStreamer::Restart()
{
    if (m_reader == nullptr)
    {
        return;
    }

    PROPVARIANT var = {0};
    var.vt = VT_I8;

    DX::ThrowIfFailed(
        m_reader->SetCurrentPosition(GUID_NULL, var)
        );
}
```

Pour implémenter la boucle audio pour une seule mémoire tampon (ou pour un son complet entièrement chargé en mémoire), vous pouvez définir le champ **LoopCount** sur **XAUDIO2\_LOOP\_INFINITE** quand vous initialisez le son. Marble Maze utilise cette technique pour lire le son de la bille qui roule.

```cpp
if(sound == RollingEvent)
{
    m_soundEffects[sound].m_audioBuffer.LoopCount = XAUDIO2_LOOP_INFINITE;
}
```

Cependant, pour la musique de fond, Marble Maze gère les mémoires tampons directement, pour obtenir un meilleur contrôle sur la quantité de mémoire utilisée. Quand la taille de vos fichiers de musique est importante, vous pouvez diffuser les données de musique dans des mémoires tampons plus petites. Cela permet d’équilibrer la taille de la mémoire et la fréquence de traitement et de diffusion des données audio du jeu.

> **Conseil** Si la fréquence d’images de votre jeu est faible ou changeante, le traitement audio sur le thread principal peut créer des arrêts ou des sauts de l’audio, car le moteur audio ne dispose pas de suffisamment de données audio en mémoire tampon. Si votre jeu subit ce problème, traitez l’audio dans un thread séparé qui n’effectue pas de rendu. Cette approche est particulièrement utile sur les ordinateurs à plusieurs processeurs, car votre jeu peut utiliser des processeurs inactifs.

 

##  Réaction aux événements du jeu


La classe **MarbleMaze** fournit des méthodes telles que **PlaySoundEffect**, **IsSoundEffectStarted**, **StopSoundEffect**, **SetSoundEffectVolume**, **SetSoundEffectPitch** et **SetSoundEffectFilter** pour permettre au jeu de contrôler la lecture et l’arrêt des sons, ainsi que les propriétés sonores telles que le volume et le pas. Par exemple, si la bille tombe dans le labyrinthe, la méthode **MarbleMaze::Update** appelle la méthode **Audio::PlaySoundEffect** pour lire le son **FallingEvent**.

```cpp
m_audio.PlaySoundEffect(FallingEvent);
```

La méthode **Audio::PlaySoundEffect** appelle la méthode [**IXAudio2SourceVoice::Start**](https://msdn.microsoft.com/library/windows/desktop/ee418471) pour démarrer la lecture du son. Si la méthode **IXAudio2SourceVoice::Start** a déjà été appelée, elle n’est pas redémarrée. **Audio::PlaySoundEffect** exécute ensuite une logique personnalisée pour certains sons.

```cpp
void Audio::PlaySoundEffect(SoundEvent sound)
{
    XAUDIO2_BUFFER buf = {0};
    XAUDIO2_VOICE_STATE state = {0};

    if (m_engineExperiencedCriticalError) {
        // If there's an error, then we'll recreate the engine on the next  
        // render pass. 
        return;
    }

    SoundEffectData* soundEffect = &m_soundEffects[sound];
    HRESULT hr = soundEffect->m_soundEffectSourceVoice->Start();
    if FAILED(hr)
    {
        m_engineExperiencedCriticalError = true;
        return;
    }

    // For one-off voices, submit a new buffer if there's none queued up, 
    // and allow up to two collisions to be queued up. 
    if(sound != RollingEvent)
    {
        XAUDIO2_VOICE_STATE state = {0};
        soundEffect->m_soundEffectSourceVoice->GetState(&state, XAUDIO2_VOICE_NOSAMPLESPLAYED);
        if (state.BuffersQueued == 0)
        {
            soundEffect->m_soundEffectSourceVoice->SubmitSourceBuffer(&soundEffect->m_audioBuffer);
        }
        else if (state.BuffersQueued < 2 && sound == CollisionEvent)
        {
            soundEffect->m_soundEffectSourceVoice->SubmitSourceBuffer(&soundEffect->m_audioBuffer);
        }

        // For the menu clicks, we want to stop the voice and replay the click right away. 
        // Note that stopping and then flushing could cause a glitch due to the 
        // waveform not being at a zero-crossing, but due to the nature of the sound  
        // (fast and 'clicky'), we don't mind. 
        if (state.BuffersQueued > 0 && sound == MenuChangeEvent)
        {
            soundEffect->m_soundEffectSourceVoice->Stop();
            soundEffect->m_soundEffectSourceVoice->FlushSourceBuffers();
            soundEffect->m_soundEffectSourceVoice->SubmitSourceBuffer(&soundEffect->m_audioBuffer);
            soundEffect->m_soundEffectSourceVoice->Start();
        }
    }

    m_soundEffects[sound].m_soundEffectStarted = true;
}
```

Pour les sons autres que le roulement, la méthode **Audio::PlaySoundEffect** appelle [**IXAudio2SourceVoice::GetState**](https://msdn.microsoft.com/library/windows/desktop/hh405047) pour déterminer le nombre de mémoires tampons que la voix source lit. Elle appelle [**IXAudio2SourceVoice::SubmitSourceBuffer**](https://msdn.microsoft.com/library/windows/desktop/ee418473) pour ajouter les données audio du son à la file d’attente d’entrée de la voix si aucune mémoire tampon n’est active. La méthode **Audio::PlaySoundEffect** permet également au son de collision d’être lu deux fois successivement. Cela se produit, par exemple, quand la bille heurte un angle du labyrinthe.

Comme décrit précédemment, la classe Audio utilise l’indicateur **XAUDIO2\_LOOP\_INFINITE** quand elle initialise le son pour l’événement de roulement. Le son démarre la lecture en boucle la première fois que la méthode **Audio::PlaySoundEffect** est appelée pour cet événement. Pour simplifier la logique de lecture pour le son de roulement, Marble Maze désactive le son au lieu de l’arrêter. Quand la vitesse de la bille change, Marble Maze modifie le pas et le volume du son pour lui donner un effet plus réaliste. Le code suivant montre comment la méthode **MarbleMaze::Update** met à jour le pas et le volume de la bille quand sa vitesse change et comment elle désactive le son en affectant la valeur zéro à son volume quand la bille s’arrête.

```cpp
// Play the roll sound only if the marble is actually rolling. 
if (ci.isRollingOnFloor && volume > 0)
{
    if (!m_audio.IsSoundEffectStarted(RollingEvent))
    {
        m_audio.PlaySoundEffect(RollingEvent);
    }

    // Update the volume and pitch by the velocity.
    m_audio.SetSoundEffectVolume(RollingEvent, volume);
    m_audio.SetSoundEffectPitch(RollingEvent, pitch);

    // The rolling sound has at most 8000Hz sounds, so we linearly  
    // ramp up the low-pass filter the faster we go. 
    // We also reduce the Q-value of the filter, starting with a  
    // relatively broad cutoff and get progressively tighter.
    m_audio.SetSoundEffectFilter(
        RollingEvent, 
        600.0f + 8000.0f * volume,
        XAUDIO2_MAX_FILTER_ONEOVERQ - volume*volume
        );
}
else
{
    m_audio.SetSoundEffectVolume(RollingEvent, 0);
}
```

## Réaction aux événements d’interruption et de reprise


Le document sur la structure de l’application Marble Maze décrit comment Marble Maze prend en charge l’interruption et la reprise. Quand le jeu est interrompu, l’audio est mis en pause. Quand le jeu reprend, l’audio recommence là où il a été arrêté. Cela correspond à la meilleure pratique qui consiste à ne pas utiliser de ressources quand cela n’est pas nécessaire.

La méthode **Audio::SuspendAudio** est appelée quand le jeu est suspendu. Cette méthode appelle la méthode [**IXAudio2::StopEngine**](https://msdn.microsoft.com/library/windows/desktop/ee418628) pour arrêter l’ensemble du son. Même si **IXAudio2::StopEngine** arrête immédiatement toutes les sorties audio, elle conserve le graphique audio et ses paramètres d’effet (par exemple, l’effet de réverbération appliqué quand la bille rebondit).

```cpp
// Uses the IXAudio2::StopEngine method to stop all audio immediately.  
// It leaves the audio graph untouched, which preserves all effect parameters   
// and effect histories (like reverb effects) voice states, pending buffers,  
// cursor positions and so on. 
// When the engines are restarted, the resulting audio will sound as if it had  
// never been stopped except for the period of silence. 
void Audio::SuspendAudio()
{
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    if (m_isAudioStarted)
    {
        m_musicEngine->StopEngine();
        m_soundEffectEngine->StopEngine();
    }
    m_isAudioStarted = false;
}
```

La méthode **Audio::ResumeAudio** est appelée quand le jeu reprend. Cette méthode utilise la méthode [**IXAudio2::StartEngine**](https://msdn.microsoft.com/library/windows/desktop/ee418626) pour redémarrer le son. Dans la mesure où l’appel à [**IXAudio2::StopEngine**](https://msdn.microsoft.com/library/windows/desktop/ee418628) conserve le graphique audio et ses paramètres d’effet, la sortie audio reprend là où elle a été arrêtée.

```cpp
// Restarts the audio streams. A call to this method must match a previous call  
// to SuspendAudio. This method causes audio to continue where it left off. 
// If there is a problem with the restart, the m_engineExperiencedCriticalError  
// flag is set. The next call to Render will recreate all the resources and  
// reset the audio pipeline. 
void Audio::ResumeAudio()
{
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    HRESULT hr = m_musicEngine->StartEngine();
    HRESULT hr2 = m_soundEffectEngine->StartEngine();

    if (FAILED(hr) || FAILED(hr2))
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

## Gestion des casques et des changements de périphériques


Marble Maze utilise des rappels de moteur pour gérer les pannes du moteur XAudio2, par exemple quand le périphérique audio change. La connexion ou la déconnexion d’un casque par l’utilisateur est une source de changement de périphérique. Nous recommandons d’implémenter le rappel de moteur qui gère les changements de périphériques. Dans le cas contraire, votre jeu ne lira plus les sons quand l’utilisateur connecte ou retire un casque, tant que le jeu n’est pas redémarré.

Audio.h définit la classe **AudioEngineCallbacks**. Cette classe implémente l’interface [**IXAudio2EngineCallback**](https://msdn.microsoft.com/library/windows/desktop/ee415910).

```cpp
class AudioEngineCallbacks: public IXAudio2EngineCallback
{
private: 
    Audio* m_audio;

public :
    AudioEngineCallbacks(){};
    void Initialize(Audio* audio);

    // Called by XAudio2 just before an audio processing pass begins.
    void _stdcall OnProcessingPassStart(){};

    // Called just after an audio processing pass ends.
    void  _stdcall OnProcessingPassEnd(){};

    // Called when a critical system error causes XAudio2
    // to be closed and restarted. The error code is given in Error.
    void  _stdcall OnCriticalError(HRESULT Error);
};
```

L’interface [**IXAudio2EngineCallback**](https://msdn.microsoft.com/library/windows/desktop/ee415910) permet à votre code d’être notifié quand des événements de traitement du son se produisent et quand le moteur rencontre une erreur critique. Pour s’inscrire pour les rappels, Marble Maze appelle la méthode [**IXAudio2::RegisterForCallbacks**](https://msdn.microsoft.com/library/windows/desktop/ee418620) une fois l’objet [**IXAudio2**](https://msdn.microsoft.com/library/windows/desktop/ee415908) créé pour le moteur de musique.

```cpp
m_musicEngineCallback.Initialize(this);
m_musicEngine->RegisterForCallbacks(&m_musicEngineCallback);
```

Marble Maze ne requiert pas de notification quand le traitement audio démarre ou se termine. Par conséquent, il implémente les méthodes [**IXAudio2EngineCallback::OnProcessingPassStart**](https://msdn.microsoft.com/library/windows/desktop/ee418463) et [**IXAudio2EngineCallback::OnProcessingPassEnd**](https://msdn.microsoft.com/library/windows/desktop/ee418462) pour qu’elles ne fassent rien. Pour la méthode [**IXAudio2EngineCallback::OnCriticalError**](https://msdn.microsoft.com/library/windows/desktop/ee418461), Marble Maze appelle la méthode **SetEngineExperiencedCriticalError**, qui définit l’indicateur **m\_engineExperiencedCriticalError**.

```cpp
// Called when a critical system error causes XAudio2 
// to be closed and restarted. The error code is given in Error. 
void  _stdcall AudioEngineCallbacks::OnCriticalError(HRESULT Error)
{
    m_audio->SetEngineExperiencedCriticalError();
}
```

```cpp
// This flag can be used to tell when the audio system 
// is experiencing critial errors.
// XAudio2 gives a critical error when the user unplugs
// the headphones and a new speaker configuration is generated.
void SetEngineExperiencedCriticalError()
{
    m_engineExperiencedCriticalError = true;
}
```

Quand une erreur critique se produit, le traitement audio s’arrête et tous les appels suivants à XAudio2 échouent. Pour résoudre ce problème, vous devez libérer l’instance XAudio2 et en créer une nouvelle. La méthode **Audio::Render**, qui est appelée à partir de la boucle de jeu à chaque image, vérifie d’abord l’indicateur **m\_engineExperiencedCriticalError**. Si cet indicateur est défini, elle l’efface, libère l’instance XAudio2 actuelle, initialise les ressources et démarre la musique de fond.

```cpp
if (m_engineExperiencedCriticalError)
{
    m_engineExperiencedCriticalError = false;
    ReleaseResources();
    Initialize();
    CreateResources();
    Start();
    if (m_engineExperiencedCriticalError)
    {
        return;
    }
}
```

Marble Maze utilise également l’indicateur **m_engineExperiencedCriticalError** pour éviter les appels à XAudio2 quand aucun périphérique audio n’est disponible. Par exemple, la méthode **MarbleMaze::Update** ne traite pas le son pour les événements de roulement ou de collision quand cet indicateur est défini. L’application essaie de réparer le moteur audio à chaque image, si nécessaire. Cependant, l’indicateur **m\_engineExperiencedCriticalError** peut toujours être défini si l’ordinateur n’a pas de périphérique audio ou si le casque est débranché et qu’aucun autre périphérique audio n’est disponible.

> **Attention** En règle générale, n’exécutez pas d’opérations bloquantes dans le corps d’un rappel de moteur. Cela peut entraîner des problèmes de performances. Marble Maze définit un indicateur dans le rappel **OnCriticalError** et gère l’erreur ultérieurement pendant la phase régulière de traitement du son. Pour plus d’informations sur les rappels XAudio2, voir [Rappels XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415745).

 

## Rubriques connexes


* [Ajout d’entrées et d’interactivité à l’exemple Marble Maze](adding-input-and-interactivity-to-the-marble-maze-sample.md)
* [Développement de Marble Maze, jeu pour UW en C++ et DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 






<!--HONumber=Mar16_HO1-->


