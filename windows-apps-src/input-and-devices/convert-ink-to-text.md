---
author: Karl-Bridge-Microsoft
Description: "Convertissez des traits d’encre en texte à l’aide de la reconnaissance de l’écriture manuscrite ou en formes à l’aide de la reconnaissance personnalisée."
title: "Reconnaître les traits d’encre Windows en tant que texte"
ms.assetid: C2F3F3CE-737F-4652-98B7-5278A462F9D3
label: Recognize Windows Ink strokes as text
template: detail.hbs
keyword: Windows Ink, Windows Inking, DirectInk, InkPresenter, InkCanvas, handwriting recognition
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 4351901984f9b1f134cfc42acbbc7f756dc6c11f

---

# Reconnaître les traits d’encre Windows en tant que texte

Convertissez des traits d’encre en texte à l’aide de la reconnaissance de l’écriture manuscrite ou en formes à l’aide de la reconnaissance personnalisée.

**API importantes**

-   [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)
-   [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)


La reconnaissance de l’écriture manuscrite est intégrée à la plateforme d’entrée manuscrite Windows, et prend en charge un ensemble étendu de paramètres régionaux et de langues.

Pour tous les exemples ici, ajoutez les références d’espaces de noms nécessaires au fonctionnement de l’écriture manuscrite Cela comprend «Windows.UI.Input.Inking».

## Reconnaissance de l’écriture manuscrite de base


Nous montrons ici comment utiliser le moteur de reconnaissance de l’écriture manuscrite associé au module linguistique installé par défaut pour interpréter un ensemble de traits sur un [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535).

L’utilisateur lance la reconnaissance en cliquant sur un bouton une fois qu’il a terminé d’écrire.

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

2.  Nous définissons ensuite certains comportements d’entrée manuscrite de base.

    L’élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) est configuré pour interpréter les données d’entrée de stylet et de souris sous forme de traits d’encre ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)). Les traits d’encre sont restitués sur [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) à l’aide de l’élément [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050) spécifié. Un écouteur de l’événement Click est également déclaré sur le bouton «Reconnaître».
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

        // Listen for button click to initiate recognition.
        recognize.Click += Recognize_Click;
    }
```

3.  Enfin, nous procédons à la reconnaissance de l’écriture manuscrite de base. Pour cet exemple, nous utilisons le gestionnaire d’événements Click du bouton «Reconnaître» pour procéder à la reconnaissance de l’écriture manuscrite.

    Un élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) stocke tous les traits d’encre dans un objet [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492). Les traits sont exposés par le biais de la propriété [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766) de l’élément **InkPresenter** et récupérés à l’aide de la méthode [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499).
```    CSharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    An [**InkRecognizerContainer**](https://msdn.microsoft.com/library/windows/apps/br208479) is created to manage the handwriting recognition process.
```    CSharp
// Create a manager for the InkRecognizer object
    // used in handwriting recognition.
    InkRecognizerContainer inkRecognizerContainer =
        new InkRecognizerContainer();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).
```    CSharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).
```    CSharp
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
```    CSharp
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

## Reconnaissance internationale


Il est possible d’utiliser un sous-ensemble complet des langues prises en charge par Windows pour la reconnaissance de l’écriture manuscrite.

Pour obtenir la liste des langues prises en charge par l’élément [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478), voir la propriété [**InkRecognizer.Name**](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkrecognizer.name.aspx).

Votre application peut interroger l’ensemble des moteurs de reconnaissance de l’écriture manuscrite installés et utiliser l’un d’entre eux ou laisser un utilisateur sélectionner sa langue par défaut.

**Remarque**  
Les utilisateurs peuvent consulter la liste des langues installées en accédant à **Paramètres-&gt; Heure et langue**. Les langues installées figurent sous **Langues**.

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
```    CSharp
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
```    CSharp
// Handle recognizer change.
    private void comboInstalledRecognizers_SelectionChanged(
        object sender, SelectionChangedEventArgs e)
    {
        inkRecognizerContainer.SetDefaultRecognizer(
            (InkRecognizer)comboInstalledRecognizers.SelectedItem);
    }
```

5.  Enfin, nous effectuons la reconnaissance de l’écriture manuscrite en fonction du module de reconnaissance sélectionné. Pour cet exemple, nous utilisons le gestionnaire d’événements Click du bouton «Reconnaître» pour procéder à la reconnaissance de l’écriture manuscrite.

    Un élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) stocke tous les traits d’encre dans un objet [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492). Les traits sont exposés par le biais de la propriété [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766) de l’élément **InkPresenter** et récupérés à l’aide de la méthode [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499).
```    CSharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes =
        inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).
```    CSharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).
```    CSharp
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
```    CSharp
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

## Reconnaissance de l’écriture manuscrite dynamique


Les deux exemples précédents exigent que l’utilisateur appuie sur un bouton pour lancer la reconnaissance. Votre application peut également procéder à la reconnaissance dynamique à l’aide de la saisie de traits associée à une fonction de chronométrage de base.

Pour cet exemple, nous allons utiliser les mêmes paramètres de trait et d’interface utilisateur que l’exemple de reconnaissance internationale précédent.

1.  Comme les exemples précédents, l’élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) est configuré pour interpréter les données d’entrée de stylet et de souris sous forme de traits d’encre ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)), et les traits d’encre sont restitués sur [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) à l’aide de l’élément [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050) spécifié.

    Au lieu d’un bouton pour lancer la reconnaissance, nous ajoutons des écouteurs pour deuxévénements de traits [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) ([**StrokesCollected**](https://msdn.microsoft.com/library/windows/apps/dn922024) et [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702)) et configurons un minuteur de base ([**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250)) avec un intervalle [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256) d’uneseconde.    
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

        // Populate the recognizer combo box with installed recognizers.
        InitializeRecognizerList();

        // Listen for combo box selection.
        comboInstalledRecognizers.SelectionChanged +=
            comboInstalledRecognizers_SelectionChanged;

        // Listen for stroke events on the InkPresenter to
        // enable dynamic recognition.
        // StrokesCollected is fired when the user stops inking by
        // lifting their pen or finger, or releasing the mouse button.
        inkCanvas.InkPresenter.StrokesCollected +=
            inkCanvas_StrokesCollected;
        // StrokeStarted is fired when ink input is first detected.
        inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
            inkCanvas_StrokeStarted;

        // Timer to manage dynamic recognition.
        recoTimer = new DispatcherTimer();
        recoTimer.Interval = new TimeSpan(0, 0, 1);
        recoTimer.Tick += recoTimer_Tick;
    }

    // Handler for the timer tick event calls the recognition function.
    private void recoTimer_Tick(object sender, object e)
    {
        Recognize_Tick();
    }

    // Handler for the InkPresenter StrokeStarted event.
    // If a new stroke starts before the next timer tick event,
    // stop the timer as the new stroke is likely the continuation
    // of a single handwriting entry.
    private void inkCanvas_StrokeStarted(InkStrokeInput sender, PointerEventArgs args)
    {
        recoTimer.Stop();
    }

    // Handler for the InkPresenter StrokesCollected event.
    // Start the recognition timer when the user stops inking by
    // lifting their pen or finger, or releasing the mouse button.
    // After one second of no ink input, recognition is initiated.
    private void inkCanvas_StrokesCollected(InkPresenter sender, InkStrokesCollectedEventArgs args)
    {
        recoTimer.Start();
    }    
```

2.  Voici les gestionnaires pour les troisévénements que nous avons ajoutés à la première étape.

    [**StrokesCollected**](https://msdn.microsoft.com/library/windows/apps/dn922024)  
    Démarre le minuteur de reconnaissance lorsque l’utilisateur arrête l’entrée manuscrite en levant son stylet ou son doigt ou en relâchant le bouton de la souris. La reconnaissance est lancée dès que l’utilisateur arrête d’écrire pendant plus d’une seconde.

    [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702)  
    Arrête la minuterie sachant que le nouveau trait est probablement la suite d’une entrée manuscrite unique, si un nouveau trait commence avant le prochain battement d’horloge.

    [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256)  
    Appelle la fonction de reconnaissance dès que l’utilisateur arrête d’écrire pendant plus d’une seconde.
```    CSharp
// Handler for the timer tick event calls the recognition function.
    private void recoTimer_Tick(object sender, object e)
    {
        Recognize_Tick();
    }

    // Handler for the InkPresenter StrokeStarted event.
    // If a new stroke starts before the next timer tick event,
    // stop the timer as the new stroke is likely the continuation
    // of a single handwriting entry.
    private void inkCanvas_StrokeStarted(InkStrokeInput sender, PointerEventArgs args)
    {
        recoTimer.Stop();
    }

    // Handler for the InkPresenter StrokesCollected event.
    // Start the recognition timer when the user stops inking by
    // lifting their pen or finger, or releasing the mouse button.
    // After one second of no ink input, recognition is initiated.
    private void inkCanvas_StrokesCollected(InkPresenter sender, InkStrokesCollectedEventArgs args)
    {
        recoTimer.Start();
    }
```

3.  Enfin, nous effectuons la reconnaissance de l’écriture manuscrite en fonction du module de reconnaissance sélectionné. Pour cet exemple, nous utilisons le gestionnaire d’événements [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256) d’un élément [**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250) pour lancer la reconnaissance de l’écriture manuscrite.

    Un élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) stocke tous les traits d’encre dans un objet [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492). Les traits sont exposés par le biais de la propriété [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766) de l’élément **InkPresenter** et récupérés à l’aide de la méthode [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499).
```    CSharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).
```    CSharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).
```    CSharp
string str = "Recognition result\n";
    // Iterate through the recognition results.
    foreach (InkRecognitionResult result in recognitionResults)
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

    Here's the recognition function, in full.
```    CSharp
// Respond to timer Tick and initiate recognition.
    private async void Recognize_Tick()
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

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

        // Stop the dynamic recognition timer.
        recoTimer.Stop();
    }
```

## Articles connexes

* [Interactions avec le stylo et le stylet](pen-and-stylus-interactions.md)

**Exemples**
* [Exemple d’entrée manuscrite](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Exemple d’entrée manuscrite simple](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Exemple d’entrée manuscrite complexe](http://go.microsoft.com/fwlink/p/?LinkID=620314)
 

 



<!--HONumber=Jul16_HO2-->


