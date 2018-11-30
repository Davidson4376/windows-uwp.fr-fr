---
title: Configuration de la fonctionnalité de profondeur-gabarit
description: Cette section décrit les étapes de configuration de la mémoire tampon de profondeur-gabarit et l’état de profondeur-gabarit pour l’étape de fusion/sortie.
ms.assetid: B3F6CDAA-93ED-4DC1-8E69-972C557C7920
keywords:
- Configuration de la fonctionnalité de profondeur-gabarit
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 98cb6c62248fbf273a9d7ca1ef0d1d82293122eb
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8207760"
---
# <a name="span-iddirect3dconceptsconfiguringdepth-stencilfunctionalityspanconfiguring-depth-stencil-functionality"></a><span id="direct3dconcepts.configuring_depth-stencil_functionality"></span>Configuration de la fonctionnalité de profondeur-gabarit


Cette section décrit les étapes de configuration de la mémoire tampon de profondeur-gabarit et l’état de profondeur-gabarit pour l’étape de fusion/sortie.

Dès lors que vous savez comment utiliser la mémoire tampon de profondeur-gabarit et l’état de profondeur-gabarit correspondant, reportez-vous aux [techniques de gabarit avancées](#advanced-stencil-techniques).

## <a name="span-idcreatedepthstencilstatespanspan-idcreatedepthstencilstatespanspan-idcreatedepthstencilstatespancreate-depth-stencil-state"></a><span id="Create_Depth_Stencil_State"></span><span id="create_depth_stencil_state"></span><span id="CREATE_DEPTH_STENCIL_STATE"></span>Créer un état de profondeur-gabarit


L'état de profondeur-gabarit indique à l’étape de fusion/sortie comment effectuer le [test de profondeur-gabarit](https://msdn.microsoft.com/library/windows/desktop/bb205120). Le test de profondeur-gabarit détermine s'il convient ou non d'écrire un pixel donné.

## <a name="span-idbinddepthstenciltotheomstagespanspan-idbinddepthstenciltotheomstagespanspan-idbinddepthstenciltotheomstagespanbind-depth-stencil-data-to-the-om-stage"></a><span id="Bind_Depth_Stencil_to_the_OM_Stage"></span><span id="bind_depth_stencil_to_the_om_stage"></span><span id="BIND_DEPTH_STENCIL_TO_THE_OM_STAGE"></span>Lier des données de profondeur-gabarit à l’étape OM


Liez l’état de profondeur-gabarit.

Liez la ressource de profondeur-gabarit à l’aide d’un affichage.

Les cibles de rendu doivent toutes avoir le même type de ressource. Si un anticrénelage multi-échantillon est utilisé, toutes les cibles de rendu liées et tous les tampons de profondeur doivent avoir les mêmes nombres d’échantillons.

Lorsqu’une mémoire tampon est utilisée comme cible de rendu, les tests de profondeur-gabarit et les cibles de rendu multiples ne sont pas pris en charge.

-   Il est possible de lier jusqu'à 8cibles de rendu simultanément.
-   Toutes les cibles de rendu doivent avoir la même taille dans toutes les dimensions (largeur et hauteur, et profondeur 3D ou taille de tableau pour les types \*Array).
-   Chaque cible de rendu peut avoir un format de données différent.
-   Les masques d'écriture contrôlent les données qui sont écrites dans une cible de rendu. Les masques d'écriture de sortie contrôlent au niveau de chaque cible de rendu et au niveau de chaque composant les données écrites sur la ou les cible(s) de rendu.

## <a name="span-idadvancedstenciltechniquesspanspan-idadvancedstenciltechniquesspanspan-idadvancedstenciltechniquesspanspan-idadvanced-stencil-techniquesspanadvanced-stencil-techniques"></a><span id="Advanced_Stencil_Techniques"></span><span id="advanced_stencil_techniques"></span><span id="ADVANCED_STENCIL_TECHNIQUES"></span><span id="advanced-stencil-techniques"></span>Techniques de gabarit avancées


La partie gabarit de la mémoire tampon de profondeur-gabarit peut être utilisée pour créer des effets de rendu, comme la composition, le transfert et la mise en plan.

-   [Composition](#compositing)
-   [Transfert](#decaling)
-   [Plans et silhouettes](#outlines-and-silhouettes)
-   [Gabarit recto-verso](#two-sided-stencil)
-   [Lecture de la mémoire tampon de profondeur-gabarit en tant que texture](#reading-the-depth-stencil-buffer-as-a-texture)

### <a name="span-idcompositingspanspan-idcompositingspanspan-idcompositingspancompositing"></a><span id="Compositing"></span><span id="compositing"></span><span id="COMPOSITING"></span>Composition

Votre application peut utiliser la mémoire tampon de gabarit pour composer des images 2D ou 3D dans une scène 3D. Un masque de mémoire tampon de gabarit est utilisé pour masquer une zone de la surface cible de rendu. Les informations 2D stockées, comme le texte ou les bitmaps, peuvent ensuite être insérées dans la zone masquée. Votre application peut également rendre des primitives 3D supplémentaires sur la région masquée du gabarit de la surface de cible de rendu. Il peut même rendre une scène entière.

Les jeux composent parfois plusieurs scènes 3D en même temps. Par exemple, les jeux de voitures incluent généralement une fonction rétroviseur. Le rétroviseur contient la vue de la scène 3D derrière le pilote. Il s'agit essentiellement d'une deuxième scène 3D composée à partir de la vue avant du pilote.

### <a name="span-iddecalingspanspan-iddecalingspanspan-iddecalingspandecaling"></a><span id="Decaling"></span><span id="decaling"></span><span id="DECALING"></span>Transfert

Les applications Direct3D utilisent le transfert pour déterminer les pixels qui sont tracés à la surface cible de rendu à partir d'une image de primitive donnée. Les applications appliquent des transferts aux images des primitives afin de rendre correctement les polygones coplanaires.

Par exemple, lorsque vous appliquez des traces de pneu et des lignes jaunes sur une piste, les inscriptions doivent apparaître directement sur la route. Toutefois, les valeurs z des marques et de la route sont identiques. Par conséquent, le tampon de profondeur peut ne pas produire une séparation nette entre les deux. Certains pixels de la primitive arrière peuvent être rendus au-dessus de la primitive avant, et inversement. L’image obtenue semble trembloter$$$ d’une image à une autre. Cet effet est appelé z-fighting ou flimmering.$$$

Pour résoudre ce problème, utilisez un gabarit pour masquer la section de la primitive arrière où apparaît le transfert. Désactivez la mise en mémoire tampon z et rendez l'image de la primitive avant dans la zone masquée de la surface de rendu cible.

La fusion de plusieurs textures peut aider à résoudre ce problème.

### <a name="span-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanspan-idoutlines-and-silhouettesoutlines-and-silhouettes"></a><span id="Outlines_and_Silhouettes"></span><span id="outlines_and_silhouettes"></span><span id="OUTLINES_AND_SILHOUETTES"></span><span id="outlines-and-silhouettes">Plans et silhouettes

Vous pouvez utiliser la mémoire tampon de gabarit pour des effets plus abstraits, comme les plans et les silhouettes.

Si votre application effectue deux passes de rendu (l'une pour générer le masque de gabarit, l'autre pour appliquer le masque de gabarit sur l’image), mais que les primitives sont légèrement plus petites sur la deuxième passe, l’image obtenue contiendra uniquement les contours de la primitive. L’application peut ensuite remplir la zone masquée par le gabarit de l’image avec une couleur unie, ce qui donne un effet de relief à la primitive.

Si le masque de gabarit est de la même taille et possède la même forme que la primitive que vous rendez, l’image résultante contient un trou où doit se trouver la primitive. Votre application peut ensuite remplir le trou avec du noir afin de produire une silhouette de la primitive.

### <a name="span-idtwosidedstencilspanspan-idtwosidedstencilspanspan-idtwosidedstencilspantwo-sided-stencil"></a><span id="Two_Sided_Stencil"></span><span id="two_sided_stencil"></span><span id="TWO_SIDED_STENCIL"></span>Gabarit recto-verso

Les volumes d’ombre sont utilisés pour dessiner des ombres avec la mémoire tampon de gabarit. L’application calcule les volumes d’ombre castés en masquant la géométrie, en calculant les bords de la silhouette et en les extrudant hors de la lumière dans un ensemble de volumes 3D. Ces volumes sont ensuite rendus deux fois dans la mémoire tampon de gabarit.

Le premier rendu dessine des polygones orientés vers l'avant et incrémente les valeurs de mémoire tampon de gabarit. Le deuxième rendu dessine des polygones orientés vers l'arrière du volume d'ombre et décrémente les valeurs de mémoire tampon de gabarit.

En règle générale, toutes les valeurs incrémentées et décrémenter incrémentées. Toutefois, la scène avait déjà été rendue avec une géométrie normale à l’origine de certains pixels au échouent au test de tampon z-buffer mesure que le volume d’ombre était rendu. Les valeurs conservées dans la mémoire tampon de gabarit correspondent aux pixels qui se trouvent dans l’ombre. Ces contenus restants de la mémoire tampon de gabarit sont utilisés en tant que masque, afin de fusionner à l'aide du canal alpha une grande zone entièrement cernée de noir dans la scène.$$$ Lorsque la mémoire tampon de gabarit agit en tant que masque, le résultat consiste à obscurcir les pixels qui sont dans les ombres.

Cela signifie que la géométrie d'ombre est dessinée deux fois pour chaque source de lumière, ce qui met la pression sur le débit de vertex du GPU. La fonctionnalité de gabarit recto-verso a été créée pour atténuer cette situation. Dans cette approche, il existe deux jeux d'états de gabarit (indiqués ci-dessous), le premier permet de définir les valeurs des triangles orientés vers l'avant et l’autre celles des triangles orientés vers l'arrière.$$$ De cette façon, un seul passage est dessiné par volume d'ombre, par lumière.

### <a name="span-idreadingthedepth-stencilbufferasatexturespanspan-idreadingthedepth-stencilbufferasatexturespanspan-idreadingthedepth-stencilbufferasatexturespanspan-idreading-the-depth-stencil-buffer-as-a-texturespanreading-the-depth-stencil-buffer-as-a-texture"></a><span id="Reading_the_Depth-Stencil_Buffer_as_a_Texture"></span><span id="reading_the_depth-stencil_buffer_as_a_texture"></span><span id="READING_THE_DEPTH-STENCIL_BUFFER_AS_A_TEXTURE"></span><span id="reading-the-depth-stencil-buffer-as-a-texture"></span>Lecture de la mémoire tampon de profondeur-gabarit en tant que texture

Une mémoire tampon de profondeur-gabarit peut être lue par un nuanceur en tant que texture. Une application qui lit une mémoire tampon de profondeur-gabarit en tant que texture effectue un rendu en deux passes; la première pour écrire dans le tampon de profondeur-gabarit, la seconde pour lire à partir de la mémoire tampon. Ceci permet un nuanceur de comparer les valeurs de profondeur ou de gabarit précédemment écrites dans la mémoire tampon avec la valeur du pixel actuellement rendu. Le résultat de la comparaison peut être utilisé pour créer divers effets, tels que le mappage d'ombres ou les particules souples dans un système à particules.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Étape Output Merger (OM)](output-merger-stage--om-.md)

[Pipeline graphique](graphics-pipeline.md)

[Étape de fusion de sortie](https://msdn.microsoft.com/library/windows/desktop/bb205120)
