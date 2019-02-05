---
ms.assetid: a128edc8-8a80-4645-ac29-908ede2d1c72
description: Utiliser une instance MediaFrameReader avec MediaCapture pour récupérer des images à partir d’appareils photos couleur, de profondeur et infrarouges, de périphériques audio ou même de sources d’images personnalisés telles que celles qui produisent des images de suivi des squelettes.
title: Traiter des images multimédias avec MediaFrameReader
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a1d5a15bd88b7adc23ccc835001c384a91e65a31
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/05/2019
ms.locfileid: "9050702"
---
# <a name="process-media-frames-with-mediaframereader"></a>Traiter des images multimédias avec MediaFrameReader

Cet article vous explique comment utiliser une instance [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader) avec [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) pour récupérer des images à partir d’une ou de plusieurs sources disponibles, notamment d’appareils photos couleur, de profondeur et infrarouges, de périphériques audio ou même de sources d’images personnalisés telles que celles qui produisent des images de suivi des squelettes. Cette fonctionnalité est conçue pour être utilisée par les applications qui effectuent le traitement en temps réel des images multimédias, telles que les applications de caméra prenant en charge la profondeur et de réalité augmentée.

Si vous souhaitez simplement capturer du contenu vidéo ou des photos, comme c’est possible avec une application standard de photographie, vous avez probablement tout intérêt à valoriser l’une des autres techniques de capture prises en charge par [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture). Pour consulter une liste des techniques de capture multimédia disponibles et des articles vous expliquant comment les utiliser, consultez la page [**Appareil photo**](camera.md).

> [!NOTE] 
> Les fonctionnalités décrites dans cet article sont disponibles uniquement à partir de Windows10, version 1607.

> [!NOTE] 
> Il existe un exemple d’application Windows universelle qui illustre l’utilisation de **MediaFrameReader** pour afficher des images de différentes sources, notamment d’appareils photos couleur, de profondeur et infrarouges. Pour plus d’informations voir [Profils d’appareil photo](https://go.microsoft.com/fwlink/?LinkId=823230).

> [!NOTE] 
> Un nouvel ensemble d’API permettant d’utiliser **MediaFrameReader** avec des données audio a été introduit dans Windows10, version1803. Pour plus d’informations, voir [Traiter des trames audio avec MediaFrameReader](process-audio-frames-with-mediaframereader.md).


## <a name="setting-up-your-project"></a>Configuration de votre projet
Comme avec toute application utilisant **MediaCapture**, vous devez déclarer que votre application utilise la fonctionnalité *webcam* avant de tenter d’accéder à un appareil photo. Si votre application capture à partir d’un périphérique audio, vous devez également déclarer la fonctionnalité *microphone*. 

**Ajouter des fonctionnalités au manifeste de l’application**

1.  Dans Microsoft Visual Studio, dans l’**Explorateur de solutions**, ouvrez le concepteur pour le manifeste de l’application en double-cliquant sur l’élément **package.appxmanifest**.
2.  Sélectionnez l’onglet **Fonctionnalités**.
3.  Activez les cases à cocher **Webcam** et **Microphone**.
4.  Pour accéder à la bibliothèque d’images et à la vidéothèque, cochez les cases correspondant à **Bibliothèque d’images** et **Vidéothèque**.

L’exemple de code de cet article utilise des API des espaces de noms suivants, en plus de celles fournies par le modèle de projet par défaut.

[!code-cs[FramesUsing](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFramesUsing)]

## <a name="select-frame-sources-and-frame-source-groups"></a>Sélectionner des sources d’images et des groupes de sources d’images
De nombreuses applications qui traitent des images multimédias doivent récupérer ces éléments de plusieurs sources simultanément, comme des appareils photos couleur et de profondeur d’un appareil. L’objet [**MediaFrameSourceGroup**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup) représente un ensemble de sources d’images multimédias pouvant être utilisées simultanément. Appelez la méthode statique [**MediaFrameSourceGroup.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup.FindAllAsync) afin de récupérer une liste de l’ensemble des groupes de sources d’images pris en charge par l’appareil actuel.

[!code-cs[FindAllAsync](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFindAllAsync)]

Vous pouvez également créer un [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceWatcher) à l’aide de [**DeviceInformation.CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/br225427) et la valeur retournée à partir de [**MediaFrameSourceGroup.GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup.GetDeviceSelector) pour recevoir des notifications lorsque des groupes de la source d’images disponibles sur l’appareil change, par exemple, lors du branchement d’un appareil photo externe. Pour plus d’informations, consultez la page [**Énumérer les appareils**](https://msdn.microsoft.com/windows/uwp/devices-sensors/enumerate-devices).

Une instance [**MediaFrameSourceGroup**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup) dispose d’une collection d’objets [**MediaFrameSourceInfo**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo) qui décrivent les sources d’images incluses dans le groupe. Une fois les groupes de sources d’images disponibles sur cet appareil récupérés, vous pouvez sélectionner le groupe exposant les sources d’images qui vous intéressent.

L’exemple suivant vous présente le moyen le plus simple de sélectionner un groupe de sources d’images. Ce code parcourt simplement en boucle l’ensemble des groupes disponibles, puis chaque élément de la collection [**SourceInfos**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup.SourceInfos). Chacune des instances **MediaFrameSourceInfo** est examinée, afin de vérifier qu’elles prennent en charge les fonctionnalités recherchées. Dans ce cas, la propriété [**MediaStreamType**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo.MediaStreamType) est examinée pour vérifier qu’elle contient la valeur [**VideoPreview**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaStreamType), synonyme de production d’un flux d’aperçu vidéo par l’appareil, et la propriété [**SourceKind**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo.SourceKind) est examinée à la recherche de la valeur [**Color**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceKind), indiquant que la source fournit des images couleur.

[!code-cs[SimpleSelect](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSimpleSelect)]

Cette méthode d’identification du groupe des sources d’images et des sources d’images fonctionne efficacement avec des cas simples, mais si vous souhaitez sélectionner des sources d’images en fonction de critères plus complexes, le processus peut rapidement devenir très lourd. Une autre méthode consiste à effectuer cette sélection à l’aide de la syntaxe Linq et d’objets anonymes. L’exemple suivant utilise la méthode d’extension **Select** afin de transformer les objets **MediaFrameSourceGroup** de la liste *frameSourceGroups* en objets anonymes avec deuxchamps: *sourceGroup*, représentant le groupe en soi et *colorSourceInfo*, qui est associé à la source des images couleur du groupe. Le champ *colorSourceInfo* est défini sur le résultat de **FirstOrDefault**, qui sélectionne le premier objet pour lequel le prédicat fourni a la valeur TRUE. Dans ce cas, le prédicat a la valeur TRUE si le type de flux est **VideoPreview**, le type de source est **Color** et si l’appareil photo se trouve sur le panneau avant de l’appareil.

À partir de la liste des objets anonymes renvoyés de la requête décrite ci-dessus, la méthode d’extension **Where**, la méthode d’extension est utilisée pour sélectionner uniquement les objets dont le champ *colorSourceInfo* n’a pas la valeur NULL. Enfin, **FirstOrDefault** est appelé pour sélectionner le premier élément de la liste.

Vous pouvez désormais utiliser les champs de l’objet sélectionné afin d’obtenir les références à l’instance **MediaFrameSourceGroup** et à l’objet **MediaFrameSourceInfo** sélectionnés représentant l’appareil photo couleur. Ces éléments seront utilisés ultérieurement pour initialiser l’objet **MediaCapture** et créer une instance **MediaFrameReader** pour la source sélectionnée. Enfin, vous avez intérêt à procéder à un test afin de déterminer si le groupe de sources a la valeur NULL, ce qui signifie que l’appareil actuel ne présente pas vos sources de capture demandées.

[!code-cs[SelectColor](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSelectColor)]

L’exemple suivant utilise une technique similaire à ce qui est décrit plus haut pour sélectionner un groupe de sources comportant des appareils photos couleur, de profondeur et infrarouges.

[!code-cs[ColorInfraredDepth](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetColorInfraredDepth)]

> [!NOTE]
> À partir de Windows10, version1803, vous pouvez utiliser la classe [**MediaCaptureVideoProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfile) pour sélectionner une source d’images multimédias avec un ensemble de fonctionnalités souhaitées. Pour plus d’informations, consultez la section **Utiliser des profils vidéo pour sélectionner une source d’images** plus loin dans cet article.


## <a name="initialize-the-mediacapture-object-to-use-the-selected-frame-source-group"></a>Initialiser l’objet MediaCapture afin d’utiliser le groupe de sources d’images sélectionné
L’étape suivante consiste à initialiser l’objet **MediaCapture** afin d’utiliser le groupe de sources d’images sélectionné à l’étape précédente.

L’objet **MediaCapture** étant généralement utilisé à partir de multiples emplacements de votre application, vous devez déclarer une variable de membre de classe pour le contenir.

[!code-cs[DeclareMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

Créez une instance de l’objet **MediaCapture** en appelant le constructeur. Ensuite, créez un objet [**MediaCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureSettings) qui sera utilisé pour initialiser l’objet **MediaCapture**. Dans cet exemple, les paramètres suivants sont utilisés:

* [**SourceGroup**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.SourceGroup) - Ce paramètre indique au système le groupe de sources utilisé pour récupérer les images. N’oubliez pas que le groupe de sources définir un ensemble de sources d’images multimédias pouvant être utilisées simultanément.
* [**SharingMode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.SharingMode) - Ce paramètre indique au système si vous avez besoin d’un contrôle exclusif sur les appareils sources de capture. Si vous le définissez sur [**ExclusiveControl**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureSharingMode), cela signifie que vous pouvez modifier les paramètres du périphérique de capture, par exemple le format des images produites. Par ailleurs, si une autre application dispose d’ores et déjà du contrôle exclusif, votre application sera mise en échec lors de la tentative d’initialisation du périphérique de capture multimédia. Si vous le définissez sur [**SharedReadOnly**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureSharingMode), vous pouvez recevoir des images des sources d’images, même si elles sont utilisées par une autre application, mais vous ne pouvez pas modifier les paramètres des appareils.
* [**MemoryPreference**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.MemoryPreference) - Si vous spécifiez [**CPU**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureMemoryPreference), le système utilise la mémoireUC qui garantit qu’à l’arrivée des images, ces dernières sont disponibles en tant qu’objets [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap). Si vous spécifiez [**Auto**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureMemoryPreference), le système sélectionne dynamiquement l’emplacement optimal de mémoire dédié au stockage des images. Si le système choisit d’utiliser la mémoireGPU, les images multimédias arrivent en tant qu’objets [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface), et non en tant qu’instances **SoftwareBitmap**.
* [**StreamingCaptureMode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.StreamingCaptureMode) - Définissez ce paramètre sur [**Video**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.StreamingCaptureMode) afin d’indiquer qu’il n’est pas nécessaire de diffuser le contenu audio en continu.

Appelez [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) afin d’initialiser **MediaCapture** avec vos paramètres souhaités. Assurez-vous d’effectuer votre appel au sein d’un bloc *try*, ceci pour vous protéger en cas de mise en échec de l’initialisation.

[!code-cs[InitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetInitMediaCapture)]

## <a name="set-the-preferred-format-for-the-frame-source"></a>Définir le format préféré de la source d’images
Pour définir le format préféré d’une source d’images, vous devez obtenir un objet [**MediaFrameSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSource) représentant la source. Pour obtenir cet objet, accédez au dictionnaire [**Frames**](https://msdn.microsoft.com/library/windows/apps/Windows.Phone.Media.Capture.CameraCaptureSequence.Frames) de l’objet **MediaCapture** initialisé, en spécifiant l’identificateur de la source d’images que vous souhaitez utiliser. C’est pourquoi nous avons enregistré l’objet [**MediaFrameSourceInfo**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo) lorsque nous sélectionnions un groupe de sources d’images.

La propriété [**MediaFrameSource.SupportedFormats**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSource.SupportedFormats) comporte une liste d’objets [**MediaFrameFormat**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameFormat) décrivant les formats pris en charge pour la source d’images. Utilisez la méthode d’extension Linq **Where** pour sélectionner un format basé sur les propriétés souhaitées. Dans cet exemple, le format sélectionné présente une largeur de 1080pixels et peut fournir des images au format RVB32bits. La méthode d’extension **FirstOrDefault** sélectionne la première entrée de la liste. Si le format sélectionné est NULL, le format demandé n’est pas pris en charge par la source des images. Si le format est pris en charge, vous pouvez demander que la source l’utilise en appelant [**SetFormatAsync**](https://msdn.microsoft.com/library/windows/apps/).

[!code-cs[GetPreferredFormat](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetGetPreferredFormat)]

## <a name="create-a-frame-reader-for-the-frame-source"></a>Créer un lecteur d’images pour la source d’images
Pour recevoir les images pour une source d’images multimédias, utilisez une instance [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader).

[!code-cs[DeclareMediaFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareMediaFrameReader)]

Instanciez le lecteur d’images en appelant [**CreateFrameReaderAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.CreateFrameReaderAsync) sur votre objet **MediaCapture** initialisé. Le premier argument à appliquer à cette méthode est la source d’images à partir de laquelle vous souhaitez recevoir des images. Vous pouvez créer un lecteur séparé d’images pour chaque source d’images que vous souhaitez utiliser. Le second argument indique au système le format de sortie dans lequel vous souhaitez que les images arrivent. Cela peut vous éviter d’avoir à faire vos propres conversions sur les images en entrée. Notez que si vous spécifiez un format qui n’est pas pris en charge par la source d’images, une exception sera transmise. Aussi, assurez-vous que cette valeur se trouve dans la collection [**SupportedFormats**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSource.SupportedFormats).  

Après avoir créé le lecteur d’images, enregistrez un gestionnaire pour l’événement [**FrameArrived**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.FrameArrived) qui est déclenché quand une nouvelle image est disponible à partir de la source.

Indiquez au système de commencer à lire des images de la source en appelant [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.StartAsync).

[!code-cs[CreateFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCreateFrameReader)]

## <a name="handle-the-frame-arrived-event"></a>Gérer l’événement déclenché à l’arrivée des images
L’événement [**MediaFrameReader.FrameArrived**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.FrameArrived) est déclenché quand une nouvelle image est disponible. Vous pouvez choisir de traiter toutes les images qui arrivent ou seulement celles dont vous avez besoin. Étant donné que le lecteur d’images déclenche l’événement sur son propre thread, il vous faudra éventuellement implémenter une logique de synchronisation afin de garantir que vous n’essayez pas d’accéder aux mêmes données à partir de plusieurs threads. Cette section vous indique comment synchroniser les images couleur des dessins sur un contrôle d’images dans une pageXAML. Ce scénario traite de la contrainte de synchronisation supplémentaire qui implique que l’ensemble des mises à jour des contrôles XAML soient effectuées sur le même thread d’interface utilisateur.

La première étape de l’affichage des images au format XAML consiste à créer un contrôle Image. 

[!code-xml[ImageElementXAML](./code/Frames_Win10/Frames_Win10/MainPage.xaml#SnippetImageElementXAML)]

Dans votre page code-behind, déclarez une variable de membre de classe de type **SoftwareBitmap**, qui sera utilisée en tant que mémoire tampon d’arrière-plan sur laquelle seront copiées l’ensemble des images entrantes. Notez que les données d’images en soi ne sont pas copiées, ce sont les références d’objet qui le sont. Par ailleurs, déclarez une valeur booléenne détectant l’exécution de votre opération d’interface utilisateur.

[!code-cs[DeclareBackBuffer](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareBackBuffer)]

Les images arrivant en tant qu’objets **SoftwareBitmap**, vous devez créer un objet [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) qui vous permet d’utiliser une instance **SoftwareBitmap** en tant que source pour un élément **Control** XAML. Vous devez définir la source d’images au sein de votre code avant de démarrer le lecteur d’images.

[!code-cs[ImageElementSource](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetImageElementSource)]

Il est désormais temps d’implémenter le gestionnaire d’événements **FrameArrived**. Quand le gestionnaire est appelé, le paramètre *sender* contient une référence à l’objet **MediaFrameReader** qui déclenche l’événement. Appelez [**TryAcquireLatestFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.TryAcquireLatestFrame) sur cet objet afin d’essayer d’obtenir la dernière image. Comme son nom l’indique, **TryAcquireLatestFrame** peut ne pas réussir à renvoyer une image. Par conséquent, quand vous accédez aux propriétés VideoMediaFrame puis SoftwareBitmap, assurez-vous de détecter la définition de la valeur NULL. Dans cet exemple, l’opérateur conditionnel NULL ? est utilisé pour accéder à l’instance **SoftwareBitmap**. Ensuite, la valeur NULL est recherchée sur l’objet récupéré.

Le contrôle **Image** peut afficher des images uniquement au format BRGA8, avec aucune valeur alpha ou avec des valeurs alpha prémultipliées. Si l’image arrivante n’est pas dans ce format, la méthode statique [**Convert**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap.Covert) est utilisée pour convertir l’image bitmap logicielle au format approprié.

Ensuite, la méthode [**Interlocked.Exchange**](https://msdn.microsoft.com/library/bb337971) est utilisée pour remplacer la référence de l’image bitmap en entrée par l’image bitmap de la mémoire tampon d’arrière-plan. Cette méthode échange des références au cours d’une opération atomique thread-safe. Après le remplacement, l’ancienne image de mémoire tampon d’arrière-plan, désormais dans la variable *softwareBitmap*, est supprimée à des fins de nettoyage de ses ressources.

Ensuite, l’instance [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Core.CoreDispatcher) associée à l’élément **Image** est utilisée pour créer une tâche qui s’exécutera sur le thread d’interface utilisateur en appelant [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317). Étant donné que les tâches asynchrones seront exécutées au sein de la tâche, l’expression lambda transmise à **RunAsync** est déclarée avec le mot-clé *async*.

Au sein de la tâche, la variable *_taskRunning* est examinée afin de garantir qu’une seule instance de la tâche s’exécute à la fois. SI la tâche n’est pas en cours d’exécution, l’élément *_taskRunning* est défini sur True, ceci pour prévenir toute nouvelle réexécution. Dans une boucle *while*, **Interlocked.Exchange** est appelé pour l’exécution d’une copie d’une mémoire tampon d’arrière-plan sur une instance **SoftwareBitmap** temporaire, jusqu’à ce que l’image de la mémoire tampon d’arrière-plan soit définie sur NULL. À chaque fois que l’image bitmap temporaire est remplie, la propriété **Source** du contrôle **Image** est castée en instance **SoftwareBitmapSource**, puis [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn997856) est appelée pour définir la source de l’image.

Enfin, la variable *_taskRunning* est redéfinie sur False, afin que la tâche puisse à nouveau s’exécuter lors du prochain appel du gestionnaire.

> [!NOTE] 
> Si vous accédez aux objets [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.VideoMediaFrame.SoftwareBitmap) ou [**Direct3DSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.VideoMediaFrame.Direct3DSurface) fournis par la propriété [**VideoMediaFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReference.VideoMediaFrame) d’un [**MediaFrameReference**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReference), le système crée une référence forte à ces objets. Autrement dit, ils ne sont pas supprimés lorsque vous appelez [**Dispose**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReference.Close) sur le conteneur **MediaFrameReference**. Vous devez appeler la méthode **Dispose** de **SoftwareBitmap** ou de **Direct3DSurface** explicitement et directement pour les objets à supprimer immédiatement. Sinon, le récupérateur de mémoire va libérer de la mémoire pour ces objets. Mais vous ne pouvez pas savoir quand cela se produit, et si le nombre de surfaces ou d’images bitmap allouées dépasse la quantité maximale autorisée par le système, le flux de nouvelles images s’arrête. Vous pouvez copier les images récupérées à l’aide de la méthode [**SoftwareBitmap.Copy**](https://docs.microsoft.com/en-us/uwp/api/windows.graphics.imaging.softwarebitmap.copy) par exemple, puis libérer les images d’origine pour contourner cette limitation. De même, si vous créez l’objet **MediaFrameReader** à l’aide de la surcharge [CreateFrameReaderAsync(Windows.Media.Capture.Frames.MediaFrameSource inputSource, System.String outputSubtype, Windows.Graphics.Imaging.BitmapSize outputSize)](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_System_String_Windows_Graphics_Imaging_BitmapSize_) ou [CreateFrameReaderAsync(Windows.Media.Capture.Frames.MediaFrameSource inputSource, System.String outputSubtype)](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_System_String_), les images retournées sont des copies des données d’images d’origine, et elles n’entraînent donc pas l’arrêt de l’acquisition des images lorsqu’elles sont conservées. 


[!code-cs[FrameArrived](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFrameArrived)]

## <a name="cleanup-resources"></a>Nettoyage des ressources
Lorsque vous avez fini de lire les images, assurez-vous d’arrêter le lecteur d’images multimédias en appelant [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.StopAsync), en annulant l’enregistrement du gestionnaire **FrameArrived** et en supprimant l’objet **MediaCapture**.

[!code-cs[Cleanup](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCleanup)]

Pour plus d’informations sur le nettoyage de vos objets de capture multimédia lorsque votre application est interrompue, consultez la section [**Afficher l’aperçu de l’appareil photo**](simple-camera-preview-access.md).

## <a name="the-framerenderer-helper-class"></a>La classe d’assistance FrameRenderer
Le [profil d’appareil photo](https://go.microsoft.com/fwlink/?LinkId=823230) Windows universel fournit une classe d’assistance qui facilite l’affichage d’images de sources couleur, infrarouges et de profondeur dans votre application. Généralement, vous ne vous contentez pas d’afficher les données infrarouge et de profondeur à l’écran, mais cette classe d’assistance est un outil utile permettant d’illustrer la fonctionnalité du lecteur d’images et de déboguer votre propre implémentation du lecteur d’images.

La classe d’assistance **FrameRenderer** implémente les méthodes suivantes.

* Constructeur **FrameRenderer** - Le constructeur initialise la classe d’assistance afin d’utiliser l’élément XAML [**Image**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Image) transmis pour l’affichage des images multimédias.
* **ProcessFrame** - Cette méthode affiche une image multimédia, représentée par une instance [**MediaFrameReference**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReference), dans l’élément **Image** transmis dans le constructeur. Vous devez généralement appeler cette méthode à partir de votre gestionnaire d’événement [**FrameArrived**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.FrameArrived), en transmettant l’image renvoyée par [**TryAcquireLatestFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.TryAcquireLatestFrame).
* **ConvertToDisplayableImage** - Cette méthode vérifie le format de l’image multimédia et si nécessaire la convertit en format affichable. Pour les images couleur, elle garantit que le format couleur est BGRA8 et que le mode alpha bitmap est prémultiplié. Pour les images de profondeur et infrarouges, chaque ligne de balayage est traitée pour convertir les valeurs de profondeur et infrarouges en gradient pseudocolor, à l’aide de la classe **PseudoColorHelper** qui est également incluse dans l’exemple et répertoriée ci-dessous.

> [!NOTE] 
> Pour procéder à une manipulation de pixels sur les images **SoftwareBitmap**, vous devez accéder à une mémoire tampon native. Pour ce faire, vous devez utiliser l’interface COM IMemoryBufferByteAccess COM incluse dans l’ensemble du code ci-dessous, et vous devez mettre à jour vos propriétés de projet afin de permettre la compilation du code non sécurisé. Pour plus d’informations, consultez la page [Créer, modifier et enregistrer des images bitmap](imaging.md).

[!code-cs[IMemoryBufferByteAccess](./code/Frames_Win10/Frames_Win10/FrameRenderer.cs#SnippetIMemoryBufferByteAccess)]

[!code-cs[FrameArrived](./code/Frames_Win10/Frames_Win10/FrameRenderer.cs#SnippetFrameRenderer)]

## <a name="use-multisourcemediaframereader-to-get-time-corellated-frames-from-multiple-sources"></a>Utiliser MultiSourceMediaFrameReader pour obtenir des images corrélées dans le temps à partir de plusieurs sources
À partir de Windows10, version1607, vous pouvez utiliser [**MultiSourceMediaFrameReader**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader) pour recevoir des images corrélées dans le temps à partir de plusieurs sources. Cette API facilite le traitement qui nécessite des images provenant de plusieurs sources et prises dans un court intervalle de temps, comme l'utilisation de la classe [**DepthCorrelatedCoordinateMapper**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.depthcorrelatedcoordinatemapper). Une limitation de l’utilisation de cette nouvelle méthode réside dans le fait que les événements déclenchés à l’arrivée des images sont uniquement déclenchés à la cadence de la source de capture plus lente. Les images supplémentaires provenant de sources plus rapides seront supprimées. En outre, étant donné que le système s'attend à ce que les images arrivent de sources différentes à différentes cadences, il ne détecte pas automatiquement si une source a cessé complètement de générer des images. L’exemple de code de cette section montre comment utiliser un événement pour créer votre propre logique de délai d’expiration qui sera appelée si des images corrélées n'arrivent pas dans une limite de temps définie par l'application.

Les étapes d’utilisation de [**MultiSourceMediaFrameReader**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader) sont similaires aux étapes d’utilisation de [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader) décrites précédemment dans cet article. Cet exemple utilise une source de couleur et une source de profondeur. Déclarez des variables de chaîne pour stocker les ID de source d’images multimédias qui seront utilisés pour sélectionner des images à partir de chaque source. Déclarez ensuite un [**ManualResetEventSlim**](https://docs.microsoft.com/dotnet/api/system.threading.manualreseteventslim?view=netframework-4.7), un [**CancellationTokenSource**](https://msdn.microsoft.com/library/system.threading.cancellationtokensource.aspx)et un [**EventHandler**](https://msdn.microsoft.com/library/system.eventhandler.aspx) qui seront utilisés pour implémenter la logique de délai d’expiration pour cet exemple. 

[!code-cs[MultiFrameDeclarations](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMultiFrameDeclarations)]

À l’aide des techniques décrites précédemment dans cet article, recherchez un [**MediaFrameSourceGroup**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup) qui inclut les sources de couleur et de profondeur requises pour cet exemple de scénario. Après avoir sélectionné le groupe de sources d’images souhaité, récupérez le [**MediaFrameSourceInfo**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo) pour chaque source d’image.

[!code-cs[SelectColorAndDepth](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSelectColorAndDepth)]

Créez et initialisez un objet **MediaCapture**, en transmettant le groupe de sources d’images sélectionné dans les paramètres d’initialisation.

[!code-cs[MultiFrameInitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMultiFrameInitMediaCapture)]

Après avoir initialisé l'objet **MediaCapture**, récupérez les objets [**MediaFrameSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSource) pour les appareils photo couleur et de profondeur. Stockez l’ID de chaque source afin de pouvoir sélectionner l’image arrivante pour la source correspondante.

[!code-cs[GetColorAndDepthSource](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetGetColorAndDepthSource)]

Créez et initialisez le **MultiSourceMediaFrameReader** en appelant [**CreateMultiSourceFrameReaderAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createmultisourceframereaderasync) et en transmettant un tableau de sources d’images que le lecteur utilisera. Inscrivez un gestionnaire d’événements pour l'événement [**FrameArrived**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader.FrameArrived). Cet exemple crée une instance de la classe d'assistance **FrameRenderer** décrite précédemment dans cet article pour restituer les images vers un contrôle **Image**. Démarrez le lecteur d'images en appelant [**StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader.StartAsync).

Inscrivez un gestionnaire d'événements pour l'événement **CorellationFailed** déclaré précédemment dans cet exemple. Nous signalerons cet événement si l'une des sources d’images multimédias utilisées cesse de générer des images. Pour finir, appelez [**Task.Run**](https://msdn.microsoft.com/library/hh195051.aspx) pour appeler la méthode d'assistance de délai d'expiration, **NotifyAboutCorrelationFailure**, sur un thread distinct. L’implémentation de cette méthode est illustrée plus loin dans cet article.

[!code-cs[InitMultiFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetInitMultiFrameReader)]

L'événement **FrameArrived** est déclenché chaque fois qu’une nouvelle image est disponible à partir de l'ensemble des sources d’images multimédias gérées par le **MultiSourceMediaFrameReader**. Cela signifie que l’événement sera déclenché selon la cadence de la source multimédia la plus lente. Si une source produit plusieurs images dans le temps que met une source plus lente à produire une image, les images supplémentaires provenant de la source rapide seront supprimées. 

Obtenez le [**MultiSourceMediaFrameReference**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereference) associé à l’événement en appelant [**TryAcquireLatestFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader.TryAcquireLatestFrame). Obtenez le **MediaFrameReference** associé à chaque source d’images multimédia en appelant [**TryGetFrameReferenceBySourceId**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereference.trygetframereferencebysourceid), en transmettant les chaînes d’identification stockées lorsque le lecteur d’images a été initialisé.

Appelez la méthode [**Set**](https://msdn.microsoft.com/library/system.threading.manualreseteventslim.set.aspx) de l'objet **ManualResetEventSlim** pour signaler l'arrivée des images. Nous vérifierons cet événement dans la méthode **NotifyCorrelationFailure** qui s’exécute dans un thread distinct. 

Pour finir, effectuez tout traitement requis sur les images multimédias corrélées dans le temps. Cet exemple affiche simplement l'image de la source de profondeur.

[!code-cs[MultiFrameArrived](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMultiFrameArrived)]

La méthode d'assistance **NotifyCorrelationFailure** a été exécutée sur un thread distinct, après le démarrage du lecteur d’images. Dans cette méthode, vérifiez si l’événement déclenché par la réception d'une image a été signalé. N’oubliez pas que, dans le gestionnaire **FrameArrived**, nous définissons cet événement à chaque arrivée d'un groupe d'images corrélées. Si l’événement n’a pas été signalé pour une période de temps définie par l’application - 5secondes est une valeur raisonnable - et que la tâche n’a pas été annulée avec **CancellationToken**, il est probable qu’une des sources d’images multimédias ait cessé de lire les images. Dans ce cas, vous souhaiterez généralement arrêter le lecteur d’images et déclencher l'événement **CorrelationFailed** défini par l'application. Dans le gestionnaire de cet événement, vous pouvez arrêter le lecteur d’images et nettoyer ses ressources associées comme indiqué précédemment dans cet article.

[!code-cs[NotifyCorrelationFailure](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetNotifyCorrelationFailure)]

[!code-cs[CorrelationFailure](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCorrelationFailure)]

## <a name="use-buffered-frame-acquisition-mode-to-preserve-the-sequence-of-acquired-frames"></a>Utiliser le mode d’acquisition en mémoire tampon des images pour conserver l’ordre des images acquises
À partir de Windows10, version1709, vous pouvez définir la propriété **[AcquisitionMode](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.AcquisitionMode)** d’un objet **MediaFrameReader** ou **MultiSourceMediaFrameReader** sur **Buffered** pour conserver l’ordre des images transmises dans votre application à partir de la source d’images.

[!code-cs[SetBufferedFrameAcquisitionMode](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSetBufferedFrameAcquisitionMode)]

Dans le mode d’acquisition par défaut, **Realtime**, si plusieurs images sont acquises à partir de la source alors que votre application gère toujours l’événement **FrameArrived** pour une image précédente, le système envoie à votre application l’image acquise la plus récente et ignore les images supplémentaires en attente dans la mémoire tampon. Ainsi, votre application dispose de l’image disponible la plus récente à tout moment. Il s’agit généralement du mode le plus utile pour les applications de vision par ordinateur en temps réel. 

En mode d’acquisition **Buffered**, le système conserve toutes les images dans la mémoire tampon et les transmet à votre application par le biais de l’événement **FrameArrived** dans leur ordre de réception. Notez que dans ce mode, lorsque la mémoire tampon du système pour les images est saturée, le système cesse d’acquérir de nouvelles images jusqu’à ce que votre application exécute l’événement **FrameArrived** pour les images précédentes, ce qui libère plus d’espace dans la mémoire tampon.

## <a name="use-mediasource-to-display-frames-in-a-mediaplayerelement"></a>Utiliser MediaSource pour afficher les images dans un objet MediaPlayerElement
À partir de Windows, version1709, vous pouvez afficher les images acquises à partir d’un objet **MediaFrameReader** directement dans un contrôle **[MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement)** de votre page XAML. Pour ce faire, utilisez **[MediaSource.CreateFromMediaFrameSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediaframesource)** pour créer un objet **[MediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource)** qui peut être utilisé directement par un objet **[MediaPlayer](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)** associé à un objet **MediaPlayerElement**. Pour des informations détaillées sur l’utilisation de **MediaPlayer** et **MediaPlayerElement**, voir [Lire du contenu audio et vidéo avec MediaPlayer](play-audio-and-video-with-mediaplayer.md).

Les exemples de code suivants illustrent une implémentation simple qui affiche les images d’un appareil photo frontal et arrière simultanément dans une page XAML.

Tout d’abord, ajoutez deux contrôles **MediaPlayerElement** à votre page XAML.

[!code-xml[MediaPlayerElement1XAML](./code/Frames_Win10/Frames_Win10/MainPage.xaml#SnippetMediaPlayerElement1XAML)]

[!code-xml[MediaPlayerElement2XAML](./code/Frames_Win10/Frames_Win10/MainPage.xaml#SnippetMediaPlayerElement2XAML)]

Ensuite, à l’aide des techniques présentées dans les sections précédentes de cet article, sélectionnez un objet **MediaFrameSourceGroup** contenant des objets **MediaFrameSourceInfo** pour les appareils photo couleur sur le panneau avant et arrière. Notez que l’objet **MediaPlayer** ne convertit pas automatiquement les images des formats sans couleur, comme les données de profondeur et infrarouges, en données de couleur. L’utilisation d’autres types de capteur peut produire des résultats inattendus. 

[!code-cs[MediaSourceSelectGroup](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMediaSourceSelectGroup)]

Initialisez l’objet **MediaCapture** afin d’utiliser l’objet **MediaFrameSourceGroup** sélectionné.

[!code-cs[MediaSourceInitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMediaSourceInitMediaCapture)]

Enfin, appelez **[MediaSource.CreateFromMediaFrameSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediaframesource)** pour créer un objet **MediaSource** pour chaque source d’images à l’aide de la propriété **[Id](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo.Id)** de l’objet **MediaFrameSourceInfo** associé pour sélectionner l’une des sources d’images dans la collection **[FrameSources](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.FrameSources)** de l’objet **MediaCapture**. Initialisez un nouvel objet **MediaPlayer** et affectez-le à un objet **MediaPlayerElement** en appelant **[SetMediaPlayer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.MediaPlayer)**. Définissez ensuite la propriété **[Source](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Source)** sur le nouvel objet **MediaSource** créé.

[!code-cs[MediaSourceMediaPlayer](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMediaSourceMediaPlayer)]

## <a name="use-video-profiles-to-select-a-frame-source"></a>Utiliser des profils vidéo pour sélectionner une source d’images

Un profil d’appareil photo, représenté par un objet [**MediaCaptureVideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn926694), représente un ensemble de fonctionnalités fournies par un appareil de capture spécifique, comme les fréquences d’images, les résolutions ou les fonctionnalités avancées comme la capture HDR. Un appareil de capture peut prendre en charge plusieurs profils, ce qui vous permet de sélectionner le profil optimisé pour votre scénario de capture. À partir de Windows10, version1803, vous pouvez utiliser **MediaCaptureVideoProfile** pour sélectionner une source d’images multimédias avec des fonctionnalités spécifiques avant d’initialiser l’objet **MediaCapture**. L’exemple de méthode suivant recherche un profil vidéo qui prend en charge la capture HDR avec Wide Color Gamut (WCG) et retourne un objet **MediaCaptureInitializationSettings** qui peut être utilisé pour initialiser l’objet **MediaCapture** pour utiliser l’appareil et le profil sélectionnés.

Commencez par appeler [**MediaFrameSourceGroup.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup.FindAllAsync) pour obtenir la liste de tous les groupes de sources d’images multimédias disponibles sur l’appareil actuel. Parcourez chaque groupe de sources et appelez [**MediaCapture.FindKnownVideoProfiles**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) pour obtenir la liste de tous les profils vidéo pour le groupe de sources actuel qui prend en charge le profil spécifié, dans ce cas HDR avec photo WCG. Si un profil répondant aux critères est trouvé, créez un objet **MediaCaptureInitializationSettings** et définissez le **VideoProfile** sur le profil de sélection et le **VideoDeviceId** sur la propriété **Id** du groupe de sources d’images multimédias actuel.

[!code-cs[GetSettingsWithProfile](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetGetSettingsWithProfile)]

Pour plus d’informations sur l’utilisation des profils d’appareil photo, voir [Profils de l’appareil photo](camera-profiles.md).

## <a name="related-topics"></a>Rubriquesassociées

* [Appareil photo](camera.md)
* [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Profils d’appareil photo](https://go.microsoft.com/fwlink/?LinkId=823230)
 

 




