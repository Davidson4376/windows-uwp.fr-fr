---
ms.assetid: 03dd256f-78c0-e1b1-3d9f-7b3afab29b2f
Pinceaux de composition
Un pinceau peint la zone d’un Visual avec sa sortie. Des pinceaux différents ont différents types de sortie.
---
# Pinceaux de composition

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Un pinceau peint la zone d’un [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) avec sa sortie. Des pinceaux différents ont différents types de sortie. L’API Composition fournit trois types de pinceau :

-   [
            **CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399) peint un élément visuel avec une couleur unie.
-   [
            **CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) peint un élément visuel avec le contenu d’une surface de composition.
-   [
            **CompositionEffectBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589406) peint un élément visuel avec le contenu d’un effet de composition.

Tous les pinceaux héritent de [**CompositionBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589398) ; ils sont créés directement ou indirectement par le [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) et constituent des ressources indépendantes du périphérique. Même si les pinceaux sont indépendants du périphérique, [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) et [**CompositionEffectBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589406) peignent un [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) en utilisant le contenu d’une surface de composition dépendant du périphérique.

-   [Prérequis](./composition-brushes.md#prerequisites)
-   [Notions de base des couleurs](./composition-brushes.md#color-basics)
    -   [Modes alpha](./composition-brushes.md#alpha-modes)
-   [Utilisation d’un pinceau couleur](./composition-brushes.md#using-color-brush)
-   [Utilisation d’un pinceau de surface](./composition-brushes.md#using-surface-brush)
-   [Configuration de l’étirement et de l’alignement](./composition-brushes.md#configuring-stretch-and-alignment)

## Prérequis

Cette vue d’ensemble suppose que vous êtes familiarisé avec la structure d’une application Composition de base, comme décrit dans [Interface utilisateur de composition](visual-layer.md).

## Notions de base des couleurs

Avant de peindre avec un [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399), vous devez choisir les couleurs. L’API Composition utilise la structure Windows Runtime, Color, pour représenter une couleur. La structure Color utilise le codage sRVB. Ce codage répartit les couleurs en quatre canaux : alpha, rouge, vert et bleu. Chaque composant est représenté par une valeur de virgule flottante comprise dans une plage standard de 0 à 1. La valeur 0 indique l’absence complète de cette couleur, tandis que la valeur 1 indique que cette couleur est entièrement présente. Pour le composant alpha, 0 représente une couleur totalement transparente, et 1 une couleur totalement opaque.

### Modes alpha

Les valeurs de couleur de [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399) sont toujours interprétées comme un paramètre alpha direct.

## Utilisation d’un pinceau couleur

Pour créer un pinceau couleur, appelez la méthode Compositor.[**CreateColorBrush**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositor.createcolorbrush.aspx), qui retourne un [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399). La couleur par défaut de **CompositionColorBrush** est \#00000000. L’illustration et le code suivants montrent une petite arborescence d’éléments visuels, et créent un rectangle tracé avec un pinceau de couleur noire et peint à l’aide d’un pinceau de couleur unie, dont la valeur de couleur est 0x9ACD32.

![CompositionColorBrush](images/composition-compositioncolorbrush.png)
```cs
Compositor _compositor;
ContainerVisual _container;
SpriteVisual visual1, visual2;
CompositionColorBrush _blackBrush, _greenBrush; 

_compositor = new Compositor();
_container = _compositor.CreateContainerVisual();

_blackBrush = _compositor.CreateColorBrush(Colors.Black);
visual1 = _compositor.CreateSpriteVisual();
visual1.Brush = _blackBrush;
visual1.Size = new Vector2(156, 156);
visual1.Offset = new Vector3(0, 0, 0);

_ greenBrush = _compositor.CreateColorBrush(Color.FromArgb(0xff, 0x9A, 0xCD, 0x32));
Visual2 = _compositor.CreateSpriteVisual();
Visual2.Brush = _greenBrush;
Visual2.Size = new Vector2(150, 150);
Visual2.Offset = new Vector3(3, 3, 0);
```

Contrairement aux autres pinceaux, la création d’un [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399) est une opération relativement peu coûteuse. Vous pouvez créer des objets **CompositionColorBrush** à chaque fois que vous effectuez un rendu avec peu ou pas d’impact sur les performances.

## Utilisation d’un pinceau de surface

Un [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) peint un élément visuel avec une surface de composition (représentée par un objet [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Dn706819)). L’illustration suivante présente un carré visuel peint avec une image bitmap de réglisse affichée sur une **ICompositionSurface** en D2D.

![CompositionSurfaceBrush](images/composition-compositionsurfacebrush.png)
Le premier exemple initialise une surface de composition à utiliser avec le pinceau. La surface de composition est créée à l’aide d’une méthode d’assistance, LoadImage qui prend un [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) et une URL en tant que chaîne. Elle charge l’image à partir de l’URL, effectue le rendu de l’image dans une [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Dn706819), et définit la surface en tant que contenu du **CompositionSurfaceBrush**. Remarque : comme **ICompositionSurface** est exposée dans le code natif uniquement, la méthode LoadImage est implémentée également dans le code natif.

```cs
LoadImage(Brush,
          "ms-appx:///Assets/liqorice.png");
```

Pour créer le pinceau de surface, appelez la méthode Compositor.[**CreateSurfaceBrush**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositor.createsurfacebrush.aspx). Cette méthode renvoie un objet [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415). Le code ci-dessous illustre le code qui peut être utilisé pour peindre un élément visuel avec le contenu d’un **CompositionSurfaceBrush**.

```cs
Compositor _compositor;
ContainerVisual _container;
SpriteVisual visual;
CompositionSurfaceBrush _surfaceBrush;

_surfaceBrush = _compositor.CreateSurfaceBrush();
LoadImage(_surfaceBrush, "ms-appx:///Assets/liqorice.png");
visual.Brush = _surfaceBrush;
```

## Configuration de l’étirement et de l’alignement

Il arrive que le contenu de la [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Dn706819) d’un [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) ne remplisse pas complètement les zones de l’élément visuel qui est peint. Le cas échéant, l’API Composition utilise les paramètres de mode [**HorizontalAlignmentRatio**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionsurfacebrush.horizontalalignmentratio.aspx), [**VerticalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.verticalalignmentratio) et [**Stretch**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.stretch) du pinceau pour déterminer comment remplir l’espace restant.

-   [
            **HorizontalAlignmentRatio**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionsurfacebrush.horizontalalignmentratio.aspx) et [**VerticalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.verticalalignmentratio) sont de type flottant, et peuvent être utilisés pour contrôler le positionnement du pinceau dans les limites de l’élément visuel.
    -   La valeur 0,0 aligne le coin gauche/supérieur du pinceau sur le coin gauche/supérieur de l’élément visuel.
    -   La valeur 0,5 aligne le centre du pinceau sur le centre de l’élément visuel.
    -   La valeur 1 aligne le coin droit/inférieur du pinceau sur le coin droit/inférieur de l’élément visuel.
-   La propriété [**Stretch**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.stretch) accepte ces valeurs qui sont définies par l’énumération [**CompositionStretch**](https://msdn.microsoft.com/library/windows/apps/Dn706786) :
    -   None : le pinceau n’est pas étiré pour remplir les limites de l’élément visuel. Utilisez ce paramètre Stretch avec précaution : si le pinceau est plus large que les limites de l’élément visuel, le contenu du pinceau est tronqué. La partie du pinceau permettant de peindre les limites de l’élément visuel peut être contrôlée à l’aide des propriétés [**HorizontalAlignmentRatio**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionsurfacebrush.horizontalalignmentratio.aspx) et [**VerticalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.verticalalignmentratio).
    -   Uniform : le pinceau est mis à l’échelle pour s’adapter aux limites de l’élément visuel ; les proportions du pinceau sont conservées. Il s’agit de la valeur par défaut.
    -   UniformToFill : le pinceau est mis à l’échelle afin de remplir complètement les limites de l’élément visuel ; les proportions du pinceau sont conservées.
    -   Fill : le pinceau est mis à l’échelle pour s’adapter aux limites de l’élément visuel. Étant donné que la hauteur et la largeur du pinceau sont mises à l’échelle indépendamment, les proportions d’origine du pinceau risquent de ne pas être préservées. C’est-à-dire que le pinceau risque d’être déformé afin de remplir complètement les limites de l’élément visuel.

 

 






<!--HONumber=Mar16_HO1-->


