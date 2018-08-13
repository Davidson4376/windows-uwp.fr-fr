---
title: Pipeline graphique
description: Le pipeline graphiqueDirect3D est conçu pour la génération de graphismes destinés aux applications de jeu en temps réel. Les données progressent de l’entrée vers la sortie en transitant par chacune des étapes configurables ou programmables de ce pipeline.
ms.assetid: C9519AD0-5425-48BD-9FF4-AED8959CA4AD
keywords:
- Pipeline graphique
- Étapes du pipeline
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6cb50af5facdbf4271c9911f0e1f536dead0b8b8
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2018
ms.locfileid: "1044908"
---
# <a name="graphics-pipeline"></a>Pipeline graphique


Le pipeline graphiqueDirect3D est conçu pour la génération de graphismes destinés aux applications de jeu en temps réel. Les données progressent de l’entrée vers la sortie en transitant par chacune des étapes configurables ou programmables de ce pipeline.

Toutes les étapes sont configurables à l’aide de l’API Direct3D. Les étapes qui utilisent des noyaux de nuanceur communs (blocs rectangulaires arrondis) sont programmables au moyen du langage de programmation [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561). En conséquence, le pipeline offre un haut degré de flexibilité et d’adaptabilité.

Les étapes les plus couramment utilisées sont l’étape du nuanceur de vertex (VS) et l’étape du nuanceur de pixels (PS). Si vous ne fournissez même pas ces étapes de nuanceur, l’application utilise un nuanceur de vertex et un nuanceur de pixels directs sans opération par défaut.

![diagramme du flux de données dans le pipeline programmable direct3d11](images/d3d11-pipeline-stages.jpg)

## <a name="input-assembler-stage"></a>Étape de l’assembleur d’entrée

|-|-| | Objectif | Les blocs [d’entrée assembleur (IA) étape](input-assembler-stage--ia-.md) primitifs et les données de contiguïté dans le pipeline, tels que des triangles, lignes et points, y compris sémantique ID pour aider à améliorer l’efficacité des nuanceurs en réduisant le traitement primitives qui n’ont pas déjà été traités. | | Entrée | Données primitif (triangles, lignes et/ou points) à partir des tampons rempli par l’utilisateur. Données de contiguïté éventuelles. Un triangle serait 3 sommets pour chacun d’eux et éventuellement 3 sommets des données contiguïté par triangle. | | Sortie | Primitives de valeurs attachées généré par le système (par exemple un ID primitif, un ID d’instance ou un ID de sommet). |

## <a name="vertex-shader-stage"></a>Étape du nuanceur de vertex

|-|-| | Objectif | La [phase de sommet Shader (VS)](vertex-shader-stage--vs-.md) traite les sommets, généralement effectuer des opérations telles que des transformations, définir l’apparence et d’éclairage. Un nuanceur de vertex prend un vertex d’entrée spécifique et produit un seul vertex de sortie. Sommet individuels opérations, telles que des Transformations, oignon, Morphing et de l’éclairage par sommet. | | Entrée | Sommet unique, avec les valeurs VertexID et ID d’instance généré par le système. Chaque sommet d’entrée de sommet shader peut être constituée de jusqu'à 16 vecteurs 32 bits (composants jusqu'à 4 chaque). | | Sortie | Un sommet unique. Chaque sommet de sortie peut être constituée de 16 jusqu'à 32 bits 4-component vecteurs. |
 
## <a name="hull-shader-stage"></a>Étape du nuanceur de coque
 
|-|-| | Objectif | La [phase de coque Shader (SH)](hull-shader-stage--hs-.md) est une des étapes du fractionnement, lequel efficacement décomposer une surface unique d’un modèle dans nombreux triangles. Un nuanceur de coque est invoqué une fois par patch et transforme les points de contrôle d’entrée qui définissent une surface d’ordre bas en points de contrôle constituant un patch. Il effectue également certains calculs par correctif pour fournir des données pour la phase Tessellator (TS) et le stade de domaine Shader (DS). | | Entrée | Entre 1 et points de contrôle d’entrée 32, qui définissent une surface de poids faible. | | Sortie | Entre 1 et 32 points de contrôle de sortie, qui constituent un correctif. Le shader coque déclare l’état pour la phase Tessellator (TS), y compris le nombre de points de contrôle, le type de face de correctif et le type de partitionnement pour utiliser lorsqu’en mosaïque. |

## <a name="tessellator-stage"></a>Étape du paveur

|-|-| | Objectif | La [phase Tessellator (TS)](tessellator-stage--ts-.md) crée un modèle d’échantillonnage du domaine qui représente le correctif de géométrie et génère un jeu d’objets plus petits (triangles, points ou lignes) qui se connectent à ces exemples. | | Entrée | Le tessellator fonctionne une seule fois par un correctif à l’aide les facteurs de fractionnement (qui spécifient la le domaine sera fractionnée) et le type de partitionnement (qui spécifie l’algorithme utilisé pour un correctif de tranche) qui sont passés à partir de la fenêtre de partage coque-shader. | | Sortie | Le tessellator sorties uv (et éventuellement w) coordonnées et la topologie de surface d’exposition à la phase de domaine-shader. |

## <a name="domain-shader-stage"></a>Étape du nuanceur de domaine

|-|-| | Objectif | [phase du domaine Shader (DS)](domain-shader-stage--ds-.md) calcule la position du sommet d’un point divisé dans le correctif de sortie; Elle calcule la position de sommet qui correspond à chaque domaine exemple. Un shader domaine est exécuté une fois par tessellator point de sortie de phase a accès en lecture seule au shader coque correctif de sortie et de sortie de constantes de correctif et coordonnées UV de sortie de la fenêtre de partage tessellator. | | Entrée | Un shader domaine consomme des points de contrôle de sortie de la [phase de coque Shader (SH)](hull-shader-stage--hs-.md). Les sorties du nuanceur de coque comprennent les éléments suivants: points de contrôle, données de constante de patch et facteurs de pavage (les facteurs de pavage peuvent inclure les valeurs utilisées par le paveur à fonction fixe et les valeurs brutes - avant l’arrondissement sous forme d’entier par le pavage - ce qui facilite notamment la géomorphose). Un shader domaine est appelé une fois par les coordonnées de sortie de la [phase Tessellator (TS)](tessellator-stage--ts-.md). | | Sortie | L’étape de domaine Shader (DS) renvoie la position du sommet d’un point divisé dans le correctif de sortie. |

## <a name="geometry-shader-stage"></a>Étape du nuanceur de géométrie

|-|-| | Objectif | La [phase Geometry Shader (GS)](geometry-shader-stage--gs-.md) traite tous primitives: triangles, lignes et points, ainsi que leurs sommets adjacents. Cette étape prend en charge l’amplification et le filtrage de géométrie. Elle est utile pour les algorithmes tels que l’expansion de point-sprites, les systèmes de particules dynamiques, la génération de fourrure, la génération de volumes d’ombre, le rendu sous forme de cubemap en une seule passe, l’échange et la configuration de matériaux par primitive, y compris la génération de coordonnées barycentriques sous la forme de données de primitive pour permettre à un nuanceur de pixels d’effectuer une interpolation d’attributs personnalisés. | | Entrée | Contrairement aux nuanceurs, qui fonctionnent sur un seul point, entrées du shader géométrie sont les sommets pour primitif (trois sommets triangles, deux sommets des lignes ou sommet unique point) complète. | | Sortie | La fenêtre de géométrie Shader (GS) est susceptible de générer plusieurs sommets en une seule topologie sélectionnée. Les topologies de sortie du nuanceur de géométrie disponibles sont <strong>tristrip</strong>, <strong>linestrip</strong> et <strong>pointlist</strong>. Le nombre de primitives émises peut varier librement au sein d’une invocation du nuanceur de géométrie, bien que le nombre maximal de vertex pouvant être émis doive être déclaré de manière statique. Longueurs de bande émis à partir d’un appel de shader géométrie peuvent être arbitraires et nouvelles bandes peuvent être créés via la fonction HLSL [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660) . |

## <a name="stream-output-stage"></a>Étape de sortie de flux

|-|-| | Objectif | La [phase de flux de sortie (SO)](stream-output-stage--so-.md) en continu génère (ou flux) données sommet de la fenêtre active précédente pour un ou plusieurs tampons en mémoire. Données transmises à la mémoire peuvent être recyclées dans le pipeline en tant que données d’entrée ou lire à partir de l’UC. | | Entrée | Données de sommet d’une étape précédente du pipeline. | | Sortie | L’étape de flux sortie (SO) en continu génère (ou flux) données du sommet de la fenêtre active précédente, telles que la fenêtre de partage Geometry Shader (GS), à un ou plusieurs tampons en mémoire. Si la fenêtre de géométrie Shader (GS) est inactive et la fenêtre sortie Stream (SO) est active, en permanence sommet des données sont envoyées à partir de la fenêtre de partage domaine Shader (DS) dans les tampons mémoire (ou si le service d’annuaire est également inactive, à partir de la fenêtre de partage sommet Shader (VS)). |

## <a name="rasterizer-stage"></a>Étape du rastériseur

|-|-| | Objectif | Les primitives clips [stade du module de rastérisation (r)](rasterizer-stage--rs-.md) qui ne figurent pas dans la vue, prépare primitives pour l’étape de Pixel Shader (PS) et détermine comment appeler pixels. Convertit vectoriel (composée de formes ou de primitives) dans une image de trame (composée de pixels) en vue de l’affichage des graphiques 3D en temps réel. | | Entrée | Sommets (x, y, z, w) dans le module de rastérisation étape sont supposés pour être homogène clip-espace. Dans cet espace de coordonnées l’axe des X pointant vers la droite, à l’Y pointant vers le haut et le Z pointe à la caméra. | | Sortie | Pixels réels qui doivent être rendus. Inclut certains attributs de sommet pour une utilisation dans interpolation par Pixel Shader. |

## <a name="pixel-shader-stage"></a>Étape du nuanceur de pixels
 
|-|-| | Objectif | La [phase de Pixel Shader (PS)](pixel-shader-stage--ps-.md) reçoit les données interpolées pour un type primitif et génère des données par pixel comme couleur. | | Entrée | Lorsque le pipeline est configuré sans un shader geometry, un shader de pixel est limité à 16, entrées 32 bits, 4-composant. Dans le cas contraire, un nuanceur de pixels peut prendre en charge jusqu’à 32entrées 32bits de 4composants. Les données d’entrée du nuanceur de pixels comprennent les attributs de vertex (qui peuvent être interpolés avec ou sans correction de la perspective) ou peuvent être traitées en tant que constantes par primitive. Les entrées du nuanceur de pixels sont interpolées à partir des attributs de vertex de la primitive faisant l’objet de la rastérisation, en fonction du mode d’interpolation déclaré. Si une primitive est découpée avant la rastérisation, le mode d’interpolation est également respecté pendant le processus de découpage. | | Sortie | Un shader de pixel peut sortie jusqu'à 8, 32 bits, 4-component couleurs ou aucune couleur si le pixel est ignoré. Composants de Registre pixel shader sortie doivent être déclarés avant de pouvoir être utilisés; chaque historique est autorisée à un masque de sortie-write distinct. |

## <a name="output-merger-stage"></a>Étape de fusion de sortie
 
|-|-| | Objectif | La [phase de fusion de sortie (OM)](output-merger-stage--om-.md) combine les différents types de données de sortie (valeurs de pixel shader, les informations de profondeur et gabarit) avec le contenu des tampons de profondeur/gabarit et la cible de rendu pour générer le résultat final pipeline. | | Entrée | Les entrées de fusion de sortie sont l’état du Pipeline, les données de pixels générées par les nuanceurs, le contenu de la cible de rendu et le contenu des tampons profondeur/gabarit. | | Sortie | Couleur du pixel de rendu final. |

## <a name="related-topics"></a>Articles connexes

- [Guide d’apprentissage des graphismes Direct3D](index.md)

- [Pipeline de calcul](compute-pipeline.md)
