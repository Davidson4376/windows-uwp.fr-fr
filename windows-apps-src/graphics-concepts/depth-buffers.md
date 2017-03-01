---
title: Tampons de profondeur
description: "Un tampon de profondeur, ou tampon z-buffer, stocke des informations de profondeur afin de définir les zones de polygones à afficher, plutôt que celles à masquer."
ms.assetid: BE83A1D7-D43D-4013-8817-EFD2B24DC58E
keywords:
- Tampons de profondeur
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 541794ea7d5df8534ddfc3272957d0b66813d18c
ms.lasthandoff: 02/07/2017

---

# <a name="depth-buffers"></a>Tampons de profondeur


Un *tampon de profondeur*, ou *tampon z-buffer*, stocke des informations de profondeur afin de définir les zones de polygones à afficher, plutôt que celles à masquer.

## <a name="span-idoverviewspanspan-idoverviewspanspan-idoverviewspanoverview"></a><span id="Overview"></span><span id="overview"></span><span id="OVERVIEW"></span>Vue d’ensemble


Un tampon de profondeur, souvent appelé tampon z-buffer ou tampon w-buffer, est une propriété du périphérique qui stocke les informations de profondeur à utiliser par Direct3D. Lorsque Direct3D effectue le rendu d’une scène sur une surface cible, il peut utiliser la mémoire d’une surface de tampon de profondeur associée en tant qu’espace de travail pour déterminer la façon dont les pixels de polygones rastérisés se masquent les uns les autres. Direct3D utilise une surface Direct3D hors écran en guise de cible sur laquelle sont écrites les valeurs de couleur finales. La surface de tampon de profondeur associée à la surface cible du rendu est utilisée pour stocker les informations de profondeur qui indiquent à Direct3D la profondeur de chacun des pixels visibles dans la scène.

Lorsqu’une scène est rastérisée avec activation du stockage dans un tampon de profondeur, chaque point de la surface de rendu est testé. Les valeurs figurant dans le tampon de profondeur peuvent être la coordonnée Z d’un point ou sa coordonnée W homogène à partir de la position (X,Y,Z,W) du point dans l’espace de projection. Très souvent, un tampon de profondeur qui utilise des valeurs Z est appelé tampon z-buffer, et celui qui utilise des valeurs W est désigné sous le terme de tampon w-buffer. Chaque type de tampon de profondeur présente des avantages et des inconvénients, qui sont décrits dans la suite de cet article.

Au début du test, la valeur de profondeur dans le tampon de profondeur est définie sur la valeur maximale autorisée pour la scène. La valeur de couleur de la surface de rendu est définie sur la valeur de couleur d’arrière-plan ou sur la valeur de couleur de la texture d’arrière-plan au niveau du point testé. Le système teste chaque polygone de la scène pour déterminer s’il croise la coordonnée actuelle (X,Y) sur la surface de rendu.

Si tel n’est pas le cas, le système vérifie si la valeur de profondeur au niveau du point actuel, qui constitue la coordonnée Z dans un tampon z-buffer, et la coordonnée W dans un tampon w-buffer, est inférieure à la valeur de profondeur stockée dans le tampon de profondeur. Si la valeur de profondeur du polygone est inférieure, elle est stockée dans le tampon de profondeur, et la valeur de couleur du polygone est écrite au niveau du point actuel sur la surface de rendu. Si la valeur de profondeur du polygone au niveau du point est supérieure, l’application teste le polygone suivant de la liste. Ce processus est illustré dans le diagramme suivant.

![diagramme du test des valeurs de profondeur](images/zbuffer.png)

## <a name="span-idbufferingtechniquesspanspan-idbufferingtechniquesspanspan-idbufferingtechniquesspanbuffering-techniques"></a><span id="Buffering_techniques"></span><span id="buffering_techniques"></span><span id="BUFFERING_TECHNIQUES"></span>Techniques de mise en mémoire tampon


Bien que la plupart des applications n’utilisent pas cette fonctionnalité, vous pouvez modifier la comparaison que Direct3D utilise pour déterminer les valeurs qui sont placées dans le tampon de profondeur et, par la suite, la surface cible du rendu. Sur certains matériels, la modification de la fonction de comparaison peut entraîner la désactivation du test Z hiérarchique.

Étant donné que la quasi-totalité des accélérateurs du marché prennent en charge le stockage des valeurs dans un tampon z-buffer, les tampons z-buffer constituent désormais le type le plus courant de tampon de profondeur. Toutefois, les tampons z-buffer ont leurs inconvénients. En raison des calculs mathématiques qu’elles impliquent, les valeurs Z générées dans un tampon z-buffer tendent à ne pas être distribuées équitablement dans la plage du tampon z-buffer (généralement comprise entre 0,0 et 1,0 inclus).

Plus précisément, le rapport entre les plans de découpage lointain et proche joue un grand rôle dans le caractère inégal de la distribution des valeurs Z. Lorsque le rapport de distance entre le plan lointain et le plan proche est de 100, 90 pour cent de la plage du tampon de profondeur sont utilisés sur les 10 premiers pour cent de la plage de profondeur de la scène. Les applications de divertissement classiques ou les simulations visuelles avec des scènes en extérieur nécessitent généralement des rapports entre le plan lointain et le plan proche compris entre 1 000 et 10 000. Lorsque le rapport est de 1 000, 98 % de la plage sont utilisés sur les 2 premiers pour cent de la plage de profondeur, et la distribution empire en cas d’utilisation de rapports plus élevés. Cela peut entraîner des artefacts de surface masqués dans les objets distants, en particulier lors de l’utilisation de tampons de profondeur de 16 bits, qui constitue la profondeur de couleur la plus couramment prise en charge.

En revanche, un tampon de profondeur basé sur la valeur W est généralement distribué plus équitablement entre les plans de découpage proche et lointain qu’un tampon z-buffer. Son principal avantage réside dans le fait que le rapport de distance entre les plans de découpage lointain et proche n’est plus un problème. Les applications peuvent ainsi prendre en charge des plages maximales élevées, tout en continuant de bénéficier d’un stockage des valeurs dans un tampon de profondeur relativement précis et proche du point de vue. Toutefois, un tampon de profondeur basé sur la valeur W n’est pas parfait et peut parfois présenter des artefacts de surface masqués pour des objets proches. Un autre inconvénient inhérent au stockage des valeurs dans un tampon w-buffer est lié à la prise en charge matérielle ; en effet, la prise en charge du stockage dans un tampon w-buffer n’est pas aussi répandue dans le matériel que celle du stockage dans un tampon z-buffer.

L’utilisation d’un tampon z-buffer requiert un traitement spécifique lors du rendu. Différentes techniques permettent d’optimiser le rendu en cas d’utilisation des tampons z-buffer. Les applications peuvent accroître les performances lors du stockage des valeurs dans un tampon z-buffer et de l’utilisation de textures en garantissant le rendu des scènes de l’avant vers l’arrière. Les primitives stockées dans un tampon z-buffer avec texture sont préalablement testées par rapport au tampon z-buffer sur la base des lignes de balayage. Si une ligne de balayage est masquée par un polygone précédemment rendu, le système la rejette rapidement et efficacement. Le stockage dans un tampon z-buffer peut améliorer les performances, mais cette technique se révèle particulièrement utile lorsqu’une scène dessine les mêmes pixels à plusieurs reprises. Ceci est difficile à calculer exactement, mais vous pouvez en effectuer une approximation fiable dans la plupart des cas. Si les mêmes pixels sont dessinés moins de deux fois, vous pouvez optimiser les performances en désactivant le stockage dans un tampon z-buffer et en effectuant le rendu de la scène de l’arrière vers l’avant.

L’interprétation réelle d’une valeur de profondeur est propre au renderer.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Articles connexes


[Tampons de profondeur et tampons stencil buffer](depth-and-stencil-buffers.md)

 

 





