---
title: Éclairage de composition
description: L’API d’éclairage de Composition peuvent être utilisées pour ajouter un éclairage 3D dynamique à votre application.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c23de238a0004066b44cfe962e2de72216eb7a6d
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318469"
---
# <a name="using-lights-in-windows-ui"></a>À l’aide des lumières dans l’interface utilisateur de Windows

Les APIs Windows.UI.Composition permettent de créer des effets et des animations en temps réel. Éclairage de composition permet d’éclairage 3D dans les applications 2D. Dans cette vue d’ensemble, nous allons exécuter via les fonctionnalités de la procédure de configuration des lumières de composition, identifier les éléments visuels pour recevoir chaque lumière et utiliser des effets pour définir les matériaux de votre contenu.

> [!NOTE]
> Lire comment [XamlLight](/uwp/api/windows.ui.xaml.media.xamllight) objets appliquent [CompositionLights](/uwp/api/Windows.UI.Composition.CompositionLight) pour éclairer les éléments d’interface utilisateur XAML, consultez [éclairage de XAML](xaml-lighting.md).

Éclairage de composition vous permet de créer intéressantes de l’interface utilisateur en permettant :

- Transformation d’une lumière indépendamment des autres objets dans la scène pour permettre des scénarios immersives comme arrière-plan de la lecture de musique.
- La possibilité d’associer un objet avec une lumière afin qu’ils se déplacent ensemble indépendant du reste de la scène pour activer des scénarios tels que Fluent [révéler](/windows/uwp/design/style/reveal) mettre en surbrillance.
- Transformation de la lumière et la scène entière en tant que groupe pour créer des matériaux et profondeur.

Éclairage de composition prend en charge trois concepts clés : **Lumière**, **cibles**, et **SceneLightingEffect**.

## <a name="light"></a>Light

[CompositionLight](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlight) vous permet de créer différentes lumières et placez-les dans l’espace de coordonnées. Ces lumières ciblent des éléments visuels que vous souhaitez identifier comme éclairés par la lumière.

### <a name="light-types"></a>Types de lumière

| type | Description |
| --- | --- |
| [AmbientLight](/uwp/api/windows.ui.composition.ambientlight) | Une source de lumière qui émet de la lumière non directionnelles qui s’affiche est reflétée par tous les éléments de la scène. |
| [DistantLight](/uwp/api/windows.ui.composition.distantlight) | Une à l’infini grande source de lumière distante qui émet de la lumière dans une seule direction. Comme le soleil. |
| [PointLight](/uwp/api/windows.ui.composition.pointlight) | Une point de source de lumière qui émet de la lumière dans toutes les directions. Comme une ampoule. |
| [SpotLight](/uwp/api/windows.ui.composition.spotlight) | Une source de lumière qui émet cônes intérieurs et extérieurs de la lumière. Comme une torche. |

## <a name="targets"></a>Cibles

Lorsque les lumières ciblent un élément visuel (ajouter à [cibles](/uwp/api/windows.ui.composition.compositionlight.targets) liste), l’élément visuel et tous ses descendants connaissent et répondre à cette source de lumière. Cela peut être aussi simple que d’un paramètre d’une source PointLight à la racine d’arborescence et de tous les éléments visuels ci-dessous réagir à l’animation de la direction de lumières point.

**ExclusionsFromTargets** vous donne la possibilité de supprimer l’éclairage d’un objet visuel ou d’une sous-arborescence d’éléments visuels de la même manière que l’ajout de cibles. Les enfants dans l’arborescence de la racine par l’élément visuel qui est exclu ne sont pas allumés en conséquence.

### <a name="sample-targets"></a>Exemple (cibles)

Dans l’exemple ci-dessous, nous utilisons un CompositionPointLight pour cibler un bloc de texte XAML.

```cs
    _pointLight = _compositor.CreatePointLight();
    _pointLight.Color = Colors.White;
    _pointLight.CoordinateSpace = text; //set up co-ordinate space for offset
    _pointLight.Targets.Add(text); //target XAML TextBlock
```

L’ajout d’une animation au décalage de la lumière du point, un effet scintillent est facilement réaliser.

```cs
_pointLight.Offset = new Vector3(-(float)TextBlock.ActualWidth, (float)TextBlock.ActualHeight / 2, (float)TextBlock.FontSize);
```

Consultez la [texte miroiter](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SampleGallery/Samples/SDK 14393/TextShimmer) exemple indiqué à la cuisine d’exemple WindowUIDevLabs pour en savoir plus.

## <a name="restrictions"></a>Restrictions

Il existe plusieurs facteurs à prendre en compte lors de la détermination de laquelle le contenu est allumée par CompositionLight.

Concept | Détails
--- | ---
**Lumière ambiante** | Ajout d’une lumière non ambiante à votre scène désactiver tous les lumière existante.  Les éléments n’est ciblés par une lumière non ambiante seront affiche en noirs.  Pour éclairer environnantes visuels n’est ciblés par la lumière de façon naturelle, utilisez une lumière ambiante conjointement avec d’autres lumières.
**Nombre de lumières** | Vous pouvez utiliser des deux voyants composition non ambiante dans n’importe quelle combinaison de cibler votre interface utilisateur. Éclairage ambiant n’est pas limitées ; repérer, point et que les lumières distantes sont.
**Lifetime** | CompositionLight peut rencontrer des conditions de durée de vie (exemple : le garbage collector peut recycler l’objet lumière avant d’être utilisée).  Nous vous recommandons de conserver une référence à vos lumières en ajoutant des lumières en tant que membre pour aider à l’application de gérer la durée de vie.
**Transformations** | Lumières doivent être placés dans un nœud situé au-dessus de l’interface utilisateur qui utilise des effets tels que [perspective transformations](/windows/uwp/design/layout/3-d-perspective-effects) dans votre structure visuelle à dessiner correctement.
**Cibles et l’espace de coordonnées** | CoordinateSpace est alors utilisée est l’espace visual dans laquelle toutes les propriétés de lumières doivent être définies. CompositionLight.Targets doit figurer dans l’arborescence CoordinateSpace est alors utilisée.

## <a name="lighting-properties"></a>Propriétés d’éclairage

Selon le type de la lumière utilisée, une lumière peut avoir des propriétés pour l’atténuation et de l’espace. Tous les types de lumière n’utilisent pas l’ensemble des propriétés.

Propriété | Description
--- | ---
**Color** | Le [couleur](/uwp/api/windows.ui.color) de la lumière. Valeurs de couleur d’éclairage sont définies par [D3D](https://docs.microsoft.com/windows/uwp/graphics-concepts/light-properties) diffusion ambiant et spéculaire qui définit la couleur qui est émise. Éclairage utilise différentes valeurs RVBA pour lumières ; le composant de couleur alpha n’est pas utilisé.
**Direction** | La direction de la lumière. La direction dans laquelle la lumière pointe est spécifiée relatif à son [CoordinateSpace est alors utilisée](/uwp/api/windows.ui.composition.distantlight.coordinatespace) Visual.
**Espace de coordonnées** | Chaque visuel a un espace de coordonnées 3D implicit. Axe X est de gauche à droite. Direction Y est de haut en bas. Direction Z est un point hors du plan. Le point d’origine de cette coordonnée est l’angle supérieur gauche de l’élément visuel, et l’unité est le Pixel indépendant de périphérique (DIP). Décalage d’une lumière défini dans ces coordonnées.
**Cônes internes et externes** | Les projecteurs émettent un cône de lumière en 2 portions : un cône interne lumineux et un cône externe. COMPOSITION permet le que contrôle sur les couleurs et les angles du cône interne et externe.
**Offset** | Décalage de la source de lumière par rapport à son espace de coordonnées Visual.

> [!NOTE]
> Lorsque plusieurs lumières atteint le même élément visuel, ou chaque fois que la valeur de couleur d’une lumière obtient suffisamment importante pour dépasser 1.0, la couleur de la lumière peut changer en raison d’un canal de couleur lumières de serrage.

### <a name="advanced-lighting-properties"></a>Avancé des propriétés d’éclairage

Propriété | Description
--- | ---
**Intensité** | Contrôle la luminosité de la lumière.
**Attenuation** | Les contrôles d’atténuation définissent la manière dont l’intensité de la lumière décroît jusqu’à la distance maximale spécifiée par la propriété d’étendue.  Constante, les propriétés d’atténuation Quadradic et linéaires peuvent être utilisées.

## <a name="getting-started-with-lighting"></a>Prise en main d’éclairage

Suivez ces étapes générales pour ajouter des lumières :

- Créer et placer l’éclairage : Créez des lumières et placez-les dans un espace de coordonnées spécifié.
- Identifier les objets à la lumière : Lumière sur les visuels pertinents de la cible.
- [Facultatif] Définir la manière dont les objets réagir aux lumières : Utiliser SceneLightingEffect avec un EffectBrush pour personnaliser la réflexion de la lumière pour afficher le SpriteVisual. Valeurs par défaut de réflexion prend en charge l’éclairage d’enfants CoordinateSpace est alors utilisée d’une source de lumière.  Un élément visuel peint avec un SceneLightingEffect remplace l’éclairage par défaut pour ce visuel.

## <a name="scenelightingeffect"></a>SceneLightingEffect

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) est utilisé pour modifier l’éclairage par défaut appliqué au contenu d’un [SpriteVisual](/uwp/api/Windows.UI.Composition.SpriteVisual) ciblé par une [CompositionLight](/uwp/api/windows.ui.composition.compositionlight).

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) est fréquemment utilisée pour la création de matériel. Un SceneLightingEffect est un effet utilisé lorsque vous souhaitez obtenir quelque chose de plus complexe, comme l’activation des propriétés sur une image de reflet et/ou en fournissant une illusion de profondeur avec une carte de normales. Un SceneLightingEffect offre la possibilité de personnaliser votre interface utilisateur en utilisant les propriétés d’éclairage comme spéculaires et diffuses des quantités. Vous pouvez personnaliser davantage les effets d’éclairage avec le reste du pipeline effets vous permettant de fusionner et composer les réactions d’éclairage différentes avec votre contenu individuellement.

> [!NOTE]
> Éclairage de la scène ne produit pas les ombres ; Il est un effet consacré au rendu 2D.  Il ne prend pas en considération éclairage 3D de scénarios qui incluent les modèles d’éclairage réel, y compris les ombres.


Propriété | Description
--- | ---
**Carte de normales** | NormalMaps créer un effet d’une texture où un normal pointant vers la lumière sera plus clair et un normal pointant vers la suite sera allumée. Pour ajouter un NormalMap à votre utilisation de visual ciblée un [CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) à l’aide de LoadedImageSurface pour charger une ressource NormalMap.
**Ambiante** | Propriétés ambiantes sont principalement utilisées pour contrôler le reflet de couleur globale.
**Spéculaire** | Réflexion spéculaire crée des points importants sur les objets, en les affichant brillant. Vous pouvez contrôler le niveau de la réflexion spéculaire, ainsi que le niveau de brillance.  Ces propriétés sont manipulées pour créer des effets tels que shinny métaux ou papier glacé matériau.
**Diffuse** | Réflexion diffuse diffuse la lumière dans toutes les directions.
**Modèle de réflexion** | [Modèle de réflexion](/uwp/api/windows.ui.composition.effects.scenelightingeffectreflectancemodel) vous permet de choisir entre [Blinn Phong](https://docs.microsoft.com/visualstudio/designers/how-to-create-a-basic-phong-shader) et physiquement Phong de Blinn en fonction.  Vous devez choisir en fonction physiquement les Blinn Phong lorsque vous souhaitez avoir condensé des surbrillances spéculaires.

### <a name="sample-scenelightingeffect"></a>Exemple (SceneLightingEffect)

L’exemple ci-dessous montre comment ajouter une carte de normale à une SceneLightingEffect.

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

## <a name="related-articles"></a>Articles associés

- [Création de documents et des lumières dans la couche visuelle](https://blogs.windows.com/buildingapps/2017/08/04/creating-materials-lights-visual-layer/)
- [Vue d’ensemble de l’éclairage](https://docs.microsoft.com/windows/uwp/graphics-concepts/lighting-overview)
- [CompositionCapabilities API](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositioncapabilities)
- [Mathématiques d’éclairage](https://docs.microsoft.com/windows/uwp/graphics-concepts/mathematics-of-lighting)
- [SceneLightingEffect](https://docs.microsoft.com/uwp/api/windows.ui.composition.effects.scenelightingeffect)
- [Référentiel GitHub de WindowsUIDevLabs](https://github.com/microsoft/WindowsCompositionSamples)
