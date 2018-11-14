---
author: laurenhughes
ms.assetid: ''
description: Cet article explique comment utiliser la classe SoftwareBitmap avec la bibliothèque Open Source Computer Vision Library (OpenCV).
title: Traiter les images bitmap avec OpenCV
ms.author: lahugh
ms.date: 03/19/2018
ms.topic: article
keywords: windows10, uwp, opencv, softwarebitmap
ms.localizationpriority: medium
ms.openlocfilehash: b9f1f2050590267d0a98779eba11bbe0b363da0c
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6664161"
---
# <a name="process-bitmaps-with-opencv"></a>Traiter les images bitmaps avec OpenCV

Cet article explique comment utiliser la classe **[SoftwareBitmap](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)**, utilisée par de nombreuses API UWP différentes pour représenter des images, avec l’Open Source Computer Vision Library (OpenCV), une bibliothèque open source de code natif qui fournit un large éventail d’algorithmes de traitement d’image. 

Les exemples de cet article vous expliquent comment créer un composant Windows Runtime en code natif qui peuvent être utilisés à partir d’une application UWP, y compris une application créée avec C#. Ce composant d’assistance expose une méthode unique, **Blur**, qui utilisera la fonction de traitement flou d’image d'OpenCV. Le composant implémente les méthodes privées qui obtiennent un pointeur vers le tampon de données d’image sous-jacent. Celui-ci peut être utilisé directement par la bibliothèque OpenCV, ce qui permet d'étendre plus facilement le composant d’assistance de façon à implémenter d'autres fonctionnalités de traitement OpenCV. 

* Pour une présentation de l’utilisation de la méthode **SoftwareBitmap**, consultez la section [Créer, modifier et enregistrer des images bitmap](imaging.md). 
* Pour plus d’informations sur l’utilisation de la bibliothèque OpenCV, accédez à [http://opencv.org](http://opencv.org).
* Pour savoir comment utiliser le composant d’assistance OpenCV illustré dans cet article avec **[MediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader)** pour implémenter le traitement d’images en temps réel des images à partir d’un appareil photo, consultez [Utiliser OpenCV avec MediaFrameReader](use-opencv-with-mediaframereader.md).
* Pour obtenir un exemple de code complet qui implémente différents effets, voir la [Profils d’appareil photo + OpenCV](https://go.microsoft.com/fwlink/?linkid=854003) dans le référentiel Windows Universal Samples GitHub.

> [!NOTE] 
> La technique utilisée par le composant OpenCVHelper, décrite en détail dans cet article, requiert que les données d’image à traiter résident dans la mémoire du processeur, pas dans celle du GPU. Pour les API qui vous permettent de demander l’emplacement des images dans la mémoire, comme la classe **[MediaCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture)**, vous devez donc spécifier la mémoire du processeur.

## <a name="create-a-helper-windows-runtime-component-for-opencv-interop"></a>Créer un composant d’assistance Windows Runtime pour l’interopérabilité OpenCV

### <a name="1-add-a-new-native-code-windows-runtime-component-project-to-your-solution"></a>1. Ajoutez un nouveau projet composant Windows Runtime en code natif à votre solution

1. Ajoutez un nouveau projet à votre solution dans Visual Studio en cliquant sur votre solution dans l’Explorateur de solutions et en sélectionnant **Ajouter-> Nouveau projet**. 
2. Dans la catégorie **Visual C++**, sélectionnez **Composant Windows Runtime (Windows universel)**. Pour cet exemple, nommez le projet «OpenCVBridge» et cliquez sur **OK**. 
3. Dans la boîte de dialogue **Nouveau projet Windows universel**, sélectionnez la cible et la version minimale du système d’exploitation pour votre application et cliquez sur **OK**.
4. Cliquez avec le bouton droit sur le fichier autogénéré Class1.cpp dans l’Explorateur de solutions, puis sélectionnez **Supprimer**. Lorsque la boîte de dialogue de confirmation s’affiche, choisissez **Supprimer**. Supprimez ensuite le fichier d’en-tête Class1.h.
5. Cliquez avec le bouton droit sur l’icône de projet OpenCVBridge et sélectionnez **Ajouter ->Classe...**. Dans la boîte de dialogue **Ajouter une classe**, saisissez «OpenCVHelper» dans le champ **Nom de la classe**, puis cliquez sur **OK**. Le code sera ajouté aux fichiers de la classe créée dans une étape ultérieure.

### <a name="2-add-the-opencv-nuget-packages-to-your-component-project"></a>2. Ajoutez les packages OpenCV NuGet à votre projet de composant

1. Cliquez avec le bouton droit sur le projet OpenCVBridge dans l’Explorateur de solutions et sélectionnez **Gérer les packages NuGet...**
2. Lorsque la boîte de dialogue Gestionnaire de package NuGet s’ouvre, sélectionnez l’onglet **Parcourir** et tapez «OpenCV.Win» dans la zone de recherche.
3. Sélectionnez «OpenCV.Win.Core» et cliquez sur **Installer**. Dans la boîte de dialogue **Aperçu**, cliquez sur **OK**.
4. Utilisez la même procédure pour installer le package «OpenCV.Win.ImgProc».

> [!NOTE]
> Les packages OpenCV.Win.Core et OpenCV.Win.ImgProc ne sont pas régulièrement mis à jour, mais sont toujours recommandés pour créer un OpenCVHelper, comme décrit dans cette page.

### <a name="3-implement-the-opencvhelper-class"></a>3. Implémentez la classe OpenCVHelper

Collez le code suivant dans le fichier d’en-tête OpenCVHelper.h. Ce code comprend les fichiers d’en-tête OpenCV pour les packages *Core* et *ImgProc* que nous avons installés et déclare trois méthodes que nous verrons dans les étapes suivantes.

[!code-cpp[OpenCVHelperHeader](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.h#SnippetOpenCVHelperHeader)]

Supprimez le contenu existant du fichier OpenCVHelper.cpp, puis ajoutez les directives include suivantes. 

[!code-cpp[OpenCVHelperInclude](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperInclude)]

Après les directives include, ajoutez les directives **using** suivantes. 

[!code-cpp[OpenCVHelperUsing](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperUsing)]

Ensuite, ajoutez la méthode **GetPointerToPixelData** à OpenCVHelper.cpp. Cette méthode prend un **[SoftwareBitmap](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)** et, à travers une série de conversions, obtient une représentation d’interface COM des données de pixels à travers laquelle nous pouvons obtenir un pointeur vers le tampon de données sous-jacent sous forme de tableau **char**. 

Tout d'abord, un **[BitmapBuffer](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapbuffer)** contenant les données de pixel est obtenu en appelant **[LockBuffer](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.lockbuffer)**, demandant un tampon en lecture/écriture afin que la bibliothèque OpenCV puisse modifier ces données de pixel.  **[CreateReference](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapbuffer.CreateReference)** est appelé pour obtenir un objet **[IMemoryBufferReference](https://docs.microsoft.com/uwp/api/windows.foundation.imemorybufferreference)**. L'interface **IMemoryBufferByteAccess** est ensuite convertie en **IInspectable**, l’interface de base de toutes les classes Windows Runtime, et **[QueryInterface](https://msdn.microsoft.com/library/windows/desktop/ms682521(v=vs.85).aspx)** est appelé pour obtenir une interface COM **[IMemoryBufferByteAccess](https://msdn.microsoft.com/library/mt297505(v=vs.85).aspx)** qui va nous permettre d’obtenir le tampon de données de pixel sous forme de tableau **char**. Enfin, remplissez le tableau **char** en appelant **[IMemoryBufferByteAccess::GetBuffer](https://msdn.microsoft.com/library/mt297506(v=vs.85).aspx)**. Si l'une des étapes de conversion de cette méthode échoue, la méthode retourne **false**, indiquant que le traitement ne peut pas se poursuivre.

[!code-cpp[OpenCVHelperGetPointerToPixelData](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperGetPointerToPixelData)]

Ensuite, ajoutez la méthode **TryConvert** illustrée ci-dessous. Cette méthode prend un **SoftwareBitmap** et tente de le convertir en un objet **Mat**, qui est l’objet matrix qu'OpenCV utilise pour représenter les tampons de données d’image. Cette méthode appelle la méthode **GetPointerToPixelData** définie ci-dessus pour obtenir une représentation sous forme de tableau **char** du tampon de données de pixels. Si cette tentative aboutit, le constructeur de la classe **Mat** est appelé et passe la largeur et la hauteur en pixels obtenues à partir de l'objet **SoftwareBitmap** source. 

> [!NOTE] 
> Cet exemple spécifie la constante CV_8UC4 en tant que format de pixel pour créer l'objet **Mat**. Cela signifie que le **SoftwareBitmap** passé dans cette méthode doit avoir une valeur de propriété **[BitmapPixelFormat](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.BitmapPixelFormat)** de **[BGRA8](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapPixelFormat)** avec alpha prémultiplié, l’équivalent de CV_8UC4, pour fonctionner avec cet exemple.

Une copie partielle de l'objet **Mat** créé est retournée à partir de la méthode afin qu'un traitement plus approfondi s'opère sur le même tampon de données de pixels référencé par le **SoftwareBitmap** et non sur une copie de cette mémoire tampon.

[!code-cpp[OpenCVHelperTryConvert](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperTryConvert)]

Enfin, cet exemple de classe d’assistance implémente une méthode de traitement d'image unique, **Blur**, qui utilise simplement la méthode **TryConvert** définie ci-dessus pour récupérer un objet **Mat** qui représente l’image bitmap source et l’image bitmap cible pour l’opération de flou et appelle ensuite la méthode **Blur** depuis la bibliothèque OpenCV ImgProc. L'autre paramètre de **flou** spécifie la taille de l’effet de flou selon les axes X et Y.

[!code-cpp[OpenCVHelperBlur](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperBlur)]


## <a name="a-simple-softwarebitmap-opencv-example-using-the-helper-component"></a>Un exemple simple de SoftwareBitmap OpenCV utilisant le composant d’assistance
Maintenant que le composant OpenCVBridge a été créé, nous pouvons créer une application simple en C# qui utilise la méthode **flou** d'OpenCV pour modifier un **SoftwareBitmap**. Pour accéder au composant Windows Runtime à partir de votre application UWP, vous devez d’abord ajouter une référence au composant. Dans l’Explorateur de solutions, cliquez sur le nœud **Références** sous votre projet d’application UWP, puis sélectionnez **Ajouter une référence...**. Dans la boîte de dialogue Gestionnaire de références, sélectionnez **Projets -> Solution**. Cochez la case en regard de votre projet OpenCVBridge, puis cliquez sur **OK**.

L’exemple de code ci-dessous permet à l’utilisateur de sélectionner un fichier image, puis utilise **[BitmapDecoder](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder)** pour créer une représentation **SoftwareBitmap** de l’image. Pour plus d’informations sur l’utilisation de la méthode **SoftwareBitmap**, consultez la section [Créer, modifier et enregistrer des images bitmap](https://docs.microsoft.com/windows/uwp/audio-video-camera/imaging).

Comme indiqué précédemment dans cet article, la classe **OpenCVHelper** nécessite que toutes les images **SoftwareBitmap** fournies soient encodées au format de pixel BGRA8 avec valeurs alpha prémultipliées. Si l’image n’est pas encore à ce format, l’exemple de code appelle **[Convertir](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.BitmapAlphaMode)** pour convertir l’image au format attendu.

Ensuite, un **SoftwareBitmap** est créé pour être utilisé comme cible de l’opération de flou. Les propriétés de l’image d’entrée sont utilisées comme arguments au constructeur pour créer une image bitmap avec format correspondant.

Une nouvelle instance de **OpenCVHelper** est créée et la méthode **Blur** est appelée, en passant les bitmap source et cible. Enfin, un élément **SoftwareBitmapSource** est créé pour attribuer l’image résultante à un contrôle **Image** XAML.


[!code-cs[OpenCVBlur](./code/ImagingWin10/cs/MainPage.OpenCV.xaml.cs#SnippetOpenCVBlur)]

## <a name="related-topics"></a>Rubriquesassociées

* [Référence des options BitmapEncoder](bitmapencoder-options-reference.md)
* [Métadonnées d’image](image-metadata.md)
 

 




