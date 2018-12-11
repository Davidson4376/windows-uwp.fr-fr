---
title: Étape Tessellator (TS)
description: L'étape du paveur (TS, Tessellator) crée un modèle d'échantillonnage du domaine qui représente le patch de géométrie et génère un ensemble d'objets plus petits (triangles, points ou lignes) qui connectent ces échantillons.
ms.assetid: 2F006F3D-5A04-4B3F-8BC7-55567EFCFA6C
keywords:
- Étape Tessellator (TS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7768d63405281d3155affc6c9f09c62568761718
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8879788"
---
# <a name="tessellator-ts-stage"></a>Étape Tessellator (TS)


L'étape du paveur (TS, Tessellator) crée un modèle d'échantillonnage du domaine qui représente le patch de géométrie et génère un ensemble d'objets plus petits (triangles, points ou lignes) qui connectent ces échantillons.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Objectif et cas d'utilisation


Le diagramme suivant présente les étapes du pipeline graphique Direct3D.

![diagramme du pipeline 11 direct3d qui présente les étapes du nuanceur de coque, du tessellator et du nuanceur de domaine.](images/d3d11-pipeline-stages-tessellation.png)

Le diagramme suivant présente la progression à travers les étapes de pavage.

![diagramme de progression du pavage](images/tess-prog.png)

La progression commence par la surface de subdivision faiblement détaillée. La progression présente ensuite le patch d’entrée avec le patch de géométrie correspondant, des échantillons de domaine et les triangles qui relient ces échantillons. Enfin, la progression présente les vertex qui correspondent à ces échantillons.

Le runtime Direct3D prend en charge les trois étapes qui implémentent le pavage, qui permet de convertir les surfaces de subdivision faiblement détaillées en primitives plus détaillées sur le GPU. Le pavage permet de décomposer les surfaces d'ordre plus élevé en structures adaptée au rendu.

Les étapes de pavage fonctionnent de concert pour convertir des surfaces d'ordre plus élevé (qui garantissent la simplicité et l'efficacité du modèle) en un grand nombre de triangles pour un rendu détaillé dans le pipeline graphique Direct3D.

Le pavage utilise le GPU pour calculer une surface plus détaillée à partir d’une surface créée à partir de patchs quad, de patchs de triangles ou d'isolignes. Pour estimer la surface d'ordre plus élevé, chaque patch est divisé en triangles, points ou des lignes à l’aide de facteurs de pavage. Le pipeline graphique Direct3D implémente le pavage à l’aide de trois étapes de pipeline:

-   [Étape du nuanceur de coque (Hull_shadr, HS)](hull-shader-stage--hs-.md) -une étape de nuanceur programmable qui produit un patch de la géométrie (et les constantes de patch) qui correspondent à chaque entrée de patch (quad, triangle ou ligne).
-   Étape du paveur (TS, Tessellator) - Une étape de pipeline à fonctions fixes qui crée un modèle d'échantillonnage du domaine qui représente le patch de géométrie et génère un ensemble d'objets plus petits (triangles, points ou lignes) qui connectent ces échantillons.
-   [Étape du nuanceur de domaine (DS)](domain-shader-stage--ds-.md) -une étape de nuanceur programmable qui calcule la position de vertex qui correspond à chaque échantillon de domaine.

En implémentant le pavage dans le matériel, un pipeline graphique peut évaluer des modèles moins détaillés (nombre de polygones inférieur) inférieurs et générer un rendu plus détaillé. Lorsqu'il est possible de procéder au pavage, le pavage implémenté par le matériel peut générer un volume incroyable de détail visuel (y compris la prise en charge pour le mappage de déplacement) sans ajouter le détail visuel pour les tailles de modèle, ni paralyser les fréquences d’actualisation.

Avantages du pavage:

-   Le pavage enregistre une grande quantité de mémoire et de bande passante, ce qui permet à une application de rendre des surfaces plus détaillées à partir de modèles basse résolution. La technique de pavage mise en place dans le pipeline graphique Direct3D prend également en charge le mappage de déplacement, ce qui peut produire des détails de surface extraordinairement détaillés.
-   Le pavage prend en charge les techniques de rendu évolutif, comme les niveaux de détail continus ou dépendants de la vue qui peuvent être calculés à la volée.
-   Le pavage améliore les performances en effectuant des calculs coûteux à une fréquence inférieure (effectuer des calculs sur un modèle moins détaillé). Cela peut inclure les calculs de fusions à l’aide de formes fusionnées ou de cibles de morphing pour une animation réaliste ou des calculs physiques pour la détection des collisions ou les dynamiques Soft Body.

Le pipeline graphique Direct3D implémente le pavage en fonction du matériel, qui transfère le travail de l’unité centrale vers le GPU. Cela peut contribuer à améliorer largement les performances si une application met en œuvre un grand nombre de cibles de morphing et/ou des modèles d'apparence/de déformation plus sophistiqués.

Le tessellator est une étape à fonctions fixes initialisée en liant un [nuanceur de coque](hull-shader-stage--hs-.md) au pipeline. (voir [Procédure: Initialiser l’étape Tessellator](https://msdn.microsoft.com/library/windows/desktop/ff476341)). L’objectif de l’étape tessellator consiste à subdiviser un domaine (quad, tri ou ligne) dans un grand nombre d’objets plus petit (triangles, points ou lignes). Le tessellator permet de décomposer un domaine canonique est un système coordonné normalisé (de zéro à un). Par exemple, un domaine quad est fractionné en carré-unité.

### <a name="span-idphasesinthetessellatortsstagespanspan-idphasesinthetessellatortsstagespanspan-idphasesinthetessellatortsstagespanphases-in-the-tessellator-ts-stage"></a><span id="Phases_in_the_Tessellator__TS__stage"></span><span id="phases_in_the_tessellator__ts__stage"></span><span id="PHASES_IN_THE_TESSELLATOR__TS__STAGE"></span>Phases de l’étape Tessellator (TS)

L’étape Tessellator (TS) se compose de deux phases:

-   La première phase traite les facteurs de pavage, résout les problèmes d'arrondis, gère les tout petits facteurs, réduit et combine les facteurs à l'aide d'opérations arithmétiques à virgule flottante 32bits.
-   La deuxième phase génère des listes de points ou de topologie basées sur le type de partitionnement sélectionné. Il s'agit de la tâche principale de l'étape tessellator. Elle utilise des fractions 16bits avec des opérations arithmétiques à virgule fixe. Les opérations arithmétiques à virgule fixe permettent une accélération matérielle tout en conservant une précision acceptable. Par exemple, avec un patch large de 64mètres, cette précision peut placer les points à une résolution de 2mm.

    | Type de partitionnement | Plage                       |
    |----------------------|-----------------------------|
    | Fractional\_odd      | \[1...63\]                  |
    | Fractional\_even     | Plage TessFactor: \[2..64\] |
    | Integer              | TessFactor range: \[1..64\] |
    | Pow2                 | Plage TessFactor: \[1..64\] |

     

Le pavage est implémenté avec les deux étapes de nuanceur programmable: un [nuanceur de coque](hull-shader-stage--hs-.md) et un [nuanceur de domaine](domain-shader-stage--ds-.md). Ces étapes de nuanceur sont programmées avec le code HLSL défini dans le modèle de nuanceur 5. Les cibles du nuanceur sont: hs\_5\_0 et ds\_5\_0. Le titre de crée le nuanceur, avant que le code pour le matériel soit extrait des nuanceurs compilés transmis dans le runtime lorsque les nuanceurs sont liés au pipeline.

### <a name="span-idenablingdisablingtessellationspanspan-idenablingdisablingtessellationspanspan-idenablingdisablingtessellationspanenablingdisabling-tessellation"></a><span id="Enabling_disabling_tessellation"></span><span id="enabling_disabling_tessellation"></span><span id="ENABLING_DISABLING_TESSELLATION"></span>Activation/désactivation du pavage

Activez le pavage en créant un nuanceur de coque et en l'associant à l’étape du nuanceur de coque (cela configure automatiquement l’étape tessellator). Pour générer les positions du vertex final à partir des correctifs fractionnés, vous devez également créer un [nuanceur de domaine](domain-shader-stage--ds-.md) et le lier à l’étape du nuanceur de domaine. Une fois que le pavage est activé, la données saisies lors de l'étape de l'assembleur d’entrée (IA) doivent être les données du patch. La topologie de l’assembleur d’entrée doit être une topologie constante de patch.

Pour désactiver le pavage, définissez le nuanceur de coque et le nuanceur de domaine sur **NULL**. L'[étape du nuanceur de géométrie (GS)](geometry-shader-stage--gs-.md) et l'[étape de sortie de flux (SO, Stream Output)](stream-output-stage--so-.md) ne peuvent pas lire des points de contrôle de sortie du nuanceur de coque ou les données du patch.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrée


Le paveur agit une fois par patch à l'aide des facteurs de pavage (qui spécifient le raffinement du pavage du domaine) et du type de partitionnement (qui spécifie l'algorithme utilisé pour découper un patch) qui sont transmis par l'étape du nuanceur de coque.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Sortie


Le paveur sort des coordonnées UV (et éventuellement W) et la topologie de surface à l'intention de l’étape du nuanceur de domaine.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Pipeline graphique](graphics-pipeline.md)

 

 




