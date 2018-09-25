---
author: normesta
description: Ce guide vous aide à créer des interfaces utilisateur UWP Fluent directement dans vos applications WPF et Windows Forms
title: Contrôles UWP dans les applications de bureau
ms.author: normesta
ms.date: 09/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp, windows forms, wpf
keywords: windows10, uwp, windows forms, wpf
ms.localizationpriority: medium
ms.openlocfilehash: 6b8c263b030cbb8f945ffb13a24b6dff3af28fcc
ms.sourcegitcommit: 232543fba1fb30bb1489b053310ed6bd4b8f15d5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/25/2018
ms.locfileid: "4173632"
---
# <a name="uwp-controls-in-desktop-applications"></a>Contrôles UWP dans les applications de bureau

> [!NOTE]
> Les API et les contrôles mentionnés dans cet article sont actuellement disponibles sous forme d’un version préliminaire pour développeurs. Bien que nous vous encourageons à les tester dans votre propre code prototype maintenant, nous ne recommandons pas que vous les utiliser dans le code de production pour l’instant. Ces API et les contrôles continueront à mûrir et stabiliser dans les futures versions de Windows. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.

Windows 10 vous permet désormais d’utiliser les contrôles UWP dans les applications de bureau non UWP afin que vous pouvez améliorer l’apparence et la fonctionnalité de vos applications de bureau existantes avec les dernières fonctionnalités de l’interface utilisateur de Windows 10 qui sont uniquement disponibles via les contrôles UWP. Cela signifie que vous pouvez utiliser les fonctionnalités UWP telles que le [Système Fluent Design](../design/fluent-design-system/index.md) et [Windows Ink](../design/input/pen-and-stylus-interactions.md) dans votre existant WPF, Windows Forms et les applications Win32 C++. Ce scénario développeur est parfois appelé *îles XAML*.

Nous proposons différentes méthodes à utiliser (îles) XAML dans vos applications de bureau, en fonction de la technologie ou infrastructure que vous utilisez.

## <a name="wrapped-controls"></a>Contrôles inclus dans un wrapper

Applications WPF et Windows Forms peuvent utiliser une sélection de contrôles UWP encapsulés dans le [Kit de ressources de la Communauté Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Vous pouvez ajouter ces contrôles directement à l’aire de conception de votre projet WPF ou Windows Forms, puis utiliser comme tout autre contrôle WPF ou Windows Forms dans votre concepteur. Nous appelons à ces contrôles *encapsulé des contrôles* dans la mesure où ils encapsulent l’interface et les fonctionnalités d’un contrôle UWP spécifique.

Les contrôles inclus dans un wrapper suivants prennent en charge Windows 10, version 1803 et versions ultérieure.

* [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview). Ce contrôle utilise le moteur de rendu Microsoft Edge pour afficher le contenu web dans une application WPF ou Windows Forms.
* [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible). Ce contrôle est une version de **WebView** compatible avec Windows 10 et les versions précédentes de Windows. Ce contrôle utilise le moteur de rendu Microsoft Edge pour afficher le contenu web sur Windows 10 (version 1803 et versions ultérieure) et le moteur de rendu d’Internet Explorer pour afficher le contenu web sur Windows 7 et Windows 8.x.

Les contrôles inclus dans un wrapper suivants prennent en charge les versions 17709 et versions ultérieures du Kit de développement logiciel Windows 10 Insider Preview build.

* [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) et [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar). Ces contrôles fournissent une surface et barres d’outils pour l’interaction utilisateur basée sur Windows Ink dans votre application de bureau Windows Forms ou WPF.
* [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement). Ce contrôle incorpore une vue qui diffuse et affiche le contenu multimédia comme vidéo dans votre application de bureau Windows Forms ou WPF.

UWP plus encapsulée contrôles pour WPF et Windows Forms applications sont prévues pour les futures versions de Windows Community Toolkit.

> [!NOTE]
> Les contrôles inclus dans un wrapper ne sont pas disponibles pour les applications de bureau Win32 C++. Ces types d’applications doivent utiliser l' [API d’hébergement de XAML UWP](#uwp-xaml-hosting-api).

## <a name="host-controls"></a>Contrôles hôtes

Pour les scénarios au-delà de celles couvertes par les contrôles encapsulés disponibles, applications WPF et Windows Forms peuvent également utiliser le contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) dans le [Kit de ressources de la Communauté Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Ce contrôle peut héberger n’importe quel contrôle UWP qui dérive de [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), y compris n’importe quel contrôle UWP fourni par le Kit de développement Windows, ainsi que des contrôles utilisateur personnalisés. Ce contrôle prend en charge les versions 17709 et versions ultérieures du Kit de développement logiciel Windows 10 Insider Preview build.

> [!NOTE]
> Hébergez des contrôles ne sont pas disponibles pour les applications de bureau Win32 C++. Ces types d’applications doivent utiliser l' [API d’hébergement de XAML UWP](#uwp-xaml-hosting-api).

## <a name="uwp-xaml-hosting-api"></a>XAML UWP API d’hébergement

Si vous avez une application Win32 C++, vous pouvez utiliser l' *API d’hébergement de XAML UWP* pour héberger n’importe quel contrôle UWP qui dérive de [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) dans n’importe quel élément d’interface utilisateur dans votre application qui comporte un handle de fenêtre associée (HWND). Cette API a été introduite dans Windows 10 Insider Preview SDK build 17709. Pour plus d’informations sur l’utilisation de cette API, reportez-vous [à l’aide de l’API dans une application de bureau d’hébergement de XAML](using-the-xaml-hosting-api.md).

> [!NOTE]
> Les applications de bureau Win32 C++ doivent utiliser le XAML UWP hébergeant des API pour héberger des contrôles UWP. Contrôles encapsulés et hébergez des contrôles ne sont pas disponibles pour ces types d’applications. Pour les applications WPF et Windows Forms, nous vous recommandons d’utiliser les contrôles encapsulés et hôte dans le Kit de ressources de la Communauté Windows au lieu du XAML UWP hébergeant des API. Ces contrôles utilisent le XAML UWP API d’hébergement en interne et fournissent une expérience de développement plus simple. Toutefois, vous pouvez utiliser l’API d’hébergement directement dans vos applications WPF et Windows Forms si vous choisissez de XAML UWP.

## <a name="architecture-overview"></a>Vue d’ensemble de l’architecture

Voici un aperçu de la façon dont ces contrôles sont organisés du point de vue architectural. Les noms utilisés dans ce diagramme sont sujets à modification.  

![Architecture de contrôle de l'hôte](images/host-controls.png)

Les API qui s’affichent en bas de ce diagramme sont livrés avec le SDK Windows. Les contrôles que vous allez ajouter à votre concepteur sont fournis sous forme de packages Nuget dans le Kit de ressources de la Communauté Windows.

Ces nouveaux contrôles ayant des limitations, prenez le temps, avant de les utiliser, d’examiner ce qui n’est pas encore pris en charge et ce qui ne fonctionne qu'avec des solutions de contournement.

## <a name="limitations"></a>Limitations

### <a name="whats-supported"></a>Fonctionnalités prises en charge

Pour l’essentiel, tout est pris en charge, sauf ce qui est explicitement indiqué sur la liste ci-dessous.

### <a name="whats-supported-only-with-workarounds"></a>Fonctionnalités prises en charge uniquement avec des solutions de contournement

:heavy_check_mark: héberge plusieurs contrôles de la boîte de réception dans les fenêtres multiples. Vous devrez placer chaque fenêtre dans son propre thread.

:heavy_check_mark: utilisation de ``x:Bind`` avec des contrôles hébergés. Vous devrez déclarer le modèle de données dans une bibliothèque .NET Standard.

:heavy_check_mark: contrôles tiers en C#. Si vous avez le code source d'un contrôle tiers, vous pouvez compiler en fonction de celui-ci.

### <a name="whats-not-yet-supported"></a>Fonctionnalités non encore prises en charge

:no_entry_sign: outils d'accessibilité qui fonctionnent en toute transparence dans l’application et les contrôles hébergés.

:no_entry_sign: contenu localisé dans les contrôles que vous ajoutez à des applications qui ne contiennent pas de package d’application Windows.

:no_entry_sign: références de ressources en XAML dans des applications qui ne contiennent pas de package d’application Windows.

:no_entry_sign: contrôles qui répondent correctement aux modifications de PPP et d'échelle.

:no_entry_sign: ajout d'un contrôle **WebView** à un contrôle utilisateur personnalisé (sur thread, hors thread ou hors processus).

:no_entry_sign: l'effet Fluent [surlignage Révéler](https://docs.microsoft.com/windows/uwp/design/style/reveal).

: no_entry_sign: entrée manuscrite Inline, @Places et @People pour les contrôles d’entrée.

:no_entry_sign: assignation de touches accélératrices.

:no_entry_sign: contrôles tiers en C++.

:no_entry_sign: hébergement de contrôles utilisateur personnalisés.

Les éléments de cette liste sont susceptibles de changer à mesure que nous continuons à améliorer l’’intégration de Fluent dans les ordinateurs de bureau.  
