---
title: Mémoires tampons d’index
description: Les mémoires tampons d’index sont des mémoires comportant des données d’index, qui sont des décalages d’entiers dans des mémoires tampons de vertex, utilisés pour l’affichage des primitives.
ms.assetid: 14D3DEC5-CF74-488B-BE41-16BF5E3201BE
keywords:
- Mémoires tampons d’index
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 36d08006fa2f32812f97daef5135a98dce16c4e5
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8929593"
---
# <a name="index-buffers"></a>Mémoires tampons d’index


Les *mémoires tampons* d’index sont des mémoires comportant des données d’index, qui sont des décalages d’entiers dans des mémoires tampons de vertex, utilisés pour l’affichage des primitives.

Les mémoires tampons d’index sont des mémoires comportant des données d’index. Les données d’index, ou les index, sont des décalages d’entiers dans des mémoires tampons de vertex, qui sont utilisés pour l’affichage de primitives.

Une mémoire tampon de vertex comportant des vertex, vous pouvez dessiner une mémoire tampon de vertex avec ou sans primitive indexée. Toutefois, ceci parce qu’une mémoire tampon d’index comporte des index, vous ne pouvez pas utiliser une mémoire de ce type sans sa mémoire tampon de vertex correspondante.

## <a name="span-idindexbufferdescriptionspanspan-idindexbufferdescriptionspanspan-idindexbufferdescriptionspanindex-buffer-description"></a><span id="Index_Buffer_Description"></span><span id="index_buffer_description"></span><span id="INDEX_BUFFER_DESCRIPTION"></span>Description d’une mémoire tampon d’index


Une mémoire tampon d’index est décrite en fonction de ses fonctionnalités. Où existe-t-elle en mémoire? Supporte-t-elle la lecture et l’écriture? Combien d’index peut-elle contenir? De quel type?

Les descriptions de mémoires tampons d’index indiquent à votre application la méthode de création d’une mémoire existante. Vous fournissez une structure de description vide, que le système renseigne avec les fonctionnalités d’une mémoire tampon d’index préalablement créée.

## <a name="span-idindexprocessingrequirementsspanspan-idindexprocessingrequirementsspanspan-idindexprocessingrequirementsspanindex-processing-requirements"></a><span id="Index_Processing_Requirements"></span><span id="index_processing_requirements"></span><span id="INDEX_PROCESSING_REQUIREMENTS"></span>Conditions requises pour le traitement des index


Les performances des opérations de traitement des index dépendent grandement de l’emplacement d’hébergement de la mémoire tampon de l’index dans la mémoire et du type de périphérique de rendu utilisé. Les applications contrôlent l’allocation de mémoire des mémoires tampons d’index lors de leur création.

L’application peut écrire directement les index sur une mémoire tampon d’index allouée dans une mémoire optimisée pour l’espace disque. Cette technique empêche toute opération ultérieure de copie redondante. Cette technique ne fonctionne pas bien si votre application lit les données d’une mémoire tampon d’index, dans la mesure où les opérations de lecture effectuées par l’hôte à partir d’une mémoire optimisée pour l’espace disque peuvent être très lentes. Par conséquent, si votre application doit lire des données durant le traitement ou écrit de manière irrégulière sur la mémoire tampon, une mémoire tampon système s’avère être un choix plus judicieux.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Mémoires tampons de vertex et d’index](vertex-and-index-buffers.md)

 

 




