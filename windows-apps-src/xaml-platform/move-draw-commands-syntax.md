---
description: Découvrez les commandes de déplacement et de dessin (ou « mini langage ») que vous pouvez utiliser pour spécifier des géométries de chemin sous forme d’une valeur d’attribut XAML.
title: Syntaxe des commandes de déplacement et de dessin
ms.assetid: 7772BC3E-A631-46FF-9940-3DD5B9D0E0D9
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 604ad25bb65486b3b388a9a03d7503b0c1ce9c03
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8214906"
---
# <a name="move-and-draw-commands-syntax"></a>Syntaxe des commandes de déplacement et de dessin


Découvrez les commandes de déplacement et de dessin (ou « mini langage ») que vous pouvez utiliser pour spécifier des géométries de chemin sous forme d’une valeur d’attribut XAML. Les commandes de déplacement et de dessin sont utilisées par de nombreux outils de conception et de création de graphiques capables de générer un graphique ou une forme de type vectoriel comme format de sérialisation et d’échange.

## <a name="properties-that-use-move-and-draw-command-strings"></a>Propriétés qui utilisent des chaînes de commande de déplacement et de dessin

La syntaxe des commandes de déplacement et de dessin est prise en charge par un convertisseur de type interne pour XAML qui analyse les commandes et produit une représentation graphique au moment de l’exécution. Cette représentation consiste essentiellement en un ensemble fini de vecteurs prêts pour la présentation. Les vecteurs ne représentent pas en eux-mêmes la totalité des détails de la présentation, et vous devez définir d’autres valeurs sur les éléments. Pour un objet [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path), vous avez aussi besoin de valeurs pour [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill), [**Stroke**](https://msdn.microsoft.com/library/windows/apps/br243383) et d’autres propriétés. Ensuite, cet objet **Path** doit être connecté d’une façon ou d’une autre à l’arborescence visuelle. Pour un objet [**PathIcon**](https://msdn.microsoft.com/library/windows/apps/dn252722), définissez la propriété [**Foreground**](https://msdn.microsoft.com/library/windows/apps/dn251974).

Windows Runtime comprend deux propriétés qui peuvent utiliser une chaîne représentant des commandes de déplacement et de dessin : [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/br243356) et [**PathIcon.Data**](https://msdn.microsoft.com/library/windows/apps/dn252723). Si vous définissez l’une de ces propriétés en spécifiant des commandes de déplacement et de dessin, vous définissez généralement cette propriété comme une valeur d’attribut XAML avec d’autres attributs requis de cet élément. Sans entrer dans les détails, voici à quoi cela ressemble:

```xml
<Path x:Name="Arrow" Fill="White" Height="11" Width="9.67"
  Data="M4.12,0 L9.67,5.47 L4.12,10.94 L0,10.88 L5.56,5.47 L0,0.06" />
```

[**PathGeometry.Figures**](https://msdn.microsoft.com/library/windows/apps/br210169) peut également utiliser des commandes de déplacement et de dessin. Vous pouvez combiner un objet [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168) qui utilise des commandes de déplacement et de dessin avec d’autres types [**Geometry**](/uwp/api/Windows.UI.Xaml.Media.Geometry) dans un objet [**GeometryGroup**](https://msdn.microsoft.com/library/windows/apps/br210057) que vous utilisez ensuite comme valeur pour [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/br243356). Toutefois, il est plus fréquent d’utiliser des commandes de déplacement et de dessin pour des données définies par des attributs.

## <a name="using-move-and-draw-commands-versus-using-a-pathgeometry"></a>Comparaison entre l’utilisation des commandes de déplacement et de dessin et l’utilisation de **PathGeometry**

Pour le code XAML Windows Runtime, les commandes de déplacement et de dessin produisent un [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168) avec un objet [**PathFigure**](https://msdn.microsoft.com/library/windows/apps/br210143) unique avec une valeur de propriété [**Figures**](https://msdn.microsoft.com/library/windows/apps/br210169). Chaque commande de dessin produit une classe dérivée de [**PathSegment**](https://msdn.microsoft.com/library/windows/apps/br210174) dans la collection [**Segments**](https://msdn.microsoft.com/library/windows/apps/br210164) de ce **PathFigure** unique, la commande de déplacement modifie la propriété [**StartPoint**](https://msdn.microsoft.com/library/windows/apps/br210166) et l’existence d’une commande de fermeture affecte à [**IsClosed**](https://msdn.microsoft.com/library/windows/apps/br210159) la valeur **true**. Vous pouvez naviguer dans cette structure en tant que modèle d’objet si vous examinez les valeurs **Data** au moment de l’exécution.

## <a name="the-basic-syntax"></a>Syntaxe de base

La syntaxe des commandes de déplacement et de dessin peut se résumer comme suit :

1.  Commencez par une règle de remplissage facultative. En général, spécifiez-la si vous ne voulez pas la valeur par défaut **EvenOdd**. (**EvenOdd** est abordé plus en détail plus loin.)
2.  Spécifiez exactement une commande de déplacement.
3.  Spécifiez une ou plusieurs commandes de dessin.
4.  Spécifiez une commande de fermeture. Vous pouvez omettre une commande de fermeture, mais votre figure serait alors ouverte (ce qui est rare).

Les règles générales de cette syntaxe sont les suivantes :

-   Chaque commande est représentée par exactement une lettre.
-   Cette lettre peut être majuscule ou minuscule. La casse a de l’importance, comme vous allez le voir.
-   Chaque commande, sauf la commande de fermeture, est généralement suivie d’un ou plusieurs nombres.
-   Si vous avez plusieurs nombres par commande, séparez-les par une virgule ou un espace.

**\[**_fillRule_**\]** _moveCommand_ _drawCommand_ **\[**_drawCommand_**\*\]** **\[**_closeCommand_**\]**

De nombreuses commandes de dessin utilisent des points qui nécessitent la définition d’une valeur _x,y_. Chaque fois que vous voyez un espace réservé \*_points_, vous pouvez supposer que vous donnez deux valeurs décimales à la valeur _x,y_ d’un point.

L’espace blanc peut souvent être omis lorsque le résultat n’est pas ambigu. Vous pouvez en effet omettre les espaces blancs si vous utilisez des virgules comme séparateurs pour tous vos ensembles de nombres (points et taille). Parexemple, cette utilisation est légale:`F1M0,58L2,56L6,60L13,51L15,53L6,64z` Il est toutefois plus courant d’inclure un espace blanc entre les commandes pour plus de clarté.

N’utilisez pas la virgule comme séparateur décimal pour les nombres décimaux ; la chaîne de commande est interprétée par XAML et ne tient pas compte des conventions de mise en forme des nombres spécifiques à la culture qui diffèrent de celles utilisées dans les paramètres régionaux **en-us**.

## <a name="syntax-specifics"></a>Spécificités de la syntaxe

**Règle de remplissage**

Il existe deux valeurs possibles pour la règle de remplissage facultative : **F0** ou **F1**. (**F** est toujours en majuscule.) **F0** est la valeur par défaut qui produit le comportement de remplissage **EvenOdd** (donc, vous ne la spécifiez généralement pas). Utilisez **F1** pour obtenir le comportement de remplissage **Nonzero**. Ces valeurs de remplissage sont alignées avec les valeurs de l’énumération [**FillRule**](https://msdn.microsoft.com/library/windows/apps/br210030).

**Commande de déplacement**

Spécifie le point de départ d’une nouvelle figure.

| Syntaxe |
|--------|
| `M ` _startPoint_ <br/>-ou-<br/>`m` _startPoint_|

| Terme | Description |
|------|-------------|
| _startPoint_ | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/>Point de départ d’une nouvelle figure.|

Un **M** majuscule indique que *startPoint* est une coordonnée absolue ; un **m** minuscule indique que *startPoint* est décalé par rapport au point précédent ou (0,0) s’il n’y avait pas de point précédent.

**Remarque**, il est légitime pour spécifier plusieurs points après la commande de déplacement. Une ligne est tracée jusqu’à ces points comme si vous aviez spécifié une commande de ligne. Toutefois, ce style n’est pas recommandé ; utilisez plutôt une commande de ligne dédiée.

**Commandes de dessin**

Une commande de dessin peut être constituée de plusieurs commandes de forme : ligne, ligne horizontale, verticale, courbe de Bézier cubique, courbe de Bézier quadratique, courbe de Bézier cubique lisse, courbe de Bézier quadratique lisse et arc elliptique.

Pour toutes les commandes de dessin, la casse a de l’importance. Les lettres majuscules dénotent des coordonnées absolues et les lettres minuscules dénotent des coordonnées relatives à la commande précédente.

Les points de contrôle pour un segment sont relatifs au point de terminaison du segment précédent. Lorsque vous entrez séquentiellement plusieurs commandes du même type, vous pouvez omettre l’entrée de commande en double. Par exemple, `L 100,200 300,400` revient à spécifier `L 100,200 L 300,400`.

**Commande de ligne**

Crée une ligne droite entre le point actuel et le point de terminaison spécifié. `l 20 30` et `L 20,30` sont des exemples de commandes de ligne valides. Définit l’équivalent d’un objet [**LineGeometry**](https://msdn.microsoft.com/library/windows/apps/br210117).

| Syntaxe |
|--------|
| `L` _endPoint_ <br/>-ou-<br/>`l` _endPoint_ |

| Terme | Description |
|------|-------------|
| endPoint | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)<br/>Point de terminaison de la ligne.|

**Commande de ligne horizontale**

Crée une ligne horizontale entre le point actuel et la coordonnéex spécifiée. `H 90` est un exemple de commande de ligne horizontale valide.

| Syntaxe |
|--------|
| `H ` _x_ <br/> -ou- <br/>`h ` _x_ |

| Terme | Description |
|------|-------------|
| x | [**Double**](https://msdn.microsoft.com/library/windows/apps/system.double.aspx) <br/> Coordonnée x du point final de la ligne. |

**Commande de ligne verticale**

Crée une ligne verticale entre le point actuel et la coordonnéey spécifiée. `v 90` est un exemple de commande de ligne verticale valide.

| Syntaxe |
|--------|
| `V ` _y_ <br/> -ou- <br/> `v ` _y_ |

| Terme | Description |
|------|-------------|
| *y* | [**Double**](https://msdn.microsoft.com/library/windows/apps/system.double.aspx) <br/> Coordonnée y du point final de la ligne. |

**Commande de courbe de Bézier cubique**

Crée une courbe de Bézier cubique entre le point actuel et le point de terminaison spécifié à l’aide des deux points de contrôle spécifiés (*controlPoint1* et *controlPoint2*). `C 100,200 200,400 300,200` est un exemple de commande de courbe valide. Définit l’équivalent d’un objet [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168) avec un objet [**BezierSegment**](https://msdn.microsoft.com/library/windows/apps/br228068).

| Syntaxe |
|--------|
| `C ` *controlPoint1* *controlPoint2* *endPoint* <br/> -ou- <br/> `c ` *controlPoint1* *controlPoint2* *endPoint* |

| Terme | Description |
|------|-------------|
| *controlPoint1* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/> Premier point de contrôle de la courbe qui détermine la tangente de début de la courbe. |
| *controlPoint2* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/> Second point de contrôle de la courbe qui détermine la tangente de fin de la courbe. |
| *endPoint* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/> Point vers lequel la courbe est tracée. | 

**Commande de courbe de Bézier quadratique**

Crée une courbe de Bézier quadratique entre le point actuel et le point de terminaison spécifié à l’aide du point de contrôle spécifié (*controlPoint*). `q 100,200 300,200` est un exemple de commande de courbe de Bézier quadratique valide. Définit l’équivalent d’un objet [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168) avec un objet [**QuadraticBezierSegment**](https://msdn.microsoft.com/library/windows/apps/br210249).

| Syntaxe |
|--------|
| `Q ` *controlPoint endPoint* <br/> -ou- <br/> `q ` *controlPoint endPoint* |

| Terme | Description |
|------|-------------|
| *controlPoint* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/> Point de contrôle de la courbe qui détermine les tangentes de début et de fin de la courbe. |
| *endPoint* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)<br/> Point vers lequel la courbe est tracée. |

**Commande de courbe de Bézier cubique lisse**

Crée une courbe de Bézier cubique entre le point actuel et le point de terminaison spécifié. Le premier point de contrôle est censé être la réflexion du deuxième point de contrôle de la commande précédente par rapport au point actuel. S’il n’y a pas de commande précédente ou si la commande précédente n’est ni une commande de courbe de Bézier cubique ni une commande de courbe de Bézier cubique lisse, vous pouvez supposer que le premier point de contrôle coïncide avec le point actuel. Le deuxième point de contrôle (point de contrôle pour la fin de la courbe) est spécifié par *controlPoint2*. Par exemple, `S 100,200 200,300` est une commande de courbe de Bézier cubique lisse valide. Cette commande définit l’équivalent d’un [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168) avec un [**BezierSegment**](https://msdn.microsoft.com/library/windows/apps/br228068) à l’endroit d’un segment de courbe précédent.

| Syntaxe |
|--------|
| `S` *controlPoint2* *endPoint* <br/> -ou- <br/>`s` *controlPoint2 endPoint* |

| Terme | Description |
|------|-------------|
| *controlPoint2* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/> Point de contrôle de la courbe qui détermine la tangente de fin de la courbe. |
| *endPoint* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)<br/> Point vers lequel la courbe est tracée. |

**Commande de courbe de Bézier quadratique lisse**

Crée une courbe de Bézier quadratique entre le point actuel et le point de terminaison spécifié. Le point de contrôle est censé être la réflexion du point de contrôle de la commande précédente par rapport au point actuel. S’il n’y a pas de commande précédente ou si la commande précédente n’est ni une commande de courbe de Bézier quadratique ni une commande de courbe de Bézier quadratique lisse, le point de contrôle coïncide avec le point actuel. Cette commande définit l’équivalent d’un [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168) avec un [**QuadraticBezierSegment**](https://msdn.microsoft.com/library/windows/apps/br210249) à l’endroit d’un segment de courbe précédent.

| Syntaxe |
|--------|
| `T` *controlPoint* *endPoint* <br/> -ou- <br/> `t` *controlPoint* *endPoint* |

| Terme | Description |
|------|-------------|
| *controlPoint* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)<br/> Point de contrôle de la courbe qui détermine la tangente de début de la courbe. |
| *endPoint* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)<br/> Point vers lequel la courbe est tracée. |

**Commande d’arc elliptique**

Crée un arc elliptique entre le point actuel et le point de terminaison spécifié. Définit l’équivalent d’un objet [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/br210168) avec un objet [**ArcSegment**](https://msdn.microsoft.com/library/windows/apps/br228054).

| Syntaxe |
|--------|
| `A ` *size* *rotationAngle* *isLargeArcFlag* *sweepDirectionFlag* *endPoint* <br/> -ou- <br/>`a ` *sizerotationAngleisLargeArcFlagsweepDirectionFlagendPoint* |

| Terme | Description |
|------|-------------|
| *size* | [**Taille**](https://msdn.microsoft.com/library/windows/apps/br225995)<br/>Rayon x et rayon y de l’arc. |
| *rotationAngle* | [**Double**](https://msdn.microsoft.com/library/windows/apps/system.double.aspx) <br/> Rotation de l’ellipse, en degrés. |
| *isLargeArcFlag* | Affectez la valeur1 si l’angle de l’arc doit être de 180degrés ou plus; sinon, affectez la valeur0. |
| *sweepDirectionFlag* | Affectez la valeur1 si l’arc est dessiné dans la direction de l’angle positif; sinon, affectez la valeur0. |
| *endPoint* | [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) <br/> Point vers lequel l’arc est tracé.|
 
**Bouton de fermeture**

Termine la figure actuelle et crée une ligne qui relie le point actuel au point de début de la figure. Cette commande crée un angle entre le dernier segment et le premier segment de la figure.

| Syntaxe |
|--------|
| `Z` <br/> - ou - <br/> `z ` |

**Syntaxe de point**

Décrit la coordonnée x et la coordonnée y d’un point. Voir aussi [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870).

| Syntaxe |
|--------|
| *x*,*y*<br/> - ou - <br/>*x* *y* |

| Terme | Description |
|------|-------------|
| *x* | [**Double**](https://msdn.microsoft.com/library/windows/apps/system.double.aspx) <br/> Coordonnée x du point. |
| *y* | [**Double**](https://msdn.microsoft.com/library/windows/apps/system.double.aspx) <br/> Coordonnée y du point. |

**Remarques supplémentaires**

Au lieu d’une valeur numérique standard, vous pouvez également utiliser les valeurs spéciales suivantes. Ces valeurs respectent la casse.

-   **Infinity** : représente **PositiveInfinity**.
-   **\-Infinity** : représente **NegativeInfinity**.
-   **NaN** : représente **NaN**.

Au lieu d’utiliser des nombres décimaux ou entiers, vous pouvez utiliser la notation scientifique. Par exemple, `+1.e17` est une valeur valide.

## <a name="design-tools-that-produce-move-and-draw-commands"></a>Outils de conception qui produisent des commandes de déplacement et de dessin

À l’aide de l’outil de **stylet** et d’autres outils de dessin dans Blend pour Microsoft Visual Studio2015 sera généralement produire un objet de [**chemin d’accès**](/uwp/api/Windows.UI.Xaml.Shapes.Path) , avec déplacement commandes et de dessin.

Il est possible que vous constatiez la présence de données de commandes de déplacement et de dessin dans certaines parties de contrôle définies dans les modèles par défaut XAML Windows Runtime de contrôles. Par exemple, certains contrôles utilisent un [**PathIcon**](https://msdn.microsoft.com/library/windows/apps/dn252722) dans lequel les données sont définies en tant que commandes de déplacement et de dessin.

Des exportateurs ou des plug-ins sont disponibles pour d’autres outils de conception de graphiques vectoriels couramment utilisés pour générer le vecteur au format XAML. Ceux-ci créent généralement des objets [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) dans un conteneur de disposition avec des commandes de déplacement et de dessin pour [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/br243356). Plusieurs éléments **Path** peuvent être présents dans le code XAML, ce qui permet d’appliquer différents types de pinceaux. Plusieurs de ces exportateurs ou plug-ins ont été écrits à l’origine pour le langage XAML dans WindowsPresentationFoundation (WPF) ou Silverlight, mais la syntaxe XAML est identique dans le langage XAML Windows Runtime. Vous pouvez généralement utiliser des blocs de code XAML d’un exportateur et les coller directement dans une page XAML WindowsRuntime. (Toutefois, si **RadialGradientBrush** faisait partie du code XAML converti, vous ne pourrez pas l’utiliser étant donné que le langage XAML Windows Runtime ne prend pas en charge ce pinceau.)

## <a name="related-topics"></a>Rubriques connexes

* [Dessiner des formes](https://msdn.microsoft.com/library/windows/apps/mt280380)
* [Utiliser des pinceaux](https://msdn.microsoft.com/library/windows/apps/mt280383)
* [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/br243356)
* [**PathIcon**](https://msdn.microsoft.com/library/windows/apps/dn252722)

