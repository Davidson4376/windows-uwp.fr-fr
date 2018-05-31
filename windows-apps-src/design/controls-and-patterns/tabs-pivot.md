---
author: serenaz
Description: Tabs and pivots enable users to navigate between frequently accessed content.
title: Onglets et sélecteurs de vue
ms.assetid: 556BC70D-CF5D-4295-A655-D58163CC1824
label: Tabs and pivots
template: detail.hbs
ms.author: sezhen
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 09e88fd070f7e7517e55d6f57563dd7608696770
ms.sourcegitcommit: 2470c6596d67e1f5ca26b44fad56a2f89773e9cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
ms.locfileid: "1675476"
---
# <a name="pivot-and-tabs"></a>Sélecteurs de vue et onglets



Le contrôle Sélecteur de vue et le modèle onglets associé sont utilisés pour naviguer entre différentes catégories de contenu fréquemment consultées. Les sélecteurs de vue permettent de naviguer entre deux ou plusieurs volets de contenu et s’appuient sur les en-têtes de texte pour désigner les différentes sections de contenu.

> **API importante**: [classe Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)

![Exemple de contrôle de sélecteur de vue.](images/pivot_Hero_main.png)

Les onglets sont une variante visuelle de Pivot, qui utilisent une combinaison d’icônes et de texte, voire juste des icônes, afin d’articuler le contenu de la section. Les onglets sont générés à l’aide du contrôle [Pivot](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx) L’[exemple Pivot](http://go.microsoft.com/fwlink/p/?LinkId=619903) montre comment personnaliser le contrôle Pivot dans le modèle onglets.

![Exemple de modèle onglets](images/tabs.png) 

## <a name="the-pivot-control"></a>Le contrôle de sélecteur de vue

Lorsque vous créez une application avec le sélecteur de vue, vous devez prendre en compte certaines variables de conception clés.

- **Étiquettes d’en-tête.**  Les en-têtes peuvent être associés à une icône, à du texte ou aux deux.
- **Alignement de l’en-tête.**  Les en-têtes peuvent être alignés à gauche ou centrés.
- **Navigation de niveau supérieur ou inférieur.**  Les sélecteurs de vue peuvent être utilisés pour les deux niveaux de navigation. Si vous le souhaitez, la [vue navigation](navigationview.md) peut servir de niveau principal avec des sélecteurs de vue servant de niveau secondaire.
- **Prise en charge des entrées tactiles.**  Pour les appareils qui prennent en charge les entrées tactiles, vous pouvez utiliser un des deux ensembles d’interactions pour naviguer entre les catégories de contenu:
    1. Appuyez sur un en-tête d’onglet/sélecteur de vue pour accéder à cette catégorie.
    2. Balayez vers la gauche ou vers la droite sur la zone de contenu pour accéder à la catégorie adjacente.

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/Pivot">ouvrir l’application et voir le contrôle Sélecteur de vue en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Contrôle Sélecteur de vue sur téléphone

![Exemple de Pivot](images/pivot_example.png)

Modèle onglets dans l’application Alarmes et horloge.

![Un exemple de modèle onglets dans les Alarmes et horloge](images/tabs_alarms-and-clock.png)

## <a name="create-a-pivot-control"></a>Créer un contrôle Pivot

Le contrôle [Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) est fourni avec les fonctionnalités de base décrites dans cette section.

Ce code XAML crée un contrôle Pivot de base avec 3sections de contenu.

```xaml
<Pivot x:Name="rootPivot" Title="Pivot Title">
    <PivotItem Header="Pivot Item 1">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 1."/>
    </PivotItem>
    <PivotItem Header="Pivot Item 2">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 2."/>
    </PivotItem>
    <PivotItem Header="Pivot Item 3">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 3."/>
    </PivotItem>
</Pivot>
```

### <a name="pivot-items"></a>Éléments du contrôle Pivot

Le contrôle Pivot est un [ItemsControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx). Il peut donc contenir une collection d’éléments de n’importe quel type. Tout élément que vous ajoutez au contrôle Pivot qui n’est pas explicitement un élément [PivotItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivotitem.aspx) est implicitement encapsulé dans un PivotItem. Un sélecteur de vue est souvent utilisé pour naviguer entre des pages de contenu. Il est donc courant de remplir la collection [Items](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.items.aspx) directement avec des éléments d’interface utilisateur XAML. Vous pouvez également affecter à la propriété [ItemsSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) une source de données. Les éléments liés dans la propriété ItemsSource peuvent être de n’importe quel type. Cependant, s’il ne s’agit pas explicitement d’éléments PivotItem, vous devez définir un [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) et un [HeaderTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.headertemplate.aspx) pour spécifier leur mode d’affichage.

Vous pouvez utiliser la propriété [SelectedItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selecteditem.aspx) pour obtenir ou définir l’élément actif du sélecteur de vue. Utilisez la propriété [SelectedItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selectedindex.aspx) pour obtenir ou définir l’index de l’élément actif.

### <a name="pivot-headers"></a>En-têtes de sélecteur de vue

Vous pouvez utiliser les propriétés [LeftHeader](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.leftheader.aspx) et [RightHeader](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.rightheader.aspx) pour ajouter d’autres contrôles à l’en-tête du sélecteur de vue.

Par exemple, vous pouvez ajouter une [CommandBar](https://docs.microsoft.com/en-us/windows/uwp/controls-and-patterns/app-bars) dans le RightHeader du sélecteur de vue.

![Exemple de barre de commandes dans l’en-tête droit du contrôle de sélecteur de vue](images/PivotHeader.png)

```xaml
<Pivot>
    <Pivot.RightHeader>
        <CommandBar OverflowButtonVisibility="Collapsed" Background="Transparent">
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

-   Un appui sur un en-tête d’élément sélecteur de vue permet d’accéder au contenu de la section de cet en-tête.
-   Un mouvement de balayage sur un en-tête d’élément sélecteur de vue vers la gauche ou la droite permet d’accéder à la section adjacente.
-   Un mouvement de balayage sur une section vers la gauche ou la droite permet d’accéder à la section adjacente.

![Exemple de balayage vers la gauche d’un contenu de section](images/pivot_w_hand.png)

Le contrôle est disponible en deux modes:

**Stationnaire**

-   Les sélecteurs de vue sont stationnaires lorsque l’espace autorisé est suffisant pour contenir tous les en-têtes de sélecteur de vue.
-   Un appui sur une étiquette de sélecteur de vue permet d’accéder à la page correspondante, bien que le sélecteur de vue proprement dit ne bouge pas. Le sélecteur de vue actif est mis en surbrillance.

**Carrousel**

-   Les sélecteurs de vue sont présentés sous forme de carrousel lorsque l’espace autorisé n’est pas suffisant pour contenir tous les en-têtes de sélecteur de vue.
-   Un appui sur une étiquette de sélecteur de vue permet d’accéder à la page correspondante. L’étiquette du sélecteur de vue actif passe en première position par rotation.
-   Placez les éléments dans une boucle carrousel de la dernière à la première section de sélecteur de vue.

> **Remarque** Les en-têtes de sélecteur de vue ne doivent pas s’afficher dans une vue Carrousel dans un [environnement de 10pieds](../devices/designing-for-tv.md). Définissez la propriété [IsHeaderItemsCarouselEnabled](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot.IsHeaderItemsCarouselEnabled) sur **false** si votre application doit s’exécuter sur Xbox.

### <a name="pivot-focus"></a>Focus du sélecteur de vue

Par défaut, le focus clavier dans un en-tête de sélecteur de vue est représenté par un trait de soulignement.

![Le focus par défaut souligne l’en-tête sélectionné](images/pivot_focus_selectedHeader.png)

Les applications qui ont un contrôle Pivot personnalisé et qui incorporent le trait de soulignement dans les éléments visuels de sélection d’en-tête peuvent utiliser la propriété [HeaderFocusVisualPlacement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.HeaderFocusVisualPlacement) pour modifier la valeur par défaut. Avec `HeaderFocusVisualPlacement="ItemHeaders"`, un focus est dessiné tout autour du panneau d’en-tête.

![L’option ItemsHeader dessine un rectangle de focus autour de tous les en-têtes de sélecteur de vue](images/pivot_focus_headers.png)

## <a name="recommendations"></a>Recommandations

- Définissez l’alignement des en-têtes d’onglet/sélecteur de vue selon la taille de l’écran. Pour les largeurs d’écran inférieures à 720epx, l’alignement au centre est généralement plus efficace. Dans la plupart des cas, l’alignement à gauche est recommandé pour les largeurs d’écran supérieures à 720epx.
- Évitez d’utiliser plus de 5en-têtes en mode carrousel (rotation), afin de ne pas provoquer de désorientation.
- Utilisez le modèle onglets uniquement si vos éléments sélecteur de vue comportent des icônes distinctes.
- Incluez du texte dans les en-têtes d’éléments sélecteur de vue pour aider les utilisateurs à comprendre la signification de chaque section de sélecteur de vue. Les icônes ne sont pas forcément explicites pour tous les utilisateurs.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- L’[exemple Sélecteur de vue](http://go.microsoft.com/fwlink/p/?LinkId=619903) - Explique comment personnaliser le contrôle Sélecteur de vue dans le modèle onglets.
- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.

## <a name="related-topics"></a>Rubriquesassociées

- [Classe Sélecteur de vue](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)
- [Informations de base relatives à la conception de la navigation](../basics/navigation-basics.md)
