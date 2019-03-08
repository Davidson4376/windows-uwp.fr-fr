---
description: Ce guide vous aide à créer des interfaces utilisateur UWP Fluent directement dans vos applications WPF et Windows Forms
title: Contrôles UWP dans des applications de bureau
ms.date: 01/11/2019
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bf25fea6ca6e8809c12324ae57a42cc712ded2a5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57619704"
---
# <a name="uwp-controls-in-desktop-applications"></a>Contrôles UWP dans des applications de bureau

> [!NOTE]
> Îles XAML sont actuellement disponibles en version préliminaire de développeur. Bien que nous vous encourageons à les tester dans votre propre code de prototype maintenant, il est déconseillé d’utiliser leur code de production pour l’instant. Ces API et les contrôles continuera à maturité et stabiliser dans les futures versions de Windows. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.
>
> Si vous avez des commentaires concernant les îles XAML, créez un nouveau problème dans le [WindowsCommunityToolkit référentiel](https://github.com/windows-toolkit/WindowsCommunityToolkit/issues) et y laisser vos commentaires. Si vous préférez envoyer vos commentaires en privé, vous pouvez l’envoyer à XamlIslandsFeedback@microsoft.com. Vos idées et les scénarios sont extrêmement importantes pour nous.

Windows 10 vous permet désormais d’utiliser les contrôles UWP dans les applications de bureau non UWP afin que vous pouvez améliorer l’apparence et les fonctionnalités de vos applications de bureau existantes avec les dernières fonctionnalités de l’interface utilisateur de Windows 10 qui sont uniquement disponibles via les contrôles UWP. Cela signifie que vous pouvez utiliser les fonctionnalités UWP comme [Windows Ink](../design/input/pen-and-stylus-interactions.md) et les contrôles qui prennent en charge la [Fluent Design System](../design/fluent-design-system/index.md) dans votre existante, Windows Forms, applications WPF et Win32 C++. Ce scénario de développement est parfois appelé *(îles) XAML*.

Nous fournissons plusieurs façons d’utiliser des îlots de XAML dans vos applications WPF, Windows Forms et Win32 C++, selon la technologie ou d’un framework que vous utilisez.

## <a name="wrapped-controls"></a>Contrôles inclus dans un wrapper

Les applications WPF et Windows Forms peuvent utiliser une sélection de contrôles UWP encapsulées dans la [Kit de ressources de communauté Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Nous faisons référence à ces contrôles en tant que *encapsulé les contrôles* , car elles encapsulent l’interface et les fonctionnalités d’un contrôle UWP spécifique. Vous pouvez ajouter ces contrôles directement à l’aire de conception de votre projet WPF ou Windows Forms, puis de les utiliser comme tout autre contrôle WPF ou Windows Forms dans votre concepteur.

> [!NOTE]
> Les contrôles inclus dans un wrapper ne sont pas disponibles pour les applications de bureau Win32 C++. Ces types d’applications doivent utiliser le [XAML UWP API d’hébergement](#uwp-xaml-hosting-api).

Les contrôles UWP encapsulées suivants sont actuellement disponibles pour les applications WPF et Windows Forms. Plus de contrôles UWP encapsulé sont prévus pour les versions futures de la boîte à outils de la Communauté Windows.

| Commande | Minimale prise en charge du système d’exploitation | Description |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, version 1803 | Utilise le moteur de rendu Microsoft Edge pour afficher le contenu web. |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | Fournit une version de **WebView** qui est compatible avec plusieurs versions de système d’exploitation. Ce contrôle utilise le moteur de rendu Microsoft Edge pour afficher le contenu web sur Windows 10 version 1803 et versions ultérieure et le moteur de rendu d’Internet Explorer pour afficher le contenu web sur les versions antérieures de Windows 10, Windows 8.x et Windows 7. |
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10, version 1809 (build 17763) | Fournir un barres d’outils de l’aire de conception et connexes pour l’interaction utilisateur Windows encrage dans votre application de bureau Windows Forms ou WPF. |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10, version 1809 (build 17763) | Incorpore une vue qui transmet en continu et restitue le contenu multimédia par exemple une vidéo dans votre application de bureau Windows Forms ou WPF. |
| [MapControl](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10, version 1809 (build 17763) | Vous permet d’afficher une carte symbolique ou photoréaliste dans votre application de bureau Windows Forms ou WPF. |

## <a name="host-controls"></a>Contrôles hôtes

Pour les scénarios, au-delà de ceux couverts par les contrôles inclus dans un wrapper disponibles, WPF et Windows Forms applications peuvent également utiliser le [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) dans contrôler le [Kit de ressources de communauté Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Ce contrôle peut héberger n’importe quel contrôle UWP qui dérive de [ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), y compris de n’importe quel contrôle UWP fournis par le Kit de développement Windows, ainsi que les contrôles utilisateur personnalisés. Ce contrôle prend en charge le SDK Windows 10 Insider Preview build 17709 et versions ultérieures.

> [!NOTE]
> Contrôles hôtes ne sont pas disponibles pour les applications de bureau Win32 C++. Ces types d’applications doivent utiliser le [XAML UWP API d’hébergement](#uwp-xaml-hosting-api).

## <a name="uwp-xaml-hosting-api"></a>XAML UWP API d’hébergement

Si vous avez une application Win32 C++, vous pouvez utiliser la *XAML UWP API d’hébergement* pour héberger n’importe quel contrôle UWP qui dérive de [ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) dans n’importe quel élément d’interface utilisateur dans votre application qui a un handle de fenêtre associée (HWND). Cette API a été introduite dans le SDK Windows 10 Insider Preview build 17709. Pour plus d’informations sur l’utilisation de cette API, consultez [à l’aide de l’API d’hébergement dans une application de bureau de XAML](using-the-xaml-hosting-api.md).

> [!NOTE]
> Les applications de bureau Win32 C++ doivent utiliser le XAML UWP API d’hébergement pour héberger des contrôles UWP. Les contrôles inclus dans un wrapper et des contrôles hôtes ne sont pas disponibles pour ces types d’applications. Pour les applications WPF et Windows Forms, nous vous recommandons d’utiliser les contrôles inclus dans un wrapper et les contrôles hôtes dans le Kit de ressources de communauté Windows au lieu du XAML UWP API d’hébergement. Ces contrôles utilisent le XAML UWP API d’hébergement en interne et fournissent une expérience de développement plus simple. Toutefois, vous pouvez utiliser le XAML UWP API d’hébergement directement dans les applications WPF et Windows Forms si vous choisissez.

## <a name="architecture-overview"></a>Présentation de l'architecture

Voici un aperçu de la façon dont ces contrôles sont organisés du point de vue architectural. Les noms utilisés dans ce diagramme sont sujets à modification.  

![Architecture de contrôle de l'hôte](images/host-controls.png)

Les API qui s’affichent en bas de ce diagramme sont livrés avec le SDK Windows. Les contrôles que vous allez ajouter à votre concepteur sont fournis sous forme de packages Nuget dans le Kit de ressources de la Communauté Windows.

Ces nouveaux contrôles ayant des limitations, prenez le temps, avant de les utiliser, d’examiner ce qui n’est pas encore pris en charge et ce qui ne fonctionne qu'avec des solutions de contournement.

## <a name="limitations"></a>Limitations

### <a name="whats-supported"></a>Fonctionnalités prises en charge

Pour l’essentiel, tout est pris en charge, sauf ce qui est explicitement indiqué sur la liste ci-dessous.

### <a name="whats-supported-only-with-workarounds"></a>Fonctionnalités prises en charge uniquement avec des solutions de contournement

:heavy_check_mark: Hébergement de plusieurs contrôles de boîte de réception à l’intérieur de plusieurs fenêtres. Vous devrez placer chaque fenêtre dans son propre thread.

:heavy_check_mark: À l’aide de ``x:Bind`` avec les contrôles hébergés. Vous devrez déclarer le modèle de données dans une bibliothèque .NET Standard.

:heavy_check_mark: C#-en fonction des contrôles tiers. Si vous avez le code source d'un contrôle tiers, vous pouvez compiler en fonction de celui-ci.

### <a name="whats-not-yet-supported"></a>Fonctionnalités non encore prises en charge

: no_entry_sign : Outils d’accessibilité qui fonctionnent de façon transparente dans toute l’application et les contrôles hébergés.

: no_entry_sign : Contenu localisé dans les contrôles que vous ajoutez aux applications qui ne contiennent pas un package d’application Windows.

: no_entry_sign : Références de ressources effectuées dans XAML au sein d’applications qui ne contiennent pas un package d’application Windows.

: no_entry_sign : Contrôles de répondre correctement aux modifications de résolution et de mise à l’échelle.

: no_entry_sign : Ajout d’un **WebView** vers un contrôle utilisateur personnalisé, (sur thread, off-thread ou en dehors de la procédure).

: no_entry_sign : Le [mise en surbrillance révèlent](https://docs.microsoft.com/windows/uwp/design/style/reveal) effet Fluent.

: no_entry_sign : Inline pour l’écriture manuscrite, @Places, et @People pour les contrôles d’entrée.

: no_entry_sign : Affectation des touches accélérateur.

: no_entry_sign : Basé sur C++ des contrôles tiers.

: no_entry_sign : Hébergement de contrôles utilisateur personnalisés.

Les éléments de cette liste sont susceptibles de changer à mesure que nous continuons à améliorer l’’intégration de Fluent dans les ordinateurs de bureau.  
