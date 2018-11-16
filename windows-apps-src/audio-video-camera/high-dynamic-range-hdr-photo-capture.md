---
author: drewbatgit
ms.assetid: 0186EA01-8446-45BA-A109-C5EB4B80F368
description: Cet article vous explique comment utiliser la classe AdvancedPhotoCapture pour capturer des photos avec plage dynamique élevée(HDR)et en basse lumière.
title: Capture de photos avec plage dynamique étendue (HDR) et en faible luminosité
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cc81fdc03096dd8cac5e844a496b38da78eb6136
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6970121"
---
# <a name="high-dynamic-range-hdr-and-low-light-photo-capture"></a>Capture de photos avec plage dynamique étendue (HDR) et en faible luminosité



Cet article vous explique comment utiliser la classe [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) pour capturer des photos avec plage dynamique élevée (HDR). Cette API permet aussi d’obtenir une image de référence à partir de la capture HDR avant la fin du traitement de l’image finale.

Les articles suivants concernent également la capture HDR :

-   Vous pouvez utiliser la classe [**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/dn948902) pour permettre au système d’évaluer le contenu du flux d’aperçu de capture multimédia pour déterminer si le traitement HDR est susceptible d’améliorer le résultat de la capture. Pour plus d’informations, consultez la page [Analyse de scène pour MediaCapture](scene-analysis-for-media-capture.md).

-   Utilisez la classe [**HdrVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926680) pour capturer une vidéo à l’aide de l’algorithme de traitement HDR intégré de Windows. Pour plus d’informations, voir [Contrôles de l’appareil de capture pour la vidéo](capture-device-controls-for-video-capture.md).

-   Vous pouvez utiliser la [**VariablePhotoSequenceCapture**](https://msdn.microsoft.com/library/windows/apps/dn652564) pour capturer une séquence de photos, chacune avec des paramètres différents, et implémenter votre propre HDR ou tout autre algorithme de traitement. Pour plus d’informations, voir [Séquence de photos variables](variable-photo-sequence.md).



> [!NOTE] 
> À partir de Windows10, version1709, l’enregistrement vidéo et l’utilisation de **AdvancedPhotoCapture** en simultané sont pris en charge.  Ce n’est pas le cas dans les versions précédentes. Ce changement signifie que vous pouvez avoir un objet **[LowLagMediaRecording](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagmediarecording)** préparé et un objet **[AdvancedPhotoCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.advancedphotocapture)** en même temps. Vous pouvez démarrer ou arrêter l’enregistrement vidéo entre les appels à **[MediaCapture.PrepareAdvancedPhotoCaptureAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.prepareadvancedphotocaptureasync)** et à **[AdvancedPhotoCapture.FinishAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.advancedphotocapture.FinishAsync)**. Vous pouvez également appeler **[AdvancedPhotoCapture.CaptureAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.advancedphotocapture.CaptureAsync)** pendant l’enregistrement vidéo. Toutefois, certains scénarios **AdvancedPhotoCapture**, comme la capture d’une photo HDR pendant l’enregistrement vidéo peut provoquer l’altération de certaines trames vidéo par la capture HDR, ce qui entraîne une expérience utilisateur négative. Pour cette raison, la liste des modes retournés par **[AdvancedPhotoControl.SupportedModes](https://docs.microsoft.com/uwp/api/windows.media.devices.advancedphotocontrol.SupportedModes)** sera différente pendant l’enregistrement vidéo. Vous devez vérifier cette valeur immédiatement après le démarrage ou l’arrêt de l’enregistrement vidéo pour vous assurer que le mode souhaité est pris en charge dans l’état d’enregistrement vidéo actuel.


> [!NOTE] 
> À partir de Windows10, version1709, lorsque l’objet **AdvancedPhotoCapture** est défini sur le mode HDR, la définition de la propriété [**FlashControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.FlashControl.Enabled) est ignorée et le Flash n’est jamais déclenché. Pour les autres modes de capture, si la propriété **FlashControl.Enabled** est définie sur true, elle remplace les paramètres **AdvancedPhotoCapture** et entraîne la capture avec flash d’une photo standard. Si [**Auto**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.FlashControl.Auto) est défini sur true, l’objet **AdvancedPhotoCapture** peut ou non utiliser un flash, en fonction du comportement par défaut du pilote de l’appareil photo pour les conditions de la scène actuelle. Dans les versions précédentes, le paramètre de flash **AdvancedPhotoCapture** remplace toujours le paramètre **FlashControl.Enabled**.

> [!NOTE] 
> Cet article repose sur les concepts et le code décrits dans [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), qui décrit comment implémenter la capture photo et vidéo de base. Nous vous recommandons de vous familiariser avec le modèle de capture simple de contenu multimédia de cet article avant d’adopter des scénarios de capture plus avancés. Le code de cet article repose sur l’hypothèse que votre application possède déjà une instance de MediaCapture initialisée correctement.

Un exemple Windows universel démontre l’utilisation de la classe **AdvancedPhotoCapture**, que vous pouvez utiliser pour voir l’API employée en contexte ou en tant que point de départ pour votre propre application. Pour plus d’informations, consultez la section [Exemple de capture avancée d’appareil photo](http://go.microsoft.com/fwlink/?LinkID=620517).

## <a name="advanced-photo-capture-namespaces"></a>Espaces de noms de capture avancée d’appareil photo

Les exemples de code de cet article utilisent les API des espaces de noms suivants en plus des espaces de noms requis pour la capture multimédia de base.

[!code-cs[HDRPhotoUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHDRPhotoUsing)]

## <a name="hdr-photo-capture"></a>Capture photo HDR

### <a name="determine-if-hdr-photo-capture-is-supported-on-the-current-device"></a>Déterminer si la capture photo HDR est prise en charge sur l’appareil actuel

La technique de capture HDR décrite dans cet article est effectuée à l’aide de l’objet [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386). Tous les appareils ne prennent pas en charge la capture HDR avec **AdvancedPhotoCapture**. Déterminez si l’appareil sur lequel votre application est en cours d’exécution prend en charge la technique en obtenant l’objet **MediaCapture** et sa [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825), puis en obtenant la propriété [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840). Vérifiez la collection [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/mt147844) du contrôleur de l’appareil vidéo pour voir si elle inclut [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845). Si tel est le cas, la capture HDR à l’aide de **AdvancedPhotoCapture** est prise en charge.

[!code-cs[HdrSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHdrSupported)]

### <a name="configure-and-prepare-the-advancedphotocapture-object"></a>Configurer et préparer l’objet AdvancedPhotoCapture

Étant donné que vous devrez accéder à l’instance [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) à partir de plusieurs emplacements dans votre code, vous devez déclarer une variable membre pour stocker l’objet.

[!code-cs[DeclareAdvancedCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareAdvancedCapture)]

Dans votre application, une fois que vous avez initialisé l’objet **MediaCapture**, créez un objet [**AdvancedPhotoCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/mt147837) et définissez le mode sur [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845). Appelez la méthode [**Configure**](https://msdn.microsoft.com/library/windows/apps/mt147841) de l’objet [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840), en passant l’objet **AdvancedPhotoCaptureSettings** que vous avez créé.

Appelez l’objet **MediaCapture** et sa méthode [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403), en passant un objet [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) indiquant le type d’encodage que la capture doit utiliser. La classe **ImageEncodingProperties** fournit des méthodes statiques pour la création d’encodages d’image pris en charge par **MediaCapture**.

**PrepareAdvancedPhotoCaptureAsync** renvoie l’objet [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) que vous allez utiliser pour lancer la capture photo. Vous pouvez utiliser cet objet pour enregistrer des gestionnaires pour [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) et pour [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387) qui sont décrits plus loin dans cet article.

[!code-cs[CreateAdvancedCaptureAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateAdvancedCaptureAsync)]

### <a name="capture-an-hdr-photo"></a>Capturer une photo HDR

Capturer une photo HDR en appelant l’objet [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) et sa méthode [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388). Cette méthode renvoie un objet [**AdvancedCapturedPhoto**](https://msdn.microsoft.com/library/windows/apps/mt181378) qui fournit la photo capturée dans sa propriété [**Frame**](https://msdn.microsoft.com/library/windows/apps/mt181382).

[!code-cs[CaptureHdrPhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureHdrPhotoAsync)]

La plupart des applications de photographie tentent d’encoder la rotation d’une photo capturée dans le fichier image, de manière à ce qu’elle soit affichée correctement par d’autres applications et appareils. Cet exemple illustre l’utilisation de la classe d’assistance **CameraRotationHelper** pour calculer l’orientation correcte pour le fichier. Cette classe est décrite et consignée dans son ensemble dans l’article [**Gérer l’orientation de l’appareil à l’aide de MediaCapture**](handle-device-orientation-with-mediacapture.md).

La méthode d’assistance **SaveCapturedFrameAsync**, qui enregistre l’image sur un disque, est décrite plus loin dans cet article.

### <a name="get-optional-reference-frame"></a>Obtenir l’image de référence facultative

Le processus HDR capture plusieurs images, puis les transforme en une seule image une fois toutes les images capturées. Vous pouvez accéder à une image après l’avoir capturée, mais sans devoir attendre la fin du processus HDR, en gérant l’événement [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392). Cette étape n’est pas nécessaire si vous êtes uniquement intéressé par le résultat de la photo HDR finale.

> [!IMPORTANT]
> [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) n’est pas déclenché sur les appareils qui prennent en charge la plage HDR matérielle et qui, par conséquent, ne génèrent pas d’images de référence. Votre application doit gérer le cas où cet événement n’est pas déclenché.

Étant donné que l’image de référence arrive hors du contexte de l’appel à **CaptureAsync**, un mécanisme permet de passer des informations de contexte au gestionnaire **OptionalReferencePhotoCaptured**. Vous devez tout d’abord appeler un objet qui contiendra vos informations de contexte. À vous de décider du nom et du contenu de cet objet. Cet exemple définit un objet qui possède des membres pour effectuer le suivi du nom de fichier et de l’orientation de l’appareil photo de la capture.

[!code-cs[AdvancedCaptureContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAdvancedCaptureContext)]

Créez une instance de votre objet de contexte, remplissez ses membres et passez-la dans la surcharge de [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388) qui accepte un objet en tant que paramètre.

[!code-cs[CaptureWithContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureWithContext)]

Dans le gestionnaire d’événements [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392), diffusez la propriété [**Context**](https://msdn.microsoft.com/library/windows/apps/mt181405) de l’objet [**OptionalReferencePhotoCapturedEventArgs**](https://msdn.microsoft.com/library/windows/apps/mt181404) sur votre classe d’objet de contexte. Cet exemple modifie le nom de fichier pour distinguer l’image de référence de l’image HDR finale, puis appelle la méthode d’assistance **SaveCapturedFrameAsync** pour enregistrer l’image.

[!code-cs[OptionalReferencePhotoCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOptionalReferencePhotoCaptured)]

### <a name="receive-a-notification-when-all-frames-have-been-captured"></a>Recevoir une notification lorsque toutes les images ont été capturées

La capture photo HDR comporte deux étapes. Plusieurs images sont capturées, puis ensuite traitées pour être transformées en image HDR finale. Vous ne pouvez pas initier une autre capture tant que les images HDR sources sont en cours de capture, mais vous pouvez lancer une capture une fois toutes les images capturées, avant que le post-traitement HDR ne soit terminé. L’événement [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387) est déclenché lorsque les captures HDR sont terminées, vous indiquant que vous pouvez lancer une autre capture. Un scénario courant consiste à désactiver le bouton de capture de votre interface utilisateur au début de la capture HDR, pour ensuite le réactiver lorsque **AllPhotosCaptured** est déclenché.

[!code-cs[AllPhotosCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAllPhotosCaptured)]

### <a name="clean-up-the-advancedphotocapture-object"></a>Nettoyer l’objet AdvancedPhotoCapture

Lorsque votre application a terminé la capture, avant d’éliminer l’objet **MediaCapture**, vous devez arrêter l’objet [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) en appelant [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/mt181391) et en définissant la variable membre sur null.

[!code-cs[CleanUpAdvancedPhotoCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpAdvancedPhotoCapture)]


## <a name="low-light-photo-capture"></a>Capture photo en basse lumière
À partir de Windows 10, version 1607, **AdvancedPhotoCapture** peut être utilisé pour capturer des photos à l’aide d’un algorithme intégré qui améliore la qualité des photos prises dans des conditions de faible luminosité. Quand vous utilisez la fonctionnalité de faible éclairage de la classe [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture), le système évalue la scène courante et si nécessaire applique un algorithme destiné à compenser les conditions de faible éclairage. Si le système détermine que l’algorithme n’est pas nécessaire, une capture standard est effectuée à la place.

Avant de solliciter une capture de photo en basse lumière, déterminez si l’appareil sur lequel votre application est en cours d’exécution prend en charge la technique en obtenant l’objet **MediaCapture** et sa [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825), puis en obtenant la propriété [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840). Vérifiez la collection [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/mt147844) du contrôleur d’appareil vidéo pour voir si elle inclut [**AdvancedPhotoMode.LowLight**](https://msdn.microsoft.com/library/windows/apps/mt147845). Si tel est le cas, la capture en basse lumière à l’aide de **AdvancedPhotoCapture** est prise en charge. 
[!code-cs[LowLightSupported1](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetLowLightSupported1)]

[!code-cs[LowLightSupported2](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetLowLightSupported2)]

Ensuite, déclarez une variable de membre dédiée au stockage de l’objet **AdvancedPhotoCapture**. 

[!code-cs[DeclareAdvancedCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareAdvancedCapture)]

Dans votre application, une fois l’objet **MediaCapture** initialisé, créez un objet [**AdvancedPhotoCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/mt147837)  et définissez le mode sur [**AdvancedPhotoMode.LowLight**](https://msdn.microsoft.com/library/windows/apps/mt147845). Appelez l’objet [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840) et sa méthode [**Configure**](https://msdn.microsoft.com/library/windows/apps/mt147841), en passant l’objet **AdvancedPhotoCaptureSettings** que vous avez créé.

Appelez l’objet **MediaCapture** et sa méthode [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403), en passant un objet [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) indiquant le type d’encodage que la capture doit utiliser. 

[!code-cs[CreateAdvancedCaptureLowLightAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateAdvancedCaptureLowLightAsync)]

Pour capturer une photo, appelez [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.CaptureAsync).

[!code-cs[CaptureLowLight](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureLowLight)]

Comme dans l’exemple HDR ci-dessus, cet exemple utilise une classe d’assistance appelée **CameraRotationHelper** afin de déterminer la valeur de rotation à encoder dans l’image pour que cette dernière s’affiche correctement sur d’autres applications et appareils. Cette classe est décrite et consignée dans son ensemble dans l’article [**Gérer l’orientation de l’appareil à l’aide de MediaCapture**](handle-device-orientation-with-mediacapture.md).

La méthode d’assistance **SaveCapturedFrameAsync**, qui enregistre l’image sur un disque, est décrite plus loin dans cet article.

Vous pouvez capturer plusieurs photos en basse lumière sans reconfigurer l’objet **AdvancedPhotoCapture**, mais quand vous avez terminé la capture, vous devez appeler [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.FinishAsync) afin de nettoyer l’objet et les ressources associées.

[!code-cs[CleanUpAdvancedPhotoCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpAdvancedPhotoCapture)]

## <a name="working-with-advancedcapturedphoto-objects"></a>Utilisation des objets AdvancedCapturedPhoto
[**AdvancedPhotoCapture.CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.CaptureAsync) renvoie un objet [**AdvancedCapturedPhoto**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedCapturedPhoto) représentant la photo capturée. Cet objet expose la propriété [**Frame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedCapturedPhoto.Frame), qui renvoie un objet [**CapturedFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame) représentant l’image. L’événement [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.OptionalReferencePhotoCaptured) fournit également un objet **CapturedFrame** dans ses arguments d’événement. Une fois que vous avez récupéré un objet de ce type, vous pouvez effectuer diverses actions, notamment la création d’une méthode [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap) ou l’enregistrement de l’image dans un fichier. 

## <a name="get-a-softwarebitmap-from-a-capturedframe"></a>Obtenir une classe SoftwareBitmap à partir d’un CapturedFrame
Il n’est pas difficile de récupérer une méthode **SoftwareBitmap** d’un objet **CapturedFrame**. Pour ce faire, accédez à la propriété [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame.SoftwareBitmap) de l’objet. Toutefois, la plupart des formats d’encodage ne prenant pas en charge **SoftwareBitmap** avec **AdvancedPhotoCapture**, vous devez examiner la propriété et vérifier qu’elle n’est pas définie sur null avant de l’utiliser.

[!code-cs[SoftwareBitmapFromCapturedFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmapFromCapturedFrame)]

Dans la version actuelle, le seul format d’encodage qui prenne en charge **SoftwareBitmap** pour **AdvancedPhotoCapture** est le format NV12 non compressé. Par conséquent, si vous souhaitez utiliser cette fonctionnalité, vous devez spécifier cet encodage lorsque vous appelez [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403). 

[!code-cs[UncompressedNv12](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUncompressedNv12)]

Bien entendu, vous pouvez toujours enregistrer l’image dans un fichier, avant de charger le fichier dans une méthode **SoftwareBitmap** dans le cadre d’une étape distincte. Pour plus d’informations sur l’utilisation de la méthode **SoftwareBitmap**, consultez la section [**Créer, modifier et enregistrer des images bitmap**](imaging.md).

## <a name="save-a-capturedframe-to-a-file"></a>Enregistrer une classe CapturedFrame dans un fichier
La classe [**CapturedFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame) implémente l’interface IInputStream interface, afin qu’elle puisse être utilisée en tant qu’entrée pour une instance [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapDecoder). Ensuite, une classe [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder) peut être utilisée pour écrire les données de l’image sur le disque.

Dans l’exemple suivant, un nouveau dossier est créé dans la bibliothèque d’image de l’utilisateur; un fichier est créé dans ce dossier. Notez que votre application devra inclure la fonctionnalité **Bibliothèque d’images** dans votre fichier de manifeste d’application afin d’accéder à ce répertoire. Un flux de fichier est alors ouvert sur le fichier spécifié. Ensuite, la méthode [**BitmapDecoder.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapDecoder.CreateAsync) est appelée pour la création du décodeur de **CapturedFrame**. La méthode [**CreateForTranscodingAsync**](https://msdn.microsoft.com/library/windows/apps/br226214) crée un encodeur à partir du flux de fichier et du décodeur.

Les étapes suivantes sont dédiées à l’encodage de l’orientation de la photo dans le fichier image à l’aide de la classe [**BitmapProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.BitmapProperties) de l’encodeur. Pour plus d’informations sur le traitement de l’orientation durant la capture d’images, consultez la section [**Gérer l’orientation de l’appareil à l’aide de MediaCapture**](handle-device-orientation-with-mediacapture.md).

Enfin, l’image est écrite sur le fichier avec un appel à [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.FlushAsync).

[!code-cs[SaveCapturedFrameAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSaveCapturedFrameAsync)]

## <a name="related-topics"></a>Rubriquesassociées

* [Appareil photo](camera.md)
* [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
