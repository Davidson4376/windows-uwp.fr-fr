---
description: Ce guide vous aide à créer des interfaces utilisateur UWP Fluent directement dans vos applications WPF et Windows Forms
title: Contrôles UWP dans les applications de bureau
ms.date: 08/20/2019
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, îlots XAML
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: bd49417d110759dc9fec4ff4c9003e842bf1d7bb
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643350"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>Héberger des contrôles XAML UWP dans les applications de bureau (îlots XAML)

À compter de Windows 10, version 1903, vous pouvez héberger des contrôles UWP dans des applications de bureau non UWP à l’aide d’une fonctionnalité appelée *îlots XAML*. Cette fonctionnalité vous permet d’améliorer l’apparence, la convivialité et les fonctionnalités de vos applications WPF, Windows Forms et C++ Win32 existantes avec les dernières fonctionnalités de l’interface utilisateur de Windows 10 qui sont uniquement disponibles via les contrôles UWP. Cela signifie que vous pouvez utiliser des fonctionnalités UWP telles que [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) et des contrôles qui prennent en charge le [système de conception Fluent](/windows/uwp/design/fluent-design-system/index) dans vos applications C++ WPF, Windows Forms et Win32 existantes.

Vous pouvez héberger n’importe quel contrôle UWP dérivé de [Windows. UI. Xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), y compris:

* Tout contrôle UWP de première partie fourni par la bibliothèque SDK Windows ou WinUI.
* Tout contrôle UWP personnalisé (par exemple, un contrôle utilisateur qui se compose de plusieurs contrôles UWP qui fonctionnent ensemble). Vous devez disposer du code source du contrôle personnalisé pour pouvoir le compiler avec votre application.

Fondamentalement, les îlots XAML sont créés à l’aide de l' *API d’hébergement XAML UWP*. Cette API est constituée de plusieurs classes de Windows Runtime et d’interfaces COM qui ont été introduites dans le kit de développement logiciel (SDK) Windows 10, version 1903. Nous fournissons également un ensemble de contrôles .NET insulaires dans la boîte à outils de la [communauté Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) qui utilisent l’API d’hébergement XAML UWP en interne et fournissent une expérience de développement plus pratique pour WPF et les applications de Windows Forms.

La façon dont vous utilisez les îlots XAML dépend de votre type d’application et des types de contrôles UWP que vous souhaitez héberger.

> [!NOTE]
> Si vous avez des commentaires sur les îlots XAML, créez un nouveau problème dans [Microsoft. Toolkit. Win32 référentiel](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) et laissez vos commentaires à cet endroit. Si vous préférez soumettre vos commentaires en privé, vous pouvez les envoyer à XamlIslandsFeedback@microsoft.com. Vos Insights et scénarios sont très importants pour nous.

## <a name="wpf-and-windows-forms-applications"></a>Applications WPF et Windows Forms

Nous recommandons que les applications WPF et Windows Forms utilisent les contrôles .NET îlot .NET disponibles dans le kit de pratiques de la communauté Windows. Ces contrôles fournissent un modèle objet qui imite (ou donne accès à) les propriétés, les méthodes et les événements des contrôles UWP correspondants. Ils gèrent également les comportements tels que la navigation au clavier et les modifications de disposition.

Il existe deux jeux de contrôles d’îlot XAML pour les applications WPF et Windows Forms: les *contrôles encapsulés* et les *contrôles hôtes*. À compter de Windows 10, la version 1903, ces contrôles sont [disponibles en version préliminaire](#feature-roadmap)pour les développeurs.

### <a name="wrapped-controls"></a>Contrôles encapsulés

Les applications WPF et Windows Forms peuvent utiliser une sélection de contrôles d’îlot XAML qui encapsulent l’interface et les fonctionnalités d’un contrôle UWP spécifique. Vous pouvez ajouter ces contrôles directement à l’aire de conception de votre projet WPF ou Windows Forms, puis les utiliser comme n’importe quel autre contrôle WPF ou Windows Forms dans le concepteur.

Les contrôles UWP suivants sont actuellement disponibles dans la boîte à outils de la communauté Windows. 

| Contrôle | Système d’exploitation minimal pris en charge | Description |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10, version 1903 | Fournissez une surface et des barres d’outils associées pour l’interaction utilisateur Windows Ink dans votre Windows Forms ou application de bureau WPF. |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10, version 1903 | Incorpore une vue qui diffuse en continu et restitue le contenu multimédia tel que la vidéo dans votre Windows Forms ou application de bureau WPF. |
| [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10, version 1903 | Vous permet d’afficher une carte symbolique ou photoréaliste dans votre Windows Forms ou dans une application de bureau WPF. |

Pour obtenir une procédure pas à pas qui montre comment utiliser les contrôles UWP encapsulés, consultez [héberger un contrôle UWP standard dans une application WPF](host-standard-control-with-xaml-islands.md).

### <a name="host-controls"></a>Contrôles hôtes

Pour les scénarios allant au-delà de ceux couverts par les contrôles encapsulés disponibles, les applications WPF et Windows Forms peuvent également utiliser le contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) disponible dans la boîte à outils de la communauté Windows.

| Contrôle | Système d’exploitation minimal pris en charge | Description |
|-----------------|-------------------------------|-------------|
| [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) | Windows 10, version 1903 | Peut héberger n’importe quel contrôle UWP dérivé de [Windows. UI. Xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), y compris les contrôles UWP internes fournis par le SDK Windows, ainsi que des contrôles personnalisés. |

Pour obtenir des procédures pas à pas qui montrent comment utiliser le contrôle **WindowsXamlHost** , consultez [héberger un contrôle UWP standard dans une application WPF](host-standard-control-with-xaml-islands.md) et [héberger un contrôle UWP personnalisé dans une application WPF à l’aide des îlots XAML](host-custom-control-with-xaml-islands.md).

> [!NOTE]
> L’utilisation du contrôle **WindowsXamlHost** pour héberger des contrôles UWP personnalisés est prise en charge uniquement dans les applications WPF et Windows Forms qui ciblent .net Core 3. L’hébergement des contrôles UWP internes fournis par le SDK Windows est pris en charge dans les applications qui ciblent le .NET Framework ou .NET Core 3.

<span id="requirements" />

### <a name="configure-your-project-to-use-the-xaml-island-net-controls"></a>Configurer votre projet pour utiliser les contrôles .NET îlot

Les contrôles .NET de l’îlot XAML requièrent Windows 10, version 1903 ou une version ultérieure. Pour utiliser ces contrôles, installez l’un des packages NuGet listés ci-dessous. Ces packages fournissent tout ce dont vous avez besoin pour utiliser les contrôles encapsulés et les contrôles hôtes de l’îlot XAML, et incluent d’autres packages NuGet connexes qui sont également requis.

| Type de contrôle | Package NuGet  | Articles connexes |
|-----------------|----------------|---------------------|
| [Contrôles encapsulés](#wrapped-controls) | Version 6.0.0-preview7 ou ultérieure de ces packages: <ul><li>WPF [Microsoft. Toolkit. WPF. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)</li><li>Windows Forms: [Microsoft. Toolkit. Forms. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)</li></ul>  | [Héberger un contrôle UWP standard dans une application WPF](host-standard-control-with-xaml-islands.md)  |
| [Contrôle hôte](#host-controls) | Version 6.0.0-preview7 ou ultérieure de ces packages: <ul><li>WPF [Microsoft. Toolkit. WPF. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost)</li><li>Windows Forms: [Microsoft. Toolkit. Forms. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost)</li></ul>  | [Héberger un contrôle UWP standard dans une application WPF](host-standard-control-with-xaml-islands.md)<br/>[Héberger un contrôle UWP personnalisé dans une application WPF](host-custom-control-with-xaml-islands.md)  |

Tenez compte des détails suivants:

* Les packages de contrôle hôte sont également inclus dans les packages de contrôle encapsulé. Vous pouvez installer les packages de contrôle encapsulé si vous souhaitez utiliser les deux ensembles de contrôles.

* Si vous hébergez un contrôle UWP personnalisé, votre projet WPF ou Windows Forms doit cibler .NET Core 3. L’hébergement de contrôles UWP personnalisés n’est pas pris en charge dans les applications qui ciblent le .NET Framework. Vous devez également effectuer quelques étapes supplémentaires pour référencer le contrôle personnalisé. Pour plus d’informations, consultez [héberger un contrôle UWP personnalisé dans une application WPF à l’aide des îlots XAML](host-custom-control-with-xaml-islands.md).

* Dans les versions antérieures de ces instructions, vous `maxversiontested` aviez ajouté l’élément à un manifeste d’application dans votre projet WPF ou Windows Forms. Tant que vous utilisez les versions préliminaires les plus récentes des packages NuGet listés ci-dessus, vous n’avez plus besoin d’ajouter cet élément à votre manifeste.

### <a name="architecture-of-xaml-island-net-controls"></a>Architecture des contrôles .NET (îlot XAML)

Voici un aperçu rapide de la façon dont les différents types de contrôles d’îlot XAML sont organisés de manière architecturale en plus de l’API d’hébergement XAML UWP.

![Architecture de contrôle de l'hôte](images/xaml-islands/host-controls.png)

Les API qui s’affichent en bas de ce diagramme sont livrés avec le SDK Windows. Les contrôles encapsulés et les contrôles hôtes sont disponibles via des packages NuGet dans la boîte à outils de la communauté Windows.

### <a name="web-view-controls"></a>Contrôles d’affichage Web

La boîte à outils de la communauté Windows fournit également les contrôles .NET suivants pour l’hébergement de contenu Web dans les applications WPF et Windows Forms. Ces contrôles sont souvent utilisés dans des scénarios de modernisation d’application de bureau similaires en tant que contrôles d’îlot XAML, et ils sont conservés dans les mêmes référentiel [Microsoft. Toolkit. Win32 référentiel](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32) que les contrôles d’îlot XAML.

| Contrôle | Système d’exploitation minimal pris en charge | Description |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, version 1803 | Utilise le moteur de rendu Microsoft Edge pour afficher le contenu Web. |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | Fournit une version de **WebView** qui est compatible avec d’autres versions de système d’exploitation. Ce contrôle utilise le moteur de rendu Microsoft Edge pour afficher le contenu Web sur Windows 10 version 1803 et ultérieure, et le moteur de rendu d’Internet Explorer pour afficher le contenu Web sur les versions antérieures de Windows 10, Windows 8. x et Windows 7. |

Pour utiliser ces contrôles, installez l’un de ces packages NuGet:

* WPF [Microsoft. Toolkit. WPF. UI. Controls. WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView)
* Windows Forms: [Microsoft. Toolkit. Forms. UI. Controls. WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView)

## <a name="c-win32-applications"></a>C++Applications Win32

Les contrôles .NET d’îlot XAML ne sont pas C++ pris en charge dans les applications Win32. Ces applications doivent utiliser à la place l' *API d’hébergement XAML UWP* fournie par le kit de développement logiciel (SDK) Windows 10 (version 1903 et ultérieure).

L’API d’hébergement XAML UWP se compose de plusieurs classes de Windows Runtime et d' C++ interfaces COM que votre application Win32 peut utiliser pour héberger n’importe quel contrôle UWP dérivé de [Windows. UI. Xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement). Vous pouvez héberger des contrôles UWP dans n’importe quel élément d’interface utilisateur de votre application à laquelle est associé un handle de fenêtre (HWND). Pour plus d’informations sur cette API, y compris les conditions préalables, consultez [utilisation de l’API d' C++ hébergement XAML UWP dans une application Win32](using-the-xaml-hosting-api.md).

> [!NOTE]
> Les contrôles encapsulés et les contrôles hôtes dans le kit de connaissances de la communauté Windows utilisent l’API d’hébergement XAML UWP en interne et implémentent tout le comportement que vous devez normalement gérer si vous utilisiez l’API d’hébergement XAML UWP directement, y compris la navigation au clavier et les modifications de disposition. Pour les applications WPF et Windows Forms, nous vous recommandons vivement d’utiliser ces contrôles au lieu de l’API d’hébergement XAML UWP directement, car ils éliminent un grand nombre des détails d’implémentation de l’utilisation de l’API.

## <a name="feature-roadmap"></a>Plan des fonctionnalités

Depuis la sortie de Windows 10, version 1903, les contrôles .NET insulaire .NET dans le kit de développement de la communauté Windows sont toujours en version préliminaire pour les développeurs jusqu’à ce que la version 1,0 des contrôles soit disponible.

* La version 1,0 des contrôles pour le .NET Framework 4.6.2 et versions ultérieures doit être publiée dans la [version 6,0 de la boîte à outils](https://github.com/windows-toolkit/WindowsCommunityToolkit/milestones).
* La version 1,0 des contrôles pour .NET Core 3 est prévue pour une version ultérieure de la boîte à outils.
* Si vous souhaitez essayer les derniers aperçus des versions 1,0 de ces contrôles pour la .NET Framework et .NET Core 3, consultez les packages NuGet version 6.0.0-preview7 (ou version ultérieure) dans la Galerie de la [boîte à outils de la communauté UWP](https://dotnet.myget.org/gallery/uwpcommunitytoolkit) .

Pour plus d’informations, consultez [ce billet de blog](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap).

## <a name="additional-resources"></a>Ressources supplémentaires

Pour plus d’informations générales et pour obtenir des didacticiels sur l’utilisation des îlots XAML, consultez les articles et ressources suivants:

* [Didacticiel de modernisation d’une application WPF](modernize-wpf-tutorial.md): Ce didacticiel fournit des instructions pas à pas pour l’utilisation des contrôles encapsulés et des contrôles hôtes dans la boîte à outils de la communauté Windows pour ajouter des contrôles UWP à une application métier WPF existante. Ce didacticiel comprend le code complet de l’application WPF, ainsi que des instructions détaillées pour chaque étape du processus.
* [Îlots XAML v1-mises à jour et](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap)feuille de route: Ce billet de blog présente de nombreuses questions courantes sur les îlots XAML et fournit un plan de développement détaillé.
