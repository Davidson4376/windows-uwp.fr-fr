---
description: This article explains how to support copy and paste in Universal Windows Platform (UWP) apps using the clipboard.
title: Copy and paste
ms.assetid: E882DC15-E12D-4420-B49D-F495BB484BEE
---
#Copy and paste

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


This article explains how to support copy and paste in Universal Windows Platform (UWP) apps using the clipboard. Copy and paste is the classic way to exchange data either between apps, or within an app, and almost every app can support clipboard operations to some degree.

## Check for built-in clipboard support


In many cases, you do not need to write code to support clipboard operations. Many of the default XAML controls you can use to create apps already support clipboard operations. For more information about which controls are available, see the [controls list][ControlsList].

## Get set up

First, include the [**Windows.ApplicationModel.DataTransfer**][DataTransfer] namespace in your app. Then, add an instance of the [**DataPackage**][DataPackage] object. This object contains both the data the user wants to copy and any properties (such as a description) that you want to include.

<!-- For some reason, the snippets in this file are all inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
DataPackage dataPackage = new DataPackage();
```

## Copy and cut

Copy and cut (also referred to as move) work almost exactly the same. Choose which operation you want using the [**DataPackage.RequestedOperation**][RequestedOperation] property.

```cs
// copy 
dataPackage.RequestedOperation = DataPackageOperation.Copy;
// or cut
dataPackage.RequestedOperation = DataPackageOperation.Move;
```

Next, you can add the data that a user has selected to the [**DataPackage**][DataPackage] object. If this data is supported by the **DataPackage** class, you can use one of the corresponding methods in the **DataPackage** object. Here's how to add text:

```cs
dataPackage.SetText("Hello World!");
```

The last step is to add the [**DataPackage**][DataPackage] to the clipboard by calling the static [**Clipboard.SetContent**][SetContent] method.

```cs
Clipboard.SetContent(dataPackage);
```
## Paste

To get the contents of the clipboard, call the static [**Clipboard.GetContent**[GetContent] method. This method returns a [**DataPackageView**][DataPackageView] that contains the content. This object is almost identical to a [**DataPackage**][DataPackage] object, except that its contents are read-only. With that object, you can use either the [**AvailableFormats**][AvailableFormats] or the [**Contains**][Contains] method to identify what formats are available. Then, you can call the corresponding **DataPackageView** method to get the data.

```cs
DataPackageView dataPackageView = Clipboard.GetContent();
if (dataPackageView.Contains(StandardDataFormats.Text))
{
    string text = await dataPackageView.GetTextAsync();
    // To output the text from this example, you need a TextBlock control
    TextOutput.Text = "Clipboard now contains: " + text;
}
```

## Track changes to the clipboard

In addition to copy and paste commands, you may also want to track clipboard changes. Do this by handling the clipboard's [**Clipboard.ContentChanged**][ContentChanged] event.

```cs
Clipboard.ContentChanged += (s, e) => 
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        string text = await dataPackageView.GetTextAsync();
        // To output the text from this example, you need a TextBlock control
        TextOutput.Text = "Clipboard now contains: " + text;
    }
}
```

<!-- LINKS --> 
[DataTransfer]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.aspx 
[DataPackage]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.aspx 
[DataPackageView]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.aspx
[DataPackagePropertySet]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackagepropertyset.aspx 
[DataRequest]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.aspx 
[DataRequested]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.datarequested.aspx 
[FailWithDisplayText]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext.aspx
[ShowShareUi]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.showshareui.aspx
[RequestedOperation]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.requestedoperation.aspx 
[ControlsList]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/mt185406.aspx 
[SetContent]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.setcontent.aspx 
[GetContent]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.getcontent.aspx
[AvailableFormats]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.availableformats.aspx 
[Contains]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.contains.aspx
[ContentChanged]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.contentchanged.aspx 

<!--HONumber=Mar16_HO1-->
