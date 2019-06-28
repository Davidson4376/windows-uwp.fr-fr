---
Description: ItemsRepeater est un contrôle léger destiné à générer et à présenter une collection d’éléments.
title: ItemsRepeater
label: ItemsRepeater
template: detail.hbs
ms.date: 02/01/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 93a81501b524826484111419899675fbb99b86fa
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66364758"
---
# <a name="itemsrepeater"></a>ItemsRepeater

[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) permet de créer des expériences de collection personnalisées à l’aide d’un système de disposition flexible, de vues personnalisées et de la virtualisation.

Contrairement à un [ListView](/uwp/api/windows.ui.xaml.controls.listview), le contrôle [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) n’offre pas d’expérience d’utilisateur final complète. Il ne propose pas d’interface utilisateur par défaut et n’offre aucune stratégie autour du focus, de la sélection ou de l’interaction utilisateur. Il s’agit d’un bloc de construction dont vous pouvez vous servir pour créer vos propres expériences à base de collections uniques et vos propres contrôles personnalisés. S’il ne propose aucune stratégie intégrée, il vous permet d’attacher une stratégie pour créer l’expérience dont vous avez besoin. Par exemple, vous pouvez définir la disposition à utiliser, la stratégie de clavier, la stratégie de sélection, etc.

D’un point de vue conceptuel, vous pouvez voir [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) comme un panneau piloté par les données et non comme un contrôle complet comme ListView. Vous spécifiez une collection d’éléments de données à afficher, un modèle d’élément qui génère un élément d’interface utilisateur pour chaque élément de données et une disposition qui détermine la façon dont les éléments sont dimensionnés et positionnés. Ensuite, ItemsRepeater génère des éléments enfants en fonction de la source de données et les affiche comme le modèle d’élément et la disposition le spécifient. Les éléments affichés n’ont pas besoin d’être homogènes, car ItemsRepeater peut charger du contenu pour représenter les éléments de données en fonction des critères que vous spécifiez dans un sélecteur de modèle de données.

| **Obtenir la bibliothèque d’interface utilisateur Windows** |
| - |
| Ce contrôle est inclus dans la bibliothèque d’interface utilisateur Windows, package NuGet qui contient les nouveaux contrôles et fonctionnalités d’interface utilisateur pour les applications UWP. Pour obtenir des informations complémentaires, notamment des instructions d’installation, consultez [Vue d’ensemble de la bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

> **API importantes** : [ItemsRepeater, classe](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), [ScrollViewer, classe](/uwp/api/windows.ui.xaml.controls.scrollviewer)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) permet de créer des affichages personnalisés pour les collections de données. Même s’il peut servir à présenter un ensemble d’éléments simple, vous pouvez souvent être utilisé comme élément d’affichage dans le modèle d’un contrôle personnalisé.

Si vous avez besoin d’un contrôle prêt à l’emploi en vue d’afficher des données dans une liste ou une grille avec une personnalisation minimale, envisagez d’utiliser un contrôle [ListView](/uwp/api/windows.ui.xaml.controls.listview) ou un [GridView](/uwp/api/windows.ui.xaml.controls.gridview).

ItemsRepeater ne propose pas de collection d’éléments intégrée. Si vous avez besoin de fournir une collection d’éléments directement, au lieu de vous lier à une source de données distincte, vous avez probablement besoin d’une expérience de stratégie élevée et avez tout intérêt à utiliser un contrôle [ListView](/uwp/api/windows.ui.xaml.controls.listview) ou [GridView](/uwp/api/windows.ui.xaml.controls.gridview).

[ItemsControl](/uwp/api/windows.ui.xaml.controls.itemscontrol) et ItemsRepeater autorisent tous deux des expériences de collection personnalisables, mais ItemsRepeater prend en charge la virtualisation des dispositions d’interface utilisateur, ce qui n’est pas le cas d’un contrôle ItemsControl. Nous recommandons d’utiliser ItemsRepeater plutôt qu’un contrôle ItemsControl, que ce soit pour présenter simplement quelques éléments de données ou pour créer un contrôle de collection personnalisé.

> [!NOTE]
> Si dans une situation donnée, vous jugez qu’un ItemsControl répond davantage à vos besoins qu’un ItemsRepeater, laissez un commentaires sur le [projet GitHub de bibliothèque d’interface utilisateur Windows](https://github.com/Microsoft/microsoft-ui-xaml/issues) et faites-le nous savoir.

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour ouvrir l’application et voir <a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a> en action.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="scrolling-with-itemsrepeater"></a>Défilement avec ItemsRepeater

[**ItemsRepeater**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) ne dérive pas de [**Control**](/uwp/api/windows.ui.xaml.controls.control). De ce fait, il ne possède pas de modèle de contrôle. Il n’offre donc pas de défilement intégré comme un ListView ou d’autres contrôles de collection.

Quand vous utilisez un **ItemsRepeater**, vous devez fournir la fonctionnalité de défilement en l’encapsulant dans un contrôle [**ScrollViewer**](/uwp/api/windows.ui.xaml.controls.scrollviewer).

> [!NOTE]
> Si votre application doit s’exécuter sur des versions antérieures de Windows (celles sorties *avant* Windows 10 version 1809), vous devez aussi héberger le **ScrollViewer** dans le [  **ItemsRepeaterScrollHost**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeaterscrollhost). 
> ```xaml
> <muxc:ItemsRepeaterScrollHost>
>     <ScrollViewer>
>         <muxc:ItemsRepeater ... />
>     </ScrollViewer>
> </muxc:ItemsRepeaterScrollHost>
> ```
> Si votre application doit s’exécuter uniquement sur les versions récentes de Windows 10 (version 1809 et ultérieures), il est pas utile d’utiliser le contrôle [**ItemsRepeaterScrollHost**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeaterscrollhost).
>
> Avant Windows 10 version 1809, **ScrollViewer** n’implémentait pas l’interface [**IScrollAnchorProvider**](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) dont a besoin le contrôle **ItemsRepeater**.  **ItemsRepeaterScrollHost** permet à **ItemsRepeater** de se coordonner avec **ScrollViewer** sur les versions antérieures dans le but de préserver l’emplacement des éléments que voit l’utilisateur.  Sinon, les éléments risquent de se déplacer ou de disparaître soudainement quand les éléments de la liste sont modifiés ou que l’application est redimensionnée.

## <a name="create-an-itemsrepeater"></a>Créer un ItemsRepeater

Pour utiliser un [**ItemsRepeater**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), vous devez lui fournir les données à afficher en définissant la propriété **ItemsSource**. Vous devez ensuite lui dire comment afficher les éléments en définissant la propriété [**ItemTemplate**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate).

### <a name="itemssource"></a>ItemsSource

Pour remplir la vue, définissez la propriété [**ItemsSource**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemssource) sur une collection d’éléments de données. Ici, **ItemsSource** est défini directement dans le code sur une instance de collection.

```csharp
ObservableCollection<string> Items = new ObservableCollection<string>();

ItemsRepeater itemsRepeater1 = new ItemsRepeater();
itemsRepeater1.ItemsSource = Items;
```

Vous pouvez aussi lier la propriété **ItemsSource** à une collection en XAML. Pour plus d’informations sur la liaison de données, voir [Vue d’ensemble de la liaison de données](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-quickstart).


```xaml
<ItemsRepeater ItemsSource="{x:Bind Items}"/>
```

### <a name="itemtemplate"></a>ItemTemplate
Pour préciser la façon dont un élément de données est visualisé, définissez la propriété [**ItemTemplate**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) sur un [**DataTemplate**](/uwp/api/windows.ui.xaml.datatemplate) ou un [ **DataTemplateSelector**](/uwp/api/windows.ui.xaml.controls.datatemplateselector) que vous avez défini. Le modèle de données définit la façon dont les données sont visualisées. Par défaut, l’élément s’affiche dans la vue avec un **TextBlock** qui utilise la représentation sous forme de chaîne de l’objet de données.

Or, vous souhaiterez probablement enrichir la présentation de vos données à l’aide d’un modèle qui définit la disposition et l’apparence d’un ou plusieurs contrôles que vous prévoyez d’utiliser pour afficher un élément individuel. Les contrôles que vous utilisez dans le modèle peuvent être liés aux propriétés de l’objet de données ou leur contenu statique peut être défini inline.

#### <a name="datatemplate"></a>DataTemplate
Dans cet exemple, l’objet de données est une chaîne simple. Le **DataTemplate** comporte une image à gauche du texte et ajoute un style au **TextBlock** pour afficher la chaîne dans une couleur bleu-vert.

> [!NOTE]
> Si vous utilisez l’[extension de balisage x:Bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) dans un **DataTemplate**, vous devez spécifier le DataType (`x:DataType`) sur le DataTemplate.

```xaml
<DataTemplate x:DataType="x:String">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="47"/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>
        <Image Source="Assets/placeholder.png" Width="32" Height="32"
               HorizontalAlignment="Left"/>
        <TextBlock Text="{x:Bind}" Foreground="Teal"
                   FontSize="15" Grid.Column="1"/>
    </Grid>
</DataTemplate>
```

Voici comment les éléments s’afficheront avec ce **DataTemplate**.

![Éléments affichés avec un modèle de données](images/listview-itemstemplate.png)

Le nombre d’éléments utilisés dans le **DataTemplate** pour un élément peut avoir un impact significatif sur les performances si votre vue affiche un grand nombre d’éléments. Pour obtenir des informations complémentaires et des exemples qui montrent comment utiliser des **DataTemplate** pour définir l’apparence des éléments de votre liste, consultez [Modèles et conteneurs d’éléments](item-containers-templates.md).

> [!TIP]
> Pour des raisons pratiques, quand vous voulez déclarer le modèle inline au lieu de le référencer en tant que ressource statique, vous pouvez spécifier le **DataTemplate** ou le **DataTemplateSelector** comme enfant direct du contrôle **ItemsRepeater**.  Il sera affecté comme valeur de la propriété **ItemTemplate**. Par exemple, ce qui suit est valide :
> ```xaml
> <ItemsRepeater ItemsSource="{x:Bind Items}">
>     <DataTemplate>
>         <!-- ... -->
>     </DataTemplate>
> </ItemsRepeater>
> ```

> [!TIP]
> Contrairement aux **ListView** et aux autres contrôles de collection, le contrôle **ItemsRepeater** n’encapsule pas les éléments d’un **DataTemplate** avec un conteneur d’éléments supplémentaire qui comporte une stratégie par défaut comme des marges, un remplissage, des visuels de sélection ou un pointeur sur un état visuel. Au lieu de cela, **ItemsRepeater** présente uniquement ce qui est défini dans le **DataTemplate**. Si vous voulez que vos éléments aient le même aspect qu’un élément de mode Liste, vous pouvez inclure explicitement un conteneur, comme **ListViewItem**, dans votre modèle de données. **ItemsRepeater** affichera les visuels **ListViewItem**, mais n’utilisera pas automatiquement d’autres fonctionnalités, comme la sélection ou l’affichage de la case à cocher à sélection multiple.
>
> De la même façon, si votre collection de données est une collection de contrôles réels, comme **Button** (`List<Button>`), vous pouvez placer un **ContentPresenter** dans votre **DataTemplate** pour afficher le contrôle.

#### <a name="datatemplateselector"></a>DataTemplateSelector

Les éléments que vous affichez dans votre vue ne doivent pas nécessairement être du même type. Vous pouvez fournir à la propriété [**ItemTemplate**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) un [**DataTemplateSelector**](/uwp/api/windows.ui.xaml.controls.datatemplateselector) pour sélectionner des  **DataTemplate** qui varient en fonction des critères que vous spécifiez.

Dans cet exemple, un **DataTemplateSelector** a été défini pour choisir entre deux **DataTemplate** afin de représenter un élément Large (grand) et un élément Small (petit).

```xaml
<ItemsRepeater ...>
    <ItemsRepeater.ItemTemplate>
        <local:VariableSizeTemplateSelector Large="{StaticResource LargeItemTemplate}" 
                                            Small="{StaticResource SmallItemTemplate}"/>
    </ItemsRepeater.ItemTemplate>
</ItemsRepeater>
```

Quand il s’agit de définir le **DataTemplateSelector** à utiliser avec **ItemsRepeater**, vous devez simplement implémenter une substitution pour la méthode [**SelectTemplateCore(Object)** ](/uwp/api/windows.ui.xaml.controls.datatemplateselector.selecttemplatecore#Windows_UI_Xaml_Controls_DataTemplateSelector_SelectTemplateCore_System_Object_). Pour plus d’informations et d’exemples, consultez [**DataTemplateSelector**](/uwp/api/windows.ui.xaml.controls.datatemplateselector).

> [!NOTE]
> Plutôt que d’utiliser des **DataTemplate** pour gérer la façon dont les éléments sont créés dans des scénarios plus avancés, vous pouvez implémenter votre propre [**Windows.UI.Xaml.Controls.IElementFactory**](/uwp/api/windows.ui.xaml.controls.ielementfactory) pour l’utiliser comme **ItemTemplate**.  Il sera chargé de générer le contenu quand cela lui sera demandé.

## <a name="configure-the-data-source"></a>Configurer la source de données

Utilisez la propriété [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemssource) pour spécifier la collection à utiliser pour générer le contenu d’éléments. Vous pouvez affecter à ItemsSource tout type qui implémente **IEnumerable**. Les autres interfaces de collection implémentées par votre source de données déterminent les fonctionnalités disponibles permettant au contrôle ItemsRepeater d’interagir avec vos données.

Cette liste présente les interfaces disponibles et indique dans quels cas envisager leur utilisation.

- [IEnumerable](/dotnet/api/system.collections.generic.ienumerable-1)(.NET) / [IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)

  - Peut être utilisé pour les petits jeux de données statiques.

    Au minimum, la source de données doit implémenter l’interface IEnumerable / IIterable. Si rien d’autre n’est pris en charge, le contrôle effectue une itération unique dans tous les éléments pour créer une copie qu’il peut utiliser pour accéder aux éléments via une valeur d’index.

- [IReadonlyList](/dotnet/api/system.collections.generic.ireadonlylist-1)(.NET) / [IVectorView](/uwp/api/windows.foundation.collections.ivectorview_t_)

  - Peut être utilisé pour des jeux de données statiques en lecture seule.

    Permet au contrôle d’accéder aux éléments par index et évite la copie interne redondante.

- [IList](/dotnet/api/system.collections.generic.ilist-1)(.NET) / [IVector](/uwp/api/windows.foundation.collections.ivector_t_)

  - Peut être utilisé pour les petits jeux de données.

    Permet au contrôle d’accéder aux éléments par index et évite la copie interne redondante.

    **Avertissement** : les modifications apportées à la liste/vecteur sans implémenter [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged) ne seront pas intégrées dans l’interface utilisateur.

- [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged)(.NET)

  - Recommandé pour prendre en charge la notification de modification.

    Permet au contrôle d’observer et de réagir aux modifications dans la source de données et d’intégrer ces modifications dans l’interface utilisateur.

- [IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)

  - Prend en charge la notification de modification.

    Comme l’interface **INotifyCollectionChanged**, elle permet au contrôle d’observer et de réagir aux modifications dans la source de données.

    **Avertissement** : Windows.Foundation.IObservableVector\<T> ne prend pas en charge l’action « Move » de déplacement. Cela peut faire perdre l’état visuel de l’interface utilisateur pour un élément.  Par exemple, un élément sélectionné et/ou qui a le focus et qui est déplacé par une action « Remove » de suppression suivie d’une action « Add » d’ajout perdra le focus et ne sera plus sélectionné.

    Platform.Collections.Vector\<T> utilise IObservableVector\<T> et présente les mêmes limites. Si la prise en charge d’une action « Move » de déplacement est nécessaire, utilisez l’interface **INotifyCollectionChanged**.  La classe .NET ObservableCollection\<T> utilise **INotifyCollectionChanged**.

- [IKeyIndexMapping](/uwp/api/microsoft.ui.xaml.controls.ikeyindexmapping)

  - Quand un identificateur unique peut être associé à chaque élément.  Recommandé quand « Reset » est utilisé comme action de modification de collection.

    Permet au contrôle de récupérer très efficacement l’interface utilisateur existante après la réception d’une action « Reset » de réinitialisation dure à l’occasion d’un événement **INotifyCollectionChanged** ou **IObservableVector**. Après avoir reçu une action « Reset » de réinitialisation, le contrôle utilise l’ID unique fourni pour associer les données actuelles à des éléments qu’il avait déjà créés. Sans correspondance entre la clé l’index, le contrôle devrait supposer qu’il doit recréer entièrement l’interface utilisateur pour les données.

Hormis IKeyIndexMapping, les interfaces listées ci-dessus fournissent le même comportement dans ItemsRepeater que dans ListView et GridView.


Les interfaces suivantes définies dans une propriété ItemsSource activent une fonctionnalité spéciale dans les contrôles ListView et GridView, mais n’ont actuellement aucun effet sur un ItemsRepeater :

- [ISupportIncrementalLoading](/uwp/api/windows.ui.xaml.data.isupportincrementalloading)
- [IItemsRangeInfo](/uwp/api/windows.ui.xaml.data.iitemsrangeinfo)
- [ISelectionInfo](/uwp/api/windows.ui.xaml.data.iselectioninfo)

> [!TIP]
> Partagez votre expérience. Dites-nous ce que vous pensez sur le [projet GitHub de la bibliothèque d’interface utilisateur Windows](https://github.com/Microsoft/microsoft-ui-xaml/issues). Pensez à nous donner votre avis sur les propositions existantes comme [#374](https://github.com/Microsoft/microsoft-ui-xaml/issues/374) : Ajouter la prise en charge du chargement incrémentiel pour ItemsRepeater.

Une autre approche du chargement incrémentiel de vos données pendant que l’utilisateur fait défiler l’écran vers le haut ou le bas consiste à observer la position de la fenêtre d’affichage du ScrollViewer et de charger davantage de données à mesure que la fenêtre d’affichage s’approche de l’étendue.

```xaml
<ScrollViewer ViewChanged="ScrollViewer_ViewChanged">
    <ItemsRepeater ItemsSource="{x:Bind MyItemsSource}" .../>
</ScrollViewer>
```

```csharp
private async void ScrollViewer_ViewChanged(object sender, ScrollViewerViewChangedEventArgs e)
{
    if (!e.IsIntermediate)
    {
        var scroller = (ScrollViewer)sender;
        var distanceToEnd = scroller.ExtentHeight - (scroller.VerticalOffset + scroller.ViewportHeight);

        // trigger if within 2 viewports of the end
        if (distanceToEnd <= 2.0 * scroller.ViewportHeight
                && MyItemsSource.HasMore && !itemsSource.Busy)
        {
            // show an indeterminate progress UI
            myLoadingIndicator.Visibility = Visibility.Visible;

            await MyItemsSource.LoadMoreItemsAsync(/*DataFetchSize*/);

            loadingIndicator.Visibility = Visibility.Collapsed;
        }
    }
}
```

## <a name="change-the-layout-of-items"></a>Modifier la disposition des éléments

Les éléments affichés par le contrôle [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) sont organisés par un objet [Layout](/uwp/api/microsoft.ui.xaml.controls.layout) qui gère le dimensionnement et le positionnement de ses éléments enfants. Quand il est utilisé avec un ItemsRepeater, l’objet Layout permet la virtualisation de l’interface utilisateur. Les dispositions fournies sont [StackLayout](/uwp/api/microsoft.ui.xaml.controls.stacklayout) et [UniformGridLayout](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout). Par défaut, ItemsRepeater utilise un StackLayout avec une orientation verticale.

### <a name="stacklayout"></a>StackLayout

[StackLayout](/uwp/api/microsoft.ui.xaml.controls.stacklayout) réorganise les éléments en une seule ligne que vous pouvez orienter horizontalement ou verticalement.

Vous pouvez définir la propriété [Spacing](/en-us/uwp/api/microsoft.ui.xaml.controls.stacklayout.spacing) pour ajuster l’espace entre les éléments. L’espacement est appliqué selon l’[Orientation](/uwp/api/microsoft.ui.xaml.controls.stacklayout.orientation) de la disposition.

![Espacement d’une disposition de pile](images/stack-layout.png)

Cet exemple montre comment définir la propriété ItemsRepeater.Layout sur un StackLayout avec une orientation horizontale et un espacement de 8 pixels.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<muxc:ItemsRepeater ItemsSource="{x:Bind Items}" ItemTemplate="{StaticResource MyTemplate}">
    <muxc:ItemsRepeater.Layout>
        <muxc:StackLayout Orientation="Horizontal" Spacing="8"/>
    </muxc:ItemsRepeater.Layout>
</muxc:ItemsRepeater>
```

### <a name="uniformgridlayout"></a>UniformGridLayout

La disposition [UniformGridLayout](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout) positionne les éléments de manière séquentielle dans une disposition de renvoi à la ligne. Les éléments sont disposés de gauche à droite quand l’[Orientation](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.orientation) est de type **Horizontal** et de haut en bas quand l’Orientation est de type **Vertical**. Les éléments sont tous dimensionnés de façon identique.

![Espacement dans une disposition de grille uniforme](images/uniform-grid-layout.png)

Le nombre d’éléments présents dans chaque ligne d’une disposition horizontale dépend de la largeur minimale des éléments. Le nombre d’éléments présents dans chaque colonne d’une disposition verticale dépend de la hauteur minimale des éléments.

- Vous pouvez indiquer explicitement la taille minimale à utiliser en définissant les propriétés [MinItemHeight](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minitemheight) et [MinItemWidth](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minitemwidth).
- Si vous ne spécifiez pas de taille minimale, la taille mesurée du premier élément est considérée comme la taille minimale par élément.

Vous pouvez aussi définir l’espacement minimal de la disposition qui doit séparer les lignes et les colonnes en définissant les propriétés [MinColumnSpacing](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.mincolumnspacing) et [MinRowSpacing](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minrowspacing).

![Espacement et dimensionnement d’une grille uniforme](images/uniform-grid-sizing-spacing.png)

Une fois que le nombre d’éléments présents dans une ligne ou une colonne a été déterminé en fonction de la taille et de la l’espacement minimaux, il peut rester de l’espace non utilisé après le dernier élément de la ligne ou de la colonne (comme l’illustre l’image précédente). Vous pouvez indiquer que l’espace restant doit être ignoré ou qu’il doit servir à accroître la taille de chaque élément ou à espacer davantage les éléments. Cela est contrôlé par les propriétés [ItemsStretch](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsstretch) et [ItemsJustification](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsjustification).

Vous pouvez définir la propriété [ItemsStretch](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsstretch) pour spécifier dans quelle mesure la taille d’élément est augmentée pour occuper l’espace restant.

Cette liste affiche les valeurs disponibles. Par défaut, l’**Orientation** est de type **Horizontal**.

- **Aucun** : l’espace restant n’est pas utilisé à la fin de la ligne. Il s’agit de l’option par défaut.
- **Fill** : les éléments sont élargis pour occuper l’espace disponible (et agrandis dans le cas d’une orientation verticale).
- **Uniform** : les éléments sont élargis pour occuper l’espace disponible mais aussi agrandis pour préserver les proportions (hauteur et largeur sont inversées dans le sens vertical).

Cette image montre l’effet des valeurs **ItemsStretch** dans une disposition horizontale.

![Étirement des éléments dans une grille uniforme](images/uniform-grid-item-stretch.png)

Quand **ItemsStretch** a la valeur **Aucun**, vous pouvez définir la propriété [ItemsJustification](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsjustification) pour préciser la façon dont l’espace restant est utilisé pour aligner les éléments.

Cette liste affiche les valeurs disponibles. Par défaut, l’**Orientation** est de type **Horizontal**.

- **Début** : les éléments sont alignés par rapport au début de la ligne. L’espace restant n’est pas utilisé à la fin de la ligne. Il s’agit de l’option par défaut.
- **Center** : les éléments sont alignés au centre de la ligne. L’espace restant est réparti uniformément au début et à la fin de la ligne.
- **End** : les éléments sont alignés par rapport à la fin de la ligne. L’espace restant n’est pas utilisé au début de la ligne.
- **SpaceAround** : les éléments sont répartis uniformément. L’espace avant et après chaque élément est identique.
- **SpaceBetween** : les éléments sont répartis uniformément. L’espace ajouté entre chaque élément est identique. Aucun espace n’est ajouté au début et à la fin de la ligne.
- **SpaceEvenly** : les éléments sont répartis uniformément avec un espacement identique entre chaque élément ainsi qu’au début et à la fin de la ligne.

Cette image montre l’effet des valeurs de **ItemsStretch** dans une disposition verticale (appliquées aux colonnes et non aux lignes).

![Justification des éléments dans une grille uniforme](images/uniform-grid-item-justification.png)

> [!TIP]
> La propriété **ItemsStretch** affecte la passe de disposition _measure_. La propriété **ItemsJustification** affecte la passe de disposition _arrange_.

Cet exemple montre comment définir la propriété **ItemsRepeater.Layout** sur un **UniformGridLayout**.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<muxc:ItemsRepeater ItemsSource="{x:Bind Items}"
                    ItemTemplate="{StaticResource MyTemplate}">
    <muxc:ItemsRepeater.Layout>
        <muxc:UniformGridLayout MinItemWidth="200"
                                MinColumnSpacing="28"
                                ItemsJustification="SpaceAround"/>
    </muxc:ItemsRepeater.Layout>
</muxc:ItemsRepeater>
```

## <a name="lifecycle-events"></a>Événements de cycle de vie

Quand vous hébergez des éléments dans un [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), vous pouvez être amené à prendre des mesures quand un élément est affiché ou qu’il cesse de l’être. Vous pouvez par exemple lancer le téléchargement asynchrone d’un contenu, associer l’élément à un mécanisme permettant d’effectuer le suivi de la sélection ou arrêter une tâche en arrière-plan.

Dans un contrôle de virtualisation, vous ne pouvez pas vous fier aux événements Chargés/Non chargés, car l’élément risque de ne pas être supprimé de l’arborescence d’éléments visuels dynamique quand il est recyclé. À la place, d’autres événements sont fournis pour gérer le cycle de vie des éléments. Ce schéma illustre le cycle de vie d’un élément dans un ItemsRepeater et à quels moments les événements associés sont déclenchés.

![Schéma des événements d’un cycle de vie](images/items-repeater-lifecycle.png)

- [**ElementPrepared**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementprepared) se produit chaque fois qu’un élément est prêt à être utilisé. Il intervient aussi bien pour un élément nouvellement créé que pour un élément déjà existant et qui est réutilisé à partir de la file d’attente de recyclage.
- [**ElementClearing**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementclearing) se produit instantanément chaque fois qu’un élément est envoyé à la file d’attente de recyclage, par exemple quand il se situe en dehors de la plage des éléments réalisés.
- [**ElementIndexChanged**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementindexchanged
) se produit pour chaque UIElement réalisé dont l’index de l’élément qu’il représente a changé. Par exemple, quand un autre élément est ajouté ou supprimé dans la source de données, l’index des éléments qui arrivent après dans l’ordre reçoivent cet événement.

Cet exemple montre comment vous pouvez utiliser ces événements pour attacher un service de sélection personnalisé destiné à effectuer le suivi de la sélection d’éléments dans un contrôle personnalisé qui utilise ItemsRepeater pour afficher les éléments.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<UserControl ...>
    ...
    <ScrollViewer>
        <muxc:ItemsRepeater ItemsSource="{x:Bind Items}"
                            ItemTemplate="{StaticResource MyTemplate}"
                            ElementPrepared="OnElementPrepared"
                            ElementIndexChanged="OnElementIndexChanged"
                            ElementClearing="OnElementClearing">
        </muxc:ItemsRepeater>
    </ScrollViewer>
    ...
</UserControl>
```

```csharp
interface ISelectable
{
    int SelectionIndex { get; set; }
    void UnregisterSelectionModel(SelectionModel selectionModel);
    void RegisterSelectionModel(SelectionModel selectionModel);
}

private void OnElementPrepared(ItemsRepeater sender, ElementPreparedEventArgs args)
{
    var selectable = args.Element as ISelectable;
    if (selectable != null)
    {
        // Wire up this item to recognize a 'select' and listen for programmatic
        // changes to the selection model to know when to update its visual state.
        selectable.SelectionIndex = args.Index;
        selectable.RegisterSelectionModel(this.SelectionModel);
    }
}

private void OnElementIndexChanged(ItemsRepeater sender, ElementIndexChangedEventArgs args)
{
    var selectable = args.Element as ISelectable;
    if (selectable != null)
    {
        // Sync the ID we use to notify the selection model when the item
        // we represent has changed location in the data source.
        selectable.SelectionIndex = args.NewIndex;
    }
}

private void OnElementClearing(ItemsRepeater sender, ElementClearingEventArgs args)
{
    var selectable = args.Element as ISelectable;
    if (selectable != null)
    {
        // Disconnect handlers to recognize a 'select' and stop
        // listening for programmatic changes to the selection model.
        selectable.UnregisterSelectionModel(this.SelectionModel);
        selectable.SelectionIndex = -1;
    }
}
```

## <a name="sorting-filtering-and-resetting-the-data"></a>Tri, filtrage et réinitialisation des données

Quand vous effectuez notamment des actions de filtrage ou de tri sur votre jeu de données, vous comparez généralement l’ancien jeu de données aux nouvelles données, puis vous émettez des notifications de modification précises via [INotifyCollectionChanged](/uwp/api/windows.ui.xaml.interop.inotifycollectionchanged). Or, il est souvent plus facile de remplacer entièrement les anciennes données par les nouvelles et de déclencher une notification de modification de collection à l’aide de l’action [Reset](/uwp/api/windows.ui.xaml.interop.notifycollectionchangedaction).

En règle générale, une réinitialisation conduit un contrôle à libérer les éléments enfants existants et à recommencer, créant ainsi l’interface utilisateur en partant de la position de défilement initiale de 0, car il ne sait pas exactement comment les données ont été modifiées au cours de la réinitialisation.

Cependant, si la collection affectée comme ItemsSource prend en charge les identificateurs uniques en implémentant l’interface [IKeyIndexMapping](/uwp/api/microsoft.ui.xaml.controls.ikeyindexmapping), le contrôle ItemsRepeater peut rapidement identifier :

- Les UIElements réutilisables pour les données qui existaient avant et après la réinitialisation
- Les éléments qui étaient auparavant visibles et qui ont été supprimés
- Les nouveaux éléments ajoutés qui seront visibles

Cela permet au contrôle ItemsRepeater d’éviter de recommencer à la position de défilement 0. Cela lui permet aussi de restaurer rapidement les UIElements pour les données qui n’ont pas été modifiées à l’occasion d’une réinitialisation, ce qui se traduit par de meilleurs niveaux de performance.

Cet exemple montre comment afficher une liste d’éléments dans une pile verticale où _MyItemsSource_ est une source de données personnalisée qui encapsule une liste sous-jacente d’éléments. Il expose une propriété _Data_ qui peut servir à réaffecter une nouvelle liste à utiliser comme source d’éléments, ce qui déclenche alors une réinitialisation.

```xaml
<ScrollViewer x:Name="sv">
    <ItemsRepeater x:Name="repeater"
                ItemsSource="{x:Bind MyItemsSource}"
                ItemTemplate="{StaticResource MyTemplate}">
       <ItemsRepeater.Layout>
           <StackLayout ItemSpacing="8"/>
       </ItemsRepeater.Layout>
   </ItemsRepeater>
</ScrollViewer>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    // Similar to an ItemsControl, a developer sets the ItemsRepeater's ItemsSource.
    // Here we provide our custom source that supports unique IDs which enables
    // ItemsRepeater to be smart about handling resets from the data.
    // Unique IDs also make it easy to do things apply sorting/filtering
    // without impacting any state (i.e. selection).
    MyItemsSource myItemsSource = new MyItemsSource(data);

    repeater.ItemsSource = myItemsSource;

    // ...

    // We can sort/filter the data using whatever mechanism makes the
    // most sense (LINQ, database query, etc.) and then reassign
    // it, which in our implementation triggers a reset.
    myItemsSource.Data = someNewData;
}

// ...


public class MyItemsSource : IReadOnlyList<ItemBase>, IKeyIndexMapping, INotifyCollectionChanged
{
    private IList<ItemBase> _data;

    public MyItemsSource(IEnumerable<ItemBase> data)
    {
        if (data == null) throw new ArgumentNullException();

        this._data = data.ToList();
    }

    public IList<ItemBase> Data
    {
        get { return _data; }
        set
        {
            _data = value;

            // Instead of tossing out existing elements and re-creating them,
            // ItemsRepeater will reuse the existing elements and match them up
            // with the data again.
            this.CollectionChanged?.Invoke(
                this,
                new NotifyCollectionChangedEventArgs(NotifyCollectionChangedAction.Reset));
        }
    }

    #region IReadOnlyList<T>

    public ItemBase this[int index] => this.Data != null
        ? this.Data[index]
        : throw new IndexOutOfRangeException();

    public int Count => this.Data != null ? this.Data.Count : 0;
    public IEnumerator<ItemBase> GetEnumerator() => this.Data.GetEnumerator();
    IEnumerator IEnumerable.GetEnumerator() => this.GetEnumerator();

    #endregion

    #region INotifyCollectionChanged

    public event NotifyCollectionChangedEventHandler CollectionChanged;

    #endregion

    #region IKeyIndexMapping

    private int lastRequestedIndex = IndexNotFound;
    private const int IndexNotFound = -1;

    // When UniqueIDs are supported, the ItemsRepeater caches the unique ID for each item
    // with the matching UIElement that represents the item.  When a reset occurs the
    // ItemsRepeater pairs up the already generated UIElements with items in the data
    // source.
    // ItemsRepeater uses IndexForUniqueId after a reset to probe the data and identify
    // the new index of an item to use as the anchor.  If that item no
    // longer exists in the data source it may try using another cached unique ID until
    // either a match is found or it determines that all the previously visible items
    // no longer exist.
    public int IndexForUniqueId(string uniqueId)
    {
        // We'll try to increase our odds of finding a match sooner by starting from the
        // position that we know was last requested and search forward.
        var start = lastRequestedIndex;
        for (int i = start; i < this.Count; i++)
        {
            if (this[i].PrimaryKey.Equals(uniqueId))
                return i;
        }

        // Then try searching backward.
        start = Math.Min(this.Count - 1, lastRequestedIndex);
        for (int i = start; i >= 0; i--)
        {
            if (this[i].PrimaryKey.Equals(uniqueId))
                return i;
        }

        return IndexNotFound;
    }

    public string UniqueIdForIndex(int index)
    {
        var key = this[index].PrimaryKey;
        lastRequestedIndex = index;
        return key;
    }

    #endregion
}

```

## <a name="create-a-custom-collection-control"></a>Créer un contrôle de collection personnalisé

Vous pouvez utiliser [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) pour créer un contrôle de collection personnalisé avec son propre type de contrôle pour présenter chaque élément.

> [!NOTE]
> Cela équivaut à utiliser **ItemsControl**, mais au lieu de dériver du contrôle **ItemsControl** et de placer un **ItemsPresenter** dans le modèle de contrôle, vous dérivez de **Control** et insérez un **ItemsRepeater** dans le modèle de contrôle. Le contrôle de collection personnalisé contient un **ItemsRepeater**, mais n’est pas un **ItemsControl**. Cela implique que vous devez aussi choisir explicitement les propriétés à exposer, et non les propriétés héritées qui ne doivent pas être prises en charge.

Cet exemple montre comment placer un [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) dans le modèle d’un contrôle personnalisé nommé _MediaCollectionView_ et comment exposer ses propriétés.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<Style TargetType="local:MediaCollectionView">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="local:MediaCollectionView">
                <Border
                    Background="{TemplateBinding Background}"
                    BorderBrush="{TemplateBinding BorderBrush}"
                    BorderThickness="{TemplateBinding BorderThickness}">
                    <ScrollViewer x:Name="ScrollViewer">
                        <muxc:ItemsRepeater x:Name="ItemsRepeater"
                                            ItemsSource="{TemplateBinding ItemsSource}"
                                            ItemTemplate="{TemplateBinding ItemTemplate}"
                                            Layout="{TemplateBinding Layout}"
                                            TabFocusNavigation="{TemplateBinding TabFocusNavigation}"/>
                    </ScrollViewer>
                </Border>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

```csharp
public sealed class MediaCollectionView : Control
{
    public object ItemsSource
    {
        get { return (object)GetValue(ItemsSourceProperty); }
        set { SetValue(ItemsSourceProperty, value); }
    }

    // Using a DependencyProperty as the backing store for ItemsSource.  This enables animation, styling, binding, etc...
    public static readonly DependencyProperty ItemsSourceProperty =
        DependencyProperty.Register(nameof(ItemsSource), typeof(object), typeof(MediaCollectionView), new PropertyMetadata(0));

    public DataTemplate ItemTemplate
    {
        get { return (DataTemplate)GetValue(ItemTemplateProperty); }
        set { SetValue(ItemTemplateProperty, value); }
    }

    // Using a DependencyProperty as the backing store for ItemTemplate.  This enables animation, styling, binding, etc...
    public static readonly DependencyProperty ItemTemplateProperty =
        DependencyProperty.Register(nameof(ItemTemplate), typeof(DataTemplate), typeof(MediaCollectionView), new PropertyMetadata(0));

    public Layout Layout
    {
        get { return (Layout)GetValue(LayoutProperty); }
        set { SetValue(LayoutProperty, value); }
    }

    // Using a DependencyProperty as the backing store for Layout.  This enables animation, styling, binding, etc...
    public static readonly DependencyProperty LayoutProperty =
        DependencyProperty.Register(nameof(Layout), typeof(Layout), typeof(MediaCollectionView), new PropertyMetadata(0));

    public MediaCollectionView()
    {
        this.DefaultStyleKey = typeof(MediaCollectionView);
    }
}
```

## <a name="display-grouped-items"></a>Afficher des éléments groupés

Vous pouvez imbriquer un [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) dans la propriété [ItemTemplate](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) d’un autre ItemsRepeater pour créer des dispositions de virtualisation imbriquées. Le framework utilisera les ressources de manière efficace en réduisant au minimum la réalisation inutile d’éléments qui ne sont pas visibles ou proches de la fenêtre d’affichage actuelle.

Cet exemple montre comment afficher une liste d’éléments groupés dans une pile verticale. Le contrôle ItemsRepeater externe génère chaque groupe. Dans le modèle de chaque groupe, un autre ItemsRepeater génère les éléments.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<ScrollViewer>
  <muxc:ItemsRepeater ItemsSource="{x:Bind AppNotifications}"
                      Layout="{StaticResource MyGroupLayout}">
    <muxc:ItemsRepeater.ItemTemplate>
      <DataTemplate x:DataType="ExampleApp:AppNotifications">
        <!-- Group -->
        <StackPanel>
          <!-- Header -->
          TextBlock Text="{x:Bind AppTitle}"/>
          <!-- Items -->
          <muxc:ItemsRepeater ItemsSource="{x:Bind Notifications}"
                              Layout="{StaticResource MyItemLayout}"
                              ItemTemplate="{StaticResource MyTemplate}"/>
          <!-- Footer -->
          <Button Content="{x:Bind FooterText}"/>
        </StackPanel>
      </DataTemplate>
    </muxc:ItemsRepeater.ItemTemplate>
  </muxc:ItemsRepeater>
</ScrollViewer>
```

Cet exemple illustre une disposition d’application constituée de diverses catégories qui peuvent changer en fonction des préférences de l’utilisateur et qui sont présentées sous forme de listes à défilement horizontal.

![Disposition imbriquée avec un ItemsRepeater](images/items-repeater-nested-layout.png)

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<!-- Include the <muxc:ItemsRepeaterScrollHost> if targeting Windows 10 versions earlier than 1809. -->
<ScrollViewer>
  <muxc:ItemsRepeater ItemsSource="{x:Bind Categories}"
                      Background="LightGreen">
    <muxc:ItemsRepeater.ItemTemplate>
      <DataTemplate x:DataType="local:Category">
        <StackPanel Margin="12,0">
          <TextBlock Text="{x:Bind Name}" Style="{ThemeResource TitleTextBlockStyle}"/>
          <!-- Include the <muxc:ItemsRepeaterScrollHost> if targeting Windows 10 versions earlier than 1809. -->
          <ScrollViewer HorizontalScrollMode="Enabled"
                                          VerticalScrollMode="Disabled"
                                          HorizontalScrollBarVisibility="Auto" >
            <muxc:ItemsRepeater ItemsSource="{x:Bind Items}"
                                Background="Orange">
              <muxc:ItemsRepeater.ItemTemplate>
                <DataTemplate x:DataType="local:CategoryItem">
                  <Grid Margin="10"
                        Height="60" Width="120"
                        Background="LightBlue">
                    <TextBlock Text="{x:Bind Name}"
                               Style="{StaticResource SubtitleTextBlockStyle}"
                               Margin="4"/>
                  </Grid>
                </DataTemplate>
              </muxc:ItemsRepeater.ItemTemplate>
              <muxc:ItemsRepeater.Layout>
                <muxc:StackLayout Orientation="Horizontal"/>
              </muxc:ItemsRepeater.Layout>
            </muxc:ItemsRepeater>
          </ScrollViewer>
        </StackPanel>
      </DataTemplate>
    </muxc:ItemsRepeater.ItemTemplate>
  </muxc:ItemsRepeater>
</ScrollViewer>
```

## <a name="bringing-an-element-into-view"></a>Affichage d’un élément dans une vue

Le framework XAML gère déjà l’affichage d’un FrameworkElement dans une vue quand il (1) reçoit le focus du clavier ou (2) reçoit le focus du Narrateur. Il peut exister d’autres cas où vous devez explicitement afficher un élément dans une vue. Par exemple, en réponse à une action utilisateur ou pour restaurer l’état de l’interface utilisateur après une navigation de page.

L’affichage d’un élément virtualisé dans une vue implique les étapes suivantes :
1. Réaliser un UIElement pour un élément
2. Exécuter la disposition pour vérifier que la position de l’élément est correcte
3. Initier une demande d’affichage de l’élément réalisé dans la vue

L’exemple ci-dessous illustre ces étapes à l’occasion de la restauration de la position de défilement d’un élément dans une liste plate verticale après une navigation de page. Dans le cas de données hiérarchiques utilisant des ItemsRepeater imbriqués, l’approche est fondamentalement la même, mais elle doit être mise en œuvre à chaque niveau de la hiérarchie.

```xaml
<ScrollViewer x:Name="scrollviewer">
  <ItemsRepeater x:Name="repeater" .../>
</ScrollViewer>
```

```csharp
public class MyPage : Page
{
    // ...

    protected override void OnNavigatedTo(NavigationEventArgs e)
    {
        base.OnNavigatedTo(e);

            // retrieve saved offset + index(es) of the tracked element and then bring it into view.
            // ... 

            var element = repeater.GetOrCreateElement(index);

            // ensure the item is given a valid position
            element.UpdateLayout();

            element.StartBringIntoView(new BringIntoViewOptions()
            {
                VerticalOffset = relativeVerticalOffset
            });
    }

    protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
    {
        base.OnNavigatingFrom(e);

        // retrieve and save the relative offset and index(es) of the scrollviewer's current anchor element ...
        var anchor = this.scrollviewer.CurrentAnchor;
        var index = this.repeater.GetElementIndex(anchor);
        var anchorBounds = anchor.TransformToVisual(this.scrollviewer).TransformBounds(new Rect(0, 0, anchor.ActualWidth, anchor.ActualHeight));
        relativeVerticalOffset = this.sv.VerticalOffset – anchorBounds.Top;
    }
}

```

## <a name="enable-accessibility"></a>Permettre l’accessibilité

[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) n’offre pas d’expérience d’accessibilité par défaut. La documentation intitulée [Facilité d’utilisation des applications UWP](/windows/uwp/design/usability) est une mine d’informations grâce à laquelle vous pourrez vérifier que votre application offre une expérience utilisateur inclusive. Si vous utilisez ItemsRepeater pour créer un contrôle personnalisé, veillez à consulter la documentation relative aux [Homologues d’automatisation personnalisés](/windows/uwp/design/accessibility/custom-automation-peers).

### <a name="keyboarding"></a>Clavier
La prise en charge minimale du clavier pour le déplacement du focus que propose ItemsRepeater s’appuie sur la [Navigation directionnelle 2D pour le clavier](/windows/uwp/design/input/focus-navigation#2d-directional-navigation-for-keyboard) de XAML.

![Navigation directionnelle](/windows/uwp/design/input/images/keyboard/directional-navigation.png)

Le [mode XYFocusKeyboardNavigation](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode) du contrôle ItemsRepeater est _activé_ par défaut. Selon l’expérience voulue, envisagez d’ajouter la prise en charge des [interactions avec le clavier](/windows/uwp/design/input/keyboard-interactions) courantes, comme les touches Début, Fin, Pg. préc, Pg. suiv.

ItemsRepeater ne vérifie pas automatiquement que l’ordre de tabulation par défaut de ses éléments (qu’ils soient virtualisés ou non) suit celui attribué aux éléments dans les données. Par défaut, la propriété [TabFocusNavigation](/uwp/api/windows.ui.xaml.uielement.tabfocusnavigation) du contrôle ItemsRepeater est définie sur [Once](/uwp/api/windows.ui.xaml.input.keyboardnavigationmode) et non sur la valeur par défaut commune _Local_.

> [!NOTE]
> Le contrôle ItemsRepeater ne garde pas automatiquement en mémoire le dernier élément qui a obtenu le focus.  Cela signifie que lorsqu’un utilisateur utilise Maj+Tab, il peut être dirigé vers le dernier élément réalisé.

### <a name="announcing-item-x-of-y-in-screen-readers"></a>Annonce « Élément _X_ sur _Y_ » dans les lecteurs d’écran

Vous devez réussir à définir les propriétés d’automatisation appropriées, comme les valeurs de **PositionInSet** et **SizeOfSet**, et veiller à ce qu’elles restent à jour quand des éléments sont ajoutés, déplacés, supprimées, etc.

Dans certaines dispositions personnalisées, l’ordre visuel suit une séquence qui n’est pas forcément évidente.  Les utilisateurs s’attendent au minimum à ce que les valeurs des propriétés PositionInSet et SizeOfSet utilisées par les lecteurs d’écran correspondent à l’ordre d’apparition des éléments dans les données (décalage de 1 pour correspondre au comptage naturel et non basé sur 0).

La meilleure façon d’y parvenir est de faire implémenter à l’homologue d’automatisation du contrôle d’élément les méthodes [GetPositionInSetCore](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpositioninsetcore) et [GetSizeOfSetCore](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getsizeofsetcore) et de rapporter la position de l’élément dans le jeu de données représenté par le contrôle. La valeur est calculée uniquement au moment de l’exécution quand une technologie d’assistance y accède et qu’il n’est plus nécessaire de la tenir à jour. La valeur correspond à l’ordre des données.

Cet exemple vous montre comment procéder quand il s’agit de présenter un contrôle personnalisé appelé _CardControl_.

```xaml
<ScrollViewer >
    <ItemsRepeater x:Name="repeater" ItemsSource="{x:Bind MyItemsSource}">
       <ItemsRepeater.ItemTemplate>
           <DataTemplate x:DataType="local:CardViewModel">
               <local:CardControl Item="{x:Bind}"/>
           </DataTemplate>
       </ItemsRepeater.ItemTemplate>
   </ItemsRepeater>
</ScrollViewer>
```

```csharp
internal sealed class CardControl : CardControlBase
{
    protected override AutomationPeer OnCreateAutomationPeer() => new CardControlAutomationPeer(this);

    private sealed class CardControlAutomationPeer : FrameworkElementAutomationPeer
    {
        private readonly CardControl owner;

        public CardControlAutomationPeer(CardControl owner) : base(owner) => this.owner = owner;

        protected override int GetPositionInSetCore()
          => ((ItemsRepeater)owner.Parent)?.GetElementIndex(this.owner) + 1 ?? base.GetPositionInSetCore();

        protected override int GetSizeOfSetCore()
          => ((ItemsRepeater)owner.Parent)?.ItemsSourceView?.Count ?? base.GetSizeOfSetCore();
    }
}
```

## <a name="related-articles"></a>Articles connexes

- [Listes](lists.md)
- [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)
- [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
