---
description: Présentation de C++/WinRT &mdash; une projection de langage C++ standard pour les API Windows Runtime.
title: Introduction à C++/WinRT
ms.date: 04/18/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, introduction
ms.localizationpriority: medium
ms.openlocfilehash: 4b6fd3f3085449c57dafdcedc60f63997e3ec807
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393430"
---
# <a name="introduction-to-cwinrt"></a>Introduction à C++/WinRT
&nbsp;
> [!VIDEO https://www.youtube.com/embed/X41j_gzSwOY]

&nbsp;
> [!VIDEO https://www.youtube.com/embed/nOFNc2uTmGs]

C++/WinRT est une projection de langage C++17 moderne entièrement standard pour les API Windows Runtime (WinRT), implémentée en tant que bibliothèque basée sur un fichier d’en-tête et conçue pour vous fournir un accès de première classe à l’API Windows moderne. Avec C++/WinRT, vous pouvez créer et utiliser des API Windows Runtime en employant n’importe quel compilateur C++17 conforme aux normes. Le SDK Windows inclut C++/WinRT. Il a été introduit dans la version 10.0.17134.0 (Windows 10, version 1803).

C++/WinRT constitue le remplacement recommandé par Microsoft pour la projection de langage [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) et la [bibliothèque de modèles C++ Windows Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live). La liste complète des [rubriques sur C++/WinRT](index.md#topics-about-cwinrt) comprend des informations sur l’interaction avec C++/CX et WRL et le portage à partir de ces langages.

> [!IMPORTANT]
> Certains des éléments les plus importants de C++/WinRT à connaître sont décrits dans les sections [Prise en charge du SDK pour C++/WinRT](#sdk-support-for-cwinrt) et [Prise en charge Visual Studio pour C++/WinRT, XAML, l’extension VSIX et le package NuGet](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="language-projections"></a>Projections de langage
Windows Runtime est basé sur les API COM (Component Object Model) et est conçu pour être accessible par le biais de *projections de langage*. Une projection masque les détails COM et fournit une expérience de programmation plus naturelle pour un langage donné.

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>La projection de langage C++/WinRT dans le contenu de référence des API UWP Windows
Quand vous naviguez dans des [API UWP Windows](https://docs.microsoft.com/uwp/api/), cliquez sur la zone de liste **Langage** en haut à droite, puis sélectionnez **C++/WinRT** pour afficher les blocs de syntaxe d’API tels qu’ils apparaissent dans la projection de langage C++/WinRT.

## <a name="visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package"></a>Prise en charge Visual Studio pour C++/WinRT, XAML, l’extension VSIX et le package NuGet
Pour la prise en charge Visual Studio, vous aurez besoin de Visual Studio 2019 ou de Visual Studio 2017 (au moins la version 15.6 ; nous recommandons au moins la version 15.7). À partir de Visual Studio Installer, installez la charge de travail **Développement pour la plateforme Windows universelle**. Dans **Détails de l’installation** > **Développement pour la plateforme Windows universelle**, cochez la ou les options des **Outils de plateforme Windows universelle C++ (v14x)** , si vous ne l’avez pas déjà fait. Et, dans **Paramètres Windows** > **Mise à jour et Sécurité** > **Pour les développeurs**, choisissez l’option **Mode développeur** plutôt que l’option **Charger la version des applications de façon indépendante**.

Nous vous recommandons de développer à l’aide des dernières versions de Visual Studio et du SDK Windows. Toutefois, si vous utilisez une version de C++/WinRT fournie avec le SDK Windows antérieure à 10.0.17763.0 (Windows 10, version 1809), pour utiliser les en-têtes d’espace de noms Windows mentionnés ci-dessus, vous aurez besoin de la version cible minimale 10.0.17134.0 du SDK Windows (Windows 10, version 1803) dans votre projet.

Vous allez télécharger et installer la dernière version de l’[ extension Visual Studio (VSIX) C++/WinRT](https://aka.ms/cppwinrt/vsix) à partir de [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

- L’extension VSIX vous fournit des modèles de projet et d’élément C++/WinRT dans Visual Studio pour vous permettre de commencer avec le développement C++/WinRT.
- De plus, elle vous offre une visualisation du débogage en mode natif (natvis) Visual Studio des types projetés C++/WinRT, fournissant une expérience semblable au débogage C#. Natvis est automatique pour les builds Debug. Vous pouvez le choisir dans les builds Release en définissant le symbole WINRT_NATVIS.

Les modèles de projet Visual Studio pour C++/WinRT sont décrits dans les sections ci-dessous. Quand vous créez un projet C++/WinRT avec la dernière version installée de l’extension VSIX, le nouveau projet C++/WinRT installe automatiquement le [package NuGet Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/). Le package NuGet **Microsoft.Windows.CppWinRT** fournit une prise en charge de build C++/WinRT (propriétés et cibles MSBuild), rendant votre projet portable entre un ordinateur de développement et un agent de build (sur lequel seul le package NuGet package est installé, et pas l’extension VSIX).

Vous pouvez aussi convertir un projet existant en installant manuellement le package NuGet **Microsoft.Windows.CppWinRT**. Après l’installation de la dernière version de l’extension VSIX (ou une mise à jour vers cette version), ouvrez le projet existant dans Visual Studio, cliquez sur **Projet** \> **Gérer les packages NuGet...** \> **Parcourir**, tapez ou collez **Microsoft.Windows.CppWinRT** dans la zone de recherche, sélectionnez l’élément dans les résultats de la recherche, puis cliquez sur **Installer** pour installer le package correspondant à ce projet. Une fois que vous avez ajouté le package, vous recevrez la prise en charge MSBuild C++/WinRT pour le projet, incluant l’appel de l’outil `cppwinrt.exe`.

> [!IMPORTANT]
> Si vous avez des projets qui ont été créés avec une version de l’extension VSIX antérieure à 1.0.190128.4 (ou mis à niveau pour fonctionner avec une telle version), consultez [Versions antérieures de l’extension VSIX](#earlier-versions-of-the-vsix-extension). Cette section contient des informations importantes sur la configuration de vos projets. Elles vous seront utiles pour mettre à niveau ces derniers afin d’utiliser la dernière version de l’extension VSIX.

- Étant donné que C++/WinRT utilise les fonctionnalités de la norme C++17, le package NuGet définit la propriété de projet **C/C++**  > **Langage** > **Norme du langage C++**  > **Norme ISO C++17 (/std:c++17)** dans Visual Studio.
- Il ajoute également l’option de compilateur [/bigobj](/cpp/build/reference/bigobj-increase-number-of-sections-in-dot-obj-file).
- Il ajoute l’option de compilateur [/await](/cpp/build/reference/await-enable-coroutine-support) pour activer `co_await`.
- Il indique au compilateur XAML d’émettre le codegen C++/WinRT.
- Vous souhaiterez peut-être également définir **Mode de conformité : Oui (/permissive-)** , ce qui contraint davantage votre code à être conforme aux normes.
- Il y a une autre propriété de projet à connaître : **C/C++**  > **Général** > **Considérer les avertissements comme des erreurs**. Définissez-la sur **Oui (/WX)** ou **Non (/WX-)** , à votre convenance. Parfois, les fichiers sources générés par l’outil `cppwinrt.exe` génèrent des avertissements jusqu’à ce que vous leur ajoutiez votre implémentation.

Avec votre système défini comme décrit ci-dessus, vous pourrez créer et générer, ou ouvrir, un projet C++/WinRT dans Visual Studio, et le déployer.

À compter de la version 2.0, le package NuGet **Microsoft.Windows.CppWinRT** inclut l’outil `cppwinrt.exe`. Vous pouvez pointer l’outil `cppwinrt.exe` sur un fichier de métadonnées Windows Runtime (`.winmd`) pour générer une bibliothèque C++ standard basée sur un fichier d’en-tête qui *projette* les API décrites dans les métadonnées pour une consommation à partir du code C++/WinRT. Les fichiers de métadonnées Windows Runtime (`.winmd`) fournissent un moyen canonique de décrire une surface d’API Windows Runtime. En pointant `cppwinrt.exe` vers des métadonnées, vous pouvez générer une bibliothèque à utiliser avec n’importe quelle classe runtime implémentée dans un composant Windows Runtime secondaire ou tiers ou implémentée dans votre propre application. Pour plus d’informations, consultez [Utiliser des API avec C++/WinRT](consume-apis.md).

Avec C++/WinRT, vous pouvez également implémenter vos propres classes runtime en utilisant du code C++ standard, sans avoir recours à une programmation de style COM. Pour une classe runtime, vous décrivez seulement vos types dans un fichier IDL. `midl.exe` et `cppwinrt.exe` génèrent alors automatiquement vos fichiers de code source de modèle d’implémentation. Vous pouvez également implémenter simplement des interfaces en dérivant d’une classe de base C++/WinRT. Pour plus d’informations, consultez [Créer des API avec C++/WinRT](author-apis.md).

Pour obtenir la liste des options de personnalisation pour l’outil `cppwinrt.exe` qui est défini via les propriétés de projet, consultez le fichier [Lisez-moi](https://github.com/microsoft/xlang/tree/master/src/package/cppwinrt/nuget/readme.md#customizing) du package NuGet Microsoft.Windows.CppWinRT.

Vous pouvez identifier un projet qui utilise la prise en charge MSBuild C++/WinRT par la présence du package NuGet **Microsoft.Windows.CppWinRT** installé dans le projet.

Voici les modèles de projet Visual Studio fournis par l’extension VSIX.

### <a name="blank-app-cwinrt"></a>Application vide (C++/WinRT)
Modèle de projet pour une application de plateforme Windows universelle (UWP) qui possède une interface utilisateur XAML.

Visual Studio fournit la prise en charge du compilateur XAML pour générer des stubs d’implémentation et d’en-tête à partir du fichier IDL (Interface Definition Language) (`.idl`) qui se trouve derrière chaque fichier de balisage XAML. Dans un fichier IDL, définissez toutes les classes runtime locales que vous souhaitez référencer dans les pages XAML de votre application, puis générez une fois le projet pour générer les modèles d’implémentation dans `Generated Files` et les définitions de type de stub dans `Generated Files\sources`. Utilisez ensuite ces définitions de type de stub comme référence pour implémenter vos classes runtime locales. Consultez [Factorisation des classes runtime dans des fichiers Midl (.idl)](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl).

La prise en charge de l’aire de conception XAML dans Visual Studio 2019 pour C++/WinRT est proche de la parité avec C#. Dans Visual Studio 2019, vous pouvez utiliser l’onglet **Événements** de la fenêtre **Propriétés** pour ajouter des gestionnaires d’événements dans un projet C++/WinRT. Vous pouvez également ajouter manuellement des gestionnaires d’événements à votre code. Pour plus d’informations, consultez [Gérer des événements en utilisant des délégués en C++/WinRT](handle-events.md).

### <a name="core-app-cwinrt"></a>Application Core (C++/WinRT)
Modèle de projet pour une application de plateforme Windows universelle (UWP) qui n’utilise pas XAML.

À la place, elle utilise l’en-tête d’espace de nom Windows C++/WinRT pour l’espace de noms Windows.ApplicationModel.Core. Après la génération et l’exécution, cliquez sur un espace vide pour ajouter un carré coloré ; cliquez ensuite sur un carré coloré pour le faire glisser.

### <a name="windows-console-application-cwinrt"></a>Application console Windows (C++/WinRT)
Modèle de projet pour une application cliente C+/WinRT pour Windows Desktop, avec une interface utilisateur de console.

### <a name="windows-desktop-application-cwinrt"></a>Application Windows Desktop (C++/WinRT)
Modèle de projet pour une application cliente C++/WinRT pour Windows Desktop, qui affiche un [Windows.Foundation.Uri](/uwp/api/windows.foundation.uri) Windows Runtime dans un **MessageBox** Win32.

### <a name="windows-runtime-component-cwinrt"></a>Composant Windows Runtime (C++/WinRT)
Modèle de projet pour un composant, en général pour une consommation à partir d’une plateforme Windows universelle (UWP).

Ce modèle illustre la chaîne d’outils `midl.exe` > `cppwinrt.exe` où les métadonnées Windows Runtime (`.winmd`) sont générées à partir du fichier IDL, puis où les stubs d’implémentation et d’en-tête sont générés à partir des métadonnées Windows Runtime.

Dans un fichier IDL, définissez les classes runtime dans votre composant, leur interface par défaut et toute autre interface qu’elles implémentent. Générez une fois le projet pour générer `module.g.cpp`, `module.h.cpp`, les modèles d’implémentation dans `Generated Files` et les définitions de type de stub dans `Generated Files\sources`. Utilisez ensuite ces définitions de type de stub comme référence pour implémenter les classes runtime dans votre composant. Consultez [Factorisation des classes runtime dans des fichiers Midl (.idl)](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl).

Regroupez le fichier binaire du composant Windows Runtime intégré et son fichier `.winmd` avec l’application UWP qui les consomme.

## <a name="earlier-versions-of-the-vsix-extension"></a>Versions antérieures de l’extension VSIX
Nous vous recommandons d’installer la dernière version de l’[extension VSIX](https://aka.ms/cppwinrt/vsix) (ou d’effectuer une mise à jour vers celle-ci). Elle est configurée pour effectuer ses propres mises à jour par défaut. Si vous procédez ainsi, et que vous avez des projets qui ont été créés avec une version de l’extension VSIX antérieure à 1.0.190128.4, cette section contient des informations importantes sur la mise à niveau de ces projets pour travailler avec la nouvelle version. Si vous n’effectuez pas de mise à jour, vous trouverez malgré tout les informations de cette section utiles.

En termes de SDK Windows et de versions de Visual Studio pris en charge, ainsi que de configuration Visual Studio, les informations de la section [Prise en charge Visual Studio pour C++/WinRT, XAML, l’extension VSIX et le package NuGet](#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) ci-dessus s’appliquent aux versions antérieures de l’extension VSIX. Les informations ci-dessous décrivent les différences importantes concernant le comportement et la configuration de projets créés avec des versions antérieures (ou mis à niveau pour fonctionner avec de telles versions).

### <a name="created-earlier-than-101810022"></a>Créé avant la version 1.0.181002.2
Si votre projet a été créé avec une version de l’extension VSIX antérieure à 1.0.181002.2, la prise en charge des builds C++/WinRT a été intégrée à cette version de l’extension VSIX. Votre projet comporte la propriété `<CppWinRTEnabled>true</CppWinRTEnabled>` définie dans le fichier `.vcxproj`.

```xml
<Project ...>
    <PropertyGroup Label="Globals">
        <CppWinRTEnabled>true</CppWinRTEnabled>
...
```

Vous pouvez mettre à niveau votre projet en installant manuellement le package NuGet **Microsoft.Windows.CppWinRT**. Après l’installation de la dernière version de l’extension VSIX (ou une mise à niveau vers cette version), ouvrez votre projet dans Visual Studio, cliquez sur **Projet** \> **Gérer les packages NuGet...** \> **Parcourir**, tapez ou collez **Microsoft.Windows.CppWinRT** dans la zone de recherche, sélectionnez l’élément dans les résultats de la recherche, puis cliquez sur **Installer** pour installer le package correspondant à votre projet.

### <a name="created-with-or-upgraded-to-between-101810022-and-101901283"></a>Créé avec une version comprise entre 1.0.181002.2 et 1.0.190128.3 (ou mis à niveau vers ces versions)
Si votre projet a été créé avec une version de l’extension VSIX comprise entre 1.0.181002.2 et 1.0.190128.3 (incluses), le package NuGet **Microsoft.Windows.CppWinRT** a été installé automatiquement dans le projet par le modèle de projet. Il est également possible que vous ayez mis à niveau un ancien projet pour utiliser une version de l’extension VSIX de cette plage. Si c’est le cas, étant donné que la prise en charge des builds était également toujours présente dans les versions de l’extension VSIX de cette plage, le package NuGet **Microsoft.Windows.CppWinRT** peut, ou non, être installé dans votre projet mis à niveau.

Pour mettre à niveau votre projet, suivez les instructions de la section précédente, puis vérifiez que votre projet a le package NuGet **Microsoft.Windows.CppWinRT** installé.

### <a name="invalid-upgrade-configurations"></a>Configurations de mise à niveau non valides
Avec la dernière version de l’extension VSIX, il n’est pas valable qu’un projet ait la propriété `<CppWinRTEnabled>true</CppWinRTEnabled>` s’il n’a pas également le package NuGet **Microsoft.Windows.CppWinRT** installé. Un projet avec cette configuration produit le message d’erreur de build « L’extension VSIX C++/WinRT ne fournit plus de prise en charge de build de projet.  Ajoutez une référence de projet au package NuGet Microsoft.Windows.CppWinRT. »

Comme indiqué ci-dessus, le package NuGet doit maintenant être installé un projet C++/WinRT.

Étant donné que l’élément `<CppWinRTEnabled>` est désormais obsolète, vous pouvez éventuellement modifier votre `.vcxproj` et supprimer l’élément. Cela n’est pas absolument nécessaire, mais c’est envisageable.

De plus, si votre `.vcxproj` contient `<RequiredBundles>$(RequiredBundles);Microsoft.Windows.CppWinRT</RequiredBundles>`, vous pouvez le supprimer afin de pouvoir effectuer une génération sans exiger que l’extension VSIX C++/WinRT soit installée.

## <a name="sdk-support-for-cwinrt"></a>Prise en charge du SDK pour C++/WinRT
Même s’il est désormais présent pour des raisons de compatibilité uniquement, à compter de la version 10.0.17134.0 (Windows 10, version 1803), le SDK Windows contient une bibliothèque C++ standard basée sur un fichier d’en-tête pour une consommation d’API Windows internes (API Windows Runtime dans les espaces de noms Windows). Ces en-têtes se trouvent dans le dossier `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt`. À compter du SDK Windows version 10.0.17763.0 (Windows 10, version 1809), ces en-têtes sont automatiquement générés dans le dossier *$(GeneratedFilesDir)* de votre projet.

À nouveau pour des raisons de compatibilité, le SDK Windows est également fourni avec l’outil `cppwinrt.exe`. Toutefois, nous vous recommandons d’installer et d’utiliser plutôt la version la plus récente de `cppwinrt.exe`, qui est incluse dans le package NuGet **Microsoft.Windows.CppWinRT**. Ce package et `cppwinrt.exe` sont décrits dans les sections ci-dessus.

## <a name="custom-types-in-the-cwinrt-projection"></a>Types personnalisés dans la projection C++/WinRT
Dans votre programmation C++/WinRT, vous pouvez utiliser les fonctionnalités de langage C++ standard et les [types de données C++ standard et C++/WinRT](std-cpp-data-types.md), notamment certains types de données de la bibliothèque C++ standard. Toutefois, vous prendrez également connaissance de certains types de données personnalisés dans la projection, que vous pourrez choisir d’utiliser. Par exemple, nous utilisons [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) dans l’exemple de code de démarrage rapide indiqué dans [Bien démarrer avec C++/WinRT](get-started.md).

[**winrt::com_array**](/uwp/cpp-ref-for-winrt/com-array) est un autre type que vous êtes susceptible d’utiliser à un moment donné. Mais vous êtes moins susceptible d’utiliser directement un type tel que [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view). Ou vous pouvez choisir de ne pas l’utiliser, de sorte que vous n’aurez aucun code à changer si un type équivalent s’affiche dans la bibliothèque C++ standard.

> [!WARNING]
> Il existe également des types que vous pouvez voir si vous étudiez attentivement les en-têtes d’espace de noms Windows C++/WinRT. Il s’agit, par exemple, de **winrt::param::hstring**, mais il existe également des exemples de collection. Ces types servent uniquement à optimiser la liaison des paramètres d’entrée. Ils génèrent d’importantes améliorations des performances et assurent le « simple fonctionnement » de la plupart des modèles d’appel pour les conteneurs et les types C++ standard associés. Ces types ne sont utilisés par la projection que dans les cas où ils ajoutent de la valeur. Ils sont hautement optimisés et ne sont pas destinés à une utilisation générale ; ne tentez pas de les utiliser vous-même. N’utilisez pas non plus d’éléments de l’espace de noms `winrt::impl`, car il s’agit de types d’implémentation susceptibles, donc, d’être modifiés. Vous devez continuer à utiliser les types standard, ou des types de l’[espace de noms winrt](/uwp/cpp-ref-for-winrt/winrt).
>
> Consultez également [Passage de paramètres à la frontière ABI](/windows/uwp/cpp-and-winrt-apis/pass-parms-to-abi).

## <a name="important-apis"></a>API importantes
* [winrt::hstring struct](/uwp/cpp-ref-for-winrt/hstring)
* [Espace de noms winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Rubriques connexes
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Extension Visual Studio (VSIX) C++/WinRT](https://aka.ms/cppwinrt/vsix)
* [Prise en main de C++/WinRT](get-started.md)
* [Types de données C++ standard et C++/WinRT](std-cpp-data-types.md)
* [Gestion des chaînes en C++/WinRT](strings.md)
* [API UWP Windows](https://docs.microsoft.com/uwp/api/)
