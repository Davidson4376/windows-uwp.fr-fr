---
title: Étape Vertex Shader (VS)
description: L'étape du nuanceur de vertex (VS) traite les vertex, notamment en effectuant des opérations telles que des transformations, ainsi que l'application d'une apparence et d'un éclairage. Un nuanceur de vertex prend un vertex d’entrée spécifique et produit un seul vertex de sortie.
ms.assetid: 5133C4BB-B4E6-4697-9276-F718AD44869C
keywords:
- Étape Vertex Shader (VS)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d6b9c67220c282ef1677559d586013c14366967a
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5813837"
---
# <a name="vertex-shader-vs-stage"></a>Étape Vertex Shader (VS)


L'étape du nuanceur de vertex (VS) traite les vertex, notamment en effectuant des opérations telles que des transformations, ainsi que l'application d'une apparence et d'un éclairage. Un nuanceur de vertex prend un vertex d’entrée spécifique et produit un seul vertex de sortie.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Objectif et cas d'utilisation


L’étape Vertex Shader (VS) est utilisée pour des traitements par vertex individuel:

-   Transformations
-   Application d’apparence
-   Morphose
-   Éclairage par vertex

L’étape Vertex Shader est une étape de nuanceur programmable; elle s’affiche sous la forme d'un bloc arrondi dans le diagramme du [pipeline graphique](graphics-pipeline.md). Cette étape du nuanceur utilise le modèle de nuanceur4.0 [nuanceur common core](https://msdn.microsoft.com/library/windows/desktop/bb509580).

L’étape Vertex Shader (VS) traite les sommets à partir de l’assembleur d’entrée. Les nuanceurs de vertex opèrent toujours sur un seul vertex d’entrée et produisent un seul vertex de sortie. L’étape du nuanceur de vertex doit toujours être active pour l'exécution du pipeline. Si aucune modification de vertex ni de transformation n’est nécessaire, un nuanceur de vertex direct doit être créé et défini sur le pipeline.

Chaque vertex d’entrée d'un nuanceur de vertex peut comprendre jusqu'à 16vecteurs de 32bits (d'un maximum de 4composants chacun). Chaque vertex de sortie peut également comprendre jusqu'à 16vecteurs de 32bits à 4composants. Tous les nuanceurs de vertex doivent avoir au moins une entrée et une sortie, qui peuvent se réduire à une valeur scalaire.

L’étape du nuanceur de vertex peut utiliser les deux valeurs générées par le système à partir de l’assembleur d’entrée: VertexID et InstanceID (voir Valeurs système et sémantiques). Dans la mesure où VertexID et InstanceID sont tous deux significatifs au niveau d'un vertex, et comme les ID générés par le matériel ne peuvent être ajoutés qu'à la première étape qui les comprend, ces valeurs d’ID peuvent être ajoutées uniquement à l’étape du nuanceur de vertex.

Les nuanceurs de vertex sont toujours exécutés sur tous les sommets, y compris les sommets adjacents dans des topologies de primitive d’entrée avec voisinage. Le nombre d'exécution du nuanceur de vertex peut être interrogé à partir de l’UC à l’aide de la statistique de pipeline VSInvocations.

Un nuanceur de vertex peut exécuter des opérations de chargement et d'échantillonnage de texture lorsque des dérivés d'espace à l'écran ne sont pas requis (à l’aide des fonctions intrinsèques HLSL: [Sample (objet de Texture HLSL DirectX)](https://msdn.microsoft.com/library/windows/desktop/bb509695), [SampleCmpLevelZero (objet de Texture HLSL DirectX)](https://msdn.microsoft.com/library/windows/desktop/bb509697), et [SampleGrad (objet de Texture HLSL DirectX)](https://msdn.microsoft.com/library/windows/desktop/bb509698)).

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrée


Vertex unique, avec des valeurs VertexID et InstanceID générées par le système. Chaque vertex d’entrée d'un nuanceur de vertex peut comprendre jusqu'à 16vecteurs de 32bits (d'un maximum de 4composants chacun).

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Sortie


Vertex unique. Chaque vertex de sortie peut également comprendre jusqu'à 16vecteurs de 32bits à 4composants.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Pipeline graphique](graphics-pipeline.md)

 

 




