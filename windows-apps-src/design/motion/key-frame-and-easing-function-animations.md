---
author: Jwmsft
title: Animations par images clés et animations de fonctions d’accélération
ms.assetid: D8AF24CD-F4C2-4562-AFD7-25010955D677
description: Les animations par clés linéaires, les animations par clés avec KeySpline ou les animations de fonctions d’accélération sont utilisées avec le même scénario.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2076866cc31f50bd7ccfc6730bca5be96ffdc7c3
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6996982"
---
# <a name="key-frame-animations-and-easing-function-animations"></a>Animations par images clés et animations de fonctions d’accélération



Les animations par images clés linéaires, les animations par images clés avec une valeur **KeySpline** ou les animations de fonctions d’accélération constituent trois techniques différentes pour pratiquement le même scénario : créer une animation de table de montage séquentiel qui est un peu plus complexe et dont le comportement est non linéaire d’un état de départ à un état de fin.

## <a name="prerequisites"></a>Prérequis

Lisez au préalable la rubrique [Animations de table de montage séquentiel](storyboarded-animations.md). Cette rubrique s’appuie sur les concepts d’animation expliqués dans la rubrique [Animations de table de montage séquentiel](storyboarded-animations.md) mais n’y reviendra pas. Par exemple, la rubrique [Animations de table de montage séquentiel](storyboarded-animations.md) décrit comment cibler les animations, les tables de montage en tant que ressources, les valeurs de la propriété [**Timeline**](https://msdn.microsoft.com/library/windows/apps/BR210517) telles que [**Duration**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.duration), [**FillBehavior**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.fillbehavior), etc.

## <a name="animating-using-key-frame-animations"></a>Animation par images clés

Les animations par images clés autorisent plusieurs valeurs cibles pouvant être atteintes à divers points de la chronologie. En d’autres termes, chaque image clé peut spécifier une valeur intermédiaire différente, et la dernière image clé atteinte correspond à la valeur finale de l’animation. En précisant plusieurs valeurs à animer, il vous est alors possible d’effectuer des animations plus complexes. Les animations par images clés utilisent également différentes logiques d’interpolation, qui sont mises en œuvre en tant que sous-classe **KeyFrame** différente par type d’animation. Chaque type d’animation par images clés utilise notamment une variation **Discrete**, **Linear**, **Spline** et **Easing** de sa classe **KeyFrame** pour spécifier ses images clés. Par exemple, pour spécifier une animation qui cible un élément [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) et utilise des images clés, vous pouvez déclarer des images clés avec [**DiscreteDoubleKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR243130), [**LinearDoubleKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR210316), [**SplineDoubleKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR210446) et [**EasingDoubleKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR210269). Vous pouvez utiliser n’importe lequel de ces types ou tous ces types dans une collection **KeyFrames** unique pour changer l’interpolation chaque fois qu’une nouvelle image clé est atteinte.

Concernant le comportement de l’interpolation, chaque image clé contrôle l’interpolation jusqu’à ce que son temps **KeyTime** soit atteint. Sa **Value** est atteinte en même temps que ce temps. S’il y a d’autres images clés après cette valeur, la valeur devient la valeur de départ de l’image clé suivante dans la séquence.

Au début de l’animation, si aucune image clé avec un **KeyTime** dont la valeur est 0:0:0 existe, la valeur de départ est la valeur non animée de la propriété. Cela est similaire au comportement d’une animation **From**/**To**/**By** en l’absence de **From**.

La durée d’une animation par images clés est implicitement la durée égale à la valeur **KeyTime** la plus élevée définie dans une de ses images clés. Vous pouvez définir un élément [**Duration**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.duration) explicite si vous voulez, mais veillez à ce que sa valeur ne soit pas inférieure à celle d’un élément **KeyTime** dans vos images clés ou vous risquez de couper une partie de l’animation.

En plus de l’élément [**Duration**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.duration), vous pouvez définir toutes les propriétés [**Timeline**](https://msdn.microsoft.com/library/windows/apps/BR210517) sur une animation par images clés, comme vous le feriez avec une animation **From**/**To**/**By**, car les classes de l’animation par images clés dérivent également de **Timeline**. Ces classes sont les suivantes:

-   [**AutoReverse**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.autoreverse) : une fois que la dernière image clé est atteinte, les images sont répétées dans l’ordre inverse à partir de la fin. Cela double la durée apparente de l’animation.
-   [**BeginTime**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.begintime) : retarde le démarrage de l’animation. La chronologie des valeurs **KeyTime** dans les images ne commence à compter que lorsque le **BeginTime** est atteint, il n’y a donc aucun risque de couper des images.
-   [**FillBehavior**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.fillbehavior) : détermine ce qui se produit lorsque la dernière image clé est atteinte. **FillBehavior** n’a aucun effet sur les images clés intermédiaires.
-   [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.repeatbehaviorproperty) :
    -   Si définie sur **Forever**, les images clés et leur chronologie se répètent indéfiniment.
    -   Si définie sur un nombre d’itérations, la chronologie se répète autant de fois que ce nombre d’itérations.
    -   Si définie sur un [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377), la chronologie se répète jusqu’à ce que cette durée soit atteinte. Cela risque de tronquer l’animation au cours de la séquence d’images clés, s’il ne s’agit pas d’un facteur entier de la durée implicite de la chronologie.
-   [**SpeedRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.speedratioproperty) (peu utilisée)

### <a name="linear-key-frames"></a>Images clés linéaires

Les images clés linéaires produisent une interpolation linaire simple de la valeur jusqu’à ce que le **KeyTime** de l’image soit atteint. Ce comportement d’interpolation est très similaire aux animations plus simples **From**/**To**/**By** décrites dans la rubrique [Animations de table de montage](storyboarded-animations.md).

Voici comment utiliser une animation par images clés pour modifier la hauteur du rendu d’un rectangle, à l’aide d’images clés linéaires. Cet exemple exécute une animation où la hauteur du rectangle augmente légèrement et de façon linéaire pendant les 4 premières secondes, puis elle augmente rapidement à la dernière seconde pour atteindre le double de la hauteur de départ.

```xml
<StackPanel>
    <StackPanel.Resources>
        <Storyboard x:Name="myStoryboard">
            <DoubleAnimationUsingKeyFrames
              Storyboard.TargetName="myRectangle"
              Storyboard.TargetProperty="(UIElement.RenderTransform).(ScaleTransform.ScaleY)">
                <LinearDoubleKeyFrame Value="1" KeyTime="0:0:0"/>
                <LinearDoubleKeyFrame Value="1.2" KeyTime="0:0:4"/>
                <LinearDoubleKeyFrame Value="2" KeyTime="0:0:5"/>
            </DoubleAnimationUsingKeyFrames>
        </Storyboard>
    </StackPanel.Resources>
</StackPanel>
```

### <a name="discrete-key-frames"></a>Images clés discrètes

Les images clés discrètes n’utilisent aucune interpolation. Lorsqu’un **KeyTime** est atteint, la nouvelle **Value** est appliquée. En fonction de la propriété d’interface utilisateur qui est animée, cela produit souvent une animation qui semble «sauter». Il s’agit du comportement esthétique voulu. Vous pouvez réduire les sauts apparents en augmentant le nombre d’images clés que vous déclarez, mais si vous recherchez une animation fluide, utilisez de préférence des images clés linéaires ou Spline.

> [!NOTE]
> Les images clés discrètes sont le seul moyen d’animer une valeur qui n’est pas de type [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx), [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) et [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723) avec une classe [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR243132). Nous reviendrons sur ce point plus loin dans cette rubrique.

### <a name="spline-key-frames"></a>Images clés Spline

Une image clé Spline génère une transition variable entre les valeurs en fonction de la valeur de la propriété **KeySpline**. Cette propriété précise les premier et dernier points de contrôle d’une courbe de Bézier qui décrit l’accélération de l’animation. Une propriété [**KeySpline**](https://msdn.microsoft.com/library/windows/apps/BR210307) définit une relation fonction-temps où le graphique fonction-temps a la forme de la courbe de Bézier. En général, vous spécifiez une valeur **KeySpline** dans une chaîne d’attributs raccourcie XAML qui comporte quatre valeurs [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) séparées par des espaces ou des virgules. Ces valeurs sont des paires X,Y pour deux points de contrôle de la courbe de Bézier. X correspond au temps et Y est le modificateur de fonction de la valeur. Chaque valeur doit toujours être comprise entre 0 et 1. Sans modification des points de contrôle définis par **KeySpline**, la ligne droite de 0,0 à 1,1 est la représentation de la fonction dans le temps pour une interpolation linéaire. Vos points de contrôle modifient la forme de la courbe et par conséquent le comportement de la fonction dans le temps pour l’animation de spline. Un graphique est probablement le meilleur moyen de le représenter visuellement. Vous pouvez exécuter l’[exemple de visualiseur de courbe clé Silverlight](http://samples.msdn.microsoft.com/Silverlight/SampleBrowser/index.htm#/?sref=KeySplineExample) dans un navigateur pour voir comment les points de contrôle modifient la courbe et comment une animation exemple s’exécute lorsqu’elle est utilisée en tant que valeur **KeySpline**.

L’exemple suivant montre trois images clés différentes appliquées à une animation, la dernière étant une animation de courbe clé pour une valeur [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) ([**SplineDoubleKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR210446)). Notez la chaîne 0.6,0.0 0.9,0.00 appliquée pour **KeySpline**. Cela produit une courbe où l’animation semble s’exécuter d’abord lentement, puis atteint rapidement la valeur juste avant que le **KeyTime** soit atteint.

```xml
<Storyboard x:Name="myStoryboard">
    <!-- Animate the TranslateTransform's X property
        from 0 to 350, then 50,
        then 200 over 10 seconds. -->
    <DoubleAnimationUsingKeyFrames
        Storyboard.TargetName="MyAnimatedTranslateTransform"
        Storyboard.TargetProperty="X"
        Duration="0:0:10" EnableDependentAnimation="True">

        <!-- Using a LinearDoubleKeyFrame, the rectangle moves 
            steadily from its starting position to 500 over 
            the first 3 seconds.  -->
        <LinearDoubleKeyFrame Value="500" KeyTime="0:0:3"/>

        <!-- Using a DiscreteDoubleKeyFrame, the rectangle suddenly 
            appears at 400 after the fourth second of the animation. -->
        <DiscreteDoubleKeyFrame Value="400" KeyTime="0:0:4"/>

        <!-- Using a SplineDoubleKeyFrame, the rectangle moves 
            back to its starting point. The
            animation starts out slowly at first and then speeds up. 
            This KeyFrame ends after the 6th second. -->
        <SplineDoubleKeyFrame KeySpline="0.6,0.0 0.9,0.00" Value="0" KeyTime="0:0:6"/>
    </DoubleAnimationUsingKeyFrames>
</Storyboard>
```

### <a name="easing-key-frames"></a>Images clés d’accélération

Une image clé d’accélération est une image clé où l’interpolation est appliquée et où la fonction dans le temps de l’interpolation est contrôlée par plusieurs formules mathématiques prédéfinies. Vous pouvez obtenir le même résultat avec une image clé spline et avec certains des types de fonction d’accélération, mais certaines fonctions d’accélération, telles que [**BackEase**](https://msdn.microsoft.com/library/windows/apps/BR243049), ne peuvent pas être reproduites avec une image clé spline.

Pour appliquer une fonction d’accélération à une image clé d’accélération, vous devez définir la propriété **EasingFunction** en tant qu’élément de propriété dans le balisage XAML de cette image clé. Pour la valeur, spécifiez un élément objet pour l’un des types de fonction d’accélération.

Cet exemple applique un [**CubicEase**](https://msdn.microsoft.com/library/windows/apps/BR243126) puis un [**BounceEase**](https://msdn.microsoft.com/library/windows/apps/BR243057) en tant qu’images clés successives à un [**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136) pour créer un effet de rebond.

```xml
<Storyboard x:Name="myStoryboard">
    <DoubleAnimationUsingKeyFrames Duration="0:0:10"
        Storyboard.TargetProperty="Height"
        Storyboard.TargetName="myEllipse">

        <!-- This keyframe animates the ellipse up to the crest 
            where it slows down and stops. -->
        <EasingDoubleKeyFrame Value="-300" KeyTime="00:00:02">
            <EasingDoubleKeyFrame.EasingFunction>
                <CubicEase/>
            </EasingDoubleKeyFrame.EasingFunction>
        </EasingDoubleKeyFrame>

        <!-- This keyframe animates the ellipse back down and makes
            it bounce. -->
        <EasingDoubleKeyFrame Value="0" KeyTime="00:00:06">
            <EasingDoubleKeyFrame.EasingFunction>
                <BounceEase Bounces="5"/>
            </EasingDoubleKeyFrame.EasingFunction>
        </EasingDoubleKeyFrame>
    </DoubleAnimationUsingKeyFrames>
</Storyboard>
```

Il s’agit juste d’un exemple de fonction d’accélération. Nous allons aborder ce sujet plus en détail dans la section suivante.

## <a name="easing-functions"></a>Fonctions d’accélération

Les fonctions d’accélération vous permettent d’appliquer des formules mathématiques personnalisées à vos animations. Les opérations mathématiques sont souvent plus utiles pour produire des animations qui simulent la physique réelle dans un système de coordonnées 2-D. Par exemple, vous voudrez peut-être qu’un objet rebondisse de façon réaliste ou se comporte comme s’il était monté sur ressorts. Vous pouvez utiliser des animations par images clés ou encore des animations **From**/**To**/**By** pour vous approcher de ces effets, mais cela supposerait un travail important et l’animation s’avérerait moins exacte qu’avec une formule mathématique.

Les fonctions d’accélération peuvent être appliquées aux animations de trois manières:

-   En utilisant une image clé d’accélération dans une animation par images clés, comme il est expliqué dans la section précédente. Utilisez [**EasingColorKeyFrame.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR210267), [**EasingDoubleKeyFrame.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.easingdoublekeyframe.easingfunction.aspx) ou [**EasingPointKeyFrame.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR210279).
-   En définissant la propriété **EasingFunction** sur l’un des types d’animation **From**/**To**/**By**. Utilisez [**ColorAnimation.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR243075), [**DoubleAnimation.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.doubleanimation.easingfunction.aspx) ou [**PointAnimation.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR210354).
-   En définissant [**GeneratedEasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR209037) comme faisant partie d’un élément [**VisualTransition**](https://msdn.microsoft.com/library/windows/apps/BR209034). Cela est spécifique à la définition des états visuels pour les contrôles. Pour plus d’informations, voir [**GeneratedEasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR209037) ou [Tables de montage séquentiel pour les états visuels](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808).

Voici la liste des fonctions d’accélération:

-   [**BackEase**](https://msdn.microsoft.com/library/windows/apps/BR243049) : réduit légèrement l’amplitude de l’animation avant qu’elle ne commence sur sa trajectoire prévue.
-   [**BounceEase**](https://msdn.microsoft.com/library/windows/apps/BR243057) : crée un effet de rebond.
-   [**CircleEase**](https://msdn.microsoft.com/library/windows/apps/BR243063) : génère une animation qui accélère ou décélère d’après une fonction circulaire.
-   [**CubicEase**](https://msdn.microsoft.com/library/windows/apps/BR243126) : génère une animation qui accélère ou décélère d’après la formule f(t) = t3.
-   [**ElasticEase**](https://msdn.microsoft.com/library/windows/apps/BR210282) : génère une animation qui évoque un ressort oscillant jusqu’à son arrêt complet.
-   [**ExponentialEase**](https://msdn.microsoft.com/library/windows/apps/BR210294) : génère une animation qui accélère ou décélère d’après une formule exponentielle.
-   [**PowerEase**](https://msdn.microsoft.com/library/windows/apps/BR210399) : génère une animation qui accélère ou décélère d’après la formule f(t) = tp, où p est égal à la propriété [**Power**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.powerease.power).
-   [**QuadraticEase**](https://msdn.microsoft.com/library/windows/apps/BR210403) : génère une animation qui accélère ou décélère d’après la formule f(t) = t2.
-   [**QuarticEase**](https://msdn.microsoft.com/library/windows/apps/BR210405) : génère une animation qui accélère ou décélère d’après la formule f(t) = t4.
-   [**QuinticEase**](https://msdn.microsoft.com/library/windows/apps/BR210407) : génère une animation qui accélère ou décélère d’après la formule f(t) = t5.
-   [**SineEase**](https://msdn.microsoft.com/library/windows/apps/BR210439) : génère une animation qui accélère ou décélère d’après une formule s’appuyant sur une fonction sinus.

Certaines des fonctions d’accélération ont des propriétés qui leur sont propres. Par exemple, [**BounceEase**](https://msdn.microsoft.com/library/windows/apps/BR243057) a deux propriétés [**Bounces**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.bounceease.bounces.aspx) et [**Bounciness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.bounceease.bounciness.aspx) qui modifient le comportement de la fonction dans le temps de cette fonction **BounceEase**. D’autres fonctions d’accélération telles que [**CubicEase**](https://msdn.microsoft.com/library/windows/apps/BR243126) n’ont pas d’autres propriétés que la propriété [**EasingMode**](https://msdn.microsoft.com/library/windows/apps/BR210275) propre à toutes les fonctions d’accélération et produisent toujours le même comportement de fonction dans le temps.

Certaines de ces fonctions d’accélération se chevauchent quelque peu, en fonction de la manière dont vous définissez les propriétés des fonctions d’accélération qui ont des propriétés. Par exemple, [**QuadraticEase**](https://msdn.microsoft.com/library/windows/apps/BR210403) fait exactement la même chose que [**PowerEase**](https://msdn.microsoft.com/library/windows/apps/BR210399) avec la propriété [**Power**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.powerease.power) égale à 2. Et [**CircleEase**](https://msdn.microsoft.com/library/windows/apps/BR243063) est une fonction [**ExponentialEase**](https://msdn.microsoft.com/library/windows/apps/BR210294) avec une valeur par défaut.

La fonction d’accélération [**BackEase**](https://msdn.microsoft.com/library/windows/apps/BR243049) est unique, car elle peut changer la valeur en dehors de la plage normale définie par **From**/**To** ou des valeurs d’images clés. Elle démarre l’animation en changeant la valeur dans la direction opposée comme dans le cas d’un comportement **From**/**To** normal, elle revient à **From** ou à la valeur de départ, puis elle exécute normalement l’animation.

Dans un exemple précédent, nous vous avons montré comment déclarer une fonction d’accélération pour une animation par images clés. L’exemple suivant applique une fonction d’accélération à une animation **From**/**To**/**By**.

```xml
<StackPanel x:Name="LayoutRoot" Background="White">
    <StackPanel.Resources>
        <Storyboard x:Name="myStoryboard">
            <DoubleAnimation From="30" To="200" Duration="00:00:3" 
                Storyboard.TargetName="myRectangle" 
                Storyboard.TargetProperty="(UIElement.RenderTransform).(ScaleTransform.ScaleY)">
                <DoubleAnimation.EasingFunction>
                    <BounceEase Bounces="2" EasingMode="EaseOut" 
                                Bounciness="2"/>
                </DoubleAnimation.EasingFunction>
            </DoubleAnimation>
        </Storyboard>
    </StackPanel.Resources>
    <Rectangle x:Name="myRectangle" Fill="Blue" Width="200" Height="30"/>
</StackPanel>
```

Lorsqu’une fonction d’accélération est appliquée à une animation **From**/**To**/**By**, elle change les caractéristiques de la fonction dans le temps concernant la manière dont la valeur est interpolée entre les valeurs de **From** et de **To** sur l’élément [**Duration**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.duration) de l’animation. Sans fonction d’accélération, l’interpolation serait linéaire.

## <a name="span-iddiscreteobjectvalueanimationsspanspan-iddiscreteobjectvalueanimationsspanspan-iddiscreteobjectvalueanimationsspandiscrete-object-value-animations"></a><span id="Discrete_object_value_animations"></span><span id="discrete_object_value_animations"></span><span id="DISCRETE_OBJECT_VALUE_ANIMATIONS"></span>Animations avec valeurs d’objets discrètes

Un type d’animation mérite une attention particulière, car elle constitue le seul moyen d’appliquer une valeur animée à des propriétés qui ne sont pas de type [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx), [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) ou [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723). Il s’agit de l’animation par images clés [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320). Créer une animation à l’aide de valeurs [**Object**](https://msdn.microsoft.com/library/windows/apps/xaml/system.object.aspx) est différent, car il n’y a aucune possibilité d’interpoler les valeurs entre les images. Lorsque le [**KeyTime**](https://msdn.microsoft.com/library/windows/apps/BR210342) de l’image est atteint, la valeur animée est immédiatement définie avec la valeur spécifiée dans la propriété **Value** de l’image clé. Étant donné qu’aucune interpolation n’est possible, vous ne pouvez utiliser qu’une seule image clé dans la collection d’images clés **ObjectAnimationUsingKeyFrames** : [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR243132).

La [**Value**](https://msdn.microsoft.com/library/windows/apps/BR210344) d’un élément [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR243132) est souvent définie à l’aide de la syntaxe de l’élément de propriété, car la valeur de l’objet que vous essayez de définir n’est souvent pas exprimable en tant que chaîne pour remplir **Value** dans la syntaxe d’attribut. Vous pouvez cependant utiliser la syntaxe d’attribut si vous utilisez une référence telle que [StaticResource](https://msdn.microsoft.com/library/windows/apps/Mt185588).

Vous verrez une image clé [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320) utilisée dans les modèles par défaut lorsqu’une propriété de modèle référence une ressource [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush). Ces ressources sont des objets [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962), et pas simplement une valeur [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723). D’autre part, elles utilisent des ressources qui sont définies comme des thèmes du système ([**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/BR208807)). Elles peuvent être attribuées directement à une valeur de type **Brush** telle que la propriété [**TextBlock.Foreground**](https://msdn.microsoft.com/library/windows/apps/BR209665) et elles n’ont pas besoin de recourir au ciblage indirect. Mais étant donné qu’un élément **SolidColorBrush** n’est pas une propriété [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx), [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) ou **Color**, vous devez utiliser un élément **ObjectAnimationUsingKeyFrames** pour utiliser la ressource.

```xml
<Style x:Key="TextButtonStyle" TargetType="Button">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="Button">
                <Grid Background="Transparent">
                    <TextBlock x:Name="Text"
                        Text="{TemplateBinding Content}"/>
                    <VisualStateManager.VisualStateGroups>
                        <VisualStateGroup x:Name="CommonStates">
                            <VisualState x:Name="Normal"/>
                            <VisualState x:Name="PointerOver">
                                <Storyboard>
                                    <ObjectAnimationUsingKeyFrames Storyboard.TargetName="Text" Storyboard.TargetProperty="Foreground">
                                        <DiscreteObjectKeyFrame KeyTime="0" Value="{StaticResource ApplicationPointerOverForegroundThemeBrush}"/>
                                    </ObjectAnimationUsingKeyFrames>
                                </Storyboard>
                            </VisualState>
                            <VisualState x:Name="Pressed">
                                <Storyboard>
                                    <ObjectAnimationUsingKeyFrames Storyboard.TargetName="Text" Storyboard.TargetProperty="Foreground">
                                        <DiscreteObjectKeyFrame KeyTime="0" Value="{StaticResource ApplicationPressedForegroundThemeBrush}"/>
                                    </ObjectAnimationUsingKeyFrames>
                                </Storyboard>
                            </VisualState>
...
                       </VisualStateGroup>
                    </VisualStateManager.VisualStateGroups>
                </Grid>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

Vous pouvez également utiliser [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320) pour animer des propriétés qui utilisent une valeur d’énumération. Voici un autre exemple de style nommé tiré des modèles Windows Runtime par défaut. Notez la façon dont il définit la propriété [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) qui prend une constante d’énumération [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR209006). Dans ce cas, vous pouvez définir la valeur à l’aide de la syntaxe d’attribut. Vous avez uniquement besoin du nom non qualifié de la constante d’une énumération pour définir une propriété avec une valeur d’énumération, par exemple «Collapsed».

```xml
<Style x:Key="BackButtonStyle" TargetType="Button">
    <Setter Property="Template">
      <Setter.Value>
        <ControlTemplate TargetType="Button">
          <Grid x:Name="RootGrid">
            <VisualStateManager.VisualStateGroups>
              <VisualStateGroup x:Name="CommonStates">
              <VisualState x:Name="Normal"/>
...           <VisualState x:Name="Disabled">
                <Storyboard>
                  <ObjectAnimationUsingKeyFrames Storyboard.TargetName="RootGrid" Storyboard.TargetProperty="Visibility">
                    <DiscreteObjectKeyFrame Value="Collapsed" KeyTime="0"/>
                  </ObjectAnimationUsingKeyFrames>
                </Storyboard>
              </VisualState>
            </VisualStateGroup>
...
          </VisualStateManager.VisualStateGroups>
        </Grid>
      </ControlTemplate>
    </Setter.Value>
  </Setter>
</Style>
```

Vous pouvez utiliser plusieurs éléments [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR243132) pour un ensemble d’images [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320). Il peut être intéressant de créer une animation « diaporama » en animant la valeur de [**Image.Source**](https://msdn.microsoft.com/library/windows/apps/BR242760). Cela peut servir d’exemple de scénario lorsque plusieurs valeurs d’objet peuvent être utiles.

 ## <a name="related-topics"></a>Rubriques connexes

* [Syntaxe de Property-path](https://msdn.microsoft.com/library/windows/apps/Mt185586)
* [Vue d’ensemble des propriétés de dépendance](https://msdn.microsoft.com/library/windows/apps/Mt185583)
* [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490)
* [**Storyboard.TargetProperty**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.targetpropertyproperty)