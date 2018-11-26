---
title: Types de lumière
description: 'La propriété du type de lumière définit le type de source lumineuse utilisé. Direct3D prend en charge 3types de lumières: les lumières à points, les projecteurs et les lumières directionnelles.'
ms.assetid: 57748CAF-6F08-4D1D-9ED6-8FAA8C5FE314
keywords:
- Types de lumière
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1815f0956fbc175fec5ca892dbeeec92b2f939ab
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7697842"
---
# <a name="light-types"></a>Types de lumière


La propriété du type de lumière définit le type de source lumineuse utilisé. Direct3D prend en charge 3types de lumières: les lumières à points, les projecteurs et les lumières directionnelles. Chaque type illumine les objets d’une scène de manière particulière, avec des niveaux variés de calcul.

## <a name="span-idpointlightspanspan-idpointlightspanspan-idpointlightspanpoint-light"></a><span id="Point_Light"></span><span id="point_light"></span><span id="POINT_LIGHT"></span>Lumière à points


Au sein d’une scène, les lumières à points présentent une couleur et une position, mais aucune direction spécifique. Elles émettent la lumière uniformément dans toutes les directions, tel qu’illustré ci-après.

![illustration de la lumière à points](images/ptlight.png)

Une ampoule est un bon exemple d’émission de lumière à points. Les lumières à points sont affectées par l’atténuation et l’étendue. Elles illuminent un maillage, par unité de vertex. Durant l’éclairage, Direct3D utilise la position de la lumière à points dans l’espace du monde et les coordonnées du vertex illuminé pour dériver un vecteur pour la direction du faisceau, et la distance parcourue par la lumière. Les 2éléments sont utilisés, avec le vertex normal, pour calculer la contribution de la lumière à l’illumination de la surface.

## <a name="span-iddirectionallightspanspan-iddirectionallightspanspan-iddirectionallightspandirectional-light"></a><span id="Directional_Light"></span><span id="directional_light"></span><span id="DIRECTIONAL_LIGHT"></span>Lumière directionnelle


Les lumières directionnelles présentent uniquement une couleur et une direction, pas de position. Elles émettent une lumière parallèle. Cela signifie que l’ensemble de l’éclairage généré par les lumières directionnelles traverse une scène, dans la même direction. Imaginez une lumière directionnelle comme une source située à une distance quasi infinie, telle que le soleil. Les lumières directionnelles n’étant pas affectées par l’atténuation ou l’étendue, la direction et la couleur que vous spécifiez sont les seuls facteurs pris en compte par Direct3D pour calculer les couleurs du vertex. En raison du nombre réduit de facteurs d’illumination, il s’agit des lumières les moins gourmandes en calcul à utiliser.

## <a name="span-idspotlightspanspan-idspotlightspanspan-idspotlightspanspotlight"></a><span id="SpotLight"></span><span id="spotlight"></span><span id="SPOTLIGHT"></span>Projecteurs


Les projecteurs possèdent des paramètres de couleur, de position et de direction d’émission de la lumière. La lumière émise par un projecteur est constituée d’un cône interne lumineux et d’un cône externe plus grand. Tel qu’indiqué dans l’illustration suivante, l’intensité de la lumière diminue entre les deux.

![illustration d’un projecteur avec un cône interne et un cône externe](images/spotlt.png)

Les projecteurs sont affectés par l’atténuation et l’étendue. Ces facteurs, tout comme la distance parcourue par la lumière vers chaque vertex, sont définis lors du calcul des effets d’éclairage pour les objets d’une scène. Le calcul de ces effets pour chaque vertex, extrêmement lourd, fait des projecteurs les plus gourmands dans ce domaine, parmi toutes les lumières de Direct3D.

Les valeurs d’atténuation, thêta et phi sont utilisées uniquement par les projecteurs. Ces valeurs définissent le diamètre des cônes interne et externe d’un projecteur, et l’atténuation de la lumière entre les deux.

Thêta est l’angle en radians du cône interne du projecteur, tandis que la valeur Phi est l’angle du cône externe de lumière. L’atténuation est la mesure de diminution de l’intensité de la lumière entre la bordure externe du cone interne et la bordure interne du cône externe. La plupart des applications définissent l’atténuation sur 1,0, de manière à ce que cette valeur soit uniforme entre les 2cônes. Vous pouvez aussi paramétrer d’autres valeurs, au besoin.

L’illustration suivante représente la relation entre ces valeurs et la manière dont elles affectent les cônes interne et externe de lumière d’un projecteur.

![illustration de l’incidence des valeurs phi et thêta sur les cônes du projecteur](images/spotlt2.png)

Les projecteurs émettent un cône de lumière en 2portions: un cône interne lumineux et un cône externe. La lumière est plus intense dans le cône interne et est inexistante à l’extérieur du cône externe, l’intensité diminuant entre des deuxzones. Ce type de réduction de l’intensité s’appelle l’atténuation.

La quantité de lumière reçue par un vertex est fonction de l’emplacement du vertex dans les cônes interne et externe. Direct3D calcule le produit scalaire du vecteur de direction du projecteur(L) et le vecteur de la lumière du vertex (D). Cette valeur, égale au cosinus de l’angle entre les 2vecteurs, indique la position du vertex. Cette dernière peut être comparée aux angles du cône de lumière, ceci pour identifier l’emplacement du vertex, dans les cônes interne ou externe. L’illustration suivante fournit une représentation graphique de l’association entre ces deuxvecteurs.

![illustration du vecteur de direction du projecteur et du vecteur entre le vertex et le projecteur](images/spotalg1.png)

Le système compare cette valeur au cosinus des angles des cônes interne et externe du projecteur. Les valeurs thêta et phi de la lumière représentent le total des angles des cônes interne et externe. Dans la mesure où l’atténuation se produit à mesure que le vertex s’éloigne du centre de l’illumination (non sur la totalité de l’angle occupé par les cônes), le processus d’exécution divise ces angles de cônes en 2 avant de calculer les cosinus.

Si le produit scalaire des vecteurs L et D est inférieur ou égal au cosinus de l’angle du cône externe, le vertex est placé au-delà du cône externe. Il ne reçoit pas de lumière. Si le produit scalaire de L et D est supérieur au cosinus de l’angle du cône interne, le vertex se situe dans le cône interne. Il reçoit la quantité maximale de lumière, affectée par l’atténuation à mesure qu’il s’écarte. Si le vertex se situe entre les 2régions, l’atténuation est calculée à l’aide de l’équation suivante.

![formule dédiée au calcul de l’intensité de la lumière, après atténuation](images/falloff.png)

où:

-   I<sub>f</sub> correspond à l’intensité de la lumière après l’atténuation
-   Alpha est l’angle entre les vecteurs L et D
-   Thêta est l’angle du cône interne
-   Phi est l’angle du cône externe
-   p est l’atténuation

Cette formule génère une valeur comprise entre 0,0 et 1,0 qui adapte l’intensité de la lumière au niveau du vertex, afin de tenir compte de l’atténuation. L’atténuation, en tant que facteur de la distance entre le vertex et la lumière, est également appliquée. Le graphique suivant montre comment différentes valeurs d’atténuation peuvent affecter la courbe d’atténuation.

![graphique de l’intensité de la lumière par rapport à la distance entre le vertex et la lumière](images/fallgraf.png)

L’effet de ces valeurs d’atténuation sur l’éclairage réel est minime. Une petite pénalité de performance est encourue dans les situations où la courbe d’atténuation est formée avec des valeurs autres que 1,0. Pour ces raisons, cette valeur est généralement définie sur 1.0.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Lumières et matériaux](lights-and-materials.md)

 

 




