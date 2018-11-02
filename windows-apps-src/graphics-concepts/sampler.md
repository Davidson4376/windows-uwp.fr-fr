---
title: Échantillonneur
description: L’échantillonnage est le processus consistant à lire des valeurs d’entrée à partir d’une texture ou d'une autre ressource. Un \ 0034;échantillonneur \ 0034; est un objet capable de lire depuis des ressources.
ms.assetid: 7ECE13BB-9FC5-44A3-B1B2-2FE163F1D627
keywords:
- Échantillonneur
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ae15e41ec1d6493f33a776c11d74e28b2c95dc34
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5927163"
---
# <a name="sampler"></a>Échantillonneur


L’échantillonnage est le processus consistant à lire des valeurs d’entrée à partir d’une texture ou d'une autre ressource. Un «échantillonneur» est un objet capable de lire depuis des ressources.

Il existe de nombreux problèmes et les artefacts de l’échantillonnage à partir d’une texture et le rendu sur une zone de l’écran. Par exemple, si la zone qui doit être rendue est de 50 x 50pixels et que la texture est de 16 x 16pixels ou de 256 par 256pixels, vous devez appliquer un étirement ou une réduction considérable de la texture. Comme cette incompatibilité de taille entraîne des artefacts indésirables, des techniques de filtrage sont utilisées pour atténuer ces artefacts. Une approche de filtrage fréquemment utilisée pour utiliser des petites textures pour les zones de rendu plus grandes est le filtrage «bilinéaire».

Un autre problème se produit lorsque la zone de rendu est située à un angle très oblique par rapport à la vue (par exemple, une texture 256 par 256 rendue sur une zone large de 100pixels, mais présentant une profondeur de 5pixels en raison de l’angle de visualisation). Dans ce cas, un filtrage «anisotropique» est souvent appliqué. Le filtrage anisotropique fournit une meilleure qualité d’image que le filtrage bilinéaire, car elle supprime les effets d’alias sans floutage excessif, mais elle nécessite davantage de ressources de calcul.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Affichages](views.md)

 

 




