---
author: drewbatgit
ms.assetid: 3E0FBB43-F6A4-4558-AA89-20E7760BA73F
description: Cet article répertorie les profils DASH (Dynamic Adaptive Streaming over HTTP) pris en charge pour les applications UWP.
title: Prise en charge du profil DASH (Dynamic Adaptive Streaming over HTTP)
ms.author: drewbat
ms.date: 02/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c411d479430f793d85863f66c64758155b8a5758
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
ms.locfileid: "1842336"
---
# <a name="dynamic-adaptive-streaming-over-http-dash-profile-support"></a>Prise en charge du profil DASH (Dynamic Adaptive Streaming over HTTP)


## <a name="supported-dash-profiles"></a>Profils DASH pris en charge
Le tableau ci-après répertorie les profils DASH pris en charge pour les applications UWP.

|Balise | Type de manifeste | Remarques|Version de juillet de Windows10|Windows10, version1511|Windows10, version1607 |Windows10, version1607 |Windows10, version1703|
|----------------|------|-------|-----------|--------------|---------|-------|--------|
|urn:mpeg&#58;dash:profile:isoff-live:2011 | Statique |     |Pris en charge            |  Pris en charge              | Pris en charge        |Pris en charge| Pris en charge|
|urn:mpeg&#58;dash:profile:isoff-main:2011 |        | Meilleur effort | Pris en charge            |  Pris en charge              | Pris en charge        |Pris en charge| Pris en charge|
|urn:mpeg&#58;dash:profile:isoff-live:2011 | Dynamique | $Time$ est pris en charge, mais $Number$ ne l’est pas dans les modèles de segment | Pas de prise en charge            | Pas de prise en charge              | Pas de prise en charge        |Pas de prise en charge| Pris en charge|


## <a name="unsupported-dash-profiles"></a>Profils DASH non pris en charge
Les profils non répertoriés dans le tableau ci-dessus ne sont pas pris en charge, notamment les suivants:

* urn:mpeg&#58;dash:profile:full:2011
* urn:mpeg&#58;dash:profile:isoff-on-demand:2011
* urn:mpeg&#58;dash:profile:mp2t-main:2011
* urn:mpeg&#58;dash:profile:mp2t-simple:2011


## <a name="related-topics"></a>Articles connexes

* [Lecture de contenu multimédia](media-playback.md)
* [Diffusion en continu adaptative](adaptive-streaming.md)
 

 




