---
description: Cet article explique comment utiliser la classe LowLightFusion pour traiter des images bitmap.
title: Traiter des images bitmap avec l’API Low Light Fusion
ms.date: 03/22/2018
ms.topic: article
keywords: Windows10, uwp, low light fusion, bitmaps, traitement d’image
ms.localizationpriority: medium
ms.openlocfilehash: e0677d2e4ce5e412c158b8a00da3a6338bae6c46
ms.sourcegitcommit: 231065c899d0de285584d41e6335251e0c2c4048
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8826377"
---
# <a name="process-bitmaps-with-the-lowlightfusion-api"></a>Traiter des images bitmap avec l’API LowLightFusion

Les images à basse lumière sont difficiles à capturer en bonne qualité, notamment avec des appareils mobiles à ouverture et taille de capteur fixes. Pour compenser un faible éclairage, les appareils peuvent augmenter la durée d'exposition ou le gain du capteur, ce qui peut entraîner un flou de mouvement et une augmentation du bruit dans les images. 

La [classe LowLightFusion](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion) améliore la qualité des images à basse lumière en échantillonnant les informations des pixels à partir de plusieurs images prises dans un court intervalle de temps, c'est-à-dire, des images prises en rafale rapide, afin de réduire le bruit et le flou de mouvement. Cette fonction est utile à ajouter à une application de retouche photo, par exemple.

Cette fonctionnalité est également mise à disposition par le biais de la [classe AdvancedPhotoCapture](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture), qui applique l’algorithme Low Light Fusion à une séquence d’images directement après la capture des images, si nécessaire. Voir la capture de [photo en basse lumière](https://docs.microsoft.com/windows/uwp/audio-video-camera/high-dynamic-range-hdr-photo-capture#low-light-photo-capture) pour savoir comment implémenter cette fonctionnalité.

## <a name="prepare-the-images-for-processing"></a>Préparer les images pour le traitement

Dans cet exemple, nous allons montrer comment utiliser la [classe LowLightFusion](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion), ainsi que la classe [FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) pour qu'un utilisateur puisse sélectionner plusieurs images et leur appliquer la fonctionnalité Low Light Fusion.

Tout d’abord, nous devons déterminer le nombre d’images (également appelées trames) acceptées par l’algorithme et créer une liste pour contenir ces images.

[!code-cs[SnippetGetMaxLLFFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetGetMaxLLFFrames)]

Une fois déterminé le nombre de trames acceptées par l’algorithme Low Light Fusion, nous pouvons utiliser la classe [FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) pour permettre à l’utilisateur de choisir les images à utiliser dans l’algorithme.

[!code-cs[SnippetGetFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetGetFrames)]

Maintenant que le nombre correct de trames est sélectionné, nous devons décoder les images en [SoftwareBitmaps](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) et vérifier que les SoftwareBitmaps sont au format approprié pour LowLightFusion.

[!code-cs[SnippetDecodeFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetDecodeFrames)]


## <a name="fuse-the-bitmaps-into-a-single-bitmap"></a>Fusionner les bitmaps en une seule image bitmap

Maintenant que nous avons un nombre approprié de trames dans un format acceptable, nous pouvons utiliser la méthode **[FuseAsync](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion.fuseasync)** pour appliquer l’algorithme Low Light Fusion. Notre résultat sera l’image traitée, avec une netteté améliorée, sous la forme d’un élément SoftwareBitmap. 

[!code-cs[SnippetFuseFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetFuseFrames)]

Enfin, nous allons nettoyer le SoftwareBitmap obtenu en l'encodant et en l’enregistrant sous la forme d'une image «normale» et conviviale, semblable aux images d’entrée du départ.

[!code-cs[SnippetEncodeFrame](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetEncodeFrame)]


## <a name="before-and-after"></a>Avant et après

Voici un exemple d’image d’entrée et de l’image de sortie qui en résulte après l’application de l’algorithme Low Light Fusion.

> [!div class="mx-tableFixed"] 
| Image d’entrée | Sortie de Low Light Fusion | 
|-------------|-------------------------|
| ![Image d’entrée de l’algorithme Low Light Fusion](./images/LLF-Input.png) | ![Image de résultat de l’algorithme Low Light Fusion](./images/LLF-Output.png) |

À partir de l’image d’entrée, vous pouvez voir que l’éclairage et la clarté des ombres autour de la bannière ont été améliorés.

## <a name="related-topics"></a>Rubriquesassociées 
[Classe LowLightFusion](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion)  
[Classe LowLightFusionResult](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusionresult)