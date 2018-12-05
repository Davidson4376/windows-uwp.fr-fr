---
title: Affichages de texture
description: Dans Direct3D, les ressources de texture sont disponibles avec un affichage, qui est un mécanisme d’interprétation matérielle d’une ressource de la mémoire.
ms.assetid: 18DABFCE-8A36-4C4E-B08E-10428B05D701
keywords:
- Affichages de texture
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e9167db4648dd193acaff0a224f3378486d171ad
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8707861"
---
# <a name="texture-views"></a>Affichages de texture


Dans Direct3D, les ressources de texture sont disponibles avec une vue, qui est un mécanisme d’interprétation matérielle d’une ressource de la mémoire. Un affichage permet à une étape du pipeline spécifique d’accéder uniquement aux [sous-ressources](resource-types.md) nécessaires, dans la représentation souhaitée par l’application.

Un affichage prend en charge la notion de ressource sans type. Une ressource sans type est une ressource créée avec une taille spécifique, mais pas avec un type de données spécifique. Les données sont interprétées dynamiquement lorsqu’elles sont liées au pipeline.

L’illustration suivante montre un exemple de liaison d’un tableau de textures 2D avec 6textures en tant que ressource de nuanceur, par la création d’une vue de ressource de nuanceur. La ressource est ensuite traitée comme un tableau de textures. (Remarque: une sous-ressource ne peut pas être liée simultanément en tant qu’entrée et sortie du pipeline).

![illustration d’un tableau de textures avec six textures](images/d3d10-cube-texture-faces.png)

Lorsque vous utilisez un tableau de textures 2D en tant que cible de rendu, la ressource peut être considérée comme un tableau de textures 2D (6dans cet exemple) avec des niveaux de mipmap (3dans cet exemple).

Créez un objet d’affichage pour une cible de rendu en appelant la méthode CreateRenderTargetView. Appelez ensuite OMSetRenderTargets pour définir l’affichage de la cible de rendu au pipeline. Générez les cibles de rendu en appelant Draw et en utilisant la méthode RenderTargetArrayIndex pour les indexer dans la texture du tableau. Vous pouvez utiliser une sous-ressource (niveau de mipmap, combinaison d’index de tableau) pour effectuer une liaison à un tableau de sous-ressources. Vous pouvez effectuer une liaison au deuxième niveau de mipmap et mettre à jour uniquement ce niveau de mipmap particulier si vous le souhaitez, comme dans l’illustration suivante.

![illustration de liaison au deuxième niveau de mipmap d’un tableau de textures](images/d3d10-cube-texture-faces-subresource.png)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques associées


[Ressources](resources.md)

 

 




