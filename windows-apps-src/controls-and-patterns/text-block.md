---
author: Jwmsft
ms.assetid: DA562509-D893-425A-AAE6-B2AE9E9F8A19
label: Text block
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 94f858912473f2a0d20f4041155b1e1ee93032a2

---
# Text block

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

 Text block is the primary control for displaying read-only text in apps. You can use it to display single-line or multi-line text, inline hyperlinks, and text with formatting like bold, italic, or underlined.

<div class="important-apis" >
<b>Important APIs</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.aspx"><strong>TextBlock class</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx"><strong>Text property</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.inlines.aspx"><strong>Inlines property</strong></a></li>
</ul>

</div>
</div>



## Is this the right control?

A text block is typically easier to use and provides better text rendering performance than a rich text block, so it's preferred for most app UI text. You can easily access and use text from a text block in your app by getting the value of the [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx) property. It also provides many of the same formatting options for customizing how your text is rendered.

Although you can put line breaks in the text, text block is designed to display a single paragraph and doesn’t support text indentation. Use a **RichTextBlock** when you need support for multiple paragraphs, multi-column text or other complex text layouts, or inline UI elements like images.

For more info about choosing the right text control, see the [Text controls](text-controls.md) article.

## Examples


## Create a text block

Here's how to define a simple TextBlock control and set its Text property to a string.

```xaml
<TextBlock Text="Hello, world!" />
```

```csharp
TextBlock textBlock1 = new TextBlock();
textBlock1.Text = "Hello, world!";
```

    <TextBlock Text="Hello, world!" />

    TextBlock textBlock1 = new TextBlock();
    textBlock1.Text = "Hello, world!";

### Content model

There are two properties you can use to add content to a TextBlock: [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx) and [Inlines](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.inlines.aspx).

The most common way to display text is to set the Text property to a string value, as shown in the previous example.

You can also add content by placing inline flow content elements in the TextBox.Inlines property, like this.
```xaml
<TextBlock><Run>Text can be <Bold>bold</Bold>, <Italic>italic</Italic>, or <Bold><Italic>both</Italic></Bold>.</Run></TextBlock>
```

Elements derived from the Inline class, such as Bold, Italic, Run, Span, and LineBreak, enable different formatting for different parts of the text. For more info, see the [Formatting text]() section. The inline Hyperlink element lets you add a hyperlink to your text. However, using Inlines also disables fast path text rendering, which is discussed in the next section.


## Performance considerations

Whenever possible, XAML uses a more efficient code path to layout text. This fast path both decreases overall memory use and greatly reduces the CPU time to do text measuring and arranging. This fast path applies only to TextBlock, so it should be preferred when possible over RichTextBlock.

Certain conditions require TextBlock to fall back to a more feature-rich and CPU intensive code path for text rendering. To keep text rendering on the fast path, be sure to follow these guidelines when setting the properties listed here.
- [**Text**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx): The most important condition is that the fast path is used only when you set text by explicitly setting the Text property, either in XAML or in code (as shown in the previous examples). Setting the text via TextBlock’s Inlines collection (such as `<TextBlock>Inline text</TextBlock>`) will disable the fast path, due to the potential complexity of multiple formats.
- [**CharacterSpacing**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.characterspacing.aspx): Only the default value of 0 is fast path.
- [**TextTrimming**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.texttrimming.aspx): Only the **None**, **CharacterEllipsis**, and **WordEllipsis** values are fast path. The **Clip** value disables the fast path.

> **Note**&nbsp;&nbsp;Prior to Windows 10, version 1607, additional properties also affect the fast path. If your app is run on an earlier version of Windows, these conditions will also cause your text to render on the slow path. For more info about versions, see Version adaptive code.
- [**Typography**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx): Only the default values for the various Typography properties are fast path.
- [**LineStackingStrategy**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.linestackingstrategy.aspx): If [LineHeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.lineheight.aspx) is not 0, the **BaselineToBaseline** and **MaxHeight** values disable the fast path.
- [**IsTextSelectionEnabled**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.istextselectionenabled.aspx): Only **false** is fast path. Setting this property to **true** disables the fast path.

You can set the [DebugSettings.IsTextPerformanceVisualizationEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.debugsettings.istextperformancevisualizationenabled.aspx) property to **true** during debugging to determine whether text is using fast path rendering. When this property is set to true, the text that is on the fast path displays in a bright green color.

>**Tip**&nbsp;&nbsp;This feature is explained in depth in this session from Build 2015- [XAML Performance: Techniques for Maximizing Universal Windows App Experiences Built with XAML](https://channel9.msdn.com/Events/Build/2015/3-698).



You typically set debug settings in the [OnLaunched](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.application.onlaunched.aspx) method override in the code-behind page for App.xaml, like this.
```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
#if DEBUG
    if (System.Diagnostics.Debugger.IsAttached)
    {
        this.DebugSettings.IsTextPerformanceVisualizationEnabled = true;
    }
#endif

// ...

}
```

In this example, the first TextBlock is rendered using the fast path, while the second is not.
```xaml
<StackPanel>
    <TextBlock Text="This text is on the fast path."/>
    <TextBlock>This text is NOT on the fast path.</TextBlock>
<StackPanel/>
```

When you run this XAML in debug mode with IsTextPerformanceVisualizationEnabled set to true, the result looks like this.

![Text rendered in debug mode](images/text-block-rendering-performance.png)

>**Caution**&nbsp;&nbsp;The color of text that is not on the fast path is not changed. If you have text in your app with its color specified as bright green, it is still displayed in bright green when it's on the slower rendering path. Be careful to not confuse text that is set to green in the app with text that is on the fast path and green because of the debug settings.

## Formatting text

Although the Text property stores plain text, you can apply various formatting options to the TextBlock control to customize how the text is rendered in your app. You can set standard control properties like FontFamily, FontSize, FontStyle, Foreground, and CharacterSpacing to change the look of the text. You can also use inline text elements and Typography attached properties to format your text. These options affect only how the TextBlock displays the text locally, so if you copy and paste the text into a rich text control, for example, no formatting is applied.

>**Note**&nbsp;&nbsp;Remember, as noted in the previous section, inline text elements and non-default typography values are not rendered on the fast path.


### Inline elements

The [Windows.UI.Xaml.Documents](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.aspx) namespace provides a variety of inline text elements that you can use to format your text, such as Bold, Italic, Run, Span, and LineBreak.

You can display a series of strings in a TextBlock, where each string has different formatting. You can do this by using a Run element to display each string with its formatting and by separating each Run element with a LineBreak element.

Here's how to define several differently formatted text strings in a TextBlock by using Run objects separated with a LineBreak.
```xaml
<TextBlock FontFamily="Arial" Width="400" Text="Sample text formatting runs">
    <LineBreak/>
    <Run Foreground="Gray" FontFamily="Courier New" FontSize="24">
        Courier New 24
    </Run>
    <LineBreak/>
    <Run Foreground="Teal" FontFamily="Times New Roman" FontSize="18" FontStyle="Italic">
        Times New Roman Italic 18
    </Run>
    <LineBreak/>
    <Run Foreground="SteelBlue" FontFamily="Verdana" FontSize="14" FontWeight="Bold">
        Verdana Bold 14
    </Run>
</TextBlock>
```

Here's the result.

![Text formatted with run elements](images/text-block-run-examples.png)

### Typography

The attached properties of the [Typography](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx) class provide access to a set of Microsoft OpenType typography properties. You can set these attached properties either on the TextBlock, or on individual inline text elements. These examples show both.
```xaml
<TextBlock Text="Hello, world!"
           Typography.Capitals="SmallCaps"
           Typography.StylisticSet4="True"/>
```

```csharp
TextBlock textBlock1 = new TextBlock();
textBlock1.Text = "Hello, world!";
Windows.UI.Xaml.Documents.Typography.SetCapitals(textBlock1, FontCapitals.SmallCaps);
Windows.UI.Xaml.Documents.Typography.SetStylisticSet4(textBlock1, true);
```

```xaml
<TextBlock>12 x <Run Typography.Fraction="Slashed">1/3</Run> = 4.</TextBlock>
```


## Related articles

[Text controls](text-controls.md)

**For designers**
- [Guidelines for spell checking](spell-checking-and-prediction.md)
- [Adding search](search.md)
- [Guidelines for text input](text-controls.md)

**For developers (XAML)**
- [**TextBox class**](https://msdn.microsoft.com/library/windows/apps/br209683)
- [**Windows.UI.Xaml.Controls PasswordBox class**](https://msdn.microsoft.com/library/windows/apps/br227519)


**For developers (other)**
- [String.Length property](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)



<!--HONumber=Aug16_HO3-->


