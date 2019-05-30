---
ms.assetid: 7DBEE5E2-C3EC-4305-823D-9095C761A1CD
description: Cet article indique comment capturer une séquence de photos variables pour capturer plusieurs trames d’images en une succession rapide et configurer des paramètres de mise au point, de flash, de sensibilité ISO, d’exposition et de compensation de l’exposition différents pour chaque trame.
title: Séquence de photos variables
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 506ea5f7fe199c3df0b6089da73a108d53d98ef3
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361374"
---
# <a name="variable-photo-sequence"></a>Séquence de photos variables



Cet article indique comment capturer une séquence de photos variables pour capturer plusieurs trames d’images en une succession rapide et configurer des paramètres de mise au point, de flash, de sensibilité ISO, d’exposition et de compensation de l’exposition différents pour chaque trame. Cette fonctionnalité permet par exemple de créer des images HDR (High Dynamic Range).

Si vous souhaitez capturer des images HDR, mais que vous ne voulez pas implémenter votre propre algorithme de traitement, vous pouvez utiliser l’API [**AdvancedPhotoCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) pour utiliser les fonctionnalités HDR intégrées dans Windows. Pour plus d’informations, voir [Capture photo avec plage dynamique élevée (HDR)](high-dynamic-range-hdr-photo-capture.md).

> [!NOTE] 
> Cet article repose sur les concepts et le code décrits dans [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), qui décrit comment implémenter la capture photo et vidéo de base. Nous vous recommandons de vous familiariser avec le modèle de capture multimédia de base dans cet article avant de passer à des scénarios de capture plus avancés. Le code de cet article part du principe que votre application possède déjà une instance de MediaCapture initialisée correctement.

## <a name="set-up-your-app-to-use-variable-photo-sequence-capture"></a>Configurer votre application pour utiliser une capture de séquence de photos variables

Outre les espaces de noms requis pour la capture multimédia de base, l’implémentation d’une capture de séquence de photos variables requiert les espaces de noms suivants.

[!code-cs[VPSUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVPSUsing)]

Déclarez une variable membre pour stocker l’objet [**VariablePhotoSequenceCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Core.VariablePhotoSequenceCapture), utilisé pour initier la capture de la séquence de photos. Déclarez un tableau d’objets [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) pour stocker chaque image capturée dans la séquence. Déclarez également un tableau pour stocker l’objet [**CapturedFrameControlValues**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CapturedFrameControlValues) pour chaque trame. Ce dernier peut être utilisé par votre algorithme de traitement d’image afin de déterminer les paramètres utilisés pour la capture de chaque trame. Enfin, déclarez un index qui sera utilisé pour suivre l’image en cours de capture dans la séquence.

[!code-cs[VPSMemberVariables](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVPSMemberVariables)]

## <a name="prepare-the-variable-photo-sequence-capture"></a>Préparer la capture de la séquence de photos variables

Une fois que vous avez initialisé votre [MediaCapture](capture-photos-and-video-with-mediacapture.md), assurez-vous que les séquences de photos variables sont prises en charge sur l’appareil actuel en obtenant une instance de l’élément [**VariablePhotoSequenceController**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.Core.VariablePhotoSequenceController)à partir de l’objet [**VideoDeviceController**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.VideoDeviceController) de la capture multimédia et en activant la propriété [**Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.variablephotosequencecontroller.supported).

[!code-cs[IsVPSSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsVPSSupported)]

Obtenez un objet [**FrameControlCapabilities**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.Core.FrameControlCapabilities) à partir du contrôleur de séquence de photos variables. Cet objet possède une propriété pour chaque paramètre pouvant être configuré par trame d’une séquence de photos. Par exemple :

-   [**Exposition**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.framecontrolcapabilities.exposure)
-   [**ExposureCompensation**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.framecontrolcapabilities.exposurecompensation)
-   [**Flash**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.framecontrolcapabilities.flash)
-   [**Focus**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.framecontrolcapabilities.focus)
-   [**IsoSpeed**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.framecontrolcapabilities.isospeed)
-   [**PhotoConfirmation**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.framecontrolcapabilities.photoconfirmationsupported)

Cet exemple définit une valeur de compensation d’exposition différente pour chaque trame. Pour vérifier que la compensation d’exposition est prise en charge pour les séquences de photos sur l’appareil actuel, vérifiez la propriété [**Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.exposurecompensationcontrol.supported) de l’objet [**FrameExposureCompensationCapabilities**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.Core.FrameExposureCompensationCapabilities) accessible par le biais de la propriété **ExposureCompensation**.

[!code-cs[IsExposureCompensationSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsExposureCompensationSupported)]

Créez un objet [**FrameController**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.Core.FrameController) pour chaque trame que vous souhaitez capturer. Cet exemple capture trois trames. Définissez les valeurs des contrôles que vous souhaitez faire varier pour chaque trame. Ensuite, supprimez la collection [**DesiredFrameControllers**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.variablephotosequencecontroller.desiredframecontrollers) de l’objet **VariablePhotoSequenceController** et ajoutez chaque contrôleur de trame à la collection.

[!code-cs[InitFrameControllers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitFrameControllers)]

Créez un objet [**ImageEncodingProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties) pour définir le codage que vous souhaitez utiliser pour les images capturées. Appelez la méthode statique [**MediaCapture.PrepareVariablePhotoSequenceCaptureAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.preparevariablephotosequencecaptureasync) en indiquant les propriétés de codage. Cette méthode renvoie un objet [**VariablePhotoSequenceCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Core.VariablePhotoSequenceCapture). Enfin, enregistrez des gestionnaires d’événements pour les événements [**PhotoCaptured**](https://docs.microsoft.com/uwp/api/windows.media.capture.core.variablephotosequencecapture.photocaptured) et [**Stopped**](https://docs.microsoft.com/uwp/api/windows.media.capture.core.variablephotosequencecapture.stopped).

[!code-cs[PrepareVPS](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPrepareVPS)]

## <a name="start-the-variable-photo-sequence-capture"></a>Démarrer la capture de la séquence de photos variables

Pour démarrer la capture de la séquence de photos variables, appelez [**VariablePhotoSequenceCapture.StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.core.variablephotosequencecapture.startasync). Veillez à initialiser les tableaux pour stocker les images capturées et les valeurs de contrôle de trame et à définir l’index actuel sur la valeur 0. Définissez la variable d’état d’enregistrement de votre application et mettez à jour votre interface utilisateur pour désactiver le démarrage d’une autre capture pendant que cette capture est en cours.

[!code-cs[StartVPSCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStartVPSCapture)]

## <a name="receive-the-captured-frames"></a>Recevoir les trames capturées

L’événement [**PhotoCaptured**](https://docs.microsoft.com/uwp/api/windows.media.capture.core.variablephotosequencecapture.photocaptured) est déclenché pour chaque trame capturée. Enregistrez les valeurs de contrôle de trame et l’image capturée pour la trame, puis augmentez l’index de trame actuel. Cet exemple indique comment obtenir une représentation [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) de chaque trame. Pour plus d’informations sur l’utilisation de **SoftwareBitmap**, voir [Acquisition d’images](imaging.md).

[!code-cs[OnPhotoCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnPhotoCaptured)]

## <a name="handle-the-completion-of-the-variable-photo-sequence-capture"></a>Achever la capture de la séquence de photos variables

L’événement [**Stopped**](https://docs.microsoft.com/uwp/api/windows.media.capture.core.variablephotosequencecapture.stopped) est déclenché lorsque toutes les trames de la séquence ont été capturées. Mettez à jour l’état d’enregistrement de votre application et votre interface utilisateur pour permettre à l’utilisateur de créer de nouvelles captures. À ce stade, vous pouvez transférer les images capturées et les valeurs de contrôle de trame dans votre code de traitement d’image.

[!code-cs[OnStopped](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnStopped)]

## <a name="update-frame-controllers"></a>Mettre à jour des contrôleurs de trame

Si vous souhaitez effectuer une autre capture de séquence de photos variables avec différents paramètres de trame, vous n’avez pas besoin de réinitialiser complètement l’élément **VariablePhotoSequenceCapture**. Vous pouvez supprimer la collection [**DesiredFrameControllers**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.variablephotosequencecontroller.desiredframecontrollers) et ajouter de nouveaux contrôleurs de trame ou bien modifier les valeurs du contrôleur de trame existant. L’exemple suivant active l’objet [**FrameFlashCapabilities**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.Core.FrameFlashCapabilities) pour vérifier que l’appareil actuel prend en charge le flash et la puissance du flash pour les trames de séquences de photos variables. Si c’est le cas, chaque trame est mise à jour pour activer le flash à 100 %. Les valeurs de compensation d’exposition précédemment définies pour chaque trame sont toujours actives.

[!code-cs[UpdateFrameControllers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUpdateFrameControllers)]

## <a name="clean-up-the-variable-photo-sequence-capture"></a>Nettoyer la capture de la séquence de photos variables

Lorsque vous avez terminé la capture de séquences de photos variables ou que votre application est mise en suspens, nettoyez l’objet de séquence de photos variables en appelant [**FinishAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.core.variablephotosequencecapture.finishasync). Annulez l’inscription des gestionnaires d’événements de l’objet et définissez-les sur la valeur Null.

[!code-cs[CleanUpVPS](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpVPS)]

## <a name="related-topics"></a>Rubriques connexes

* [Appareil photo](camera.md)
* [Photo de base, vidéo, audio et de capture à MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




