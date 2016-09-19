---
author: mcleblanc
title: Choosing a programming language
ms.assetid: 6CA46432-BF03-4B20-9187-565B3503B497
description: Choosing a programming language
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: c7d99923a0deac61e289d3a2796b8dd1720430b4

---

# Getting started: Choosing a programming language

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

## Choosing a programming language

Before we go any further, you should know about the programming languages that you can choose from when you develop Universal Windows Platform (UWP) apps. Although the walkthroughs in this article use C#, you can develop UWP apps using one or more programming languages (see [Languages, tools and frameworks](https://msdn.microsoft.com/library/windows/apps/dn465799)).

You can develop using C++, C#, Microsoft Visual Basic, and JavaScript. JavaScript uses HTML5 markup for UI layout, and the other languages use a markup language called *Extensible Application Markup Language (XAML)* to describe their UI.

Although we're focusing on C# in this article, the other languages offer unique benefits, which you may want to explore. For example, if your app's performance is a primary concern, especially for intensive graphics, then C++ might be the right choice. The Microsoft .NET version of Visual Basic is great for Visual Basic app developers. JavaScript with HTML5 is great for those coming from a web development background. For more info, see one of the following:

-   [Create your first Windows Store app using C++](https://msdn.microsoft.com/library/windows/apps/hh974580)
-   [Create your first Windows Store app using C# or Visual Basic](https://msdn.microsoft.com/library/windows/apps/hh974581)
-   [Create your first Windows Store app using JavaScript](https://msdn.microsoft.com/library/windows/apps/br211385)
-   [Create your first Windows Phone Store app using C# or Visual Basic](http://go.microsoft.com/fwlink/p/?LinkID=397877)
-   [WinJS on Windows Phone 8.1](http://go.microsoft.com/fwlink/p/?LinkID=397879)

**Note**  For apps that use 3D graphics, the OpenGL and OpenGL ES standards are not natively available for UWP apps. If you would rather not rewrite your OpenGL ES code into Microsoft DirectX, you may be interested to know about **Angle**. Angle is an on-going project designed to convert OpenGL to DirectX by translating OpenGL API calls into DirectX API calls. To learn more, see the following:
-   [Angle](https://code.google.com/p/angleproject/)
-   [Create your first Windows Store app using DirectX](https://msdn.microsoft.com/library/windows/apps/br229580)
-   [Windows Store app samples that use DirectX](http://go.microsoft.com/fwlink/p/?LinkId=263603)
-   [Where is the DirectX SDK?](https://msdn.microsoft.com/library/windows/desktop/ee663275)

## Giving C# a go

As an iOS developer, you're accustomed to Objective-C and Swift. The closest Microsoft programming language to both is C#. For most developers and most apps, we think C# is the easiest and fastest language to learn and use, so this article's info and walkthroughs focus on that language. To learn more about C#, see the following:

-   [Create your first Windows Store app using C# or Visual Basic](https://msdn.microsoft.com/library/windows/apps/hh974581)
-   [Windows Store app samples that use C#](http://go.microsoft.com/fwlink/p/?LinkId=263453)
-   [Visual C#](http://go.microsoft.com/fwlink/p/?LinkId=263450)

Following is a class written in Objective-C and C#. The Objective-C version is shown first, followed by the C# version.

```obj-c
// Objective-C header: SampleClass.h.

#import <Foundation/Foundation.h>

@interface SampleClass : NSObject {
    BOOL localVariable;
}

@property (nonatomic) BOOL localVariable;

-(int) addThis: (int) firstNumber andThis: (int) secondNumber;

@end
```

```obj-c
// Objective-C implementation.

#import "SampleClass.h"

@implementation SampleClass

@synthesize localVariable = _localVariable;

- (id)init {
    self = [super init];
    if (self) {
        localVariable = true;
    }
    return self;
}

-(int) addThis: (int) firstNumber andThis: (int) secondNumber {
    return firstNumber + secondNumber;
}

@end
```

```obj-c
// Objective-C usage.

SampleClass *mySampleClass = [[SampleClass alloc] init];
mySampleClass.localVariable = false;
int result = [mySampleClass addThis:1 andThis:2];
```

Now, for the C# version. You'll see that like Swift, the header and the implementation are not in separate files.

```csharp
// C# header and implementation.

using System;

namespace MyApp  // Defines this code' s scope.
{
    class SampleClass
    {
        private bool localVariable;

        public SampleClass() // Constructor.
        {
            localVariable = true;
        }

        public bool myLocalVariable // Property.
        {
            get
            {
                return localVariable;
            }
            set
            {
                localVariable = value; 
            }
        }

        public int AddTwoNumbers(int numberOne, int numberTwo)
        {
            return numberOne + numberTwo;
        }        
    }
}
```

```csharp
// C# usage.

SampleClass mySampleClass = new SampleClass();
mySampleClass.myLocalVariable = false;
int result = mySampleClass.AddTwoNumbers(1, 2);
```

C# is an easy language to pick up, and comes with the many support classes and frameworks that make up .NET. In no time, you'll be happily writing your code without a square bracket in sight!

## Next step

[Getting started: Getting around in Visual Studio](getting-started-getting-around-in-visual-studio.md)



<!--HONumber=Aug16_HO3-->


