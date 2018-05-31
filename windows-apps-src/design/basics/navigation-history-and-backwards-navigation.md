---
author: serenaz
Description: The Universal Windows Platform (UWP) provides a consistent back navigation system for traversing the user's navigation history within an app and, depending on the device, from app to app.
title: Historique de navigation et navigation vers l’arrière (applicationsWindows)
ms.assetid: e9876b4c-242d-402d-a8ef-3487398ed9b3
isNew: true
label: History and backwards navigation
template: detail.hbs
op-migration-status: ready
ms.author: sezhen
ms.date: 11/22/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cefcefc9df85b512456c5fb2e556ad95e56d4999
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1831983"
---
#  <a name="navigation-history-and-backwards-navigation-for-uwp-apps"></a>Historique de navigation et navigation vers l’arrière pour les applicationsUWP

> **API importantes**: [classe SystemNavigationManager](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager.BackRequested), [événement BackRequested](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager), [OnNavigatedTo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto#Windows_UI_Xaml_Controls_Page_OnNavigatedTo_Windows_UI_Xaml_Navigation_NavigationEventArgs_)

Pour des raisons similaires, la plateforme Windows universelle (UWP) fournit un système de navigation cohérent pour la fonctionnalité Précédent afin que l’utilisateur puisse parcourir l’historique de navigation dans une application et, en fonction de l’appareil, d’une application à l’autre.

Pour implémenter la navigation vers l’arrière dans votre application, placez un [bouton Précédent](#Back-button) dans l’angle supérieur gauche de l’interface utilisateur de votre application. Si votre application utilise le contrôle [NavigationView](../controls-and-patterns/navigationview.md), vous pouvez ensuite utiliser le [bouton Précédent intégré de NavigationView](../controls-and-patterns/navigationview.md#backwards-navigation). 

Lorsqu’il appuie sur le bouton Précédent, l’utilisateur s’attend à accéder à l’emplacement précédent dans l’historique de navigation de l’application. Sachez qu’il vous incombe de décider des actions de navigation à ajouter à l’historique de navigation et de la réponse à un appui sur le bouton Précédent.

## <a name="back-button"></a>Bouton Précédent

Pour créer un bouton Précédent, utilisez le contrôle [Bouton](../controls-and-patterns/buttons.md) et le style `NavigationBackButtonNormalStyle`, puis placer le bouton dans l’angle supérieur gauche de l’interface utilisateur de votre application.

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
<Page x:Class="AppName.MainPage">
...
<Button x:Name="BackButton" Click="Back_Click" Style="{StaticResource NavigationBackButtonNormalStyle}"/>
...
<Page/>
```

Code-behind:

```csharp
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

ici, nous inscrivons un écouteur global pour l’événement [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.BackRequested) dans le `App.xaml`fichier code-behind. Vous pouvez vous inscrire pour cet événement dans chaque page si vous souhaitez exclure des pages spécifiques de la navigation vers l’arrière, ou si vous souhaitez exécuter du code de niveau page avant d’afficher la page.

App.xaml code-behind:

```csharp
Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
Frame rootFrame = Window.Current.Content;
rootFrame.PointerPressed += On_PointerPressed;

private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
{
    e.Handled = On_BackRequested();
}

private void On_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    bool isXButton1Pressed = e.GetCurrentPoint(sender as UIElement).Properties.PointerUpdateKind == PointerUpdateKind.XButton1Pressed;

    if (isXButton1Pressed)
    {
        e.Handled = On_BackRequested();
    }
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

![bouton Précédent de la barre de titre](images/nav-back-pc.png)

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
<p>L’utilisateur navigue d’une page à une autre dans le même groupe d’homologues. Aucun élément de navigation (par exemple les onglets/tableaux croisés dynamiques ou un volet de navigation ancré) fournissant une navigation directe vers les deux pages n’est toujours présent.</p></td>
<td style="vertical-align:top;"><strong>Oui</strong>
<p>Dans l’illustration suivante, l’utilisateur navigue entre deuxpages dans le même groupe d’homologues. Comme les pages n’utilisent pas d’onglets ni de volet de navigation ancré, la navigation est ajoutée à l’historique de navigation.</p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-noosnavelement.png" alt="Navigation within a peer group" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>Page à page, même groupe d’homologues, avec un élément de navigation à l’écran</strong>
<p>L’utilisateur navigue d’une page à une autre dans le même groupe d’homologues. Les deux pages sont affichées dans le même élément de navigation. Par exemple, les deuxpages utilisent le même élément onglets/tableaux croisés dynamiques, ou elles s’affichent dans un volet de navigation ancré.</p></td>
<td style="vertical-align:top;"><strong>Non</strong>
<p>Lorsque l’utilisateur appuie sur le bouton Précédent, il retourne à la dernière page avant d’avoir accédé au groupe d’homologues actuel.</p>
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