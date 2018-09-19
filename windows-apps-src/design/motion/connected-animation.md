---
author: mijacobs
description: Les animations connectées vous permettent de créer une expérience de navigation dynamique et attrayante en animant la transition d’un élément entre deux affichages.
title: Animation connectée
template: detail.hbs
ms.author: jimwalk
ms.date: 10/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: a789a8f082192b79b3e96990827f9a4f6a0eacbc
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/19/2018
ms.locfileid: "4054247"
---
# <a name="connected-animation-for-uwp-apps"></a>Animation connectée pour les applications UWP

## <a name="what-is-connected-animation"></a>Qu'est-ce que l’animation connectée?

Les animations connectées vous permettent de créer une expérience de navigation dynamique et attrayante en animant la transition d’un élément entre deux affichages. Cela permet à l’utilisateur de conserver son contexte et d'assurer la continuité entre les vues.
Dans une animation connectée, un élément «perdure» entre deux affichages lors d'une modification du contenu de l’interface utilisateur, et survole l'écran depuis son emplacement dans l'affichage source à sa destination dans le nouvel affichage. Cela met en évidence le contenu commun des vues et crée un effet dynamique très esthétique dans le cadre d’une transition.

## <a name="see-it-in-action"></a>Regardez-le en pleine action

Dans cette courte vidéo, une application utilise une animation connectée pour animer une image qui «perdure» pour faire partie de l’en-tête de la page suivante. L’effet aide à conserver le contexte de l’utilisateur pendant toute la transition.

![Exemple d’interface utilisateur de mouvement en continu](images/continuous3.gif)

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

## <a name="how-to-implement"></a>Mise en application

La configuration d’une animation connectée comprend deux étapes:

1.  *Préparer* un objet d’animation sur la page source, ce qui indique au système que l’élément source participera à l’animation connectée 
2.  *Démarrer* l’animation sur la page de destination, en transférant une référence vers l’élément de destination

Entre les deux étapes, l’élément source sera gelé au-dessus des autres interfaces utilisateur de l’application, ce qui vous permettra d’effectuer toutes les autres animations de transition simultanément. Par conséquent, vous ne devez pas attendre plus de 250 millisecondes environ entre les deux étapes dans la mesure où la présence de l’élément source peut devenir gênante. Si vous préparez une animation et que vous ne la démarrez pas dans les trois secondes, le système la supprimera et tous les appels subséquents vers **TryStart** échoueront.

Vous pouvez accéder à l’instance actuelle de ConnectedAnimationService en appelant **ConnectedAnimationService.GetForCurrentView**. Pour préparer une animation, appelez **PrepareToAnimate** sur cette instance, en transmettant une référence à une clé unique, en plus de l’élément d’interface utilisateur à utiliser dans la transition. La clé unique vous permet de récupérer l’animation ultérieurement, par exemple sur une autre page.

```csharp
ConnectedAnimationService.GetForCurrentView().PrepareToAnimate("image", SourceImage);
```

Pour démarrer l’animation, appelez **ConnectedAnimation.TryStart**. Vous pouvez récupérer l’instance d’animation appropriée en appelant **ConnectedAnimationService.GetAnimation** avec la clé unique que vous avez fournie lors de la création de l’animation.

```csharp
ConnectedAnimation imageAnimation = 
    ConnectedAnimationService.GetForCurrentView().GetAnimation("image");
if (imageAnimation != null)
{
    imageAnimation.TryStart(DestinationImage);
}
```

Voici un exemple complet de l’utilisation de ConnectedAnimationService pour créer une transition entre deux pages.

*SourcePage.xaml*

```xaml
<Image x:Name="SourceImage"
       Width="200"
       Height="200"
       Stretch="Fill"
       Source="Assets/StoreLogo.png" />
```

*SourcePage.xaml.cs*

```csharp
private void NavigateToDestinationPage()
{
    ConnectedAnimationService.GetForCurrentView().PrepareToAnimate("image", SourceImage);
    Frame.Navigate(typeof(DestinationPage));
}
```

*DestinationPage.xaml*

```xaml
<Image x:Name="DestinationImage"
       Width="400"
       Height="400"
       Stretch="Fill"
       Source="Assets/StoreLogo.png" />
```

*DestinationPage.xaml.cs*

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    ConnectedAnimation imageAnimation = 
        ConnectedAnimationService.GetForCurrentView().GetAnimation("image");
    if (imageAnimation != null)
    {
        imageAnimation.TryStart(DestinationImage);
    }
}
```

## <a name="connected-animation-in-list-and-grid-experiences"></a>Expériences d'animation connectée dans des listes et des grilles

Souvent, vous devrez créer une animation connectée depuis ou vers un contrôle de liste ou de grille. Vous pouvez utiliser deux méthodes de **ListView** et de **GridView**, **PrepareConnectedAnimation** et **TryStartConnectedAnimationAsync**, pour simplifier ce processus.
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

Pour préparer une animation connectée avec l’ellipse correspondant à un élément de liste donné, appelez la méthode **PrepareConnectedAnimation** avec une clé unique, l’élément et le nom «PortraitEllipse».

```csharp
void PrepareAnimationWithItem(ContactsItem item)
{
     ContactsListView.PrepareConnectedAnimation("portrait", item, "PortraitEllipse");
}
```

Vous pouvez également démarrer une animation avec cet élément en tant que destination: par exemple, lorsque vous revenez vers une vue détaillée, utilisez **TryStartConnectedAnimationAsync**. Si vous avez chargé uniquement la source de données pour **ListView**, **TryStartConnectedAnimationAsync** attendra de lancer l’animation jusqu'à ce que le conteneur d’éléments correspondant soit créé.

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

Une *animation coordonnée* est un type spécial d’animation d’entrée, dans lequel un élément s’affiche à côté de la cible de l’animation connectée, et s'anime en tandem avec l’élément d’animation connectée lorsqu’il se déplace sur l’écran. Les animations coordonnées peuvent renforcer l'effet visuel d'une transition et attirer davantage l’attention de l’utilisateur sur le contexte commun entre les affichages source et destination. Dans ces images, l’interface utilisateur de la légende pour l’élément s'anime à l'aide d’une animation coordonnée.

Utilisez la surcharge de deux paramètres de **TryStart** pour ajouter des éléments coordonnés à une animation connectée. Cet exemple montre une animation coordonnée d’une disposition de grille nommée «DescriptionRoot» qui doit s'animer en tandem avec un élément d’animation connectée nommé «CoverImage».

*DestinationPage.xaml*

```xaml
<Grid>
    <Image x:Name="CoverImage" />
    <Grid x:Name="DescriptionRoot" />
</Grid>
```

*DestinationPage.xaml.cs*

```csharp
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
- N’attendez pas les demandes réseau ou d’autres opérations asynchrones longues entre les phases de préparation et de démarrage d’une animation connectée. Vous devrez peut-être charger au préalable les informations nécessaires pour exécuter la transition à l'avance, ou utiliser une image d'espace réservé basse résolution pendant le chargement d’une image haute résolution dans l’affichage de destination.
- Utilisez [SuppressNavigationTransitionInfo](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) afin d’éviter une animation de transition dans une **Trame** si vous utilisez **ConnectedAnimationService**, dans la mesure où les animations connectées ne sont pas destinées à être utilisées simultanément avec les transitions de navigation par défaut. Consultez [NavigationThemeTransition](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.navigationthemetransition.aspx) pour en savoir plus sur l’utilisation des transitions de navigation.


## <a name="download-the-code-samples"></a>Téléchargez des exemples de code

Consultez l'[exemple d'animation connectée](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2014393/ConnectedAnimationSample) dans la galerie d'exemples [WindowsUIDevLabs](https://github.com/Microsoft/WindowsUIDevLabs).

## <a name="related-articles"></a>Articles connexes

- [ConnectedAnimation](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.connectedanimation)
- [ConnectedAnimationService](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.animation.connectedanimation.aspx)
- [NavigationThemeTransition](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.navigationthemetransition.aspx)
