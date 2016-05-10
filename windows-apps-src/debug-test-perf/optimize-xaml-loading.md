---
author: mcleblanc
ms.assetid: 569E8C27-FA01-41D8-80B9-1E3E637D5B99
title: Optimize your XAML markup
description: Parsing XAML markup to construct objects in memory is time-consuming for a complex UI. Here are some things you can do to improve XAML markup parse and load time and memory efficiency for your app.
---
# Optimize your XAML markup

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Parsing XAML markup to construct objects in memory is time-consuming for a complex UI. Here are some things you can do to improve XAML markup parse and load time and memory efficiency for your app.

At app startup, limit the XAML markup that is loaded to only what you need for your initial UI. Examine the markup in your initial page and confirm it contains nothing that it doesn't need. If a page references a user control or a resource defined in a different file, then the framework parses that file, too.

In this example, because InitialPage.xaml uses one resource from ExampleResourceDictionary.xaml, the whole of ExampleResourceDictionary.xaml must be parsed at startup.

**InitialPage.xaml.**

```xml
<Page x:Class="ExampleNamespace.InitialPage" ...> 
    <UserControl.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="ExampleResourceDictionary.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </UserControl.Resources>

    <Grid>
        <TextBox Foreground="{StaticResource TextBrush}"/>
    </Grid>
</Page>
```

**ExampleResourceDictionary.xaml.**

```xml
<ResourceDictionary>
    <SolidColorBrush x:Key="TextBrush" Color="#FF3F42CC"/>

    <!--This ResourceDictionary contains many other resources that
    used in the app, but not during startup.-->
</ResourceDictionary>
```

If you use a resource on many pages throughout your app, then storing it in App.xaml is a good practice, and avoids duplication. But App.xaml is parsed at app startup so any resource that is used in only one page (unless that page is the initial page) should be put into the page's local resources. This counter-example shows App.xaml containing resources that are used by only one page (that's not the initial page). This needlessly increases app startup time.

**InitialPage.xaml.**

```xml
<Page ...>  <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->   
    <StackPanel>  
        <TextBox Foreground="{StaticResource InitialPageTextBrush}"/> 
    </StackPanel> 
</Page> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**SecondPage.xaml.**

```xml
<Page ...>  <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
    <StackPanel> 
        <Button Content="Submit" Foreground="{StaticResource SecondPageTextBrush}" /> 
    </StackPanel> 
</Page> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**App.xaml**

```xml
<Application ...> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
     <Application.Resources>  
        <SolidColorBrush x:Key="DefaultAppTextBrush" Color="#FF3F42CC"/> 
        <SolidColorBrush x:Key="InitialPageTextBrush" Color="#FF3F42CC"/> 
        <SolidColorBrush x:Key="SecondPageTextBrush" Color="#FF3F42CC"/> 
        <SolidColorBrush x:Key="ThirdPageTextBrush" Color="#FF3F42CC"/> 
    </Application.Resources> 
</Application> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

The way to make the above counter-example more efficient is to move `SecondPageTextBrush` into SecondPage.xaml and to move `ThirdPageTextBrush` into ThirdPage.xaml. `InitialPageTextBrush` can remain in App.xaml because application resources must be parsed at app startup in any case.

## Minimize element count

Although the XAML platform is capable of displaying large numbers of elements, you can make your app lay out and render faster by using the fewest number of elements to achieve the visuals you want.

-   Layout panels have a [**Background**](https://msdn.microsoft.com/library/windows/apps/BR227512) property so there's no need to put a [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) in front of a Panel just to color it.

**Inefficient.**

```xml
<Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
        <Rectangle Fill="Black"/> 
    </Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**Efficient.**

```xml
<Grid Background="Black"/>
```

-   If you reuse the same vector-based element enough times, it becomes more efficient to use an [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) element instead. Vector-based elements can be more expensive because the CPU must create each individual element separately. The image file needs to be decoded only once.

## Consolidate multiple brushes that look the same into one resource

The XAML platform tries to cache commonly-used objects so that they can be reused as often as possible. But XAML cannot easily tell if a brush declared in one piece of markup is the same as a brush declared in another. The example here uses [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) to demonstrate, but the case is more likely and more important with [**GradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210068).

**Inefficient.**

```xml
<Page ... > <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
    <StackPanel>  
        <TextBlock>  
            <TextBlock.Foreground>  
                <SolidColorBrush Color="#FFFFA500"/> 
            </TextBlock.Foreground> 
        </TextBox> 
        <Button Content="Submit"> 
            <Button.Foreground> 
                <SolidColorBrush Color="#FFFFA500"/> 
            </Button.Foreground> 
        </Button> 
    </StackPanel> 
</Page> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

Also check for brushes that use predefined colors: `"Orange"` and `"#FFFFA500"` are the same color. To fix the duplication, define the brush as a resource. If controls in other pages use the same brush, move it to App.xaml.

**Efficient.**

```xml
<Page ... >
    <Page.Resources>
        <SolidColorBrush x:Key="BrandBrush" Color="#FFFFA500"/>
    </Page.Resources>
    
    <StackPanel>
        <TextBlock Foreground="{StaticResource BrandBrush}" />
        <Button Content="Submit" Foreground="{StaticResource BrandBrush}" />
    </StackPanel>
</Page>
```

## Minimize overdrawing

Overdrawing is where more than one object is drawn in the same screen pixels. Note that there is sometimes a trade-off between this guidance and the desire to minimize element count.

-   If an element isn't visible because it's transparent or hidden behind other elements, and it's not contributing to layout, then delete it. If the element is not visible in the initial visual state but it is visible in other visual states then set [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) to **Collapsed** on the element itself and change the value to **Visible** in the appropriate states. There will be exceptions to this heuristic: in general, the value a property has in the major of visual states is best set locally on the element.
-   Use a composite element instead of layering multiple elements to create an effect. In this example, the result is a two-toned shape where the top half is black (from the background of the [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)) and the bottom half is gray (from the semi-transparent white [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) alpha-blended over the black background of the **Grid**). Here, 150% of the pixels necessary to achieve the result are being filled.

**Inefficient.**
    
```xml
    <Grid Background="Black"> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
        <Grid.RowDefinitions> 
            <RowDefinition Height="*"/> 
            <RowDefinition Height="*"/> 
        </Grid.RowDefinitions> 
        <Rectangle Grid.Row="1" Fill="White" Opacity=".5"/> 
    </Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**Efficient.**

```xml
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <Rectangle Fill="Black"/>
        <Rectangle Grid.Row="1" Fill="#FF7F7F7F"/>
    </Grid>
```

-   A layout panel can have two purposes: to color an area, and to lay out child elements. If an element further back in z-order is already coloring an area then a layout panel in front does not need to paint that area: instead it can just focus on laying out its children. Here's an example.

**Inefficient.**

```xml
    <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
    <GridView Background="Blue">  
        <GridView.ItemTemplate> 
            <DataTemplate> 
                <Grid Background="Blue"/> 
            </DataTemplate> 
        </GridView.ItemTemplate> 
    </GridView> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**Efficient.**

```xml
    <GridView Background="Blue">  
        <GridView.ItemTemplate> 
            <DataTemplate> 
                <Grid/> 
            </DataTemplate>
        </GridView.ItemTemplate>
    </GridView> 
```

If the [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) has to be hit-testable then set a background value of transparent on it.

-   Use a [**Border**](https://msdn.microsoft.com/library/windows/apps/BR209253) element to draw a border around an object. In this example, a [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) is used as a makeshift border around a [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683). But all the pixels in the center cell are overdrawn.

**Inefficient.**

```xml
    <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
    <Grid Background="Blue" Width="300" Height="45"> 
        <Grid.RowDefinitions> 
            <RowDefinition Height="5"/> 
            <RowDefinition/> 
            <RowDefinition Height="5"/> 
        </Grid.RowDefinitions> 
        <Grid.ColumnDefinitions> 
            <ColumnDefinition Width="5"/> 
            <ColumnDefinition/> 
            <ColumnDefinition Width="5"/> 
        </Grid.ColumnDefinitions> 
        <TextBox Grid.Row="1" Grid.Column="1"></TextBox> 
    </Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**Efficient.**

```xml
    <Border BorderBrush="Blue" BorderThickness="5" Width="300" Height="45">
        <TextBox/>
    </Border>
```

-   Be aware of margins. Two neighboring elements will overlap (possibly accidentally) if negative margins extend into another’s render bounds and cause overdrawing.

Use [**DebugSettings.IsOverdrawHeatMapEnabled**](https://msdn.microsoft.com/library/windows/apps/Hh701823) as a visual diagnostic. You may find objects being drawn that you weren't aware were in the scene.

## Cache static content

Another source of overdrawing is a shape made from many overlapping elements. If you set [**CacheMode**](https://msdn.microsoft.com/library/windows/apps/BR228084) to **BitmapCache** on the [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) that contains the composite shape then the platform renders the element to a bitmap once and then uses that bitmap each frame instead of overdrawing.

**Inefficient.**

```xml
<Canvas Background="White">
    <Ellipse Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Left="21" Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Top="13" Canvas.Left="10" Height="40" Width="40" Fill="Blue"/>
</Canvas>
```

![Venn diagram with three solid circles](images/solidvenn.png)

The image above is the result, but here's a map of the overdrawn regions. Darker red indicates higher amounts of overdraw.

![Venn diagram that shows overlapping areas](images/translucentvenn.png)

**Efficient.**

```xml
<Canvas Background="White" CacheMode="BitmapCache">
    <Ellipse Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Left="21" Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Top="13" Canvas.Left="10" Height="40" Width="40" Fill="Blue"/>
</Canvas>
```

Note the use of [**CacheMode**](https://msdn.microsoft.com/library/windows/apps/BR228084). Don't use this technique if any of the sub-shapes animate because the bitmap cache will likely need to be regenerated every frame, defeating the purpose.

## ResourceDictionaries

ResourceDictionaries are generally used to store your resources at a somewhat global level. Resources that your app wants to reference in multiple places. For example, styles, brushes, templates, and so on. In general, we have optimized ResourceDictionaries to not instantiate resources unless they're asked for. But there are few places where you need to be a little careful.

**Resource with x:Name**. Any resource with x:Name will not benefit from the platform optimization, but instead it will be instantiated as soon as the ResourceDictionary is created. This happens because x:Name tells the platform that your app needs field access to this resource, so the platform needs to create something to create a reference to.

**ResourceDictionaries in a UserControl**. ResourceDictionaries defined inside of a UserControl carry a penalty. The platform will create a copy of such a ResourceDictionary for every instance of the UserControl. If you have a UserControl that is used a lot, then move the ResourceDictionary out of the UserControl and put it the page level.

## Use XBF2

XBF2 is a binary representation of XAML markup that avoids all text-parsing costs at runtime. It also optimizes your binary for load and tree creation, and allows "fast-path" for XAML types to improve heap and object creation costs, for example VSM, ResourceDictionary, Styles, and so on. It is completely memory-mapped so there is no heap footprint for loading and reading a XAML Page. In addition, it reduces the disk footprint of stored XAML pages in an appx. XBF2 is a more compact representation and it can reduce disk footprint of comparative XAML/XBF1 files by up to 50%. For example, the built-in Photos app saw around a 60% reduction after conversion to XBF2 dropping from around ~1mb of XBF1 assets to ~400kb of XBF2 assets. We have also seen apps benefit anywhere from 15 to 20% in CPU and 10 to 15% in Win32 heap.

XAML built-in controls and dictionaries that the framework provides are already fully XBF2-enabled. For your own app, ensure that your project file declares TargetPlatformVersion 8.2 or later.

To check whether you have XBF2, open your app in a binary editor; the 12th and 13th bytes are 00 02 if you have XBF2.

