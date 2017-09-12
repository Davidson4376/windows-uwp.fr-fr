---
author: Jwmsft
Description: "Les onglets et sélecteurs de vue permettent aux utilisateurs de naviguer entre les contenus fréquemment consultés."
title: "Onglets et sélecteurs de vue"
ms.assetid: 556BC70D-CF5D-4295-A655-D58163CC1824
label: Tabs and pivots
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.openlocfilehash: 263236b4c3ef61afc963544017588cbf3027496d
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/22/2017
---
# <a name="pivot-and-tabs"></a>Sélecteurs de vue et onglets

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Le contrôle Pivot et le modèle onglets associé sont utilisés pour naviguer entre différentes catégories de contenu fréquemment consultées. Les sélecteurs de vue permettent la navigation entre deux ou plusieurs volets de contenu et s’appuient sur les en-têtes de texte pour articuler les différentes sections de contenu.

> **API importante**: [classe Pivot](https://msdn.microsoft.com/library/windows/apps/dn608241)

![Exemples d’onglets](images/pivot_Hero_main.png)

Les onglets sont une variante visuelle de Pivot, qui utilisent une combinaison d’icônes et de texte, voire juste des icônes, afin d’articuler le contenu de la section. Les onglets sont générés à l’aide du contrôle [Pivot](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx) L’[exemple Pivot](http://go.microsoft.com/fwlink/p/?LinkId=619903) montre comment personnaliser le contrôle Pivot dans le modèle onglets.


## <a name="the-pivot-pattern"></a>Modèle de sélecteur de vue

Lorsque vous créez une application avec le sélecteur de vue, vous devez prendre en compte certaines variables de conception clés.

- **Étiquettes d’en-tête.**  Les en-têtes peuvent être associés à une icône, à du texte ou aux deux.
- **Alignement de l’en-tête.**  Les en-têtes peuvent être alignés à gauche ou centrés.
- **Navigation de niveau supérieur ou inférieur.**  Les sélecteurs de vue peuvent être utilisés pour les deux niveaux de navigation. Si vous le souhaitez, le [volet de navigation](navigationview.md) peut servir de niveau principal avec des sélecteurs de vue servant de niveau secondaire.
- **Prise en charge des entrées tactiles.**  Pour les appareils qui prennent en charge les entrées tactiles, vous pouvez utiliser un des deux ensembles d’interactions pour naviguer entre les catégories de contenu:
    1. Appuyez sur un en-tête d’onglet/sélecteur de vue pour accéder à cette catégorie.
    2. Balayez vers la gauche ou vers la droite sur la zone de contenu pour accéder à la catégorie adjacente.

## <a name="examples"></a>Exemples

Contrôle Pivot sur téléphone

![Exemple de Pivot](images/pivot_example.png)

Modèle onglets dans l’application Alarmes et horloge.

![Un exemple de modèle onglets dans les Alarmes et horloge](images/tabs_alarms-and-clock.png)

## <a name="create-a-pivot-control"></a>Créer un contrôle Pivot

Le contrôle [Pivot](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx) est fourni avec les fonctionnalités de base décrites dans cette section.

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

> Remarque&nbsp;&nbsp; Les en-têtes de sélecteur de vue ne doivent pas s’afficher dans une vue Carrousel dans un [environnement de 10pieds](../input-and-devices/designing-for-tv.md). Définissez la propriété [IsHeaderItemsCarouselEnabled](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot#Windows_UI_Xaml_Controls_Pivot_IsHeaderItemsCarouselEnabled) sur **false** si votre application doit s’exécuter sur Xbox.


**Carrousel**

-   Les sélecteurs de vue sont présentés sous forme de carrousel lorsque l’espace autorisé n’est pas suffisant pour contenir tous les en-têtes de sélecteur de vue.
-   Un appui sur une étiquette de sélecteur de vue permet d’accéder à la page correspondante. L’étiquette du sélecteur de vue actif passe en première position par rotation.
-   Placez les éléments sélecteur de vue dans une boucle carrousel de la dernière à la première section de sélecteur de vue.

### <a name="pivot-focus"></a>Focus du sélecteur de vue

Par défaut, le focus clavier dans un en-tête de sélecteur de vue est représenté par un trait de soulignement.

![Le focus par défaut souligne l’en-tête sélectionné](images/pivot_focus_selectedHeader.png)

Les applications qui ont un contrôle Pivot personnalisé et qui incorporent le trait de soulignement dans les éléments visuels de sélection d’en-tête peuvent utiliser la propriété [HeaderFocusVisualPlacement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot#Windows_UI_Xaml_Controls_Pivot_HeaderFocusVisualPlacement) pour modifier la valeur par défaut. Avec `HeaderFocusVisualPlacement="ItemHeaders"`, un focus est dessiné tout autour du panneau d’en-tête.

![L’option ItemsHeader dessine un rectangle de focus autour de tous les en-têtes de sélecteur de vue](images/pivot_focus_headers.png)

## <a name="recommendations"></a>Recommandations

-   Définissez l’alignement des en-têtes d’onglet/sélecteur de vue selon la taille de l’écran. Pour les largeurs d’écran inférieures à 720epx, l’alignement au centre est généralement plus efficace. Dans la plupart des cas, l’alignement à gauche est recommandé pour les largeurs d’écran supérieures à 720epx.
-   Évitez d’utiliser plus de 5en-têtes en mode carrousel (rotation), afin de ne pas provoquer de désorientation.
-   Utilisez le modèle onglets uniquement si vos éléments sélecteur de vue comportent des icônes distinctes.
-   Incluez du texte dans les en-têtes d’éléments sélecteur de vue pour aider les utilisateurs à comprendre la signification de chaque section de sélecteur de vue. Les icônes ne sont pas forcément explicites pour tous les utilisateurs.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code
- [Exemple Pivot](http://go.microsoft.com/fwlink/p/?LinkId=619903)<br/>
    Découvrez comment personnaliser le contrôle Pivot dans le modèle onglets.
- [Exemple d’éléments de base d’une interface utilisateur XAML](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)<br/>
    Affichez tous les contrôles XAML dans un format interactif.

## <a name="related-topics"></a>Rubriques connexes
- [Informations de base relatives à la conception de la navigation](../layout/navigation-basics.md)
