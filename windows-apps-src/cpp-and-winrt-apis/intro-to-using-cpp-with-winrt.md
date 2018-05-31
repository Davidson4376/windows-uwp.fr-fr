---
author: stevewhims
description: Présentation de C++/WinRT &mdash;une projection de langage C++ standard pour les API Windows Runtime.
title: Présentation de C++/WinRT
ms.author: stwhi
ms.date: 05/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, standard, c++, cpp, winrt, projection
ms.localizationpriority: medium
ms.openlocfilehash: 968afd6fdad1e7bf6b3c38d929ab79eefa71819a
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832323"
---
# <a name="introduction-to-cwinrt"></a>Présentation de C++/WinRT
> [!NOTE]
> **Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.**

Introduit dans la version10.0.17134.0 (Windows10, version1803), le SDK Windows inclut désormais C++/WinRT.

> [!IMPORTANT]
> Les éléments les plus importants de C++/WinRT à connaître sont décrits dans les sections [Prise en charge du Kit de développement logiciel (SDK) pour C++/WinRT](#sdk-support-for-cwinrt) et [Prise en charge de Visual Studio pour C++/WinRT et VSIX](#visual-studio-support-for-cwinrt-and-the-vsix).

C++/WinRT est une projection de langage C++17 moderne entièrement standard pour les API Windows Runtime (WinRT), implémentée uniquement dans les fichiers d’en-tête et conçue pour vous fournir un accès de première classe à l’API Windows moderne. Avec C++/WinRT, vous pouvez créer et utiliser des API Windows Runtime en utilisant n’importe quel compilateur C++17 conforme aux normes.

## <a name="language-projections"></a>Projections de langage
Windows Runtime est basé sur les API COM (Component Object Model) et est conçu pour être accessible via des *projections de langage*. Une projection masque les détails COM et fournit une expérience de programmation plus naturelle pour un langage donné.

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>La projection de langage C++/WinRT dans le contenu de référence des API UWP Windows
Lorsque vous naviguez dans des [API UWP Windows](https://docs.microsoft.com/uwp/api/), cliquez sur la zone de liste **Langage** dans l’angle supérieur droit et sélectionnez **C++/WinRT** pour afficher les blocs de syntaxe d’API tels qu’ils apparaissent dans la projection de langage C++/WinRT.

## <a name="sdk-support-for-cwinrt"></a>Prise en charge du Kit de développement logiciel (SDK) pour C++/WinRT
À compter de la version10.0.17134.0 (Windows10, version1803), le SDK Windows contient les en-têtes et outils de l'espace de nom Windows de la projection C++/WinRT. Un outil important est `cppwinrt.exe`qui, à partir d’un `.winmd`, génère des fichiers de code source qui projettent les métadonnées en C++/WinRT. Les fichiers de métadonnées Windows Runtime (`.winmd`) fournissent un moyen canonique de décrire une surface d’API Windows Runtime. À partir des métadonnées Windows Runtime, `cppwinrt.exe` génère une bibliothèque C++ standard qui décrit en détail &mdash;ou *projette*&mdash; la surface d’API, qu’il s’agisse d’API Windows ou d’API tierces de composants Windows Runtime. `Cppwinrt.exe` joue un rôle important dans votre processus de développement à la fois pour utiliser les API internes et tierces, et créer vos propres API dans des composants.

## <a name="visual-studio-support-for-cwinrt-and-the-vsix"></a>Prise en charge de Visual Studio pour C++/WinRT et VSIX
Pour les modèles de projet C++/WinRT dans Visual Studio, mais aussi les propriétés et cibles MSBuild C++/WinRT, téléchargez et installez l’[extension Visual Studio (VSIX) C++/WinRT](https://aka.ms/cppwinrt/vsix) à partir de [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

Vous aurez besoin de Visual Studio2017 version15.6 ou ultérieure et du SDK Windows version10.0.17134.0 (Windows10, version1803). Vous pourrez ensuite créer un nouveau projet dans Visual Studio, ou convertir un projet existant en ajoutant la propriété `<CppWinRTProject>true</CppWinRTProject>` dans son fichier `.vcxproj`, dans Projet>PropertyGroup. Une fois que vous avez ajouté cette propriété, vous recevrez la prise en charge MSBuild C++/WinRT pour le projet, incluant l’appel de l’outil `cppwinrt.exe`.

Étant donné que C++/WinRT utilise les fonctionnalités de la norme C++17, il a besoin de la propriété de projet **C/C++** > **Langage** > **ISO C++17 Standard (/std:c++17)**. Vous souhaiterez peut-être également définir **Conformance mode: Yes (/permissive-)**, ce qui contraint davantage votre code à être conforme aux normes.

N’oubliez pas une autre propriété de projet qui est **C/C++** > **Général** > **Treat Warnings As Errors**. Définissez ce paramètre sur **Yes(/WX)** ou **No (/WX-)** à votre convenance. Parfois, les fichiers source générés par l’outil `cppwinrt.exe` génèrent des avertissements jusqu'à ce que vous leur ajoutiez votre implémentation.

Voici les modèles de projet Visual Studio fournis par VSIX.

### <a name="windows-console-application-cwinrt"></a>Windows Console Application (C++/WinRT)
Modèle de projet pour une application cliente C+/WinRT pour Windows Desktop, avec une interface utilisateur de console.

### <a name="blank-app-cwinrt"></a>Blank App (C++/WinRT)
Modèle de projet pour une application de plateforme Windows universelle (UWP) qui possède une interface utilisateur XAML.

Visual Studio fournit la prise en charge du compilateur XAML pour générer des stubs d’implémentation et d’en-tête à partir du fichier IDL (Interface Definition Language) (`.idl`) qui se trouve derrière chaque fichier de balisage XAML. Dans un fichier IDL, définissez toutes les classes runtime locales que vous souhaitez référencer dans les pages XAML de votre application, puis générez une fois le projet pour générer les modèles d’implémentation dans `Generated Files` et les définitions de type de stub dans `Generated Files\sources`. Puis, utilisez ces définitions de type de stub comme référence pour implémenter vos classes runtime locales. Nous vous recommandons de déclarer chaque classe runtime dans son propre fichier IDL.

### <a name="core-app-cwinrt"></a>Core App (C++/WinRT)
Modèle de projet pour une application de plateforme Windows universelle (UWP) qui n’utilise pas XAML.

Au lieu de cela, elle utilise l’en-tête d’espace de nom Windows de la projection C++/WinRT pour l’espace de noms Windows.ApplicationModel.Core. Après génération et exécution, cliquez sur un espace vide pour ajouter un carré coloré; cliquez ensuite sur un carré coloré pour le faire glisser.

### <a name="windows-runtime-component-cwinrt"></a>Composant Windows Runtime (C++/WinRT)
Modèle de projet pour un composant, en règle générale pour l’utilisation depuis une plateforme Windows universelle (UWP).

Ce modèle illustre la chaîne d’outils `midl.exe` > `cppwinrt.exe`, dans laquelle les métadonnées Windows Runtime (`.winmd`) sont générées à partir du fichier IDL, puis les stubs d’implémentation et d’en-tête sont générés à partir des métadonnées Windows Runtime.

Dans un fichier IDL, définissez les classes runtime dans votre composant, leur interface par défaut et toute autre interface qu’elles implémentent. Générez une fois le projet pour générer `module.g.cpp`, `module.h.cpp`, les modèles d’implémentation dans `Generated Files` et les définitions de type de stub dans `Generated Files\sources`. Puis utilisez ces définitions de type de stub comme référence pour implémenter les classes runtime dans votre composant. Nous vous recommandons de déclarer chaque classe runtime dans son propre fichier IDL.

Regroupez le binaire du composant Windows Runtime intégré et son `.winmd` avec l'application UWP qui les utilise.

## <a name="a-cwinrt-quick-start"></a>Démarrage rapide avec C++/WinRT
Créez un nouveau projet **Windows Console Application (C++/WinRT)**. Modifiez `main.cpp` pour qu’il ressemble à ce qui suit.

```cppwinrt
// main.cpp

#include "pch.h"
#include <iostream>
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>

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
        hstring titleAsHstring = syndicationItem.Title().Text();
        std::wcout << titleAsHstring.c_str() << std::endl;
    }
}
```

Les en-têtes inclus `winrt/Windows.Foundation.h` et `winrt/Windows.Web.Syndication.h` sont dans le SDK, dans le dossier `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt\`. Visual Studio inclut ce chemin d’accès dans sa macro *IncludePath*. Les en-têtes contiennent des API Windows projetées en C++/WinRT. Chaque fois que vous souhaitez utiliser des types à partir des espaces de noms Windows, incluez les en-têtes d’espaces de noms Windows de projection C++/WinRT correspondants, comme suit. Les directives `using namespace` sont facultatives, mais pratiques.

Tous les types projetés sont dans l’espace de noms racine C++/WinRT **winrt**. [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) et le SDK Windows déclarent tous les deux les types dans l’espace de noms racine **Windows**. Ces espaces de noms distincts vous permettent de migrer de C++/CX vers C++/WinRT à votre propre rythme.

[**SyndicationClient::RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) est un exemple d’une fonction Windows Runtime asynchrone. L’exemple de code reçoit un objet d’opération asynchrone à partir de **RetrieveFeedAsync** et appelle **get** sur cet objet pour bloquer le thread appelant et attendre les résultats. Pour plus d’informations sur la concurrence et les techniques non bloquantes, voir [Opérations concurrentes et asynchrones avec C++/WinRT](concurrency.md).

[**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items) est une étendue définie par les itérateurs retournés par les fonctions **begin** et **end** (ou leurs variantes constant, reverse et constant-reverse). Pour cette raison, vous pouvez énumérer des **Items** avec une instruction `for` basée sur l’étendue ou avec la fonction modèle **std::for_each**.

Le code obtient ensuite le texte du titre du flux, en tant qu’objet [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) (voir [Gestion des chaînes en C++/WinRT](strings.md)). Le **hstring** est ensuite généré, via **c_str**, qui vous semblera familier si vous avez utilisé des chaînes de la bibliothèque C++ standard.

Comme vous pouvez le constater, C++/WinRT encourage les expressions C++ modernes de type classe, telles que `syndicationItem.Title().Text()`. Il s’agit d’un style de programmation plus fluide et différent de la programmation COM classique. Vous n’avez pas besoin d’initialiser explicitement COM (**winrt::init_apartment** le fait pour vous), d’utiliser des pointeurs COM, ni de gérer les codes de retour HRESULT. C++/WinRT convertit les HRESULT d’erreur en exceptions pour un style de programmation naturel et moderne.

## <a name="custom-types-in-the-cwinrt-projection"></a>Types personnalisés dans la projection C++/WinRT
Vous pouvez utiliser les fonctionnalités de langage C++ standard et les [types de données C++ standard et C++/WinRT](std-cpp-data-types.md)&mdash;y compris certains types de données de la bibliothèque C++ standard&mdash; dans votre programmation C++/WinRT. Toutefois, vous prendrez également connaissance de certains types de données personnalisés dans la projection, que vous pourrez choisir d’utiliser. Par exemple, nous avons utilisé [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) dans le démarrage rapide ci-dessus.

[**winrt::com_array**](/uwp/cpp-ref-for-winrt/com-array) est un autre type que vous êtes susceptible d’utiliser à un moment donné. Mais vous êtes moins susceptible d’utiliser directement un type tel que [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view). Ou vous pouvez choisir de ne l’utiliser, de sorte que vous n’aurez aucun code à changer si un type équivalent s’affiche dans la bibliothèque C++ standard.

Il existe également des types que vous pouvez voir si vous étudiez attentivement les en-têtes de noms d’espace Windows de projection C++/WinRT. Un exemple est **winrt::param::hstring**. Ceux-là existent uniquement pour des raisons d’efficacité, et vous ne devriez pas les utiliser dans votre code.

## <a name="important-apis"></a>API importantes
* [SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed.Items](/uwp/api/windows.web.syndication.syndicationfeed.items)
* [Structure winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [Espace de noms winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Rubriquesassociées
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Gestion des chaînes en C++/WinRT](strings.md)
* [Visual Studio Marketplace](https://marketplace.visualstudio.com/)
* [API UWP Windows](https://docs.microsoft.com/uwp/api/)
