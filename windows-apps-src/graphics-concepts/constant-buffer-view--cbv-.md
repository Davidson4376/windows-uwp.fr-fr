---
title: "Affichage des mémoires tampons de constantes (CBV)"
description: "Les mémoires tampons contiennent des données constantes de nuanceur. Leur intérêt consiste à rendre les données persistantes et accessibles depuis n&quot;importe quel nuanceur GPU jusqu&quot;à ce qu’il soit nécessaire de les modifier."
ms.assetid: 99AEC6B0-A43B-4B61-8C3A-ECC8DE1B69A7
keywords: "Affichage des mémoires tampons de constantes (CBV)"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 755c80e895281b93e9e37144526ed1790c63199f
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="constant-buffer-view-cbv"></a>Affichage des mémoires tampons de constantes (CBV)


Les mémoires tampons contiennent des données constantes de nuanceur. Leur intérêt consiste à rendre les données persistantes et accessibles depuis n'importe quel nuanceur GPU jusqu'à ce qu’il soit nécessaire de les modifier.

Les données types d'une mémoire tampon de constantes seraient les matrices universelles, de projection et d’affichage, qui restent constantes tout au long du dessin d’une seule image.

La disposition de la mémoire tampon de constantes doit correspondre à la disposition HLSL (reportez-vous à l'article [Règles d'empaquetage des variables constantes](https://msdn.microsoft.com/library/windows/desktop/bb509632.aspx)).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Affichages](views.md)

 

 




