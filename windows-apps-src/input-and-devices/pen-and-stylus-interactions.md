---
Description: Créer des applications de plateforme Windows universelle (UWP) qui prennent en charge les interactions personnalisées à partir de stylos et de stylets, y compris l’encre numérique pour des expériences de dessin et d’écriture naturelles.
title: Interactions avec le stylo et le stylet
ms.assetid: 3DA4F2D2-5405-42A1-9ED9-3A87BCD84C43
label: Stylo et stylet
template: detail.hbs
---

# Interactions avec le stylo et le stylet


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Optimisez la conception de votre application de plateforme Windows universelle (UWP) pour l’entrée tactile, et définissez la prise en charge du stylo de base par défaut.

La plateforme d’entrée manuscrite UWP, associée à un stylet, fournit un moyen naturel de créer des notes manuscrites, des dessins et des annotations. Cette plateforme prend en charge la capture de données d’entrée manuscrite à partir de l’entrée du numériseur, la génération de données d’entrée manuscrite, le rendu de ces données sous forme de traits d’encre sur le périphérique de sortie, la gestion des données d’entrée manuscrite et la reconnaissance de l’écriture manuscrite.

![pavé tactile](images/input-patterns/input-pen.jpg)


**API importantes**

-   [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)
-   [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)
-   [**Windows.UI.Input.Inking.Core**](https://msdn.microsoft.com/library/windows/apps/dn958452)


En plus de servir de [**périphérique de pointage**](https://msdn.microsoft.com/library/windows/apps/br225633) (semblable à la souris et à la fonctionnalité tactile), un stylo/stylet est généralement associé au fait d’écrire et de dessiner directement sur un écran à l’aide d’encre numérique.

La plateforme d’entrée manuscrite de la plateforme universelle Windows, associée à un stylet, propose un moyen naturel de créer des notes manuscrites, des dessins et des annotations numériques. La plateforme prend en charge la capture d’entrée du numériseur sous forme de données d’entrée manuscrite, la génération et la gestion de données d’entrée manuscrite, la restitution de ces données sous forme de traits et la conversion de l’encre en texte via la reconnaissance d’écriture manuscrite.

En plus de capturer la position et les mouvements de base du stylet lorsque l’utilisateur écrit ou dessine, votre application peut également effectuer le suivi des niveaux de pression variables utilisés pour un trait et en effectuer le suivi. Ces informations, ainsi que les paramètres relatifs à la forme de la pointe, à sa taille et à sa rotation, à la couleur de l’encre et à l’utilisation (entrée manuscrite normale, effacement, surlignage et sélection), vous permettent de proposer des expériences utilisateur ressemblant étroitement à l’écriture ou au dessin sur papier à l’aide d’un stylo, d’un crayon ou d’un pinceau.

**Remarque** Votre application prend également en charge l’entrée manuscrite à partir d’autres appareils de pointage, comme les numériseurs tactiles et les souris.

 

La plateforme d’entrée manuscrite est très flexible. Elle est conçue pour prendre en charge différents niveaux de fonctionnalité, en fonction de vos besoins.

La plateforme d’entrée manuscrite comprend trois composants :

-   [
            **InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) : un contrôle de plateforme d’interface utilisateur XAML qui, par défaut, reçoit et affiche toutes les entrées à partir d’un stylet comme un trait d’encre ou un trait d’effacement.

-   [
            **InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) : un objet code-behind, instancié avec un contrôle [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) (exposé par le biais de la propriété [**InkCanvas.InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)). Cet objet fournit toutes les fonctionnalités d’entrée manuscrite par défaut exposées par l’élément **InkCanvas**, ainsi qu’un ensemble complet d’API pour plus de personnalisation.

-   [
            **IInkD2DRenderer**](https://msdn.microsoft.com/library/mt147263) : permet le rendu des traits d’encre sur le contexte de périphérique Direct2D désigné d’une application Windows universelle, au lieu du contrôle [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) par défaut. Cela permet une totale personnalisation de l’expérience relative à l’entrée manuscrite.

## <span id="inkcanvas"> </span> <span id="INKCANVAS"> </span>Entrée manuscrite de base avec InkCanvas


Pour les fonctionnalités de base liées à l’écriture manuscrite, il suffit de placer un élément [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) n’importe où sur une page.

[
            **InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) prend en charge l’entrée manuscrite uniquement à partir d’un stylet. L’entrée est restituée sous la forme d’un trait d’encre à l’aide des paramètres par défaut pour la couleur et l’épaisseur, ou traitée sous forme d’un effaceur de trait (lorsque l’entrée manuscrite est effectuée à partir d’une pointe d’effaceur ou que la pointe du crayon est modifiée à l’aide d’un bouton d’effacement).

Dans cet exemple, un élément [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) recouvre une image d’arrière-plan.

```XAML
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

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><img src="images/ink_basic_1_small.png" alt="The blank InkCanvas with a background image" /></td>
<td align="left"><img src="images/ink_basic_2_small.png" alt="The InkCanvas with ink strokes" /></td>
<td align="left"><img src="images/ink_basic_3_small.png" alt="The InkCanvas with one stroke erased" /></td>
</tr>
<tr class="even">
<td align="left">[<strong>InkCanvas</strong>] vide (https://msdn.microsoft.com/library/windows/apps/dn858535) avec une image d’arrière-plan.</td>
<td align="left">[<strong>InkCanvas</strong>] (https://msdn.microsoft.com/library/windows/apps/dn858535) avec des traits d’encre.</td>
<td align="left">[<strong>InkCanvas</strong>](https://msdn.microsoft.com/library/windows/apps/dn858535) avec un trait effacé.
<p>Notez comment fonctionne l’effacement sur un trait complet et non seulement sur une partie.</p></td>
</tr>
</tbody>
</table>

 

La fonctionnalité d’entrée manuscrite prise en charge par le contrôle [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) est proposée par un objet code-behind appelé [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011).

Pour l’entrée manuscrite de base, vous n’avez pas à vous préoccuper de l’élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011). Toutefois, pour personnaliser et configurer le comportement de l’entrée manuscrite sur l’élément [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535), vous devez accéder à son objet **InkPresenter** correspondant.

## <span id="inkpresenter"> </span> <span id="INKPRESENTER"> </span>Personnalisation de base avec InkPresenter


Un objet [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) est instancié avec chaque contrôle [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535).

En plus de fournir tous les comportements d’entrée manuscrite par défaut de son contrôle [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) correspondant, l’élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) propose un ensemble complet d’API pour personnaliser davantage les traits. Cela inclut les propriétés de trait, les types d’appareils d’entrée pris en charge, et si l’entrée est traitée par l’objet ou transmise à l’application.

**Remarque**  
L’élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) ne peut pas être instancié directement. Au lieu de cela, il est accessible via la propriété [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) de l’élément [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535).

 

Ici, l’élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) est configuré pour interpréter les données d’entrée de stylet et de souris sous forme de traits d’encre. Nous définissons également des attributs de trait d’encre initiaux utilisés pour restituer les traits dans l’élément [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535).

```CSharp
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

```XAML
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

```CSharp
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

|                                                                                      |                                                                                          |
|--------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| ![InkCanvas avec des traits d’encre noire par défaut](images/ink-basic-custom-1-small.png) | ![InkCanvas avec des traits d’encre rouge sélectionnés par l’utilisateur.](images/ink-basic-custom-2-small.png) |
| [
            **InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) avec des traits d’encre noire par défaut.        | [
            **InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) avec des traits d’encre rouge sélectionnés par l’utilisateur.        |

 

Pour exploiter des fonctionnalités dépassant l’entrée manuscrite et l’effacement, telles que la sélection de trait, votre application doit identifier une entrée spécifique afin que l’élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) la transmette directement non traitée pour être gérée par votre application.

## <span id="passthrough"> </span> <span id="PASSTHROUGH"> </span>Entrée directe pour traitement avancé


Par défaut, [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) traite toutes les entrées sous forme de trait d’encre ou d’effacement. Cela inclut les entrées modifiées par une affordance de matériel secondaire, telle qu’un bouton de stylet, un bouton droit de souris ou un élément similaire.

Lorsque vous utilisez ces moyens secondaires, les utilisateurs attendent généralement des fonctionnalités supplémentaires ou une modification de comportement.

Dans certains cas, vous devrez peut-être exposer les fonctionnalités d’entrée manuscrite de base pour les crayons sans affordance secondaire (fonctionnalité qui n’est généralement pas associée à la pointe du stylet), les autres types d’appareils, les fonctionnalités supplémentaires ou le comportement modifié en fonction de la sélection d’un utilisateur dans l’interface utilisateur de votre application.

Pour prendre cela en charge, [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) peut être configuré pour ne pas traiter l’entrée spécifique. Cette entrée non traitée est ensuite directement transmise à votre application pour traitement.

L’exemple de code suivant décrit comment activer la sélection de trait lorsqu’une entrée est modifiée à l’aide d’un bouton de stylet (ou le bouton droit de la souris).

Pour cet exemple, nous utilisons les fichiers MainPage.xaml et MainPage.xaml.cs pour héberger l’ensemble du code.

1.  Tout d’abord, nous configurons l’interface utilisateur dans MainPage.xaml.

    Ici, nous ajoutons un canevas (sous l’élément [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)) pour dessiner le trait de sélection. Une couche distincte permet de dessiner le trait de sélection sans modifier l’élément **InkCanvas** ni son contenu.

    ![InkCanvas vide avec un canevas de sélection sous-jacent](images/ink-unprocessed-1-small.png)

```    XAML
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

```    CSharp
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

```    CSharp
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

```    CSharp
// Handle unprocessed pointer events from modifed input.
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

```    CSharp
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

```    CSharp
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

```    CSharp
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

## <span id="iinkd2drenderer"> </span> <span id="IINKD2DRENDERER"> </span>Restitution d’une entrée manuscrite personnalisée


Par défaut, l’entrée manuscrite est traitée sur un thread d’arrière-plan de faible latence et restituée « humide » comme elle est dessinée. Lorsque le trait est terminé (stylet ou doigt relevé, ou bouton de la souris relâché), le trait est traité sur le thread de l’interface utilisateur et restitué « sec » à la couche [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) (au-dessus du contenu de l’application et en remplaçant l’encre humide).

La plateforme d’entrée manuscrite vous permet de remplacer ce comportement et de personnaliser entièrement l’expérience d’entrée manuscrite par un séchage personnalisé de cette dernière.

Le séchage personnalisé requiert un objet [**IInkD2DRenderer**](https://msdn.microsoft.com/library/mt147263) pour gérer l’entrée manuscrite et la restituer au contexte de périphérique Direct2D de votre application Windows universelle, au lieu du contrôle [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) par défaut.

En appelant [**ActivateCustomDrying**](https://msdn.microsoft.com/library/windows/apps/dn922012) (avant le chargement de [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)), une application crée un objet [**InkSynchronizer**](https://msdn.microsoft.com/library/windows/apps/dn903979) pour personnaliser la manière dont un trait d’encre est restitué sec à un élément [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) ou [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050). Par exemple, un trait d’encre pourrait être rastérisé et intégré au contexte d’application plutôt que sous forme de couche **InkCanvas** distincte.

Pour voir un exemple complet de cette fonctionnalité, reportez-vous à l’[exemple d’entrée manuscrite complexe](http://go.microsoft.com/fwlink/p/?LinkID=620314).


## Autres articles de cette section 
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Rubrique</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Recognize ink strokes](convert-ink-to-text.md)</p></td>
<td align="left"><p>Convertissez des traits d’encre en texte à l’aide de la reconnaissance de l’écriture manuscrite ou en formes à l’aide de la reconnaissance personnalisée.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Store and retrieve ink strokes](save-and-load-ink.md)</p></td>
<td align="left"><p>Stockez des données de traits d’encre dans un fichier GIF (Graphics Interchange Format) à l’aide des métadonnées intégrées ISF (Ink Serialized Format).</p></td>
</tr>
</tbody>
</table>

 


## <span id="related_topics"> </span>Articles connexes


* [Gérer les entrées du pointeur](handle-pointer-input.md)
* [Identifier des périphériques d’entrée](identify-input-devices.md)
**Exemples**
* [Exemple d’entrée manuscrite](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Exemple d’entrée manuscrite simple](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Exemple d’entrée manuscrite complexe](http://go.microsoft.com/fwlink/p/?LinkID=620314)
* [Exemple d’entrée de base](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Exemple d’entrée à faible latence](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Exemple de mode d’interaction utilisateur](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Exemple de visuels de focus](http://go.microsoft.com/fwlink/p/?LinkID=619895)
**Exemples d’archive**
* [Entrée : exemple de fonctionnalités de périphériques](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrée : exemple d’événements d’entrée utilisateur XAML](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Exemple de zoom, de panoramique et de défilement XAML](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrée : mouvements et manipulations avec GestureRecognizer](http://go.microsoft.com/fwlink/p/?LinkID=231605)
 

 






<!--HONumber=Mar16_HO1-->


