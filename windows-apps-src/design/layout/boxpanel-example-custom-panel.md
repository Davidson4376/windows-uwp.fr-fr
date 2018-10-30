---
author: muhsinking
Description: Learn to write code for a custom Panel class, implementing ArrangeOverride and MeasureOverride methods, and using the Children property.
MS-HAID: dev\_ctrl\_layout\_txt.boxpanel\_example\_custom\_panel
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: BoxPanel, exemple de panneau personnalisé (applications Windows)
ms.assetid: 981999DB-81B1-4B9C-A786-3025B62B74D6
label: BoxPanel, an example custom panel
template: detail.hbs
op-migration-status: ready
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7d29b85c7ec3ec9ec0114a3a49dff834f859511e
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5756531"
---
# <a name="boxpanel-an-example-custom-panel"></a>BoxPanel, exemple de panneau personnalisé

 

Apprenez à écrire du code pour une classe [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) personnalisée, en implémentant les méthodes [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) et [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730), et en utilisant la propriété [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514). 

> **API importantes**: [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511), [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711), [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 

L’exemple de code illustre une implémentation de panneau personnalisé, mais nous ne consacrons pas beaucoup de temps à expliquer les concepts de disposition qui influencent la façon dont vous pouvez personnaliser un panneau pour différents scénarios de disposition. Pour plus d’informations sur ces concepts de disposition et sur la manière dont ils peuvent s’appliquer à votre propre scénario de disposition, voir [Vue d’ensemble des panneaux personnalisés XAML](custom-panels-overview.md).

Un *panneau* est un objet qui fournit un comportement de disposition pour les éléments enfants qu’il contient, lorsque le système de disposition XAML s’exécute et que l’interface utilisateur de votre application est affichée. Vous pouvez définir des panneaux personnalisés pour la disposition XAML en dérivant une classe personnalisée à partir de la classe [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511). Vous fournissez le comportement de votre panneau en substituant les méthodes [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) et [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) et en fournissant la logique qui mesure et organise les éléments enfants. Cet exemple dérive de **Panel**. Lorsque vous commencez à partir de **Panel**, les méthodes **ArrangeOverride** et **MeasureOverride** n’ont pas de comportement de départ. Votre code fournit la passerelle par laquelle les éléments enfants sont portés à la connaissance du système de disposition XAML et sont affichés dans l’interface utilisateur. Il est donc très important que votre code prenne en compte tous les éléments enfants et suive les modèles attendus par le système de disposition.

## <a name="your-layout-scenario"></a>Votre scénario de disposition

Quand vous définissez un panneau personnalisé, vous définissez un scénario de disposition

Un scénario de disposition indique :

-   ce que fait le panneau quand il possède des éléments enfants,
-   quand il a des contraintes sur son propre espace,
-   comment la logique du panneau détermine toutes les mesures, placement, positions et dimensionnements qui ont pour résultat la disposition des enfants dans l’interface utilisateur.

L’exemple `BoxPanel` fourni ici concerne un scénario spécifique. Pour des raisons de simplification du code, nous n’expliquerons pas le scénario en détail dans cet exemple. Nous nous concentrons plutôt sur les étapes nécessaires et sur les modèles de codage. Si vous souhaitez d’abord en savoir plus sur le scénario, passez directement à [« Le scénario de `BoxPanel` »](#the-scenario-for-boxpanel) et revenez ensuite au code.

## <a name="start-by-deriving-from-panel"></a>Commencer par dériver une classe à partir de **Panel**

Commencez par dériver une classe personnalisée à partir de [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511). Le moyen le plus simple consiste sans doute à définir un fichier de code distinct pour cette classe, à l’aide des options de menu contextuel **Ajouter** | **Nouvel élément** | **Classe** pour un projet dans l’**Explorateur de solutions** de Microsoft Visual Studio. Nommez la classe (et le fichier) `BoxPanel`.

Le fichier de modèle d’une classe ne commence pas par beaucoup d’instructions **using**, car il n’est pas destiné spécifiquement aux applications de plateforme Windows universelle (UWP). Commencez par ajouter des instructions **using**. Le fichier de modèle débute également par quelques instructions **using** dont vous n’aurez probablement pas besoin et que vous pouvez donc supprimer. Voici une liste d’instructions **using** qui peuvent résoudre des types dont vous aurez besoin pour du code de panneau personnalisé classique:

```CSharp
using System;
using System.Collections.Generic; // if you need to cast IEnumerable for iteration, or define your own collection properties
using Windows.Foundation; // Point, Size, and Rect
using Windows.UI.Xaml; // DependencyObject, UIElement, and FrameworkElement
using Windows.UI.Xaml.Controls; // Panel
using Windows.UI.Xaml.Media; // if you need Brushes or other utilities
```

Maintenant que vous pouvez résoudre [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511), faites-en la classe de base de `BoxPanel`. Rendez également `BoxPanel` public :

```CSharp
public class BoxPanel : Panel
{
}
```

Au niveau de la classe, définissez certaines valeurs **int** et **double** qui seront partagées par plusieurs de vos fonctions logiques, mais qui n’auront pas besoin d’être exposées comme API publiques. Dans l’exemple, elles se nomment : `maxrc`, `rowcount`, `colcount`, `cellwidth`, `cellheight`, `maxcellheight` et `aspectratio`.

Après cela, le fichier de code complet ressemble à ceci (les commentaires sur **using** ont été supprimés, maintenant que vous savez pourquoi ces instructions sont là):

```CSharp
using System;
using System.Collections.Generic;
using Windows.Foundation;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

public class BoxPanel : Panel 
{
    int maxrc, rowcount, colcount;
    double cellwidth, cellheight, maxcellheight, aspectratio;
}
```

Dorénavant, nous vous montrerons une définition de membre à la fois, qu’il s’agisse d’une substitution de méthode ou d’un élément de prise en charge tel qu’une propriété de dépendance. Vous pouvez ajouter ces éléments au squelette ci-dessus dans n’importe quel ordre. Nous ne remontrerons pas les instructions **using** ni la définition de la portée de classe dans les extraits avant le code final.

## **<a name="measureoverride"></a>MeasureOverride**


```CSharp
protected override Size MeasureOverride(Size availableSize)
{
    Size returnSize;
    // Determine the square that can contain this number of items.
    maxrc = (int)Math.Ceiling(Math.Sqrt(Children.Count));
    // Get an aspect ratio from availableSize, decides whether to trim row or column.
    aspectratio = availableSize.Width / availableSize.Height;

    // Now trim this square down to a rect, many times an entire row or column can be omitted.
    if (aspectratio > 1)
    {
        rowcount = maxrc;
        colcount = (maxrc > 2 && Children.Count < maxrc * (maxrc - 1)) ? maxrc - 1 : maxrc;
    } 
    else 
    {
        rowcount = (maxrc > 2 && Children.Count < maxrc * (maxrc - 1)) ? maxrc - 1 : maxrc;
        colcount = maxrc;
    }

    // Now that we have a column count, divide available horizontal, that's our cell width.
    cellwidth = (int)Math.Floor(availableSize.Width / colcount);
    // Next get a cell height, same logic of dividing available vertical by rowcount.
    cellheight = Double.IsInfinity(availableSize.Height) ? Double.PositiveInfinity : availableSize.Height / rowcount;
           
    foreach (UIElement child in Children)
    {
        child.Measure(new Size(cellwidth, cellheight));
        maxcellheight = (child.DesiredSize.Height > maxcellheight) ? child.DesiredSize.Height : maxcellheight;
    }
    return LimitUnboundedSize(availableSize);
}
```

Le modèle nécessaire d’une implémentation [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) est la boucle qui parcourt chaque élément dans [**Panel.Children**](https://msdn.microsoft.com/library/windows/apps/br227514). Vous devez toujours appeler la méthode [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) sur chacun de ces éléments. **Measure** possède un paramètre de type [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995). Vous passez ici la taille que votre panneau s’engage à mettre à disposition de cet élément enfant. Avant de pouvoir parcourir la boucle et de commencer à appeler **Measure**, vous devez donc connaître la quantité d’espace que chaque cellule peut allouer. À partir de la méthode **MeasureOverride**, vous avez la valeur *availableSize*. Il s’agit de la taille qui a été utilisée par le parent du panneau quand il a appelé **Measure**, ce qui a déclenché initialement l’appel de cette méthode **MeasureOverride**. La logique la plus classique consiste à établir un schéma selon lequel chaque élément enfant divise l’espace de la taille disponible (*availableSize*) globale du panneau. Vous transmettez ensuite chaque division de taille à la méthode **Measure** de chaque élément enfant.

La manière dont `BoxPanel` divise la taille est assez simple : il divise son espace en un nombre de cases déterminé en grande partie par le nombre d’éléments. Les tailles des cases sont établies en fonction du nombre de lignes et de colonnes et de la taille globale disponible. Parfois, une ligne ou une colonne d’un carré n’est pas nécessaire. Dans ce cas, elle est supprimée, et le panneau devient un rectangle plutôt qu’un carré en termes de rapport ligne/colonne. Pour plus d’informations sur cette logique, passez directement à [«Le scénario de BoxPanel»](#the-scenario-for-boxpanel).

Que fait donc la passe de mesure? Elle définit une valeur pour la propriété [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) en lecture seule sur chaque élément où la méthode [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) a été appelée. Le fait d’avoir une valeur **DesiredSize** peut être important une fois la passe d’organisation atteinte, car la propriété **DesiredSize** indique ce que peut ou doit être la taille lors de l’organisation et du rendu final. Même si vous n’utilisez pas **DesiredSize** dans votre propre logique, le système en a besoin.

Ce panneau peut être utilisé quand le composant hauteur de *availableSize* est sans limite. Dans ce cas, le panneau n’a aucune hauteur connue à diviser. La logique de la passe de mesure signale alors à chaque enfant qu’il n’a pas encore de hauteur limitée. Pour cela, elle transmet un objet [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) à l’appel de [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) pour les enfants pour lesquels la propriété [**Size.Height**](https://msdn.microsoft.com/library/windows/apps/hh763910) est infinie. Cette opération est autorisée. Quand la méthode **Measure** est appelée, la logique veut que [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) prenne la plus petite de ces valeurs: la valeur transmise à **Measure** ou la taille naturelle de l’élément tirée de facteurs tels que les valeurs [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) et [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) définies de manière explicite.

> [!NOTE]
> La logique interne de [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635) a également le comportement suivant: **StackPanel** passe une valeur de dimension infinie à [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) sur les enfants pour indiquer l’absence de contraintes sur les enfants de la dimension d’orientation. **StackPanel** se dimensionne en général de manière dynamique pour pouvoir accueillir tous les enfants d’une pile qui croît dans cette dimension.

Toutefois, le panneau proprement dit ne peut pas retourner d’objet [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) avec une valeur infinie à partir de [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730); cela lève une exception durant la disposition. Une partie de la logique consiste donc à trouver la hauteur maximale demandée par chaque enfant et à utiliser cette hauteur comme hauteur de cellule dans le cas où elle ne provient pas déjà des propres contraintes de taille du panneau. Voici la fonction d’assistance `LimitUnboundedSize` référencée dans le code précédent, qui prend ensuite cette hauteur maximale de cellule et l’utilise pour donner au panneau une hauteur finie à retourner et garantit que `cellheight` est un nombre fini avant d’initier la passe d’organisation :

```CSharp
// This method is called only if one of the availableSize dimensions of measure is infinite.
// That can happen to height if the panel is close to the root of main app window.
// In this case, base the height of a cell on the max height from desired size
// and base the height of the panel on that number times the #rows.

Size LimitUnboundedSize(Size input)
{
    if (Double.IsInfinity(input.Height))
    {
        input.Height = maxcellheight * colcount;
        cellheight = maxcellheight;
    }
    return input;
}
```

## **<a name="arrangeoverride"></a>ArrangeOverride**

```CSharp
protected override Size ArrangeOverride(Size finalSize)
{
     int count = 1
     double x, y;
     foreach (UIElement child in Children)
     {
          x = (count - 1) % colcount * cellwidth;
          y = ((int)(count - 1) / colcount) * cellheight;
          Point anchorPoint = new Point(x, y);
          child.Arrange(new Rect(anchorPoint, child.DesiredSize));
          count++;
     }
     return finalSize;
}
```

Le modèle nécessaire d’une implémentation [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) est la boucle qui parcourt chaque élément dans [**Panel.Children**](https://msdn.microsoft.com/library/windows/apps/br227514). Vous devez toujours appeler la méthode [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) sur chacun de ces éléments.

Notez qu’il y a moins de calculs que dans [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730), ce qui est normal. La taille des enfants est déjà connue grâce à la propre logique **MeasureOverride** du panneau ou grâce à la valeur [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) de chaque enfant définie durant la passe de mesure. Toutefois, il reste encore à décider de l’emplacement où apparaîtra chaque enfant dans le panneau. Dans un panneau classique, chaque enfant doit être affiché à une position différente. Dans les scénarios ordinaires, il n’est pas souhaitable d’avoir un panneau avec des éléments qui se chevauchent (bien qu’il ne soit pas interdit de créer des panneaux avec des chevauchements intentionnels, si votre scénario l’impose.)

Ce panneau organise les éléments selon un concept de lignes et de colonnes. Le nombre de lignes et de colonnes a déjà été calculé (il était nécessaire pour la mesure). La forme des lignes et des colonnes et la taille connue de chaque cellule contribuent maintenant à la logique de définition d’une position de rendu (l’objet `anchorPoint`) pour chaque élément contenu dans ce panneau. Ce [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) et la valeur [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) déjà obtenue suite à la mesure sont utilisés comme composants pour la construction d’un objet [**Rect**](https://msdn.microsoft.com/library/windows/apps/br225994). **Rect** est le type d’entrée pour [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914).

Les panneaux doivent parfois tronquer leur contenu. Dans ce cas, la taille coupée est celle présente dans [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921), car la logique de [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) la définit comme le minimum de ce qui a été transmis à **Measure**, ou d’autres facteurs de taille naturelle. Ainsi, il n’est généralement pas nécessaire de se préoccuper de la troncature durant [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914). Celle-ci aura simplement lieu en fonction de la transmission de la valeur **DesiredSize** lors de chaque appel à **Arrange**.

Un décompte n’est pas toujours nécessaire durant le bouclage si toutes les informations dont vous avez besoin pour définir la position de rendu sont déjà connues par un autre moyen. Par exemple, dans la logique de disposition [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267), la position dans la collection [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) n’a pas d’importance. Toutes les informations nécessaires pour positionner chaque élément dans un objet **Canvas** sont connues par la lecture des valeurs [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) et [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/hh759772) des enfants dans le cadre de la logique d’organisation. La logique `BoxPanel` a besoin d’un décompte à des fins de comparaison avec *colcount*, afin de savoir quand commencer une nouvelle ligne et décaler la valeur *y*.

Il est courant que la valeur *finalSize* d’entrée et la valeur [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) retournée à partir d’une implémentation [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) soient identiques. Pour plus d’informations à ce sujet, voir «**ArrangeOverride**» dans [Vue d’ensemble des panneaux personnalisés XAML](custom-panels-overview.md).

## <a name="a-refinement-controlling-the-row-vs-column-count"></a>Un affinement: le contrôle du nombre de lignes et de colonnes

Vous pourriez compiler et utiliser ce panneau tel quel. Nous allons toutefois ajouter un petit affinement. Dans le code fourni, la logique place la ligne ou colonne supplémentaire du côté où la proportion est la plus longue. Pour un meilleur contrôle des formes des cellules, il peut être souhaitable de choisir un ensemble de cellules4x3 plutôt que 3x4, même si les proportions du panneau sont définies sur «Portrait». Nous allons donc ajouter une propriété de dépendance facultative que le consommateur du panneau peut définir pour contrôler le comportement. Voici la définition de cette propriété de dépendance. Elle est très simple:

```CSharp
public static readonly DependencyProperty UseOppositeRCRatioProperty =
   DependencyProperty.Register("UseOppositeRCRatio", typeof(bool), typeof(BoxPanel), null);

public bool UseSquareCells
{
    get { return (bool)GetValue(UseOppositeRCRatioProperty); }
    set { SetValue(UseOppositeRCRatioProperty, value); }
}
```

Et voici comment l’utilisation de `UseOppositeRCRatio` affecte la logique de mesure. Tout ce qu’elle fait, c’est modifier la façon dont `rowcount` et `colcount` sont dérivées de `maxrc` et des proportions réelles, ce qui provoque des différences de taille correspondantes pour chaque cellule. Quand `UseOppositeRCRatio` est **true**, la valeur des proportions réelles est inversée avant d’être utilisée pour définir le nombre de lignes et de colonnes.

```CSharp
if (UseOppositeRCRatio) { aspectratio = 1 / aspectratio;}
```

## <a name="the-scenario-for-boxpanel"></a>Le scénario de BoxPanel

`BoxPanel` est un panneau pour lequel le principal facteur qui permet de déterminer le mode de répartition de l’espace est la connaissance du nombre d’éléments enfants et la division de l’espace disponible connu pour le panneau. Les panneaux sont, à la base, des formes rectangulaires. De nombreux panneaux opèrent en divisant cet espace rectangulaire en plusieurs rectangles. C’est ce que fait [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) pour ses cellules. Dans le cas de **Grid**, la taille des cellules est définie par les valeurs [**ColumnDefinition**](https://msdn.microsoft.com/library/windows/apps/br209324) et [**RowDefinition**](https://msdn.microsoft.com/library/windows/apps/br227606) et les éléments déclarent la cellule exacte dans laquelle ils vont avec les propriétés jointes [**Grid.Row**](https://msdn.microsoft.com/library/windows/apps/hh759795) et [**Grid.Column**](https://msdn.microsoft.com/library/windows/apps/hh759774). Pour obtenir une bonne disposition à partir d’une **Grid**, il faut généralement connaître au préalable le nombre d’éléments enfants, pour qu’il y ait suffisamment de cellules et que chaque élément enfant définisse ses propriétés jointes en fonction de la taille de sa propre cellule.

Et si le nombre d’enfants est dynamique ? C’est parfaitement possible ; votre code d’application peut ajouter des éléments aux collections en réponse à toute condition d’exécution dynamique que vous jugez assez importante pour devoir mettre à jour votre interface utilisateur. Si vous utilisez la liaison de données vers des collections ou des objets métier, l’obtention de ces mises à jour et la mise à jour de l’interface utilisateur sont gérées automatiquement. Il s’agit donc de la technique de prédilection (voir [Présentation détaillée de la liaison de données](https://msdn.microsoft.com/library/windows/apps/mt210946)).

Mais les scénarios d’application ne se prêtent pas tous à la liaison de données. Parfois, vous devez créer de nouveaux éléments d’interface utilisateur au moment de l’exécution et les rendre visibles. `BoxPanel` convient à ce scénario. Une modification du nombre d’éléments enfants ne constitue pas un problème pour `BoxPanel`, car il utilise le nombre d’enfants dans les calculs et ajuste à la fois les éléments enfants nouveaux et existants pour qu’ils soient tous contenus dans la nouvelle disposition.

Un scénario avancé pour étendre `BoxPanel` (non illustré ici) consisterait à la fois à gérer les enfants dynamiques et à utiliser la valeur [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) d’un enfant comme facteur prioritaire pour le dimensionnement des cellules individuelles. Ce scénario pourrait utiliser des tailles de lignes et de colonnes variables ou des formes autres que des grilles afin de réduire l’espace «perdu». Cela requiert une stratégie afin de déterminer comment plusieurs rectangles de différentes tailles et proportions peuvent rentrer dans un rectangle contenant du point de vue esthétique et en cas de très petite taille. `BoxPanel` n’offre pas cette fonctionnalité. Il utilise une technique plus simple pour diviser l’espace. `BoxPanel`La technique employée par consiste à déterminer le plus petit nombre de carré supérieur au nombre d’enfants. Par exemple, 9éléments pourraient être contenus dans un carré de 3x3. 10éléments nécessitent un carré de 4x4. Toutefois, vous pouvez souvent ajuster les éléments tout en supprimant une ligne ou une colonne dans le carré de départ, pour gagner de l’espace. Avec 10éléments, par exemple, vous pourriez utiliser un rectangle de 4x3 ou de 3x4.

Vous vous demandez peut-être pourquoi le panneau ne choisirait pas plutôt un rectangle de 5x2 pour 10éléments. En pratique, les panneaux sont dimensionnés sous la forme de rectangles qui ont rarement des proportions fortement orientées. La technique des moindres carrés permet à la logique de dimensionnement de bien fonctionner avec les formes de disposition classiques tout en évitant les dimensionnements où des proportions irrégulières sont appliquées aux cellules.

## <a name="related-topics"></a>Rubriquesassociées

**Référence**

* [**FrameworkElement.ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)
* [**FrameworkElement.MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730)
* [**Panneau**](https://msdn.microsoft.com/library/windows/apps/br227511)

**Concepts**

* [Alignement, marge et espacement](alignment-margin-padding.md)
