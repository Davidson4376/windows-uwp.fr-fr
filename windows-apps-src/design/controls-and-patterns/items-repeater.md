---
Description: ItemsRepeater est un contrôle léger pour générer et de présenter une collection d’éléments.
title: ItemsRepeater
label: ItemsRepeater
template: detail.hbs
ms.date: 02/01/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3344230bc52013825d94cfbe3668acfa0d7a2e13
ms.sourcegitcommit: c10d7843ccacb8529cb1f53948ee0077298a886d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58913999"
---
# <a name="itemsrepeater"></a>ItemsRepeater

Utilisez un [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) pour créer une collection personnalisée des expériences à l’aide d’un système de disposition flexible, des vues personnalisées et la virtualisation.

Contrairement aux [ListView](/uwp/api/windows.ui.xaml.controls.listview), [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) ne fournit pas une expérience complète de l’utilisateur final, il a sans valeur par défaut de l’interface utilisateur, n’ainsi qu’aucune stratégie autour de l’interaction utilisateur, la sélection ou le focus. Au lieu de cela, il est un bloc de construction que vous pouvez utiliser pour créer vos propres expériences de basée sur une collection uniques et les contrôles personnalisés. Si elle n’a aucune stratégie intégrée, permet à attacher la stratégie afin de générer l’expérience que vous avez besoin. Par exemple, vous pouvez définir la disposition à utiliser, la stratégie keyboarding, de la stratégie de sélection, etc.

Vous pouvez considérer [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) conceptuellement comme un panneau piloté par les données, plutôt que comme un contrôle complet, comme ListView. Vous spécifiez une collection d’éléments de données à afficher, un modèle d’élément qui génère un élément d’interface utilisateur pour chaque élément de données et une disposition qui détermine comment les éléments sont dimensionnés et positionnés. Ensuite, ItemsRepeater génère des éléments enfants en fonction de la source de données et les affiche comme spécifié par le modèle d’élément et la disposition. Les éléments affichés n’avez pas besoin être homogènes, car ItemsRepeater peuvent charger du contenu pour représenter les éléments de données en fonction de critères que vous spécifiez dans un sélecteur de modèle de données.

| **Obtenir la bibliothèque d’interface utilisateur Windows** |
| - |
| Ce contrôle est inclus dans le cadre de la bibliothèque d’interface utilisateur de Windows, un package NuGet qui contient les nouveaux contrôles et fonctionnalités de l’interface utilisateur pour les applications UWP. Pour plus d’informations, y compris les instructions d’installation, consultez le [vue d’ensemble de la bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

> **API importantes** : [Classe de ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), [classe de ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Utilisez un [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) pour créer des affichages personnalisés pour les collections de données. Pendant qu’il peut être utilisé pour présenter un ensemble d’éléments de base, vous pouvez souvent l’utiliser en tant que l’élément de graphique dans le modèle d’un contrôle personnalisé.

Si vous avez besoin d’un contrôle out-of-the-box pour afficher des données dans une liste ou grille avec une personnalisation minimale, envisagez d’utiliser un [ListView](/uwp/api/windows.ui.xaml.controls.listview) ou [GridView](/uwp/api/windows.ui.xaml.controls.gridview).

ItemsRepeater n’a pas d’une collection d’éléments intégrée. Si vous devez fournir une collection d’éléments directement, plutôt que de la liaison à une source de données distinct, ensuite, vous êtes susceptible d’ayant besoin d’une expérience plus haute-policy et devez utiliser [ListView](/uwp/api/windows.ui.xaml.controls.listview) ou [GridView](/uwp/api/windows.ui.xaml.controls.gridview).

[ItemsControl](/uwp/api/windows.ui.xaml.controls.itemscontrol) ItemsRepeater les deux expériences de collection personnalisable, mais ItemsRepeater prend en charge la virtualisation mises en page de l’interface utilisateur, n’est pas le cas de ItemsControl. Nous recommandons d’utiliser ItemsRepeater au lieu de ItemsControl, si sa pour simplement présenter quelques éléments à partir des données ou la création d’un contrôle de la collection personnalisée.

> [!NOTE]
> Si vous avez une situation où vous pensez que le ItemsControl répond à vos besoins et ItemsRepeater ne, veuillez laisser des commentaires sur le [projet GitHub de bibliothèque d’interface utilisateur Windows](https://github.com/Microsoft/microsoft-ui-xaml/issues) et donnez-nous votre avis.

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous avez le <strong style="font-weight: semi-bold">galerie de contrôles XAML</strong> application installée, cliquez ici pour ouvrir l’application et consultez le <a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a> en action.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="scrolling-with-itemsrepeater"></a>Le défilement avec ItemsRepeater

[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) ne dérive pas de [contrôle](/uwp/api/windows.ui.xaml.controls.control), donc il n’a pas un modèle de contrôle. Par conséquent, il ne contient pas toutes intégré le défilement, comme un ListView ou effectuer d’autres contrôles de la collection.

Lorsque vous utilisez un ItemsRepeater, vous devez fournir des fonctionnalités de défilement en l’encapsulant dans un [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer) contrôle.

## <a name="create-an-itemsrepeater"></a>Créer un ItemsRepeater

Pour utiliser un [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), vous devez lui attribuer les données à afficher en définissant la propriété ItemsSource. Ensuite, lui indiquer comment afficher les éléments en définissant le [ItemTemplate](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) propriété.

### <a name="itemssource"></a>ItemsSource

Pour remplir la vue, définissez la [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemssource) propriété à une collection d’éléments de données. Ici, l’ItemsSource est définie dans le code directement à une instance d’une collection.

```csharp
ObservableCollection<string> Items = new ObservableCollection<string>();

ItemsRepeater itemsRepeater1 = new ItemsRepeater();
itemsRepeater1.ItemsSource = Items;
```

Vous pouvez également lier la propriété ItemsSource à une collection en XAML. Pour plus d’informations sur la liaison de données, voir [Vue d’ensemble de la liaison de données](https://msdn.microsoft.com/windows/uwp/data-binding/data-binding-quickstart).


```xaml
<ItemsRepeater ItemsSource="{x:Bind Items}"/>
```

### <a name="data-template"></a>Modèle de données

Un modèle de données d’un élément définit la manière dont les données sont visualisées. Par défaut, l’élément est affiché dans la vue en tant que la représentation sous forme de chaîne de l’objet de données à laquelle elle est liée à l’aide d’un TextBlock. Toutefois, en général, on souhaite afficher une représentation enrichie des données. Pour spécifier exactement comment les éléments sont affichés, vous définissez un [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate). Le code XAML dans l’objet DataTemplate définit la disposition et l’apparence des contrôles qui permettent d’afficher un élément spécifique. Les contrôles dans la disposition peuvent être liés aux propriétés d’un objet de données ou leur contenu statique peut être défini inline. Vous affectez le DataTemplate pour le [ItemTemplate](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) propriété de la ItemsRepeater.

Dans cet exemple, l’objet de données est une chaîne simple. Vous utilisez un DataTemplate pour ajouter une image à gauche du texte et appliquer un style TextBlock pour afficher la chaîne en bleu-vert.

> [!NOTE]
> Si vous utilisez l’[extension de balisage x:Bind](https://msdn.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) dans un DataTemplate, vous devez spécifier le DataType (`x:DataType`) sur le DataTemplate.

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

Voici comment les éléments sera affiché avec ce modèle de données.

![Éléments affichés avec un modèle de données](images/listview-itemstemplate.png)

Le nombre d’éléments utilisés dans le modèle de données pour un élément peut avoir un impact significatif sur les performances si votre vue affiche un grand nombre d’éléments. Pour plus d’informations et des exemples montrant comment utiliser des modèles de données pour définir l’apparence des éléments dans votre liste, consultez [conteneurs et les modèles d’élément](item-containers-templates.md).

> [!TIP]
> ItemsRepeater ne renvoie pas le contenu du modèle DataTemplate dans un conteneur d’éléments tels que ListView et autres contrôles de la collection. Au lieu de cela, ItemsRepeater présente uniquement ce qui est défini dans le DataTemplate. Si vous voulez que vos éléments ont le même aspect comme un élément de liste, vous pouvez utiliser un conteneur, comme ListViewItem, dans votre modèle de données. ItemsRepeater affichera les éléments visuels ListViewItem, mais ne rend pas utiliser d’autres fonctionnalités, telles que la sélection ou l’affichage de la case à cocher à sélection multiple.
>
> De même, si votre collection de données est une collection de contrôles réels, comme bouton (`List<Button>`), vous pouvez placer un ContentPresenter dans votre modèle DataTemplate pour afficher le contrôle.

#### <a name="datatemplateselector"></a>DataTemplateSelector

Les éléments que vous affichez dans votre affichage n’avez pas besoin être du même type. [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) peut utiliser un **DataTemplateSelector** pour charger un modèle de données pour représenter les éléments de données en fonction de critères que vous spécifiez. Pour plus d’informations et des exemples, consultez [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector).

> [!NOTE]
> Une alternative à l’aide de DataTemplate ou DataTemplateSelector consiste à implémenter votre propre classe dérivée de [Microsoft.UI.Xaml.Controls.ElementFactory](/uwp/api/microsoft.ui.xaml.controls.elementfactory) qui est chargée de générer le contenu de demande.

## <a name="configure-the-data-source"></a>Configurer la source de données

Utiliser le [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemssource) propriété pour spécifier la collection à utiliser pour générer le contenu d’éléments. Vous pouvez définir l’ItemsSource sur n’importe quel type qui implémente **IEnumerable**. Interfaces de collection supplémentaires implémentées par votre source de données déterminent quelle fonctionnalité est disponible pour le ItemsRepeater pour interagir avec vos données.

Cette liste affiche les interfaces disponibles et le moment d’envisager l’utilisation de chacun d’eux.

- [IEnumerable](/dotnet/api/system.collections.generic.ienumerable-1)(.NET) / [IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)

  - Utilisable pour les petits jeux de données statiques.

    Au minimum, la source de données doit implémenter l’interface IEnumerable / interface IIterable. Si c’est tout ce qui est pris en charge le contrôle effectue une itération dans tous les éléments une fois pour créer une copie qu’il peut utiliser pour accéder aux éléments via une valeur d’index.

- [IReadonlyList](/dotnet/api/system.collections.generic.ireadonlylist-1)(.NET) / [IVectorView](/uwp/api/windows.foundation.collections.ivectorview_t_)

  - Peut être utilisé pour les jeux de données en lecture seule, statiques.

    Active le contrôle d’accès aux éléments par index et permet d’éviter la copie interne redondante.

- [IList](/dotnet/api/system.collections.generic.ilist-1)(.NET) / [IVector](/uwp/api/windows.foundation.collections.ivector_t_)

  - Peut être utilisé pour les jeux de données statiques.

    Active le contrôle d’accès aux éléments par index et permet d’éviter la copie interne redondante.

    **Avertissement** : Modifications apportées à la liste/vecteur sans implémentation [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged) ne seront pas répercutées dans l’interface utilisateur.

- [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged)(.NET)

  - Recommandé pour prendre en charge la notification de modification.

    Active le contrôle à observer et réagir aux modifications de la source de données et refléter ces modifications dans l’interface utilisateur.

- [IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)

  - Prend en charge la notification de modification

    Comme le **INotifyCollectionChanged** interface, cela permet le contrôle à observer et à réagir aux modifications de la source de données.

    **Avertissement** : Le Windows.Foundation.IObservableVector\<T > ne prend pas en charge une action de « Déplacement ». Cela peut entraîner l’interface utilisateur pour un élément perd son état visuel.  Par exemple, un élément qui est actuellement sélectionné et/ou a le focus où le déplacement est obtenu par une suppression suivie d’un 'Add' perd le focus et ne plus être sélectionné.

    Le Platform.Collections.Vector\<T > utilise IObservableVector\<T > et a cette limitation même. Si la prise en charge pour une action de « Déplacement » est nécessaire puis utiliser le **INotifyCollectionChanged** interface.  .NET ObservableCollection\<T > classe utilise **INotifyCollectionChanged**.

- [IKeyIndexMapping](/uwp/api/microsoft.ui.xaml.controls.ikeyindexmapping)

  - Quand un identificateur unique peut être associé à chaque élément.  Recommandé lors de l’utilisation de « Reset » en tant que l’action de modification de collection.

    Active le contrôle à restaurer très efficacement l’interface utilisateur existante après la réception d’une action « Réinitialiser » dure en tant que partie d’un **INotifyCollectionChanged** ou **IObservableVector** événement. Après avoir reçu une réinitialisation, le contrôle utilise l’ID unique fourni à associer les données actuelles des éléments qu’il avait déjà créés. Sans la clé de mappage des index, le contrôle aurait supposer qu’il doit reprendre à zéro lors de la création de l’interface utilisateur pour les données.

Les interfaces répertoriées ci-dessus, autre que IKeyIndexMapping, fournissent le même comportement dans ItemsRepeater comme ils le font dans ListView et GridView.


Les interfaces suivantes sur un ItemsSource activant des fonctionnalités spéciales dans les contrôles ListView et GridView, mais actuellement n’ont aucun effet sur un ItemsRepeater :

- [ISupportIncrementalLoading](/uwp/api/windows.ui.xaml.data.isupportincrementalloading)
- [IItemsRangeInfo](/uwp/api/windows.ui.xaml.data.iitemsrangeinfo)
- [ISelectionInfo](/uwp/api/windows.ui.xaml.data.iselectioninfo)

> [!TIP]
> Partagez votre expérience. Faites-nous savoir ce que vous pensez sur le [projet GitHub de bibliothèque d’interface utilisateur Windows](https://github.com/Microsoft/microsoft-ui-xaml/issues). Envisagez d’ajouter vos idées sur les propositions existantes comme [#374](https://github.com/Microsoft/microsoft-ui-xaml/issues/374): Ajouter la prise en charge de chargement incrémentiel de ItemsRepeater.

Une autre approche pour charger vos données de façon incrémentielle que l’utilisateur fait défiler vers le haut ou vers le bas consiste à observer la position de la fenêtre d’affichage du ScrollViewer et charger des données plus l’étendue d’approche de la fenêtre d’affichage.

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

Éléments affichés par le [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) sont organisées par un [disposition](/uwp/api/microsoft.ui.xaml.controls.layout) objet qui gère le dimensionnement et le positionnement de ses éléments enfants. Lorsqu’il est utilisé avec un ItemsRepeater, l’objet de disposition permet la virtualisation de l’interface utilisateur. Les dispositions fournies sont [StackLayout](/uwp/api/microsoft.ui.xaml.controls.stacklayout) et [UniformGridLayout](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout). Par défaut, ItemsRepeater utilise un StackLayout avec une orientation verticale.

### <a name="stacklayout"></a>StackLayout

[StackLayout](/uwp/api/microsoft.ui.xaml.controls.stacklayout) réorganise les éléments sur une seule ligne, vous pouvez orienter horizontalement ou verticalement.

Vous pouvez définir le [espacement](/en-us/uwp/api/microsoft.ui.xaml.controls.stacklayout.spacing) propriété pour ajuster la quantité d’espace entre les éléments. Espacement est appliqué dans la direction de la mise en page [Orientation](/uwp/api/microsoft.ui.xaml.controls.stacklayout.orientation).

![Espacement de disposition de pile](images/stack-layout.png)

Cet exemple montre comment définir la propriété ItemsRepeater.Layout sur un StackLayout avec l’orientation horizontale et l’espacement de 8 pixels.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<muxc:ItemsRepeater ItemsSource="{x:Bind Items}" ItemTemplate="{StaticResource MyTemplate}">
    <muxc:ItemsRepeater.Layout>
        <muxc:StackLayout Orientation="Horizontal" Spacing="8"/>
    </muxc:ItemsRepeater.Layout>
</muxc:ItemsRepeater>
```

### <a name="uniformgridlayout"></a>UniformGridLayout

Le [UniformGridLayout](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout) positionne les éléments de manière séquentielle dans une disposition d’habillage. Les éléments sont présentés dans l’ordre de gauche à droite lorsque la [Orientation](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.orientation) est **Horizontal**et disposé de haut en bas lors de l’Orientation est **Vertical**. Chaque élément est égales.

![Espacement de disposition grille uniforme](images/uniform-grid-layout.png)

Le nombre d’éléments dans chaque ligne d’une disposition horizontale est influencé par la largeur de l’élément minimale. Le nombre d’éléments dans chaque colonne d’une disposition verticale est influencé par la hauteur d’élément minimale.

- Vous pouvez fournir explicitement une taille minimale à utiliser en définissant le [MinItemHeight](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minitemheight) et [MinItemWidth](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minitemwidth) propriétés.
- Si vous ne spécifiez pas une taille minimale, la taille mesurée du premier élément est considéré comme la taille minimale par élément.

Vous pouvez également définir un espacement minimal pour la mise en page à inclure entre les lignes et colonnes en définissant le [MinColumnSpacing](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.mincolumnspacing) et [MinRowSpacing](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minrowspacing) propriétés.

![Espacement et dimensionnement de grille uniforme](images/uniform-grid-sizing-spacing.png)

Une fois que le nombre si les éléments dans une ligne ou une colonne a été déterminé dépend de la taille minimale de l’élément et l’espacement, il peut exister d’espace inutilisé restés après le dernier élément dans la ligne ou colonne (comme illustré dans l’image précédente). Vous pouvez spécifier si tout espace supplémentaire est ignoré, utilisé pour augmenter la taille de chaque élément ou pour créer un espace supplémentaire entre les éléments. Cela est contrôlé par le [ItemsStretch](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsstretch) et [ItemsJustification](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsjustification) propriétés.

Vous pouvez définir le [ItemsStretch](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsstretch) propriété pour spécifier la façon dont la taille de l’élément est augmentée pour remplir l’espace inutilisé.

Cette liste affiche les valeurs disponibles. Les définitions de supposent la valeur par défaut **Orientation** de **Horizontal**.

- **Aucun** : Espace supplémentaire ne reste inutilisé à la fin de la ligne. Il s’agit de l’option par défaut.
- **Remplir**: Les éléments bénéficient d’une largeur supplémentaire à utiliser l’espace disponible (hauteur si vertical).
- **Uniform**: Les éléments sont données largeur supplémentaire à utiliser l’espace disponible et attribués à hauteur supplémentaire afin de conserver les proportions (hauteur et largeur commutent si vertical).

Cette image montre l’effet de la **ItemsStretch** valeurs dans une disposition horizontale.

![Stretch d’élément de grille uniforme](images/uniform-grid-item-stretch.png)

Lorsque **ItemsStretch** est **aucun**, vous pouvez définir le [ItemsJustification](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsjustification) propriété pour spécifier comment supplémentaire espace est utilisé pour aligner les éléments.

Cette liste affiche les valeurs disponibles. Les définitions de supposent la valeur par défaut **Orientation** de **Horizontal**.

- **Début** : Les éléments sont alignés avec le début de la ligne. Espace supplémentaire ne reste inutilisé à la fin de la ligne. Il s’agit de l’option par défaut.
- **Centre**: Dans le centre de la ligne, les éléments sont alignés. Espace restant est réparti uniformément au début et à la fin de la ligne.
- **Fin**: Les éléments sont alignés avec la fin de la ligne. Espace supplémentaire ne reste inutilisé au début de la ligne.
- **SpaceAround**: Les éléments sont répartis uniformément. Une quantité égale de l’espace est ajoutée avant et après chaque élément.
- **SpaceBetween**: Les éléments sont répartis uniformément. Une quantité égale de l’espace est ajoutée entre chaque élément. Aucun espace n’est ajouté au début et à la fin de la ligne.
- **SpaceEvenly**: Les éléments sont répartis uniformément avec une quantité égale de l’espace entre chaque élément et au début et à la fin de la ligne.

Cette image montre l’effet de la **ItemsStretch** valeurs dans une disposition verticale (appliqué aux colonnes au lieu de lignes).

![Justification d’élément de grille uniforme](images/uniform-grid-item-justification.png)

> [!TIP]
> Le **ItemsStretch** propriété affecte le _mesure_ passe de disposition. Le **ItemsJustification** propriété affecte le _réorganiser_ passe de disposition.

Cet exemple montre comment définir le **ItemsRepeater.Layout** propriété à un **UniformGridLayout**.

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

Lorsque vous hébergez des éléments dans un [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), vous devrez peut-être prendre des mesures quand un élément est affiché ou s’arrête affiché, par exemple comme démarrer un téléchargement asynchrone de certains contenus, associer l’élément à un mécanisme pour effectuer le suivi de la sélection, ou arrêter une tâche en arrière-plan.

Dans un contrôle de virtualisation, vous ne pouvez pas compter sur les événements de charger/décharger, car l’élément ne soit pas supprimé à partir de l’arborescence d’éléments visuels en direct lorsqu’elle est recyclée. Au lieu de cela, les autres événements sont fournis pour gérer le cycle de vie d’éléments. Ce diagramme illustre le cycle de vie d’un élément dans un ItemsRepeater, et lorsque les événements associés sont déclenchés.

![Diagramme des événements de cycle de vie](images/items-repeater-lifecycle.png)

- [**ElementPrepared** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementprepared) se produit chaque fois qu’un élément est prête pour utilisation. Cela se produit pour un élément qui vient d’être créé ainsi que sur un élément qui existe déjà et est en cours réutilisée à partir de la file d’attente de recyclage.
- [**ElementClearing** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementclearing) se produit immédiatement chaque fois qu’un élément a été envoyé à la file d’attente de recyclage, comme quand il se situe en dehors de la plage de réalisé des éléments.
- [**ElementIndexChanged** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementindexchanged
) se produit pour chaque réalisées UIElement dont l’index de l’élément qu’il représente a changé. Par exemple, lorsqu’un autre élément est ajouté ou supprimé dans la source de données, l’index pour les éléments qui suivent dans l’ordre recevoir cet événement.

Cet exemple montre comment vous pouvez utiliser ces événements à attacher un service de sélection personnalisée pour effectuer le suivi de la sélection d’éléments dans un contrôle personnalisé qui utilise ItemsRepeater pour afficher les éléments.

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

Lorsque vous effectuez des actions telles que le filtrage ou le tri de votre jeu de données, vous traditionnellement pourrez avoir comparé le précédent jeu de données pour les nouvelles données, puis émis des notifications de modification granulaire via [INotifyCollectionChanged](/uwp/api/windows.ui.xaml.interop.inotifycollectionchanged). Toutefois, il est souvent plus facile à complètement remplacer les anciennes données avec les nouvelles données et déclencher une notification de modification de collection à l’aide du [réinitialiser](/uwp/api/windows.ui.xaml.interop.notifycollectionchangedaction) action à la place.

En règle générale, une réinitialisation, le contrôle de version des éléments enfants existants et recommencer, création de l’interface utilisateur à partir du début à la position de défilement 0 car il connaît pas exactement comment les données ont changé au cours de la réinitialisation.

Toutefois, si la collection est affecté en tant que l’ItemsSource prend en charge les identificateurs uniques en implémentant la [IKeyIndexMapping](/uwp/api/microsoft.ui.xaml.controls.ikeyindexmapping) interface, puis le ItemsRepeater permet d’identifier rapidement :

- éléments d’interface utilisateur réutilisable pour les données qui existaient avant et après la réinitialisation
- précédemment les éléments visibles qui ont été supprimés
- nouveaux éléments ajoutés qui seront visibles

Cela permet la ItemsRepeater éviter recommencer à partir de la position de défilement 0. Elle lui permet également de restaurer rapidement les éléments d’interface utilisateur pour les données qui n’ont pas modifié dans une réinitialisation, ce qui entraîne de meilleures performances.

Cet exemple montre comment afficher une liste d’éléments dans une pile verticale où _MyItemsSource_ est une source de données personnalisée qui encapsule une liste d’éléments sous-jacente. Il expose une _données_ propriété qui peut être utilisée pour réattribuer une nouvelle liste à utiliser comme source d’éléments, qui déclenche ensuite une réinitialisation.

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

## <a name="create-a-custom-collection-control"></a>Créer un contrôle de la collection personnalisée

Vous pouvez utiliser la [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) pour créer un contrôle de collecte personnalisé avec son propre type de contrôle pour présenter chaque élément.

> [!NOTE]
> Cela ressemble à utiliser **ItemsControl**, mais au lieu de dériver à partir de **ItemsControl** et en plaçant un **ItemsPresenter** dans le modèle de contrôle, vous dérivez à partir de  **Contrôle** et insérez un **ItemsRepeater** dans le modèle de contrôle. Le contrôle de la collection personnalisée » a une « **ItemsRepeater** par rapport à » est un « **ItemsControl**. Cela implique que vous devez également explicitement choisir les propriétés à exposer, plutôt que laquelle des propriétés héritées aux prend ne pas en charge.

Cet exemple montre comment placer un [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) dans le modèle d’un contrôle personnalisé nommé _MediaCollectionView_ et exposer ses propriétés.

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

## <a name="display-grouped-items"></a>Afficher les éléments groupés

Vous pouvez imbriquer une [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) dans le [ItemTemplate](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) de ItemsRepeater une autre pour créer une prédiction imbriquée virtualisation des dispositions. Le framework fera une utilisation efficace des ressources en réduisant au minimum la réalisation inutile d’éléments qui ne sont pas visibles ou près de la fenêtre d’affichage actuel.

Cet exemple montre comment vous pouvez afficher une liste des éléments regroupés dans une pile verticale. Le ItemsRepeater externe génère chaque groupe. Dans le modèle pour chaque groupe, un autre ItemsRepeater génère les éléments.

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

Cet exemple montre une disposition pour une application qui a des catégories différentes qui peuvent changer à la préférence de l’utilisateur et sont présentées sous forme de défilement horizontale des listes, comme illustré ici.

![Disposition imbriquée avec éléments repeater](images/items-repeater-nested-layout.png)

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<ScrollViewer>
  <muxc:ItemsRepeater ItemsSource="{x:Bind Categories}"
                      Background="LightGreen">
    <muxc:ItemsRepeater.ItemTemplate>
      <DataTemplate x:DataType="local:Category">
        <StackPanel Margin="12,0">
          <TextBlock Text="{x:Bind Name}" Style="{ThemeResource TitleTextBlockStyle}"/>
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

## <a name="bringing-an-element-into-view"></a>Mettre un élément dans la vue

Framework de le XAML gère déjà l’apport d’un FrameworkElement dans l’affichage lorsqu’il reçoit le focus clavier (1) ou (2) reçoit le focus du Narrateur. Il existe peut-être d’autres cas où vous devez explicitement afficher un élément dans la vue. Par exemple, en réponse à une action utilisateur ou pour restaurer l’état de l’interface utilisateur après une navigation entre les pages.

Placer un élément virtualisé en vue implique les étapes suivantes :
1. Réaliser un UIElement pour un élément
2. Exécuter la mise en page pour vous assurer de l’élément a une position valide
3. Initier une demande à afficher l’élément réalisé

L’exemple ci-dessous illustre ces étapes dans le cadre de la restauration de la position de défilement d’un élément dans une liste plate, verticale après une navigation entre les pages. Dans le cas des données hiérarchiques à l’aide de ItemsRepeaters imbriquées, l’approche est essentiellement la même, mais doit être effectuée à chaque niveau de la hiérarchie.

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

## <a name="enable-accessibility"></a>Activer l’accessibilité

[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) ne fournit pas d’une expérience de l’accessibilité par défaut. La documentation sur [facilité d’utilisation pour les applications UWP](/windows/uwp/design/usability) fournit une mine d’informations pour vous assurer que votre application fournit une expérience utilisateur inclusif. Si vous utilisez le ItemsRepeater pour créer un contrôle personnalisé veillez à consultez la documentation sur [les homologues automation personnalisée](/windows/uwp/design/accessibility/custom-automation-peers).

### <a name="keyboarding"></a>Clavier
La prise en charge keyboarding minimale pour le déplacement de focus ItemsRepeater fournit repose sur du XAML [2D Navigation directionnelle pour Keyboarding](/windows/uwp/design/input/focus-navigation#2d-directional-navigation-for-keyboard).

![Navigation de direction](/windows/uwp/design/input/images/keyboard/directional-navigation.png)

Le ItemsRepeater [XYFocusKeyboardNavigation mode](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode) est _activé_ par défaut. En fonction de l’expérience prévu, vous pouvez envisager la prise en charge pour common [Interactions de clavier](/windows/uwp/design/input/keyboard-interactions) , accueil, fin, PG. préc et PageDown.

Le ItemsRepeater garantit automatiquement que l’ordre de tabulation par défaut pour ses éléments (qu’il soit virtualisé ou non) suit le même ordre que les éléments sont indiqués dans les données. Par défaut le ItemsRepeater a son [TabFocusNavigation](/uwp/api/windows.ui.xaml.uielement.tabfocusnavigation) propriété définie sur [une fois](/uwp/api/windows.ui.xaml.input.keyboardnavigationmode) au lieu de la valeur par défaut courantes _Local_.

> [!NOTE]
> Le ItemsRepeater ne retient pas automatiquement le dernier élément ayant le focus.  Cela signifie que lorsqu’un utilisateur est à l’aide de MAJ + Tab, il peuvent être dirigé vers la dernière réalisé élément.

### <a name="announcing-item-x-of-y-in-screen-readers"></a>Annonce de « élément _X_ de _Y_» dans les lecteurs d’écran

Vous devez gérer la définition des propriétés automation appropriées, telles que les valeurs pour **PositionInSet** et **SizeOfSet**et s’assurer qu’ils restent à jour lorsque des éléments sont ajoutés, déplacées, supprimées, etc.

Dans certaines dispositions personnalisées est peut-être une séquence évident pour l’ordre visuel.  Les utilisateurs attendent au minimum que les valeurs des propriétés PositionInSet et SizeOfSet utilisées par les lecteurs d’écran correspondra à l’ordre que les éléments s’affichent dans les données (décalage par 1 pour correspondre à naturelle de comptage et en cours à partir de 0).

La meilleure façon d’y parvenir consiste à avoir l’homologue automation pour le contrôle d’élément implémente le [GetPositionInSetCore](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpositioninsetcore) et [GetSizeOfSetCore](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getsizeofsetcore) méthodes et la position de l’élément dans le jeu de données de rapport représenté par le contrôle. La valeur est calculée uniquement au moment de l’exécution lors de l’accès par une technologie d’assistance et de mise à jour devient un problème. La valeur correspond à l’ordre des données.

Cet exemple montre comment vous pouvez le faire lors de la présentation d’un contrôle personnalisé appelé _CardControl_.

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
