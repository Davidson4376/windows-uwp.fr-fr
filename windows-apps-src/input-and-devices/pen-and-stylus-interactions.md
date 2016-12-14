---
author: Karl-Bridge-Microsoft
Description: "Créez des applications de plateforme Windows universelle (UWP) qui prennent en charge les interactions personnalisées à partir de stylos et de stylets, y compris l’encre numérique pour des expériences de dessin et d’écriture naturelles."
title: "Interactions avec le stylet et Windows Ink dans les applications UWP"
ms.assetid: 3DA4F2D2-5405-42A1-9ED9-3A87BCD84C43
label: Pen interactions and Windows Ink in UWP apps
template: detail.hbs
keywords: "Windows Ink, entrée manuscrite Windows, DirectInk, InkPresenter, InkCanvas"
translationtype: Human Translation
ms.sourcegitcommit: 0f7f54c5c5baccdedfe32bc7c71994e43a93f032
ms.openlocfilehash: 49098df1bf7fb72264a633aae37f941951cee2cf

---

# <a name="pen-interactions-and-windows-ink-in-uwp-apps"></a>Interactions avec le stylet et Windows Ink dans les applications UWP
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

<!--
![touchpad](images/input-patterns/input-pen.jpg)
-->

<!--
![Surface Pen](images/ink/hero2-small.png)  
*Surface Studio with Surface Pen* (available for purchase at the [Microsoft Store](https://aka.ms/purchasesurfacepen)).
-->

![Stylet Surface](images/ink/hero-small.png)  
*Stylet Surface* (disponible à l’achat dans la [Boutique Microsoft](https://aka.ms/purchasesurfacepen)).

## <a name="overview"></a>Vue d’ensemble

Optimisez votre application de plateforme Windows universelle (UWP) pour la saisie au stylet, afin de fournir une fonctionnalité standard d’[**appareil de pointage**](https://msdn.microsoft.com/library/windows/apps/br225633) et d’offrir une expérience Windows Ink optimale à vos utilisateurs.

> [!NOTE]
> Cette rubrique est dédiée à la plateforme Windows Ink. Pour en savoir plus sur la gestion des entrées au pointeur (similaires aux fonctionnalités tactiles, de la souris et du pavé tactile), consultez [Gérer les entrées du pointeur](handle-pointer-input.md).

| Vidéos |   |
| --- | --- |
| <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Ink-in-Your-UWP-App/player" width="300" height="200" allowFullScreen frameBorder="0"></iframe> | <iframe src="https://channel9.msdn.com/Events/Ignite/2016/BRK2060/player" width="300" height="200" allowFullScreen frameBorder="0"></iframe> |
| *Utilisation d’entrée manuscrite dans votre application UWP* | *Utiliser le stylet et l’entrée manuscrite Windows pour créer des applications d’entreprise plus conviviales* |

La plateforme Windows Ink, associée à un stylet, permet de créer des notes manuscrites, des dessins et des annotations plus naturellement. La plateforme prend en charge la capture d’entrée du numériseur sous forme de données d’entrée manuscrite, la génération et la gestion de données d’entrée manuscrite, la restitution de ces données sous forme de traits et la conversion de l’encre en texte via la reconnaissance d’écriture manuscrite.

En plus de capturer la position et les mouvements de base du stylet lorsque l’utilisateur écrit ou dessine, votre application peut également effectuer le suivi des niveaux de pression variables utilisés pour un trait et en effectuer le suivi. Ces informations, ainsi que les paramètres relatifs à la forme de la pointe, à sa taille et à sa rotation, à la couleur de l’encre et à l’utilisation (entrée manuscrite normale, effacement, surlignage et sélection), vous permettent de proposer des expériences utilisateur ressemblant étroitement à l’écriture ou au dessin sur papier à l’aide d’un stylo, d’un crayon ou d’un pinceau.

> [!NOTE]
> Votre application prend également en charge l’entrée manuscrite à partir d’autres appareils de pointage, comme les numériseurs tactiles et les souris. 

La plateforme d’entrée manuscrite est très flexible. Elle est conçue pour prendre en charge différents niveaux de fonctionnalité, en fonction de vos besoins.

Pour obtenir des recommandations en matière d’expérience utilisateur avec Windows Ink, consultez [Contrôles pour l’entrée manuscrite](../controls-and-patterns/inking-controls.md).

## <a name="components-of-the-windows-ink-platform"></a>Composants de la plateforme Windows Ink

| Composant | Description |
| --- | --- |
| [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) | Un contrôle de plateforme d’interface utilisateur XAML, qui reçoit et affiche par défaut toutes les entrées à partir d’un stylet comme un trait d’encre ou un trait d’effacement.<br/>Pour plus d’informations sur l’utilisation de l’élément InkCanvas, consultez [Reconnaître les traits d’encre Windows en tant que texte](convert-ink-to-text.md) et [Stocker et récupérer les données de traits Windows Ink](save-and-load-ink.md). |
| [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) | Un objet code-behind, instancié avec un contrôle [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) (exposé par le biais de la propriété [**InkCanvas.InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)). Cet objet fournit toutes les fonctionnalités d’entrée manuscrite par défaut exposées par l’élément **InkCanvas**, ainsi qu’un ensemble complet d’API pour plus de personnalisation.<br/>Pour plus d’informations sur l’utilisation de l’élément InkPresenter, consultez [Reconnaître les traits d’encre Windows en tant que texte](convert-ink-to-text.md) et [Stocker et récupérer les données de traits Windows Ink](save-and-load-ink.md). |
| [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) | Ajoutez un élément InkToolbar par défaut à une application d’entrée manuscrite de plateforme Windows universelle (UWP), ajoutez un bouton de stylet personnalisé à l’élément InkToolbar et liez le bouton de stylet personnalisé à une définition de stylet personnalisé. Un contrôle de plateforme d’interface utilisateur XAML contenant une collection extensible et personnalisable de boutons qui activent des fonctionnalités d’entrée manuscrite dans un élément InkCanvas associé.<br/>Pour plus d’informations sur l’utilisation de l’élément InkToolbar, consultez [Ajouter un élément InkToolbar à une application d’entrée manuscrite de plateforme Windows universelle (UWP)](ink-toolbar.md). |
| [**IInkD2DRenderer**](https://msdn.microsoft.com/library/mt147263) | Permet le rendu des traits d’encre sur le contexte d’appareil Direct2D désigné d’une application Windows universelle, au lieu du contrôle [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) par défaut. Cela offre une personnalisation totale de l’expérience d’entrée manuscrite.<br/>Pour plus d’informations, consultez [l’exemple d’entrée manuscrite complexe](http://go.microsoft.com/fwlink/p/?LinkID=620314). |

## <a name="basic-inking-with-inkcanvas"></a>Entrée manuscrite de base avec InkCanvas

Pour les fonctionnalités de base d’entrée manuscrite, il suffit de placer un élément [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) n’importe où sur une page.

Par défaut, l’élément [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) prend en charge l’entrée manuscrite uniquement à partir d’un stylet. L’entrée est restituée sous la forme d’un trait d’encre à l’aide des paramètres par défaut pour la couleur et l’épaisseur (un stylo à bille noir avec une épaisseur de 2 pixels), ou traitée sous forme d’un effaceur de trait (lorsque l’entrée manuscrite est effectuée à partir d’une pointe d’effaceur ou que la pointe du stylet est modifiée à l’aide d’un bouton d’effacement).

> [!NOTE]
> Si une pointe d’effaceur ou le bouton n’est pas présent, l’élément InkCanvas peut être configuré pour traiter les entrées à partir de la pointe du stylet comme un trait d’effacement.

Dans cet exemple, un élément [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) recouvre une image d’arrière-plan.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header"
                   Text="Basic ink sample"
                   Style="{ThemeResource HeaderTextBlockStyle}"
                   Margin="10,0,0,0" />            
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

Cette série d’images montre comment une entrée manuscrite de stylet est restituée par ce contrôle [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535).

| ![InkCanvas vide avec une image d’arrière-plan](images/ink_basic_1_small.png) | ![InkCanvas avec des traits d’encre](images/ink_basic_2_small.png) | ![InkCanvas avec un trait effacé](images/ink_basic_3_small.png) |
| --- | --- | ---|
| [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) vide avec une image d’arrière-plan. | [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) avec des traits d’encre. | [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) avec un trait effacé (notez comment fonctionne l’effacement sur un trait complet et non seulement sur une partie). |

La fonctionnalité d’entrée manuscrite prise en charge par le contrôle [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) est proposée par un objet code-behind appelé [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011).

Pour l’entrée manuscrite de base, vous n’avez pas à vous préoccuper de l’élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011). Toutefois, pour personnaliser et configurer le comportement de l’entrée manuscrite sur l’élément [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535), vous devez accéder à son objet **InkPresenter** correspondant.

## <a name="basic-customization-with-inkpresenter"></a>Personnalisation de base avec InkPresenter

Un objet [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) est instancié avec chaque contrôle [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535).

En plus de fournir tous les comportements d’entrée manuscrite par défaut de son contrôle [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) correspondant, l’élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) propose un ensemble complet d’API pour personnaliser davantage les traits. Cela inclut les propriétés de trait, les types d’appareils d’entrée pris en charge, et si l’entrée est traitée par l’objet ou transmise à l’application.

> [!NOTE]
> L’élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) ne peut pas être instancié directement. Au lieu de cela, il est accessible via la propriété [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) de l’élément [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535). 

Ici, l’élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) est configuré pour interpréter les données d’entrée de stylet et de souris sous forme de traits d’encre. Nous définissons également des attributs de trait d’encre initiaux utilisés pour restituer les traits dans l’élément [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535).

```csharp
public MainPage()
{
    this.InitializeComponent();

    // Set supported inking device types.
    inkCanvas.InkPresenter.InputDeviceTypes =
        Windows.UI.Core.CoreInputDeviceTypes.Mouse |
        Windows.UI.Core.CoreInputDeviceTypes.Pen;

    // Set initial ink stroke attributes.
    InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
    drawingAttributes.Color = Windows.UI.Colors.Black;
    drawingAttributes.IgnorePressure = false;
    drawingAttributes.FitToCurve = true;
    inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);
}
```

Les attributs de traits d’encre peuvent être définis de manière dynamique pour s’adapter aux préférences de l’utilisateur ou aux exigences de l’application.

Ici, nous laissons un utilisateur choisir parmi une liste de couleurs d’encre.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header"
                   Text="Basic ink customization sample"
                   VerticalAlignment="Center"
                   Style="{ThemeResource HeaderTextBlockStyle}"
                   Margin="10,0,0,0" />
        <TextBlock Text="Color:"
                   Style="{StaticResource SubheaderTextBlockStyle}"
                   VerticalAlignment="Center"
                   Margin="50,0,10,0"/>
        <ComboBox x:Name="PenColor"
                  VerticalAlignment="Center"
                  SelectedIndex="0"
                  SelectionChanged="OnPenColorChanged">
            <ComboBoxItem Content="Black"/>
            <ComboBoxItem Content="Red"/>
        </ComboBox>
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

Nous traitons ensuite les modifications de manière à tenir compte de la couleur sélectionnée et actualisons les attributs de traits d’encre en conséquence.

```csharp
// Update ink stroke color for new strokes.
private void OnPenColorChanged(object sender, SelectionChangedEventArgs e)
{
    if (inkCanvas != null)
    {
        InkDrawingAttributes drawingAttributes =
            inkCanvas.InkPresenter.CopyDefaultDrawingAttributes();

        string value = ((ComboBoxItem)PenColor.SelectedItem).Content.ToString();

        switch (value)
        {
            case "Black":
                drawingAttributes.Color = Windows.UI.Colors.Black;
                break;
            case "Red":
                drawingAttributes.Color = Windows.UI.Colors.Red;
                break;
            default:
                drawingAttributes.Color = Windows.UI.Colors.Black;
                break;
        };

        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);
    }
}
```

Ces images montrent comment l’élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) traite et personnalise l’entrée de stylet.

| ![InkCanvas avec des traits d’encre noire par défaut](images/ink-basic-custom-1-small.png) | ![InkCanvas avec des traits d’encre rouge sélectionnés par l’utilisateur.](images/ink-basic-custom-2-small.png) |
| --- | --- |
| [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) avec des traits d’encre noire par défaut. | [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) avec des traits d’encre rouge sélectionnés par l’utilisateur. | 

Pour exploiter des fonctionnalités dépassant l’entrée manuscrite et l’effacement, telles que la sélection de trait, votre application doit identifier une entrée spécifique afin que l’élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) la transmette directement non traitée afin d’être gérée par votre application.

## <a name="pass-through-input-for-advanced-processing"></a>Entrée directe pour traitement avancé

Par défaut, [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) traite toutes les entrées sous forme de trait d’encre ou d’effacement. Cela inclut les entrées modifiées par une affordance de matériel secondaire, telle qu’un bouton de stylet, un bouton droit de souris ou un élément similaire.

Lorsque vous utilisez ces moyens secondaires, les utilisateurs attendent généralement des fonctionnalités supplémentaires ou une modification de comportement.

Dans certains cas, vous devrez peut-être exposer les fonctionnalités d’entrée manuscrite de base pour les crayons sans affordance secondaire (fonctionnalité qui n’est généralement pas associée à la pointe du stylet), les autres types d’appareils, les fonctionnalités supplémentaires ou le comportement modifié en fonction de la sélection d’un utilisateur dans l’interface utilisateur de votre application.

Pour prendre cela en charge, [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) peut être configuré pour ne pas traiter l’entrée spécifique. Cette entrée non traitée est ensuite directement transmise à votre application pour traitement.

L’exemple de code suivant décrit comment activer la sélection de trait lorsqu’une entrée est modifiée à l’aide d’un bouton de stylet (ou le bouton droit de la souris).

Pour cet exemple, nous utilisons les fichiers MainPage.xaml et MainPage.xaml.cs pour héberger l’ensemble du code.

1.  Tout d’abord, nous configurons l’interface utilisateur dans MainPage.xaml.

    Ici, nous ajoutons un canevas (sous l’élément [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)) pour dessiner le trait de sélection. Une couche distincte permet de dessiner le trait de sélection sans modifier l’élément **InkCanvas** ni son contenu.

    ![InkCanvas vide avec un canevas de sélection sous-jacent](images/ink-unprocessed-1-small.png)

      ```xaml
        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
          <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
          </Grid.RowDefinitions>
          <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="Header"
              Text="Advanced ink customization sample"
              VerticalAlignment="Center"
              Style="{ThemeResource HeaderTextBlockStyle}"
              Margin="10,0,0,0" />
          </StackPanel>
          <Grid Grid.Row="1">
            <!-- Canvas for displaying selection UI. -->
            <Canvas x:Name="selectionCanvas"/>
            <!-- Inking area -->
            <InkCanvas x:Name="inkCanvas"/>
          </Grid>
        </Grid>
      ```

2.  Dans MainPage.xaml.cs, nous déclarons quelques variables globales pour conserver des références à des aspects de l’interface utilisateur de sélection. Plus précisément, le trait du lasso de sélection et le rectangle englobant qui met en surbrillance les traits sélectionnés.

      ```csharp
        // Stroke selection tool.
        private Polyline lasso;
        // Stroke selection area.
        private Rect boundingRect;
      ```

3.  Ensuite, nous configurons l’élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) pour interpréter les données d’entrée de stylet et de souris sous forme de traits d’encre et définir certains attributs de trait d’encre initiaux utilisés pour restituer les traits dans l’élément [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535).

    Plus important encore, nous utilisons la propriété [**InputProcessingConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn948764) de l’élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) pour indiquer que toute entrée modifiée doit être traitée par l’application. La modification d’une entrée est spécifiée en attribuant à **InputProcessingConfiguration.RightDragAction** la valeur [**InkInputRightDragAction.LeaveUnprocessed**](https://msdn.microsoft.com/library/windows/apps/dn948760).

    Nous affectons ensuite des écouteurs pour les événements [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/dn914712), [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/dn914711), et [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/dn914713) non traités transmis directement par [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081). Toutes les fonctionnalités de sélection sont implémentées dans les gestionnaires de ces événements.

    Enfin, nous affectons des écouteurs pour les événements [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702) et [**StrokesErased**](https://msdn.microsoft.com/library/windows/apps/dn948767) de [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081). Nous utilisons les gestionnaires de ces événements pour nettoyer l’interface utilisateur de sélection si un nouveau trait est commencé ou un trait existant effacé.

    ![InkCanvas avec des traits d’encre noire par défaut](images/ink-unprocessed-2-small.png)

      ```csharp
        public MainPage()
        {
          this.InitializeComponent();

          // Set supported inking device types.
          inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

          // Set initial ink stroke attributes.
          InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
          drawingAttributes.Color = Windows.UI.Colors.Black;
          drawingAttributes.IgnorePressure = false;
          drawingAttributes.FitToCurve = true;
          inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

          // By default, the InkPresenter processes input modified by
          // a secondary affordance (pen barrel button, right mouse
          // button, or similar) as ink.
          // To pass through modified input to the app for custom processing
          // on the app UI thread instead of the background ink thread, set
          // InputProcessingConfiguration.RightDragAction to LeaveUnprocessed.
          inkCanvas.InkPresenter.InputProcessingConfiguration.RightDragAction =
              InkInputRightDragAction.LeaveUnprocessed;

          // Listen for unprocessed pointer events from modified input.
          // The input is used to provide selection functionality.
          inkCanvas.InkPresenter.UnprocessedInput.PointerPressed +=
              UnprocessedInput_PointerPressed;
          inkCanvas.InkPresenter.UnprocessedInput.PointerMoved +=
              UnprocessedInput_PointerMoved;
          inkCanvas.InkPresenter.UnprocessedInput.PointerReleased +=
              UnprocessedInput_PointerReleased;

          // Listen for new ink or erase strokes to clean up selection UI.
          inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
              StrokeInput_StrokeStarted;
          inkCanvas.InkPresenter.StrokesErased +=
              InkPresenter_StrokesErased;
        }
      ```

4.  Nous définissons ensuite des gestionnaires pour les événements [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/dn914712), [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/dn914711), et [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/dn914713) non traités transmis directement par [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081).

    Toutes les fonctionnalités de sélection sont implémentées dans ces gestionnaires, y compris le trait de lasso et le rectangle englobant.

    ![Lasso de sélection](images/ink-unprocessed-3-small.png)

      ```csharp
        // Handle unprocessed pointer events from modified input.
        // The input is used to provide selection functionality.
        // Selection UI is drawn on a canvas under the InkCanvas.
        private void UnprocessedInput_PointerPressed(
          InkUnprocessedInput sender, PointerEventArgs args)
        {
          // Initialize a selection lasso.
          lasso = new Polyline()
          {
            Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
              StrokeThickness = 1,
              StrokeDashArray = new DoubleCollection() { 5, 2 },
              };

              lasso.Points.Add(args.CurrentPoint.RawPosition);

              selectionCanvas.Children.Add(lasso);
          }

          private void UnprocessedInput_PointerMoved(
            InkUnprocessedInput sender, PointerEventArgs args)
          {
            // Add a point to the lasso Polyline object.
            lasso.Points.Add(args.CurrentPoint.RawPosition);
          }

          private void UnprocessedInput_PointerReleased(
            InkUnprocessedInput sender, PointerEventArgs args)
          {
            // Add the final point to the Polyline object and
            // select strokes within the lasso area.
            // Draw a bounding box on the selection canvas
            // around the selected ink strokes.
            lasso.Points.Add(args.CurrentPoint.RawPosition);

            boundingRect =
              inkCanvas.InkPresenter.StrokeContainer.SelectWithPolyLine(
                lasso.Points);

            DrawBoundingRect();
          }
      ```

5.  Pour conclure le gestionnaire d’événements PointerReleased, nous effaçons tout le contenu de la couche de sélection (le trait du lasso), puis dessinons un rectangle englobant unique autour des traits d’encre englobés par la zone du lasso.

    ![Rectangle englobant de sélection](images/ink-unprocessed-4-small.png)

      ```csharp
        // Draw a bounding rectangle, on the selection canvas, encompassing
        // all ink strokes within the lasso area.
        private void DrawBoundingRect()
        {
          // Clear all existing content from the selection canvas.
          selectionCanvas.Children.Clear();

          // Draw a bounding rectangle only if there are ink strokes
          // within the lasso area.
          if (!((boundingRect.Width == 0) ||
            (boundingRect.Height == 0) ||
            boundingRect.IsEmpty))
            {
              var rectangle = new Rectangle()
              {
                Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
                  StrokeThickness = 1,
                  StrokeDashArray = new DoubleCollection() { 5, 2 },
                  Width = boundingRect.Width,
                  Height = boundingRect.Height
              };

              Canvas.SetLeft(rectangle, boundingRect.X);
              Canvas.SetTop(rectangle, boundingRect.Y);

              selectionCanvas.Children.Add(rectangle);
            }
          }
      ```

6.  Enfin, nous définissons des gestionnaires pour les événements InkPresenter [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702) et [**StrokesErased**](https://msdn.microsoft.com/library/windows/apps/dn948767).

    Ces deux événements appellent simplement la même fonction de nettoyage pour effacer la sélection actuelle chaque fois qu’un nouveau trait est détecté.

      ```csharp
        // Handle new ink or erase strokes to clean up selection UI.
        private void StrokeInput_StrokeStarted(
          InkStrokeInput sender, Windows.UI.Core.PointerEventArgs args)
        {
          ClearSelection();
        }

        private void InkPresenter_StrokesErased(
          InkPresenter sender, InkStrokesErasedEventArgs args)
        {
          ClearSelection();
        }
      ```

7.  Voici la fonction permettant de supprimer l’ensemble de l’interface utilisateur de sélection du canevas de sélection au commencement d’un nouveau trait ou à l’effacement d’un trait existant.

      ```csharp
        // Clean up selection UI.
        private void ClearSelection()
        {
          var strokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
          foreach (var stroke in strokes)
          {
            stroke.Selected = false;
          }
          ClearDrawnBoundingRect();
        }

        private void ClearDrawnBoundingRect()
        {
          if (selectionCanvas.Children.Any())
          {
            selectionCanvas.Children.Clear();
            boundingRect = Rect.Empty;
          }
        }
      ```

## <a name="custom-ink-rendering"></a>Restitution d’une entrée manuscrite personnalisée

Par défaut, l’entrée manuscrite est traitée sur un thread d’arrière-plan de faible latence et restituée « humide » comme elle est dessinée. Lorsque le trait est terminé (stylet ou doigt relevé, ou bouton de la souris relâché), le trait est traité sur le thread de l’interface utilisateur et restitué « sec » à la couche [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) (au-dessus du contenu de l’application et en remplaçant l’encre humide).

La plateforme d’entrée manuscrite vous permet de remplacer ce comportement et de personnaliser entièrement l’expérience d’entrée manuscrite par un séchage personnalisé de cette dernière.

Le séchage personnalisé requiert un objet [**IInkD2DRenderer**](https://msdn.microsoft.com/library/mt147263) pour gérer l’entrée manuscrite et la restituer au contexte de périphérique Direct2D de votre application Windows universelle, au lieu du contrôle [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) par défaut.

En appelant [**ActivateCustomDrying**](https://msdn.microsoft.com/library/windows/apps/dn922012) (avant le chargement de [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)), une application crée un objet [**InkSynchronizer**](https://msdn.microsoft.com/library/windows/apps/dn903979) pour personnaliser la manière dont un trait d’encre est restitué sec à un élément [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) ou [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050). Par exemple, un trait d’encre pourrait être rastérisé et intégré au contenu d’application plutôt que d’être intégré sous forme de couche **InkCanvas** distincte.

Pour obtenir un exemple complet de cette fonctionnalité, consultez [l’exemple d’entrée manuscrite complexe](http://go.microsoft.com/fwlink/p/?LinkID=620314).

> [!NOTE]
> Séchage personnalisé et élément [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)  
> Si votre application remplace le comportement par défaut du rendu d’entrée manuscrite de l’élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) par une implémentation de séchage personnalisé, les traits d’encre restitués ne sont plus disponibles pour l’élément InkToolbar et les commandes d’effacement intégrées de l’élément InkToolbar ne fonctionneront pas comme prévu. Pour fournir des fonctionnalités d’effacement, vous devez gérer tous les événements de pointeur, effectuer le test de positionnement sur chaque trait et remplacer la commande intégrée « Effacer toutes les entrées manuscrites ».

## <a name="other-articles-in-this-section"></a>Autres articles de cette section

| Rubrique | Description |
| --- | --- |
| [Reconnaître les traits d’encre](convert-ink-to-text.md) | Convertissez des traits d’encre en texte à l’aide de la reconnaissance de l’écriture manuscrite ou en formes à l’aide de la reconnaissance personnalisée. |
| [Stocker et récupérer des traits d’encre](save-and-load-ink.md) | Stockez des données de traits d’encre dans un fichier GIF (Graphics Interchange Format) à l’aide des métadonnées intégrées ISF (Ink Serialized Format). |
| [Ajouter un élément InkToolbar à une application d’entrée manuscrite de plateforme Windows universelle (UWP)](ink-toolbar.md) | Ajoutez un élément InkToolbar par défaut à une application d’entrée manuscrite de plateforme Windows universelle (UWP), ajoutez un bouton de stylet personnalisé à l’élément InkToolbar et liez le bouton de stylet personnalisé à une définition de stylet personnalisé. |

## <a name="related-articles"></a>Articles connexes

* [Gérer les entrées du pointeur](handle-pointer-input.md)
* [Identifier des périphériques d’entrée](identify-input-devices.md)

**API**

* [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)
* [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)
* [**Windows.UI.Input.Inking.Core**](https://msdn.microsoft.com/library/windows/apps/dn958452)

**Exemples**
* [Exemple d’entrée manuscrite](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Exemple d’entrée manuscrite simple](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Exemple d’entrée manuscrite complexe](http://go.microsoft.com/fwlink/p/?LinkID=620314)
* [Exemple de livre de coloriage](https://aka.ms/cpubsample-coloringbook)
* [Exemple de notes de famille](https://aka.ms/cpubsample-familynotessample)
* [Exemple d’entrée de base](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Exemple d’entrée à faible latence](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Exemple de mode d’interaction utilisateur](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Exemple de visuels de focus](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**Exemples d’archive**
* [Entrée : exemple de fonctionnalités de périphériques](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrée : exemple d’événements d’entrée utilisateur XAML](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Exemple de zoom, de panoramique et de défilement XAML](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrée : mouvements et manipulations avec GestureRecognizer](http://go.microsoft.com/fwlink/p/?LinkID=231605)
 

 



<!--HONumber=Dec16_HO1-->


