---
title: Étape du nuanceur de coque (HS)
description: L’étape du nuanceur de coque (HS, Hull Shader) constitue l’une des étapes de pavage qui décomposent efficacement une surface unique d’un modèle en un grand nombre de triangles.
ms.assetid: C62F6F15-CAD7-4C72-9735-00762E346C4C
keywords:
- Étape du nuanceur de coque (HS)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d2aed18d476f966e644fa095aa6a5a518ebbe959
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6987454"
---
# <a name="hull-shader-hs-stage"></a>Étape du nuanceur de coque (HS)


L’étape du nuanceur de coque (HS, Hull Shader) constitue l’une des étapes de pavage qui décomposent efficacement une surface unique d’un modèle en un grand nombre de triangles. L’étape du nuanceur de coque (HS) génère un patch de géométrie (et des constantes de patch) qui correspondent à chaque patch d’entrée (quad, triangle ou ligne). Un nuanceur de coque est invoqué une fois par patch et transforme les points de contrôle d’entrée qui définissent une surface d’ordre bas en points de contrôle constituant un patch. Ce nuanceur effectue également certains calculs par patch afin de fournir des données pour l’[étape du paveur (TS)](tessellator-stage--ts-.md) et l’[étape du nuanceur de domaine (DS)](domain-shader-stage--ds-.md).

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Objectif et cas d’utilisation


![diagramme de l’étape du nuanceur de coque](images/d3d11-hull-shader.png)

Les trois étapes de pavage fonctionnent de concert pour convertir des surfaces d’ordre plus élevé (qui garantissent la simplicité et l’efficacité du modèle) en un grand nombre de triangles pour un rendu détaillé dans le pipeline graphique. Les étapes de pavage comprennent l’étape du nuanceur de coque (HS), l’[étape du paveur (TS)](tessellator-stage--ts-.md) et l’[étape du nuanceur de domaine (DS)](domain-shader-stage--ds-.md).

L’étape du nuanceur de coque (HS) est une étape de nuanceur programmable. Un nuanceur de coque est implémenté avec une fonction HLSL.

Un nuanceur de coque opère en deux phases, une phase de points de contrôle et une phase de calcul des constantes par patch, qui sont exécutées en parallèle par le matériel. Le compilateur HLSL extrait le parallélisme dans un nuanceur de coque et l’encode sous forme de bytecode qui pilote le matériel.

-   La phase des points de contrôle s’exécute une fois par point de contrôle en lisant les points de contrôle pour un patch et en générant un seul point de contrôle de sortie (identifié par un élément **ControlPointID**).
-   La phase de calcul des constantes par patch s’exécute une fois par patch pour générer des facteurs de pavage d’arête et d’autres constantes de patch. En interne, de nombreuses phases de calcul des constantes par patch peuvent s’exécuter simultanément. La phase de calcul des constantes par patch dispose d’un accès en lecture seule à tous les points de contrôle d’entrée et de sortie.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrée


Entre 1 et 32points de contrôle d’entrée, qui définissent conjointement une surface d’ordre bas.

-   Le nuanceur de coque déclare l’état requis par l’[étape du paveur (TS)](tessellator-stage--ts-.md). Cet état inclut des informations telles que le nombre de points de contrôle, le type de face de patch et le type de partitionnement à utiliser lors du pavage. Ces informations apparaissent sous forme de déclarations qui précèdent généralement le code du nuanceur.
-   Les facteurs de pavage déterminent la quantité de subdivision de chaque patch.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Sortie


Entre 1 et 32points de contrôle de sortie, qui composent un patch.

-   La sortie du nuanceur est comprise entre 1 et 32points de contrôle, quel que soit le nombre de facteurs de pavage. Les points de contrôle sortis par un nuanceur de coque peuvent être consommés par l’étape du nuanceur de domaine. Les données de constante de patch peuvent être consommées par un nuanceur de domaine. Les facteurs de pavage peuvent être consommés par l’[étape du paveur (TS)](tessellator-stage--ts-.md) et par l’[étape du nuanceur de domaine (DS)](domain-shader-stage--ds-.md).
-   Si le nuanceur de coque définit un facteur de pavage d’arête sur une valeur≤0 ou NaN (n’est pas un nombre), le patch sera éliminé (omis). En conséquence, l’étape du paveur pourra ou non s’exécuter, le nuanceur de domaine ne s’exécutera pas, et aucune sortie visible ne sera générée pour ce patch.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemple


```
[patchsize(12)]
[patchconstantfunc(MyPatchConstantFunc)]
MyOutPoint main(uint Id : SV_ControlPointID,
     InputPatch<MyInPoint, 12> InPts)
{
     MyOutPoint result;
     
     ...
     
     result = TransformControlPoint( InPts[Id] );

     return result;
}
```

Consultez l’article [Procédure: Création d’un nuanceur de coque](https://msdn.microsoft.com/library/windows/desktop/ff476338).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Articles connexes


[Pipeline graphique](graphics-pipeline.md)

 

 




