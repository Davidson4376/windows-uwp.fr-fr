---
ms.assetid: 90F07341-01F4-4205-8161-92DD2EB49860
title: Effets de perspective 3D pour une interface utilisateur en XAML
description: Vous pouvez appliquer des effets 3D au contenu de vos applications Windows Runtime à l’aide de transformations de perspective. Par exemple, vous pouvez donner l’illusion qu’un objet est tourné vers vous ou vers l’arrière, comme illustré ici.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8db56882833b9d3bd8a6d2932d04e07a72b205e2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66365249"
---
# <a name="3-d-perspective-effects-for-xaml-ui"></a>Effets de perspective 3D pour une interface utilisateur en XAML


Vous pouvez appliquer des effets 3D au contenu de vos applications Windows Runtime à l’aide de transformations de perspective. Par exemple, vous pouvez donner l’illusion qu’un objet est tourné vers vous ou vers l’arrière, comme illustré ici.

![Image avec transformation de perspective](images/3dsimple.png)

Une autre utilisation courante des transformations de perspective consiste à organiser les objets les uns par rapport aux autres de manière à créer un effet 3D, comme ci-dessous.

![Empilement d’objets pour créer un effet 3D](images/3dstacking.png)

Outre créer des effets 3D statiques, vous pouvez animer les propriétés des transformations de perspective pour créer des effets 3D mobiles.

Les champs d’application des transformations de perspective ne se limitent pas aux images ; vous pouvez appliquer ces effets à n’importe quel objet [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement), y compris aux contrôles. Par exemple, vous pouvez appliquer un effet 3D à la totalité d’un conteneur de contrôles tel que celui-ci :

![Effet 3D appliqué à un conteneur d’éléments](images/skewedstackpanel.png)

Voici le code XAML utilisé pour créer cet exemple :

```xml
<StackPanel Margin="35" Background="Gray">    
    <StackPanel.Projection>        
        <PlaneProjection RotationX="-35" RotationY="-35" RotationZ="15"  />    
    </StackPanel.Projection>    
    <TextBlock Margin="10">Type Something Below</TextBlock>    
    <TextBox Margin="10"></TextBox>    
    <Button Margin="10" Content="Click" Width="100" />
</StackPanel>
```

Ici, nous mettons l’accent sur les propriétés de l’objet [**PlaneProjection**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PlaneProjection) qui permet de faire pivoter des objets et de les déplacer dans un espace 3D. L’exemple suivant vous permet d’expérimenter ces propriétés et de voir leur impact sur un objet.

## <a name="planeprojection-class"></a>Classe PlaneProjection

Vous pouvez appliquer des effets 3D à n’importe quel objet [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement), en définissant sa propriété [**Projection**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.projection) à l’aide d’un objet [**PlaneProjection**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PlaneProjection). L’objet **PlaneProjection** définit le rendu de la transformation dans l’espace. L’exemple suivant illustre un cas simple.

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationX="-35"   />
    </Image.Projection>
</Image>
```

Cette figure montre le rendu de l’image obtenue. Les axes X, Y et Z sont illustrés par des lignes rouges. L’image est pivotée de 35 degrés vers l’arrière autour de l’axe X à l’aide de la propriété [**RotationX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationx).

![Rotation X moins 35 degrés](images/3drotatexminus35.png)

La propriété [**RotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationy) applique une rotation autour de l’axe Y du centre de rotation.

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationY="-35"   />
    </Image.Projection>
</Image>
```

![Rotation Y moins 35 degrés](images/3drotateyminus35.png)

La propriété [**RotationZ**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationz) applique une rotation autour de l’axe Z du centre de rotation (ligne perpendiculaire au plan de l’objet).

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationZ="-45"/>
    </Image.Projection>
</Image>
```

![Rotation Z moins 45 degrés](images/3drotatezminus35.png)

Les propriétés de rotation peuvent spécifier une valeur positive ou négative afin que la rotation soit effectuée dans l’un ou l’autre sens. La valeur absolue peut être supérieure à 360, ce qui fait pivoter l’objet de plus d’une rotation complète.

Vous pouvez déplacer le centre de rotation à l’aide des propriétés [**CenterOfRotationX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationx), [**CenterOfRotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationy) et [**CenterOfRotationZ**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationz). Par défaut, les axes de rotation passent directement par le centre de l’objet, obligeant celui-ci à pivoter autour de son centre. Toutefois, si vous déplacez le centre de rotation vers le bord externe de l’objet, celui-ci pivotera autour de ce bord. La valeur par défaut des propriétés**CenterOfRotationX** et **CenterOfRotationY** est 0,5, tandis que la valeur par défaut de la propriété **CenterOfRotationZ** est 0. Dans le cas des propriétés **CenterOfRotationX** et **CenterOfRotationY**, les valeurs comprises entre 0 et 1 définissent le point pivot à un emplacement dans l’objet. La valeur 0 désigne un bord de l’objet, tandis que la valeur 1 représente le bord opposé. Les valeurs en dehors de cette plage sont autorisées et entraînent le déplacement du centre de rotation. Étant donné que l’axe Z du centre de rotation passe par le plan de l’objet, vous pouvez déplacer le centre de rotation derrière l’objet, à l’aide d’un nombre négatif, et devant l’objet (vers vous), à l’aide d’un nombre positif.

[**CenterOfRotationX** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationx) déplace le centre de rotation le long de parallèle à l’objet lors de l’axe des x [ **CenterOfRotationY** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationy) déplace le centre ou la rotation le long de l’axe des y l’objet. Les illustrations suivantes montrent l’utilisation de différentes valeurs pour la propriété **CenterOfRotationY**.

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationX="-35" CenterOfRotationY="0.5" />
    </Image.Projection>
</Image>
```

**CenterOfRotationY = "0.5" (default)**

![CenterOfRotationY égal à 0.5](images/3drotatexminus35.png)
```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationX="-35" CenterOfRotationY="0.1"/>
    </Image.Projection>
</Image>
```

**CenterOfRotationY = "0.1"**

![CenterOfRotationY égal à 0.1](images/3dcenterofrotationy0point1.png)

Comme vous pouvez le constater, l’image pivote autour du centre lorsque la propriété [**CenterOfRotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationy) est définie sur la valeur par défaut 0.5, et proche du bord supérieur lorsqu’elle est définie sur 0.1. Vous pouvez observer un comportement similaire si vous modifiez la propriété [**CenterOfRotationX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationx) de manière à opérer un déplacement vers l’endroit où la propriété [**RotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationy) fait pivoter l’objet.

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationY="-35" CenterOfRotationX="0.5" />
    </Image.Projection>
</Image>
```

**CenterOfRotationX = "0.5" (default)**

![CenterOfRotationX égal 0.5](images/3drotateyminus35.png)
```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationY="-35" CenterOfRotationX="0.5" />
    </Image.Projection>
</Image>
```

**CenterOfRotationX = "0.9" (right-hand edge)**

![CenterOfRotationX égal à 0.9](images/3dcenterofrotationx0point9.png)

Utilisez la propriété [**CenterOfRotationZ**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationz) pour placer le centre de rotation au-dessus ou au-dessous du plan de l’objet. Ainsi, vous pouvez faire pivoter l’objet autour du point, à l’image d’une planète en orbite autour d’une étoile.

## <a name="positioning-an-object"></a>Positionnement d’un objet

À ce stade, vous avez appris comment faire pivoter un objet dans l’espace. Vous pouvez positionner ces objets pivotés dans l’espace les uns par rapport aux autres à l’aide des propriétés suivantes :

-   [**LocalOffsetX** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsetx) déplace un objet le long de l’axe des abscisses du plan d’un objet pivoté.
-   [**LocalOffsetY** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsety) déplace un objet le long de l’axe des ordonnées du plan d’un objet pivoté.
-   [**LocalOffsetZ** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsetz) déplace un objet le long de l’axe z du plan d’un objet pivoté.
-   [**GlobalOffsetX** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsetx) déplace un objet le long de l’axe des abscisses aligné sur l’écran.
-   [**GlobalOffsetY** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsety) déplace un objet le long de l’axe des ordonnées aligné sur l’écran.
-   [**GlobalOffsetZ** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsetz) déplace un objet le long de l’axe des z aligné sur l’écran.

**Décalage local**

Les propriétés [**LocalOffsetX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsetx), [**LocalOffsetY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsety) et [**LocalOffsetZ**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsetz) déplacent un objet le long de l’axe respectif du plan de l’objet une fois que celui-ci a subi une rotation. Par conséquent, la rotation de l’objet détermine la direction du déplacement de l’objet. Pour illustrer ce concept, l’exemple suivant anime la propriété **LocalOffsetX** de 0 à 400 degrés et la propriété [**RotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationy) de 0 à 65 degrés.

Dans l’exemple précédent, l’objet se déplace le long de son axe X. Au tout début de l’animation, lorsque la valeur de la propriété [**RotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationy) est proche de zéro (parallèle à l’écran), l’objet se déplace le long de l’écran dans le sens X, mais à mesure que l’objet pivote vers vous, il se déplace le long de l’axe X du plan de l’objet en votre direction. À l’opposé, si vous animez la propriété **RotationY** avec une ampleur de -65 degrés, l’objet dessine une courbe en s’éloignant de vous.

[**LocalOffsetY** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsety) fonctionne de manière similaire à [ **LocalOffsetX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsetx), sauf qu’elle se déplace le long de l’axe vertical, ainsi, la modification [ **RotationX** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationx) affecte la direction **LocalOffsetY** déplace l’objet. Dans l’exemple suivant, la propriété **LocalOffsetY** est animée de 0 à 400 degrés, tandis que la propriété **RotationX** est animée de 0 à 65 degrés.

[**LocalOffsetZ** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsetz) se traduit par l’objet perpendiculaire au plan de l’objet comme si un vecteur a été dessiné directement via le centre derrière l’objet out vers vous. Pour illustrer le fonctionnement de la propriété **LocalOffsetZ**, l’exemple suivant anime la propriété **LocalOffsetZ** entre 0 et 400 degrés et la propriété [**RotationX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationx) entre 0 et 65 degrés.

Au début de l’animation, lorsque la valeur de la propriété [**RotationX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationx) est proche de zéro (parallèle à l’écran), l’objet se déplace directement vers vous, mais à mesure que la face de l’objet pivote vers le bas, celui-ci se déplace vers le bas.

**Décalage global**

Les propriétés [**GlobalOffsetX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsetx), [**GlobalOffsetY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsety) et [**GlobalOffsetZ**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsetz) déplacent l’objet le long d’axes par rapport à l’écran. En d’autres termes, à la différence des propriétés de décalage local, l’axe de déplacement de l’objet est indépendant de la rotation appliquée à celui-ci. Ces propriétés sont utiles lorsque vous souhaitez simplement déplacer l’objet le long de l’axe X, Y ou Z de l’écran sans vous soucier de la rotation appliquée à l’objet.

Dans l’exemple suivant, la propriété [**GlobalOffsetX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsetx) est animée de 0 à 400 degrés, tandis que la propriété [**RotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationy) est animée de 0 à 65 degrés.

Dans cet exemple, l’objet ne change pas de course à mesure qu’il pivote. En effet, l’objet est déplacé le long de l’axe X de l’écran indépendamment de sa rotation.

## <a name="positioning-an-object"></a>Positionnement d’un objet

Vous pouvez utiliser les types [**Matrix3DProjection**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Matrix3DProjection) et [**Matrix3D**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Media3D.Matrix3D) dans le cadre de scénarios 3D partiels plus complexes que permet l’objet [**PlaneProjection**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PlaneProjection). L’objet **Matrix3DProjection** met à votre disposition une matrice de transformation 3D complète que vous pouvez appliquer à n’importe quel objet [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement), ce qui vous permet d’appliquer aux éléments des matrices de perspective et des matrices de transformation de modèle arbitraires. Gardez à l’esprit que ces API sont minimales ; par conséquent, si vous les utilisez, vous devez écrire le code qui crée correctement les matrices de transformation 3D. Il est donc plus facile d’utiliser l’objet **PlaneProjection** pour des scénarios 3D simples.
