---
Description: Le contrôle Pivot permet d’utiliser le balayage tactile dans un petit ensemble de sections de contenu.
title: Pivot
template: detail.hbs
ms.date: 06/19/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e975c47ae783fe9984950cf30cc82844b344aa7c
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319273"
---
# <a name="pivot"></a>Pivot

Le contrôle [Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) permet d’utiliser le balayage tactile dans un petit ensemble de sections de contenu.

> **API importantes** : [Classe Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot), [Classe NavigationView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.NavigationView)

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/Pivot">ouvrir l’application et voir comment fonctionne le contrôle Pivot</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Le contrôle Pivot, tout comme [NavigationView](navigationview.md), souligne l’élément sélectionné.

![Le focus par défaut souligne l’en-tête sélectionné](images/pivot_focus_selectedHeader.png)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Pour créer des modèles de navigation supérieure et d’onglets communs, nous recommandons d’utiliser [NavigationView](navigationview.md), qui s’adapte automatiquement aux différentes tailles d’écran et permet une meilleure personnalisation.

Toutefois, si votre navigation nécessite un balayage tactile, nous vous recommandons d’utiliser le contrôle Pivot.

Les autres différences importantes qui existent entre le contrôle NavigationView et le contrôle Pivot se situent au niveau du comportement de dépassement par défaut et au niveau de l’API de navigation :

- Les carrousels Pivot permettent un dépassement des éléments, alors que NavigationView utilise un dépassement par menu déroulant pour que les utilisateurs puissent voir tous les éléments.
- Pivot gère la navigation entre les différentes sections de contenu, tandis que NavigationView permet de mieux contrôler le comportement de navigation.

## <a name="use-navigationview-instead-of-pivot"></a>Utiliser NavigationView au lieu de Pivot

Si l’interface utilisateur de votre application utilise le contrôle Pivot, vous pouvez le convertir en contrôle NavigationView à l’aide du code ci-dessous.

Ce code XAML crée un contrôle NavigationView avec trois sections de contenu, comme dans l’exemple de Pivot de [Créer un contrôle Pivot](#create-a-pivot-control).

```xaml
<NavigationView x:Name="rootNavigationView" Header="Category Title"
 ItemInvoked="NavView_ItemInvoked">
    <NavigationView.MenuItems>
        <NavigationViewItem Content="Section 1" x:Name="Section1Content" />
        <NavigationViewItem Content="Section 2" x:Name="Section2Content" />
        <NavigationViewItem Content="Section 3" x:Name="Section3Content" />
    </NavigationView.MenuItems>
</NavigationView>

<Page x:Class="AppName.Section1Page">
    <TextBlock Text="Content of section 1."/>
</Page>

<Page x:Class="AppName.Section2Page">
    <TextBlock Text="Content of section 2."/>
</Page>

<Page x:Class="AppName.Section3Page">
    <TextBlock Text="Content of section 3."/>
</Page>
```

NavigationView permet de contrôler davantage la personnalisation de la navigation et nécessite pour cela du code-behind adapté. Pour accompagner le code XAML ci-dessus, utilisez le code-behind suivant :

```csharp
private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{
    FrameNavigationOptions navOptions = new FrameNavigationOptions();
    navOptions.TransitionInfoOverride = new SlideNavigationTransitionInfo() {
         SlideNavigationTransitionDirection=args.RecommendedNavigationTransitionInfo
    };

    navOptions.IsNavigationStackEnabled = False;

    switch (item.Name)
    {
        case "Section1Content":
            ContentFrame.NavigateToType(typeof(Section1Page), null, navOptions);
            break;

        case "Section2Content":
            ContentFrame.NavigateToType(typeof(Section2Page), null, navOptions);
            break;

        case "Section3Content":
            ContentFrame.NavigateToType(typeof(Section3Page), null, navOptions);
            break;
    }  
}
```

Ce code reproduit l’expérience de navigation intégrée du contrôle Pivot, mais n’inclut pas la fonctionnalité de balayage tactile entre les sections de contenu. Toutefois, vous pouvez aussi personnaliser plusieurs éléments, y compris la transition animée, les paramètres de navigation et les fonctionnalités de pile.

## <a name="create-a-pivot-control"></a>Créer un contrôle Pivot

Ce code crée un contrôle Pivot de base avec trois sections de contenu.

```xaml
<Pivot x:Name="rootPivot" Title="Category Title">
    <PivotItem Header="Section 1">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 1."/>
    </PivotItem>
    <PivotItem Header="Section 2">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 2."/>
    </PivotItem>
    <PivotItem Header="Section 3">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 3."/>
    </PivotItem>
</Pivot>
```

### <a name="pivot-items"></a>Éléments du contrôle Pivot

Le contrôle Pivot est un [ItemsControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl). Il peut donc contenir une collection d’éléments de n’importe quel type. Tout élément que vous ajoutez au contrôle Pivot qui n’est pas explicitement un élément [PivotItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PivotItem) est implicitement encapsulé dans un PivotItem. Un sélecteur de vue est souvent utilisé pour naviguer entre des pages de contenu. Il est donc courant de remplir la collection [Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) directement avec des éléments d’interface utilisateur XAML. Vous pouvez également affecter à la propriété [ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) une source de données. Les éléments liés dans la propriété ItemsSource peuvent être de n’importe quel type. Cependant, s’il ne s’agit pas explicitement d’éléments PivotItem, vous devez définir un [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) et un [HeaderTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.headertemplate) pour spécifier leur mode d’affichage.

Vous pouvez utiliser la propriété [SelectedItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.selecteditem) pour obtenir ou définir l’élément actif du sélecteur de vue. Utilisez la propriété [SelectedItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.selectedindex) pour obtenir ou définir l’index de l’élément actif.

### <a name="pivot-headers"></a>En-têtes de sélecteur de vue

Vous pouvez utiliser les propriétés [LeftHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.leftheader) et [RightHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.rightheader) pour ajouter d’autres contrôles à l’en-tête du sélecteur de vue.

Par exemple, vous pouvez ajouter une [CommandBar](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/app-bars) dans le RightHeader du contrôle Pivot.

```xaml
<Pivot>
    <Pivot.RightHeader>
        <CommandBar>
                <AppBarButton Icon="Add"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Edit"/>
                <AppBarButton Icon="Delete"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Save"/>
        </CommandBar>
    </Pivot.RightHeader>
</Pivot>
```

### <a name="pivot-interaction"></a>Interaction avec le sélecteur de vue

Le contrôle offre les interactions d’entrée tactile suivantes :

- Un appui sur un en-tête d’élément sélecteur de vue permet d’accéder au contenu de la section de cet en-tête.
- Un mouvement de balayage sur un en-tête d’élément sélecteur de vue vers la gauche ou la droite permet d’accéder à la section adjacente.
- Un mouvement de balayage sur une section vers la gauche ou la droite permet d’accéder à la section adjacente.

Le contrôle est disponible en deux modes :

**Stationnaire**

- Les sélecteurs de vue sont stationnaires lorsque l’espace autorisé est suffisant pour contenir tous les en-têtes de sélecteur de vue.
- Un appui sur une étiquette de sélecteur de vue permet d’accéder à la page correspondante, bien que le sélecteur de vue proprement dit ne bouge pas. Le sélecteur de vue actif est mis en surbrillance.

**Carrousel**

- Les sélecteurs de vue sont présentés sous forme de carrousel lorsque l’espace autorisé n’est pas suffisant pour contenir tous les en-têtes de sélecteur de vue.
- Un appui sur une étiquette de sélecteur de vue permet d’accéder à la page correspondante. L’étiquette du sélecteur de vue actif passe en première position par rotation.
- Placez les éléments sélecteur de vue dans une boucle carrousel de la dernière à la première section de sélecteur de vue.

> **Remarque** Dans un [environnement de type « 10-foot »](../devices/designing-for-tv.md), les en-têtes Pivot ne doivent pas s’afficher dans une vue Carrousel. Définissez la propriété [IsHeaderItemsCarouselEnabled](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot.IsHeaderItemsCarouselEnabled) sur **false** si votre application doit s’exécuter sur Xbox.

## <a name="recommendations"></a>Recommandations

- Évitez d’utiliser plus de 5 en-têtes en mode carrousel (rotation), afin de ne pas provoquer de désorientation.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemple de Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) : tous les contrôles XAML dans un format interactif.

## <a name="related-topics"></a>Rubriques connexes

- [Classe Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)
- [Informations de base relatives à la conception de la navigation](../basics/navigation-basics.md)