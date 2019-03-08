---
Description: Le contrôle de tableau croisé dynamique permet de balayage tactile entre un petit ensemble de sections de contenu.
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
ms.openlocfilehash: 56079bc51d3efa8f7ecaaee21379a6e9caf7d440
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642924"
---
# <a name="pivot"></a>Pivot

Le [Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) contrôle permet de balayage tactile entre un petit ensemble de sections de contenu.

> **API importantes**: [Classe de tableau croisé dynamique](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot), [NavigationView classe](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.NavigationView)

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous avez le <strong style="font-weight: semi-bold">galerie de contrôles XAML</strong> application installée, cliquez ici pour <a href="xamlcontrolsgallery:/item/Pivot">ouvrir l’application et que le contrôle de tableau croisé dynamique en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application de la galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Contrôler le tableau croisé dynamique, comme [NavigationView](navigationview.md), souligne l’élément sélectionné.

![Le focus par défaut souligne l’en-tête sélectionné](images/pivot_focus_selectedHeader.png)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Pour obtenir la navigation supérieure courantes et les modèles d’onglets, nous recommandons d’utiliser [NavigationView](navigationview.md), qui s’adapte aux différentes tailles d’écran et permet une personnalisation supérieure automatiquement.

Toutefois, si votre navigation requiert balayant-touch, nous recommandons à l’aide du tableau croisé dynamique.

Les autres principales différences entre les contrôles NavigationView et de tableau croisé dynamique sont le comportement de dépassement de capacité par défaut et l’API de la navigation :

- Dépassement de capacité tapis roulants éléments, tandis que NavigationView utilise une liste déroulante du menu de dépassement de capacité afin que les utilisateurs peuvent voir tous les éléments de tableau croisé dynamique.
- Pivot gère la navigation entre des sections de contenu, tandis que NavigationView permet de mieux contrôler le comportement de navigation.

## <a name="use-navigationview-instead-of-pivot"></a>Utilisez NavigationView au lieu de tableau croisé dynamique

Si l’interface utilisateur de votre application utilise le contrôle de tableau croisé dynamique, puis vous pouvez convertir Pivot NavigationView par le code ci-dessous.

Ce XAML crée un NavigationView avec 3 sections du contenu, comme dans l’exemple de tableau croisé dynamique dans [créer un contrôle pivot](#create-a-pivot-control).

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

NavigationView fournit davantage de contrôle sur la personnalisation de la navigation et nécessite du code-behind correspondant. Pour accompagner le XAML ci-dessus, utilisez le code-behind suivant :

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

Ce code reproduit l’expérience de navigation intégrée du contrôle de tableau croisé dynamique, moins l’expérience tactile-balayant entre des sections de contenu. Toutefois, comme vous pouvez le voir, vous pouvez également personnaliser plusieurs points, y compris la transition animée, des paramètres de navigation et les fonctionnalités de pile.

## <a name="create-a-pivot-control"></a>Créer un contrôle Pivot

Ce code crée un contrôle Pivot de base avec 3 sections du contenu.

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

Le contrôle Pivot est un [ItemsControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx). Il peut donc contenir une collection d’éléments de n’importe quel type. Tout élément que vous ajoutez au contrôle Pivot qui n’est pas explicitement un élément [PivotItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivotitem.aspx) est implicitement encapsulé dans un PivotItem. Un sélecteur de vue est souvent utilisé pour naviguer entre des pages de contenu. Il est donc courant de remplir la collection [Items](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.items.aspx) directement avec des éléments d’interface utilisateur XAML. Vous pouvez également affecter à la propriété [ItemsSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) une source de données. Les éléments liés dans la propriété ItemsSource peuvent être de n’importe quel type. Cependant, s’il ne s’agit pas explicitement d’éléments PivotItem, vous devez définir un [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) et un [HeaderTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.headertemplate.aspx) pour spécifier leur mode d’affichage.

Vous pouvez utiliser la propriété [SelectedItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selecteditem.aspx) pour obtenir ou définir l’élément actif du sélecteur de vue. Utilisez la propriété [SelectedItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selectedindex.aspx) pour obtenir ou définir l’index de l’élément actif.

### <a name="pivot-headers"></a>En-têtes de sélecteur de vue

Vous pouvez utiliser les propriétés [LeftHeader](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.leftheader.aspx) et [RightHeader](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.rightheader.aspx) pour ajouter d’autres contrôles à l’en-tête du sélecteur de vue.

Par exemple, vous pouvez ajouter une [CommandBar](https://docs.microsoft.com/en-us/windows/uwp/controls-and-patterns/app-bars) dans le RightHeader du sélecteur de vue.

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

> **Remarque** Les en-têtes de sélecteur de vue ne doivent pas s’afficher dans une vue Carrousel dans un [environnement de 10 pieds](../devices/designing-for-tv.md). Définissez la propriété [IsHeaderItemsCarouselEnabled](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot.IsHeaderItemsCarouselEnabled) sur **false** si votre application doit s’exécuter sur Xbox.

## <a name="recommendations"></a>Recommandations

- Évitez d’utiliser plus de 5 en-têtes en mode carrousel (rotation), afin de ne pas provoquer de désorientation.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) - Affichez tous les contrôles XAML dans un format interactif.

## <a name="related-topics"></a>Rubriques connexes

- [Classe de tableau croisé dynamique](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)
- [Principes fondamentaux de conception de navigation](../basics/navigation-basics.md)