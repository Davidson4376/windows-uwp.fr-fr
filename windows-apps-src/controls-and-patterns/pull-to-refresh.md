---
author: Jwmsft
Description: "Utilisez le modèle Tirer pour actualiser un affichage Liste."
title: Tirer pour actualiser
label: Pull-to-refresh
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: aaeb1e74-b795-4015-bf41-02cb1d6f467e
pm-contact: predavid
design-contact: kimsea
dev-contact: stpete
doc-status: Published
ms.openlocfilehash: 51a8c9a2e4618e054374308918a74cf2095119ef
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/22/2017
---
# <a name="pull-to-refresh"></a>Tirer pour actualiser

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Le modèle Tirer pour actualiser permet à l’utilisateur de dérouler une liste de données à l’aide de la fonction tactile afin de récupérer des données supplémentaires. Tirer pour actualiser est largement utilisé sur les applications mobiles, mais est utile sur n’importe quel appareil doté d’un écran tactile. Vous pouvez gérer des [les événements de manipulation](../input-and-devices/touch-interactions.md#manipulation-events) afin d’implémenter le modèle Tirer pour actualiser dans votre application.

> **API importantes**: [classe ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx), [classe GridView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx)

L’[exemple Tirer pour actualiser](http://go.microsoft.com/fwlink/p/?LinkId=620635) montre comment étendre le contrôle [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) pour prendre en charge ce modèle. Dans cet article, nous utilisons cet exemple pour expliquer les points clés de l’implémentation du modèle Tirer pour actualiser.

![Exemple de Tirer pour actualiser](images/ptr-phone-1.png)

## <a name="is-this-the-right-pattern"></a>Est-ce le modèle approprié?

Utilisez le modèle Tirer pour actualiser lorsque vous avez une liste ou une grille de données que l’utilisateur est susceptible de vouloir actualiser régulièrement et si votre application est susceptible de s’exécuter sur des appareils tactiles mobiles.

## <a name="implement-pull-to-refresh"></a>Implémenter Tirer pour actualiser

Pour implémenter Tirer pour actualiser, vous avez besoin de gérer des événements de manipulation pour détecter lorsqu’un utilisateur a déroulé la liste, fournir un retour visuel et actualiser les données. L’[exemple Tirer pour actualiser](http://go.microsoft.com/fwlink/p/?LinkId=620635) nous montre ici comment procéder. Tout le code n’est pas affiché ici. Vous devez télécharger l’exemple ou afficher le code sur GitHub.

L’exemple Tirer pour actualiser crée un contrôle appelé `RefreshableListView` qui étend le contrôle **ListView**. Ce contrôle ajoute un indicateur d’actualisation permettant de fournir un retour visuel et traite les événements de manipulation sur la visionneuse à défilement interne de l’affichage Liste. Il ajoute également 2événements pour vous avertir du déroulement de la liste et du moment auquel les données doivent être actualisées. RefreshableListView indique uniquement que les données doivent être actualisées. Vous devez gérer l’événement dans votre application pour mettre à jour les données, et ce code sera différent pour chaque application.

RefreshableListView propose un mode d’actualisation automatique qui détermine lorsque l’actualisation est demandée et comment l’indicateur d’actualisation bascule hors de vue. L’actualisation automatique peut être activée ou désactivée.
- Désactivé: une actualisation est demandée uniquement si la liste est relâchée en cas de dépassement du seuil `PullThreshold`. L’indicateur s’anime en dehors de la vue lorsque l’utilisateur relâche le défilement. L’indicateur de barre d’état s’affiche s’il est disponible (sur le téléphone).
- Activé: une actualisation est demandée dès que le seuil `PullThreshold` est dépassé en cas de relâchement ou non. L’indicateur reste en vue jusqu’à ce que les nouvelles données soient récupérées, puis s’anime hors vue. Une méthode **Deferral** est utilisée pour notifier l’application une fois la recherche de données terminée.

> **Remarque**&nbsp;&nbsp;Le code de l’exemple est également applicable à un élément [GridView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx). Pour modifier un contrôle GridView, dérivez la classe personnalisée du contrôle GridView au lieu de ListView et modifiez le modèle GridView par défaut.

## <a name="add-a-refresh-indicator"></a>Ajouter un indicateur d’actualisation

Il est important de fournir un retour visuel pour l’utilisateur afin de l’informer que votre application prend en charge le modèle Tirer pour actualiser. RefreshableListView a une propriété `RefreshIndicatorContent` qui vous permet de définir l’indicateur visuel dans votre code XAML. Il inclut également un indicateur de texte par défaut auquel vous revenez si vous ne définissez pas l’élément `RefreshIndicatorContent`.

Voici les instructions recommandées pour l’indicateur d’actualisation.

![Traits rouges de l’indicateur d’actualisation](images/ptr-redlines-1.png)

**Modifier le modèle d’affichage Liste**

Dans l’exemple Tirer pour actualiser, le modèle de contrôle `RefreshableListView` modifie le modèle **ListView** standard en ajoutant un indicateur d’actualisation. L’indicateur d’actualisation est placé dans une [grille](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.grid.aspx) au-dessus de l’élément [ItemsPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemspresenter.aspx), qui est la partie affichant les éléments de liste.

> **Remarque**&nbsp;&nbsp;La `DefaultRefreshIndicatorContent` zone de texte fournit un indicateur de texte de secours affiché uniquement si la propriété `RefreshIndicatorContent` n’est pas définie.

Voici la partie du modèle de contrôle modifiée à partir du modèle ListView par défaut.

**XAML**
```xaml
<!-- Styles/Styles.xaml -->
<Grid x:Name="ScrollerContent" VerticalAlignment="Top">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    <Border x:Name="RefreshIndicator" VerticalAlignment="Top" Grid.Row="1">
        <Grid>
            <TextBlock x:Name="DefaultRefreshIndicatorContent" HorizontalAlignment="Center" 
                       Foreground="White" FontSize="20" Margin="20, 35, 20, 20"/>
            <ContentPresenter Content="{TemplateBinding RefreshIndicatorContent}"></ContentPresenter>
        </Grid>
    </Border>
    <ItemsPresenter FooterTransitions="{TemplateBinding FooterTransitions}" 
                    FooterTemplate="{TemplateBinding FooterTemplate}" 
                    Footer="{TemplateBinding Footer}" 
                    HeaderTemplate="{TemplateBinding HeaderTemplate}" 
                    Header="{TemplateBinding Header}" 
                    HeaderTransitions="{TemplateBinding HeaderTransitions}" 
                    Padding="{TemplateBinding Padding}"
                    Grid.Row="1"
                    x:Name="ItemsPresenter"/>
</Grid>
```

**Définir le contenu en XAML**

Vous définissez le contenu de l’indicateur d’actualisation dans le code XAML pour votre affichage Liste. Le contenu XAML que vous définissez est affiché par l’élément [ContentPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentpresenter.aspx) (`<ContentPresenter Content="{TemplateBinding RefreshIndicatorContent}">`) de l’indicateur d’actualisation. Si vous ne définissez pas ce type de contenu, l’indicateur de texte par défaut s’affiche à la place.

**XAML**
```xaml
<!-- MainPage.xaml -->
<c:RefreshableListView
    <!-- ... See sample for removed code. -->
    AutoRefresh="{x:Bind Path=UseAutoRefresh, Mode=OneWay}"
    ItemsSource="{x:Bind Items}"
    PullProgressChanged="listView_PullProgressChanged"
    RefreshRequested="listView_RefreshRequested">

    <c:RefreshableListView.RefreshIndicatorContent>
        <Grid Height="100" Background="Transparent">
            <FontIcon
                Margin="0,0,0,30"
                HorizontalAlignment="Center"
                VerticalAlignment="Bottom"
                FontFamily="Segoe MDL2 Assets"
                FontSize="20"
                Glyph="&#xE72C;"
                RenderTransformOrigin="0.5,0.5">
                <FontIcon.RenderTransform>
                    <RotateTransform x:Name="SpinnerTransform" Angle="0" />
                </FontIcon.RenderTransform>
            </FontIcon>
        </Grid>
    </c:RefreshableListView.RefreshIndicatorContent>
    
    <!-- ... See sample for removed code. -->

</c:RefreshableListView>
```

**Animer le compteur**

Lorsque la liste est extraite vers le bas, l’événement de RefreshableListView `PullProgressChanged` survient. Vous gérez cet événement dans votre application pour contrôler l’indicateur d’actualisation. Dans l’exemple, ce plan conceptuel est démarré pour animer l’élément [RotateTransform](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.rotatetransform.aspx) de l’indicateur et faire tourner l’indicateur d’actualisation. 

**XAML**
```xaml
<!-- MainPage.xaml -->
<Storyboard x:Name="SpinnerStoryboard">
    <DoubleAnimation
        Duration="00:00:00.5"
        FillBehavior="HoldEnd"
        From="0"
        RepeatBehavior="Forever"
        Storyboard.TargetName="SpinnerTransform"
        Storyboard.TargetProperty="Angle"
        To="360" />
</Storyboard>
```

## <a name="handle-scroll-viewer-manipulation-events"></a>Gérer les événements de manipulation de la visionneuse à défilement

Le modèle de contrôle d’affichage Liste intègre un élément [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) qui permet à un utilisateur de faire défiler les éléments de liste. Pour implémenter le modèle Tirer pour actualiser, vous devez gérer les événements de manipulation sur la visionneuse à défilement intégrée, ainsi que plusieurs événements connexes. Pour plus d’informations sur les événements de manipulation, voir [Interactions tactiles](../input-and-devices/touch-interactions.md).

**OnApplyTemplate**

Pour obtenir l’accès à la visionneuse à défilement et à d’autres parties du modèle afin de pouvoir ajouter des gestionnaires d’événements et les appeler ultérieurement dans votre code, vous devez remplacer la méthode [OnApplyTemplate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.onapplytemplate.aspx). Dans OnApplyTemplate, vous appelez [GetTemplateChild](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.gettemplatechild.aspx) pour obtenir une référence à une partie nommée dans le modèle de contrôle, que vous pouvez enregistrer de manière à l’utiliser ultérieurement dans votre code.

Dans l’exemple, les variables utilisées pour stocker les parties du modèle sont déclarées dans la région Variables privées. Une fois récupérés dans la méthode OnApplyTemplate, les gestionnaires d’événements sont ajoutés pour les événements [DirectManipulationStarted](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.directmanipulationstarted.aspx), [DirectManipulationCompleted](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.directmanipulationcompleted.aspx), [ViewChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.viewchanged.aspx), et [PointerPressed](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed.aspx)

**DirectManipulationStarted**

Afin de lancer une action Tirer pour actualiser, le contenu doit être déroulé jusqu’en haut de la visionneuse à défilement lorsque l’utilisateur commence à tirer vers le bas. Dans le cas contraire, il est supposé que l’utilisateur effectue cette action pour avoir une vue panoramique de la liste. Le code de ce gestionnaire détermine si la manipulation a commencé alors que le contenu se trouvait en haut de la visionneuse à défilement et peut aboutir à l’actualisation de la liste. Le fait que le contrôle soit «actualisable» est défini en conséquence. 

Si le contrôle peut être actualisé, des gestionnaires d’événements pour les animations sont également ajoutés.

**DirectManipulationCompleted**

Lorsque l’utilisateur cesse de tirer la liste vers le bas, le code de ce gestionnaire vérifie qu’une actualisation a été activée lors de la manipulation. Si une actualisation a été activée, l’événement `RefreshRequested` est déclenché et la commande `RefreshCommand` est exécutée.

Les gestionnaires d’événements pour les animations sont également supprimés.

En fonction de la valeur de la propriété `AutoRefresh`, la liste peut animer l’arrière vers le haut immédiatement ou attendre que l’actualisation soit terminée, puis animer l’arrière vers le haut. Un objet [Deferral](https://msdn.microsoft.com/library/windows/apps/windows.foundation.deferral.aspx) est utilisé pour marquer l’achèvement de l’actualisation. À ce moment, l’interface utilisateur de l’indicateur d’actualisation est masquée.

Cette partie du gestionnaire d’événements DirectManipulationCompleted déclenche l’événement `RefreshRequested` et obtient l’objet Deferral si nécessaire.

**C#**
```csharp
if (this.RefreshRequested != null)
{
    RefreshRequestedEventArgs refreshRequestedEventArgs = new RefreshRequestedEventArgs(
        this.AutoRefresh ? new DeferralCompletedHandler(RefreshCompleted) : null);
    this.RefreshRequested(this, refreshRequestedEventArgs);
    if (this.AutoRefresh)
    {
        m_scrollerContent.ManipulationMode = ManipulationModes.None;
        if (!refreshRequestedEventArgs.WasDeferralRetrieved)
        {
            // The Deferral object was not retrieved in the event handler.
            // Animate the content up right away.
            this.RefreshCompleted();
        }
    }
}
```

**ViewChanged**

Deux cas sont gérés dans le gestionnaire d’événements ViewChanged.

Tout d’abord, si l’affichage est modifié en raison du zoom de la visionneuse à défilement, il est possible d’annuler l’état «actualisable» du contrôle.

Ensuite, si le contenu a fini de s’animer vers le haut à la fin d’une actualisation automatique, les rectangles de remplissage sont masqués, les interactions tactiles avec la visionneuse à défilement sont réactivées et [VerticalOffset](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.verticaloffset.aspx) est défini sur 0.

**PointerPressed**

L’action Tirer pour actualiser se produit uniquement lorsque la liste est tirée vers le bas par une manipulation tactile. Dans le gestionnaire d’événements PointerPressed, le code vérifie quel type de pointeur a entraîné l’événement et définit une variable (`m_pointerPressed`) pour indiquer s’il s’agissait d’un pointeur tactile. Cette variable est utilisée dans le gestionnaire DirectManipulationStarted. S’il ne s’agit pas d’un pointeur tactile, le gestionnaire DirectManipulationStarted revient sans avoir effectué d’action.

## <a name="add-pull-and-refresh-events"></a>Ajouter des événements de tirage et d’actualisation

RefreshableListView ajoute 2événements que vous pouvez gérer dans votre application pour actualiser les données et gérer l’indicateur d’actualisation.

Pour plus d’informations sur les événements, voir [Vue d’ensemble des événements et des événements routés](https://msdn.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview).

**RefreshRequested**

L’événement «RefreshRequested» notifie votre application que l’utilisateur a tiré la liste pour l’actualiser. Vous gérez cet événement afin de rechercher de nouvelles données et mettre à jour votre liste.

Voici le gestionnaire d’événements issu de l’exemple. L’élément important à remarquer est qu’il vérifie la propriété `AutoRefresh` d’affichage de liste et obtient un report si la valeur est **true**. Avec un report, l’indicateur d’actualisation n’est pas arrêté et est masqué jusqu’à ce que l’actualisation soit terminée.

**C#**
```csharp
private async void listView_RefreshRequested(object sender, RefreshableListView.RefreshRequestedEventArgs e)
{
    using (Deferral deferral = listView.AutoRefresh ? e.GetDeferral() : null)
    {
        await FetchAndInsertItemsAsync(_rand.Next(1, 5));

        if (SpinnerStoryboard.GetCurrentState() != Windows.UI.Xaml.Media.Animation.ClockState.Stopped)
        {
            SpinnerStoryboard.Stop();
        }
    }
}
```

**PullProgressChanged**

Dans l’exemple, le contenu de l’indicateur d’actualisation est fourni et contrôlé par l’application. L’événement «PullProgressChanged» notifie votre application lorsque l’utilisateur tire la liste afin de pouvoir démarrer, arrêter et réinitialiser l’indicateur d’actualisation. 

## <a name="composition-animations"></a>Animations de composition

Par défaut, le contenu d’une visionneuse à défilement s’arrête lorsque la barre de défilement atteint le haut. Pour permettre à l’utilisateur de continuer à tirer la liste vers le bas, vous devez accéder à la couche visuelle et animer le contenu de la liste. Pour ce faire, l’exemple utilise des [animations composition](https://msdn.microsoft.com/windows/uwp/composition/composition-animation) et plus précisément des [animations par expressions](https://msdn.microsoft.com/windows/uwp/composition/composition-animation#expression-animations).

Dans l’exemple, ce travail s’effectue principalement dans le gestionnaire d’événements `CompositionTarget_Rendering` et la méthode `UpdateCompositionAnimations`.

## <a name="related-articles"></a>Articles connexes

- [Application de styles aux contrôles](styling-controls.md)
- [Interactions tactiles](../input-and-devices/touch-interactions.md)
- [Affichage Liste et affichage Grille](listview-and-gridview.md)
- [Modèles d’élément d’affichage Liste](listview-item-templates.md)
- [Animations par expressions](https://msdn.microsoft.com/windows/uwp/composition/composition-animation#expression-animations)