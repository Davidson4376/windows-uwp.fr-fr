---
title: Tirer pour actualiser à l'aide de modificateurs de source
description: Créer des contrôles personnalisés Tirer pour actualiser à l’aide de SourceModifiers
ms.date: 10/10/2017
ms.topic: article
keywords: Windows10, uwp, animation
ms.localizationpriority: medium
ms.openlocfilehash: 834f631cd5c4b8696e75f83f194b95f809b1cf8a
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8932595"
---
# <a name="pull-to-refresh-with-source-modifiers"></a>Tirer pour actualiser à l'aide de modificateurs de source

Dans cet article, nous expliquons plus en détail comment utiliser la fonctionnalité SourceModifier d’un InteractionTracker et illustrons son utilisation en créant un contrôle personnalisé tirer pour actualiser.

## <a name="prerequisites"></a>Éléments prérequis

À ce stade, nous partons du principe que vous êtes familiarisé avec les concepts abordés dans les articles suivants:

- [Animations pilotées par une entrée](input-driven-animations.md)
- [Expériences personnalisées de manipulation avec InteractionTracker](interaction-tracker-manipulations.md)
- [Animations basées sur une relation](relation-animations.md)

## <a name="what-is-a-sourcemodifier-and-why-are-they-useful"></a>Qu’est-ce qu'un SourceModifier et quelle est son utilité?

Comme les [InertiaModifiers](inertia-modifiers.md), les SourceModifiers vous donnent un contrôle plus précis sur le mouvement d’un InteractionTracker. Mais contrairement aux InertiaModifiers qui définissent le mouvement une fois que l'InteractionTracker est entré en inertie, les SourceModifiers définissent le mouvement alors que l'InteractionTracker est toujours en état d’interaction. Dans ce cas, vous souhaitez une expérience différente de l'interaction classique «du bout des doigts».

Un exemple classique est l’expérience tirer pour actualiser: quand l’utilisateur tire sur la liste pour en actualiser le contenu, que la liste se développe à la vitesse déterminée par le doigt et s’arrête après une certaine distance, ce mouvement paraît brusque et mécanique. Une expérience plus naturelle consisterait à introduire une sensation de résistance lorsque l’utilisateur interagit activement avec la liste. Cette petite nuance rend l’expérience utilisateur globale d’interaction avec une liste plus dynamique et attrayante. Dans la section Exemple, nous examinerons plus en détail la procédure pour créer cette expérience.

Il existe 2 types de modificateurs de source:

- DeltaPosition: est le delta entre la position actuelle de la trame et la position de la trame précédente du doigt pendant l’interaction panoramique tactile. Ce modificateur de source vous permet de modifier la position de delta de l’interaction avant de l’envoyer pour traitement supplémentaire. Il s’agit d’un paramètre de type Vector3 et le développeur peut choisir de modifier les attributs de la position X, Y ou Z e la position avant de la transmettre à l'InteractionTracker.
- DeltaScale: est le delta entre l’échelle de la trame actuelle et l'échelle de la trame précédente qui a été appliquée au cours de l’interaction de zoom tactile. Ce modificateur de source vous permet de modifier le niveau de zoom de l’interaction. Il s’agit d’un attribut de type float que le développeur peut modifier avant de le transmettre à l'InteractionTracker.

Lorsque l'InteractionTracker se trouve en état d'interaction, il évalue chacun des modificateurs de source qui lui sont affectés et détermine si l'un d’eux s’applique. Cela signifie que vous pouvez créer et affecter plusieurs modificateurs de source à un InteractionTracker. Mais, lorsque vous définissez chacun d'eux, vous devez effectuer les opérations suivantes:

1. Définir la condition: une Expression qui définit l’instruction conditionnelle du moment où ce modificateur de source spécifique doit être appliqué.
1. Définir le DeltaPosition/DeltaScale: l’expression du modificateur de source qui modifie le DeltaPosition ou DeltaScale lorsque la condition définie ci-dessus est remplie.

## <a name="example"></a>Exemple

Maintenant, examinons comment vous pouvez utiliser des modificateurs de source pour créer une expérience tirer pour actualiser personnalisée avec un contrôle ListView XAML existant. Nous allons utiliser un canevas comme «panneau d'actualisation» qui sera empilé sur un contrôle ListView XAML pour créer cette expérience.

Pour l'expérience utilisateur final, nous voulons créer l’effet de «résistance» lorsque l’utilisateur effectue un mouvement panoramique actif sur la liste (à l'aide d'une interaction tactile) et arrête ce mouvement à une position qui dépasse un certain point.

![Liste avec tirer pour actualiser](images/animation/city-list.gif)

Vous trouverez le code de travail de cette expérience dans le [référentiel Window UI Dev Labs sur GitHub](https://github.com/Microsoft/WindowsUIDevLabs). Voici la procédure détaillée de conception de cette expérience.
Dans votre code de balisage XAML, vous disposez des éléments suivants:

```xaml
<StackPanel Height="500" MaxHeight="500" x:Name="ContentPanel" HorizontalAlignment="Left" VerticalAlignment="Top" >
 <Canvas Width="400" Height="100" x:Name="RefreshPanel" >
<Image x:Name="FirstGear" Source="ms-appx:///Assets/Loading.png" Width="20" Height="20" Canvas.Left="200" Canvas.Top="70"/>
 </Canvas>
 <ListView x:Name="ThumbnailList"
 MaxWidth="400"
 Height="500"
ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.IsScrollInertiaEnabled="False" ScrollViewer.IsVerticalScrollChainingEnabled="True" >
 <ListView.ItemTemplate>
 ……
 </ListView.ItemTemplate>
 </ListView>
</StackPanel>
```

Étant donné que le contrôle ListView (`ThumbnailList`) est un contrôle XAML qui effectue déjà un défilement, le défilement doit être chaîné à son parent (`ContentPanel`) lorsqu’il atteint l’élément supérieur et ne peut pas défiler davantage. (Vous devez appliquer les modificateurs de source dans le ContentPanel). Pour ce faire, vous devez définir ScrollViewer.IsVerticalScrollChainingEnabled sur la valeur **true** dans le balisage de ListView. Vous devez également définir le mode de chaînage de la VisualInteractionSource sur **Toujours**.

Vous devez définir le gestionnaire de PointerPressedEvent avec le paramètre _handledEventsToo_ sur **true**. Sans cette option, le PointerPressedEvent ne sera pas chaîné au ContentPanel car le contrôle ListView marquera ces événements comme gérés et ils ne seront pas transmis à la chaîne visuelle.

```csharp
//The PointerPressed handler needs to be added using AddHandler method with the //handledEventsToo boolean set to "true"
//instead of the XAML element's "PointerPressed=Window_PointerPressed",
//because the list view needs to chain PointerPressed handled events as well.
ContentPanel.AddHandler(PointerPressedEvent, new PointerEventHandler( Window_PointerPressed), true);
```

Maintenant, vous êtes prêt effectuer la liaison avec l'InteractionTracker. Commencez par configurer InteractionTracker, la VisualInteractionSource et l’Expression qui va tirer profit de la position de l'InteractionTracker.

```csharp
// InteractionTracker and VisualInteractionSource setup.
_root = ElementCompositionPreview.GetElementVisual(Root);
_compositor = _root.Compositor;
_tracker = InteractionTracker.Create(_compositor);
_interactionSource = VisualInteractionSource.Create(_root);
_interactionSource.PositionYSourceMode = InteractionSourceMode.EnabledWithInertia;
_interactionSource.PositionYChainingMode = InteractionChainingMode.Always;
_tracker.InteractionSources.Add(_interactionSource);
float refreshPanelHeight = (float)RefreshPanel.ActualHeight;
_tracker.MaxPosition = new Vector3((float)Root.ActualWidth, 0, 0);
_tracker.MinPosition = new Vector3(-(float)Root.ActualWidth, -refreshPanelHeight, 0);

// Use the Tacker's Position (negated) to apply to the Offset of the Image.
// The -{refreshPanelHeight} is to hide the refresh panel
m_positionExpression = _compositor.CreateExpressionAnimation($"-tracker.Position.Y - {refreshPanelHeight} ");
m_positionExpression.SetReferenceParameter("tracker", _tracker);
_contentPanelVisual.StartAnimation("Offset.Y", m_positionExpression);
```

Avec cette configuration, le panneau d’actualisation se trouve hors de la fenêtre d’affichage dans la position de départ et tout ce que voit l’utilisateur est l'élément ListView. Lorsque le mouvement panoramique atteint le ContentPanel, l’événement PointerPressed est déclenché, dans lequel vous demandez au système d’utiliser InteractionTracker pour piloter l'expérience de manipulation.

```csharp
private void Window_PointerPressed(object sender, PointerRoutedEventArgs e)
{
if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Touch) {
 // Tell the system to use the gestures from this pointer point (if it can).
 _interactionSource.TryRedirectForManipulation(e.GetCurrentPoint(null));
 }
}
```

> [!NOTE]
> Si le chaînage des événements gérés n’est pas nécessaire, vous pouvez ajouter un gestionnaire de PointerPressedEvent directement via le balisage XAML à l’aide de l’attribut (`PointerPressed="Window_PointerPressed"`).

L’étape suivante consiste à configurer les modificateurs de source. Vous utiliserez 2modificateurs de source pour obtenir ce comportement: _Résistance_ et _Arrêter_.

- Résistance: déplacez le DeltaPosition.Y à la moitié de la vitesse jusqu'à ce qu’il atteigne la hauteur du RefreshPanel.

```csharp
CompositionConditionalValue resistanceModifier = CompositionConditionalValue.Create (_compositor);
ExpressionAnimation resistanceCondition = _compositor.CreateExpressionAnimation(
 $"-tracker.Position.Y < {pullToRefreshDistance}");
resistanceCondition.SetReferenceParameter("tracker", _tracker);
ExpressionAnimation resistanceAlternateValue = _compositor.CreateExpressionAnimation(
 "source.DeltaPosition.Y / 3");
resistanceAlternateValue.SetReferenceParameter("source", _interactionSource);
resistanceModifier.Condition = resistanceCondition;
resistanceModifier.Value = resistanceAlternateValue;
```

- Arrêter: arrêtez le mouvement une fois que l’ensemble du RefreshPanel est sur l’écran.

```csharp
CompositionConditionalValue stoppingModifier = CompositionConditionalValue.Create (_compositor);
ExpressionAnimation stoppingCondition = _compositor.CreateExpressionAnimation(
 $"-tracker.Position.Y >= {pullToRefreshDistance}");
stoppingCondition.SetReferenceParameter("tracker", _tracker);
ExpressionAnimation stoppingAlternateValue = _compositor.CreateExpressionAnimation("0");
stoppingModifier.Condition = stoppingCondition;
stoppingModifier.Value = stoppingAlternateValue;
Now add the 2 source modifiers to the InteractionTracker.
List<CompositionConditionalValue> modifierList = new List<CompositionConditionalValue>()
{ resistanceModifier, stoppingModifier };
_interactionSource.ConfigureDeltaPositionYModifiers(modifierList);
```

Ce diagramme permet de visualiser la configuration des SourceModifiers.

![Diagramme du mouvement panoramique](images/animation/source-modifiers-diagram.png)

Désormais, avec les SourceModifiers, vous remarquez que lors qu'un mouvement panoramique tire le ListView vers le bas et atteint l'élément supérieur, le panneau d’actualisation est tiré deux fois moins vite que le mouvement panoramique jusqu'à ce qu’il atteigne la hauteur du RefreshPanel, puis cesse de bouger.

Dans l’exemple complet, une animation par images clés est utilisée pour faire tourner une icône lors de l’interaction dans le canevas RefreshPanel. Vous pouvez utiliser n'importe quel contenu à sa place ou utiliser la position du InteractionTracker pour piloter cette animation séparément.
