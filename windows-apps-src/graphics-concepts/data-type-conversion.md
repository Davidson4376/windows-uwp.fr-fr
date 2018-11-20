---
title: Conversion de types de données
description: Les sections suivantes décrivent la façon dont Direct3D gère les conversions entre les types de données.
ms.assetid: B50AB8DE-CAED-465B-B18C-81F3A984B8AC
keywords:
- Conversion de types de données
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a6ceb1e779f8622d3e358bc131b21f6ec66ac2f8
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7417922"
---
# <a name="data-type-conversion"></a>Conversion de types de données


Les sections suivantes décrivent la façon dont Direct3D gère les conversions entre les types de données.

## <a name="span-iddatatypeterminologyspanspan-iddatatypeterminologyspanspan-iddatatypeterminologyspandata-type-terminology"></a><span id="Data_Type_Terminology"></span><span id="data_type_terminology"></span><span id="DATA_TYPE_TERMINOLOGY"></span>Terminologie relative aux types de données


Les ensembles de termes utilisés dans cet article pour caractériser les différentes conversions de formats sont répertoriés ci-dessous.

| Terme  | Définition                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|-------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SNORM | Entier normalisé signé, signifiant que pour un complément à 2 d’un nombre sur nbits, la valeur maximale correspond à 1.0f (par exemple, la valeur sur 5bits 01111 est mappée sur 1.0f), et la valeur minimale correspond à -1.0f (par exemple, la valeur sur 5bits 10000 est mappée sur -1.0f). En outre, le second nombre minimal est mappé sur -1.0f (par exemple, la valeur sur 5bits 10001 est mappée sur -1.0f). Il existe donc deux représentations d’entier pour -1.0f. Il n’existe qu’une seule représentation pour 0.0f, et qu’une seule représentation pour 1.0f. Il en résulte un ensemble de représentations d’entier pour les valeurs en virgule flottante espacées de manière égale dans la plage (-1.0f...0.0f), ainsi qu’un ensemble complémentaire de représentations pour les nombres dans la plage (0.0f...1.0f). |
| UNORM | Entier normalisé non signé, ce qui signifie que pour un nombre sur nbits, un ensemble de 0 correspond à 0.0f, et un ensemble de 1 à 1.0f. Une séquence de valeurs en virgule flottante espacées de manière égale entre 0.0f et 1.0f est représentée. Par exemple, un nombre UNORM sur 2bits représente 0.0f, 1/3, 2/3 et 1.0f.                                                                                                                                                                                                                                                                                                                                                                                                                                |
| SINT  | Entier signé. Entier complément à 2. Par exemple, un nombre SINT sur 3bits représente les valeurs intégrales -4, -3, -2, -1, 0, 1, 2, 3.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| UINT  | Entier non signé. Par exemple, un nombre UINT sur 3bits représente les valeurs intégrales 0, 1, 2, 3, 4, 5, 6, 7.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| FLOAT | Valeur en virgule flottante dans toute représentation définie par Direct3D.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| SRVB  | Comparable à UNORM, dans la mesure où pour un nombre sur nbits, un ensemble de 0 correspond à 0.0f, et un ensemble de 1 correspond à 1.0f. Toutefois, contrairement à UNORM, la séquence SRVB de codages d’entiers non signés entre un ensemble de 0 et un ensemble de 1 représente une progression non linéaire dans l’interprétation en virgule flottante des nombres compris entre 0.0f et 1.0f. En gros, si cette progression non linéaire, SRVB, est affichée sous la forme d’une séquence de couleurs, elle apparaît à un observateur «moyen» en tant que pente linéaire de niveaux de luminosité, dans des conditions de visualisation «moyennes» et sur un affichage «moyen». Pour plus d’informations, consultez la norme de couleurs SRVB, CEI61996-2-1, sur le site de la CEI (Commission électrotechnique internationale).                |

 

Les termes ci-dessus sont fréquemment utilisés comme «modificateurs de nom de format», où ils décrivent à la fois le mode de disposition des données en mémoire et la conversion à effectuer dans le chemin de transport (pouvant inclure un filtrage) à partir de la mémoire à destination ou en provenance d’une unité de pipeline telle qu’un nuanceur.

## <a name="span-idfloatingpointconversionspanspan-idfloatingpointconversionspanspan-idfloatingpointconversionspanfloating-point-conversion"></a><span id="Floating_Point_Conversion"></span><span id="floating_point_conversion"></span><span id="FLOATING_POINT_CONVERSION"></span>Conversion de virgule flottante


Chaque fois qu’une conversion de virgule flottante se produit entre différentes représentations, y compris vers ou depuis des représentations non en virgule flottante, les règles ci-après s’appliquent.

### <a name="span-idconververtingfromahigherrangerepresentationtoalowerrangerepresentationspanspan-idconververtingfromahigherrangerepresentationtoalowerrangerepresentationspanspan-idconververtingfromahigherrangerepresentationtoalowerrangerepresentationspanconververting-from-a-higher-range-representation-to-a-lower-range-representation"></a><span id="Conververting_from_a_higher_range_representation_to_a_lower_range_representation"></span><span id="conververting_from_a_higher_range_representation_to_a_lower_range_representation"></span><span id="CONVERVERTING_FROM_A_HIGHER_RANGE_REPRESENTATION_TO_A_LOWER_RANGE_REPRESENTATION"></span>Conversion à partir d’une représentation de plage supérieure vers une représentation de plage inférieure

-   L’arrondi à zéro est utilisé lors d’une conversion vers un autre format flottant. Si la cible est un format entier ou virgule fixe, l’arrondi au chiffre pair le plus proche est utilisé, sauf si la conversion est explicitement mentionnée comme devant utiliser un autre comportement d’arrondi, tel qu’un arrondi au chiffre le plus proche pour la conversion FLOAT vers SNORM, FLOAT vers UNORM ou FLOAT vers SRVB. D’autres exceptions correspondent aux instructions de nuanceur ftoi et ftou, qui utilisent l’arrondi à zéro. Enfin, les conversions du format flottant vers le format fixe qui sont utilisées par l’échantillonneur de textures et par le rastériseur présentent une tolérance spécifiée mesurée en ULP (Unit in the Last Place, écart entre deux flottants positifs successifs) par rapport à un idéal infiniment précis.
-   Pour les valeurs sources supérieures à la plage dynamique d’un format cible de plage inférieure (par exemple, une valeur flottante élevée sur 32bits est écrite sous la forme d’un RenderTarget flottant sur 16bits), la valeur résultante correspond à la valeur représentable (signée de façon appropriée) maximale, n’incluant PAS d’infini signé (en raison de l’arrondi à zéro décrit ci-dessus).
-   Une représentation NaN (n’est pas un nombre) dans un format de plage supérieure est convertie en représentation NaN dans le format de plage inférieure si cette représentation existe. Si le format inférieur ne comporte aucune représentation NaN, le résultat est égal à 0.
-   Une représentation INF dans un format de plage supérieure est convertie en représentation INF dans le format de plage inférieure si elle est disponible. Si le format inférieur ne comporte aucune représentation INF, la représentation dans un format de plage supérieure est convertie en valeur maximale représentable. Le signe est conservé s’il est disponible dans le format cible.
-   Une représentation de nombre dénormalisé dans un format de plage supérieure est convertie en représentation de nombre dénormalisé dans le format de plage inférieure si cette représentation est disponible et que la conversion est possible; dans le cas contraire, le résultat est égal à 0. Le bit de signe est conservé s’il est disponible dans le format cible.

### <a name="span-idconvertingfromalowerrangerepresentationtoahigherrangerepresentationspanspan-idconvertingfromalowerrangerepresentationtoahigherrangerepresentationspanspan-idconvertingfromalowerrangerepresentationtoahigherrangerepresentationspanconverting-from-a-lower-range-representation-to-a-higher-range-representation"></a><span id="Converting_from_a_lower_range_representation_to_a_higher_range_representation"></span><span id="converting_from_a_lower_range_representation_to_a_higher_range_representation"></span><span id="CONVERTING_FROM_A_LOWER_RANGE_REPRESENTATION_TO_A_HIGHER_RANGE_REPRESENTATION"></span>Conversion à partir d’une représentation de plage inférieure vers une représentation de plage supérieure

-   Une représentation NaN dans un format de plage inférieure est convertie en représentation NaN dans le format de plage supérieure si cette représentation est disponible. Si le format de plage supérieure ne comporte aucune représentation NaN, la représentation NaN dans le format de plage inférieure est convertie en valeur0.
-   Une représentation INF dans un format de plage inférieure est convertie en représentation INF dans le format de plage supérieure si cette représentation est disponible. Si le format supérieur ne comporte aucune représentation INF, la représentation INF dans le format de plage inférieure est convertie en valeur maximale représentable (MAX\_FLOAT dans ce format). Le signe est conservé s’il est disponible dans le format cible.
-   Une représentation de nombre dénormalisé dans un format de plage inférieure est convertie en une représentation de nombre normalisé dans le format de plage supérieure si elle est disponible, ou en une représentation de nombre dénormalisé dans le format de plage supérieure si elle existe. Si le format de plage supérieure ne comporte aucune représentation de nombre dénormalisé, la représentation de nombre dénormalisé dans un format de plage inférieure est convertie en valeur0. Le signe est conservé s’il est disponible dans le format cible. Notez que les nombres flottants sur 32bits sont considérés comme un format sans représentation de nombre dénormalisé (étant donné que les représentations de nombre dénormalisé dans les opérations portant sur des nombres flottants sur 32bits sont remplacées par 0 avec conservation du signe).

## <a name="span-idintegerconversionspanspan-idintegerconversionspanspan-idintegerconversionspaninteger-conversion"></a><span id="Integer_Conversion"></span><span id="integer_conversion"></span><span id="INTEGER_CONVERSION"></span>Conversion d’entier


Le tableau ci-après décrit les conversions des différentes représentations décrites ci-dessus en d’autres représentations. Seules les conversions réellement effectuées dans Direct3D sont présentées.

Dans le cas des entiers, sauf mention contraire, toutes les conversions vers/depuis des représentations d’entier en représentations de nombre flottant décrites ci-dessous seront effectuées exactement.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Type de données source</th>
<th align="left">Type de données cible</th>
<th align="left">Règle de conversion</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">SNORM</td>
<td align="left">FLOAT</td>
<td align="left"><p>Pour une valeur d’entier sur nbits représentant la plage signée [-1.0f à 1.0f], la conversion en virgule flottante s’effectue comme suit.</p>
<ul>
<li>La valeur la plus négative est mappée sur -1.0f. Par exemple, la valeur sur 5bits 10000 est mappée sur -1.0f.</li>
<li>Toute autre valeur est convertie en nombre flottant (appelons-le c), puis produit le résultat suivant = c * (1.0f / (2⁽ⁿ⁻¹⁾-1)). Par exemple, la valeur sur 5bits 10001 est convertie sous la forme -15.0f, puis divisée par 15.0f, ce qui donne -1.0f.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">FLOAT</td>
<td align="left">SNORM</td>
<td align="left"><p>Pour un nombre en virgule flottante donné, la conversion en une valeur d’entier sur nbits représentant la plage signée [-1.0f à 1.0f] s’effectue comme suit.</p>
<ul>
<li>Supposons que c représente la valeur de départ.</li>
<li>Si c est une valeur NaN, le résultat est égal à 0.</li>
<li>Si c&gt;1.0f, incluant INF, il est limité à 1.0f.</li>
<li>Si c&lt;-1.0f, incluant -INF, il est limité à -1.0f.</li>
<li>Conversion à partir d’une échelle de nombre flottant en échelle de nombre entier: c = c * (2ⁿ⁻¹-1).</li>
<li>La conversion en entier s’effectue comme suit.
<ul>
<li>Si c&gt;= 0, alors c = c + 0.5f; sinon, c = c - 0.5f.</li>
<li>La fraction décimale est ignorée, et la valeur en virgule flottante restante (intégrale) est directement convertie en entier.</li>
</ul></li>
</ul>
<p>Pour cette conversion, la tolérance autorisée est de D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP ULP (côté entier). Cela signifie qu’après la conversion d’une échelle de nombre flottant en échelle de nombre entier, toute valeur comprise dans D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP ULP d’une valeur de format cible représentable peut être mappée sur cette valeur. L’exigence supplémentaire en matière de réversibilité des données garantit le fait que la conversion est non décroissante sur la totalité de la plage, et que toutes les valeurs de sortie sont possibles. (Dans les constantes présentées ici, <em>xx</em> doit être remplacé par la versionDirect3D, telle que 10, 11 ou 12.)</p></td>
</tr>
<tr class="odd">
<td align="left">UNORM</td>
<td align="left">FLOAT</td>
<td align="left"><p>La valeur de départ sur nbits est convertie en flottant (0.0f, 1.0f, 2.0f, etc.), puis divisée par (2ⁿ-1).</p></td>
</tr>
<tr class="even">
<td align="left">FLOAT</td>
<td align="left">UNORM</td>
<td align="left"><p>Supposons que c représente la valeur de départ.</p>
<ul>
<li>Si c est une valeur NaN, le résultat est égal à 0.</li>
<li>Si c&gt;1.0f, incluant INF, il est limité à 1.0f.</li>
<li>Si c&lt;0.0f, incluant -INF, il est limité à 0.0f.</li>
<li>Conversion à partir d’une échelle de nombre flottant en échelle de nombre entier: c = c * (2ⁿ-1).</li>
<li>Conversion en entier.
<ul>
<li>c = c + 0.5f.</li>
<li>La fraction décimale est ignorée, et la valeur en virgule flottante restante (intégrale) est directement convertie en entier.</li>
</ul></li>
</ul>
<p>Pour cette conversion, la tolérance autorisée est de D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP ULP (côté entier). Cela signifie qu’après la conversion d’une échelle de nombre flottant en échelle de nombre entier, toute valeur comprise dans D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP ULP d’une valeur de format cible représentable peut être mappée sur cette valeur. L’exigence supplémentaire en matière de réversibilité des données garantit le fait que la conversion est non décroissante sur la totalité de la plage, et que toutes les valeurs de sortie sont possibles.</p></td>
</tr>
<tr class="odd">
<td align="left">SRVB</td>
<td align="left">FLOAT</td>
<td align="left"><p>L’exemple qui suit constitue la conversion SRVB en FLOAT idéale.</p>
<ul>
<li>Considérons la valeur de départ sur nbits, convertissons-la en flottant (0.0f, 1.0f, 2.0f, etc.) et appelons ce dernier c.</li>
<li>c = c * (1.0f / (2ⁿ-1))</li>
<li>Si (c &lt; = D3D<em>xx</em>_SRGB_TO_FLOAT_THRESHOLD) alors: résultat = c / D3D<em>xx</em>_SRGB_TO_FLOAT_DENOMINATOR_1, sinon: résultat = ((c + D3D<em>xx</em>_SRGB_TO_FLOAT_OFFSET)/D3D<em>xx</em>_SRGB_TO_FLOAT_DENOMINATOR_2)D3D<em>xx</em>_SRGB_TO_FLOAT_EXPONENT</li>
</ul>
<p>Pour cette conversion, la tolérance autorisée est de D3D<em>xx</em>_SRGB_TO_FLOAT_TOLERANCE_IN_ULP ULP (côté SRVB).</p></td>
</tr>
<tr class="even">
<td align="left">FLOAT</td>
<td align="left">SRVB</td>
<td align="left"><p>L’exemple qui suit constitue la conversion FLOAT -&gt; SRVB idéale.</p>
<p>Supposons que le composant de couleur SRVB cible comporte nbits:</p>
<ul>
<li>Supposons que la valeur de départ soit c.</li>
<li>Si c est une valeur NaN, le résultat est égal à 0.</li>
<li>Si c&gt;1.0f, incluant INF, il est limité à 1.0f.</li>
<li>Si c&lt;0.0f, incluant -INF, il est limité à 0.0f.</li>
<li>Si (c &lt;= D3D<em>xx</em>_FLOAT_TO_SRGB_THRESHOLD) alors: c = D3D<em>xx</em>_FLOAT_TO_SRGB_SCALE_1 * c, sinon: c = D3D<em>xx</em>_FLOAT_TO_SRGB_SCALE_2 * c(D3D<em>xx</em>_FLOAT_TO_SRGB_EXPONENT_NUMERATOR/D3D<em>xx</em>_FLOAT_TO_SRGB_EXPONENT_DENOMINATOR) - D3D<em>xx</em>_FLOAT_TO_SRGB_OFFSET</li>
<li>Conversion à partir d’une échelle de nombre flottant en échelle de nombre entier: c = c * (2ⁿ-1).</li>
<li>Conversion en entier:
<ul>
<li>c = c + 0.5f.</li>
<li>La fraction décimale est ignorée, et la valeur en virgule flottante restante (intégrale) est directement convertie en entier.</li>
</ul></li>
</ul>
<p>Pour cette conversion, la tolérance autorisée est de D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP ULP (côté entier). Cela signifie qu’après la conversion d’une échelle de nombre flottant en échelle de nombre entier, toute valeur comprise dans D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP ULP d’une valeur de format cible représentable peut être mappée sur cette valeur. L’exigence supplémentaire en matière de réversibilité des données garantit le fait que la conversion est non décroissante sur la totalité de la plage, et que toutes les valeurs de sortie sont possibles.</p></td>
</tr>
<tr class="odd">
<td align="left">SINT</td>
<td align="left">SINT avec un nombre de bits supérieur</td>
<td align="left"><p>Pour convertir un type SINT en type SINT avec un nombre de bits supérieur, le bit de poids fort (MSB) du nombre de départ est &quot;répété par extension de signe&quot; sur les bits supplémentaires disponibles dans le format cible.</p></td>
</tr>
<tr class="even">
<td align="left">UINT</td>
<td align="left">SINT avec un nombre de bits supérieur</td>
<td align="left"><p>Pour convertir un type UINT en type SINT avec un nombre de bits supérieur, le nombre est copié sur les bits de poids faible (LSB) du format cible, et les bits de poids fort supplémentaires sont remplis à l’aide de zéros.</p></td>
</tr>
<tr class="odd">
<td align="left">SINT</td>
<td align="left">UINT avec un nombre de bits supérieur</td>
<td align="left"><p>Pour convertir un type SINT en type UINT avec un nombre de bits supérieur: si le nombre est négatif, la valeur est limitée à 0. Dans le cas contraire, le nombre est copié sur les bits de poids faible du format cible, et les bits de poids fort supplémentaires sont remplis à l’aide de zéros.</p></td>
</tr>
<tr class="even">
<td align="left">UINT</td>
<td align="left">UINT avec un nombre de bits supérieur</td>
<td align="left"><p>Pour convertir un type UINT en type UINT avec un nombre de bits supérieur, le nombre est copié sur les bits de poids faible du format cible, et les bits de poids fort supplémentaires sont remplis à l’aide de zéros.</p></td>
</tr>
<tr class="odd">
<td align="left">SINT ou UINT</td>
<td align="left">SINT ou UINT avec un nombre de bits inférieur ou égal</td>
<td align="left"><p>Pour convertir un type SINT ou UINT en type SINT ou UINT avec un nombre de bits inférieur ou égal (et/ou un changement de signe), la valeur de départ est simplement limitée à la plage du format cible.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idfixedpointintegerconversionspanspan-idfixedpointintegerconversionspanspan-idfixedpointintegerconversionspanfixed-point-integer-conversion"></a><span id="Fixed_Point_Integer_Conversion"></span><span id="fixed_point_integer_conversion"></span><span id="FIXED_POINT_INTEGER_CONVERSION"></span>Conversion d’entier en virgule fixe


Les entiers en virgule fixe sont de simples entiers d’une certaine taille de bits qui comportent un signe décimal implicite à un emplacement fixe.

Le type de données «entier» omniprésent est un cas spécial d’entier en virgule fixe présentant la décimale à la fin du nombre.

Les représentations de nombres en virgule fixe sont caractérisées sous la forme i.f, où i est le nombre de bits d’entier, et f le nombre de bits fractionnaires. Par exemple, 16,8 signifie 16bits d’entier suivis de 8bits de fraction. La partie entière est stockée dans un complément à 2, au moins tel que défini ici (bien qu’il puisse également être défini de la même manière pour les entiers non signés). La partie fractionnaire est stockée sous une forme non signée. La partie fractionnaire représente toujours la fraction positive entre les deux valeurs intégrales les plus proches, en commençant par la plus négative.

Les opérations d’ajout et de soustraction sur les nombres en virgule fixe sont simplement effectuées à l’aide de l’arithmétique standard sur les entiers, sans tenir aucun compte de l’emplacement de la décimale implicite. L’ajout de la valeur1 à un nombre en virgule fixe 16,8 équivaut simplement à ajouter 256, puisque la décimale se trouve à 8places de la fin de poids faible du nombre. Il est également possible d’effectuer d’autres opérations, telles que des multiplications, à l’aide de l’arithmétique sur les entiers, à condition de tenir compte de leur effet sur la décimale fixe. Par exemple, le fait de multiplier deux entiers 16,8 à l’aide d’une multiplication d’entier produit un résultat de 32,16.

Les représentations d’entiers en virgule fixe sont utilisées de deux façons dans Direct3D.

-   Les positions de vertex après le découpage dans le rastériseur sont ancrées sur la virgule fixe, afin de garantir une distribution uniforme dans l’ensemble de la zone. De nombreuses opérations de rastériseur, telles que l’élimination d’une face, s’appliquent aux positions ancrées sur la virgule fixe, alors que d’autres opérations, comme la configuration de l’interpolateur d’attributs, utilisent des positions précédemment ancrées sur la virgule fixe qui ont été reconverties en virgule flottante.
-   Les coordonnées de texture pour les opérations d’échantillonnage sont ancrées sur la virgule fixe (après avoir été mises à l’échelle par taille de texture), afin de garantir une distribution uniforme dans l’ensemble de l’espace de texture, par le choix d’emplacements/de valeurs de pondération de filtrage. Les valeurs de pondération sont reconverties en virgule flottante avant l’exécution proprement dite de l’arithmétique de filtrage.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Type de données source</th>
<th align="left">Type de données cible</th>
<th align="left">Règle de conversion</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">FLOAT</td>
<td align="left">Entier en virgule fixe</td>
<td align="left"><p>Voici la procédure générale de conversion d’un nombre en virgule flottante n en entier en virgule fixe i.f, où i est le nombre de bits d’entier (signés), et f le nombre de bits fractionnaires.</p>
<ul>
<li>Calcul de FixedMin = -2⁽ⁱ⁻¹⁾</li>
<li>Calcul FixedMax = 2⁽ⁱ⁻¹⁾ - 2<sup>(-f)</sup></li>
<li>Si n est NaN, résultat = 0; si n est + Inf, résultat = FixedMax*2<sup>f</sup>; si n est -Inf, résultat = FixedMin*2<sup>f</sup></li>
<li>Si n &gt;= FixedMax, résultat = Fixedmax*2<sup>f</sup>; si n &lt;= FixedMin, résultat = FixedMin*2<sup>f</sup></li>
<li>Sinon, calcul de n*2<sup>f</sup> et conversion en entier.</li>
</ul>
<p>Pour ces implémentations, la tolérance autorisée est de D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP ULP dans l’entier résultant, au lieu de la valeur infiniment précise n*2<sup>f</sup> après la dernière étape ci-dessus.</p></td>
</tr>
<tr class="even">
<td align="left">Entier en virgule fixe</td>
<td align="left">FLOAT</td>
<td align="left"><p>Supposons que la représentation en virgule fixe spécifique à convertir en représentation flottante ne contienne pas plus de 24bits d’informations au total, dont pas plus de 23bits figurent dans la partie fractionnaire. Supposons qu’un nombre en virgule fixe donné, fxp, se trouve sous la forme i.f (ibits d’entier, fbits de fraction). La conversion en valeur flottante ressemble au pseudo-code suivant.</p>
<p>float résultat = (float) (fxp &gt; &gt; f) + / / extraction de nombre entier</p>
((float) (fxp &amp; (2<sup>f</sup> - 1)) / (2<sup>f</sup>)); extraction de la fraction</td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Annexes](appendix.md)

 

 




