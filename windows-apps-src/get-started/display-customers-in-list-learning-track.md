---
title: Piste d'apprentissage - Afficher les clients dans une liste
description: Découvrez ce que vous devez faire pour afficher une collection d’objets Client dans une liste.
ms.date: 05/07/2018
ms.topic: article
keywords: prise en main, uwp, windows10, piste d'apprentissage, liaison de données, liste
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bd4a1f6747ea68623039b7eac22ac08aaa15d9ea
ms.sourcegitcommit: 231065c899d0de285584d41e6335251e0c2c4048
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8827855"
---
# <a name="display-customers-in-a-list"></a>Afficher les clients dans une liste

Pouvoir afficher et manipuler des données réelles dans l’interface utilisateur est essentiel pour les fonctionnalités de nombreuses applications. Cet article vous montre ce que vous devez savoir pour afficher une collection d’objets Client dans une liste.

Il ne s'agit pas d'un didacticiel. Si vous en souhaitez un, consultez notre [didacticiel de liaison de données](../data-binding/xaml-basics-data-binding.md) qui vous fournira une expérience interactive pas à pas.

Nous allons commencer par une présentation rapide de la liaison de données - en quoi elle consiste et comment elle fonctionne. Puis nous allons ajouter un **ListView** à l’interface utilisateur, ajouter une liaison de données et la personnaliser avec des fonctionnalités supplémentaires. 

## <a name="what-do-you-need-to-know"></a>Ce que vous devez savoir

La liaison de données est un moyen d’afficher les données d’une application dans son interface utilisateur. Cela permet d'établir une *séparation des responsabilités* dans votre application, en gardant votre interface utilisateur séparée de votre autre code. Cela crée un modèle conceptuel plus fluide et plus facile à lire et à gérer.

Chaque liaison de données comporte deux parties:

* une source qui fournit les données à lier;
* une cible dans l’interface utilisateur où les données sont affichées.

Pour implémenter une liaison de données, vous devez ajouter du code à la source qui fournit les données à la liaison. Vous devez également ajouter l'une des deux extensions de balisage à votre code XAML pour spécifier les propriétés de source de données. Voici la principale différence entre les deux:

* [**x:Bind**](../xaml-platform/x-bind-markup-extension.md) est fortement typée et génère du code au moment de la compilation pour de meilleures performances. x:Bind est par défaut une liaison à usage unique optimisée pour l’affichage rapide des données en lecture seule qui ne changent pas.
* [**Binding**](../xaml-platform/binding-markup-extension.md) est faiblement typée et assemblée lors de l’exécution. Elle produit de moins bonnes performances que x:Bind. Dans la plupart des cas, vous devez utiliser x:Bind au lieu de Binding. Toutefois, vous la trouverez probablement dans un ancien code. Binding est par défaut un transfert de données à sens unique optimisé pour les données en lecture seule susceptibles d'être modifiées à la source.

Nous vous recommandons d’utiliser **x:Bind** chaque fois que c'est possible, comme nous allons l'illustrer dans les extraits de code de cet article. Pour plus d’informations sur les différences, consultez [Comparaison des fonctionnalités {x:Bind} et {Binding}](../data-binding/data-binding-in-depth.md#xbind-and-binding-feature-comparison).

## <a name="create-a-data-source"></a>Créer une source de données

Tout d’abord, vous avez besoin d’une classe pour représenter vos données client. Pour vous donner un point de référence, nous allons illustrer le processus dans cet exemple simple:

```csharp
public class Customer
{
    public string Name { get; set; }
}
```

## <a name="create-a-list"></a>Créer une liste

Pour pouvoir afficher les clients, vous devez créer une liste pour les contenir. L'[affichage liste](../design/controls-and-patterns/listview-and-gridview.md) est un contrôle XAML de base idéal pour cette tâche. Votre contrôle ListView nécessite actuellement une position dans la page et aura rapidement besoin d'une valeur pour sa propriété **ItemSource**.

```xaml
<ListView ItemsSource=""
    HorizontalAlignment="Center"
    VerticalAlignment="Center"/>
```

Une fois que vous avez lié des données à votre contrôle ListView, nous vous invitons à vous reporter à la documentation et à vous familiariser avec la personnalisation de son apparence et de sa disposition afin de mieux répondre à vos besoins.

## <a name="bind-data-to-your-list"></a>Lier des données à votre liste

Maintenant que vous avez créé une interface utilisateur de base pour contenir vos liaisons, vous devez configurer votre source pour les fournir. Voici un exemple de la manière de procéder:

```csharp
public sealed partial class MainPage : Page
{
    public ObservableCollection<Customer> Customers { get; }
        = new ObservableCollection<Customer>();

    public MainPage()
    {
        this.InitializeComponent();
          // Add some customers
        this.Customers.Add(new Customer() { Name = "NAME1"});
        this.Customers.Add(new Customer() { Name = "NAME2"});
        this.Customers.Add(new Customer() { Name = "NAME3"});
    }
}
```
```xaml
<ListView ItemsSource="{x:Bind Customers}"
    HorizontalAlignment="Center"
    VerticalAlignment="Center">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Customer">
            <TextBlock Text="{x:Bind Name}"/>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

La [Vue d’ensemble de la liaison de données](../data-binding/data-binding-quickstart.md#binding-to-a-collection-of-items) vous explique comment régler un problème similaire dans sa section sur la liaison à une collection d’éléments. Le présent exemple illustre les étapes essentielles suivantes:

* Dans le code-behind de votre interface utilisateur, créez une propriété de type **ObservableCollection<T>** pour contenir les objets client.
* Liez l'**ItemSource** de votre contrôle ListView à cette propriété.
* Fournissez un **ItemTemplate** de base au contrôle ListView, qui configurera l’affichage de chaque élément dans la liste.

N’hésitez pas à consulter la documentation [Affichage liste](../design/controls-and-patterns/listview-and-gridview.md) si vous souhaitez personnaliser la disposition, ajouter une sélection d’éléments ou adapter le **DataTemplate** que vous venez de créer. Mais que se passe-t-il si vous souhaitez modifier vos clients?

## <a name="edit-your-customers-through-the-ui"></a>Modifier vos clients par le biais de l’interface utilisateur

Vous avez affiché les clients dans une liste, mais la liaison de données B=binding vous permet d'en faire plus. Que se passe-t-il si vous pouvez modifier vos données directement à partir de l’interface utilisateur? Pour ce faire, nous allons tout d’abord parler des trois modes de liaison de données:

* *À usage unique*: cette liaison de données est activée uniquement une fois et ne réagit pas aux modifications apportées.
* *À sens unique*: cette liaison de données met à jour l’interface utilisateur avec les modifications apportées à la source de données.
* *Bidirectionnelle*: cette liaison de données met à jour l’interface utilisateur avec les modifications apportées à la source de données et met également à jour les données avec les modifications apportées dans l’interface utilisateur.

Si vous avez suivi les extraits de code antérieurs, la liaison que vous avez apportée utilise x:Bind et ne spécifie pas de mode, ce qui produit une liaison à usage unique. Si vous souhaitez modifier vos clients directement à partir de l’interface utilisateur, vous devez la remplacer par une liaison bidirectionnelle, afin que les modifications des données soient transférées vers les objets client. [Présentation détaillée de la liaison de données](../data-binding/data-binding-in-depth.md) contient des informations supplémentaires.

La liaison bidirectionnelle met également à jour l’interface utilisateur si la source de données est modifiée. Pour que cela fonctionne, vous devez implémenter [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged(d=robot).aspx) sur la source et vous assurer que ses méthodes setter de propriété déclenchent l'événement **PropertyChanged**. Une pratique courante consiste à les faire appeler une méthode d’assistance telle que la méthode **OnPropertyChanged**, comme illustré ci-dessous:

```csharp
public class Customer : INotifyPropertyChanged
{
    private string _name;
    public string Name
    {
        get => _name;
        set
        {
            if (_name != value)
                {
                    _name = value;
                    this.OnPropertyChanged();
                }
        }
    }
    public event PropertyChangedEventHandler PropertyChanged;

    public void OnPropertyChanged([CallerMemberName] string propertyName = null) =>
        this.PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
}
```
Rendez ensuite le texte dans votre ListView modifiable à l’aide d'un **TextBox** au lieu d’un **TextBlock** et assurez-vous de définir le **Mode** sur vos liaisons de données sur **TwoWay** (bidirectionnel).

```xaml
<ListView ItemsSource="{x:Bind Customers}"
    HorizontalAlignment="Center"
    VerticalAlignment="Center">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Customer">
            <TextBox Text="{x:Bind Name, Mode=TwoWay}"/>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Un moyen rapide de vous assurer que cela fonctionne consiste à ajouter un deuxième ListView avec des contrôles TextBox et des liaisons OneWay. Les valeurs de la seconde liste changent automatiquement lorsque vous modifiez la première.

> [!NOTE]
> Modifier directement à l’intérieur d’un contrôle ListView est un moyen simple de montrer une liaison bidirectionnelle en action, mais cela peut poser des problèmes de facilité d’utilisation. Si vous cherchez à améliorer votre application, envisagez d’utiliser d'[autres contrôles XAML](../design/controls-and-patterns/controls-and-events-intro.md) pour modifier vos données et garder votre ListView en affichage seul.

## <a name="going-further"></a>Aller plus loin

Maintenant que vous avez créé une liste de clients avec une liaison bidirectionnelle, n’hésitez pas à vous reporter à la documentation dont nous avons fourni les liens et à tester. Vous pouvez également consulter notre [didacticiel de liaison de données](../data-binding/xaml-basics-data-binding.md) si vous souhaitez une procédure pas à pas sur les liaisons de base et avancées ou examiner des contrôles tels que le [modèle Maître/Détails](../design/controls-and-patterns/master-details.md) pour créer une interface utilisateur plus robuste.

## <a name="useful-apis-and-docs"></a>API et documents utiles

Voici un résumé rapide des API et des autres documents utiles pour vous aider à commencer à utiliser la Liaison de données.

### <a name="useful-apis"></a>API utiles

| API | Description |
|------|---------------|
| [Modèle de données](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate) | Décrit la structure visuelle d’un objet de données, pour permettre l’affichage d'éléments spécifiques dans l’interface utilisateur. |
| [x:Bind](../xaml-platform/x-bind-markup-extension.md) | Documentation sur l’extension de balisage x:Bind recommandée. |
| [Liaison](../xaml-platform/binding-markup-extension.md) | Documentation sur l’ancienne extension de balisage Binding. |
| [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) | Contrôle d'interface utilisateur qui affiche des éléments de données dans une pile verticale. |
| [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) | Contrôle de texte de base pour l’affichage de données de texte modifiables dans l’interface utilisateur. |
| [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged(d=robot).aspx) | Interface pour rendre les données observables, en fournissant une liaison de données. |
| [ItemsControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) | La propriété **ItemsSource** de cette classe permet à un contrôle ListView d'établir une liaison avec une source de données. |

### <a name="useful-docs"></a>Documents utiles

| Sujet | Description |
|-------|----------------|
| [Présentation détaillée de la liaison de données](../data-binding/data-binding-in-depth.md) | Vue d’ensemble des principes de liaison de données |
| [Vue d’ensemble de la liaison de données](../data-binding/data-binding-quickstart.md) | Informations conceptuelles détaillées sur la liaison de données. |
| [Affichage de liste](../design/controls-and-patterns/listview-and-gridview.md) | Informations sur la création et la configuration d’un contrôle ListView, y compris l’implémentation d’un **DataTemplate** |

## <a name="useful-code-samples"></a>Exemples de code utiles

| Exemple de code | Description |
|-----------------|---------------|
| [Didacticiel de liaison de données](../data-binding/xaml-basics-data-binding.md) | Expérience interactive pas à pas sur les principes fondamentaux de la liaison de données. |
| [ListView et GridView](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView) | Découvrez des contrôles ListView plus complexes avec liaison de données |
| [QuizGame](https://github.com/Microsoft/Windows-appsample-networkhelper) | Regardez la liaison de données en action, notamment la classe **BindableBase** (dans le dossier «Common») pour une implémentation standard de **INotifyPropertyChanged**. |
