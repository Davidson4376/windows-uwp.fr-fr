---
Description: The date picker gives you a standardized way to let users pick a localized date value using touch, mouse, or keyboard input. 
title: Date picker
ms.assetid: d4a01425-4dee-4de3-9a05-3e85c3fc03cb
isNew: true
label: Date picker
template: detail.hbs
---

# Date picker

The date picker gives you a standardized way to let users pick a localized date value using touch, mouse, or keyboard input. 

<span class="sidebar_heading" style="font-weight: bold;">Important APIs</span>

-   [**DatePicker class**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.aspx)
-   [**Date property**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.date.aspx)

## Is this the right control?
Use a date picker to let a user pick a known date, such as a date of birth, where the context of the calendar is not important.

For more info about choosing the right date control, see the [Date and time controls](date-and-time.md) article.

## Examples

The entry point displays the chosen date, and when the user selects the entry point, a picker surface expands vertically from the middle for the user to make a selection. The date picker overlays other UI; it doesn't push other UI out of the way.

![Example of the date picker expanding](images/controls_datepicker_expand.png)

## Create a date picker

This example shows how to create a simple date picker with a header.

```xaml
<DatePicker x:Name=birthDatePicker Header="Date of birth"/>
```

```csharp
DatePicker birthDatePicker = new DatePicker();
birthDatePicker.Header = "Date of birth";
```

The resulting date picker looks like this:

![Example of date picker](images/date-picker-closed.png)

> **Note**&nbsp;&nbsp;For important info about date values, see [DateTime and Calendar values](date-and-time.md#datetime-and-calendar-values) in the Date and time controls article.

\[This article contains information that is specific to Universal Windows Platform (UWP) apps and Windows 10. For Windows 8.1 guidance, please download the [Windows 8.1 guidelines PDF](https://go.microsoft.com/fwlink/p/?linkid=258743).\]

## Related topics

* [Date and time controls](date-and-time.md)
* [Calendar date picker](calendar-date-picker.md)
* [Calendar view](calendar-view.md)
* [Time picker](time-picker.md)
<!--HONumber=Mar16_HO1-->
