---
title: Composants Windows Runtime
description: Les composants Windows Runtime sont des objets autonomes que vous pouvez instancier et utiliser dans n’importe quel langage, notamment C\#, Visual Basic, JavaScript et C++.
ms.assetid: 55887622-828b-4318-87f2-25592268f7c1
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 76026c4f499a068bab689eadf050e0ec6277bbe8
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393682"
---
# <a name="windows-runtime-components"></a>Composants Windows Runtime
Les composants Windows Runtime sont des objets autonomes que vous pouvez instancier et utiliser dans n’importe quel langage, notamment C\#, Visual Basic, JavaScript et C++.

Vous pouvez utiliser Visual Studio et C#, Visual Basic ou C++ pour créer des composants Windows Runtime utilisables dans les applications de plateforme Windows universelle (UWP).

| Rubrique | Description |
|-------|-------------|
| [Composants Windows Runtime avec C++/CX](creating-windows-runtime-components-in-cpp.md) | Cette rubrique explique comment utiliser C++/CX pour créer un composant Windows Runtime&mdash;composant pouvant être appelé à partir d’une application Windows universelle générée avec n’importe quel langage Windows Runtime. |
| [Procédure pas à pas pour créer un composant C++/CX/Windows Runtime et l’appeler à partir de JavaScript ou C#](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md) | Cette procédure pas à pas indique comment créer une DLL de composant Windows Runtime de base qui peut être appelée à partir de JavaScript, C# ou Visual Basic. Avant d’entreprendre cette procédure pas à pas, vous devez maîtriser des concepts tels que l’interface binaire abstraite (ABI), les classes ref et les extensions des composants Visual C++ qui facilitent l’utilisation des classes ref. Pour plus d’informations, consultez [Création de composants Windows Runtime en C++](creating-windows-runtime-components-in-cpp.md) et [Informations de référence sur le langage Visual C++ (C++/CX)](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx). |
| [Composants Windows Runtime avec C# et Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) | Vous pouvez utiliser du code managé pour créer vos propres types Windows Runtime, empaquetés dans un composant Windows Runtime. Vous pouvez utiliser votre composant dans les applications de plateforme Windows universelle (UWP) avec C++, JavaScript, Visual Basic ou C#. Cette rubrique présente les règles de création d’un composant et décrit certains aspects de la prise en charge de .NET pour Windows Runtime. En règle générale, cette prise en charge est conçue pour être transparente pour les programmeurs .NET. Toutefois, lorsque vous créez un composant à utiliser avec JavaScript ou C++, vous devez tenir compte des différences de prise en charge de Windows Runtime par ces langages. |
| [Procédure pas à pas pour créer un composant C# ou Visual Basic et l’appeler à partir de JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) | Cette procédure pas à pas montre comment utiliser .NET avec Visual Basic ou C# pour créer vos propres types Windows Runtime, empaquetés dans un composant Windows Runtime, et comment appeler le composant à partir de votre application Windows universelle générée pour Windows à l’aide de JavaScript. |
| [Déclenchement d’événements dans les composants Windows Runtime](raising-events-in-windows-runtime-components.md) | Si votre composant Windows Runtime déclenche un événement d’un type délégué défini par l’utilisateur sur un thread d’arrière-plan (thread de travail) et que vous souhaitez que JavaScript puisse recevoir l’événement, vous pouvez l’implémenter ou le déclencher de plusieurs manières : | 
| [Composants Windows Runtime répartis pour les applications UWP à chargement indépendant](brokered-windows-runtime-components-for-side-loaded-windows-store-apps.md) | Cette rubrique présente une fonctionnalité de la mise à jour Windows 10 et versions supérieures, destinée aux entreprises, qui permet aux applications .NET tactiles d’utiliser le code responsable des opérations d’entreprise stratégiques. |
