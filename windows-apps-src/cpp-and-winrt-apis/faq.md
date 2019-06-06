---
description: Réponses aux questions que vous êtes susceptibles de vous poser sur la création et l’utilisation d’API Windows Runtime avec C++/WinRT.
title: Forum aux questions sur C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, questions, fréquentes, FAQ, forum aux questions
ms.localizationpriority: medium
ms.openlocfilehash: 914cf884b97d14af523cc61b0fcce719104783ba
ms.sourcegitcommit: 1f39b67f2711b96c6b4e7ed7107a9a47127d4e8f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66721690"
---
# <a name="frequently-asked-questions-about-cwinrt"></a>Forum aux questions sur C++/WinRT
Des réponses aux questions que vous êtes susceptible d’avoir sur la création et utilisation de Windows Runtime APIs avec [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

> [!NOTE]
> Si votre question concerne un message d’erreur que vous avez vu, consultez également la rubrique [Résolution des problèmes C++/WinRT](troubleshooting.md).

## <a name="how-do-i-retarget-my-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>Comment recibler C + c++ / projet WinRT vers une version ultérieure du SDK Windows ?
Consultez [comment recibler votre C + c++ / projet WinRT vers une version ultérieure du SDK Windows](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk).

## <a name="why-wont-my-new-project-compile-now-that-ive-moved-to-cwinrt-20"></a>Pourquoi n’est pas mon nouveau projet compilé, maintenant que j’ai déplacé vers C++WinRT 2.0 ?
Pour l’ensemble de modifications (y compris les modifications avec rupture), consultez [actualités et les changements dans C++WinRT 2.0](news.md#news-and-changes-in-cwinrt-20). Par exemple, si vous utilisez basées sur une plage `for` sur une collection de Windows Runtime, puis vous maintenant devrez `#include <winrt/Windows.Foundation.Collections.h>`.

## <a name="why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134"></a>Pourquoi mon nouveau projet n’est pas compilé ? J’utilise Visual Studio 2017 (version 15.8.0 ou une version ultérieure) et le SDK version 17134
Si vous utilisez Visual Studio 2017 (version 15.8.0 ou une version ultérieure) et ciblant le SDK Windows version 10.0.17134.0 (Windows 10, version 1803), puis nouvellement créés C++/WinRT projet risque de ne pas compiler avec l’erreur «*erreur C3861 : 'from_abi' : Identificateur introuvable*» et à d’autres erreurs provenant de *base.h*. La solution consiste à une cible une version ultérieure (conforme plus) version du Kit de développement logiciel Windows ou de la propriété de projet de jeu **C/C++**  > **langage** > **mode de conformité : Ne** (en outre, si **/ permissive-** apparaît dans la propriété de projet **C/C++**  > **ligne de commande** sous **des Options supplémentaires** , puis le supprimer).

## <a name="how-do-i-resolve-the-build-error-the-cwinrt-vsix-no-longer-provides-project-build-support--please-add-a-project-reference-to-the-microsoftwindowscppwinrt-nuget-package"></a>Comment pour résoudre l’erreur de build « C + c++ / WinRT VSIX ne fournit plus de prise en charge de génération de projet.  Ajoutez une référence de projet au package Nuget de Microsoft.Windows.CppWinRT » ?
Installer le **Microsoft.Windows.CppWinRT** package NuGet dans votre projet. Pour plus d’informations, consultez [les versions antérieures de l’extension VSIX](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsix"></a>Quelles sont les exigences pour C / c++ / WinRT Visual Studio Extension (VSIX) ?
Pour la version 1.0.190128.4 de l’extension VSIX et versions ultérieures, consultez [prise en charge de Visual Studio pour C / c++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package). Pour les autres versions, consultez [les versions antérieures de l’extension VSIX](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

## <a name="whats-a-runtime-class"></a>Qu’est-ce qu’une *classe runtime* ?
Une classe runtime est un type qui peut être activé et utilisé via des interfaces COM modernes, généralement au-delà des limites exécutables. Toutefois, une classe runtime peut également être utilisée au sein de l’unité de compilation qui l’implémente. Vous déclarez une classe runtime dans le langage IDL (Interface Definition Language), et vous pouvez l’implémenter en C++ standard à l’aide de C++/WinRT.

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>Que signifient *type projeté* et *type d’implémentation* ?
Si vous ne fait qu’*utiliser* une classe Windows Runtime (classe runtime), vous serez confronté exclusivement à des *types projetés*. C++/WinRT étant une *projection de langage*, les types projetés font partie de la surface de Windows Runtime qui est *projetée* en C++ avec C++/WinRT. Pour plus d’informations, consultez [consommer des API avec C / c++ / WinRT](consume-apis.md).

Le *type d’implémentation* contient l’implémentation d’une classe runtime, il est donc uniquement disponible dans le projet qui implémente la classe runtime. Lorsque vous travaillez dans un projet qui implémente des classes runtime (un projet de composant Windows Runtime ou un projet qui utilise l’interface utilisateur XAML), il est important d’être à l’aise avec la distinction entre votre type d’implémentation pour une classe runtime et le type projeté qui représente la classe runtime projetée en C++/WinRT. Pour obtenir plus d’informations, voir [Créer des API avec C++/WinRT](author-apis.md).

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>Ai-je besoin de déclarer un constructeur dans le fichier IDL de ma classe runtime ?
Uniquement si la classe runtime est conçue pour être utilisée depuis l’extérieur de son unité de compilation d’implémentation (c’est un composant Windows Runtime destiné à une utilisation générale par les applications clientes Windows Runtime). Pour plus d’informations sur l’objectif et les conséquences de la déclaration d’un ou de plusieurs constructeurs dans IDL, voir [Constructeurs de classe runtime](author-apis.md#runtime-class-constructors).

## <a name="why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error"></a>Pourquoi l’éditeur de liens me donne une « erreur LNK2019 : Erreur de symbole externe non résolu » ?
Si le symbole non résolu est une API des en-têtes d’espace de noms Windows pour la projection C++/WinRT (dans l'espace de noms **winrt**), l’API est déclarée en avance dans un en-tête que vous avez inclus, mais sa définition se trouve dans un en-tête que vous n’avez pas encore inclus. Incluez l’en-tête nommé pour l’espace de noms de l’API et régénérez. Pour plus d’informations, voir [en-têtes de projection C++/WinRT](consume-apis.md#cwinrt-projection-headers).

Si le symbole non résolu est une fonction gratuite de Windows Runtime, tels que [RoInitialize](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roinitialize), vous devez lier explicitement le [WindowsApp.lib](/uwp/win32-and-com/win32-apis) bibliothèque PARAPLUIE dans votre projet. La projection C++/WinRT dépend de certains de ces points d’entrée et fonctions libres (non-membres). Si vous utilisez un des modèles de projet [extension Visual Studio (VSIX) C++/WinRT](https://aka.ms/cppwinrt/vsix) pour votre application, `WindowsApp.lib` est lié automatiquement pour vous. Dans le cas contraire, vous pouvez utiliser des paramètres de lien entre projets pour l'inclure, ou le faire dans le code source.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

Il est important que vous résolvez toutes les erreurs de l’éditeur de liens que vous pouvez en liant **WindowsApp.lib** au lieu d’une autre bibliothèque de liaison statique, sinon votre application ne sont pas transmettre le [Kit de Certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md) tests utilisées par Visual Studio et par le Microsoft Store pour valider les soumissions (ce qui signifie qu’il par conséquent ne sera pas possible pour votre application à être ingéré avec succès par le Microsoft Store).

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>Dois-je implémenter [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable) et, si tel est le cas, comment ?
Si vous disposez d’une classe runtime qui libère les ressources dans son destructeur, et que cette classe runtime est conçue pour être utilisée depuis l’extérieur de son unité de compilation d’implémentation (c’est un composant Windows Runtime destiné à une utilisation générale par les applications clientes Windows Runtime), nous vous recommandons d’implémenter également **IClosable** pour prendre en charge l’utilisation de votre classe runtime par les langues qui ne possèdent pas de finalisation déterministe. Assurez-vous que vos ressources sont libérées si le destructeur, [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close) ou les deux sont appelés. **IClosable::Close** peut être appelé un nombre de fois arbitraire.

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>Ai-je besoin d’appeler [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close) sur les classes runtime que j’utilise ?
**IClosable** existe pour prendre en charge les langues qui ne possèdent pas de finalisation déterministe. Par conséquent, vous ne devez pas appeler **IClosable::Close** à partir de C++/WinRT, sauf dans de très rares cas impliquant des concurrences de fermeture ou des étreintes semi-fatales (semi-deadly embraces). Si vous utilisez des types **Windows.UI.Composition**, par exemple, vous pouvez rencontrer des cas où vous souhaiterez disposer des objets dans une séquence définie, au lieu de laisser la destruction du wrapper C++/WinRT faire le travail pour vous.

## <a name="can-i-use-llvmclang-to-compile-with-cwinrt"></a>Puis-je utiliser LLVM/Clang pour compiler avec C++/WinRT ?
Nous ne prenons pas en charge la chaîne d’outils LLVM et Clang pour C++/WinRT, mais nous l'utilisons en interne pour valider la conformité aux normes de C++/WinRT. Par exemple, si vous souhaitez émuler ce que nous faisons en interne, vous pouvez essayer une expérience telle que celle décrite ci-dessous.

Accédez à la [LLVM Download Page](https://releases.llvm.org/download.html), recherchez **Download LLVM 6.0.0** > **Pre-Built Binaries**et téléchargez **Clang for Windows (64-bit)** . Pendant l’installation, choisissez d’ajouter LLVM à la variable système PATH afin de pouvoir l'appeler à partir d’une invite de commandes. Dans le cadre de cette expérience, vous pouvez ignorer les erreurs « Failed to find MSBuild toolsets directory » (Impossible de trouver le répertoire des jeux d’outils MSBuild) ou MSVC integration install failed (Échec d'installation de l’intégration MSVC), si vous les voyez. Il existe plusieurs façons d’appeler LLVM/Clang ; l’exemple ci-dessous illustre une seule méthode.

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

Visual Studio est l’outil de développement que nous prenons en charge et recommandons pour C++/WinRT. Consultez [prise en charge de Visual Studio pour C / c++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="why-doesnt-the-generated-implementation-function-for-a-read-only-property-have-the-const-qualifier"></a>Pourquoi n’a pas la fonction d’implémentation généré pour une propriété en lecture seule le `const` qualificateur ?
Lorsque vous déclarez une propriété en lecture seule dans [MIDL 3.0](/uwp/midl-3/), vous pourriez vous attendre le `cppwinrt.exe` outil pour générer une fonction de mise en œuvre pour vous `const`-qualifié (const fonction traite les *cette* pointeur en tant que const).

Nous recommandons certainement à l’aide de const autant que possible, mais la `cppwinrt.exe` outil lui-même ne tente pas de raison sur quelle implémentation fonctions peuvent parfaitement être constantes, et qui ne peut pas. Vous pouvez choisir d’effectuer l’un de vos fonctions de mise en œuvre const, comme dans cet exemple.

```cppwinrt
struct MyStringable : winrt::implements<MyStringable, winrt::Windows::Foundation::IStringable>
{
    winrt::hstring ToString() const
    {
        return L"MyStringable";
    }
};
```

Vous pouvez retirer cet `const` qualificateur sur **ToString** si vous décidez que vous avez besoin de modifier un état de l’objet dans son implémentation. Mais chacune de vos membres fonctions constantes ou non-const, pas les deux. En d’autres termes, ne pas surcharger d’une fonction de mise en œuvre sur `const`.

À part de vos fonctions d’implémentation, une autre autre lieu const est fourni dans l’image se trouve dans les projections de fonction Windows Runtime. Prenons le code.

```cppwinrt
int main()
{
    winrt::Windows::Foundation::IStringable s{ winrt::make<MyStringable>() };
    auto result{ s.ToString() };
}
```

Pour l’appel à **ToString** ci-dessus, le **atteindre la déclaration** commande dans Visual Studio montre que la projection du Runtime Windows **IStringable::ToString** dans C++ / c++ / WinRT ressemble à ceci.

```cppwinrt
winrt::hstring ToString() const;
```

Fonctions sur la projection sont de type const, quel que soit le mode que vous choisissez de qualifier votre implémentation d’eux. Dans les coulisses, la projection appelle interface binaire d’application (ABI), qui équivaut à un appel via un pointeur d’interface COM. Le seul état qui le projetée **ToString** interagit avec ce pointeur d’interface COM ; et elle n’est certainement aucun nécessaire de modifier ce pointeur, par conséquent, la fonction est const. Cela vous donne l’assurance qu’il ne changera pas quoi que ce soit sur le **IStringable** référence que vous appelez via, et elle garantit que vous pouvez appeler **ToString** même avec une référence const à un  **IStringable**.

Comprendre que ces exemples de `const` sont des détails d’implémentation de C++ / c++ / WinRT projections et les implémentations ; ils constituent l’hygiène de code pour votre bénéfice. Il n’existe aucun `const` sur le COM ni l’ABI de Runtime Windows (pour les fonctions membres).

## <a name="do-you-have-any-recommendations-for-decreasing-the-code-size-for-cwinrt-binaries"></a>Vous avez aucune recommandation pour diminuer la taille du code pour C / c++ / WinRT binaires ?
Lorsque vous travaillez avec des objets Windows Runtime, vous devez éviter le modèle de codage indiqué ci-dessous, car il peut avoir un impact négatif sur votre application en provoquant la plus de code binaire que nécessaire pour être généré.

```cppwinrt
anobject.b().c().d();
anobject.b().c().e();
anobject.b().c().f();
```

Dans le monde de Windows Runtime, le compilateur ne peut pas mettre en cache soit la valeur de `c()` ou les interfaces pour chaque méthode est appelée via une indirection («. »). Sauf si vous intervenez, cela entraîne plusieurs appels virtuels et une surcharge de comptage de références. Le modèle ci-dessus pourrait facilement générer deux fois plus code comme cela est strictement nécessaire. Au lieu de cela, préférez le modèle illustré ci-dessous, là où vous le pouvez. Il génère beaucoup moins de code, et elle peut également considérablement améliorer les performances de votre exécution.

```cppwinrt
auto a{ anobject.b().c() };
a.d();
a.e();
a.f();
```

Le modèle recommandé ci-dessus s’applique non seulement pour C++ / c++ / WinRT, mais à toutes les projections de langage Windows Runtime.

## <a name="how-do-i-turn-a-string-into-a-typemdashfor-navigation-for-example"></a>Comment convertir une chaîne en type&mdash;pour la navigation, par exemple ?
À la fin de la [exemple de code de mode de Navigation](/windows/uwp/design/controls-and-patterns/navigationview#code-example) (qui est essentiellement dans C#), il existe un C + c++ / WinRT extrait de code montrant comment effectuer cette opération.

> [!NOTE]
> Si cette rubrique n’a pas répondu à votre question, vous pouvez rechercher une aide en vous rendant sur le [Communauté des développeurs C++ de Visual Studio](https://developercommunity.visualstudio.com/spaces/62/index.html), ou à l’aide de la [ `c++-winrt` balise sur Stack Overflow](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt).
