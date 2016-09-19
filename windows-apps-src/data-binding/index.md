---
author: mcleblanc
ms.assetid: 83b4be37-6613-4d00-a48a-0451a24a30fb
title: Data binding
description: Data binding is a way for your app's UI to display data, and optionally to stay in sync with that data.
translationtype: Human Translation
ms.sourcegitcommit: 3144758352b99f8c145a3c7be8a6c43d6a002104
ms.openlocfilehash: e26b9753b3169e124c710a3f2e20907c749e5ec3

---

# Data binding

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Data binding is a way for your app's UI to display data, and optionally to stay in sync with that data. Data binding allows you to separate the concern of data from the concern of UI, and that results in a simpler conceptual model as well as better readability, testability, and maintainability of your app. In markup, you can choose to use either the [{x:Bind} markup extension](https://msdn.microsoft.com/library/windows/apps/Mt204783) or the [{Binding} markup extension](https://msdn.microsoft.com/library/windows/apps/Mt204782). And you can even use a mixture of the two in the same app—even on the same UI element. {x:Bind} is new for Windows 10 and it has better performance.

| Topic | Description |
|-------|-------------|
| [Data binding overview](data-binding-quickstart.md) | This topic shows you how to bind a control (or other UI element) to a single item or bind an items control to a collection of items in a Universal Windows Platform (UWP) app. In addition, we show how to control the rendering of items, implement a details view based on a selection, and convert data for display. For more detailed info, see [Data binding in depth](data-binding-in-depth.md). | 
| [Data binding in depth](data-binding-in-depth.md) | This topic describes data binding features in detail. |
| [Sample data on the design surface, and for prototyping](displaying-data-in-the-designer.md) | In order to have your controls populated with data in the Visual Studio designer (so that you can work on your app's layout, templates, and other visual properties), there are various ways in which you can use design-time sample data. Sample data can also be really useful and time-saving if you're building a sketch (or prototype) app. You can use sample data in your sketch or prototype at run-time to illustrate your ideas without going as far as connecting to real, live data. |
| [Bind hierarchical data and create a master/details view](how-to-bind-to-hierarchical-data-and-create-a-master-details-view.md) | You can make a multi-level master/details (also known as list-details) view of hierarchical data by binding items controls to [<strong>CollectionViewSource</strong>](https://msdn.microsoft.com/library/windows/apps/BR209833) instances that are bound together in a chain. |




<!--HONumber=Aug16_HO5-->


