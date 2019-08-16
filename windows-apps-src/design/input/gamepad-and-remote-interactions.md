---
Description: Optimisez votre application pour l’entrée à partir du boîtier d’accès Xbox et du contrôle à distance.
title: Interactions entre le boîtier de commande et la télécommande
ms.assetid: 784a08dc-2736-4bd3-bea0-08da16b1bd47
label: Gamepad and remote interactions
template: detail.hbs
isNew: true
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 440f758e5db8bd77d3f26290eb59d7684e5f87a3
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867653"
---
# <a name="gamepad-and-remote-control-interactions"></a>Interactions entre le boîtier de commande et la télécommande

![image clavier et boîtier de commande](images/keyboard/keyboard-gamepad.jpg)

***De nombreuses expériences d’interaction sont partagées entre le boîtier, le contrôle à distance et le clavier.***

Créez des expériences d’interaction dans vos applications de plateforme Windows universelle (UWP) qui garantissent que votre application est utilisable et accessible via les types d’entrée traditionnels des PC, ordinateurs portables et tablettes (souris, clavier, toucher, etc.), ainsi que les types d’entrée. type de l’expérience TV et Xbox *10-foot* , comme le boîtier de commande et le contrôle à distance.

Consultez [conception pour la Xbox et la télévision](../devices/designing-for-tv.md) pour obtenir des conseils de conception générale sur les applications UWP dans une expérience de *10 mètres* .

## <a name="overview"></a>Vue d'ensemble

Dans cette rubrique, nous expliquons ce que vous devez prendre en compte dans votre conception d’interaction (ou ce que vous ne faites pas, si la plateforme le regarde à votre place) et que vous fournissez des conseils, des recommandations et des suggestions pour créer des applications UWP qui sont agréables à utiliser, quelle que soit appareil, type d’entrée ou préférences et préférences de l’utilisateur.

En fin de ligne, votre application doit être aussi intuitive et facile à utiliser dans l’environnement à *2 pieds* que dans l’environnement de *10 pieds* (et vice versa). Prendre en charge les appareils préférés de l’utilisateur, faire en sorte que l’interface utilisateur soit claire et Unmistakable, organisez le contenu afin que la navigation soit cohérente et prévisible, et fournissez aux utilisateurs le chemin le plus rapide possible pour ce qu’ils souhaitent faire.

> [!NOTE]
> La plupart des extraits de code dans cette rubrique sont en langage XAML/C#. Mais, les principes et les concepts s’appliquent à toutes les applications UWP. Si vous développez une application UWP en HTML/JavaScript pour Xbox, consultez l’excellente bibliothèque [TVHelpers](https://github.com/Microsoft/TVHelpers/wiki) sur GitHub.


## <a name="optimize-for-both-2-foot-and-10-foot-experiences"></a>Optimiser pour les expériences en 2 et 10 pieds

Au minimum, nous vous recommandons de tester vos applications pour vous assurer qu’elles fonctionnent bien dans les scénarios de 2 et 10 pieds, et que toutes les fonctionnalités sont détectables et accessibles au [boîtier de commande Xbox et au contrôle à distance](#gamepad-and-remote-control).

Voici d’autres façons d’optimiser votre application pour une utilisation dans des expériences en 2 et 10 pieds et avec tous les périphériques d’entrée (chacun des liens vers la section appropriée de cette rubrique).

> [!NOTE]
> Étant donné que les boîtiers de commande Xbox et les contrôles à distance prennent en charge de nombreux comportements et expériences du clavier UWP, ces recommandations sont appropriées pour les deux types d’entrée. Pour plus d’informations sur le clavier, consultez [interactions du clavier](keyboard-interactions.md) .

| Fonctionnalité        | Description           |
| -------------------------------------------------------------- |--------------------------------|
| [Navigation dans le focus XY et interaction](#xy-focus-navigation-and-interaction) | La **navigation de focus XY** permet à l’utilisateur de naviguer dans l’interface utilisateur de votre application. Toutefois, cela limite la navigation à quatre directions : haut, bas, gauche et droite. Cette section apporte des recommandations pour y remédier ainsi que d’autres considérations. |
| [Mode de la souris](#mouse-mode)|La navigation au focus XY n’est pas pratique, voire possible, pour certains types d’applications, telles que les cartes ou le dessin et la peinture d’applications. Dans ce cas, le **mode souris** permet aux utilisateurs de naviguer librement avec un boîtier ou un contrôle à distance, tout comme une souris sur un PC.|
| [Focus visuel](#focus-visual)  | L’élément visuel de focus est une bordure qui met en surbrillance l’élément d’interface utilisateur actuellement actif. Cela permet à l’utilisateur d’identifier rapidement l’interface utilisateur avec laquelle il navigue ou d’interagir.  |
| [Focalisation sur l’engagement](#focus-engagement) | L’engagement de focus oblige l’utilisateur à appuyer sur le bouton **a/SELECT** sur un boîtier de commande ou un contrôle à distance lorsqu’un élément d’interface utilisateur a le focus pour interagir avec lui. |
| [Boutons matériels](#hardware-buttons) | Le boîtier de commande et le contrôle à distance fournissent des boutons et des configurations très différents. |

## <a name="gamepad-and-remote-control"></a>Boîtier de commande et télécommande

Comme le clavier et la souris pour PC et la fonction tactile pour les téléphones et tablettes, le boîtier de commande et la télécommande sont les principaux périphériques d’entrée pour l’expérience « 10-foot ».
Cette section présente les boutons matériels et leur fonction.
Dans les sections [Interaction et navigation en mode focus XY](#xy-focus-navigation-and-interaction) et [Mode souris](#mouse-mode), vous découvrirez comment optimiser votre application en cas d’utilisation de ces périphériques d’entrée.

La qualité du comportement du boîtier de commande et de la télécommande lors de la première utilisation dépend de la prise en charge correcte du clavier dans votre application. Un moyen efficace de vous assurer que votre application fonctionne correctement avec le boîtier de commande/télécommande est de voir si celle-ci fonctionne correctement avec le clavier sur PC, puis testez-la avec un boîtier de commande/télécommande pour déterminer les points faibles dans votre interface utilisateur.

### <a name="hardware-buttons"></a>Boutons matériels

Tout au long de ce document, les boutons seront appelés par leurs noms fournis dans le schéma suivant.

![Schéma affichant les boutons d’un boîtier de commande et d’une télécommande](images/designing-for-tv/hardware-buttons-gamepad-remote.png)

Comme vous pouvez le constater sur le schéma, certains des boutons pris en charge sur le boîtier de commande ne le sont pas sur la télécommande, et vice versa. Bien que vous puissiez utiliser des boutons qui sont uniquement pris en charge sur un périphérique d’entrée (pour rendre plus rapide la navigation dans l’interface utilisateur), n’oubliez pas que leur utilisation pour des interactions critiques peut créer une situation dans laquelle l’utilisateur se trouve dans l’impossibilité d’interagir avec certaines parties de l’interface utilisateur.

Le tableau suivant répertorie tous les boutons matériels pris en charge par les applications UWP et les périphériques d’entrée qui les prennent en charge.

| Bouton                    | Boîtier de commande   | Télécommande    |
|---------------------------|-----------|-------------------|
| Bouton A/Sélectionner           | Oui       | Oui               |
| Bouton B/Précédent             | Oui       | Oui               |
| Bouton multidirectionnel   | Oui       | Oui               |
| Bouton Menu               | Oui       | Oui               |
| Bouton Afficher               | Oui       | Oui               |
| Boutons X et Y           | Oui       | Non                |
| Stick analogique gauche                | Oui       | Non                |
| Stick analogique droit               | Oui       | Non                |
| Gâchette gauche et droite   | Oui       | Non                |
| Gâchettes hautes gauche et droite    | Oui       | Non                |
| Bouton OneGuide           | Non        | Oui               |
| Bouton de volume             | Non        | Oui               |
| Bouton de changement de chaîne            | Non        | Oui               |
| Boutons de contrôle multimédia     | Non        | Oui               |
| Bouton Muet               | Non        | Oui               |

### <a name="built-in-button-support"></a>Prise en charge des boutons intégrés

La plateforme UWP mappe automatiquement le comportement d’entrée du clavier existant sur les entrées du boîtier de commande et de la télécommande. Le tableau suivant répertorie ces mappages intégrés.

| Clavier              | Boîtier de commande/Télécommande                        |
|-----------------------|---------------------------------------|
| Touches de direction            | Bouton multidirectionnel (également stick analogique gauche sur le boîtier de commande)    |
| Barre d’espace              | Bouton A/Sélectionner                       |
| Entrée                 | Bouton A/Sélectionner                       |
| Échappement                | Bouton B/Précédent*                        |

\*Lorsque les événements [KeyDown](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) et [KeyUp](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) pour le bouton B ne sont pas gérés par l’application, l’événement [SystemNavigationManager.BackRequested](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.backrequested) est déclenché, ce qui devrait entraîner la navigation vers l’arrière dans l’application. Cependant, vous devez implémenter cela vous-même, comme dans l’extrait de code suivant :

```csharp
// This code goes in the MainPage class

public MainPage()
{
    this.InitializeComponent();

    // Handling Page Back navigation behaviors
    SystemNavigationManager.GetForCurrentView().BackRequested +=
        SystemNavigationManager_BackRequested;
}

private void SystemNavigationManager_BackRequested(
    object sender,
    BackRequestedEventArgs e)
{
    if (!e.Handled)
    {
        e.Handled = this.BackRequested();
    }
}

public Frame AppFrame { get { return this.Frame; } }

private bool BackRequested()
{
    // Get a hold of the current frame so that we can inspect the app back stack
    if (this.AppFrame == null)
        return false;

    // Check to see if this is the top-most page on the app back stack
    if (this.AppFrame.CanGoBack)
    {
        // If not, set the event to handled and go back to the previous page in the
        // app.
        this.AppFrame.GoBack();
        return true;
    }
    return false;
}
```

> [!NOTE]
> Si le bouton B est utilisé pour revenir en arrière, n’affichez pas de bouton précédent dans l’interface utilisateur. Si vous utilisez un [affichage de navigation](../controls-and-patterns/navigationview.md), le bouton précédent est automatiquement masqué. Pour plus d’informations sur la navigation vers l’arrière, voir [Historique de navigation et navigation vers l’arrière pour les applications UWP](../basics/navigation-history-and-backwards-navigation.md).

Les applications UWP sur Xbox One permettent également d’appuyer sur le bouton **Menu** pour ouvrir les menus contextuels. Pour plus d’informations, voir [CommandBar et ContextFlyout](#commandbar-and-contextflyout).

### <a name="accelerator-support"></a>Prise en charge des boutons accélérateurs

Les boutons accélérateurs permettant d’accélérer la navigation dans une interface utilisateur. Cependant, ces boutons peuvent être propres à certains périphériques d’entrée ; certains utilisateurs ne seront donc pas en mesure d’utiliser ces fonctions. En réalité, le boîtier de commande est le seul périphérique d’entrée qui prend en charge les fonctions d’accélération pour les applications UWP sur Xbox One.

Le tableau suivant répertorie la prise en charge intégrée des accélérateurs dans l’UWP, en plus de ce que vous pouvez implémenter vous-même. Intégrez ces comportements à votre interface utilisateur personnalisée afin de proposer une expérience utilisateur cohérente et conviviale.

| Interaction   | Clavier/Souris   | Boîtier de commande      | Intégrée pour :  | Recommandée pour : |
|---------------|------------|--------------|----------------|------------------|
| Page vers le haut/bas  | Page vers le haut/bas | Gâchette gauche/droite | [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView), [ListBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox), [ListViewBase](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewBase), [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView), `ScrollViewer`, [Selector](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.Selector), [LoopingSelector](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.LoopingSelector), [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [FlipView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView) | Affichages qui prennent en charge le défilement vertical
| Page vers la gauche/droite | Aucun | Gâchettes hautes gauche/droite | [Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot), [ListBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox), [ListViewBase](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewBase), [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView), `ScrollViewer`, [Selector](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.Selector), [LoopingSelector](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.LoopingSelector), [FlipView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView) | Affichages qui prennent en charge le défilement horizontal
| Zoom avant/arrière        | Ctrl +/- | Gâchette gauche/droite | Aucun | `ScrollViewer`, les vues qui prennent en charge le zoom avant et arrière |
| Ouvrir/fermer le volet de navigation | Aucun | Vue | Aucun | Volets de navigation |
| Rechercher | Aucun | Bouton Y | Aucun | Raccourci pour la fonction de recherche principale dans l’application |
| [Ouvrir le menu contextuel](#commandbar-and-contextflyout) | Clic droit | Bouton Menu | [ContextFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout) | Menus contextuels |

## <a name="xy-focus-navigation-and-interaction"></a>Interaction et navigation en mode focus XY

Si votre application prend en charge une navigation en mode focus appropriée pour le clavier, cela conviendra également pour le boîtier de commande et la télécommande.
La navigation avec les touches de direction est mappée au **bouton multidirectionnel** (ainsi qu’au **stick analogique gauche** sur le boîtier de commande), et l’interaction avec les éléments d’interface utilisateur est mappée à la touche **Entrée/Sélectionner** (voir [Boîtier de commande et télécommande](#gamepad-and-remote-control)).

De nombreux événements et propriétés sont utilisés par le boîtier de commande et le clavier&mdash;les deux déclenchent des événements `KeyDown` et `KeyUp`, et accéderont uniquement à des contrôles qui sont dotés des propriétés `IsTabStop="True"` et `Visibility="Visible"`. Pour des recommandations en matière de conception de clavier, voir [Interactions avec le clavier](../input/keyboard-interactions.md).

Si la prise en charge du clavier est correctement implémentée, votre application fonctionnera assez bien ; toutefois, du travail supplémentaire peut être nécessaire pour la prise en charge de chaque scénario. Réfléchissez aux besoins spécifiques de votre application pour proposer une expérience utilisateur optimale.

> [!IMPORTANT]
> Le mode souris est activé par défaut pour les applications UWP qui s’exécutent sur Xbox One. Pour désactiver le mode souris et activer la navigation en mode focus XY, définissez `Application.RequiresPointerMode=WhenRequested`.

### <a name="debugging-focus-issues"></a>Débogage des problèmes de focus

La méthode [FocusManager.GetFocusedElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement) vous indique sur quel élément le focus se trouve actuellement. Cela s’avère utile dans des situations où l’emplacement du focus visuel n’est pas clairement identifiable. Vous pouvez consigner les informations dans la fenêtre Sortie de Visual Studio :

```csharp
page.GotFocus += (object sender, RoutedEventArgs e) =>
{
    FrameworkElement focus = FocusManager.GetFocusedElement() as FrameworkElement;
    if (focus != null)
    {
        Debug.WriteLine("got focus: " + focus.Name + " (" +
            focus.GetType().ToString() + ")");
    }
};
```

La navigation en mode XY peut ne pas fonctionner comme prévu pour trois raisons courantes :

* La valeur de la propriété [IsTabStop](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istabstop) ou [Visibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) est incorrecte.
* Le contrôle sur lequel se trouve le focus est en réalité plus large que prévu&mdash;La navigation en mode XY examine la taille totale du contrôle ([ActualWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) et [ActualHeight](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualheight)), et pas seulement la partie du contrôle effectuant le rendu d’un élément important.
* Un contrôle pouvant être actif se trouve au-dessus d’un autre&mdash;La navigation en mode XY ne prend pas en charge les contrôles qui se chevauchent.

Si la navigation en mode XY ne fonctionne toujours pas après correction de ces problèmes courants, vous pouvez pointer manuellement vers l’élément sur lequel vous voulez un focus à l’aide de la méthode décrite dans [Remplacement de la navigation par défaut](#overriding-the-default-navigation).

Si la navigation en mode XY fonctionne comme prévu, mais qu’aucun focus visuel n’est affiché, l’un des problèmes suivants peut en être la cause :

* Vous avez remodélisé le contrôle sans inclure de focus visuel. Définissez `UseSystemFocusVisuals="True"` ou ajoutez un focus visuel manuellement.
* Vous avez déplacé le focus en appelant la méthode `Focus(FocusState.Pointer)`. Le paramètre [FocusState](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FocusState) détermine ce qui se produit pour le focus visuel. En général, vous devez définir ce paramètre sur `FocusState.Programmatic`, ce qui maintient la visibilité du focus visuel si ce dernier était déjà visible, et le maintient masqué si c’était le cas précédemment.

Le reste de cette section décrit en détail les problèmes courants de conception lorsque la navigation en mode XY est utilisée, et propose plusieurs méthodes pour résoudre ces problèmes.

### <a name="inaccessible-ui"></a>Interface utilisateur inaccessible

Étant donné que la navigation en mode focus XY limite le déplacement vers le haut, le bas, à gauche et à droite, il existe des scénarios où certaines parties de l’interface utilisateur ne sont pas accessibles.
Le schéma suivant illustre un exemple du type de disposition d’interface utilisateur que la navigation en mode focus XY ne prend pas en charge.
Notez que l’élément au milieu n’est pas accessible à l’aide du boîtier de commande/télécommande dans la mesure où la navigation horizontale et la navigation verticale sont prioritaires ; l’élément du milieu ne sera jamais d’une priorité suffisamment élevée pour avoir le focus.

![Éléments dans les quatre coins avec un élément inaccessible au milieu](images/designing-for-tv/2d-navigation-best-practices-ui-layout-to-avoid.png)

Si, pour une raison quelconque, il n’est pas possible de réorganiser l’interface utilisateur, utilisez l’une des techniques décrites dans la section suivante pour remplacer le comportement de focus par défaut.

### <a name="overriding-the-default-navigation"></a>Remplacement de la navigation par défaut

Si la plateforme Windows universelle tente de garantir la convivialité de la navigation par bouton multidirectionnel/stick analogique gauche, elle ne peut pas garantir un comportement optimisé pour les intentions de votre application.
Pour vous assurer que la navigation est optimisée pour votre application, la meilleure solution consiste à la tester avec une manette de jeu et à vérifier que tous les éléments d’interface utilisateur sont accessibles par l’utilisateur, d’une manière qui soit pertinente pour les scénarios de votre application. Si les scénarios de votre application nécessitent un comportement ne pouvant être obtenu par le biais de la navigation en mode focus XY, envisagez de suivre les conseils indiqués dans les sections suivantes et/ou de remplacer le comportement afin de placer le focus sur un élément logique.

L’extrait de code suivant montre comment remplacer le comportement de navigation en mode focus XY :

```xml
<StackPanel>
    <Button x:Name="MyBtnLeft"
            Content="Search" />
    <Button x:Name="MyBtnRight"
            Content="Delete"/>
    <Button x:Name="MyBtnTop"
            Content="Update" />
    <Button x:Name="MyBtnDown"
            Content="Undo" />
    <Button Content="Home"  
            XYFocusLeft="{x:Bind MyBtnLeft}"
            XYFocusRight="{x:Bind MyBtnRight}"
            XYFocusDown="{x:Bind MyBtnDown}"
            XYFocusUp="{x:Bind MyBtnTop}" />
</StackPanel>
```

Dans ce cas, lorsque le focus est sur le bouton `Home` et que l’utilisateur navigue vers la gauche, le focus se déplace sur le bouton `MyBtnLeft` ; si l’utilisateur navigue vers la droite, le focus se déplace sur le bouton `MyBtnRight` et ainsi de suite.

Pour empêcher le focus de se déplacer dans une certaine direction à partir d’un contrôle, utilisez la propriété `XYFocus*` pour pointer sur ce même contrôle :

```xml
<Button Name="HomeButton"  
        Content="Home"  
        XYFocusLeft ="{x:Bind HomeButton}" />
```

Avec ces propriétés `XYFocus`, un parent de contrôle peut également forcer la navigation de ses enfants lorsque le candidat au focus suivant se trouve en dehors de son arborescence visuelle, à moins que l’enfant qui a le focus utilise la même propriété `XYFocus`.

```xml
<StackPanel Orientation="Horizontal" Margin="300,300">
    <UserControl XYFocusRight="{x:Bind ButtonThree}">
        <StackPanel>
            <Button Content="One"/>
            <Button Content="Two"/>
        </StackPanel>
    </UserControl>
    <StackPanel>
        <Button x:Name="ButtonThree" Content="Three"/>
        <Button Content="Four"/>
    </StackPanel>
</StackPanel>
```

Dans l’exemple ci-dessus, si le focus est sur l’objet `Button` Two et l’utilisateur navigue vers la droite, le meilleur candidat au focus est l’objet `Button` Four ; cependant, le focus est déplacé vers l’objet `Button` Three, car le parent `UserControl` l’oblige à naviguer vers celui-ci lorsqu’il se trouve en dehors de son arborescence visuelle.

### <a name="path-of-least-clicks"></a>Chemin nécessitant le moins de clics

Permettez à l’utilisateur d’effectuer les tâches les plus courantes avec le moins de clics possible. Dans l’exemple suivant, la classe [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) est placée entre le bouton **Lecture** (qui a initialement le focus) et un élément couramment utilisé ; un élément inutile est ainsi placé entre les tâches prioritaires.

![Meilleures pratiques en matière de navigation nécessitant le moins de clics](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks.png)

Dans l’exemple suivant, la classe [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) est placée au-dessus du bouton **Lecture** à la place.
Le simple fait de réorganiser l’interface utilisateur afin que les éléments inutiles ne soient pas placés entre les tâches prioritaires améliore considérablement la facilité d’utilisation de votre application.

![TextBlock déplacée au-dessus du bouton de lecture afin qu’elle ne soit plus entre les tâches prioritaires](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks-2.png)

### <a name="commandbar-and-contextflyout"></a>CommandBar et ContextFlyout

Lorsque vous utilisez un [CommandBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar), gardez à l’esprit le problème de défilement d’une liste comme indiqué [dans problème: Éléments d’interface utilisateur situés après une longue liste de](#problem-ui-elements-located-after-long-scrolling-list-grid)défilement/grille. L’image suivante illustre une disposition d’interface utilisateur avec la `CommandBar` en bas d’une liste/grille. L’utilisateur devrait faire défiler toute la liste/grille pour atteindre la `CommandBar`.

![CommandBar en bas de la liste/grille](images/designing-for-tv/2d-navigation-best-practices-commandbar-and-contextflyout.png)

Que se passe-t `CommandBar` -il si vous placez le au *-dessus* de la liste/grille? Bien qu’un utilisateur ayant effectué un défilement vers le bas de la liste/grille doive en refaire un vers le haut pour atteindre le contrôle `CommandBar`, cette navigation est un peu plus courte que la configuration précédente. On suppose ici que le focus initial de votre application est placé à côté ou au-dessus de la `CommandBar` ; cette approche ne fonctionne pas aussi bien si le focus initial se trouve sous la liste/grille. Si ces éléments `CommandBar` sont des éléments d’action globale auxquels l’utilisateur n’accède que rarement (tels qu’un bouton **Synchronisation**), le fait de les disposer au-dessus de la liste/grille reste acceptable.

Bien qu’il soit impossible d’empiler les éléments d’une `CommandBar` verticalement, le fait de les placer en face du défilement (par exemple, à gauche ou à droite d’une liste avec défilement vertical, ou en haut ou en bas d’une liste avec défilement horizontal) constitue une autre option que vous devrez peut-être prendre en compte si elle fonctionne bien pour la disposition de votre interface utilisateur.

Si votre application dispose d’une `CommandBar` dont les éléments doivent être facilement accessibles aux utilisateurs, vous devrez peut-être les placer à l’intérieur d’un [ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout) et les supprimer de la `CommandBar`. `ContextFlyout`est une propriété d' [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) et est le [menu contextuel](../controls-and-patterns/dialogs-and-flyouts/index.md) associé à cet élément. Sur PC, lorsque cliquez à l’aide du bouton droit sur un élément avec un `ContextFlyout`, ce menu contextuel apparaît. Sur Xbox One, cela se produit lorsque vous appuyez sur le bouton **Menu** tandis que le focus se trouve sur un tel élément.

### <a name="ui-layout-challenges"></a>Défis en matière de disposition de l’interface utilisateur

Certaines dispositions d’interface utilisateur posent plus de défis en raison de la navigation en mode focus XY et doivent être évaluées au cas par cas. Bien qu’il n’existe pas de méthode « absolue » et que la solution que vous choisissez dépend des besoins spécifiques de votre application, certaines techniques permettent de créer une excellente expérience TV.

Pour mieux comprendre ce point, examinons un exemple d’application qui illustre certains de ces problèmes et les techniques permettant de les résoudre.

> [!NOTE]
> Cet exemple d’application sert à illustrer des problèmes d’interface utilisateur et leurs solutions potentielles, et ne vise pas à présenter la meilleure expérience utilisateur possible pour votre application.

Ce qui suit est un exemple d’application pour le secteur immobilier qui affiche une liste des maisons disponibles à la vente, une carte, la description des propriétés, ainsi que d’autres informations. Cette application pose trois défis que vous pouvez surmonter à l’aide des techniques suivantes :

- [Réorganisation de l’interface utilisateur](#ui-rearrange)
- [Focalisation sur l’engagement](#engagement)
- [Mode de la souris](#mouse-mode)

![Exemple d’application pour le secteur immobilier](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app.png)

#### Problème : Éléments d’interface utilisateur situés après une longue liste de défilement/grille<a name="problem-ui-elements-located-after-long-scrolling-list-grid"></a>

La [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) des propriétés visibles dans l’image suivante est une très longue liste à faire défiler. Si l’[activation](#focus-engagement) n’est *pas* requise pour la `ListView`, le focus se place sur le premier élément de la liste lorsque l’utilisateur navigue vers cette dernière. L’utilisateur doit parcourir tous les éléments de la liste pour atteindre le bouton **Précédent** ou **Suivant**. Dans ces cas peu pratiques où l’utilisateur doit parcourir toute la liste &mdash;c’est-à-dire, lorsque la liste est trop longue pour que cette expérience soit acceptable&mdash;, vous devez envisager d’autres options.

![Application pour le secteur immobilier : une liste comprenant 50 éléments nécessite 51 clics pour atteindre les boutons du bas.](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app-list.png)

#### <a name="solutions"></a>Solutions

**Réorganisation de l’interface utilisateur<a name="ui-rearrange"></a>**

À moins que le focus initial ne soit placé au bas de la page, les éléments d’interface utilisateur placés au-dessus d’une liste à long défilement sont en général plus facilement accessibles que s’ils étaient placés au-dessous.
Si cette nouvelle disposition fonctionne pour d’autres appareils, il serait moins coûteux en ressources de modifier la disposition pour toutes les familles d’appareils au lieu d’apporter des modifications d’interface utilisateur spécifiques pour Xbox One.
En outre, le fait de placer des éléments d’interface utilisateur à l’opposé du sens de défilement (autrement dit, horizontalement pour une liste à défilement vertical ou verticalement pour une liste à défilement horizontal) améliorera davantage l’accessibilité.

![Application pour le secteur immobilier : Placement des boutons au-dessus d’une liste à long défilement](images/designing-for-tv/2d-focus-navigation-and-interaction-ui-rearrange.png)

**Focalisation sur l’engagement<a name="engagement"></a>**

Lorsqu’une activation est *requise*, la totalité de la `ListView` devient une cible du focus unique. L’utilisateur sera en mesure d’ignorer le contenu de la liste afin d’accéder au prochain élément pouvant être actif. Pour en savoir plus sur les contrôles prenant en charge l’activation et sur leur utilisation, consultez la section [Activation du focus](#focus-engagement).

![Application pour le secteur immobilier : définition de l’activation sur Requis pour atteindre les boutons Précédent/Suivant en un clic](images/designing-for-tv/2d-focus-navigation-and-interaction-engagement.png)

#### <a name="problem-scrollviewer-without-any-focusable-elements"></a>Problème : ScrollViewer sans élément pouvant être actif

Étant donné que la navigation en mode focus XY dépend de la navigation vers un seul élément d’interface utilisateur pouvant être actif à la fois, un [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) ne contenant aucun élément pouvant être actif (par exemple, contenant seulement du texte), peut provoquer un scénario dans lequel l’utilisateur n’est pas en mesure d’afficher l’ensemble du contenu dans le `ScrollViewer`.
Pour connaître les solutions de ce scénario et des scénarios connexes, voir [Activation du focus](#focus-engagement).

![Application immobilière: ScrollViewer avec texte uniquement](images/designing-for-tv/2d-focus-navigation-and-interaction-scrollviewer.png)

#### <a name="problem-free-scrolling-ui"></a>Problème : Interface utilisateur de défilement libre

Si votre application nécessite une interface utilisateur à défilement libre, telle qu’une surface de dessin ou, dans l’exemple présent, une carte, la navigation en mode focus XY ne fonctionne simplement pas.
Dans ce cas, vous pouvez activer le [mode souris](#mouse-mode) pour permettre à l’utilisateur de naviguer librement à l’intérieur d’un élément d’interface utilisateur.

![Mapper un élément d’interface utilisateur à l’aide du mode souris](images/designing-for-tv/map-mouse-mode.png)

## <a name="mouse-mode"></a>Mode souris

Comme décrit dans [Interaction et navigation en mode focus XY](#xy-focus-navigation-and-interaction), le focus est déplacé à l’aide du système de navigation XY sur Xbox One, ce qui permet à l’utilisateur de déplacer le focus entre les contrôles en effectuant des déplacements vers le haut, le bas, la gauche et la droite.
Toutefois, certains contrôles comme [WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) et [MapControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) nécessitent une interaction semblable à celle faite avec la souris, avec laquelle les utilisateurs peuvent déplacer le pointeur librement à l’intérieur des limites du contrôle.
Certaines applications permettent également de déplacer le pointeur sur la page entière ; l’expérience boîtier de commande/télécommande est alors semblable à l’expérience souris sur PC.

Pour ces scénarios, demandez un pointeur (mode souris) pour la page entière, ou sur un contrôle à l’intérieur d’une page.
Par exemple, votre application peut contenir une page comportant un contrôle `WebView` qui utilise le mode souris uniquement à l’intérieur du contrôle tandis que la navigation en mode focus XY est utilisée partout ailleurs.
Pour demander un pointeur, vous pouvez spécifier si vous le voulez **lorsqu’un contrôle ou une page sont actifs** ou **lorsqu’une page a le focus**.

> [!NOTE]
> La demande de pointeur lorsqu’un contrôle a le focus n’est pas prise en charge.

Pour les applications XAML et les applications web hébergées qui s’exécutent sur Xbox One, le mode souris est activé par défaut pour l’ensemble de l’application. Il est vivement conseillé de désactiver cette fonctionnalité et d’optimiser votre application pour la navigation en mode XY. Pour ce faire, définissez la propriété `Application.RequiresPointerMode` sur `WhenRequested` afin d’activer le mode souris uniquement lorsqu’un contrôle ou une page le demandent.

Pour ce faire, dans une application XAML, utilisez le code suivant dans votre classe `App` :

```csharp
public App()
{
    this.InitializeComponent();
    this.RequiresPointerMode =
        Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
    this.Suspending += OnSuspending;
}
```

Pour plus d’informations et des exemples de code HTML/JavaScript, consultez [Comment désactiver le mode souris](../../xbox-apps/how-to-disable-mouse-mode.md).

Le schéma suivant montre les mappages de bouton pour le boîtier de commande/la télécommande en mode souris.

![Mappages de bouton pour boîtier de commande/télécommande en mode souris](images/designing-for-tv/10ft_infographics_mouse-mode.png)

> [!NOTE]
> Le mode souris est uniquement pris en charge sur Xbox One avec boîtier de commande/télécommande. Il est ignoré sans avertissement dans d’autres familles d’appareils et types d’entrée.

Utilisez la propriété [RequiresPointer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.requirespointer) dans un contrôle ou une page pour activer le mode souris sur ceux-ci. Cette propriété a trois valeurs possibles : `Never` (valeur par défaut), `WhenEngaged` et `WhenFocused`.

### <a name="activating-mouse-mode-on-a-control"></a>Activation du mode souris sur un contrôle

Lorsque l’utilisateur active un contrôle avec `RequiresPointer="WhenEngaged"`, le mode souris est activé sur le contrôle en question jusqu’à ce que l’utilisateur le désactive. L’extrait de code suivant montre un `MapControl` simple qui, lorsqu’il est activé, active le mode souris :

```xml
<Page>
    <Grid>
        <MapControl IsEngagementRequired="true"
                    RequiresPointer="WhenEngaged"/>
    </Grid>
</Page>
```

> [!NOTE]
> Si un contrôle active le mode souris lorsqu’il est employé, il doit également être employé avec `IsEngagementRequired="true"` ; sinon, le mode souris n’est pas activé.

Lorsqu’un contrôle est en mode souris, ses contrôles imbriqués le sont également. Le mode demandé de ses enfants est ignoré &mdash; un parent ne peut pas être en mode souris si ses enfants ne le sont pas également.

En outre, le mode demandé d’un contrôle est examiné uniquement lorsqu’il a le focus ; le mode n’est pas changé dynamiquement tant qu’il a le focus.

### <a name="activating-mouse-mode-on-a-page"></a>Activation du mode souris dans une page

Lorsqu’une page dispose de la propriété `RequiresPointer="WhenFocused"`, le mode souris est activé pour la page entière lorsqu’elle a le focus. L’extrait de code suivant illustre l’obtention de cette propriété par une page :

```xml
<Page RequiresPointer="WhenFocused">
    ...
</Page>
```

> [!NOTE]
> La valeur `WhenFocused` est uniquement prise en charge dans les objets [Page](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page). Une exception est levée si vous tentez de définir cette valeur sur un contrôle.

### <a name="disabling-mouse-mode-for-full-screen-content"></a>Désactivation du mode souris pour le contenu en plein écran

Lors de l’affichage de vidéos ou d’autres types de contenu en plein écran, il est généralement préférable de masquer le curseur, car celui-ci peut distraire l’utilisateur. Ce scénario se produit lorsque le reste de l’application utilise le mode souris, mais que vous souhaitez désactiver ce mode lors de l’affichage de contenu en plein écran. Pour ce faire, placez le contenu en plein écran dans son propre objet `Page`, puis suivez les étapes ci-dessous.

1. Dans l’objet `App`, définissez `RequiresPointerMode="WhenRequested"`.
2. Dans chaque objet `Page`, *excepté* l’objet `Page` en plein écran, définissez `RequiresPointer="WhenFocused"`.
3. Pour l’objet `Page` en plein écran, définissez `RequiresPointer="Never"`.

De cette façon, le curseur n’apparaîtra jamais lors de l’affichage de contenu plein écran.

## <a name="focus-visual"></a>Visuel du focus

Le visuel du focus est le contour de l’élément de l’interface utilisateur sur lequel se trouve actuellement le focus. Cela permet de guider l’utilisateur pour une navigation facile dans votre interface utilisateur et sans se perdre.

Avec l’ajout d’une mise à jour visuelle et de nombreuses options de personnalisation au visuel du focus, les développeurs peuvent être certains qu’un seul visuel du focus fonctionne correctement sur les PC et Xbox One, ainsi que sur tous les appareils Windows 10 prenant en charge le clavier et/ou le boîtier de commande/télécommande.

Bien que le même visuel du focus puisse être utilisé sur différentes plateformes, le contexte diffère légèrement pour l’expérience « 10-foot ». Vous devez supposer que l’utilisateur ne se concentre pas sur l’ensemble de l’écran de TV. C’est pourquoi il est important que l’élément ayant actuellement le focus s’affiche toujours clairement à l’utilisateur ; cela évite la frustration de devoir rechercher les éléments visuels.

Il est également important de garder à l’esprit que le visuel du focus s’affiche par défaut lorsqu’un boîtier de commande ou une télécommande sont utilisés, mais *pas* un clavier. Il s’affiche donc lorsque vous exécutez votre application sur Xbox One, même si vous ne l’avez pas implémenté.

### <a name="initial-focus-visual-placement"></a>Placement initial du visuel du focus

Lors du lancement d’une application ou de la navigation vers une page, placez le focus sur un élément d’interface utilisateur approprié (le premier élément auquel l’utilisateur applique une action). Par exemple, une application de photos peut placer le focus sur le premier élément dans la galerie de photos, et une application de musique, dans laquelle l’utilisateur a ouvert une vue détaillée d’un morceau, peut placer le focus sur le bouton Lecture pour faciliter la lecture de la musique.

Essayez de placer le focus initial dans la zone supérieure gauche de votre application (ou en haut à droite pour un flux de droite à gauche). La plupart des utilisateurs ont tendance à se concentrer sur cet angle en premier car c’est à cet endroit que commence généralement le flux de contenu d’une application.

### <a name="making-focus-clearly-visible"></a>Rendre le focus clairement visible

Un visuel du focus doit toujours être visible à l’écran pour que l’utilisateur puisse reprendre à l’endroit où il s’est arrêté sans avoir à chercher le focus. De même, des éléments pouvant être actifs doivent toujours se trouver à l’écran&mdash;Par exemple, n’utilisez pas de fenêtres contextuelles contenant uniquement du texte sans éléments pouvant être actifs.

Les expériences en plein écran, telles que la lecture de vidéos ou le visionnage de photos, pourraient constituer une exception à cette règle. Dans ces cas, l’affichage du focus visuel n’est pas approprié.

### <a name="reveal-focus"></a>Effet Révéler focus

Révéler focus est un effet visuel qui anime la bordure des éléments susceptibles d’être activés, comme un bouton, lorsque l’utilisateur déplace le focus du clavier ou du boîtier de commande sur ces derniers. En ajoutant un éclat animé autour de la bordure des éléments actifs, Révéler focus permet aux utilisateurs de mieux comprendre où le focus se trouve et où il va.

Par défaut, Révéler focus est désactivé. Pour des expériences « 10-foot », vous devez accepter de faire apparaître le focus en définissant [Application.FocusVisualKind propriété](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind) dans le constructeur de votre application.

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

Pour plus d’informations, voir les recommandations fournies pour [Révéler focus](/windows/uwp/design/style/reveal-focus).

### <a name="customizing-the-focus-visual"></a>Personnaliser le focus visuel

Si vous souhaitez personnaliser le focus visuel, vous pouvez le faire en modifiant les propriétés relatives au focus visuel pour chaque contrôle. Vous pouvez tirer parti de plusieurs de ces propriétés afin de personnaliser votre application.

Vous pouvez même désactiver le focus visuel fourni par le système en dessinant votre propre focus visuel à l’aide des états visuels. Pour en savoir plus, voir [VisualState](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState).

### <a name="light-dismiss-overlay"></a>Superposition de l’abandon interactif

Pour attirer l’attention de l’utilisateur sur les éléments d’interface utilisateur que ce dernier manipule avec le contrôleur de jeu ou la télécommande, l’UWP ajoute automatiquement une couche « de fumée » qui couvre les zones en dehors de l’interface utilisateur contextuelle lorsque l’application s’exécute sur Xbox One. Cela ne nécessite aucun travail supplémentaire, mais est un élément à prendre en compte lors de la conception de votre interface utilisateur. Vous pouvez définir la propriété `LightDismissOverlayMode` sur un `FlyoutBase` quelconque pour activer ou désactiver la couche de fumée ; la valeur par défaut est `Auto`, ce qui signifie qu’elle est activée sur Xbox et désactivée ailleurs. Pour plus d’informations, voir [Boîte de dialogue modale et abandon interactif](../controls-and-patterns/menus.md).

## <a name="focus-engagement"></a>Activation du focus

L’activation du focus est conçue pour faciliter l’utilisation d’une manette de jeu ou d’une télécommande pour interagir avec une application.

> [!NOTE]
> La définition de l’activation du focus n’affecte pas le clavier ou les autres périphériques d’entrée.

Lorsque la propriété `IsFocusEngagementEnabled` d’un objet [FrameworkElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) est définie sur `True`, elle signale le contrôle comme nécessitant l’activation du focus. Cela signifie que l’utilisateur doit appuyer sur le bouton **A/Sélectionner** pour activer le contrôle et interagir avec ce dernier. Lorsqu’il a terminé, l’utilisateur peut appuyer sur le bouton **B/Précédent** pour désactiver le contrôle et naviguer hors de ce dernier.

> [!NOTE]
> `IsFocusEngagementEnabled`est une nouvelle API qui n’est pas encore documentée.

### <a name="focus-trapping"></a>Interruption du focus

L’interruption du focus survient lorsqu’un utilisateur tente d’accéder à l’interface utilisateur d’une application, mais la navigation est « interrompue » au sein d’un contrôle, rendant difficile, voire impossible, le déplacement en dehors de ce contrôle.

L’exemple suivant montre une interface utilisateur provoquant l’interruption du focus.

![Boutons à gauche et à droite d’un curseur horizontal](images/designing-for-tv/focus-engagement-focus-trapping.png)

Si l’utilisateur veut accéder au bouton droit à partir du bouton gauche, il serait logique de supposer que la seule action requise serait d’appuyer deux fois sur le bouton multidirectionnel/stick analogique gauche.
Toutefois, si le [Slider](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) ne nécessite pas d’activation, le comportement suivant se produit : lorsque l’utilisateur appuie à droite pour la première fois, le focus est décalé vers le `Slider` ; lorsqu’il réappuie à droite, la poignée du `Slider` se déplace à droite. L’utilisateur continuerait de déplacer la poignée vers la droite sans toutefois réussir à atteindre le bouton.

Plusieurs approches existent pour contourner ce problème. L’une consiste à concevoir une disposition différente, comme dans l’exemple d’application pour le secteur immobilier dans [Interaction et navigation en mode focus XY](#xy-focus-navigation-and-interaction), où nous avons déplacé les boutons **Précédent** et **Suivant** au-dessus de `ListView`. L’empilement vertical (et non horizontal) des contrôles, comme le montre l’image suivante, peut résoudre le problème.

![Boutons au-dessus et en dessous d’un curseur horizontal](images/designing-for-tv/focus-engagement-focus-trapping-2.png)

À présent, l’utilisateur peut accéder à chaque contrôle en appuyant sur les boutons haut et bas sur le bouton multidirectionnel/stick analogique gauche. Lorsque le `Slider` a le focus, l’utilisateur peut appuyer sur le bouton gauche/droite pour déplacer la poignée du `Slider`, comme prévu.

Une autre approche pour résoudre ce problème consiste à demander une activation du `Slider`. Si vous définissez `IsFocusEngagementEnabled="True"`, cela se traduit par le comportement suivant.

![Nécessité d’activation du focus sur un curseur afin que l’utilisateur puisse accéder au bouton situé à droite](images/designing-for-tv/focus-engagement-slider.png)

Lorsque le `Slider` nécessite une activation du focus, l’utilisateur peut accéder au bouton situé à droite en appuyant simplement deux fois à droite sur le bouton multidirectionnel/stick analogique gauche. Cette solution est excellente, car elle ne nécessite aucun ajustement de l’interface utilisateur et produit le comportement attendu.

### <a name="items-controls"></a>Contrôles d’éléments

Outre le contrôle [Slider](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider), il existe d’autres contrôles que vous souhaiterez peut-être activer :

- [ListBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox)
- [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)
- [FlipView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView)

Contrairement au contrôle `Slider`, ces contrôles n’interrompent pas le focus en leur sein ; cependant, ils peuvent poser des problèmes en matière de facilité d’utilisation s’ils contiennent de grandes quantités de données. Voici un exemple d’un contrôle `ListView` qui contient une grande quantité de données.

![Contrôle ListView avec une grande quantité de données et des boutons au-dessus et en dessous](images/designing-for-tv/focus-engagement-list-and-grid-controls.png)

Comme pour l’exemple `Slider`, nous allons essayer de naviguer à partir du bouton du haut vers le bouton du bas avec un boîtier de commande/une télécommande.
En partant du bouton du haut, où se trouve le focus, le fait d’appuyer sur la flèche Bas sur le bouton multidirectionnel/stick analogique gauche déplace le focus sur le premier élément dans le contrôle `ListView` (« Item 1 »).
Lorsque l’utilisateur appuie à nouveau, le focus est déplacé sur l’élément suivant dans la liste, et non sur le bouton situé en bas.
Pour accéder au bouton du bas, l’utilisateur doit d’abord parcourir chaque élément du contrôle `ListView`.
Si le contrôle `ListView` contient une grande quantité de données, cela peut s’avérer peu pratique et dégrader l’expérience utilisateur.

Pour résoudre ce problème, définissez la propriété `IsFocusEngagementEnabled="True"` sur `ListView` pour demander son activation.
Cela permet à l’utilisateur de traverser rapidement le contrôle `ListView` en appuyant simplement sur le bouton Bas. Néanmoins, l’utilisateur n’est pas en mesure de parcourir la liste ou de sélectionner un élément dans celle-ci, sauf s’il l’active en appuyant sur le bouton **A/Sélectionner** lorsqu’il a le focus, puis en appuyant sur le bouton **B/Précédent** pour le désactiver.

![Contrôle ListView avec activation requise](images/designing-for-tv/focus-engagement-list-and-grid-controls-2.png)

#### <a name="scrollviewer"></a>ScrollViewer

Le contrôle [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer), qui possède des caractéristiques particulières, diffère légèrement des contrôles précédents. Si vous disposez d’un `ScrollViewer` avec du contenu pouvant être actif, par défaut, la navigation vers `ScrollViewer` vous permet de parcourir ses éléments pouvant être actifs. Comme pour un contrôle `ListView`, vous devez parcourir chaque élément pour naviguer en dehors du contrôle `ScrollViewer`.

Si le contrôle `ScrollViewer` n’a *pas* de contenu pouvant être actif &mdash; par exemple, s’il contient uniquement du texte&mdash;, vous pouvez définir `IsFocusEngagementEnabled="True"` afin que l’utilisateur puisse activer `ScrollViewer` à l’aide du bouton **A/Sélectionner**. Une fois activé, l’utilisateur peut faire défiler le texte à l’aide du **bouton multidirectionnel/stick analogique gauche**, puis appuyer sur le bouton **B/Précédent** pour le désactiver.

Une autre approche consiste à définir `IsTabStop="True"` sur le contrôle `ScrollViewer` afin que l’utilisateur n’ait pas à activer le contrôle. Il peut simplement y placer le focus, puis le faire défiler à l’aide du **bouton multidirectionnel/stick analogique gauche** lorsqu’il n’existe pas d’éléments pouvant être actifs au sein du contrôle `ScrollViewer`.&mdash;

### <a name="focus-engagement-defaults"></a>Paramètres par défaut de l’activation du focus

Certains contrôles provoquent une interruption du focus assez fréquente pour justifier une activation du focus pour leurs paramètres par défaut. D’autres ont désactivé cette fonction par défaut, mais peuvent tirer profit de son activation. Le tableau suivant répertorie ces contrôles et leurs comportements d’activation du focus par défaut.

| Contrôle               | Paramètre par défaut de l’activation du focus  |
|-----------------------|---------------------------|
| CalendarDatePicker    | Activé                        |
| FlipView              | Désactivé                       |
| GridView              | Désactivé                       |
| Listbox               | Désactivé                       |
| Affichage de liste              | Désactivé                       |
| ScrollViewer          | Désactivé                       |
| SemanticZoom          | Désactivé                       |
| Curseur                | Activé                        |

Aucun autre contrôle UWP n’a de modification comportementale ou visuelle lorsque `IsFocusEngagementEnabled="True"`.

## <a name="summary"></a>Récapitulatif

Vous pouvez créer des applications UWP optimisées pour un appareil ou une expérience spécifique, mais le plateforme Windows universelle vous permet également de créer des applications qui peuvent être utilisées avec succès sur plusieurs appareils, à la fois dans des expériences en 2 et 10 pieds, et indépendamment des entrées capacité de l’appareil ou de l’utilisateur. À l’aide des recommandations de cet article, vous pouvez vous assurer que votre application est aussi efficace que possible à la fois sur la télévision et sur un PC.

## <a name="related-articles"></a>Articles connexes

- [Conception pour Xbox et TV](../devices/designing-for-tv.md)
- [Applications Device Primer for plateforme Windows universelle (UWP)](index.md)
