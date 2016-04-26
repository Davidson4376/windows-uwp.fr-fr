---
ms.assetid: CC0D6E9B-128D-488B-912F-318F5EE2B8D3
description: Cet article décrit comment utiliser la classe CameraCaptureUI afin de capturer des photos ou des vidéos à l’aide de l’interface utilisateur de l’appareil photo intégré à Windows.
title: Capturer des photos et des vidéos à l’aide de CameraCaptureUI
---

# Capturer des photos et des vidéos à l’aide de CameraCaptureUI

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Cet article décrit comment utiliser la classe CameraCaptureUI afin de capturer des photos ou des vidéos à l’aide de l’interface utilisateur de l’appareil photo intégré à Windows. Cette fonctionnalité est facile à utiliser et permet à votre application d’obtenir une photo ou une vidéo capturée par l’utilisateur avec seulement quelques lignes de code.

Si votre scénario requiert un contrôle de bas niveau plus robuste de l’opération de capture, utilisez l’objet [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) et implémentez votre propre expérience de capture. Pour plus d’informations, voir [Capturer des photos et des vidéos avec MediaCapture](capture-photos-and-video-with-mediacapture.md).

## Capturer une photo avec CameraCaptureUI

Pour utiliser l’interface utilisateur de capture d’appareil photo, incluez l’espace de noms [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/br226738) dans votre projet. Pour les opérations de fichier avec le fichier image renvoyé, incluez [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346).

[!code-cs[UsingCaptureUI](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingCaptureUI)]

Pour capturer une photo, créez un objet [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030). À l’aide de la propriété [**PhotoSettings**](https://msdn.microsoft.com/library/windows/apps/br241058) de l’objet, vous pouvez spécifier les propriétés de la photo renvoyée, comme le format d’image. Par défaut, l’interface utilisateur de capture de l’appareil photo permet à l’utilisateur de rogner la photo avant de la renvoyer. Cette fonctionnalité peut être désactivée avec la propriété [**AllowCropping**](https://msdn.microsoft.com/library/windows/apps/br241042). Cet exemple définit la [**CroppedSizeInPixels**](https://msdn.microsoft.com/library/windows/apps/br241044) pour demander que l’image renvoyée soit au format 200 x 200 pixels.

**Important** Le rognage d’images via CameraCaptureUI n’est pas pris en charge pour les appareils de la famille d’appareils mobiles. La valeur de la propriété [**AllowCropping**](https://msdn.microsoft.com/library/windows/apps/br241042) est ignorée lorsque votre application est exécutée sur ces appareils.

Appelez [**CaptureFileAsync**](https://msdn.microsoft.com/library/windows/apps/br241057) et spécifiez [**CameraCaptureUIMode.Photo**](https://msdn.microsoft.com/library/windows/apps/br241040) pour indiquer qu’une photo doit être capturée. La méthode renvoie une instance [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) contenant l’image si la capture est réussie. Si l’utilisateur annule la capture, l’objet renvoyé est null.

[!code-cs[CapturePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCapturePhoto)]

Une fois que vous avez le **StorageFile** contenant la photo capturée, vous pouvez créer un objet [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) qui à utiliser avec plusieurs fonctionnalités d’application Windows universelle différentes.

Vous devez d’abord inclure l’espace de noms [**Windows.Graphics.Imaging**](https://msdn.microsoft.com/library/windows/apps/br226400) dans votre projet.

[!code-cs[UsingSoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmap)]

Appelez [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/br227116) pour obtenir un flux à partir du fichier image. Appelez [**BitmapDecoder.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/br226182) pour obtenir un décodeur d’image bitmap à partir du flux. Appelez ensuite [**GetSoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887332) pour obtenir une représentation **SoftwareBitmap** de l’image.

[!code-cs[SoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmap)]

Pour afficher l’image dans votre interface utilisateur, déclarez un contrôle [**Image**](https://msdn.microsoft.com/library/windows/apps/br242752) dans votre page XAML.

[!code-xml[ImageControl](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetImageControl)]

Pour utiliser l’image bitmap logicielle dans votre page XAML, incluez l’espace de noms using [**Windows.UI.Xaml.Media.Imaging**](https://msdn.microsoft.com/library/windows/apps/br243258) dans votre projet.

[!code-cs[UsingSoftwareBitmapSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmapSource)]

Le contrôle **Image** nécessite que la source d’image soit au format BGRA8 avec alpha prémultiplié ou sans alpha. Appelez la méthode statique [**SoftwareBitmap.Convert**](https://msdn.microsoft.com/library/windows/apps/dn887362) pour créer une image bitmap logicielle au format souhaité. Ensuite, créez un objet [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854) et appelez [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn997856) pour affecter l’image bitmap logicielle à la source. Enfin, définissez le contrôle **Image** et sa propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/br242760) pour afficher la photo capturée dans l’interface utilisateur.

[!code-cs[SetImageSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetImageSource)]

## Capturer une vidéo avec CameraCaptureUI

Pour capturer une vidéo, créez un objet [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030). À l’aide de la propriété [**VideoSettings**](https://msdn.microsoft.com/library/windows/apps/br241059) de l’objet, vous pouvez spécifier les propriétés de la vidéo renvoyée, comme le format.

Appelez [**CaptureFileAsync**](https://msdn.microsoft.com/library/windows/apps/br241057) et spécifiez [**Video**](https://msdn.microsoft.com/library/windows/apps/br241059) pour indiquer qu’une vidéo doit être capturée. La méthode renvoie une instance [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) contenant la vidéo si la capture est réussie. Si l’utilisateur annule la capture, l’objet renvoyé est null.

[!code-cs[CaptureVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCaptureVideo)]

Ce que vous faites du fichier vidéo capturé dépend du scénario pour votre application. Le reste de cet article décrit comment créer rapidement une composition multimédia à partir d’une ou plusieurs vidéos capturées et l’afficher dans votre interface utilisateur.

Tout d’abord, ajoutez un contrôle [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) dans lequel la composition vidéo s’affichera dans votre page XAML.

[!code-xml[MediaElement](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetMediaElement)]

Ajoutez les espaces de noms [**Windows.Media.Editing**](https://msdn.microsoft.com/library/windows/apps/dn640565) et [**Windows.Media.Core**](https://msdn.microsoft.com/library/windows/apps/dn278962) à votre projet.


[!code-cs[UsingMediaComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingMediaComposition)]

Déclarez les variables membres pour un objet [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646) et une [**MediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn282716) qui restera valide pour la durée de vie de la page.

[!code-cs[DeclareMediaComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetDeclareMediaComposition)]

Avant de capturer des vidéos, vous devez créer une instance de la classe **MediaComposition**.

[!code-cs[InitComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetInitComposition)]

Une fois le fichier vidéo renvoyé depuis l’interface utilisateur de capture d’appareil photo, créez un [**MediaClip**](https://msdn.microsoft.com/library/windows/apps/dn652596) en appelant [**MediaClip.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652607). Ajouter le clip multimédia à la collection [**Clips**](https://msdn.microsoft.com/library/windows/apps/dn652648) de la composition.

Appelez [**GeneratePreviewMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn652674) pour créer l’objet **MediaStreamSource** à partir de la composition.

[!code-cs[AddToComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetAddToComposition)]

Enfin, définissez la source de flux de manière à utiliser la méthode [**SetMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn299029) de l’élément multimédia pour afficher la composition dans l’interface utilisateur.

[!code-cs[SetMediaElementSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetMediaElementSource)]

Vous pouvez continuer à capturer des clips vidéo et les ajouter à la composition. Pour plus d’informations sur les compositions multimédias, voir [Compositions multimédias et modification](media-compositions-and-editing.md).

**Remarque**  
Cet article s’adresse aux développeurs de Windows 10 qui développent des applications pour la plateforme Windows universelle (UWP). Si vous développez une application pour Windows 8.x ou Windows Phone 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Rubriques connexes

* [Capturer des photos et des vidéos à l’aide de MediaCapture](capture-photos-and-video-with-mediacapture.md)
* [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030)
 

 






<!--HONumber=Mar16_HO1-->


