---
author: Karl-Bridge-Microsoft
Description: Build Universal Windows Platform (UWP) apps that support custom interactions from pen and stylus devices, including digital ink for natural writing and drawing experiences.
title: Pen and stylus interactions in UWP apps
ms.assetid: 3DA4F2D2-5405-42A1-9ED9-3A87BCD84C43
label: Pen and stylus interactions in UWP apps
template: detail.hbs
keyword: Windows Ink, Windows Inking, DirectInk, InkPresenter, InkCanvas
---

# Pen and stylus interactions in UWP apps

Optimize your Universal Windows Platform (UWP) app for pen input to provide both standard [**pointer device**](https://msdn.microsoft.com/library/windows/apps/br225633) functionality and the best Windows Ink experience for your users.

> Note: This topic focuses on the Windows Ink platform. See [Handle pointer input](handle-pointer-input.md) for general pointer input handling (similar to mouse, touch, and touchpad).

![touchpad](images/input-patterns/input-pen.jpg)

**Important APIs**

-   [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)
-   [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)
-   [**Windows.UI.Input.Inking.Core**](https://msdn.microsoft.com/library/windows/apps/dn958452)

The Windows Ink platform, together with a pen device, provides a natural way to create digital handwritten notes, drawings, and annotations. The platform supports capturing digitizer input as ink data, generating ink data, managing ink data, rendering ink data as ink strokes on the output device, and converting ink to text through handwriting recognition.

In addition to capturing the basic position and movement of the pen as the user writes or draws, your app can also track and collect the varying amounts of pressure used throughout a stroke. This information, along with settings for pen tip shape, size, and rotation, ink color, and purpose (plain ink, erasing, highlighting, and selecting), enables you to provide user experiences that closely resemble writing or drawing on paper with a pen, pencil, or brush.

**Note**  Your app can also support ink input from other pointer-based devices, including touch digitizers and mouse devices. 

The ink platform is very flexible. It is designed to support various levels of functionality, depending on your requirements.

There are three components to the ink platform:

-   [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) - A XAML UI platform control that, by default, receives and displays all input from a pen as either an ink stroke or an erase stroke.

-   [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) - A code-behind object, instantiated along with an [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) control (exposed through the [**InkCanvas.InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) property). This object provides all default inking functionality exposed by the **InkCanvas**, along with a comprehensive set of APIs for additional customization and personalization.

-   [**IInkD2DRenderer**](https://msdn.microsoft.com/library/mt147263) - Enables the rendering of ink strokes onto the designated Direct2D device context of a Universal Windows app, instead of the default [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) control. This enables full customization of the inking experience.

## <span id="inkcanvas"></span><span id="INKCANVAS"></span>Basic inking with InkCanvas


For basic inking functionality, just place an [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) anywhere on a page.

The [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) supports ink input only from a pen. The input is either rendered as an ink stroke using default settings for color and thickness, or treated as a stroke eraser (when input is from an eraser tip or the pen tip modified with an erase button).

In this example, an [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) overlays a background image.

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

This series of images shows how pen input is rendered by this [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) control.

| ![The blank InkCanvas with a background image](images/ink_basic_1_small.png) | ![The InkCanvas with ink strokes](images/ink_basic_2_small.png) | ![The InkCanvas with one stroke erased](images/ink_basic_3_small.png) |
| --- | --- | ---|
| The blank [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) with a background image. | The [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) with ink strokes. | The [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) with one stroke erased (note how erase operates on an entire stroke, not a portion). |

The inking functionality supported by the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) control is provided by a code-behind object called the [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011).

For basic inking, you don't have to concern yourself with the [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011). However, to customize and configure inking behavior on the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535), you must access its corresponding **InkPresenter** object.

## <span id="inkpresenter"></span><span id="INKPRESENTER"></span>Basic customization with InkPresenter


An [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) object is instantiated with each [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) control.

Along with providing all default inking behaviors of its corresponding [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) control, the [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) provides a comprehensive set of APIs for additional stroke customization. This includes stroke properties, supported input device types, and whether input is processed by the object or passed to the app.

**Note**  
The [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) cannot be instantiated directly. Instead, it is accessed through the [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) property of the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535).

 

Here, we configure the [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) to interpret input data from both pen and mouse as ink strokes. We also set some initial ink stroke attributes used for rendering strokes to the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535).

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

Ink stroke attributes can be set dynamically to accommodate user preferences or app requirements.

Here, we let a user choose from a list of ink colors.

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

We then handle changes to the selected color and update the ink stroke attributes accordingly.

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

These images shows how pen input is processed and customized by the [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081).

| ![the inkcanvas with default black ink strokes](images/ink-basic-custom-1-small.png) | ![the inkcanvas with user selected red ink strokes](images/ink-basic-custom-2-small.png) |
| --- | -- |
| The [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) with default black ink strokes. | The [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) with user selected red ink strokes. |

 

To provide functionality beyond inking and erasing, such as stroke selection, your app must identify specific input for the [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) to pass through unprocessed for handling by your app.

## <span id="passthrough"></span><span id="PASSTHROUGH"></span>Pass-through input for advanced processing


By default, [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) processes all input as either an ink stroke or an erase stroke. This includes input modified by a secondary hardware affordance such as a pen barrel button, a right mouse button, or similar.

When using these secondary affordances, users typically expect some additional functionality or a modified behavior.

In some cases, you might need to expose basic ink functionality for pens without secondary affordances (functionality not usually associated with the pen tip), other input device types, or additional functionality, or modified behavior, based on a user selection in your app's UI.

To support this, [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) can be configured to leave specific input unprocessed. This unprocessed input is then passed through to your app for processing.

The following code example steps through how to enable stroke selection when input is modified with a pen barrel button (or right mouse button).

For this example, we use the MainPage.xaml and MainPage.xaml.cs files to host all code.

1.  First, we set up the UI in MainPage.xaml.

    Here, we add a canvas (below the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)) to draw the selection stroke. Using a separate layer to draw the selection stroke leaves the **InkCanvas** and its content untouched.

    ![the blank inkcanvas with an underlying selection canvas](images/ink-unprocessed-1-small.png)
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

2.  In MainPage.xaml.cs, we declare a couple of global variables for keeping references to aspects of the selection UI. Specifically, the selection lasso stroke and the bounding rectangle that highlights the selected strokes.
```    CSharp
// Stroke selection tool.
    private Polyline lasso;
    // Stroke selection area.
    private Rect boundingRect;
```

3.  Next, we configure the [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) to interpret input data from both pen and mouse as ink strokes, and set some initial ink stroke attributes used for rendering strokes to the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535).

    Most importantly, we use the [**InputProcessingConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn948764) property of the [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) to indicate that any modified input should be processed by the app. Modified input is specified by assigning **InputProcessingConfiguration.RightDragAction** a value of [**InkInputRightDragAction.LeaveUnprocessed**](https://msdn.microsoft.com/library/windows/apps/dn948760).

    We then assign listeners for the unprocessed [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/dn914712), [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/dn914711), and [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/dn914713) events passed through by the [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081). All selection functionality is implemented in the handlers for these events.

    Finally, we assign listeners for the [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702) and [**StrokesErased**](https://msdn.microsoft.com/library/windows/apps/dn948767) events of the [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081). We use the handlers for these events to clean up the selection UI if a new stroke is started or an existing stroke is erased.

    ![the inkcanvas with default black ink strokes](images/ink-unprocessed-2-small.png)
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

4.  We then define handlers for the unprocessed [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/dn914712), [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/dn914711), and [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/dn914713) events passed through by the [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081).

    All selection functionality is implemented in these handlers, including the lasso stroke and the bounding rectangle.

    ![the selection lasso](images/ink-unprocessed-3-small.png)
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

5.  To conclude the PointerReleased event handler, we clear the selection layer of all content (the lasso stroke) and then draw a single bounding rectangle around the ink strokes encompassed by the lasso area.

    ![the selection bounding rect](images/ink-unprocessed-4-small.png)
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

6.  Finally, we define handlers for the [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702) and [**StrokesErased**](https://msdn.microsoft.com/library/windows/apps/dn948767) InkPresenter events.

    These both just call the same cleanup function to clear the current selection whenever a new stroke is detected.
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

7.  Here's the function to remove all selection UI from the selection canvas when a new stroke is started or an existing stroke is erased.
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

## <span id="iinkd2drenderer"></span><span id="IINKD2DRENDERER"></span>Custom ink rendering


By default, ink input is processed on a low-latency background thread and rendered "wet" as it is drawn. When the stroke is completed (pen or finger lifted, or mouse button released), the stroke is processed on the UI thread and rendered "dry" to the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) layer (above the application content and replacing the wet ink).

The ink platform enables you to override this behavior and completely customize the inking experience by custom drying the ink input.

Custom drying requires an [**IInkD2DRenderer**](https://msdn.microsoft.com/library/mt147263) object to manage the ink input and render it to the Direct2D device context of your Universal Windows app, instead of the default [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) control.

By calling [**ActivateCustomDrying**](https://msdn.microsoft.com/library/windows/apps/dn922012) (before the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) is loaded), an app creates an [**InkSynchronizer**](https://msdn.microsoft.com/library/windows/apps/dn903979) object to customize how an ink stroke is rendered dry to a [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) or [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050). For example, an ink stroke could be rasterized and integrated into application content instead of as a separate **InkCanvas** layer.

For a full example of this functionality, see the [Complex ink sample](http://go.microsoft.com/fwlink/p/?LinkID=620314) .


## Other articles in this section 
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Topic</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Recognize ink strokes](convert-ink-to-text.md)</p></td>
<td align="left"><p>Convert ink strokes to text using handwriting recognition, or to shapes using custom recognition.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Store and retrieve ink strokes](save-and-load-ink.md)</p></td>
<td align="left"><p>Store ink stroke data in a Graphics Interchange Format (GIF) file using embedded Ink Serialized Format (ISF) metadata.</p></td>
</tr>
</tbody>
</table>

 


## <span id="related_topics"></span>Related articles


* [Handle pointer input](handle-pointer-input.md)
* [Identify input devices](identify-input-devices.md)

**Samples**
* [Ink sample](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Simple ink sample](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Complex ink sample](http://go.microsoft.com/fwlink/p/?LinkID=620314)
* [Basic input sample](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Low latency input sample](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [User interaction mode sample](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Focus visuals sample](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**Archive Samples**
* [Input: Device capabilities sample](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Input: XAML user input events sample](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [XAML scrolling, panning, and zooming sample](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Input: Gestures and manipulations with GestureRecognizer](http://go.microsoft.com/fwlink/p/?LinkID=231605)
 

 




