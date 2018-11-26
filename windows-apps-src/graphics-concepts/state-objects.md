---
title: Objets d’état
description: L'état de l’appareil est regroupé dans des objets d’état qui réduisent considérablement le coût des changements d’état. Il existe plusieurs objets d’état, et chacun d’eux est conçu pour initialiser un ensemble d’état pour une étape du pipeline spécifique. Les objets d’état varient selon la version de Direct3D.
ms.assetid: D998745C-2B75-4E59-9923-AD1A17A92E05
keywords:
- Objets d’état
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3437119979073a5cec27948fc90f954e06c2fc93
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7707120"
---
# <a name="state-objects"></a>Objets d’état


L'état de l’appareil est regroupé dans des objets d’état qui réduisent considérablement le coût des changements d’état. Il existe plusieurs objets d’état, et chacun d’eux est conçu pour initialiser un ensemble d’état pour une étape du pipeline spécifique. Les objets d’état varient selon la version de Direct3D.

## <a name="span-idinputlayoutspanspan-idinputlayoutspanspan-idinputlayoutspaninput-layout-state"></a><span id="Input_Layout"></span><span id="input_layout"></span><span id="INPUT_LAYOUT"></span>État du schéma d'entrée


Ce groupe d’état détermine comment l'[étape de l’assembleur d’entrée (IA)](input-assembler-stage--ia-.md) lit les mémoires tampons de données de l’entrée et l'assemble pour une utilisation par le nuanceur de vertex. Cela inclut l’état par exemple, le nombre d’éléments dans le tampon d’entrée et la signature des données d’entrée. L’étape d’assembleur d’entrée (IA) diffuse en continu les primitives de la mémoire vers le pipeline.

## <a name="span-idrasterizerspanspan-idrasterizerspanspan-idrasterizerspanrasterizer-state"></a><span id="Rasterizer"></span><span id="rasterizer"></span><span id="RASTERIZER"></span>État du rastériseur


Ce groupe de l’état initialise l'[étape Rastériseur (RS)](rasterizer-stage--rs-.md). Cet objet comprend l’état tels que les modes remplir ou éliminer, l’activation d’un rectangle symétrique pour le découpage et la définition des paramètres de l’échantillonnage multiple. Cette étape convertit les primitives en pixels, effectue des opérations comme le détourage et le mappage des primitives à la fenêtre d’affichage.

## <a name="span-iddepthstencilspanspan-iddepthstencilspanspan-iddepthstencilspandepth-stencil-state"></a><span id="DepthStencil"></span><span id="depthstencil"></span><span id="DEPTHSTENCIL"></span>État de profondeur-gabarit


Ce groupe d’états initialise la portion de profondeur-gabarit de l'[étape de fusion de sortie (OM)](output-merger-stage--om-.md). Plus spécifiquement, cet objet initialise le test de la profondeur et du gabarit.

## <a name="span-idblendspanspan-idblendspanspan-idblendspanblend-state"></a><span id="Blend"></span><span id="blend"></span><span id="BLEND"></span>État de fusion


Ce groupe de l’état initialise la portion de fusion de l'[étape de fusion de sortie (OM)](output-merger-stage--om-.md).

## <a name="span-idsamplerspanspan-idsamplerspanspan-idsamplerspansampler-state"></a><span id="Sampler"></span><span id="sampler"></span><span id="SAMPLER"></span>État de l’échantillonneur


Ce groupe d'états initialise un objet d’échantillonneur. Un objet d’échantillonneur est utilisé par les étapes du nuanceur pour filtrer les textures dans la mémoire.

Dans Direct3D, l’objet d’échantillonneur n’est pas lié à une texture spécifique, il décrit uniquement comment effectuer un filtrage à partir de n'importe quelle ressource.

## <a name="span-idperformanceconsiderationsspanspan-idperformanceconsiderationsspanspan-idperformanceconsiderationsspanperformance-considerations"></a><span id="Performance_Considerations"></span><span id="performance_considerations"></span><span id="PERFORMANCE_CONSIDERATIONS"></span>Considérations sur les performances


Le fait de concevoir l’API de manière à utiliser des objets d’état offre plusieurs avantages en termes de performances. Ceux-ci incluent la validation d’état au moment de la création d’objet, l’activation de la mise en cache des objets d’état dans le matériel et la réduction considérable de la quantité d’état transmise au cours d’un appel d’API de définition d'état (en passant les informations handle à l’objet d’état plutôt qu'à l’état).

Pour réussir à améliorer ces performances, vous devez créer vos objets d'état lorsque votre application démarre, bien avant votre boucle de rendu. Les objets d’état sont immuables, c'est-à-dire que lorsqu'ils sont créés, vous ne pouvez pas les modifier. À la place, vous devez les détruire et les recréer.

Vous pouvez créer plusieurs objets d’échantillonneur avec différentes combinaisons échantillonneur-état. La modification de l’état de l'échantillonneur s’effectue ensuite en appelant l’API «Set» appropriée qui permet de passer les informations handle à l'objet (par opposition à l’état de l’échantillonneur). Cela réduit considérablement la quantité de charge lors de chaque image de rendu de changement d’état dans la mesure où le nombre d’appels et la quantité de données sont largement réduits.

Sinon, vous pouvez choisir d’utiliser le système d’effet qui gère automatiquement la création et la destruction efficaces des objets d’état de votre application.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Pipeline graphique](graphics-pipeline.md)

[Affichages](views.md)

 

 




