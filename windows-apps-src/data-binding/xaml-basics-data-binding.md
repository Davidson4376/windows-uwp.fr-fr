---
title: Créer des liaisons de données
description: Cet article décrit les principes fondamentaux de la liaison de données en XAML
keywords: XAML, UWP, Prise en main
ms.date: 08/30/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 21a053934d7391d12f7cd987026524b9ff4c279d
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8192653"
---
# <a name="create-data-bindings"></a>Créer des liaisons de données

Supposons que vous avez conçu et implémenté une interface utilisateur à l'aspect élégant, remplie d'images d'espace réservé, de texte modèle «lorem ipsum» et de contrôles qui ne font rien pour le moment. Vous voulez ensuite la connecter à des données réelles et transformer ce prototype de conception en application vivante. 

Dans ce didacticiel, vous allez apprendre à remplacer votre texte modèle à l'aide de liaisons de données et à créer d'autres liens directs entre votre interface utilisateur et vos données. Vous apprendrez également à mettre en forme ou convertir vos données pour l’affichage et à synchroniser votre interface utilisateur avec vos données. Une fois que vous aurez terminé ce didacticiel, vous pourrez améliorer la simplicité et l’organisation du code XAML et C# et le rendrez ainsi plus facile à gérer et à développer.

Vous allez commencer avec une version simplifiée de l’exemple PhotoLab. Cette version de démarrage comprend la couche de données complète, ainsi que des dispositions de pages XAML de base et exclut de nombreuses fonctionnalités afin que le code soit plus facile à parcourir. Ce didacticiel ne génère pas l’application complète, donc veillez à examiner la version définitive pour voir des fonctionnalités telles que les animations personnalisées et le support téléphonique. Vous pouvez trouver la version finale dans le dossier racine du référentiel [Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab). 

## <a name="prerequisites"></a>Éléments prérequis

* [VisualStudio2017 et la dernière version du SDK Windows10](https://developer.microsoft.com/windows/downloads).

## <a name="part-0-get-the-code"></a>Partie0: obtenir le code
Le point de départ de ce laboratoire se trouve dans le référentiel d’exemples PhotoLab, dans le dossier [xaml-basics-starting-points/data-binding](https://github.com/Microsoft/Windows-appsample-photo-lab/tree/master/xaml-basics-starting-points/data-binding). Après avoir cloné ou téléchargé le référentiel, vous pouvez modifier le projet en ouvrant PhotoLab.sln avec Visual Studio2017.

L’application PhotoLab comporte deux pages principales:

**MainPage.xaml:** présente un affichage de galerie de photos, ainsi que des informations sur chaque fichier d’image.
![MainPage](../design/basics/images/xaml-basics/mainpage.png)

**DetailPage.xaml:** affiche une seule photo une fois qu'elle a été sélectionnée. Un menu d'édition volant permet de modifier la photo, de la renommer et de l'enregistrer.
![DetailPage](../design/basics/images/xaml-basics/detailpage.png)

## <a name="part-1-replace-the-placeholders"></a>Partie1: remplacer les espaces réservés

Vous allez créer des liaisons à usage unique dans le modèle de données XAML pour afficher les images réelles et les métadonnées d’image à la place du contenu de l’espace réservé. 

Les liaisons uniques sont des données immuables en lecture seule. Elles sont donc très performantes et faciles à créer, ce qui vous permet d’afficher de grands ensembles de données dans des contrôles **GridView** et **ListView**. 

**Remplacer les espaces réservés par des liaisons à usage unique**

1. Ouvrez le dossier xaml-basics-starting-points\data-binding et lancez le fichier PhotoLab.sln. 

2. Assurez-vous que votre plateforme de solution est définie sur x86 ou x64 (et non ARM), puis exécutez l’application. Cela vous montre l’état de l’application avec les espaces réservés de l’interface utilisateur, avant l'ajout des liaisons. 

    ![Application en cours d’exécution avec des images et du texte d'espace réservé](../design/basics/images/xaml-basics/gallery-with-placeholder-templates.png)

3. Ouvrez MainPage.xaml et recherchez un **DataTemplate** nommé **ImageGridView_DefaultItemTemplate**. Vous devez mettre à jour ce modèle pour utiliser des liaisons de données. 

    **Avant:**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate">
    ```

    La valeur **x:Key** est utilisée par l'objet **ImageGridView** pour sélectionner ce modèle pour l'affichage des objets de données. 

4. Ajoutez une valeur **x:DataType** au modèle. 

    **Après:**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate" 
                  x:DataType="local:ImageFileInfo">
    ```

    **x:DataType** indique le type auquel correspond le modèle. Dans ce cas, il s’agit d’un modèle pour la classe **ImageFileInfo** (où «local:» indique l’espace de noms local, comme défini dans une déclaration xmlns dans la partie supérieure du fichier).
    
    **x:DataType** est nécessaire lorsque vous utilisez des expressions **x:Bind** dans un modèle de données, comme expliqué ci-après. 

5. Dans le **DataTemplate**, recherchez l'élément **Image** nommé **ItemImage** et remplacez sa valeur **Source** comme indiqué. 

    **Avant:**
    ```xaml
    <Image x:Name="ItemImage" 
           Source="/Assets/StoreLogo.png" 
           Stretch="Uniform" />
    ```
    
    **Après:**
    ```xaml
    <Image x:Name="ItemImage" 
           Source="{x:Bind ImageSource}" 
           Stretch="Uniform" />
    ```
    
    **x:Name** identifie un élément XAML afin de pouvoir y faire référence ailleurs dans le code XAML et dans le code-behind. 

    Les expressions **x:Bind** fournissent une valeur à une propriété de l’interface utilisateur en obtenant la valeur à partir d’une propriété **data-object**. Dans les modèles, la propriété indiquée est une propriété de toute valeur définie pour **x:DataType**. Dans ce cas, la source de données est la propriété **ImageFileInfo.ImageSource**. 
    
    > [!NOTE] 
    > La valeur **x:Bind** permet également à l’éditeur de connaître le type de données, vous pouvez donc utiliser IntelliSense au lieu de taper le nom de propriété dans une expression **x:Bind**. Effectuez des tests dans le code que vous venez de coller: placez le curseur juste après **x:Bind** et appuyez sur la barre d’espace pour afficher la liste des propriétés avec lesquelles vous pouvez établir une liaison.

6. Remplacez les valeurs des autres contrôles d’interface utilisateur de la même façon. (Essayez de le faire avec IntelliSense au lieu de copier/coller!)

    **Avant:**
    ```xaml
    <TextBlock Text="Placeholder" ... />
    <StackPanel ... >
        <TextBlock Text="PNG file" ... />
        <TextBlock Text="50 x 50" ... />
    </StackPanel>
    <telerikInput:RadRating Value="3" ... />
    ```
    
    **Après:**
    ```xaml
    <TextBlock Text="{x:Bind ImageTitle}" ... />
    <StackPanel ... >
        <TextBlock Text="{x:Bind ImageFileType}" ... />
        <TextBlock Text="{x:Bind ImageDimensions}" ... />
    </StackPanel>
    <telerikInput:RadRating Value="{x:Bind ImageRating}" ... />
    ```

Exécutez l’application pour voir à quoi elle ressemble pour le moment. Il n'y a plus aucun espace réservé! Nous sommes bien partis. 

![Application en cours d’exécution avec des images réelles et du texte à la place des espaces réservés](../design/basics/images/xaml-basics/gallery-with-populated-templates.png)

> [!Note]
> Si vous souhaitez aller plus loin, essayez d’ajouter un nouveau TextBlock au modèle de données et utilisez le conseil x:Bind IntelliSense pour trouver une propriété à afficher. 

## <a name="part-2-use-binding-to-connect-the-gallery-ui-to-the-images"></a>Partie2: utiliser une liaison pour connecter l’interface utilisateur de galerie aux images

Ici, vous allez créer des liaisons à usage unique dans la page XAML pour connecter l’affichage de la galerie à la collection d’images, en remplaçant le code procédural existant qui effectue cette opération dans le code-behind. Vous allez également créer un bouton **Supprimer** pour voir comment l’affichage de galerie change lorsque les images sont supprimées de la collection. En même temps, vous allez apprendre à lier des événements aux gestionnaires d’événements pour bénéficier de davantage de souplesse que celle fournie par les gestionnaires d’événements classiques. 

Toutes les liaisons traitées jusqu’à présent se trouvent à l’intérieur de modèles de données et font référence aux propriétés de la classe indiquée par la valeur **x:DataType**. Qu’en est-il du reste du code XAML dans votre page? 

Les expressions **x:Bind** en dehors des modèles de données sont toujours liées à la page elle-même. Cela signifie que vous pouvez référencer tout ce que vous placez dans le code-behind ou déclarez en XAML, y compris les propriétés personnalisées et les propriétés d’autres contrôles d’interface utilisateur sur la page (dans la mesure où ils ont une valeur **x:Name**). 

Dans l’exemple PhotoLab, une utilisation possible d'une telle liaison consiste à connecter le contrôle principal **GridView** directement à la collection d’images, au lieu de le faire dans le code-behind. Vous verrez ensuite d’autres exemples. 

**Lier le contrôle GridView principal à la collection Images**

1. Dans MainPage.xaml.cs, recherchez la méthode **OnNavigatedTo** et supprimez le code qui définit **ItemsSource**.

    **Avant:**
    ```c#
    ImageGridView.ItemsSource = Images;
    ```

    **Après:**
    ```c#
    // Replaced with XAML binding:
    // ImageGridView.ItemsSource = Images;
    ```

2. Dans MainPage.xaml, recherchez le **GridView** nommé **ImageGridView** et ajoutez un attribut **ItemsSource**. Pour la valeur, utilisez une expression **x:Bind** qui fait référence à la propriété **Images** implémentée dans le code-behind. 

    **Avant:**
    ```xaml
    <GridView x:Name="ImageGridView" 
    ```

    **Après:**
    ```xaml
    <GridView x:Name="ImageGridView" 
              ItemsSource="{x:Bind Images}" 
    ```

    La propriété **Images** est de type **ObservableCollection\<ImageFileInfo\>**, donc chaque élément affiché dans le **GridView** est de type **ImageFileInfo**. Cela correspond à la valeur **x:DataType** décrite dans la partie1. 

Toutes les liaisons que nous avons examinées jusqu'à présent sont des liaisons à usage unique en lecture seule, ce qui est le comportement par défaut des expressions brutes **x:Bind**. Les données sont chargées uniquement lors de l’initialisation, ce qui donne des liaisons à hautes performances, idéales pour prendre en charge plusieurs vues complexes de jeux de données volumineux. 

Même la liaison **ItemsSource** que vous venez d'ajouter est une liaison à usage unique en lecture seule avec une valeur de propriété immuable, mais il y a une distinction importante à faire. La valeur immuable de la propriété **Images** est une instance spécifique d’une collection, initialisée une fois, comme illustré ici.

```Csharp
private ObservableCollection<ImageFileInfo> Images { get; }
    = new ObservableCollection<ImageFileInfo>();
```

La valeur de propriété **Images** ne change jamais, mais étant donné que la propriété est de type **ObservableCollection\<T\>**, le *contenu* de la collection peut changer, auquel cas la liaison remarque automatiquement les modifications et met à jour l’interface utilisateur. 

Pour tester cela, nous allons ajouter temporairement un bouton qui supprime l’image actuellement sélectionnée. Ce bouton ne se trouve pas dans la version définitive, car la sélection d’une image vous redirige vers une page de détails. Toutefois, le comportement de **ObservableCollection\<T\>** reste important dans l’exemple PhotoLab final, car le code XAML est initialisé dans le constructeur de page (par le biais de l’appel de méthode **InitializeComponent**), mais la collection **Images** est remplie ultérieurement dans la méthode **OnNavigatedTo**. 

**Ajouter un bouton de suppression**

1. Dans MainPage.xaml, recherchez la **CommandBar** nommée **MainCommandBar** et ajoutez un nouveau bouton avant le bouton de zoom. (Les contrôles de zoom ne fonctionnent pas encore. Vous allez les associer dans la partie suivante de ce didacticiel.)

    ```xaml
    <AppBarButton Icon="Delete"
                  Label="Delete selected image"
                  Click="{x:Bind DeleteSelectedImage}" />
    ```

    Si vous êtes déjà familiarisé avec le code XAML, cette valeur **Click** peut paraître inhabituelle. Dans les versions précédentes du langage XAML, vous deviez lui affecter une méthode avec une signature de gestionnaire d’événements spécifique, comprenant généralement des paramètres d’expéditeur de l’événement et un objet d'arguments d’événement spécifique. Vous pouvez toujours utiliser cette technique lorsque vous avez besoin d'arguments d’événement, mais avec x:Bind, vous pouvez également vous connecter à d’autres méthodes. Par exemple, si vous n'avez pas besoin de données d’événement, vous pouvez vous connecter à des méthodes qui n'ont pas de paramètres, comme nous le faisons ici.

    <!-- TODO add doc links about event binding - and doc links in general? -->

2. Dans MainPage.xaml.cs, ajoutez la méthode **DeleteSelectedImage**.

    ```c#
    private void DeleteSelectedImage() =>
        Images.Remove(ImageGridView.SelectedItem as ImageFileInfo);
    ```

    Cette méthode supprime simplement l’image sélectionnée à partir de la collection **Images**. 

Maintenant, exécutez l’application et utilisez le bouton pour supprimer quelques images. Comme vous pouvez le constater, l’interface utilisateur est mise à jour automatiquement, grâce à la liaison de données et au type **ObservableCollection\<T\>**. 

> [!Note]
> Par défi, essayez d’ajouter deux boutons qui déplacent l’image sélectionnée vers le haut ou vers le bas dans la liste, puis établissez une liaison x:Bind entre leurs événements Click et deux nouvelles méthodes similaires à DeleteSelectedImage.
 
## <a name="part-3-set-up-the-zoom-slider"></a>Partie3: configurer le curseur de zoom 

Dans cette partie, vous allez créer des liaisons à sens unique entre un contrôle dans le modèle de données et le curseur de zoom, qui se trouve en dehors du modèle. Vous apprendrez également que vous pouvez utiliser une liaison de données avec de nombreuses propriétés de contrôle, pas seulement celles les plus évidentes comme **TextBlock.Text** et **Image.Source**. 

**Lier le modèle de données d’image au curseur de zoom**

* Recherchez le **DataTemplate** nommé **ImageGridView_DefaultItemTemplate** et remplacez les valeurs **Hauteur** et **Largeur** du contrôle **Grid** en haut du modèle.

    **Avant**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate" 
                  x:DataType="local:ImageFileInfo">
        <Grid Height="200"
              Width="200"
              Margin="{StaticResource LargeItemMargin}">
    ```
    
    **Après**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate" 
                  x:DataType="local:ImageFileInfo">
        <Grid Height="{Binding Value, ElementName=ZoomSlider}"
              Width="{Binding Value, ElementName=ZoomSlider}"
              Margin="{StaticResource LargeItemMargin}">
    ```
    
    <!-- TODO talk about dependency properties --> 
    
    Avez-vous remarqué qu’il s’agit d'expressions **Binding** et non d'expressions **x:Bind**? Il s’agit de l’ancienne méthode pour effectuer des liaisons de données et elle est en grande partie obsolète. **x:Bind** fait presque tout ce que **Binding** fait et bien plus encore. Toutefois, lorsque vous utilisez **x:Bind** dans un modèle de données, celui-ci se lie au type déclaré dans la valeur **x:DataType**. Comment lier un élément du modèle à un élément de la page XAML ou du code-behind? Vous devez utiliser une ancienne expression **Binding**. 
    
    Les expressions **Binding** ne reconnaissent pas la valeur **x:DataType**, mais ces expressions **Binding** ont des valeurs **ElementName** qui fonctionnent de la même manière. Elles indiquent au moteur de liaison que **Binding Value** est une liaison avec la propriété **Value** de l’élément spécifié sur la page (autrement dit, l’élément avec cette valeur **x:Name**). Si vous souhaitez établir une liaison avec une propriété du code-behind, cela doit ressembler à ```{Binding MyCodeBehindProperty, ElementName=page}``` où **page** fait référence à la valeur **x:Name** définie dans l'élément **Page** en XAML. 
    
    > [!NOTE]
    > Par défaut, les expressions **Binding** sont à *sens* unique, ce qui signifie qu’elles mettent automatiquement à jour l’interface utilisateur lorsque la valeur de propriété liée change. 
    > 
    > En revanche, la valeur par défaut de **x:Bind** est à *usage* unique, ce qui signifie que toutes les modifications apportées à la propriété liée sont ignorées. Il s’agit de la valeur par défaut, car c'est l’option qui offre les plus hautes performances et la plupart des liaisons sont établies avec des données fixes en lecture seule. 
    >
    > La leçon qu'il faut en tirer est que si vous utilisez **x:Bind** avec des propriétés dont la valeur peut changer, vous devez ajouter ```Mode=OneWay``` ou ```Mode=TwoWay```. Vous trouverez des exemples dans la section suivante.

Exécutez l’application et utilisez le curseur pour modifier les dimensions du modèle d’image. Comme vous pouvez le constater, l’effet est assez puissant sans nécessiter beaucoup de code. 

![Application en cours d’exécution avec un curseur de zoom s'affichant](../design/basics/images/xaml-basics/gallery-with-zoom-control.png)

> [!NOTE]
> Par défi, essayez de lier d’autres propriétés de l’interface utilisateur à la propriété **Value** du curseur de zoom, ou à d’autres curseurs que vous ajoutez après le curseur de zoom. Par exemple, vous pouvez lier la propriété **FontSize** du **TitleTextBlock** à un nouveau curseur avec la valeur par défaut **24**. Veillez à définir des valeurs minimales et maximales raisonnables.

## <a name="part-4-improve-the-zoom-experience"></a>Partie4: améliorer l’expérience de zoom 

Dans cette partie, vous allez ajouter une propriété **ItemSize** personnalisée au code-behind et créer des liaisons à sens unique à partir du modèle d'image vers la nouvelle propriété. La valeur **ItemSize** sera mise à jour par le curseur de zoom, ainsi que par d’autres facteurs tels que le bouton bascule **Ajuster à l’écran** et la taille de fenêtre, pour permettre une plus grande précision. 

Contrairement aux propriétés de contrôle intégrées, vos propriétés personnalisées ne mettent pas automatiquement à jour l’interface utilisateur, même dans le cas de liaisons à sens unique et bidirectionnelles. Elles fonctionnent correctement avec des liaisons à **usage** unique, mais si vous souhaitez que vos modifications de propriété apparaissent réellement dans votre interface utilisateur, vous devez effectuer certaines opérations. 

**Créer la propriété ItemSize afin qu’elle mette à jour l’interface utilisateur**

1. Dans MainPage.xaml.cs, modifiez la signature de la classe **MainPage** afin qu’elle implémente l'interface **INotifyPropertyChanged**.

    **Avant:**
    ```c#
    public sealed partial class MainPage : Page
    ```

    **Après:**
    ```c#
    public sealed partial class MainPage : Page, INotifyPropertyChanged
    ```

    Cela indique au système de liaison que MainPage possède un événement PropertyChanged (ajouté ensuite) que les liaisons peuvent détecter pour mettre à jour l’interface utilisateur. 

2. Ajoutez un événement **PropertyChanged** à la classe **MainPage**.

    ```c#
    public event PropertyChangedEventHandler PropertyChanged;
    ```

    Cet événement fournit l’implémentation complète requise par l'interface **INotifyPropertyChanged**. Toutefois, pour que cela produise un effet, vous devez explicitement déclencher l’événement dans vos propriétés personnalisées. 

3. Ajoutez une propriété **ItemSize** et déclenchez l'événement **PropertyChanged** dans sa méthode setter.

    ```c#
    public double ItemSize
    {
        get => _itemSize;
        set
        {
            if (_itemSize != value)
            {
                _itemSize = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(ItemSize)));
            }
        }
    }
    private double _itemSize;
    ```

    La propriété **ItemSize** expose la valeur d’un champ **_itemSize** privé. L'utilisation d’un champ de sauvegarde comme celui-ci permet à la propriété de vérifier si une nouvelle valeur est identique à l’ancienne avant de déclencher un événement **PropertyChanged** potentiellement inutile.

    L’événement lui-même est déclenché par la méthode **Invoke**. Le point d’interrogation vérifie si l'événement **PropertyChanged** est null, c'est-à-dire, si des gestionnaires d’événements ont déjà été ajoutés. Chaque liaison à sens unique ou bidirectionnelle ajoute un gestionnaire d’événements en arrière-plan, mais si rien n'est détecté, il ne se produit rien de plus. Si toutefois **PropertyChanged** n’est pas null, la méthode **Invoke** est appelée avec une référence à la source d’événement (la page elle-même, représentée par le mot clé **this**) et un objet **event-args** qui indique le nom de la propriété. Grâce à ces informations, toutes les liaisons à sens unique ou bidirectionnelles avec la propriété **ItemSize** sont informées des modifications pour pouvoir mettre à jour l’interface utilisateur liée. 

4. Dans MainPage.xaml, recherchez le **DataTemplate** nommé **ImageGridView_DefaultItemTemplate** et remplacez les valeurs **Hauteur** et **Largeur** du contrôle **Grid** en haut du modèle. (Si vous avez effectué la liaison de contrôle à contrôle dans la partie précédente de ce didacticiel, les seules modifications à apporter consistent à remplacer **Value** par **ItemSize** et **ZoomSlider** par **page**. Veillez à faire cette opération à la fois pour Hauteur et Largeur!)

    **Avant**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate" 
                  x:DataType="local:ImageFileInfo">
        <Grid Height="{Binding Value, ElementName=ZoomSlider}"
            Width="{Binding Value, ElementName=ZoomSlider}"
            Margin="{StaticResource LargeItemMargin}">
    ```
    
    **Après**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate" 
                  x:DataType="local:ImageFileInfo">
        <Grid Height="{Binding ItemSize, ElementName=page}"
              Width="{Binding ItemSize, ElementName=page}"
              Margin="{StaticResource LargeItemMargin}">
    ```

Maintenant que l’interface utilisateur peut répondre aux modifications de **ItemSize**, vous devez effectuer quelques modifications. Comme indiqué précédemment, la valeur **ItemSize** est calculée à partir de l’état actuel de plusieurs contrôles d’interface utilisateur, mais le calcul doit être effectué à chaque fois que ces contrôles modifient l’état. Pour ce faire, vous allez utiliser une liaison d’événement pour que certaines modifications de l’interface utilisateur appellent une méthode d’assistance qui mette à jour **ItemSize**. 

**Mettre à jour la valeur de la propriété ItemSize**

1. Ajoutez la méthode **DetermineItemSize** à MainPage.xaml.cs.

    ```c#
    private void DetermineItemSize()
    {
        if (FitScreenToggle != null
            && FitScreenToggle.IsOn == true
            && ImageGridView != null
            && ZoomSlider != null)
        {
            // The 'margins' value represents the total of the margins around the
            // image in the grid item. 8 from the ItemTemplate root grid + 8 from
            // the ItemContainerStyle * (Right + Left). If those values change, 
            // this value needs to be updated to match.
            int margins = (int)this.Resources["LargeItemMarginValue"] * 4;
            double gridWidth = ImageGridView.ActualWidth -
                (int)this.Resources["DesktopWindowSidePaddingValue"];
    
            if (AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Mobile" &&
                UIViewSettings.GetForCurrentView().UserInteractionMode ==
                    UserInteractionMode.Touch)
            {
                margins = (int)this.Resources["SmallItemMarginValue"] * 4;
                gridWidth = ImageGridView.ActualWidth -
                    (int)this.Resources["MobileWindowSidePaddingValue"];
            }
    
            double ItemWidth = ZoomSlider.Value + margins;
            // We need at least 1 column.
            int columns = (int)Math.Max(gridWidth / ItemWidth, 1);
    
            // Adjust the available grid width to account for margins around each item.
            double adjustedGridWidth = gridWidth - (columns * margins);
    
            ItemSize = (adjustedGridWidth / columns);
        }
        else
        {
            ItemSize = ZoomSlider.Value;
        }
    }
    ```

2. Dans MainPage.xaml, naviguez vers le haut du fichier et ajoutez une liaison d’événement **SizeChanged** à l'élément **Page**.

    **Avant:**
    ```xaml
    <Page x:Name="page"  
    ```

    **Après:**
    ```xaml
    <Page x:Name="page" 
          SizeChanged="{x:Bind DetermineItemSize}"
    ```

3. Recherchez le **Slider** nommé **ZoomSlider** et ajoutez une liaison d’événement **ValueChanged**.

    **Avant:**
    ```xaml
    <Slider x:Name="ZoomSlider"
    ```

    **Après:**
    ```xaml
    <Slider x:Name="ZoomSlider"
            ValueChanged="{x:Bind DetermineItemSize}"
    ```

4. Recherchez le **ToggleSwitch** nommé **FitScreenToggle** et ajoutez une liaison d’événement **Toggled**.

    **Avant:**
    ```xaml
    <ToggleSwitch x:Name="FitScreenToggle"
    ```

    **Après:**
    ```xaml
    <ToggleSwitch x:Name="FitScreenToggle"
                  Toggled="{x:Bind DetermineItemSize}"
    ```

Exécutez l’application et utilisez le curseur de zoom et le bouton bascule **Ajuster à l’écran** pour modifier les dimensions du modèle d’image. Comme vous pouvez le constater, les dernières modifications activent une expérience de zoom/redimensionnement plus précise tout en conservant la bonne organisation du code. 

![Application en cours d’exécution avec la fonction ajuster à l’écran activée](../design/basics/images/xaml-basics/gallery-with-fit-to-screen.png)

> [!NOTE]
> Par défi, essayez d’ajouter un **TextBlock** après le **ZoomSlider** et de lier la propriété **Text** à la propriété **ItemSize**. Dans la mesure où il ne se trouve pas dans un modèle de données, vous pouvez utiliser **x:Bind** au lieu de **Binding** comme dans les liaisons **ItemSize** précédentes.  
}

## <a name="part-5-enable-user-edits"></a>Partie5: autoriser les modifications par l’utilisateur

Ici, vous allez créer des liaisons bidirectionnelles pour permettre aux utilisateurs de mettre à jour les valeurs, notamment le titre de l’image, l'évaluation et divers effets visuels. 

Pour ce faire, vous devez mettre à jour la **DetailPage** existante, qui fournit une visionneuse d’image unique, un contrôle de zoom et une interface utilisateur d'édition.  

Toutefois, vous devez d’abord associer la **DetailPage** pour que l’application accède à celle-ci lorsque l’utilisateur clique sur une image dans l'affichage de la galerie.

**Associer la DetailPage**

1. Dans MainPage.xaml, recherchez le **GridView** nommé **ImageGridView** et ajoutez une valeur **ItemClick**. 

    > [!TIP] 
    > Si vous tapez dans la modification ci-dessous au lieu de copier/coller, vous verrez s'afficher une fenêtre contextuelle IntelliSense indiquant «\<New Event Handler\>». Si vous appuyez sur la touche Tab, la valeur sera renseignée par un nom de gestionnaire de méthode par défaut et la méthode présentée dans l’étape suivante sera automatiquement remplacée. Vous pouvez ensuite appuyer sur F12 pour accéder à la méthode dans le code-behind. 

    **Avant:**
    ```xaml
    <GridView x:Name="ImageGridView"
    ```

    **Après:**
    ```xaml
    <GridView x:Name="ImageGridView"
              ItemClick="ImageGridView_ItemClick"
    ```

    > [!NOTE] 
    > Nous utilisons ici un gestionnaire d’événements classique au lieu d’une expression x:Bind. Ce, parce que nous avons besoin de voir les données d’événement, comme indiqué ci-après. 

2. Dans MainPage.xaml.cs, ajoutez le gestionnaire d’événements (ou renseignez-le, si vous avez utilisé l'astuce indiquée dans la dernière étape).

    ```c#
    private void ImageGridView_ItemClick(object sender, ItemClickEventArgs e)
    {
        this.Frame.Navigate(typeof(DetailPage), e.ClickedItem);
    }
    ```

    Cette méthode accède simplement à la page de détails, en transmettant le dernier élément cliqué, qui est un objet **ImageFileInfo** utilisé par **DetailPage.OnNavigatedTo** pour l'initialisation de la page. Vous n’aurez pas à mettre en œuvre cette méthode dans ce didacticiel, mais vous pouvez jeter un œil pour voir ce qu’elle fait. 
    
3. (Facultatif) Supprimez ou mettez en commentaires les contrôles que vous avez ajoutés dans les précédents points de lecture qui fonctionnent avec l’image actuellement sélectionnée. Les conserver n'aura pas d'effet nuisible, mais il sera désormais beaucoup plus difficile de sélectionner une image sans accéder à la page de détails. 

Maintenant que vous avez connecté les deux pages, exécutez l’application et observez le résultat. Tout fonctionne à l’exception des contrôles sur le volet d’édition, qui ne répondent pas lorsque vous essayez de modifier les valeurs. 

Comme vous pouvez le constater, la zone de texte du titre affiche le titre et vous permet d’entrer des modifications. Vous devez déplacer le focus sur un autre contrôle pour valider les modifications, mais le titre dans le coin supérieur gauche de l’écran ne se met pas encore à jour. 

Tous les contrôles sont déjà liés à l’aide des expressions brutes **x:Bind** que nous avons présentées dans la partie1. Si vous vous rappelez, cela signifie que ce sont toutes des liaisons à usage unique, ce qui explique pourquoi les modifications apportées aux valeurs ne sont pas enregistrées. Pour résoudre ce problème, il suffit de les transformer en liaisons bidirectionnelles. 

**Rendre les contrôles d’édition interactifs**

1. Dans DetailPage.xaml, recherchez le **TextBlock** nommé **TitleTextBlock** et le contrôle **RadRating** qui se trouve après et mettez à jour leurs expressions **x:Bind** pour inclure **Mode=TwoWay**.

    **Avant:**
    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               Text="{x:Bind item.ImageTitle}"
               ... >
    <telerikInput:RadRating Value="{x:Bind item.ImageRating}"
                            ... >
    ```

    **Après:**
    ```xaml
    <TextBlock x:Name="TitleTextBlock" 
               Text="{x:Bind item.ImageTitle, Mode=TwoWay}" 
               ... >
    <telerikInput:RadRating Value="{x:Bind item.ImageRating, Mode=TwoWay}" 
                            ... >
    ```

2. Faites de même pour tous les curseurs d'effet qui suivent le contrôle d’évaluation.

    ```xaml
    <Slider Header="Exposure"    ... Value="{x:Bind item.Exposure, Mode=TwoWay}" ...
    <Slider Header="Temperature" ... Value="{x:Bind item.Temperature, Mode=TwoWay}" ... 
    <Slider Header="Tint"        ... Value="{x:Bind item.Tint, Mode=TwoWay}" ... 
    <Slider Header="Contrast"    ... Value="{x:Bind item.Contrast, Mode=TwoWay}" ... 
    <Slider Header="Saturation"  ... Value="{x:Bind item.Saturation, Mode=TwoWay}" ...
    <Slider Header="Blur"        ... Value="{x:Bind item.Blur, Mode=TwoWay}" ... 
    ```

Le mode bidirectionnel signifie, comme on pourrait s’y attendre, que les données se déplacent dans les deux sens chaque fois que des modifications sont apportées de part et d'autre. 

Comme les liaisons à sens unique abordées précédemment, ces liaisons bidirectionnelles mettent désormais à jour l’interface utilisateur chaque fois que les propriétés liées changent, grâce à l'implémentation de **INotifyPropertyChanged** dans la classe **ImageFileInfo**. Avec une liaison bidirectionnelle, toutefois, les valeurs se déplacent également de l’interface utilisateur vers les propriétés liées à chaque fois que l’utilisateur interagit avec le contrôle. Rien d'autre n'est nécessaire du côté XAML. 

Exécutez l’application et essayez les contrôles d’édition. Comme vous pouvez le constater, lorsque vous apportez une modification, cela affecte désormais les valeurs de l’image, et ces modifications sont conservées lorsque vous revenez à la page principale. 

## <a name="part-6-format-values-through-function-binding"></a>Partie6: mettre en forme les valeurs par le biais d'une liaison de fonction

Il reste un dernier problème à résoudre. Lorsque vous déplacez les curseurs d'effet, les étiquettes en regard de ces curseurs ne changent toujours pas. 

![Curseurs d'effet avec des valeurs d'étiquette par défaut](../design/basics/images/xaml-basics/effect-sliders-before-label-fix.png)

La dernière partie de ce didacticiel consiste à ajouter des liaisons qui mettent en forme les valeurs de curseur pour l’affichage.

**Lier les étiquettes de curseur d’effet et mettre en forme les valeurs d'affichage**

1. Recherchez le **TextBlock** après le curseur **Exposure** et remplacez la valeur **Text** par l’expression de liaison indiquée ici.

    **Avant:**
    ```xaml
    <Slider Header="Exposure" ... />
    <TextBlock ... Text="0.00" />
    ```

    **Après:**
    ```xaml
    <Slider Header="Exposure" ... />
    <TextBlock ... Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
    ```

    Il s’agit d’une fonction de liaison, car vous établissez une liaison avec la valeur de retour d’une méthode. La méthode doit être accessible par le biais du code-behind de la page ou du type **x:DataType** si vous êtes dans un modèle de données. Dans ce cas, il faut utiliser la méthode familière .NET **ToString**, accessible via la propriété d’élément de la page, puis par le biais de la propriété **Exposure** de l’élément. (Cet exemple illustre comment vous pouvez établir des liaisons avec des méthodes et des propriétés qui sont profondément imbriquées dans une chaîne de connexions.)

    La liaison de fonction est un moyen idéal pour mettre en forme des valeurs d’affichage, car vous pouvez transmettre d’autres sources de liaison comme arguments de méthode et l’expression de liaison détectera les modifications apportées à ces valeurs comme prévu avec le mode à sens unique. Dans cet exemple, l'argument **culture** est une référence à un champ inaltérable implémenté dans le code-behind, mais il aurait pu s'agir tout aussi facilement d'une propriété qui déclenche des événements **PropertyChanged**. Dans ce cas, toute modification apportée à la valeur de propriété fait que l'expression **x:Bind** appelle **ToString** avec la nouvelle valeur et met à jour l’interface utilisateur avec le résultat. 

2. Faites de même pour les **TextBlocks** qui étiquettent les autres curseurs d'effet.

    ```xaml
    <Slider Header="Temperature" ... />
    <TextBlock ... Text="{x:Bind item.Temperature.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Tint" ... />
    <TextBlock ... Text="{x:Bind item.Tint.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Contrast" ... />
    <TextBlock ... Text="{x:Bind item.Contrast.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Saturation" ... />
    <TextBlock ... Text="{x:Bind item.Saturation.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Blur" ... />
    <TextBlock ... Text="{x:Bind item.Blur.ToString('N', culture), Mode=OneWay}" />
    ```

Désormais, lorsque vous exécutez l’application, tout fonctionne, y compris les étiquettes de curseur. 

![Curseurs d'effet avec des étiquettes qui fonctionnent](../design/basics/images/xaml-basics/effect-sliders-after-label-fix.png)

> [!NOTE]
> Essayez d’utiliser la liaison de fonction avec le **TextBlock** à partir du dernier point de lecture et liez-le à une nouvelle méthode qui retourne une chaîne au format «000 x 000» lorsque vous lui transmettez la valeur **ItemSize**.


## <a name="conclusion"></a>Conclusion

Ce didacticiel vous a donné un aperçu de la liaison de données et vous a montré quelques-unes des fonctionnalités disponibles. Un mot d’avertissement avant de conclure: toutes les liaisons ne sont pas possibles et parfois les valeurs que vous essayez de connecter sont incompatibles avec les propriétés que vous essayez de lier. La liaison offre une grande souplesse, mais elle ne fonctionnera pas dans tous les cas.

Un exemple de problème non résolu par une liaison est le cas où un contrôle ne possède aucune propriété appropriée pour la liaison, comme avec la fonctionnalité de zoom de la page de détails. Ce curseur de zoom doit interagir avec le **ScrollViewer** qui affiche l’image, mais **ScrollViewer** ne peut être mis à jour que par le biais de sa méthode **ChangeView**. Dans ce cas, nous utilisons les gestionnaires d’événements classiques pour assurer la synchronisation du **ScrollViewer** et du curseur de zoom. Voir les méthodes **DetailPage**, **ZoomSlider_ValueChanged** et **MainImageScroll_ViewChanged** pour plus d’informations.

Néanmoins, la liaison est un moyen puissant et souple de simplifier votre code et d'établir une distinction entre la logique de votre interface utilisateur et votre logique de données. Cela facilite grandement les réglages de part et d'autre de cette division, tout en limitant les risques d’introduire des bogues de l’autre côté. 

Un exemple de séparation des données et de l’interface utilisateur est l'utilisation de la propriété **ImageFileInfo.ImageTitle**. Cette propriété (et la propriété **ImageRating**) est légèrement différente de la propriété **ItemSize** que vous avez créée dans la partie4, car la valeur est stockée dans les métadonnées du fichier (exposées par le biais du type **ImageProperties**) plutôt que dans un champ. En outre, **ImageTitle** retourne la valeur **ImageName** (définie sur le nom de fichier) s’il n’existe aucun titre dans les métadonnées du fichier. 

```c#
public string ImageTitle
{
    get => String.IsNullOrEmpty(ImageProperties.Title) ? ImageName : ImageProperties.Title;
    set
    {
        if (ImageProperties.Title != value)
        {
            ImageProperties.Title = value;
            var ignoreResult = ImageProperties.SavePropertiesAsync();
            OnPropertyChanged();
        }
    }
}
```

Comme vous pouvez le constater, la méthode setter met à jour la propriété **ImageProperties.Title**, puis appelle **SavePropertiesAsync** pour écrire la nouvelle valeur dans le fichier. (Il s’agit d’une méthode asynchrone, mais nous ne pouvons pas utiliser le mot clé **await** dans une propriété, ce qui n'est de toute façon pas souhaitable puisque les accesseurs Get et les méthodes setter de propriété doivent se terminer immédiatement. Donc, à la place, vous appelez la méthode et ignorez l'objet **Task** qu'elle retourne.) 

<!-- TODO more screenshots? -->

## <a name="going-further"></a>Aller plus loin

Maintenant que vous avez terminé ce laboratoire, vous avez suffisamment de connaissances en liaison pour résoudre les problèmes par vous-même.

Comme vous l'avez peut-être remarqué, si vous modifiez le niveau de zoom sur la page de détails, il se réinitialise automatiquement lorsque vous naviguez vers l’arrière, puis sélectionnez de nouveau la même image. Pouvez-vous trouver comment conserver et restaurer le niveau de zoom de chaque image individuellement? Bonne chance!
    
Vous disposez normalement de toutes les informations nécessaires dans ce didacticiel, mais si vous avez besoin de plus de conseils, il suffit d'un clic pour accéder à la documentation sur la liaison de données. Commencez ici:

+ [Extension de balisage {x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)
+ [Présentation détaillée de la liaison de données](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)