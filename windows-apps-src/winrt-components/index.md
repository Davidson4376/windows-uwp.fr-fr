---
Windows Runtime components
.
ms.assetid: 55887622-828b-4318-87f2-25592268f7c1
---

# Windows Runtime components


\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Windows Runtime components are self-contained objects that you can instantiate and use from any language, including C\#, Visual Basic, JavaScript, and C++.

You can use Visual Studio and C\#, Visual Basic, or C++ to create Windows Runtime components that can be used in Universal Windows Platform (UWP) apps.

| Topic | Description |
|-------|-------------|
| [Creating Windows Runtime Components in C++](creating-windows-runtime-components-in-cpp.md) | This article shows how to use C++ to create a Windows Runtime component, which is a DLL that's callable from a Universal Windows app that's built by using JavaScript—or C#, Visual Basic, or C++. |
| [Walkthrough: Creating a basic Windows Runtime component in C++ and calling it from JavaScript or C#](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md) | This walkthrough shows how to create a basic Windows Runtime Component DLL that's callable from JavaScript, C#, or Visual Basic. Before you begin this walkthrough, make sure that you understand concepts such as the Abstract Binary Interface (ABI), ref classes, and the Visual C++ Component Extensions that make working with ref classes easier. For more information, see [Creating Windows Runtime Components in C++](creating-windows-runtime-components-in-cpp.md) and [Visual C++ Language Reference (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699871.aspx). |
| [Creating Windows Runtime Components in C# and Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) | Starting with the .NET Framework 4.5, you can use managed code to create your own Windows Runtime types, packaged in a Windows Runtime component. You can use your component in Universal Windows Platform (UWP) apps with C++, JavaScript, Visual Basic, or C#. This article outlines the rules for creating a component, and discusses some aspects of .NET Framework support for the Windows Runtime. In general, that support is designed to be transparent to the .NET Framework programmer. However, when you create a component to use with JavaScript or C++, you need to be aware of differences in the way those languages support the Windows Runtime. |
| [Walkthrough: Creating a Simple Windows Runtime component and calling it from JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) | This walkthrough shows how you can use the .NET Framework with Visual Basic or C# to create your own Windows Runtime types, packaged in a Windows Runtime component, and how to call the component from your Universal Windows app built for Windows using JavaScript. |
| [Raising Events in Windows Runtime components](raising-events-in-windows-runtime-components.md) | If your Windows Runtime component raises an event of a user-defined delegate type on a background thread (worker thread) and you want JavaScript to be able to receive the event, you can implement and/or raise it in one of these ways: |
 

 

 



<!--HONumber=Mar16_HO1-->
