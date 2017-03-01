---
title: "Mappages de lumière spéculaire"
description: "Lorsqu’ils sont éclairés par une source lumineuse, les objets brillants présentant des matériaux hautement réfléchissants reçoivent des surbrillances spéculaires."
ms.assetid: 9B3AC5EC-DDAA-4671-BC81-0E3C79D87A81
keywords:
- "Mappages de lumière spéculaire"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 25c5bafd10734439ca1fdf3451be2510525d06be
ms.lasthandoff: 02/07/2017

---

# <a name="specular-light-maps"></a>Mappages de lumière spéculaire


Lorsqu’ils sont éclairés par une source lumineuse, les objets brillants présentant des matériaux hautement réfléchissants reçoivent des surbrillances spéculaires. Parfois, il est possible d’obtenir des surbrillances plus précises en appliquant des mappages de lumière spéculaire sur les primitives, plutôt qu’en utilisant des surbrillances spéculaires produites par le module d’éclairage.

Pour effectuer un mappage de lumière spéculaire, ajoutez le mappage de lumière spéculaire à la texture de la primitive, puis modulez (multipliez le résultat par) le mappage de lumière RVB.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Mappage lumineux avec textures](light-mapping-with-textures.md)

 

 





