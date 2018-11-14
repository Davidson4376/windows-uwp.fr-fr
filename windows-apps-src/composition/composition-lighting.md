---
author: daneuber
title: Éclairage de la composition
description: Les API de l’éclairage de Composition peut servir à ajouter l’éclairage dynamique 3D à votre application.
ms.author: jimwalk
ms.date: 07/16/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c5c7bfcb06eb673b0516cef7882685ebd19ddb97
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6184766"
---
# <a name="using-lights-in-windows-ui"></a>L’utilisation des éclairages dans l’interface utilisateur Windows

Les APIs Windows.UI.Composition permettent de créer des effets et animations en temps réel. Éclairage de la composition permet d’éclairage 3D dans les applications 2D. Dans cette vue d’ensemble, nous allons exécuter par le biais de la fonctionnalité de procédure pour configurer les lumières de la composition, à identifier les éléments visuels pour la réception de chaque lumière et utiliser des effets pour définir les matériaux de votre contenu.

> [!NOTE]
> Pour lire comment les objets de [classe XamlLight](/uwp/api/windows.ui.xaml.media.xamllight) s’appliquent [Compositionlight](/uwp/api/Windows.UI.Composition.CompositionLight) pour éclairer les éléments UIElements XAML, voir [l’éclairage XAML](xaml-lighting.md).

Éclairage de la composition vous permet de créer intéressant de l’interface utilisateur en autorisant:

- Transformation d’une lumière, indépendamment des autres objets de la scène pour activer les scénarios IMMERSIFS comme les scènes de la lecture de musique.
- La possibilité d’associer un objet avec une lumière afin qu’ils se déplacent indépendamment du reste de la scène pour activer des scénarios tels que Fluent [révéler](/design/style/reveal.md) la mise en surbrillance.
- Transformation de la lumière et de la scène entière en tant qu’un groupe pour créer des matériaux et profondeur.

Éclairage de la composition prend en charge trois concepts clés: **clair**, les **cibles**et **SceneLightingEffect**.

## <a name="light"></a>Light

[CompositionLight](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlight) vous permet de créer des différentes lumières et les placer dans l’espace de coordonnées. Ces lumières ciblent des éléments visuels que vous souhaitez identifier comme éclairés par la lumière.

### <a name="light-types"></a>Types de lumière

| Type | Description |
| --- | --- |
| [AmbientLight](/uwp/api/windows.ui.composition.ambientlight) | Une source de lumière qui émet de la lumière non directionnelle qui s’affiche réfléchie par tous les éléments de la scène. |
| [DistantLight](/uwp/api/windows.ui.composition.distantlight) | Une grande INFINIMENT source de lumière distante qui émet de la lumière dans un seul sens. Comme le soleil. |
| [PointLight](/uwp/api/windows.ui.composition.pointlight) | Une point de source lumineuse qui émet de la lumière dans toutes les directions. Comme une ampoule. |
| [Point lumineux](/uwp/api/windows.ui.composition.spotlight) | Une source de lumière qui émet des cônes interne et externe de lumière. Comme une lampe torche. |

## <a name="targets"></a>Cibles

Lorsque les lumières ciblent un élément visuel (ajouter à la liste de [cibles](/uwp/api/windows.ui.composition.compositionlight.targets) ), l’élément visuel et tous ses descendants reconnaissent et de répondre à cette source de lumière. Cela peut être aussi simple comme un paramètre d’une source PointLight à la racine d’une arborescence et tous les visuels ci-dessous réagir à l’animation de la direction de lumières point.

**ExclusionsFromTargets** vous donne la possibilité de supprimer l’éclairage d’un élément visuel ou d’une sous-arborescence d’éléments visuels de la même manière que l’ajout des cibles. Enfants dans l’arborescence de la racine par l’élément visuel qui est exclu ne sont pas allumés en conséquence.

### <a name="sample-targets"></a>Exemple (cibles)

Dans l’exemple ci-dessous, nous utilisons un CompositionPointLight pour cibler un contrôle TextBlock XAML.

```cs
    _pointLight = _compositor.CreatePointLight();
    _pointLight.Color = Colors.White;
    _pointLight.CoordinateSpace = text; //set up co-ordinate space for offset
    _pointLight.Targets.Add(text); //target XAML TextBlock
```

En ajoutant une animation pour le décalage de la lumière à points, un effet scintillent est facile.

```cs
_pointLight.Offset = new Vector3(-(float)TextBlock.ActualWidth, (float)TextBlock.ActualHeight / 2, (float)TextBlock.FontSize);
```

Voir l’exemple de [Texte miroiter](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2014393/TextShimmer) complet à la cuisine d’exemple WindowUIDevLabs pour en savoir plus.

## <a name="restrictions"></a>Restrictions

Il existe plusieurs facteurs à prendre en compte lors de la détermination du contenu qui est allumé par CompositionLight.

Concept | Détails
--- | ---
**Lumière ambiante** | Ajout d’une lumière non ambiante à votre scène désactive tous les lumière existante.  Les éléments non ciblés par une lumière non ambiante seront affiche en noirs.  Pour éclairer les éléments visuels environnants ne pas ciblés par la lumière dans un moyen naturel, utilisez une lumière ambiante conjointement avec d’autres lumières.
**Nombre de lumières** | Vous pouvez utiliser n’importe quel deux lumières de composition non ambiante dans n’importe quelle combinaison à cibler votre interface utilisateur. Les éclairages ambiantes ne sont pas restreintes; spot, point et les lumières distantes sont.
**Durée de vie** | CompositionLight peut rencontrer des conditions de durée de vie (exemple: le garbage collector peuvent recycler l’objet de lumière avant d’être utilisé).  Nous vous recommandons de conserver une référence à vos lumières en ajoutant des éclairages en tant que membre pour aider à l’application à gérer la durée de vie.
**Transformations** | Les lumières doivent être placés dans un nœud au-dessus de l’interface utilisateur qui utilise des effets comme les [transformations de perspective](/design/layout/3-d-perspective-effects.md) dans votre structure visuelle à dessiner correctement.
**Cibles et l’espace de coordonnées** | CoordinateSpace est alors utilisée est l’espace visuel dans lequel toutes les propriétés de lumières doivent être définies. CompositionLight.Targets doit être dans l’arborescence CoordinateSpace est alors utilisée.

## <a name="lighting-properties"></a>Propriétés d’éclairage

Selon le type de la lumière utilisé, une lumière peut avoir des propriétés pour l’atténuation et l’espace. Tous les types de lumière n’utilisent pas l’ensemble des propriétés.

Propriété | Description
--- | ---
**Couleur** | La [couleur](/uwp/api/windows.ui.color) de la lumière. Couleur de valeurs sont définies par [D3D](https://docs.microsoft.com/windows/uwp/graphics-concepts/light-properties) diffuses ambiant et spéculaires qui définit la couleur qui est émise d’éclairage. Éclairage utilise des valeurs RVBA pour les lumières; le composant de couleur alpha n’est pas utilisé.
**Direction** | La direction de la lumière. La direction dans laquelle pointe la lumière est spécifiée par rapport à son Visual [CoordinateSpace est alors utilisée](/uwp/api/windows.ui.composition.distantlight.coordinatespace) .
**Espace de coordonnées** | Chaque élément visuel dispose d’un espace de coordonnées 3D implicite. Direction X est de gauche à droite. Direction Y est de haut en bas. Direction Z est un point en dehors du plan. Le point d’origine de cette coordonnée est l’angle supérieur gauche de l’élément visuel, et l’unité est Pixel indépendant de périphérique (DIP). Décalage de la lumière défini dans ces coordonnées.
**Cônes interne et externe** | Les projecteurs émettent un cône de lumière en 2portions: un cône interne lumineux et un cône externe. COMPOSITION permet de que vous contrôlez couleur et des angles du cône interne et externe.
**Offset** | Décalage de la source de lumière par rapport à son espace de coordonnées Visual.

> [!NOTE]
> Lorsque plusieurs lumières atteint le même visuel, ou lorsqu’il est suffisamment large pour dépasser 1.0 valeur de couleur de la lumière, la couleur de la lumière peut changer en raison du déplacement de couleur d’un canal de couleur de lumières.

### <a name="advanced-lighting-properties"></a>Avancée de propriétés d’éclairage

Propriété | Description
--- | ---
**Intensité** | Contrôle de la luminosité de la lumière.
**Atténuation** | Les contrôles d’atténuation définissent la manière dont l’intensité de la lumière décroît jusqu’à la distance maximale spécifiée par la propriété d’étendue.  Constantes, Quadradic et linéaire propriétés d’atténuation peuvent être utilisées.

## <a name="getting-started-with-lighting"></a>Prise en main d’éclairage

Suivez ces étapes générales pour ajouter des lumières:

- Créer et placer les lumières: créer des lumières et les placer dans un espace de coordonnées spécifié.
- Identifier les objets à la lumière: cibler la lumière à effets visuels pertinentes.
- [Facultatif] Définir des objets individuels comment réagir à des lumières: SceneLightingEffect d’utilisation avec une EffectBrush pour personnaliser la réflexion de la lumière pour l’affichage de l’élément SpriteVisual. Valeurs par défaut de réflexion prend en charge l’éclairage des enfants de CoordinateSpace est alors utilisée d’une source lumière.  Un élément visuel peint avec un SceneLightingEffect remplace l’éclairage par défaut pour ce visuel.

## <a name="scenelightingeffect"></a>SceneLightingEffect

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) est utilisé pour modifier l’éclairage par défaut appliquée au contenu d’un [SpriteVisual](/uwp/api/Windows.UI.Composition.SpriteVisual) ciblée par un [CompositionLight](/uwp/api/windows.ui.composition.compositionlight).

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) est fréquemment utilisé pour la création de matériau. Un SceneLightingEffect est un effet utilisé lorsque vous souhaitez obtenir quelque chose de plus complexes, comme l’activation des propriétés réfléchissantes sur une image et/ou en fournissant une illusion de profondeur avec une carte normale. Un SceneLightingEffect offre la possibilité de personnaliser votre interface utilisateur en utilisant les propriétés d’éclairage comme montants diffuses et spéculaires. Vous pouvez personnaliser davantage les effets d’éclairage avec le reste du pipeline effets vous individuellement permettant de fusion et de composer des réactions d’éclairage différentes avec votre contenu.

> [!NOTE]
> Éclairage de scène ne génère pas d’ombres. Il s’agit d’un effet axé sur le rendu 2D.  Il ne prend pas en considération éclairage 3D de scénarios que comprennent les modèles d’éclairage réel, y compris les ombres.


Propriété | Description
--- | ---
**Carte Normal** | NormalMaps créer un effet d’une texture où un normal pointant vers la lumière seront plus brillantes et un normal pointant vers la distance sera plus faible. Pour ajouter un NormalMap à votre utilisation visual ciblée un [CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) à l’aide de LoadedImageSurface pour charger une ressource NormalMap.
**Ambiant** | Propriétés ambiantes sont principalement utilisées pour contrôler la réflexion de couleur globale.
**Spéculaire** | Réflexion spéculaire crée mises en surbrillance sur des objets, en les faisant apparaître brillant. Vous pouvez contrôler le niveau de réflexion spéculaire ainsi que le niveau de briller.  Ces propriétés sont manipulées pour créer des effets de matériau comme shinny métaux ou papier photo.
**Diffuse** | Diffus de réflexion diffuse de la lumière dans toutes les directions.
**Modèle de réflexion** | [Modèle de réflexion](/uwp/api/windows.ui.composition.effects.scenelightingeffectreflectancemodel) vous permet de choisir entre [Blinn Phong](https://docs.microsoft.com/visualstudio/designers/how-to-create-a-basic-phong-shader) et physiquement Phong de Blinn en fonction.  Vous choisissez physiquement basée sur la Phong de Blinn lorsque vous souhaitez avoir condensé des surbrillances spéculaires.

### <a name="sample-scenelightingeffect"></a>Exemple (SceneLightingEffect)

L’exemple ci-dessous montre comment ajouter un mappage normal à un SceneLightingEffect.

```cs
CompositionBrush CreateNormalMapBrush(ICompositionSurface normalMapImage)
{
    var colorSourceEffect = new ColorSourceEffect()
    {
        Color = Colors.White
    };
    var sceneLightingEffect = new SceneLightingEffect()
    {
        NormalMapSource = new CompositionEffectSourceParameter("NormalMap")
    };

    var compositeEffect = new ArithmeticCompositeEffect()
    {
        Source1 = colorSourceEffect,
        Source2 = sceneLightingEffect,
    };

    var factory = _compositor.CreateEffectFactory(sceneLightingEffect);

    var normalMapBrush = _compositor.CreateSurfaceBrush();
    normalMapBrush.Surface = normalMapImage;
    normalMapBrush.Stretch = CompositionStretch.Fill;

    var brush = factory.CreateBrush();
    brush.SetSourceParameter("NormalMap", normalMapBrush);

    return brush;
}
```

## <a name="related-articles"></a>Articles connexes

- [Création des matériaux et les lumières de la couche visuelle](https://blogs.windows.com/buildingapps/2017/08/04/creating-materials-lights-visual-layer/)
- [Vue d’ensemble de l’éclairage](https://docs.microsoft.com/windows/uwp/graphics-concepts/lighting-overview)
- [CompositionCapabilities API](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositioncapabilities)
- [Formules mathématiques d’éclairage](https://docs.microsoft.com/windows/uwp/graphics-concepts/mathematics-of-lighting)
- [SceneLightingEffect](https://docs.microsoft.com/uwp/api/windows.ui.composition.effects.scenelightingeffect)
- [Référentiel de GitHub WindowsUIDevLabs](https://github.com/Microsoft/WindowsUIDevLabs)
