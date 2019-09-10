---
description: Réponses aux questions que vous êtes susceptibles de vous poser sur la création et l’utilisation d’API Windows Runtime avec C++/WinRT.
title: Forum aux questions sur C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, questions, fréquentes, FAQ, forum aux questions
ms.localizationpriority: medium
ms.openlocfilehash: a8da69f0041c71ecfc7429cae2ed51eee0f87d5e
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393490"
---
# <a name="frequently-asked-questions-about-cwinrt"></a>Forum aux questions sur C++/WinRT
Réponses aux questions que vous êtes susceptibles de vous poser sur la création et l’utilisation d’API Windows Runtime avec [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

> [!NOTE]
> Si votre question concerne un message d’erreur que vous avez vu, consultez également la rubrique [Résolution des problèmes C++/WinRT](troubleshooting.md).

## <a name="how-do-i-retarget-my-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>Comment faire pour recibler mon projet C++/WinRT vers une version ultérieure du SDK Windows ?
Consultez [Guide pratique pour recibler votre projet C++/WinRT vers une version ultérieure du SDK Windows](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk).

## <a name="why-wont-my-new-project-compile-now-that-ive-moved-to-cwinrt-20"></a>Pourquoi la compilation de mon nouveau projet échoue-t-elle maintenant que je suis passé à C++WinRT 2.0 ?
Pour connaître l’ensemble de modifications (y compris les changements cassants), consultez [Nouveautés et changements dans C++WinRT 2.0](news.md#news-and-changes-in-cwinrt-20). Par exemple, si vous utilisez un `for` basé sur une plage sur une collection Windows Runtime, vous devez maintenant `#include <winrt/Windows.Foundation.Collections.h>`.

## <a name="why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134"></a>Pourquoi la compilation de mon nouveau projet échoue-t-elle ? J’utilise Visual Studio 2017 (version 15.8.0 ou ultérieure) et le SDK version 17134
Si vous utilisez Visual Studio 2017 (version 15.8.0 ou ultérieure) et que vous ciblez le SDK Windows version 10.0.17134.0 (Windows 10 version 1803), la compilation d’un nouveau projet C++/WinRT risque d’échouer avec l’erreur « *erreur C3861 : 'from_abi' : Identificateur introuvable* » et avec d’autres erreurs provenant de *base.h*. La solution consiste à cibler une version ultérieure (plus conforme) du SDK Windows, ou à définir la propriété de projet **C/C++**  > **Langage** > **Mode de conformité : Non** (en outre, si **/permissive-** apparaît dans la propriété de projet **C /C++**  > **Ligne de commande** sous **Options supplémentaires**, supprimez-la).

## <a name="how-do-i-resolve-the-build-error-the-cwinrt-vsix-no-longer-provides-project-build-support--please-add-a-project-reference-to-the-microsoftwindowscppwinrt-nuget-package"></a>Comment faire pour résoudre l’erreur de build « Le VSIX C++WinRT ne fournit plus de prise en charge de build de projet.  Ajoutez une référence de projet au package Nuget Microsoft.Windows.CppWinRT » ?
Installez le package NuGet **Microsoft.Windows.CppWinRT** dans votre projet. Pour plus d’informations, consultez [Versions antérieures de l’extension VSIX](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsix"></a>Quelles sont les exigences pour l’extension Visual Studio (VSIX) C++/WinRT ?
Pour les versions 1.0.190128.4 et ultérieures de l’extension VSIX, consultez [Prise en charge de Visual Studio pour C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package). Pour les autres versions, consultez [Versions antérieures de l’extension VSIX](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

## <a name="whats-a-runtime-class"></a>Qu’est-ce qu’une *classe runtime* ?
Une classe runtime est un type qui peut être activé et utilisé via des interfaces COM modernes, généralement au-delà des limites exécutables. Toutefois, une classe runtime peut également être utilisée au sein de l’unité de compilation qui l’implémente. Vous déclarez une classe runtime dans le langage IDL (Interface Definition Language), et vous pouvez l’implémenter en C++ standard à l’aide de C++/WinRT.

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>Que signifient *type projeté* et *type d’implémentation* ?
Si vous ne faitez qu’*utiliser* une classe Windows Runtime (classe runtime), vous serez confronté exclusivement à des *types projetés*. C++/WinRT étant une *projection de langage*, les types projetés font partie de la surface de Windows Runtime qui est *projetée* en C++ avec C++/WinRT. Pour plus d’informations, consultez [Utiliser des API avec C++/WinRT](consume-apis.md).

Le *type d’implémentation* contient l’implémentation d’une classe runtime ; il est donc uniquement disponible dans le projet qui implémente la classe runtime. Quand vous travaillez dans un projet qui implémente des classes runtime (un projet de composant Windows Runtime ou un projet qui utilise l’interface utilisateur XAML), il est important d’être à l’aise avec la distinction entre votre type d’implémentation pour une classe runtime et le type projeté qui représente la classe runtime projetée en C++/WinRT. Pour plus d’informations, consultez [Créer des API avec C++/WinRT](author-apis.md).

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>Ai-je besoin de déclarer un constructeur dans le fichier IDL de ma classe runtime ?
Uniquement si la classe runtime est conçue pour être utilisée à partir de l’extérieur de son unité de compilation d’implémentation (c’est un composant Windows Runtime destiné à une utilisation générale par les applications clientes Windows Runtime). Pour plus d’informations sur l’objectif et les conséquences de la déclaration d’un ou plusieurs constructeurs dans IDL, consultez [Constructeurs de classe runtime](author-apis.md#runtime-class-constructors).

## <a name="why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error"></a>Pourquoi l’éditeur de liens retourne-t-il une erreur « LNK2019 : symbole externe non résolu » ?
Si le symbole non résolu est une API des en-têtes d’espace de noms Windows pour la projection C++/WinRT (dans l’espace de noms **winrt**), l’API est déclarée en avance dans un en-tête que vous avez inclus, mais sa définition se trouve dans un en-tête que vous n’avez pas encore inclus. Incluez l’en-tête nommé pour l’espace de noms de l’API et régénérez. Pour plus d’informations, consultez [En-têtes de projection C++/WinRT](consume-apis.md#cwinrt-projection-headers).

Si le symbole non résolu est une fonction libre de Windows Runtime, telle que [RoInitialize](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roinitialize), vous devez inclure explicitement la bibliothèque parapluie [WindowsApp.lib](/uwp/win32-and-com/win32-apis) dans votre projet. La projection C++/WinRT dépend de certains de ces points d’entrée et fonctions libres (non-membres). Si vous utilisez l’un des modèles de projet [extension Visual Studio (VSIX) C++/WinRT](https://aka.ms/cppwinrt/vsix) pour votre application, `WindowsApp.lib` est lié automatiquement pour vous. Dans le cas contraire, vous pouvez utiliser des paramètres de lien entre projets pour l’inclure, ou le faire dans le code source.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

Il est important de résoudre le plus d’erreurs de l’éditeur de liens possible en liant **WindowsApp.lib** au lieu d’une autre bibliothèque de liens statiques, sinon votre application échouera aux tests du [Kit de certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md) utilisés par Visual Studio et par le Microsoft Store pour valider les soumissions (ce qui signifie que votre application ne pourra pas être ingérée par le Microsoft Store).

## <a name="why-am-i-getting-a-class-not-registered-exception"></a>Pourquoi suis-je confronté à l’exception « classe non enregistrée » ?

Quand vous construisez une classe runtime ou accédez à un membre statique, une exception est levée au moment du runtime avec une valeur HRESULT égale à REGDB_E_CLASSNOTREGISTERED.

L’une des causes de cette exception provient de l’impossibilité de charger votre composant Windows Runtime. Vérifiez que le fichier de métadonnées Windows Runtime du composant (`.winmd`) porte le même nom que le fichier binaire du composant (`.dll`), qui est également le nom du projet et le nom de l’espace de noms racine. Vérifiez également que les métadonnées Windows Runtime et le fichier binaire ont été correctement copiés par le processus de génération dans le dossier `Appx` de l’application consommatrice. Vérifiez enfin que le fichier `AppxManifest.xml` de l’application consommatrice (également dans le dossier `Appx`) contient un élément **&lt;InProcessServer&gt;** qui déclare correctement la classe activable et le nom du fichier binaire.

### <a name="uniform-construction"></a>Construction uniforme

Cette erreur peut également se produire si vous essayez d’instancier une classe runtime implémentée localement par le biais de l’un des constructeurs du type projeté (autre que son constructeur **std::nullptr_t**). Pour ce faire, vous avez besoin de la fonctionnalité C++/WinRT 2.0, souvent appelée « construction uniforme ». Si vous souhaitez vous abonner à cette fonctionnalité, pour plus d'informations et des exemples de code, consultez [Accepter la construction uniforme et l'accès à l’implémentation direct](/windows/uwp/cpp-and-winrt-apis/author-apis#opt-in-to-uniform-construction-and-direct-implementation-access).

Pour instancier vos classes runtime implémentées localement *sans* faire appel à la construction uniforme, consultez [Contrôles XAML ; liaison à une propriété C++/WinRT](binding-property.md).

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>Dois-je implémenter [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable) et, si ou, comment ?
Si vous disposez d’une classe runtime qui libère les ressources dans son destructeur, et que cette classe runtime est conçue pour être utilisée à partir de l’extérieur de son unité de compilation d’implémentation (c’est un composant Windows Runtime destiné à une utilisation générale par les applications clientes Windows Runtime), nous vous recommandons d’implémenter également **IClosable** pour prendre en charge l’utilisation de votre classe runtime par les langages qui n’offrent pas de finalisation déterministe. Vérifiez que vos ressources sont libérées si le destructeur, [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close) ou les deux sont appelés. **IClosable::Close** peut être appelé un nombre de fois arbitraire.

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>Ai-je besoin d’appeler [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close) sur les classes runtime que j’utilise ?
**IClosable** existe pour prendre en charge les langages qui n’offrent pas de finalisation déterministe. Par conséquent, vous ne devez pas appeler **IClosable::Close** à partir de C++/WinRT, sauf dans de très rares cas impliquant des concurrences de fermeture ou des étreintes semi-fatales (semi-deadly embraces). Si vous utilisez des types **Windows.UI.Composition**, par exemple, vous pouvez rencontrer des cas où vous souhaiterez disposer des objets dans une séquence définie, au lieu de laisser la destruction du wrapper C++/WinRT faire le travail pour vous.

## <a name="can-i-use-llvmclang-to-compile-with-cwinrt"></a>Puis-je utiliser LLVM/Clang pour compiler avec C++/WinRT ?
Nous ne prenons pas en charge la chaîne d’outils LLVM et Clang pour C++/WinRT, mais nous l’utilisons en interne pour valider la conformité aux normes de C++/WinRT. Par exemple, si vous souhaitez émuler ce que nous faisons en interne, vous pouvez essayer une expérience telle que celle décrite ci-dessous.

Accédez à la [page de téléchargement LLVM](https://releases.llvm.org/download.html), recherchez **Download LLVM 6.0.0** > **Pre-Built Binaries**, puis téléchargez **Clang for Windows (64-bit)** . Pendant l’installation, choisissez d’ajouter LLVM à la variable système PATH afin de pouvoir l’appeler à partir d’une invite de commandes. Dans le cadre de cette expérience, vous pouvez ignorer les erreurs « Failed to find MSBuild toolsets directory » (Impossible de trouver le répertoire des jeux d’outils MSBuild) ou « MSVC integration install failed » (Échec d’installation de l’intégration MSVC), si vous les voyez. Il existe plusieurs façons d’appeler LLVM/Clang ; l’exemple ci-dessous illustre une seule méthode.

```cmd
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

Étant donné que C++/WinRT utilise les fonctionnalités de la norme C++17, vous devez utiliser les indicateurs de compilateur nécessaires pour obtenir cette prise en charge ; ces indicateurs diffèrent d’un compilateur à un autre.

Visual Studio est l’outil de développement que nous prenons en charge et recommandons pour C++/WinRT. Voir [Prise en charge de Visual Studio pour C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="why-doesnt-the-generated-implementation-function-for-a-read-only-property-have-the-const-qualifier"></a>Pourquoi la fonction d’implémentation générée pour une propriété en lecture seule n’a-t-elle pas le qualificateur `const` ?
Quand vous déclarez une propriété en lecture seule dans [MIDL 3.0](/uwp/midl-3/), vous pourriez vous attendre à ce que l’outil `cppwinrt.exe` génère pour vous une fonction d’implémentation qualifiée `const` (une fonction const traite le pointeur *this* comme une constante).

Nous recommandons certainement l’utilisation de const dans la mesure du possible, mais l’outil `cppwinrt.exe` proprement dit ne tente pas de faire la distinction entre les fonctions d’implémentation const et les autres. Vous pouvez choisir de rendre n’importe laquelle de vos fonctions d’implémentation const, comme dans cet exemple.

```cppwinrt
struct MyStringable : winrt::implements<MyStringable, winrt::Windows::Foundation::IStringable>
{
    winrt::hstring ToString() const
    {
        return L"MyStringable";
    }
};
```

Vous pouvez retirer ce qualificateur `const` sur **ToString** si vous décidez que vous avez besoin de modifier l’état d’un objet dans son implémentation. Mais rendez chacune de vos fonctions membres const ou non-const, pas les deux. Autrement dit, ne surchargez pas une fonction d’implémentation sur `const`.

Outre vos fonctions d’implémentation, les projections de fonctions Windows Runtime constituent l’un des autres emplacements où const entre en jeu. Prenez cet exemple de code :

```cppwinrt
int main()
{
    winrt::Windows::Foundation::IStringable s{ winrt::make<MyStringable>() };
    auto result{ s.ToString() };
}
```

Pour l’appel à **ToString** ci-dessus, la commande **Go To Declaration** dans Visual Studio montre que la projection du Windows Runtime **IStringable::ToString** en C++/WinRT ressemble à ceci :

```cppwinrt
winrt::hstring ToString() const;
```

Les fonctions sur la projection sont de type const, quelle que soit la façon dont vous choisissez de qualifier leur implémentation. Dans les coulisses, la projection appelle l’interface binaire-programme (ABI, Application Binary Interface), ce qui équivaut à un appel par biais d’un pointeur d’interface COM. Le seul état avec lequel le **ToString** projeté interagit est ce pointeur d’interface COM, et il n’a certainement nul besoin de modifier ce pointeur. Par conséquent, la fonction est const. Cela vous donne la garantie qu’elle ne changera rien concernant la référence **IStringable** par laquelle vous appelez, et garantit que vous pouvez appeler **ToString** même avec une référence const à un **IStringable**.

Sachez que ces exemples de `const` sont des détails d’implémentation de projections et d’implémentations C++/WinRT ; ils constituent une hygiène de code pour votre bénéfice. Il n’existe pas de `const` sur le COM, ni d’ABI Windows Runtime (pour les fonctions membres).

## <a name="do-you-have-any-recommendations-for-decreasing-the-code-size-for-cwinrt-binaries"></a>Avez-vous des recommandations afin de diminuer la taille du code pour les binaires C++/WinRT ?
Quand vous travaillez avec des objets Windows Runtime, vous devez éviter le modèle de codage indiqué ci-dessous, car il peut avoir un impact négatif sur votre application en générant plus de code binaire que nécessaire.

```cppwinrt
anobject.b().c().d();
anobject.b().c().e();
anobject.b().c().f();
```

Dans le monde de Windows Runtime, le compilateur ne peut pas mettre en cache la valeur de `c()` ou les interfaces pour chaque méthode appelée par le biais d’une indirection ('.'). Sauf si vous intervenez, cela provoque plus d’appels virtuels et une surcharge de comptage des références. Le modèle ci-dessus pourrait facilement générer deux fois plus de code que ce qui est strictement nécessaire. Au lieu de cela, privilégiez dans la mesure du possible le modèle illustré ci-dessous. Il génère beaucoup moins de code, et peut aussi améliorer considérablement les performances d’exécution.

```cppwinrt
auto a{ anobject.b().c() };
a.d();
a.e();
a.f();
```

Le modèle recommandé ci-dessus s’applique non seulement à C++/WinRT, mais aussi à toutes les projections de langage Windows Runtime.

## <a name="how-do-i-turn-a-string-into-a-typemdashfor-navigation-for-example"></a>Comment convertir une chaîne en type, par exemple pour la navigation ?
À la fin de l’[exemple de code d’affichage Navigation](/windows/uwp/design/controls-and-patterns/navigationview#code-example) (qui est pour la plupart en C#) se trouve un extrait de code C++/WinRT illustrant comment procéder.

## <a name="how-do-i-resolve-ambiguities-with-getcurrenttime-andor-try"></a>Comment faire pour résoudre les ambiguïtés avec GetCurrentTime et/ou TRY ?

Le fichier d’en-tête `winrt/Windows.UI.Xaml.Media.Animation.h` déclare une méthode nommée **GetCurrentTime**, tandis que `windows.h` (par le biais de `winbase.h`) définit une macro nommée **GetCurrentTime**. Quand les deux entrent en conflit, le compilateur C++ génère « *Erreur C4002 : trop d’arguments pour l’appel de macro de type fonction GetCurrentTime* ».

De même, `winrt/Windows.Globalization.h` déclare une méthode nommée **TRY**, tandis que `afx.h` définit une macro nommée **GetCurrentTime**. Quand un conflit se produit, le compilateur C++ génère « *Erreur C2334 : jetons inattendus avant '{' ; corps apparent de la fonction ignoré* ».

Pour résoudre l’un de ces problèmes ou les deux, procédez comme suit.

```cppwinrt
#pragma push_macro("GetCurrentTime")
#pragma push_macro("TRY")
#undef GetCurrentTime
#undef TRY
#include <winrt/include_your_cppwinrt_headers_here.h>
#include <winrt/include_your_cppwinrt_headers_here.h>
#pragma pop_macro("TRY")
#pragma pop_macro("GetCurrentTime")
```

> [!NOTE]
> Si cette rubrique n’a pas répondu à votre question, vous pouvez rechercher de l’aide en accédant à la page de la [Communauté de développeurs Visual Studio C++](https://developercommunity.visualstudio.com/spaces/62/index.html) ou en utilisant le mot-clé [`c++-winrt` sur Stack Overflow](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt).
