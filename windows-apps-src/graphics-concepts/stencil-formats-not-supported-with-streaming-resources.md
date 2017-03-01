---
title: Formats de gabarit non pris en charge par les ressources de diffusion en continu
description: Les formats contenant un gabarit ne sont pas pris en charge par les ressources de diffusion en continu.
ms.assetid: 90A572A4-3C76-4795-BAE9-FCC72B5F07AD
keywords:
- Formats de gabarit non pris en charge par les ressources de diffusion en continu
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 96eb1ec610b5aba743caf92584f52f5785046155
ms.lasthandoff: 02/07/2017

---

# <a name="stencil-formats-not-supported-with-streaming-resources"></a>Formats de gabarit non pris en charge par les ressources de diffusion en continu


Les formats contenant un gabarit ne sont pas pris en charge par les ressources de diffusion en continu.

Les formats contenant un gabarit incluent les formats DXGI\_FORMAT\_D24\_UNORM\_S8\_UINT (et les formats connexes de la famille R24G8) et DXGI\_FORMAT\_D32\_FLOAT\_S8X24\_UINT (et les formats connexes de la famille R32G8X24).

Certaines implémentations stockent les informations de profondeur et de gabarit dans des emplacements distincts tandis que d'autres les stockent ensemble. Pour chacun des deux cas, les vignettes devront être gérées différemment et aucune API unique ne pourra extraire ou rationaliser les différences. Pour les prochains matériels, nous vous conseillons de privilégier une prise en charge de surfaces de gabarit et de profondeur indépendantes, celles-ci étant restituées différemment sous forme de vignettes.

Pour une profondeur de 32 bits, vous disposeriez de vignettes 128 x 128 , tandis qu'avec un gabarit de 8 bits, elles seraient de 256 x 256. Ainsi, en raison de l'hétérogénéité des caractéristiques de profondeur et de gabarit, vos applications présenteraient des formes de vignette incohérentes. Ce problème existe déjà avec des formats différents de surfaces de cible de rendu.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Partage de ressources de diffusion en continu avec des processus et des appareils](streaming-resource-cross-process-and-device-sharing.md)

 

 





