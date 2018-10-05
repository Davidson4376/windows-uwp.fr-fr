---
author: stevewhims
description: Pour vous aider à utiliser rapidement C++/WinRT, cette rubrique présente un exemple simple de code.
title: Prise en main de C++/WinRT
ms.author: stwhi
ms.date: 09/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, standard, c++, cpp, winrt, projection, utiliser rapidement, prise en main
ms.localizationpriority: medium
ms.openlocfilehash: b5954aa8236a9abeee6e5c74a200f77fcccf97e3
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4384345"
---
# <a name="get-started-with-cwinrt"></a>Prise en main de C++/WinRT
Pour vous aider à rapidement avec à l’aide de [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), cette rubrique présente un exemple de code simple.

## <a name="a-cwinrt-quick-start"></a>Démarrage rapide avec C++/WinRT
> [!NOTE]
> Pour plus d’informations sur l'installation et l'utilisation de l'extension Visual Studio (VSIX) C++/WinRT (qui fournit la prise en charge des modèles de projet, ainsi que les propriétés et cibles MSBuild C++/WinRT), voir [Prise en charge de Visual Studio de C++/WinRT et VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

Créez un nouveau projet **Windows Console Application (C++/WinRT)**.

> [!IMPORTANT]
> Si vous utilisez Visual Studio 2017 (version 15.8.0 ou une version ultérieure) et vous ciblez le SDK Windows version 10.0.17134.0 (Windows 10, version 1803), puis un nouvellement créé C + / WinRT projet peut échouer compiler avec l’erreur «*erreur C3861: 'from_abi': identificateur pas trouvé*» et à d’autres erreurs dans *base.h*d’origine. La solution consiste à soit cible une version ultérieure (conforme plus) version du SDK Windows ou de la propriété de projet de jeu **C/C++** > **langue** > **Conformance mode: N°** (en outre, si **/ permissive-** s’affiche dans la propriété de projet ** C/C++** > **langue** > de**ligne de commande** sous **Options supplémentaires**, puis supprimez).

Modifiez `pch.h` et `main.cpp` pour qu’ils ressemblent à ce qui suit.

```cppwinrt
// pch.h
...
#include <iostream>
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
...
```

```cppwinrt
// main.cpp
#include "pch.h"

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

int main()
{
    winrt::init_apartment();

    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
    for (const SyndicationItem syndicationItem : syndicationFeed.Items())
    {
        winrt::hstring titleAsHstring = syndicationItem.Title().Text();
        std::wcout << titleAsHstring.c_str() << std::endl;
    }
}
```

Nous allons prendre l’exemple de code court ci-dessus élément par élément et expliquer ce qui se passe dans chaque partie.

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
```

Les en-têtes que nous incluons font partie du SDK, présent dans le dossier `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt`. Visual Studio inclut ce chemin d’accès dans sa macro *IncludePath*. Les en-têtes contiennent des API Windows projetées en C++/WinRT. En d’autres termes, pour chaque type Windows, C++/WinRT définit un équivalent compatible en C++ (appelé le *type projeté*). Le type projeté a le même nom complet que le type Windows, mais il est placé dans l'espace de noms C++ **winrt**. Placer ces inclusions dans votre en-tête précompilé permet de réduire les temps de builds incrémentielles.

> [!IMPORTANT]
> Chaque fois que vous souhaitez utiliser un type à partir d’un espace de noms Windows, incluez le fichier d'en-tête de l'espace de noms Windows C++/WinRT, comme indiqué. L'en-tête *correspondant* est celui qui a le même nom que l’espace de noms du type. Par exemple, pour utiliser la projection C++/WinRT pour la classe runtime [**Windows::Foundation::Collections::PropertySet**](/uwp/api/windows.foundation.collections.propertyset), `#include <winrt/Windows.Foundation.Collections.h>`.

```cppwinrt
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;
```

Les directives `using namespace` sont facultatives, mais pratiques. Le modèle présenté ci-dessus pour ces directives (qui permet la recherche de nom non qualifié pour tous les éléments dans l'espace de noms **winrt**) convient pour commencer un nouveau projet et C++/WinRT est la seule projection de langage que vous utilisez à l’intérieur de ce projet. Si, en revanche, vous êtes mélangez du code C++/WinRT avec du code [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) et/ou du code de l’interface binaire d’application (ABI) du SDK (dans le cadre d'un portage à partir de ou d'une interaction avec un de ces modèles ou les deux), consultez les rubriques [interopérabilité entre C++/WinRT et C++/CX](interop-winrt-cx.md), [Passer de C++/CX à C++/WinRT](move-to-winrt-from-cx.md) et [interopérabilité entre C++/WinRT et ABI](interop-winrt-abi.md).

```cppwinrt
winrt::init_apartment();
```

L’appel à **winrt::init_apartment** initialise COM; par défaut, en mode de cloisonnement multi-thread.

```cppwinrt
Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
SyndicationClient syndicationClient;
```

Empilez-allouez deux objets: ils représentent l’uri du blog Windows et un client de syndication. Nous construisons l’uri avec un littéral de chaîne étendue simple (voir [Gestion des chaînes en C++/WinRT](strings.md) pour obtenir d’autres exemples d’utilisation des chaînes).

```cppwinrt
SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
```

[**SyndicationClient::RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) est un exemple d’une fonction Windows Runtime asynchrone. L’exemple de code reçoit un objet d’opération asynchrone à partir de **RetrieveFeedAsync** et appelle **get** sur cet objet pour bloquer le thread appelant et attendre le résultat (qui un flux de syndication, dans ce cas). Pour plus d’informations sur la concurrence et les techniques non bloquantes, voir [Opérations concurrentes et asynchrones avec C++/WinRT](concurrency.md).

```cppwinrt
for (const SyndicationItem syndicationItem : syndicationFeed.Items()) { ... }
```

[**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items) est une étendue définie par les itérateurs retournés par les fonctions **begin** et **end** (ou leurs variantes constant, reverse et constant-reverse). Pour cette raison, vous pouvez énumérer des **Items** avec une instruction `for` basée sur l’étendue ou avec la fonction modèle **std::for_each**.

```cppwinrt
winrt::hstring titleAsHstring = syndicationItem.Title().Text();
std::wcout << titleAsHstring.c_str() << std::endl;
```

Obtient le texte du titre du flux, en tant qu’objet [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) (plus de détails dans [Gestion des chaînes en C++/WinRT](strings.md)). Le **hstring** est ensuite généré, via la fonction **c_str**, qui reflète le modèle utilisé avec des chaînes de la bibliothèque C++ Standard.

Comme vous pouvez le constater, C++/WinRT encourage les expressions C++ modernes de type classe, telles que `syndicationItem.Title().Text()`. Il s’agit d’un style de programmation plus fluide et différent de la programmation COM classique. Vous n’avez pas besoin d’initialiser directement COM, ni d’utiliser des pointeurs COM.

Vous n'avez pas non plus besoin de gérer les codes de retour HRESULT. C++/WinRT convertit les HRESULT d’erreur en exceptions telles que [**winrt::hresult-error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) pour un style de programmation naturel et moderne. Pour plus d’informations sur la gestion des erreurs et obtenir des exemples de code, voir [Gestion des erreurs avec C++/WinRT](error-handling.md).

## <a name="important-apis"></a>API importantes
* [Méthode SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [Propriété SyndicationFeed.Items](/uwp/api/windows.web.syndication.syndicationfeed.items)
* [Structure winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [structure d’erreur HRESULT](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)

## <a name="related-topics"></a>Rubriquesassociées
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Gestion des erreurs avec C++/WinRT](error-handling.md)
* [Interopérabilité entre C++/WinRT et C++/CX](interop-winrt-cx.md)
* [Interopérabilité entre C++/WinRT et ABI](interop-winrt-abi.md)
* [Passer de C++/CX à C++/WinRT](move-to-winrt-from-cx.md)
* [Gestion des chaînes en C++/WinRT](strings.md)
