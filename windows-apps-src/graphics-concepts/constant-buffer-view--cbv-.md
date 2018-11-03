---
title: Affichage des mémoires tampons de constantes (CBV)
description: Les mémoires tampons contiennent des données constantes de nuanceur. Leur intérêt consiste à rendre les données persistantes et accessibles depuis n'importe quel nuanceur GPU jusqu'à ce qu’il soit nécessaire de les modifier.
ms.assetid: 99AEC6B0-A43B-4B61-8C3A-ECC8DE1B69A7
keywords:
- Affichage des mémoires tampons de constantes (CBV)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cca90c705c7bb4dd1c7e283a9c3ed267cd282b56
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5988296"
---
# <a name="constant-buffer-view-cbv"></a>Affichage des mémoires tampons de constantes (CBV)


Les mémoires tampons contiennent des données constantes de nuanceur. Leur intérêt consiste à rendre les données persistantes et accessibles depuis n'importe quel nuanceur GPU jusqu'à ce qu’il soit nécessaire de les modifier.

Les données types d'une mémoire tampon de constantes seraient les matrices universelles, de projection et d’affichage, qui restent constantes tout au long du dessin d’une seule image.

La disposition de la mémoire tampon de constantes doit correspondre à la disposition HLSL (reportez-vous à l'article [Règles d'empaquetage des variables constantes](https://msdn.microsoft.com/library/windows/desktop/bb509632.aspx)).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Affichages](views.md)

 

 




