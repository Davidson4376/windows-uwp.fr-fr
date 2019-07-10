---
description: Photo Editor est un exemple d’application UWP qui illustre le développement à l’aide de la projection de langage C++/WinRT. L’exemple d’application vous permet de récupérer des photos à partir de la bibliothèque Images, puis de modifier l’image sélectionnée avec des effets de photo assortis.
title: Exemple d’application C++/WinRT Photo Editor
ms.date: 04/23/2019
ms.topic: article
keywords: Windows 10, uwp, standard, c++, cpp, winrt, projection, exemple, application, photo, éditeur
ms.localizationpriority: medium
ms.openlocfilehash: bbac9d6a6d08d327bf1bf55de5e86cbb1edd51ea
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64745190"
---
# <a name="photo-editor-cwinrt-sample-application"></a>Exemple d’application C++/WinRT Photo Editor

> [!NOTE]
> L’exemple est ciblé et testé pour Windows 10, version 1903 (10.0 ; Build 18362) et Visual Studio 2019. Si vous préférez, vous pouvez utiliser les propriétés du projet pour recibler le(s) projet(s) vers Windows 10, version 1809 (10.0 ; Build 17763), et/ou ouvrir l’exemple avec Visual Studio 2017.

Vous pouvez dupliquer ou télécharger l’exemple d’application à partir du référentiel [Exemple d’application C++/WinRT Photo Editor](https://github.com/Microsoft/Windows-appsample-photo-editor) GitHub.

L'application Photo Editor est un exemple d’application de plateforme Windows universelle (UWP) qui illustre le développement à l'aide de la projection de langage [C++/WinRT](intro-to-using-cpp-with-winrt.md). L’exemple d’application vous permet de récupérer des photos à partir de la bibliothèque **Images**, puis de modifier l’image sélectionnée avec des effets de photo assortis. Dans le code source de l’exemple, vous verrez un certain nombre de pratiques courantes&mdash;comme la [liaison de données](binding-property.md) et les [actions et opérations asynchrones](concurrency.md)&mdash;effectuées à l’aide de la projection C++/WinRT. Voici quelques-unes des fonctionnalités spécifiques illustrées par l’exemple.

- Utilisation de la syntaxe et des bibliothèques de la norme C++17 avec les API Windows Runtime (WinRT).
- Utilisation des coroutines, notamment l’utilisation de co_await, co_return, [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) et [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_).
- Création et utilisation des types projetés et des types d’implémentation de la classe Windows Runtime personnalisée (classe runtime). Pour plus d’informations sur ces termes, consultez [Utiliser des API avec C++/WinRT](consume-apis.md) et [Créer des API avec C++/WinRT](author-apis.md).
- [Gestion des événements](handle-events.md), notamment l’utilisation de jetons d’événements à révocation automatique.
- Utilisation du package NuGet Win2D externe et de [Windows::UI::Composition](/uwp/api/windows.ui.composition), pour les effets d’image.
- Liaison de données XAML, notamment l'[extension de balisage {x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension).
- Création de styles et personnalisation de l’interface utilisateur en XAML, notamment avec l'utilisation des [animations connectées](../design/motion/connected-animation.md).
