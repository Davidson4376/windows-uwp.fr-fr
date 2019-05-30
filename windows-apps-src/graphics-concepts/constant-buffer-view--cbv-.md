---
title: Affichage des mémoires tampons de constantes (CBV)
description: Les mémoires tampons contiennent des données constantes de nuanceur. Leur intérêt consiste à rendre les données persistantes et accessibles depuis n'importe quel nuanceur GPU jusqu'à ce qu’il soit nécessaire de les modifier.
ms.assetid: 99AEC6B0-A43B-4B61-8C3A-ECC8DE1B69A7
keywords:
- Affichage des mémoires tampons de constantes (CBV)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7179f8644970a24a9e7b9ce50a4bcb4d5d225d46
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370456"
---
# <a name="constant-buffer-view-cbv"></a>Affichage des mémoires tampons de constantes (CBV)


Les mémoires tampons contiennent des données constantes de nuanceur. Leur intérêt consiste à rendre les données persistantes et accessibles depuis n'importe quel nuanceur GPU jusqu'à ce qu’il soit nécessaire de les modifier.

Les données types d'une mémoire tampon de constantes seraient les matrices universelles, de projection et d’affichage, qui restent constantes tout au long du dessin d’une seule image.

La disposition de la mémoire tampon de constantes doit correspondre à la disposition HLSL (reportez-vous à l'article [Règles d'empaquetage des variables constantes](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-packing-rules)).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Vues](views.md)

 

 




