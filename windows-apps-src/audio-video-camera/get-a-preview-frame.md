---
ms.assetid: 05E418B4-5A62-42BD-BF66-A0762216D033
description: Cette rubrique vous montre comment obtenir une image d’aperçu à partir du flux d’aperçu de capture multimédia.
title: Obtenir une image d’aperçu
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7faa018dad336b6e22dd236e57585cade38f8a94
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7979851"
---
# <a name="get-a-preview-frame"></a>Obtenir une image d’aperçu


Cette rubrique vous montre comment obtenir une image d’aperçu à partir du flux d’aperçu de capture multimédia.

> [!NOTE] 
> Cet article repose sur les concepts et le code décrits dans [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), qui décrit comment implémenter la capture photo et vidéo de base. Nous vous recommandons de vous familiariser avec le modèle de capture multimédia de base de cet article avant de passer à des scénarios de capture plus avancés. Le code de cet article part du principe que votre application possède déjà une instance de MediaCapture correctement lancée et que vous disposez d’un [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) avec un flux d’aperçu vidéo actif.

Outre les espaces de noms nécessaires pour la capture multimédia de base, la capture d’une image d’aperçu nécessite les espaces de noms suivants.

[!code-cs[PreviewFrameUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPreviewFrameUsing)]

Lorsque vous demandez une image d’aperçu, vous pouvez spécifier le format dans lequel vous souhaitez recevoir la trame en créant un objet [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917) avec le format de votre choix. Cet exemple crée une image vidéo avec la même résolution que celle du flux d’aperçu en appelant [**VideoDeviceController.GetMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211995) et en spécifiant[**MediaStreamType.VideoPreview**](https://msdn.microsoft.com/library/windows/apps/br226640) pour demander les propriétés du flux d’aperçu. La largeur et la hauteur du flux d’aperçu sont utilisées pour créer l’image vidéo.

[!code-cs[CreateFormatFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateFormatFrame)]

Si votre objet [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) est initialisé et que vous avez un flux d’aperçu actif, appelez [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926711) pour obtenir un flux d’aperçu. Passez l’image vidéo créée à la dernière étape pour spécifier le format de l’image renvoyée.

[!code-cs[GetPreviewFrameAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewFrameAsync)]

Obtenez une représentation [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) de l’image d’aperçu en accédant à la propriété [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn930926) de l’objet [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917). Pour plus d’informations sur l’enregistrement, le chargement et la modification des images bitmap logicielles, voir [Acquisition d’images](imaging.md).

[!code-cs[GetPreviewBitmap](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewBitmap)]

Vous pouvez également obtenir une représentation [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn965505) de l’image d’aperçu si vous souhaitez utiliser l’image avec les API Direct3D.

[!code-cs[GetPreviewSurface](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewSurface)]

> [!IMPORTANT]
> La propriété [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn930926) ou la propriété [**Direct3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn930920) de l’objet **VideoFrame** renvoyé peut être null selon la manière dont vous appelez **GetPreviewFrameAsync** et en fonction de l’appareil sur lequel votre application est exécutée.

> - Si vous appelez la surcharge de [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926713) qui accepte un argument **VideoFrame**, l’objet **VideoFrame** renvoyé a une valeur **SoftwareBitmap** non null et la propriété **Direct3DSurface** est null.
> - Si vous appelez la surcharge de [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926712) qui n’a aucun argument sur un appareil utilisant une surface Direct3D pour représenter l’image en interne, la propriété **Direct3DSurface** est non null et la propriété **SoftwareBitmap** est null.
> - Si vous appelez la surcharge de [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926712) qui n’a aucun argument sur un appareil n’utilisant pas une surface Direct3D pour représenter l’image en interne, la propriété **SoftwareBitmap** sera non null et la propriété **Direct3DSurface** sera null.

Votre application doit toujours rechercher une valeur null avant d’essayer d’agir sur des objets renvoyés par les propriétés **SoftwareBitmap** ou **Direct3DSurface**.

Lorsque vous avez fini d’utiliser l’image d’aperçu, veillez à appeler sa méthode [**Close**](https://msdn.microsoft.com/library/windows/apps/dn930918) (projetée vers Dispose en C#) pour libérer les ressources utilisées par l’image. Vous pouvez aussi utiliser le modèle **using** qui supprime automatiquement l’objet.

[!code-cs[CleanUpPreviewFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpPreviewFrame)]

## <a name="related-topics"></a>Rubriquesconnexes

* [Appareil photo](camera.md)
* [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




