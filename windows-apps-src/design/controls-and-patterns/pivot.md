---
author: QuinnRadich
Description: The Pivot control enables touch-swiping between a small set of content sections.
title: Pivot
template: detail.hbs
ms.author: quradic
ms.date: 06/19/2018
ms.topic: article
keywords: windows10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 386fba3cec00de6c443daa60409fe3bb74621fa1
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5863913"
---
# <a name="pivot"></a>Pivot

Le contrôle de [sélecteur de vue](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) permet balayage tactile entre un petit ensemble de sections de contenu.

> **API importantes**: [classe Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot), [classe NavigationView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.NavigationView)

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong> est installée, cliquez ici pour <a href="xamlcontrolsgallery:/item/Pivot">Ouvrir l’application et voir le contrôle de sélecteur de vue en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Le contrôle Pivot, tout comme [NavigationView](navigationview.md), souligne l’élément sélectionné.

![Le focus par défaut souligne l’en-tête sélectionné](images/pivot_focus_selectedHeader.png)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

Pour obtenir navigation supérieure courantes et des modèles d’onglets, nous vous recommandons d’à l’aide de [NavigationView](navigationview.md), qui s’adapte à différentes tailles d’écran et permet la personnalisation supérieure automatiquement.

Toutefois, si votre navigation requiert le balayage-tactile, nous recommandons d’utiliser le sélecteur de vue.

Les autres principales différences entre les contrôles NavigationView et sélecteur de vue sont le comportement de dépassement de capacité par défaut et l’API de la navigation:

- Dépassement carrousels éléments, tandis que NavigationView utilise une liste déroulante du menu de dépassement de capacité afin que les utilisateurs puissent voir tous les éléments de sélecteur de vue.
- Sélecteur de vue gère la navigation entre les sections de contenu, tandis que NavigationView permet de mieux contrôler le comportement de navigation.

## <a name="use-navigationview-instead-of-pivot"></a>Utiliser NavigationView au lieu de sélecteur de vue

Si l’interface utilisateur de votre application utilise le contrôle de sélecteur de vue, vous pouvez convertir Pivot à NavigationView avec le code ci-dessous.

Ce code XAML crée un NavigationView avec 3 sections de contenu, comme dans l’exemple Pivot dans [créer un contrôle de sélecteur de vue](#create-a-pivot-control).

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

NavigationView fournit plus de contrôle sur la personnalisation de la navigation et nécessite le code-behind correspondant. Pour accompagner le code XAML ci-dessus, utilisez le code-behind suivant:

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

Ce code simule l’expérience de navigation intégrée du contrôle Pivot, moins l’expérience tactile-balayage entre les sections de contenu. Toutefois, comme vous pouvez le constater, vous pouvez également personnaliser plusieurs points, y compris la transition animée, paramètres de navigation et des fonctionnalités de pile.

## <a name="create-a-pivot-control"></a>Créer un contrôle Pivot

Ce code crée un contrôle Pivot de base avec 3 sections de contenu.

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

Le contrôle offre les interactions d’entrée tactile suivantes:

- Un appui sur un en-tête d’élément sélecteur de vue permet d’accéder au contenu de la section de cet en-tête.
- Un mouvement de balayage sur un en-tête d’élément sélecteur de vue vers la gauche ou la droite permet d’accéder à la section adjacente.
- Un mouvement de balayage sur une section vers la gauche ou la droite permet d’accéder à la section adjacente.

Le contrôle est disponible en deux modes:

**Stationnaire**

- Les sélecteurs de vue sont stationnaires lorsque l’espace autorisé est suffisant pour contenir tous les en-têtes de sélecteur de vue.
- Un appui sur une étiquette de sélecteur de vue permet d’accéder à la page correspondante, bien que le sélecteur de vue proprement dit ne bouge pas. Le sélecteur de vue actif est mis en surbrillance.

**Carrousel**

- Les sélecteurs de vue sont présentés sous forme de carrousel lorsque l’espace autorisé n’est pas suffisant pour contenir tous les en-têtes de sélecteur de vue.
- Un appui sur une étiquette de sélecteur de vue permet d’accéder à la page correspondante. L’étiquette du sélecteur de vue actif passe en première position par rotation.
- Placez les éléments dans une boucle carrousel de la dernière à la première section de sélecteur de vue.

> **Remarque** Les en-têtes de sélecteur de vue ne doivent pas s’afficher dans une vue Carrousel dans un [environnement de 10pieds](../devices/designing-for-tv.md). Définissez la propriété [IsHeaderItemsCarouselEnabled](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot.IsHeaderItemsCarouselEnabled) sur **false** si votre application doit s’exécuter sur Xbox.

## <a name="recommendations"></a>Recommandations

- Évitez d’utiliser plus de 5en-têtes en mode carrousel (rotation), afin de ne pas provoquer de désorientation.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.

## <a name="related-topics"></a>Rubriquesassociées

- [Classe Sélecteur de vue](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)
- [Informations de base relatives à la conception de la navigation](../basics/navigation-basics.md)