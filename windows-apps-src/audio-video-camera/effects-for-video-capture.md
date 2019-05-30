---
ms.assetid: E0189423-1DF3-4052-AB2E-846EA18254C4
description: Cette rubrique vous montre comment appliquer des effets aux flux de prévisualisation et d’enregistrement vidéo de l’appareil photo. Elle explique également comment utiliser l’effet de stabilisation vidéo.
title: Effets de capture vidéo
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ea79794f18da01438392bb0bc37b127668a53538
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361768"
---
# <a name="effects-for-video-capture"></a>Effets de capture vidéo


Cette rubrique vous montre comment appliquer des effets aux flux de prévisualisation et d’enregistrement vidéo de l’appareil photo. Elle explique également comment utiliser l’effet de stabilisation vidéo.

> [!NOTE] 
> Cet article repose sur les concepts et le code décrits dans [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), qui décrit comment implémenter la capture photo et vidéo de base. Nous vous recommandons de vous familiariser avec le modèle de capture simple de contenu multimédia de cet article avant d’adopter des scénarios de capture plus avancés. Le code de cet article part du principe que votre application possède déjà une instance de MediaCapture initialisée correctement.

## <a name="adding-and-removing-effects-from-the-camera-video-stream"></a>Ajout et suppression d’effets dans le flux vidéo de l’appareil photo
Pour capturer ou prévisualiser une vidéo à partir de l’appareil photo de votre appareil, vous utilisez l’objet [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) comme décrit dans [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md). Une fois que vous avez initialisé l’objet **MediaCapture**, vous pouvez ajouter un ou plusieurs effets vidéo au flux de prévisualisation ou de capture en appelant la méthode [**AddVideoEffectAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync), qui transmet un objet [**IVideoEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IVideoEffectDefinition) représentant l’effet à ajouter, ainsi qu’un membre de l’énumération [**MediaStreamType**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaStreamType) indiquant si l’effet doit être ajouté au flux de prévisualisation ou au flux d’enregistrement de l’appareil photo.

> [!NOTE]
> Sur certains appareils, le flux de prévisualisation et le flux de capture sont identiques, ce qui signifie que si vous spécifiez **MediaStreamType.VideoPreview** ou **MediaStreamType.VideoRecord** lorsque vous appelez **AddVideoEffectAsync**, l’effet s’appliquera aussi bien au flux de prévisualisation qu’au flux d’enregistrement. Vous pouvez déterminer si les flux de prévisualisation et d’enregistrement sont les mêmes sur l’appareil actuel en vérifiant la propriété [**VideoDeviceCharacteristic**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapturesettings.videodevicecharacteristic) de [**classe MediaCaptureSettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.mediacapturesettings) pour l’objet **MediaCapture**. Si la valeur de cette propriété est **VideoDeviceCharacteristic.AllStreamsIdentical** ou **VideoDeviceCharacteristic.PreviewRecordStreamsIdentical**, les flux sont identiques et tout effet que vous appliquerez à l’un se répercutera sur l’autre.

L’exemple suivant ajoute un effet à la fois au flux de prévisualisation et au flux d’enregistrement de l’appareil photo. Cet exemple explique comment vérifier si les flux d’enregistrement et de prévisualisation sont identiques.

[!code-cs[BasicAddEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetBasicAddEffect)]

Notez que **AddVideoEffectAsync** retourne un objet qui implémente [**IMediaExtension**](https://docs.microsoft.com/uwp/api/Windows.Media.IMediaExtension) représentant l’effet vidéo ajouté. Certains effets vous permettent de modifier les paramètres d’effet en passant un [**PropertySet**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.PropertySet) dans la méthode [**SetProperties**](https://docs.microsoft.com/uwp/api/windows.media.imediaextension.setproperties).

À partir de Windows 10, version 1607, vous pouvez également utiliser l’objet retourné par **AddVideoEffectAsync** pour supprimer l’effet du pipeline vidéo en le passant à [**RemoveEffectAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.removeeffectasync). **RemoveEffectAsync** détermine automatiquement si le paramètre d’objet de l’effet a été ajouté au flux de prévisualisation ou d’enregistrement, ce qui signifie que vous n’avez pas besoin de spécifier le type de flux lors de l’appel.

[!code-cs[RemoveOneEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetRemoveOneEffect)]

Vous pouvez également supprimer tous les effets du flux de prévisualisation ou de capture en appelant la méthode [**ClearEffectsAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.cleareffectsasync) et en spécifiant le flux pour lequel tous les effets doivent être supprimés.

[!code-cs[ClearAllEffects](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetClearAllEffects)]

## <a name="video-stabilization-effect"></a>Effet de stabilisation vidéo

L’effet de stabilisation vidéo manipule les cadres d’un flux vidéo pour réduire les secousses provoquées lorsque vous tenez l’appareil de capture dans votre main. Étant donné que cette technique décale les pixels vers la droite, vers la gauche, vers le haut ou vers le bas, et que l’effet ne peut pas déterminer quel est le contenu à l’extérieur du cadre vidéo, la vidéo stabilisée est légèrement rognée par rapport à la vidéo d’origine. Une fonction utilitaire est fournie pour vous permettre d’ajuster vos paramètres d’encodage vidéo pour gérer au mieux le rognage généré par l’effet.

Sur les appareils qui la prennent en charge, la stabilisation d’image optique (OIS, Optical Image Stabilization) stabilise la vidéo en manipulant mécaniquement l’appareil de capture et, par conséquent, il n’est pas nécessaire de rogner les bords des images vidéo. Pour plus d’informations, voir [Contrôles de l’appareil de capture pour la vidéo](capture-device-controls-for-video-capture.md).

### <a name="set-up-your-app-to-use-video-stabilization"></a>Configurer votre application pour utiliser la stabilisation vidéo

Outre les espaces de noms nécessaires pour la capture multimédia de base, l’effet de stabilisation vidéo requiert les espaces de noms suivants.

[!code-cs[VideoStabilizationEffectUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetVideoStabilizationEffectUsing)]

Déclarez une variable membre pour stocker l’objet [**VideoStabilizationEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.VideoStabilizationEffect). Dans le cadre de l’implémentation de l’effet, vous allez modifier les propriétés d’encodage que vous utilisez pour coder la vidéo capturée. Déclarez deux variables pour stocker une copie de sauvegarde des données initiales et les propriétés d’encodage de sortie afin de pouvoir les restaurer ultérieurement lorsque l’effet sera désactivé. Enfin, déclarez une variable membre de type [**MediaEncodingProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile), car cet objet est accessible à partir de différents emplacements au sein de votre code.

[!code-cs[DeclareVideoStabilizationEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetDeclareVideoStabilizationEffect)]

Pour ce scénario, vous devez attribuer l’objet de profil d’encodage multimédia à une variable membre afin de pouvoir y accéder plus tard.

[!code-cs[EncodingProfileMember](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetEncodingProfileMember)]

### <a name="initialize-the-video-stabilization-effect"></a>Initialiser l’effet de stabilisation vidéo

Une fois l’objet **MediaCapture** initialisé, créez une instance de l’objet [**VideoStabilizationEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.VideoStabilizationEffectDefinition). Appelez [**MediaCapture.AddVideoEffectAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync) pour ajouter l’effet au pipeline vidéo et récupérer une instance de la classe [**VideoStabilizationEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.VideoStabilizationEffect). Spécifiez [**MediaStreamType.VideoRecord**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaStreamType) pour indiquer que l’effet doit être appliqué au flux d’enregistrement vidéo.

Inscrivez un gestionnaire d’événements pour l’événement [**EnabledChanged**](https://docs.microsoft.com/uwp/api/windows.media.core.videostabilizationeffect.enabledchanged) et appelez la méthode d’assistance **SetUpVideoStabilizationRecommendationAsync**. Ces deux thèmes seront décrits plus loin dans cet article. Enfin, définissez la propriété [**Enabled**](https://docs.microsoft.com/uwp/api/windows.media.core.videostabilizationeffect.enabled) de l’effet sur la valeur True pour activer l’effet.

[!code-cs[CreateVideoStabilizationEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetCreateVideoStabilizationEffect)]

### <a name="use-recommended-encoding-properties"></a>Utiliser les propriétés d’encodage recommandées

Comme indiqué précédemment dans cet article, la technique utilisée par l’effet de stabilisation vidéo provoque un rognage léger de la vidéo stabilisée par rapport à la vidéo source. Définissez la fonction d’assistance suivante dans votre code afin d’ajuster les propriétés d’encodage vidéo pour mieux gérer cette limitation provoquée par l’effet. Cette étape n’est pas requise pour utiliser l’effet de stabilisation vidéo, mais si vous ne l’effectuez pas, la vidéo sera légèrement améliorée et par conséquent, la fidélité visuelle ne sera plus d’aussi bonne qualité.

Appelez [**GetRecommendedStreamConfiguration**](https://docs.microsoft.com/uwp/api/windows.media.core.videostabilizationeffect.getrecommendedstreamconfiguration) dans l’instance d’effet de stabilisation vidéo, en passant l’objet [**VideoDeviceController**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.VideoDeviceController), qui informe l’effet sur les propriétés actuelles d’encodage de flux d’entrée, et votre **MediaEncodingProfile**, qui informe l’effet sur les propriétés actuelles d’encodage de sortie. Cette méthode renvoie un objet [**VideoStreamConfiguration**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.VideoStreamConfiguration) contenant de nouvelles recommandations de propriétés d’encodage de flux d’entrée et de sortie.

Lorsque l’appareil le permet, les propriétés d’encodage d’entrée recommandées ont une résolution plus élevée que les paramètres initiaux fournis, de manière à ce que la perte de résolution soit minimale une fois l’effet de rognage appliqué.

Appelez [**VideoDeviceController.SetMediaStreamPropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.media.devices.videodevicecontroller.setmediastreampropertiesasync) pour définir les nouvelles propriétés d’encodage. Avant de définir les nouvelles propriétés, utilisez la variable membre pour stocker les propriétés d’encodage initiales afin de pouvoir modifier de nouveau les paramètres lorsque l’effet sera désactivé.

Si l’effet de stabilisation vidéo doit rogner la vidéo de sortie, les propriétés d’encodage de sortie recommandées auront la taille de la vidéo rognée. Cela signifie que la résolution de sortie correspondra à la taille de la vidéo rognée. Si vous n’utilisez pas les propriétés de sortie recommandées, la vidéo est redimensionnée pour correspondre à la taille initiale de sortie, ce qui se traduit par une perte de fidélité visuelle.

Définissez la propriété [**Video**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.video) de l’objet **MediaEncodingProfile**. Avant de définir les nouvelles propriétés, utilisez la variable membre pour stocker les propriétés d’encodage initiales afin de pouvoir modifier de nouveau les paramètres lorsque l’effet sera désactivé.

[!code-cs[SetUpVideoStabilizationRecommendationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetSetUpVideoStabilizationRecommendationAsync)]

### <a name="handle-the-video-stabilization-effect-being-disabled"></a>Gérer la désactivation de l’effet de stabilisation vidéo

Le système peut désactiver automatiquement l’effet de stabilisation vidéo si le débit de pixels est trop élevé ou s’il détecte que l’effet s’exécute lentement. Dans ce cas, l’événement EnabledChanged est déclenché. L’instance **VideoStabilizationEffect** du paramètre *sender* indique le nouvel état de l’effet, activé ou désactivé. [  **VideoStabilizationEffectEnabledChangedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.VideoStabilizationEffectEnabledChangedEventArgs)a une valeur [**VideoStabilizationEffectEnabledChangedReason**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.VideoStabilizationEffectEnabledChangedReason) indiquant pourquoi l’effet a été activé ou désactivé. Cet événement est également déclenché si vous activez ou désactivez l’effet par programme, auquel cas la raison sera **Programmatic**.

En règle générale, cet événement vous permet d’ajuster l’interface utilisateur de votre application pour indiquer l’état actuel de stabilisation vidéo.

[!code-cs[VideoStabilizationEnabledChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetVideoStabilizationEnabledChanged)]

### <a name="clean-up-the-video-stabilization-effect"></a>Nettoyer l’effet de stabilisation vidéo

Pour nettoyer l’effet de stabilisation vidéo, appelez [**ClearEffectsAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.removeeffectasync) pour effacer tous les effets du pipeline vidéo. Si les variables membres contenant les propriétés d’encodage initiales ne sont pas null, utilisez-les pour restaurer les propriétés d’encodage. Pour finir, supprimez le gestionnaire d’événements **EnabledChanged** et définissez l’effet sur la valeur null.

[!code-cs[CleanUpVisualStabilizationEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetCleanUpVisualStabilizationEffect)]

## <a name="related-topics"></a>Rubriques connexes

* [Appareil photo](camera.md)
* [Photo de base, vidéo, audio et de capture à MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




