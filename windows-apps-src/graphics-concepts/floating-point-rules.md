---
title: Règles en matière de virgule flottante
description: Direct3D prend en charge plusieurs représentations à virgule flottante. Tous les calculs en virgule flottante sont régis par un sous-ensemble défini de règles de représentation des nombres en virgule flottante 32bits simple précision conformément à l’IEEE754.
ms.assetid: 3B0C95E2-1025-4F70-BF14-EBFF2BB53AFF
keywords:
- Règles en matière de virgule flottante
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bcfdb8f6258547ff210d80136a6113e04092aad2
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5753438"
---
# <a name="span-iddirect3dconceptsfloating-pointrulesspanfloating-point-rules"></a><span id="direct3dconcepts.floating-point_rules"></span>Règles en matière de virgule flottante


Direct3D prend en charge plusieurs représentations en virgule flottante. Tous les calculs en virgule flottante sont régis par un sous-ensemble défini de règles de représentation des nombres en virgule flottante 32bits simple précision conformément à l’IEEE754.

## <a name="span-idalpha32bitspanspan-idalpha32bitspan32-bit-floating-point-rules"></a><span id="alpha_32_bit"></span><span id="ALPHA_32_BIT"></span>Règles en matière de virgule flottante 32bits


Il existe deux ensembles de règles: les règles qui sont conformes à la normeIEEE-754, et celles qui s’en écartent.

### <a name="span-idalpha754rulesspanspan-idalpha754rulesspanspan-idalpha754rulesspanhonored-ieee-754-rules"></a><span id="alpha_754_Rules"></span><span id="alpha_754_rules"></span><span id="ALPHA_754_RULES"></span>Règles respectant la norme IEEE-754

Certaines de ces règles constituent une simple option dans laquelle IEEE-754 propose des choix.

-   La division par 0 produit +/-INF (infini), sauf 0/0 qui donne une valeur NaN (n’est pas un nombre).
-   Le logarithme de (+/-) 0 produit -INF.  

    Le logarithme d’une valeur négative (différente de -0) produit NaN.
-   La racine carrée réciproque (rsq) ou la racine carrée (sqrt) d’une valeur négative produit NaN.  

    La seule exception est -0; sqrt(-0) produit -0, et rsq(-0) donne -INF.
-   INF - INF = NaN
-   (+/-)INF / (+/-)INF = NaN
-   (+/-)INF \* 0 = NaN
-   NaN (toute opération) toute valeur = NaN
-   Lorsque l’une des opérandes ou les deux correspondent à NaN, les comparaisons EQ (égal à), GT (supérieur à), GE (supérieur ou égal à), LT (inférieur à) et LE (inférieur ou égal à) renvoient la valeur **FALSE**.
-   Les comparaisons ignorent le signe de la valeur0 (donc, +0 est égal à -0).
-   Lorsque l’une des opérandes ou les deux correspondent à NaN, la comparaison NE (différent de) renvoie la valeur **TRUE**.
-   Les comparaisons de toute valeur différente de NaN avec +/-INF renvoient le résultat correct.

### <a name="span-idalpha754deviationsspanspan-idalpha754deviationsspanspan-idalpha754deviationsspandeviations-or-additional-requirements-from-ieee-754-rules"></a><span id="alpha_754_Deviations"></span><span id="alpha_754_deviations"></span><span id="ALPHA_754_DEVIATIONS"></span>Écarts ou exigences supplémentaires par rapport aux règles IEEE-754

-   IEEE-754 exige que les opérations en virgule flottante produisent un résultat qui constitue la valeur représentable la plus proche d’un résultat infiniment précis, connu sous le terme d’arrondi au chiffre pair le plus proche.

    Direct3D11 et les versions ultérieures stipulent la même exigence que la norme IEEE-754: les opérations en virgule flottante sur 32bits produisent un résultat ne s’écartant pas de plus de 0,5ULP (Unit in the Last Place, écart entre deux flottants positifs successifs) du résultat infiniment précis. Cela signifie par exemple que le matériel est autorisé à tronquer les résultats à 32bits au lieu d’effectuer un arrondi au chiffre pair le plus proche, car cette opération entraînerait une erreur de 0,5ULP au plus. Cette règle s’applique uniquement aux additions, aux soustractions et aux multiplications.

    Les versions antérieures de Direct3D présentent une exigence moins stricte que la norme IEEE-754: les opérations en virgule flottante sur 32bits produisent un résultat ne s’écartant pas de plus de 1ULP du résultat infiniment précis. Cela signifie par exemple que le matériel est autorisé à tronquer les résultats à 32bits au lieu d’effectuer un arrondi au chiffre pair le plus proche, car cette opération entraînerait une erreur de 1ULP au plus.

-   Il n’existe aucune prise en charge pour les exceptions de virgule flottante, les bits d’état ou les interruptions.
-   Les nombres dénormalisés sont remplacés par une valeur zéro avec conservation du signe sur l’entrée et la sortie de toute opération mathématique en virgule flottante. Il existe des exceptions pour des opérations d’E/S ou de déplacement de données qui ne manipulent pas les données.
-   Les états qui contiennent des valeurs en virgule flottante, telles que des valeurs Viewport MinDepth/MaxDepth ou BorderColor, peuvent être fournis sous forme de valeurs dénormalisées et être ou non vidés avant que le matériel ne les utilise.
-   Les opérations min ou max vident les nombres dénormalisés pour la comparaison, mais le résultat peut ou non faire l’objet d’un vidage des nombres dénormalisés.
-   L’entrée d’une valeur NaN dans une opération produit toujours une valeur NaN en sortie. Toutefois, il n’est pas nécessaire que la configuration binaire exacte de la valeur NaN reste identique (sauf si l’opération est une instruction de déplacement brute, ce qui ne modifie pas les données).
-   Les opérations min ou max dans lesquelles une seule opérande est une valeur NaN renvoient l’autre opérande comme résultat (contrairement aux règles de comparaison que nous avons examinées plus haut). Ceci est une règle énoncée par la norme IEEE754R.

    La spécification IEEE-754R relative aux opérations min et max en virgule flottante stipule que si l’une des entrées de l’opération min ou max est une valeur NaN silencieuse (QNaN), le résultat de l’opération correspond à l’autre paramètre. Par exemple:

    ```ManagedCPlusPlus
    min(x,QNaN) == min(QNaN,x) == x (same for max)
    ```

    Une révision apportée à la spécification IEEE-754R stipule désormais un autre comportement pour les opérations min et max lorsque l’une des entrées est une valeur NaN «signalée» (SNaN) opposée à une valeur QNaN:

    ```ManagedCPlusPlus
    min(x,SNaN) == min(SNaN,x) == QNaN (same for max)
     
    ```

    En règle générale, Direct3D respecte les normes IEEE-754 et IEEE-754R sur le plan arithmétique. Toutefois, dans ce cas précis, il existe une différence.

    Les règles arithmétiques dans Direct3D10 et les versions ultérieures ne font pas de distinction entre les valeurs NaN silencieuses et signalées (QNaN/SNaN). Toutes les valeurs NaN sont gérées de la même façon. Dans le cas des opérations min et max, le comportement Direct3D vis-à-vis de toute valeur NaN est identique au traitement des valeurs QNaN défini dans la spécification IEEE-754R. (À titre de complément d’information, si les deux entrées correspondent à une valeur NaN, n’importe quelle valeur NaN est renvoyée.)

-   Une autre règle IEEE754R stipule que min(-0,+0) == min(+0,-0) == -0, et que max(-0,+0) == max(+0,-0) == +0, ce qui conserve le signe, par contraste avec les règles de comparaison relatives au zéro signé (comme nous l’avons vu précédemment). Direct3D recommande le comportement IEEE754R dans ce cas précis, mais ne l’applique pas; le résultat de la comparaison de valeurs 0 est autorisé à dépendre de l’ordre des paramètres, à l’aide d’une comparaison qui ignore les signes.
-   x\*1.0f produit toujours x (à l’exception du vidage des nombres dénormalisés).
-   x/1.0f produit toujours x (à l’exception du vidage des nombres dénormalisés).
-   x+/-0.0f produit toujours x (à l’exception du vidage des nombres dénormalisés). Mais -0 + 0 = +0.
-   Les opérations fusionnées (telles que mad, dp3) produisent des résultats aussi précis que le pire ordre séquentiel d’évaluation possible du développement non fusionné de l’opération. La définition du pire ordre possible, pour les besoins de tolérance, n’est pas une définition fixe pour une opération fusionne donnée; elle dépend des valeurs spécifiques des entrées. Une tolérance de 1ULP est autorisée pour chacune des étapes du développement non fusionné (dans le cas des instructions appelées par Direct3D avec une tolérance plus souple que 1ULP, cette tolérance plus souple est autorisée).
-   Les opérations fusionnées respectent les mêmes règles en matière de valeurs NaN que les opérations non fusionnées.
-   Les opérations sqrt et rcp présentent une tolérance de 1ULP. Les instructions de réciproque de nuanceur et de racine carrée réciproque, [**rcp**](https://msdn.microsoft.com/library/windows/desktop/hh447205) et [**rsq**](https://msdn.microsoft.com/library/windows/desktop/hh447221), présentent leur propre exigence distincte assouplie en matière de précision.
-   Les multiplications et les divisions opèrent chacune au niveau de précision des nombres en virgule flottante 32bits (niveau de précision de 0,5ULP pour les multiplication, et de 1,0ULP pour la réciproque). Si x/y est implémenté directement, les résultats doivent présenter une précision supérieure ou égale à celle d’une méthode à deux étapes.

## <a name="span-iddoubleprec64bitspanspan-iddoubleprec64bitspan64-bit-double-precision-floating-point-rules"></a><span id="double_prec_64_bit"></span><span id="DOUBLE_PREC_64_BIT"></span>Règles en matière de virgule flottante 64bits (double précision)


Les pilotes matériels et d’affichage prennent en charge en option les nombres en virgule flottante double précision. Pour indiquer cette prise en charge, lorsque vous appelez la méthode [**ID3D11Device::CheckFeatureSupport**](https://msdn.microsoft.com/library/windows/desktop/ff476497) avec la constante [**D3D11\_FEATURE\_DOUBLES**](https://msdn.microsoft.com/library/windows/desktop/ff476124#d3d11-feature-doubles), le pilote définit le membre **DoublePrecisionFloatShaderOps** de la structure [**D3D11\_FEATURE\_DATA\_DOUBLES**](https://msdn.microsoft.com/library/windows/desktop/ff476127) sur la valeur TRUE. Le pilote et le matériel doivent alors prendre en charge toutes les instructions en virgule flottante double précision.

Les instructions double précision respectent les exigences de comportement stipulées par la norme IEEE754R.

Les données double précision requièrent la prise en charge de la génération de valeurs dénormalisées (aucun comportement de remplacement de ces valeurs par zéro). De même, les instructions ne lisent pas les données dénormalisées sous la forme d’un zéro signé, mais respectent la valeur dénormalisée.

## <a name="span-idalpha16bitspanspan-idalpha16bitspan16-bit-floating-point-rules"></a><span id="alpha_16_bit"></span><span id="ALPHA_16_BIT"></span>Règles en matière de virgule flottante 16bits


Direct3D prend également en charge les représentations de nombres en virgule flottante 16bits.

Format:

-   1 bit de signe (s) à la position binaire du bit de poids fort (MSB)
-   5bits d’exposant biaisé (e)
-   10bits de fraction (f), avec un bit masqué supplémentaire

Une valeur float16 (v) respecte les règles suivantes:

-   Si e == 31 et f != 0, alors v est égal à NaN, quel que soit s.
-   Si e == 31 et f == 0, alors v = (-1)s\*infini (infini signé)
-   Si e est compris entre 0 et 31, alors v = (-1)s\*2(e-15)\*(1.f).
-   Si e == 0 et f != 0, alors v = (-1)s\*2(e-14)\*(0.f) (nombres dénormalisés).
-   Si e == 0 et f == 0, alors v = (-1)s\*0 (zéro signé).

Les règles en matière de virgule flottante 32bits s’appliquent également aux nombres en virgule flottante 16bits, et sont ajustées pour la disposition binaire précédemment décrite. Les exceptions à cette règle sont les suivantes:

-   Précision: les opérations non fusionnées sur les nombres en virgule flottante 16bits produisent un résultat correspondant à la valeur représentable la plus proche d’un résultat infiniment précis (arrondi au chiffre pair le plus proche appliqué aux valeurs 16bits conformément à la norme IEEE-754). Les règles en matière de virgule flottante 32bits respectent une tolérance de 1ULP, et celles en matière de virgule flottante 16bits appliquent une tolérance de 0,5ULP pour les opérations non fusionnées et de 0,6ULP pour les opérations fusionnées.
-   Les nombres en virgule flottante 16bits conservent les valeurs dénormalisées.

## <a name="span-idalpha11bitspanspan-idalpha11bitspan11-bit-and-10-bit-floating-point-rules"></a><span id="alpha_11_bit"></span><span id="ALPHA_11_BIT"></span>Règles en matière de virgule flottante 11bits et 10bits


Direct3D prend également en charge les formats de virgule flottante 11 et 10bits.

Format:

-   Aucun bit de signe
-   5bits d’exposant biaisé (e)
-   6bits de fraction (f) pour un format 11bits, 5bits de fraction (f) pour un format 10bits, avec un bit masqué supplémentaire dans les deux cas

Une valeur float11/float10 (v) respecte les règles suivantes:

-   Si e == 31 et f != 0, alors v est égal à NaN.
-   Si e == 31 et f == 0, alors v = +infini.
-   Si e est compris entre 0 et 31, alors v = 2(e-15)\*(1.f).
-   Si e == 0 et f != 0, alors v = \*2(e-14)\*(0.f) (nombres dénormalisés).
-   Si e == 0 et f == 0, alors v = 0 (zéro).

Les règles en matière de virgule flottante 32bits s’appliquent également aux nombres en virgule flottante 11 et 10bits, et sont ajustées pour la disposition binaire précédemment décrite. Les exceptions sont les suivantes:

-   Précision: les règles en matière de virgule flottante 32bits respectent une tolérance de 0,5ULP.
-   Les nombres en virgule flottante 10/11bits conservent les valeurs dénormalisées.
-   Toute opération qui produirait un résultat inférieur à zéro est limitée à zéro.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Articles connexes


[Annexes](appendix.md)

[Ressources](https://msdn.microsoft.com/library/windows/desktop/ff476894)

[Textures](https://msdn.microsoft.com/library/windows/desktop/ff476902)

 

 




