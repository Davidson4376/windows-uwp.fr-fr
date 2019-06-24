---
description: Les animations connectées vous permettent de créer une expérience de navigation dynamique et attrayante en animant la transition d’un élément entre deux affichages.
title: Animation connectée
template: detail.hbs
ms.date: 10/04/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f448f481b2b55a42cbaa158cc4b07261e2d7717b
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317137"
---
# <a name="connected-animation-for-uwp-apps"></a>Animation connectée pour les applications UWP

Les animations connectées vous permettent de créer une expérience de navigation dynamique et attrayante en animant la transition d’un élément entre deux affichages. Cela permet à l’utilisateur de conserver son contexte et d'assurer la continuité entre les vues.

Une animation connectée, un élément apparaît « continuer » entre deux vues lors d’une modification dans l’interface utilisateur contenu, battant sur l’écran à partir de son emplacement dans la vue de source vers sa destination dans la nouvelle vue. Cela met l’accent sur le contenu commun entre les vues et crée un effet de belle et dynamique dans le cadre d’une transition.

> **API importantes** :  [Classe de ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation), [ConnectedAnimationService classe](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)


## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si vous avez le <strong style="font-weight: semi-bold">galerie de contrôles XAML</strong> application installée, cliquez ici pour <a href="xamlcontrolsgallery:/item/ConnectedAnimation">ouvrez l’application et voir Animation connectés en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Dans cette vidéo, une application utilise une animation connectée pour animer une image de l’élément comme il « continue » fera partie de l’en-tête de la page suivante. L’effet aide à conserver le contexte de l’utilisateur pendant toute la transition.

![Animation connectée](images/connected-animations/example.gif)

<!-- 
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=b2daa5ee%2Dbe15%2D4503%2Db541%2D1328a6587c36&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

## <a name="video-summary"></a>Résumé de la vidéo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev005/player]

## <a name="connected-animation-and-the-fluent-design-system"></a>Animation connectée et Fluent Design System

 Le système de conception Fluent vous aide à créer une interface utilisateur moderne et audacieuse qui incorpore la lumière, la profondeur, le mouvement, les matières et la notion d'échelle. Animation connectée est un composant de Fluent Design System qui ajoute du mouvement à votre application. Pour plus d’informations, voir [Présentation de Fluent Design pour UWP](/windows/apps/fluent-design-system).

## <a name="why-connected-animation"></a>Pourquoi utiliser l'animation connectée ?

Lorsqu'il navigue entre des pages, il est important que l’utilisateur comprenne le nouveau contenu qui lui est présenté après la navigation et en quoi celui-ci est pertinent avec ses choix de navigation. Les animations connectées offrent une métaphore visuelle puissante qui met l’accent sur la relation entre les deux affichages en attirant l'attention de l’utilisateur sur le contenu commun entre eux. En outre, les animations connectées renforcent l'intérêt visuel et donne un aspect soigné à la navigation entre les pages qui permet de différencier le modèle de mouvement de votre application.

## <a name="when-to-use-connected-animation"></a>Quand utiliser une animation connectée

Les animations connectées sont généralement utilisées lorsque l'utilisateur change de page, bien qu'elles puissent être appliquées à n'importe quelle expérience qui implique une modification du contenu d'une interface utilisateur et qui exige que l'utilisateur préserve le contexte. Envisagez d’utiliser une animation connectée au lieu d’une [transition de navigation d'exploration](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition) chaque fois qu’il existe une image ou un autre élément d’interface utilisateur commun entre des affichages source et de destination.

## <a name="configure-connected-animation"></a>Configurer l’animation connectée

> [!IMPORTANT]
> Cette fonctionnalité requiert cible version de votre application Windows 10, version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou version ultérieure. La propriété de Configuration n’est pas disponible dans les kits SDK antérieures. Vous pouvez cibler une version minimale inférieure à SDK 17763 à l’aide de code adaptatif ou XAML conditionnel. Pour plus d’informations, consultez [Version des applications flexibles](/windows/uwp/debug-test-perf/version-adaptive-apps).

À compter de Windows 10, version 1809, plus les animations connectées génèrent conception Fluent en fournissant l’animation configurations adaptées spécifiquement pour l’avant et vers l’arrière navigation entre les pages.

Vous spécifiez une configuration de l’animation en définissant la propriété de Configuration sur la ConnectedAnimation. (Nous allons montrer des exemples dans la section suivante).

Ce tableau décrit les différentes configurations disponibles. Pour plus d’informations sur les principes du mouvement appliqué dans ces animations, consultez [une direction et la gravité](index.md).

| [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration) |
| - |
| Cela est la configuration par défaut et est recommandée pour la navigation vers l’avant. |
Lorsque l’utilisateur navigue vers l’avant dans l’application (de A à B), l’élément connecté s’affiche physiquement « mener à bien la page ». Ce faisant, l’élément s’affiche pour avancer dans z-espace et dépose un peu comme un effet de gravité tenant. Pour surmonter les effets de la gravité, l’élément gagne la rapidité et d’accélérer dans sa position finale. Le résultat est une animation de « mise à l’échelle et dip ». |

| [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration) |
| - |
| Lorsque l’utilisateur navigue vers l’arrière dans l’application (B à A), l’animation est plus directe. L’élément connecté linéairement se traduit par de B un à l’aide une decelerate cubique Bézier fonction d’accélération. L’intuitif descendante visual renvoie l’utilisateur à leur état précédent aussi rapidement que possible tout en conservant le contexte du flux de navigation. |

| [BasicConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.basicconnectedanimationconfiguration) |
| - |
| Ceci est la valeur par défaut (et uniquement) animation utilisée dans les versions antérieures de Windows 10, version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)). |

### <a name="connectedanimationservice-configuration"></a>Configuration de ConnectedAnimationService

Le [ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice) classe a deux propriétés qui s’appliquent pour les animations individuelles plutôt que l’ensemble du service.

- [DefaultDuration](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaultduration)
- [DefaultEasingFunction](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaulteasingfunction)

Pour obtenir divers effets, certaines configurations ignorent ces propriétés sur ConnectedAnimationService et utilisent leurs propres valeurs à la place, comme décrit dans cette table.

| Configuration | Égards DefaultDuration ? | Égards DefaultEasingFunction ? |
| - | - | - |
| Gravité | Oui | Oui* <br/> **La traduction de base de A à B utilise cette fonction d’accélération, mais « dip de gravité » a sa propre fonction d’accélération.*  |
| Direct | Non <br/> *Anime 150ms plus.*| Non <br/> *Utilise la fonction d’accélération de Decelerate.* |
| De base | Oui | Oui |

## <a name="how-to-implement-connected-animation"></a>Comment implémenter une animation connectée

La configuration d’une animation connectée comprend deux étapes :

1. *Préparer* un objet d’animation dans la page source, ce qui indique au système que l’élément source participera à l’animation connectée.
1. *Démarrer* l’animation dans la page de destination, en passant une référence à l’élément de destination.

Lors de la navigation de la page source, appelez [ConnectedAnimationService.GetForCurrentView](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getforcurrentview) pour obtenir une instance de ConnectedAnimationService. Pour préparer une animation, appelez [PrepareToAnimate](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.preparetoanimate) sur cette instance et transmettre une clé unique et un élément d’interface utilisateur à utiliser lors de la transition. La clé unique vous permet de récupérer l’animation ultérieurement sur la page de destination.

```csharp
ConnectedAnimationService.GetForCurrentView()
    .PrepareToAnimate("forwardAnimation", SourceImage);
```

Lorsque le volet de navigation se produit, démarrez l’animation dans la page de destination. Pour démarrer l’animation, appelez [ConnectedAnimation.TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart). Vous pouvez récupérer l’instance d’animation appropriée en appelant [ConnectedAnimationService.GetAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getanimation) avec la clé unique que vous avez fournie lors de la création de l’animation.

```csharp
ConnectedAnimation animation =
    ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
if (animation != null)
{
    animation.TryStart(DestinationImage);
}
```

### <a name="forward-navigation"></a>Navigation vers l’avant

Cet exemple montre comment utiliser ConnectedAnimationService pour créer une transition pour la navigation vers l’avant entre les deux pages (Page_A à Page_B).

La configuration de l’animation recommandés pour la navigation vers l’avant est [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration). Il s’agit par défaut, vous n’avez pas besoin de définir le [Configuration](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.configuration) propriété, sauf si vous souhaitez spécifier une configuration différente.

Configurez l’animation dans la page source.

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

Démarrer l’animation dans la page de destination.

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

### <a name="back-navigation"></a>Navigation de retour

Pour la navigation de retour (Page_B à Page_A), vous suivez les mêmes étapes, mais les pages source et destination sont inversés.

Lorsque l’utilisateur revient en arrière, ils s’attendent à l’application doit être retourné à l’état précédent dès que possible. Par conséquent, la configuration recommandée est [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration). Cette animation est plus rapide et plus directe et utilise l’accélération decelerate.

Configurez l’animation dans la page source.

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

Démarrer l’animation dans la page de destination.

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

Entre le moment que l’animation est configurée et lorsqu’il est démarré, l’élément source paraît figé au-dessus de toute autre interface utilisateur dans l’application. Cela vous permet d’effectuer toutes les animations de transition simultanément. Pour cette raison, vous ne devriez pas attendre plus de 250 ~ les millisecondes entre les deux étapes car la présence de l’élément source peut devenir perturbante. Si vous préparez une animation et que vous ne la démarrez pas dans les trois secondes, le système la supprimera et tous les appels subséquents vers [TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart) échoueront.

## <a name="connected-animation-in-list-and-grid-experiences"></a>Expériences d'animation connectée dans des listes et des grilles

Souvent, vous devrez créer une animation connectée depuis ou vers un contrôle de liste ou de grille. Vous pouvez utiliser les deux méthodes sur [ListView](/uwp/api/windows.ui.xaml.controls.listview) et [GridView](/uwp/api/windows.ui.xaml.controls.gridview), [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) et [TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync), Pour simplifier ce processus.

Par exemple, supposons que vous avez une **ListView** qui contient un élément nommé « PortraitEllipse » dans son modèle de données.

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

Pour préparer une animation connectée avec les points de suspension correspondant à un élément de liste donné, appelez le [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) méthode avec une clé unique, l’élément et le nom « PortraitEllipse ».

```csharp
void PrepareAnimationWithItem(ContactsItem item)
{
     ContactsListView.PrepareConnectedAnimation("portrait", item, "PortraitEllipse");
}
```

Pour démarrer une animation avec cet élément comme destination, par exemple lorsque naviguer vers l’arrière à partir d’une vue détaillée, utilisez [TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync). Si vous venez de charger la source de données pour le ListView, TryStartConnectedAnimationAsync attend pour démarrer l’animation jusqu'à ce que le conteneur d’élément correspondant a été créé.

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

Un *coordonné animation* est un type spécial d’animation d’entrée où un élément s’affiche, ainsi que la cible d’animation connectée, animer en tandem avec l’élément d’animation connecté comme il se trouve sur l’écran. Les animations coordonnées peuvent renforcer l'effet visuel d'une transition et attirer davantage l’attention de l’utilisateur sur le contexte commun entre les affichages source et destination. Dans ces images, l’interface utilisateur de la légende pour l’élément s'anime à l'aide d’une animation coordonnée.

Lorsqu’une animation coordonnée utilise la configuration de la gravité, gravité est appliqué à l’élément d’animation connectés et les éléments de coordonnée. Les éléments coordonnés seront « coup » en même temps que l’élément connecté afin que les éléments restent véritablement coordonnés.

Utilisez la surcharge de deux paramètres de **TryStart** pour ajouter des éléments coordonnés à une animation connectée. Cet exemple montre une animation coordonnée d’une disposition Grille nommée « DescriptionRoot » qui entre en tandem avec un élément d’animation connecté nommé « CoverImage ».

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
- Utilisez [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration) pour la navigation vers l’avant.
- Utilisez [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration) pour sauvegarder la navigation.
- N’attendez pas sur les demandes du réseau ou autres opérations asynchrones longue entre la préparation et le démarrage d’une animation connectée. Vous devrez peut-être charger au préalable les informations nécessaires pour exécuter la transition à l'avance, ou utiliser une image d'espace réservé basse résolution pendant le chargement d’une image haute résolution dans l’affichage de destination.
- Utilisez [SuppressNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) pour empêcher une animation de transition dans un **Frame** si vous utilisez **ConnectedAnimationService**, depuis les animations connectées ne sont pas destinées à être utilisée simultanément avec les transitions de navigation par défaut. Consultez [NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition) pour en savoir plus sur l’utilisation des transitions de navigation.

## <a name="related-articles"></a>Articles associés

[ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

[NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)
