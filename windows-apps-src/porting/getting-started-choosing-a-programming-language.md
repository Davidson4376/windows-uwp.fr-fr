---
author: mcleblanc
title: Choix d’un langage de programmation
ms.assetid: 6CA46432-BF03-4B20-9187-565B3503B497
description: Choix d’un langage de programmation
---

# Prise en main &#58; Choix d’un langage de programmation

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

## Choix d’un langage de programmation

Avant d’aller plus loin, vous devez connaître les langages de programmation permettant de développer des applications pour la plateforme Windows universelle (UWP). Bien que les procédures pas à pas du présent article utilisent le langage C#, vous pouvez développer des applications pour la plateforme Windows universelle (UWP) à l’aide d’un ou de plusieurs langages de programmation (voir [Langages, outils et infrastructures](https://msdn.microsoft.com/library/windows/apps/dn465799)).

Vous pouvez développer à l’aide des langages C++, C#, Microsoft Visual Basic et JavaScript. JavaScript utilise le balisage HTML5 pour la disposition de l’interface utilisateur et les autres langages utilisent un langage de balisage appelé *XAML (Extensible Application Markup Language)* pour décrire leur interface utilisateur.

Bien que l’article se concentre ici sur C#, les autres langages offrent des avantages uniques qu’il peut être intéressant d’explorer. Par exemple, si la performance de votre application est une priorité, surtout pour des composants graphiques complexes, C++ peut s’avérer le meilleur choix. La version Microsoft .NET de Visual Basic donne de très bons résultats pour les développeurs d’applications Visual Basic. JavaScript avec HTML5 convient parfaitement pour les professionnels spécialisés dans le développement Web. Pour plus d’informations, voir les rubriques suivantes :

-   [Créer votre première application du Windows Store en C++](https://msdn.microsoft.com/library/windows/apps/hh974580)
-   [Créer votre première application du Windows Store en C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/hh974581)
-   [Créer votre première application du Windows Store en JavaScript](https://msdn.microsoft.com/library/windows/apps/br211385)
-   [Créer votre première application du Windows Phone Store en C# ou Visual Basic](http://go.microsoft.com/fwlink/p/?LinkID=397877)
-   [WinJS sur Windows Phone 8.1](http://go.microsoft.com/fwlink/p/?LinkID=397879)

**Remarque** Pour les applications avec des graphiques 3D, les normes OpenGL et OpenGL ES ne sont pas disponibles en mode natif pour les applications UWP. Si vous préférez ne pas réécrire votre code OpenGL ES dans Microsoft DirectX, vous pourriez être intéressé par **Angle**. Angle est un projet en cours conçu pour convertir OpenGL en DirectX en transformant des appels d’API OpenGL en appels d’API DirectX. Pour en savoir plus, consultez les rubriques suivantes :
-   [Angle](https://code.google.com/p/angleproject/)
-   [Créer votre première application du Windows Store à l’aide de DirectX](https://msdn.microsoft.com/library/windows/apps/br229580)
-   [Exemples d’applications du Windows Store utilisant DirectX](http://go.microsoft.com/fwlink/p/?LinkId=263603)
-   [Où est le Kit de développement logiciel (SDK) DirectX ?](https://msdn.microsoft.com/library/windows/desktop/ee663275)

## Essayer C#

En tant que développeur iOS, vous êtes accoutumé à Objective-C et à Swift. Le langage de programmation Microsoft le plus proche des deux est C#. Pour la majorité des développeurs et des applications, nous pensons que C# est le langage le plus simple et le plus rapide à apprendre et à utiliser. C’est pourquoi les informations et procédures fournies dans cet article se concentrent sur ce langage. Pour en savoir plus sur le langage C#, voir les rubriques suivantes :

-   [Créer votre première application du Windows Store en C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/hh974581)
-   [Exemples d’applications du Windows Store utilisant C##](http://go.microsoft.com/fwlink/p/?LinkId=263453)
-   [Visual C#](http://go.microsoft.com/fwlink/p/?LinkId=263450)

Voici une classe écrite avec les deux langages, Objective-C et C#. La version Objective-C apparaît en premier, suivie de la version C#.

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

Voyons la version C#. Vous remarquerez que, comme dans Swift, l’en-tête et l’implémentation ne figurent pas dans des fichiers distincts.

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

C# est un langage facile à comprendre. Il est fourni avec les nombreuses classes et infrastructures de prise en charge de .NET. En un clin de œil, vous écrirez avec bonheur votre code sans le moindre crochet !

## Étape suivante

[Prise en main : Savoir maîtriser Visual Studio](getting-started-getting-around-in-visual-studio.md)


<!--HONumber=May16_HO2-->


