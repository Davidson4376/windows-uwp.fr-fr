---
author: drewbatgit
ms.assetid: C4DB495D-1F91-40EF-A55C-5CABBF3269A2
description: "Les API de l’espace de noms Windows.Media.Editing permettent de développer rapidement des applications permettant aux utilisateurs de créer des compositions multimédias à partir de fichiers sources audio et vidéo."
title: "Compositions multimédias et modification"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: ee46b6d4ad116034cd84f062e7bf710ff8600479

---

# Compositions multimédias et modification

\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Cet article vous montre comment utiliser les API de l’espace de noms [**Windows.Media.Editing**](https://msdn.microsoft.com/library/windows/apps/dn640565) pour développer rapidement des applications qui permettent aux utilisateurs de créer des compositions multimédias à partir de fichiers sources audio et vidéo. Les fonctionnalités de l’infrastructure incluent la possibilité d’ajouter simultanément plusieurs clips vidéo, d’ajouter des superpositions d’images et de vidéos, d’ajouter du son en arrière-plan et d’appliquer des effets audio et vidéos, par programme. Une fois créées, les compositions multimédias peuvent être rendues dans un fichier multimédia plat pour lecture ou partage, mais il est également possible de les sérialiser et désérialiser à partir du disque en permettant à l’utilisateur de charger et de modifier les compositions précédemment créées. Toutes ces fonctionnalités sont proposées dans l’interface simple d’utilisation Windows Runtime, qui réduit considérablement la quantité de code requis et sa complexité pour effectuer ces tâches, en comparaison avec l’APi [Microsoft Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197) de bas niveau.

## Créer une composition multimédia

La classe [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646) est le conteneur de tous les clips multimédias qui constituent la composition et est responsable du rendu de la composition finale, du chargement et de l’enregistrement des compositions sur le disque et de fournir un flux d’aperçu de la composition afin que l’utilisateur puisse l’afficher dans l’interface utilisateur. Pour utiliser **MediaComposition** dans votre application, vous devez inclure l’espace de noms [**Windows.Media.Editing**](https://msdn.microsoft.com/library/windows/apps/dn640565), ainsi que l’espace de noms [**Windows.Media.Core**](https://msdn.microsoft.com/library/windows/apps/dn278962) qui fournit les API associées dont vous aurez besoin.

[!code-cs[Namespace1](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetNamespace1)]

L’objet **MediaComposition** est accessible à partir de plusieurs points dans votre code. Vous allez généralement déclarer une variable de membre qui permettra de le stocker.

[!code-cs[DeclareMediaComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetDeclareMediaComposition)]

Le constructeur de **MediaComposition** ne prend pas d’arguments.

[!code-cs[MediaCompositionConstructor](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetMediaCompositionConstructor)]

## Ajouter des clips multimédias à une composition

Les compositions multimédia contiennent généralement un ou plusieurs clips vidéo. Vous pouvez utiliser un élément [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/hh738369) pour permettre à l’utilisateur de sélectionner un fichier vidéo. Une fois le fichier sélectionné, créez un nouvel objet [**MediaClip**](https://msdn.microsoft.com/library/windows/apps/dn652596) destiné à contenir le clip vidéo en appelant [**MediaClip.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652607). Ajoutez ensuite le clip à la liste [**Clips**](https://msdn.microsoft.com/library/windows/apps/dn652648) de l’objet **MediaComposition**.

[!code-cs[PickFileAndAddClip](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetPickFileAndAddClip)]

-   Les clips multimédias s’affichent dans l’élément **MediaComposition** dans l’ordre dans lequel ils apparaissent dans la liste [**Clips**](https://msdn.microsoft.com/library/windows/apps/dn652648).

-   Un élément **MediaClip** ne peut être inclus qu’une seule fois dans une composition. L’ajout d’un élément **MediaClip** déjà utilisé par la composition se traduit par une erreur. Pour réutiliser un clip vidéo plusieurs fois dans une composition, appelez [**Clone**](https://msdn.microsoft.com/library/windows/apps/dn652599) pour créer de nouveaux objets **MediaClip**, qui peuvent ensuite être ajoutés à la composition.

-   Les applications Windows universelles ne sont pas autorisées à accéder à l’ensemble du système de fichiers. La propriété [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) de la classe [**StorageApplicationPermissions**](https://msdn.microsoft.com/library/windows/apps/br207456) permet à votre application de stocker l’enregistrement d’un fichier qui a été sélectionné par l’utilisateur afin que vous puissiez conserver les autorisations d’accès au fichier. La propriété **FutureAccessList** comporte 1000 entrées maximum. Votre application a donc besoin de gérer la liste pour s’assurer qu’elle n’est pas complète. Cela est particulièrement important si vous envisagez de prendre en charge le chargement et la modification de compositions déjà créées.

-   Un élément **MediaComposition** prend en charge les clips vidéo au format MP4.

-   Si un fichier vidéo contient plusieurs pistes audio intégrées, vous pouvez sélectionner la piste audio à utiliser dans la composition en définissant la propriété [**SelectedEmbeddedAudioTrackIndex**](https://msdn.microsoft.com/library/windows/apps/dn652627).

-   Créez un élément **MediaClip** avec une seule couleur de remplissage de la trame tout entière en appelant [**CreateFromColor**](https://msdn.microsoft.com/library/windows/apps/dn652605) et en spécifiant une couleur et une durée pour le clip.

-   Créez un élément **MediaClip** à partir d’un fichier image en appelant [**CreateFromImageFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652610) et en spécifiant un fichier image et une durée pour le clip.

-   Créez un élément **MediaClip** à partir d’une [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn965505) en appelant [**CreateFromSurface**](https://msdn.microsoft.com/library/dn764774) et en spécifiant une surface et une durée à partir du clip.

## Afficher un aperçu de la composition dans un objet MediaElement

Pour permettre à l’utilisateur d’afficher la composition multimédia, ajoutez une classe [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) au fichier XAML qui définit votre interface utilisateur.

[!code-xml[MediaElement](./code/MediaEditing/cs/MainPage.xaml#SnippetMediaElement)]

Déclarez une variable de membre de type [**MediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn282716).


[!code-cs[DeclareMediaStreamSource](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetDeclareMediaStreamSource)]

Appelez la méthode [**GeneratePreviewMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn652674) de l’objet **MediaComposition** pour créer un élément **MediaStreamSource** pour la composition, puis appelez la méthode [**SetMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn299029) de l’élément **MediaElement**. La composition peut désormais être affichée dans l’interface utilisateur.


[!code-cs[UpdateMediaElementSource](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetUpdateMediaElementSource)]

-   L’élément **MediaComposition** doit contenir au moins un clip multimédia avant d’appeler [**GeneratePreviewMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn652674), sans quoi l’objet renvoyé sera null.

-   La chronologie de **MediaElement** n’est pas automatiquement mise à jour de manière à refléter les changements relatifs à la composition. Il est recommandé d’appeler à la fois **GeneratePreviewMediaStreamSource** et **SetMediaStreamSource** chaque fois que vous apportez des modifications à la composition et que vous voulez mettre à jour l’interface utilisateur.

Il est recommandé de définir l’objet **MediaStreamSource** et la propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) de **MediaElement** sur null lorsque l’utilisateur quitte la page afin de libérer les ressources associées.

[!code-cs[OnNavigatedFrom](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

## Restituer la composition dans un fichier vidéo

Pour restituer une composition multimédia dans un fichier vidéo plat de manière à pouvoir la partager et la visualiser sur d’autres appareils, vous devrez utiliser les API de l’espace de noms [**Windows.Media.Transcoding**](https://msdn.microsoft.com/library/windows/apps/br207105). Pour mettre à jour l’interface utilisateur en fonction de la progression de l’opération asynchrone, vous aurez également besoin des API de l’espace de noms [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383).

[!code-cs[Namespace2](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetNamespace2)]

Après avoir autorisé l’utilisateur à sélectionner un fichier de sortie à l’aide d’une classe [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871), restituez la composition dans le fichier sélectionné en appelant la méthode [**RenderToFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652690) de l’objet **MediaComposition**. Le reste du code de l’exemple suivant applique simplement le modèle de gestion d’une [**AsyncOperationWithProgress**](https://msdn.microsoft.com/library/windows/desktop/br205807).

[!code-cs[RenderCompositionToFile](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetRenderCompositionToFile)]

-   La préférence [**MediaTrimmingPreference**](https://msdn.microsoft.com/library/windows/apps/dn640561) vous permet de privilégier la vitesse de l’opération de transcodage par rapport à la précision de découpage des clips multimédias adjacents. **Fast** accélère le transcodage par un découpage moins précis et **Precise** le ralentit par une augmentation de la précision du découpage.

## Découper un clip vidéo

Réduisez la durée d’un clip vidéo d’une composition en définissant la propriété [**TrimTimeFromStart**](https://msdn.microsoft.com/library/windows/apps/dn652637) des objets [**MediaClip**](https://msdn.microsoft.com/library/windows/apps/dn652596), la propriété [**TrimTimeFromEnd**](https://msdn.microsoft.com/library/windows/apps/dn652634) ou les deux.

[!code-cs[TrimClipBeforeCurrentPosition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetTrimClipBeforeCurrentPosition)]

-   Vous pouvez utiliser l’interface utilisateur que vous souhaitez pour permettre à l’utilisateur de spécifier les valeurs de découpage de début et de fin. L’exemple ci-dessus utilise la propriété [**Position**](https://msdn.microsoft.com/library/windows/apps/br227407) de l’élément **MediaElement**, pour commencer par déterminer quel MediaClip est lu au niveau de la position actuelle de la composition en vérifiant les propriétés [**StartTimeInComposition**](https://msdn.microsoft.com/library/windows/apps/dn652629) et [**EndTimeInComposition**](https://msdn.microsoft.com/library/windows/apps/dn652618). Les propriétés **Position** et **StartTimeInComposition** servent à nouveau pour calculer la durée à découper depuis le début du clip. La méthode **FirstOrDefault** est une méthode d’extension à partir de l’espace de noms **System.Linq** qui simplifie le code permettant de sélectionner les éléments d’une liste.
-   La propriété [**OriginalDuration**](https://msdn.microsoft.com/library/windows/apps/dn652625) de l’objet **MediaClip** vous permet de connaître la durée du clip multimédia sans découpage.
-   La propriété [**TrimmedDuration**](https://msdn.microsoft.com/library/windows/apps/dn652631) vous permet de connaître la durée du clip multimédia après découpage.
-   La spécification d’une valeur de découpage supérieure à la durée initiale du clip ne génère pas d’erreur. Toutefois, si une composition ne contient qu’un seul clip, et que celui-ci a subi un découpage complet le ramenant à une durée égale à zéro dû à la spécification d’une valeur de découpage importante, un appel ultérieur à [**GeneratePreviewMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn652674) renvoie la valeur null, comme si la composition ne comportait aucun clip.

## Ajouter une piste audio en arrière-plan à une composition

Pour ajouter une piste audio en arrière-plan à une composition, chargez un fichier audio, puis créez un objet [**BackgroundAudioTrack**](https://msdn.microsoft.com/library/windows/apps/dn652544) en appelant la méthode de fabrique [**BackgroundAudioTrack.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652561). Ajoutez ensuite l’élément **BackgroundAudioTrack** à la propriété [**BackgroundAudioTracks**](https://msdn.microsoft.com/library/windows/apps/dn652647) de la composition.

[!code-cs[AddBackgroundAudioTrack](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddBackgroundAudioTrack)]

-   Un élément **MediaComposition** prend en charge les pistes audio en arrière-plan aux formats suivants: MP3, WAV, FLAC.

-   Une piste audio en arrière-plan

-   Comme pour les fichiers vidéo, vous devez utiliser la classe [**StorageApplicationPermissions**](https://msdn.microsoft.com/library/windows/apps/br207456) pour préserver l’accès aux fichiers de la composition.

-   Comme pour **MediaClip**, un élément **BackgroundAudioTrack** ne peut être inclus qu’une seule fois dans une composition. L’ajout d’un élément **BackgroundAudioTrack** déjà utilisé par la composition se traduit par une erreur. Pour réutiliser une piste audio plusieurs fois dans une composition, appelez [**Clone**](https://msdn.microsoft.com/library/windows/apps/dn652599) pour créer de nouveaux objets **MediaClip**, qui peuvent ensuite être ajoutés à la composition.

-   Par défaut, la lecture des pistes audio en arrière-plan commence au début de la composition. En présence de plusieurs pistes en arrière-plan, la lecture de toutes les pistes commence au début de la composition. Pour que la lecture d’une piste audio en arrière-plan se déclenche à un autre moment, définissez la propriété [**Delay**](https://msdn.microsoft.com/library/windows/apps/dn652563) sur le décalage souhaité.

## Ajouter une superposition à une composition

Les superpositions permettent d’empiler plusieurs couches de vidéos les unes sur les autres au sein d’une composition. Une composition peut contenir plusieurs couches de superposition, qui peuvent elles-mêmes inclure plusieurs superpositions. Créez un objet [**MediaOverlay**](https://msdn.microsoft.com/library/windows/apps/dn764793) en transmettant un élément **MediaClip** à son constructeur. Définissez la position et l’opacité de la superposition, puis créez une classe [**MediaOverlayLayer**](https://msdn.microsoft.com/library/windows/apps/dn764795) et ajoutez l’élément **MediaOverlay** à sa liste [**Overlays**](https://msdn.microsoft.com/library/windows/desktop/dn280411). Ajoutez enfin l’élément **MediaOverlayLayer** à la liste [**OverlayLayers**](https://msdn.microsoft.com/library/windows/apps/dn764791) de la composition.

[!code-cs[AddOverlay](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddOverlay)]

-   Les superpositions au sein d’une couche sont classées selon l’ordre-z en fonction de leur ordre dans la liste **Overlays** de la couche qui les contient. Les indices supérieurs de la liste sont restitués au-dessus des indices inférieurs. Il en va de même pour les couches de superposition d’une composition. Une couche dont l’indice est supérieur dans la liste **OverlayLayers** de la composition sera restituée au-dessus des couches à indice inférieur.

-   Dans la mesure où les superpositions sont empilées les unes sur les autres au lieu d’être lues séquentiellement, toutes les superpositions commencent par défaut la lecture au début de la composition. Pour qu’une superposition commence la lecture à un autre moment, définissez la propriété [**Delay**](https://msdn.microsoft.com/library/windows/apps/dn764810) sur le décalage souhaité.

## Ajouter des effets à un clip multimédia

Chaque **MediaClip** d’une composition possède une liste d’effets audio et vidéo auxquels plusieurs effets peuvent être ajoutés. Les effets doivent respectivement implémenter [**IAudioEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608044) et [**IVideoEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608047). L’exemple suivant utilise la position actuelle de l’élément multimédia de manière à choisir l’élément **MediaClip** affiché actuellement, puis crée une nouvelle instance de la classe [**VideoStabilizationEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn926762) et l’ajoute à la liste [**VideoEffectDefinitions**](https://msdn.microsoft.com/library/windows/apps/dn652643) du clip multimédia.

[!code-cs[AddVideoEffect](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddVideoEffect)]

## Enregistrer une composition dans un fichier

Les compositions multimédia peuvent être sérialisées dans un fichier pour être modifiées ultérieurement. Sélectionnez un fichier de sortie, puis appelez la méthode [**SaveAsync**](https://msdn.microsoft.com/library/windows/apps/dn640554) de la classe [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646) pour enregistrer la composition.

[!code-cs[SaveComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetSaveComposition)]

## Charger une composition à partir d’un fichier

Les compositions multimédia peuvent être désérialisées à partir d’un fichier pour permettre à l’utilisateur d’afficher et de modifier la composition. Sélectionnez un fichier de composition, puis appelez la méthode [**LoadAsync**](https://msdn.microsoft.com/library/windows/apps/dn652684) de la classe [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646) pour charger la composition.

[!code-cs[OpenComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetOpenComposition)]

-   Si un fichier multimédia de la composition ne se trouve pas dans un emplacement auquel votre application a accès et ne se trouve pas dans la propriété [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) de la classe [**StorageApplicationPermissions**](https://msdn.microsoft.com/library/windows/apps/br207456) pour votre application, une erreur sera générée lors du chargement de la composition.

 

 







<!--HONumber=Aug16_HO3-->


