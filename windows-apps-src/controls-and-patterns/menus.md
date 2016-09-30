---
author: mijacobs
Description: A flyout is a lightweight popup that is used to temporarily show UI that is related to what the user is currently doing.
title: Menus and context menus
label: Menus and context menus
template: detail.hbs
---
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 
# Menus and context menus

Menus and context menus display a list of commands or options when the user requests them.

![Example of a typical context menu](images/controls_contextmenu_singlepane.png)

<div class="important-apis" >
<b>Important APIs</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/dn299030">MenuFlyout class</a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx">ContextFlyout property</a></li>
<li><a href="https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx">FlyoutBase.AttachedFlyout property</a></li>
</ul>

</div>
</div>




## Is this the right control?
Menus and context menus save space by organizing commands and hiding them until the user needs them. If a particular command will be used frequently and you have the space available, consider placing it directly in its own element, rather than in a menu, so that users don't have to go through a menu to get to it. 

Menus and context menus are for organizing commands; to display arbitrary content, such as an notification or to request confirmation, use a [dialog or a flyout](dialogs.md).  


## Menus vs. context menus

Menus and context menus are identical in how they look and what they can contain. In fact, you use the same control, [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030), to create them. The only difference is how you let the user access it. 

When should you use a menu or a context menu?
* If the host element is a button or some other command element whose primary role is to present additional commands, use a menu.
* If the host element is some other type of element that has another primary purpose (such as presenting text or an image), use a context menu. 

For example, use a menu on a button in a navigation pane to provide additional navigation options. In this scenario, the primary purpose of the button control is to provide access to a menu. 

If you want to add commands (such as cut, copy, and paste) to a text element, use a context menu instead of a menu. In this scenario, the primary role of the text element is to present and edit text; additional commands (such as cut, copy, and paste) are secondary and belong in a context menu. 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>Menus</b></p>
<p>
<ul>
<li>Have a single entry point (a File menu at the top of the screen, for example) that is always displayed.</li>
<li>Are usually attached to a button or a parent menu item.</li>
<li>Are invoked by left-clicking (or an equivalent action, such as tapping with your finger).</li>
<li>Are associated with an element via its [Flyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx) or [FlyoutBase.AttachedFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx) properties.</li>
</ul>
</p>

  </div>
  <div class="side-by-side-content-right">
   <p><b>Context menus</b></p>
   
<ul>
<li>Are attched to a single element, but are only accessible when the context makes sense.</li>
<li>Are invoked by right clicking (or an equivalent action, such as pressing and holding with your finger).</li>
<li>Are associated with an element via its [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) property.</li>
</ul>
  </div>
</div>
</div>

## Create a menu or a context menu

To create a menu or a context menu, you use the [MenuFlyout class](https://msdn.microsoft.com/library/windows/apps/dn299030). You define the contents of the menu by adding [MenuFlyoutItem](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx), [ToggleMenuFlyoutItem](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx), and [MenuFlyoutSeparator](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx) objects to the MenuFlyout. These objects are for:
* [MenuFlyoutItem](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx)—Performing an immediate action.
* [ToggleMenuFlyoutItem](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx)—Switching an option on or off.
* [MenuFlyoutSeparator](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx)—Visually separating menu items.


This example creates a [MenuFlyout class](https://msdn.microsoft.com/library/windows/apps/dn299030) and uses the [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) property, a property available to most controls, to show the [MenuFlyout class](https://msdn.microsoft.com/library/windows/apps/dn299030) as a context menu.

````xaml
<Rectangle 
  Height="100" Width="100" 
  Tapped="Rectangle_Tapped">
  <Rectangle.ContextFlyout>
    <MenuFlyout>
      <MenuFlyoutItem Text="Change color" Click="ChangeColorItem_Click" />
    </MenuFlyout>
  </Rectangle.ContextFlyout>
  <Rectangle.Fill>
    <SolidColorBrush x:Name="rectangleFill" Color="Red" />
  </Rectangle.Fill>
</Rectangle>
````

````csharp
private void ChangeColorItem_Click(object sender, RoutedEventArgs e)
{
    // Change the color from red to blue or blue to red. 
    if (rectangleFill.Color == Windows.UI.Colors.Red)
    {
        rectangleFill.Color = Windows.UI.Colors.Blue;
    }
    else
    {
        rectangleFill.Color = Windows.UI.Colors.Red;
    }
}
````

The next example is nearly identical, but instead of using the [ContextFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) property to show the [MenuFlyout class](https://msdn.microsoft.com/library/windows/apps/dn299030) as a context menu, the example uses the [FlyoutBase.ShowAttachedFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout) property to show it as a menu. 

````xaml
<Rectangle 
  Height="100" Width="100" 
  Tapped="Rectangle_Tapped">
  <FlyoutBase.AttachedFlyout>
    <MenuFlyout>
      <MenuFlyoutItem Text="Change color" Click="ChangeColorItem_Click" />
    </MenuFlyout>
  </FlyoutBase.AttachedFlyout>
  <Rectangle.Fill>
    <SolidColorBrush x:Name="rectangleFill" Color="Red" />
  </Rectangle.Fill>
</Rectangle>
````

````csharp
private void Rectangle_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);
}

private void ChangeColorItem_Click(object sender, RoutedEventArgs e)
{
    // Change the color from red to blue or blue to red. 
    if (rectangleFill.Color == Windows.UI.Colors.Red)
    {
        rectangleFill.Color = Windows.UI.Colors.Blue;
    }
    else
    {
        rectangleFill.Color = Windows.UI.Colors.Red;
    }
}
````

> **Note**&nbsp;&nbsp;Light dismiss controls&mdash;such as menus, context menus, and other flyouts&mdash;trap keyboard and gamepad focus inside the transient UI until dismissed. To provide a visual cue for this behavior, light dismiss controls on Xbox will draw an overlay that dims the visibility of out of scope UI. This behavior can be modified with the new [LightDismissOverlayMode](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode.aspx) property. By default, transient UIs will draw the light dismiss overlay on Xbox but not other device families, but apps can choose to force the overlay to be always **On** or always **Off**.
> 
> ```xaml
> <MenuFlyout LightDismissOverlayMode="Off">
```

## Get the samples
*   [XAML UI basics](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)<br/>
    See all of the XAML controls in an interactive format.

## Related articles

- [**MenuFlyout class**](https://msdn.microsoft.com/library/windows/apps/dn299030)
