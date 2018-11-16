---
author: drewbatgit
ms.assetid: 66d0c3dc-81f6-4d9a-904b-281f8a334dd0
description: Cet article vous présente le moyen le plus simple de capturer des photos et des vidéos à l’aide de la classe MediaCapture.
title: Capture photo, vidéo et audio de base à l’aide de MediaCapture
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7204c01cc80c50dacb2009b2138a7c046974dc62
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7111452"
---
# <a name="basic-photo-video-and-audio-capture-with-mediacapture"></a>Capture photo, vidéo et audio de base à l’aide de MediaCapture


Cet article vous présente le moyen le plus simple de capturer des photos et des vidéos à l’aide de la classe [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture). La classe **MediaCapture** expose un jeu robuste d’API qui fournit un contrôle de niveau inférieur sur le pipeline de capture et prend en charge des scénarios de capture avancés, mais cet article est destiné à vous aider à ajouter rapidement et facilement la capture multimédia à votre application. Pour en savoir plus sur les fonctions fournies par **MediaCapture**, consultez la section [**Caméra**](camera.md).

Si vous souhaitez simplement capturer une photo ou une vidéo et n’avez pas l’intention d’ajouter de fonctions supplémentaires de capture multimédia, ou si vous ne souhaitez pas créer votre propre interface utilisateur d’appareil photo, vous pouvez utiliser la classe [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CameraCaptureUI). Avec elle, vous lancez simplement l’application intégrée d’appareil photo afin de recevoir le fichier audio ou vidéo capturé. Pour plus d’informations, consultez la section [**Capturer des photos et des vidéos à l’aide de l’interface utilisateur de l’appareil photo intégré à Windows**](capture-photos-and-video-with-cameracaptureui.md).

Le code fourni dans cet article a été adapté à partir de l’exemple [**CameraStarterKit**](https://go.microsoft.com/fwlink/?linkid=619479). Vous pouvez télécharger l’exemple pour voir le code utilisé en contexte ou pour vous en servir comme base pour votre propre application.

## <a name="add-capability-declarations-to-the-app-manifest"></a>Ajouter des déclarations de fonctionnalités au manifeste de l’application

Afin que votre application puisse accéder à l’appareil photo d’un appareil, vous devez déclarer que cette application utilise les fonctionnalités de l’appareil *webcam* et *microphone*. Si vous souhaitez enregistrer dans la bibliothèque d’images ou dans la vidéothèque de l’utilisateur des photos et des vidéos capturées, vous devez également déclarer les fonctionnalités *picturesLibrary* et *videosLibrary*.

**Pour ajouter des fonctionnalités au manifeste de l’application**

1.  Dans Microsoft Visual Studio, dans l’**Explorateur de solutions**, ouvrez le concepteur pour le manifeste de l’application en double-cliquant sur l’élément **package.appxmanifest**.
2.  Sélectionnez l’onglet **Fonctionnalités**.
3.  Activez les cases à cocher **Webcam** et **Microphone**.
4.  Pour accéder à la bibliothèque d’images et à la vidéothèque, cochez les cases correspondant à **Bibliothèque d’images** et **Vidéothèque**.


## <a name="initialize-the-mediacapture-object"></a>Initialiser l’objet MediaCapture
Toutes les méthodes de capture décrites dans cet article nécessitent la première étape d’initialisation de l’objet [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture), exécutée via l’appel du constructeur, puis de [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.InitializeAsync). Dans la mesure où l’objet **MediaCapture** est accessible depuis plusieurs emplacements de votre application, déclarez une variable de classe pour stocker l’objet.  Implémentez un gestionnaire pour l’événement [**Failed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.Failed) de l’objet **MediaCapture** afin d’être informé d’un éventuel échec de l’opération de capture.

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

[!code-cs[InitMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetInitMediaCapture)]

## <a name="set-up-the-camera-preview"></a>Définir l’aperçu de l’appareil photo
Il est possible de capturer des photos, vidéos et audio à l’aide de **MediaCapture** sans afficher l’aperçu de l’appareil photo, mais en règle générale, vous souhaitez afficher le flux d’aperçu de manière à ce que l’utilisateur n’ait aucune visibilité sur le contenu capturé. Par ailleurs, quelques fonctions **MediaCapture** nécessitent l’exécution du flux d’aperçu pour être activées. Il s’agit notamment la mise au point, l’exposition et la balance des blancs automatiques. Pour savoir comment configurer l’aperçu de l’appareil photo, consultez la page [**Afficher l’aperçu de l’appareil photo**](simple-camera-preview-access.md).

## <a name="capture-a-photo-to-a-softwarebitmap"></a>Capturer une photo sur une classe SoftwareBitmap
La classe [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap) a été introduite dans Windows10 afin d’offrir une représentation commune des images entre plusieurs fonctions. Si vous souhaitez capturer une photo avant de l’utiliser immédiatement dans votre application, en l’affichant par exemple au format XAML au lieu de la capturer dans un fichier, vous devez la capturer dans une classe **SoftwareBitmap**. Vous avez toujours la possibilité d’enregistrer l’image sur un disque ultérieurement.

Après l’initialisation de l’objet **MediaCapture**, vous pouvez capturer une photo dans une méthode **SoftwareBitmap** à l’aide de la classe [**LowLagPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoCapture). Récupérez une instance de cette classe en appelant [**PrepareLowLagPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagPhotoCaptureAsync), en passant un objet [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties) spécifiant le format d’image souhaité. [**CreateUncompressed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties.CreateUncompressed) génère un encodage non compressé avec le format de pixel spécifié. Capturez une photo en appelant [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoCapture.CaptureAsync), qui renvoie un objet [**CapturedPhoto**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedPhoto). Récupérez une méthode **SoftwareBitmap** en accédant à la propriété [**Frame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedPhoto.Frame), puis à la propriété [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame.SoftwareBitmap).

Si vous le souhaitez, vous pouvez capturer plusieurs photos en appelant à plusieurs reprises **CaptureAsync**. Quand vous avez terminé la capture, appelez [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.FinishAsync) afin d’arrêter la session **LowLagPhotoCapture** et de libérer les ressources associées. Après avoir appelé **FinishAsync**, pour recommencer à capturer les photos, vous devrez rappeler [**PrepareLowLagPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagPhotoCaptureAsync) afin de réinitialiser la session de capture avant d’appeler [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoCapture.CaptureAsync).

[!code-cs[CaptureToSoftwareBitmap](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureToSoftwareBitmap)]

À partir de Windows, version1803, vous pouvez accéder à la propriété [**BitmapProperties**](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframe.bitmapproperties) de la classe **CapturedFrame** retournée par **CaptureAsync** pour récupérer les métadonnées relatives à la photo capturée. Vous pouvez transmettre ces données à un objet **BitmapEncoder** pour enregistrer les métadonnées dans un fichier. Auparavant, il n’existait aucun moyen d’accéder à ces données pour les formats d’image non compressés. Vous pouvez également accéder à la propriété [**ControlValues**](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframe.controlvalues) pour récupérer un objet [**CapturedFrameControlValues**](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframecontrolvalues) qui décrit les valeurs de contrôle, telles que l’exposition et la balance des blancs, pour l’image capturée.

Pour plus d’informations sur l’utilisation de **BitmapEncoder** et de l'objet **SoftwareBitmap**, notamment l’affichage dans une page XAML, voir [**Créer, modifier et enregistrer des images bitmap**](imaging.md). 

Pour plus d’informations sur la définition des valeurs de contrôle de l’appareil de capture, voir [Contrôles de l’appareil de capture pour la photo et la vidéo](capture-device-controls-for-photo-and-video-capture.md).

À partir de Windows10, version1803, vous pouvez obtenir les métadonnées, telles que les informations EXIF, pour les photos capturées dans un format non compressé en accédant à la propriété [**BitmapProperties**](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframe.bitmapproperties) de la classe **CapturedFrame** retournée par **MediaCapture**. Dans les versions précédentes, ces données étaient uniquement accessibles dans l’en-tête des photos capturées dans un format de fichier compressé. Vous pouvez transmettre ces données à un objet [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder) lors de l’écriture manuelle d’un fichier image. Pour plus d’informations sur l’encodage des bitmaps, voir [Créer, modifier et enregistrer des images bitmap](imaging.md).  Vous pouvez également accéder aux valeurs de contrôle de trame, comme les paramètres d’exposition et de flash utilisés lors de la capture de l’image en accédant à la propriété [**ControlValues**](https://docs.microsoft.com/en-us/uwp/api/windows.media.capture.capturedframe.controlvalues). Pour plus d’informations, voir [Contrôles de l’appareil de capture pour la capture photo et vidéo](capture-device-controls-for-photo-and-video-capture.md).

## <a name="capture-a-photo-to-a-file"></a>Capturer une photo dans un fichier
Une application de photographie classique enregistre une photo capturée sur un disque ou sur un stockage cloud et doit ajouter des métadonnées, comme l’orientation de la photo, au fichier. L’exemple suivant vous explique comment capturer une photo dans un fichier. Vous avez toujours la possibilité de créer ultérieurement une méthode **SoftwareBitmap** à partir du fichier image. 

La technique décrite dans cet exemple capture la photo dans un flux en mémoire, puis procède à son transcodage sur un fichier sur disque. Cet exemple utilise [**GetLibraryAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageLibrary.GetLibraryAsync) pour récupérer la bibliothèque d’images de l’utilisateur, puis la propriété [**SaveFolder**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageLibrary.SaveFolder) pour récupérer un dossier d’enregistrement de référence par défaut. N’oubliez pas d’ajouter la fonctionnalité **Bibliothèque d’images** à votre manifeste d’application pour accéder à ce dossier. [**CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageFolder.CreateFileAsync) crée un nouvel élément [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageFile) sur lequel enregistrer la photo.

Créez une classe [**InMemoryRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.Streams.InMemoryRandomAccessStream), puis appelez [**CapturePhotoToStreamAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.CapturePhotoToStreamAsync) pour capturer une photo dans le flux, en passant le flux et un objet [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties) spécifiant le format d’image à utiliser. Vous pouvez créer des propriétés d’encodage personnalisées en initialisant vous-même l’objet, mais la classe fournit des méthodes statiques, comme [**ImageEncodingProperties.CreateJpeg**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties.CreateJpeg), pour les formats d’encodage courants. Ensuite, créez un flux de fichiers sur le fichier de sortie en appelant [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageFile.OpenAsync). Créez une classe [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapDecoder) pour décoder l’image du flux en mémoire, puis créez une classe [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder) afin d’encoder l’image sur un fichier en appelant [**CreateForTranscodingAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.CreateForTranscodingAsync).

Vous pouvez éventuellement créer un objet [**BitmapPropertySet**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapPropertySet), puis appeler [**SetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226252.aspx) sur l’encodeur d’image afin d’inclure les métadonnées sur la photo dans le fichier image. Pour plus d’informations sur les propriétés d’encodage, consultez la section [**Métadonnées d’image**](image-metadata.md). Pour la plupart des applications de photographie, une bonne gestion de l’orientation de l’appareil est essentielle. Pour plus d’informations, consultez la section [**Gérer l’orientation de l’appareil à l’aide de MediaCapture**](handle-device-orientation-with-mediacapture.md).

Enfin, appelez [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.FlushAsync) sur l’objet d’encodeur afin de procéder au transcodage de la photo du flux en mémoire sur le fichier.

[!code-cs[CaptureToFile](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureToFile)]

Pour plus d’utilisation sur l’utilisation des fichiers et des dossiers, consultez la section [**Fichiers, dossiers et bibliothèques**](https://msdn.microsoft.com/windows/uwp/files/index).

## <a name="capture-a-video"></a>Capturer une vidéo
Ajoutez rapidement une capture vidéo à votre application à l’aide de la classe [**LowLagMediaRecording**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording). Tout d’abord, déclarez une variable de classe associée à l’objet.

[!code-cs[LowLagMediaRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetLowLagMediaRecording)]

Ensuite, créez un objet **StorageFile** sur lequel enregistrer la vidéo. Notez que pour procéder à un enregistrement sur la vidéothèque de l’utilisateur, vous devez ajouter la fonctionnalité **Vidéothèque** à votre manifeste d’application. Appelez [**PrepareLowLagRecordToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagRecordToStorageFileAsync) afin d’initialiser l’enregistrement du contenu multimédia, en passant un fichier de stockage et un objet [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile) spécifiant l’encodage pour la vidéo. La classe fournit des méthodes statiques, comme [**CreateMp4**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile.CreateMp4), permettant de créer des profils d’encodage vidéo courants.

Enfin, appelez [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StartAsync) afin de commencer la capture vidéo.

[!code-cs[StartVideoCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartVideoCapture)]

Pour arrêter l’enregistrement vidéo, appelez [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StopAsync).

[!code-cs[StopRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStopRecording)]

Vous pouvez continuer à appeler **StartAsync** et **StopAsync** pour capturer des vidéos supplémentaires. Quand vous avez terminé la capture des vidéos, appelez [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.FinishAsync) pour supprimer la session de capture et nettoyer les ressources associées. Après cet appel, vous devez appeler de nouveau **PrepareLowLagRecordToStorageFileAsync** pour réinitialiser la session de capture avant d’appeler **StartAsync**.

[!code-cs[FinishAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetFinishAsync)]

Lorsque vous capturez de la vidéo, vous devez enregistrer un gestionnaire pour l’événement [**RecordLimitationExceeded**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.RecordLimitationExceeded) de l’objet **MediaCapture**, qui sera déclenché par la système d’exploitation si vous dépassez la limite d’un enregistrement unique, généralement de troisheures. Dans le gestionnaire de l’événement, vous devez finaliser votre enregistrement en appelant [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StopAsync).

[!code-cs[RecordLimitationExceeded](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceeded)]

[!code-cs[RecordLimitationExceededHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceededHandler)]

### <a name="play-and-edit-captured-video-files"></a>Lire et modifier des fichiers vidéo capturés
Une fois que vous avez capturé une vidéo dans un fichier, vous pouvez charger le fichier et le lire dans l’interface utilisateur de votre application. Pour ce faire, utilisez le contrôle XAML **[MediaPlayerElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)** et un objet **[MediaPlayer](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)** associé. Pour plus d’informations sur la lecture de contenus multimédias dans une page XAML, voir [Lire du contenu audio et vidéo avec MediaPlayer](play-audio-and-video-with-mediaplayer.md).

Vous pouvez également créer un objet **[MediaClip](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip)** à partir d’un fichier vidéo en appelant **[CreateFromFileAsync](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.createfromfileasync)**.  Un objet **[MediaComposition](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition)** fournit des fonctionnalités de modification vidéo de base telles que la réorganisation de la séquence d’objets **MediaClip**, le découpage de la longueur de la vidéo, la création de couches, l’ajout d’une musique de fond et l’application d’effets vidéo. Pour plus d’informations sur l’utilisation des compositions multimédias, voir [Compositions multimédias et modification](media-compositions-and-editing.md).

## <a name="pause-and-resume-video-recording"></a>Interrompre et reprendre l’enregistrement vidéo
Pour suspendre un enregistrement vidéo et le reprendre sans créer de fichier de sortie séparé, appelez [**PauseAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.PauseAsync), puis [**ResumeAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.ResumeAsync).

[!code-cs[PauseRecordingSimple](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetPauseRecordingSimple)]

[!code-cs[ResumeRecordingSimple](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetResumeRecordingSimple)]

À partir de Windows10, version 1607, vous pouvez suspendre un enregistrement vidéo et recevoir la dernière image capturée avant l’interruption. Vous pouvez alors superposer cette image sur l’aperçu de l’appareil photo afin de permettre à l’utilisateur d’aligner l’appareil photo avec l’image interrompue avant de reprendre l’enregistrement. L’appel à [**PauseWithResultAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.PauseWithResultAsync) renvoie un objet [**MediaCapturePauseResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapturePauseResult). la propriété [**LastFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapturePauseResult.LastFrame) est un objet [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.VideoFrame) représentant la dernière image. Pour afficher l’image au format XAML, récupérez la représentation **SoftwareBitmap** de l’image vidéo. Actuellement, seules les images au format BGRA8 avec un canal alpha prémultiplié ou vide sont prises en charge, aussi appelez [**Convert**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap.Covert) pour récupérer le format approprié, si nécessaire.  Créez un nouvel objet [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource), puis appelez [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource.SetBitmapAsync) pour l’initialiser. Enfin, définissez la propriété **Source** d’un contrôle XAML [**Image**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Image) afin d’afficher l’image. Pour que cette astuce fonctionne, votre image doit être alignée sur le contrôle **CaptureElement** et présenter une valeur d’opacité inférieure à 1. N’oubliez pas que vous pouvez modifier l’interface utilisateur uniquement sur le thread dédié. Dès lors, effectuez cet appel à l’intérieur de [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Core.CoreDispatcher.RunAsync).

**PauseWithResultAsync** renvoie également la durée de la vidéo enregistrée dans le segment précédent, si vous souhaitez effectuer un suivi de la durée totale enregistrée.

[!code-cs[PauseCaptureWithResult](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetPauseCaptureWithResult)]

Quand vous reprenez l’enregistrement, vous pouvez définir la source de l’image sur Null, puis la masquer.

[!code-cs[ResumeCaptureWithResult](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetResumeCaptureWithResult)]

Notez que vous pouvez également récupérer une image résultante quand vous arrêtez la vidéo en appelant [**StopWithResultAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StopWithResultAsync).


## <a name="capture-audio"></a>Capturer l’audio 
Vous pouvez rapidement ajouter la capture audio à votre application en appliquant la technique décrite ci-dessus relative à la capture vidéo. L’exemple ci-dessous est relatif à la création d’un élément **StorageFile** dans le dossier des données de l’application. Appelez [**PrepareLowLagRecordToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagRecordToStorageFileAsync) pour initialiser la session de capture, en passant le fichier et une classe [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile) générée dans l’exemple par la méthode statique [**CreateMp3**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile.CreateMp3). Pour démarrer l’enregistrement, appelez [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StartAsync).

[!code-cs[StartAudioCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartAudioCapture)]


Appelez [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoSequenceCapture.StopAsync) afin d’arrêter l’enregistrement audio.

## <a name="related-topics"></a>Rubriquesassociées

* [Caméra](camera.md)
[!code-cs[StopRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStopRecording)]

Vous pouvez appeler **StartAsync** et **StopAsync** à plusieurs reprises pour enregistrer plusieurs fichiers audio. Quand vous avez terminé la capture audio, appelez [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.FinishAsync) pour supprimer la session de capture et nettoyer les ressources associées. Après cet appel, vous devez appeler de nouveau **PrepareLowLagRecordToStorageFileAsync** pour réinitialiser la session de capture avant d’appeler **StartAsync**.

[!code-cs[FinishAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetFinishAsync)]


## <a name="detect-and-respond-to-audio-level-changes-by-the-system"></a>Détecter les changements de niveau audio initiés par le système et y répondre
À partir de Windows10, version1803, votre application peut détecter quand le système baisse ou désactive le niveau audio des flux de capture audio et de rendu audio de votre application. Par exemple, le système peut désactiver les flux de votre application lorsqu’elle passe à l’arrière-plan. La classe [**AudioStateMonitor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor) vous permet de vous inscrire pour recevoir un événement lorsque le système modifie le volume d’un flux audio. Obtenez une instance de **AudioStateMonitor** pour l’analyse des flux de capture audio en appelant [**CreateForCaptureMonitoring**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.createforcapturemonitoring#Windows_Media_Audio_AudioStateMonitor_CreateForCaptureMonitoring). Obtenez une instance pour l’analyse des flux de rendu audio en appelant [**CreateForRenderMonitoring**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.createforrendermonitoring). Enregistrez un gestionnaire pour l’événement [**SoundLevelChanged**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged) de chaque analyse afin d’être averti lorsque l’audio pour la catégorie de flux correspondante est modifié par le système.

[!code-cs[AudioStateMonitorUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetAudioStateMonitorUsing)]

[!code-cs[AudioStateVars](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetAudioStateVars)]

[!code-cs[RegisterAudioStateMonitor](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRegisterAudioStateMonitor)]

Dans le gestionnaire **SoundLevelChanged** du flux de capture, vous pouvez vérifier la propriété [**SoundLevel**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevel) de l’expéditeur **AudioStateMonitor** pour déterminer le nouveau niveau sonore. Notez qu’un flux de capture ne doit jamais être baissé ou «atténué» par le système. Il doit uniquement être désactivé ou restauré sur le volume complet. Si le flux audio est désactivé, vous pouvez arrêter une capture en cours. Si le flux audio est restauré sur le volume complet, vous pouvez redémarrer la capture. L’exemple suivant utilise des variables de classe booléennes pour déterminer si l’application est en cours de capture audio et si la capture a été arrêtée en raison du changement d’état audio. Ces variables sont utilisées pour déterminer le moment opportun pour arrêter ou démarrer par programme la capture audio.

[!code-cs[CaptureSoundLevelChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureSoundLevelChanged)]

L’exemple de code suivant illustre une implémentation du gestionnaire **SoundLevelChanged** pour le rendu audio. Selon votre scénario d’application et le type de contenu lu, vous pouvez suspendre la lecture audio lorsque le niveau sonore est atténué. Pour plus d’informations sur la gestion des changements de niveau sonore pour la lecture multimédia, voir [Lire du contenu audio et vidéo avec MediaPlayer](play-audio-and-video-with-mediaplayer.md).

[!code-cs[RenderSoundLevelChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRenderSoundLevelChanged)]


* [Capturer des photos et des vidéos à l’aide de l’interface utilisateur de l’appareil photo intégré à Windows](capture-photos-and-video-with-cameracaptureui.md)
* [Gérer l’orientation de l’appareil à l’aide de MediaCapture](handle-device-orientation-with-mediacapture.md)
* [Créer, modifier et enregistrer des images bitmap](imaging.md)
* [Fichiers, dossiers et bibliothèques](https://msdn.microsoft.com/windows/uwp/files/index)

