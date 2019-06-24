---
ms.assetid: af3941c0-3508-4ba2-a79e-fc71657c605f
description: Cet article vous explique comment gérer l’orientation de l’appareil quand vous capturez des photos et des vidéos à l’aide d’une classe d’assistance.
title: Gérer l’orientation de l’appareil à l’aide de MediaCapture
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0e49bd31ff005799e1c3fc7f0bc44be8bd7c216c
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318416"
---
# <a name="handle-device-orientation-with-mediacapture"></a>Gérer l’orientation de l’appareil à l’aide de MediaCapture
Lorsque votre application capture une photo ou une vidéo qui est destinée à être vue en dehors, comme lors de l’enregistrement sur un fichier sur l’appareil de l’utilisateur ou d’un partage en ligne, il est important de coder l’image à l’aide des métadonnées appropriées d’orientation, de manière à ce que le contenu soit orienté correctement lorsqu’une autre application ou un autre appareil l’affiche. La détermination des données appropriées d’orientation à inclure dans un fichier multimédia peut être une tâche complexe, dans la mesure où diverses variables sont à examiner, dont l’orientation du châssis de l’appareil, l’orientation de l’affichage et le placement de l’appareil photo sur le châssis (appareil photo frontal ou arrière). 

Pour simplifier le processus de gestion de l’orientation, nous vous recommandons d’utiliser une classe d’assistance **CameraRotationHelper**, pour laquelle une définition complète est fournie à la fin de cet article. Vous pouvez ajouter cette classe à votre projet, puis suivre les instructions de cet article pour ajouter la prise en charge de l’orientation à votre application caméra. La classe d’assistance simplifie la rotation des contrôles de votre interface utilisateur d’appareil photo, de manière à ce qu’ils soient correctement affichés du point de vue de l’utilisateur.

> [!NOTE] 
> Cet article repose sur le code et les concepts évoqués dans l’article [**Capture photo, vidéo et audio de base à l’aide de MediaCapture**](basic-photo-video-and-audio-capture-with-mediacapture.md). Nous vous recommandons de vous familiariser avec les concepts de base de l’utilisation de la classe **MediaCapture** avant d’ajouter la prise en charge de l’orientation à votre application.

## <a name="namespaces-used-in-this-article"></a>Espaces de noms utilisés dans cet article
L’exemple de code de cet article utilise des API des espaces de noms suivants, que vous devez inclure dans votre code. 

[!code-cs[OrientationUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetOrientationUsing)]

La première étape de l’ajout de la prise en charge de l’orientation à votre application consiste à verrouiller l’affichage, de manière à ce qu’il ne fasse pas l’objet d’une rotation automatique lors de la rotation de l’appareil. La rotation automatique de l’interface utilisateur fonctionne efficacement pour la plupart des types d’applications, mais elle n’est pas intuitive pour les utilisateurs quand l’aperçu de l’appareil photo fait l’objet d’une rotation. Verrouillez l’orientation de l’affichage en définissant la propriété [**DisplayInformation.AutoRotationPreferences**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.autorotationpreferences) sur [**DisplayOrientations.Landscape**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations). 

[!code-cs[AutoRotationPreference](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetAutoRotationPreference)]

## <a name="tracking-the-camera-device-location"></a>Suivi de l’emplacement de l’appareil photo
Pour calculer l’orientation appropriée du contenu multimédia capturé, votre application doit déterminer l’emplacement de l’appareil photo sur le châssis. Ajoutez une variable de membre booléenne afin de détecter si l’appareil photo est externe à l’appareil, tel qu’une webcam USB. Ajoutez une autre variable booléenne afin de détecter si l’aperçu doit être mis en miroir, ce qui est le cas lors de l’utilisation d’un appareil photo de face. Par ailleurs, ajoutez une variable dédiée au stockage de l’objet **DeviceInformation** représentant l’appareil photo sélectionné.

[!code-cs[CameraDeviceLocationBools](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCameraDeviceLocationBools)]
[!code-cs[DeclareCameraDevice](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareCameraDevice)]

## <a name="select-a-camera-device-and-initialize-the-mediacapture-object"></a>Sélectionner un appareil photo et initialiser l’objet MediaCapture
L’article [**Capture photo, vidéo et audio de base à l’aide de MediaCapture**](basic-photo-video-and-audio-capture-with-mediacapture.md) vous explique comment initialiser l’objet **MediaCapture** avec quelques lignes de code. Pour la prise en charge de l’orientation de l’appareil photo, nous ajouterons quelques étapes supplémentaires au processus d’initialisation.

Tout d’abord, appelez [**DeviceInformation.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) en passant le sélecteur d’appareil [**DeviceClass.VideoCapture**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceClass) afin de récupérer une liste de l’ensemble des appareils de capture vidéo disponibles. Ensuite, sélectionnez le premier appareil de la liste où l’emplacement de l’appareil photo est connu, correspondant à la valeur fournie, qui est dans cet exemple un appareil photo de face. Si aucun appareil photo ne se trouve sur le panneau souhaité, le premier appareil photo (ou l’appareil par défaut) est utilisé.

Si un appareil photo est identifié, un nouvel objet [**MediaCaptureInitializationSettings**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureInitializationSettings) est créé et la propriété [**VideoDeviceId**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.videodeviceid) est définie sur l’appareil sélectionné. Ensuite, créez l’objet [**InitializeAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.initializeasync), en passant l’objet de paramètres afin d’indiquer au système d’utiliser l’appareil photo sélectionné.

Ensuite, vérifiez si le panneau d’appareil sélectionné est Null ou inconnu. Si c’est le cas, l’appareil photo est externe, ce qui signifie que sa rotation n’est pas liée à la rotation de l’appareil. Si le panneau est connu et se trouve sur la partie avant du châssis de l’appareil, nous savons que l’aperçu doit être mis en miroir, tout comme le suivi de variable défini.

[!code-cs[InitMediaCaptureWithOrientation](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetInitMediaCaptureWithOrientation)]
## <a name="initialize-the-camerarotationhelper-class"></a>Initialiser la classe CameraRotationHelper

Nous commençons maintenant à utiliser la classe **CameraRotationHelper**. Déclarez une variable de membre de classe dédiée au stockage de l’objet. Appelez le constructeur, en passant l’emplacement du châssis de l’appareil photo sélectionné. La classe d’assistance utilise ces informations pour calculer l’orientation appropriée du contenu multimédia capturé, du flux d’aperçu et de l’interface utilisateur. Enregistrez un gestionnaire pour l’événement **OrientationChanged** de la classe d’assistance, qui sera déclenché quand nous aurons besoin de mettre à jour l’orientation de l’interface utilisateur ou du flux d’aperçu.

[!code-cs[DeclareRotationHelper](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareRotationHelper)]

[!code-cs[InitRotationHelper](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetInitRotationHelper)]

## <a name="add-orientation-data-to-the-camera-preview-stream"></a>Ajouter les données d’orientation au flux d’aperçu de l’appareil photo
La procédure d’ajout de l’orientation appropriée aux métadonnées du flux d’aperçu n’affecte pas la manière dont l’aperçu apparaît pour les utilisateurs, mais elle permet au système de coder correctement les images du flux d’aperçu.

Pour démarrer l’aperçu de l’appareil photo, appelez [**MediaCapture.StartPreviewAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.startpreviewasync). Avant cela, vérifiez la variable de membre afin de savoir si l’aperçu doit être mis en miroir (pour un appareil photo de face). Si tel est le cas, définissez la propriété [**FlowDirection**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.flowdirection) de [**CaptureElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CaptureElement), appelée *PreviewControl* dans cet exemple, sur [**FlowDirection.RightToLeft**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FlowDirection). Après avoir démarré l’aperçu, appelez la méthode d’assistance **SetPreviewRotationAsync** afin de définir la rotation de l’aperçu. Vous trouverez ci-après l’implémentation de cette méthode.

[!code-cs[StartPreviewWithRotationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartPreviewWithRotationAsync)]

Nous définissons la rotation de l’aperçu dans une méthode séparée, de manière à ce qu’elle puisse être mise à jour lorsque l’orientation du téléphone est modifiée sans réinitialisation du flux d’aperçu. Si l’appareil photo est externe à l’appareil, aucune action n’est entreprise. Dans le cas contraire, l’élément **GetCameraPreviewOrientation** de la méthode **CameraRotationHelper** est appelé. Il appelle l’orientation appropriée du flux d’aperçu. 

Pour définir les métadonnées, les propriétés du flux d’aperçu sont récupérées en appelant [**VideoDeviceController.GetMediaStreamProperties**](https://docs.microsoft.com/uwp/api/windows.media.devices.videodevicecontroller.getmediastreamproperties). Ensuite, créez le GUID représentant l’attribut Media Foundation Transform (MFT) pour la rotation du flux vidéo. Dans C++, vous pouvez utiliser la constante [**MF_MT_VIDEO_ROTATION**](https://docs.microsoft.com/windows/desktop/medfound/mf-mt-video-rotation), mais dans C# vous devez spécifier manuellement la valeur GUID. 

Ajoutez une valeur de propriété à l’objet des propriétés du flux, en spécifiant le GUID en tant que clé et la rotation d’aperçu en tant que valeur. Cette propriété doit se voir associer des valeurs en unités de degrés de rotation vers la gauche, de manière à ce que l’objet **ConvertSimpleOrientationToClockwiseDegrees** de la méthode **CameraRotationHelper** soit utilisé pour convertir la valeur simple d’orientation. Enfin, appelez [**SetEncodingPropertiesAsync**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture#Windows_Media_Capture_MediaCapture_SetEncodingPropertiesAsync_Windows_Media_Capture_MediaStreamType_Windows_Media_MediaProperties_IMediaEncodingProperties_Windows_Media_MediaProperties_MediaPropertySet_) afin d’appliquer la nouvelle propriété de rotation au flux.

[!code-cs[SetPreviewRotationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSetPreviewRotationAsync)]

Ensuite, ajoutez le gestionnaire à l’événement **CameraRotationHelper.OrientationChanged**. Cet événement transmet un argument qui vous indique si le flux d’aperçu doit faire l’objet d’une rotation. Si l’orientation de l’appareil a été modifiée pour que ce dernier soit positionné vers le haut ou vers le bas, la valeur est False. SI l’aperçu doit faire l’objet d’une rotation, appelez **SetPreviewRotationAsync**, valeur définie précédemment.

Ensuite, dans le gestionnaire d’événement **OrientationChanged**, mettez à jour votre interface utilisateur si nécessaire. Récupérez l’orientation courante recommandée d’interface utilisateur de la classe d’assistance en appelant **GetUIOrientation**, puis convertissez la valeur en degrés de rotation horaire, comme cela se faire avec les transformations XAML. Générez un élément [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform) à partir de la valeur d’orientation, puis définissez la propriété [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform)de vos contrôles XAML. En fonction de la disposition de votre interface utilisateur, il vous faudra effectuer des ajustements supplémentaires venant s’ajouter à la rotation des contrôles. Par ailleurs, n’oubliez pas que l’ensemble des mises à jour de votre interface utilisateur doivent être effectuées sur le thread d’interface utilisateur. Dès lors, vous devez placer ce code dans un appel à [**RunAsync**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher#Windows_UI_Core_CoreDispatcher_RunAsync_Windows_UI_Core_CoreDispatcherPriority_Windows_UI_Core_DispatchedHandler_).  

[!code-cs[HelperOrientationChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetHelperOrientationChanged)]

## <a name="capture-a-photo-with-orientation-data"></a>Capturer une photo avec des données d’orientation
L’article [**Capture photo, vidéo et audio de base à l’aide de MediaCapture**](basic-photo-video-and-audio-capture-with-mediacapture.md) vous explique comment capturer une photo dans un fichier, via une capture préalable dans un flux en mémoire, puis l’utilisation d’un décodeur dédié à la lecture des données de l’image du flux et d’un encodeur dédié au transcodage des données d’image dans un fichier. Les données d’orientation, obtenues à partir de la classe **CameraRotationHelper**, peuvent être ajoutées au fichier d’image durant l’opération de transcodage.

Dans l’exemple suivant, une photo est capturée dans une classe [**InMemoryRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.InMemoryRandomAccessStream) avec un appel à [**CapturePhotoToStreamAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.capturephototostreamasync), tandis qu’un élément [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) est généré à partir du flux. Ensuite, un élément [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) est créé et ouvert afin de récupérer un [**IRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IRandomAccessStream) dédié à l’écriture sur le fichier. 

Avant de procéder au transcodage du fichier, l’orientation de la photo est récupérée de la méthode de la classe d’assistance **GetCameraCaptureOrientation**. Cette méthode renvoie un objet [**SimpleOrientation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.SimpleOrientation) qui est converti en objet [**PhotoOrientation**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.PhotoOrientation) avec la méthode d’assistance **ConvertSimpleOrientationToPhotoOrientation**. Ensuite, un nouvel objet **BitmapPropertySet** est créé et une propriété est ajoutée. Sa clé est « System.Photo.Orientation » et la valeur est l’orientation de la photo, exprimée en tant qu’élément **BitmapTypedValue**. « System.Photo.Orientation » est l’une des nombreuses propriétés Windows qui peuvent être ajoutées en tant que métadonnées à un fichier d’image. Pour consulter une liste de l’ensemble des propriétés associées à la photo, consultez la page [**Propriétés Windows - Photo**](https://docs.microsoft.com/previous-versions/ff516600(v=vs.85)). Pour plus d’informations sur l’utilisation des métadonnées dans les images, consultez la page [**Métadonnées d’image**](image-metadata.md).

Enfin, l’ensemble de propriétés comprenant les données d’orientation est défini pour l’encodeur à l’aide d’un appel à [**SetPropertiesAsync**](https://docs.microsoft.com/windows/uwp/develop/) et l’image est transcodée à l’aide d’un appel à [**FlushAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync).

[!code-cs[CapturePhotoWithOrientation](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCapturePhotoWithOrientation)]

## <a name="capture-a-video-with-orientation-data"></a>Capturer une vidéo avec des données d’orientation
La capture de vidéo de base est décrite dans l’article [**Capture photo, vidéo et audio de base à l’aide de MediaCapture**](basic-photo-video-and-audio-capture-with-mediacapture.md). L’ajout de données d’orientation à l’encodage de la vidéo capturée s’effectue au moyen de la technique décrite précédemment dans cette section, relative à l’ajout des données d’orientation au flux d’aperçu.

Dans l’exemple suivant, un fichier créé servira de support à l’écriture de la vidéo capturée. Un profil d’encodage MP4 est généré à l’aide de la méthode statique [**CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4). L’orientation appropriée de la vidéo est obtenue de la classe **CameraRotationHelper** avec un appel à **GetCameraCaptureOrientation**. La propriété de rotation nécessitant l’expression de l’orientation en degrés anti-horaire, la méthode d’assistance **ConvertSimpleOrientationToClockwiseDegrees** est appelée pour convertir la valeur d’orientation. Ensuite, créez le GUID représentant l’attribut Media Foundation Transform (MFT) pour la rotation du flux vidéo. Dans C++, vous pouvez utiliser la constante [**MF_MT_VIDEO_ROTATION**](https://docs.microsoft.com/windows/desktop/medfound/mf-mt-video-rotation), mais dans C# vous devez spécifier manuellement la valeur GUID. Ajoutez une valeur de propriété à l’objet des propriétés du flux, en spécifiant le GUID en tant que clé et la rotation en tant que valeur. Enfin, appelez [**StartRecordToStorageFileAsync**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture#Windows_Media_Capture_MediaCapture_StartRecordToStorageFileAsync_Windows_Media_MediaProperties_MediaEncodingProfile_Windows_Storage_IStorageFile_) afin de démarrer l’enregistrement de la vidéo encodée avec les données d’orientation.

[!code-cs[StartRecordingWithOrientationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartRecordingWithOrientationAsync)]

## <a name="camerarotationhelper-full-code-listing"></a>Code complet CameraRotationHelper
L’extrait de code suivant répertorie le code complet de la classe **CameraRotationHelper** qui gère les capteurs d’orientation du matériel, calcule les valeurs appropriées d’orientation pour les photos et les vidéos et fournit des méthodes d’assistance pour convertir entre les différentes représentations d’orientation utilisées par diverses fonctions Windows. Si vous suivez les recommandations fournies dans l’article ci-dessus, vous pouvez ajouter cette classe à votre projet en l’état, sans apporter de modifications. Bien entendu, n’hésitez pas à personnaliser le code suivant afin de satisfaire les exigences de votre scénario spécifique.

Cette classe d’assistance utilise la variable [**SimpleOrientationSensor**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.SimpleOrientationSensor) de l’appareil afin de déterminer l’orientation courante du châssis d’appareil et la classe [**DisplayInformation**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayInformation) afin de déterminer l’orientation courante de l’affichage. Chacune de ces classes fournit des événements qui sont déclenchés quand l’orientation courante est modifiée. Le panneau sur lequel l’appareil de capture est monté (frontal, arrière ou externe) est utilisé pour déterminer si le flux d’aperçu doit être mis en miroir. Par ailleurs, la propriété [**EnclosureLocation.RotationAngleInDegreesClockwise**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.enclosurelocation.rotationangleindegreesclockwise), prise en charge par certains appareils, est utilisée pour déterminer l’orientation selon laquelle l’appareil photo est monté sur les châssis.

Les méthodes suivantes peuvent être utilisées pour récupérer les valeurs recommandées d’orientation pour les tâches spécifiques de l’application Caméra :

* **GetUIOrientation** : renvoie l’orientation suggérée pour les éléments d’interface utilisateur d’appareil photo.
* **GetCameraCaptureOrientation** : renvoie l’orientation suggérée pour l’encodage dans les métadonnées de l’image.
* **GetCameraPreviewOrientation** : renvoie l’orientation suggérée pour le flux d’aperçu, afin de fournir une expérience utilisateur naturelle.

[!code-cs[CameraRotationHelperFull](./code/SimpleCameraPreview_Win10/cs/CameraRotationHelper.cs#SnippetCameraRotationHelperFull)]



## <a name="related-topics"></a>Rubriques connexes

* [Appareil photo](camera.md)
* [Photo de base, vidéo, audio et de capture à MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




