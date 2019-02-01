---
description: Pour vous aider à utiliser rapidement C++/WinRT, cette rubrique présente un exemple simple de code.
title: Prise en main de C++/WinRT
ms.date: 10/19/2018
ms.topic: article
keywords: windows10, uwp, standard, c++, cpp, winrt, projection, utiliser rapidement, prise en main
ms.localizationpriority: medium
ms.openlocfilehash: c0d11a8718f61666d6285d8a1c91b48992044b22
ms.sourcegitcommit: 2d2483819957619b6de21b678caf887f3b1342af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2019
ms.locfileid: "9042351"
---
# <a name="get-started-with-cwinrt"></a>Prise en main de C++/WinRT

Pour vous aider à rapidement avec à l’aide de [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), cette rubrique présente un exemple de code simple basé sur un nouveau **Windows Console Application (C++ / WinRT)** projet. Cette rubrique montre également comment [Ajouter C++ / WinRT prise en charge pour un projet d’application de bureau Windows](#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

> [!IMPORTANT]
> Si vous utilisez Visual Studio 2017 (version 15.8.0 ou une version ultérieure) et vous ciblez le SDK Windows version 10.0.17134.0 (Windows 10, version 1803), puis un nouvellement créé C + / WinRT projet peut échouer compiler avec l’erreur «*erreur C3861: 'from_abi': identificateur pas trouvé*» et à d’autres erreurs dans *base.h*d’origine. La solution consiste à soit cible une version ultérieure (conforme plus) version du SDK Windows ou de la propriété de projet de jeu **C/C++** > **langue** > **Conformance mode: N°** (en outre, si **/ permissive-** s’affiche dans la propriété de projet ** C/C++** > **langue** > de**ligne de commande** sous **Options supplémentaires**, puis supprimez).

## <a name="a-cwinrt-quick-start"></a>Démarrage rapide avec C++/WinRT

> [!NOTE]
> Pour plus d’informations sur l’installation et à l’aide de C++ / WinRT Extension Visual Studio (VSIX) (qui fournit la prise en charge des modèles de projet) voir [prise en charge de Visual Studio pour C++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Créez un nouveau projet **Windows Console Application (C++/WinRT)**.

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

## <a name="modify-a-windows-desktop-application-project-to-add-cwinrt-support"></a>Modifier un projet d’application de bureau Windows pour ajouter C++ / WinRT support

Cette section vous montre comment vous pouvez ajouter C++ / WinRT prise en charge pour un projet d’application de bureau Windows que vous pourriez rencontrer. Si vous n’avez pas un projet d’application de bureau Windows existant, vous pouvez suivre, ainsi que les étapes suivantes en premier une création. Par exemple, ouvrez Visual Studio et créez un **Visual C++** \> **Windows Desktop** \> projet**d’Application de bureau Windows** .

Vous pouvez également installer la [C++ / WinRT Visual Studio Extension (VSIX)](https://aka.ms/cppwinrt/vsix). Pour plus d’informations, voir [prise en charge de Visual Studio pour C++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

### <a name="set-project-properties"></a>Définir les propriétés de projet

Accédez à **l’onglet Général**des propriétés du projet \> **Version du SDK Windows**et sélectionnez **Toutes les Configurations** et **Toutes les plateformes**. Vérifiez que la **Version du SDK Windows** est définie sur 10.0.17134.0 (Windows 10, version 1803) ou une version ultérieure.

Vérifiez que vous n’êtes pas affecté par [Pourquoi mon projet ne sera pas compilé?](/windows/uwp/cpp-and-winrt-apis/faq).

Étant donné que C++ / WinRT utilise les fonctionnalités de la norme C ++ 17, définissez la propriété de projet **C/C++** > **langue** > **Standard de langage C++** à *ISO C ++ 17 Standard (/ std: c ++ 17)*.

### <a name="the-precompiled-header"></a>L’en-tête précompilé

Renommez votre `stdafx.h` et `stdafx.cpp` à `pch.h` et `pch.cpp`, respectivement. Définissez la propriété de projet **C/C++** > **En-têtes précompilés** >  *pch.h***Fichier d’en-tête précompilé** .

Rechercher et remplacer tous les `#include "stdafx.h"` avec `#include "pch.h"`.

Dans `pch.h`, incluez `winrt/base.h`.

```cppwinrt
// pch.h
...
#include <winrt/base.h>
```

### <a name="linking"></a>Créer un lien

C++ / projection de langage WinRT dépend de certaines fonctions (non-membres) gratuites de Windows Runtime et les points d’entrée, nécessitent une liaison à la bibliothèque PARAPLUIE [WindowsApp.lib](/uwp/win32-and-com/win32-apis) . Cette section décrit les trois façons de satisfaire la demande l’éditeur de liens.

La première option consiste à ajouter à votre Visual Studio projet toutes C++ / WinRT MSBuild propriétés et cibles. Pour ce faire, installez le [package Microsoft.Windows.CppWinRT NuGet](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) dans votre projet. Ouvrez le projet dans Visual Studio, cliquez sur **le projet** \> **Gérer les Packages NuGet …**  \>  **Parcourir**, tapez ou collez **Microsoft.Windows.CppWinRT** dans la zone de recherche, sélectionnez l’élément dans les résultats de recherche, puis cliquez sur **installer** pour installer le package de ce projet.

Vous pouvez également utiliser les paramètres de lien de projet pour une liaison explicite `WindowsApp.lib`. Ou, vous pouvez le faire dans le code source (dans `pch.h`, par exemple) comme suit.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

Vous pouvez maintenant compiler et lier et ajouter C++ / WinRT code à votre projet (par exemple, le code affiché dans la [A les langages c++ / WinRT démarrage rapide](#a-cwinrt-quick-start) section ci-dessus)

## <a name="important-apis"></a>API importantes
* [Méthode SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [Propriété SyndicationFeed.Items](/uwp/api/windows.web.syndication.syndicationfeed.items)
* [Structure winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [structure WinRT::HRESULT-erreur](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)

## <a name="related-topics"></a>Rubriquesconnexes
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Gestion des erreurs avec C++/WinRT](error-handling.md)
* [Interopérabilité entre C++/WinRT et C++/CX](interop-winrt-cx.md)
* [Interopérabilité entre C++/WinRT et ABI](interop-winrt-abi.md)
* [Passer de C++/CX à C++/WinRT](move-to-winrt-from-cx.md)
* [Gestion des chaînes en C++/WinRT](strings.md)
