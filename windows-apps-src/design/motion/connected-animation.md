---
description: Les animations connectées vous permettent de créer une expérience de navigation dynamique et attrayante en animant la transition d’un élément entre deux affichages.
title: Animation connectée
template: detail.hbs
ms.date: 10/04/2018
ms.topic: article
keywords: windows10, uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: ce639faac66e93b65a398e6d9cdc700546fc68ab
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8213724"
---
# <a name="connected-animation-for-uwp-apps"></a>Animation connectée pour les applications UWP

Les animations connectées vous permettent de créer une expérience de navigation dynamique et attrayante en animant la transition d’un élément entre deux affichages. Cela permet à l’utilisateur de conserver son contexte et d'assurer la continuité entre les vues.

Dans une animation connectée, un élément s’affiche «perdure» entre deux affichages lors d’une modification du contenu de l’interface utilisateur, survole l’écran à partir de son emplacement dans la vue de source vers sa destination dans la nouvelle vue. Cela met en évidence le contenu commun entre les vues et crée un effet dynamique magnifique dans le cadre d’une transition.

> **API importantes**: [classe ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation), [classe ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

## <a name="see-it-in-action"></a>Regardez-le en pleine action

Dans cette courte vidéo, une application utilise une animation connectée pour animer une image qui «perdure» faire partie de l’en-tête de la page suivante. L’effet aide à conserver le contexte de l’utilisateur pendant toute la transition.

![Animation connectée](images/connected-animations/example.gif)

<!-- 
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=b2daa5ee%2Dbe15%2D4503%2Db541%2D1328a6587c36&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

## <a name="video-summary"></a>Résumé de la vidéo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev005/player]

## <a name="connected-animation-and-the-fluent-design-system"></a>Animation connectée et Fluent Design System

 Fluent Design System vous aide à créer une interface utilisateur moderne et audacieuse qui incorpore la lumière, la profondeur, le mouvement, les matières et la notion d'échelle. Animation connectée est un composant de Fluent Design System qui ajoute du mouvement à votre application. Pour plus d’informations, voir [Présentation de Fluent Design pour UWP](../fluent-design-system/index.md).

## <a name="why-connected-animation"></a>Pourquoi utiliser l'animation connectée?

Lorsqu'il navigue entre des pages, il est important que l’utilisateur comprenne le nouveau contenu qui lui est présenté après la navigation et en quoi celui-ci est pertinent avec ses choix de navigation. Les animations connectées offrent une métaphore visuelle puissante qui met l’accent sur la relation entre les deux affichages en attirant l'attention de l’utilisateur sur le contenu commun entre eux. En outre, les animations connectées renforcent l'intérêt visuel et donne un aspect soigné à la navigation entre les pages qui permet de différencier le modèle de mouvement de votre application.

## <a name="when-to-use-connected-animation"></a>Quand utiliser une animation connectée

Les animations connectées sont généralement utilisées lorsque l'utilisateur change de page, bien qu'elles puissent être appliquées à n'importe quelle expérience qui implique une modification du contenu d'une interface utilisateur et qui exige que l'utilisateur préserve le contexte. Envisagez d’utiliser une animation connectée au lieu d’une [transition de navigation d'exploration](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.navigationthemetransition.aspx) chaque fois qu’il existe une image ou un autre élément d’interface utilisateur commun entre des affichages source et de destination.

## <a name="configure-connected-animation"></a>Configurer l’animation connectée

> [!IMPORTANT]
> Cette fonctionnalité nécessite que version de cible de votre application soit Windows 10, version 1809 ([Kit de développement logiciel 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou une version ultérieure. La propriété de Configuration n’est pas disponible dans le SDK antérieures. Vous pouvez cibler une version minimale inférieure à SDK 17763 à l’aide du code adaptatif ou le XAML conditionnel. Pour plus d’informations, voir les [applications adaptatives de Version](/debug-test-perf/version-adaptive-apps).

À compter de Windows 10, version 1809, les animations connectées davantage génèrent Fluent design en fournissant l’animation des configurations adaptées spécifiquement pour vers l’avant et vers l’arrière navigation entre les pages.

Vous spécifiez une configuration de l’animation en définissant la propriété de Configuration sur la ConnectedAnimation. (Nous allons montrer des exemples dans la section suivante).

Le tableau suivant décrit les configurations disponibles. Pour plus d’informations sur les principes du mouvement appliqué dans ces animations, voir la [direction et gravité](index.md).

| [GravityConnectedAnimationConfiguration]() |
| - |
| Cela est la configuration par défaut et est recommandée pour la navigation vers l’avant. |
Lorsque l’utilisateur navigue vers l’avant dans l’application (de A à B), l’élément connecté s’affiche physiquement» pour mener à bien la page». En procédant ainsi, l’élément s’affiche pour déplacer vers l’avant dans un espace de z et descend un peu comme un effet de la gravité en tirant mise en attente. Pour résoudre les effets de la gravité, l’élément vitesse les gains et accélère dans sa position finale. Le résultat est une animation «mise à l’échelle et dip». |

| [DirectConnectedAnimationConfiguration]() |
| - |
| Lorsque l’utilisateur navigue vers l’arrière dans l’application (B vers A), l’animation est plus directe. L’élément connecté convertit linéaire des B à l’aide une decelerate cubique courbe de Bézier fonction d’accélération. L’affordance visuelle vers l’arrière renvoie l’utilisateur à leur état antérieur aussi vite que possible tout en conservant le contexte du flux de navigation. |

| [BasicConnectedAnimationConfiguration]() |
| - |
| Il s’agit de la valeur par défaut (et uniquement) animation utilisée dans les versions antérieures à Windows 10, version 1809 ([Kit de développement logiciel 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)). |

### <a name="connectedanimationservice-configuration"></a>Configuration de ConnectedAnimationService

La classe [ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice) possède deux propriétés qui s’appliquent aux animations individuelles plutôt que le service global.

- [DefaultDuration](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaultduration)
- [DefaultEasingFunction](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaulteasingfunction)

Pour obtenir les effets différents, certaines configurations ignorent ces propriétés sur ConnectedAnimationService et utilisent leurs propres valeurs à la place, comme décrit dans le tableau suivant.

| Configuration | Égards DefaultDuration? | Égards DefaultEasingFunction? |
| - | - | - |
| Gravité | Oui | Oui* <br/> **La traduction de base de A vers B utilise cette fonction d’accélération, mais le «dip gravité» a son propre fonction d’accélération.*  |
| Direct | Non <br/> *Cet effet anime plus de 150 ms.*| Non <br/> *Utilise la fonction d’accélération de Decelerate.* |
| De base | Oui | Oui |

## <a name="how-to-implement-connected-animation"></a>Comment implémenter une animation connectée

La configuration d’une animation connectée comprend deux étapes:

1. *Préparer* un objet d’animation sur la page source, ce qui indique au système que l’élément source participera à l’animation connectée.
1. *Démarrez* l’animation sur la page de destination, en passant une référence à l’élément de destination.

Lors de la navigation à partir de la page source, appelez [ConnectedAnimationService.GetForCurrentView](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getforcurrentview) pour obtenir une instance de ConnectedAnimationService. Pour préparer une animation, appelez [PrepareToAnimate](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.preparetoanimate) sur cette instance et transmettez une clé unique et l’élément d’interface utilisateur que vous voulez utiliser dans la transition. La clé unique vous permet de récupérer l’animation plus loin sur la page de destination.

```csharp
ConnectedAnimationService.GetForCurrentView()
    .PrepareToAnimate("forwardAnimation", SourceImage);
```

Lorsque la navigation se produit, démarrez l’animation dans la page de destination. Pour démarrer l’animation, appelez [ConnectedAnimation.TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart). Vous pouvez récupérer l’instance d’animation appropriée en appelant [ConnectedAnimationService.GetAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getanimation) avec la clé unique que vous avez fournie lors de la création de l’animation.

```csharp
ConnectedAnimation animation =
    ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
if (animation != null)
{
    animation.TryStart(DestinationImage);
}
```

### <a name="forward-navigation"></a>Navigation vers l’avant

Cet exemple montre l’utilisation de ConnectedAnimationService pour créer une transition pour la navigation vers l’avant entre deux pages (Page_A à Page_B).

La configuration de l’animation recommandée pour la navigation vers l’avant est [GravityConnectedAnimationConfiguration](). Il s’agit de la valeur par défaut, vous n’avez pas besoin de définir la propriété de [Configuration](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.configuration) , sauf si vous souhaitez spécifier une configuration différente.

Configuration de l’animation dans la page source.

```xaml
<!-- Page_A.xaml -->

<Image x:Name="SourceImage"
       HorizontalAlignment="Left" VerticalAlignment="Top"
       Width="200" Height="200"
       Stretch="Fill"
       Source="Assets/StoreLogo.png"
       PointerPressed="SourceImage_PointerPressed"/>
```

```csharp
// Page_A.xaml.cs

private void SourceImage_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    // Navigate to detail page.
    // Suppress the default animation to avoid conflict with the connected animation.
    Frame.Navigate(typeof(Page_B), null, new SuppressNavigationTransitionInfo());
}

protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    ConnectedAnimationService.GetForCurrentView()
        .PrepareToAnimate("forwardAnimation", SourceImage);
    // You don't need to explicitly set the Configuration property because
    // the recommended Gravity configuration is default.
    // For custom animation, use:
    // animation.Configuration = new BasicConnectedAnimationConfiguration();
}
```

Démarrez l’animation dans la page de destination.

```xaml
<!-- Page_B.xaml -->

<Image x:Name="DestinationImage"
       Width="400" Height="400"
       Stretch="Fill"
       Source="Assets/StoreLogo.png" />
```

```csharp
// Page_B.xaml.cs

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    ConnectedAnimation animation =
        ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
    if (animation != null)
    {
        animation.TryStart(DestinationImage);
    }
}
```

### <a name="back-navigation"></a>Navigation vers l’arrière

Pour la navigation vers l’arrière (Page_B à Page_A), vous suivez les mêmes étapes, mais les pages source et destination sont inversés.

Lorsque l’utilisateur navigue vers l’arrière, ils s’attendent à l’application à retourner à l’état précédent dès que possible. Par conséquent, la configuration recommandée est [DirectConnectedAnimationConfiguration](). Cette animation est plus rapide, plus direct et utilise la fonctionnalité de décélération.

Configuration de l’animation dans la page source.

```csharp
// Page_B.xaml.cs

protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    if (e.NavigationMode == NavigationMode.Back)
    {
        ConnectedAnimationService.GetForCurrentView()
            .PrepareToAnimate("backAnimation", DestinationImage);

        // Use the recommended configuration for back animation.
        animation.Configuration = new DirectConnectedAnimationConfiguration();
    }
}
```

Démarrez l’animation dans la page de destination.

```csharp
// Page_A.xaml.cs

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    ConnectedAnimation animation =
        ConnectedAnimationService.GetForCurrentView().GetAnimation("backAnimation");
    if (animation != null)
    {
        animation.TryStart(SourceImage);
    }
}
```

Entre le moment que l’animation est configurée et lorsque son démarrage, l’élément source apparaît gelé au-dessus des autre interfaces utilisateur dans l’application. Cela vous permet d’effectuer toutes les autres animations de transition simultanément. Pour cette raison, vous ne devez pas attendre plus de 250 millisecondes environ entre les deux étapes dans la mesure où la présence de l’élément source peut devenir gênante. Si vous préparez une animation et que vous ne la démarrez pas dans les trois secondes, le système la supprimera et tous les appels subséquents vers [TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart) échoueront.

## <a name="connected-animation-in-list-and-grid-experiences"></a>Expériences d'animation connectée dans des listes et des grilles

Souvent, vous devrez créer une animation connectée depuis ou vers un contrôle de liste ou de grille. Vous pouvez utiliser les deux méthodes sur [ListView](/uwp/api/windows.ui.xaml.controls.listview) et [GridView](/uwp/api/windows.ui.xaml.controls.gridview), [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) et [TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync), pour simplifier ce processus.

Par exemple, supposons que vous avez une **ListView** qui contient un élément nommé «PortraitEllipse» dans son modèle de données.

```xaml
<ListView x:Name="ContactsListView" Loaded="ContactsListView_Loaded">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="vm:ContactsItem">
            <Grid>
                …
                <Ellipse x:Name="PortraitEllipse" … />
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Pour préparer une animation connectée avec l’ellipse correspondant à un élément de liste donné, appelez la méthode [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) avec une clé unique, l’élément et le nom «PortraitEllipse».

```csharp
void PrepareAnimationWithItem(ContactsItem item)
{
     ContactsListView.PrepareConnectedAnimation("portrait", item, "PortraitEllipse");
}
```

Pour démarrer une animation avec cet élément comme destination, par exemple, lorsque naviguer vers l’arrière vers une vue détaillée, utilisez [TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync). Si vous avez chargé uniquement la source de données pour ListView, TryStartConnectedAnimationAsync attendra de lancer l’animation jusqu'à ce que le conteneur d’éléments correspondant soit créé.

```csharp
private void ContactsListView_Loaded(object sender, RoutedEventArgs e)
{
    ContactsItem item = GetPersistedItem(); // Get persisted item
    if (item != null)
    {
        ContactsListView.ScrollIntoView(item);
        ConnectedAnimation animation =
            ConnectedAnimationService.GetForCurrentView().GetAnimation("portrait");
        if (animation != null)
        {
            await ContactsListView.TryStartConnectedAnimationAsync(
                animation, item, "PortraitEllipse");
        }
    }
}
```

## <a name="coordinated-animation"></a>Animation coordonnée

![Animation coordonnée](images/connected-animations/coordinated_example.gif)

<!--
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=9066bbbe%2Dcf58%2D4ab4%2Db274%2D595616f5d0a0&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

Une *animation coordonnée* est un type spécial d’animation d’entrée où un élément s’affiche, ainsi que la cible de l’animation connectée, anime en tandem avec l’élément d’animation connectée lorsqu’il se déplace sur l’écran. Les animations coordonnées peuvent renforcer l'effet visuel d'une transition et attirer davantage l’attention de l’utilisateur sur le contexte commun entre les affichages source et destination. Dans ces images, l’interface utilisateur de la légende pour l’élément s'anime à l'aide d’une animation coordonnée.

Lorsqu’une animation coordonnée utilise la configuration de la gravité, la gravité est appliquée à l’élément d’animation connectée et les éléments coordonnés. Les éléments coordonnés seront «coup» en même temps que l’élément connecté. ainsi, les éléments ne véritablement coordonnés.

Utilisez la surcharge de deux paramètres de **TryStart** pour ajouter des éléments coordonnés à une animation connectée. Cet exemple montre une animation coordonnée d’une disposition de grille nommée «Descriptionroot» doit animer qui passe en tandem avec un élément d’animation connectée nommé «CoverImage».

```xaml
<!-- DestinationPage.xaml -->
<Grid>
    <Image x:Name="CoverImage" />
    <Grid x:Name="DescriptionRoot" />
</Grid>
```

```csharp
// DestinationPage.xaml.cs
void OnNavigatedTo(NavigationEventArgs e)
{
    var animationService = ConnectedAnimationService.GetForCurrentView();
    var animation = animationService.GetAnimation("coverImage");

    if (animation != null)
    {
        // Don’t need to capture the return value as we are not scheduling any subsequent
        // animations
        animation.TryStart(CoverImage, new UIElement[] { DescriptionRoot });
     }
}
```

## <a name="dos-and-donts"></a>À faire et à ne pas faire

- Utilisez une animation connectée dans les transitions de page où un élément est commun aux pages source et destination.
- Utiliser [GravityConnectedAnimationConfiguration]() pour la navigation vers l’avant.
- Utilisez [DirectConnectedAnimationConfiguration]() pour la navigation vers l’arrière.
- N’attendez pas les demandes réseau ou d’autres opérations asynchrones longues entre ces préparation et de démarrage d’une animation connectée. Vous devrez peut-être charger au préalable les informations nécessaires pour exécuter la transition à l'avance, ou utiliser une image d'espace réservé basse résolution pendant le chargement d’une image haute résolution dans l’affichage de destination.
- Utilisez [SuppressNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) pour éviter une animation de transition dans une **image** si vous utilisez **ConnectedAnimationService**, dans la mesure où les animations connectées ne sont pas destinées à être utilisées simultanément avec la navigation par défaut transitions. Consultez [NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition) pour en savoir plus sur l’utilisation des transitions de navigation.

## <a name="download-the-code-samples"></a>Téléchargez des exemples de code

Consultez l'[exemple d'animation connectée](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2014393/ConnectedAnimationSample) dans la galerie d'exemples [WindowsUIDevLabs](https://github.com/Microsoft/WindowsUIDevLabs).

## <a name="related-articles"></a>Articles connexes

[ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

[NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)
