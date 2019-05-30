---
ms.assetid: 708170E1-777A-4E4A-9F77-5AB28B88B107
description: Cet article vous montre comment utiliser les contrôles des appareils manuels pour activer les scénarios de capture vidéo, y compris la vidéo HDR et la priorité de l’exposition.
title: Contrôles d’appareil photo manuel pour la capture vidéo
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d20f2d372354cf7bbfa596318f165c424f08c8ee
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66358863"
---
# <a name="manual-camera-controls-for-video-capture"></a>Contrôles d’appareil photo manuel pour la capture vidéo



Cet article vous montre comment utiliser les contrôles des appareils manuels pour activer les scénarios de capture vidéo, y compris la vidéo HDR et la priorité de l’exposition.

Les contrôles des appareils vidéo mentionnés dans cet article sont tous ajoutés à votre application en utilisant le même modèle. Tout d’abord, vérifiez si le contrôle est pris en charge sur l’appareil sur lequel votre application est en cours d’exécution. Si le contrôle est pris en charge, définissez le mode de votre choix pour le contrôle. En règle générale, si un contrôle particulier n’est pas pris en charge sur l’appareil actuel, vous devez désactiver ou masquer l’élément d’interface utilisateur qui permet à l’utilisateur d’activer la fonctionnalité.

Toutes les API de contrôle des appareils mentionnées dans cet article sont membres de l’espace de noms [**Windows.Media.Devices**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices).

[!code-cs[VideoControllersUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoControllersUsing)]

> [!NOTE] 
> Cet article repose sur les concepts et le code décrits dans [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), qui décrit comment implémenter la capture photo et vidéo de base. Nous vous recommandons de vous familiariser avec le modèle de capture simple de contenu multimédia de cet article avant d’adopter des scénarios de capture plus avancés. Le code de cet article part du principe que votre application possède déjà une instance de MediaCapture initialisée correctement.

## <a name="hdr-video"></a>Vidéo HDR

La fonctionnalité vidéo HDR (High Dynamic Range) applique le traitement HDR au flux vidéo de l’appareil de capture. Déterminez si la vidéo HDR est prise en charge en vérifiant la propriété [**HdrVideoControl.Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.hdrvideocontrol.supported).

Le contrôle vidéo HDR prend en charge trois modes : activé, désactivé et automatique, ce qui signifie que l’appareil détermine dynamiquement si le traitement vidéo HDR est susceptible d’améliorer la capture multimédia et l’active, le cas échéant. Pour déterminer si un mode particulier est pris en charge sur l’appareil actuel, vérifiez que la collection [**HdrVideoControl.SupportedModes**](https://docs.microsoft.com/uwp/api/windows.media.devices.hdrvideocontrol.supportedmodes) contient le mode de votre choix.

Activez ou désactivez le traitement vidéo HDR en définissant le [**HdrVideoControl.Mode**](https://docs.microsoft.com/uwp/api/windows.media.devices.hdrvideocontrol.mode) sur le mode de votre choix.

[!code-cs[SetHdrVideoMode](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetHdrVideoMode)]

## <a name="exposure-priority"></a>Priorité d’exposition

Lorsqu’il est activé, le [**ExposurePriorityVideoControl**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.ExposurePriorityVideoControl) évalue les images vidéo issues de l’appareil de capture pour déterminer si la vidéo capture une scène de faible éclairage. Dans ce cas, le contrôle réduit la fréquence d’images de la vidéo capturée pour augmenter le temps d’exposition pour chaque image et améliorer la qualité visuelle de la vidéo capturée.

Déterminez si le contrôle de priorité d’exposition est pris en charge sur l’appareil actuel en vérifiant la propriété [**ExposurePriorityVideoControl.Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.exposurepriorityvideocontrol.supported).

Activez ou désactivez le contrôle de priorité d’exposition en définissant le [**ExposurePriorityVideoControl.Enabled**](https://docs.microsoft.com/uwp/api/windows.media.devices.exposurepriorityvideocontrol.enabled) sur le mode de votre choix.

[!code-cs[EnableExposurePriority](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetEnableExposurePriority)]

## <a name="temporal-denoising"></a>Suppression du bruit temporel
À partir de Windows 10, version 1803, vous pouvez activer la suppression du bruit temporel pour la vidéo sur les appareils qui la prennent en charge. Cette fonctionnalité fusionne les données d’image de plusieurs images adjacentes en temps réel pour produire des images vidéo avec moins de bruit visuel.

L'objet [**VideoTemporalDenoisingControl**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingcontrol) permet à votre application de déterminer si la suppression du bruit temporel est prise en charge sur l’appareil actuel, auquel cas, quels modes de suppression du bruit sont pris en charge. Les modes DÉBRUITAGE disponibles sont [ **hors**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingmode), [ **sur**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingmode), et [ **automatique** ](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingmode). Un appareil, n’acceptent pas tous les modes, mais chaque appareil doit prendre en charge soit **automatique** ou **sur** et **hors**.

L’exemple suivant utilise une interface utilisateur simple pour fournir des cases d’option permettant à l’utilisateur de basculer entre les modes de suppression du bruit.

[!code-xml[SnippetDenoiseXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetDenoiseXAML)]

Dans la méthode suivante, la propriété [**VideoTemporalDenoisingControl.Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingcontrol.supported) est cochée pour voir si la suppression du bruit temporel est prise en charge sur l’appareil actuel. Si oui, nous vérifions que les modes **Off** et **Auto** ou **On** sont pris en charge, auquel cas, nos cases d’option deviennent visibles. Ensuite, les boutons **Auto** et **On** deviennent visibles si ces méthodes sont prises en charge.

[!code-cs[SnippetUpdateDenoiseCapabilities](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetUpdateDenoiseCapabilities)]

Dans le gestionnaire d’événement **Checked** pour les cases d’option, le nom du bouton est coché et le mode correspondant est défini en définissant la propriété [**VideoTemporalDenoisingControl.Mode**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingcontrol.mode).

[!code-cs[SnippetDenoiseButtonChecked](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDenoiseButtonChecked)]

### <a name="disabling-temporal-denoising-while-processing-frames"></a>Désactivation de la suppression du bruit temporel lors du traitement des images
Une vidéo qui a été traitée à l’aide de la suppression du bruit temporel peut être plus agréable à l’œil humain. Toutefois, étant donné que la suppression du bruit temporel peut avoir un impact sur la cohérence de l’image et réduire la quantité de détails de l’image, les applications qui exécutent le traitement des images sur les trames, par exemple l’enregistrement ou la reconnaissance optique des caractères, peuvent désactiver par programme la suppression du bruit lorsque le traitement des images est activé.

L’exemple suivant détermine quels modes de suppression du bruit sont pris en charge et stocke ces informations dans des variables de classe.

[!code-cs[SnippetDenoiseFrameReaderVars](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDenoiseFrameReaderVars)]

[!code-cs[SnippetDenoiseCapabilitiesForFrameProcessing](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDenoiseCapabilitiesForFrameProcessing)]

Lorsque l’application active le traitement des images, elle définit le mode de suppression du bruit sur **Off** si ce mode est pris en charge, afin que le traitement des images puisse utiliser des images brutes qui n’ont pas fait l’objet d’une suppression du bruit.

[!code-cs[SnippetEnableFrameProcessing](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetEnableFrameProcessing)]

Lorsque l’application désactive le traitement des images, elle définit le mode de suppression du bruit sur **On** ou **Auto**, selon le mode pris en charge.

[!code-cs[SnippetDisableFrameProcessing](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDisableFrameProcessing)]

Pour plus d’informations sur l’obtention d’images vidéo pour le traitement des images, voir [Traiter des images multimédias avec MediaFrameReader](process-media-frames-with-mediaframereader.md).

## <a name="related-topics"></a>Rubriques connexes

* [Appareil photo](camera.md)
* [Photo de base, vidéo, audio et de capture à MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Blocs de processus multimédias avec MediaFrameReader](process-media-frames-with-mediaframereader.md)
*  [**VideoTemporalDenoisingControl**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingcontrol)
 




