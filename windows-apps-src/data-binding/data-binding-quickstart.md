---
author: mcleblanc
ms.assetid: A9D54DEC-CD1B-4043-ADE4-32CD4977D1BF
title: "Vue d’ensemble de la liaison de données"
description: "Cette rubrique vous montre comment lier un contrôle (ou un autre élément d’interface utilisateur) à un élément individuel ou lier un contrôle d’éléments à ou un contrôle de liste à une collection d’éléments dans une application UWP."
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 092df9799982dc5da5cc085b2e73a5dd376c0eb8

---
Vue d’ensemble de la liaison de données
=====================

\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Cette rubrique vous montre comment lier un contrôle (ou un autre élément d’interface utilisateur) à un élément individuel ou lier un contrôle d’éléments à ou un contrôle de liste à une collection d’éléments dans une application de plateforme Windows universelle (UWP). Elle explique également comment contrôler le rendu des éléments, implémenter un affichage détails en fonction d’une sélection et convertir des données pour l’affichage. Pour obtenir des informations plus détaillées, consultez [Présentation détaillée de la liaison de données](data-binding-in-depth.md).

Connaissances requises
-------------------------------------------------------------------------------------------------------------

Dans cette rubrique, nous partons du principe que vous savez créer une application UWP de base. Pour obtenir des instructions pour la création de votre première application UWP, consultez [Créer votre première application UWP en C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/Hh974581).

Créer le projet
---------------------------------------------------------------------------------------------------------------------------------

Commencez par créer un projet **Application vide (universelle Windows)**. Nommez-le «Quickstart».

Liaison à un élément unique
---------------------------------------------------------------------------------------------------------------------------------------------------------

Chaque liaison se compose d’une cible et d’une source de liaison. En règle générale, la cible est une propriété d’un contrôle ou d’un autre élément d’interface utilisateur, et la source est une propriété d’une instance de classe (un modèle de données ou un modèle d’affichage). Cet exemple montre comment lier un contrôle à un élément unique. La cible est la propriété **Text** d’un contrôle **TextBlock**. La source est une instance d’une classe simple nommée **Recording**, qui représente un enregistrement audio. Examinons d’abord la classe.

Ajoutez une nouvelle classe à votre projet, nommez-la Recording.cs (si vous utilisez C#) et ajoutez-lui ce code.

```csharp
namespace Quickstart
{
    public class Recording
    {
        public string ArtistName { get; set; }
        public string CompositionName { get; set; }
        public DateTime ReleaseDateTime { get; set; }

        public Recording()
        {
            this.ArtistName = "Wolfgang Amadeus Mozart";
            this.CompositionName = "Andante in C for Piano";
            this.ReleaseDateTime = new DateTime(1761, 1, 1);
        }

        public string OneLineSummary
        {
            get
            {
                return $"{this.CompositionName} by {this.ArtistName}, released: "
                    + this.ReleaseDateTime.ToString("d");
            }
        }
    }

    public class RecordingViewModel
    {
        private Recording defaultRecording = new Recording();
        public Recording DefaultRecording { get { return this.defaultRecording; } }
    }
}
```

```cpp
#include <sstream>

namespace Quickstart
{
    public ref class Recording sealed
    {
    private:
        Platform::String^ artistName;
        Platform::String^ compositionName;
        Windows::Globalization::Calendar^ releaseDateTime;

    public:
        Recording(Platform::String^ artistName, Platform::String^ compositionName,
            Windows::Globalization::Calendar^ releaseDateTime) :
            artistName{ artistName },
            compositionName{ compositionName },
            releaseDateTime{ releaseDateTime } {}

        property Platform::String^ ArtistName
        {
            Platform::String^ get() { return this->artistName; }
        }

        property Platform::String^ CompositionName
        {
            Platform::String^ get() { return this->compositionName; }
        }

        property Windows::Globalization::Calendar^ ReleaseDateTime
        {
            Windows::Globalization::Calendar^ get() { return this->releaseDateTime; }
        }

        property Platform::String^ OneLineSummary
        {
            Platform::String^ get()
            {
                std::wstringstream wstringstream;
                wstringstream << this->CompositionName->Data();
                wstringstream << L" by " << this->ArtistName->Data();
                wstringstream << L", released: " << this->ReleaseDateTime->MonthAsNumericString()->Data();
                wstringstream << L"/" << this->ReleaseDateTime->DayAsString()->Data();
                wstringstream << L"/" << this->ReleaseDateTime->YearAsString()->Data();
                return ref new Platform::String(wstringstream.str().c-str());
            }
        }
    };

    public ref class RecordingViewModel sealed
    {
    private:
        Recording^ defaultRecording;

    public:
        RecordingViewModel()
        {
            Windows::Globalization::Calendar^ releaseDateTime = ref new Windows::Globalization::Calendar();
            releaseDateTime->Month = 1;
            releaseDateTime->Day = 1;
            releaseDateTime->Year = 1761;
            this->defaultRecording = ref new Recording{ L"Wolfgang Amadeus Mozart", L"Andante in C for Piano", releaseDateTime };
        }

        property Recording^ DefaultRecording
        {
            Recording^ get() { return this->defaultRecording; };
        }
    };
}
```

Ensuite, exposez la classe de source de liaison à partir de la classe qui représente votre page de balisage. Pour ce faire, nous ajoutons une propriété de type **RecordingViewModel** à **MainPage**.

```csharp
namespace Quickstart
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.ViewModel = new RecordingViewModel();
        }

        public RecordingViewModel ViewModel { get; set; }
    }
}
```

```cpp
namespace Quickstart
{
    public ref class MainPage sealed
    {
    private:
        RecordingViewModel^ viewModel;

    public:
        MainPage()
        {
            InitializeComponent();
            this->viewModel = ref new RecordingViewModel();
        }

        property RecordingViewModel^ ViewModel
        {
            RecordingViewModel^ get() { return this->viewModel; };
        }
    };
}
```

La dernière étape consiste à lier un contrôle **TextBlock** à la propriété **ViewModel.DefaultRecording.OneLiner**.

```xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock Text="{x:Bind ViewModel.DefaultRecording.OneLineSummary}"
        HorizontalAlignment="Center"
        VerticalAlignment="Center"/>
    </Grid>
</Page>
```

Résultat:

![Liaison d’une zone de texte](images/xaml-databinding0.png)

Liaison à une collection d’éléments
------------------------------------------------------------------------------------------------------------------

Un scénario courant consiste à créer une liaison à une collection d’objets métier. Dans C# et Visual Basic, la classe [**ObservableCollection&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/xaml/ms668604.aspx) générique est un bon choix de collection pour la liaison de données, car elle implémente les interfaces [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.componentmodel.inotifypropertychanged.aspx) et [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx). Ces interfaces envoient une notification de modification aux liaisons lorsque des éléments sont ajoutés ou supprimés ou qu’une propriété de la liste est elle-même modifiée. Si vous voulez que vos contrôles liés soient mis à jour avec les modifications apportées aux propriétés des objets de la collection, l’objet métier doit également implémenter **INotifyPropertyChanged**. Pour plus d’informations, consultez [Présentation détaillée de la liaison de données](data-binding-in-depth.md).

L’exemple suivant lie une classe [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) à une collection d’objets `Recording`. Commençons par ajouter la collection à notre modèle d’affichage. Il suffit d’ajouter ces nouveaux membres à la classe **RecordingViewModel**.

```csharp
    public class RecordingViewModel
    {
        ...
        
        private ObservableCollection<Recording> recordings = new ObservableCollection<Recording>();
        public ObservableCollection<Recording> Recordings { get { return this.recordings; } }

        public RecordingViewModel()
        {
            this.recordings.Add(new Recording() { ArtistName = "Johann Sebastian Bach",
            CompositionName = "Mass in B minor", ReleaseDateTime = new DateTime(1748, 7, 8) });
            this.recordings.Add(new Recording() { ArtistName = "Ludwig van Beethoven",
            CompositionName = "Third Symphony", ReleaseDateTime = new DateTime(1805, 2, 11) });
            this.recordings.Add(new Recording() { ArtistName = "George Frideric Handel",
            CompositionName = "Serse", ReleaseDateTime = new DateTime(1737, 12, 3) });
        }
    }
```

```cpp
    public ref class RecordingViewModel sealed
    {
    private:
        ...
        Windows::Foundation::Collections::IVector<Recording^>^ recordings;

    public:
        RecordingViewModel()
        {
            ...

            releaseDateTime = ref new Windows::Globalization::Calendar();
            releaseDateTime->Month = 7;
            releaseDateTime->Day = 8;
            releaseDateTime->Year = 1748;
            Recording^ recording = ref new Recording{ L"Johann Sebastian Bach", L"Mass in B minor", releaseDateTime };
            this->Recordings->Append(recording);

            releaseDateTime = ref new Windows::Globalization::Calendar();
            releaseDateTime->Month = 2;
            releaseDateTime->Day = 11;
            releaseDateTime->Year = 1805;
            recording = ref new Recording{ L"Ludwig van Beethoven", L"Third Symphony", releaseDateTime };
            this->Recordings->Append(recording);

            releaseDateTime = ref new Windows::Globalization::Calendar();
            releaseDateTime->Month = 12;
            releaseDateTime->Day = 3;
            releaseDateTime->Year = 1737;
            recording = ref new Recording{ L"George Frideric Handel", L"Serse", releaseDateTime };
            this->Recordings->Append(recording);
        }

        ...

        property Windows::Foundation::Collections::IVector<Recording^>^ Recordings
        {
            Windows::Foundation::Collections::IVector<Recording^>^ get()
            {
                if (this->recordings == nullptr)
                {
                    this->recordings = ref new Platform::Collections::Vector<Recording^>();
                }
                return this->recordings;
            };
        }
    };
```

Ensuite, liez un contrôle [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) à la propriété **ViewModel.Recordings**.

```xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <ListView ItemsSource="{x:Bind ViewModel.Recordings}"
        HorizontalAlignment="Center" VerticalAlignment="Center"/>
    </Grid>
</Page>
```

Nous n’avons pas encore fourni de modèle de données pour la classe **Recording**. Par conséquent, le mieux que l’infrastructure d’interface utilisateur puisse faire est d’appeler [**ToString**](https://msdn.microsoft.com/library/windows/apps/system.object.tostring.aspx) pour chaque élément de [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878). L’implémentation par défaut de **ToString** consiste à renvoyer le nom du type.

![Liaison d’un affichage liste](images/xaml-databinding1.png)

Pour résoudre ce problème, nous pouvons soit remplacer [**ToString**](https://msdn.microsoft.com/library/windows/apps/system.object.tostring.aspx) pour renvoyer la valeur de **OneLineSummary**, soit fournir un modèle de données. L’option du modèle de données est plus courante et sans doute plus souple. Pour spécifier un modèle de données, vous devez utiliser la propriété [**ContentTemplate**](https://msdn.microsoft.com/library/windows/apps/BR209369) d’un contrôle de contenu ou la propriété [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242830) d’un contrôle d’éléments. Voici deux manières de créer un modèle de données pour **Recording** avec une illustration du résultat:

```xml
    <ListView ItemsSource="{x:Bind ViewModel.Recordings}"
        HorizontalAlignment="Center" VerticalAlignment="Center">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="local:Recording">
                <TextBlock Text="{x:Bind OneLineSummary}"/>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
```

![Liaison d’un affichage liste](images/xaml-databinding2.png)

```xml
    <ListView ItemsSource="{x:Bind ViewModel.Recordings}"
    HorizontalAlignment="Center" VerticalAlignment="Center">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="local:Recording">
                <StackPanel Orientation="Horizontal" Margin="6">
                    <SymbolIcon Symbol="Audio" Margin="0,0,12,0"/>
                    <StackPanel>
                        <TextBlock Text="{x:Bind ArtistName}" FontWeight="Bold"/>
                        <TextBlock Text="{x:Bind CompositionName}"/>
                    </StackPanel>
                </StackPanel>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
```

![Liaison d’un affichage liste](images/xaml-databinding3.png)

Pour en savoir plus sur la syntaxe XAML, consultez [Créer une interface utilisateur avec XAML](https://msdn.microsoft.com/library/windows/apps/Mt228349). Pour en savoir plus sur la mise en forme de contrôle, consultez [Définir des dispositions avec XAML](https://msdn.microsoft.com/library/windows/apps/Mt228350).

Ajout d’un affichage de détails
-----------------------------------------------------------------------------------------------------

Vous pouvez choisir d’afficher tous les détails des objets **Recording** dans les éléments [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878). Cependant, l’espace occupé à l’écran sera considérable. Au lieu de cela, vous pouvez afficher juste assez de données dans l’élément à identifier puis, lorsque l’utilisateur effectue une sélection, afficher tous les détails de l’élément sélectionné dans un élément d’interface utilisateur distinct appelé affichage détails. Cette disposition est également connue sous le nom d’affichage maître/détails ou d’affichage liste/détails.

Il existe deux façons de procéder : Vous pouvez lier l’affichage de détails à la propriété [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/BR209770) de [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878). Ou vous pouvez utiliser une [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833): liez à la fois le contrôle **ListView** et l’affichage détails à la classe **CollectionViewSource** (qui s’occupera de l’élément actuellement sélectionné pour vous). Ces deux techniques sont présentées ci-dessous et donnent les mêmes résultats que dans l’illustration.

**Remarque** Jusqu’à présent, nous avons uniquement utilisé l’[extension de balisage {x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783), mais les deux techniques que nous allons présenter ci-dessous requièrent l’[extension de balisage {Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782), plus souple (mais moins performante).

Tout d’abord, voici la technique [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/BR209770). Si vous utilisez des extensions de composant Visual C++ (C++/CX), dans la mesure où [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) sera utilisé, vous devrez ajouter l’attribut [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) à la classe **Recording**.

```cpp
    [Windows::UI::Xaml::Data::Bindable]
    public ref class Recording sealed
    {
        ...
    };
```

La seule autre modification nécessaire concerne le balisage.

```xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
            <ListView x:Name="recordingsListView" ItemsSource="{x:Bind ViewModel.Recordings}">
                <ListView.ItemTemplate>
                    <DataTemplate x:DataType="local:Recording">
                        <StackPanel Orientation="Horizontal" Margin="6">
                            <SymbolIcon Symbol="Audio" Margin="0,0,12,0"/>
                            <StackPanel>
                                <TextBlock Text="{x:Bind CompositionName}"/>
                            </StackPanel>
                        </StackPanel>
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
            <StackPanel DataContext="{Binding SelectedItem, ElementName=recordingsListView}"
            Margin="0,24,0,0">
                <TextBlock Text="{Binding ArtistName}"/>
                <TextBlock Text="{Binding CompositionName}"/>
                <TextBlock Text="{Binding ReleaseDateTime}"/>
            </StackPanel>
        </StackPanel>
    </Grid>
</Page>
```

Pour la technique [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833), commencez par ajouter une classe **CollectionViewSource** comme ressource de page.

```xml
    <Page.Resources>
        <CollectionViewSource x:Name="RecordingsCollection" Source="{x:Bind ViewModel.Recordings}"/>
    </Page.Resources>
```

Ensuite, réglez les liaisons sur le contrôle [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) (qui n’a plus besoin d’être nommé) et sur l’affichage de détails de manière à utiliser la classe [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833). Notez qu’en liant l’affichage de détails directement à la classe **CollectionViewSource**, vous impliquez que vous souhaitez créer une liaison à l’élément actuel dans les liaisons où le chemin est introuvable au sein même de la collection. Il est inutile de spécifier la propriété **CurrentItem** en tant que chemin de la liaison, bien que vous puissiez le faire s’il existe la moindre ambiguïté.

```xml
    ...

    <ListView ItemsSource="{Binding Source={StaticResource RecordingsCollection}}">

    ...

    <StackPanel DataContext="{Binding Source={StaticResource RecordingsCollection}}" ...>
    ...
```

Et voici le résultat identique dans chacun des cas.

![Liaison d’un affichage liste](images/xaml-databinding4.png)

Mise en forme ou conversion des valeurs de données pour l’affichage
--------------------------------------------------------------------------------------------------------------------------------------------

Le rendu ci-dessus présente un léger problème. La propriété **ReleaseDateTime** n’est pas simplement une date. Il s’agit d’une valeur [**DateTime**](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetime.aspx), ce qui signifie que nous obtenons plus d’informations que nécessaire. Une solution consiste à ajouter à la classe **Recording** une propriété de chaîne qui renvoie `this.ReleaseDateTime.ToString("d")`. Nommer cette propriété **ReleaseDate** indiquerait qu’elle renvoie une date, et non pas une date et une heure. Nommer cette propriété **ReleaseDateAsString** indiquerait en plus qu’elle renvoie une chaîne.

Une solution plus souple consiste à utiliser un élément connu sous le nom de convertisseur de valeurs. Voici un exemple montrant comment créer votre propre convertisseur de valeurs. Ajoutez ce code à votre fichier de code source Recording.cs.

```csharp
public class StringFormatter : Windows.UI.Xaml.Data.IValueConverter
{
    // This converts the value object to the string to display.
    // This will work with most simple types.
    public object Convert(object value, Type targetType,
        object parameter, string language)
    {
        // Retrieve the format string and use it to format the value.
        string formatString = parameter as string;
        if (!string.IsNullOrEmpty(formatString))
        {
            return string.Format(formatString, value);
        }

        // If the format string is null or empty, simply
        // call ToString() on the value.
        return value.ToString();
    }

    // No need to implement converting back on a one-way binding
    public object ConvertBack(object value, Type targetType,
        object parameter, string language)
    {
        throw new NotImplementedException();
    }
}
```

Nous pouvons maintenant ajouter une instance de **StringFormatter** comme ressource de page et l’utiliser dans notre liaison. Nous passons la chaîne de format dans le convertisseur à partir du balisage pour une souplesse de mise en forme optimale.

```xml
    <Page.Resources>
        <local:StringFormatter x:Key="StringFormatterValueConverter"/>
    </Page.Resources>
    ...

    <TextBlock Text="{Binding ReleaseDateTime,
        Converter={StaticResource StringFormatterValueConverter},
        ConverterParameter=Released: \{0:d\}}"/>

    ...
```

Résultat :

![Affichage d’une date avec une mise en forme personnalisée](images/xaml-databinding5.png)





<!--HONumber=Jul16_HO2-->


