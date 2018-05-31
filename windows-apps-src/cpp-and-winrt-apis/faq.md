---
author: stevewhims
description: Réponses aux questions que vous êtes susceptibles de vous poser sur la création et l’utilisation d’API Windows Runtime avec C++/WinRT.
title: Forum aux questions sur C++/WinRT
ms.author: stwhi
ms.date: 04/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, standard, c++, cpp, winrt, projection, questions, fréquentes, FAQ, forum aux questions
ms.localizationpriority: medium
ms.openlocfilehash: aad5c5ed2123af39ebb6aff0c9098586ce958196
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832033"
---
# <a name="frequently-asked-questions-about-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Forum aux questions sur [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
> [!NOTE]
> **Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.**

Réponses aux questions que vous êtes susceptibles d’avoir sur la création et l’utilisation d’API Windows Runtime avec C++/WinRT.

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsixhttpsakamscppwinrtvsix"></a>Quelles sont les exigences pour l’[extension Visual Studio (VSIX)](https://aka.ms/cppwinrt/vsix) C++/WinRT?
[VSIX](https://aka.ms/cppwinrt/vsix) applique une version cible du SDK Windows minimale de 10.0.17134.0 (Windows10, version1803). Vous aurez également besoin de Visual Studio2017 version15.6 ou ultérieure. Vous pouvez identifier un projet qui utilise VSIX par la présence de `<CppWinRTEnabled>true</CppWinRTEnabled>` dans `<PropertyGroup Label="Globals">` dans le fichier `.vcxproj`. Pour plus d’informations, voir [Prise en charge de Visual Studio pour C++/WinRT et VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

## <a name="whats-a-runtime-class"></a>Qu’est-ce qu’une *classe runtime*?
Une classe runtime est un type qui peut être activé et utilisé via des interfaces COM modernes, généralement au-delà des limites exécutables. Toutefois, une classe runtime peut également être utilisée au sein de l’unité de compilation qui l’implémente. Vous déclarez une classe runtime dans le langage IDL (Interface Definition Language), et vous pouvez l’implémenter en C++ standard à l’aide de C++/WinRT.

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>Que signifient *type projeté* et *type d’implémentation*?
Si vous ne fait qu’*utiliser* une classe Windows Runtime (classe runtime), vous serez confronté exclusivement à des *types projetés*. C++/WinRT étant une *projection de langage*, les types projetés font partie de la surface de Windows Runtime qui est *projetée* en C++ avec C++/WinRT. Pour obtenir plus d’informations, voir [Utiliser des API avec C++/WinRT](consume-apis.md).

Le *type d’implémentation* contient l’implémentation d’une classe runtime, il est donc uniquement disponible dans le projet qui implémente la classe runtime. Lorsque vous travaillez dans un projet qui implémente des classes runtime (un projet de composant Windows Runtime ou un projet qui utilise l’interface utilisateur XAML), il est important d’être à l’aise avec la distinction entre votre type d’implémentation pour une classe runtime et le type projeté qui représente la classe runtime projetée en C++/WinRT. Pour obtenir plus d’informations, voir [Créer des API avec C++/WinRT](author-apis.md).

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>Dois-je implémenter [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable) et, si tel est le cas, comment?
Si vous disposez d’une classe runtime qui libère les ressources dans son destructeur, et que cette classe runtime est conçue pour être utilisée depuis l’extérieur de son unité de compilation d’implémentation (c’est un composant Windows Runtime destiné à une utilisation générale par les applications clientes Windows Runtime), nous vous recommandons d’implémenter également **IClosable** pour prendre en charge l’utilisation de votre classe runtime par les langues qui ne possèdent pas de finalisation déterministe. Assurez-vous que vos ressources sont libérées si le destructeur, [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.Close) ou les deux sont appelés. **IClosable::Close** peut être appelé un nombre de fois arbitraire.

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosablewindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>Ai-je besoin d’appeler [**IClosable::Close**](/uwp/api/windows.foundation.iclosable#Windows_Foundation_IClosable_Close_) sur les classes runtime que j’utilise?
**IClosable** existe pour prendre en charge les langues qui ne possèdent pas de finalisation déterministe. Par conséquent, vous ne devez pas appeler **IClosable::Close** à partir de C++/WinRT, sauf dans de très rares cas impliquant des concurrences de fermeture ou des étreintes semi-fatales (semi-deadly embraces). Si vous utilisez des types **Windows.UI.Composition**, par exemple, vous pouvez rencontrer des cas où vous souhaiterez disposer des objets dans une séquence définie, au lieu de laisser la destruction du wrapper C++/WinRT faire le travail pour vous.

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>Ai-je besoin de déclarer un constructeur dans le fichier IDL de ma classe runtime?
Uniquement si la classe runtime est conçue pour être utilisée depuis l’extérieur de son unité de compilation d’implémentation (c’est un composant Windows Runtime destiné à une utilisation générale par les applications clientes Windows Runtime). Pour plus d’informations sur l’objectif et les conséquences de la déclaration d’un ou de plusieurs constructeurs dans IDL, voir [Constructeurs de classe runtime](author-apis.md#runtime-class-constructors).