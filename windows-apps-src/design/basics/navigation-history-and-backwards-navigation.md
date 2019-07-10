---
Description: Découvrez comment implémenter la navigation vers l’arrière afin de parcourir l’historique de navigation de l’utilisateur dans une application UWP.
title: Historique de navigation et navigation vers l’arrière (applications Windows)
template: detail.hbs
op-migration-status: ready
ms.date: 4/9/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8e3ab6760ed3eff1d284e51205de261796db0fb2
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63799165"
---
# <a name="navigation-history-and-backwards-navigation-for-uwp-apps"></a>Historique de navigation et navigation vers l’arrière pour les applications UWP

> **API importantes** : [événement BackRequested](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager.BackRequested), [classe SystemNavigationManager](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager), [OnNavigatedTo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto#Windows_UI_Xaml_Controls_Page_OnNavigatedTo_Windows_UI_Xaml_Navigation_NavigationEventArgs_)

La plateforme Windows universelle (UWP) fournit un système de navigation vers l’arrière cohérent afin de parcourir l’historique de navigation de l’utilisateur dans une application et, en fonction de l’appareil, d’une application à l’autre.

Pour implémenter la navigation vers l’arrière dans votre application, placez un [bouton Précédent](#back-button) dans l’angle supérieur gauche de l’interface utilisateur de votre application. Si votre application utilise le contrôle [NavigationView](../controls-and-patterns/navigationview.md), vous pouvez ensuite utiliser le [bouton Précédent intégré de NavigationView](../controls-and-patterns/navigationview.md#backwards-navigation).

Lorsqu’il appuie sur le bouton Précédent, l’utilisateur s’attend à accéder à l’emplacement précédent dans l’historique de navigation de l’application. Sachez qu’il vous incombe de décider des actions de navigation à ajouter à l’historique de navigation et de la réponse à un appui sur le bouton Précédent.

## <a name="back-button"></a>Bouton Précédent

Pour créer un bouton Précédent, utilisez le contrôle [Button](../controls-and-patterns/buttons.md) et le style `NavigationBackButtonNormalStyle`, puis placez le bouton dans l’angle supérieur gauche de l’interface utilisateur de votre application (pour plus d’informations, consultez les exemples de code XAML ci-dessous).

![Bouton Précédent dans l’angle supérieur gauche de l’interface utilisateur de l’application](images/back-nav/BackEnabled.png)

```xaml
<Button Style="{StaticResource NavigationBackButtonNormalStyle}"/>
```

Si votre application possède un objet [CommandBar](../controls-and-patterns/app-bars.md) supérieur, le contrôle Button de 44 pixels de haut ne s’aligne pas correctement sur les contrôles AppBarButtons de 48 pixels. Pour éviter ce problème, alignez le haut du contrôle Button dans la limite de 48 pixels.

![Bouton Précédent dans la barre de commandes supérieure](images/back-nav/CommandBar.png)

```xaml
<Button VerticalAlignment="Top" HorizontalAlignment="Left" 
Style="{StaticResource NavigationBackButtonNormalStyle}"/>
```

Pour réduire le déplacement des éléments d’interface utilisateur dans votre application, affichez un bouton Précédent désactivé lorsque vous n’avez plus rien dans la pile arrière (consultez l’exemple de code ci-dessous). Toutefois, si vous pensez que votre application n’aura jamais de pile arrière, vous n’avez pas du tout besoin d’afficher le bouton Précédent.

![États du bouton Précédent](images/back-nav/BackDisabled.png)

## <a name="code-example"></a>Exemple de code

L’exemple de code suivant montre comment implémenter le comportement de navigation vers l’arrière avec un bouton Précédent. Le code répond à l’événement [**ClickButton**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.Click) et désactive/active la visibilité du bouton dans [**OnNavigatedTo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto#Windows_UI_Xaml_Controls_Page_OnNavigatedTo_Windows_UI_Xaml_Navigation_NavigationEventArgs_), lequel est appelé lors de la navigation vers une nouvelle page. L’exemple de code gère également les entrées provenant de l’utilisation des touches Précédent des systèmes matériels et logiciels en inscrivant un écouteur pour l’événement [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.BackRequested).

```xaml
<!-- MainPage.xaml -->
<Page x:Class="AppName.MainPage">
...
<Button x:Name="BackButton" Click="Back_Click" Style="{StaticResource NavigationBackButtonNormalStyle}"/>
...
<Page/>
```

Code-behind :

```csharp
// MainPage.xaml.cs
public MainPage()
{
    KeyboardAccelerator GoBack = new KeyboardAccelerator();
    GoBack.Key = VirtualKey.GoBack;
    GoBack.Invoked += BackInvoked;
    KeyboardAccelerator AltLeft = new KeyboardAccelerator();
    AltLeft.Key = VirtualKey.Left;
    AltLeft.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(GoBack);
    this.KeyboardAccelerators.Add(AltLeft);
    // ALT routes here
    AltLeft.Modifiers = VirtualKeyModifiers.Menu;
}

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    BackButton.IsEnabled = this.Frame.CanGoBack;
}

private void Back_Click(object sender, RoutedEventArgs e)
{
    On_BackRequested();
}

// Handles system-level BackRequested events and page-level back button Click events
private bool On_BackRequested()
{
    if (this.Frame.CanGoBack)
    {
        this.Frame.GoBack();
        return true;
    }
    return false;
}

private void BackInvoked (KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    On_BackRequested();
    args.Handled = true;
}
```

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"

#include "winrt/Windows.System.h"
#include "winrt/Windows.UI.Xaml.Controls.h"
#include "winrt/Windows.UI.Xaml.Input.h"
#include "winrt/Windows.UI.Xaml.Navigation.h"

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml;

namespace winrt::PageNavTest::implementation
{
    MainPage::MainPage()
    {
        InitializeComponent();

        Windows::UI::Xaml::Input::KeyboardAccelerator goBack;
        goBack.Key(Windows::System::VirtualKey::GoBack);
        goBack.Invoked({ this, &MainPage::BackInvoked });
        Windows::UI::Xaml::Input::KeyboardAccelerator altLeft;
        altLeft.Key(Windows::System::VirtualKey::Left);
        altLeft.Invoked({ this, &MainPage::BackInvoked });
        KeyboardAccelerators().Append(goBack);
        KeyboardAccelerators().Append(altLeft);
        // ALT routes here.
        altLeft.Modifiers(Windows::System::VirtualKeyModifiers::Menu);
    }

    void MainPage::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
    {
        BackButton().IsEnabled(Frame().CanGoBack());
    }

    void MainPage::Back_Click(IInspectable const&, RoutedEventArgs const&)
    {
        On_BackRequested();
    }

    // Handles system-level BackRequested events and page-level back button Click events.
    bool MainPage::On_BackRequested()
    {
        if (Frame().CanGoBack())
        {
            Frame().GoBack();
            return true;
        }
        return false;
    }

    void MainPage::BackInvoked(Windows::UI::Xaml::Input::KeyboardAccelerator const& sender,
        Windows::UI::Xaml::Input::KeyboardAcceleratorInvokedEventArgs const& args)
    {
        args.Handled(On_BackRequested());
    }
}
```

Nous gérons ci-dessus la navigation vers l’arrière pour une seule page. Vous pouvez gérer la navigation dans chaque page si vous souhaitez exclure des pages spécifiques de la navigation vers l’arrière, ou si vous souhaitez exécuter du code de niveau page avant d’afficher la page.

Pour gérer la navigation vers l’arrière pour toute une application, vous allez inscrire un écouteur global pour l’événement [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.BackRequested) dans le fichier code-behind `App.xaml`.

Fichier code-behind app.xaml :

```csharp
// App.xaml.cs
Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
Frame rootFrame = Window.Current.Content;
rootFrame.PointerPressed += On_PointerPressed;

private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
{
    e.Handled = On_BackRequested();
}

private void On_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    bool isXButton1Pressed =
        e.GetCurrentPoint(sender as UIElement).Properties.PointerUpdateKind == PointerUpdateKind.XButton1Pressed;

    if (isXButton1Pressed)
    {
        e.Handled = On_BackRequested();
    }
}

private bool On_BackRequested()
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        rootFrame.GoBack();
        return true;
    }
    return false;
}
```

```cppwinrt
// App.cpp
#include <winrt/Windows.UI.Core.h>
#include "winrt/Windows.UI.Input.h"
#include "winrt/Windows.UI.Xaml.Input.h"

#include "App.h"
#include "MainPage.h"

using namespace winrt;
...

    Windows::UI::Core::SystemNavigationManager::GetForCurrentView().BackRequested({ this, &App::App_BackRequested });
    Frame rootFrame{ nullptr };
    auto content = Window::Current().Content();
    if (content)
    {
        rootFrame = content.try_as<Frame>();
    }
    rootFrame.PointerPressed({ this, &App::On_PointerPressed });
...

void App::App_BackRequested(IInspectable const& /* sender */, Windows::UI::Core::BackRequestedEventArgs const& e)
{
    e.Handled(On_BackRequested());
}

void App::On_PointerPressed(IInspectable const& sender, Windows::UI::Xaml::Input::PointerRoutedEventArgs const& e)
{
    bool isXButton1Pressed =
        e.GetCurrentPoint(sender.as<UIElement>()).Properties().PointerUpdateKind() == Windows::UI::Input::PointerUpdateKind::XButton1Pressed;

    if (isXButton1Pressed)
    {
        e.Handled(On_BackRequested());
    }
}

// Handles system-level BackRequested events.
bool App::On_BackRequested()
{
    if (Frame().CanGoBack())
    {
        Frame().GoBack();
        return true;
    }
    return false;
}
```

## <a name="optimizing-for-different-device-and-form-factors"></a>Optimisation pour différents appareils et facteurs de forme

Cette recommandation de conception pour une navigation vers l’arrière est applicable à tous les appareils, mais certains appareils et facteurs de forme peuvent bénéficier d’une optimisation. Cela dépend également du bouton Précédent matériel pris en charge par différents interpréteurs de commandes.

- **Téléphone/tablette** : un bouton Précédent matériel ou logiciel est toujours présent sur un appareil mobile et une tablette mais, pour des questions de clarté, nous vous recommandons de représenter un bouton Précédent dans l’application.
- **Bureau/hub** : représentez le bouton Précédent dans l’application dans l’angle supérieur gauche de l’interface utilisateur de votre application.
- **Xbox/TV** : ne représentez pas de bouton Précédent pour éviter d’alourdir inutilement l’interface utilisateur. Au lieu de cela, utilisez le bouton B du boîtier de commande pour naviguer vers l’arrière.

Si votre application s’exécute sur plusieurs appareils, [créez un déclencheur visuel personnalisé pour Xbox](../devices/designing-for-tv.md#custom-visual-state-trigger-for-xbox) pour modifier la visibilité du bouton. Le contrôle NavigationView modifie automatiquement la visibilité du bouton Précédent si votre application s’exécute sur Xbox. 

Nous vous recommandons de prendre en charge les entrées suivantes pour la navigation vers l’arrière. (Notez que certaines de ces entrées ne sont pas prises en charge par le système BackRequested et doivent être gérées par des événements distincts.)

| Entrée | Événement |
| --- | --- |
| Touche Retour arrière Windows | BackRequested |
| Bouton Précédent matériel | BackRequested |
| Bouton Précédent du mode tablette de l’interpréteur de commandes | BackRequested |
| VirtualKey.XButton1 | PointerPressed |
| VirtualKey.GoBack | KeyboardAccelerator.BackInvoked |
| Alt+Flèche gauche | KeyboardAccelerator.BackInvoked |

Les exemples de code ci-dessus montrent comment gérer toutes ces entrées.

## <a name="system-back-behavior-for-backward-compatibilities"></a>Comportement vers l’arrière du système pour la compatibilité descendante

Auparavant, les applications UWP utilisaient [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility) pour la navigation vers l’arrière. L’API continuera d’être prise en charge pour assurer la compatibilité descendante, mais nous vous recommandons de ne plus utiliser [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility). Au lieu de cela, votre application doit représenter son propre bouton Précédent.

Si votre application continue d’utiliser [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility), l’interface utilisateur du système affichera le bouton Précédent du système à l’intérieur de la barre de titre. (Les interactions utilisateur et l’apparence du bouton Précédent sont identiques à celles des builds précédentes.)

![Bouton Précédent de la barre de titre](images/nav-back-pc.png)

### <a name="system-back-bar"></a>Barre Précédent système

> [!NOTE]
> « Barre Précédent système » est uniquement une description, pas un nom officiel.

La barre Précédent système est une « bande » qui est insérée entre la bande d’onglets et la zone de contenu de l’application. La bande s’étend sur toute la largeur de l’application et le bouton Précédent se trouve sur son bord gauche. La hauteur verticale de la bande est de 32 pixels pour garantir une taille de cible tactile adéquate pour le bouton Précédent.

La barre Précédent système s’affiche de façon dynamique, en fonction de la visibilité du bouton Précédent. Lorsque le bouton Précédent est visible, la barre Précédent système est insérée et déplace le contenu de l’application de 32 pixels vers le bas sous la bande d’onglets. Lorsque le bouton Précédent est masqué, la barre Précédent système est supprimée de manière dynamique, le contenu de l’application étant décalé de 32 pixels vers le haut contre la bande d’onglets. Pour éviter que l’interface utilisateur de votre application ne se déplace vers le haut ou vers le bas, nous vous recommandons de représenter un [bouton Précédent dans l’application](#back-button).

Les [personnalisations de la barre de titre](../shell/title-bar.md) s’appliquent à la fois à la bande d’onglets et à la barre Précédent système de l’application. Si votre application spécifie des propriétés de couleur du premier plan et de l’arrière-plan avec[ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar), les couleurs s’appliquent à la bande d’onglets et à la barre Précédent système.

## <a name="guidelines-for-custom-back-navigation-behavior"></a>Recommandations sur le comportement personnalisé de navigation vers l’arrière

Si vous choisissez de fournir votre propre navigation de pile Back, l’expérience doit être cohérente avec les autres applications. Nous vous recommandons de suivre les modèles d’actions de navigation suivants :

<div class="mx-responsive-img">
<table>
<thead>
<tr class="header">
<th align="left">Action de navigation</th>
<th align="left">Ajouter à l’historique de navigation ?</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;"><strong>Page à page, différents groupes d’homologues</strong></td>
<td style="vertical-align:top;"><strong>Oui</strong>
<p>Dans cette illustration, l’utilisateur navigue du niveau 1 de l’application vers le niveau 2, en traversant des groupes d’homologues. La navigation est donc ajoutée à l’historique de navigation.</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly1.png" alt="Navigation across peer groups" /></p>
<p>Dans l’illustration suivante, l’utilisateur navigue entre deux groupes d’homologues du même niveau. La navigation est donc ajoutée à l’historique de navigation.</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly2.png" alt="Navigation across peer groups" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>Page à page, même groupe d’homologues, pas d’élément de navigation à l’écran</strong>
<p>L’utilisateur navigue d’une page à une autre dans le même groupe d’homologues. Aucun élément de navigation à l’écran (tel que <a href="https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview">NavigationView</a>) ne fournit une navigation directe vers les deux pages.</p></td>
<td style="vertical-align:top;"><strong>Oui</strong>
<p>Dans l’illustration suivante, l’utilisateur navigue entre deux pages dans le même groupe d’homologues et la navigation doit être ajoutée à l’historique de navigation.</p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-noosnavelement.png" alt="Navigation within a peer group" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>Page à page, même groupe d’homologues, avec un élément de navigation à l’écran</strong>
<p>L’utilisateur navigue d’une page à une autre dans le même groupe d’homologues. Les deux pages sont affichées dans le même élément de navigation, tel que <a href="https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview">NavigationView</a>.</p></td>
<td style="vertical-align:top;"><strong>Cela dépend</strong>
<p>Oui, ajouter à l’historique de navigation, avec deux exceptions notables. Si vous pensez que des utilisateurs de votre application passeront fréquemment d’une page à l’autre dans le groupe d’homologues, ou si vous souhaitez conserver la hiérarchie de navigation, n’effectuez aucun ajout à l’historique de navigation. Dans ce cas, lorsque l’utilisateur appuie sur le bouton Précédent, il retourne à la dernière page avant d’avoir accédé au groupe d’homologues actuel. </p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-yesosnavelement.png" alt="Navigation across peer groups when a navigation element is present" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>Afficher une interface utilisateur temporaire</strong>
<p>L’application affiche une fenêtre indépendante ou une fenêtre enfant, comme une boîte de dialogue, un écran de démarrage ou un clavier visuel. Elle passe également en mode spécial, comme le mode de sélection multiple.</p></td>
<td style="vertical-align:top;"><strong>Non</strong>
<p>Lorsque l’utilisateur appuie sur le bouton Précédent, il ignore l’interface utilisateur temporaire (masque le clavier visuel, annule la boîte de dialogue, etc.) et retourne dans la page qui a généré l’interface utilisateur temporaire.</p>
<p><img src="images/back-nav/back-transui.png" alt="Showing a transient UI" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>Énumérer les éléments</strong>
<p>L’application affiche le contenu d’un élément visuel, tel que les détails de l’élément sélectionné dans la liste des éléments maîtres/détails.</p></td>
<td style="vertical-align:top;"><strong>Non</strong>
<p>L’énumération des éléments est semblable à la navigation au sein d’un groupe d’homologues. Lorsque l’utilisateur appuie sur le bouton Précédent, il navigue vers la page qui a précédé la page actuelle qui comporte l’énumération d’éléments.</p>
<p><img src="images/back-nav/nav-enumerate.png" alt="Iterm enumeration" /></p></td>
</tr>
</tbody>
</table>
</div>

### <a name="resuming"></a>Resuming

Quand l’utilisateur bascule vers une autre application et retourne dans votre application, nous recommandons un retour à la dernière page de l’historique de navigation.

## <a name="related-articles"></a>Articles connexes

- [Notions de base sur la navigation](navigation-basics.md)
