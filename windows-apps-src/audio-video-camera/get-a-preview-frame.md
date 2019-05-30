---
ms.assetid: 05E418B4-5A62-42BD-BF66-A0762216D033
description: Cette rubrique vous montre comment obtenir une image d’aperçu à partir du flux d’aperçu de capture multimédia.
title: Obtenir une image d’aperçu
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9963955649b98f226fbb81871b2ac391035ba41a
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360885"
---
# <a name="get-a-preview-frame"></a>Obtenir une image d’aperçu


Cette rubrique vous montre comment obtenir une image d’aperçu à partir du flux d’aperçu de capture multimédia.

> [!NOTE] 
> Cet article repose sur les concepts et le code décrits dans [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), qui décrit comment implémenter la capture photo et vidéo de base. Nous vous recommandons de vous familiariser avec le modèle de capture simple de contenu multimédia de cet article avant d’adopter des scénarios de capture plus avancés. Le code de cet article part du principe que votre application possède déjà une instance de MediaCapture correctement lancée et que vous disposez d’un [**CaptureElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) avec un flux d’aperçu vidéo actif.

Outre les espaces de noms nécessaires pour la capture multimédia de base, la capture d’une image d’aperçu nécessite les espaces de noms suivants.

[!code-cs[PreviewFrameUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPreviewFrameUsing)]

Lorsque vous demandez une image d’aperçu, vous pouvez spécifier le format dans lequel vous souhaitez recevoir la trame en créant un objet [**VideoFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.VideoFrame) avec le format de votre choix. Cet exemple crée une image vidéo avec la même résolution que celle du flux d’aperçu en appelant [**VideoDeviceController.GetMediaStreamProperties**](https://docs.microsoft.com/uwp/api/windows.media.devices.videodevicecontroller.getmediastreamproperties) et en spécifiant[**MediaStreamType.VideoPreview**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaStreamType) pour demander les propriétés du flux d’aperçu. La largeur et la hauteur du flux d’aperçu sont utilisées pour créer l’image vidéo.

[!code-cs[CreateFormatFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateFormatFrame)]

Si votre objet [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) est initialisé et que vous avez un flux d’aperçu actif, appelez [**GetPreviewFrameAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.getpreviewframeasync) pour obtenir un flux d’aperçu. Passez l’image vidéo créée à la dernière étape pour spécifier le format de l’image renvoyée.

[!code-cs[GetPreviewFrameAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewFrameAsync)]

Obtenez une représentation [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) de l’image d’aperçu en accédant à la propriété [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.media.videoframe.softwarebitmap) de l’objet [**VideoFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.VideoFrame). Pour plus d’informations sur l’enregistrement, le chargement et la modification des images bitmap logicielles, voir [Acquisition d’images](imaging.md).

[!code-cs[GetPreviewBitmap](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewBitmap)]

Vous pouvez également obtenir une représentation [**IDirect3DSurface**](https://docs.microsoft.com/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) de l’image d’aperçu si vous souhaitez utiliser l’image avec les API Direct3D.

[!code-cs[GetPreviewSurface](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewSurface)]

> [!IMPORTANT]
> La propriété [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.media.videoframe.softwarebitmap) ou la propriété [**Direct3DSurface**](https://docs.microsoft.com/uwp/api/windows.media.videoframe.direct3dsurface) de l’objet **VideoFrame** renvoyé peut être null selon la manière dont vous appelez **GetPreviewFrameAsync** et en fonction de l’appareil sur lequel votre application est exécutée.

> - Si vous appelez la surcharge de [**GetPreviewFrameAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.getpreviewframeasync) qui accepte un argument **VideoFrame**, l’objet **VideoFrame** renvoyé a une valeur **SoftwareBitmap** non null et la propriété **Direct3DSurface** est null.
> - Si vous appelez la surcharge de [**GetPreviewFrameAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.getpreviewframeasync) qui n’a aucun argument sur un appareil utilisant une surface Direct3D pour représenter l’image en interne, la propriété **Direct3DSurface** est non null et la propriété **SoftwareBitmap** est null.
> - Si vous appelez la surcharge de [**GetPreviewFrameAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.getpreviewframeasync) qui n’a aucun argument sur un appareil n’utilisant pas une surface Direct3D pour représenter l’image en interne, la propriété **SoftwareBitmap** sera non null et la propriété **Direct3DSurface** sera null.

Votre application doit toujours rechercher une valeur null avant d’essayer d’agir sur des objets renvoyés par les propriétés **SoftwareBitmap** ou **Direct3DSurface**.

Lorsque vous avez fini d’utiliser l’image d’aperçu, veillez à appeler sa méthode [**Close**](https://docs.microsoft.com/uwp/api/windows.media.videoframe.close) (projetée vers Dispose en C#) pour libérer les ressources utilisées par l’image. Vous pouvez aussi utiliser le modèle **using** qui supprime automatiquement l’objet.

[!code-cs[CleanUpPreviewFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpPreviewFrame)]

## <a name="related-topics"></a>Rubriques connexes

* [Appareil photo](camera.md)
* [Photo de base, vidéo, audio et de capture à MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




