---
title: Mémoires tampons de gabarits
description: Une mémoire tampon de gabarit est utilisée pour masquer les pixels d'une image afin de produire des effets spéciaux.
ms.assetid: 544B3B9E-31E3-41DA-8081-CC3477447E94
keywords:
- Mémoires tampons de gabarits
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 285e4a70062c57c957530aa1e548c22c4cf7711e
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8458743"
---
# <a name="stencil-buffers"></a>Mémoires tampons de gabarits


Un *tampon stencil buffer* est utilisé pour masquer les pixels d’une image afin de produire des effets spéciaux. Le masque permet de contrôler si le pixel est ou non dessiné. Ces effets spéciaux incluent notamment la composition, le transfert, la dissolution, le fondu et le balayage, le contour et la silhouette, ainsi que le gabarit recto verso. Certains des effets les plus courants sont présentés ci-dessous.

La mémoire tampon de gabarit permet d'activer ou de désactiver le dessin sur la surface de rendu cible sur une base pixel par pixel. À son niveau le plus fondamental, il permet aux applications de masquer des sections de l’image rendue afin de ne pas les afficher. Les applications utilisent souvent des mémoires tampons de gabarits pour des effets spéciaux comme les fondus, le transfert et la mise en plan.

Les informations sur les mémoire tampon de gabarit sont intégrées dans les données du tampon z.

## <a name="span-idhowthestencilbufferworksspanspan-idhowthestencilbufferworksspanspan-idhowthestencilbufferworksspanhow-the-stencil-buffer-works"></a><span id="How_the_Stencil_Buffer_Works"></span><span id="how_the_stencil_buffer_works"></span><span id="HOW_THE_STENCIL_BUFFER_WORKS"></span>Fonctionnement de la mémoire tampon de gabarit


Direct3D procède à un test sur les contenus de la mémoire tampon de gabarit sur une base pixel par pixel. Pour chaque pixel de la surface cible, il procède à un test à l’aide de la valeur correspondante dans la mémoire tampon de gabarit, d'une valeur de référence du gabarit et d'une valeur de masque du gabarit. Si le test réussit, Direct3D exécute une action. Le test est réalisé en suivant les étapes suivantes:

1.  Exécutez une opération AND au niveau du bit de la valeur de référence du gabarit avec le masque de gabarit.
2.  Exécutez une opération AND au niveau du bit de la valeur de mémoire tampon de gabarit pour le pixel actuel à l'aide du masque de gabarit.
3.  Comparez le résultat de l’étape1 au résultat de l’étape2 à l’aide de la fonction de comparaison.

Les étapes ci-dessus sont présentées dans la ligne de code suivante:

```
(StencilRef & StencilMask) CompFunc (StencilBufferValue & StencilMask)
```

-   StencilRef représente la valeur de référence du gabarit.
-   StencilMask représente la valeur du masque de gabarit.
-   CompFunc représente la fonction de comparaison.
-   StencilBufferValue représente les contenus de la mémoire tampon de gabarit du pixel actuel.
-   Le symbole esperluette (&) représente l’opération AND au niveau du bit.

Le pixel actuel est écrit sur la surface cible si le test de gabarit réussit. Dans le cas contraire, il est ignoré. Le comportement de comparaison par défaut consiste à écrire le pixel, quel que soit la façon dont chaque opération au niveau du bit se révèle. Vous pouvez modifier ce comportement en modifiant la valeur d’un type énuméré pour identifier la fonction de comparaison souhaitée.

Votre application peut personnaliser le fonctionnement de la mémoire tampon de gabarit. Il peut définir la fonction de comparaison, le masque de gabarit et la valeur de référence de gabarit. Il peut également contrôler l’action effectuée par Direct3D lorsque le test du gabarit réussit ou échoue.

## <a name="span-idcompositingspanspan-idcompositingspanspan-idcompositingspancompositing"></a><span id="Compositing"></span><span id="compositing"></span><span id="COMPOSITING"></span>Composition


Votre application peut utiliser la mémoire tampon de gabarit pour composer des images 2D ou 3D dans une scène 3D. Un masque de mémoire tampon de gabarit est utilisé pour masquer une zone de la surface cible de rendu. Les informations 2D stockées, comme le texte ou les bitmaps, peuvent ensuite être insérées dans la zone masquée. Votre application peut également rendre des primitives 3D supplémentaires sur la région masquée du gabarit de la surface de cible de rendu. Il peut même rendre une scène entière.

Les jeux composent parfois plusieurs scènes 3D en même temps. Par exemple, les jeux de voitures incluent généralement une fonction rétroviseur. Le rétroviseur contient la vue de la scène 3D derrière le pilote. Il s'agit essentiellement d'une deuxième scène 3D composée à partir de la vue avant du pilote.

## <a name="span-iddecalingspanspan-iddecalingspanspan-iddecalingspandecaling"></a><span id="Decaling"></span><span id="decaling"></span><span id="DECALING"></span>Transfert


Les applications Direct3D utilisent le transfert pour déterminer les pixels qui sont tracés à la surface cible de rendu à partir d'une image de primitive donnée. Les applications appliquent des transferts aux images des primitives afin de rendre correctement les polygones coplanaires.

Par exemple, lorsque vous appliquez des traces de pneu et des lignes jaunes sur une piste, les inscriptions doivent apparaître directement sur la route. Toutefois, les valeurs z des marques et de la route sont identiques. Par conséquent, le tampon de profondeur peut ne pas produire une séparation nette entre les deux. Certains pixels de la primitive arrière peuvent être rendus au-dessus de la primitive avant, et inversement. L’image obtenue semble trembloter$$$ d’une image à une autre. Cet effet est appelé *z-fighting* ou *flimmering*.

Pour résoudre ce problème, utilisez un gabarit pour masquer la section de la primitive arrière où apparaît le transfert. Désactivez la mise en mémoire tampon z et rendez l'image de la primitive avant dans la zone masquée de la surface de rendu cible.

Bien qu'il soit possible d'utiliser plusieurs fusions de textures pour résoudre ce problème, cette action limite le nombre d’effets spéciaux que votre application peut produire. L’utilisation de la mémoire tampon de gabarit pour appliquer les transferts permet de s'épargner les étapes de fusion de textures pour les autres effets.

## <a name="span-iddissolvesfadesandswipesspanspan-iddissolvesfadesandswipesspanspan-iddissolvesfadesandswipesspandissolves-fades-and-swipes"></a><span id="Dissolves__fades__and_swipes"></span><span id="dissolves__fades__and_swipes"></span><span id="DISSOLVES__FADES__AND_SWIPES"></span>Dissolutions, fondus et balayages


De plus en plus applications utilisent des effets spéciaux qui sont couramment utilisés dans les films et les vidéos, comme les dissolutions, les balayages et les fondus.

Dans une dissolution, une image est progressivement remplacée par une autre dans une séquence d’images fluide. Bien que Direct3D propose des méthodes d’utilisation de plusieurs fusions de texture pour obtenir le même effet, les applications qui utilisent le gabarit de mémoire tampon pour les dissolutions peuvent utiliser les fonctions de fusion de textures pour d’autres effets pendant qu'ils procèdent à une dissolution.

Lorsque votre application effectue une dissolution, elle doit rendre deux images différentes. Elle utilise le gabarit de mémoire tampon à partir duquel les pixels de chaque image sont dessinés sur la surface de rendu cible. Vous pouvez définir une série de masques de gabarit et les copier dans le gabarit de mémoire tampon sur les images successives. Sinon, vous pouvez définir un masque de gabarit de base pour la première image, puis le modifier de manière incrémentielle.

Au début de la dissolution, votre application définit la fonction de gabarit et le masque de gabarit de manière à ce que la plupart des pixels de l’image de démarrage passent avec succès le test de gabarit. La plupart des pixels de l’image de fin doivent échouer au test de gabarit. Sur les images successives, le masque de gabarit est mis à jour de manière à ce que le nombre de pixels de l'image de départ qui réussissent le test soit de plus en plus faible. Plus les images défilent et moins les pixels de l'image de fin échouent au test. De cette manière, votre application peut effectuer une dissolution à l'aide de n’importe quel modèle de dissolution arbitraire.

Le fondu à l'ouverture ou à la fermeture sont des cas particuliers de dissolution. Lorsque vous effectuez un fondu à l'ouverture, le gabarit de mémoire est utilisé pour procéder à une dissolution à partir d’une image noire ou blanche pour le rendu d’une scène 3D. Au contraire, pour le fondu à la fermeture, votre application démarre par le rendu d'une scène 3D et enchaîne par un fondu en noir ou blanc. Le fondu peut être effectué à l’aide de n’importe quel modèle arbitraire que vous souhaitez utiliser.

Les applications Direct3D utilisent une technique similaire pour les balayages. Par exemple, lorsqu’une application effectue un balayage de gauche à droite, l’image de fin apparaît progressivement au-dessus de l’image de départ de gauche à droite. Comme dans une dissolution, vous devez définir une série de masques de gabarit qui sont chargés dans la mémoire tampon de gabarit sur des images successives ou successivement modifier le masque de gabarit de départ. Les masques de gabarit sont utilisés pour désactiver l’écriture de pixels de l’image de départ et pour permettre l’écriture de pixels de l’image de fin.

Un balayage est plus complexe qu’une dissolution car votre application doit lire des pixels à partir de l’image de fin dans l’ordre inverse du balayage. Autrement dit, si le balayage se déplace de gauche à droite, votre application doit lire des pixels de l’image de fin de droite à gauche.

## <a name="span-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanoutlines-and-silhouettes"></a><span id="Outlines_and_silhouettes"></span><span id="outlines_and_silhouettes"></span><span id="OUTLINES_AND_SILHOUETTES"></span>Plans et silhouettes


Vous pouvez utiliser la mémoire tampon de gabarit pour des effets plus abstraits, comme les plans et les silhouettes.

Si votre application applique un masque de gabarit à l’image d’une primitive qui possède la même forme mais qui est légèrement plus petite, l’image qui en résulte contient uniquement le plan de la primitive. L’application peut ensuite remplir la zone masquée par le gabarit de l’image avec une couleur unie, ce qui donne un effet de relief à la primitive.

Si le masque de gabarit est de la même taille et possède la même forme que la primitive que vous rendez, l’image résultante contient un trou où doit se trouver la primitive. Votre application peut ensuite remplir le trou avec du noir afin de produire une silhouette de la primitive.

## <a name="span-idtwo-sidedstencilspanspan-idtwo-sidedstencilspanspan-idtwo-sidedstencilspantwo-sided-stencil"></a><span id="Two-sided_stencil"></span><span id="two-sided_stencil"></span><span id="TWO-SIDED_STENCIL"></span>Gabarit recto-verso


Les volumes d’ombre sont utilisés pour dessiner des ombres avec la mémoire tampon de gabarit. L’application calcule les volumes d’ombre castés en masquant la géométrie, en calculant les bords de la silhouette et en les extrudant hors de la lumière dans un ensemble de volumes 3D. Ces volumes sont ensuite rendus deux fois dans la mémoire tampon de gabarit.

Le premier rendu dessine des polygones orientés vers l'avant et incrémente les valeurs de mémoire tampon de gabarit. Le deuxième rendu dessine des polygones orientés vers l'arrière du volume d'ombre et décrémente les valeurs de mémoire tampon de gabarit. En règle générale, toutes les valeurs incrémentées et décrémenter incrémentées. Toutefois, la scène avait déjà été rendue avec une géométrie normale à l’origine de certains pixels au échouent au test de tampon z-buffer mesure que le volume d’ombre était rendu. Les valeurs conservées dans la mémoire tampon de gabarit correspondent aux pixels qui se trouvent dans l’ombre. Ces contenus restants de la mémoire tampon de gabarit sont utilisés en tant que masque, afin de fusionner à l'aide du canal alpha une grande zone entièrement cernée de noir dans la scène.$$$ Lorsque la mémoire tampon de gabarit agit en tant que masque, le résultat consiste à obscurcir les pixels qui sont dans les ombres.

Cela signifie que la géométrie d'ombre est dessinée deux fois pour chaque source de lumière, ce qui met la pression sur le débit de vertex du GPU. La fonctionnalité de gabarit recto-verso a été créée pour atténuer cette situation. Dans cette approche, il existe deux jeux d'états de gabarit (indiqués ci-dessous), le premier permet de définir les valeurs des triangles orientés vers l'avant et l’autre celles des triangles orientés vers l'arrière.$$$ De cette façon, un seul passage est dessiné par volume d'ombre, par lumière.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Tampons de profondeur et tampons stencil buffer](depth-and-stencil-buffers.md)

 

 




