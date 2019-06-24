---
ms.assetid: F46D5E18-10A3-4F7B-AD67-76437C77E4BC
title: Vue d’ensemble des transformations
description: Découvrez comment utiliser des transformations dans le Runtime Windows &\#160 ; API, en modifiant les systèmes de coordonnées relatifs des éléments dans l’interface utilisateur.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: da0031dbb87bffb457786170494140dbee0b2a6e
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317117"
---
# <a name="transforms-overview"></a>Vue d’ensemble des transformations

Apprenez à utiliser les transformations dans l’API Windows Runtime, en changeant le système de coordonnées relatives des éléments dans l’interface utilisateur. Cela permet de modifier l’apparence d’éléments XAML, par exemple avec la mise à l’échelle, la rotation ou la transformation de la position dans l’espace x-y.

## <a name="span-idwhatisatransformspanspan-idwhatisatransformspanspan-idwhatisatransformspanwhat-is-a-transform"></a><span id="What_is_a_transform_"></span><span id="what_is_a_transform_"></span><span id="WHAT_IS_A_TRANSFORM_"></span>Qu’est une transformation ?

Une *transformation* définit la façon de mapper, ou transformer, les points d’un espace de coordonnées vers un autre espace de coordonnées. Quand une transformation est appliquée à un élément d’interface utilisateur, elle modifie le rendu de cet élément d’interface utilisateur à l’écran.

On distingue quatre catégories de transformations : translation, rotation, mise à l’échelle et inclinaison. Pour pouvoir utiliser les API graphiques pour modifier l’apparence des éléments d’interface utilisateur, il est généralement plus facile de créer des transformations qui ne définissent qu’une seule opération à la fois. Ainsi, Windows Runtime définit une classe discrète pour chacune de ces catégories de transformation :

-   [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform): se traduit par un élément dans l’espace de x-y, en définissant les valeurs pour [ **X** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.translatetransform.x) et [ **Y** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.translatetransform.y).
-   [**ScaleTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ScaleTransform): met à l’échelle de la transformation basée sur un point central, en définissant les valeurs pour [ **CenterX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.centerx), [ **CenterY** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.centery), [ **ScaleX** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.scalex) et [ **ScaleY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.scaleyproperty).
-   [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform): fait pivoter dans l’espace de x-y, en définissant les valeurs pour [ **Angle**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.rotatetransform.angle), [ **CenterX** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.rotatetransform.centerx) et [ **CenterY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.rotatetransform.centery).
-   [**SkewTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SkewTransform): incline ou découpe dans l’espace de x-y, en définissant les valeurs pour [ **AngleX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.skewtransform.anglex), [ **AngleY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.skewtransform.angley), [ **CenterX** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.skewtransform.centerx) et [ **CenterY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.centeryproperty).

Parmi elles, vous utiliserez sans doute plus souvent [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform) et [**ScaleTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ScaleTransform) dans vos scénario d’interface utilisateur.

Vous pouvez combiner des transformations, et il existe deux classes Windows Runtime qui prennent en charge : [**CompositeTransform** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CompositeTransform) et [ **TransformGroup**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TransformGroup). Dans **CompositeTransform**, les transformations sont appliquées dans l’ordre suivant : mise à l’échelle, inclinaison, rotation, translation. Si vous voulez que les transformations soient appliquées dans un autre ordre, utilisez **TransformGroup** au lieu de **CompositeTransform**. Pour plus d’informations, voir [**CompositeTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CompositeTransform).

## <a name="span-idtransformsandlayoutspanspan-idtransformsandlayoutspanspan-idtransformsandlayoutspantransforms-and-layout"></a><span id="Transforms_and_layout"></span><span id="transforms_and_layout"></span><span id="TRANSFORMS_AND_LAYOUT"></span>Transformations et la disposition

Dans une disposition XAML, les transformations sont appliquées après la passe de disposition. Ainsi, les calculs relatifs à l’espace disponible et les autres décisions relatives à la mise en page ont été effectués avant l’application des transformations. La disposition étant prioritaire, vous aurez parfois des résultats inattendus si vous transformez des éléments situés dans une cellule [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) ou tout conteneur de disposition similaire qui alloue de l’espace durant la disposition. L’élément transformé peut paraître tronqué ou masqué, car il essaie de figurer dans une zone où n’ont pas été calculées les dimensions postérieures à la transformation durant la répartition de l’espace dans son conteneur parent. Vous devrez peut-être tester les résultats des transformations et modifier certains paramètres. Par exemple, au lieu de vous fier à la disposition adaptative et au redimensionnement proportionnel, vous devrez peut-être modifier les propriétés **Center** ou déclarer des mesures de pixels fixes pour l’espace de disposition pour vous assurer que le parent alloue suffisamment d’espace.

**Remarque de la migration :**  Windows Presentation Foundation (WPF) avait un **LayoutTransform** propriété qui applique des transformations avant la phase de disposition. Toutefois, Windows Runtime XAML ne prend pas en charge la propriété **LayoutTransform**. (Microsoft Silverlight ne disposait pas non plus de cette propriété.)

## <a name="span-idapplyingatransformtoauielementspanspan-idapplyingatransformtoauielementspanspan-idapplyingatransformtoauielementspanapplying-a-transform-to-a-ui-element"></a><span id="Applying_a_transform_to_a_UI_element"></span><span id="applying_a_transform_to_a_ui_element"></span><span id="APPLYING_A_TRANSFORM_TO_A_UI_ELEMENT"></span>Appliquer une transformation à un élément d’interface utilisateur

Quand vous appliquez une transformation à un objet, vous avez généralement pour but de définir la propriété [**UIElement.RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform). La définition de cette propriété ne change pas littéralement l’objet pixel par pixel. En fait, la propriété applique la transformation dans l’espace de coordonnées local où existe cet objet. Ensuite, la logique et l’opération de rendu (postérieures à la disposition) affichent les espaces de coordonnées combinés, ce qui donne l’impression que l’objet a changé d’apparence et éventuellement de disposition (si [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform) a été appliqué).

Par défaut, chaque transformation d’affichage est centrée au point d’origine du système de coordonnées local de l’objet cible, (0,0). La seule exception est [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform), qui n’a aucune propriété de centrage à définir, car l’effet de translation est le même quel que soit son centre. Mais les autres transformations ont chacune des propriétés qui définissent les valeurs **CenterX** et **CenterY**.

Chaque fois que vous utilisez des transformations avec [ **UIElement.RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform), n’oubliez pas qu’il existe une autre propriété [ **UIElement** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) qui affecte le comportement de la transformation : [**RenderTransformOrigin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransformorigin). **RenderTransformOrigin** déclare si l’ensemble de la transformation doit s’appliquer au point par défaut (0,0) d’un élément ou à un autre point d’origine dans l’espace de coordonnées relatif de cet élément. Pour les éléments classiques, (0,0) place la transformation dans le coin supérieur gauche. Selon l’effet souhaité, vous pouvez choisir de changer **RenderTransformOrigin** au lieu de modifier les valeurs **CenterX** et **CenterY** des transformations. Notez que si vous appliquez à la fois les valeurs **RenderTransformOrigin** et **CenterX** / **CenterY**, les résultats peuvent être assez déroutants, surtout si vous animez l’une des valeurs.

Pour les besoins des tests de positionnement, un objet auquel une transformation est appliquée continue de répondre à l’entrée d’une manière attendue qui est cohérente avec son apparence visuelle dans l’espace x-y. Par exemple, si vous avez utilisé [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform) pour déplacer un objet [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) de 400 pixels latéralement dans l’interface utilisateur, **Rectangle** répond aux événements [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed) quand l’utilisateur appuie à l’emplacement où **Rectangle** apparaît visuellement. Vous n’obtiendrez pas de faux événements si l’utilisateur appuie dans la zone où **Rectangle** se trouvait avant sa translation. En ce qui concerne les aspects relatifs à l’index Z qui affectent les tests de positionnement, l’application d’une transformation ne fait aucune différence. L’index Z, qui détermine l’élément qui gère les événements d’entrée pour un point de l’espace x-y, est toujours évalué en fonction de l’ordre enfant déclaré dans un conteneur. Cet ordre est généralement le même que celui dans lequel vous déclarez les éléments en XAML. Toutefois, pour les éléments enfants d’un objet [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas), vous pouvez modifier l’ordre en appliquant la propriété jointe [**Canvas.ZIndex**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc190397(v=vs.95)) aux éléments enfants.

## <a name="span-idothertransformpropertiesspanspan-idothertransformpropertiesspanspan-idothertransformpropertiesspanother-transform-properties"></a><span id="Other_transform_properties"></span><span id="other_transform_properties"></span><span id="OTHER_TRANSFORM_PROPERTIES"></span>Autres propriétés de transformation

-   [**Brush.Transform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.brush.transform), [**Brush.RelativeTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.brush.relativetransform): Ces influence comment un [ **pinceau** ](/uwp/api/Windows.UI.Xaml.Media.Brush) utilise espace de coordonnées dans la zone qui le **pinceau** est appliqué pour définir les propriétés visuelles telles que les arrière-plans et colorier. Ces transformations ne sont pas pertinentes pour les pinceaux les plus courants (qui définissent généralement des couleurs unies avec [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)), mais peuvent parfois être utiles quand vous peignez une zone avec un objet [**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush) ou [**LinearGradientBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush).
-   [**Geometry.Transform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.geometry.transform): Vous pouvez utiliser cette propriété pour appliquer une transformation à une géométrie avant d’utiliser cette géométrie pour un [ **Path.Data** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.path.data) valeur de propriété.

## <a name="span-idanimatingatransformspanspan-idanimatingatransformspanspan-idanimatingatransformspananimating-a-transform"></a><span id="Animating_a_transform"></span><span id="animating_a_transform"></span><span id="ANIMATING_A_TRANSFORM"></span>Animer une transformation

[**Transformer** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Transform) objets peuvent être animés. Pour animer un élément **Transform**, appliquez à la propriété que vous souhaitez animer une animation d’un type compatible. Cela signifie généralement que vous utilisez des objets [**DoubleAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DoubleAnimation) ou [**DoubleAnimationUsingKeyFrames**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.doubleanimationusingkeyframes) pour définir l’animation, car toutes les propriétés de transformation sont de type [**Double**](https://docs.microsoft.com/dotnet/api/system.double?redirectedfrom=MSDN). Les animations qui affectent une transformation utilisée pour une valeur [**UIElement.RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) ne sont pas considérées comme étant des animations dépendantes, même si elles ont une durée non nulle. Pour plus d’informations sur les animations dépendantes, voir [Animations dans une table de montage séquentiel](/windows/uwp/design/motion/storyboarded-animations).

Si vous animez des propriétés pour produire un effet comparable à une transformation en termes d’aspect visuel (par exemple l’animation de [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) et [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) pour un élément [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) au lieu de l’application d’un élément [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform)), ces animations sont presque toujours traitées comme des animations dépendantes. Vous devez activer les animations. En outre, l’animation peut entraîner des problèmes de performances importants, surtout si vous essayez de prendre en charge l’interaction de l’utilisateur pendant l’animation de l’objet. C’est pourquoi il est préférable d’utiliser une transformation et de l’animer plutôt que d’animer toute autre propriété où l’animation serait traitée comme une animation dépendante.

Pour cibler la transformation, [**Transform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Transform) doit exister en tant que valeur de [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform). Généralement, vous devez placer un élément pour le type de transformation approprié dans le code XAML initial, parfois sans propriétés définies pour cette transformation.

En principe, vous devez utiliser une technique de ciblage indirect pour appliquer des animations aux propriétés d’une transformation. Pour plus d’informations sur la syntaxe du ciblage indirect, voir [Animations dans une table de montage séquentiel](/windows/uwp/design/motion/storyboarded-animations) et [Syntaxe du chemin de propriété](https://docs.microsoft.com/windows/uwp/xaml-platform/property-path-syntax).

Les styles par défaut des contrôles définissent parfois les animations des transformations dans le cadre du comportement de leur état visuel. Par exemple, les états visuels de [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) utilisent des valeurs [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform) animées pour « faire tourner » les points de l’anneau.

Voici un exemple simple d’animation d’une transformation. Dans le cas présent, il s’agit d’animer l’élément [**Angle**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.rotatetransform.angle) d’un objet [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform) pour faire tourner un [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) sur place autour de son centre visuel. Comme cet exemple nomme **RotateTransform**, le ciblage d’animation indirect n’est pas nécessaire. Toutefois, vous pouvez omettre de nommer la transformation, nommer l’élément auquel la transformation s’applique et utiliser le ciblage indirect tel que `(UIElement.RenderTransform).(RotateTransform.Angle)`.

```xml
<StackPanel Margin="15">
  <StackPanel.Resources>
    <Storyboard x:Name="myStoryboard">
      <DoubleAnimation
       Storyboard.TargetName="myTransform"
       Storyboard.TargetProperty="Angle"
       From="0" To="360" Duration="0:0:5" 
       RepeatBehavior="Forever" />
    </Storyboard>
  </StackPanel.Resources>
  <Rectangle Width="50" Height="50" Fill="RoyalBlue"
   PointerPressed="StartAnimation">
    <Rectangle.RenderTransform>
      <RotateTransform x:Name="myTransform" Angle="45" CenterX="25" CenterY="25" />
    </Rectangle.RenderTransform>
  </Rectangle>
</StackPanel>
```

```xml
void StartAnimation (object sender, RoutedEventArgs e) {
    myStoryboard.Begin();
}
```

## <a name="span-idaccountingforcoordinateframesofreferenceatruntimespanspan-idaccountingforcoordinateframesofreferenceatruntimespanspan-idaccountingforcoordinateframesofreferenceatruntimespanaccounting-for-coordinate-frames-of-reference-at-run-time"></a><span id="Accounting_for_coordinate_frames_of_reference_at_run_time"></span><span id="accounting_for_coordinate_frames_of_reference_at_run_time"></span><span id="ACCOUNTING_FOR_COORDINATE_FRAMES_OF_REFERENCE_AT_RUN_TIME"></span>Comptabilisation des coordonnées de référence en cours d’exécution

[**UIElement** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) a une méthode nommée [ **TransformToVisual**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transformtovisual), qui permet de générer un [ **transformer** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Transform) qui met en corrélation les images de référence de coordonnée pour deux éléments d’interface utilisateur. Vous pouvez l’utiliser pour comparer un élément au système de coordonnées de référence par défaut de l’application, si vous passez l’élément visuel racine comme premier paramètre. Cela peut être utile si vous avez capturé l’événement d’entrée d’un élément distinct, ou si vous essayez de prévoir le comportement de disposition sans demander de passe de disposition.

Les données d’événement obtenues à partir des événements de pointeur permettent d’accéder à une méthode [**GetCurrentPoint**](https://docs.microsoft.com/uwp/api/windows.ui.input.pointerpoint.getcurrentpoint), où vous pouvez spécifier un paramètre *relativeTo* afin de remplacer le système de coordonnées de référence par un élément spécifique au lieu de l’élément par défaut de l’application. Cette approche permet d’appliquer une transformation par translation de manière interne et de transformer automatiquement les données de coordonnées x-y au moment de la création de l’objet [**PointerPoint**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.PointerPoint) renvoyé.

## <a name="span-iddescribingatransformmathematicallyspanspan-iddescribingatransformmathematicallyspanspan-iddescribingatransformmathematicallyspandescribing-a-transform-mathematically"></a><span id="Describing_a_transform_mathematically"></span><span id="describing_a_transform_mathematically"></span><span id="DESCRIBING_A_TRANSFORM_MATHEMATICALLY"></span>Qui décrit une transformation mathématique

Une transformation peut être décrite en termes de matrice de transformation. Une matrice 3x3 permet de décrire les transformations dans un plan x-y en deux dimensions. Les matrices de transformations affines peuvent être multipliées pour former un nombre quelconque de transformations linéaires, par exemple la rotation et l’inclinaison, puis la translation. La dernière colonne d’une matrice de transformation affine est égale à (0, 0, 1). Vous devez donc spécifier uniquement les membres des deux premières colonnes dans la description mathématique.

La description mathématique d’une transformation peut vous être utile si vous avez des connaissances en mathématiques ou que vous êtes habitué aux techniques de programmation graphique qui utilisent également des matrices pour décrire les transformations de l’espace de coordonnées. Il existe un [ **transformer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Transform)-classe dérivée qui vous permet d’exprimer une transformation directement en termes de son matrice 3 x 3 : [**MatrixTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.MatrixTransform). **MatrixTransform** a un [ **matrice** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrixtransform.matrix) propriété, qui contient une structure qui a six propriétés : [**M11**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m11), [ **M12**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m12), [ **M21**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m21), [ **M22** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m22), [ **OffsetX** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.offsetx) et [ **OffsetY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.offsety). Chaque propriété [**Matrix**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Matrix) utilise une valeur **Double** et correspond aux six valeurs pertinentes (colonnes 1 et 2) d’une matrice de transformation affine.

|                                             |                                             |     |
|---------------------------------------------|---------------------------------------------|-----|
| [**M11**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m11)         | [**M21**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m21)         | 0   |
| [**M12**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m12)         | [**M22**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m22)         | 0   |
| [**OffsetX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.offsetx) | [**OffsetY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.offsety) | 1   |

 

Toute transformation que vous pouvez décrire avec un objet [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform), [**ScaleTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ScaleTransform), [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform) ou [**SkewTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SkewTransform) peut également être décrite par un objet [**MatrixTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.MatrixTransform) avec une valeur [**Matrix**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Matrix). Mais en général, vous n’utilisez que **TranslateTransform** et les autres, car il est plus facile de conceptualiser les propriétés de ces classes de transformation que de définir les composants de vecteur dans **Matrix**. Il est également plus facile d’animer les propriétés discrètes des transformations. **Matrix** est en fait une structure, et non un objet [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) ; c’est pourquoi il ne peut pas prendre en charge des valeurs individuelles animées.

Certains outils de conception XAML qui vous permettent d’appliquer des opérations de transformation sérialisent les résultats en tant que [**MatrixTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.MatrixTransform). Dans ce cas, il est peut-être préférable de réutiliser le même outil de conception pour changer l’effet de transformation et resérialiser le code XAML, au lieu d’essayer de manipuler les valeurs de [**Matrix**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Matrix) vous-même directement dans le code XAML.

## <a name="span-id3-dtransformsspanspan-id3-dtransformsspanspan-id3-dtransformsspan3-d-transforms"></a><span id="3-D_transforms"></span><span id="3-d_transforms"></span><span id="3-D_TRANSFORMS"></span>Transformations 3D

Dans Windows 10, XAML a introduit une nouvelle propriété, [**UIElement.Transform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transform3d), qui peut être utilisée pour créer des effets 3D dans l’interface utilisateur. Pour ce faire, utilisez [**PerspectiveTransform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.media3d.perspectivetransform3d) pour ajouter une perspective 3D partagée ou « caméra » à votre scène, puis utilisez [**CompositeTransform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.media3d.compositetransform3d) pour transformer un élément au sein d’un espace en 3D, comme si vous utilisiez [**CompositeTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CompositeTransform). Pour plus d’informations sur comment implémenter des transformations 3D, voir [**UIElement.Transform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transform3d).

 La propriété [**UIElement.Projection**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.projection) peut être utilisée pour des effets 3D plus simples s’appliquant à un objet unique. L’utilisation de [**PlaneProjection**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PlaneProjection) en tant que valeur de cette propriété revient à appliquer une transformation de perspective fixe et une ou plusieurs transformations 3D à l’élément. Ce type de transformation est décrit de manière plus détaillée dans [Effets de perspective 3D pour une interface utilisateur en XAML](3-d-perspective-effects.md).

## <a name="span-idrelatedtopicsspanrelated-topics"></a><span id="related_topics"></span>Rubriques connexes

* [**UIElement.Transform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transform3d)
* [Effets de perspective 3D pour XAML UI](3-d-perspective-effects.md)
* [**Transform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Transform)
 

 





