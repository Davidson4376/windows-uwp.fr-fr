---
author: serenaz
Description: Learn how to implement backwards navigation for traversing the user's navigation history within an UWP app.
title: Historique de navigation et navigation vers l’arrière (applicationsWindows)
ms.assetid: e9876b4c-242d-402d-a8ef-3487398ed9b3
isNew: true
label: History and backwards navigation
template: detail.hbs
op-migration-status: ready
ms.author: sezhen
ms.date: 06/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0400e04a86675adccd1da14d8cb2652028fbfd30
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "2860768"
---
# <a name="navigation-history-and-backwards-navigation-for-uwp-apps"></a>Historique de navigation et navigation vers l’arrière pour les applicationsUWP

> **API importantes**: [classe SystemNavigationManager](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager.BackRequested), [événement BackRequested](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager), [OnNavigatedTo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto#Windows_UI_Xaml_Controls_Page_OnNavigatedTo_Windows_UI_Xaml_Navigation_NavigationEventArgs_)

Pour des raisons similaires, la plateforme Windows universelle (UWP) fournit un système de navigation cohérent pour la fonctionnalité Précédent afin que l’utilisateur puisse parcourir l’historique de navigation dans une application et, en fonction de l’appareil, d’une application à l’autre.

Pour implémenter la navigation vers l’arrière dans votre application, placez un [bouton Précédent](#Back-button) dans l’angle supérieur gauche de l’interface utilisateur de votre application. Si votre application utilise le contrôle [NavigationView](../controls-and-patterns/navigationview.md), vous pouvez ensuite utiliser le [bouton Précédent intégré de NavigationView](../controls-and-patterns/navigationview.md#backwards-navigation).

Lorsqu’il appuie sur le bouton Précédent, l’utilisateur s’attend à accéder à l’emplacement précédent dans l’historique de navigation de l’application. Sachez qu’il vous incombe de décider des actions de navigation à ajouter à l’historique de navigation et de la réponse à un appui sur le bouton Précédent.

## <a name="back-button"></a>Bouton Précédent

Pour créer un bouton précédent, utilisez le contrôle de [bouton](../controls-and-patterns/buttons.md) avec le `NavigationBackButtonNormalStyle` de style et, le bouton dans le coin supérieur gauche de l’interface utilisateur de votre application (pour plus d’informations, consultez les exemples de code XAML ci-dessous).

![Bouton Précédent dans l’angle supérieur gauche de l’interface utilisateur de l’application](images/back-nav/BackEnabled.png)

```xaml
<Button Style="{StaticResource NavigationBackButtonNormalStyle}"/>
```

Si votre application possède une [barre de commandes](../controls-and-patterns/app-bars.md) supérieure, le contrôle Bouton de 44pixels de haut ne s’aligne pas correctement avec les contrôles AppBarButtons de 48pixels. Pour éviter ce problème, alignez le haut du contrôle Bouton dans la limite de 48pixels.

![Bouton Précédent sur la barre de commandes supérieure](images/back-nav/CommandBar.png)

```xaml
<Button VerticalAlignment="Top" HorizontalAlignment="Left" 
Style="{StaticResource NavigationBackButtonNormalStyle}"/>
```

Pour réduire le déplacement des éléments d’interface utilisateur dans votre application, affichez un bouton Précédent désactivé lorsque vous n’avez plus rien dans le backstack (voir l’exemple de code ci-dessous).

![États du bouton Précédent](images/back-nav/BackDisabled.png)

## <a name="code-example"></a>Exemple de code

L’exemple de code suivant montre comment implémenter le comportement de navigation vers l’arrière avec un bouton Précédent. Le code répond à l’événement de [**clic**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.Click) du bouton, et désactive/active la visibilité du bouton dans [**OnNavigatedTo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto#Windows_UI_Xaml_Controls_Page_OnNavigatedTo_Windows_UI_Xaml_Navigation_NavigationEventArgs_), lequel est appelé lors de la navigation vers une nouvelle page. L’exemple de code gère également les entrées provenant de l’utilisation des touches Précédent des systèmes matériels et logiciels en inscrivant un écouteur pour l’événement [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.BackRequested).

```xaml
<!-- MainPage.xaml -->
<Page x:Class="AppName.MainPage">
...
<Button x:Name="BackButton" Click="Back_Click" Style="{StaticResource NavigationBackButtonNormalStyle}"/>
...
<Page/>
```

Code-behind:

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

Ci-dessus, nous gérer descendante la navigation pour une seule page. Vous pouvez gérer la navigation dans chaque page si vous souhaitez exclure des pages spécifiques de navigation arrière, ou vous souhaitez exécuter du code au niveau de la page avant d’afficher la page.

Pour gérer la compatibilité descendante navigation pour une application entière, vous allez enregistrer un écouteur global pour l’événement [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.BackRequested) dans les `App.xaml` fichier code-behind.

App.xaml code-behind:

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

## <a name="optimizing-for-different-device-and-form-factors"></a>optimisation pour différents appareils et formats

Ces recommandations de conception pour une navigation vers l’arrière est applicable à tous les appareils, mais certains appareils et formats peuvent bénéficier d’une optimisation. Cela dépend également du bouton matériel Précédent pris en charge par différents interpréteurs de commandes.

- **Téléphone/tablette**: un bouton Précédent matériel ou logiciel est toujours présent sur un appareil mobile et une tablette, mais pour des questions de clarté, nous vous recommandons de prévoir un bouton Précédent dans l’application.
- **Bureau/Hub**: dessinez le bouton Précédent dans l’application dans l’angle supérieur gauche de l’interface utilisateur de votre application.
- **Xbox/TV**: ne prévoyez pas de bouton Précédent pour éviter d’alourdir inutilement l’interface utilisateur. Au lieu de cela, utilisez le boutonB du boîtier de commande pour naviguer vers l’arrière.

Si votre application s’exécute sur plusieurs appareils, [créez un déclencheur visuel personnalisé pour Xbox](../devices/designing-for-tv.md#custom-visual-state-trigger-for-xbox) pour modifier la visibilité du bouton. Le contrôle NavigationView modifie automatiquement la visibilité du bouton Précédent si votre application s’exécute sur Xbox. 

Nous vous recommandons de prendre en charge les entrées suivantes pour la navigation vers l’arrière. (Notez que certaines de ces entrées ne sont pas prises en charge par le système BackRequested et doivent être gérées par des événements distincts).

| Entrée | Événement |
| --- | --- |
| Touche Retour arrière Windows | BackRequested |
| Bouton Précédent matériel | BackRequested |
| Bouton Précédent du mode Tablette de l’interpréteur de commandes | BackRequested |
| VirtualKey.XButton1 | PointerPressed |
| VirtualKey.GoBack | KeyboardAccelerator.BackInvoked |
| Alt+Gauche | KeyboardAccelerator.BackInvoked |

Les exemples de code ci-dessus montrent comment gérer toutes ces entrées.

## <a name="system-back-behavior-for-backward-compatibilities"></a>Comportement précédent du système pour la compatibilité vers l’arrière

Auparavant, les applicationsUWP utilisaient [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility) pour la navigation vers l’arrière. L’API continuera d’être prise en charge pour la compatibilité vers l’arrière, mais nous vous recommandons de ne plus utiliser le bouton Précédent de barre de titre. Au lieu de cela, votre application doit disposer de son propre bouton Précédent.

Si votre application continue à utiliser [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility), le bouton Précédent sera basculé dans la barre de titre, comme d’habitude.

- Si votre application n’est **pas à onglets**, le bouton de retour s’affiche à l’intérieur de la barre de titre. Les interactions d’utilisateur et une expérience visual pour le bouton de retour sont identiques à ceux des versions précédentes.

    ![Bouton précédent de la barre de titre](images/nav-back-pc.png)

- Si une application est **à onglets**, puis le bouton de retour s’affiche à l’intérieur d’une sauvegarde de système nouvelle barre.

    ![Système dessinées back barre](images/back-nav/tabs.png)

### <a name="system-back-bar"></a>Système barre

> [!NOTE]
> «Système barre» est uniquement une description, pas un nom officiel.

Le système vers l’arrière barre est une bande est insérée entre la bande de l’onglet et la zone de contenu d’application s. La bande s'étend sur toute la largeur de l’application et le bouton Précédent se trouve sur son bord gauche. La bande a une hauteur de 32 pixels pour garantir tactile adéquate la taille cible pour le bouton de retour.

La barre Précédent système s'affiche de façon dynamique, en fonction de la visibilité du bouton Précédent. Lorsque le bouton de retour est visible, le système vers l’arrière barre est insérée, en progressant de contenu d’application vers le bas 32 pixels au-dessous la bande de l’onglet. Lorsque le bouton de retour est masqué, l’arrière du système est dynamiquement supprimé, en progressant de contenu d’application 32 pixels pour répondre à la bande de l’onglet. Pour éviter d’avoir MAJ de l’interface utilisateur de votre application haut ou vers le bas, nous vous recommandons d’un [bouton de retour dans l’application](#back-button)de dessin.

[Personnalisations de barre de titre](../shell/title-bar.md) seront reportés à l’onglet application et le système de retour barre. Si votre application spécifie les propriétés de couleur d’arrière-plan et de premier plan avec [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar), puis les couleurs seront appliquées à l’arrière-plan de l’onglet et le système de barre.

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
<p>Dans cette illustration, l’utilisateur navigue du niveau1 de l’application vers le niveau2, en traversant des groupes d’homologues. La navigation est donc ajoutée à l’historique de navigation.</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly1.png" alt="Navigation across peer groups" /></p>
<p>Dans l’illustration suivante, l’utilisateur navigue entre deux groupes d’homologues du même niveau. La navigation est donc ajoutée à l’historique de navigation.</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly2.png" alt="Navigation across peer groups" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>Page à page, même groupe d’homologues, pas d’élément de navigation à l’écran</strong>
<p>L’utilisateur navigue d’une page à une autre dans le même groupe d’homologues. Il n’existe pas à l’écran élément de navigation (par exemple, [NavigationView](../controls-and-patterns/navigationview.md)) qui fournit une navigation directe aux deux pages.</p></td>
<td style="vertical-align:top;"><strong>Oui</strong>
<p>Dans l’illustration suivante, l’utilisateur navigue entre les deux pages dans le même groupe homologue et la navigation doit être ajoutée à l’historique de navigation.</p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-noosnavelement.png" alt="Navigation within a peer group" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>Page à page, même groupe d’homologues, avec un élément de navigation à l’écran</strong>
<p>L’utilisateur navigue d’une page à une autre dans le même groupe d’homologues. Les deux pages sont indiqués dans le même élément de navigation, tel que [NavigationView](../controls-and-patterns/navigationview.md).</p></td>
<td style="vertical-align:top;"><strong>Cela dépend</strong>
<p>Oui, ajouter à l’historique de navigation, avec deux exceptions notables. Si vous prévoyez que les utilisateurs de votre application pour basculer entre les pages dans le groupe homologue fréquemment ou si vous souhaitez conserver la hiérarchie de navigation, puis n’ajoutez pas à l’historique de navigation. Dans ce cas, lorsque l’utilisateur appuie sur le bouton Précédent, il retourne à la dernière page avant d’avoir accédé au groupe d’homologues actuel. </p>
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

### <a name="resuming"></a>Reprise

Quand l’utilisateur bascule vers une autre application et retourne dans votre application, nous recommandons un retour à la dernière page de l’historique de navigation.

## <a name="related-articles"></a>Articles associés

- [Notions de base sur la navigation](navigation-basics.md)