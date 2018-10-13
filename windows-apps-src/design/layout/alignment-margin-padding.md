---
author: QuinnRadich
Description: Use alignment, margin, and padding properties to arrange the layout of elements on a page.
title: Alignement, marge et remplissage pour la disposition
ms.author: quradic
ms.date: 03/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7a45e89c63ec12cb7b77997eac741ebedc415c54
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4571431"
---
# <a name="alignment-margin-padding"></a>Alignement, marge, remplissage

Dans les applications UWP, la plupart des éléments d’interface utilisateur héritent de la classe [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement). Chaque classe FrameworkElement possède des propriétés de dimensions, d’alignement, de marge et de remplissage qui influencent le comportement de disposition. Le guide suivant fournit une vue d’ensemble sur la façon d'utiliser ces propriétés de disposition pour vous assurer que l’interface utilisateur de votre application est lisible et facile à utiliser dans n’importe quel contexte.

## <a name="dimensions-height-width"></a>Dimensions (Height, Width)
Un dimensionnement correct garantit que tout le contenu est clair et lisible. Les utilisateurs ne devraient pas avoir à faire défiler ou à zoomer pour déchiffrer le contenu principal.

![Diagramme illustrant les dimensions](images/dimensions.svg)

- [**Height**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height) et [**Width**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.width) spécifient la taille d’un élément. Les valeurs par défaut sont mathématiquement NaN (Not A Number). Vous pouvez définir des valeurs fixes mesurées en [pixels effectifs](../basics/design-and-ui-intro.md#effective-pixels-and-scaling), ou vous pouvez utiliser **Auto** ou le [dimensionnement proportionnel](layout-panels.md#grid) pour un comportement fluide.

- [**ActualHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualheight) et [**ActualWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) sont des propriétés en lecture seule qui fournissent la taille d’un élément au moment de l'exécution. Si les dispositions fluides grandissent ou rétrécissent, alors les valeurs changent dans un événement [**SizeChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged). Notez qu’un [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) ne changera pas les valeurs ActualHeight et ActualWidth.

- [**MinWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.minwidth)/[**MaxWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.maxwidth) et [**MinHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.minheight)/[**MaxHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.maxheight) spécifient les valeurs qui restreignent la taille d’un élément tout en autorisant un redimensionnement fluide.

- [**FontSize**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.fontsize) et d’autres propriétés de texte contrôlent la taille de disposition des éléments de texte. Alors que les éléments de texte n’ont pas de dimensions déclarées explicitement, ils ont des valeurs ActualWidth et ActualHeight calculées. 

## <a name="alignment"></a>Alignement
L’alignement rend votre interface utilisateur claire, organisée et équilibrée, et peut être également utilisé pour établir des relations et une hiérarchie visuelle.

![Diagramme illustrant l’alignement](images/alignment.svg)

- [**HorizontalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment) et [**VerticalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment) spécifient la manière dont un élément doit être positionné dans son conteneur parent.
    - Les valeurs de **HorizontalAlignment** sont **Left**, **Center**, **Right** et **Stretch**.
    - Les valeurs de **VerticalAlignment** sont **Top**, **Center**, **Bottom** et **Stretch**

- Avec **Stretch**, valeur par défaut pour les deux propriétés, les éléments remplissent tout l’espace qui leur est alloué dans le conteneur parent. Une valeur Height et Width à nombre réel annule une valeur Stretch, qui agira à la place en tant que valeur de type Center. Certains contrôles, tels que Button, remplacent la valeur Stretch par défaut dans leur style par défaut.

- [**HorizontalContentAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.horizontalcontentalignment) et [**VerticalContentAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.verticalcontentalignment) spécifient la manière dont les éléments enfants sont positionnés dans un conteneur.

- L’alignement peut affecter la troncature dans un panneau de disposition. Par exemple, avec `HorizontalAlignment="Left"`, le côté droit de l’élément est tronqué si le contenu est plus grand que ActualWidth.

- Les éléments de texte utilisent la propriété [**TextAlignment**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.textalignment). En règle générale, nous recommandons d’utiliser un alignement à gauche, la valeur par défaut. Pour plus d’informations sur le texte des styles, voir [Typographie](../style/typography.md).

## <a name="margin-and-padding"></a>Marge et remplissage
Les propriétés Margin et Padding évitent que l’interface utilisateur n’apparaisse trop désordonnée ou fragmentée, et facilitent l’utilisation de certaines entrées, comme le stylet et la fonction tactile. Voici une illustration affichant les marges et le remplissage pour un conteneur et son contenu.

![Diagramme de marges et de remplissage XAML](images/xaml-layout-margins-padding.svg)

### <a name="margin"></a>Marge
La propriété [**Margin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.margin) contrôle la quantité d’espace vide autour d’un élément. Margin n’ajoute pas de pixels aux propriétés ActualHeight et ActualWidth, et n’est pas considéré comme faisant partie de l’élément pour les tests de positionnement et la détection des événements d’entrée.

- Les valeurs de marge peuvent être uniformes ou distinctes. Avec `Margin="20"`, une marge uniforme de 20pixels est appliquée à l’élément à gauche, en haut, à droite et en bas. Avec `Margin="0,10,5,25"`, les valeurs sont appliquées à gauche, en haut, à droite et en bas (dans cet ordre). 

- Les marges s’ajoutent. Si deux éléments spécifient tous les deux une marge uniforme de 10pixels et qu’il s’agit d’homologues adjacents dans une orientation quelconque, la distance entre eux est de 20pixels.

- Les marges négatives sont autorisées. Toutefois, leur utilisation n’est pas très courante car elles peuvent souvent provoquer une troncature ou un chevauchement d’homologues.

- Les valeurs de marge sont contraintes en dernier, faites donc attention aux marges, car les conteneurs peuvent tronquer ou contraindre des éléments. Une valeur de marge peut entraîner le non-affichage d’un élément; avec une marge appliquée, la dimension d’un élément peut être contrainte à 0.

### <a name="padding"></a>Remplissage
La propriété [**Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.padding) contrôle la quantité d’espace entre la bordure interne d’un élément et son contenu enfant ou ses éléments. Une valeur Padding positive diminue la zone de contenu de l’élément. 

Contrairement à Margin, Padding n’est pas une propriété de FrameworkElement. Il existe plusieurs classes qui définissent chacune leur propre propriété Padding:

-   [**Control.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.padding): hérite de toutes les classes dérivées [**Control**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls). Comme les contrôles ne possèdent pas tous du contenu, cette propriété n’a aucun effet pour ces contrôles. Si le contrôle a une bordure, le remplissage s’applique à l’intérieur de celle-ci.
-   [**Border.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.padding): définit l’espace entre la ligne de rectangle créée par [**BorderThickness**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.borderthickness)/[**BorderBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.borderbrush) et l’élément [**Child**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.child).
-   [**ItemsPresenter.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemspresenter.padding): contribue à l’apparence des éléments dans les contrôles d’éléments, en plaçant le remplissage spécifié autour de chaque élément.
-   [**TextBlock.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.padding) et [**RichTextBlock.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richtextblock.padding): étendent le cadre englobant autour du texte de l’élément de texte. Ces éléments de texte n’ayant pas d’**arrière-plan**, il peut être difficile de voir. Pour cette raison, utilisez les paramètres [**Margin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.block.margin) sur les conteneurs [**Block**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.block) à la place.

Dans chacun de ces cas, les éléments ont aussi une propriété Margin. Si les propriétés Margin et Padding sont toutes les deux appliquées, elles s’ajoutent: la distance apparente entre un conteneur extérieur et tout contenu intérieur est égale à la marge plus au remplissage.

### <a name="example"></a>Exemple
Examinons les effets des éléments Margin et Padding sur des contrôles réels. Voici un élément TextBox à l’intérieur d’un élément Grid avec des valeurs par défaut définies sur 0 pour Margin et Padding.

![Élément TextBox avec marge et remplissage valant 0](images/xaml-layout-textbox-no-margins-padding.svg)

Voici les mêmes éléments TextBox et Grid avec des valeurs positives pour Margin et Padding sur l’élément TextBox comme illustré dans ce code XAML.

```xaml
<Grid BorderBrush="Blue" BorderThickness="4" Width="200">
    <TextBox Text="This is text in a TextBox." Margin="20" Padding="24,16"/>
</Grid>
```

![Élément TextBox avec valeurs de marge et de remplissage positives](images/xaml-layout-textbox-with-margins-padding.svg)


## <a name="style-resources"></a>Ressources de style
Vous n’êtes pas obligé de définir chaque valeur de propriété individuellement sur un contrôle. Il est généralement plus efficace de regrouper les valeurs de propriété au sein d’une ressource [**Style**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) et d’appliquer le Style à un contrôle. Cela est particulièrement vrai lorsque vous devez appliquer les mêmes valeurs de propriétés à plusieurs contrôles. Pour plus d’informations sur les styles, voir [Styles XAML](../controls-and-patterns/xaml-styles.md).

## <a name="general-recommendations"></a>Recommandations générales
- N’appliquez des valeurs de mesure qu’à certains éléments clés et utilisez le comportement de disposition fluide pour les autres éléments. Cela garantit une [interface utilisateur réactive](responsive-design.md) quand la largeur de la fenêtre change.

- Si vous utilisez des valeurs de mesure, **toutes les dimensions, marges et remplissages doivent se faire par incréments de 4epx**. Quand UWP utilise des [pixels effectifs et une mise à l’échelle](../basics/design-and-ui-intro.md#effective-pixels-and-scaling) pour rendre votre application lisible sur tous les appareils et toutes les tailles d’écran, elle met à l’échelle les éléments d’interface utilisateur par multiples de 4. L’utilisation de valeurs par incréments de 4 offre un rendu optimal en alignant les pixels entiers.

- Pour les fenêtres étroites (inférieures à 640pixels en largeur), nous recommandons des reliures de 12epx, tandis que pour les fenêtres plus larges, nous recommandons des reliures de 24epx.

![Reliures recommandées](images/12-gutter.svg)

## <a name="related-topics"></a>Rubriquesassociées
* [**FrameworkElement.Height**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height)
* [**FrameworkElement.Width**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.width)
* [**FrameworkElement.HorizontalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment)
* [**FrameworkElement.VerticalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment)
* [**FrameworkElement.Margin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.margin)
* [**Control.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.padding)
