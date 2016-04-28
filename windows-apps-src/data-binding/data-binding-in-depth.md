---
ms.assetid: 41E1B4F1-6CAF-4128-A61A-4E400B149011
title: Présentation détaillée de la liaison de données
description: La liaison de données est un moyen dont dispose l’interface utilisateur de votre application pour afficher des données et éventuellement rester synchronisée avec ces données.
---
# Présentation détaillée de la liaison de données

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


** API importantes **

-   [**Classe Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820)
-   [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713)
-   [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899)

**Remarque** Cette rubrique décrit en détail les fonctionnalités de liaison de données. Pour une brève présentation pratique, voir [Vue d’ensemble de la liaison de données](data-binding-quickstart.md).

 

La liaison de données est un moyen dont dispose l’interface utilisateur de votre application pour afficher des données et éventuellement rester synchronisée avec ces données. La liaison de données vous permet de séparer les problématiques liées aux données de celles liées à l’interface utilisateur, ce qui se traduit par un modèle conceptuel plus simple et l’amélioration de la lisibilité, de la testabilité et de la gestion de la maintenance de votre application.

Vous pouvez utiliser la liaison de données pour simplement afficher des valeurs à partir d’une source de données lorsque l’interface utilisateur est affichée pour la première fois, et non pas pour répondre aux modifications apportées à ces valeurs. Cette liaison, dite ponctuelle, est particulièrement adaptée aux données dont les valeurs ne changent pas au cours de l’exécution. Vous pouvez également choisir d’« observer » les valeurs et de mettre à jour l’interface utilisateur lorsque ces valeurs changent. Cette liaison, dite à sens unique, est particulièrement adaptée aux données en lecture seule. Enfin, vous pouvez choisir d’observer les valeurs et de mettre à jour l’interface utilisateur de telle sorte que les modifications apportées par l’utilisateur aux valeurs de l’interface utilisateur soient transmises automatiquement à la source de données. Cette liaison, dite bidirectionnelle, est particulièrement adaptée aux données en lecture-écriture. Voici quelques exemples.

-   Vous pouvez utiliser une liaison ponctuelle pour lier un contrôle [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) à une photo de l’utilisateur actuel.
-   Vous pouvez utiliser une liaison à sens unique pour lier un contrôle [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) à une collection d’articles d’actualité en temps réel regroupés par section de journal.
-   Vous pouvez utiliser une liaison bidirectionnelle pour lier un contrôle [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) au nom d’un client dans un formulaire.

Il existe deux types de liaison, qui sont généralement tous deux déclarés dans le balisage de l’interface utilisateur. Vous pouvez choisir d’utiliser l’[extension de balisage {x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) ou l’[extension de balisage {Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782). Vous pouvez même utiliser une combinaison des deux dans la même application, voire pour un même élément d’interface utilisateur. Nouveauté de Windows 10, {x:Bind} offre de meilleures performances. {Binding} propose plus de fonctionnalités. Toutes les informations présentées dans cette rubrique s’appliquent à ces deux types de liaison, sauf mention contraire explicite.

**Exemples d’applications illustrant {x:Bind}**

-   [Exemple avec {x:Bind}](http://go.microsoft.com/fwlink/p/?linkid=619989).
-   [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame).
-   [Exemple d’éléments de base d’une interface utilisateur XAML](http://go.microsoft.com/fwlink/p/?linkid=619992).

**Exemples d’applications illustrant {Binding}**

-   Téléchargez l’application [Bookstore1](http://go.microsoft.com/fwlink/?linkid=532950).
-   Téléchargez l’application [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952).

Chaque liaison implique les éléments suivants
------------------------------------

-   Une *source de liaison*. C’est la source de données de la liaison ; il peut s’agir d’une instance de n’importe quelle classe qui présente des membres dont vous souhaitez afficher les valeurs dans votre interface utilisateur.
-   Une *cible de liaison*. Il s’agit d’une propriété [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/BR242362) de l’élément [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706) de votre interface utilisateur qui affiche les données.
-   Un *objet de liaison*. C’est l’élément qui transfère les valeurs de données de la source à la cible, et éventuellement de la cible à la source. L’objet de liaison est créé au moment du chargement du XAML à partir de votre extension de balisage [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) ou [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782).

Dans les sections suivantes, nous allons examiner de plus près la source de liaison, la cible de liaison et l’objet de liaison. Nous relierons ensuite ces sections avec un exemple de liaison du contenu d’un bouton à une propriété de chaîne nommée **NextButtonText**, qui appartient à une classe nommée **HostViewModel**.

Source de liaison
--------------

Voici une implémentation très rudimentaire d’une classe que nous pourrions utiliser comme source de liaison.

**Remarque** Si vous utilisez [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) avec des extensions de composant Visual C++ (C++/CX), vous devrez ajouter l’attribut [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) à votre classe source de liaison. Si vous utilisez [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783), vous n’aurez pas besoin de cet attribut. Voir [Ajout d’un affichage de détails](data-binding-quickstart.md#adding-a-details-view) pour un extrait de code.

```csharp
public class HostViewModel
{
    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText { get; set; }
}
```

Cette implémentation de **HostViewModel** et sa propriété **NextButtonText** conviennent uniquement à une liaison ponctuelle. Toutefois, les liaisons à sens unique et bidirectionnelles sont très courantes, et avec ces types de liaison, l’interface utilisateur se met à jour automatiquement en réponse aux modifications apportées aux valeurs de données de la source de liaison. Pour que ces types de liaison fonctionnent correctement, vous devez rendre votre source de liaison « observable » par l’objet de liaison. Par conséquent, dans notre exemple, si nous voulons créer une liaison à sens unique ou bidirectionnelle à la propriété **NextButtonText**, toutes les modifications de la valeur de cette propriété qui surviennent au moment de l’exécution doivent être rendues observables par l’objet de liaison.

Une manière de le faire consiste à dériver d’un objet [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/BR242356) la classe qui représente votre source de liaison et à exposer une valeur de données par l’intermédiaire d’une propriété [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/BR242362). C’est de cette façon qu’un élément [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706) devient observable. Les éléments **FrameworkElements** constituent de bonnes sources de liaison sans aucune modification.

Une méthode moins lourde pour rendre une classe observable (méthode obligatoire pour les classes possédant déjà une classe de base) consiste à implémenter [**System.ComponentModel.INotifyPropertyChanged**](T:System.ComponentModel.INotifyPropertyChanged). Cela implique en fait simplement l’implémentation d’un événement unique nommé **PropertyChanged**. Vous trouverez ci-dessous un exemple utilisant **HostViewModel**.

**Remarque** Pour C++/CX, vous devez implémenter [**Windows::UI::Xaml::Data::INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899) et la classe source de liaison doit avoir l’attribut [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) ou implémenter [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878).

``` csharp
public class HostViewModel : INotifyPropertyChanged
{
    private string nextButtonText;

    public event PropertyChangedEventHandler PropertyChanged = delegate { };

    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText
    {
        get { return this.nextButtonText; }
        set
        {
            this.nextButtonText = value;
            this.OnPropertyChanged();
        }
    }

    public void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        // Raise the PropertyChanged event, passing the name of the property whose value has changed.
        this.PropertyChanged(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

La propriété **NextButtonText** est maintenant observable. Lorsque vous créez une liaison à sens unique ou bidirectionnelle à cette propriété (nous vous expliquerons comment ultérieurement), l’objet de liaison qui en résulte s’abonne à l’événement **PropertyChanged**. Lorsque cet événement est déclenché, le gestionnaire de l’objet de liaison reçoit un argument contenant le nom de la propriété qui a changé. Voilà comment l’objet de liaison connaît la propriété dont il doit lire à nouveau la valeur.

Pour ne pas avoir à implémenter plusieurs fois le modèle présenté ci-dessus, vous pouvez simplement dériver de la classe de base **BindableBase**, disponible dans l’exemple [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame) (dans le dossier « Common »). En voici un exemple :

``` csharp
public class HostViewModel : BindableBase
{
    private string nextButtonText;

    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText
    {
        get { return this.nextButtonText; }
        set { this.SetProperty(ref this.nextButtonText, value); }
    }
}
```

Le déclenchement de l’événement **PropertyChanged** à l’aide d’un argument [**String.Empty**](T:System.String) ou **null** indique que toutes les propriétés autres que les propriétés d’indexeur doivent être lues à nouveau. Vous pouvez déclencher l’événement pour indiquer que les propriétés d’indexeur de l’objet ont été modifiées en utilisant un argument « Item\[*indexer*\] » pour des indexeurs spécifiques (où *indexer* correspond à la valeur d’index) ou une valeur « Item\[\] » pour tous les indexeurs.

Une source de liaison peut être traitée comme un objet simple dont les propriétés contiennent des données ou comme une collection d’objets. Dans le code C# et Visual Basic, vous pouvez créer une liaison ponctuelle à un objet qui implémente [**List(Of T)**](T:System.Collections.Generic.List%601) pour afficher une collection qui ne change pas au moment de l’exécution. Pour une collection observable (permettant d’observer quand des éléments sont ajoutés à la collection et supprimés de celle-ci), créez une liaison à sens unique à [**ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601) à la place. Dans le code C++, vous pouvez établir une liaison à [**Vector&lt;T&gt;**](https://msdn.microsoft.com/en-us/library/dn858385.aspx) à la fois pour les collections observables et non observables. Pour lier vos propres classes de collection, utilisez les recommandations figurant dans le tableau suivant.

| Scénario                                                        | C# et VB (CLR)                                                                                                                                                                                                                                                                                                                                                                                                                   | C++/CX                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|-----------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Créer une liaison à un objet.                                              | Il peut s’agir de tout objet.                                                                                                                                                                                                                                                                                                                                                                                                                 | L’objet doit avoir [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) ou implémenter [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878).                                                                                                                                                                                                                                                                                                             |
| Obtenir les mises à jour des modifications de la propriété à partir d’un objet lié.                | L’objet doit implémenter [**System.ComponentModel. INotifyPropertyChanged**](T:System.ComponentModel.INotifyPropertyChanged).                                                                                                                                                                                                                                                                                                         | L’objet doit implémenter [**Windows.UI.Xaml.Data. INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899).                                                                                                                                                                                                                                                                                                                                                           |
| Lier à une collection.                                           | [**List(Of T)**](T:System.Collections.Generic.List%601)                                                                                                                                                                                                                                                                                                                                                                            | [**Platform::Collections::Vector&lt;T&gt;**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh441570.aspx)                                                                                                                                                                                                                                                                                                                                                                                         |
| Obtenir les mises à jour des modifications de la collection à partir d’une collection liée.          | [**ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601)                                                                                                                                                                                                                                                                                                                                        | [**Platform::Collections::Vector&lt;T&gt;**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh441570.aspx)                                                                                                                                                                                                                                                                                                                                                                                         |
| Implémenter une collection prenant en charge la liaison.                   | Étendre [**List(Of T)**](T:System.Collections.Generic.List%601) ou implémenter [**IList**](T:System.Collections.IList), [**IList**](T:System.Collections.Generic.IList%601)(de [**Object**](T:System.Object)), [**IEnumerable**](T:System.Collections.IEnumerable) ou [**IEnumerable**](T:System.Collections.Generic.IEnumerable%601)(de **Object**). La liaison à des éléments **IList(Of T)** et **IEnumerable(Of T)** génériques n’est pas prise en charge. | Implémenter [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979), [**IBindableIterable**](https://msdn.microsoft.com/library/windows/apps/Hh701957), [**IVector**](https://msdn.microsoft.com/library/windows/apps/BR206631)&lt;[**Object**](T:System.Object)^&gt;, [**IIterable**](https://msdn.microsoft.com/library/windows/apps/BR226024)&lt;**Object**^&gt;, **IVector**&lt;[**IInspectable**](https://msdn.microsoft.com/library/BR205821)\*&gt; ou **IIterable**&lt;**IInspectable**\*&gt;. La liaison à des éléments génériques **IVector&lt;T&gt;** et **IIterable&lt;T&gt;** n’est pas prise en charge. |
| Implémenter une collection qui prend en charge les mises à jour des modifications de la collection. | Étendre [**ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601) ou implémenter des éléments [**IList**](T:System.Collections.IList) et [**INotifyCollectionChanged**](T:System.Collections.Specialized.INotifyCollectionChanged) (non génériques).                                                                                                                                                               | Implémenter [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979) et [**IBindableObservableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701974).                                                                                                                                                                                                                                                                                                                       |
| Implémenter une collection prenant en charge un chargement incrémentiel.       | Étendre [**ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601) ou implémenter des éléments [**IList**](T:System.Collections.IList) et [**INotifyCollectionChanged**](T:System.Collections.Specialized.INotifyCollectionChanged) (non génériques). En outre, implémenter [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916).                                                          | Implémenter [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979), [**IBindableObservableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701974) et [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916).                                                                                                                                                                                                                                         |

 
Vous pouvez lier des contrôles de listes à des sources de données très importantes et parvenir toutefois à atteindre un niveau de performance élevé à l’aide du chargement incrémentiel. Vous pouvez par exemple lier les contrôles de listes aux résultats de la requête d’image dans Bing sans avoir à charger tous les résultats simultanément. Vous pouvez charger une partie des résultats immédiatement, puis charger d’autres résultats si nécessaire. Pour prendre en charge le chargement incrémentiel, vous devez implémenter [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916) sur une source de données qui prend en charge la notification de modification apportée à la collection. Lorsque le moteur de liaison de données demande davantage de données, votre source de données doit effectuer les demandes appropriées, intégrer les résultats, puis envoyer les notifications appropriées afin de mettre à jour l’interface utilisateur.

Cible de liaison
--------------

Dans les deux exemples ci-dessous, la propriété **Button.Content** est la cible de liaison et sa valeur est définie sur une extension de balisage qui déclare l’objet de liaison. L’extension [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) est illustrée en premier, puis [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782). Déclarer les liaisons dans le balisage de liaisons dans le balisage est le scénario le plus courant (cela s’avère pratique, lisible et offre une compatibilité avec les outils). Cependant, vous pouvez éviter le balisage et créer de façon impérative (par programme) une instance de la classe [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820) à la place si nécessaire.

<!-- XAML lang specifier not yet supported in OP. Using XML for now. -->
``` xml
<Button Content="{x:Bind ...}" ... /> 
```

``` xml
<Button Content="{Binding ...}" ... /> 
```

Binding object declared using {x:Bind}
--------------------------------------

There's one step we need to do before we author our [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) markup. We need to expose our binding source class from the class that represents our page of markup. We do that by adding a property (of type **HostViewModel** in this case) to our **HostView** page class.

``` csharp
namespace QuizGame.View
{
    public sealed partial class HostView : Page
    {
        public HostView()
        {
            this.InitializeComponent();
            this.ViewModel = new HostViewModel();
        }
    
        public HostViewModel ViewModel { get; set; }
    }
}
``` Nous pouvons alors examiner de plus près le balisage qui déclare l’objet de liaison. L’exemple ci-dessous utilise la même cible de liaison **Button.Content** que dans la section « Cible de liaison » précédemment et montre qu’elle est liée à la propriété **HostViewModel.NextButtonText**.

``` xml
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

Notice the value that we specify for **Path**. This value is interpreted in the context of the page itself, and in this case the path begins by referencing the **ViewModel** property that we just added to the **HostView** page. That property returns a **HostViewModel** instance, and so we can dot into that object to access the **HostViewModel.NextButtonText** property. And we specify **Mode**, to override the [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) default of one-time.

The [**Path**](https://msdn.microsoft.com/library/windows/apps/BR209820-path) property supports a variety of syntax options for binding to nested properties, attached properties, and integer and string indexers. For more info, see [Property-path syntax](https://msdn.microsoft.com/library/windows/apps/Mt185586). Binding to string indexers gives you the effect of binding to dynamic properties without having to implement [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878). For other settings, see [{x:Bind} markup extension](https://msdn.microsoft.com/library/windows/apps/Mt204783).

**Note**  Changes to [**TextBox.Text**](https://msdn.microsoft.com/library/windows/apps/BR209683-text) are sent to a two-way bound source when the [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) loses focus, and not after every user keystroke.

**DataTemplate and x:DataType**

Inside a [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348) (whether used as an item template, a content template, or a header template), the value of **Path** is not interpreted in the context of the page, but in the context of the data object being templated. So that its bindings can be validated (and efficient code generated for them) at compile-time, a **DataTemplate** needs to declare the type of its data object using **x:DataType**. The example given below could be used as the **ItemTemplate** of an items control bound to a collection of **SampleDataGroup** objects.

``` xml
<DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

**Weakly-typed objects in your Path**

Consider for example that you have a type named SampleDataGroup, which implements a string property named Title. And you have a property MainPage.SampleDataGroupAsObject, which is of type object but which actually returns an instance of SampleDataGroup. The binding `<TextBlock Text="{x:Bind SampleDataGroupAsObject.Title}"/>` will result in a compile error because the Title property is not found on the type object. The remedy for this is to add a cast to your Path syntax like this: `<TextBlock Text="{x:Bind SampleDataGroupAsObject.(data:SampleDataGroup.Title)}"/>`. Here's another example where Element is declared as object but is actually a TextBlock: `<TextBlock Text="{x:Bind Element.Text}"/>`. And a cast remedies the issue: `<TextBlock Text="{x:Bind Element.(TextBlock.Text)}"/>`.

**En cas de chargement asynchrone des données** Le code pour prendre en charge **{x:Bind}** est généré au moment de la compilation dans les classes partielles pour vos pages. Ces fichiers se trouvent dans votre dossier `obj`, et portent des noms tels que (pour C#) `<view name>.g.cs`. The generated code includes a handler for your page's [**Loading**](https://msdn.microsoft.com/library/windows/apps/BR208706-loading) event, and that handler calls the **Initialize** method on a generated class that represent's your page's bindings. **Initialize** in turn calls **Update** to begin moving data between the binding source and the target. **Loading** is raised just before the first measure pass of the page or user control. So if your data is loaded asynchronously it may not be ready by the time **Initialize** is called. So, after you've loaded data, you can force one-time bindings to be initialized by calling `this->Bindings->Update();`. Si vous avez uniquement besoin des liaisons uniques pour les données chargées de manière asynchrone, il est préférable de les initialiser de cette manière plutôt que d’utiliser des liaisons à sens unique et d’écouter les modifications. Si vos données ne subissent pas de modifications affinées et si elles sont susceptibles d’être mises à jour dans le cadre d’une action spécifique, vous pouvez rendre vos liaisons uniques et forcer une mise à jour manuelle à tout moment avec un appel à **Update**.

**Limitations** 

**{x:Bind}** n’est pas adapté aux scénarios tardifs, tels que la navigation dans la structure du dictionnaire d’un objet JSON, ni le « duck typing » (typage canard) qui est une forme faible de typage basé sur les correspondances lexicales des noms de propriétés (« si ça ressemble à un canard, si ça nage comme un canard et si ça cancane comme un canard, c’est qu’il s’agit sans doute d’un canard »). Avec le « duck typing », une liaison à la propriété Age peut aussi bien être satisfaite par un objet Person que par un objet Wine. Pour ces scénarios, utilisez **{Binding}**.

Objet de liaison déclaré à l’aide de {Binding} ---------------------------------------

[{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) suppose, par défaut, que vous créiez une liaison à la propriété [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) de votre page de balisage. Nous allons donc définir la propriété **DataContext** de notre page en tant qu’instance de notre classe de source de liaison (de type **HostViewModel** dans le cas présent). L’exemple ci-dessous illustre le balisage qui déclare l’objet de liaison. Nous utilisons la même cible de liaison **Button.Content** que dans la section « Cible de liaison » précédemment et nous la lions à la propriété **HostViewModel.NextButtonText**.

``` xml
<Page xmlns:viewmodel="using:QuizGame.ViewModel" ... >
    <Page.DataContext>
        <viewmodel:HostViewModel/>
    </Page.DataContext>
    ...
    <Button Content="{Binding Path=NextButtonText}" ... />
</Page>
```

Notice the value that we specify for **Path**. This value is interpreted in the context of the page's [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713), which in this example is set to an instance of **HostViewModel**. The path references the **HostViewModel.NextButtonText** property. We can omit **Mode**, because the [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) default of one-way works here.

The default value of [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) for a UI element is the inherited value of its parent. You can of course override that default by setting **DataContext** explicitly, which is in turn inherited by children by default. Setting **DataContext** explicitly on an element is useful when you want to have multiple bindings that use the same source.

A binding object has a **Source** property, which defaults to the [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) of the UI element on which the binding is declared. You can override this default by setting **Source**, **RelativeSource**, or **ElementName** explicitly on the binding (see [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) for details).

Inside a [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348), the [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) is set to the data object being templated. The example given below could be used as the **ItemTemplate** of an items control bound to a collection of any type that has string properties named **Title** and **Description**.

``` xml
<DataTemplate x:Key="SimpleItemTemplate">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{Binding Title}"/>
      <TextBlock Text="{Binding Description"/>
    </StackPanel>
  </DataTemplate>
```

**Note**  By default, changes to [**TextBox.Text**](https://msdn.microsoft.com/library/windows/apps/BR209683-text) are sent to a two-way bound source when the [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) loses focus. To cause changes to be sent after every user keystroke, set **UpdateSourceTrigger** to **PropertyChanged** on the binding in markup. You can also completely take control of when changes are sent to the source by setting **UpdateSourceTrigger** to **Explicit**. You then handle events on the text box (typically [**TextBox.TextChanged**](https://msdn.microsoft.com/library/windows/apps/BR209683-textchanged)), call [**GetBindingExpression**](https://msdn.microsoft.com/library/windows/apps/BR208706-getbindingexpression) on the target to get a [**BindingExpression**](https://msdn.microsoft.com/library/windows/apps/BR209820expression) object, and finally call [**BindingExpression.UpdateSource**](https://msdn.microsoft.com/library/windows/apps/BR209820expression-updatesource) to programmatically update the data source.

The [**Path**](https://msdn.microsoft.com/library/windows/apps/BR209820-path) property supports a variety of syntax options for binding to nested properties, attached properties, and integer and string indexers. For more info, see [Property-path syntax](https://msdn.microsoft.com/library/windows/apps/Mt185586). Binding to string indexers gives you the effect of binding to dynamic properties without having to implement [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878). The [**ElementName**](https://msdn.microsoft.com/library/windows/apps/BR209820-elementname) property is useful for element-to-element binding. The [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/BR209820-relativesource) property has several uses, one of which is as a more powerful alternative to template binding inside a [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/BR209391). For other settings, see [{Binding} markup extension](https://msdn.microsoft.com/library/windows/apps/Mt204782) and the [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820) class.

What if the source and the target are not the same type?
--------------------------------------------------------

If you want to control the visibility of a UI element based on the value of a boolean property, or if you want to render a UI element with a color that's a function of a numeric value's range or trend, or if you want to display a date and/or time value in a UI element property that expects a string, then you'll need to convert values from one type to another. There will be cases where the right solution is to expose another property of the right type from your binding source class, and keep the conversion logic encapsulated and testable there. But that isn't flexible nor scalable when you have large numbers, or large combinations, of source and target properties. In that case you'll want to use something known as a value converter. This section describes how to implement and consume a value converter.

Here's a value converter, suitable for a one-time or a one-way binding, that converts a [**DateTime**](T:System.DateTime) value to a string value containing the month. The class implements [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/BR209903).

``` csharp
public class DateToStringConverter : IValueConverter
{
    // Define the Convert method to convert a DateTime value to 
    // a month string.
    public object Convert(object value, Type targetType, 
        object parameter, string language)
    {
        // value is the data from the source object.
        DateTime thisdate = (DateTime)value;
        int monthnum = thisdate.Month;
        string month;
        switch (monthnum)
        {
            case 1:
                month = "January";
                break;
            case 2:
                month = "February";
                break;
            default:
                month = "Month not found";
                break;
        }
        // Return the value to pass to the target.
        return month;
    }

    // ConvertBack is not implemented for a OneWay binding.
    public object ConvertBack(object value, Type targetType, 
        object parameter, string language)
    {
        throw new NotImplementedException();
    }
}
```

``` vbnet
Public Class DateToStringConverter
    Implements IValueConverter

    ' Define the Convert method to change a DateTime object to
    ' a month string.
    Public Function Convert(ByVal value As Object, -
        ByVal targetType As Type, ByVal parameter As Object, -
        ByVal language As String) As Object -
        Implements IValueConverter.Convert

        ' value is the data from the source object.
        Dim thisdate As DateTime = CType(value, DateTime)
        Dim monthnum As Integer = thisdate.Month
        Dim month As String
        Select Case (monthnum)
            Case 1
                month = "January"
            Case 2
                month = "February"
            Case Else
                month = "Month not found"
        End Select
        ' Return the value to pass to the target.
        Return month

    End Function

    ' ConvertBack is not implemented for a OneWay binding.
    Public Function ConvertBack(ByVal value As Object, -
        ByVal targetType As Type, ByVal parameter As Object, -
        ByVal language As String) As Object -
        Implements IValueConverter.ConvertBack

        Throw New NotImplementedException

    End Function
End Class
``` Et voici comment ce convertisseur est utilisé dans votre balisage d’objet de liaison.

``` xml
<UserControl.Resources>
  <local:DateToStringConverter x:Key="Converter1"/>
</UserControl.Resources>

...

<TextBlock Grid.Column="0" 
  Text="{x:Bind ViewModel.Month, Converter={StaticResource Converter1}}"/>

<TextBlock Grid.Column="0" 
  Text="{Binding Month, Converter={StaticResource Converter1}}"/>
```

The binding engine calls the [**Convert**](https://msdn.microsoft.com/library/windows/apps/BR209903-convert) and [**ConvertBack**](https://msdn.microsoft.com/library/windows/apps/BR209903-convertback) methods if the [**Converter**](https://msdn.microsoft.com/library/windows/apps/BR209820-converter) parameter is defined for the binding. When data is passed from the source, the binding engine calls **Convert** and passes the returned data to the target. When data is passed from the target (for a two-way binding), the binding engine calls **ConvertBack** and passes the returned data to the source.

The converter also has optional parameters: [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/BR209820-converterlanguage), which allows specifying the language to be used in the conversion, and [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/BR209820-converterparameter), which allows passing a parameter for the conversion logic. For an example that uses a converter parameter, see [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/BR209903).

**Note**  If there is an error in the conversion, do not throw an exception. Instead, return [**DependencyProperty.UnsetValue**](https://msdn.microsoft.com/library/windows/apps/BR242362-unsetvalue), which will stop the data transfer.

To display a default value to use whenever the binding source cannot be resolved, set the **FallbackValue** property on the binding object in markup. This is useful to handle conversion and formatting errors. It is also useful to bind to source properties that might not exist on all objects in a bound collection of heterogeneous types.

If you bind a text control to a value that is not a string, the data binding engine will convert the value to a string. If the value is a reference type, the data binding engine will retrieve the string value by calling [**ICustomPropertyProvider.GetStringRepresentation**](https://msdn.microsoft.com/library/windows/apps/BR209878-getstringrepresentation) or [**IStringable.ToString**](https://msdn.microsoft.com/library/Dn302136) if available, and will otherwise call [**Object.ToString**](M:System.Object.ToString). Note, however, that the binding engine will ignore any **ToString** implementation that hides the base-class implementation. Subclass implementations should override the base class **ToString** method instead. Similarly, in native languages, all managed objects appear to implement [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878) and [**IStringable**](https://msdn.microsoft.com/library/Dn302135). However, all calls to **GetStringRepresentation** and **IStringable.ToString** are routed to **Object.ToString** or an override of that method, and never to a new **ToString** implementation that hides the base-class implementation.

Resource dictionaries with {x:Bind}
-----------------------------------

The [{x:Bind} markup extension](https://msdn.microsoft.com/library/windows/apps/Mt204783) depends on code generation, so it needs a code-behind file containing a constructor that calls **InitializeComponent** (to initialize the generated code). You re-use the resource dictionary by instantiating its type (so that **InitializeComponent** is called) instead of referencing its filename. Here's an example of what to do if you have an existing resource dictionary and you want to use {x:Bind} in it.

TemplatesResourceDictionary.xaml

``` xml
<ResourceDictionary
    x:Class="ExampleNamespace.TemplatesResourceDictionary"
    .....
    xmlns:examplenamespace="using:ExampleNamespace">
    
    <DataTemplate x:Key="EmployeeTemplate" x:DataType="examplenamespace:IEmployee">
        <Grid>
            <TextBlock Text="{x:Bind Name}"/>
        </Grid>
    </DataTemplate>
</ResourceDictionary>
```

TemplatesResourceDictionary.xaml.cs

``` csharp
using Windows.UI.Xaml.Data;
 
namespace ExampleNamespace
{
    public partial class TemplatesResourceDictionary
    {
        public TemplatesResourceDictionary()
        {
            InitializeComponent();
        }
    }
}
``` MainPage.xaml ``` xml
<Page x:Class="ExampleNamespace.MainPage"
    ....
    xmlns:examplenamespace="using:ExampleNamespace">

    <Page.Resources>
        <ResourceDictionary>
            .... 
            <ResourceDictionary.MergedDictionaries>
                <examplenamespace:TemplatesResourceDictionary/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Page.Resources>
</Page>
```

Event binding and ICommand
--------------------------

[{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) supports a feature called event binding. With this feature, you can specify the handler for an event using a binding, which is an additional option on top of handling events with a method on the code-behind file. Let's say you have a **RootFrame** property on your **MainPage** class.

``` csharp
    public sealed partial class MainPage : Page
    {
        ....    
        public Frame RootFrame { get { return Window.Current.Content as Frame; } }
    }
``` Vous pouvez alors lier l’événement **Click** d’un bouton à une méthode sur l’objet **Frame** renvoyé par la propriété **RootFrame** comme suit. Notez que nous avons également lié la propriété **IsEnabled** du bouton à un autre membre du même élément **Frame**.

``` xml
    <AppBarButton Icon="Forward" IsCompact="True"
    IsEnabled="{x:Bind RootFrame.CanGoForward, Mode=OneWay}"
    Click="{x:Bind RootFrame.GoForward}"/>
```

Overloaded methods cannot be used to handle an event with this technique. Also, if the method that handles the event has parameters then they must all be assignable from the types of all of the event's parameters, respectively. In this case, [**Frame.GoForward**](https://msdn.microsoft.com/library/windows/apps/BR242693) is not overloaded and it has no parameters (but it would still be valid even if it took two **object** parameters). [**Frame.GoBack**](https://msdn.microsoft.com/library/windows/apps/Dn996568) is overloaded, though, so we can't use that method with this technique.

The event binding technique is similar to implementing and consuming commands (a command is a property that returns an object that implements the [**ICommand**](T:System.Windows.Input.ICommand) interface). Both [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) and [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) work with commands. So that you don't have to implement the command pattern multiple times, you can use the **DelegateCommand** helper class that you'll find in the [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame) sample (in the "Common" folder).

## Binding to a collection of folders or files

You can use the APIs in the [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346) namespace to retrieve folder and file data. However, the various **GetFilesAsync**, **GetFoldersAsync**, and **GetItemsAsync** methods do not return values that are suitable for binding to list controls. Instead, you must bind to the return values of the [**GetVirtualizedFilesVector**](https://msdn.microsoft.com/library/windows/apps/Hh701422), [**GetVirtualizedFoldersVector**](https://msdn.microsoft.com/library/windows/apps/Hh701428), and [**GetVirtualizedItemsVector**](https://msdn.microsoft.com/library/windows/apps/Hh701430) methods of the [**FileInformationFactory**](https://msdn.microsoft.com/library/windows/apps/BR207501) class. The following code example from the [StorageDataSource and GetVirtualizedFilesVector sample](http://go.microsoft.com/fwlink/p/?linkid=228621) shows the typical usage pattern. Remember to declare the **picturesLibrary** capability in your app package manifest, and confirm that there are pictures in your Pictures library folder.

``` csharp
        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var library = Windows.Storage.KnownFolders.PicturesLibrary;
            var queryOptions = new Windows.Storage.Search.QueryOptions();
            queryOptions.FolderDepth = Windows.Storage.Search.FolderDepth.Deep;
            queryOptions.IndexerOption = Windows.Storage.Search.IndexerOption.UseIndexerWhenAvailable;

            var fileQuery = library.CreateFileQueryWithOptions(queryOptions);

            var fif = new Windows.Storage.BulkAccess.FileInformationFactory(
                fileQuery,
                Windows.Storage.FileProperties.ThumbnailMode.PicturesView,
                190,
                Windows.Storage.FileProperties.ThumbnailOptions.UseCurrentScale,
                false
                );

            var dataSource = fif.GetVirtualizedFilesVector();
            this.PicturesListView.ItemsSource = dataSource;
        }
``` Généralement, vous utiliserez cette approche pour créer un affichage en lecture seule des informations sur le fichier et le dossier. Vous pouvez créer des liaisons bidirectionnelles aux propriétés de fichiers et de dossiers, par exemple pour permettre aux utilisateurs de noter une chanson dans un affichage de musique. Toutefois, les modifications apportées ne sont pas conservées tant que vous n’avez pas appelé la méthode **SavePropertiesAsync** appropriée (par exemple, [**MusicProperties.SavePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/BR207760)). Vous devez valider les modifications lorsque l’élément ne correspond plus à la zone active afin de ne pas déclencher la réinitialisation de la sélection.

Notez que la liaison bidirectionnelle utilisant cette technique ne fonctionne qu’avec les emplacements indexés, tels que Musique. Vous pouvez déterminer si un emplacement est indexé en appelant la méthode [**FolderInformation.GetIndexedStateAsync**](https://msdn.microsoft.com/library/windows/apps/BR207627).

Notez également qu’un vecteur virtualisé peut retourner **null** pour certains éléments avant qu’il ne renseigne leur valeur. Par exemple, vous devriez rechercher la valeur **null** avant d’utiliser la valeur [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/BR209770) d’un contrôle de liste lié à un vecteur virtualisé, ou utiliser [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/BR209768) à la place.

Liaison de données groupées en fonction d’une clé -------------------------------- Si vous prenez une collection plate d’éléments (des ouvrages, par exemple, représentés par une classe **BookSku**) et que vous groupez les éléments en utilisant une propriété commune en tant que clé (la propriété **BookSku.AuthorName**, par exemple), vous obtenez ce qu’on appelle des données groupées. Lorsque vous groupez les données, la collection n’est plus plate. Les données groupées sont une collection d’objets de groupe, où chaque objet de groupe possède a) une clé et b) une collection d’éléments dont la propriété correspond à cette clé. Pour reprendre l’exemple des ouvrages, le regroupement par nom d’auteur a pour résultat une collection de groupes où chaque groupe possède a) une clé, qui est le nom d’un auteur, et b) une collection de **BookSku** dont la propriété **AuthorName** correspond à la clé du groupe.

En règle générale, pour afficher une collection, vous liez la propriété [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/BR242828) d’un contrôle d’éléments (tel que [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) ou [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705)) directement à une propriété qui renvoie une collection. S’il s’agit d’une collection plate d’éléments, vous n’avez pas besoin d’effectuer d’opération particulière. Mais s’il s’agit d’une collection d’objets de groupe (comme c’est le cas pour une liaison à des données groupées), vous avez besoin des services d’un objet intermédiaire appelé [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833), qui se trouve entre le contrôle d’éléments et la source de liaison. Vous liez l’objet **CollectionViewSource** à la propriété qui renvoie les données groupées et vous liez le contrôle d’éléments à l’objet **CollectionViewSource**. L’objet **CollectionViewSource** a pour autre avantage d’assurer le suivi de l’élément actif, de sorte que vous pouvez synchroniser plusieurs contrôles d’éléments en permanence en les liant tous au même objet **CollectionViewSource**. Vous pouvez également accéder à l’élément actif par programme via la propriété [**ICollectionView.CurrentItem**](https://msdn.microsoft.com/library/windows/apps/BR209857) de l’objet renvoyé par la propriété [**CollectionViewSource.View**](https://msdn.microsoft.com/library/windows/apps/BR209833-view).

Pour activer la fonctionnalité de regroupement d’un objet [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833), définissez [**IsSourceGrouped**](https://msdn.microsoft.com/library/windows/apps/BR209833-issourcegrouped) sur **true**. La nécessité de définir également la propriété [**ItemsPath**](https://msdn.microsoft.com/library/windows/apps/BR209833-itemspath) dépend de la manière dont vous créez vos objets de groupe. Vous pouvez créer un objet de groupe de deux manières : à l’aide d’un modèle « is-a-group » ou d’un modèle « has-a-group ». Dans le modèle « is-a-group », l’objet de groupe dérive d’un type de collection (par exemple, **List&lt;T&gt;**) et correspond donc en fait au groupe d’éléments. Avec ce modèle, vous n’avez pas besoin de définir **ItemsPath**. Dans le modèle « has-a-group », l’objet de groupe comporte une ou plusieurs propriétés d’un type de collection (par exemple, **List&lt;T&gt;**), de sorte que le groupe comporte un groupe d’éléments sous la forme d’une propriété (ou plusieurs groupes d’éléments sous la forme de plusieurs propriétés). Avec ce modèle, vous devez définir **ItemsPath** sur le nom de la propriété qui contient le groupe d’éléments.

L’exemple suivant illustre le modèle « has-a-group ». La classe de page comporte une propriété nommée [**ViewModel**](https://msdn.microsoft.com/library/windows/apps/BR208713), qui renvoie une instance de notre modèle d’affichage. L’objet [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) se lie à la propriété **Authors** du modèle d’affichage (**Authors** est la collection d’objets de groupe) et indique que c’est la propriété **Author.BookSkus** qui contient les éléments groupés. Enfin, la classe [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) est liée à l’objet **CollectionViewSource** et son style de groupe est défini de manière à pouvoir afficher les éléments en groupes.

``` csharp
    <Page.Resources>
        <CollectionViewSource
        x:Name="AuthorHasACollectionOfBookSku"
        Source="{x:Bind ViewModel.Authors}"
        IsSourceGrouped="true"
        ItemsPath="BookSkus"/>
    </Page.Resources>
    ...

    <GridView
    ItemsSource="{Binding Source={StaticResource AuthorHasACollectionOfBookSku}}" ...>
        <GridView.GroupStyle>
            <GroupStyle
                HeaderTemplate="{StaticResource AuthorGroupHeaderTemplateWide}" ... />
        </GridView.GroupStyle>
    </GridView>
```

Note that the [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/BR242828) must use [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) (and not [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783)) because it needs to set the **Source** property to a resource. To see the above example in the context of the complete app, download the [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) sample app. Unlike the markup shown above, [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) uses {Binding} exclusively.

You can implement the "is-a-group" pattern in one of two ways. One way is to author your own group class. Derive the class from **List&lt;T&gt;** (where *T* is the type of the items). For example, `public class Author : List<BookSku>`. La deuxième consiste à utiliser une expression [LINQ](http://msdn.microsoft.com/library/bb397926.aspx) afin de créer dynamiquement des objets de groupe (et une classe de groupe) à partir des valeurs de propriétés similaires des éléments **BookSku**. Cette approche, consistant à conserver simplement une liste plate d’éléments et à les regrouper à la volée, est courante pour les applications qui accèdent aux données à partir d’un service cloud. Elle vous offre la possibilité de regrouper les ouvrages par auteur et par genre (par exemple) sans avoir à recourir à des classes de groupes spécifiques, comme **Author** et **Genre**.

L’exemple suivant illustre le modèle « is-a-group » avec [LINQ](http://msdn.microsoft.com/library/bb397926.aspx). Cette fois-ci, nous regroupons les ouvrages par genre, avec le nom du genre affiché dans les en-têtes des groupes. Cela est indiqué par le chemin de la propriété « Key » en référence à la valeur du groupe [**Key**](P:System.Linq.IGrouping%602.Key).

``` csharp
    using System.Linq;

    ...

    private IOrderedEnumerable<IGrouping<string, BookSku>> genres; public IOrderedEnumerable<IGrouping<string, BookSku>> Genres
    {
        get
        {
            si (this.genres == null)
            {
                this.genres = depuis l’ouvrage dans this.bookSkus
                grouper l’ouvrage par book.genre dans grp
                orderby grp.Key sélectionner grp;
            }
            renvoie this.genres;
        }
    } ```

Remember that when using [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) with data templates we need to indicate the type being bound to by setting an **x:DataType** value. If the type is generic then we can't express that in markup so we need to use [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) instead in the group style header template.

``` xml
    <Grid.Resources>
        <CollectionViewSource x:Name="GenreIsACollectionOfBookSku"
        Source="{Binding Genres}"
        IsSourceGrouped="true"/>
    </Grid.Resources>
    <GridView ItemsSource="{Binding Source={StaticResource GenreIsACollectionOfBookSku}}">
        <GridView.ItemTemplate x:DataType="local:BookTemplate">
            <DataTemplate>
                <TextBlock Text="{x:Bind Title}"/>
            </DataTemplate>
        </GridView.ItemTemplate>
        <GridView.GroupStyle>
            <GroupStyle>
                <GroupStyle.HeaderTemplate>
                    <DataTemplate>
                        <TextBlock Text="{Binding Key}"/>
                    </DataTemplate>
                </GroupStyle.HeaderTemplate>
            </GroupStyle>
        </GridView.GroupStyle>
    </GridView>
```

A [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/Hh702601) control is a great way for your users to view and navigate grouped data. The [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) sample app illustrates how to use the **SemanticZoom**. In that app, you can view a list of books grouped by author (the zoomed-in view) or you can zoom out to see a jump list of authors (the zoomed-out view). The jump list affords much quicker navigation than scrolling through the list of books. The zoomed-in and zoomed-out views are actually **ListView** or **GridView** controls bound to the same **CollectionViewSource**.

![An illustration of a SemanticZoom](images/sezo.png)

When you bind to hierarchical data—such as subcategories within categories—you can choose to display the hierarchical levels in your UI with a series of items controls. A selection in one items control determines the contents of subsequent items controls. You can keep the lists synchronized by binding each list to its own [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) and binding the **CollectionViewSource** instances together in a chain. This is called a master/details (or list/details) view. For more info, see [How to bind to hierarchical data and create a master/details view](how-to-bind-to-hierarchical-data-and-create-a-master-details-view.md).

Diagnosing and debugging data binding problems
-----------------------------------------------

Your binding markup contains the names of properties (and, for C#, sometimes fields and methods). So when you rename a property, you'll also need to change any binding that references it. Forgetting to do that leads to a typical example of a data binding bug, and your app either won't compile or won't run correctly.

The binding objects created by [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) and [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) are largely functionally equivalent. But {x:Bind} has type information for the binding source, and it generates source code at compile-time. With {x:Bind} you get the same kind of problem detection that you get with the rest of your code. That includes compile-time validation of your binding expressions, and debugging by setting breakpoints in the source code generated as the partial class for your page. These classes can be found in the files in your `obj` folder, with names like (for C#) `<view name>.g.cs`). Si vous rencontrez un problème avec une liaison, activez l’option **Arrêter sur les exceptions non gérées** dans le débogueur Microsoft Visual Studio. Le débogueur interrompra alors l’exécution et vous pourrez déboguer le problème en question. Le code généré par {x:Bind} suit le même modèle pour chaque partie du graphique des nœuds de la source de liaison et vous pouvez utiliser les informations de la fenêtre **Pile des appels** pour vous aider à déterminer la séquence d’appels qui a donné lieu au problème.

[
            {Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) ne comporte pas d’informations de type pour la source de liaison. Cependant, lorsque vous exécutez votre application avec le débogueur attaché, toute erreur de liaison apparaît dans la fenêtre **Sortie** de Visual Studio.

Création de liaisons dans le code -------------------------

**Remarque** Cette section s’applique uniquement à [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782), car vous ne pouvez pas créer de liaisons [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) dans le code. Toutefois, il est possible de profiter des avantages de {x:Bind} avec [**DependencyProperty.RegisterPropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/BR242356-registerpropertychangedcallback), qui vous permet de vous inscrire aux notifications de modification des propriétés de dépendance.

Vous pouvez également connecter des éléments d’interface utilisateur aux données en utilisant un code procédural et non XAML. Pour ce faire, créez un objet [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820), définissez les propriétés appropriées, puis appelez [**FrameworkElement.SetBinding**](https://msdn.microsoft.com/library/windows/apps/BR208706-setbinding) ou [**BindingOperations.SetBinding**](https://msdn.microsoft.com/library/windows/apps/BR209820operations-setbinding). La création de liaisons par programme est utile quand vous voulez choisir les valeurs des propriétés de liaison au moment de l’exécution ou partager une liaison unique entre plusieurs contrôles. Notez, toutefois, que vous ne pouvez pas modifier les valeurs des propriétés de liaison après avoir appelé **SetBinding**.

L’exemple suivant explique comment implémenter une liaison dans le code.

``` xml
<TextBox x:Name="MyTextBox" Text="Text"/>
```

```csharp
// Create an instance of the MyColors class 
// that implements INotifyPropertyChanged.
MyColors textcolor = new MyColors();

// Brush1 is set to be a SolidColorBrush with the value Red.
textcolor.Brush1 = new SolidColorBrush(Colors.Red);

// Set the DataContext of the TextBox MyTextBox.
MyTextBox.DataContext = textcolor;

// Create the binding and associate it with the text box.
Binding binding = new Binding() { Path = new PropertyPath("Brush1") };
MyTextBox.SetBinding(TextBox.ForegroundProperty, binding);
```

``` vbnet
' Create an instance of the MyColors class 
' that implements INotifyPropertyChanged. 
Dim textcolor As New MyColors()

' Brush1 is set to be a SolidColorBrush with the value Red. 
textcolor.Brush1 = New SolidColorBrush(Colors.Red)

' Set the DataContext of the TextBox MyTextBox. 
MyTextBox.DataContext = textcolor

' Create the binding and associate it with the text box.
Dim binding As New Binding() With {.Path = New PropertyPath("Brush1")}
MyTextBox.SetBinding(TextBox.ForegroundProperty, binding)
```

Comparaison des fonctionnalités {x:Bind} et {Binding}
------------------------------------------

| Fonctionnalité | {x:Bind} | {Binding} | Remarques |
|---------|----------|-----------|-------|
| Path est la propriété par défaut | `{x:Bind a.b.c}` | `{Binding a.b.c}` | | 
| Propriété Path | `{x:Bind Path=a.b.c}` | `{Binding Path=a.b.c}` | Dans x:Bind, Path a pour racine Page par défaut et non DataContext. | | Indexer | `{x:Bind Groups[2].Title}` | `{Binding Groups[2].Title}` | Se lie à l’élément spécifié dans la collection. Seuls les index basés sur des entiers sont pris en charge. | | Attached properties | `{x:Bind Button22.(Grid.Row)}` | `{Binding Button22.(Grid.Row)}` | Les propriétés jointes sont spécifiées à l’aide de parenthèses. Si la propriété n’est pas déclarée dans un espace de noms XAML, vous devez la faire précéder d’un espace de noms xml mappé sur un espace de noms de code au début du document. | | Transtypage | `{x:Bind groups[0].(data:SampleDataGroup.Title)}` | Non requis< | Les conversions de type (transtypage) sont spécifiées à l’aide de parenthèses. Si la propriété n’est pas déclarée dans un espace de noms XAML, vous devez la faire précéder d’un espace de noms xml mappé sur un espace de noms de code au début du document. | 
| Converter | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}}` | Les convertisseurs doivent être déclarés à la racine de Page/ResourceDictionary ou dans le fichier App.xaml. | 
| ConverterParameter, ConverterLanguage | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | Les convertisseurs doivent être déclarés à la racine de Page/ResourceDictionary ou dans le fichier App.xaml. | 
| TargetNullValue | `{x:Bind Name, TargetNullValue=0}` | `{Binding Name, TargetNullValue=0}` | Utilisée lorsque le nœud terminal de l’expression de liaison présente la valeur null. Utilisez des guillemets simples pour une valeur de chaîne. | 
| FallbackValue | `{x:Bind Name, FallbackValue='empty'}` | `{Binding Name, FallbackValue='empty'}` | Utilisée lorsqu’une partie du chemin de la liaison (à l’exception du nœud terminal) présente la valeur null. | 
| ElementName | `{x:Bind slider1.Value}` | `{Binding Value, ElementName=slider1}` | Avec {x:Bind}, vous créez une liaison à un champ ; Path a pour racine Page par défaut, de sorte que tout élément nommé est accessible via son champ. | 
| RelativeSource: Self | `<Rectangle x:Name="rect1" Width="200" Height="{x:Bind rect1.Width}" ... />` | `<Rectangle Width="200" Height="{Binding Width, RelativeSource={RelativeSource Self}}" ... />` | With {x:Bind}, name the element and use its name in Path. | 
| RelativeSource: TemplatedParent | Not supported | `{Binding <path>, RelativeSource={RelativeSource TemplatedParent}}` | La liaison de modèle standard peut être utilisée dans les modèles de contrôle dans la plupart des cas. Toutefois, faites appel à TemplatedParent quand vous devez utiliser un convertisseur ou une liaison bidirectionnelle.< | 
| Source | Non pris en charge | `<ListView ItemsSource="{Binding Orders, Source={StaticResource MyData}}"/>` | For {x:Bind} use a property or a static path instead. | 
| Mode | `{x:Bind Name, Mode=OneWay}` | `{Binding Name, Mode=TwoWay}` | Mode can be OneTime, OneWay, or TwoWay. {x:Bind} defaults to OneTime; {Binding} defaults to OneWay. | 
| UpdateSourceTrigger | Not supported | `<Binding UpdateSourceTrigger="[Default | PropertyChanged | Explicit]"/>` | {x:Bind} utilise le comportement PropertyChanged dans tous les cas, sauf pour TextBox.Text, où il attend la perte de focus pour mettre à jour la source. | 




<!--HONumber=Mar16_HO1-->


