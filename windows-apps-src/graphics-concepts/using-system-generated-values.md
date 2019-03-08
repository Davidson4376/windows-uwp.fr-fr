---
title: Utilisation de valeurs générées par le système
description: Les valeurs générées par le système sont produites par l'étape d'assembleur d'entrée (reposant sur les sémantiques d'entrée fournies par l'utilisateur) afin d'accroître l'efficacité des opérations du nuanceur.
ms.assetid: C7CBA81D-68CA-4E9A-95E3-8185C280C843
keywords:
- Utilisation de valeurs générées par le système
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6efe7aa27721f519ba93052abf2d0e8189f58941
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622314"
---
# <a name="span-iddirect3dconceptsusingsystem-generatedvaluesspanusing-system-generated-values"></a><span id="direct3dconcepts.using_system-generated_values"></span>À l’aide des valeurs générées par le système


Les valeurs générées par le système sont produites par l'[étape d'assembleur d'entrée](input-assembler-stage--ia-.md) (reposant sur les [sémantiques](https://msdn.microsoft.com/library/windows/desktop/bb509647) d'entrée fournies par l'utilisateur) afin d'accroître l'efficacité des opérations du nuanceur. L’association des données, comme un ID d’instance (visible par l’[étape du nuanceur de vertex](vertex-shader-stage--vs-.md)), un ID de vertex (visible par le nuanceur de vertex) ou un ID de primitive (visible par l’[étape du nuanceur de vertex](geometry-shader-stage--gs-.md)/[du nuanceur de pixel](pixel-shader-stage--ps-.md)) permet à une étape ultérieure de nuanceur de rechercher ces valeurs système afin d’optimiser son traitement.

Par exemple, l’étape de Visual Studio peut rechercher l’ID d’instance pour récupérer les données de vertex supplémentaires pour le nuanceur ou pour effectuer d’autres opérations ; les étapes GS et PS peuvent utiliser l’ID de primitive pour récupérer des données par primitive de la même manière.

## <a name="span-idvertexidspanspan-idvertexidspanspan-idvertexidspanvertexid"></a><span id="VertexID"></span><span id="vertexid"></span><span id="VERTEXID"></span>VertexID


Un ID de vertex est utilisé par chaque étape du nuanceur pour identifier chaque vertex. Il s'agit d'un entier non signé 32 bits dont la valeur par défaut est 0. Il est attribué à un vertex lorsque la primitive est traitée par l'[étape d’assembleur d’entrée (IA)](input-assembler-stage--ia-.md). Attachez la sémantique d'ID de vertex à la déclaration d’entrée du nuanceur pour informer l’étape IA qu'il faut générer un ID de vertex.

L’IA ajoute un ID de à chaque vertex pour une utilisation par les étapes de nuanceur. Pour chaque appel de dessin, l’ID de vertex est incrémenté de 1. Dans les appels de dessin indexés, le nombre rétablit la valeur de départ. Si l’ID de vertex est dépassé (supérieur à 2³² – 1), il se réinitialise à 0.

Pour tous les types de primitives, les sommets ont un ID de vertex associé (indépendant du voisinage).

## <a name="span-idprimitiveidspanspan-idprimitiveidspanspan-idprimitiveidspanprimitiveid"></a><span id="PrimitiveID"></span><span id="primitiveid"></span><span id="PRIMITIVEID"></span>PrimitiveID


Un ID de primitive est utilisé par chaque étape du nuanceur pour identifier chaque primitive. Il s'agit d'un entier non signé 32 bits dont la valeur par défaut est 0. Il est attribué à une primitive lorsque la primitive est traitée par l'[étape d’assembleur d’entrée (IA)](input-assembler-stage--ia-.md). Pour informer l’étape IA qu'il faut générer un ID de primitive, attachez la sémantique d'ID de primitive à la déclaration d’entrée du nuanceur.

L’étape IA ajoute un ID de primitive à chaque primitive pour une utilisation par l'[étape Geometry Shader (GS)](geometry-shader-stage--gs-.md) ou l'[étape Vertex Shader (VS)](vertex-shader-stage--vs-.md) (selon laquelle est la première active après l’étape IA). Pour chaque appel de dessin indexé, l’ID de primitive est incrémenté de 1, cependant, l’ID de primitive se réinitialise à 0 à chaque fois qu'une nouvelle instance commence. Tous les autres appels de dessin ne modifient pas la valeur de l’ID d’instance. Si l’ID d'instance est dépassé (supérieur à 2³² – 1), il se réinitialise à 0.

L'[étape Pixel Shader (PS)](pixel-shader-stage--ps-.md) ne dispose pas d'entrée distincte pour un ID de primitive ; toutefois, toute entrée de nuanceur de pixels qui spécifie qu'un ID de primitive utilise un mode d’interpolation constante.

Il n’existe aucune prise en charge pour générer automatiquement un ID de primitive pour les primitives adjacentes. Pour les types de primitives avec voisinage (par exemple, une bande de triangles avec voisinage), un ID de primitive est conservé uniquement pour les primitives intérieures (les primitives non adjacentes), comme pour un ensemble des primitives dans une bande de triangles sans voisinage.

## <a name="span-idinstanceidspanspan-idinstanceidspanspan-idinstanceidspaninstanceid"></a><span id="InstanceID"></span><span id="instanceid"></span><span id="INSTANCEID"></span>InstanceID


Un ID d’instance est utilisé par chaque étape du nuanceur pour identifier l’instance de la géométrie en cours de traitement. Il s'agit d'un entier non signé 32 bits dont la valeur par défaut est 0.

L'[étape d’assembleur d’entrée (IA)](input-assembler-stage--ia-.md) ajoute un ID d’instance à chaque vertex si la déclaration d’entrée de nuanceur de vertex inclut la sémantique d’ID d’instance. Pour chaque appel de dessin indexé, l'ID d’instance est incrémenté de 1. Tous les autres appels de dessin ne modifient pas la valeur de l’ID d’instance. Si l’ID d'instance est dépassé (supérieur à 2³² – 1), il se réinitialise à 0.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemple


L’illustration suivante montre comment les valeurs système sont attachées à une bande de triangles instanciée dans l'[étape d’assembleur d’entrée (IA)](input-assembler-stage--ia-.md).

![Illustration des valeurs système pour une bande de triangles instanciée](images/d3d10-ia-example.png)

Ces tableaux indiquent les valeurs système générées pour les deux instances de la même bande de triangles. La première instance (instance U) est affichée en bleu et la deuxième instance (instance V) en vert. Les lignes pleines relient les sommets dans les primitives, les lignes en pointillés relient les sommets adjacents.

Les tableaux suivants indiquent les valeurs générées par le système pour l’instance U.

| Données de vertex    | C,U | D,U | E,U | F,U | G,U | H,U | I,U | J,U | K,U | L,U |
|----------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| **VertexID**   | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   |
| **InstanceID** | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |

 

L'instance de bande de triangles U a 3 primitives de triangle, avec les valeurs générées par le système suivantes :

|                 |     |     |     |
|-----------------|-----|-----|-----|
| **PrimitiveID** | 0   | 1   | 2   |
| **InstanceID**  | 0   | 0   | 0   |

 

Les tableaux suivants indiquent les valeurs générées par le système pour l’instance V.

| Données de vertex    | C,V | D,V | E,V | F,V | G,V | H,V | I,V | J,V | K,V | L,V |
|----------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| **VertexID**   | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   |
| **InstanceID** | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   |

 

L'instance de bande de triangles V a 3 primitives de triangle, avec les valeurs générées par le système suivantes :

|                 |     |     |     |
|-----------------|-----|-----|-----|
| **PrimitiveID** | 0   | 1   | 2   |
| **InstanceID**  | 1   | 1   | 1   |

 

L'[étape d’assembleur d’entrée (IA)](input-assembler-stage--ia-.md) génère les ID (vertex, primitive et instance). Notez également que chaque instance est attribuée à un ID d’instance unique. Les données se terminent par la bande-couper, qui sépare chaque instance de la bande de triangles.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Étape de l’assembleur d’entrée (IA)](input-assembler-stage--ia-.md)

 

 




