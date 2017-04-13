---
author: drewbatgit
ms.assetid: 3E0FBB43-F6A4-4558-AA89-20E7760BA73F
description: "Cet article répertorie les profils DASH (Dynamic Adaptive Streaming over HTTP) pris en charge pour les applications UWP."
title: Prise en charge du profil DASH (Dynamic Adaptive Streaming over HTTP)
ms.author: drewbat
ms.date: 02/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 9ca8f2d1783a73ae38fed1d3ee1e58b809ddd2aa
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="dynamic-adaptive-streaming-over-http-dash-profile-support"></a>Prise en charge du profil DASH (Dynamic Adaptive Streaming over HTTP)
Le tableau suivant répertorie les profils DASH pris en charge pour les applications UWP.



|Balise | Type de manifeste | Remarques|Version de juillet de Windows10|Windows10, version1511|Windows10, version1607 |Windows10, version1607 |Windows10, version1703|
|----------------|------|-------|-----------|--------------|---------|-------|--------|
|urn:mpeg:dash:profile:isoff-live:2011 | Statique |     |Pris en charge            |  Pris en charge              | Pris en charge        |Pris en charge| Pris en charge|
|urn:mpeg:dash:profile:isoff-main:2011 |        | Meilleur effort | Pris en charge            |  Pris en charge              | Pris en charge        |Pris en charge| Pris en charge|
|urn:mpeg:dash:profile:isoff-live:2011 | Dynamique | $Time$ est pris en charge, mais $Number$ ne l’est pas dans les modèles de segment | Pas pris en charge            | Pas de prise en charge              | Pas de prise en charge        |Pas de prise en charge| Pris en charge|


## <a name="related-topics"></a>Rubriques connexes

* [Lecture de contenu multimédia](media-playback.md)
* [Diffusion en continu adaptative](adaptive-streaming.md)
 

 




