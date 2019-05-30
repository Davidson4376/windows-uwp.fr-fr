---
title: Pipeline graphique
description: Le pipeline graphique Direct3D est conçu pour la génération de graphismes destinés aux applications de jeu en temps réel. Les données progressent de l’entrée vers la sortie en transitant par chacune des étapes configurables ou programmables de ce pipeline.
ms.assetid: C9519AD0-5425-48BD-9FF4-AED8959CA4AD
keywords:
- Pipeline graphique
- Étapes du pipeline
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1b931268dc20f40c1bc1d7c700f346d29d6aa9d6
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370611"
---
# <a name="graphics-pipeline"></a>Pipeline graphique


Le pipeline graphique Direct3D est conçu pour la génération de graphismes destinés aux applications de jeu en temps réel. Les données progressent de l’entrée vers la sortie en transitant par chacune des étapes configurables ou programmables de ce pipeline.

Toutes les étapes sont configurables à l’aide de l’API Direct3D. Les étapes qui utilisent des noyaux de nuanceur communs (blocs rectangulaires arrondis) sont programmables au moyen du langage de programmation [HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl). En conséquence, le pipeline offre un haut degré de flexibilité et d’adaptabilité.

Les étapes les plus couramment utilisées sont l’étape du nuanceur de vertex (VS) et l’étape du nuanceur de pixels (PS). Si vous ne fournissez même pas ces étapes de nuanceur, l’application utilise un nuanceur de vertex et un nuanceur de pixels directs sans opération par défaut.

![diagramme du flux de données dans le pipeline programmable direct3d 11](images/d3d11-pipeline-stages.jpg)

## <a name="input-assembler-stage"></a>Étape de l’assembleur d’entrée

|-|-| |Objectif|L'[étape Assembleur de données](input-assembler-stage--ia-.md) fournit des données primitives et adjacentes au pipeline, comme des triangles, des lignes et des points, y compris des identifiants sémantiques pour rendre les nuanceurs encore plus efficaces en réduisant le traitement des données primitives déjà traitées.| |Entrée|Données primitives (triangles, lignes et/ou points), provenant des tampons renseignés par les utilisateurs dans la mémoire. Données de contiguïté éventuelles. Un triangle se composerait de 3 vertex pour chaque triangle et éventuellement de 3 vertex pour les données adjacentes par triangle. | |Sortie| Les données primitives avec valeurs générées par le système associé (par exemple, un ID de données primitives, un ID d’instance ou un ID de vertex). |

## <a name="vertex-shader-stage"></a>Étape du nuanceur de vertex

|-|-| |Objectif| L'[étape du nuanceur de vertex](vertex-shader-stage--vs-.md) traite les vertex, généralement par le biais d’opérations de type transformation, application d’apparence et éclairage. Un nuanceur de vertex prend un vertex d’entrée spécifique et produit un seul vertex de sortie. Les opérations par vertex individuel, telles que des transformations, les applications d'apparence, la transformation et les conditions d’éclairage par vertex. | | Entrée | Un vertex unique, avec des valeurs générées par le système VertexID et InstanceID. Chaque entrée de nuanceur de vertex peut comprendre jusqu'à 16 vecteurs de 32 bits (jusqu'à 4 composants chacun). | |Sortie| Un vertex unique. Chaque vertex de sortie peut comprendre jusqu'à 16 vecteurs de 32 bits à 4 composants.|
 
## <a name="hull-shader-stage"></a>Étape Nuanceur de coque
 
|-|-| |Objectif| L'[étape nuanceur de coque](hull-shader-stage--hs-.md) est une des étapes qui divisent efficacement une surface unique en de nombreux triangles. Un nuanceur de coque est invoqué une fois par patch et transforme les points de contrôle d’entrée qui définissent une surface d’ordre bas en points de contrôle constituant un patch. Cette étape effectue également des calculs par correctif pour fournir des données pour les étapes Réduction en mosaïque et Nuanceur de domaine. | | Entrée | Entre 1 et 32 points de contrôle d’entrée, qui définissent ensemble une surface d'ordre bas. | | Sortie | Entre 1 et 32 points de contrôle de sortie qui constituent un correctif. Le nuanceur de coque déclare l'état de l'étape de réduction en mosaïque, y compris le nombre de points de contrôle, le type de face de correctif et le type de partitionnement à utiliser lors de la réduction en mosaïque.

## <a name="tessellator-stage"></a>Étape Réduction en mosaïque

|-|-| | Objectif | L'[étape Réduction en mosaïque](tessellator-stage--ts-.md) crée un modèle d’échantillonnage du domaine qui représente le correctif de géométrie et génère un ensemble d’objets de plus petite taille (triangles, points ou lignes) qui connectent ces échantillons. | | Entrée | Le processus de réduction en mosaïque fonctionne une fois par correctif, à l’aide de facteurs de pavage (qui spécifient le degré de finesse avec lequel le domaine sera fractionné) et le type de partitionnement (qui spécifie l’algorithme utilisé pour diviser le correctif) qui sont transférés depuis l’étape du nuanceur de coque. | |Sortie|Le processus de réduction en mosaïque produit des coordonnées UV (et, de manière facultative, des W) et la topologie de la surface en vue de l'étape de nuanceur de coque.|

## <a name="domain-shader-stage"></a>Étape Nuanceur de domaine

|-|-| |Objectif|L'[étape Nuanceur de domaine](domain-shader-stage--ds-.md) calcule la position du vertex d'un point sous-divisé dans le correctif de sortie, et la position de vertex qui correspond à chaque modèle de domaine. Un nuanceur de domaine est exécuté une fois pour chaque point de sortie de l'étape de réduction en mosaïque et est doté d'un accès en lecture seule au correctif de sortie du nuanceur de coque et aux valeurs constantes des correctifs de sortie, ainsi qu'aux coordonnées UV de sortie de l’étape de réduction en mosaïque. | | Sortie | Un nuanceur de domaine utilise les points de contrôle de sortie à partir de l'[étape de nuanceur de coque](hull-shader-stage--hs-.md). Les sorties du nuanceur de coque sont les suivantes : Points de contrôle, données de constantes de correctifs et les facteurs de pavage (les facteurs de pavage peuvent inclure les valeurs utilisées par du paveur fonction fixe, ainsi que les valeurs brutes - avant d’être arrondies par pavage entier - qui facilite la geomorphing, par exemple ). Un nuanceur de domaine est appelé une fois pour chaque coordonnée de sortie à partir de l'[étape de réduction en mosaïque](tessellator-stage--ts-.md). | | Sortie | L’étape de nuanceur de domaine génère la position du vertex d'un point sous-divisé dans le correctif de sortie. |

## <a name="geometry-shader-stage"></a>Étape Nuanceur de géométrie

|-|-| |Objectif| L'[étape de nuanceur de géométrie](geometry-shader-stage--gs-.md) traite les primitifs entiers : triangles, lignes et points, ainsi que les vertex adjacents. Cette étape prend en charge l’amplification et le filtrage de géométrie. Elle est utile pour les algorithmes tels que l’expansion de point-sprites, les systèmes de particules dynamiques, la génération de fourrure, la génération de volumes d’ombre, le rendu sous forme de cubemap en une seule passe, l’échange et la configuration de matériaux par primitive, y compris la génération de coordonnées barycentriques sous la forme de données de primitive pour permettre à un nuanceur de pixels d’effectuer une interpolation d’attributs personnalisés. | | Entrée | Contrairement aux nuanceurs de vertex, qui fonctionnent sur un seul vertex, les entrées du nuanceur de géométrie sont des vertex destinés à une donnée primitive complète (trois vertex pour les triangles, deux vertex pour les lignes ou un vertex unique pour le point). | | Sortie | L’étape nuanceur de géométrie est susceptible de générer plusieurs vertex pour former une seule topologie sélectionnée. Les topologies de sortie du nuanceur de géométrie disponibles sont <strong>tristrip</strong>, <strong>linestrip</strong> et <strong>pointlist</strong>. Le nombre de primitives émises peut varier librement au sein d’une invocation du nuanceur de géométrie, bien que le nombre maximal de vertex pouvant être émis doive être déclaré de manière statique. Les longueurs de bande d'un appel de nuanceur de géométrie peuvent être arbitraires et les nouvelles bandes peuvent être créées via la fonction [RestartStrip](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-so-restartstrip) HLSL.|

## <a name="stream-output-stage"></a>Étape Sortie du flux

|-|-| |Objectif| L'[étape Sortie du flux](stream-output-stage--so-.md) produit en continu (ou diffuse) des données de vertex à partir de l'étape précédente active vers un ou plusieurs tampons de la mémoire. Les données diffusées à partir de la mémoire peuvent être renvoyées dans le pipeline en tant que données d’entrée ou peuvent être lues à partir de l’UC. | | Entrée | Les données de vertex issues d’une précédente étape du pipeline. | | Sortie | L'étape de flux de sortie génère en permanence (ou diffuse) les données de vertex à partir de l’étape précédente active, telle que l’étape Nuanceur de géométrie, pour un ou plusieurs tampons en mémoire. Si l'étape de nuanceur de géométrie est inactive, et que l'étape de sortie de flux est active, celle-ci génère en continu des données de vertex depuis l'étape de nuanceur de domaines vers les tampons en mémoire (ou, si cette étape est inactive, depuis l'étape de nuanceur de vertex).|

## <a name="rasterizer-stage"></a>Étape Rastériseur

|-|-| |Objectif|L'[étape de rastériseur](rasterizer-stage--rs-.md) extrait les primitives qui ne sont pas visibles, les prépare pour l'étape de nuanceur de pixels et détermine le mode d’invocation des nuanceurs de pixels. Convertit les informations vectorielles (composées de formes ou de primitives) dans une image raster (composée de pixels) pour afficher les graphiques 3D en temps réel. | | Entrée | Les vertex (x, y, z, w) parvenant à l'étape rastériseur sont considérés comme étant dans un espace d'image homogène. Dans cet espace de coordonnées, l’axe X pointe vers la droite, l’axe Y pointe vers le haut et l'axe Z pointe vers la caméra. | | Sortie | Les pixels réels qui doivent être rendus. Cela inclut certains attributs de vertex utilisés dans l'interpolation par le nuanceur de pixels.|

## <a name="pixel-shader-stage"></a>Étape du nuanceur de pixels
 
|-|-| | Objectif | L'[étape de nuanceur de pixels](pixel-shader-stage--ps-.md) reçoit des données interpolées pour une primitive et génère des données par pixel telles que la couleur. | | Entrée | Lorsque le pipeline est configuré sans un nuanceur de géométrie, un nuanceur de pixels est limité à 16, 32 bits, et 4 composants. Dans le cas contraire, un nuanceur de pixels peut prendre en charge jusqu’à 32 entrées 32 bits de 4 composants. Les données d’entrée du nuanceur de pixels comprennent les attributs de vertex (qui peuvent être interpolés avec ou sans correction de la perspective) ou peuvent être traitées en tant que constantes par primitive. Les entrées du nuanceur de pixels sont interpolées à partir des attributs de vertex de la primitive faisant l’objet de la rastérisation, en fonction du mode d’interpolation déclaré. Si une primitive est tronquée avant la rastérisation, le mode d’interpolation est également réalisé pendant le processus de découpage. | |Sortie|Un nuanceur de pixel peut produire jusqu'à 8 couleurs, 32 bits, à 4 composants, ou aucune couleur si le pixel est éliminé. Les composants enregistrés de sortie du nuanceur de pixel doivent être déclarés avant de pouvoir être utilisés, et chaque enregistrement a le droit à un masque d'écriture de sortie.|

## <a name="output-merger-stage"></a>Étape Fusion de sortie
 
|-|-| | Objectif | L'[étape de fusion de sortie](output-merger-stage--om-.md) combine les différents types de données de sortie (valeurs du nuanceur de pixels, informations sur la profondeur et le gabarit) avec le contenu de la cible de rendu et des tampons de profondeur/gabarit pour générer le résultat final du pipeline. | | Entrée | Les entrées de la fusion de sortie sont l’état du Pipeline, les données de pixel générées par les nuanceurs de pixels, le contenu des cibles de rendu et le contenu des mémoires tampons de profondeur/gabarit. | | Sortie | La couleur rendue du pixel finale. |

## <a name="related-topics"></a>Rubriques connexes

- [Guide d’apprentissage graphiques Direct3D](index.md)

- [Pipeline de calcul](compute-pipeline.md)
