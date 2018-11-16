---
author: drewbatgit
Description: This article describes how to create a Windows Runtime component that implements the IBasicAudioEffect interface to allow you to create custom effects for audio streams.
title: Effets audio personnalisés
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.assetid: 360faf3f-7e73-4db4-8324-3391f801d827
ms.localizationpriority: medium
ms.openlocfilehash: a9c62dac489be07a08acc4ad17ecb79bcc54f57a
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6974086"
---
# <a name="custom-audio-effects"></a>Effets audio personnalisés

Cet article explique comment créer un composant WindowsRuntime implémentant l’interface [**IBasicAudioEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect) pour créer des effets personnalisés de flux audio. Vous pouvez utiliser les effets personnalisés avec plusieurs API Windows Runtime différentes, notamment [MediaCapture](https://msdn.microsoft.com/library/windows/apps/br241124), qui fournit un accès à la caméra d’un appareil, [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646), qui vous permet de créer des compositions complexes à partir de clips multimédias, et [**AudioGraph**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioGraph), qui vous permet d’assembler rapidement un graphique composé de différents nœuds prémixés et d’entrée/sortie audio.

## <a name="add-a-custom-effect-to-your-app"></a>Ajouter un effet personnalisé à votre application


Un effet audio personnalisé est défini dans une classe qui implémente l’interface [**IBasicAudioEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect). Cette classe ne peut pas être incluse directement dans le projet de votre application. À la place, vous devez utiliser un composant Windows Runtime pour héberger votre classe d’effet audio.

**Ajouter un composant Windows Runtime pour votre effet audio**

1.  Dans Microsoft Visual Studio, quand votre solution est ouverte, accédez au menu **Fichier**, sélectionnez **Ajouter-&gt;Nouveau projet.**
2.  Sélectionnez le type de projet **Composant Windows Runtime (Windows universel)**.
3.  Pour cet exemple, nommez le projet *AudioEffectComponent*. Ce nom sera référencé dans le code ultérieurement.
4.  Cliquez sur **OK**.
5.  Le modèle de projet crée une classe appelée Class1.cs. Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur l’icône de Class1.cs et sélectionnez **Renommer**.
6.  Renommez le fichier *ExampleAudioEffect.cs*. Visual Studio affiche une invite vous demandant si vous voulez mettre à jour toutes les références sous le nouveau nom. Cliquez sur **Oui**.
7.  Ouvrez **ExampleAudioEffect.cs** et mettez à jour la définition de classe pour implémenter l’interface [**IBasicAudioEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect).


[!code-cs[ImplementIBasicAudioEffect](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetImplementIBasicAudioEffect)]

Vous devez inclure les espaces de noms suivants dans votre fichier de classe effet afin d’accéder à tous les types utilisés dans les exemples de cet article.

[!code-cs[EffectUsing](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetEffectUsing)]

## <a name="implement-the-ibasicaudioeffect-interface"></a>Implémentation de l’interface IBasicAudioEffect

Votre effet audio doit implémenter toutes les méthodes et propriétés de l’interface [**IBasicAudioEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect). Cette section vous explique simplement comment implémenter cette interface pour créer un effet d’écho de base.

### <a name="supportedencodingproperties-property"></a>Propriété SupportedEncodingProperties

Le système vérifie la propriété [**SupportedEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect.SupportedEncodingProperties) pour déterminer quelles propriétés d’encodage sont prises en charge par votre effet. Notez que si le consommateur de votre effet ne peut pas encoder un contenu audio en utilisant les propriétés que vous spécifiez, le système appelle [**Close**](https://msdn.microsoft.com/library/windows/apps/dn764782) sur votre effet et supprime votre effet du pipeline audio. Dans cet exemple, les objets [**AudioEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.AudioEncodingProperties) sont créés et ajoutés à la liste retournée pour prendre en charge le codage mono de type float sur 32bits à 44,1kHz et 48kHz.

[!code-cs[SupportedEncodingProperties](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetSupportedEncodingProperties)]

### <a name="setencodingproperties-method"></a>Méthode SetEncodingProperties

Le système appelle [**SetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn919884) sur votre effet pour vous indiquer les propriétés de codage du flux audio sur lequel l’effet est appliqué. Afin d’implémenter un effet d’écho, cet exemple utilise une mémoire tampon pour stocker une seconde de données audio. Cette méthode permet d’initialiser la taille de la mémoire tampon sur le nombre d’échantillons produits pendant une seconde de données audio, en fonction du taux d’échantillonnage dans lequel l’élément audio est encodé. L’effet de retard utilise également un compteur entier pour garder une trace de la position actuelle dans le tampon de retard. Dans la mesure où la méthode **SetEncodingProperties** est appelée chaque fois que l’effet est ajouté au pipeline audio, il s’agit du moment idéal pour remettre la valeur à 0. Vous pouvez également capturer l’objet **AudioEncodingProperties** transmis à cette méthode pour l’utiliser à un autre point de votre effet.

[!code-cs[DeclareEchoBuffer](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetDeclareEchoBuffer)]
[!code-cs[SetEncodingProperties](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetSetEncodingProperties)]


### <a name="setproperties-method"></a>Méthode SetProperties

La méthode [**SetProperties**](https://msdn.microsoft.com/library/windows/apps/br240986) permet à l’application qui utilise votre effet d’ajuster les paramètres d’effet. Les propriétés sont transmises sous forme de carte [**IPropertySet**](https://msdn.microsoft.com/library/windows/apps/br226054) des noms et des valeurs de propriété.

[!code-cs[SetProperties](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetSetProperties)]

Cet exemple simple permet de mixer l’échantillon audio actuel avec une valeur issue du tampon de retard, en fonction de la valeur de la propriété **Mix**. Une propriété est déclarée et TryGetValue est utilisé pour obtenir la valeur définie par l’application d’appel. Si aucune valeur n’a été définie, la valeur par défaut .5 est utilisée. Notez que cette propriété est en lecture seule. La valeur de propriété doit être définie à l’aide de **SetProperties**.

[!code-cs[MixProperty](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetMixProperty)]

### <a name="processframe-method"></a>Méthode ProcessFrame

C’est dans la méthode [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764784) que l’effet modifie les données audio du flux. La méthode est appelée une fois par trame et un objet [**ProcessAudioFrameContext**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.ProcessAudioFrameContext) lui est transmis. Cet objet contient un objet [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.AudioFrame) en entrée qui contient la trame entrante à traiter et un objet **AudioFrame** en sortie sur lequel vous écrivez des données audio qui seront transmises dans le reste du pipeline audio. Une trame audio est une mémoire tampon d’échantillons audio qui représente une courte tranche de données audio.

L’accès à la mémoire tampon de données d’une **AudioFrame** nécessite une interopérabilité COM; autrement dit, vous devez inclure l’espace de noms **System.Runtime.InteropServices** dans le fichier de classe de votre effet, puis ajouter le code suivant à l’intérieur de l’espace de noms de votre effet pour importer l’interface permettant d’accéder à la mémoire tampon audio.

[!code-cs[ComImport](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetComImport)]

> [!NOTE]
> Dans la mesure où cette technique accède à une mémoire tampon d’image native non gérée, vous devez configurer votre projet pour autoriser du code unsafe.
> 1.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet AudioEffectComponent et sélectionnez **Propriétés**.
> 2.  Sélectionnez l’onglet **Générer**.
> 3.  Cochez la case **Autoriser du code unsafe**.

 

Vous pouvez maintenant ajouter l’implémentation de la méthode **ProcessFrame** à votre effet. Tout d’abord, cette méthode obtient un objet [**AudioBuffer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.AudioBuffer) à partir des trames audio en entrée et en sortie. Notez que la trame en sortie est ouverte pour l’écriture, et que la trame en entrée l’est pour la lecture. Ensuite, un [**IMemoryBufferReference**](https://msdn.microsoft.com/library/windows/apps/dn921671) est obtenu pour chaque tampon en appelant [**CreateReference**](https://msdn.microsoft.com/library/windows/apps/dn949046). Ensuite, le tampon de données réel est obtenu en transtypant les objets **IMemoryBufferReference** en tant qu’interface d’interopérabilité COM définie ci-dessus, **IMemoryByteAccess**, puis en appelant **GetBuffer**.

Maintenant que les tampons de données ont été obtenus, vous pouvez lire à partir du tampon en entrée et écrire sur le tampon en sortie. Pour chaque échantillon contenu dans le tampon en entrée, la valeur est obtenue et multipliée par 1 - **Mix** pour définir la valeur sèche de l’effet. Un échantillon est ensuite récupéré à partir de la position actuelle dans la mémoire tampon d’écho et multiplié par **Mix** pour définir la valeur humide de l’effet. L’échantillon de sortie est défini comme étant égal à la somme des valeurs sèche et humide. Pour finir, chaque échantillon d’entrée est stocké dans la mémoire tampon d’écho; l’index d’échantillons actuel est alors incrémenté.

[!code-cs[ProcessFrame](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetProcessFrame)]



### <a name="close-method"></a>Méthode Close

Le système appelle la méthode [**Close**](https://msdn.microsoft.com/library/windows/apps/dn764782) [**Close**](https://msdn.microsoft.com/library/windows/apps/dn764782) sur votre classe lorsque l’effet doit être arrêté. Vous devez utiliser cette méthode pour supprimer les ressources que vous avez créées. L’argument de la méthode est un [**MediaEffectClosedReason**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.MediaEffectClosedReason) qui vous permet de savoir si l’effet a été fermé normalement, si une erreur s’est produite, ou si l’effet ne prend pas en charge le format de codage nécessaire.

[!code-cs[Close](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetClose)]

### <a name="discardqueuedframes-method"></a>Méthode DiscardQueuedFrames

La méthode [**DiscardQueuedFrames**](https://msdn.microsoft.com/library/windows/apps/dn764790) est appelée lorsque l’effet doit être réinitialisé. Dans ce cas, le scénario courant est que l’effet stocke les trames précédemment traitées pour les utiliser dans le traitement de la trame active. Quand cette méthode est appelée, vous devez supprimer l’ensemble des trames précédentes que vous avez enregistrées. Cette méthode peut être utilisée pour réinitialiser un état associé aux trames précédentes, pas seulement les trames audio cumulées.

[!code-cs[DiscardQueuedFrames](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetDiscardQueuedFrames)]

### <a name="timeindependent-property"></a>Propriété TimeIndependent

La propriété TimeIndependent [**TimeIndependent**](https://msdn.microsoft.com/library/windows/apps/dn764803) permet au système de savoir si votre effet n’exige pas un minutage uniforme. Lorsqu’elle est définie sur true, le système peut utiliser les optimisations qui améliorent la performance de l’effet.

[!code-cs[TimeIndependent](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetTimeIndependent)]

### <a name="useinputframeforoutput-property"></a>Propriété UseInputFrameForOutput

Définissez la propriété [**UseInputFrameForOutput**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect.UseInputFrameForOutput) sur **true** pour indiquer au système que votre effet écrira sa sortie dans la mémoire tampon audio de l’objet [**InputFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.ProcessAudioFrameContext.InputFrame) de la méthode [**ProcessAudioFrameContext**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.ProcessAudioFrameContext) transmise à [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764784) au lieu d’écrire sur l’objet [**OutputFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.ProcessAudioFrameContext.OutputFrame). 

[!code-cs[UseInputFrameForOutput](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetUseInputFrameForOutput)]


## <a name="adding-your-custom-effect-to-your-app"></a>Ajout d’un effet personnalisé à votre application


Pour utiliser votre effet audio dans votre application, vous devez ajouter une référence au projet d’effet à votre application.

1.  Dans l’Explorateur de solutions, sous votre projet d’application, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence**.
2.  Développez l’onglet **Projets**, sélectionnez **Solution**, puis cochez la case du nom de votre projet d’effet. Dans cet exemple, le nom est *AudioEffectComponent*.
3.  Cliquez sur **OK**

Si votre classe d’effet audio est déclarée dans un autre espace de noms, veillez à inclure cet espace de noms dans votre fichier de code.

[!code-cs[UsingAudioEffectComponent](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetUsingAudioEffectComponent)]

### <a name="add-your-custom-effect-to-an-audiograph-node"></a>Ajout de l’effet personnalisé à un nœud AudioGraph
Pour obtenir des informations générales sur l’utilisation de graphiques audio, voir [Graphiques audio](audio-graphs.md). L’extrait de code suivant montre comment ajouter à un nœud de graphiques audio l’exemple d’effet d’écho illustré dans cet article. Un [**PropertySet**](https://msdn.microsoft.com/library/windows/apps/Windows.Foundation.Collections.PropertySet) est tout d’abord créé et une valeur est affectée à la propriété **Mix**, définie par l’effet. Le constructeur [**AudioEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.AudioEffectDefinition) est ensuite appelé, ce qui permet de transmettre le nom de classe complet du type d’effet personnalisé et du jeu de propriétés. Enfin, la définition de l’effet est ajoutée à la propriété [**EffectDefinitions**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioFileInputNode.EffectDefinitions) d’un objet [**FileInputNode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.CreateAudioFileInputNodeResult.FileInputNode) existant; le contenu audio émis est alors traité par l’effet personnalisé. 

[!code-cs[AddCustomEffect](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddCustomEffect)]

Une fois ajouté à un nœud, l’effet personnalisé peut être désactivé en appelant la méthode [**DisableEffectsByDefinition**](https://msdn.microsoft.com/library/windows/apps/dn958480) et en transmettant l’objet **AudioEffectDefinition**. Pour plus d’informations sur l’utilisation de graphiques audio dans votre application, voir [Graphiques audio](audio-graphs.md).

### <a name="add-your-custom-effect-to-a-clip-in-a-mediacomposition"></a>Ajouter un effet personnalisé à un clip dans une composition multimédia

L’extrait de code suivant illustre l’ajout de l’effet audio personnalisé à un clip vidéo et à une piste audio en arrière-plan dans une composition multimédia. Pour obtenir des instructions générales sur la création de compositions multimédias à partir de clips vidéo et sur l’ajout de pistes audio en arrière-plan, voir [Compositions multimédias et modification](media-compositions-and-editing.md).

[!code-cs[AddCustomAudioEffect](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddCustomAudioEffect)]



## <a name="related-topics"></a>Rubriquesconnexes
* [Accès à l’aperçu simple de l’appareil photo](simple-camera-preview-access.md)
* [Compositions multimédias et modification](media-compositions-and-editing.md)
* [Documentation Win2D](http://go.microsoft.com/fwlink/p/?LinkId=519078)
* [Lecture de contenu multimédia](media-playback.md)

 



