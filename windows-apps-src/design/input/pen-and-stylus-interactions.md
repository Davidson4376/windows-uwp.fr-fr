---
Description: Build Universal Windows Platform (UWP) apps that support custom interactions from pen and stylus devices, including digital ink for natural writing and drawing experiences.
title: Interactions avec le stylet et WindowsInk dans les applications UWP
ms.assetid: 3DA4F2D2-5405-42A1-9ED9-3A87BCD84C43
label: Pen interactions and Windows Ink in UWP apps
template: detail.hbs
keywords: Windows Ink, entrée manuscrite Windows, DirectInk, InkPresenter, InkCanvas, reconnaissance de l’écriture manuscrite, interaction utilisateur, entrées
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 22477ab0facfcb67d44057a91c7c3a49df57f8b9
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/29/2018
ms.locfileid: "7988256"
---
# <a name="pen-interactions-and-windows-ink-in-uwp-apps"></a>Interactions avec le stylet et WindowsInk dans les applications UWP

![Stylet Surface](images/ink/hero-small.png)  
*Stylet Surface* (disponible à l’achat dans la [Boutique Microsoft](https://aka.ms/purchasesurfacepen)).

## <a name="overview"></a>Vue d’ensemble

Optimisez votre application de plateforme Windows universelle (UWP) pour la saisie au stylet, afin de fournir une fonctionnalité standard d’[**appareil de pointage**](https://msdn.microsoft.com/library/windows/apps/br225633) et d’offrir une expérience WindowsInk optimale à vos utilisateurs.

> [!NOTE]
> Cette rubrique est dédiée à la plateforme WindowsInk. Pour en savoir plus sur la gestion des entrées au pointeur (similaires aux fonctionnalités tactiles, de la souris et du pavé tactile), consultez [Gérer les entrées du pointeur](handle-pointer-input.md).

| Vidéos |   |
| --- | --- |
| <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Ink-in-Your-UWP-App/player" width="300" height="200" allowFullScreen frameBorder="0"></iframe> | <iframe src="https://channel9.msdn.com/Events/Ignite/2016/BRK2060/player" width="300" height="200" allowFullScreen frameBorder="0"></iframe> |
| *Utilisation d’entrée manuscrite dans votre application UWP* | *Permet de créer plus conviviale enterpriseapps stylet et Windows Ink* |

La plateforme WindowsInk, associée à un stylet, permet de créer des notes manuscrites, des dessins et des annotations plus naturellement. La plateforme prend en charge la capture d’entrée du numériseur sous forme de données d’entrée manuscrite, la génération et la gestion de données d’entrée manuscrite, la restitution de ces données sous forme de traits et la conversion de l’encre en texte via la reconnaissance d’écriture manuscrite.

En plus de capturer la position et les mouvements de base du stylet lorsque l’utilisateur écrit ou dessine, votre application peut également effectuer le suivi des niveaux de pression variables utilisés pour un trait et en effectuer le suivi. Ces informations, ainsi que les paramètres relatifs à la forme de la pointe, à sa taille et à sa rotation, à la couleur de l’encre et à l’utilisation (entrée manuscrite normale, effacement, surlignage et sélection), vous permettent de proposer des expériences utilisateur ressemblant étroitement à l’écriture ou au dessin sur papier à l’aide d’un stylo, d’un crayon ou d’un pinceau.

> [!NOTE]
> Votre application prend également en charge l’entrée manuscrite à partir d’autres appareils de pointage, comme les numériseurs tactiles et les souris. 

La plateforme d’entrée manuscrite est très flexible. Elle est conçue pour prendre en charge différents niveaux de fonctionnalité, en fonction de vos besoins.

Pour obtenir des recommandations en matière d’expérience utilisateur avec Windows Ink, consultez [Contrôles pour l’entrée manuscrite](../controls-and-patterns/inking-controls.md).

## <a name="components-of-the-windows-ink-platform"></a>Composants de la plateforme Windows Ink

| Composant | Description |
| --- | --- |
| [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) | Un contrôle de plateforme XAMLUI qui, par défaut, reçoit et affiche toutes les entrées à partir d’un stylet comme un trait d’encre ou un trait d’effacement.<br/>Pour plus d’informations sur l’utilisation de l’élément InkCanvas, consultez [Reconnaître les traits d’encre Windows en tant que texte](convert-ink-to-text.md) et [Stocker et récupérer les données de traits Windows Ink](save-and-load-ink.md). |
| [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) | Un objet code-behind, instancié avec un contrôle [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) (exposé par le biais de la propriété [**InkCanvas.InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081)). Cet objet fournit toutes les fonctionnalités d’entrée manuscrite par défaut exposées par l’élément **InkCanvas**, ainsi qu’un ensemble complet d’API pour plus de personnalisation.<br/>Pour plus d’informations sur l’utilisation de l’élément InkPresenter, consultez [Reconnaître les traits d’encre Windows en tant que texte](convert-ink-to-text.md) et [Stocker et récupérer les données de traits Windows Ink](save-and-load-ink.md). |
| [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) | Un contrôle de plateforme XAMLUI contenant une collection extensible et personnalisable de boutons qui activent des fonctionnalités d’entrée manuscrite dans un [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas)associé.<br/>Pour plus d’informations sur l’utilisation de l’élément InkToolbar, consultez [Ajouter un élément InkToolbar à une application d’entrée manuscrite de plateforme Windows universelle (UWP)](ink-toolbar.md). |
| [**IInkD2DRenderer**](https://msdn.microsoft.com/library/mt147263) | Permet le rendu des traits d’encre sur le contexte d’appareil Direct2D désigné d’une application Windows universelle, au lieu du contrôle [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) par défaut. Cela offre une personnalisation totale de l’expérience d’entrée manuscrite.<br/>Pour plus d’informations, consultez [l’exemple d’entrée manuscrite complexe](https://go.microsoft.com/fwlink/p/?LinkID=620314). |

## <a name="basic-inking-with-inkcanvas"></a>Entrée manuscrite de base avec InkCanvas

Pour ajouter des fonctionnalités d’écriture manuscrite de base, il suffit de placer une commande de plateforme UWP [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) sur la page appropriée dans votre application.

Par défaut, l’élément [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) prend en charge l’entrée manuscrite uniquement à partir d’un stylet. L’entrée est restituée sous la forme d’un trait d’encre à l’aide des paramètres par défaut pour la couleur et l’épaisseur (un stylo à bille noir avec une épaisseur de 2 pixels), ou traitée sous forme d’un effaceur de trait (lorsque l’entrée manuscrite est effectuée à partir d’une pointe d’effaceur ou que la pointe du stylet est modifiée à l’aide d’un bouton d’effacement).

> [!NOTE]
> Si une pointe d’effaceur ou le bouton n’est pas présent, l’élément InkCanvas peut être configuré pour traiter les entrées à partir de la pointe du stylet comme un trait d’effacement.

Dans cet exemple, un élément [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) recouvre une image d’arrière-plan.

> [!NOTE]
> Un InkCanvas a des propriétés par défaut [**Hauteur**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.Height) et [**Largeur**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.Width) égales à zéro, sauf s’il s’agit de l’enfant d’un élément qui dimensionne automatiquement ses éléments enfants. 

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

> [!NOTE]
> L’élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) ne peut pas être instancié directement. Au lieu de cela, il est accessible via la propriété [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) de l’élément [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535). 

En plus de fournir tous les comportements d’entrée manuscrite par défaut de son contrôle [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) correspondant, l’élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) propose un ensemble complet d’API pour personnaliser davantage les traits et gérer de manière plus fine la saisie effectuée à l’aide du stylet (standard et modifiée). Cela inclut les propriétés de trait, les types d’appareils d’entrée pris en charge, et si l’entrée est traitée par l’objet ou transmise à l’application pour traitement.

> [!NOTE]
> L’entrée manuscrite standard (mine du stylet ou mine/bouton de la gomme) n’est pas modifiée avec une affordance du matériel secondaire, tel qu’un bouton gomme du stylet, le bouton droit de la souris ou un mécanisme similaire. 

Par défaut, l’entrée manuscrite est prise en charge uniquement pour la saisie effectuée à l’aide du stylet. Ici, l’élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) est configuré pour interpréter les données d’entrée de stylet et de souris sous forme de traits d’encre. Nous définissons également des attributs de trait d’encre initiaux utilisés pour restituer les traits dans l’élément [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535).

Pour activer la souris et l’entrée manuscrite tactile, définissez la propriété [**InputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.InputDeviceTypes) de [**InkPresenter**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.inkpresenter) sur la combinaison de valeurs [**CoreInputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.core.coreinputdevicetypes) de votre choix.

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

Par défaut, [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) traite toutes les entrées sous forme de trait d’encre ou de trait d’effacement, y compris les entrées modifiées par une affordance de matériel secondaire comme un bouton de stylet, un bouton droit de souris, ou un élément similaire. Toutefois, les utilisateurs attendent généralement des fonctionnalités supplémentaires ou une modification de comportement avec ces affordances secondaires.

Dans certains cas, vous devrez peut-être également exposer des fonctionnalités supplémentaires pour les crayons sans affordance secondaire (fonctionnalité qui n’est généralement pas associée à la pointe du stylet), les autres types de périphériques d’entrée ou un type de comportement modifié en fonction de la sélection d’un utilisateur dans l’interface utilisateur de votre application.

Pour prendre cela en charge, [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) peut être configuré pour ne pas traiter l’entrée spécifique. Cette entrée non traitée est ensuite directement transmise à votre application pour traitement.

### <a name="example---use-unprocessed-input-to-implement-stroke-selection"></a>Exemple - Utilisation d’une entrée non traitée pour implémenter la sélection de traits 

La plateforme WindowsInk ne fournit pas de prise en charge intégrée pour les actions qui nécessitent une entrée modifiée, comme une sélection de traits. Pour prendre en charge de telles fonctionnalités, vous devez fournir une solution personnalisée dans vos applications. 

L’exemple de code suivant (tout le code se trouve dans les fichiers MainPage.xaml et MainPage.xaml.cs) décrit comment activer la sélection de traits lorsqu’une entrée est modifiée à l’aide d’un bouton de stylet (ou le bouton droit de la souris).

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

    Plus important encore, nous utilisons la propriété [**InputProcessingConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn948764) de l’élément [InkPresenter](https://msdn.microsoft.com/library/windows/apps/dn899081) pour indiquer que toute entrée modifiée doit être traitée par l’application. La modification d’une entrée est spécifiée en attribuant à **InputProcessingConfiguration.RightDragAction** la valeur [**InkInputRightDragAction.LeaveUnprocessed**](https://msdn.microsoft.com/library/windows/apps/dn948760). Lorsque cette valeur est définie, le [InkPresenter](https://msdn.microsoft.com/library/windows/apps/dn899081) transmet à la classe [InkUnprocessedInput](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkunprocessedinput) un ensemble d’événements de pointeur que vous pouvez gérer.

    Nous affectons des écouteurs pour les événements [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/dn914712), [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/dn914711) et [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/dn914713) non traités transmis directement par [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081). Toutes les fonctionnalités de sélection sont implémentées dans les gestionnaires de ces événements.

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

Par défaut, l’entrée manuscrite est traitée sur un thread d’arrière-plan de faible latence et restituée en cours, ou «humide» comme elle est dessinée. Lorsque le trait est terminé (stylet ou doigt relevé, ou bouton de la souris relâché), le trait est traité sur le thread de l’interface utilisateur et restitué « sec » à la couche [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) (au-dessus du contenu de l’application et en remplaçant l’encre humide).

Vous pouvez remplacer ce comportement par défaut et contrôler entièrement l’expérience d’entrée manuscrite par un «séchage personnalisé» des traits d’encre humides. Alors que le comportement par défaut est généralement suffisant pour la plupart des applications, il existe quelques cas où le séchage personnalisé peut être requis, notamment dans les cas suivants:
- Gestion plus efficace de collections de traits d’encre volumineuses ou complexes
- Prise en charge plus efficace des panoramiques et des zooms sur les canevas d’encre de grande taille
- Entrelacement d’entrées manuscrites et d’autres objets, tels que des formes ou du texte, tout en conservant l’ordre de plan 
- Séchage et conversion synchrone des entrées manuscrites dans une forme DirectX (par exemple, une ligne droite ou une forme rastérisée et intégrée au contenu de l’application plutôt qu’en tant que couche **InkCanvas** distincte).

Le séchage personnalisé requiert un objet [**IInkD2DRenderer**](https://msdn.microsoft.com/library/mt147263) pour gérer l’entrée manuscrite et la restituer au contexte de périphérique Direct2D de votre application Windows universelle, au lieu du contrôle [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) par défaut.

En appelant [**ActivateCustomDrying**](https://msdn.microsoft.com/library/windows/apps/dn922012) (avant le chargement de [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)), une application crée un objet [**InkSynchronizer**](https://msdn.microsoft.com/library/windows/apps/dn903979) pour personnaliser la manière dont un trait d’encre est restitué sec à un élément [**SurfaceImageSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) ou [**VirtualSurfaceImageSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.virtualsurfaceimagesource). 

[**SurfaceImageSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) et [**VirtualSurfaceImageSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.virtualsurfaceimagesource) fournissent une surface partagée DirectX pour votre application dans laquelle il est possible de dessiner et de composer dans le contenu de votre application, même si les instances de serveur virtuel fournissent une surface virtuelle plus grande que l’écran pour un panoramique et un zoom performants. Étant donné que les mises à jour visuelles de ces surfaces sont synchronisées avec le thread d’interface utilisateur XAML, lorsque l’entrée manuscrite est restituée, l’encre humide peut être supprimée du InkCanvas simultanément. 

Vous pouvez également personnaliser l’encre sèche pour un [SwapChainPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.swapchainpanel), mais la synchronisation avec le thread d’interface utilisateur n’est pas garantie et il peut y avoir un délai entre le moment où l’encre est restituée dans votre SwapChainPanel et celui où l’entre est supprimée du InkCanvas.

Pour obtenir un exemple complet de cette fonctionnalité, consultez l’[exemple d’entrée manuscrite complexe](https://go.microsoft.com/fwlink/p/?LinkID=620314).

> [!NOTE]
> Séchage personnalisé et élément [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)  
> Si votre application remplace le comportement par défaut du rendu d’entrée manuscrite de l’élément [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) par une implémentation de séchage personnalisé, les traits d’encre restitués ne sont plus disponibles pour l’élément InkToolbar et les commandes d’effacement intégrées de l’élément InkToolbar ne fonctionneront pas comme prévu. Pour fournir des fonctionnalités d’effacement, vous devez gérer tous les événements de pointeur, effectuer le test de positionnement sur chaque trait et remplacer la commande intégrée «Effacer toutes les entrées manuscrites».

## <a name="other-articles-in-this-section"></a>Autres articles de cette section

| Rubrique | Description |
| --- | --- |
| [Reconnaître les traits d’encre](convert-ink-to-text.md) | Convertissez des traits d’encre en texte à l’aide de la reconnaissance de l’écriture manuscrite ou en formes à l’aide de la reconnaissance personnalisée. |
| [Stocker et récupérer des traits d’encre](save-and-load-ink.md) | Stockez des données de traits d’encre dans un fichier GIF (Graphics Interchange Format) à l’aide des métadonnées intégrées ISF (Ink Serialized Format). |
| [Ajouter un élément InkToolbar à une application d’entrée manuscrite de plateforme Windows universelle (UWP)](ink-toolbar.md) | Ajoutez un élément InkToolbar par défaut à une application d’entrée manuscrite de plateforme Windows universelle (UWP), ajoutez un bouton de stylet personnalisé à l’élément InkToolbar et liez le bouton de stylet personnalisé à une définition de stylet personnalisé. |

## <a name="related-articles"></a>Articles connexes

* [Prise en main: Prise en charge des entrées manuscrites dans votre application UWP](../../get-started/ink-walkthrough.md)
* [Gérer les entrées du pointeur](handle-pointer-input.md)
* [Identifier des périphériques d’entrée](identify-input-devices.md)

**API**

* [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)
* [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)
* [**Windows.UI.Input.Inking.Core**](https://msdn.microsoft.com/library/windows/apps/dn958452)

**Exemples**
* [Didacticiel de prise en main: Prise en charge des entrées manuscrites dans votre application UWP](https://aka.ms/appsample-ink)
* [Exemple d’entrée manuscrite simple (C#/C++)](https://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Exemple d’entrée manuscrite complexe (C++)](https://go.microsoft.com/fwlink/p/?LinkID=620314)
* [Exemple d’entrée manuscrite (JavaScript)](https://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Exemple de livre de coloriage](https://aka.ms/cpubsample-coloringbook)
* [Exemple de notes de famille](https://aka.ms/cpubsample-familynotessample)
* [Exemple d’entrée de base](https://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Exemple d’entrée à faible latence](https://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Exemple de mode d’interaction utilisateur](https://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Exemple de visuels de focus](https://go.microsoft.com/fwlink/p/?LinkID=619895)

**Exemples d’archive**
* [Entrée : exemple de fonctionnalités de périphériques](https://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrée : exemple d’événements d’entrée utilisateur XAML](https://go.microsoft.com/fwlink/p/?linkid=226855)
* [Exemple de zoom, de panoramique et de défilement XAML](https://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrée : mouvements et manipulations avec GestureRecognizer](https://go.microsoft.com/fwlink/p/?LinkID=231605)
