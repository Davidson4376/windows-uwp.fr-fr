---
author: Jwmsft
ms.assetid: 02141F86-355E-4046-86EA-2A89D615B7DB
title: Utiliser des pinceaux
description: Les objets Brush permettent de peindre les intérieurs ou les contours de formes, de texte et de parties de contrôles, afin que l’objet peint soit visible dans une interface utilisateur.
ms.author: jimwalk
ms.date: 07/13/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0473ee984461bf46be4ebf866a564f0d51e0cfc5
ms.sourcegitcommit: 194ab5aa395226580753869c6b66fce88be83522
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2018
ms.locfileid: "4155312"
---
# <a name="using-brushes-to-paint-backgrounds-foregrounds-and-outlines"></a>Utilisation des pinceaux pour peindre des arrière-plans, des premiers plans et des contours

Utilisez les objets [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) pour peindre les intérieurs ou les contours de formes, de texte et de parties de contrôles XAML, afin que l’objet peint soit visible dans une interface utilisateur. Examinons les pinceaux proposés et leur utilisation.

> **API importantes**:  [classe Brush](/uwp/api/Windows.UI.Xaml.Media.Brush)

## <a name="introduction-to-brushes"></a>Vue d’ensemble des pinceaux

Pour peindre un objet tel qu’un objet [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) ou les parties d’un objet [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) qui apparaît sur la zone de dessin de l’application, vous utilisez un objet [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush). Par exemple, si vous définissez la propriété [**Fill**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.fill.aspx) de l’objet **Shape** ou les propriétés [**Background**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.background.aspx) et [**Foreground**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.foreground.aspx) d’un objet **Control** sur une valeur **Brush**, cette valeur **Brush** détermine la façon dont l’élément d’interface utilisateur est peint ou restitué dans l’interface utilisateur. 

Les différents types de pinceaux sont les suivants: 
-   [**AcrylicBrush**](/uwp/api/windows.ui.xaml.media.acrylicbrush)
-   [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962)
-   [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210108) 
-   [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101)
-   [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/BR227703)
-   [**XamlCompositionBrushBase**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)

## <a name="solid-color-brushes"></a>Pinceaux de couleur unie

Un pinceau [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) peint une zone en une couleur [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723) unique, comme le rouge ou le bleu. Il s’agit du pinceau le plus élémentaire. Il existe trois façons de définir un pinceau **SolidColorBrush** et la couleur associée en XAML: par le biais des noms de couleur prédéfinis, des valeurs de couleur hexadécimales ou de la syntaxe des éléments de propriété.

### <a name="predefined-color-names"></a>Noms de couleur prédéfinis

Vous pouvez utiliser un nom de couleur prédéfini, comme [**Yellow**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.yellow.aspx) ou [**Magenta**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.magenta.aspx). Vous disposez de 256couleurs nommées. L’analyseur XAML convertit le nom de la couleur en une structure [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723) avec les canaux de couleur appropriés. Étant donné que les 256couleurs nommées reposent sur les noms de couleurs *X11* issus de la spécification CSS3 (Cascading Style Sheets, Level3), vous connaissez peut-être déjà cette liste de couleurs nommées si vous avez de l’expérience en matière de développement ou de conception de sites web.

Vous trouverez ci-dessous un exemple dans lequel la propriété [**Fill**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.fill.aspx) d’un objet [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) présente la couleur prédéfinie [**Red**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.red.aspx).

```xml
<Rectangle Width="100" Height="100" Fill="Red" />
```

L’image ci-après illustre l’application de l’objet [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) à l’objet [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle).

![Rendu de SolidColorBrush](images/brushes-solidcolorbrush.jpg)

Si vous définissez un objet [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) à l’aide d’un code plutôt que de XAML, chaque couleur nommée est disponible en tant que valeur de propriété statique de la classe [**Colors**](https://msdn.microsoft.com/library/windows/apps/windows.ui.colors). Par exemple, pour déclarer une valeur [**Color**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.solidcolorbrush.color.aspx) d’un objet **SolidColorBrush** afin de représenter la couleur nommée «Orchid», définissez la valeur **Color** sur la valeur statique [**Colors.Orchid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.orchid.aspx).

### <a name="hexadecimal-color-values"></a>Valeurs de couleur hexadécimales

Vous pouvez, à l’aide d’une chaîne au format hexadécimal, déclarer des valeurs de couleurs 24bits précises avec un canal alpha sur 8bits pour un objet [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962). Deuxcaractères dans la plage 0 à F définissent chaque valeur du composant, et l’ordre des valeurs de composants de la chaîne hexadécimale est le suivant: canal alpha (opacité), canal rouge, canal vert et canal bleu (**ARGB**). Par exemple, la valeur hexadécimale «\#FFFF0000» définit un rouge entièrement opaque (alpha=«FF», rouge=«FF», vert=«00» et bleu=«00»).

L’exemple XAML suivant définit la propriété [**Fill**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.fill.aspx) d’un objet [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) sur la valeur hexadécimale «\#FFFF0000» et aboutit au même résultat que l’utilisation de la couleur nommée [**Colors.Red**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.red.aspx).

```xml
<StackPanel>
  <Rectangle Width="100" Height="100" Fill="#FFFF0000" />
</StackPanel>
```

### <a name="span-idpropertyelementsyntaxspanspan-idpropertyelementsyntaxspanspan-idpropertyelementsyntaxspanproperty-element-syntax"></a><span id="Property_element_syntax__"></span><span id="property_element_syntax__"></span><span id="PROPERTY_ELEMENT_SYNTAX__"></span>Syntaxe des éléments de la propriété

Vous pouvez utiliser la syntaxe des éléments de propriété pour définir un pinceau [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962). Cette syntaxe est plus détaillée que les méthodes précédentes, mais elle vous permet de spécifier des valeurs de propriétés supplémentaires sur un élément, telles que la propriété [**Opacity**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.brush.opacity.aspx). Pour plus d’informations sur la syntaxe XAML, y compris la syntaxe des éléments de propriété, voir [Vue d’ensemble du langage XAML](https://msdn.microsoft.com/library/windows/apps/Mt185595) et [Guide la syntaxe XAML](https://msdn.microsoft.com/library/windows/apps/Mt185596).

Dans les exemples précédents, la chaîne «SolidColorBrush» n’apparaît nulle part dans la syntaxe. Le pinceau en cours de création est généré implicitement et automatiquement, dans le cadre d’un raccourci délibéré en langage XAML qui assure la simplicité de la définition de l’interface utilisateur dans les cas les plus courants. L’exemple suivant permet de créer un objet [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) et de créer explicitement l’objet [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) en tant que valeur d’élément pour une propriété [**Rectangle.Fill**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.fill.aspx). La propriété [**Color**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.solidcolorbrush.color.aspx) de l’objet **SolidColorBrush** est définie sur la valeur [**Blue**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.blue.aspx), tandis que la propriété [**Opacity**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.brush.opacity.aspx) présente la valeur0,5.

```xml
<Rectangle Width="200" Height="150">
    <Rectangle.Fill>
        <SolidColorBrush Color="Blue" Opacity="0.5" />
    </Rectangle.Fill>
</Rectangle>
```

## <a name="span-idlineargradientbrushesspanspan-idlineargradientbrushesspanspan-idlineargradientbrushesspanlinear-gradient-brushes"></a><span id="Linear_gradient_brushes_"></span><span id="linear_gradient_brushes_"></span><span id="LINEAR_GRADIENT_BRUSHES_"></span>Pinceaux de dégradé linéaire

Un objet [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210108) peint une zone en dégradé défini le long d’une ligne. Cette ligne est appelée *axe de dégradé*. Vous devez spécifier les couleurs du dégradé et leur emplacement le long de l’axe à l’aide d’objets [**GradientStop**](https://msdn.microsoft.com/library/windows/apps/BR210078). Par défaut, l’axe de dégradé court du coin supérieur gauche vers le coin inférieur droit de la zone peinte par le pinceau, ce qui produit une ombre diagonale.

Le point de dégradé [**GradientStop**](https://msdn.microsoft.com/library/windows/apps/BR210078) est le bloc de construction élémentaire d’un pinceau de dégradé. Un point de dégradé spécifie la nature de la propriété [**Color**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.gradientstop.color.aspx) du pinceau pour une propriété [**Offset**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.gradientstop.offset.aspx) le long de l’axe de dégradé, lorsque le pinceau est appliqué à la zone peinte.

La propriété [**Color**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.gradientstop.color.aspx) du point de dégradé spécifie sa couleur. Vous pouvez définir la couleur à l’aide d’un nom de couleur prédéfini ou de valeurs **ARGB** hexadécimales.

La propriété [**Offset**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.gradientstop.offset.aspx) d’un objet [**GradientStop**](https://msdn.microsoft.com/library/windows/apps/BR210078) spécifie la position de chaque objet **GradientStop** le long de l’axe de dégradé. La valeur **Offset** correspond à une valeur **double** de0 à 1. Si la propriété **Offset** a pour valeur0, l’objet **GradientStop** est placé au début de l’axe de dégradé, en d’autres termes près du point [**StartPoint**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.lineargradientbrush.startpoint.aspx) correspondant. Si la propriété **Offset** présente la valeur1, l’objet **GradientStop** est placé au niveau du point [**EndPoint**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.lineargradientbrush.endpoint.aspx). Au minimum, un objet [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210108) utile doit comporter deux valeurs **GradientStop**, chaque valeur **GradientStop** devant spécifier une propriété [**Color**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.gradientstop.color.aspx) différente et posséder une propriété **Offset** différente dont la valeur est comprise entre0 et1.

L’exemple suivant permet de créer un dégradé linéaire constitué de quatre couleurs afin de peindre un objet [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle).

```xml
<!-- This rectangle is painted with a diagonal linear gradient. -->
<Rectangle Width="200" Height="100">
    <Rectangle.Fill>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="Yellow" Offset="0.0" x:Name="GradientStop1"/>
            <GradientStop Color="Red" Offset="0.25" x:Name="GradientStop2"/>
            <GradientStop Color="Blue" Offset="0.75" x:Name="GradientStop3"/>
            <GradientStop Color="LimeGreen" Offset="1.0" x:Name="GradientStop4"/>
        </LinearGradientBrush>
    </Rectangle.Fill>
</Rectangle>
```

La couleur de chaque point entre les extrémités est interpolée de façon linéaire sous la forme d’une combinaison de la couleur spécifiée par les deux points limites. L’illustration met en relief ces points de dégradés. Les cercles indiquent la position des points de dégradé, et la ligne en pointillés représente l’axe de dégradé.

![Points de dégradé](images/linear-gradients-stops.png) Vous pouvez changer la ligne de positionnement des points de dégradé en affectant aux propriétés [**StartPoint**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.lineargradientbrush.startpoint.aspx) et [**EndPoint**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.lineargradientbrush.endpoint.aspx) des valeurs différentes des valeurs par défaut initiales `(0,0)` et `(1,1)`. En modifiant les valeurs de coordonnées **StartPoint** et **EndPoint**, vous pouvez créer des dégradés horizontaux ou verticaux, inverser le sens du dégradé ou condenser son étalement afin de l’appliquer à une plage plus petite que la zone peinte totale. Pour condenser le dégradé, vous attribuez à la propriété **StartPoint** et/ou à la propriété **EndPoint** des valeurs comprises entre0 et1. Par exemple, pour obtenir un dégradé horizontal dont l’effet se produit en totalité sur la moitié gauche du pinceau, tandis que le côté droit conserve la dernière couleur unie définie par l’objet [**GradientStop**](https://msdn.microsoft.com/library/windows/apps/BR210078), vous spécifiez une propriété **StartPoint** d’une valeur `(0,0)` et une propriété **EndPoint** d’une valeur `(0.5,0)`.

### <a name="span-idusetoolstomakegradientsspanspan-idusetoolstomakegradientsspanspan-idusetoolstomakegradientsspanuse-tools-to-make-gradients"></a><span id="Use_tools_to_make_gradients"></span><span id="use_tools_to_make_gradients"></span><span id="USE_TOOLS_TO_MAKE_GRADIENTS"></span>Utiliser les outils pour rendre les dégradés

Maintenant que vous savez comment fonctionnent les dégradés linéaires, vous pouvez utiliser Visual Studio ou Blend pour les créer plus facilement. Pour créer un dégradé, sélectionnez l’objet auquel vous souhaitez l’appliquer dans l’aire de conception ou dans une vue XAML. Développez **Pinceau**, puis sélectionnez l’onglet **Dégradé linéaire** (voir la capture d’écran suivante).

![Créer un dégradé linéaire à l’aide de Visual Studio](images/tool-gradient-brush-1.png)

Vous pouvez à présent modifier les couleurs des points de dégradé et modifier leurs positions à l’aide de la barre au bas de la fenêtre. Vous pouvez également ajouter de nouveaux points de dégradé en cliquant sur la barre, ou en supprimer en les faisant glisser à l’extérieur de la barre (voir la capture d’écran suivante).

![Barre au bas de la fenêtre de propriétés qui contrôle les points de dégradé.](images/tool-gradient-brush-2.png)

## <a name="span-idimagebrushesspanspan-idimagebrushesspanspan-idimagebrushesspanimage-brushes"></a><span id="Image_brushes"></span><span id="image_brushes"></span><span id="IMAGE_BRUSHES"></span>Pinceaux image

Un objet [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) peint une zone avec une image qui provient d’une source de fichier image. Vous devez définir la propriété [**ImageSource**](https://msdn.microsoft.com/library/windows/apps/BR210107) sur le chemin de l’image à charger. En règle générale, la source d’image est issue d’un élément **Content** qui fait partie des ressources de votre application.

Par défaut, lorsque vous utilisez un pinceau [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101), l’image associée est étirée de manière à remplir entièrement la zone peinte. Elle peut ainsi apparaître déformée si les proportions de cette zone sont différentes de celles de l’image. Vous pouvez changer ce comportement en modifiant la propriété [**Stretch**](https://msdn.microsoft.com/library/windows/apps/BR242975). Pour cela, remplacez sa valeur par défaut **Fill** par la valeur **None**, **Uniform** ou **UniformToFill**.

L’exemple suivant permet de créer un pinceau [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) avec la propriété [**ImageSource**](https://msdn.microsoft.com/library/windows/apps/BR210107) définie sur une image nommée licorice.jpg que vous devez inclure comme ressource dans l’application. L’objet **ImageBrush** peint ensuite la zone définie par une forme [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse).

```xml
<Ellipse Height="200" Width="300">
   <Ellipse.Fill>
     <ImageBrush ImageSource="licorice.jpg" />
   </Ellipse.Fill>
</Ellipse>
```

Voici le rendu de l’objet [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101):

![Rendu d’ImageBrush](images/brushes-imagebrush.jpg)

[**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) et [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) font référence à un fichier source d’image à l’aide d’un URI (Uniform Resource Identifier), où ce fichier utilise plusieurs formats d’image possibles. Ces fichiers sources d’image sont spécifiés sous forme d’URI. Pour plus d’informations sur la spécification de sources d’images, sur les formats d’images utilisables et sur leur empaquetage dans une application, voir [Image et ImageBrush](https://msdn.microsoft.com/library/windows/apps/Mt280382).

## <a name="brushes-and-text"></a>Pinceaux et texte

Vous pouvez également utiliser des pinceaux pour appliquer des caractéristiques de rendu à des éléments de texte. Par exemple, la propriété [**Foreground**](https://msdn.microsoft.com/library/windows/apps/BR209665) de [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) accepte un pinceau [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush). Vous pouvez appliquer n’importe quel pinceau décrit ici à du texte. Toutefois, soyez prudent lorsque vous appliquez un pinceau à du texte, car vous risquez de rendre le texte illisible si vous utilisez des pinceaux qui se fondent dans l’arrière-plan du texte ou qui affectent la netteté du contour des caractères du texte. Utilisez le pinceau [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) par souci de lisibilité de l’élément de texte dans la plupart des cas, à moins que vous ne souhaitiez que l’élément de texte soit majoritairement décoratif.

Même lorsque vous utilisez une couleur unie, veillez à ce que le contraste entre la couleur du texte que vous choisissez et la couleur d’arrière-plan du conteneur de disposition du texte soit suffisant. Le niveau de contraste entre le premier plan du texte et l’arrière-plan du conteneur du texte revêt de l’importance en termes d’accessibilité.

## <a name="webviewbrush"></a>WebViewBrush

Un objet [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/BR227703) est un type particulier de pinceau pouvant accéder au contenu qui est normalement affiché dans un contrôle [**WebView**](https://msdn.microsoft.com/library/windows/apps/BR227702). Au lieu de restituer le contenu dans la zone de contrôle **WebView** rectangulaire, l’objet **WebViewBrush** peint ce contenu sur un autre élément qui possède une propriété de type [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) pour une surface de rendu. L’objet **WebViewBrush** n’est pas approprié pour chaque scénario de pinceau, mais il est utile pour les transitions d’un objet **WebView**. Pour plus d’informations, voir [**WebViewBrush**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush).

## <a name="xamlcompositionbrushbase"></a>XamlCompositionBrushBase

[**XamlCompositionBrushBase**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase) est une catégorie de base utilisée pour créer des pinceaux personnalisés utilisant [**CompositionBrush**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.CompositionBrush) pour peindre des éléments d'interface utilisateur XAML.

Cela active l’interopérabilité «liste déroulante»entre les couches Windows.UI.Xaml et Windows.UI.Composition, tel que décrit dans la [**vue d’ensemble de la couche visuelle**](/windows/uwp/composition/visual-layer). 

Pour créer un pinceau personnalisé, créez une nouvelle catégorie qui hérite de XamlCompositionBrushBase et qui implémente les méthodes requises.

Par exemple, cela peut être utilisé pour appliquer des [**effets**](/windows/uwp/composition/composition-effects) aux éléments d’interface utilisateur XAML à l'aide d'un [**CompositionEffectBrush**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.CompositionEffectBrush), comme un **GaussianBlurEffect** ou un [**SceneLightingEffect**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) qui contrôle les propriétés réfléchissantes d’un élément UIElement XAML lorsque celui-ci est allumé par un [**XamlLight**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.xamllight).

Pour obtenir des exemples de code, consultez la page de référence pour [**XamlCompositionBrushBase**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).

## <a name="brushes-as-xaml-resources"></a>Pinceaux en tant que ressources XAML

Vous pouvez déclarer n’importe quel pinceau en tant que ressource XAML à clé dans un dictionnaire de ressources XAML. Cela permet de répliquer facilement les mêmes valeurs de pinceau pour plusieurs éléments d’une interface utilisateur. Les valeurs de pinceau sont ensuite partagées et appliquées dans toute situation où vous référencez la ressource de pinceau en tant qu’utilisation de [{StaticResource}](https://msdn.microsoft.com/library/windows/apps/Mt185588) dans votre XAML. Cela englobe les situations où un modèle de contrôle XAML référence le pinceau partagé et où le modèle de contrôle est lui-même une ressource XAML à clé.

## <a name="brushes-in-code"></a>Pinceaux dans le code

Il est beaucoup plus courant de spécifier des pinceaux en XAML que d’utiliser du code pour en définir. Cela est dû au fait que les pinceaux sont généralement définis en tant que ressources XAML, et que les valeurs de pinceau sont souvent la sortie des outils de conception ou sinon, qu’elles font partie d’une définition d’interface utilisateur XAML. Quand même, pour les quelques cas où vous voulez définir un pinceau à l’aide de code, tous les types [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) sont disponibles pour l’instanciation du code.

Pour créer une classe [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) dans le code, utilisez le constructeur qui prend un paramètre [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723). Passez une valeur qui est une propriété statique de la classe [**Colors**](https://msdn.microsoft.com/library/windows/apps/windows.ui.colors) comme celle-ci:

```cs
SolidColorBrush blueBrush = new SolidColorBrush(Windows.UI.Colors.Blue);
```

```vb
Dim blueBrush as SolidColorBrush = New SolidColorBrush(Windows.UI.Colors.Blue)
```

```cppwinrt
Windows::UI::Xaml::Media::SolidColorBrush blueBrush{ Windows::UI::Colors::Blue() };
```

```cpp
blueBrush = ref new SolidColorBrush(Windows::UI::Colors::Blue);
```

Pour [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/BR227703) et [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101), utilisez le constructeur par défaut, puis appelez d’autres API avant d’essayer d’utiliser ce pinceau pour une propriété d’interface utilisateur.

-   La propriété [**ImageSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imagebrush.imagesourceproperty.aspx) requiert une classe [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/BR243235) (et non un URI) lorsque vous définissez une classe [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) à l’aide de code. Si votre source est un flux, utilisez la méthode [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/JJ191522) pour initialiser la valeur. Si votre source est un URI incluant du contenu de votre application qui utilise les modèles **ms-appx** ou **ms-resource**, utilisez le constructeur [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/br243238.aspx) qui prend un URI. Vous pouvez également envisager de gérer l’événement [**ImageOpened**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.imageopened.aspx) s’il existe des problèmes de délai liés à la récupération ou au décodage de la source de l’image, auquel cas l’affichage d’un contenu alternatif peut se révéler nécessaire tant que la source de l’image n’est pas disponible.
-   Pour la classe [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/BR227703), vous pouvez avoir besoin d’appeler la méthode [**Redraw**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewbrush.redraw.aspx) si vous avez récemment réinitialisé la propriété [**SourceName**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewbrush.sourcename.aspx) ou que le contenu de la classe [**WebView**](https://msdn.microsoft.com/library/windows/apps/BR227702) est également remplacé à l’aide de code.

Pour obtenir des exemples de code, voir les pages de référence pour [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/BR227703),  [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) et [**XamlCompositionBrushBase**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).
 

 




