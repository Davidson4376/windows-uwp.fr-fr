---
author: TylerMSFT
ms.assetid: 23FE28F1-89C5-4A17-A732-A722648F9C5E
title: Asynchronous programming
description: This topic describes asynchronous programming in the Universal Windows Platform (UWP) and its representation in C#, Microsoft Visual Basic .NET, Visual C\+\+ component extensions (C\+\+/CX), and JavaScript.
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: c033c1d985b9373d9cbadf38463610aa1922163e

---
# Asynchronous programming

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


This topic describes asynchronous programming in the Universal Windows Platform (UWP) and its representation in C#, Microsoft Visual Basic .NET, Visual C++ component extensions (C++/CX), and JavaScript.

Using asynchronous programming helps your app stay responsive when it does work that might take an extended amount of time. For example, an app that downloads content from the Internet might spend several seconds waiting for the content to arrive. If you used a synchronous method on the UI thread to retrieve the content, the app is blocked until the method returns. The app won't respond to user interaction, and because it seems non-responsive, the user might become frustrated. A much better way is to use asynchronous programming, where the app continues to run and respond to the UI while it waits for an operation to complete.

For methods that might take a long time to complete, asynchronous programming is the norm and not the exception in the UWP. JavaScript, C#, Visual Basic, and C++/CX each provide language support for asynchronous methods.

## Asynchronous programming in the UWP

Many UWP features like the [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/BR241124) APIs and [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171) APIs are exposed as asynchronous APIs. By convention, the names of asynchronous APIs end with "Async" to indicate that part of their execution may take place after the API has been invoked.

When you use asynchronous APIs in your Universal Windows Platform (UWP) app, your code makes non-blocking calls in a consistent way. When you implement these asynchronous patterns in your own API definitions, callers can understand and use your code in a predictable way.

Here are some common tasks that require calling asynchronous UWP APIs.

-   Displaying a message dialog

-   Working with the file system, displaying a file picker

-   Sending and receiving data to and from the Internet

-   Using sockets, streams, connectivity

-   Working with appointments, contacts, calendar

-   Working with file types, such as opening Portable Document Format (PDF) files or decoding image or media formats

-   Interacting with a device or a service

With UWP asynchronous pattern, you may be able to avoid explicitly manage threads at all. Each programming language supports the asynchronous pattern for the UWP in its own way:

| Programming language | Asynchronous representation           |
|----------------------|---------------------------------------|
| C#                  | **async** keyword, **await** operator |
| Visual Basic         | **Async** keyword, **Await** operator |
| C++/CX               | **task** class, **.then** method      |
| JavaScript           | promise object, **then** function     |

 

## Asynchronous patterns in UWP using C# and Visual Basic


A typical segment of code written in C# or Visual Basic executes synchronously, meaning that when a line executes, it finishes before the next line executes. There have been previous Microsoft .NET programming models for asynchronous execution, but the resulting code tends to emphasize the mechanics of executing asynchronous code instead of focusing on the task that the code is trying to accomplish. The UWP, .NET framework, and C# and Visual Basic compilers have added features that abstract the asynchronous mechanics out of your code. For .NET and the UWP you can write asynchronous code that focuses on what your code does instead of how and when to do it. Your asynchronous code will look reasonably similar to synchronous code. For more info, see [Call asynchronous APIs in C# or Visual Basic](call-asynchronous-apis-in-csharp-or-visual-basic.md).

## Asynchronous patterns in UWP with C++


In C++/CX, asynchronous programming is based on the [**task class**](https://msdn.microsoft.com/library/windows/apps/xaml/hh750113.aspx), and its [**then method**](https://msdn.microsoft.com/library/windows/apps/xaml/hh750044.aspx). The syntax is similar to that of JavaScript promises. The **task class** and its related types also provide the capability for cancellation and management of the thread context. For more info, see [Asynchronous programming in C++](asynchronous-programming-in-cpp-universal-windows-platform-apps.md).

The [**create\_async function**](https://msdn.microsoft.com/library/windows/apps/xaml/hh750102.aspx) provides support for producing asynchronous APIs that can be consumed from JavaScript or any other language that supports the UWP. For more info, see [Creating Asynchronous Operations in C++](https://msdn.microsoft.com/library/windows/apps/xaml/hh750082.aspx).

## Asynchronous patterns in UWP using JavaScript

In JavaScript, asynchronous programming follows the [Common JS Promises/A](http://wiki.commonjs.org/wiki/Promises/A) proposed standard by having asynchronous methods return promise objects. Promises are used in both the UWP and Windows Library for JavaScript.

A promise object represents a value that will be fulfilled in the future. In the UWP you get a promise object from a factory function, which by convention has a name that ends with "Async".

In many cases, calling an asynchronous function is almost as simple as calling a conventional function. The difference is that you use the [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728) or the [**done**](https://msdn.microsoft.com/library/windows/apps/Hh701079) method to assign the handlers for results or errors and to start the operation.

## Related topics

* [Call asynchronous APIs in C# or Visual Basic](call-asynchronous-apis-in-csharp-or-visual-basic.md)
* [Asynchronous Programming with Async and Await (C# and Visual Basic)](http://msdn.microsoft.com/library/hh191443(vs.110).aspx)
* [Reversi sample feature scenarios: asynchronous code](https://msdn.microsoft.com/library/windows/apps/xaml/jj712233.aspx#async)




<!--HONumber=Aug16_HO3-->


