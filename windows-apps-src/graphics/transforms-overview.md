---
ms.assetid: F46D5E18-10A3-4F7B-AD67-76437C77E4BC
title: Vue d’ensemble des transformations
description: Apprenez à utiliser les transformations dans l’API Windows Runtime, en changeant le système de coordonnées relatives des éléments dans l’interface utilisateur.
---

# Vue d’ensemble des transformations

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Apprenez à utiliser les transformations dans l’API Windows Runtime, en changeant le système de coordonnées relatives des éléments dans l’interface utilisateur. Cela permet de modifier l’apparence d’éléments XAML individuels, par exemple avec la mise à l’échelle, la rotation ou la transformation de la position dans l’espace x-y.

## <span id="What_is_a_transform_"> </span> <span id="what_is_a_transform_"> </span> <span id="WHAT_IS_A_TRANSFORM_"> </span>Qu’est-ce qu’une transformation ?

Une *transformation* définit la façon de mapper (ou transformer) les points d’un espace de coordonnées sur un autre espace de coordonnées. Quand une transformation est appliquée à un élément d’interface utilisateur, elle modifie le rendu de cet élément d’interface utilisateur à l’écran.

On distingue quatre catégories de transformations : translation, rotation, mise à l’échelle et inclinaison. Pour pouvoir utiliser les API graphiques pour modifier l’apparence des éléments d’interface utilisateur, il est généralement plus facile de créer des transformations qui ne définissent qu’une seule opération à la fois. Ainsi, Windows Runtime définit une classe discrète pour chacune de ces catégories de transformation :

-   [
            **TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) : permet de translater un élément dans l’espace x-y, en définissant les valeurs de [**X**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.translatetransform.x.aspx) et [**Y**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.translatetransform.y).
-   [
            **ScaleTransform**](https://msdn.microsoft.com/library/windows/apps/BR242940) : permet de mettre à l’échelle la transformation en fonction d’un point central, en définissant les valeurs de [**CenterX**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.scaletransform.centerx.aspx), [**CenterY**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.scaletransform.centery.aspx), [**ScaleX**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.scaletransform.scalex.aspx) et [**ScaleY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.scaletransform.scaleyproperty).
-   [
            **RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932) : permet de pivoter dans l’espace x-y, en définissant les valeurs de [**Angle**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.rotatetransform.angle.aspx), [**CenterX**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.rotatetransform.centerx.aspx) et [**CenterY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.rotatetransform.centery).
-   [
            **SkewTransform**](https://msdn.microsoft.com/library/windows/apps/BR242950) : permet d’incliner dans l’espace x-y, en définissant les valeurs de [**AngleX**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.skewtransform.anglex.aspx), [**AngleY**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.skewtransform.angley.aspx), [**CenterX**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.skewtransform.centerx.aspx) et [**CenterY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.scaletransform.centeryproperty).

Parmi elles, vous utiliserez sans doute plus souvent [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) et [**ScaleTransform**](https://msdn.microsoft.com/library/windows/apps/BR242940) dans vos scénario d’interface utilisateur.

Vous pouvez combiner les transformations. Cette opération est prise en charge par deux classes Windows Runtime : [**CompositeTransform**](https://msdn.microsoft.com/library/windows/apps/BR228105) et [**TransformGroup**](https://msdn.microsoft.com/library/windows/apps/BR243022). Dans **CompositeTransform**, les transformations sont appliquées dans l’ordre suivant : mise à l’échelle, inclinaison, rotation, translation. Si vous voulez que les transformations soient appliquées dans un autre ordre, utilisez **TransformGroup** au lieu de **CompositeTransform**. Pour plus d’informations, voir [**CompositeTransform**](https://msdn.microsoft.com/library/windows/apps/BR228105).

## <span id="Transforms_and_layout"> </span> <span id="transforms_and_layout"> </span> <span id="TRANSFORMS_AND_LAYOUT"> </span>Transformations et disposition

Dans une disposition XAML, les transformations sont appliquées après la passe de disposition. Ainsi, les calculs relatifs à l’espace disponible et les autres décisions concernant la mise en page ont été effectués avant l’application des transformations. La disposition étant prioritaire, vous aurez parfois des résultats inattendus si vous transformez des éléments situés dans une cellule [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) ou tout conteneur de disposition similaire qui alloue de l’espace durant la disposition. L’élément transformé peut paraître tronqué ou masqué, car il essaie de figurer dans une zone où n’ont pas été calculées les dimensions postérieures à la transformation durant la répartition de l’espace dans son conteneur parent. Vous devrez peut-être tester les résultats des transformations et modifier certains paramètres. Par exemple, au lieu de vous fier à la disposition adaptative et au redimensionnement proportionnel, vous devrez peut-être modifier les propriétés **Center** ou déclarer des mesures de pixels fixes pour l’espace de disposition pour vous assurer que le parent alloue suffisamment d’espace.

**Remarque relative à la migration : **WPF (Windows Presentation Foundation) avait une propriété **LayoutTransform** qui appliquait les transformations avant la passe de disposition. Toutefois, Windows Runtime XAML ne prend pas en charge la propriété **LayoutTransform**. (Microsoft Silverlight ne disposait pas non plus de cette propriété.)

## <span id="Applying_a_transform_to_a_UI_element"> </span> <span id="applying_a_transform_to_a_ui_element"> </span> <span id="APPLYING_A_TRANSFORM_TO_A_UI_ELEMENT"> </span>Application d’une transformation à un élément d’interface utilisateur

Quand vous appliquez une transformation à un objet, vous avez généralement pour but de définir la propriété [**UIElement.RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980). La définition de cette propriété ne change pas littéralement l’objet pixel par pixel. En fait, la propriété applique la transformation dans l’espace de coordonnées local où existe cet objet. Ensuite, la logique et l’opération de rendu (postérieures à la disposition) affichent les espaces de coordonnées combinés, ce qui donne l’impression que l’objet a changé d’apparence et éventuellement de disposition (si [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) a été appliqué).

Par défaut, chaque transformation d’affichage est centrée au point d’origine du système de coordonnées local de l’objet cible, (0,0). La seule exception est [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027), qui n’a aucune propriété de centrage à définir, car l’effet de translation est le même quel que soit son centre. Mais les autres transformations ont chacune des propriétés qui définissent les valeurs **CenterX** et **CenterY**.

Chaque fois que vous utilisez des transformations avec [**UIElement.RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980), rappelez-vous qu’il existe une autre propriété pour [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) qui affecte le comportement de la transformation : [**RenderTransformOrigin**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.rendertransformorigin.aspx). **RenderTransformOrigin** déclare si l’ensemble de la transformation doit s’appliquer au point par défaut (0,0) d’un élément ou à un autre point d’origine dans l’espace de coordonnées relatif de cet élément. Pour les éléments classiques, (0,0) place la transformation dans le coin supérieur gauche. Selon l’effet souhaité, vous pouvez choisir de changer **RenderTransformOrigin** au lieu de modifier les valeurs **CenterX** et **CenterY** des transformations. Notez que si vous appliquez à la fois les valeurs **RenderTransformOrigin** et **CenterX** / **CenterY**, les résultats peuvent être assez déroutants, surtout si vous animez l’une des valeurs.

Pour les besoins des tests de positionnement, un objet auquel une transformation est appliquée continue de répondre à l’entrée d’une manière attendue qui est cohérente avec son apparence visuelle dans l’espace x-y. Par exemple, si vous avez utilisé [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) pour déplacer un objet [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) de 400 pixels latéralement dans l’interface utilisateur, **Rectangle** répond aux événements [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.pointerpressed.aspx) quand l’utilisateur appuie à l’emplacement où **Rectangle** apparaît visuellement. Vous n’obtiendrez pas de faux événements si l’utilisateur appuie dans la zone où **Rectangle** se trouvait avant sa translation. En ce qui concerne les aspects relatifs à l’index Z qui affectent les tests de positionnement, l’application d’une transformation ne fait aucune différence. L’index Z, qui détermine l’élément qui gère les événements d’entrée pour un point de l’espace x-y, est toujours évalué en fonction de l’ordre enfant déclaré dans un conteneur. Cet ordre est généralement le même que celui dans lequel vous déclarez les éléments en XAML. Toutefois, pour les éléments enfants d’un objet [**Canvas**](https://msdn.microsoft.com/library/windows/apps/BR209267), vous pouvez modifier l’ordre en appliquant la propriété jointe [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.zindex.aspx) aux éléments enfants.

## <span id="Other_transform_properties"> </span> <span id="other_transform_properties"> </span> <span id="OTHER_TRANSFORM_PROPERTIES"> </span>Autres propriétés de transformation

-   [
            **Brush.Transform**](https://msdn.microsoft.com/library/windows/apps/BR228082) et [**Brush.RelativeTransform**](https://msdn.microsoft.com/library/windows/apps/BR228080) influencent la façon dont un élément [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076) utilise l’espace de coordonnées dans la zone où le **Brush** est appliqué afin de définir les propriétés visuelles telles que les premiers plans et les arrière-plans. Ces transformations ne sont pas pertinentes pour les pinceaux les plus courants (qui définissent généralement des couleurs unies avec [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962)), mais peuvent parfois être utiles quand vous peignez une zone avec un objet [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) ou [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210108).
-   La propriété [**Geometry.Transform**](https://msdn.microsoft.com/library/windows/apps/BR210066) vous permet d’appliquer une transformation à une figure géométrique avant d’utiliser cette dernière pour une valeur de propriété [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/BR243356).

## <span id="Animating_a_transform"> </span> <span id="animating_a_transform"> </span> <span id="ANIMATING_A_TRANSFORM"> </span>Animation d’une transformation

Les objets [**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006) peuvent être animés. Pour animer un élément **Transform**, appliquez à la propriété que vous souhaitez animer une animation d’un type compatible. Cela signifie généralement que vous utilisez des objets [**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136) ou [**DoubleAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR243136usingkeyframes) pour définir l’animation, car toutes les propriétés de transformation sont de type [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx). Les animations qui affectent une transformation utilisée pour une valeur [**UIElement.RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980) ne sont pas considérées comme étant des animations dépendantes, même si elles ont une durée non nulle. Pour plus d’informations sur les animations dépendantes, voir [Animations dans une table de montage séquentiel](storyboarded-animations.md).

Si vous animez des propriétés pour produire un effet comparable à une transformation en termes d’aspect visuel (par exemple l’animation de [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751) et [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) pour un élément [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706) au lieu de l’application d’un élément [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027)), ces animations sont presque toujours traitées comme des animations dépendantes. Vous devez activer les animations. En outre, l’animation peut entraîner des problèmes de performances importants, surtout si vous essayez de prendre en charge l’interaction de l’utilisateur pendant l’animation de l’objet. C’est pourquoi il est préférable d’utiliser une transformation et de l’animer plutôt que d’animer toute autre propriété où l’animation serait traitée comme une animation dépendante.

Pour cibler la transformation, [**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006) doit exister en tant que valeur de [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980). Généralement, vous devez placer un élément pour le type de transformation approprié dans le code XAML initial, parfois sans propriétés définies pour cette transformation.

En principe, vous devez utiliser une technique de ciblage indirect pour appliquer des animations aux propriétés d’une transformation. Pour plus d’informations sur la syntaxe du ciblage indirect, voir [Animations dans une table de montage séquentiel](storyboarded-animations.md) et [Syntaxe du chemin de propriété](https://msdn.microsoft.com/library/windows/apps/Mt185586).

Les styles par défaut des contrôles définissent parfois les animations des transformations dans le cadre du comportement de leur état visuel. Par exemple, les états visuels de [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/BR227538) utilisent des valeurs [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932) animées pour « faire tourner » les points de l’anneau.

Voici un exemple simple d’animation d’une transformation. Dans le cas présent, il s’agit d’animer l’élément [**Angle**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.rotatetransform.angle.aspx) d’un objet [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932) pour faire tourner un [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) sur place autour de son centre visuel. Comme cet exemple nomme **RotateTransform**, le ciblage d’animation indirect n’est pas nécessaire. Toutefois, vous pouvez omettre de nommer la transformation, nommer l’élément auquel la transformation s’applique et utiliser le ciblage indirect tel que `(UIElement.RenderTransform).(RotateTransform.Angle)`.

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

## <span id="Accounting_for_coordinate_frames_of_reference_at_run_time"> </span> <span id="accounting_for_coordinate_frames_of_reference_at_run_time"> </span> <span id="ACCOUNTING_FOR_COORDINATE_FRAMES_OF_REFERENCE_AT_RUN_TIME"> </span>Prise en compte des systèmes de coordonnées de référence au moment de l’exécution

[
            **UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) comporte une méthode nommée [**TransformToVisual**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.transformtovisual.aspx), qui peut générer un élément [**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006) mettant en corrélation les systèmes de coordonnées de référence pour deux éléments d’interface utilisateur. Vous pouvez l’utiliser pour comparer un élément au système de coordonnées de référence par défaut de l’application, si vous passez l’élément visuel racine comme premier paramètre. Cela peut être utile si vous avez capturé l’événement d’entrée d’un élément distinct, ou si vous essayez de prévoir le comportement de disposition sans demander de passe de disposition.

Les données d’événement obtenues à partir des événements de pointeur permettent d’accéder à une méthode [**GetCurrentPoint**](https://msdn.microsoft.com/library/windows/apps/BR212141), où vous pouvez spécifier un paramètre *relativeTo* afin de remplacer le système de coordonnées de référence par un élément spécifique au lieu de l’élément par défaut de l’application. Cette approche permet d’appliquer une transformation par translation de manière interne et de transformer automatiquement les données de coordonnées x-y au moment de la création de l’objet [**PointerPoint**](https://msdn.microsoft.com/library/windows/apps/BR242038) renvoyé.

## <span id="Describing_a_transform_mathematically"> </span> <span id="describing_a_transform_mathematically"> </span> <span id="DESCRIBING_A_TRANSFORM_MATHEMATICALLY"> </span>Description mathématique d’une transformation

Une transformation peut être décrite en termes de matrice de transformation. Une matrice 3x3 permet de décrire les transformations dans un plan x-y en deux dimensions. Les matrices de transformations affines peuvent être multipliées pour former un nombre quelconque de transformations linéaires, par exemple la rotation et l’inclinaison, puis la translation. La dernière colonne d’une matrice de transformation affine est égale à (0, 0, 1). Vous devez donc spécifier uniquement les membres des deux premières colonnes dans la description mathématique.

La description mathématique d’une transformation peut vous être utile si vous avez des connaissances en mathématiques ou que vous êtes habitué aux techniques de programmation graphique qui utilisent également des matrices pour décrire les transformations de l’espace de coordonnées. Il existe une classe dérivée de [**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006) qui vous permet d’exprimer une transformation directement à partir de sa matrice 3x3 : [**MatrixTransform**](https://msdn.microsoft.com/library/windows/apps/BR210137). **MatrixTransform** comporte une propriété [**Matrix**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.matrixtransform.matrix.aspx), qui possède une structure ayant six propriétés : [**M11**](https://msdn.microsoft.com/library/windows/apps/Hh673847), [**M12**](https://msdn.microsoft.com/library/windows/apps/Hh673853), [**M21**](https://msdn.microsoft.com/library/windows/apps/Hh673851), [**M22**](https://msdn.microsoft.com/library/windows/apps/Hh673849), [**OffsetX**](https://msdn.microsoft.com/library/windows/apps/Hh673810) et [**OffsetY**](https://msdn.microsoft.com/library/windows/apps/Hh673816). Chaque propriété [**Matrix**](https://msdn.microsoft.com/library/windows/apps/BR210127) utilise une valeur **Double** et correspond aux six valeurs pertinentes (colonnes 1 et 2) d’une matrice de transformation affine.

|                                             |                                             |     |
|---------------------------------------------|---------------------------------------------|-----|
| [**M11**](https://msdn.microsoft.com/library/windows/apps/Hh673847)         | [**M21**](https://msdn.microsoft.com/library/windows/apps/Hh673851)         | 0   |
| [**M12**](https://msdn.microsoft.com/library/windows/apps/Hh673853)         | [**M22**](https://msdn.microsoft.com/library/windows/apps/Hh673849)         | 0   |
| [**OffsetX**](https://msdn.microsoft.com/library/windows/apps/Hh673810) | [**OffsetY**](https://msdn.microsoft.com/library/windows/apps/Hh673816) | 1   |

 

Toute transformation que vous pouvez décrire avec un objet [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027), [**ScaleTransform**](https://msdn.microsoft.com/library/windows/apps/BR242940), [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932) ou [**SkewTransform**](https://msdn.microsoft.com/library/windows/apps/BR242950) peut également être décrite par un objet [**MatrixTransform**](https://msdn.microsoft.com/library/windows/apps/BR210137) avec une valeur [**Matrix**](https://msdn.microsoft.com/library/windows/apps/BR210127). Mais en général, vous n’utilisez que **TranslateTransform** et les autres, car il est plus facile de conceptualiser les propriétés de ces classes de transformation que de définir les composants de vecteur dans **Matrix**. Il est également plus facile d’animer les propriétés discrètes des transformations. **Matrix** est en fait une structure, et non un objet [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/BR242356) ; c’est pourquoi il ne peut pas prendre en charge des valeurs individuelles animées.

Certains outils de conception XAML qui vous permettent d’appliquer des opérations de transformation sérialisent les résultats en tant que [**MatrixTransform**](https://msdn.microsoft.com/library/windows/apps/BR210137). Dans ce cas, il peut être préférable de réutiliser le même outil de conception pour changer l’effet de transformation et resérialiser le code XAML, au lieu d’essayer de manipuler vous-même les valeurs de [**Matrix**](https://msdn.microsoft.com/library/windows/apps/BR210127) directement dans le code XAML.

## <span id="3-D_transforms"> </span> <span id="3-d_transforms"> </span> <span id="3-D_TRANSFORMS"> </span>Transformations 3D

Vous pouvez appliquer des effets 3D à tout objet [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) à l’aide des *transformations de perspective*. Par exemple, vous pouvez donner l’illusion qu’un objet est tourné vers vous ou vers l’arrière, selon un plan de perspective. Pour ce faire, affectez la valeur [**PlaneProjection**](https://msdn.microsoft.com/library/windows/apps/BR210192) à la propriété [**Projection**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.projection.aspx) de l’objet **UIElement**. La classe **PlaneProjection** définit l’affichage de la transformation dans une simulation d’espace 3D. Ce type de transformation est décrit de manière plus détaillée dans [Effets de perspective 3D pour une interface utilisateur en XAML](3-d-perspective-effects.md).

**Remarque** Il est techniquement possible d’obtenir des résultats similaires en combinant les classes [**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006), mais la technique [**PlaneProjection**](https://msdn.microsoft.com/library/windows/apps/BR210192) produit généralement un meilleur effet avec des images en tant que pinceaux. En outre, il est beaucoup plus facile d’obtenir les valeurs de propriétés exactes.

 

## <span id="related_topics"> </span>Rubriques connexes

* [Dessiner des formes](drawing-shapes.md)
* [Effets de perspective 3D pour une interface utilisateur en XAML](3-d-perspective-effects.md)
* [**Classe Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006)
 

 




<!--HONumber=Mar16_HO1-->
