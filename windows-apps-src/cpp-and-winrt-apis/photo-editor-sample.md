---
author: JoshuaPartlow
description: Photo Editor est un exemple d’application UWP qui illustre le développement à l'aide de la projection de langage C++/WinRT. L’exemple d’application vous permet de récupérer des photos à partir de la bibliothèque Images, puis de modifier l’image sélectionnée avec des effets de photo assortis.
title: Exemple d’application C++/WinRT Photo Editor
ms.author: wdg-dev-content
ms.date: 06/08/2018
ms.topic: article
keywords: Windows10, uwp, standard, c++, cpp, winrt, projection, exemple, application, photo, éditeur
ms.localizationpriority: medium
ms.openlocfilehash: 60bfcd79ed2d659aff8d435bd397df05eb45af72
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/14/2018
ms.locfileid: "6861312"
---
# <a name="photo-editor-cwinrt-sample-application"></a>Exemple d’application C++/WinRT Photo Editor
Vous pouvez dupliquer ou télécharger l’exemple d’application à partir du référentiel [Exemple d’application C++/WinRT Photo Editor](https://github.com/Microsoft/Windows-appsample-photo-editor) GitHub.

L'application Photo Editor est un exemple d’application de plateforme Windows universelle (UWP) qui illustre le développement à l'aide de la projection de langage [C++/WinRT](intro-to-using-cpp-with-winrt.md). L’exemple d’application vous permet de récupérer des photos à partir de la bibliothèque **Images**, puis de modifier l’image sélectionnée avec des effets de photo assortis. Dans le code source de l’exemple, vous verrez un certain nombre de pratiques courantes&mdash;comme la [liaison de données](binding-property.md) et les [actions et opérations asynchrones](concurrency.md)&mdash;effectuées à l’aide de la projection C++/WinRT. Voici quelques-unes des fonctionnalités spécifiques illustrées par l’exemple.
    
- Utilisation de la syntaxe et des bibliothèques de la norme C++17 avec les API Windows Runtime (WinRT).
- Utilisation des coroutines, notamment l’utilisation de co_await, co_return, [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) et [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_).
- Création et utilisation des types projetés et des types d’implémentation de la classe Windows Runtime personnalisée (classe runtime). Pour plus d’informations sur ces termes, consultez [Utiliser des API avec C++/WinRT](consume-apis.md) et [Créer des API avec C++/WinRT](author-apis.md).
- [Gestion des événements](handle-events.md), notamment l’utilisation de jetons d’événements à révocation automatique.
- Utilisation du package NuGet Win2D externe et de [Windows::UI::Composition](/uwp/api/windows.ui.composition), pour les effets d’image.
- Liaison de données XAML, notamment l'[extension de balisage {x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension).
- Création de styles et personnalisation de l’interface utilisateur en XAML, notamment avec l'utilisation des [animations connectées](../design/motion/connected-animation.md).
