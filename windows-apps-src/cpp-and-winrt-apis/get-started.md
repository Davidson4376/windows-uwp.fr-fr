---
description: Pour vous aider à utiliser rapidement C++/WinRT, cette rubrique présente un exemple simple de code.
title: Prise en main de C++/WinRT
ms.date: 04/18/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, utiliser rapidement, prise en main
ms.localizationpriority: medium
ms.openlocfilehash: 64104124a6342da3f6963c61bafc871838fd00f6
ms.sourcegitcommit: 1f39b67f2711b96c6b4e7ed7107a9a47127d4e8f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66721676"
---
# <a name="get-started-with-cwinrt"></a>Prise en main de C++/WinRT

Pour vous aider à rapidement à l’utilisation de [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), cette rubrique présente un exemple de code simple basé sur un nouveau **Application de Console Windows (C++ / c++ / WinRT)** projet. Cette rubrique montre également comment [ajouter C + c++ / WinRT le support pour un projet d’application Windows Desktop](#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

> [!NOTE]
> Bien que nous vous recommandons de développer avec les dernières versions de Visual Studio et le Kit de développement Windows, si vous utilisez Visual Studio 2017 (version 15.8.0 ou une version ultérieure) et ciblant le SDK Windows version 10.0.17134.0 (Windows 10, version 1803), puis un C++/WinRT projet risque de ne pas compiler avec l’erreur «*erreur C3861 : 'from_abi' : identificateur introuvable*» et à d’autres erreurs provenant de *base.h*. La solution consiste à une cible une version ultérieure (conforme plus) version du Kit de développement logiciel Windows ou de la propriété de projet de jeu **C/C++**  > **langage** > **mode de conformité : Ne** (en outre, si **/ permissive-** apparaît dans la propriété de projet **C/C++**  > **langage** > **ligne de commande**  sous **des Options supplémentaires**, puis le supprimer).

## <a name="a-cwinrt-quick-start"></a>Démarrage rapide avec C++/WinRT

> [!NOTE]
> Pour plus d’informations sur l’installation et à l’aide de la C++Extension WinRT Visual Studio (VSIX) et le package NuGet (qui ensemble fournissent le modèle de projet et créez prise en charge), consultez [prise en charge de Visual Studio pour C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Créez un nouveau projet **Windows Console Application (C++/WinRT)** .

Modifiez `pch.h` et `main.cpp` pour qu’ils ressemblent à ce qui suit.

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>
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
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>
```

Avec les paramètres de projet par défaut, les en-têtes inclus sont fournis à partir du SDK Windows, dans le dossier`%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt`. Visual Studio inclut ce chemin d’accès dans sa macro *IncludePath*. Mais il n’existe aucune dépendance strict sur le Kit de développement Windows, étant donné que votre projet (via le `cppwinrt.exe` outil) génère ces mêmes en-têtes dans votre projet *$(GeneratedFilesDir)* dossier. Ils seront chargés à partir de ce dossier si elles ne peuvent pas être trouvées ailleurs, ou si vous modifiez les paramètres de votre projet.

Les en-têtes contiennent des API Windows projetées en C++/WinRT. En d’autres termes, pour chaque type Windows, C++/WinRT définit un équivalent compatible en C++ (appelé le *type projeté*). Le type projeté a le même nom complet que le type Windows, mais il est placé dans l'espace de noms C++ **winrt**. Placer ces inclusions dans votre en-tête précompilé permet de réduire les temps de builds incrémentielles.

> [!IMPORTANT]
> Chaque fois que vous souhaitez utiliser un type à partir d’un espaces de noms Windows, incluent le correspondantes C++fichier d’en-tête espace de noms WinRT Windows, comme indiqué ci-dessus. L'en-tête *correspondant* est celui qui a le même nom que l’espace de noms du type. Par exemple, pour utiliser la projection C++/WinRT pour la classe runtime [**Windows::Foundation::Collections::PropertySet**](/uwp/api/windows.foundation.collections.propertyset), `#include <winrt/Windows.Foundation.Collections.h>`. Si vous incluez `winrt/Windows.Foundation.Collections.h`, alors vous n’avez pas *également* devons inclure `winrt/Windows.Foundation.h`. Chaque C++/en-tête de projection WinRT inclut automatiquement son fichier d’en-tête espace de noms parent ; Pour vous éviter *devez* inclure explicitement. Cependant, si vous le faites, cela ne produira pas d'erreur.

```cppwinrt
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;
```

Les directives `using namespace` sont facultatives, mais pratiques. Le modèle présenté ci-dessus pour ces directives (qui permet la recherche de nom non qualifié pour tous les éléments dans l'espace de noms **winrt**) convient pour commencer un nouveau projet et C++/WinRT est la seule projection de langage que vous utilisez à l’intérieur de ce projet. Si, en revanche, vous êtes mélangez du code C++/WinRT avec du code [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) et/ou du code de l’interface binaire d’application (ABI) du SDK (dans le cadre d'un portage à partir de ou d'une interaction avec un de ces modèles ou les deux), consultez les rubriques [interopérabilité entre C++/WinRT et C++/CX](interop-winrt-cx.md), [Passer de C++/CX à C++/WinRT](move-to-winrt-from-cx.md) et [interopérabilité entre C++/WinRT et ABI](interop-winrt-abi.md).

```cppwinrt
winrt::init_apartment();
```

L’appel à **winrt::init_apartment** initialise le thread dans le Runtime Windows ; par défaut, dans un multithread cloisonné. L’appel initialise également COM.

```cppwinrt
Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
SyndicationClient syndicationClient;
```

Empilez-allouez deux objets : ils représentent l’uri du blog Windows et un client de syndication. Nous construisons l’uri avec un littéral de chaîne étendue simple (voir [Gestion des chaînes en C++/WinRT](strings.md) pour obtenir d’autres exemples d’utilisation des chaînes).

```cppwinrt
SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
```

[**SyndicationClient::RetrieveFeedAsync** ](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) est un exemple d’une fonction Windows Runtime asynchrone. L’exemple de code reçoit un objet d’opération asynchrone à partir de **RetrieveFeedAsync** et appelle **get** sur cet objet pour bloquer le thread appelant et attendre le résultat (qui un flux de syndication, dans ce cas). Pour plus d’informations sur la concurrence et les techniques non bloquantes, voir [Opérations concurrentes et asynchrones avec C++/WinRT](concurrency.md).

```cppwinrt
for (const SyndicationItem syndicationItem : syndicationFeed.Items()) { ... }
```

[**SyndicationFeed.Items** ](/uwp/api/windows.web.syndication.syndicationfeed.items) est une plage définie par les itérateurs retournés à partir de **commencer** et **fin** fonctions (ou leurs variantes constante inverses et constante-inverse). Pour cette raison, vous pouvez énumérer des **Items** avec une instruction `for` basée sur l’étendue ou avec la fonction modèle **std::for_each**. Chaque fois que vous effectuer une itération sur une collection de Windows Runtime comme suit, vous allez devoir `#include <winrt/Windows.Foundation.Collections.h>`.

```cppwinrt
winrt::hstring titleAsHstring = syndicationItem.Title().Text();
std::wcout << titleAsHstring.c_str() << std::endl;
```

Obtient le texte du titre du flux, en tant qu’objet [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) (plus de détails dans [Gestion des chaînes en C++/WinRT](strings.md)). Le **hstring** est ensuite généré, via la fonction **c_str**, qui reflète le modèle utilisé avec des chaînes de la bibliothèque C++ Standard.

Comme vous pouvez le constater, C++/WinRT encourage les expressions C++ modernes de type classe, telles que `syndicationItem.Title().Text()`. Il s’agit d’un style de programmation plus fluide et différent de la programmation COM classique. Vous n’avez pas besoin d’initialiser directement COM, ni d’utiliser des pointeurs COM.

Vous n'avez pas non plus besoin de gérer les codes de retour HRESULT. C++/WinRT convertit les HRESULT d’erreur en exceptions telles que [**winrt::hresult-error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) pour un style de programmation naturel et moderne. Pour plus d’informations sur la gestion des erreurs et obtenir des exemples de code, voir [Gestion des erreurs avec C++/WinRT](error-handling.md).

## <a name="modify-a-windows-desktop-application-project-to-add-cwinrt-support"></a>Modifier un projet d’application de bureau de Windows pour ajouter C + c++ / WinRT support

Cette section vous montre comment vous pouvez ajouter C + c++ / WinRT le support pour un projet d’application de bureau de Windows dont vous disposez. Si vous n’avez pas un projet d’application de bureau de Windows, vous pouvez suivre avec ces étapes en premier une création. Par exemple, ouvrez Visual Studio et créez un **Visual C++** \> **Windows Desktop** \> **Application de bureau Windows** projet.

Vous pouvez éventuellement installer le [ C++Extension WinRT Visual Studio (VSIX)](https://aka.ms/cppwinrt/vsix) et le package NuGet. Pour plus d’informations, consultez [prise en charge de Visual Studio pour C / c++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

### <a name="set-project-properties"></a>Définir les propriétés de projet

Accédez à la propriété de projet **général** \> **Windows SDK Version**, puis sélectionnez **toutes les Configurations** et **toutes les plateformes**. Vérifiez que **Windows SDK Version** est définie sur 10.0.17134.0 (Windows 10, version 1803) ou supérieur.

Vérifiez que vous n’êtes pas affecté par [Pourquoi mon nouveau projet n’est pas compilé ?](/windows/uwp/cpp-and-winrt-apis/faq).

Étant donné que C++ / c++ / WinRT utilise des fonctionnalités à partir de la propriété 17 projet standard, affectez la valeur C ++ **C/C++**  > **langage** > **norme du langage C++** à *Norme ISO C ++ 17 (/ std : c ++ 17)* .

### <a name="the-precompiled-header"></a>L’en-tête précompilé

Le modèle de projet par défaut crée un en-tête précompilé, nommé `framework.h`, ou `stdafx.h`. Pour renommer `pch.h`. Si vous avez un `stdafx.cpp` de fichier, puis à renommer `pch.cpp`. Définir la propriété de projet **C /C++**  > **en-têtes précompilés** > **en-tête précompilé** à *créer (/Yc)* , et **fichier d’en-tête précompilé** à *pch.h*.

Rechercher et remplacer toutes les `#include "framework.h"` (ou `#include "stdafx.h"`) avec `#include "pch.h"`.

Dans `pch.h`, inclure `winrt/base.h`.

```cppwinrt
// pch.h
...
#include <winrt/base.h>
```

### <a name="linking"></a>Liaison

C++ / c++ / projection de langage WinRT dépend de certaines fonctions (tiers) gratuites de Windows Runtime et les points d’entrée, nécessitant une liaison à la [WindowsApp.lib](/uwp/win32-and-com/win32-apis) bibliothèque de PARAPLUIE. Cette section décrit trois manières de satisfaire l’éditeur de liens.

La première option consiste à ajouter à votre Visual Studio tous C++ / projet c++ / WinRT MSBuild propriétés et cibles. Pour ce faire, vous devez installer le [Microsoft.Windows.CppWinRT NuGet package](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) dans votre projet. Ouvrez le projet dans Visual Studio, cliquez sur **projet** \> **gérer les Packages NuGet...** \> **Parcourir**, tapez ou collez **Microsoft.Windows.CppWinRT** dans la zone de recherche, sélectionnez l’élément dans les résultats de recherche, puis cliquez sur **installer** pour installer le package pour ce projet.

Vous pouvez également utiliser des paramètres de lien de projet pour une liaison explicite `WindowsApp.lib`. Ou, vous pouvez le faire dans le code source (dans `pch.h`, par exemple) comme suit.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

Vous pouvez maintenant compiler et lier et ajouter C++code /WinRT à votre projet (par exemple, le code similaire à celle illustrée dans la [A C++rapide /WinRT](#a-cwinrt-quick-start) section ci-dessus).

## <a name="important-apis"></a>API importantes
* [SyndicationClient::RetrieveFeedAsync (méthode)](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [Propriété de SyndicationFeed.Items](/uwp/api/windows.web.syndication.syndicationfeed.items)
* [WinRT::hstring struct](/uwp/cpp-ref-for-winrt/hstring)
* [Erreur de WinRT::HRESULT struct](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)

## <a name="related-topics"></a>Rubriques connexes
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Gestion des erreurs avec C++/WinRT](error-handling.md)
* [Interopérabilité entre C++/WinRT et C++/CX](interop-winrt-cx.md)
* [Interopérabilité entre C++/WinRT et ABI](interop-winrt-abi.md)
* [Passer de C++/CX à C++/WinRT](move-to-winrt-from-cx.md)
* [Chaîne gère en C / c++ / WinRT](strings.md)
