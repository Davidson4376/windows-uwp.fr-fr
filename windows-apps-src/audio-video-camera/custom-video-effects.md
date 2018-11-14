---
author: drewbatgit
Description: This article describes how to create a Windows Runtime component that implements the IBasicVideoEffect interface to allow you to create custom effects for video streams.
MS-HAID: dev\_audio\_vid\_camera.custom\_video\_effects
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Effets vidéo personnalisés
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.assetid: 40a6bd32-a756-400f-ba34-2c5f507262c0
ms.localizationpriority: medium
ms.openlocfilehash: 08d861355a235c9217f51ce6f925224a27a562ef
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/08/2018
ms.locfileid: "6147223"
---
# <a name="custom-video-effects"></a>Effets vidéo personnalisés




Cet article explique comment créer un composant WindowsRuntime implémentant l’interface [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788) pour créer des effets personnalisés de flux vidéo. Vous pouvez utiliser les effets personnalisés avec plusieurs API Windows Runtime différentes, notamment [MediaCapture](https://msdn.microsoft.com/library/windows/apps/br241124), qui fournit un accès à la caméra d’un appareil, et [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646), qui vous permet de créer des compositions complexes à partir de clips multimédias.

## <a name="add-a-custom-effect-to-your-app"></a>Ajouter un effet personnalisé à votre application


Un effet vidéo personnalisé est défini dans une classe qui implémente l’interface [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788). Cette classe ne peut pas être incluse directement dans le projet de votre application. À la place, vous devez utiliser un composant Windows Runtime pour héberger votre classe d’effet vidéo.

**Ajouter un composant Windows Runtime pour votre effet vidéo**

1.  Dans Microsoft Visual Studio, quand votre solution est ouverte, accédez au menu **Fichier**, sélectionnez **Ajouter-&gt;Nouveau projet.**
2.  Sélectionnez le type de projet **Composant Windows Runtime (Windows universel)**.
3.  Pour cet exemple, nommez le projet *VideoEffectComponent*. Ce nom sera référencé dans le code ultérieurement.
4.  Cliquez sur **OK**.
5.  Le modèle de projet crée une classe appelée Class1.cs. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur l’icône de Class1.cs et sélectionnez **Renommer**.
6.  Renommez le fichier *ExampleVideoEffect.cs*. Visual Studio affiche une invite vous demandant si vous voulez mettre à jour toutes les références sous le nouveau nom. Cliquez sur **Oui**.
7.  Ouvrez **ExampleVideoEffect.cs** et mettez à jour la définition de classe pour implémenter l’interface [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788).

[!code-cs[ImplementIBasicVideoEffect](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetImplementIBasicVideoEffect)]


Vous devez inclure les espaces de noms suivants dans votre fichier de classe effet afin d’accéder à tous les types utilisés dans les exemples de cet article.

[!code-cs[EffectUsing](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetEffectUsing)]


## <a name="implement-the-ibasicvideoeffect-interface-using-software-processing"></a>Implémenter l’interface IBasicVideoEffect à l’aide du traitement logiciel


Votre effet vidéo doit implémenter toutes les méthodes et propriétés de l’interface [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788). Cette section vous explique la procédure d’implémentation simple de cette interface à l’aide d’un traitement logiciel.

### <a name="close-method"></a>Méthode Close

Le système appelle la méthode [**Fermer**](https://msdn.microsoft.com/library/windows/apps/dn764789) sur votre classe lorsque l’effet doit être arrêté. Vous devez utiliser cette méthode pour supprimer les ressources que vous avez créées. L’argument de la méthode est un [**MediaEffectClosedReason**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.MediaEffectClosedReason) qui vous permet de savoir si l’effet a été fermé normalement, si une erreur s’est produite, ou si l’effet ne prend pas en charge le format de codage nécessaire.

[!code-cs[Close](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetClose)]


### <a name="discardqueuedframes-method"></a>Méthode DiscardQueuedFrames

La méthode [**DiscardQueuedFrames**](https://msdn.microsoft.com/library/windows/apps/dn764790) est appelée lorsque l’effet doit être réinitialisé. Dans ce cas, le scénario courant est que l’effet stocke les trames précédemment traitées pour les utiliser dans le traitement de la trame active. Quand cette méthode est appelée, vous devez supprimer l’ensemble des trames précédentes que vous avez enregistrées. Cette méthode peut être utilisée pour réinitialiser un état associé aux trames précédentes, pas seulement les trames vidéo cumulées.


[!code-cs[DiscardQueuedFrames](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetDiscardQueuedFrames)]



### <a name="isreadonly-property"></a>Propriété IsReadOnly

La propriété [**IsReadOnly**](https://msdn.microsoft.com/library/windows/apps/dn764792) permet d’indiquer au système si votre effet va écrire dans la sortie de l’effet. Si votre application ne modifie pas les trames vidéo (par exemple, un effet qui effectue seulement une analyse des trames vidéo), vous devez définir cette propriété sur true. Ainsi, le système copie efficacement à votre place l’entrée de trame dans la sortie de trame.

> [!TIP]
> Quand la propriété [**IsReadOnly**](https://msdn.microsoft.com/library/windows/apps/dn764792) est définie sur true, le système copie la trame d’entrée sur la trame de sortie avant l’appel de [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764794). Le fait de définir la propriété **IsReadOnly** sur true ne vous empêche pas d’écrire sur les trames de sortie de l’effet dans **ProcessFrame**.


[!code-cs[IsReadOnly](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetIsReadOnly)]

### <a name="setencodingproperties-method"></a>Méthode SetEncodingProperties

Le système appelle [**SetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn919884) sur votre effet pour vous indiquer les propriétés de codage du flux vidéo sur lequel l’effet est appliqué. Cette méthode fournit également une référence à l’appareil Direct3D utilisé pour le rendu matériel. L’utilisation de cet appareil est expliquée dans l’exemple de traitement matériel plus loin dans cet article.

[!code-cs[SetEncodingProperties](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSetEncodingProperties)]


### <a name="supportedencodingproperties-property"></a>Propriété SupportedEncodingProperties

Le système vérifie la propriété [**SupportedEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn764799) pour déterminer quelles propriétés d’encodage sont prises en charge par votre effet. Notez que si le consommateur de votre effet ne peut pas encoder une vidéo en utilisant les propriétés que vous spécifiez, il appelle [**Close**](https://msdn.microsoft.com/library/windows/apps/dn764789) sur votre effet et supprime votre effet du pipeline vidéo.


[!code-cs[SupportedEncodingProperties](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSupportedEncodingProperties)]


> [!NOTE] 
> Si vous renvoyez une liste vide d’objets [**VideoEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701217) à partir de **SupportedEncodingProperties**, le système utilise par défaut le codage ARGB32.

 

### <a name="supportedmemorytypes-property"></a>Propriété SupportedMemoryTypes

Le système vérifie que la propriété [**SupportedMemoryTypes**](https://msdn.microsoft.com/library/windows/apps/dn764801) pour déterminer si l’effet va accéder aux trames vidéo dans la mémoire du logiciel ou dans la mémoire du matériel (GPU). Si vous renvoyez [**MediaMemoryTypes.Cpu**](https://msdn.microsoft.com/library/windows/apps/dn764822), l’effet sera transmis aux trames d’entrée et de sortie qui contiennent des données d’image dans les objets [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358). Si vous renvoyez **MediaMemoryTypes.Gpu**, l’effet sera transmis aux trames en entrée et en sortie qui contiennent des données d’image dans les objets [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn965505).

[!code-cs[SupportedMemoryTypes](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSupportedMemoryTypes)]


> [!NOTE]
> Si vous spécifiez [**MediaMemoryTypes.GpuAndCpu**](https://msdn.microsoft.com/library/windows/apps/dn764822), le système utilise la mémoire du GPU ou du système, selon laquelle est la plus performante pour le pipeline. Quand vous utilisez cette valeur, vous devez vérifier la méthode [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764794) pour voir si le [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) ou le [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn965505) transmis dans la méthode contient des données, puis traiter la trame en fonction.

 

### <a name="timeindependent-property"></a>Propriété TimeIndependent

La propriété [**TimeIndependent**](https://msdn.microsoft.com/library/windows/apps/dn764803) permet au système de savoir si votre effet n’exige pas un minutage uniforme. Lorsqu’elle est définie sur true, le système peut utiliser les optimisations qui améliorent la performance de l’effet.

[!code-cs[TimeIndependent](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetTimeIndependent)]

### <a name="setproperties-method"></a>Méthode SetProperties

La méthode [**SetProperties**](https://msdn.microsoft.com/library/windows/apps/br240986) permet à l’application qui utilise votre effet d’ajuster les paramètres d’effet. Les propriétés sont transmises sous forme de carte [**IPropertySet**](https://msdn.microsoft.com/library/windows/apps/br226054) des noms et des valeurs de propriété.


[!code-cs[SetProperties](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetSetProperties)]


Cet exemple simple estompe les pixels dans chaque trame vidéo en fonction d’une valeur spécifiée. Une propriété est déclarée et TryGetValue est utilisé pour obtenir la valeur définie par l’application d’appel. Si aucune valeur n’a été définie, la valeur par défaut .5 est utilisée.

[!code-cs[FadeValue](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetFadeValue)]


### <a name="processframe-method"></a>Méthode ProcessFrame

C’est dans la méthode [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764794) que l’effet modifie les données d’image de la vidéo. La méthode est appelée une fois par trame et un objet [**ProcessVideoFrameContext**](https://msdn.microsoft.com/library/windows/apps/dn764826) lui est transmis. Cet objet contient un objet [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917) en entrée qui contient la trame entrante à traiter et un objet **VideoFrame** en sortie sur lequel vous écrivez des données d’image qui seront transmises dans le reste du pipeline vidéo. Chacun de ces objets **VideoFrame** comporte une propriété [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn930926) et une propriété [**Direct3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn930920), mais lesquels d’entre eux peuvent être utilisés est déterminé par la valeur renvoyée à partir de la propriété [**SupportedMemoryTypes**](https://msdn.microsoft.com/library/windows/apps/dn764801).

Cet exemple montre une implémentation simple de la méthode **ProcessFrame** à l’aide du traitement logiciel. Pour plus d’informations sur l’utilisation des objets [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358), voir [Acquisition d’images](imaging.md). Un exemple d’implémentation de **ProcessFrame** à l’aide du traitement logiciel est illustré plus loin dans cet article.

L’accès à la mémoire tampon de données d’un objet **SoftwareBitmap** nécessite l’interopérabilité COM. Vous devez donc inclure l’espace de noms **System.Runtime.InteropServices** dans votre fichier de classe effet.

[!code-cs[COMUsing](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetCOMUsing)]


Ajoutez le code suivant à l’intérieur de l’espace de noms de l’effet pour importer l’interface et accéder à la mémoire tampon d’image.

[!code-cs[COMImport](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetCOMImport)]


> [!NOTE]
> Dans la mesure où cette technique accède à une mémoire tampon d’image native non gérée, vous devez configurer votre projet pour autoriser du code unsafe.
> 1.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet VideoEffectComponent et sélectionnez **Propriétés**.
> 2.  Sélectionnez l’onglet **Générer**.
> 3.  Cochez la case **Autoriser du code unsafe**.

 

Vous pouvez maintenant ajouter l’implémentation de la méthode **ProcessFrame**. Tout d’abord, cette méthode obtient un objet [**BitmapBuffer**](https://msdn.microsoft.com/library/windows/apps/dn887325) à partir des bitmaps logiciels en entrée et en sortie. Notez que la trame en sortie est ouverte pour l’écriture, et que la trame en entrée l’est pour la lecture. Ensuite, un [**IMemoryBufferReference**](https://msdn.microsoft.com/library/windows/apps/dn921671) est obtenu pour chaque tampon en appelant [**CreateReference**](https://msdn.microsoft.com/library/windows/apps/dn949046). Ensuite, le tampon de données réel est obtenu en transtypant les objets **IMemoryBufferReference** en tant qu’interface d’interopérabilité COM définie ci-dessus, **IMemoryByteAccess**, puis en appelant **GetBuffer**.

Maintenant que les tampons de données ont été obtenus, vous pouvez lire à partir du tampon en entrée et écrire sur le tampon en sortie. La disposition du tampon est obtenue en appelant [**GetPlaneDescription**](https://msdn.microsoft.com/library/windows/apps/dn887330), qui fournit des informations sur la largeur, la longueur et le décalage initial du tampon. Les bits par pixel sont déterminés par les propriétés de codage définies au préalable avec la méthode [**SetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn919884). Les informations sur le format du tampon sont utilisées pour trouver l’index dans le tampon pour chaque pixel. La valeur du pixel du tampon source est copiée dans le tampon cible. Les valeurs de couleur sont multipliées par la propriété FadeValue définie pour cet effet afin de les diminuer du montant spécifié.

[!code-cs[ProcessFrameSoftwareBitmap](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs#SnippetProcessFrameSoftwareBitmap)]


## <a name="implement-the-ibasicvideoeffect-interface-using-hardware-processing"></a>Implémenter l’interface IBasicVideoEffect à l’aide du traitement matériel


La création d’un effet vidéo personnalisé à l’aide du traitement matériel (GPU) est presque identique à l’utilisation du traitement logiciel tel que décrit ci-dessus. Cette section vous explique les différences dans un effet qui utilise le traitement matériel. Cet exemple utilise l’API Windows Runtime Win2D. Pour plus d’informations sur l’utilisation de Win2D, voir la [documentation Win2D](http://go.microsoft.com/fwlink/?LinkId=519078).

Utilisez les étapes suivantes pour ajouter le package NuGet Win2D au projet que vous avez créé comme décrit dans la section **Ajouter un effet personnalisé à votre application** au début de cet article.

**Pour ajouter le package NuGet Win2D à votre projet d’effet**

1.  Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet **VideoEffectComponent** et sélectionnez **Gérer les packages NuGet**.
2.  En haut de la fenêtre, sélectionnez l’onglet **Explorer**.
3.  Dans la zone de recherche, entrez **Win2D**.
4.  Sélectionnez **Win2D.uwp**, puis **Installer** dans le volet droit.
5.  La boîte de dialogue **Examiner les modifications** vous indique le package à installer. Cliquez sur **OK**.
6.  Acceptez la licence de package.

Outre les espaces de noms inclus dans l’installation de base du projet, vous devez inclure les espaces de noms suivants fournis par Win2D.

[!code-cs[UsingWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetUsingWin2D)]


Dans la mesure où cet effet utilisera la mémoire GPU pour les opérations sur les données d’image, vous devez renvoyer [**MediaMemoryTypes.Gpu**](https://msdn.microsoft.com/library/windows/apps/dn764822) à partir de la propriété [**SupportedMemoryTypes**](https://msdn.microsoft.com/library/windows/apps/dn764801).

[!code-cs[SupportedMemoryTypesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSupportedMemoryTypesWin2D)]


Définissez les propriétés de codage que votre effet prend en charge avec la propriété [**SupportedEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn764799). Quand vous utilisez Win2D, vous devez utiliser le codage ARGB32.

[!code-cs[SupportedEncodingPropertiesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSupportedEncodingPropertiesWin2D)]


Utilisez la méthode [**SetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn919884) pour créer un nouvel objet **CanvasDevice** Win2D à partir du [**IDirect3DDevice**](https://msdn.microsoft.com/library/windows/apps/dn895092) transmis dans la méthode.

[!code-cs[SetEncodingPropertiesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSetEncodingPropertiesWin2D)]


L’implémentation de [**SetProperties**](https://msdn.microsoft.com/library/windows/apps/br240986) est identique à l’exemple de traitement logiciel précédent. Cet exemple utilise une propriété **BlurAmount** pour configurer un effet de flou Win2D.

[!code-cs[SetPropertiesWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetSetPropertiesWin2D)]

[!code-cs[BlurAmountWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetBlurAmountWin2D)]


La dernière étape consiste à implémenter la méthode [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764794) qui traite réellement les données d’image.

À l’aide d’API Win2D, un **CanvasBitmap** est créé à partir de la propriété [**Direct3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn930920) de la trame en entrée. Un **CanvasRenderTarget** est créé à partir du **Direct3DSurface** de la trame en sortie et un **CanvasDrawingSession** est créé à partir de cette cible de rendu. Un nouveau **GaussianBlurEffect** Win2D est initialisé à l’aide de la propriété **BlurAmount** que l’effet expose via [**SetProperties**](https://msdn.microsoft.com/library/windows/apps/br240986). Enfin, la méthode **CanvasDrawingSession.DrawImage** est appelée pour dessiner le bitmap en entrée sur la cible de rendu à l’aide de l’effet de flou.

[!code-cs[ProcessFrameWin2D](./code/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs#SnippetProcessFrameWin2D)]


## <a name="adding-your-custom-effect-to-your-app"></a>Ajout d’un effet personnalisé à votre application


Pour utiliser votre effet vidéo dans votre application, vous devez ajouter une référence au projet d’effet à votre application.

1.  Dans l’Explorateur de solutions, sous votre projet d’application, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence**.
2.  Développez l’onglet **Projets**, sélectionnez **Solution**, puis cochez la case du nom de votre projet d’effet. Dans cet exemple, le nom est *VideoEffectComponent*.
3.  Cliquez sur **OK**.

### <a name="add-your-custom-effect-to-a-camera-video-stream"></a>Ajouter un effet personnalisé à un flux vidéo de caméra

Vous pouvez configurer un flux d’aperçu simple à partir de la caméra en suivant les étapes décrites dans l’article [Accès à l’aperçu simple de l’appareil photo](simple-camera-preview-access.md). En suivant ces étapes, vous obtiendrez un objet [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) initialisé qui est utilisé pour accéder au flux vidéo de la caméra.

Pour ajouter votre effet vidéo personnalisé à un flux de caméra, créez d’abord un objet [**VideoEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608055), en transmettant l’espace de noms et le nom de classe de votre effet. Appelez ensuite la méthode [**AddVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn878035) de l’objet **MediaCapture** pour ajouter l’effet au flux spécifié. Cet exemple utilise la valeur [**MediaStreamType.VideoPreview**](https://msdn.microsoft.com/library/windows/apps/br226640) pour indiquer que l’effet doit être ajouté au flux d’aperçu. Si votre application prend en charge la capture vidéo, vous pouvez également utiliser **MediaStreamType.VideoRecord** pour ajouter l’effet au flux de capture. **AddVideoEffect** renvoie un objet [**IMediaExtension**](https://msdn.microsoft.com/library/windows/apps/br240985) représentant votre effet personnalisé. Vous pouvez utiliser la méthode SetProperties pour définir la configuration de votre effet.

Une fois l’effet ajouté, [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613) est appelé pour démarrer le flux d’aperçu.

[!code-cs[AddVideoEffectAsync](./code/VideoEffect_Win10/cs/VideoEffect_Win10/MainPage.xaml.cs#SnippetAddVideoEffectAsync)]



### <a name="add-your-custom-effect-to-a-clip-in-a-mediacomposition"></a>Ajouter un effet personnalisé à un clip dans une composition multimédia

Pour obtenir des instructions générales sur la création des compositions multimédias à partir de clips vidéo, voir [Compositions multimédias et modification](media-compositions-and-editing.md). L’extrait de code suivant illustre la création d’une composition multimédia simple qui utilise un effet vidéo personnalisé. Un objet [**MediaClip**](https://msdn.microsoft.com/library/windows/apps/dn652596) est créé en appelant [**CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652607), en transmettant un fichier vidéo sélectionné par l’utilisateur avec un [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) et le clip est ajouté à un nouveau [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646). Ensuite, un objet [**VideoEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608055) est créé, en transmettant l’espace de noms et le nom de classe de votre effet au constructeur. Enfin, la définition de l’effet est ajoutée à la collection [**VideoEffectDefinitions**](https://msdn.microsoft.com/library/windows/apps/dn652643) de l’objet **MediaClip**.


[!code-cs[AddEffectToComposition](./code/VideoEffect_Win10/cs/VideoEffect_Win10/MainPage.xaml.cs#SnippetAddEffectToComposition)]


## <a name="related-topics"></a>Rubriquesassociées
* [Accès à l’aperçu simple de l’appareil photo](simple-camera-preview-access.md)
* [Compositions multimédias et modification](media-compositions-and-editing.md)
* [Documentation Win2D](http://go.microsoft.com/fwlink/p/?LinkId=519078)
* [Lecture de contenu multimédia](media-playback.md)