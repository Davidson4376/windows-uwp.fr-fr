---
description: Présentation de C++/WinRT &mdash; une projection de langage C++ standard pour les API Windows Runtime.
title: Présentation de C++/WinRT
ms.date: 01/31/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, présentation
ms.localizationpriority: medium
ms.openlocfilehash: 883463f291864016ebc32f2d510936452c931366
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649694"
---
# <a name="introduction-to-cwinrt"></a>Présentation de C++/WinRT
&nbsp;
> [!VIDEO https://www.youtube.com/embed/nOFNc2uTmGs]

C++/WinRT est une projection de langage C++17 moderne entièrement standard pour les API Windows Runtime (WinRT), implémentée en tant que bibliothèque basée sur un fichier d'en-tête et conçue pour vous fournir un accès de première classe à l’API Windows moderne. Avec C++/WinRT, vous pouvez créer et utiliser des API Windows Runtime en utilisant n’importe quel compilateur C++17 conforme aux normes. Le SDK Windows inclut C++/WinRT. Il a été introduit dans la version 10.0.17134.0 (Windows 10, version 1803).

C++ / c++ / WinRT est le remplacement de recommandée de Microsoft pour le [C++ / c++ / CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) projection de langage et le [bibliothèque de modèles Windows Runtime C++ (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live). La liste complète des [rubriques sur C++ / c++ / WinRT](index.md#topics-about-cwinrt) inclut des informations sur les interopération avec et portage à partir de, C++ / c++ / CX et WRL.

> [!IMPORTANT]
> Deux des éléments plus importants de C++ / c++ / WinRT à connaître sont décrites dans les sections [prennent en charge des SDK pour C / c++ / WinRT](#sdk-support-for-cwinrt) et [prise en charge de Visual Studio pour C++ / c++ / WinRT, XAML, l’extension VSIX et le package NuGet package](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="language-projections"></a>Projections de langage
Windows Runtime est basé sur les API COM (Component Object Model) et est conçu pour être accessible via des *projections de langage*. Une projection masque les détails COM et fournit une expérience de programmation plus naturelle pour un langage donné.

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>La projection de langage C++/WinRT dans le contenu de référence des API UWP Windows
Lorsque vous naviguez dans des [API UWP Windows](https://docs.microsoft.com/uwp/api/), cliquez sur la zone de liste **Langage** dans l’angle supérieur droit et sélectionnez **C++/WinRT** pour afficher les blocs de syntaxe d’API tels qu’ils apparaissent dans la projection de langage C++/WinRT.

## <a name="sdk-support-for-cwinrt"></a>Prise en charge du Kit de développement logiciel (SDK) pour C++/WinRT
À compter de la version 10.0.17134.0 (Windows 10, version 1803), le SDK Windows contient une bibliothèque basée sur un fichier d'en-tête C++ standard pour l’utilisation des API Windows internes (API Windows Runtime dans les espaces de noms Windows). C++/WinRT est également fourni avec l'outil `cppwinrt.exe`, que vous pouvez pointer sur un fichier de métadonnées Windows Runtime (`.winmd`) pour générer une bibliothèque basée sur un fichier d'en-tête C++ standard qui *projette* les API décrites dans les métadonnées pour une consommation à partir du code C++/WinRT. Les fichiers de métadonnées Windows Runtime (`.winmd`) fournissent un moyen canonique de décrire une surface d’API Windows Runtime. En pointant `cppwinrt.exe` vers des métadonnées, vous pouvez générer une bibliothèque à utiliser avec n’importe quelle classe runtime implémentée dans un composant Windows Runtime secondaire ou tiers ou implémentée dans votre propre application. Pour plus d’informations, voir [Utiliser des API avec C++/WinRT](consume-apis.md).

Avec C++/WinRT, vous pouvez également implémenter vos propres classes runtime en utilisant du code C++ standard, sans avoir recours à une programmation de style COM. Pour une classe runtime, vous décrivez seulement vos types dans un fichier IDL, et `midl.exe` et `cppwinrt.exe` génèrent vos fichiers de code source de modèle d'implémentation pour vous. Vous pouvez également implémenter simplement des interfaces en dérivant d'une classe de base C++/WinRT. Pour plus d’informations, voir [Créer des API avec C++/WinRT](author-apis.md).

## <a name="visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package"></a>Prise en charge de Visual Studio pour C / c++ / WinRT, XAML, l’extension VSIX et le package NuGet
Pour la prise en charge de Visual Studio, en plus du SDK Windows cible version minimum 10.0.17134.0 (Windows 10, version 1803), vous devez Visual Studio 2017 (au moins la version 15.6 ; nous recommandons au moins 15.7), ou Visual Studio 2019. Si vous n’avez pas déjà installé, vous devez installer le **outils de plateforme Windows universelle C++** option à partir de Visual Studio Installer. Et, dans Windows **paramètres** > **mise à jour \& sécurité** > **pour les développeurs**, choisissez le **développeur mode** option plutôt que la **et supprimer des applications** option.

Vous devrez télécharger et installer la dernière version de la [C++ / c++ / WinRT Visual Studio Extension (VSIX)](https://aka.ms/cppwinrt/vsix) à partir de la [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

- L’extension VSIX vous donne C + c++ / WinRT les modèles de projet et d’élément dans Visual Studio, afin que vous pouvez commencer avec C / c++ / WinRT développement.
- En outre, il vous donne débogage natif de Visual Studio visualization (natvis) de C / c++ / WinRT projeté types ; en fournissant une expérience similaire à C# débogage. Natvis est automatique pour les versions Debug. Vous pouvez le choisir dans les versions Release en définissant le symbole WINRT_NATVIS.

Les modèles de projet Visual Studio pour C / c++ / WinRT sont décrits ci-dessous. Lorsque vous créez un nouveau C++ / c++ / WinRT des projets avec la dernière version de l’extension VSIX est installée, le nouveau C++ / c++ / WinRT projet installe automatiquement le [Microsoft.Windows.CppWinRT NuGet package](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/). Le **Microsoft.Windows.CppWinRT** package NuGet fournit C + c++ / WinRT build prise en charge (cibles et propriétés MSBuild,) rendre votre projet portable entre un ordinateur de développement et un agent de build (sur lequel uniquement le package NuGet, et pas l’extension VSIX, est installé).

> [!IMPORTANT]
> Si vous avez des projets qui ont été créés avec (ou mis à niveau pour fonctionner avec) une version de l’extension VSIX précédemment que 1.0.190128.4, puis consultez [les versions antérieures de l’extension VSIX](#earlier-versions-of-the-vsix-extension). Cette section contient des informations importantes sur la configuration de vos projets, vous devez savoir pour les mettre à niveau pour utiliser la dernière version de l’extension VSIX.

Étant donné que C++ / c++ / WinRT utilise des fonctionnalités à partir de la norme C ++ 17, il a besoin de propriété de projet **C/C++** > **langage** > **norme du langage C++**  >  **Norme ISO C ++ 17 (/ std : c ++ 17)**. Vous pouvez également définir **mode de conformité : Oui (/ permissive-)**, ce qui limite davantage votre code doit être conforme aux normes.

N’oubliez pas une autre propriété de projet qui est **C/C++** > **Général** > **Treat Warnings As Errors**. Définissez ce paramètre sur **Yes(/WX)** ou **No (/WX-)** à votre convenance. Parfois, les fichiers source générés par l’outil `cppwinrt.exe` génèrent des avertissements jusqu'à ce que vous leur ajoutiez votre implémentation.

Avec votre système est définie comme décrit ci-dessus, vous serez en mesure de créer et de créer ou d’ouvrir, C++ / c++ / WinRT de projet dans Visual Studio et le déployer.

Ou bien, vous pouvez convertir un projet existant en installant manuellement la **Microsoft.Windows.CppWinRT** package NuGet. Après l’installation (ou mise à jour vers) la dernière version de l’extension VSIX, ouvrez le projet existant dans Visual Studio, cliquez sur **projet** \> **gérer les Packages NuGet...** \> **Parcourir**, tapez ou collez **Microsoft.Windows.CppWinRT** dans la zone de recherche, sélectionnez l’élément dans les résultats de recherche, puis cliquez sur **installer** pour installer le package pour ce projet. Une fois que vous avez ajouté le package, vous obtiendrez C + c++ / prise en charge de MSBuild de WinRT pour le projet, notamment l’appel la `cppwinrt.exe` outil.

Vous pouvez identifier un projet qui utilise C++ / c++ / WinRT MSBuild les prise en charge par la présence de la **Microsoft.Windows.CppWinRT** package NuGet installé au sein du projet.

Voici les modèles de projet Visual Studio fournis par l’extension VSIX.

### <a name="windows-console-application-cwinrt"></a>Windows Console Application (C++/WinRT)
Modèle de projet pour une application cliente C+/WinRT pour Windows Desktop, avec une interface utilisateur de console.

### <a name="blank-app-cwinrt"></a>Blank App (C++/WinRT)
Modèle de projet pour une application de plateforme Windows universelle (UWP) qui possède une interface utilisateur XAML.

Visual Studio fournit la prise en charge du compilateur XAML pour générer des stubs d’implémentation et d’en-tête à partir du fichier IDL (Interface Definition Language) (`.idl`) qui se trouve derrière chaque fichier de balisage XAML. Dans un fichier IDL, définissez toutes les classes runtime locales que vous souhaitez référencer dans les pages XAML de votre application, puis générez une fois le projet pour générer les modèles d’implémentation dans `Generated Files` et les définitions de type de stub dans `Generated Files\sources`. Puis, utilisez ces définitions de type de stub comme référence pour implémenter vos classes runtime locales. Nous vous recommandons de déclarer chaque classe runtime dans son propre fichier IDL.

XAML conception surface prise en charge Visual Studio pour C / c++ / WinRT est proche de parité avec C#. Seule exception concerne le **événements** onglet de la **propriétés** fenêtre. Avec un C# projet, vous pouvez utiliser cet onglet pour ajouter des gestionnaires d’événements ; avec un C / c++ / WinRT, projet, cette fonction n’est pas présente. Mais consultez [gérer les événements à l’aide de délégués en C / c++ / WinRT](handle-events.md) pour plus d’informations sur l’ajout de gestionnaires d’événements à votre code.

### <a name="core-app-cwinrt"></a>Core App (C++/WinRT)
Modèle de projet pour une application de plateforme Windows universelle (UWP) qui n’utilise pas XAML.

Au lieu de cela, elle utilise l’en-tête d’espace de nom Windows C++/WinRT pour l’espace de noms Windows.ApplicationModel.Core. Après génération et exécution, cliquez sur un espace vide pour ajouter un carré coloré ; cliquez ensuite sur un carré coloré pour le faire glisser.

### <a name="windows-runtime-component-cwinrt"></a>Composant Windows Runtime (C++/WinRT)
Modèle de projet pour un composant, en règle générale pour l’utilisation depuis une plateforme Windows universelle (UWP).

Ce modèle illustre la chaîne d’outils `midl.exe` > `cppwinrt.exe`, dans laquelle les métadonnées Windows Runtime (`.winmd`) sont générées à partir du fichier IDL, puis les stubs d’implémentation et d’en-tête sont générés à partir des métadonnées Windows Runtime.

Dans un fichier IDL, définissez les classes runtime dans votre composant, leur interface par défaut et toute autre interface qu’elles implémentent. Générez une fois le projet pour générer `module.g.cpp`, `module.h.cpp`, les modèles d’implémentation dans `Generated Files` et les définitions de type de stub dans `Generated Files\sources`. Puis utilisez ces définitions de type de stub comme référence pour implémenter les classes runtime dans votre composant. Nous vous recommandons de déclarer chaque classe runtime dans son propre fichier IDL.

Regroupez le binaire du composant Windows Runtime intégré et son `.winmd` avec l'application UWP qui les utilise.

## <a name="earlier-versions-of-the-vsix-extension"></a>Versions antérieures de l’extension VSIX
Nous vous recommandons d’installer (ou mettre à jour vers) la dernière version de la [extension VSIX](https://aka.ms/cppwinrt/vsix). Il est configuré pour mettre à jour automatiquement par défaut. Si vous le faire, vous avez des projets qui ont été créés avec une version de l’extension VSIX antérieures à 1.0.190128.4, puis cette section contient des informations importantes sur la mise à niveau ces projets pour travailler avec la nouvelle version. Si vous ne mettez à jour, puis vous seront toujours utiles les informations dans cette section.

En termes de prise en charge Windows SDK et les versions de Visual Studio et configuration de Visual Studio, les informations dans le [prise en charge de Visual Studio pour C / c++ / WinRT, XAML, l’extension VSIX et le package NuGet](#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) section ci-dessus s’applique à une version antérieure versions de l’extension VSIX. Les informations ci-dessous décrit les différences importantes concernant le comportement et la configuration de projets créée avec (ou mis à niveau pour travailler avec) précédemment versions.

### <a name="created-earlier-than-101810022"></a>Créé antérieurement à 1.0.181002.2
Si votre projet a été créé avec une version de l’extension VSIX antérieures à 1.0.181002.2, puis C + c++ / prise en charge de la build WinRT a été intégrée dans cette version de l’extension VSIX. Votre projet comporte le `<CppWinRTEnabled>true</CppWinRTEnabled>` propriété définie dans le `.vcxproj` fichier.

```xml
<Project ...>
    <PropertyGroup Label="Globals">
        <CppWinRTEnabled>true</CppWinRTEnabled>
...
```

Vous pouvez mettre à niveau votre projet en installant manuellement la **Microsoft.Windows.CppWinRT** package NuGet. Après l’installation (ou mise à niveau vers) la dernière version de l’extension VSIX, ouvrez votre projet dans Visual Studio, cliquez sur **projet** \> **gérer les Packages NuGet...** \> **Parcourir**, tapez ou collez **Microsoft.Windows.CppWinRT** dans la zone de recherche, sélectionnez l’élément dans les résultats de recherche, puis cliquez sur **installer** pour installer le package pour votre projet.

### <a name="created-with-or-upgraded-to-between-101810022-and-101901283"></a>Créé avec (ou mis à niveau vers) entre 1.0.181002.2 et 1.0.190128.3
Si votre projet a été créé avec une version de l’extension VSIX entre 1.0.181002.2 et 1.0.190128.3, inclus, puis le **Microsoft.Windows.CppWinRT** package NuGet a été installé dans le projet automatiquement par le projet modèle. Vous pouvez également ont mis à niveau un projet antérieur pour utiliser une version de l’extension VSIX dans cette plage. Si vous l’avez fait, puis&mdash;étant donné que la prise en charge de la build a été également toujours présent dans les versions de l’extension VSIX dans cette plage&mdash;votre projet mis à niveau peut ou ne peut pas avoir le **Microsoft.Windows.CppWinRT** package NuGet installé.

Pour mettre à niveau votre projet, suivez les instructions dans la section précédente et assurez-vous que votre projet n’a le **Microsoft.Windows.CppWinRT** package NuGet installé.

### <a name="invalid-upgrade-configurations"></a>Configurations de mise à niveau non valides
Avec la dernière version de l’extension VSIX, il n’est pas valide pour un projet pour que le `<CppWinRTEnabled>true</CppWinRTEnabled>` propriété si elle n’a pas également la **Microsoft.Windows.CppWinRT** package NuGet installé. Un projet avec cette configuration génère le message d’erreur de build, « C + c++ / WinRT VSIX ne fournit plus de prise en charge de génération de projet.  Ajoutez une référence de projet au package Nuget de Microsoft.Windows.CppWinRT. »

Comme mentionné ci-dessus, C++ / c++ / WinRT projet doit maintenant pour que le package NuGet installé qu’il contient.

Dans la mesure où le `<CppWinRTEnabled>` élément est désormais obsolète, vous pouvez éventuellement modifier votre `.vcxproj`et supprimer l’élément. Il n’est pas strictement nécessaire, mais il est envisageable.

En outre, si votre `.vcxproj` contient `<RequiredBundles>$(RequiredBundles);Microsoft.Windows.CppWinRT</RequiredBundles>`, vous pouvez le supprimer afin que vous puissiez générer sans nécessiter de C++ / c++ / extension WinRT VSIX à installer.

## <a name="custom-types-in-the-cwinrt-projection"></a>Types personnalisés dans la projection C++/WinRT
Dans votre C + c++ / WinRT de programmation, vous pouvez utiliser les fonctionnalités du langage C++ standards et [les types de données Standard C++ et C++ / c++ / WinRT](std-cpp-data-types.md)&mdash;, y compris certains types de données de bibliothèque C++ Standard. Toutefois, vous prendrez également connaissance de certains types de données personnalisés dans la projection, que vous pourrez choisir d’utiliser. Par exemple, nous utilisons [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) dans l’exemple de code de démarrage rapide de [Prise en main de C++/WinRT](get-started.md).

[**WinRT::com_array** ](/uwp/cpp-ref-for-winrt/com-array) est un autre type que vous êtes susceptible d’utiliser à un moment donné. Mais vous êtes moins susceptible d’utiliser directement un type tel que [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view). Ou vous pouvez choisir de ne l’utiliser, de sorte que vous n’aurez aucun code à changer si un type équivalent s’affiche dans la bibliothèque C++ standard.

> [!WARNING]
> Il existe également des types que vous pouvez voir si vous étudiez attentivement les en-têtes de noms d’espace Windows C++/WinRT. Il s'agit, par exemple, de **winrt::param::hstring**, mais il existe également des exemples de collection. Ces types servent uniquement à optimiser la liaison des paramètres d’entrée. Ils génèrent d'importantes améliorations des performances et assurent le « simple fonctionnement » de la plupart des modèles d’appel pour les conteneurs et les types C++ standards associés. Ces types ne sont utilisés par la projection que dans les cas où ils ajoutent de la valeur. Ils sont hautement optimisés et ne sont pas destinés à une utilisation générale ; ne tentez pas de les utiliser vous-même. N'utilisez pas non plus d'éléments de l'espace de noms `winrt::impl`, car il s'agit de types d’implémentation, susceptibles donc d'être modifiés. Vous devez continuer à utiliser les types standard, ou des types de l'[espace de noms winrt](/uwp/cpp-ref-for-winrt/winrt).

## <a name="important-apis"></a>API importantes
* [WinRT::hstring struct](/uwp/cpp-ref-for-winrt/hstring)
* [espace de noms WinRT](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Rubriques connexes
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++ / c++ / WinRT Visual Studio Extension (VSIX)](https://aka.ms/cppwinrt/vsix)
* [Bien démarrer avec C / c++ / WinRT](get-started.md)
* [Les types de données C++ standard et C / c++ / WinRT](std-cpp-data-types.md)
* [Chaîne gère en C / c++ / WinRT](strings.md)
* [Windows API UWP](https://docs.microsoft.com/uwp/api/)
