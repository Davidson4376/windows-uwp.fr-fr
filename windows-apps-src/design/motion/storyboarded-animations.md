---
ms.assetid: 0CBCEEA0-2B0E-44A1-A09A-F7A939632F3A
title: Animations dans une table de montage séquentiel
description: Les animations de table de montage séquentiel ne sont pas seulement des animations au sens visuel.
ms.date: 07/13/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1107670e837dff294739e9ba38c7dea9004d1d62
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340345"
---
# <a name="storyboarded-animations"></a>Animations dans une table de montage séquentiel

Les animations de table de montage séquentiel ne sont pas seulement des animations au sens visuel. Une animation de table de montage séquentiel permet de changer la valeur d’une propriété de dépendance en tant que fonction de temps. Vous pouvez avoir notamment besoin d’une animation de table de montage séquentiel qui ne provient pas de la bibliothèque des animations pour définir l’état visuel d’un contrôle dans un modèle de contrôle ou une définition de page.

## <a name="differences-with-silverlight-and-wpf"></a>Différences entre Silverlight et WPF

Si vous connaissez déjà Microsoft Silverlight ou Windows Presentation Foundation (WPF), lisez cette section. Dans le cas contraire, vous pouvez l’ignorer.

En règle générale, la création d’animations de table de montage séquentiel dans une application du Windows Runtime est similaire à Silverlight ou WPF. Des différences importantes sont cependant à noter :

-   Les animations de table de montage séquentiel ne sont pas le seul moyen d’animer visuellement une interface utilisateur, ni le moyen le plus simple à la disposition des développeurs d’applications pour ce faire. À la place des animations de table de montage séquentiel, il est souvent préférable en matière de conception d’utiliser des animations de thème et de transition. Vous pouvez ainsi créer rapidement des animations d’interface utilisateur recommandées sans avoir besoin de cibler des propriétés d’animation, ce qui peut s’avérer délicat. Pour plus d’informations, voir [Vue d’ensemble des animations](xaml-animation.md).
-   Dans Windows Runtime, de nombreux contrôles XAML comportent des animations de thème et de transition inhérentes à leur comportement. Dans la plupart des cas, les contrôles WPF et Silverlight n’avaient pas de comportement d’animation par défaut.
-   Les applications personnalisées que vous créez ne peuvent pas toutes s’exécuter par défaut dans une application Windows Runtime, si le système d’animations détermine qu’elles peuvent nuire aux performances de votre interface utilisateur. Les animations pour lesquelles le système détermine qu’il pourrait y avoir un impact sur les performances portent le nom d’*animations dépendantes*. Elles sont dépendantes car le minutage de votre animation opère directement sur le thread d’interface utilisateur, qui est également celui sur lequel l’entrée utilisateur active et d’autres mises à jour tentent d’appliquer les modifications d’interface utilisateur au moment de l’exécution. Une animation dépendante qui consomme d’importantes ressources système sur le thread d’interface utilisateur peut dans certaines situations donner l’impression que l’application a cessé de répondre. Si votre animation entraîne la modification de la disposition ou est susceptible d’avoir une incidence sur les performances du thread d’interface utilisateur, vous devez alors activer l’animation dans votre code de façon explicite pour la voir s’exécuter. C’est là qu’entre en jeu la propriété **EnableDependentAnimation** sur des classes d’animation spécifiques. Pour plus d’informations, voir [Animations dépendantes et indépendantes](./storyboarded-animations.md#dependent-and-independent-animations).
-   Les fonctions d’accélération personnalisées ne sont actuellement pas prises en charge dans Windows Runtime.

## <a name="defining-storyboarded-animations"></a>Définition d’animations de table de montage séquentiel

Une animation de table de montage séquentiel permet de changer la valeur d’une propriété de dépendance en tant que fonction de temps. La propriété que vous animez n’est pas toujours une propriété qui a une incidence directe sur l’interface utilisateur de votre application. Mais étant donné que XAML sert à la définition de l’interface utilisateur des applications, vous êtes en général amené à animer une propriété d’interface utilisateur. Par exemple, vous pouvez animer l’angle d’un [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform) ou la valeur de la couleur de l’arrière-plan d’un bouton.

Vous pouvez être principalement amené à définir une animation de table de montage séquentiel, si vous êtes un créateur de contrôle ou remodélisez un contrôle, afin de définir des états visuels. Pour plus d’informations, voir [Animations dans une table de montage séquentiel pour les états visuels](https://docs.microsoft.com/previous-versions/windows/apps/jj819808(v=win.10)).

Que vous définissiez des états visuels ou une animation personnalisée pour une application, les concepts et les API pour les animations de table de montage décrits dans cette rubrique s’appliquent pour la plupart d’entre eux aux deux cas.

Afin d’être animée, la propriété que vous ciblez avec une animation de table de montage séquentiel doit être une *propriété de dépendance*. Une propriété de dépendance est une fonctionnalité clé de l’implémentation Windows Runtime XAML. Les propriétés accessibles en écriture de la plupart des éléments d’interface utilisateur sont en général implémentées en tant que propriétés de dépendance. Vous pouvez ainsi les animer, appliquer des valeurs liées aux données ou appliquer un [**Style**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) et cibler la propriété avec un [**Setter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Setter). Pour plus d’informations sur le fonctionnement des propriétés de dépendance, voir [Vue d’ensemble des propriétés de dépendance](https://docs.microsoft.com/windows/uwp/xaml-platform/dependency-properties-overview).

La plupart du temps, vous définissez une animation de table de montage séquentiel en écrivant du code XAML. Si vous utilisez un outil tel que Microsoft Visual Studio, le balisage XAML sera produit automatiquement. Il est également possible de définir une animation de table de montage séquentiel avec du code, mais cela est moins courant.

Examinons un exemple simple. Dans cet exemple XAML, la propriété [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) est animée sur un objet [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) particulier.

```xaml
<Page ...>
  <Page.Resources>
    <!-- Storyboard resource: Animates a rectangle's opacity. -->
    <Storyboard x:Name="myStoryboard">
      <DoubleAnimation
        Storyboard.TargetName="MyAnimatedRectangle"
        Storyboard.TargetProperty="Opacity"
        From="1.0" To="0.0" Duration="0:0:1"/>
    </Storyboard>
  </Page.Resources>

  <!--Page root element, UI definition-->
  <Grid>
    <Rectangle x:Name="MyAnimatedRectangle"
      Width="300" Height="200" Fill="Blue"/>
  </Grid>
</Page>
```

### <a name="identifying-the-object-to-animate"></a>Identification de l’objet à animer

Dans l’exemple précédent, la table de montage séquentiel animait la propriété [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) d’un [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle). La déclaration des animations ne s’effectue pas sur l’objet lui-même. Elle s’effectue dans la définition de chaque animation de table de montage séquentiel. Les tables de montage séquentiel sont en général définies dans du balisage XAML qui n’est pas à proximité de la définition d’interface utilisateur XAML de l’objet à animer. En fait, elles sont en général configurées comme une ressource XAML.

Pour connecter une animation à une cible, vous devez référencer la cible avec son nom de programmation. Vous devez toujours appliquer l’[attribut x:Name](https://docs.microsoft.com/windows/uwp/xaml-platform/x-name-attribute) dans la définition de l’interface utilisateur XAML pour nommer l’objet que vous voulez animer. Vous devez ensuite cibler l’objet à animer en définissant [**Storyboard.TargetName**](https://docs.microsoft.com/dotnet/api/system.windows.media.animation.storyboard.targetname) dans la définition de l’animation. Pour la valeur de **Storyboard.TargetName**, vous devez utiliser la chaîne de nom de l’objet cible, qui est ce que vous avez défini auparavant et ailleurs avec l’attribut x:Name.

### <a name="targeting-the-dependency-property-to-animate"></a>Ciblage de la propriété de dépendance à animer

Vous définissez la valeur de [**Storyboard.TargetProperty**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms616983(v=vs.95)) dans l’animation. Cela détermine quelle propriété spécifique de l’objet ciblé est animée.

Parfois vous devez cibler une propriété qui n’est pas une propriété immédiate de l’objet cible, mais qui est imbriquée plus profondément dans une relation objet-propriété. Vous devez souvent faire cela afin d’explorer un ensemble de valeurs de propriétés et d’objets contributives jusqu’à ce que vous puissiez référencer un type de propriété qui peut être animé ([**Double**](https://docs.microsoft.com/dotnet/api/system.double), [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point), [**Color**](https://docs.microsoft.com/uwp/api/Windows.UI.Color)). Ce concept porte le nom de *ciblage indirect* et la syntaxe de ciblage d’une propriété de cette manière porte le nom de *chemin d’accès de propriété*.

Voici un exemple : Un exemple courant de scénario pour une animation de table de montage séquentiel consiste à changer la couleur d’une partie de l’interface utilisateur ou d’un contrôle d’une application afin d’indiquer l’état de ce contrôle. Supposons que vous vouliez animer le [**Foreground**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.foreground) d’un [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) pour qu’il soit vert au lieu d’être rouge. Vous vous attendez à ce qu’intervienne [**ColorAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ColorAnimation) et vous avez raison. Cependant, aucune des propriétés sur les éléments d’interface utilisateur qui modifient la couleur de l’objet n’est de type [**Color**](https://docs.microsoft.com/uwp/api/Windows.UI.Color). Elles sont en fait de type [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush). Par conséquent, pour votre animation, vous devez cibler la propriété [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) de la classe [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) dont le type est dérivé de **Brush**. Ce type est en général utilisé pour les propriétés de couleur d’interface utilisateur. Voici ce que cela donne pour la formation d’un chemin d’accès de propriété pour le ciblage d’une propriété de votre animation :

```xaml
<Storyboard x:Name="myStoryboard">
  <ColorAnimation
    Storyboard.TargetName="tb1"
    Storyboard.TargetProperty="(TextBlock.Foreground).(SolidColorBrush.Color)"
    From="Red" To="Green"/>
</Storyboard>
```

Voici comment interpréter chacune des parties de la syntaxe :

- Chaque paire de parenthèses () contient un nom de propriété.
- Le nom de la propriété comporte un point. Ce point sépare un nom de type et un nom de propriété, par conséquent la propriété que vous identifiez est correcte.
- Le point au milieu, c’est-à-dire celui qui n’est pas entre parenthèses, est une étape. Pour la syntaxe cela veut dire, prendre la valeur de la première propriété (qui est un objet), aller dans son modèle d’objet et cibler une sous-propriété spécifique de la valeur de la première propriété.

Voici une liste des scénarios de ciblage d’animation pour lesquels vous utiliserez probablement le ciblage de propriété indirect avec des chaînes de chemin d’accès de propriété qui sont similaires à la syntaxe que vous utiliserez :

- Animation de la valeur [**X**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.translatetransform.x) d’un [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform), telle qu’elle est appliquée à un [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform): `(UIElement.RenderTransform).(TranslateTransform.X)`
- Animation d’une [**couleur**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) dans un [**GradientStop**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.GradientStop) d’un [**LinearGradientBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush), telle qu’elle est appliquée à un [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill): `(Shape.Fill).(GradientBrush.GradientStops)[0].(GradientStop.Color)`
- Animation de la valeur [**X**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.translatetransform.x) d’un [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform), qui est de 1 à 4 transformations dans un [**TransformGroup**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TransformGroup), comme appliqué à un [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform): `(UIElement.RenderTransform).(TransformGroup.Children)[3].(TranslateTransform.X)`

Vous remarquerez que certains de ces exemples utilisent des crochets autour des nombres. Il s’agit d’indexeurs. Ils indiquent que le nom de la propriété qui les précède a pour valeur une collection dans laquelle vous voulez un élément (identifié par un index basé sur zéro).

Vous pouvez également animer des propriétés XAML jointes. Veillez à toujours placer le nom de la propriété jointe entre parenthèses, par exemple `(Canvas.Left)`. Pour plus d’informations, voir [Animation des propriétés XAML jointes](./storyboarded-animations.md#animating-xaml-attached-properties).

Pour plus d’informations sur la façon d’utiliser un chemin d’accès de propriété pour le ciblage indirect de la propriété à animer, voir [Syntaxe de Property-path](https://docs.microsoft.com/windows/uwp/xaml-platform/property-path-syntax) ou [**Propriété jointe Storyboard.TargetProperty**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms616983(v=vs.95)).

### <a name="animation-types"></a>Types d’animation

Le système d’animations de Windows Runtime comporte trois types spécifiques auxquels les animations de table de montage séquentiel peuvent s’appliquer :

-   [**Double**](https://docs.microsoft.com/dotnet/api/system.double), peut être animé avec n’importe quel [ **DoubleAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DoubleAnimation)
-   [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point), peut être animé avec un [ **PointAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointAnimation)
-   [**Color**](https://docs.microsoft.com/uwp/api/Windows.UI.Color), peut être animé avec un [ **ColorAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ColorAnimation)

Il existe également un type d’animation [**Object**](https://docs.microsoft.com/dotnet/api/system.object) généralisé pour les valeurs de référence d’objet dont nous parlerons plus tard.

### <a name="specifying-the-animated-values"></a>Spécification des valeurs animées

Jusqu’à présent, nous vous avons montré comment cibler l’objet et la propriété à animer mais nous ne vous avons pas expliqué ce qu’il arrive à la valeur de la propriété lorsque l’animation est exécutée.

Les types d’animation que nous vous avons présentés sont parfois appelés animations **From**/**To**/**By**. Cela veut dire que l’animation change au fur et à mesure la valeur d’une propriété en utilisant les informations provenant de sa définition :

-   La valeur commence à la valeur **From**. Si vous ne spécifiez pas une valeur **From**, la valeur de départ est la valeur de la propriété animée avant que l’animation s’exécute. Il peut s’agir d’une valeur par défaut, d’une valeur provenant d’un style ou d’un modèle ou d’une valeur appliquée spécifiquement par une définition d’interface utilisateur XAML ou le code de l’application.
-   À la fin de l’animation, la valeur est la valeur **To**.
-   Ou, pour spécifier une valeur de fin par rapport à la valeur de départ, définissez la propriété **By** au lieu de la propriété **To**.
-   Si vous ne spécifiez pas une valeur **To** ou une valeur **By**, la valeur de fin est la valeur de la propriété animée avant que l’animation s’exécute. Dans ce cas, il vaut mieux que vous ayez une valeur **From**, car autrement l’animation ne changera pas la valeur ; ses valeurs de début et de fin étant les mêmes.
-   En général, une animation a au moins une des trois valeurs **From**, **By** ou **To**, mais jamais les trois.

Revenons à l’exemple XAML précédent et examinons de nouveau les valeurs **From** et **To** et la valeur **Duration**. L’exemple anime la propriété [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) et le type de la propriété **Opacity** est [**Double**](https://docs.microsoft.com/dotnet/api/system.double). Par conséquent, l’animation à utiliser ici est [**DoubleAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DoubleAnimation).

`From="1.0" To="0.0"` indique que lors de l’exécution de l’animation, la propriété [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) commence à la valeur 1 et s’anime à 0. Concernant la signification de ces valeurs [**Double**](https://docs.microsoft.com/dotnet/api/system.double) pour la propriété **Opacity**, avec cette animation, l’objet sera opaque au début puis deviendra transparent.

```xaml
...
<Storyboard x:Name="myStoryboard">
  <DoubleAnimation
    Storyboard.TargetName="MyAnimatedRectangle"
    Storyboard.TargetProperty="Opacity"
    From="1.0" To="0.0" Duration="0:0:1"/>
</Storyboard>
...
```

`Duration="0:0:1"` spécifie la durée pendant laquelle l’animation dure, c’est-à-dire la vitesse à laquelle le rectangle apparaît. Une propriété [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration) est spécifiée sous la forme *hours*:*minutes*:*seconds*. La durée dans cet exemple est d’une seconde.

Pour plus d’informations sur les valeurs [**Duration**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Duration) et la syntaxe XAML, voir [**Duration**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Duration).

> [!NOTE]
> Dans le cas de l’exemple que nous vous avons montré, si vous êtes sûr que pour l’état de départ de l’objet animé, [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) doit toujours être égal à 1, par défaut ou explicitement, vous pouvez omettre la valeur **From**. L’animation utilisera alors la valeur de départ et le résultat sera le même.

### <a name="fromtoby-are-nullable"></a>From/To/By sont des propriétés Nullable.

Nous avons mentionné précédemment que vous pouvez omettre **From**, **To** ou **By** et par conséquent utiliser les valeurs non animées actuelles en remplacement d’une valeur manquante. Le type des propriétés **From**, **To** ou **By** d’une animation n’est pas celui auquel vous vous attendiez. Par exemple, le type de la propriété [**DoubleAnimation.To**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.doubleanimation.easingfunction) n’est pas [**Double**](https://docs.microsoft.com/dotnet/api/system.double). En fait, il s’agit du type [**Nullable**](https://docs.microsoft.com/dotnet/api/system.nullable-1) pour **Double**. Et sa valeur par défaut est **null** et non 0. Cette valeur **null** indique au système d’animations que vous n’avez pas spécifiquement défini une valeur pour une propriété **From**, **To** ou **By**. Les C++ extensions de composantsC++visuels (/CX) n’ont pas de type **Nullable** . par conséquent, elle utilise [IReference](https://docs.microsoft.com/uwp/api/Windows.Foundation.IReference_T_) à la place.

### <a name="other-properties-of-an-animation"></a>Autres propriétés d’une animation

Les propriétés suivantes décrites dans cette section sont toutes facultatives du fait qu’elles ont des valeurs par défaut qui conviennent à la plupart des animations.

### <a name="autoreverse"></a>**AutoReverse**

Si vous ne spécifiez pas [**AutoReverse**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.autoreverse) ou [**RepeatBehavior**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.repeatbehavior) sur une animation, cette animation s’exécutera une fois pour la durée spécifiée par [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration).

La propriété [**AutoReverse**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.autoreverse) spécifie si une chronologie doit repartir en arrière une fois qu’elle est arrivée au terme de sa durée comme l’indique sa propriété [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration). Si vous l’avez définie sur **true**, l’animation repart en arrière une fois qu’elle est arrivée au terme de sa durée déclarée comme l’indique sa propriété [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration). Pour cela, elle remplace sa valeur de fin (**To**) par sa valeur de début (**From**). Cela signifie que la durée de l’animation est le double de la valeur de sa propriété [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration).

### <a name="repeatbehavior"></a>**RepeatBehavior**

La propriété [**RepeatBehavior**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.repeatbehavior) spécifie le nombre de fois qu’une chronologie est exécutée ou une durée plus longue pendant laquelle la chronologie doit se répéter. Par défaut, le nombre d’itérations d’une chronologie est « 1x », ce qui signifie qu’elle s’exécute une fois selon la valeur de sa propriété [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration) et qu’elle ne se répète pas.

Vous pouvez faire en sorte que l’animation exécute plusieurs itérations. Par exemple, avec une valeur « 3x », l’animation s’exécute trois fois Ou, vous pouvez spécifier une valeur différente de [**Duration**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Duration) pour [**RepeatBehavior**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.repeatbehavior). Cette valeur de **Duration** ne doit pas être plus longue que la valeur de **Duration** de l’animation elle-même pour être effective. Par exemple, si vous spécifiez une propriété **RepeatBehavior** avec la valeur « 0:0:10 », pour une animation dont la propriété [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration) a la valeur « 0:0:2 », cette animation se répète cinq fois. Si ces valeurs ne sont pas divisibles équitablement, l’animation est tronquée au moment où la valeur de **RepeatBehavior** est atteinte, ce qui peut être en plein milieu. Enfin, vous pouvez spécifier la valeur spéciale « Forever », qui exécute l’animation indéfiniment jusqu’à ce qu’elle soit délibérément arrêtée.

Pour plus d’informations sur les valeurs [**RepeatBehavior**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.RepeatBehavior) et la syntaxe XAML, voir [**RepeatBehavior**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.RepeatBehavior).

### <a name="fillbehaviorstop"></a>**FillBehavior = "arrêter"**

Par défaut, lorsqu’une animation se termine, l’animation conserve la valeur finale de **To** ou modifiée de **By** pour sa propriété de durée, même une fois celle-ci dépassée. Cependant, si vous définissez la valeur de la propriété [**FillBehavior**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.fillbehavior) sur [**FillBehavior.Stop**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FillBehavior), la valeur de la valeur animée avant que l’animation ait été appliquée est rétablie, ou plus précisément, la valeur effective déterminée par le système de propriétés de dépendance (pour plus d’informations sur cette distinction, voir [Vue d’ensemble des propriétés de dépendance](https://docs.microsoft.com/windows/uwp/xaml-platform/dependency-properties-overview)).

### <a name="begintime"></a>**BeginTime**

Par défaut, la valeur de la propriété [**BeginTime**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.begintime) d’une animation est « 0:0:0 ». Par conséquent, l’animation commence dès que son [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) (table de montage séquentiel) s’exécute. Vous pouvez changer cette valeur si le **Storyboard** contient plusieurs animations et si vous voulez échelonner le démarrage des autres animations par rapport à une animation initiale, ou pour créer délibérément un léger retard.

### <a name="speedratio"></a>**SpeedRatio**

Si un [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) contient plusieurs animations, vous pouvez changer la fréquence d’une ou de plusieurs animations par rapport au **Storyboard**. Le **Storyboard** parent détermine l’écoulement de la [**Duration**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Duration) pendant que les animations s’exécutent. Cette propriété n’est pas très souvent utilisée. Pour plus d’informations, voir [**SpeedRatio**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.speedratio).

## <a name="defining-more-than-one-animation-in-a-storyboard"></a>Définition de plusieurs animations dans un **Storyboard**

Un [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) peut contenir plusieurs définitions d’animation. Vous pouvez avoir plusieurs animations si vous appliquez des animations associées à deux propriétés du même objet cible. Par exemple, si vous changez la valeur des propriétés [**TranslateX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.compositetransform.translatex) et [**TranslateY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.compositetransform.translatey) d’un [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform) utilisé comme propriété [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) d’un élément d’interface utilisateur, cet élément sera traduit en diagonale. Il vous faut pour cela deux animations différentes, mais il est préférable que les animations appartiennent au même **Storyboard**, car ces deux animations devront toujours être exécutées ensemble.

Les animations ne doivent pas obligatoirement être du même type ou cibler le même objet. Elles peuvent avoir des durées et des valeurs de propriété différentes.

Lorsque le [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) parent s’exécute, chacune des animations qu’il contient s’exécute également.

La classe [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) et les types d’animation ont en commun beaucoup de propriétés d’animation, car ils ont la même classe de base [**Timeline**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Timeline). Par conséquent, un **Storyboard** peut avoir une propriété [**RepeatBehavior**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.repeatbehavior) ou une propriété [**BeginTime**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.begintime). Vous ne définissez pas ces propriétés sur un **Storyboard** sauf si vous voulez que toutes les animations qu’il contient aient ce comportement. En règle générale, toute propriété **Timeline** définie pour un **Storyboard** s’applique à toutes ses animations enfants. Si cette propriété n’est pas définie, le **Storyboard** a une durée implicite qui est calculée à partir de la valeur la plus longue de la propriété [**Duration**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Duration) des animations qu’il contient. Si une propriété [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration) définie explicitement sur un **Storyboard** est plus courte que celle de l’une de ses animations enfants, cette animation sera coupée, ce qui n’est en général pas l’objectif recherché.

Une table de montage séquentiel (storyboard) ne peut pas contenir deux animations qui tentent de cibler et animer la même propriété sur le même objet. Si vous tentez cela, vous obtiendrez une erreur d’exécution lorsque la table de montage séquentiel tente de s’exécuter. Cette restriction s’applique même si les animations ne se chevauchent pas, car leurs durées et leurs valeurs de la propriété [**BeginTime**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.begintime) sont délibérément différentes. Si vous voulez vraiment appliquer une chronologie d’animation plus complexe à la même propriété dans une table de montage séquentiel, vous devez pour cela utiliser une animation par images clés. Voir [Animations par images clés et animations de fonctions d’accélération](key-frame-and-easing-function-animations.md).

Le système d’animations peut appliquer plusieurs animations à la valeur d’une propriété, si celles-ci proviennent de plusieurs tables de montage séquentiel. L’utilisation délibérée de ce comportement pour des tables de montage séquentiel exécutées simultanément n’est pas courante. Cependant, il est possible qu’une animation définie pour une application que vous appliquez à une propriété d’un contrôle modifie la valeur **HoldEnd** d’une animation qui a été précédemment exécutée dans le cadre du modèle d’état visuel du contrôle.

## <a name="defining-a-storyboard-as-a-resource"></a>Définition d’une table de montage séquentiel en tant que ressource

Un objet [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) (table de montage séquentiel) est le conteneur dans lequel vous placez des objets d’animation. En général, vous définissez le **Storyboard** (table de montage séquentiel) en tant que ressource qui est disponible pour l’objet que vous voulez animer, soit dans la propriété [**Resources**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.resources) au niveau page, soit dans la propriété [**Application.Resources**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.resources).

L’exemple suivant montre comment le [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) sera contenu dans une définition [**Resources**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.resources) de niveau page, dans laquelle le **Storyboard** est une ressource à clé de la [**Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) racine. Notez l’[attribut x:Name](https://docs.microsoft.com/windows/uwp/xaml-platform/x-name-attribute). Cet attribut vous permet de définir un nom de variable pour le **Storyboard** afin que d’autres éléments dans le balisage XAML ainsi que le code puissent référencer le **Storyboard** ultérieurement.

```xaml
<Page ...>
  <Page.Resources>
    <!-- Storyboard resource: Animates a rectangle's opacity. -->
    <Storyboard x:Name="myStoryboard">
      <DoubleAnimation
        Storyboard.TargetName="MyAnimatedRectangle"
        Storyboard.TargetProperty="Opacity"
        From="1.0" To="0.0" Duration="0:0:1"/>
    </Storyboard>
  </Page.Resources>
  <!--Page root element, UI definition-->
  <Grid>
    <Rectangle x:Name="MyAnimatedRectangle"
      Width="300" Height="200" Fill="Blue"/>
  </Grid>
</Page>
```

Définir les ressources à la racine XAML d’un fichier XAML tel que page.xaml ou app.xaml est une pratique courante pour organiser les ressources à clé dans le code XAML. Vous pouvez aussi factoriser les ressources dans des fichiers distincts et les fusionner dans des applications ou des pages. Pour plus d’informations, voir [Références aux ressources ResourceDictionary et XAML](https://docs.microsoft.com/windows/uwp/controls-and-patterns/resourcedictionary-and-xaml-resource-references).

> [!NOTE]
> Windows Runtime XAML prend en charge l’identification de ressources à l’aide de l’[attribut x:Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute) ou de l’[attribut x:Name](https://docs.microsoft.com/windows/uwp/xaml-platform/x-name-attribute). L’utilisation de l’attribut x:Name est plus courante pour un [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard), car vous devrez le référencer plus tard par un nom de variable pour pouvoir appeler sa méthode [**Begin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.begin) et exécuter les animations. Si vous utilisez l’[attribut x:Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute), vous devrez utiliser des méthodes [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) telles que l’indexeur [**Item**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.item) pour le récupérer en tant que ressource à clé et effectuer un cast de cet objet vers le **Storyboard** pour utiliser les méthodes **Storyboard**.

### <a name="storyboards-for-visual-states"></a>Tables de montage séquentiel pour les états visuels

Vous pouvez aussi placer vos animations dans une unité [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) quand vous déclarez les animations d’état visuel pour l’apparence visuelle d’un contrôle. Dans ce cas, les éléments **Storyboard** que vous définissez vont dans un conteneur [**VisualState**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState) imbriqué plus profondément dans un [**Style**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) (c’est **Style** qui est la ressource à clé). Vous n’avez pas besoin de clé ou de nom pour votre **Storyboard** dans ce cas, car c’est le **VisualState** qui a un nom cible qui peut être appelé par le [**VisualStateManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.visualstatemanager). Les styles des contrôles sont souvent factorisés dans des fichiers XAML [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) distincts, plutôt que placés dans une collection **Resources** de pages ou d’applications. Pour plus d’informations, voir [Animations dans une table de montage séquentiel pour les états visuels](https://docs.microsoft.com/previous-versions/windows/apps/jj819808(v=win.10)).

## <a name="dependent-and-independent-animations"></a>Animations dépendantes et indépendantes

Nous allons maintenant aborder des points importants concernant le fonctionnement du système d’animations. Notamment, l’interaction de l’animation avec le rendu d’une application Windows Runtime à l’écran et l’utilisation des threads de traitement par ce rendu. Une application Windows Runtime comporte toujours un thread d’interface utilisateur principal qui est chargé de mettre à jour l’écran avec les informations actuelles. Elle comporte également un thread de composition qui permet de précalculer des dispositions immédiatement avant qu’elles n’apparaissent. Lorsque vous animez l’interface utilisateur, cela risque de mettre à rude épreuve le thread d’interface utilisateur. Le système doit redessiner de grandes zones de l’écran avec des intervalles assez courts entre chaque actualisation. Cela est nécessaire pour capturer la dernière valeur de la propriété animée. Si vous ne faites pas attention, une animation peut rendre l’interface utilisateur moins réactive ou nuire aux performances d’autres fonctionnalités de l’application se trouvant également sur le même thread d’interface utilisateur.

Une animation qui est considérée comment pouvant ralentir le thread d’interface utilisateur est appelée une *animation dépendante*. Une animation ne présentant pas ce risque est appelée une *animation indépendante*. La distinction entre les animations dépendantes et indépendantes n’est pas uniquement déterminée par leur type ([**DoubleAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DoubleAnimation), etc.) comme nous l’avons décrit auparavant. Elle est déterminée par les propriétés que vous animez et d’autres facteurs tels que l’héritage et la composition des contrôles. Dans certains cas, il peut arriver que, même si une animation change l’interface utilisateur, elle n’ait qu’un impact minime sur le thread d’interface utilisateur. Elle est alors traitée par le thread de composition en tant qu’animation indépendante.

Une animation est indépendante si elle a les caractéristiques suivantes :

-   La propriété [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration) de l’animation est de 0 seconde (voir Avertissement).
-   L’animation cible [**UIElement.Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity).
-   L’animation cible une valeur de sous-propriété de ces propriétés [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) : [**Transform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transform3d), [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform), [**projection**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.projection), [**clip**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.clip)
-   L’animation cible [**Canvas.Left**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.left) ou [**Canvas.Top**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.top).
-   L’animation cible une valeur [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) et utilise un [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) pour animer sa propriété [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color).
-   L’animation est de type [**ObjectAnimationUsingKeyFrames**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames).

> [!WARNING]
> Pour que votre animation soit traitée en tant qu’animation indépendante, vous devez définir explicitement `Duration="0"`. Par exemple, si vous supprimez `Duration="0"` de ce code XAML, l’animation est considérée en tant qu’animation dépendante, même si la propriété [**KeyTime**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.doublekeyframe.keytime) de l’image est « 0:0:0 ».

```xaml
<Storyboard>
  <DoubleAnimationUsingKeyFrames
    Duration="0"
    Storyboard.TargetName="Button2"
    Storyboard.TargetProperty="Width">
    <DiscreteDoubleKeyFrame KeyTime="0:0:0" Value="200"/>
  </DoubleAnimationUsingKeyFrames>
</Storyboard>
```

Si votre animation ne correspond pas à ces critères, il s’agit probablement d’une animation dépendante. Par défaut, le système d’animations n’exécutera pas une animation dépendante. Par conséquent, au cours du processus de développement et de test, il est possible que vous ne voyiez pas votre animation s’exécuter. Vous pouvez quand même utiliser cette animation, mais vous devez spécifiquement l’activer en tant qu’animation dépendante. Pour activer votre animation, attribuez la valeur **true** à la propriété **EnableDependentAnimation** de l’objet animation. (Chaque sous-classe [**Timeline**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Timeline) qui représente une animation implémente différemment cette propriété. Mais toutes les implémentations de cette propriété sont nommées `EnableDependentAnimation`.)

Le fait que l’activation des animations dépendantes repose sur le développeur d’application est un aspect de conception délibéré du système d’animations et du développement d’applications. Nous voulons que les développeurs tiennent compte du fait que les animations ont un impact sur la réactivité de votre interface utilisateur. Les animations défectueuses sont difficiles à isoler et déboguer dans une application complète. Par conséquent, il est préférable d’activer uniquement les animations dépendantes dont vous avez besoin pour l’interface utilisateur de votre application. Nous avons donc voulu éviter que des animations décoratives qui utilisent de nombreux cycles nuisent aux performances de votre application. Pour plus de conseils relatifs aux performances des animations, voir [Optimiser les animations et le contenu multimédia](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-animations-and-media).

En tant que développeur d’application, vous pouvez également choisir d’appliquer un paramètre pour toute l’application qui désactive toujours les animations dépendantes, même celles où la propriété **EnableDependentAnimation** a la valeur **true**. Voir [**Timeline.AllowDependentAnimations**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.allowdependentanimations).

> [!TIP]
> Si vous utilisez le volet animation dans Blend pour Visual Studio 2019, chaque fois que vous tentez d’appliquer une animation dépendante à une propriété d’état visuel, des avertissements s’affichent dans le concepteur. Les avertissements ne s’affichent pas dans la sortie ou la Liste d’erreurs de la génération. Si vous modifiez du code XAML manuellement, le concepteur n’affiche pas d’avertissement. Au moment de l’exécution lors du débogage, la sortie de débogage du volet de sortie indique un avertissement indiquant que l’animation n’est pas indépendante et sera ignorée.


## <a name="starting-and-controlling-an-animation"></a>Démarrage et contrôle d’une animation

Tout ce que nous vous avons montré jusqu’à présent n’entraîne pas l’exécution ou l’application d’une animation ! Les changements de valeur qu’une animation déclare en XAML sont latents et ne se produisent que lorsque l’animation démarre et est exécutée. Vous devez explicitement démarrer une animation en tenant compte de la durée de vie de l’application ou de l’expérience utilisateur. Au niveau le plus simple, le démarrage d’une animation s’effectue en appelant la méthode [**Begin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.begin) sur le [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) qui est le parent de cette animation. Vous ne pouvez pas appeler des méthodes depuis XAML directement ; par conséquent, quoi que vous fassiez pour activer vos animations, vous devrez le faire dans le code. Il pourra s’agir du « code-behind », c’est-à-dire du code des pages ou des composants de votre application, ou peut-être de la logique du contrôle si vous définissez une classe de contrôle personnalisée.

En général, vous appelez la méthode [**Begin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.begin) et laissez l’animation s’exécuter jusqu’au bout. Cependant, vous pouvez également utiliser les méthodes [**Pause**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.pause), [**Resume**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.resume) et [**Stop**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.stop) pour contrôler le [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) au moment de l’exécution, ainsi que d’autres API prévues pour des scénarios de contrôle d’animations plus avancés.

Lorsque vous appelez [**Begin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.begin) sur une table de montage séquentiel qui contient une animation qui se répète indéfiniment (`RepeatBehavior="Forever"`), cette animation s’exécute jusqu’à ce que la page la contenant soit déchargée ou que vous appeliez [**Pause**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.pause) ou [**Stop**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.stop).

### <a name="starting-an-animation-from-app-code"></a>Démarrage d’une animation à partir du code de l’application

Vous pouvez démarrer des animations automatiquement ou en réponse à des actions de l’utilisateur. Pour les démarrer automatiquement, vous devez utiliser en général un événement de durée de vie d’objet tel que [**Loaded**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded) qui servira de déclencheur de l’animation. L’événement **Loaded** est idéal pour cela, car à ce stade, l’interface utilisateur est prête à répondre aux actions de l’utilisateur, et l’animation ne sera pas coupée au début à cause du chargement en cours d’une autre partie de l’interface utilisateur.

Dans notre exemple, l’événement [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed) est attaché au rectangle pour que l’animation se lance quand l’utilisateur clique sur le rectangle.

```xaml
<Rectangle PointerPressed="Rectangle_Tapped"
  x:Name="MyAnimatedRectangle"
  Width="300" Height="200" Fill="Blue"/>
```

Le gestionnaire d’événements démarre le [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) (l’animation) en utilisant la méthode [**Begin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.begin) de l’objet **Storyboard**.

```csharp
myStoryboard.Begin();
```

```cppwinrt
myStoryboard().Begin();
```

```cpp
myStoryboard->Begin();
```

```vb
myStoryBoard.Begin()
```

Vous pouvez gérer l’événement [**Completed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.completed) si vous souhaitez qu’une autre logique s’exécute après que l’animation a fini d’appliquer les valeurs. De même, pour le dépannage des interactions système de propriétés/animation, la méthode [**GetAnimationBaseValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.getanimationbasevalue) peut être utile.

> [!TIP]
> Lorsque vous écrivez le code d’un scénario d’application et envisagez de démarrer une animation directement à partir de ce code, vérifiez si la bibliothèque d’animations ne contient pas déjà une animation ou une transition correspondant à votre scénario d’interface utilisateur. Les animations de la bibliothèque s’exécutent de façon homogène sur toutes les applications Windows Runtime et sont plus faciles à utiliser.

 

### <a name="animations-for-visual-states"></a>Animations pour les états visuels

Un [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) s’exécute différemment selon s’il est utilisé pour définir l’état visuel d’un contrôle ou si une application l’exécute directement. Appliqué à une définition d’état visuel dans XAML, le **Storyboard** est un élément d’un [**VisualState**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState). L’API [**VisualStateManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.visualstatemanager), quant à elle, permet de contrôler l’état. Les animations s’exécuteront selon leurs valeurs et les valeurs des propriétés [**Timeline**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Timeline) lorsque le **VisualState** qui les contient est utilisé par un contrôle. Pour plus d’informations, voir [Tables de montage pour les états visuels](https://docs.microsoft.com/previous-versions/windows/apps/jj819808(v=win.10)). Pour les états visuels, la propriété [**FillBehavior**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.fillbehavior) apparente est différente. Si un état visuel change, tous les changements de propriété appliqués par l’état précédent et ses animations sont annulés, même si le nouvel état visuel n’applique pas de façon spécifique une nouvelle animation à une propriété.

### <a name="storyboard-and-eventtrigger"></a>**Storyboard** et **EventTrigger**

Il existe un moyen de démarrer une animation qui peut être déclaré entièrement dans XAML. Cependant, cette technique n’est plus courante. Il s’agit d’une syntaxe héritée de WPF et d’anciennes versions de Silverlight avant la prise en charge de [**VisualStateManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.visualstatemanager). Cette syntaxe de [**EventTrigger**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.EventTrigger) fonctionne toujours dans Windows Runtime XAML pour des raisons d’importation et de compatibilité, mais uniquement comme déclencheur de l’événement [**FrameworkElement.Loaded**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded). Toute tentative de déclencher d’autres événements lèvera des exceptions ou échouera. Pour plus d’informations, voir [**EventTrigger**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.EventTrigger) ou [**BeginStoryboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.BeginStoryboard).

## <a name="animating-xaml-attached-properties"></a>Animation des propriétés XAML jointes

Il ne s’agit pas d’un scénario courant, mais vous pouvez appliquer une valeur animée à une propriété XAML jointe. Pour plus d’informations sur la nature des propriétés jointes et leur fonctionnement, voir [Vue d’ensemble des propriétés jointes](https://docs.microsoft.com/windows/uwp/xaml-platform/attached-properties-overview). Le ciblage des propriétés jointes nécessite une [syntaxe de chemin de propriété](https://docs.microsoft.com/windows/uwp/xaml-platform/property-path-syntax) qui place entre parenthèses le nom de la propriété. Vous pouvez animer les propriétés jointes intégrées telles que [**Canvas.ZIndex**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc190397(v=vs.95)) à l’aide d’un objet [**ObjectAnimationUsingKeyFrames**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames) qui applique les valeurs entières discrètes. Toutefois, une limitation existante d’une implémentation XAML Windows Runtime ne vous permet pas d’animer une propriété jointe personnalisée.

## <a name="more-animation-types-and-next-steps-for-learning-about-animating-your-ui"></a>Autres types d’animation et étapes suivantes pour apprendre à animer votre interface utilisateur

Jusqu’à présent nous vous avons montré les animations personnalisées qui s’animent entre deux valeurs et qui interpolent de façon linéaire les valeurs pendant que l’animation s’exécute. Ces animations sont appelées **From**/**To**/**By**. Mais il existe un autre type d’animation qui vous permet de déclarer des valeurs intermédiaires entre le début et la fin de l’animation. Il s’agit des *animations par images clés*. Il existe aussi un moyen d’altérer la logique d’interpolation sur une animation **From**/**To**/**By** ou une animation par images clés. Pour cela, il suffit d’appliquer une fonction d’accélération. Pour plus d’informations sur ces concepts, voir [Animations par images clés et animations de fonctions d’accélération](key-frame-and-easing-function-animations.md).

## <a name="related-topics"></a>Rubriques connexes

* [Syntaxe de chemin de propriété](https://docs.microsoft.com/windows/uwp/xaml-platform/property-path-syntax)
* [Vue d’ensemble des propriétés de dépendance](https://docs.microsoft.com/windows/uwp/xaml-platform/dependency-properties-overview)
* [Animations de fonctions d’image clé et d’accélération](key-frame-and-easing-function-animations.md)
* [Animations de storyboards pour les États visuels](https://docs.microsoft.com/previous-versions/windows/apps/jj819808(v=win.10))
* [Modèles de contrôles](https://docs.microsoft.com/windows/uwp/controls-and-patterns/control-templates)
* [**S**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard)
* [**Storyboard. TargetProperty**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms616983(v=vs.95))
 

 




