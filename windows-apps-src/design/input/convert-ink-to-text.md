---
author: Karl-Bridge-Microsoft
Description: Use handwriting recognition and ink analysis to recognize Windows Ink strokes as text and shapes.
title: Reconnaître les traits WindowsInk en tant que texte et formes
ms.assetid: C2F3F3CE-737F-4652-98B7-5278A462F9D3
label: Recognize Windows Ink strokes as text
template: detail.hbs
keywords: Windows Ink, entrée manuscrite Windows, DirectInk, InkPresenter, InkCanvas, reconnaissance de l’écriture manuscrite, interaction utilisateur, entrées
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 83142b0a3b24e25f8e7a922d800262f505cd8cc2
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5818303"
---
# <a name="recognize-windows-ink-strokes-as-text-and-shapes"></a>Reconnaître les traits WindowsInk en tant que texte et formes

Convertissez les traits d’encre en texte et formes grâce aux fonctionnalités de reconnaissance intégrées à WindowsInk.

> **API importantes**: [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535), [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)


## <a name="free-form-recognition-with-ink-analysis"></a>Reconnaissance des formes libres grâce à l’analyse des entrées manuscrites

Nous présentons ici la manière d’utiliser le moteur d’analyse WindowsInk ([Windows.UI.Input.Inking.Analysis](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis)) pour classifier, analyser et reconnaître un ensemble de traits de forme libre sur un [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535), aussi bien sous forme de texte que de formes. En plus de la reconnaissance de texte et de forme, l’analyse des entrées manuscrites peut également servir à reconnaître la structure d’un document, des listes à puces et des dessins génériques.

> [!NOTE]
> Pour des scénarios de texte basiques, à lignes simples et bruts, comme les entrées de formulaire, consultez [La reconnaissance de l’écriture manuscrite de base](#constrained-handwriting-recognition) plus loin dans cette rubrique.

Dans cet exemple, l’utilisateur lance la reconnaissance en cliquant sur un bouton pour indiquer qu’il a terminé de dessiner.

**Télécharger cet exemple depuis [Exemple d’analyse des entrées manuscrites (de base)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)**

1.  Tout d’abord, nous configurons l’interface utilisateur (MainPage.xaml). 

    L’interface utilisateur inclut un bouton «Reconnaître», une classe [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)et un [**Canvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas) de base. Lorsque le bouton «Reconnaître» est activé, tous les traits d’encre sur le canevas d’encre sont analysés et (si reconnus) les formes et les textes correspondants sont tracés sur le canevas de base. Les traits d’encre d’origine sont supprimés du canevas d’encre.
```xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" 
                    Orientation="Horizontal" 
                    Grid.Row="0">
            <TextBlock x:Name="Header" 
                        Text="Basic ink analysis sample" 
                        Style="{ThemeResource HeaderTextBlockStyle}" 
                        Margin="10,0,0,0" />
            <Button x:Name="recognize" 
                    Content="Recognize" 
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid x:Name="drawingCanvas" Grid.Row="1">

            <!-- The canvas where we render the replacement text and shapes. -->
            <Canvas x:Name="recognitionCanvas" />
            <!-- The canvas for ink input. -->
            <InkCanvas x:Name="inkCanvas" />

        </Grid>
    </Grid>
```
2. Dans le fichier code-behind de l’interface utilisateur (MainPage.xaml.cs), nous ajoutons les références de type espace de noms requises pour notre entrée manuscrite et notre fonctionnalité d’analyse des entrées manuscrites:
    - [Windows.UI.Input.Inking](https://docs.microsoft.com/uwp/api/windows.ui.input.inking)
    - [Windows.UI.Input.Inking.Analysis](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis)
    - [Windows.UI.Xaml.Shapes](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes)

3. Puis nous spécifions nos variables globales:
```csharp
    InkAnalyzer inkAnalyzer = new InkAnalyzer();
    IReadOnlyList<InkStroke> inkStrokes = null;
    InkAnalysisResult inkAnalysisResults = null;
```
4.  Nous définissons ensuite certains comportements de base d’entrées manuscrites:
    - L’élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) est configuré pour interpréter les données d’entrées de stylet, de souris et d’interactions tactiles, sous forme de traits d’encre ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)). 
    - Les traits d’encre sont restitués sur [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) à l’aide de l’élément [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050) spécifié. 
    - Un écouteur de l’événement Click est également déclaré sur le bouton «Reconnaître».
```csharp
/// <summary>
/// Initialize the UI page.
/// </summary>
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen | 
            Windows.UI.Core.CoreInputDeviceTypes.Touch;

        // Set initial ink stroke attributes.
        InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
        drawingAttributes.Color = Windows.UI.Colors.Black;
        drawingAttributes.IgnorePressure = false;
        drawingAttributes.FitToCurve = true;
        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

        // Listen for button click to initiate recognition.
        recognize.Click += RecognizeStrokes_Click;
    }
```
5.  Dans cet exemple, nous utilisons l’analyse des entrées manuscrites dans le gestionnaire d’événements Click du bouton «Reconnaître».
    - Tout d’abord, appelez [**GetStrokes**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.inkstrokecontainer.GetStrokes) sur le [**StrokeContainer**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.inkpresenter.StrokeContainer) de la [**InkCanvas.InkPresenter**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.inkcanvas.InkPresenter) pour obtenir la collection de tous les traits d’encre en cours.
    - Si des traits d’encre sont présents, transmettez-les dans un appel à [**AddDataForStrokes**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer#Windows_UI_Input_Inking_Analysis_InkAnalyzer_AddDataForStrokes_Windows_Foundation_Collections_IIterable_Windows_UI_Input_Inking_InkStroke__) de l’InkAnalyzer.
    - Nous essayons de reconnaître des dessins et du texte, mais vous pouvez utiliser la méthode [**SetStrokeDataKind**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.setstrokedatakind) pour indiquer si vous êtes intéressé uniquement par le texte (y compris la structure du document et les listes à puces) ou par les dessins (y compris la reconnaissance de formes).
    - Appelez [**AnalyzeAsync**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.AnalyzeAsync) pour lancer l’analyse des entrées manuscrites et obtenir le [**InkAnalysisResult**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult).
    - Si [**État**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult.Status) renvoie à l’état **Mis à jour**, appelez [**FindNodes**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisroot.findnodes) pour les deux [**InkAnalysisNodeKind.InkWord**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind) et [**InkAnalysisNodeKind.InkDrawing**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind).
    - Itérez à travers tous les ensembles de types de nœuds et tracez le texte ou la forme respectifs sur le canevas de reconnaissance (sous le canevas d’encre).
    - Enfin, supprimez les nœuds reconnus de l’InkAnalyzer et les traits d’encre correspondants du canevas d’encre.
```csharp
/// <summary>
/// The "Analyze" button click handler.
/// Ink recognition is performed here.
/// </summary>
/// <param name="sender">Source of the click event</param>
/// <param name="e">Event args for the button click routed event</param>
private async void RecognizeStrokes_Click(object sender, RoutedEventArgs e)
    {
        inkStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
        // Ensure an ink stroke is present.
        if (inkStrokes.Count > 0)
        {
            inkAnalyzer.AddDataForStrokes(inkStrokes);

            // In this example, we try to recognizing both 
            // writing and drawing, so the platform default 
            // of "InkAnalysisStrokeKind.Auto" is used.
            // If you're only interested in a specific type of recognition,
            // such as writing or drawing, you can constrain recognition 
            // using the SetStrokDataKind method as follows:
            // foreach (var stroke in strokesText)
            // {
            //     analyzerText.SetStrokeDataKind(
            //      stroke.Id, InkAnalysisStrokeKind.Writing);
            // }
            // This can improve both efficiency and recognition results.
            inkAnalysisResults = await inkAnalyzer.AnalyzeAsync();

            // Have ink strokes on the canvas changed?
            if (inkAnalysisResults.Status == InkAnalysisStatus.Updated)
            {
                // Find all strokes that are recognized as handwriting and 
                // create a corresponding ink analysis InkWord node.
                var inkwordNodes = 
                    inkAnalyzer.AnalysisRoot.FindNodes(
                        InkAnalysisNodeKind.InkWord);

                // Iterate through each InkWord node.
                // Draw primary recognized text on recognitionCanvas 
                // (for this example, we ignore alternatives), and delete 
                // ink analysis data and recognized strokes.
                foreach (InkAnalysisInkWord node in inkwordNodes)
                {
                    // Draw a TextBlock object on the recognitionCanvas.
                    DrawText(node.RecognizedText, node.BoundingRect);

                    foreach (var strokeId in node.GetStrokeIds())
                    {
                        var stroke = 
                            inkCanvas.InkPresenter.StrokeContainer.GetStrokeById(strokeId);
                        stroke.Selected = true;
                    }
                    inkAnalyzer.RemoveDataForStrokes(node.GetStrokeIds());
                }
                inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();

                // Find all strokes that are recognized as a drawing and 
                // create a corresponding ink analysis InkDrawing node.
                var inkdrawingNodes =
                    inkAnalyzer.AnalysisRoot.FindNodes(
                        InkAnalysisNodeKind.InkDrawing);
                // Iterate through each InkDrawing node.
                // Draw recognized shapes on recognitionCanvas and
                // delete ink analysis data and recognized strokes.
                foreach (InkAnalysisInkDrawing node in inkdrawingNodes)
                {
                    if (node.DrawingKind == InkAnalysisDrawingKind.Drawing)
                    {
                        // Catch and process unsupported shapes (lines and so on) here.
                    }
                    // Process generalized shapes here (ellipses and polygons).
                    else
                    {
                        // Draw an Ellipse object on the recognitionCanvas (circle is a specialized ellipse).
                        if (node.DrawingKind == InkAnalysisDrawingKind.Circle || node.DrawingKind == InkAnalysisDrawingKind.Ellipse)
                        {
                            DrawEllipse(node);
                        }
                        // Draw a Polygon object on the recognitionCanvas.
                        else
                        {
                            DrawPolygon(node);
                        }
                        foreach (var strokeId in node.GetStrokeIds())
                        {
                            var stroke = inkCanvas.InkPresenter.StrokeContainer.GetStrokeById(strokeId);
                            stroke.Selected = true;
                        }
                    }
                    inkAnalyzer.RemoveDataForStrokes(node.GetStrokeIds());
                }
                inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
            }
        }
    }
```
6. Voici la fonction pour tracer un TextBlock sur notre canevas de reconnaissance. Nous utilisons le rectangle englobant du trait d’encre associé sur le canevas d’encre pour définir la position et la taille de police du TextBlock.
```csharp
/// <summary>
/// Draw ink recognition text string on the recognitionCanvas.
/// </summary>
/// <param name="recognizedText">The string returned by text recognition.</param>
/// <param name="boundingRect">The bounding rect of the original ink writing.</param>
private void DrawText(string recognizedText, Rect boundingRect)
{
    TextBlock text = new TextBlock();
    Canvas.SetTop(text, boundingRect.Top);
    Canvas.SetLeft(text, boundingRect.Left);

    text.Text = recognizedText;
    text.FontSize = boundingRect.Height;

    recognitionCanvas.Children.Add(text);
}
```
7. Voici les fonctions qui permettent de tracer des ellipses et des polygones sur notre canevas de reconnaissance. Nous utilisons le rectangle englobant du trait d’encre associé sur le canevas d’encre pour définir la position et la taille de police des formes.
```csharp
    // Draw an ellipse on the recognitionCanvas.
    private void DrawEllipse(InkAnalysisInkDrawing shape)
    {
        var points = shape.Points;
        Ellipse ellipse = new Ellipse();

        ellipse.Width = shape.BoundingRect.Width;
        ellipse.Height = shape.BoundingRect.Height;

        Canvas.SetTop(ellipse, shape.BoundingRect.Top);
        Canvas.SetLeft(ellipse, shape.BoundingRect.Left);

        var brush = new SolidColorBrush(Windows.UI.ColorHelper.FromArgb(255, 0, 0, 255));
        ellipse.Stroke = brush;
        ellipse.StrokeThickness = 2;
        recognitionCanvas.Children.Add(ellipse);
    }

    // Draw a polygon on the recognitionCanvas.
    private void DrawPolygon(InkAnalysisInkDrawing shape)
    {
        List<Point> points = new List<Point>(shape.Points);
        Polygon polygon = new Polygon();

        foreach (Point point in points)
        {
            polygon.Points.Add(point);
        }

        var brush = new SolidColorBrush(Windows.UI.ColorHelper.FromArgb(255, 0, 0, 255));
        polygon.Stroke = brush;
        polygon.StrokeThickness = 2;
        recognitionCanvas.Children.Add(polygon);
    }
```

Voici un exemple en contexte:

| Avant l’analyse | Après l’analyse |
| --- | --- |
| ![Avant l’analyse](images/ink/ink-analysis-raw2-small.png) | ![Après l’analyse](images/ink/ink-analysis-analyzed2-small.png) |

---

## <a name="constrained-handwriting-recognition"></a>Reconnaissance de l’écriture manuscrite contrainte

Dans la section précédente ([Reconnaissance des formes libres avec analyse des entrées manuscrites](#free-form-recognition-with-ink-analysis)), nous avons montré comment utiliser les [API d’analyse d’entrées manuscrites](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis) pour analyser et reconnaître les traits d’encre arbitraires dans une zone InkCanvas.

Dans cette section nous présentons comment utiliser le moteur de reconnaissance de l’écriture manuscrite WindowsInk (et non l’analyse des entrées manuscrites) pour convertir un ensemble de traits sur un [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) en texte (basé sur le module linguistique installé par défaut).

> [!NOTE]
> La reconnaissance de base de l’écriture manuscrite présentée dans cette section est idéale pour les scénarios d’entrée de texte en lignes simples, par exemple une entrée de formulaire. Pour les scénarios de reconnaissance plus riches qui incluent l’analyse et l’interprétation de la structure du document, des éléments de liste, des formes et des dessins (en plus de la reconnaissance du texte), consultez la section précédente: [Reconnaissance de formes libres avec analyse des entrées manuscrites](#free-form-recognition-with-ink-analysis).

Dans cet exemple, l’utilisateur lance la reconnaissance en cliquant sur un bouton pour indiquer qu’il a terminé d’écrire.

**Télécharger cet exemple depuis [Exemple de reconnaissance de l’écriture manuscrite WindowsInk (de base)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)**

1.  Tout d’abord, nous configurons l’interface utilisateur.

    L’interface utilisateur inclut un bouton Reconnaître, [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) et une zone d’affichage des résultats de la reconnaissance.    
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel"
                    Orientation="Horizontal"
                    Grid.Row="0">
            <TextBlock x:Name="Header"
                       Text="Basic ink recognition sample"
                       Style="{ThemeResource HeaderTextBlockStyle}"
                       Margin="10,0,0,0" />
            <Button x:Name="recognize"
                    Content="Recognize"
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="Auto"/>
            </Grid.RowDefinitions>
            <InkCanvas x:Name="inkCanvas"
                       Grid.Row="0"/>
            <TextBlock x:Name="recognitionResult"
                       Grid.Row="1"
                       Margin="50,0,10,0"/>
        </Grid>
    </Grid>
```

2. Pour cet exemple, vous devez tout d’abord ajouter les références de type espace de noms, requis pour notre fonctionnalité d’entrée manuscrite:
    - [Windows.UI.Input](https://docs.microsoft.com/uwp/api/windows.ui.input)
    - [Windows.UI.Input.Inking](https://docs.microsoft.com/uwp/api/windows.ui.input.inking)


3.  Nous définissons ensuite certains comportements de base d’entrée manuscrite

    L’élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) est configuré pour interpréter les données d’entrée de stylet et de souris sous forme de traits d’encre ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)). Les traits d’encre sont restitués sur [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) à l’aide de l’élément [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050) spécifié. Un écouteur de l’événement Click est également déclaré sur le bouton «Reconnaître».
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

        // Listen for button click to initiate recognition.
        recognize.Click += Recognize_Click;
    }
```

4.  Enfin, nous procédons à la reconnaissance de l’écriture manuscrite de base. Pour cet exemple, nous utilisons le gestionnaire d’événements Click du bouton «Reconnaître» pour procéder à la reconnaissance de l’écriture manuscrite.

    Un élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) stocke tous les traits d’encre dans un objet [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492). Les traits sont exposés par le biais de la propriété [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766) de l’élément **InkPresenter** et récupérés à l’aide de la méthode [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499).
```csharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    An [**InkRecognizerContainer**](https://msdn.microsoft.com/library/windows/apps/br208479) is created to manage the handwriting recognition process.

```csharp
// Create a manager for the InkRecognizer object
    // used in handwriting recognition.
    InkRecognizerContainer inkRecognizerContainer =
        new InkRecognizerContainer();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).

```csharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).

```csharp
string str = "Recognition result\n";
    // Iterate through the recognition results.
    foreach (var result in recognitionResults)
    {
        // Get all recognition candidates from each recognition result.
        IReadOnlyList<string> candidates = result.GetTextCandidates();
        str += "Candidates: " + candidates.Count.ToString() + "\n";
        foreach (string candidate in candidates)
        {
            str += candidate + " ";
        }
    }
    // Display the recognition candidates.
    recognitionResult.Text = str;
    // Clear the ink canvas once recognition is complete.
    inkCanvas.InkPresenter.StrokeContainer.Clear();
```

    Here's the click handler example, in full.

```csharp
// Handle button click to initiate recognition.
    private async void Recognize_Click(object sender, RoutedEventArgs e)
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

        // Ensure an ink stroke is present.
        if (currentStrokes.Count > 0)
        {
            // Create a manager for the InkRecognizer object
            // used in handwriting recognition.
            InkRecognizerContainer inkRecognizerContainer =
                new InkRecognizerContainer();

            // inkRecognizerContainer is null if a recognition engine is not available.
            if (!(inkRecognizerContainer == null))
            {
                // Recognize all ink strokes on the ink canvas.
                IReadOnlyList<InkRecognitionResult> recognitionResults =
                    await inkRecognizerContainer.RecognizeAsync(
                        inkCanvas.InkPresenter.StrokeContainer,
                        InkRecognitionTarget.All);
                // Process and display the recognition results.
                if (recognitionResults.Count > 0)
                {
                    string str = "Recognition result\n";
                    // Iterate through the recognition results.
                    foreach (var result in recognitionResults)
                    {
                        // Get all recognition candidates from each recognition result.
                        IReadOnlyList<string> candidates = result.GetTextCandidates();
                        str += "Candidates: " + candidates.Count.ToString() + "\n";
                        foreach (string candidate in candidates)
                        {
                            str += candidate + " ";
                        }
                    }
                    // Display the recognition candidates.
                    recognitionResult.Text = str;
                    // Clear the ink canvas once recognition is complete.
                    inkCanvas.InkPresenter.StrokeContainer.Clear();
                }
                else
                {
                    recognitionResult.Text = "No recognition results.";
                }
            }
            else
            {
                Windows.UI.Popups.MessageDialog messageDialog = new Windows.UI.Popups.MessageDialog("You must install handwriting recognition engine.");
                await messageDialog.ShowAsync();
            }
        }
        else
        {
            recognitionResult.Text = "No ink strokes to recognize.";
        }
    }
```

## <a name="international-recognition"></a>Reconnaissance internationale

La reconnaissance de l’écriture manuscrite intégrée à la plateforme d’entrée manuscrite Windows inclut un sous-ensemble étendu de paramètres régionaux et de langues pris en charge par Windows.

Consultez la rubrique portant sur la propriété [**InkRecognizer.Name**](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkrecognizer.name.aspx) pour obtenir la liste des langues prises en charge par l’élément [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478) .

Votre application peut interroger l’ensemble des moteurs de reconnaissance de l’écriture manuscrite installés et utiliser l’un d’entre eux ou laisser un utilisateur sélectionner sa langue par défaut.

**Remarque**  les utilisateurs peuvent voir une liste des langues installées en accédant à **paramètres -&gt; heure et langue**. Les langues installées figurent sous **Langues**.

Pour installer de nouveaux modules linguistiques et activer la reconnaissance de l’écriture manuscrite pour cette langue, procédez comme suit:

1.  Accédez à **Paramètres &gt; Heure et langue &gt; Région et langue**.
2.  Cliquez sur **Ajouter une langue**.
3.  Sélectionnez une langue de la liste, puis choisissez la version de la région. La langue est désormais répertoriée dans la page **Région et langue**.
4.  Cliquez sur la langue et sélectionnez **Options**.
5.  Dans la page **Options de langue**, téléchargez le **moteur de reconnaissance de l’écriture manuscrite** (vous pouvez également télécharger le module linguistique dans son intégralité, le moteur de reconnaissance vocale et la disposition du clavier ici).

 

Nous montrons ici comment utiliser le moteur de reconnaissance de l’écriture manuscrite pour interpréter un ensemble de traits sur un élément [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) basé sur le module de reconnaissance sélectionné.

L’utilisateur lance la reconnaissance en cliquant sur un bouton une fois qu’il a terminé d’écrire.

1.  Tout d’abord, nous configurons l’interface utilisateur.

    L’interface utilisateur inclut un bouton Reconnaître, une zone de liste modifiable qui répertorie toutes les modules de reconnaissance de l’écriture manuscrite installés, l’élément [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535), et une zone d’affichage des résultats de reconnaissance.
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel"
                    Orientation="Horizontal"
                    Grid.Row="0">
            <TextBlock x:Name="Header"
                       Text="Advanced international ink recognition sample"
                       Style="{ThemeResource HeaderTextBlockStyle}"
                       Margin="10,0,0,0" />
            <ComboBox x:Name="comboInstalledRecognizers"
                     Margin="50,0,10,0">
                <ComboBox.ItemTemplate>
                    <DataTemplate>
                        <StackPanel Orientation="Horizontal">
                            <TextBlock Text="{Binding Name}" />
                        </StackPanel>
                    </DataTemplate>
                </ComboBox.ItemTemplate>
            </ComboBox>
            <Button x:Name="buttonRecognize"
                    Content="Recognize"
                    IsEnabled="False"
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="Auto"/>
            </Grid.RowDefinitions>
            <InkCanvas x:Name="inkCanvas"
                       Grid.Row="0"/>
            <TextBlock x:Name="recognitionResult"
                       Grid.Row="1"
                       Margin="50,0,10,0"/>
        </Grid>
    </Grid>
```

2.  Nous définissons ensuite certains comportements d’entrée manuscrite de base.

    L’élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) est configuré pour interpréter les données d’entrée de stylet et de souris sous forme de traits d’encre ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)). Les traits d’encre sont restitués sur [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) à l’aide de l’élément [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050) spécifié.

    Nous appelons une fonction `InitializeRecognizerList` pour remplir la zone de liste modifiable du module de reconnaissance avec une liste des modules de reconnaissance de l’écriture manuscrite installés.

    Nous déclarons également des écouteurs pour l’événement Click sur le bouton Reconnaître et l’événement modifié de la sélection sur la zone de liste modifiable du module de reconnaissance.
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

         // Populate the recognizer combo box with installed recognizers.
         InitializeRecognizerList();

         // Listen for combo box selection.
         comboInstalledRecognizers.SelectionChanged +=
             comboInstalledRecognizers_SelectionChanged;

         // Listen for button click to initiate recognition.
         buttonRecognize.Click += Recognize_Click;
     }
```

3.  Nous remplissons la zone de liste modifiable du module de reconnaissance avec une liste des modules de reconnaissance de l’écriture manuscrite installés.

    Un élément [**InkRecognizerContainer**](https://msdn.microsoft.com/library/windows/apps/br208479) est créé pour gérer le processus de reconnaissance manuscrite. Utilisez cet objet pour appeler [**GetRecognizers**](https://msdn.microsoft.com/library/windows/apps/br208480) et récupérer la liste des modules de reconnaissance installés pour remplir la zone de liste modifiable du module de reconnaissance.
```csharp
// Populate the recognizer combo box with installed recognizers.
    private void InitializeRecognizerList()
    {
        // Create a manager for the handwriting recognition process.
        inkRecognizerContainer = new InkRecognizerContainer();
        // Retrieve the collection of installed handwriting recognizers.
        IReadOnlyList<InkRecognizer> installedRecognizers =
            inkRecognizerContainer.GetRecognizers();
        // inkRecognizerContainer is null if a recognition engine is not available.
        if (!(inkRecognizerContainer == null))
        {
            comboInstalledRecognizers.ItemsSource = installedRecognizers;
            buttonRecognize.IsEnabled = true;
        }
    }
```

4.  Mettez à jour le module de reconnaissance de l’écriture manuscrite en cas de modification de la sélection de la zone de liste modifiable.

    Utilisez [**InkRecognizerContainer**](https://msdn.microsoft.com/library/windows/apps/br208479) pour appeler [**SetDefaultRecognizer**](https://msdn.microsoft.com/library/windows/apps/hh920328) en fonction du module de reconnaissance sélectionné à partir de la zone de liste modifiable correspondante.
```csharp
// Handle recognizer change.
    private void comboInstalledRecognizers_SelectionChanged(
        object sender, SelectionChangedEventArgs e)
    {
        inkRecognizerContainer.SetDefaultRecognizer(
            (InkRecognizer)comboInstalledRecognizers.SelectedItem);
    }
```

5.  Enfin, nous effectuons la reconnaissance de l’écriture manuscrite en fonction du module de reconnaissance sélectionné. Pour cet exemple, nous utilisons le gestionnaire d’événements Click du bouton «Reconnaître» pour procéder à la reconnaissance de l’écriture manuscrite.

    Un élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) stocke tous les traits d’encre dans un objet [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492). Les traits sont exposés par le biais de la propriété [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766) de **l’InkPresenter** et récupérés à l’aide de la méthode [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499).
```csharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes =
        inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).

```csharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).

```csharp
string str = "Recognition result\n";
    // Iterate through the recognition results.
    foreach (InkRecognitionResult result in recognitionResults)
    {
        // Get all recognition candidates from each recognition result.
        IReadOnlyList<string> candidates =
            result.GetTextCandidates();
        str += "Candidates: " + candidates.Count.ToString() + "\n";
        foreach (string candidate in candidates)
        {
            str += candidate + " ";
        }
    }
    // Display the recognition candidates.
    recognitionResult.Text = str;
    // Clear the ink canvas once recognition is complete.
    inkCanvas.InkPresenter.StrokeContainer.Clear();
```

    Here's the click handler example, in full.
    
```csharp
// Handle button click to initiate recognition.
    private async void Recognize_Click(object sender, RoutedEventArgs e)
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes =
            inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

        // Ensure an ink stroke is present.
        if (currentStrokes.Count > 0)
        {
            // inkRecognizerContainer is null if a recognition engine is not available.
            if (!(inkRecognizerContainer == null))
            {
                // Recognize all ink strokes on the ink canvas.
                IReadOnlyList<InkRecognitionResult> recognitionResults =
                    await inkRecognizerContainer.RecognizeAsync(
                        inkCanvas.InkPresenter.StrokeContainer,
                        InkRecognitionTarget.All);
                // Process and display the recognition results.
                if (recognitionResults.Count > 0)
                {
                    string str = "Recognition result\n";
                    // Iterate through the recognition results.
                    foreach (InkRecognitionResult result in recognitionResults)
                    {
                        // Get all recognition candidates from each recognition result.
                        IReadOnlyList<string> candidates =
                            result.GetTextCandidates();
                        str += "Candidates: " + candidates.Count.ToString() + "\n";
                        foreach (string candidate in candidates)
                        {
                            str += candidate + " ";
                        }
                    }
                    // Display the recognition candidates.
                    recognitionResult.Text = str;
                    // Clear the ink canvas once recognition is complete.
                    inkCanvas.InkPresenter.StrokeContainer.Clear();
                }
                else
                {
                    recognitionResult.Text = "No recognition results.";
                }
            }
            else
            {
                Windows.UI.Popups.MessageDialog messageDialog =
                    new Windows.UI.Popups.MessageDialog(
                        "You must install handwriting recognition engine.");
                await messageDialog.ShowAsync();
            }
        }
        else
        {
            recognitionResult.Text = "No ink strokes to recognize.";
        }
    }
```

## <a name="dynamic-recognition"></a>Reconnaissance dynamique

Alors que les deux exemples précédents exigent l’utilisateur appuie sur un bouton pour lancer la reconnaissance, vous pouvez également effectuer la reconnaissance dynamique à l’aide de la saisie de traits associée à une fonction de chronométrage de base.

Pour cet exemple, nous allons utiliser les mêmes paramètres de trait et d’interface utilisateur que l’exemple de reconnaissance internationale précédent.

1. Ces objets globaux ([InkAnalyzer](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalyzer), [InkStroke](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstroke), [InkAnalysisResult](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult), [DispatcherTimer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dispatchertimer)) sont utilisés dans l’ensemble de notre application.    
```csharp
    // Stroke recognition globals.
    InkAnalyzer inkAnalyzer;
    DispatcherTimer recoTimer;
```

2.  Au lieu d’un bouton pour lancer la reconnaissance, nous ajoutons des écouteurs pour deuxévénements de traits [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) ([**StrokesCollected**](https://msdn.microsoft.com/library/windows/apps/dn922024) et [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702)) et configurons un minuteur de base ([**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250)) avec un intervalle [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256) d’uneseconde.    
```csharp
    public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Listen for stroke events on the InkPresenter to 
        // enable dynamic recognition.
        // StrokesCollected is fired when the user stops inking by 
        // lifting their pen or finger, or releasing the mouse button.
        inkCanvas.InkPresenter.StrokesCollected += inkCanvas_StrokesCollected;
        // StrokeStarted is fired when ink input is first detected.
        inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
            inkCanvas_StrokeStarted;

        inkAnalyzer = new InkAnalyzer();

        // Timer to manage dynamic recognition.
        recoTimer = new DispatcherTimer();
        recoTimer.Interval = TimeSpan.FromSeconds(1);
        recoTimer.Tick += recoTimer_TickAsync;
    }
```

3.  Nous définissons ensuite des gestionnaires pour les événements InkPresenter que nous avons déclarés lors de la première étape (nous avons également remplacé l’événement de page [**OnNavigatingFrom**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.page.onnavigatingfrom) pour gérer le minuteur).

    - [**StrokesCollected**](https://msdn.microsoft.com/library/windows/apps/dn922024)  
    Ajoutez des traits d’encre ([**AddDataForStrokes**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.adddataforstrokes)) au InkAnalyzer et démarrez le minuteur de reconnaissance dès que l’utilisateur arrête l’entrée manuscrite (en levant son stylet ou un doigt ou en relâchant le bouton de souris). La reconnaissance est lancée dès que l’utilisateur arrête d’écrire pendant plus d’une seconde.  

        Utilisez la méthode [**SetStrokeDataKind**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.setstrokedatakind) pour indiquer si vous êtes intéressé uniquement par le texte (y compris la structure du document et les listes à puces) ou par les dessins (y compris la reconnaissance de formes).

    - [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702)  
    Arrête la minuterie sachant que le nouveau trait est probablement la suite d’une entrée manuscrite unique, si un nouveau trait commence avant le prochain battement d’horloge.
```csharp
    // Handler for the InkPresenter StrokeStarted event.
    // Don't perform analysis while a stroke is in progress.
    // If a new stroke starts before the next timer tick event,
    // stop the timer as the new stroke is likely the continuation
    // of a single handwriting entry.
    private void inkCanvas_StrokeStarted(InkStrokeInput sender, PointerEventArgs args)
    {
        recoTimer.Stop();
    }    
    // Handler for the InkPresenter StrokesCollected event.
    // Stop the timer and add the collected strokes to the InkAnalyzer.
    // Start the recognition timer when the user stops inking (by 
    // lifting their pen or finger, or releasing the mouse button).
    // If ink input is not detected after one second, initiate recognition.
    private void inkCanvas_StrokesCollected(InkPresenter sender, InkStrokesCollectedEventArgs args)
    {
        recoTimer.Stop();
        // If you're only interested in a specific type of recognition,
        // such as writing or drawing, you can constrain recognition 
        // using the SetStrokDataKind method, which can improve both 
        // efficiency and recognition results.
        // In this example, "InkAnalysisStrokeKind.Writing" is used.
        foreach (var stroke in args.Strokes)
        {
            inkAnalyzer.AddDataForStroke(stroke);
            inkAnalyzer.SetStrokeDataKind(stroke.Id, InkAnalysisStrokeKind.Writing);
        }
        recoTimer.Start();
    }    
    // Override the Page OnNavigatingFrom event handler to 
    // stop our timer if user leaves page.
    protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
    {
        recoTimer.Stop();
    } 
```

4.  Enfin, nous procédons à la reconnaissance de l’écriture manuscrite. Pour cet exemple, nous utilisons le gestionnaire d’événements [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256) d’un élément [**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250) pour lancer la reconnaissance de l’écriture manuscrite.
    - Appelez [**AnalyzeAsync**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.AnalyzeAsync) pour lancer l’analyse des entrées manuscrites et obtenir le [**InkAnalysisResult**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult).
    - Si [**État**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult.Status) renvoie à l’état **Mis à jour**, appelez [**FindNodes**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisroot.findnodes) pour les types de nœuds de [**InkAnalysisNodeKind.InkWord**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind).
    - Parcourez les nœuds et affichez le texte reconnu.
    - Enfin, supprimez les nœuds reconnus de l’InkAnalyzer et les traits d’encre correspondants du canevas d’encre.
```csharp
    private async void recoTimer_TickAsync(object sender, object e)
    {
        recoTimer.Stop();
        if (!inkAnalyzer.IsAnalyzing)
        {
            InkAnalysisResult result = await inkAnalyzer.AnalyzeAsync();

            // Have ink strokes on the canvas changed?
            if (result.Status == InkAnalysisStatus.Updated)
            {
                // Find all strokes that are recognized as handwriting and 
                // create a corresponding ink analysis InkWord node.
                var inkwordNodes =
                    inkAnalyzer.AnalysisRoot.FindNodes(
                        InkAnalysisNodeKind.InkWord);

                // Iterate through each InkWord node.
                // Display the primary recognized text (for this example, 
                // we ignore alternatives), and then delete the 
                // ink analysis data and recognized strokes.
                foreach (InkAnalysisInkWord node in inkwordNodes)
                {
                    string recognizedText = node.RecognizedText;
                    // Display the recognition candidates.
                    recognitionResult.Text = recognizedText;

                    foreach (var strokeId in node.GetStrokeIds())
                    {
                        var stroke =
                            inkCanvas.InkPresenter.StrokeContainer.GetStrokeById(strokeId);
                        stroke.Selected = true;
                    }
                    inkAnalyzer.RemoveDataForStrokes(node.GetStrokeIds());
                }
                inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
            }
        }
        else
        {
            // Ink analyzer is busy. Wait a while and try again.
            recoTimer.Start();
        }
    }
```

## <a name="related-articles"></a>Articles connexes

* [Interactions avec le stylo et le stylet](pen-and-stylus-interactions.md)

**Exemples de la rubrique**
* [Exemple d’analyse des entrées manuscrites (de base) (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)
* [Exemple de reconnaissance de l’écriture manuscrite WindowsInk (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)

**Autres exemples**
* [Exemple d’entrée manuscrite simple (C#/C++)](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Exemple d’entrée manuscrite complexe (C++)](http://go.microsoft.com/fwlink/p/?LinkID=620314)
* [Exemple d’entrée manuscrite (JavaScript)](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Prise en main du didacticiel: prendre en charge l’entrée manuscrite dans votre application UWP](https://aka.ms/appsample-ink)
* [Exemple de livre de coloriage](https://aka.ms/cpubsample-coloringbook)
* [Exemple de notes de famille](https://aka.ms/cpubsample-familynotessample)


 
