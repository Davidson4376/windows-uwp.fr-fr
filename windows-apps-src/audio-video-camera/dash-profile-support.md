---
ms.assetid: 3E0FBB43-F6A4-4558-AA89-20E7760BA73F
description: Cet article répertorie les profils DASH (Dynamic Adaptive Streaming over HTTP) pris en charge pour les applications UWP.
title: Prise en charge du profil DASH (Dynamic Adaptive Streaming over HTTP)
ms.date: 02/15/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 268020c06be57d8ac300f1202046e52b6e1d2507
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867384"
---
# <a name="dynamic-adaptive-streaming-over-http-dash-profile-support"></a>Prise en charge du profil DASH (Dynamic Adaptive Streaming over HTTP)


## <a name="supported-dash-profiles"></a>Profils DASH pris en charge
Le tableau ci-après répertorie les profils DASH pris en charge pour les applications UWP.

|Tag | Type de manifeste | Notes|Version de juillet de Windows 10|Windows 10, version 1511|Windows 10, version 1607 |Windows 10, version 1607 |Windows 10, version 1703| Windows 10, version 1809
|----------------|------|-------|-----------|--------------|---------|-------|--------|--------|
|urn:mpeg&#58;dash:profile:isoff-live:2011 | Statique |     |Pris en charge            |  Pris en charge              | Pris en charge        |Pris en charge| Pris en charge| Pris en charge|
|urn:mpeg&#58;dash:profile:isoff-main:2011 |        | Meilleur effort | Pris en charge            |  Pris en charge              | Pris en charge        |Pris en charge| Pris en charge| Pris en charge|
|urn:mpeg&#58;dash:profile:isoff-live:2011 | dynamique | $Time$ est pris en charge, mais $Number$ ne l’est pas dans les modèles de segment | Non pris en charge            | Non pris en charge              | Non pris en charge        |Non pris en charge| Pris en charge| Pris en charge|
|urn:mpeg&#58;dash:profile:isoff-on-demand:2011 |        |  | Non pris en charge            |  Non pris en charge              | Non pris en charge        |Non pris en charge| Non pris en charge| Pris en charge|


## <a name="unsupported-dash-profiles"></a>Profils DASH non pris en charge
Les profils non répertoriés dans le tableau ci-dessus ne sont pas pris en charge, notamment les suivants :

* urn:mpeg&#58;dash:profile:full:2011
* urn:mpeg&#58;dash:profile:mp2t-main:2011
* urn:mpeg&#58;dash:profile:mp2t-simple:2011


## <a name="related-topics"></a>Rubriques connexes

* [Lecture de contenu multimédia](media-playback.md)
* [Diffusion adaptative](adaptive-streaming.md)
 

 




