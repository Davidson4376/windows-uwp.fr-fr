---
title: 'Tutoriel : Créer une interface utilisateur'
description: Cet article décrit les principes fondamentaux de la création d’interfaces utilisateur en XAML
keywords: XAML, UWP, Bien démarrer
ms.date: 08/30/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d4da7e661b0a9c1f389ae93fe4da4782629014f4
ms.sourcegitcommit: 51d884c3646ba3595c016e95bbfedb7ecd668a88
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67820522"
---
# <a name="tutorial-create-a-user-interface"></a>Tutoriel : Créer une interface utilisateur

Dans ce tutoriel, vous allez apprendre à créer une interface utilisateur de base pour un programme de retouche d’images. Pour ce faire, vous allez effectuer plusieurs opérations : 

+ Utilisation des outils XAML de Visual Studio, par exemple, le concepteur XAML, la Boîte à outils, l’éditeur XAML, le panneau Propriétés et la Structure du document pour ajouter des contrôles et du contenu à votre interface utilisateur
+ Utilisation de certains des panneaux de disposition XAML les plus courants, tels que RelativePanel, Grid et StackPanel.

Le programme de retouche d’images comporte deux pages/écrans :

La **page principale**, qui présente un affichage de galerie de photos, ainsi que des informations sur chaque fichier image.

![MainPage](images/xaml-basics/mainpage.png)

La **page de détails**, qui affiche une seule photo une fois qu’elle a été sélectionnée. Un menu d’édition volant permet de modifier la photo, de la renommer et de l’enregistrer.

![DetailPage](images/xaml-basics/detailpage.png)


## <a name="prerequisites"></a>Conditions préalables

* Visual Studio 2019 : [Télécharger Visual Studio 2019 Community (gratuit)](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15&campaign=WinDevCenter&ocid=wdgcx-windevcenter-community-download) 
* SDK Windows 10 (10.0.15063.468 ou version ultérieure) :  [Télécharger le SDK Windows le plus récent (gratuit)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)

## <a name="part-0-get-the-starter-code-from-github"></a>Partie 0 : Obtenir le code de démarrage à partir de GitHub

Dans ce tutoriel, vous allez commencer avec une version simplifiée de l’exemple PhotoLab. 

1. Accédez à [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab). Vous accédez alors à la page GitHub de l’exemple. 
2. Vous devez ensuite cloner ou télécharger l’exemple. Cliquez sur le bouton **Cloner ou télécharger**. Un sous-menu s’affiche.
    <figure>
        <img src="images/xaml-basics/clone-repo.png" alt="The Clone or download menu on GitHub">
        <figcaption>Le menu <b>Cloner ou télécharger</b> dans la page GitHub de l’exemple PhotoLab.</figcaption>
    </figure>

    **Si vous n’êtes pas familiarisé avec GitHub :**
    
    a. Cliquez sur **Télécharger le zip**, puis enregistrez le fichier localement. Cela télécharge un fichier .zip contenant tous les fichiers projet dont vous avez besoin.
    b. Extrayez le fichier. Utilisez l’Explorateur de fichiers pour accéder au fichier .zip que vous venez de télécharger, cliquez dessus avec le bouton droit, puis sélectionnez **Extraire tout...** c. Accédez à votre copie locale de l’exemple, puis au répertoire `Windows-appsample-photo-lab-master\xaml-basics-starting-points\user-interface`.    

    **Si vous maîtrisez GitHub :**

    a. Clonez localement la branche maîtresse du dépôt.
    b. Accédez au répertoire `Windows-appsample-photo-lab\xaml-basics-starting-points\user-interface`.

3. Ouvrez le projet en cliquant sur `Photolab.sln`.

## <a name="part-1-add-a-textblock-using-xaml-designer"></a>Partie 1 : Ajouter un contrôle TextBlock à l’aide du concepteur XAML

Visual Studio fournit plusieurs outils pour faciliter la création de votre interface utilisateur XAML. Le concepteur XAML vous permet de faire glisser des contrôles sur l’aire de conception et de voir à quoi ils ressemblent avant d’exécuter l’application. Le panneau Propriétés vous permet d’afficher et de définir toutes les propriétés du contrôle qui sont actives dans le concepteur. La Structure du document montre la structure parent-enfant de l’arborescence visuelle XAML de votre interface utilisateur. L’éditeur XAML vous permet d’entrer et de modifier directement le balisage XAML.

Voici l’interface utilisateur de Visual Studio avec les outils étiquetés.

![Disposition Visual Studio](images/xaml-basics/visual-studio-tools.png)

Chacun de ces outils facilitant la création de votre interface utilisateur, nous allons tous les utiliser dans ce tutoriel. Vous allez commencer par utiliser le concepteur XAML pour ajouter un contrôle. 

**Ajouter un contrôle à l’aide du concepteur XAML :**

1. Dans l’Explorateur de solutions, double-cliquez sur **MainPage.xaml** pour l’ouvrir. Cela affiche la page principale de l’application sans aucun élément d’interface utilisateur ajouté.

2. Avant d’aller plus loin, vous devez apporter quelques ajustements à Visual Studio.

    - Assurez-vous que la plateforme de solution est définie sur x86 ou x64 (et non ARM).
    - Définissez le concepteur XAML de la page principale pour afficher l’aperçu du Bureau 13,3".

    Vous devez voir les deux paramètres dans la partie supérieure de la fenêtre, comme illustré ici.

    ![Paramètres VS](images/xaml-basics/layout-vs-settings.png)

    Vous pouvez exécuter l’application maintenant, mais vous ne verrez pas grand-chose. Nous allons ajouter quelques éléments d’interface utilisateur pour rendre les choses plus intéressantes.

3. Dans la Boîte à outils, développez **Contrôles XAML communs** et recherchez le contrôle [TextBlock](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock). Faites glisser un contrôle TextBlock sur l’aire de conception près du coin supérieur gauche de la page.

    TextBlock est ajouté à la page et le concepteur définit des propriétés en fonction de ce qu’il estime le mieux adapté à la disposition souhaitée. Une surbrillance bleue apparaît autour du TextBlock pour indiquer qu’il est désormais l’objet actif. Remarquez les marges et les autres paramètres ajoutés par le concepteur. Votre code XAML doit ressembler à ceci. Ne vous inquiétez pas s’il n’a pas exactement le même format. Nous l’avons abrégé pour en faciliter la lecture.

    ```xaml
    <TextBlock x:Name="textBlock"
               HorizontalAlignment="Left"
               Margin="351,44,0,0"
               TextWrapping="Wrap"
               Text="TextBlock"
               VerticalAlignment="Top"/>
    ```

    Dans les étapes suivantes, vous allez mettre à jour ces valeurs.

4. Dans le panneau Propriétés, modifiez la valeur Name du TextBlock de **textBlock** en **TitleTextBlock**. (Assurez-vous que le contrôle TextBlock est toujours l’objet actif.)

5. Sous **Common**, remplacez la valeur Text par **Collection**.

    ![Propriétés de TextBlock](images/xaml-basics/text-block-properties.png)

    Dans l’éditeur XAML, votre code XAML se présente désormais comme suit.

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               HorizontalAlignment="Left"
               Margin="351,44,0,0"
               TextWrapping="Wrap"
               Text="Collection"
               VerticalAlignment="Top"/>
    ```

6. Pour positionner le TextBlock, vous devez tout d’abord supprimer les valeurs de propriété qui ont été ajoutées par Visual Studio. Dans Structure du document, cliquez avec le bouton droit sur **TitleTextBlock**, puis sélectionnez **Disposition > Tout réinitialiser**.

![Structure du document](images/xaml-basics/doc-outline-reset.png)

7. Dans le panneau Propriétés, entrez **margin** dans la zone de recherche pour trouver facilement la propriété **Margin**. Définissez les marges à gauche et en bas à 24.

    ![Marges de TextBlock](images/xaml-basics/margins.png)

    Les marges sont le moyen le plus simple pour positionner un élément sur la page. Elles sont utiles pour optimiser votre disposition, mais l’utilisation de valeurs de marge importantes, comme celles ajoutées par Visual Studio, complique l’adaptation de votre interface utilisateur à différentes tailles d’écran et doit donc être évitée.

    Pour plus d’informations, consultez [Alignement, marges et espacement](../layout/alignment-margin-padding.md).

8. Dans le panneau Structure du document, cliquez avec le bouton droit sur **TitleTextBlock**, puis sélectionnez **Modifier le style > Appliquer la ressource > TitleTextBlockStyle**. Cela applique un style défini par le système au texte du titre.

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               TextWrapping="Wrap"
               Text="Collection"
               Margin="24,0,0,24"
               Style="{StaticResource TitleTextBlockStyle}"/>
    ```

9. Dans le panneau Propriétés, entrez **textwrapping** dans la zone de recherche pour trouver facilement la propriété **TextWrapping**. Cliquez sur le _marqueur de propriété_ de la propriété **TextWrapping** pour ouvrir son menu. (Le _marqueur de propriété_ est le symbole de petite case situé à droite de chaque valeur de propriété. Le _marqueur de propriété_ est noir pour indiquer que la propriété est définie sur une valeur autre que par défaut.) Dans le menu **Propriété**, sélectionnez **Réinitialiser** pour réinitialiser la propriété TextWrapping.

    Visual Studio ajoute cette propriété, mais comme elle est déjà définie dans le style que vous avez appliqué, vous n’en avez pas besoin ici.

Vous avez ajouté la première partie de l’interface utilisateur à votre application ! Exécutez l’application maintenant pour voir à quoi elle ressemble.

Vous avez peut-être remarqué que, dans le concepteur XAML, votre application affichait un texte blanc sur un arrière-plan noir mais, lorsque vous l’avez exécutée, elle a affiché un texte noir sur fond blanc. C’est parce que Windows possède à la fois un thème sombre et clair, et que le thème par défaut varie selon l’appareil. Sur un PC, le thème par défaut est Clair. Vous pouvez cliquer sur l’icône d’engrenage en haut du concepteur XAML pour ouvrir les Paramètres d’aperçu de l’appareil et régler le thème sur Clair afin que l’application dans le concepteur XAML ait le même aspect que sur votre PC.

> [!NOTE]
> Dans cette partie du tutoriel, vous avez ajouté un contrôle en faisant un glisser-déplacer. Vous pouvez aussi ajouter un contrôle en double-cliquant dessus dans la Boîte à outils. Essayez et regardez les différences dans le code XAML généré par Visual Studio.

## <a name="part-2-add-a-gridview-control-using-the-xaml-editor"></a>Partie 2 : Ajouter un contrôle GridView à l’aide de l’éditeur XAML

Dans la partie 1, vous avez eu une idée de l’utilisation du concepteur XAML et de certains des autres outils fournis par Visual Studio. Ici, vous allez utiliser l’éditeur XAML pour travailler directement avec le balisage XAML. À mesure que vous vous familiarisez avec XAML, vous trouverez sans doute que c’est un moyen plus efficace de travailler.

Tout d’abord, vous remplacerez la disposition racine [Grid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) par un [**RelativePanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel). Le RelativePanel facilite la réorganisation des blocs de l’interface utilisateur par rapport au panneau ou à d’autres éléments de l’interface utilisateur. Vous constaterez son utilité dans le tutoriel [Disposition adaptative en XAML](xaml-basics-adaptive-layout.md). 

Ensuite, vous allez ajouter un contrôle [GridView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview) pour afficher vos données.

**Ajouter un contrôle à l’aide de l’éditeur XAML**

1. Dans l’éditeur XAML, remplacez la racine **Grid** par un **RelativePanel**.

    **Avant**
    ```xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
          <TextBlock x:Name="TitleTextBlock"
                     Text="Collection"
                     Margin="24,0,0,24"
                     Style="{StaticResource TitleTextBlockStyle}"/>
    </Grid>
    ```

    **Après**
    ```xaml
    <RelativePanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock x:Name="TitleTextBlock"
                   Text="Collection"
                   Margin="24,0,0,24"
                   Style="{StaticResource TitleTextBlockStyle}"/>
    </RelativePanel>
    ```

    Pour plus d’informations sur les dispositions utilisant un **RelativePanel**, consultez [Panneaux de disposition](https://docs.microsoft.com/windows/uwp/layout/layout-panels#relativepanel).

2. Sous l’élément **TextBlock**, ajoutez un **contrôle GridView** nommé « ImageGridView ». Définissez les _propriétés jointes_ **RelativePanel** pour placer le contrôle sous le texte du titre et l’étirer sur toute la largeur de l’écran.

    **Ajoutez ce code XAML**

    ```xaml
    <GridView x:Name="ImageGridView"
              Margin="0,0,0,8"
              RelativePanel.AlignLeftWithPanel="True"
              RelativePanel.AlignRightWithPanel="True"
              RelativePanel.Below="TitleTextBlock"/>
    ```

    **Après le TextBlock**
    ```xaml
    <RelativePanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock x:Name="TitleTextBlock"
                   Text="Collection"
                   Margin="24,0,0,24"
                   Style="{StaticResource TitleTextBlockStyle}"/>
        
        <!-- Add the GridView here. -->

    </RelativePanel>
    ```

    Pour plus d’informations sur les propriétés jointes du panneau, consultez [Panneaux de disposition](https://docs.microsoft.com/windows/uwp/layout/layout-panels).

3. Pour que le **GridView** affiche quelque chose, vous devez lui donner une collection de données à afficher. Ouvrez MainPage.xaml.cs et recherchez la méthode **GetItemsAsync**. Cette méthode remplit une collection appelée Images, qui est une propriété que nous avons ajoutée à MainPage.

    Après la boucle **foreach** dans **GetItemsAsync**, ajoutez cette ligne de code.

    ```csharp
    ImageGridView.ItemsSource = Images;
    ```

    Cela définit la propriété **ItemsSource** du GridView sur la collection **Images** de l’application et donne au **GridView** quelque chose à afficher.

C’est le moment idéal pour exécuter l’application et vérifier que tout fonctionne. Voici à quoi cela doit ressembler.

![Point de contrôle de l’interface utilisateur de l’application 1](images/xaml-basics/layout-0.png)

Vous remarquerez que l’application ne montre pas encore d’images. Par défaut, elle affiche la valeur ToString du type de données qui se trouvent dans la collection. Ensuite, vous allez créer un modèle de données pour définir la façon dont les données s’affichent.

> [!NOTE]
> Vous pouvez en savoir plus sur les dispositions utilisant un **RelativePanel** dans l’article [Panneaux de disposition](https://docs.microsoft.com/windows/uwp/layout/layout-panels#relativepanel). Jetez un coup d’œil, puis essayez quelques dispositions différentes en définissant les propriétés jointes RelativePanel sur le **TextBlock** et sur **GridView**.

## <a name="part-3-add-a-datatemplate-to-display-your-data"></a>Partie 3 : Ajouter un DataTemplate pour afficher vos données

Maintenant, vous allez créer un [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate) qui indique au GridView comment afficher vos données. Pour obtenir une explication exhaustive des modèles de données, consultez [Conteneurs et modèles d’éléments](../controls-and-patterns/item-containers-templates.md).

Pour l’instant, vous allez seulement ajouter des espaces réservés pour vous aider à créer la disposition souhaitée. Dans le tutoriel [Liaison de données XAML](../../data-binding/xaml-basics-data-binding.md), vous allez remplacer ces espaces réservés par des données réelles à partir de la classe **ImageFileInfo**. Vous pouvez ouvrir le fichier ImageFileInfo.cs maintenant si vous souhaitez voir à quoi ressemble l’objet de données.

**Ajouter un modèle de données à un affichage Grille**

1. Ouvrez MainPage.xaml.

2. Pour afficher l’évaluation, vous utilisez le contrôle **RadRating** à partir du package NuGet [Telerik UI for UWP](https://github.com/telerik/UI-For-UWP). Ajoutez une référence d’espace de noms XAML qui spécifie l’espace de noms pour les contrôles Telerik. Placez-la dans la balise d’ouverture **Page**, juste après les autres entrées « xmlns: ».

    **Ajoutez ce code XAML**

    ```xaml
    xmlns:telerikInput="using:Telerik.UI.Xaml.Controls.Input"
    ```

    **Après la dernière entrée « xmlns: »**

    ```xaml
    <Page x:Name="page"
      x:Class="PhotoLab.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:PhotoLab"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:telerikInput="using:Telerik.UI.Xaml.Controls.Input"
      mc:Ignorable="d"
      NavigationCacheMode="Enabled">
    ```

    Pour plus d’informations sur les espaces de noms XAML, consultez [Espaces de noms XAML et mappage d’espaces de noms](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-namespaces-and-namespace-mapping).

3. Dans Structure du document, cliquez avec le bouton droit sur **ImageGridView**. Dans le menu contextuel, sélectionnez **Modifier les modèles supplémentaires > Modifier les éléments générés (ItemTemplate) > Créer vide...** . La boîte de dialogue **Créer une ressource** s’ouvre.

4. Dans la boîte de dialogue, remplacez la valeur de Nom (clé) par **ImageGridView_DefaultItemTemplate**, puis cliquez sur **OK**.

    Plusieurs choses se produisent lorsque vous cliquez sur **OK**.

    - Un **DataTemplate** est ajouté à la section Page.Resources de MainPage.xaml.

        ```xaml
        <Page.Resources>
            <DataTemplate x:Key="ImageGridView_DefaultItemTemplate">
                <Grid/>
            </DataTemplate>
        </Page.Resources>
        ```

    - L’étendue de la Structure du document est définie sur ce **DataTemplate**.

        Lorsque vous avez terminé la création du modèle de données, vous pouvez cliquer sur la flèche vers le haut dans le coin supérieur gauche de la Structure du document pour revenir à l’étendue de la page.

    - La propriété **ItemTemplate** du contrôle GridView est définie sur la ressource **DataTemplate**.

    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"/>
    ```

5. Dans la ressource **ImageGridView_DefaultItemTemplate**, donnez à la racine **Grid** une hauteur et une largeur de **300**, et une marge de **8**. Ajoutez deux lignes, puis définissez la hauteur de la deuxième ligne sur **Automatique**.

    **Avant**
    ```xaml
    <Grid/>
    ```

    **Après**
    ```xaml
    <Grid Height="300"
          Width="300"
          Margin="8">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
    </Grid>
    ```

    Pour plus d’informations sur les dispositions de grille, consultez [Panneaux de disposition](https://docs.microsoft.com/windows/uwp/layout/layout-panels#grid).

6. Ajoutez des contrôles à la grille.

    a. Ajoutez un contrôle **Image** dans la première ligne de grille. C’est là que l’image s’affichera mais, pour le moment, vous allez utiliser le logo Store de l’application comme espace réservé.

    b. Ajoutez des contrôles **TextBlock** pour afficher le nom, le type de fichier et les dimensions de l’image. Pour ce faire, vous utilisez des contrôles **StackPanel** pour organiser les blocs de texte.

    Pour plus d’informations sur la disposition **StackPanel**, consultez [Panneaux de disposition](https://docs.microsoft.com/windows/uwp/layout/layout-panels#stackpanel)

    c. Ajoutez le contrôle **RadRating** au **StackPanel** externe (vertical). Placez-le après le **StackPanel** interne (horizontal).

    **Modèle définitif**

    ```xaml
    <Grid Height="300"
          Width="300"
          Margin="8">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Image x:Name="ItemImage"
               Source="Assets/StoreLogo.png"
               Stretch="Uniform" />

        <StackPanel Orientation="Vertical"
                    Grid.Row="1">
            <TextBlock Text="ImageTitle"
                       HorizontalAlignment="Center"
                       Style="{StaticResource SubtitleTextBlockStyle}" />
            <StackPanel Orientation="Horizontal"
                        HorizontalAlignment="Center">
                <TextBlock Text="ImageFileType"
                           HorizontalAlignment="Center"
                           Style="{StaticResource CaptionTextBlockStyle}" />
                <TextBlock Text="ImageDimensions"
                           HorizontalAlignment="Center"
                           Style="{StaticResource CaptionTextBlockStyle}"
                           Margin="8,0,0,0" />
            </StackPanel>

            <telerikInput:RadRating Value="3"
                                    IsReadOnly="True">
                <telerikInput:RadRating.FilledIconContentTemplate>
                    <DataTemplate>
                        <SymbolIcon Symbol="SolidStar"
                                    Foreground="White" />
                    </DataTemplate>
                </telerikInput:RadRating.FilledIconContentTemplate>
                <telerikInput:RadRating.EmptyIconContentTemplate>
                    <DataTemplate>
                        <SymbolIcon Symbol="OutlineStar"
                                    Foreground="White" />
                    </DataTemplate>
                </telerikInput:RadRating.EmptyIconContentTemplate>
            </telerikInput:RadRating>

        </StackPanel>
    </Grid>
    ```

Exécutez l’application maintenant pour voir le **GridView** avec le modèle d’élément que vous venez de créer. Il se peut que vous ne voyiez pas le contrôle d’évaluation, parce qu’il présente des étoiles blanches sur un arrière-plan blanc. Vous allez modifier la couleur d’arrière-plan par la suite.

![Point de contrôle de l’interface utilisateur de l’application 2](images/xaml-basics/layout-1.png)

## <a name="part-4-modify-the-item-container-style"></a>Partie 4 : Modifier le style du conteneur d’éléments

Un modèle de contrôle d’un élément contient les visuels qui affichent l’état, tels que la sélection, le pointage et le focus. Ces visuels sont générés au-dessus ou en dessous du modèle de données. Ici, vous allez modifier les propriétés **Background** et **Margin** du modèle de contrôle afin de donner aux éléments de **GridView** un arrière-plan grisé.

**Modifier le conteneur d’éléments**

1. Dans Structure du document, cliquez avec le bouton droit sur **ImageGridView**. Dans le menu contextuel, sélectionnez **Modifier les modèles supplémentaires > Modifier le conteneur d’éléments généré (ItemContainerStyle) > Modifier une copie...** . La boîte de dialogue **Créer une ressource** s’ouvre.

2. Dans la boîte de dialogue, remplacez la valeur de Nom (clé) par **ImageGridView_DefaultItemContainerStyle**, puis cliquez sur **OK**.

    Une copie du style par défaut est ajoutée à la section **Page.Resources** de votre code XAML.

    ```xaml
    <Style x:Key="ImageGridView_DefaultItemContainerStyle" TargetType="GridViewItem">
        <Setter Property="FontFamily" Value="{ThemeResource ContentControlThemeFontFamily}"/>
        <Setter Property="FontSize" Value="{ThemeResource ControlContentThemeFontSize}"/>
        <Setter Property="Background" Value="{ThemeResource GridViewItemBackground}"/>
        <Setter Property="Foreground" Value="{ThemeResource GridViewItemForeground}"/>
        <Setter Property="TabNavigation" Value="Local"/>
        <Setter Property="IsHoldingEnabled" Value="True"/>
        <Setter Property="HorizontalContentAlignment" Value="Center"/>
        <Setter Property="VerticalContentAlignment" Value="Center"/>
        <Setter Property="Margin" Value="0,0,4,4"/>
        <Setter Property="MinWidth" Value="{ThemeResource GridViewItemMinWidth}"/>
        <Setter Property="MinHeight" Value="{ThemeResource GridViewItemMinHeight}"/>
        <Setter Property="AllowDrop" Value="False"/>
        <Setter Property="UseSystemFocusVisuals" Value="True"/>
        <Setter Property="FocusVisualMargin" Value="-2"/>
        <Setter Property="FocusVisualPrimaryBrush" Value="{ThemeResource GridViewItemFocusVisualPrimaryBrush}"/>
        <Setter Property="FocusVisualPrimaryThickness" Value="2"/>
        <Setter Property="FocusVisualSecondaryBrush" Value="{ThemeResource GridViewItemFocusVisualSecondaryBrush}"/>
        <Setter Property="FocusVisualSecondaryThickness" Value="1"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="GridViewItem">
                <!-- XAML removed for clarity
                    <ListViewItemPresenter ... />
                -->   
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
    ```

    Le style par défaut **GridViewItem** définit un grand nombre de propriétés. Vous devez toujours commencer par une copie du style par défaut et modifier uniquement les propriétés nécessaires. Dans le cas contraire, les visuels risquent de ne pas s’afficher comme vous le souhaitez, car certaines propriétés n’auront pas été définies correctement.

    De plus, comme dans l’étape précédente, la propriété **ItemContainerStyle** du GridView est définie sur la nouvelle ressource **Style**.

    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"
                  ItemContainerStyle="{StaticResource ImageGridView_DefaultItemContainerStyle}"/>
    ```

3. Remplacez la valeur de la propriété **Background** par **Gris**.

    **Avant**
    ```xaml
        <Setter Property="Background" Value="{ThemeResource GridViewItemBackground}"/>
    ```

    **Après**
    ```xaml
        <Setter Property="Background" Value="Gray"/>
    ```

4. Remplacez la valeur de la propriété **Margin** par **8**.

    **Avant**
    ```xaml
        <Setter Property="Margin" Value="0,0,4,4"/>
    ```

    **Après**
    ```xaml
        <Setter Property="Margin" Value="8"/>
    ```

Exécutez l’application et regardez à quoi elle ressemble désormais. Redimensionnez la fenêtre de l’application. Le **GridView** s’occupe de réorganiser les images pour vous mais, pour certaines largeurs, il reste beaucoup d’espace sur le côté droit de la fenêtre d’application. Il serait préférable de centrer les images. Nous allons nous en occuper après.

![Point de contrôle de l’interface utilisateur de l’application 3](images/xaml-basics/layout-2.png)

> [!Note]
> Si vous souhaitez faire des essais, essayez de définir les propriétés Background et Margin sur des valeurs différentes pour en voir l’effet.

## <a name="part-5-apply-some-final-adjustments-to-the-layout"></a>Partie 5 : Appliquer quelques derniers ajustements à la disposition

Pour centrer les images dans la page, vous devez régler l’alignement de la grille dans la page. Avez-vous besoin de régler l’alignement des images dans le **GridView** ? Est-ce important ? Voyons voir.

Pour plus d’informations sur l’alignement, consultez [Alignement, marges et espacement](../layout/alignment-margin-padding.md).

(Vous pouvez essayer de paramétrer le **Background** du **GridView** sur votre couleur préférée dans cette étape. Cela vous permet de voir plus clairement ce qui se passe dans la disposition.)

**Modifier l’alignement des images**

1. Dans le **Gridview**, définissez la propriété **HorizontalAlignment** sur **Centre**.

    **Avant**
    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"
                  ItemContainerStyle="{StaticResource ImageGridView_DefaultItemContainerStyle}"/>
    ```

    **Après**
    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"
                  ItemContainerStyle="{StaticResource ImageGridView_DefaultItemContainerStyle}" 
                  HorizontalAlignment="Center"/>
    ```

2. Exécutez l’application et redimensionnez la fenêtre. Faites défiler vers le bas pour afficher plus d’images.

    Les images sont centrées, ce qui améliore l’aspect. Toutefois, la barre de défilement est alignée sur le bord du **GridView** au lieu du bord de la fenêtre. Pour résoudre ce problème, vous devez centrer les images dans le **GridView** plutôt que de centrer le **GridView** dans la page. Cela représente un peu plus de travail, mais le résultat sera meilleur à la fin.

3. Supprimez le paramètre **HorizontalAlignment** de l’étape précédente.

4. Dans Structure du document, cliquez avec le bouton droit sur **ImageGridView**. Dans le menu contextuel, sélectionnez **Modifier les modèles supplémentaires > Modifier la disposition des éléments (ItemsPanel) > Modifier une copie...** . La boîte de dialogue **Créer une ressource** s’ouvre.

5. Dans la boîte de dialogue, remplacez la valeur de Nom (clé) par **ImageGridView_ItemsPanelTemplate**, puis cliquez sur **OK**.

    Une copie du modèle **ItemsPanelTemplate** par défaut est ajoutée à la section **Page.Resources** de votre code XAML. (Et comme auparavant, le **GridView** est mis à jour pour faire référence à cette ressource.)

    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" />
    </ItemsPanelTemplate>
    ```

    De la même manière que vous avez utilisé plusieurs panneaux pour disposer les contrôles dans votre application, le **GridView** a un panneau interne qui gère la disposition de ses éléments. Maintenant que vous avez accès à ce panneau (**ItemsWrapGrid**), vous pouvez modifier ses propriétés pour changer la disposition des éléments à l’intérieur du **GridView**.

6. Dans la grille **ItemsWrapGrid**, définissez la propriété **HorizontalAlignment** sur **Centre**.

    **Avant**
    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" />
    </ItemsPanelTemplate>
    ```

    **Après**
    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal"
                       HorizontalAlignment="Center"/>
    </ItemsPanelTemplate>
    ```

7. Exécutez l’application et redimensionnez à nouveau la fenêtre. Faites défiler vers le bas pour afficher plus d’images.

![Point de contrôle de l’interface utilisateur de l’application 4](images/xaml-basics/layout-3.png)

Désormais, la barre de défilement est alignée sur le bord de la fenêtre. Bravo ! Vous avez créé l’interface utilisateur de base de votre application.

## <a name="going-further"></a>Aller plus loin

Maintenant que vous avez créé l’interface utilisateur de base, validez les autres tutoriels, basés aussi sur l’exemple PhotoLab : 

* Ajoutez des images réelles et des données dans le [tutoriel de liaison de données XAML](../../data-binding/xaml-basics-data-binding.md).
* Rendez l’interface utilisateur adaptable en fonction des tailles d’écran dans le [tutoriel de disposition adaptative en XAML](xaml-basics-adaptive-layout.md).


## <a name="get-the-final-version-of-the-photolab-sample"></a>Obtenir la version finale de l’exemple PhotoLab

Comme ce tutoriel ne génère pas l’application de retouche photo complète, veillez à examiner la [version finale](https://github.com/Microsoft/Windows-appsample-photo-lab) pour voir d’autres fonctionnalités, comme les animations personnalisées et le support téléphonique.

