---
title: Concepts de texture de base
description: Les premières images 3D générées par ordinateur, bien qu'assez sophistiquées pour leur époque, avaient tendance à présenter un aspect plastique brillant.
ms.assetid: 3CA3905D-E837-48EB-A81F-319AA1C6537E
keywords:
- Concepts de texture de base
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2124c9e620694f62414c865e6d3247cd25ce70d0
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8881492"
---
# <a name="basic-texturing-concepts"></a>Concepts de texture de base


Les premières images 3D générées par ordinateur, bien qu’assez sophistiquées pour leur époque, avaient tendance à présenter un aspect plastique brillant. Il leur manquait les marques (rayures, fissures, empreintes digitales et traînées) qui donnent aux objets 3D une complexité visuelle réaliste. Les textures sont connues pour améliorer le réalisme des images 3D générées par ordinateur.

Dans son utilisation quotidienne, le mot «texture» fait référence à l'aspect lisse ou rugueux d’un objet. Dans le domaine du graphisme informatique, une texture désigne une image bitmap de couleurs de pixels qui donnent à un objet l’apparence d'une texture.

Dans la mesure où les textures Direct3D sont des images bitmap, n’importe quelle image bitmap peut être appliquée à une primitive Direct3D. Par exemple, les applications peuvent créer et manipuler des objets qui semblent avoir un motif de grain de bois. Des effets d'herbe, de poussière et de roches peuvent être appliqués à un ensemble de primitives 3D qui forment une colline. On obtient ainsi un flanc de colline à l'aspect réaliste. Vous pouvez également utiliser la texture pour créer d'autres effets, par exemple des panneaux le long d’une route, des couches rocheuses sur une falaise ou l’apparence d'un marbre sur un sol.

Direct3D prend également en charge des techniques de texture plus avancées, telles que la fusion de textures (avec ou sans transparence) et le mappage lumineux. Voir [Fusion de textures](texture-blending.md) et [Mappage lumineux avec textures](light-mapping-with-textures.md).

Si votre application crée un périphérique de couche d’abstraction matérielle ou un périphérique logiciel, elle peut utiliser des textures 8, 16, 24, 32, 64 ou 128bits.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Textures](textures.md)

 

 




