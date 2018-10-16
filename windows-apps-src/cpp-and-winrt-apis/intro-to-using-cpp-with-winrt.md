---
author: stevewhims
description: Présentation de C++/WinRT &mdash;une projection de langage C++ standard pour les API Windows Runtime.
title: Présentation de C++/WinRT
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, standard, c++, cpp, winrt, projection, présentation
ms.localizationpriority: medium
ms.openlocfilehash: 7848a0b00e2f6a2bbaa84c413dc26f1ea7238e8e
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2018
ms.locfileid: "4623690"
---
# <a name="introduction-to-cwinrt"></a>Présentation de C++/WinRT
&nbsp;
> [!VIDEO https://www.youtube.com/embed/nOFNc2uTmGs]

C++/WinRT est une projection de langage C++17 moderne entièrement standard pour les API Windows Runtime (WinRT), implémentée en tant que bibliothèque basée sur un fichier d'en-tête et conçue pour vous fournir un accès de première classe à l’API Windows moderne. Avec C++/WinRT, vous pouvez créer et utiliser des API Windows Runtime en utilisant n’importe quel compilateur C++17 conforme aux normes. Le SDK Windows inclut C++/WinRT. Il a été introduit dans la version10.0.17134.0 (Windows10, version1803).

C++ / WinRT est remplacement recommandé par Microsoft pour le [C++ / CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) projection de langage et la [Bibliothèque de modèles Windows Runtime C++ (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live). La liste complète des [rubriques sur C++ / WinRT](index.md#topics-about-cwinrt) contient des informations sur à la fois en interopérant avec lui et le portage de C++ / CX et WRL.

> [!IMPORTANT]
> Deux des éléments les plus importants de C++/WinRT à connaître sont décrits dans les sections [Prise en charge du Kit de développement logiciel (SDK) pour C++/WinRT](#sdk-support-for-cwinrt) et [Prise en charge de Visual Studio pour C++/WinRT et VSIX](#visual-studio-support-for-cwinrt-and-the-vsix).

## <a name="language-projections"></a>Projections de langage
Windows Runtime est basé sur les API COM (Component Object Model) et est conçu pour être accessible via des *projections de langage*. Une projection masque les détails COM et fournit une expérience de programmation plus naturelle pour un langage donné.

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>La projection de langage C++/WinRT dans le contenu de référence des API UWP Windows
Lorsque vous naviguez dans des [API UWP Windows](https://docs.microsoft.com/uwp/api/), cliquez sur la zone de liste **Langage** dans l’angle supérieur droit et sélectionnez **C++/WinRT** pour afficher les blocs de syntaxe d’API tels qu’ils apparaissent dans la projection de langage C++/WinRT.

## <a name="sdk-support-for-cwinrt"></a>Prise en charge du Kit de développement logiciel (SDK) pour C++/WinRT
À compter de la version10.0.17134.0 (Windows10, version1803), le SDK Windows contient une bibliothèque basée sur un fichier d'en-tête C++ standard pour l’utilisation des API Windows internes (API Windows Runtime dans les espaces de noms Windows). C++/WinRT est également fourni avec l'outil `cppwinrt.exe`, que vous pouvez pointer sur un fichier de métadonnées Windows Runtime (`.winmd`) pour générer une bibliothèque basée sur un fichier d'en-tête C++ standard qui *projette* les API décrites dans les métadonnées pour une consommation à partir du code C++/WinRT. Les fichiers de métadonnées Windows Runtime (`.winmd`) fournissent un moyen canonique de décrire une surface d’API Windows Runtime. En pointant `cppwinrt.exe` vers des métadonnées, vous pouvez générer une bibliothèque à utiliser avec n’importe quelle classe runtime implémentée dans un composant Windows Runtime secondaire ou tiers ou implémentée dans votre propre application. Pour plus d’informations, voir [Utiliser des API avec C++/WinRT](consume-apis.md).

Avec C++/WinRT, vous pouvez également implémenter vos propres classes runtime en utilisant du code C++ standard, sans avoir recours à une programmation de style COM. Pour une classe runtime, vous décrivez seulement vos types dans un fichier IDL, et `midl.exe` et `cppwinrt.exe` génèrent vos fichiers de code source de modèle d'implémentation pour vous. Vous pouvez également implémenter simplement des interfaces en dérivant d'une classe de base C++/WinRT. Pour plus d’informations, voir [Créer des API avec C++/WinRT](author-apis.md).

## <a name="visual-studio-support-for-cwinrt-and-the-vsix"></a>Prise en charge de Visual Studio pour C++/WinRT et VSIX
Pour les modèles de projet C++/WinRT dans Visual Studio, mais aussi les propriétés et cibles MSBuild C++/WinRT, téléchargez et installez l’[extension Visual Studio (VSIX) C++/WinRT](https://aka.ms/cppwinrt/vsix) à partir de [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

> [!NOTE]
> Avec la version 1.0.181002.2 (ou version ultérieure) du projet VSIX installé, création d’un nouveau C++ / WinRT projet installe automatiquement le [package Microsoft.Windows.CppWinRT NuGet](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) pour ce projet. Le package Microsoft.Windows.CppWinRT NuGet fournit améliorée C++ / WinRT projet build prise en charge, qui devient votre projet portable entre un ordinateur de développement et d’un agent de build (sur lequel seul le package NuGet et pas VSIX, est installé).
>
> Pour un projet existant&mdash;une fois que vous avez installé la version 1.0.181002.2 (ou version ultérieure) du projet VSIX&mdash;nous recommandons que vous ouvrez le projet dans Visual Studio, cliquez sur **le projet** \> **Gérer les Packages NuGet …**  \>  **Parcourir**, tapez ou collez **Microsoft.Windows.CppWinRT** dans la zone de recherche, sélectionnez l’élément dans les résultats de recherche, puis cliquez sur **installer** pour installer le package de ce projet.

Vous aurez besoin de Visual Studio 2017 (vous devez au moins la version 15,6, mais nous vous recommandons d’au moins la version 15.7) et du SDK Windows version 10.0.17134.0 (Windows 10, version 1803). Si vous n’avez pas déjà installé, vous devrez installer l’option **Outils de plateforme Windows universelle C++** dans Visual Studio Installer. Et, dans les **paramètres**Windows > **mise à jour \ & sécurité** > **pour les développeurs**, choisissez l’option de **mode développeur** , plutôt que l’option de **chargement indépendant d’applications** .

Vous serez alors en mesure de créer et de concevoir ou d’ouvrir, C++ / WinRT de projet dans Visual Studio et le déployer. Par ailleurs, vous pouvez convertir un projet existant en ajoutant le `<CppWinRTEnabled>true</CppWinRTEnabled>` propriété à son `.vcxproj` fichier.

```xml
<Project ...>
    <PropertyGroup Label="Globals">
        <CppWinRTEnabled>true</CppWinRTEnabled>
...
```

Une fois que vous avez ajouté cette propriété, vous recevrez la prise en charge MSBuild C++/WinRT pour le projet, incluant l’appel de l’outil `cppwinrt.exe`.

Étant donné que C++ / WinRT utilise les fonctionnalités de la norme C ++ 17, il doit projeter propriété **C/C++** > **langue** > **Standard de langage C++** > **ISO C ++ 17 Standard (/ std: c ++ 17)**. Vous souhaiterez peut-être également définir **Conformance mode: Yes (/permissive-)**, ce qui contraint davantage votre code à être conforme aux normes.

N’oubliez pas une autre propriété de projet qui est **C/C++** > **Général** > **Treat Warnings As Errors**. Définissez ce paramètre sur **Yes(/WX)** ou **No (/WX-)** à votre convenance. Parfois, les fichiers source générés par l’outil `cppwinrt.exe` génèrent des avertissements jusqu'à ce que vous leur ajoutiez votre implémentation.

VSIX offre également une visualisation de débogage de Visual Studio en natif (natvis) des types projetés C++/WinRT; en fournissant une expérience semblable au débogage de C#. Natvis est automatique pour les versions Debug. Vous pouvez le choisir dans les versions Release en définissant le symbole WINRT_NATVIS.

Voici les modèles de projet Visual Studio fournis par VSIX.

### <a name="windows-console-application-cwinrt"></a>Windows Console Application (C++/WinRT)
Modèle de projet pour une application cliente C+/WinRT pour Windows Desktop, avec une interface utilisateur de console.

### <a name="blank-app-cwinrt"></a>Blank App (C++/WinRT)
Modèle de projet pour une application de plateforme Windows universelle (UWP) qui possède une interface utilisateur XAML.

Visual Studio fournit la prise en charge du compilateur XAML pour générer des stubs d’implémentation et d’en-tête à partir du fichier IDL (Interface Definition Language) (`.idl`) qui se trouve derrière chaque fichier de balisage XAML. Dans un fichier IDL, définissez toutes les classes runtime locales que vous souhaitez référencer dans les pages XAML de votre application, puis générez une fois le projet pour générer les modèles d’implémentation dans `Generated Files` et les définitions de type de stub dans `Generated Files\sources`. Puis, utilisez ces définitions de type de stub comme référence pour implémenter vos classes runtime locales. Nous vous recommandons de déclarer chaque classe runtime dans son propre fichier IDL.

### <a name="core-app-cwinrt"></a>Core App (C++/WinRT)
Modèle de projet pour une application de plateforme Windows universelle (UWP) qui n’utilise pas XAML.

Au lieu de cela, elle utilise l’en-tête d’espace de nom Windows C++/WinRT pour l’espace de noms Windows.ApplicationModel.Core. Après génération et exécution, cliquez sur un espace vide pour ajouter un carré coloré; cliquez ensuite sur un carré coloré pour le faire glisser.

### <a name="windows-runtime-component-cwinrt"></a>Composant Windows Runtime (C++/WinRT)
Modèle de projet pour un composant, en règle générale pour l’utilisation depuis une plateforme Windows universelle (UWP).

Ce modèle illustre la chaîne d’outils `midl.exe` > `cppwinrt.exe`, dans laquelle les métadonnées Windows Runtime (`.winmd`) sont générées à partir du fichier IDL, puis les stubs d’implémentation et d’en-tête sont générés à partir des métadonnées Windows Runtime.

Dans un fichier IDL, définissez les classes runtime dans votre composant, leur interface par défaut et toute autre interface qu’elles implémentent. Générez une fois le projet pour générer `module.g.cpp`, `module.h.cpp`, les modèles d’implémentation dans `Generated Files` et les définitions de type de stub dans `Generated Files\sources`. Puis utilisez ces définitions de type de stub comme référence pour implémenter les classes runtime dans votre composant. Nous vous recommandons de déclarer chaque classe runtime dans son propre fichier IDL.

Regroupez le binaire du composant Windows Runtime intégré et son `.winmd` avec l'application UWP qui les utilise.

## <a name="custom-types-in-the-cwinrt-projection"></a>Types personnalisés dans la projection C++/WinRT
Dans votre programmation C++/WinRT, vous pouvez utiliser les fonctionnalités de langage C++ standard et les [types de données C++ standard et C++/WinRT](std-cpp-data-types.md)&mdash;y compris certains types de données de la bibliothèque C++ standard. Toutefois, vous prendrez également connaissance de certains types de données personnalisés dans la projection, que vous pourrez choisir d’utiliser. Par exemple, nous utilisons [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) dans l’exemple de code de démarrage rapide de [Prise en main de C++/WinRT](get-started.md).

[**winrt::com_array**](/uwp/cpp-ref-for-winrt/com-array) est un autre type que vous êtes susceptible d’utiliser à un moment donné. Mais vous êtes moins susceptible d’utiliser directement un type tel que [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view). Ou vous pouvez choisir de ne l’utiliser, de sorte que vous n’aurez aucun code à changer si un type équivalent s’affiche dans la bibliothèque C++ standard.

> [!WARNING]
> Il existe également des types que vous pouvez voir si vous étudiez attentivement les en-têtes de noms d’espace Windows C++/WinRT. Il s'agit, par exemple, de **winrt::param::hstring**, mais il existe également des exemples de collection. Ces types servent uniquement à optimiser la liaison des paramètres d’entrée. Ils génèrent d'importantes améliorations des performances et assurent le «simple fonctionnement» de la plupart des modèles d’appel pour les conteneurs et les types C++ standards associés. Ces types ne sont utilisés par la projection que dans les cas où ils ajoutent de la valeur. Ils sont hautement optimisés et ne sont pas destinés à une utilisation générale; ne tentez pas de les utiliser vous-même. N'utilisez pas non plus d'éléments de l'espace de noms `winrt::impl`, car il s'agit de types d’implémentation, susceptibles donc d'être modifiés. Vous devez continuer à utiliser les types standard, ou des types de l'[espace de noms winrt](/uwp/cpp-ref-for-winrt/winrt).

## <a name="important-apis"></a>API importantes
* [Structure winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [Espace de noms winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Rubriquesassociées
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Extension Visual Studio (VSIX) C++/WinRT](https://aka.ms/cppwinrt/vsix)
* [Prise en main de C++/WinRT](get-started.md)
* [Types de données C++ standard et C++/WinRT](std-cpp-data-types.md)
* [Gestion des chaînes en C++/WinRT](strings.md)
* [API UWP Windows](https://docs.microsoft.com/uwp/api/)
