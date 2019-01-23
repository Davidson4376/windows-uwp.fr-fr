---
description: Réponses aux questions que vous êtes susceptibles de vous poser sur la création et l’utilisation d’API Windows Runtime avec C++/WinRT.
title: Forum aux questions sur C++/WinRT
ms.date: 10/26/2018
ms.topic: article
keywords: windows10, uwp, standard, c++, cpp, winrt, projection, questions, fréquentes, FAQ, forum aux questions
ms.localizationpriority: medium
ms.openlocfilehash: 3be87431d421b2307bc298934adc991d6e71c6ab
ms.sourcegitcommit: 4a359aecafb73d73b5a8e78f7907e565a2a43c41
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2019
ms.locfileid: "9024508"
---
# <a name="frequently-asked-questions-about-cwinrt"></a>Forum aux questions sur C++/WinRT
Réponses aux questions que vous êtes susceptible d’avoir sur la création et l’utilisation de Windows Runtime APIs avec [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

> [!NOTE]
> Si votre question concerne un message d’erreur que vous avez vu, consultez également la rubrique [Résolution des problèmes C++/WinRT](troubleshooting.md).

## <a name="how-do-i-retarget-my-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>Comment recibler mon C++ / WinRT projet vers une version ultérieure du SDK Windows?

Voir [comment recibler votre C++ / WinRT projet vers une version ultérieure du SDK Windows](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk).

## <a name="why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134"></a>Pourquoi mon projet ne sera pas compilé? Je suis à l’aide de Visual Studio 2017 (version 15.8.0 ou une version ultérieure) et du SDK version 17134

Si vous utilisez Visual Studio 2017 (version 15.8.0 ou une version ultérieure) et vous ciblez le SDK Windows version 10.0.17134.0 (Windows 10, version 1803), puis un nouvellement créé C + / WinRT projet peut échouer compiler avec l’erreur «*erreur C3861: 'from_abi': identificateur pas trouvé*» et à d’autres erreurs dans *base.h*d’origine. La solution consiste à soit cible une version ultérieure (conforme plus) version du SDK Windows ou de la propriété de projet de jeu **C/C++** > **langue** > **Conformance mode: N°** (en outre, si **/ permissive-** s’affiche dans la propriété de projet ** C/C++** > de**ligne de commande** sous **Options supplémentaires**, puis supprimez).

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsixhttpsakamscppwinrtvsix"></a>Quelles sont les exigences pour l’[extension Visual Studio (VSIX) C++/WinRT](https://aka.ms/cppwinrt/vsix)?
[VSIX](https://aka.ms/cppwinrt/vsix) applique une version cible du SDK Windows minimale de 10.0.17134.0 (Windows10, version1803). Vous avez également besoin de Visual Studio2017 (au moins la version15,6; nous recommandons au moins la version15.7). Vous pouvez identifier un projet qui utilise VSIX par la présence de `<CppWinRTEnabled>true</CppWinRTEnabled>` dans `<PropertyGroup Label="Globals">` dans le fichier `.vcxproj`. Pour plus d’informations, voir [Prise en charge de Visual Studio pour C++/WinRT et VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-and-the-vsix).

## <a name="whats-a-runtime-class"></a>Qu’est-ce qu’une *classe runtime*?
Une classe runtime est un type qui peut être activé et utilisé via des interfaces COM modernes, généralement au-delà des limites exécutables. Toutefois, une classe runtime peut également être utilisée au sein de l’unité de compilation qui l’implémente. Vous déclarez une classe runtime dans le langage IDL (Interface Definition Language), et vous pouvez l’implémenter en C++ standard à l’aide de C++/WinRT.

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>Que signifient *type projeté* et *type d’implémentation*?
Si vous ne fait qu’*utiliser* une classe Windows Runtime (classe runtime), vous serez confronté exclusivement à des *types projetés*. C++/WinRT étant une *projection de langage*, les types projetés font partie de la surface de Windows Runtime qui est *projetée* en C++ avec C++/WinRT. Pour plus d’informations, voir [utiliser des API avec C++ / WinRT](consume-apis.md).

Le *type d’implémentation* contient l’implémentation d’une classe runtime, il est donc uniquement disponible dans le projet qui implémente la classe runtime. Lorsque vous travaillez dans un projet qui implémente des classes runtime (un projet de composant Windows Runtime ou un projet qui utilise l’interface utilisateur XAML), il est important d’être à l’aise avec la distinction entre votre type d’implémentation pour une classe runtime et le type projeté qui représente la classe runtime projetée en C++/WinRT. Pour obtenir plus d’informations, voir [Créer des API avec C++/WinRT](author-apis.md).

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>Ai-je besoin de déclarer un constructeur dans le fichier IDL de ma classe runtime?
Uniquement si la classe runtime est conçue pour être utilisée depuis l’extérieur de son unité de compilation d’implémentation (c’est un composant Windows Runtime destiné à une utilisation générale par les applications clientes Windows Runtime). Pour plus d’informations sur l’objectif et les conséquences de la déclaration d’un ou de plusieurs constructeurs dans IDL, voir [Constructeurs de classe runtime](author-apis.md#runtime-class-constructors).

## <a name="why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error"></a>Pourquoi l’éditeur de liens me donne-t-il une erreur «LNK2019: symbole externe non résolu»?
Si le symbole non résolu est une API des en-têtes d’espace de noms Windows pour la projection C++/WinRT (dans l'espace de noms **winrt**), l’API est déclarée en avance dans un en-tête que vous avez inclus, mais sa définition se trouve dans un en-tête que vous n’avez pas encore inclus. Incluez l’en-tête nommé pour l’espace de noms de l’API et régénérez. Pour plus d’informations, voir [en-têtes de projection C++/WinRT](consume-apis.md#cwinrt-projection-headers).

Si le symbole non résolu est une fonction libre de Windows Runtime, tels que [RoInitialize](https://msdn.microsoft.com/library/br224650), vous devez lier explicitement la bibliothèque PARAPLUIE [WindowsApp.lib](/uwp/win32-and-com/win32-apis) dans votre projet. La projection C++/WinRT dépend de certains de ces points d’entrée et fonctions libres (non-membres). Si vous utilisez un des modèles de projet [extension Visual Studio (VSIX) C++/WinRT](https://aka.ms/cppwinrt/vsix) pour votre application, `WindowsApp.lib` est lié automatiquement pour vous. Dans le cas contraire, vous pouvez utiliser des paramètres de lien entre projets pour l'inclure, ou le faire dans le code source.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

Il est important que vous résolvez les éventuelles erreurs de l’éditeur de liens que vous pouvez en liant **WindowsApp.lib** au lieu d’une autre bibliothèque de liaison statique, dans le cas contraire votre application ne sont pas passer les tests du [Kit de Certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md) utilisés par Visual Studio et le Microsoft Store pour valider les soumissions (ce qui signifie qu’il ne sont pas par conséquent être possible de votre application pour être correctement ingérés par le Microsoft Store).

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>Dois-je implémenter [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable) et, si tel est le cas, comment?
Si vous disposez d’une classe runtime qui libère les ressources dans son destructeur, et que cette classe runtime est conçue pour être utilisée depuis l’extérieur de son unité de compilation d’implémentation (c’est un composant Windows Runtime destiné à une utilisation générale par les applications clientes Windows Runtime), nous vous recommandons d’implémenter également **IClosable** pour prendre en charge l’utilisation de votre classe runtime par les langues qui ne possèdent pas de finalisation déterministe. Assurez-vous que vos ressources sont libérées si le destructeur, [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.Close) ou les deux sont appelés. **IClosable::Close** peut être appelé un nombre de fois arbitraire.

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosablewindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>Ai-je besoin d’appeler [**IClosable::Close**](/uwp/api/windows.foundation.iclosable#Windows_Foundation_IClosable_Close_) sur les classes runtime que j’utilise?
**IClosable** existe pour prendre en charge les langues qui ne possèdent pas de finalisation déterministe. Par conséquent, vous ne devez pas appeler **IClosable::Close** à partir de C++/WinRT, sauf dans de très rares cas impliquant des concurrences de fermeture ou des étreintes semi-fatales (semi-deadly embraces). Si vous utilisez des types **Windows.UI.Composition**, par exemple, vous pouvez rencontrer des cas où vous souhaiterez disposer des objets dans une séquence définie, au lieu de laisser la destruction du wrapper C++/WinRT faire le travail pour vous.

## <a name="can-i-use-llvmclang-to-compile-with-cwinrt"></a>Puis-je utiliser LLVM/Clang pour compiler avec C++/WinRT?
Nous ne prenons pas en charge la chaîne d’outils LLVM et Clang pour C++/WinRT, mais nous l'utilisons en interne pour valider la conformité aux normes de C++/WinRT. Par exemple, si vous souhaitez émuler ce que nous faisons en interne, vous pouvez essayer une expérience telle que celle décrite ci-dessous.

Accédez à la [LLVM Download Page](https://releases.llvm.org/download.html), recherchez **Download LLVM 6.0.0** > **Pre-Built Binaries**et téléchargez **Clang for Windows (64-bit)**. Pendant l’installation, choisissez d’ajouter LLVM à la variable système PATH afin de pouvoir l'appeler à partir d’une invite de commandes. Dans le cadre de cette expérience, vous pouvez ignorer les erreurs «Failed to find MSBuild toolsets directory» (Impossible de trouver le répertoire des jeux d’outils MSBuild) ou MSVC integration install failed (Échec d'installation de l’intégration MSVC), si vous les voyez. Il existe plusieurs façons d’appeler LLVM/Clang; l’exemple ci-dessous illustre une seule méthode.

```
C:\ExperimentWithLLVMClang>type main.cpp
// main.cpp
#pragma comment(lib, "windowsapp")
#pragma comment(lib, "ole32")

#include <winrt/Windows.Foundation.h>
#include <stdio.h>
#include <iostream>

using namespace winrt;

int main()
{
    winrt::init_apartment();
    Windows::Foundation::Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    std::wcout << rssFeedUri.Domain().c_str() << std::endl;
}

C:\ExperimentWithLLVMClang>clang-cl main.cpp /EHsc /I ..\.. -Xclang -std=c++17 -Xclang -Wno-delete-non-virtual-dtor -o app.exe

C:\ExperimentWithLLVMClang>app
windows.com
```

Étant donné que C++/WinRT utilise les fonctionnalités de la norme C++17, vous devez utiliser les indicateurs de compilateur nécessaires pour obtenir cette prise en charge; ces indicateurs diffèrent d’un compilateur à un autre.

Visual Studio est l’outil de développement que nous prenons en charge et recommandons pour C++/WinRT. Voir [Prise en charge de Visual Studio pour C++/WinRT et VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-and-the-vsix).

## <a name="why-doesnt-the-generated-implementation-function-for-a-read-only-property-have-the-const-qualifier"></a>Pourquoi la fonction d’implémentation généré pour une propriété en lecture seule n’a pas la `const` qualificateur?

Lorsque vous déclarez une propriété en lecture seule dans [MIDL 3.0](/uwp/midl-3/), vous pouvez vous attendre le `cppwinrt.exe` outil pour générer une fonction d’implémentation pour vous `const`-qualifié (une fonction const traite le pointeur *this* comme const).

Nous recommandons certainement à l’aide de const dans la mesure du possible, mais la `cppwinrt.exe` outil lui-même ne tente pas de raison sur l’implémentation fonctions peut-être intentionnellement const, et qui ne peut pas. Vous pouvez choisir de rendre une des fonctions de votre implémentation const, comme dans cet exemple.

```cppwinrt
struct MyStringable : winrt::implements<MyStringable, winrt::Windows::Foundation::IStringable>
{
    winrt::hstring ToString() const
    {
        return L"MyStringable";
    }
};
```

Vous pouvez supprimer ces `const` qualificateur sur **ToString** doit vous décidez que vous devez modifier un état de l’objet dans son implémentation. Mais chacun des membres de votre fonctions const ou non const, pas les deux. En d’autres termes, ne surchargez pas une fonction de mise en œuvre sur `const`.

Permet d’identifier les fonctions de votre implémentation, un autre autres lieu const est fourni dans l’image se trouve dans les projections de fonction Windows Runtime. Envisagez de ce code.

```cppwinrt
int main()
{
    winrt::Windows::Foundation::IStringable s{ winrt::make<MyStringable>() };
    auto result{ s.ToString() };
}
```

Pour l’appel à **ToString** ci-dessus, la commande **d’Atteindre la déclaration** dans Visual Studio qui affiche la projection de la **IStringable::ToString** Windows Runtime en C++ / WinRT se présente comme suit.

```
winrt::hstring ToString() const;
```

Fonctions de la projection sont const, quelle que soit la méthode vous choisissez pour qualifier votre implémentation de leur. En coulisse, la projection appelle l’interface de binaire d’application (ABI), le qui équivaut à un appel par le biais d’un pointeur d’interface COM. Le seul état la projeté **ToString** interagit avec est ce pointeur d’interface COM; et il n’a pas besoin de modifier ce pointeur, afin que la fonction est const certainement. Ceci permet de vous la garantie que cela ne changera rien aborder la référence **IStringable** que vous appelez par le biais de, et vous vous assurez que vous pouvez appeler **ToString** même avec const faire référence à une **IStringable**.

Comprendre que ces exemples de `const` détails d’implémentation de C++ / WinRT projections et les implémentations; ils constituent hygiène de code à votre avantage. Il n’existe pas de `const` sur le COM, ni ABI Windows Runtime (pour les fonctions de membres).

## <a name="do-you-have-any-recommendations-for-decreasing-the-code-size-for-cwinrt-binaries"></a>Vous disposez des recommandations pour réduire la taille de code pour C++ / WinRT binaires?

Lorsque vous travaillez avec des objets Windows Runtime, vous devez éviter le modèle de codage illustré ci-dessous, car elle peut avoir un impact négatif sur votre application par à l’origine du code binaire plus que nécessaire pour être générés.

```cppwinrt
anobject.b().c().d();
anobject.b().c().e();
anobject.b().c().f();
```

Dans le monde Windows Runtime, le compilateur ne peut pas mettre en cache soit la valeur de `c()` ou des interfaces pour chaque méthode qui est appelée par le biais d’une indirection («.»). Sauf si vous intervenez, qui se traduit par plusieurs appels virtuels et une surcharge de décompte de références. Le modèle ci-dessus peut facilement générer deux fois plus en tant que code comme strictement nécessaire. Au lieu de cela, préférez le modèle indiqué ci-dessous partout où vous pouvez. Elle génère beaucoup moins de code, et elle peut également considérablement améliorer les performances d’exécution.

```cppwinrt
auto a{ anobject.b().c() };
a.d();
a.e();
a.f();
```

Le modèle recommandé ci-dessus s’applique pas seulement à C++ / WinRT, mais à tous les projections de langage Windows Runtime.

## <a name="how-do-i-turn-a-string-into-a-typemdashfor-navigation-for-example"></a>Comment activer une chaîne en un type&mdash;pour la navigation, par exemple?

À la fin de l' [exemple de code de mode de Navigation](/windows/uwp/design/controls-and-patterns/navigationview#code-example) (qui est principalement en c#), il existe un C++ / WinRT extrait de code montrant comment procéder.

> [!NOTE]
> Si cette rubrique n’a pas répondu à votre question, vous pouvez trouver de l’aide en consultant la [Communauté de développeurs Visual Studio C++](https://developercommunity.visualstudio.com/spaces/62/index.html), ou à l’aide de la [ `c++-winrt` balise sur Stack Overflow](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt).
