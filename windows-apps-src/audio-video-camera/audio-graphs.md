---
ms.assetid: CB924E17-C726-48E7-A445-364781F4CCA1
description: Ici est décrite l’utilisation des API dans l’espace Windows.Media.Audio pour créer des graphiques pour le routage audio, le mixage et le traitement audio.
title: Graphiques audio
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 75066b566fde3f25ea4feb2ed82358b106ffcf7c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359122"
---
# <a name="audio-graphs"></a>Graphiques audio



Cet article montre comment utiliser les API dans l’espace de noms [**Windows.Media.Audio**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio) pour créer des graphiques pour le routage audio, le mixage et les scénarios de traitement audio.

Un *graphique audio* est un ensemble de nœuds audio interconnectés par lequel passe le flux des données audio. 

- Les *nœuds d’entrée audio* transmettent au graphique les données en provenance des appareils d’entrée audio, des fichiers audio ou du code personnalisé. 

- L’audio traitée par le graphique se dirige vers les *nœuds de sortie audio*. L’audio peut être routée à la sortie du graphique vers des appareils de sortie audio, des fichiers audio ou du code personnalisé. 

- Les *nœuds prémixés* extraient l’audio à partir d’un ou plusieurs nœuds et la combine en une seule sortie pouvant être routée vers d’autres nœuds dans le graphique. 

Une fois tous les nœuds créés et les connexions entre eux configurées, vous pouvez démarrer le graphique audio. Le flux de données audio est alors transmis des nœuds d’entrée aux nœuds de sortie, par le biais des nœuds prémixés. Ce modèle facilite et accélère l’implémentation de scénarios, tels que l’enregistrement sur un fichier audio à partir du microphone de l’appareil, la lecture audio à partir d’un fichier vers le haut-parleur de l’appareil ou le mixage audio à partir de plusieurs sources.

Des scénarios supplémentaires sont disponibles en ajoutant des effets audio au graphique audio. Chaque nœud d’un graphique audio peut contenir plusieurs effets audio qui effectuent le traitement sur l’audio traversant le nœud. Plusieurs effets intégrés comme l’écho, l’égaliseur, la limitation et la réverbération peuvent être attachés à un nœud audio simplement avec quelques lignes de code. Vous pouvez aussi créer vos propres effets audio pour produire les mêmes effets intégrés.

> [!NOTE]
> L’[exemple AudioGraph UWP](https://go.microsoft.com/fwlink/?LinkId=619481) implémente le code décrit dans cette vue d’ensemble. Vous pouvez télécharger l’exemple pour voir le code en contexte ou pour vous en servir comme point de départ pour votre propre application.

## <a name="choosing-windows-runtime-audiograph-or-xaudio2"></a>Choix de Windows Runtime AudioGraph ou XAudio2

Les API de graphique audio Windows Runtime offrent des fonctionnalités qui peuvent également être implémentées à l’aide d’[API XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-apis-portal) COM. Les fonctionnalités de l’infrastructure de graphique audio suivantes diffèrent entre Windows Runtime et XAudio2.

Les API de graphique audio Windows Runtime :

-   Sont bien plus simples d’utilisation que XAudio2.
-   Peuvent être utilisées à partir de C# , en plus de la prise en charge pour C++.
-   Peuvent utiliser directement des fichiers audio, notamment les formats de fichier compressé. XAudio2 fonctionne uniquement sur les mémoires tampons audio et n’offre pas de fonctionnalités d’E/S fichier.
-   Peut utiliser le pipeline audio à faible latence dans Windows 10.
-   Prennent en charge le changement de point de terminaison automatique lorsque les paramètres de point de terminaison par défaut sont utilisés. Par exemple, si l’utilisateur bascule du haut-parleur de l’appareil à un casque, l’audio est automatiquement redirigée vers la nouvelle entrée.

## <a name="audiograph-class"></a>Classe AudioGraph

La classe [**AudioGraph**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioGraph) est le parent de tous les nœuds qui composent le graphique. Utilisez cet objet pour créer des instances de tous les types de nœud audio. Créez une instance de la classe **AudioGraph** en initialisant un objet [**AudioGraphSettings**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioGraphSettings) contenant des paramètres de configuration pour le graphique, puis en appelant [**AudioGraph.CreateAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.). Le [**CreateAudioGraphResult**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.CreateAudioGraphResult) renvoyé donne accès au graphique audio créé ou fournit une valeur d’erreur en cas d’échec de création du graphique audio.

[!code-cs[DeclareAudioGraph](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareAudioGraph)]

[!code-cs[InitAudioGraph](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetInitAudioGraph)]

-   Tous les types de nœud audio sont créés à l’aide de la créer\* méthodes de la **AudioGraph** classe.
-   La méthode [**AudioGraph.Start**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.start) entraîne le graphique audio à traiter les données audio. La méthode [**AudioGraph.Stop**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.stop) arrête le traitement audio. Chaque nœud du graphique peut être démarré et arrêté de façon indépendante lorsque le graphique est en cours d’exécution, mais aucun nœud n’est actif lorsque le graphique est arrêté. [**ResetAllNodes** ](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.resetallnodes) provoque tous les nœuds dans le graphique pour supprimer toutes les données actuellement dans les tampons audio.
-   L’événement [**QuantumStarted**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.quantumstarted) se produit lorsque le graphique commence le traitement d’un nouveau quantum de données audio. L’événement [**QuantumProcessed**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.quantumprocessed) se produit lorsque le traitement d’un quantum est terminé.

-   La seule propriété [**AudioGraphSettings**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioGraphSettings) requise est [**AudioRenderCategory**](https://docs.microsoft.com/uwp/api/Windows.Media.Render.AudioRenderCategory). Cette valeur permet au système d’optimiser le pipeline audio pour la catégorie spécifiée.
-   La taille du quantum du graphique audio détermine le nombre d’échantillons traités en une seule fois. Par défaut, la taille du quantum est de 10 ms, basée sur le taux d’échantillonnage par défaut. Si vous spécifiez une taille de quantum personnalisée en définissant la propriété [**DesiredSamplesPerQuantum**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.desiredsamplesperquantum), vous devez également définir la propriété [**QuantumSizeSelectionMode**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.quantumsizeselectionmode) sur **ClosestToDesired**, ou la valeur indiquée sera ignorée. Si cette valeur est utilisée, le système choisit une taille de quantum aussi proche que possible de celle que vous avez spécifiée. Pour déterminer la taille réelle du quantum, vérifiez la propriété [**SamplesPerQuantum**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.samplesperquantum) du **AudioGraph** après sa création.
-   Si vous prévoyez seulement d’utiliser le graphique audio avec des fichiers et que vous n’envisagez pas de sortie vers un appareil audio, il est recommandé d’utiliser la taille de quantum par défaut en ne définissant pas la propriété [**DesiredSamplesPerQuantum**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.desiredsamplesperquantum).
-   La propriété [**DesiredRenderDeviceAudioProcessing**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.desiredrenderdeviceaudioprocessing) détermine la quantité de traitement que l’appareil de rendu principal effectue sur la sortie du graphique audio. Le paramètre **Default** permet au système d’utiliser le traitement audio par défaut pour la catégorie de rendu audio spécifiée. Ce traitement peut considérablement améliorer le son sur certains appareils, en particulier les appareils mobiles équipés de petits haut-parleurs. Le paramètre **Raw** peut améliorer les performances en réduisant la quantité de traitement du signal, mais peut entraîner une qualité audio médiocre sur certains appareils.
-   Si la [**QuantumSizeSelectionMode**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.quantumsizeselectionmode) est définie sur **LowestLatency**, le graphique audio utilise automatiquement **Raw** pour [**DesiredRenderDeviceAudioProcessing**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.desiredrenderdeviceaudioprocessing).
- À partir de Windows 10, version 1803, vous pouvez utiliser la propriété [**AudioGraphSettings.MaxPlaybackSpeedFactor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.maxplaybackspeedfactor) pour définir une valeur maximale utilisée pour les propriétés [**AudioFileInputNode.PlaybackSpeedFactor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.playbackspeedfactor), [**AudioFrameInputNode.PlaybackSpeedFactor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeinputnode.playbackspeedfactor) et [**MediaSourceInputNode.PlaybackSpeedFactor**](https://docs.microsoft.com/uwp/api/windows.media.audio.mediasourceinputnode.playbackspeedfactor). Lorsqu’un graphique audio prend en charge un facteur de vitesse de lecture supérieur à 1, le système doit allouer plus de mémoire afin de maintenir une mémoire tampon suffisante de données audio. Pour cette raison, la définition de **MaxPlaybackSpeedFactor** sur la valeur la plus basse requise par votre application permet de réduire la consommation de mémoire de votre application. Si votre application lit uniquement le contenu à la vitesse normale, il est recommandé de définir MaxPlaybackSpeedFactor sur 1.
-   La [**EncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.encodingproperties) détermine le format audio utilisé par le graphique. Seuls les formats flottants 32 bits sont pris en charge.
-   La [**PrimaryRenderDevice**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.primaryrenderdevice) configure l’appareil de rendu principal pour le graphique audio. Si vous ne définissez pas cette propriété, l’appareil système par défaut est utilisé. L’appareil de rendu principal est utilisé pour calculer les tailles de quantum pour les autres nœuds du graphique. S’il n’existe aucun appareil de rendu audio sur le système, la création de graphique audio échouera.

Vous pouvez laisser le graphique audio utiliser l’appareil de rendu audio par défaut ou utiliser la classe [**Windows.Devices.Enumeration.DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) pour obtenir la liste des appareils disponibles sur le système en appelant [**FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) et en utilisant le sélecteur d’appareil de rendu audio renvoyé par [**Windows.Media.Devices.MediaDevice.GetAudioRenderSelector**](https://docs.microsoft.com/uwp/api/windows.media.devices.mediadevice.getaudiorenderselector). Vous pouvez choisir l’un des objets **DeviceInformation** renvoyés par programme ou afficher l’interface utilisateur pour permettre à l’utilisateur de sélectionner un appareil et de l’utiliser pour définir la propriété [**PrimaryRenderDevice**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.primaryrenderdevice).

[!code-cs[EnumerateAudioRenderDevices](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetEnumerateAudioRenderDevices)]

##  <a name="device-input-node"></a>Nœud d’entrée d’appareil

Un nœud d’entrée d’appareil transmet l’audio au graphique à partir d’un appareil de capture audio connecté au système, par exemple un microphone. Créez un objet [**DeviceInputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioDeviceInputNode) qui utilise l’appareil de capture audio par défaut du système en appelant [**CreateDeviceInputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createdeviceinputnodeasync). Indiquez une [**AudioRenderCategory**](https://docs.microsoft.com/uwp/api/Windows.Media.Render.AudioRenderCategory) pour permettre au système d’optimiser le pipeline audio pour la catégorie spécifiée.

[!code-cs[DeclareDeviceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareDeviceInputNode)]


[!code-cs[CreateDeviceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateDeviceInputNode)]

Si vous souhaitez spécifier un périphérique de capture audio spécifique pour le nœud de périphérique d’entrée, vous pouvez utiliser la [ **Windows.Devices.Enumeration.DeviceInformation** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) classe pour obtenir une liste d’audio disponibles du système périphériques de capture en appelant [ **FindAllAsync** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) et en passant dans le sélecteur d’appareils rendu audio retourné par [  **Windows.Media.Devices.MediaDevice.GetAudioCaptureSelector**](https://docs.microsoft.com/uwp/api/windows.media.devices.mediadevice.getaudiocaptureselector). Vous pouvez choisir l’un des objets **DeviceInformation** renvoyés par programme ou afficher l’interface utilisateur pour permettre à l’utilisateur de sélectionner un appareil pour le transmettre à [**CreateDeviceInputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createdeviceinputnodeasync).

[!code-cs[EnumerateAudioCaptureDevices](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetEnumerateAudioCaptureDevices)]

##  <a name="device-output-node"></a>Nœud de sortie d’appareil

Un nœud de sortie d’appareil transmet l’audio du graphique vers l’appareil de rendu audio, par exemple des haut-parleurs ou un casque. Créez un [**DeviceOutputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode) en appelant [**CreateDeviceOutputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createdeviceoutputnodeasync). Le nœud de sortie utilise la [**PrimaryRenderDevice**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.primaryrenderdevice) du graphique audio.

[!code-cs[DeclareDeviceOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareDeviceOutputNode)]

[!code-cs[CreateDeviceOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateDeviceOutputNode)]

##  <a name="file-input-node"></a>Nœud d’entrée de fichier

Un nœud d’entrée de fichier permet de transmettre les données d’un fichier audio au graphique. Créez un [**AudioFileInputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFileInputNode) en appelant [**CreateFileInputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createfileinputnodeasync).

[!code-cs[DeclareFileInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFileInputNode)]


[!code-cs[CreateFileInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFileInputNode)]

-   Les nœuds d’entrée de fichier prennent en charge les formats suivants : mp3, wav, wma et m4a.
-   Définissez la propriété [**StartTime**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.starttime) pour spécifier le décalage dans le fichier où la lecture doit commencer. Si cette propriété est null, le début du fichier est utilisé. Définissez la propriété [**EndTime**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.endtime) pour spécifier le décalage dans le fichier où la lecture doit se terminer. Si cette propriété est null, la fin du fichier est utilisée. L’heure de début doit être antérieure à l’heure de fin, et l’heure de fin doit être inférieure ou égale à la durée du fichier audio, qui peut être déterminée en vérifiant la valeur de propriété [**Duration**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.duration).
-   Recherchez une position dans le fichier audio en appelant [**Seek**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.seek) et en spécifiant le décalage dans le fichier vers lequel la position de lecture doit être déplacée. La valeur spécifiée doit être comprise entre [**StartTime**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.starttime) et [**EndTime**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.endtime). Obtenez la position de lecture actuelle du nœud avec la propriété [**Position**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.position) en lecture seule.
-   Activez la boucle du fichier audio en définissant la propriété [**LoopCount**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.loopcount). Lorsqu’elle n’est pas null, cette valeur indique le nombre de fois que le fichier est lu après la lecture initiale. Par exemple, la définition de **LoopCount** sur 1 entraîne la lecture du fichier 2 fois en tout, la définition sur 5 entraîne la lecture du fichier 6 fois en tout. Si vous définissez **LoopCount** sur null, le fichier sera lu en boucle indéfiniment. Pour arrêter la lecture en boucle, définissez la valeur sur 0.
-   Réglez la vitesse à laquelle le fichier audio est lu en définissant la [**PlaybackSpeedFactor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.playbackspeedfactor). Une valeur de 1 correspond à la vitesse d’origine du fichier, 0,5 correspond à la vitesse intermédiaire et 2 correspond à la vitesse double.

##  <a name="mediasource-input-node"></a>Nœud d’entrée MediaSource

La classe [**MediaSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource) offre une méthode courante de référencement de contenu multimédia à partir de différentes sources et présente un modèle commun d’accès aux données multimédias, quel que soit le format multimédia sous-jacent, qui peut être un fichier sur disque, un flux ou une source réseau de diffusion en continu adaptative. Un nœud [**MediaSourceAudioInputNode](https://docs.microsoft.com/uwp/api/windows.media.audio.mediasourceaudioinputnode) vous permet de diriger les données audio d’un objet **MediaSource** dans le graphique audio. Créez un objet **MediaSourceAudioInputNode** en appelant [**CreateMediaSourceAudioInputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createmediasourceaudioinputnodeasync#Windows_Media_Audio_AudioGraph_CreateMediaSourceAudioInputNodeAsync_Windows_Media_Core_MediaSource_), qui transmet un objet **MediaSource** représentant le contenu que vous souhaitez lire. Un objet [**CreateMediaSourceAudioInputNodeResult](https://docs.microsoft.com/uwp/api/windows.media.audio.createmediasourceaudioinputnoderesult) est retourné. Vous pouvez l’utiliser pour déterminer l’état de l’opération en vérifiant la propriété [**Status**](https://docs.microsoft.com/uwp/api/windows.media.audio.createmediasourceaudioinputnoderesult.status). Si l’état est **Success**, vous pouvez obtenir l’objet **MediaSourceAudioInputNode** créé en accédant à la propriété [**Node**](https://docs.microsoft.com/uwp/api/windows.media.audio.createmediasourceaudioinputnoderesult.node). L’exemple suivant illustre la création d’un nœud à partir d’un objet AdaptiveMediaSource représentant la diffusion du contenu sur le réseau. Pour plus d’informations sur l’utilisation de **MediaSource**, voir [Éléments, playlists et pistes multimédias](media-playback-with-mediasource.md). Pour plus d’informations sur la diffusion de contenu multimédia sur Internet, voir [Streaming adaptatif](adaptive-streaming.md).

[!code-cs[DeclareMediaSourceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareMediaSourceInputNode)]

[!code-cs[CreateMediaSourceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateMediaSourceInputNode)]

Pour recevoir une notification lorsque la lecture a atteint la fin du contenu **MediaSource**, enregistrez un gestionnaire pour l’événement [**MediaSourceCompleted**](https://docs.microsoft.com/uwp/api/windows.media.audio.mediasourceaudioinputnode.mediasourcecompleted). 

[!code-cs[RegisterMediaSourceCompleted](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetRegisterMediaSourceCompleted)]

[!code-cs[MediaSourceCompleted](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetMediaSourceCompleted)]

La lecture d’un fichier à partir d’un disque est toujours susceptible de se terminer avec succès, mais le contenu multimédia diffusé à partir d’une source réseau peut échouer pendant la lecture en raison d’une modification de la connexion réseau ou d’autres problèmes qui sont hors du contrôle du graphique audio. Si un objet **MediaSource** devient illisible pendant la lecture, le graphique audio déclenche l’événement [**UnrecoverableErrorOccurred**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.unrecoverableerroroccurred). Vous pouvez utiliser le gestionnaire de cet événement pour arrêter et supprimer le graphique audio et réinitialiser votre graphique. 

[!code-cs[RegisterUnrecoverableError](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetRegisterUnrecoverableError)]

[!code-cs[UnrecoverableError](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetUnrecoverableError)]

##  <a name="file-output-node"></a>Nœud de sortie de fichier

Un nœud de sortie de fichier vous permet de diriger les données audio du graphique vers un fichier audio. Créez un [**AudioFileOutputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFileOutputNode) en appelant [**CreateFileOutputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createfileoutputnodeasync).

[!code-cs[DeclareFileOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFileOutputNode)]


[!code-cs[CreateFileOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFileOutputNode)]

-   Les nœuds de sortie de fichier prennent en charge les formats suivants : mp3, wav, wma et m4a.
-   Vous devez appeler [**AudioFileOutputNode.Stop**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileoutputnode.stop) pour arrêter le traitement du nœud avant d’appeler [**AudioFileOutputNode.FinalizeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileoutputnode.finalizeasync) ou une exception sera levée.

##  <a name="audio-frame-input-node"></a>Nœud d’entrée de trame audio

Un nœud d’entrée de trame audio vous permet de transmettre au graphique audio les données audio que vous générez dans votre propre code. Cela permet notamment la création d’un synthétiseur logiciel personnalisé. Créez un [**AudioFrameInputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFrameInputNode) en appelant [**CreateFrameInputNode**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createframeinputnode).

[!code-cs[DeclareFrameInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFrameInputNode)]


[!code-cs[CreateFrameInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFrameInputNode)]

L’événement [**FrameInputNode.QuantumStarted**](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeinputnode.quantumstarted) est déclenché lorsque le graphique audio est prêt à commencer le traitement du prochain quantum de données audio. Vous fournissez à cet événement vos données audio personnalisées depuis le gestionnaire.

[!code-cs[QuantumStarted](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetQuantumStarted)]

-   L’objet [**FrameInputNodeQuantumStartedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.FrameInputNodeQuantumStartedEventArgs) transmis au gestionnaire d’événements **QuantumStarted** expose la propriété [**RequiredSamples**](https://docs.microsoft.com/uwp/api/windows.media.audio.frameinputnodequantumstartedeventargs.requiredsamples) qui indique le nombre d’échantillons dont le graphique audio a besoin pour remplir le quantum à traiter.
-   Appelez [**AudioFrameInputNode.AddFrame**](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeinputnode.addframe) pour transmettre un objet [**AudioFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioFrame) contenant les données audio au graphique.
- Un nouvel ensemble d’API permettant d’utiliser **MediaFrameReader** avec des données audio a été introduit dans Windows 10, version 1803. Ces API permettent d’obtenir des objets **AudioFrame** à partir d’une source d’images multimédias, qui peuvent être transmis dans un objet **FrameInputNode** à l’aide de la méthode **AddFrame**. Pour plus d’informations, voir [Traiter des trames audio avec MediaFrameReader](process-audio-frames-with-mediaframereader.md).
-   Un exemple d’implémentation de la méthode d’assistance **GenerateAudioData** est indiqué ci-dessous.

Pour remplir un [**AudioFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioFrame) avec des données audio, vous devez accéder à la mémoire tampon sous-jacente de la trame audio. Pour ce faire, vous devez initialiser l’interface COM **IMemoryBufferByteAccess** en ajoutant le code suivant dans votre espace de noms.

[!code-cs[ComImportIMemoryBufferByteAccess](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetComImportIMemoryBufferByteAccess)]

Le code suivant montre un exemple d’implémentation de méthode d’assistance **GenerateAudioData** qui crée une [**AudioFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioFrame) et la remplit avec des données audio.

[!code-cs[GenerateAudioData](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetGenerateAudioData)]

-   Étant donné que cette méthode accède à la mémoire tampon brute sous-jacente des types Windows Runtime, elle doit être déclarée à l’aide du mot clé **unsafe**. Vous devez également configurer votre projet dans Microsoft Visual Studio pour permettre la compilation du code non sécurisé en ouvrant la page **Propriétés** du projet, en cliquant sur la page de propriétés **Générer**, puis en sélectionnant la case à cocher **Autoriser les blocs de code unsafe**.
-   Initialisez une nouvelle instance de [**AudioFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioFrame), dans l’espace de noms **Windows.Media**, en passant la taille de mémoire tampon souhaitée au constructeur. La taille de mémoire tampon est le nombre d’échantillons multiplié par la taille de chaque échantillon.
-   Obtenez la [**AudioBuffer**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioBuffer)de la trame audio en appelant [**LockBuffer**](https://docs.microsoft.com/uwp/api/windows.media.audioframe.lockbuffer).
-   Obtenez une instance de l’interface COM [**IMemoryBufferByteAccess**](https://docs.microsoft.com/previous-versions//mt297505(v=vs.85)) à partir de la mémoire tampon audio en appelant [**CreateReference**](https://docs.microsoft.com/uwp/api/windows.media.audiobuffer.createreference).
-   Obtenez un pointeur vers les données de mémoire tampon audio brutes en appelant [**IMemoryBufferByteAccess.GetBuffer**](https://docs.microsoft.com/windows/desktop/WinRT/imemorybufferbyteaccess-getbuffer) et en le diffusant vers le type d’exemple de données des données audio.
-   Remplissez la mémoire tampon avec les données et renvoyez la [**AudioFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioFrame) pour la soumettre au graphique audio.

##  <a name="audio-frame-output-node"></a>Nœud de sortie de trame audio

Un nœud de sortie de trame audio vous permet de recevoir et de traiter la sortie de données audio à partir du graphique audio à l’aide du code personnalisé que vous créez. Cela permet d’effectuer par exemple une analyse de signal sur la sortie audio. Créez un [**AudioFrameOutputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFrameOutputNode) en appelant [**CreateFrameOutputNode**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createframeoutputnode).

[!code-cs[DeclareFrameOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFrameOutputNode)]

[!code-cs[CreateFrameOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFrameOutputNode)]

L’événement [**AudioGraph.QuantumStarted**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioGraph.QuantumStarted) est déclenché lorsque le graphique audio a commencé le traitement d’un quantum de données audio. Vous pouvez accéder aux données audio à partir du gestionnaire pour cet événement. 

> [!NOTE]
> Si vous souhaitez récupérer des trames audio à une cadence régulière, avec synchronisation avec le graphique audio, appelez [AudioFrameOutputNode.GetFrame](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeoutputnode.GetFrame) à partir du gestionnaire d’événements **QuantumStarted** synchrone. L'événement **QuantumProcessed** est déclenché en mode asynchrone une fois que le moteur audio a terminé le traitement audio, ce qui signifie que sa cadence peut être irrégulière. Par conséquent, vous ne devez pas utiliser l'événement **QuantumProcessed** pour le traitement synchronisé des données de trame audio.

[!code-cs[SnippetQuantumStartedFrameOutput](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetQuantumStartedFrameOutput)]

-   Appelez [**GetFrame**](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeoutputnode.getframe) pour obtenir du graphique un objet [**AudioFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioFrame) contenant les données audio.
-   Un exemple d’implémentation de la méthode d’assistance **ProcessFrameOutput** est indiqué ci-dessous.

[!code-cs[ProcessFrameOutput](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetProcessFrameOutput)]

-   Comme dans l’exemple de nœud d’entrée de trame audio ci-dessus, vous devez déclarer l’interface COM **IMemoryBufferByteAccess** et configurer votre projet pour autoriser le code non sécurisé à accéder à la mémoire tampon audio sous-jacente.
-   Obtenez la [**AudioBuffer**](https://docs.microsoft.com/uwp/api/Windows.Media.AudioBuffer)de la trame audio en appelant [**LockBuffer**](https://docs.microsoft.com/uwp/api/windows.media.audioframe.lockbuffer).
-   Obtenez une instance de l’interface COM **IMemoryBufferByteAccess** à partir de la mémoire tampon audio en appelant [**CreateReference**](https://docs.microsoft.com/uwp/api/windows.media.audiobuffer.createreference).
-   Obtenez un pointeur vers les données de mémoire tampon audio brutes en appelant **IMemoryBufferByteAccess.GetBuffer** et en le diffusant vers le type d’exemple de données des données audio.

## <a name="node-connections-and-submix-nodes"></a>Connexions de nœud et nœuds prémixés

Tous les types de nœud d’entrée exposent la méthode **AddOutgoingConnection** qui achemine les données audio produites par le nœud vers le nœud transmis dans la méthode. L’exemple suivant connecte un [**AudioFileInputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFileInputNode) à un [**AudioDeviceOutputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode) qui est un programme d’installation simple pour la lecture d’un fichier audio sur les haut-parleurs de l’appareil.

[!code-cs[AddOutgoingConnection1](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection1)]

Vous pouvez créer plusieurs connexions à partir d’un nœud d’entrée vers d’autres nœuds. L’exemple suivant ajoute une autre connexion à partir du [**AudioFileInputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFileInputNode) vers un [**AudioFileOutputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioFileOutputNode). À présent, le son du fichier audio est lu par le haut-parleur de l’appareil et est également écrit dans un fichier audio.

[!code-cs[AddOutgoingConnection2](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection2)]

Les nœuds de sortie peuvent également recevoir plus d’une connexion à partir d’autres nœuds. Dans l’exemple suivant, une connexion est établie à partir d’un [**AudioDeviceInputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioDeviceInputNode) vers le nœud [**AudioDeviceOutput**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode). Étant donné que le nœud de sortie dispose de connexions issues du nœud d’entrée de fichier et du nœud d’entrée d’appareil, la sortie contient un mélange d’audio des deux sources. **AddOutgoingConnection** fournit une surcharge qui vous permet de spécifier une valeur gain pour le signal passant par la connexion.

[!code-cs[AddOutgoingConnection3](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection3)]

Bien que les nœuds de sortie puissent accepter des connexions de plusieurs nœuds, vous voudrez peut-être créer une combinaison intermédiaire de signaux à partir d’un ou plusieurs nœuds avant de transmettre la combinaison à une sortie. Par exemple, vous souhaitez peut-être définir le niveau ou appliquer des effets à un sous-ensemble de signaux audio dans un graphique. Pour cela, utilisez le [**AudioSubmixNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioSubmixNode). Vous pouvez vous connecter à un nœud prémixé à partir d’un ou plusieurs nœuds d’entrée ou à partir d’autres nœuds prémixés. Dans l’exemple suivant, un nouveau nœud prémixé est créé avec [**AudioGraph.CreateSubmixNode**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createsubmixnode). Ensuite, les connexions sont ajoutées à partir d’un nœud d’entrée de fichier et d’un nœud de sortie de trame dans le nœud prémixé. Enfin, le nœud prémixé est connecté à un nœud de sortie de fichier.

[!code-cs[CreateSubmixNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateSubmixNode)]

## <a name="starting-and-stopping-audio-graph-nodes"></a>Démarrage et arrêt de nœuds de graphique audio

Lorsque [**AudioGraph.Start**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.start) est appelée, le graphique audio commence à traiter les données audio. Chaque type de nœud fournit les méthodes **Start** et **Stop** qui entraînent le démarrage du nœud individuel ou l’arrêt du traitement des données. Lorsque [**AudioGraph.Stop**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.stop) est appelée, le traitement audio de tous les nœuds est arrêté indépendamment de l’état de chacun d’entre eux. L’état de chaque nœud peut être défini lorsque le graphique audio est arrêté. Par exemple, vous pouvez appeler **Stop** sur un nœud individuel pendant que le graphique est arrêt, puis appeler **AudioGraph.Start**, et le nœud individuel restera à l’arrêt.

Tous les types de nœud exposent la propriété **ConsumeInput** qui, lorsqu’elle est définie sur False, permet au nœud de poursuivre le traitement audio, mais l’empêche de consommer des données audio en cours d’entrée à partir d’autres nœuds.

Tous les types de nœud exposent la méthode **Reset** qui pousse le nœud à ignorer les données audio présentes actuellement dans sa mémoire tampon.

## <a name="adding-audio-effects"></a>Ajout d’effets audio

L’API de graphique audio vous permet d’ajouter des effets audio pour chaque type de nœud dans un graphique. Les nœuds de sortie, les nœuds d’entrée et les nœuds prémixés peuvent avoir un nombre illimité d’effets audio (dans la mesure des capacités du matériel). L’exemple suivant illustre l’ajout de l’effet d’écho intégré à un nœud prémixé.

[!code-cs[AddEffect](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddEffect)]

-   Tous les effets audio implémentent [**IAudioEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IAudioEffectDefinition). Chaque nœud expose une propriété **EffectDefinitions** représentant la liste des effets appliqués à ce nœud. Ajoutez un effet en ajoutant son objet de définition à la liste.
-   Il existe plusieurs classes de définition d’effet fournies dans l’espace de noms **Windows.Media.Audio**. Par exemple :
    -   [**EchoEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.EchoEffectDefinition)
    -   [**EqualizerEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.EqualizerEffectDefinition)
    -   [**LimiterEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.LimiterEffectDefinition)
    -   [**ReverbEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.ReverbEffectDefinition)
-   Vous pouvez créer vos propres effets audio qui implémentent [**IAudioEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IAudioEffectDefinition) et les appliquer à n’importe quel nœud dans un graphique audio.
-   Chaque type de nœud expose une méthode **DisableEffectsByDefinition** qui désactive tous les effets dans la liste du nœud **EffectDefinitions** qui ont été ajoutés à l’aide de la définition spécifiée. **EnableEffectsByDefinition** active les effets avec la définition spécifiée.

## <a name="spatial-audio"></a>Audio spatial
À partir de Windows 10, version 1607, **AudioGraph** prend en charge l’audio spatial, ce qui vous permet de spécifier l’emplacement de l’espace 3D à partir duquel l’audio est émis d’un nœud d’entrée ou prémixé. Vous pouvez également spécifier une forme et la direction d’émission de l’audio, une vitesse qui sera utilisée pour appliquer un effet Doppler à l’audio du nœud et définir un modèle de décroissance décrivant l’atténuation de l’audio avec la distance. 

Pour créer un émetteur, vous pouvez dans un premier temps créer une forme dans laquelle l’audio est projeté de l’émetteur, qui peut être conique ou omnidirectionnel. La classe [**AudioNodeEmitterShape**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioNodeEmitterShape) fournit des méthodes statiques dédiées à la création de chacune de ces formes. Ensuite, créez un modèle de décroissance. Ce dernier définit la manière dont le volume de l’audio de l’émetteur décroît à mesure de l’augmentation de la distance avec l’écouteur. La méthode [**CreateNatural**](https://docs.microsoft.com/uwp/api/windows.media.audio.audionodeemitterdecaymodel.createnatural) crée un modèle de décroissance qui émule la décroissance naturelle de l’audio à l’aide d’un modèle de décroissance fonction du carré de la distance. Enfin, créez un objet [**AudioNodeEmitterSettings**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioNodeEmitterSettings). Actuellement, cet objet est utilisé uniquement pour activer et désactiver l’atténuation de Doppler basée sur la vélocité de l’audio de l’émetteur. Appelez le constructeur [**AudioNodeEmitter**](https://docs.microsoft.com/uwp/api/windows.media.audio.audionodeemitter.), en passant les objets d’initialisation nouvellement créés. Par défaut, l’émetteur est placé à l’origine, mais vous pouvez définir sa position avec la propriété [**Position**](https://docs.microsoft.com/uwp/api/windows.media.audio.audionodeemitter.position).

> [!NOTE]
> Les émetteurs de nœud audio peuvent traiter uniquement de l’audio au format mono avec un taux d’échantillonnage de 48 kHz. Toute tentative d’utilisation d’audio stéréo ou d’audio avec un taux d’échantillonnage différent provoquera la levée d’une exception.

Vous affectez l’émetteur à un nœud audio lors de sa création en appliquant la méthode de création surchargée pour le type de nœud souhaité. Dans cet exemple, [**CreateFileInputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createfileinputnodeasync) est utilisé pour créer un nœud d’entrée de fichier à partir du fichier spécifié et l’objet [**AudioNodeEmitter**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioNodeEmitter) que vous souhaitez associer au nœud.

[!code-cs[CreateEmitter](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateEmitter)]

La classe [**AudioDeviceOutputNode**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode) qui produit de l’audio du graphique à destination de l’utilisateur présente un objet écouteur, accessible avec la propriété [**Listener**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiodeviceoutputnode.listener), qui représente l’emplacement, l’orientation et la vélocité de l’utilisateur dans l’espace 3D. Les positions de l’ensemble des émetteurs dans le graphique sont relatives à la position et à l’orientation de l’objet émetteur. Par défaut, l’écouteur se trouve à l’origine (0,0,0) orientée vers l’avant le long de l’axe Z, mais vous pouvez définir sa position et son orientation avec les propriétés [**Position**](https://docs.microsoft.com/uwp/api/windows.media.audio.audionodelistener.position) et [**Orientation**](https://docs.microsoft.com/uwp/api/windows.media.audio.audionodelistener.orientation).

[!code-cs[Listener](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetListener)]

Vous pouvez mettre à jour l’emplacement, la vitesse et la direction des émetteurs lors de l’exécution pour simuler le mouvement d’une source audio par le biais de l’espace 3D.

[!code-cs[UpdateEmitter](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetUpdateEmitter)]

Vous pouvez également mettre à jour l’emplacement, la vitesse et la direction de l’objet écouteur lors de l’exécution pour simuler le mouvement de l’utilisateur par le biais de l’espace 3D.

[!code-cs[UpdateListener](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetUpdateListener)]

Par défaut, l’audio spatial est calculé à l’aide de l’algorithme HRTF (Head-Relative Transfer Function) afin d’atténuer l’audio en fonction de sa forme, de sa vitesse et de sa position par rapport à l’écouteur. Vous pouvez définir la propriété [**SpatialAudioModel**](https://docs.microsoft.com/uwp/api/windows.media.audio.audionodeemitter.spatialaudiomodel) sur **FoldDown** afin d’utiliser une méthode simple de mélange stéréo de simulation de l’audio spatial moins précise, mais également moins exigeante pour le processeur et les ressources mémoire.

## <a name="see-also"></a>Voir aussi
- [Lecture de contenu multimédia](media-playback.md)
 

 




