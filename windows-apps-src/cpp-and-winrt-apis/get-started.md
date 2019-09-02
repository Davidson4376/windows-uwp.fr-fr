---
description: Pour vous aider à utiliser rapidement C++/WinRT, cette rubrique présente un exemple simple de code.
title: Bien démarrer avec C++/WinRT
ms.date: 04/18/2019
ms.topic: article
keywords: windows 10, uwp, standard, norme, c++, cpp, winrt, projection, bien démarrer, prise en main
ms.localizationpriority: medium
ms.openlocfilehash: e900cdddaa86857554c11605d00f59fd45a7e5ee
ms.sourcegitcommit: b186f70f0b1601b6b6a5db91d87d034f6b155e3b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70063784"
---
# <a name="get-started-with-cwinrt"></a>Bien démarrer avec C++/WinRT

Pour vous aider à vous familiariser avec [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), cette rubrique présente un exemple de code simple basé sur un nouveau projet **Application console Windows (C++/WinRT)** . Cette rubrique montre également comment [ajouter la prise en charge de C++/WinRT à un projet d’application de bureau Windows](#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

> [!NOTE]
> Nous vous recommandons de développer à l’aide des dernières versions de Visual Studio et du kit SDK Windows. Toutefois, si vous utilisez Visual Studio 2017 (version 15.8.0 ou ultérieure) et si vous ciblez le kit SDK Windows version 10.0.17134.0 (Windows 10, version 1803), la compilation d’un projet C++/WinRT créé risque d’être un échec. Vous verrez notamment s’afficher l’« *erreur C3861 : 'from_abi' : identificateur introuvable* » et d’autres erreurs provenant de *base.h*. La solution consiste à cibler une version ultérieure (plus conforme) du kit SDK Windows, ou à définir la propriété de projet **C/C++**  > **Langage** > **Mode de conformité : Non** (de plus, si **/permissive-** apparaît dans la propriété de projet **C/C++**  > **Langage** > **Ligne de commande** sous **Options supplémentaires**, supprimez-la).

## <a name="a-cwinrt-quick-start"></a>Démarrage rapide avec C++/WinRT

> [!NOTE]
> Pour plus d’informations sur la configuration du développement Visual Studio pour C++/WinRT&mdash;notamment l’installation et l’utilisation de l’extension VSIX (Visual Studio Extension) C++/WinRT et du package NuGet (qui fournissent ensemble la prise en charge des modèles et des builds de projet)&mdash;, consultez [Prise en charge de Visual Studio pour C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Créez un projet **Application console Windows (C++/WinRT)** .

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

Nous allons utiliser le court exemple de code ci-dessus, partie par partie, pour expliquer ce qui se passe dans chaque partie.

```cppwinrt
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>
```

Avec les paramètres de projet par défaut, les en-têtes inclus proviennent du kit SDK Windows, situé dans le dossier `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt`. Visual Studio inclut ce chemin dans sa macro *IncludePath*. Toutefois, il n’existe aucune dépendance stricte au kit SDK Windows, car votre projet (via l’outil `cppwinrt.exe`) génère les mêmes en-têtes dans le dossier *$(GeneratedFilesDir)* du projet. Ils sont chargés à partir de ce dossier s’ils sont introuvables ailleurs, ou si vous changez vos paramètres de projet.

Les en-têtes contiennent les API Windows projetées dans C++/WinRT. En d’autres termes, pour chaque type Windows, C++/WinRT définit un équivalent compatible en C++ (appelé *type projeté*). Un type projeté a le même nom complet que le type Windows, mais il est placé dans l’espace de noms C++ **winrt**. Le fait de placer ces inclusions dans votre en-tête précompilé permet de réduire les délais de builds incrémentielles.

> [!IMPORTANT]
> Chaque fois que vous souhaitez utiliser un type à partir d’un espace de noms Windows, incluez le fichier d’en-tête d’espace de noms Windows C++/WinRT, comme indiqué ci-dessus. L’en-tête *correspondant* est celui qui a le même nom que l’espace de noms du type. Par exemple, pour utiliser la projection C++/WinRT pour la classe runtime [**Windows::Foundation::Collections::PropertySet**](/uwp/api/windows.foundation.collections.propertyset), `#include <winrt/Windows.Foundation.Collections.h>`. Si vous incluez `winrt/Windows.Foundation.Collections.h`, vous n’avez pas *également* à inclure `winrt/Windows.Foundation.h`. Chaque en-tête de projection C++/WinRT inclut automatiquement son fichier d’en-tête d’espace de noms parent. Vous n’avez donc pas *besoin* de l’inclure explicitement. Toutefois, si vous le faites, aucune erreur n’est générée.

```cppwinrt
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;
```

Les directives `using namespace` sont facultatives, mais bien pratiques. Le modèle présenté ci-dessus pour ces directives (qui permet la recherche de nom non qualifié dans l’espace de noms **winrt**) convient pour commencer un nouveau projet. C++/WinRT est la seule projection de langage que vous utilisez dans ce projet. Si, en revanche, vous mélangez du code C++/WinRT à du code [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) et/ou du code ABI (interface binaire d’application) du kit SDK (dans le cadre d’un portage ou d’une interaction basée sur l’un de ces modèles ou les deux), consultez les rubriques [Interopérabilité entre C++/WinRT et C++/CX](interop-winrt-cx.md), [Passer de C++/CX à C++/WinRT](move-to-winrt-from-cx.md) et [Interopérabilité entre C++/WinRT et ABI](interop-winrt-abi.md).

```cppwinrt
winrt::init_apartment();
```

L’appel à **winrt::init_apartment** initialise le thread dans Windows Runtime, notamment dans un multithread cloisonné par défaut. L’appel initialise également COM.

```cppwinrt
Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
SyndicationClient syndicationClient;
```

Effectuez une allocation par pile de deux objets : ils représentent l’URI du blog Windows et un client de syndication. Nous construisons l’URI avec un simple littéral de chaîne étendue (consultez [Gestion des chaînes en C++/WinRT](strings.md) pour obtenir d’autres exemples d’utilisation des chaînes).

```cppwinrt
SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
```

[**SyndicationClient::RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) est un exemple de fonction Windows Runtime asynchrone. L’exemple de code reçoit un objet d’opération asynchrone à partir de **RetrieveFeedAsync** et appelle **get** sur cet objet pour bloquer le thread appelant et attendre le résultat (qui, dans le cas présent, est un flux de syndication). Pour plus d’informations sur la concurrence et les techniques non bloquantes, consultez [Opérations concurrentes et asynchrones avec C++/WinRT](concurrency.md).

```cppwinrt
for (const SyndicationItem syndicationItem : syndicationFeed.Items()) { ... }
```

[**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items) est une plage, définie par les itérateurs retournés par les fonctions **begin** et **end** (ou leurs variantes constant, reverse et constant-reverse). Ainsi, vous pouvez énumérer les **Items** avec une instruction `for` basée sur une plage ou avec la fonction de modèle **std::for_each**. Chaque fois que vous itérez sur une collection Windows Runtime comme celle-ci, vous devez utiliser `#include <winrt/Windows.Foundation.Collections.h>`.

```cppwinrt
winrt::hstring titleAsHstring = syndicationItem.Title().Text();
std::wcout << titleAsHstring.c_str() << std::endl;
```

Obtient le texte du titre du flux, en tant qu’objet [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) (pour plus d’informations, consultez [Gestion des chaînes en C++/WinRT](strings.md)). Le **hstring** est ensuite généré, via la fonction **c_str**, qui reflète le modèle utilisé avec les chaînes de la bibliothèque normalisée du C++.

Comme vous pouvez le constater, C++/WinRT encourage les expressions C++ modernes de type classe, par exemple `syndicationItem.Title().Text()`. Ce style de programmation est différent et plus élégant que le style de programmation COM classique. Vous n’avez pas besoin d’initialiser directement COM, ni d’utiliser des pointeurs COM.

Vous n’avez pas non plus besoin de prendre en charge les codes de retour HRESULT. C++/WinRT convertit les valeurs d’erreur HRESULT en exceptions telles que [**winrt::hresult-error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) pour un style de programmation naturel et moderne. Pour plus d’informations sur la gestion des erreurs et sur l’obtention d’exemples de code, consultez [Gestion des erreurs avec C++/WinRT](error-handling.md).

## <a name="modify-a-windows-desktop-application-project-to-add-cwinrt-support"></a>Modifier un projet d’application de bureau Windows pour ajouter la prise en charge de C++/WinRT

Cette section vous montre comment ajouter la prise en charge de C++/WinRT à un projet d’application de bureau Windows. Si vous n’avez pas de projet d’application de bureau Windows, suivez les étapes indiquées en commençant par en créer un. Par exemple, ouvrez Visual Studio, puis créez un projet via **Visual C++** \> **Windows Desktop** \> **Application de bureau Windows**.

Vous pouvez éventuellement installer l’[extension VSIX (Visual Studio Extension) C++/WinRT](https://aka.ms/cppwinrt/vsix) et le package NuGet. Pour plus d’informations, consultez [Prise en charge de Visual Studio pour C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

### <a name="set-project-properties"></a>Définir les propriétés du projet

Accédez à la propriété de projet **Général** \> **Version du SDK Windows**, puis sélectionnez **Toutes les configurations** et **Toutes les plateformes**. Vérifiez que **Version du kit SDK Windows** a la valeur 10.0.17134.0 (Windows 10 version 1803) ou une valeur correspondant à une version ultérieure.

Vérifiez que vous n’êtes pas concerné par le problème suivant : [Pourquoi la compilation de mon nouveau projet échoue-t-elle ?](/windows/uwp/cpp-and-winrt-apis/faq).

Dans la mesure où C++/WinRT utilise les fonctionnalités de la norme C++17, affectez à la propriété de projet **C/C++**  > **Langage** > **Norme du langage C++** la valeur *Norme ISO C++17 (/std:c++17)* .

### <a name="the-precompiled-header"></a>En-tête précompilé

Le modèle de projet par défaut crée automatiquement un en-tête précompilé, nommé `framework.h` ou `stdafx.h`. Renommez-le en `pch.h`. Si vous avez un fichier `stdafx.cpp`, renommez-le en `pch.cpp`. Affectez à la propriété de projet **C/C++**  > **En-têtes précompilés** > **En-tête précompilé** la valeur *Créer (/Yc)* , et à **Fichier d’en-tête précompilé** la valeur *pch.h*.

Recherchez et remplacez tous les `#include "framework.h"` (ou `#include "stdafx.h"`) par `#include "pch.h"`.

Dans `pch.h`, incluez `winrt/base.h`.

```cppwinrt
// pch.h
...
#include <winrt/base.h>
```

### <a name="linking"></a>Liaison

La projection de langage C++/WinRT dépend de certaines fonctions libres (non-membres) Windows Runtime et de certains points d’entrée qui nécessitent une liaison à la bibliothèque parapluie [WindowsApp.lib](/uwp/win32-and-com/win32-apis). Cette section décrit trois manières de satisfaire l’éditeur de liens.

La première option consiste à ajouter à votre projet Visual Studio l’ensemble des propriétés et cibles MSBuild C++/WinRT. Pour ce faire, installez le [package NuGet Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) dans votre projet. Ouvrez le projet dans Visual Studio, cliquez sur **Projet** \> **Gérer les packages NuGet** \> **Parcourir**, tapez ou collez **Microsoft.Windows.CppWinRT** dans la zone de recherche, sélectionnez l’élément dans les résultats de la recherche, puis cliquez sur **Installer** pour installer le package correspondant au projet.

Vous pouvez également utiliser les paramètres relatifs aux liens du projet pour lier explicitement `WindowsApp.lib`. Ou, vous pouvez le faire dans le code source (dans `pch.h`, par exemple) comme ceci.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

Vous pouvez désormais effectuer les opérations de compilation et d’édition des liens. Vous pouvez également ajouter du code C++/WinRT à votre projet (par exemple, du code similaire à celui présenté dans la section [Démarrage rapide avec C++/WinRT](#a-cwinrt-quick-start), ci-dessus).

## <a name="the-three-main-scenarios-for-cwinrt"></a>Les trois principaux scénarios pour C++/WinRT

Au fur et à mesure que vous vous familiarisez avec C++/WinRT et utilisez la présente documentation, vous remarquerez probablement qu’il existe trois scénarios principaux, comme décrit dans les sections suivantes.

### <a name="consuming-windows-runtime-apis-and-types"></a>Utilisation des API et des types Windows Runtime

En d’autres termes, *utilisant* ou *appelant* des API. Par exemple, en effectuant des appels d’API pour communiquer à l’aide de Bluetooth, diffuser et présenter une vidéo, intégrer avec Windows Shell, etc. C++/WinRT prend pleinement en charge cette catégorie de scénario. Pour plus d’informations, consultez [Utiliser des API avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/consume-apis).

### <a name="authoring-windows-runtime-apis-and-types"></a>Création d’API et de types Windows Runtime

En d’autres termes, *produisant* des API et des types. Par exemple, production des types d’API décrits dans la section ci-dessus, des API graphiques, des API du stockage et du système de fichiers, des API de mise en réseau, etc. Pour plus d’informations, consultez [Créer des API avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis).

La création d’API avec C++/WinRT est un peu plus complexe que leur consommation car vous devez utiliser IDL pour définir la forme de l’API avant de pouvoir l’implémenter. Pour une procédure pas à pas, consultez [Contrôles XAML ; liaison à une propriété C++/WinRT](/windows/uwp/cpp-and-winrt-apis/binding-property).

### <a name="xaml-applications"></a>Applications XAML

Ce scénario traite de la création d’applications et de contrôles sur l’infrastructure d’interface utilisateur XAML. L'utilisation d'une application XAML revient à combiner consommation et création. Mais XAML étant aujourd'hui l'infrastructure d'interface utilisateur dominante sur Windows, et son influence sur Windows Runtime étant proportionnelle, il mérite sa propre catégorie de scénario.

Sachez que XAML fonctionne mieux avec les langages de programmation qui offrent une réflexion. Dans C++/WinRT, vous pouvez être amené à effectuer quelques manipulations supplémentaires pour interagir avec l’infrastructure XAML. Ces différents cas sont traités dans la documentation. Consultez notamment [Contrôles XAML ; liaison à une propriété C++/WinRT](/windows/uwp/cpp-and-winrt-apis/binding-property) et [Contrôles XAML personnalisés (basés sur un modèle) avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/xaml-cust-ctrl).

## <a name="important-apis"></a>API importantes
* [SyndicationClient::RetrieveFeedAsync, méthode](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed.Items, propriété](/uwp/api/windows.web.syndication.syndicationfeed.items)
* [winrt::hstring, struct](/uwp/cpp-ref-for-winrt/hstring)
* [winrt::hresult-error, struct](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)

## <a name="related-topics"></a>Rubriques connexes
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Gestion des erreurs avec C++/WinRT](error-handling.md)
* [Interopérabilité entre C++/WinRT et C++/CX](interop-winrt-cx.md)
* [Interopérabilité entre C++/WinRT et ABI](interop-winrt-abi.md)
* [Passer de C++/CX à C++/WinRT](move-to-winrt-from-cx.md)
* [Gestion des chaînes en C++/WinRT](strings.md)
