---
author: drewbatgit
ms.assetid: 0186EA01-8446-45BA-A109-C5EB4B80F368
description: La classe AdvancedPhotoCapture vous permet de capturer des photos HDR.
title: "Capture photo avec plage dynamique élevée (HDR)"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 3015aa4338ddb0c0a006eb631026261a4453f376

---

# Capture photo avec plage dynamique élevée (HDR)

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


La classe [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) vous permet de capturer des photos HDR. Cette API permet aussi d’obtenir une image de référence à partir de la capture HDR avant la fin du traitement de l’image finale.

Les articles suivants concernent également la capture HDR :

-   Vous pouvez utiliser la [**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/dn948902) pour permettre au système d’évaluer le contenu du flux d’aperçu de capture multimédia pour déterminer si le traitement HDR est susceptible d’améliorer le résultat de la capture. Pour plus d’informations, voir [Analyse de scène de capture multimédia](scene-analysis-for-media-capture.md).

-   Utilisez la classe [**HdrVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926680) pour capturer une vidéo à l’aide de l’algorithme de traitement HDR intégré de Windows. Pour plus d’informations, voir [Contrôles de l’appareil de capture pour la vidéo](capture-device-controls-for-video-capture.md).

-   Vous pouvez utiliser la [**VariablePhotoSequenceCapture**](https://msdn.microsoft.com/library/windows/apps/dn652564) pour capturer une séquence de photos, chacune avec des paramètres différents, et implémenter votre propre HDR ou tout autre algorithme de traitement. Pour plus d’informations, voir [Séquence de photos variable](variable-photo-sequence.md).

**Remarque**
-   L’enregistrement simultané de capture photo et vidéo **AdvancedPhotoCapture** n’est pas pris en charge.

-   L’utilisation du flash au cours de la capture photo avancée n’est pas prise en charge.

**Remarque** Cet article repose sur les concepts et sur le code décrits dans [Capturer des photos et des vidéos à l’aide de MediaCapture](capture-photos-and-video-with-mediacapture.md), qui détaille les étapes d’implémentation de capture photo et vidéo de base. Il est recommandé de vous familiariser avec le modèle de capture multimédia de base dans cet article avant de passer à des scénarios de capture plus avancés. Le code de cet article part du principe que votre application possède déjà une instance de MediaCapture initialisée correctement.

## Espaces de noms de capture photo HDR

Outre les espaces de noms nécessaires pour la capture multimédia de base, votre application requiert les espaces de noms suivants pour utiliser la capture photo HDR.

[!code-cs[HDRPhotoUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHDRPhotoUsing)]


## Déterminer si la capture photo HDR est prise en charge sur l’appareil actuel

La technique de capture HDR décrite dans cet article est effectuée à l’aide de l’objet [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386). Tous les appareils ne prennent pas en charge la capture HDR avec **AdvancedPhotoCapture**. Déterminez si l’appareil sur lequel votre application est en cours d’exécution prend en charge la technique en obtenant l’objet **MediaCapture** et sa [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825), puis en obtenant la propriété [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840). Vérifiez la collection [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/mt147844) du contrôleur d’appareil vidéo pour voir si elle inclut [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845). Si tel est le cas, la capture HDR à l’aide de **AdvancedPhotoCapture** est prise en charge.

[!code-cs[HdrSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHdrSupported)]

## Configurer et préparer l’objet AdvancedPhotoCapture

Étant donné que vous devrez accéder à l’instance [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) à partir de plusieurs emplacements dans votre code, vous devez déclarer une variable membre pour stocker l’objet.

[!code-cs[DeclareAdvancedCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareAdvancedCapture)]

Dans votre application, une fois l’objet **MediaCapture** initialisé, créez un objet [**AdvancedPhotoCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/mt147837) et définissez le mode sur [**AdvancedPhotoMode.Hdr**](https://msdn.microsoft.com/library/windows/apps/mt147845). Appelez l’objet [**AdvancedPhotoControl**](https://msdn.microsoft.com/library/windows/apps/mt147840) et sa méthode [**Configure**](https://msdn.microsoft.com/library/windows/apps/mt147841), en passant l’objet **AdvancedPhotoCaptureSettings** que vous avez créé.

Appelez l’objet **MediaCapture** et sa méthode [**PrepareAdvancedPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181403), en passant un objet [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) indiquant le type d’encodage que la capture doit utiliser. La classe **ImageEncodingProperties** fournit des méthodes statiques pour la création d’encodages d’image pris en charge par **MediaCapture**.

**PrepareAdvancedPhotoCaptureAsync** renvoie l’objet [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) que vous allez utiliser pour lancer la capture photo. Vous pouvez utiliser cet objet pour enregistrer des gestionnaires pour [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) et pour [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387) qui sont décrits plus loin dans cet article.

[!code-cs[CreateAdvancedCaptureAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateAdvancedCaptureAsync)]

## Capturer une photo HDR

Capturer une photo HDR en appelant l’objet [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) et sa méthode [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388). Cette méthode renvoie un objet [**AdvancedCapturedPhoto**](https://msdn.microsoft.com/library/windows/apps/mt181378) qui fournit la photo capturée dans sa propriété [**Frame**](https://msdn.microsoft.com/library/windows/apps/mt181382).

[!code-cs[CaptureHdrPhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureHdrPhotoAsync)]

**ConvertOrientationToPhotoOrientation** et **ReencodeAndSavePhotoAsync** sont des méthodes d’assistance qui sont abordées dans le cadre du scénario de capture multimédia de base dans l’article [Capturer des photos et des vidéos à l’aide de MediaCapture](capture-photos-and-video-with-mediacapture.md).

## Obtenir l’image de référence facultative

Le processus HDR capture plusieurs images, puis les transforme en une seule image une fois toutes les images capturées. Vous pouvez accéder à une image après l’avoir capturée, mais sans devoir attendre la fin du processus HDR, en gérant l’événement [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392). Cette étape n’est pas nécessaire si vous êtes uniquement intéressé par le résultat de la photo HDR finale.

**Important** [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392) n’est pas déclenché sur les appareils qui prennent en charge la plage HDR matérielle et qui, par conséquent, ne génèrent pas d’images de référence. Votre application doit gérer le cas où cet événement n’est pas déclenché.

Étant donné que l’image de référence arrive hors du contexte de l’appel à **CaptureAsync**, un mécanisme permet de passer des informations de contexte au gestionnaire **OptionalReferencePhotoCaptured**. Vous devez tout d’abord définir un objet qui contiendra vos informations de contexte. À vous de décider du nom et du contenu de cet objet. Cet exemple définit un objet qui possède des membres pour effectuer le suivi du nom de fichier et de l’orientation de l’appareil photo de la capture.

[!code-cs[AdvancedCaptureContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAdvancedCaptureContext)]

Créez une instance de votre objet de contexte, remplissez ses membres et passez-la dans la surcharge de [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/mt181388) qui accepte un objet en tant que paramètre.

[!code-cs[CaptureWithContext](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureWithContext)]

Dans le gestionnaire d’événements [**OptionalReferencePhotoCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181392), diffusez la propriété [**Context**](https://msdn.microsoft.com/library/windows/apps/mt181405) de l’objet [**OptionalReferencePhotoCapturedEventArgs**](https://msdn.microsoft.com/library/windows/apps/mt181404) sur votre classe d’objet de contexte. Cet exemple modifie le nom de fichier pour distinguer l’image de référence de l’image HDR finale, puis appelle la méthode d’assistance **ReencodeAndSavePhotoAsync** pour enregistrer l’image.

[!code-cs[OptionalReferencePhotoCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOptionalReferencePhotoCaptured)]

## Recevoir une notification lorsque toutes les images ont été capturées

La capture photo HDR comporte deux étapes. Plusieurs images sont capturées, puis ensuite traitées pour être transformées en image HDR finale. Vous ne pouvez pas initier une autre capture tant que les images HDR sources sont en cours de capture, mais vous pouvez lancer une capture une fois toutes les images capturées, avant que le post-traitement HDR ne soit terminé. L’événement [**AllPhotosCaptured**](https://msdn.microsoft.com/library/windows/apps/mt181387) est déclenché lorsque les captures HDR sont terminées, vous indiquant que vous pouvez lancer une autre capture. Un scénario courant consiste à désactiver le bouton de capture de votre interface utilisateur au début de la capture HDR, pour ensuite le réactiver lorsque **AllPhotosCaptured** est déclenché.

[!code-cs[AllPhotosCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAllPhotosCaptured)]

## Nettoyer l’objet AdvancedPhotoCapture

Lorsque votre application a terminé la capture, avant d’éliminer l’objet **MediaCapture**, vous devez arrêter l’objet [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/mt181386) en appelant [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/mt181391) et en définissant la variable membre sur null.

[!code-cs[CleanUpAdvancedPhotoCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpAdvancedPhotoCapture)]

## Rubriques connexes

* [Capturer des photos et des vidéos à l’aide de MediaCapture](capture-photos-and-video-with-mediacapture.md)



<!--HONumber=Jun16_HO4-->


