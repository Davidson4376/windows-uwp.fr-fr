---
title: Pipeline graphique
description: "Le pipeline graphiqueDirect3D est conçu pour la génération de graphismes destinés aux applications de jeu en temps réel. Les données progressent de l’entrée vers la sortie en transitant par chacune des étapes configurables ou programmables de ce pipeline."
ms.assetid: C9519AD0-5425-48BD-9FF4-AED8959CA4AD
keywords:
- Pipeline graphique
- "Étapes du pipeline"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: af583deb51b93bffc05c4371466fa8412bb0476d
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="graphics-pipeline"></a>Pipeline graphique


Le pipeline graphiqueDirect3D est conçu pour la génération de graphismes destinés aux applications de jeu en temps réel. Les données progressent de l’entrée vers la sortie en transitant par chacune des étapes configurables ou programmables de ce pipeline.

Toutes les étapes sont configurables à l’aide de l’API Direct3D. Les étapes qui utilisent des noyaux de nuanceur communs (blocs rectangulaires arrondis) sont programmables au moyen du langage de programmation [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561). En conséquence, le pipeline offre un haut degré de flexibilité et d’adaptabilité.

Les étapes les plus couramment utilisées sont l’étape du nuanceur de vertex (VS) et l’étape du nuanceur de pixels (PS). Si vous ne fournissez même pas ces étapes de nuanceur, l’application utilise un nuanceur de vertex et un nuanceur de pixels directs sans opération par défaut.

![diagramme du flux de données dans le pipeline programmable direct3d11](images/d3d11-pipeline-stages.jpg)

## <a name="input-assembler-stage"></a>Étape de l’assembleur d’entrée

 

| | |
|-|-|
|Objectif|L’[étape de l’assembleur d’entrée (IA, Input Assembler)](input-assembler-stage--ia-.md) fournit au pipeline des données de primitive et de contiguïté, telles que des triangles, des lignes et des points, incluant des ID sémantiques pour améliorer l’efficacité des nuanceurs en restreignant le traitement aux primitives qui n’ont pas encore été traitées.|
|Entrée|Données de primitive (triangles, lignes et/ou points) provenant de mémoires tampons en mémoire alimentées par l’utilisateur. Données de contiguïté éventuelles. Par exemple, il existe 3vertex par triangle, et éventuellement 3vertex pour les données de contiguïté par triangle.|
|Sortie|Primitives avec valeurs attachées générées par le système (par exemple, ID de primitive, ID d’instance ou ID de vertex).|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Input Assembler (IA) stage](input-assembler-stage--ia-.md) supplies primitive and adjacency data to the pipeline, such as triangles, lines and points, including semantics IDs to help make shaders more efficient by reducing processing to primitives that haven't already been processed.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"> Primitive data (triangles, lines, and/or points), from user-filled buffers in memory. And possibly adjacency data. A triangle would be 3 vertices for each triangle and possibly 3 vertices for adjacency data per triangle.  </td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">Primitives with attached system-generated values (such as a primitive ID, an instance ID, or a vertex ID). </td>
</tr>
</tbody>
</table>
 -->



## <a name="vertex-shader-stage"></a>Étape du nuanceur de vertex
 

| | |
|-|-|
|Objectif|L’[étape du nuanceur de vertex (VS, Vertex Shader)](vertex-shader-stage--vs-.md) traite les vertex, notamment en leur appliquant des transformations, une apparence et un éclairage. Un nuanceur de vertex prend un vertex d’entrée spécifique et produit un seul vertex de sortie. Il applique différentes opérations par vertex, telles que des transformations, une morphose, ainsi que l’application d’une apparence et d’un éclairage.|
|Entrée|Vertex unique, avec des valeurs VertexID et InstanceID générées par le système. Chaque vertex d’entrée d'un nuanceur de vertex peut comprendre jusqu'à 16vecteurs de 32bits (d'un maximum de 4composants chacun).|
|Sortie|Vertex unique. Chaque vertex de sortie peut également comprendre jusqu’à 16vecteurs de 32bits à 4composants.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Vertex Shader (VS) stage](vertex-shader-stage--vs-.md) processes vertices, typically performing operations such as transformations, skinning, and lighting. A vertex shader takes a single input vertex and produces a single output vertex. Individual per-vertex operations, such as:
<ul>
<li>Transformations</li>
<li>Skinning</li>
<li>Morphing</li>
<li>Per-vertex lighting</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">A single vertex, with VertexID and InstanceID system-generated values. Each vertex shader input vertex can be comprised of up to 16 32-bit vectors (up to 4 components each).</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">A single vertex. Each output vertex can be comprised of as many as 16 32-bit 4-component vectors.</td>
</tr>
</tbody>
</table>
-->
 

## <a name="hull-shader-stage"></a>Étape du nuanceur de coque
 

| | |
|-|-|
|Objectif|L’[étape du nuanceur de coque (HS, Hull Shader)](hull-shader-stage--hs-.md) constitue l’une des étapes de pavage qui décomposent efficacement une surface unique d’un modèle en un grand nombre de triangles. Un nuanceur de coque est invoqué une fois par patch et transforme les points de contrôle d’entrée qui définissent une surface d’ordre bas en points de contrôle constituant un patch. Ce nuanceur effectue également certains calculs par patch afin de fournir des données pour l’étape du paveur (TS) et l’étape du nuanceur de domaine (DS).|
|Entrée|Entre 1 et 32points de contrôle d’entrée, qui définissent conjointement une surface d’ordre bas.|
|Sortie|Entre 1 et 32points de contrôle de sortie, qui composent un patch. Le nuanceur de coque déclare l’état requis par l’étape du paveur (TS), y compris le nombre de points de contrôle, le type de face du patch, ainsi que le type de partitionnement à utiliser lors du pavage.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Hull Shader (HS) stage](hull-shader-stage--hs-.md) is one of the tessellation stages, which efficiently break up a single surface of a model into many triangles. A hull shader is invoked once per patch, and it transforms input control points that define a low-order surface into control points that make up a patch. It also does some per-patch calculations to provide data for the Tessellator (TS) stage and the Domain Shader (DS) stage.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Between 1 and 32 input control points, which together define a low-order surface. </td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">Between 1 and 32 output control points, which together make up a patch. The hull shader declares the state for the Tessellator (TS) stage, including the number of control points, the type of patch face, and the type of partitioning to use when tessellating. </td>
</tr>
</tbody>
</table>
-->

## <a name="tessellator-stage"></a>Étape du paveur
 

| | |
|-|-|
|Objectif|L’[étape du paveur (TS, Tessellator)](tessellator-stage--ts-.md) crée un modèle d’échantillonnage du domaine qui représente le patch de géométrie et génère un ensemble d’objets plus petits (triangles, points ou lignes) qui relient ces échantillons.|
|Entrée|Le paveur opère une fois par patch à l’aide des facteurs de pavage (qui spécifient le raffinement du pavage du domaine) et du type de partitionnement (qui spécifie l’algorithme utilisé pour découper un patch) qui sont transmis par l’étape du nuanceur de coque. |
|Sortie|Le paveur produit en sortie des coordonnées UV (et éventuellement W) et la topologie de surface à l’intention de l’étape du nuanceur de domaine.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Tessellator (TS) stage](tessellator-stage--ts-.md) creates a sampling pattern of the domain that represents the geometry patch and generates a set of smaller objects (triangles, points, or lines) that connect these samples.   </td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"> The tessellator operates once per patch using the tessellation factors (which specify how finely the domain will be tessellated) and the type of partitioning (which specifies the algorithm used to slice up a patch) that are passed in from the hull-shader stage. </td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The tessellator outputs uv (and optionally w) coordinates and the surface topology to the domain-shader stage.     </td>
</tr>
</tbody>
</table>
 -->

## <a name="domain-shader-stage"></a>Étape du nuanceur de domaine
 

| | |
|-|-|
|Objectif|L’[étape du nuanceur de domaine (DS, Domain Shader)](domain-shader-stage--ds-.md) calcule la position de vertex d’un point subdivisé dans le patch de sortie; ce calcul porte sur la position de vertex correspondant à chaque échantillon de domaine. Un nuanceur de domaine est exécuté une fois par point de sortie de l’étape du paveur et dispose d’un accès en lecture seule au patch de sortie et aux constantes de patch de sortie du nuanceur de coque, ainsi qu’aux coordonnéesUV de sortie de l’étape du paveur.|
|Entrée|Un nuanceur de domaine consomme les points de contrôle de sortie de l’[étape du nuanceur de coque (HS)](hull-shader-stage--hs-.md). Les sorties du nuanceur de coque comprennent les éléments suivants: points de contrôle, données de constante de patch et facteurs de pavage (les facteurs de pavage peuvent inclure les valeurs utilisées par le paveur à fonction fixe et les valeurs brutes - avant l’arrondissement sous forme d’entier par le pavage - ce qui facilite notamment la géomorphose). Un nuanceur de domaine est invoqué une fois par coordonnée de sortie issue de l’[étape du paveur (TS)](tessellator-stage--ts-.md).|
|Sortie|L’étape du nuanceur de domaine (DS) produit en sortie la position de vertex d’un point subdivisé dans le patch de sortie.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Domain Shader (DS) stage](domain-shader-stage--ds-.md) calculates the vertex position of a subdivided point in the output patch; it calculates the vertex position that corresponds to each domain sample. A domain shader is run once per tessellator stage output point and has read-only access to the hull shader output patch and output patch constants, and the tessellator stage output UV coordinates.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"><ul>
<li>A domain shader consumes output control points from the [Hull Shader (HS) stage](hull-shader-stage--hs-.md). The hull shader outputs include:
<ul>
<li>Control points.</li>
<li>Patch constant data.</li>
<li>Tessellation factors. The tessellation factors can include the values used by the fixed-function tessellator as well as the raw values (before rounding by integer tessellation, for example), which facilitates geomorphing, for example.</li>
</ul></li>
<li>A domain shader is invoked once per output coordinate from the [Tessellator (TS) stage](tessellator-stage--ts-.md).</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The Domain Shader (DS) stage outputs the vertex position of a subdivided point in the output patch.</td>
</tr>
</tbody>
</table>
-->
 

## <a name="geometry-shader-stage"></a>Étape du nuanceur de géométrie
 

| | |
|-|-|
|Objectif|L’[étape du nuanceur de géométrie (GS, Geometry Shader)](geometry-shader-stage--gs-.md) traite des primitives complètes (triangles, lignes et points), ainsi que leurs vertex adjacents. Cette étape prend en charge l’amplification et le filtrage de géométrie. Elle est utile pour les algorithmes tels que l’expansion de point-sprites, les systèmes de particules dynamiques, la génération de fourrure, la génération de volumes d’ombre, le rendu sous forme de cubemap en une seule passe, l’échange et la configuration de matériaux par primitive, y compris la génération de coordonnées barycentriques sous la forme de données de primitive pour permettre à un nuanceur de pixels d’effectuer une interpolation d’attributs personnalisés. |
|Entrée|Contrairement aux nuanceurs de vertex, qui opèrent sur un seul vertex, les entrées du nuanceur de géométrie correspondent aux vertex d’une primitive complète (trois vertex pour les triangles, deux vertex pour les lignes ou un seul vertex pour les points).|
|Sortie|L’étape du nuanceur de géométrie (GS) peut produire en sortie plusieurs vertex formant une seule topologie sélectionnée. Les topologies de sortie du nuanceur de géométrie disponibles sont <strong>tristrip</strong>, <strong>linestrip</strong> et <strong>pointlist</strong>. Le nombre de primitives émises peut varier librement au sein d’une invocation du nuanceur de géométrie, bien que le nombre maximal de vertex pouvant être émis doive être déclaré de manière statique. Les longueurs de bande émises à partir d’une invocation du nuanceur de géométrie peuvent être arbitraires, et d’autres bandes peuvent être créées par le biais de la fonction HLSL [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660).|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Geometry Shader (GS) stage](geometry-shader-stage--gs-.md) processes entire primitives: triangles, lines, and points, along with their adjacent vertices. It is useful for algorithms including Point Sprite Expansion, Dynamic Particle Systems, and Shadow Volume Generation. It supports geometry amplification and de-amplification.
<ul>
<li>Point Sprite Expansion</li>
<li>Dynamic Particle Systems</li>
<li>Fur/Fin Generation</li>
<li>Shadow Volume Generation</li>
<li>Single Pass Render-to-Cubemap</li>
<li>Per-Primitive Material Swapping</li>
<li>Per-Primitive Material Setup, including generation of barycentric coordinates as primitive data so that a pixel shader can perform custom attribute interpolation.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Unlike vertex shaders, which operate on a single vertex, the geometry shader's inputs are the vertices for a full primitive (three vertices for triangles, two vertices for lines, or single vertex for point).</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The Geometry Shader (GS) stage is capable of outputting multiple vertices forming a single selected topology. Available geometry shader output topologies are <strong>tristrip</strong>, <strong>linestrip</strong>, and <strong>pointlist</strong>. The number of primitives emitted can vary freely within any invocation of the geometry shader, though the maximum number of vertices that could be emitted must be declared statically. Strip lengths emitted from a geometry shader invocation can be arbitrary, and new strips can be created via the [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660) HLSL function.</td>
</tr>
</tbody>
</table>
-->
 

## <a name="stream-output-stage"></a>Étape de sortie de flux
 

| | |
|-|-|
|Objectif|L’[étape de sortie de flux (SO, Stream Output)](stream-output-stage--so-.md) sort (diffuse) en continu les données de vertex de l’étape active précédente et les stocke dans une ou plusieurs mémoires tampon en mémoire. Les données diffusées et stockées en mémoire peuvent être renvoyées dans le pipeline en tant que données d’entrée ou être lues par le processeur.|
|Entrée|Données de vertex issues d’une étape précédente du pipeline.|
|Sortie|L’étape de sortie de flux (SO) sort (diffuse) en continu les données de vertex de l’étape active précédente, telle que l’étape du nuanceur de géométrie (GS), et les stocke dans une ou plusieurs mémoires tampon en mémoire. Si l’étape du nuanceur de géométrie (GS) est inactive et que l’étape de sortie de flux (SO) est active, cette dernière diffuse en continu les données de vertex de l’étape du nuanceur de domaine (DS) (ou de l’étape du nuanceur de vertex (VS) si l’étape DS est également inactive) et les stocke dans des mémoires tampon en mémoire.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Stream Output (SO) stage](stream-output-stage--so-.md) continuously outputs (or streams) vertex data from the previous active stage to one or more buffers in memory. Data streamed out to memory can be recirculated back into the pipeline as input data, or read-back from the CPU.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Vertex data from a previous pipeline stage.   </td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The Stream Output (SO) stage continuously outputs (or streams) vertex data from the previous active stage, such as the Geometry Shader (GS) stage, to one or more buffers in memory. If the Geometry Shader (GS) stage is inactive, and the Stream Output (SO) stage is active, it continuously outputs vertex data from the Domain Shader (DS) stage to buffers in memory (or if the DS is also inactive, from the Vertex Shader (VS) stage).</td>
</tr>
</tbody>
</table>
-->

## <a name="rasterizer-stage"></a>Étape du rastériseur
 

| | |
|-|-|
|Objectif|L’[étape du rastériseur ((RS, Rasterizer)](rasterizer-stage--rs-.md) extrait les primitives qui ne figurent pas dans la vue, prépare les primitives pour l’étape du nuanceur de pixels (PS) et détermine le mode d’invocation des nuanceurs de pixels. Cette étape convertit des informations de vecteur (composées de formes ou de primitives) en une image raster (constituée de pixels) afin d’afficher des graphismes3D en temps réel.|
|Entrée|Les vertex (X,Y,Z,W) introduits dans l’étape du rastériseur sont supposés se trouver dans un espace de découpage homogène. Dans cet espace de coordonnées, l’axeX pointe vers la droite, l’axeY pointe vers le haut et l’axeZ pointe dans la direction opposée à la caméra.|
|Sortie|Pixels réels qui doivent faire l’objet du rendu. Inclut certains attributs de vertex à utiliser dans l’interpolation par le nuanceur de pixels.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Rasterizer (RS) stage](rasterizer-stage--rs-.md) clips primitives that aren't in view, prepares primitives for the Pixel Shader (PS) stage, and determines how to invoke pixel shaders. Converts vector information (composed of shapes or primitives) into a raster image (composed of pixels) for the purpose of displaying real-time 3D graphics.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Vertices (x,y,z,w) coming into the Rasterizer stage are assumed to be in homogeneous clip-space. In this coordinate space the X axis points right, Y points up and Z points away from camera.</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The actual pixels that need to be rendered. Includes some vertex attributes for use in interpolation by the pixel Shader.</td>
</tr>
</tbody>
</table>
 -->

## <a name="pixel-shader-stage"></a>Étape du nuanceur de pixels
 

| | |
|-|-|
|Objectif|L’[étape du nuanceur de pixels (PS, Pixel Shader)](pixel-shader-stage--ps-.md) reçoit les données interpolées pour une primitive et génère des données par pixel telles que la couleur.|
|Entrée|Lorsque le pipeline est configuré sans nuanceur de géométrie, un nuanceur de pixels est limité à 16entrées 32bits de 4composants. Dans le cas contraire, un nuanceur de pixels peut prendre en charge jusqu’à 32entrées 32bits de 4composants. Les données d’entrée du nuanceur de pixels comprennent les attributs de vertex (qui peuvent être interpolés avec ou sans correction de la perspective) ou peuvent être traitées en tant que constantes par primitive. Les entrées du nuanceur de pixels sont interpolées à partir des attributs de vertex de la primitive faisant l’objet de la rastérisation, en fonction du mode d’interpolation déclaré. Si une primitive est extraite avant la rastérisation, le mode d’interpolation est également respecté pendant le processus de découpage. |
|Sortie|Un nuanceur de pixels peut produire en sortie jusqu’à 8couleurs 32bits de 4composants, ou aucune couleur si le pixel est ignoré. Les composants du registre de sortie du nuanceur de pixels doivent être déclarés avant d’être utilisés; un masque d’écriture de sortie distinct est autorisé pour chaque registre.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Pixel Shader (PS) stage](pixel-shader-stage--ps-.md) receives interpolated data for a primitive and generates per-pixel data such as color.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"><p>When the pipeline is configured without a geometry shader, a pixel shader is limited to 16, 32-bit, 4-component inputs. Otherwise, a pixel shader can take up to 32, 32-bit, 4-component inputs.</p>
<p>Pixel shader input data includes vertex attributes (that can be interpolated with or without perspective correction) or can be treated as per-primitive constants. Pixel shader inputs are interpolated from the vertex attributes of the primitive being rasterized, based on the interpolation mode declared. If a primitive gets clipped before rasterization, the interpolation mode is honored during the clipping process as well.</p></td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left"><p>A pixel shader can output up to 8, 32-bit, 4-component colors, or no color if the pixel is discarded. Pixel shader output register components must be declared before they can be used; each register is allowed a distinct output-write mask.</p></td>
</tr>
</tbody>
</table>
-->
 

## <a name="output-merger-stage"></a>Étape de fusion de sortie
 

| | |
|-|-|
|Objectif|L’[étape de fusion de sortie (OM, Output Merger)](output-merger-stage--om-.md) combine plusieurs types de données de sortie (valeurs du nuanceur de pixels, informations de profondeur et de stencil) avec le contenu de la cible de rendu et des tampons de profondeur/stencil buffer pour générer le résultat final du pipeline.|
|Entrée|Les entrées de l’étape de fusion de sortie sont l’état du pipeline, les données de pixel générées par les nuanceurs de pixels, le contenu des cibles de rendu et le contenu des tampons de profondeur/stencil buffer.|
|Sortie|Couleur finale du pixel rendu.|
| | |


<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Output Merger (OM) stage](output-merger-stage--om-.md) combines various types of output data (pixel shader values, depth and stencil information) with the contents of the render target and depth/stencil buffers to generate the final pipeline result.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"><ul>
<li>Pipeline state</li>
<li>The pixel data generated by the pixel shaders</li>
<li>The contents of the render targets</li>
<li>The contents of the depth/stencil buffers.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The final rendered pixel color.</td>
</tr>
</tbody>
</table>


| | |
|-|-|
|Purpose|xxxx|
|Input|yyyy|
|Output|zzzz|
| | |
-->

## <a name="related-topics"></a>Articles connexes


[Guide d’apprentissage des graphismes Direct3D](index.md)

[Pipeline de calcul](compute-pipeline.md)

 

 
