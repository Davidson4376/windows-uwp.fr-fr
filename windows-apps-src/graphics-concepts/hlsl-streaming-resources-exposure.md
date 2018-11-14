---
title: Exposition des ressources de diffusion en continu HLSL
description: La prise en charge des ressources de diffusion en continu dans Shader Model5 requiert une syntaxe Microsoft HLSL (High Level Shader Language, langage de nuanceur de haut niveau) spécifique.
ms.assetid: 00A40D82-0565-43DC-82AB-0675B7E772E3
keywords:
- Exposition des ressources de diffusion en continu HLSL
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a8523f4895c541ffb3b92ee00d5b62c57343ae00
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6664365"
---
# <a name="hlsl-streaming-resources-exposure"></a>Exposition des ressources de diffusion en continu HLSL


La prise en charge des ressources de diffusion en continu dans [Shader Model5](https://msdn.microsoft.com/library/windows/desktop/ff471356) requiert une syntaxe Microsoft HLSL (High Level Shader Language, langage de nuanceur de haut niveau) spécifique.

La syntaxe HLSL pour Shader Model5 est uniquement autorisée sur les périphériques incluant une prise en charge des ressources de diffusion en continu. Toutes les méthodes HLSL pertinentes pour les ressources de diffusion en continu qui sont répertoriées dans le tableau ci-dessous acceptent un ou deux paramètres facultatifs supplémentaires(feedback, ou clamp et feedback dans cet ordre). Voici un exemple de méthode **Sample**:

**Sample(sampler, location \[, offset \[, clamp \[, feedback\] \] \])**

[**Texture2D.Sample(S,float,int,float,uint)**](https://msdn.microsoft.com/library/windows/desktop/dn393787) constitue un exemple de méthode **Sample**.

Les paramètres offset, clamp et feedback sont facultatifs. Vous devez spécifier tous les paramètres facultatifs jusqu’à celui dont vous avez besoin, ce qui est cohérent avec les règles C++ relatives aux arguments de fonction par défaut. Par exemple, si vous souhaitez obtenir l’état des commentaires (feedback), vous devez spécifier explicitement les paramètres offset et clamp dans la méthode **Sample**, même si ces paramètres ne sont pas nécessairement requis d’un point de vue logique.

Le paramètre clamp est une valeur flottante scalaire. La valeur littérale clamp=0.0f indique que l’opération de limitation (clamp) n’est pas effectuée.

Le paramètre feedback est une variable **uint** que vous pouvez fournir à la fonction intrinsèque [**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) demandant un accès à la mémoire. Vous ne devez pas modifier ni interpréter la valeur du paramètre feedback; toutefois, le compilateur ne fournit pas d’analyse ni de diagnostics avancés destinés à détecter si vous avez modifié cette valeur.

La syntaxe de la fonction [**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) est la suivante:

**bool CheckAccessFullyMapped(in uint FeedbackVar);**

La fonction [**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) interprète la valeur de *FeedbackVar* et renvoie la valeur true si toutes les données sur lesquelles porte l’accès ont été mappées dans la ressource; dans le cas contraire, **CheckAccessFullyMapped** renvoie la valeur false.

Si les paramètres clamp ou feedback sont spécifiés, le compilateur émet une variante de l’instruction de base. Par exemple, un exemple de ressource de diffusion en continu génère l’instruction `sample_cl_s`.

Si ni le paramètre clamp ni le paramètre feedback ne sont spécifiés, le compilateur émet l’instruction de base, de sorte qu’aucune modification n’est apportée au comportement actuel.

La définition du paramètre clamp sur la valeur 0.0f indique qu’aucune limitation n’est effectuée; le compilateur du pilote peut donc adapter davantage l’instruction au matériel cible. Si le paramètre feedback correspond à un registre NULL dans une instruction, il n’est pas utilisé; le compilateur du pilote peut donc adapter davantage l’instruction à l’architecture cible.

Si le compilateur HLSL déduit que le paramètre clamp présente la valeur0.0f et que le paramètre feedback n’est pas utilisé, il émet l’instruction de base correspondante (par exemple, `sample` plutôt que `sample_cl_s`).

Si un accès aux ressources de diffusion en continu est constitué de plusieurs instructions de code d’octet, par exemple dans le cas de ressources structurées, le compilateur agrège les différentes valeurs feedback par le biais de l’opération OU afin de produire la valeur feedback finale. Vous ne voyez donc qu’une seule valeur feedback pour ce type d’accès complexe.

Voici le tableau récapitulatif des méthodes HLSL qui sont modifiées pour la prise en charge des paramètres feedback et/ou clamp. Toutes ces méthodes fonctionnent sur les ressources découpées en vignettes et sur les ressources autres que celles de diffusion en continu de toutes les dimensions. Les ressources autres que celles de diffusion en continu apparaissent toujours comme étant entièrement mappées.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><a href="https://msdn.microsoft.com/library/windows/desktop/ff471359">Objets HLSL</a> </th>
<th align="left">Méthodes intrinsèques avec l’option feedback (*) - option clamp également incluse</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[RW]Texture2D</p>
<p>[RW]Texture2DArray</p>
<p>TextureCUBE</p>
<p>TextureCUBEArray</p></td>
<td align="left"><p>Gather</p>
<p>GatherRed</p>
<p>GatherGreen</p>
<p>GatherBlue</p>
<p>GatherAlpha</p>
<p>GatherCmp</p>
<p>GatherCmpRed</p>
<p>GatherCmpGreen</p>
<p>GatherCmpBlue</p>
<p>GatherCmpAlpha</p></td>
</tr>
<tr class="even">
<td align="left"><p>[RW]Texture1D</p>
<p>[RW]Texture1DArray</p>
<p>[RW]Texture2D</p>
<p>[RW]Texture2DArray</p>
<p>[RW]Texture3D</p>
<p>TextureCUBE</p>
<p>TextureCUBEArray</p></td>
<td align="left"><p>Sample*</p>
<p>SampleBias*</p>
<p>SampleCmp*</p>
<p>SampleCmpLevelZero</p>
<p>SampleGrad*</p>
<p>SampleLevel</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[RW]Texture1D</p>
<p>[RW]Texture1DArray</p>
<p>[RW]Texture2D</p>
<p>Texture2DMS</p>
<p>[RW]Texture2DArray</p>
<p>Texture2DArrayMS</p>
<p>[RW]Texture3D</p>
<p>[RW]Buffer</p>
<p>[RW]ByteAddressBuffer</p>
<p>[RW]StructuredBuffer</p></td>
<td align="left">Load</td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Articles connexes


[Accès du pipeline aux ressources de diffusion en continu](pipeline-access-to-streaming-resources.md)

 

 




