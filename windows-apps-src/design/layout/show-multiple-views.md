---
Description: Affichez les différentes parties de votre application dans des fenêtres distinctes.
title: Afficher plusieurs vues d’une application
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ee49b5fe5b5956e9069ea196c4d2e029b3a15763
ms.sourcegitcommit: 3cc6eb3bab78f7e68c37226c40410ebca73f82a9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/02/2019
ms.locfileid: "68729520"
---
# <a name="show-multiple-views-for-an-app"></a>Afficher plusieurs vues d’une application

![Mode filaire illustrant une application avec plusieurs fenêtres](images/multi-view.gif)

Aidez les utilisateurs à accroître leur productivité en leur permettant d’afficher des parties indépendantes de votre application dans des fenêtres distinctes. Lorsque vous créez plusieurs fenêtres pour une application, la barre des tâches affiche chaque fenêtre séparément. Les utilisateurs peuvent déplacer, redimensionner, afficher et masquer des fenêtres d’application indépendamment et ils peuvent basculer d’une fenêtre à une autre comme s’il s’agissait d’applications distinctes.

> **API importantes** : Espace de noms [Windows. UI. ViewManagement](/uwp/api/windows.ui.viewmanagement), [espace de noms Windows. UI. windowmanagement](/uwp/api/windows.ui.windowmanagement)

## <a name="when-should-an-app-use-multiple-views"></a>Quand une application doit-elle utiliser plusieurs vues ?

Il existe un grand nombre de scénarios qui peuvent tirer parti de plusieurs vues. Voici quelques exemples :

- Une application de messagerie qui permet aux utilisateurs d’afficher une liste des messages reçus tout en rédigeant un e-mail
- Une application de carnet d’adresses qui permet aux utilisateurs de comparer les informations de contact de plusieurs personnes côte à côte
- Une application de lecteur de musique qui permet aux utilisateurs de voir ce qui est en cours de lecture tout en naviguant dans une liste d’autres morceaux disponibles
- Une application de prise de notes qui permet aux utilisateurs de copier des informations à partir d’une page de notes sur une autre
- Une application de lecture qui permet aux utilisateurs d’ouvrir plusieurs articles à lire plus tard, après avoir pu accéder à tous les gros titres de l'actualité

Chaque disposition d'application est unique, mais nous vous recommandons d'inclure un bouton « nouvelle fenêtre » dans un emplacement prévisible, comme le coin supérieur droit du contenu, qui pourra être ouvert dans une nouvelle fenêtre. Envisagez également d’inclure une option de [menu contextuel](../controls-and-patterns/menus.md) pour «ouvrir dans une nouvelle fenêtre».

Pour créer des instances distinctes de votre application (plutôt que des fenêtres distinctes pour la même instance), consultez [créer une application UWP multi-instance](../../launch-resume/multi-instance-uwp.md).

## <a name="windowing-hosts"></a>Ordinateurs hôtes de fenêtrage

Il existe différentes façons d’héberger du contenu UWP dans une application.

- [CoreWindow](/uwp/api/windows.ui.core.corewindow)/[ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)

     Une vue d’application est l’association de type 1:1 d’un thread et d’une fenêtre que l’application utilise pour afficher le contenu. La première vue créée lors du démarrage de votre application s’appelle la *vue principale*. Chaque CoreWindow/ApplicationView fonctionne dans son propre thread. Le fait de travailler sur des threads d’interface utilisateur différents peut compliquer les applications à plusieurs fenêtres.

    La vue principale de votre application est toujours hébergée dans un ApplicationView. Le contenu d’une fenêtre secondaire peut être hébergé dans un ApplicationView ou dans un AppWindow.

    Pour savoir comment utiliser ApplicationView pour afficher les fenêtres secondaires dans votre application, consultez [utiliser ApplicationView](application-view.md).
- [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)

    AppWindow simplifie la création d’applications UWP multifenêtres, car il opère sur le même thread d’interface utilisateur qu’à partir duquel il est créé.

    La classe AppWindow et les autres API de l’espace de noms [windowmanagement](/uwp/api/windows.ui.windowmanagement) sont disponibles à partir de Windows 10, version 1903 (SDK 18362). Si votre application cible des versions antérieures de Windows 10, vous devez utiliser ApplicationView pour créer des fenêtres secondaires.

    Pour savoir comment utiliser AppWindow pour afficher les fenêtres secondaires dans votre application, consultez [utiliser AppWindow](app-window.md).

    > [!NOTE]
    > AppWindow est actuellement en version préliminaire. Cela signifie que vous pouvez envoyer des applications qui utilisent AppWindow vers le Windows Store, mais certains composants de la plateforme et de l’infrastructure sont connus pour ne pas fonctionner avec AppWindow (voir [limitations](/uwp/api/windows.ui.windowmanagement.appwindow#limitations)).
- [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) (Îlots XAML)

     Le contenu XAML UWP dans une application Win32 (à l’aide de HWND), également appelé îlots XAML, est hébergé dans un DesktopWindowXamlSource.

    Pour plus d’informations sur les îlots XAML, consultez [utilisation de l’API d’hébergement XAML UWP dans une application de bureau](/windows/apps/desktop/modernize/using-the-xaml-hosting-api)

### <a name="make-code-portable-across-windowing-hosts"></a>Rendre le code portable sur les hôtes de fenêtrage

Quand le contenu XAML est affiché dans un [CoreWindow](/uwp/api/windows.ui.core.corewindow), il y a toujours un [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) associé et une [fenêtre](/uwp/api/windows.ui.xaml.window)XAML. Vous pouvez utiliser des API sur ces classes pour récupérer des informations telles que les limites de la fenêtre. Pour récupérer une instance de ces classes, utilisez la méthode statique [CoreWindow. GetForCurrentThread](/uwp/api/windows.ui.core.corewindow.getforcurrentthread) , la méthode [ApplicationView. GetForCurrentView](/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview) ou la propriété [Window. Current](/uwp/api/windows.ui.xaml.window.current) . De plus, il existe de nombreuses classes qui utilisent `GetForCurrentView` le modèle pour récupérer une instance de la classe, par exemple [DisplayInformation. GetForCurrentView](/uwp/api/windows.graphics.display.displayinformation.getforcurrentview).

Ces API fonctionnent parce qu’il n’existe qu’une seule arborescence de contenu XAML pour un CoreWindow/ApplicationView, de sorte que le XAML sait que le contexte dans lequel il est hébergé est celui de CoreWindow/ApplicationView.

Quand le contenu XAML s’exécute à l’intérieur d’un AppWindow ou d’un DesktopWindowXamlSource, plusieurs arborescences de contenu XAML peuvent s’exécuter simultanément sur le même thread. Dans ce cas, ces API ne fournissent pas les informations appropriées, car le contenu n’est plus exécuté à l’intérieur de la fenêtre CoreWindow/ApplicationView (et de la fenêtre XAML) actuelle.

Pour vous assurer que votre code fonctionne correctement sur tous les hôtes de fenêtrage, vous devez remplacer les API qui reposent sur [CoreWindow](/uwp/api/windows.ui.core.corewindow), [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)et [Window](/uwp/api/windows.ui.xaml.window) avec les nouvelles API qui obtiennent leur contexte à partir de la classe [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) .
La classe XamlRoot représente une arborescence de contenu XAML et des informations sur le contexte dans lequel elle est hébergée, qu’il s’agisse d’un CoreWindow, d’un AppWindow ou d’un DesktopWindowXamlSource. Cette couche d’abstraction vous permet d’écrire le même code, quel que soit l’hôte de fenêtrage dans lequel le XAML s’exécute.

Ce tableau montre le code qui ne fonctionne pas correctement entre les hôtes de fenêtrage et le nouveau code portable avec lequel vous pouvez le remplacer, ainsi que certaines API que vous n’avez pas besoin de modifier.

| Si vous avez utilisé... | Remplacer par... |
| - | - |
| CoreWindow. GetForCurrentThread (). [Limites](/uwp/api/windows.ui.core.corewindow.bounds) | _UIElement_. XamlRoot. [Taille](/uwp/api/windows.ui.xaml.xamlroot.size) |
| CoreWindow. GetForCurrentThread (). [SizeChanged](/uwp/api/windows.ui.core.corewindow.sizechanged) | _UIElement_. XamlRoot. [Modifié](/uwp/api/windows.ui.xaml.xamlroot.changed) le |
| CoreWindow. [Visible](/uwp/api/windows.ui.core.corewindow.visible) | _UIElement_. XamlRoot. [IsHostVisible](/uwp/api/windows.ui.xaml.xamlroot.ishostvisible) |
| CoreWindow. [VisibilityChanged](/uwp/api/windows.ui.core.corewindow.visibilitychanged) | _UIElement_. XamlRoot. [Modifié](/uwp/api/windows.ui.xaml.xamlroot.changed) le |
| CoreWindow. GetForCurrentThread (). [GetKeyState](/uwp/api/windows.ui.core.corewindow.getkeystate) | Inchangé. Cela est pris en charge dans AppWindow et DesktopWindowXamlSource. |
| CoreWindow. GetForCurrentThread (). [GetAsyncKeyState](/uwp/api/windows.ui.core.corewindow.getasynckeystate) | Inchangé. Cela est pris en charge dans AppWindow et DesktopWindowXamlSource. |
| Fenetre. [En cours](/uwp/api/windows.ui.xaml.window.current) | Retourne l’objet de fenêtre XAML principal qui est étroitement lié au CoreWindow actuel. Consultez la remarque après ce tableau. |
| Window. Current. [Limites](/uwp/api/windows.ui.xaml.window.bounds) | _UIElement_. XamlRoot. [Taille](/uwp/api/windows.ui.xaml.xamlroot.size) |
| Window. Current. [Contenu](/uwp/api/windows.ui.xaml.window.content) | Racine UIElement = _UIElement_. XamlRoot. [Contenu](/uwp/api/windows.ui.xaml.xamlroot.content) |
| Window. Current. [Compositeur](/uwp/api/windows.ui.xaml.window.compositor) | Inchangé. Cela est pris en charge dans AppWindow et DesktopWindowXamlSource. |
| VisualTreeHelper. [GetOpenPopups](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopups)<br/>Dans les applications des îlots XAML, cela génère une erreur. Dans les applications AppWindow, cette opération retourne les menus contextuels ouverts dans la fenêtre principale. | VisualTreeHelper. [GetOpenPopupsForXamlRoot](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopupsforxamlroot) (_UIElement_. XamlRoot) |
| Focus. [GetFocusedElement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement) | Focus. [GetFocusedElement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement#Windows_UI_Xaml_Input_FocusManager_GetFocusedElement_Windows_UI_Xaml_XamlRoot_) (_UIElement_. XamlRoot) |
| contentDialog. ShowAsync () | contentDialog. [XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot)  =  _UIElement_. XamlRoot;<br/>contentDialog. ShowAsync (); |
| menuFlyout. ShowAt (null, New point (10, 10)); | menuFlyout. [XamlRoot](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.xamlroot)  =  _UIElement_. XamlRoot;<br/>menuFlyout. ShowAt (null, New point (10, 10)); |

> [!NOTE]
> Pour le contenu XAML dans un DesktopWindowXamlSource, il existe un CoreWindow/une fenêtre sur le thread, mais il est toujours invisible et sa taille est de 1x1. Elle est toujours accessible à l’application, mais ne renvoie pas de limites significatives ni de visibilité.
>
>Pour le contenu XAML dans un AppWindow, il y aura toujours exactement un CoreWindow sur le même thread. Si vous appelez une `GetForCurrentView` API `GetForCurrentThread` ou, cette API retourne un objet qui reflète l’état du CoreWindow sur le thread, et non l’un des AppWindows qui peuvent s’exécuter sur ce thread.


## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées

- Fournissez un point d’entrée clair à la vue secondaire en utilisant le glyphe « ouvrir une nouvelle fenêtre ».
- Communiquez l’objectif de la vue secondaire aux utilisateurs.
- Assurez-vous que votre application est entièrement fonctionnelle dans une vue unique et que les utilisateurs ouvrent une vue secondaire uniquement pour des raisons pratiques.
- N’utilisez pas la vue secondaire pour fournir des notifications ou d'autres éléments visuels temporaires.

## <a name="related-topics"></a>Rubriques connexes

- [Utiliser AppWindow](app-window.md)
- [Utiliser ApplicationView](application-view.md)
- [ApplicationViewSwitcher](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)
- [CreateNewView](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)
