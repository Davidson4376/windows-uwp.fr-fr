---
title: Pipeline graphique
description: Le pipeline graphiqueDirect3D est conçu pour la génération de graphismes destinés aux applications de jeu en temps réel. Les données progressent de l’entrée vers la sortie en transitant par chacune des étapes configurables ou programmables de ce pipeline.
ms.assetid: C9519AD0-5425-48BD-9FF4-AED8959CA4AD
keywords:
- Pipeline graphique
- Étapes du pipeline
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 55621cec768e0aac680c3a84fd803e591459a97d
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8922440"
---
# <a name="graphics-pipeline"></a>Pipeline graphique


Le pipeline graphiqueDirect3D est conçu pour la génération de graphismes destinés aux applications de jeu en temps réel. Les données progressent de l’entrée vers la sortie en transitant par chacune des étapes configurables ou programmables de ce pipeline.

Toutes les étapes sont configurables à l’aide de l’API Direct3D. Les étapes qui utilisent des noyaux de nuanceur communs (blocs rectangulaires arrondis) sont programmables au moyen du langage de programmation [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561). En conséquence, le pipeline offre un haut degré de flexibilité et d’adaptabilité.

Les étapes les plus couramment utilisées sont l’étape du nuanceur de vertex (VS) et l’étape du nuanceur de pixels (PS). Si vous ne fournissez même pas ces étapes de nuanceur, l’application utilise un nuanceur de vertex et un nuanceur de pixels directs sans opération par défaut.

![diagramme du flux de données dans le pipeline programmable direct3d11](images/d3d11-pipeline-stages.jpg)

## <a name="input-assembler-stage"></a>Étape de l’assembleur d’entrée

|-|-| | Objectif | [L’étape d’assembleur d’entrée (IA)](input-assembler-stage--ia-.md) fournit primitive et les données de voisinage dans le pipeline, tels que des triangles, lignes et points, incluant des ID sémantiques pour vous aider à améliorer l’efficacité des nuanceurs en restreignant le traitement aux primitives qui n’ont pas été déjà traitées. | | Entrée | Données de primitive (triangles, lignes et/ou points), à partir des mémoires tampons renseignées par dans la mémoire. Données de contiguïté éventuelles. Un triangle serait 3 vertex pour chaque triangle et éventuellement 3 vertex pour les données de voisinage par triangle. | | Sortie | Les primitives avec valeurs attachées générées par le système (par exemple, un ID de primitive, un ID d’instance ou un ID de vertex). |

## <a name="vertex-shader-stage"></a>Étape du nuanceur de vertex

|-|-| | Objectif | L' [étape Vertex Shader (VS)](vertex-shader-stage--vs-.md) traite les vertex, en effectuant des opérations telles que des transformations, une apparence et éclairage. Un nuanceur de vertex prend un vertex d’entrée spécifique et produit un seul vertex de sortie. Opérations par vertex, telles que des Transformations, une, Morphose et d’éclairage par vertex. | | Entrée | Vertex unique, avec des valeurs VertexID et InstanceID générées par le système. Chaque vertex d’entrée de nuanceur de vertex peut comprendre jusqu'à 16 vecteurs de 32 bits (jusqu'à 4 composants chacun) de. | | Sortie | Vertex unique. Chaque vertex de sortie peut également comprendre jusqu'à 16 vecteurs de 4 composants 32 bits de. |
 
## <a name="hull-shader-stage"></a>Étape du nuanceur de coque
 
|-|-| | Objectif | L' [étape du nuanceur de coque (HS)](hull-shader-stage--hs-.md) est l’un des étapes de pavage qui décomposent efficacement une surface unique d’un modèle dans un grand nombre de triangles. Un nuanceur de coque est invoqué une fois par patch et transforme les points de contrôle d’entrée qui définissent une surface d’ordre bas en points de contrôle constituant un patch. Ce nuanceur effectue également certains calculs par patch afin de fournir des données pour l’étape du Paveur (TS) et l’étape du nuanceur de domaine (DS). | | Entrée | Entre 1 et les points de contrôle d’entrée 32, qui définissent conjointement une surface d’ordre bas. | | Sortie | Entre 1 et 32 points de contrôle de sortie qui composent un patch. Le nuanceur de coque déclare l’état de l’étape du Paveur (TS), y compris le nombre de points de contrôle, le type de face de patch et le type de partitionnement à utiliser lors du pavage. |

## <a name="tessellator-stage"></a>Étape du paveur

|-|-| | Objectif | L' [étape du Paveur (TS)](tessellator-stage--ts-.md) crée un modèle d’échantillonnage du domaine qui représente le patch de géométrie et génère un ensemble d’objets plus petits (triangles, points ou lignes) qui connectent ces échantillons. | | Entrée | Le paveur opère une fois par patch à l’aide de facteurs de pavage (qui spécifient la manière dont le raffinement du domaine être) et le type de partitionnement (qui spécifie l’algorithme utilisé pour découper un patch) qui sont transmis par l’étape du nuanceur de coque. | | Sortie | Le paveur sort aux coordonnées uv (et éventuellement w) coordonnées et la topologie de surface à l’étape du nuanceur de domaine. |

## <a name="domain-shader-stage"></a>Étape du nuanceur de domaine

|-|-| | Objectif | L' [étape du nuanceur de domaine (DS)](domain-shader-stage--ds-.md) calcule la position du vertex d’un point subdivisé dans le patch de sortie; Il calcule la position du vertex correspondant à chaque échantillon de domaine. Un nuanceur de domaine est exécuté une fois par étape du paveur point de sortie de l’étape a un accès en lecture seule au nuanceur de coque patch de sortie et constantes de patch de sortie et aux coordonnées UV de sortie de l’étape tessellator. | | Entrée | Un nuanceur de domaine consomme les points de contrôle de sortie de l' [étape du nuanceur de coque (HS)](hull-shader-stage--hs-.md). Les sorties du nuanceur de coque comprennent les éléments suivants: points de contrôle, données de constante de patch et facteurs de pavage (les facteurs de pavage peuvent inclure les valeurs utilisées par le paveur à fonction fixe et les valeurs brutes - avant l’arrondissement sous forme d’entier par le pavage - ce qui facilite notamment la géomorphose). Un nuanceur de domaine est invoqué une fois par coordonnée de sortie de l' [étape du Paveur (TS)](tessellator-stage--ts-.md). | | Sortie | L’étape du nuanceur de domaine (DS) produit en sortie la position du vertex d’un point subdivisé dans le patch de sortie. |

## <a name="geometry-shader-stage"></a>Étape du nuanceur de géométrie

|-|-| | Objectif | L' [étape du nuanceur de géométrie (GS)](geometry-shader-stage--gs-.md) traite les primitives complètes: triangles, lignes et points, ainsi que leurs vertex adjacents. Cette étape prend en charge l’amplification et le filtrage de géométrie. Elle est utile pour les algorithmes tels que l’expansion de point-sprites, les systèmes de particules dynamiques, la génération de fourrure, la génération de volumes d’ombre, le rendu sous forme de cubemap en une seule passe, l’échange et la configuration de matériaux par primitive, y compris la génération de coordonnées barycentriques sous la forme de données de primitive pour permettre à un nuanceur de pixels d’effectuer une interpolation d’attributs personnalisés. | | Entrée | Contrairement aux nuanceurs de vertex, qui opèrent sur un seul vertex, entrées du nuanceur de géométrie correspondent aux vertex d’une primitive complète (trois vertex pour les triangles, deux vertex pour les lignes ou vertex unique pour les points). | | Sortie | L’étape du nuanceur de géométrie (GS) est capable de produire en sortie plusieurs vertex formant une seule topologie sélectionnée. Les topologies de sortie du nuanceur de géométrie disponibles sont <strong>tristrip</strong>, <strong>linestrip</strong> et <strong>pointlist</strong>. Le nombre de primitives émises peut varier librement au sein d’une invocation du nuanceur de géométrie, bien que le nombre maximal de vertex pouvant être émis doive être déclaré de manière statique. Les longueurs de bande émises à partir d’une invocation du nuanceur de géométrie peuvent être arbitraires, et d’autres bandes peuvent être créés via la fonction HLSL [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660) . |

## <a name="stream-output-stage"></a>Étape de sortie de flux

|-|-| | Objectif | En permanence l' [étape de sortie de flux (SO)](stream-output-stage--so-.md) sort (ou diffuse) des données de vertex de l’étape active précédente à un ou plusieurs mémoires tampon en mémoire. Les données diffusées dans la mémoire peuvent être renvoyées dans le pipeline en tant que données d’entrée ou lues par l’UC. | | Entrée | Les données de vertex à partir d’une étape précédente du pipeline. | | Sortie | En permanence l’étape de sortie de flux (SO) sort (ou diffuse) des données de vertex de l’étape active précédente, telle que l’étape du nuanceur de géométrie (GS), à un ou plusieurs mémoires tampon en mémoire. Si l’étape du nuanceur de géométrie (GS) est inactif et que l’étape de sortie de flux (SO) est actif, il diffuse en continu les données de vertex de l’étape du nuanceur de domaine (DS) dans les mémoires tampon en mémoire (ou si le service d’annuaire est également inactive, à partir de l’étape Vertex Shader (VS)). |

## <a name="rasterizer-stage"></a>Étape du rastériseur

|-|-| | Objectif | [L’étape du rastériseur (RS)](rasterizer-stage--rs-.md) extrait les primitives qui ne sont pas dans la vue, les prépare pour l’étape du nuanceur de pixels (PS) et détermine le mode d’invocation des nuanceurs de pixels. Convertit informations de vecteur (composées de formes ou de primitives) en une image raster (constituée de pixels) afin d’afficher des graphiques 3D en temps réel. | | Entrée | Sommets (x, y, z, w) introduits dans le module de rastérisation de scène sont supposés se pour trouver dans l’espace de détourage homogène. Dans cet espace de coordonnées l’axe X pointe vers la droite, Y pointe vers le haut et Z pointe à la caméra. | | Sortie | Pixels réels qui doivent être rendus. Inclut certains attributs de vertex pour une utilisation dans l’interpolation par le nuanceur de pixels. |

## <a name="pixel-shader-stage"></a>Étape du nuanceur de pixels
 
|-|-| | Objectif | L' [étape du nuanceur de pixels (PS)](pixel-shader-stage--ps-.md) reçoit les données interpolées pour une primitive et génère des données par pixel telles que la couleur. | | Entrée | Lorsque le pipeline est configuré sans nuanceur de géométrie, un nuanceur de pixels est limité à 16 entrées 32 bits de 4 composants. Dans le cas contraire, un nuanceur de pixels peut prendre en charge jusqu’à 32entrées 32bits de 4composants. Les données d’entrée du nuanceur de pixels comprennent les attributs de vertex (qui peuvent être interpolés avec ou sans correction de la perspective) ou peuvent être traitées en tant que constantes par primitive. Les entrées du nuanceur de pixels sont interpolées à partir des attributs de vertex de la primitive faisant l’objet de la rastérisation, en fonction du mode d’interpolation déclaré. Si une primitive est découpée avant la rastérisation, le mode d’interpolation est également respecté pendant le processus de découpage. | | Sortie | Un nuanceur de pixels peut sortir jusqu'à 8, 32 bits, les couleurs de 4 composants, ou aucune couleur si le pixel est ignoré. Composants de Registre de sortie de nuanceur de pixels doivent être déclarées avant de pouvoir être utilisés; un masque d’écriture de sortie distinct est autorisé pour chaque registre. |

## <a name="output-merger-stage"></a>Étape de fusion de sortie
 
|-|-| | Objectif | L' [étape de fusion / sortie (OM)](output-merger-stage--om-.md) combine plusieurs types de données de sortie (valeurs du nuanceur de pixels, informations de profondeur et gabarit) avec le contenu de la cible de rendu de profondeur/gabarit tampons pour générer le résultat final du pipeline. | | Entrée | Les entrées de fusion / sortie sont l’état du Pipeline, les données de pixel générées par les nuanceurs de pixels, le contenu des cibles de rendu et le contenu des mémoires tampons de profondeur/gabarit. | | Sortie | Couleur de pixel finale du rendu. |

## <a name="related-topics"></a>Articles connexes

- [Guide d’apprentissage des graphismes Direct3D](index.md)

- [Pipeline de calcul](compute-pipeline.md)
