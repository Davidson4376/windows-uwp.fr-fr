---
description: Ce guide vous aide à créer des interfaces utilisateur UWP Fluent directement dans vos applications WPF et Windows Forms
title: Contrôles UWP dans les applications de bureau
ms.date: 07/26/2019
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, îlots XAML
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 560d339476ef3cd45f30bfc678661fb0a4a11ee1
ms.sourcegitcommit: f6af7aeb8506379a184207035c8e43288cb31453
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68601540"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>Héberger des contrôles XAML UWP dans les applications de bureau (îlots XAML)

À compter de Windows 10, version 1903, vous pouvez héberger des contrôles UWP dans des applications de bureau non UWP à l’aide d’une fonctionnalité appelée *îlots XAML*. Cette fonctionnalité vous permet d’améliorer l’apparence, la convivialité et les fonctionnalités de vos applications de bureau existantes avec les dernières fonctionnalités de l’interface utilisateur de Windows 10 qui sont uniquement disponibles via les contrôles UWP. Cela signifie que vous pouvez utiliser des fonctionnalités UWP telles que [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) et des contrôles qui prennent en charge le [système de conception Fluent](/windows/uwp/design/fluent-design-system/index) dans vos applications C++ WPF, Windows Forms et Win32 existantes.

Nous fournissons plusieurs façons d’utiliser des îlots XAML dans vos applications WPF C++ , Windows Forms et Win32, en fonction de la technologie ou de l’infrastructure que vous utilisez. 

> [!NOTE]
> Si vous avez des commentaires sur les îlots XAML, créez un nouveau problème dans [Microsoft. Toolkit. Win32 référentiel](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) et laissez vos commentaires à cet endroit. Si vous préférez soumettre vos commentaires en privé, vous pouvez les envoyer à XamlIslandsFeedback@microsoft.com. Vos Insights et scénarios sont très importants pour nous.

## <a name="how-do-xaml-islands-work"></a>Fonctionnement des îlots XAML

À compter de Windows 10, version 1903, nous fournissons deux façons d’utiliser des îlots XAML dans vos applications C++ WPF, Windows Forms et Win32:

* L’SDK Windows fournit plusieurs classes Windows Runtime et interfaces COM que votre application peut utiliser pour héberger n’importe quel contrôle UWP dérivé de [**Windows. UI. Xaml. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement). Collectivement, ces classes et interfaces sont appelées l' *API d’hébergement XAML UWP*, et elles vous permettent d’héberger des contrôles UWP dans n’importe quel élément d’interface utilisateur de votre application auquel est associé un handle de fenêtre (HWND). Pour plus d’informations sur cette API, consultez [utilisation de l’API d’hébergement XAML](using-the-xaml-hosting-api.md).

* Le [Kit de pratiques de la communauté Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) fournit également des contrôles îlot XAML supplémentaires pour WPF et Windows Forms. Ces contrôles utilisent l’API d’hébergement XAML UWP en interne et implémentent tout le comportement que vous devrez sinon gérer vous-même si vous utilisiez l’API d’hébergement XAML UWP directement, y compris la navigation au clavier et les modifications de disposition. Pour les applications WPF et Windows Forms, nous vous recommandons vivement d’utiliser ces contrôles au lieu de l’API d’hébergement XAML UWP directement, car ils éliminent un grand nombre des détails d’implémentation de l’utilisation de l’API. Notez que depuis la version 1903 de Windows 10, ces contrôles sont [disponibles en version préliminaire](#feature-roadmap)pour les développeurs.

> [!NOTE]
> C++Les applications de bureau Win32 doivent utiliser l’API d’hébergement XAML UWP pour héberger des contrôles UWP. Les contrôles d’îlot XAML dans la boîte à outils de la communauté C++ Windows ne sont pas disponibles pour les applications de bureau Win32.

Il existe deux types de contrôles îlot XAML fournis par le kit de tâches de la communauté Windows pour les applications WPF et Windows Forms: *contrôles encapsulés* et *contrôles hôtes*. 

### <a name="wrapped-controls"></a>Contrôles encapsulés

Les applications WPF et Windows Forms peuvent utiliser une sélection de contrôles UWP encapsulés dans la boîte à outils de la [communauté Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Il s’agit de *contrôles encapsulés* , car ils encapsulent l’interface et les fonctionnalités d’un contrôle UWP spécifique. Vous pouvez ajouter ces contrôles directement à l’aire de conception de votre projet WPF ou Windows Forms, puis les utiliser comme n’importe quel autre contrôle WPF ou Windows Forms dans votre concepteur.

Les contrôles UWP suivants pour l’implémentation des îlots XAML sont actuellement disponibles pour WPF (consultez le package [Microsoft. Toolkit. WPF. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) ) et les applications de Windows Forms (consultez le package [Microsoft. Toolkit. Forms. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) ).

| Contrôle | Système d’exploitation minimal pris en charge | Description |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10, version 1903 | Fournissez une surface et des barres d’outils associées pour l’interaction utilisateur Windows Ink dans votre Windows Forms ou application de bureau WPF. |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10, version 1903 | Incorpore une vue qui diffuse en continu et restitue le contenu multimédia tel que la vidéo dans votre Windows Forms ou application de bureau WPF. |
| [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10, version 1903 | Vous permet d’afficher une carte symbolique ou photoréaliste dans votre Windows Forms ou dans une application de bureau WPF. |

En plus des contrôles encapsulés pour les îlots XAML, le kit de connaissances de la communauté Windows fournit également les contrôles suivants pour l’hébergement de contenu Web dans WPF (consultez le package [Microsoft. Toolkit. WPF. UI. Controls. WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView) ) et les applications de Windows Forms (consultez package [Microsoft. Toolkit. Forms. UI. Controls. WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView) ).

| Contrôle | Système d’exploitation minimal pris en charge | Description |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, version 1803 | Utilise le moteur de rendu Microsoft Edge pour afficher le contenu Web. |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | Fournit une version de **WebView** qui est compatible avec d’autres versions de système d’exploitation. Ce contrôle utilise le moteur de rendu Microsoft Edge pour afficher le contenu Web sur Windows 10 version 1803 et ultérieure, et le moteur de rendu d’Internet Explorer pour afficher le contenu Web sur les versions antérieures de Windows 10, Windows 8. x et Windows 7. |

### <a name="host-controls"></a>Contrôles hôtes

Pour les scénarios au-delà de ceux couverts par les contrôles encapsulés disponibles, les applications WPF et Windows Forms peuvent également utiliser le contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) dans le kit de fonctionnalités de la [communauté Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/). Ce contrôle peut héberger n’importe quel contrôle UWP dérivé de [**Windows. UI. Xaml. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), y compris tout contrôle UWP fourni par le SDK Windows, ainsi que des contrôles utilisateur personnalisés. Ce contrôle prend en charge Windows 10 Insider Preview SDK Build 17709 et versions ultérieures.

Ces contrôles sont disponibles dans les packages [Microsoft. Toolkit. WPF. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) (pour WPF) et [Microsoft. Toolkit. Forms. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost) (pour Windows Forms). Ces packages sont inclus dans les packages [Microsoft. Toolkit. WPF. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) et [Microsoft. Toolkit. Forms. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) qui contiennent les contrôles encapsulés.

### <a name="architecture-overview"></a>Présentation de l'architecture

Voici un aperçu de la façon dont ces contrôles sont organisés du point de vue architectural.

![Architecture de contrôle de l'hôte](images/xaml-islands/host-controls.png)

Les API qui s’affichent en bas de ce diagramme sont livrés avec le SDK Windows. Les contrôles encapsulés et les contrôles hôtes sont disponibles via des packages NuGet dans la boîte à outils de la communauté Windows.

<span id="requirements" />

## <a name="configure-your-project-to-use-xaml-islands"></a>Configurer votre projet pour utiliser des îlots XAML

Les îlots XAML requièrent Windows 10, version 1903 et versions ultérieures. Pour utiliser des îlots XAML dans votre application, vous devez d’abord configurer votre projet:

1. Modifiez votre projet pour qu’il utilise des API Windows Runtime. Pour obtenir des instructions, consultez [cet article](desktop-to-uwp-enhance.md#set-up-your-project).
2. Installez l’un de ces packages NuGet dans votre projet. Veillez à installer la version 6.0.0-Preview 6.4 ou une version ultérieure du package.
    * WPF Installer [Microsoft. Toolkit. WPF. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)
    * Windows Forms: [Microsoft. Toolkit. Forms. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)
    * C++32 [Microsoft. Toolkit. Win32. UI. XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication)

> [!NOTE]
> Dans les versions antérieures de ces instructions, vous deviez ajouter l’élément **maxversiontested** à un manifeste d’application de votre projet. À compter de la dernière version d’évaluation des packages NuGet, vous n’avez plus besoin d’ajouter cet élément à votre manifeste.

## <a name="feature-roadmap"></a>Plan des fonctionnalités

À compter de la sortie de Windows 10, la version 1903, les contrôles encapsulés et les contrôles hôtes de la boîte à outils de la communauté Windows sont toujours en version préliminaire pour les développeurs jusqu’à ce que la version 1,0 des contrôles soit disponible.

* La version 1,0 des contrôles pour le .NET Framework 4.6.2 et versions ultérieures doit être publiée dans la [version 6,0 de la boîte à outils](https://github.com/windows-toolkit/WindowsCommunityToolkit/milestones).
* La version 1,0 des contrôles pour .NET Core 3 est prévue pour une version ultérieure de la boîte à outils.
* Si vous souhaitez tester les versions les plus récentes des versions 1,0 de ces contrôles pour le .NET Framework et .NET Core 3, consultez les packages NuGet **-preview 6.4** NuGet dans la Galerie de la [boîte à outils de la communauté UWP](https://dotnet.myget.org/gallery/uwpcommunitytoolkit) .

Pour plus d’informations, consultez [ce billet de blog](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap).

## <a name="additional-resources"></a>Ressources supplémentaires

Pour plus d’informations générales et pour obtenir des didacticiels sur l’utilisation des îlots XAML, consultez les articles et ressources suivants:

* [Didacticiel de modernisation d’une application WPF](modernize-wpf-tutorial.md): Ce didacticiel fournit des instructions pas à pas pour l’utilisation des contrôles encapsulés et des contrôles hôtes dans la boîte à outils de la communauté Windows pour ajouter des contrôles UWP à une application métier WPF existante. Ce didacticiel comprend le code complet de l’application WPF, ainsi que des instructions détaillées pour chaque étape du processus.
* [Îlots XAML v1-mises à jour et](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap)feuille de route: Ce billet de blog présente de nombreuses questions courantes sur les îlots XAML et fournit un plan de développement détaillé.
