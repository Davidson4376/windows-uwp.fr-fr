---
Description: Design your app so that it looks good and functions well on your television.
title: Conception pour Xbox et TV
ms.assetid: 780209cb-3e8a-4cf7-8f80-8b8f449580bf
label: Designing for Xbox and TV
template: detail.hbs
isNew: true
keywords: Xbox, télévision, expérience «10-foot», boîtier de commande, télécommande, entrées, interactions
ms.date: 11/13/2018
ms.topic: article
pm-contact: chigy
design-contact: jeffarn
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 7b09e90ea71872421a0caf1edcc1a03ce25fef42
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8930660"
---
# <a name="designing-for-xbox-and-tv"></a>Conception pour Xbox et télévision

Concevez votre application de plateformeWindows universelle (UWP) pour une esthétique et un fonctionnement optimaux sur les écrans de télévision et XboxOne.

## <a name="overview"></a>Vue d’ensemble

La plateformeWindows universelle vous permet de créer des expériences agréables sur plusieurs types d’appareils Windows10.
La plupart des fonctionnalités fournies par l’infrastructureUWP permettent aux applications d’utiliser la même interface utilisateur sur ces appareils, sans travail supplémentaire.
Toutefois, des éléments particuliers sont à prendre en compte afin d’optimiser votre application et l’adapter pour un fonctionnement parfait sur les écrans de télévision et XboxOne.

L’expérience qui consiste à se trouver assis sur son fauteuil en face de la télévision et à interagir avec celle-ci à l’aide d’un boîtier de commande ou d’une télécommande est appelée le «**10-foot experience**»
Ce nom vient du fait que l’utilisateur se trouve généralement à 3mètres (10pieds) de l’écran.
Cela soulève des défis propres à cette expérience, qui ne sont pas présents dans l’expérience «*2-foot experience*» ou lors d’interactions avec un PC.
Si vous développez une application pour XboxOne ou tout autre appareil dont la sortie et l’entrée se font respectivement sur télévision et par boîtier de commande, vous devez toujours garder ceci à l’esprit.

Toutes les étapes décrites dans cet article ne sont pas nécessaires pour que votre application fonctionne pour les expériences «10-foot experience», mais le fait de les comprendre et de prendre les décisions appropriées pour votre application créera une meilleure «10-footexperience», mieux adaptée aux besoins spécifiques de votre application.
Lorsque vous concevez une application pour un environnement de 3mètres, prenez en compte les principes de conception suivants.

### <a name="simple"></a>Simplicité

La conception pour un environnement de 3mètres présente des défis uniques. La résolution et la distance d’affichage peuvent rendre difficile pour les utilisateurs l’assimilation d’une trop grande quantité d’informations.
Faites en sorte que votre conception soit épurée et réduite aux composants les plus simples. La quantité d’informations affichées sur une télévision doit être comparable à ce que vous pourriez voir sur un téléphone mobile, plutôt que sur un bureau d’ordinateur.

![Écran d’accueil de XboxOne](images/designing-for-tv/xbox-home-screen.png)

### <a name="coherent"></a>Cohérence

Les applications UWP dans un environnement de 3mètres doivent être intuitives et faciles d’utilisation. Le focus doit apparaître clairement.
Organisez le contenu de manière à ce que la navigation soit prévisible et cohérente. Fournissez à vos utilisateurs le chemin d’accès le plus rapide au contenu souhaité.

![L’application XboxOneMovies](images/designing-for-tv/xbox-movies-app.png)

_**Tous les films présentés dans la capture d’écran sont disponibles sur Films et TVMicrosoft.**_  

### <a name="captivating"></a>Captivant

Les expériences les plus immersives et cinématographiques se passent sur grand écran. Des paysages de bord à bord et l’utilisation de couleurs et d’une typographie vives font passer vos applications au niveau supérieur. Osez. Imaginez.

![Application des avatars de XboxOne](images/designing-for-tv/xbox-avatar-app.png)

### <a name="optimizations-for-the-10-foot-experience"></a>Optimisations en matière d’expérience «10-foot»

À présent que vous connaissez les principes d’une bonne conception d’applicationUWP pour une expérience «10-foot», lisez les descriptions suivantes pour vous approprier les différentes façons d’optimiser votre application et créer une expérience utilisateur améliorée.

| Fonctionnalité        | Description           |
| -------------------------------------------------------------- |--------------------------------|
| [Boîtier de commande et télécommande](#gamepad-and-remote-control)      | Le bon fonctionnement de votre application avec un boîtier de commande et une télécommande représente l’étape la plus importante de l’optimisation des expériences «10-foot». Vous pouvez cependant apporter des améliorations relatives aux boîtiers de commande et aux télécommandes pour optimiser l’expérience d’interaction utilisateur sur un appareil où leurs actions sont relativement limitées. |
| [Interaction et navigation en mode focusXY](#xy-focus-navigation-and-interaction) | La plateforme UWP propose une **navigation en mode focus XY** qui permet à l’utilisateur de naviguer dans l’interface utilisateur de votre application. Toutefois, cela limite la navigation à quatre directions: haut, bas, gauche et droite. Cette section apporte des recommandations pour y remédier ainsi que d’autres considérations. |
| [Mode souris](#mouse-mode)|Dans certaines interfaces utilisateur, telles que les cartes et les surfaces de dessin, l’utilisation de la navigation en mode focusXY est impossible ou peu pratique. Pour ces interfaces, la plateforme UWP propose le **mode souris** pour que l’utilisateur puisse naviguer librement avec le boîtier de commande/la télécommande, comme avec une souris sur un ordinateur de bureau.|
| [Visuel du focus](#focus-visual)  | Le visuel du focus est le contour de l’élément de l’interface utilisateur sur lequel se trouve actuellement le focus. Cela permet de guider l’utilisateur pour une navigation facile dans votre interface utilisateur et sans se perdre. Si le focus n’est pas clairement visible, l’utilisateur peut se perdre dans votre interface utilisateur et garder ainsi une mauvaise impression de son expérience.  |
| [Activation du focus](#focus-engagement) | Définir l’activation du focus sur un élément d’interface utilisateur nécessite que l’utilisateur appuie sur le bouton **A/Sélectionner** pour interagir avec lui. Cela peut contribuer à créer une meilleure expérience pour l’utilisateur lors de sa navigation dans l’interface utilisateur de votre application.
| [Redimensionnement des éléments de l’interface utilisateur](#ui-element-sizing)  | La plateforme Windows universelle utilise la [mise à l’échelle et les pixels effectifs](../basics/design-and-ui-intro.md#effective-pixels-and-scaling) pour mettre à l’échelle l’interface utilisateur en fonction de la distance d’affichage. Le fait de comprendre le redimensionnement et de l’appliquer à votre interface utilisateur vous aide à optimiser votre environnement de 3mètres.  |
|  [Zones adaptées à l’écran de TV](#tv-safe-area) | La plateformeUWP évite automatiquement et par défaut l’affichage de contenu dans les zones non adaptées à l’écran de TV (près des bords de l’écran). Cela crée cependant un effet «d’encadré»; l’interface utilisateur semble alors s’afficher dans un cadre. Pour que votre application soit véritablement immersive sur les écrans de télévision, vous devez la modifier afin qu’elle s’étende jusqu’aux bords des écrans compatibles. |
| [Couleurs](#colors)  |  La plateforme UWP prend en charge les thèmes de couleur. Une application qui respecte le thème du système sera **foncée** par défaut sur Xbox One. Si votre application possède un thème de couleur spécifique, gardez à l’esprit que certaines couleurs ne fonctionnent pas correctement sur les écrans de télévision et doivent donc être évitées. |
| [Son](../style/sound.md)    | Les sons jouent un rôle clé dans l’expérience «10-foot», contribuant ainsi à l’envoi de commentaires à l’utilisateur. La plateforme UWP fournit des fonctionnalités qui activent automatiquement les sons des contrôles courants lorsque l’application s’exécute sur XboxOne. Découvrez la prise en charge des sons intégrée à la plateforme UWP et comment en tirer partie.    |
| [Recommandations en matière de contrôles d’interface utilisateur](#guidelines-for-ui-controls)  |  Il existe plusieurs contrôles d’interface utilisateur qui fonctionnent correctement sur plusieurs appareils, mais pour lesquels certains éléments doivent être pris en compte s’ils sont utilisés sur un téléviseur. Découvrez certaines meilleures pratiques portant sur l’utilisation de ces contrôles lors de la conception pour l’expérience «10-foot». |
| [Déclencheur d’état visuel personnalisé pour Xbox](#custom-visual-state-trigger-for-xbox) | Pour personnaliser votre applicationUWP pour l’expérience «10-foot», nous vous recommandons d’utiliser un *déclencheur d’état visuel* personnalisé pour modifier la disposition lorsque l’application détecte son lancement sur une console Xbox.

> [!NOTE]
> La plupart des extraits de code dans cette rubrique sont en langage XAML/C#. Mais, les principes et les concepts s’appliquent à toutes les applicationsUWP. Si vous développez une application UWP en HTML/JavaScript pour Xbox, consultez l’excellente bibliothèque [TVHelpers](https://github.com/Microsoft/TVHelpers/wiki) sur GitHub.

## <a name="gamepad-and-remote-control"></a>Boîtier de commande et télécommande

Comme le clavier et la souris pour PC et la fonction tactile pour les téléphones et tablettes, le boîtier de commande et la télécommande sont les principaux périphériques d’entrée pour l’expérience «10-foot».
Cette section présente les boutons matériels et leur fonction.
Dans les sections [Interaction et navigation en mode focus XY](#xy-focus-navigation-and-interaction) et [Mode souris](#mouse-mode), vous découvrirez comment optimiser votre application en cas d’utilisation de ces périphériques d’entrée.

La qualité du comportement du boîtier de commande et de la télécommande lors de la première utilisation dépend de la prise en charge correcte du clavier dans votre application. Un moyen efficace de vous assurer que votre application fonctionne correctement avec le boîtier de commande/télécommande est de voir si celle-ci fonctionne correctement avec le clavier sur PC, puis testez-la avec un boîtier de commande/télécommande pour déterminer les points faibles dans votre interface utilisateur.

### <a name="hardware-buttons"></a>Boutons matériels

Tout au long de ce document, les boutons seront appelés par leurs noms fournis dans le schéma suivant.

![Schéma affichant les boutons d’un boîtier de commande et d’une télécommande](images/designing-for-tv/hardware-buttons-gamepad-remote.png)

Comme vous pouvez le constater sur le schéma, certains des boutons pris en charge sur le boîtier de commande ne le sont pas sur la télécommande, et vice versa. Bien que vous puissiez utiliser des boutons qui sont uniquement pris en charge sur un périphérique d’entrée (pour rendre plus rapide la navigation dans l’interface utilisateur), n’oubliez pas que leur utilisation pour des interactions critiques peut créer une situation dans laquelle l’utilisateur se trouve dans l’impossibilité d’interagir avec certaines parties de l’interface utilisateur.

Le tableau suivant répertorie tous les boutons matériels pris en charge par les applications UWP et les périphériques d’entrée qui les prennent en charge.

| Button                    | Boîtier de commande   | Télécommande    |
|---------------------------|-----------|-------------------|
| BoutonA/Sélectionner           | Oui       | Oui               |
| Bouton B/Précédent             | Oui       | Oui               |
| Bouton multidirectionnel   | Oui       | Oui               |
| Touche Menu               | Oui       | Oui               |
| Touche Affichage               | Oui       | Oui               |
| Boutons X et Y           | Oui       | Non                |
| Stick analogique gauche                | Oui       | Non                |
| Stick analogique droit               | Oui       | Non                |
| Gâchette gauche et droite   | Oui       | Non                |
| Gâchettes hautes gauche et droite    | Oui       | Non                |
| BoutonOneGuide           | Non        | Oui               |
| Bouton de volume             | Non        | Oui               |
| Bouton de changement de chaîne            | Non        | Oui               |
| Boutons de contrôle multimédia     | Non        | Oui               |
| Bouton Muet               | Non        | Oui               |

### <a name="built-in-button-support"></a>Prise en charge des boutons intégrés

La plateforme UWP mappe automatiquement le comportement d’entrée du clavier existant sur les entrées du boîtier de commande et de la télécommande. Le tableau suivant répertorie ces mappages intégrés.

| Clavier              | Boîtier de commande/Télécommande                        |
|-----------------------|---------------------------------------|
| Touches de direction            | Bouton multidirectionnel (également stick analogique gauche sur le boîtier de commande)    |
| Espace              | BoutonA/Sélectionner                       |
| Entrée                 | BoutonA/Sélectionner                       |
| Échap                | Bouton B/Précédent*                        |

\*Lorsque les événements [KeyDown](https://msdn.microsoft.com/library/windows/apps/br208941.aspx) et [KeyUp](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.keyup.aspx) pour le boutonB ne sont pas gérés par l’application, l’événement [SystemNavigationManager.BackRequested](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.systemnavigationmanager.backrequested.aspx) est déclenché, ce qui doit se traduire par une navigation vers l’arrière au sein de l’application. Cependant, vous devez implémenter cela vous-même, comme dans l’extrait de code suivant:

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
> Si le boutonB est utilisé pour revenir en arrière, n’affichez pas de bouton précédent dans l’interface utilisateur. Si vous utilisez un [affichage de navigation](../controls-and-patterns/navigationview.md), le bouton précédent est automatiquement masqué. Pour plus d’informations sur la navigation vers l’arrière, voir [Historique de navigation et navigation vers l’arrière pour les applications UWP](../basics/navigation-history-and-backwards-navigation.md).

Les applications UWP sur XboxOne permettent également d’appuyer sur le bouton **Menu** pour ouvrir les menus contextuels. Pour plus d’informations, voir [CommandBar et ContextFlyout](#commandbar-and-contextflyout).

### <a name="accelerator-support"></a>Prise en charge des boutons accélérateurs

Les boutons accélérateurs permettant d’accélérer la navigation dans une interface utilisateur. Cependant, ces boutons peuvent être propres à certains périphériques d’entrée; certains utilisateurs ne seront donc pas en mesure d’utiliser ces fonctions. En réalité, le boîtier de commande est le seul périphérique d’entrée qui prend en charge les fonctions d’accélération pour les applicationsUWP sur XboxOne.

Le tableau suivant répertorie la prise en charge intégrée des accélérateurs dans l’UWP, en plus de ce que vous pouvez implémenter vous-même. Intégrez ces comportements à votre interface utilisateur personnalisée afin de proposer une expérience utilisateur cohérente et conviviale.

| Interaction   | Clavier/Souris   | Boîtier de commande      | Intégrée pour:  | Recommandée pour: |
|---------------|------------|--------------|----------------|------------------|
| Page vers le haut/bas  | Page vers le haut/bas | Gâchette gauche/droite | [CalendarView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.calendarview.aspx), [ListBox](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listbox.aspx), [ListViewBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.aspx), [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx), `ScrollViewer`, [Selector](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.aspx), [LoopingSelector](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.loopingselector.aspx), [ComboBox](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.combobox.aspx), [FlipView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flipview.aspx) | Affichages qui prennent en charge le défilement vertical
| Page vers la gauche/droite | Aucun | Gâchettes hautes gauche/droite | [Pivot](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.pivot.aspx), [ListBox](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listbox.aspx), [ListViewBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.aspx), [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx), `ScrollViewer`, [Selector](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.aspx), [LoopingSelector](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.loopingselector.aspx), [FlipView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flipview.aspx) | Affichages qui prennent en charge le défilement horizontal
| Zoom avant/arrière        | Ctrl+/- | Gâchette gauche/droite | Aucun | `ScrollViewer`, les affichages qui prennent en charge le zoom avant et arrière |
| Ouvrir/fermer le volet de navigation | Aucun | Affichage | Aucun | Volets de navigation |
| [Rechercher](#search-experience) | Aucun | Bouton Y | Aucun | Raccourci pour la fonction de recherche principale dans l’application |
| [Ouvrir le menu contextuel](#commandbar-and-contextflyout) | Clic droit | Touche Menu | [ContextFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout) | Menus contextuels |

## <a name="xy-focus-navigation-and-interaction"></a>Interaction et navigation en mode focusXY

Si votre application prend en charge une navigation en mode focus appropriée pour le clavier, cela conviendra également pour le boîtier de commande et la télécommande.
La navigation avec les touches de direction est mappée au **bouton multidirectionnel** (ainsi qu’au **stick analogique gauche** sur le boîtier de commande), et l’interaction avec les éléments d’interface utilisateur est mappée à la touche **Entrée/Sélectionner** (voir [Boîtier de commande et télécommande](#gamepad-and-remote-control)).

De nombreux événements et propriétés sont utilisés par le boîtier de commande et le clavier&mdash;les deux déclenchent des événements `KeyDown` et `KeyUp`, et accéderont uniquement à des contrôles qui sont dotés des propriétés `IsTabStop="True"` et `Visibility="Visible"`. Pour des recommandations en matière de conception de clavier, voir [Interactions avec le clavier](../input/keyboard-interactions.md).

Si la prise en charge du clavier est correctement implémentée, votre application fonctionnera assez bien; toutefois, du travail supplémentaire peut être nécessaire pour la prise en charge de chaque scénario. Réfléchissez aux besoins spécifiques de votre application pour proposer une expérience utilisateur optimale.

> [!IMPORTANT]
> Le mode souris est activé par défaut pour les applications UWP qui s’exécutent sur XboxOne. Pour désactiver le mode souris et activer la navigation en mode focus XY, définissez `Application.RequiresPointerMode=WhenRequested`.

### <a name="debugging-focus-issues"></a>Débogage des problèmes de focus

La méthode [FocusManager.GetFocusedElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.focusmanager.getfocusedelement.aspx) vous indique sur quel élément le focus se trouve actuellement. Cela s’avère utile dans des situations où l’emplacement du focus visuel n’est pas clairement identifiable. Vous pouvez consigner les informations dans la fenêtre Sortie de VisualStudio:

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

La navigation en mode XY peut ne pas fonctionner comme prévu pour trois raisons courantes:

* La valeur de la propriété [IsTabStop](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.istabstop.aspx) ou [Visibility](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.visibility.aspx) est incorrecte.
* Le contrôle sur lequel se trouve le focus est en réalité plus large que prévu&mdash;La navigation en mode XY examine la taille totale du contrôle ([ActualWidth](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.actualwidth.aspx) et [ActualHeight](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.actualheight.aspx)), et pas seulement la partie du contrôle effectuant le rendu d’un élément important.
* Un contrôle pouvant être actif se trouve au-dessus d’un autre&mdash;La navigation en mode XY ne prend pas en charge les contrôles qui se chevauchent.

Si la navigation en mode XY ne fonctionne toujours pas après correction de ces problèmes courants, vous pouvez pointer manuellement vers l’élément sur lequel vous voulez un focus à l’aide de la méthode décrite dans [Remplacement de la navigation par défaut](#overriding-the-default-navigation).

Si la navigation en mode XY fonctionne comme prévu, mais qu’aucun focus visuel n’est affiché, l’un des problèmes suivants peut en être la cause:

* Vous avez remodélisé le contrôle sans inclure de focus visuel. Définissez `UseSystemFocusVisuals="True"` ou ajoutez un focus visuel manuellement.
* Vous avez déplacé le focus en appelant la méthode `Focus(FocusState.Pointer)`. Le paramètre [FocusState](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.focusstate.aspx) détermine ce qui se produit pour le focus visuel. En général, vous devez définir ce paramètre sur `FocusState.Programmatic`, ce qui maintient la visibilité du focus visuel si ce dernier était déjà visible, et le maintient masqué si c’était le cas précédemment.

Le reste de cette section décrit en détail les problèmes courants de conception lorsque la navigation en mode XY est utilisée, et propose plusieurs méthodes pour résoudre ces problèmes.

### <a name="inaccessible-ui"></a>Interface utilisateur inaccessible

Étant donné que la navigation en mode focusXY limite le déplacement vers le haut, le bas, à gauche et à droite, il existe des scénarios où certaines parties de l’interface utilisateur ne sont pas accessibles.
Le schéma suivant illustre un exemple du type de disposition d’interface utilisateur que la navigation en mode focusXY ne prend pas en charge.
Notez que l’élément au milieu n’est pas accessible à l’aide du boîtier de commande/télécommande dans la mesure où la navigation horizontale et la navigation verticale sont prioritaires; l’élément du milieu ne sera jamais d’une priorité suffisamment élevée pour avoir le focus.

![Éléments dans les quatre coins avec un élément inaccessible au milieu](images/designing-for-tv/2d-navigation-best-practices-ui-layout-to-avoid.png)

Si, pour une raison quelconque, il n’est pas possible de réorganiser l’interface utilisateur, utilisez l’une des techniques décrites dans la section suivante pour remplacer le comportement de focus par défaut.

### <a name="overriding-the-default-navigation"></a>Remplacement de la navigation par défaut

Si la plateforme Windows universelle tente de garantir la convivialité de la navigation par bouton multidirectionnel/stick analogique gauche, elle ne peut pas garantir un comportement optimisé pour les intentions de votre application.
Pour vous assurer que la navigation est optimisée pour votre application, la meilleure solution consiste à la tester avec une manette de jeu et à vérifier que tous les éléments d’interface utilisateur sont accessibles par l’utilisateur, d’une manière qui soit pertinente pour les scénarios de votre application. Si les scénarios de votre application nécessitent un comportement ne pouvant être obtenu par le biais de la navigation en mode focusXY, envisagez de suivre les conseils indiqués dans les sections suivantes et/ou de remplacer le comportement afin de placer le focus sur un élément logique.

L’extrait de code suivant montre comment remplacer le comportement de navigation en mode focusXY:

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

Pour empêcher le focus de se déplacer dans une certaine direction à partir d’un contrôle, utilisez la propriété `XYFocus*` pour pointer sur ce même contrôle:

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

Dans l’exemple ci-dessus, si le focus est sur l’objet `Button` Two et l’utilisateur navigue vers la droite, le meilleur candidat au focus est l’objet `Button` Four; cependant, le focus est déplacé vers l’objet `Button` Three, car le parent `UserControl` l’oblige à naviguer vers celui-ci lorsqu’il se trouve en dehors de son arborescence visuelle.

### <a name="path-of-least-clicks"></a>Chemin nécessitant le moins de clics

Permettez à l’utilisateur d’effectuer les tâches les plus courantes avec le moins de clics possible. Dans l’exemple suivant, la classe [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) est placée entre le bouton **Lecture** (qui a initialement le focus) et un élément couramment utilisé; un élément inutile est ainsi placé entre les tâches prioritaires.

![Meilleures pratiques en matière de navigation nécessitant le moins de clics](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks.png)

Dans l’exemple suivant, la classe [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) est placée au-dessus du bouton **Lecture** à la place.
Le simple fait de réorganiser l’interface utilisateur afin que les éléments inutiles ne soient pas placés entre les tâches prioritaires améliore considérablement la facilité d’utilisation de votre application.

![TextBlock déplacée au-dessus du bouton de lecture afin qu’elle ne soit plus entre les tâches prioritaires](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks-2.png)

### <a name="commandbar-and-contextflyout"></a>CommandBar et ContextFlyout

Lorsque vous utilisez une [CommandBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx), n’oubliez pas le problème de défilement dans une liste, comme mentionné dans [Problème: Éléments d’interface utilisateur localisés suite à un long défilement dans une liste/grille](#problem-ui-elements-located-after-long-scrolling-list-grid). L’image suivante illustre une disposition d’interface utilisateur avec la `CommandBar` en bas d’une liste/grille. L’utilisateur devrait faire défiler toute la liste/grille pour atteindre la `CommandBar`.

![CommandBar en bas de la liste/grille](images/designing-for-tv/2d-navigation-best-practices-commandbar-and-contextflyout.png)

Que se passe-t-il si vous placez le contrôle `CommandBar` *au-dessus* de la liste/grille? Bien qu’un utilisateur ayant effectué un défilement vers le bas de la liste/grille doive en refaire un vers le haut pour atteindre le contrôle `CommandBar`, cette navigation est un peu plus courte que la configuration précédente. On suppose ici que le focus initial de votre application est placé à côté ou au-dessus de la `CommandBar` ; cette approche ne fonctionne pas aussi bien si le focus initial se trouve sous la liste/grille. Si ces éléments `CommandBar` sont des éléments d’action globale auxquels l’utilisateur n’accède que rarement (tels qu’un bouton **Synchronisation**), le fait de les disposer au-dessus de la liste/grille reste acceptable.

Bien qu’il soit impossible d’empiler les éléments d’une `CommandBar` verticalement, le fait de les placer en face du défilement (par exemple, à gauche ou à droite d’une liste avec défilement vertical, ou en haut ou en bas d’une liste avec défilement horizontal) constitue une autre option que vous devrez peut-être prendre en compte si elle fonctionne bien pour la disposition de votre interface utilisateur.

Si votre application dispose d’une `CommandBar` dont les éléments doivent être facilement accessibles aux utilisateurs, vous devrez peut-être les placer à l’intérieur d’un [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) et les supprimer de la `CommandBar`. `ContextFlyout` est une propriété de [UIElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.aspx) et constitue le [menu contextuel](../controls-and-patterns/dialogs-and-flyouts/index.md) associé à cet élément. Sur PC, lorsque cliquez à l’aide du bouton droit sur un élément avec un `ContextFlyout`, ce menu contextuel apparaît. Sur XboxOne, cela se produit lorsque vous appuyez sur le bouton **Menu** tandis que le focus se trouve sur un tel élément.

### <a name="ui-layout-challenges"></a>Défis en matière de disposition de l’interface utilisateur

Certaines dispositions d’interface utilisateur posent plus de défis en raison de la navigation en mode focusXY et doivent être évaluées au cas par cas. Bien qu’il n’existe pas de méthode «absolue» et que la solution que vous choisissez dépend des besoins spécifiques de votre application, certaines techniques permettent de créer une excellente expérienceTV.

Pour mieux comprendre ce point, examinons un exemple d’application qui illustre certains de ces problèmes et les techniques permettant de les résoudre.

> [!NOTE]
> Cet exemple d’application sert à illustrer des problèmes d’interface utilisateur et leurs solutions potentielles, et ne vise pas à présenter la meilleure expérience utilisateur possible pour votre application.

Ce qui suit est un exemple d’application pour le secteur immobilier qui affiche une liste des maisons disponibles à la vente, une carte, la description des propriétés, ainsi que d’autres informations. Cette application pose trois défis que vous pouvez surmonter à l’aide des techniques suivantes:

- [Réorganisation de l’interface utilisateur](#ui-rearrange)
- [Activation du focus](#engagement)
- [Mode souris](#mouse-mode)

![Exemple d’application pour le secteur immobilier](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app.png)

#### Problème: Éléments d’interface utilisateur situés après une liste/grille à long défilement <a name="problem-ui-elements-located-after-long-scrolling-list-grid"></a>

La [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) des propriétés visibles dans l’image suivante est une très longue liste à faire défiler. Si l’[activation](#focus-engagement) n’est *pas* requise pour la `ListView`, le focus se place sur le premier élément de la liste lorsque l’utilisateur navigue vers cette dernière. L’utilisateur doit parcourir tous les éléments de la liste pour atteindre le bouton **Précédent** ou **Suivant**. Dans ces cas peu pratiques où l’utilisateur doit parcourir toute la liste &mdash;c’est-à-dire, lorsque la liste est trop longue pour que cette expérience soit acceptable&mdash;, vous devez envisager d’autres options.

![Application pour le secteur immobilier: une liste comprenant 50éléments nécessite 51clics pour atteindre les boutons du bas.](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app-list.png)

#### <a name="solutions"></a>Solutions

**Réorganisation de l’interface utilisateur <a name="ui-rearrange"></a>**

À moins que le focus initial ne soit placé au bas de la page, les éléments d’interface utilisateur placés au-dessus d’une liste à long défilement sont en général plus facilement accessibles que s’ils étaient placés au-dessous.
Si cette nouvelle disposition fonctionne pour d’autres appareils, il serait moins coûteux en ressources de modifier la disposition pour toutes les familles d’appareils au lieu d’apporter des modifications d’interface utilisateur spécifiques pour XboxOne.
En outre, le fait de placer des éléments d’interface utilisateur à l’opposé du sens de défilement (autrement dit, horizontalement pour une liste à défilement vertical ou verticalement pour une liste à défilement horizontal) améliorera davantage l’accessibilité.

![Application pour le secteur immobilier: Placement des boutons au-dessus d’une liste à long défilement](images/designing-for-tv/2d-focus-navigation-and-interaction-ui-rearrange.png)

**Activation du focus <a name="engagement"></a>**

Lorsqu’une activation est *requise*, la totalité de la `ListView` devient une cible du focus unique. L’utilisateur sera en mesure d’ignorer le contenu de la liste afin d’accéder au prochain élément pouvant être actif. Pour en savoir plus sur les contrôles prenant en charge l’activation et sur leur utilisation, consultez la section [Activation du focus](#focus-engagement).

![Application pour le secteur immobilier: définition de l’activation sur Requis pour atteindre les boutons Précédent/Suivant en un clic](images/designing-for-tv/2d-focus-navigation-and-interaction-engagement.png)

#### <a name="problem-scrollviewer-without-any-focusable-elements"></a>Problème: ScrollViewer sans élément pouvant être actif

Étant donné que la navigation en mode focus XY dépend de la navigation vers un seul élément d’interface utilisateur pouvant être actif à la fois, un [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) ne contenant aucun élément pouvant être actif (par exemple, contenant seulement du texte), peut provoquer un scénario dans lequel l’utilisateur n’est pas en mesure d’afficher l’ensemble du contenu dans le `ScrollViewer`.
Pour connaître les solutions de ce scénario et des scénarios connexes, voir [Activation du focus](#focus-engagement).

![Application pour le secteur immobilier: ScrollViewer contenant uniquement du texte](images/designing-for-tv/2d-focus-navigation-and-interaction-scrollviewer.png)

#### <a name="problem-free-scrolling-ui"></a>Problème: interface utilisateur à défilement libre

Si votre application nécessite une interface utilisateur à défilement libre, telle qu’une surface de dessin ou, dans l’exemple présent, une carte, la navigation en mode focusXY ne fonctionne simplement pas.
Dans ce cas, vous pouvez activer le [mode souris](#mouse-mode) pour permettre à l’utilisateur de naviguer librement à l’intérieur d’un élément d’interface utilisateur.

![Mapper un élément d’interface utilisateur à l’aide du mode souris](images/designing-for-tv/map-mouse-mode.png)

## <a name="mouse-mode"></a>Mode souris

Comme décrit dans [Interaction et navigation en mode focus XY](#xy-focus-navigation-and-interaction), le focus est déplacé à l’aide du système de navigation XY sur XboxOne, ce qui permet à l’utilisateur de déplacer le focus entre les contrôles en effectuant des déplacements vers le haut, le bas, la gauche et la droite.
Toutefois, certains contrôles comme [WebView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.webview.aspx) et [MapControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mapcontrol.aspx) nécessitent une interaction semblable à celle faite avec la souris, avec laquelle les utilisateurs peuvent déplacer le pointeur librement à l’intérieur des limites du contrôle.
Certaines applications permettent également de déplacer le pointeur sur la page entière; l’expérience boîtier de commande/télécommande est alors semblable à l’expérience souris sur PC.

Pour ces scénarios, demandez un pointeur (mode souris) pour la page entière, ou sur un contrôle à l’intérieur d’une page.
Par exemple, votre application peut contenir une page comportant un contrôle `WebView` qui utilise le mode souris uniquement à l’intérieur du contrôle tandis que la navigation en mode focus XY est utilisée partout ailleurs.
Pour demander un pointeur, vous pouvez spécifier si vous le voulez **lorsqu’un contrôle ou une page sont actifs** ou **lorsqu’une page a le focus**.

> [!NOTE]
> La demande de pointeur lorsqu’un contrôle a le focus n’est pas prise en charge.

Pour les applications XAML et les applications web hébergées qui s’exécutent sur XboxOne, le mode souris est activé par défaut pour l’ensemble de l’application. Il est vivement conseillé de désactiver cette fonctionnalité et d’optimiser votre application pour la navigation en modeXY. Pour ce faire, définissez la propriété `Application.RequiresPointerMode` sur `WhenRequested` afin d’activer le mode souris uniquement lorsqu’un contrôle ou une page le demandent.

Pour ce faire, dans une application XAML, utilisez le code suivant dans votre classe `App`:

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
> Le mode souris est uniquement pris en charge sur XboxOne avec boîtier de commande/télécommande. Il est ignoré sans avertissement dans d’autres familles d’appareils et types d’entrée.

Utilisez la propriété [RequiresPointer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.requirespointer) dans un contrôle ou une page pour activer le mode souris sur ceux-ci. Cette propriété a trois valeurs possibles: `Never` (valeur par défaut), `WhenEngaged` et `WhenFocused`.

### <a name="activating-mouse-mode-on-a-control"></a>Activation du mode souris sur un contrôle

Lorsque l’utilisateur active un contrôle avec `RequiresPointer="WhenEngaged"`, le mode souris est activé sur le contrôle en question jusqu’à ce que l’utilisateur le désactive. L’extrait de code suivant montre un `MapControl` simple qui, lorsqu’il est activé, active le mode souris:

```xml
<Page>
    <Grid>
        <MapControl IsEngagementRequired="true"
                    RequiresPointer="WhenEngaged"/>
    </Grid>
</Page>
```

> [!NOTE]
> Si un contrôle active le mode souris lorsqu’il est employé, il doit également être employé avec `IsEngagementRequired="true"`; sinon, le mode souris n’est pas activé.

Lorsqu’un contrôle est en mode souris, ses contrôles imbriqués le sont également. Le mode demandé de ses enfants est ignoré &mdash; un parent ne peut pas être en mode souris si ses enfants ne le sont pas également.

En outre, le mode demandé d’un contrôle est examiné uniquement lorsqu’il a le focus; le mode n’est pas changé dynamiquement tant qu’il a le focus.

### <a name="activating-mouse-mode-on-a-page"></a>Activation du mode souris dans une page

Lorsqu’une page dispose de la propriété `RequiresPointer="WhenFocused"`, le mode souris est activé pour la page entière lorsqu’elle a le focus. L’extrait de code suivant illustre l’obtention de cette propriété par une page:

```xml
<Page RequiresPointer="WhenFocused">
    ...
</Page>
```

> [!NOTE]
> La valeur `WhenFocused` est uniquement prise en charge dans les objets [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx). Une exception est levée si vous tentez de définir cette valeur sur un contrôle.

### <a name="disabling-mouse-mode-for-full-screen-content"></a>Désactivation du mode souris pour le contenu en plein écran

Lors de l’affichage de vidéos ou d’autres types de contenu en plein écran, il est généralement préférable de masquer le curseur, car celui-ci peut distraire l’utilisateur. Ce scénario se produit lorsque le reste de l’application utilise le mode souris, mais que vous souhaitez désactiver ce mode lors de l’affichage de contenu en plein écran. Pour ce faire, placez le contenu en plein écran dans son propre objet `Page`, puis suivez les étapes ci-dessous.

1. Dans l’objet `App`, définissez `RequiresPointerMode="WhenRequested"`.
2. Dans chaque objet `Page`, *excepté* l’objet `Page` en plein écran, définissez `RequiresPointer="WhenFocused"`.
3. Pour l’objet `Page` en plein écran, définissez `RequiresPointer="Never"`.

De cette façon, le curseur n’apparaîtra jamais lors de l’affichage de contenu plein écran.

## <a name="focus-visual"></a>Visuel du focus

Le visuel du focus est le contour de l’élément de l’interface utilisateur sur lequel se trouve actuellement le focus. Cela permet de guider l’utilisateur pour une navigation facile dans votre interface utilisateur et sans se perdre.

Avec l’ajout d’une mise à jour visuelle et de nombreuses options de personnalisation au visuel du focus, les développeurs peuvent être certains qu’un seul visuel du focus fonctionne correctement sur les PC et XboxOne, ainsi que sur tous les appareils Windows10 prenant en charge le clavier et/ou le boîtier de commande/télécommande.

Bien que le même visuel du focus puisse être utilisé sur différentes plateformes, le contexte diffère légèrement pour l’expérience «10-foot». Vous devez supposer que l’utilisateur ne se concentre pas sur l’ensemble de l’écran de TV. C’est pourquoi il est important que l’élément ayant actuellement le focus s’affiche toujours clairement à l’utilisateur; cela évite la frustration de devoir rechercher les éléments visuels.

Il est également important de garder à l’esprit que le visuel du focus s’affiche par défaut lorsqu’un boîtier de commande ou une télécommande sont utilisés, mais *pas* un clavier. Il s’affiche donc lorsque vous exécutez votre application sur XboxOne, même si vous ne l’avez pas implémenté.

### <a name="initial-focus-visual-placement"></a>Placement initial du visuel du focus

Lors du lancement d’une application ou de la navigation vers une page, placez le focus sur un élément d’interface utilisateur approprié (le premier élément auquel l’utilisateur applique une action). Par exemple, une application de photos peut placer le focus sur le premier élément dans la galerie de photos, et une application de musique, dans laquelle l’utilisateur a ouvert une vue détaillée d’un morceau, peut placer le focus sur le bouton Lecture pour faciliter la lecture de la musique.

Essayez de placer le focus initial dans la zone supérieure gauche de votre application (ou en haut à droite pour un flux de droite à gauche). La plupart des utilisateurs ont tendance à se concentrer sur cet angle en premier car c’est à cet endroit que commence généralement le flux de contenu d’une application.

### <a name="making-focus-clearly-visible"></a>Rendre le focus clairement visible

Un visuel du focus doit toujours être visible à l’écran pour que l’utilisateur puisse reprendre à l’endroit où il s’est arrêté sans avoir à chercher le focus. De même, des éléments pouvant être actifs doivent toujours se trouver à l’écran&mdash;Par exemple, n’utilisez pas de fenêtres contextuelles contenant uniquement du texte sans éléments pouvant être actifs.

Les expériences en plein écran, telles que la lecture de vidéos ou le visionnage de photos, pourraient constituer une exception à cette règle. Dans ces cas, l’affichage du focus visuel n’est pas approprié.

### <a name="reveal-focus"></a>Effet Révéler focus

Révéler focus est un effet visuel qui anime la bordure des éléments susceptibles d’être activés, comme un bouton, lorsque l’utilisateur déplace le focus du clavier ou du boîtier de commande sur ces derniers. En ajoutant un éclat animé autour de la bordure des éléments actifs, Révéler focus permet aux utilisateurs de mieux comprendre où le focus se trouve et où il va.

Par défaut, Révéler focus est désactivé. Pour des expériences «10-foot», vous devez accepter de faire apparaître le focus en définissant [Application.FocusVisualKind propriété](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind) dans le constructeur de votre application.

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

Pour plus d’informations, voir les recommandations fournies pour [Révéler focus](/windows/uwp/design/style/reveal-focus).

### <a name="customizing-the-focus-visual"></a>Personnaliser le focus visuel

Si vous souhaitez personnaliser le focus visuel, vous pouvez le faire en modifiant les propriétés relatives au focus visuel pour chaque contrôle. Vous pouvez tirer parti de plusieurs de ces propriétés afin de personnaliser votre application.

Vous pouvez même désactiver le focus visuel fourni par le système en dessinant votre propre focus visuel à l’aide des états visuels. Pour en savoir plus, voir [VisualState](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstate.Aspx).

### <a name="light-dismiss-overlay"></a>Superposition de l’abandon interactif

Pour attirer l’attention de l’utilisateur sur les éléments d’interface utilisateur que ce dernier manipule avec le contrôleur de jeu ou la télécommande, l’UWP ajoute automatiquement une couche «de fumée» qui couvre les zones en dehors de l’interface utilisateur contextuelle lorsque l’application s’exécute sur XboxOne. Cela ne nécessite aucun travail supplémentaire, mais est un élément à prendre en compte lors de la conception de votre interface utilisateur. Vous pouvez définir la propriété `LightDismissOverlayMode` sur un `FlyoutBase` quelconque pour activer ou désactiver la couche de fumée; la valeur par défaut est `Auto`, ce qui signifie qu’elle est activée sur Xbox et désactivée ailleurs. Pour plus d’informations, voir [Boîte de dialogue modale et abandon interactif](../controls-and-patterns/menus.md).

## <a name="focus-engagement"></a>Activation du focus

L’activation du focus est conçue pour faciliter l’utilisation d’une manette de jeu ou d’une télécommande pour interagir avec une application.

> [!NOTE]
> La définition de l’activation du focus n’affecte pas le clavier ou les autres périphériques d’entrée.

Lorsque la propriété `IsFocusEngagementEnabled` d’un objet [FrameworkElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.aspx) est définie sur `True`, elle signale le contrôle comme nécessitant l’activation du focus. Cela signifie que l’utilisateur doit appuyer sur le bouton **A/Sélectionner** pour activer le contrôle et interagir avec ce dernier. Lorsqu’il a terminé, l’utilisateur peut appuyer sur le bouton **B/Précédent** pour désactiver le contrôle et naviguer hors de ce dernier.

> [!NOTE]
> `IsFocusEngagementEnabled` est une nouvelle API qui n’a pas encore été documentée.

### <a name="focus-trapping"></a>Interruption du focus

L’interruption du focus survient lorsqu’un utilisateur tente d’accéder à l’interface utilisateur d’une application, mais la navigation est «interrompue» au sein d’un contrôle, rendant difficile, voire impossible, le déplacement en dehors de ce contrôle.

L’exemple suivant montre une interface utilisateur provoquant l’interruption du focus.

![Boutons à gauche et à droite d’un curseur horizontal](images/designing-for-tv/focus-engagement-focus-trapping.png)

Si l’utilisateur veut accéder au bouton droit à partir du bouton gauche, il serait logique de supposer que la seule action requise serait d’appuyer deux fois sur le bouton multidirectionnel/stick analogique gauche.
Toutefois, si le [Slider](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.slider.aspx) ne nécessite pas d’activation, le comportement suivant se produit : lorsque l’utilisateur appuie à droite pour la première fois, le focus est décalé vers le `Slider` ; lorsqu’il réappuie à droite, la poignée du `Slider` se déplace à droite. L’utilisateur continuerait de déplacer la poignée vers la droite sans toutefois réussir à atteindre le bouton.

Plusieurs approches existent pour contourner ce problème. L’une consiste à concevoir une disposition différente, comme dans l’exemple d’application pour le secteur immobilier dans [Interaction et navigation en mode focus XY](#xy-focus-navigation-and-interaction), où nous avons déplacé les boutons **Précédent** et **Suivant** au-dessus de `ListView`. L’empilement vertical (et non horizontal) des contrôles, comme le montre l’image suivante, peut résoudre le problème.

![Boutons au-dessus et en dessous d’un curseur horizontal](images/designing-for-tv/focus-engagement-focus-trapping-2.png)

À présent, l’utilisateur peut accéder à chaque contrôle en appuyant sur les boutons haut et bas sur le bouton multidirectionnel/stick analogique gauche. Lorsque le `Slider` a le focus, l’utilisateur peut appuyer sur le bouton gauche/droite pour déplacer la poignée du `Slider`, comme prévu.

Une autre approche pour résoudre ce problème consiste à demander une activation du `Slider`. Si vous définissez `IsFocusEngagementEnabled="True"`, cela se traduit par le comportement suivant.

![Nécessité d’activation du focus sur un curseur afin que l’utilisateur puisse accéder au bouton situé à droite](images/designing-for-tv/focus-engagement-slider.png)

Lorsque le `Slider` nécessite une activation du focus, l’utilisateur peut accéder au bouton situé à droite en appuyant simplement deux fois à droite sur le bouton multidirectionnel/stick analogique gauche. Cette solution est excellente, car elle ne nécessite aucun ajustement de l’interface utilisateur et produit le comportement attendu.

### <a name="items-controls"></a>Contrôles d’éléments

Outre le contrôle [Slider](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.slider.aspx), il existe d’autres contrôles que vous souhaiterez peut-être activer:

- [Listbox](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listbox.aspx)
- [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)
- [GridView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx)
- [FlipView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.flipview)

Contrairement au contrôle `Slider`, ces contrôles n’interrompent pas le focus en leur sein ; cependant, ils peuvent poser des problèmes en matière de facilité d’utilisation s’ils contiennent de grandes quantités de données. Voici un exemple d’un contrôle `ListView` qui contient une grande quantité de données.

![ContrôleListView avec une grande quantité de données et des boutons au-dessus et en dessous](images/designing-for-tv/focus-engagement-list-and-grid-controls.png)

Comme pour l’exemple `Slider`, nous allons essayer de naviguer à partir du bouton du haut vers le bouton du bas avec un boîtier de commande/une télécommande.
En partant du bouton du haut, où se trouve le focus, le fait d’appuyer sur la flèche Bas sur le bouton multidirectionnel/stick analogique gauche déplace le focus sur le premier élément dans le contrôle `ListView` (« Item 1 »).
Lorsque l’utilisateur appuie à nouveau, le focus est déplacé sur l’élément suivant dans la liste, et non sur le bouton situé en bas.
Pour accéder au bouton du bas, l’utilisateur doit d’abord parcourir chaque élément du contrôle `ListView`.
Si le contrôle `ListView` contient une grande quantité de données, cela peut s’avérer peu pratique et dégrader l’expérience utilisateur.

Pour résoudre ce problème, définissez la propriété `IsFocusEngagementEnabled="True"` sur `ListView` pour demander son activation.
Cela permet à l’utilisateur de traverser rapidement le contrôle `ListView` en appuyant simplement sur le bouton Bas. Néanmoins, l’utilisateur n’est pas en mesure de parcourir la liste ou de sélectionner un élément dans celle-ci, sauf s’il l’active en appuyant sur le bouton **A/Sélectionner** lorsqu’il a le focus, puis en appuyant sur le bouton **B/Précédent** pour le désactiver.

![Contrôle ListView avec activation requise](images/designing-for-tv/focus-engagement-list-and-grid-controls-2.png)

#### <a name="scrollviewer"></a>ScrollViewer

Le contrôle [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx), qui possède des caractéristiques particulières, diffère légèrement des contrôles précédents. Si vous disposez d’un `ScrollViewer` avec du contenu pouvant être actif, par défaut, la navigation vers `ScrollViewer` vous permet de parcourir ses éléments pouvant être actifs. Comme pour un contrôle `ListView`, vous devez parcourir chaque élément pour naviguer en dehors du contrôle `ScrollViewer`.

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
| ListView              | Désactivé                       |
| ScrollViewer          | Désactivé                       |
| SemanticZoom          | Désactivé                       |
| Slider                | Activé                        |

Aucun autre contrôle UWP n’a de modification comportementale ou visuelle lorsque `IsFocusEngagementEnabled="True"`.

## <a name="ui-element-sizing"></a>Redimensionnement des éléments de l’interface utilisateur

Étant donné que l’utilisateur d’une application dans un environnement de 3mètres utilise un boîtier de commande ou une télécommande et se trouve à plusieurs mètres de l’écran, vous devez incorporer à votre conception certains éléments liés à l’interface utilisateur.
Assurez-vous que le contenu sur l’interface utilisateur présente la densité appropriée et que l’interface n’est pas trop encombrée, afin que l’utilisateur puisse la parcourir et sélectionner des éléments en toute simplicité. N’oubliez pas que la simplicité est le maître mot.

### <a name="scale-factor-and-adaptive-layout"></a>Facteur d’échelle et disposition adaptative

Le **facteur d’échelle** permet de s’assurer que le dimensionnement des éléments de l’interface utilisateur est approprié pour l’appareil sur lequel s’exécute l’application.
Sur le Bureau, ce paramètre se trouve dans **Paramètres &gt; Système &gt; Affichage** sous forme de curseur.
Ce paramètre existe également sur les appareils mobiles prenant en charge ce paramètre.

![Modifier la taille du texte, des applications et des autres éléments](images/designing-for-tv/ui-scaling.png)

Sur XboxOne, aucun paramètre système ne permet d’effectuer ces opérations. Cependant, pour que les éléments d’interface utilisateur UWP aient une taille appropriée pour la TV, ils sont dimensionnés par défaut à **200%** pour les applications XAML et à **150%** pour les applications HTML.
Tant que les éléments de l’interface utilisateur ont une taille appropriée pour les autres appareils, ce sera également le cas pour la TV.
XboxOne effectue le rendu de votre application à 1080p (1920x1080pixels). Par conséquent, lorsque vous intégrez une application à partir d’autres appareils, comme un PC, assurez-vous de l’aspect correct de l’interface utilisateur à 960x540pixels à une échelle de 100% (ou à 1280x720pixels à une échelle de 100% pour les applications HTML) à l’aide de [techniques adaptatives](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

La conception pour Xbox diffère quelque peu de la conception pour PC, car une seule résolution est concernée: 1920x1080.
Peu importe si l’utilisateur possède une télévision dont la résolution est supérieure&mdash; les applications UWP sont toujours mises à l’échelle à 1080p.

Les tailles de ressource appropriées sont également extraites pour votre application à partir de l’échelle200% (ou 150% pour les applications HTML) lorsque l’application est exécutée sur XboxOne, quelle que soit la résolution de la télévision.

### <a name="content-density"></a>Densité de contenu

Lorsque vous concevez votre application, n’oubliez pas que l’utilisateur se trouve relativement loin de l’interface utilisateur et interagit avec cette dernière à l’aide d’un contrôleur de jeu ou d’une télécommande, qui rend la navigation plus chronophage que s’il utilisait une souris ou une entrée tactile.

#### <a name="sizes-of-ui-controls"></a>Tailles des contrôles d’interface utilisateur

La taille des éléments interactifs de l’interface utilisateur doit être d’au moins 32pixels (pixels effectifs). Ceci est la valeur par défaut pour les contrôlesUWP courants. Lorsqu’elle est utilisée à une échelle de 200%, elle permet d’assurer la visibilité à distance des éléments de l’interface utilisateur et de réduire la densité du contenu.

![BoutonUWP à une échelle de 100% et 200%](images/designing-for-tv/button-100-200.png)

#### <a name="number-of-clicks"></a>Nombre de clics.

Lorsque l’utilisateur navigue d’un bord de l’écran de télévision à l’autre, la simplification de votre interface utilisateur doit se faire en **sixclics** maximum. Là encore s’applique le principe de la **simplicité**. Pour en savoir plus, voir [Chemin nécessitant le moins de clics](#path-of-least-clicks).

![6 icônes pour traverser](images/designing-for-tv/six-clicks.png)

### <a name="text-sizes"></a>Tailles de texte

Pour rendre votre interface utilisateur visible à distance, appuyez-vous sur les règles suivantes:

* Texte principal et contenu de lecture: 15epx au minimum
* Texte non critique et contenu supplémentaire: 12epx au minimum

Lorsque vous utilisez du texte supérieur à la normale dans votre interface utilisateur, choisissez une taille qui ne limite pas trop l’espace de l’écran (en occupant de l’espace que d’autres contenus pourraient remplir).

### <a name="opting-out-of-scale-factor"></a>Désactivation du facteur d’échelle

Nous recommandons que votre application tire parti de la prise en charge du facteur d’échelle, ce qui lui permettra de s’exécuter correctement sur tous les appareils, ayant été mise à l’échelle pour chaque type d’appareil.
Il reste cependant possible de désactiver ce comportement et de concevoir toutes vos interfaces utilisateur à une échelle de 100%. Notez que vous ne pouvez pas remplacer le facteur d’échelle par une valeur autre que 100%.

Dans les applications XAML, vous pouvez annuler le facteur d’échelle en utilisant l’extrait de code suivant:

```csharp
bool result =
    Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```

`result` vous indique l’opération d’annulation a abouti.

Pour plus d’informations et des exemples de code HTML/JavaScript, consultez [Comment désactiver la mise à l’échelle](../../xbox-apps/disable-scaling.md).

Veillez à calculer la taille appropriée des éléments d’interface utilisateur en doublant le nombre *effectif* de pixels *réels* mentionnés dans cette rubrique (ou en multipliant la valeur par 1,5 pour les applications HTML).

## <a name="tv-safe-area"></a>Zones adaptées à l’écran de TV

Le contenu n’occupe pas l’ensemble de l’écran de toutes les télévisions pour des raisons historiques, mais aussi technologiques. La plateformeUWP évite d’afficher du contenu d’interface utilisateur dans des zones inadaptées à l’écran de TV; elle dessine uniquement l’arrière-plan de la page.

La zone inadaptée à l’écran de TV est représentée par la zone bleue dans l’image suivante.

![Zone inadaptée à l’écran de TV](images/designing-for-tv/tv-unsafe-area.png)

Vous pouvez définir l’arrière-plan sur une couleur statique ou une couleur de thème, voire sur une image, comme le démontrent les extraits de code suivants.

### <a name="theme-color"></a>Couleur de thème

```xml
<Page x:Class="Sample.MainPage"
      Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"/>
```

### <a name="image"></a>Image

```xml
<Page x:Class="Sample.MainPage"
      Background="\Assets\Background.png"/>
```

Votre application ressemblera à ceci sans travail supplémentaire.

![Zones adaptées à l’écran de TV](images/designing-for-tv/tv-safe-area.png)

Ce n’est pas une solution optimale car elle crée un effet d’«encadré», donnant l’impression que des parties de l’interface utilisateur, telles que le volet et la grille de navigation, ont été tronquées. Vous pouvez cependant faire des optimisations pour étendre certaines parties de l’interface utilisateur à l’ensemble de l’écran. Cela donne à l’application un aspect plus cinématographique.

### <a name="drawing-ui-to-the-edge"></a>Étendre l’IU à l’ensemble de l’écran

Nous vous recommandons d’étendre certains éléments d’interface utilisateur à l’ensemble de l’écran pour apporter à l’utilisateur une véritable expérience d’immersion. Cela comprend les classes [ScrollViewers](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) et [CommandBars](../controls-and-patterns/navigationview.md) ainsi que les [volets de navigation](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx).

En revanche, il est important que le texte et les éléments interactifs ne soient jamais près des bords de l’écran; cela garantit qu’ils ne seront pas tronqués sur certaines télévisions. Nous recommandons d’étendre seulement les éléments visuels non essentiels à une distance de 5% des bords de l’écran. Comme mentionné dans [Redimensionnement des éléments de l’interface utilisateur](#ui-element-sizing), une application UWP suivant le facteur d’échelle par défaut de la console XboxOne (200%) utilise une zone de 960x540epx. Vous devez donc éviter de placer l’interface utilisateur primordiale de votre application dans les zones suivantes:

- 27epx à partir du haut et du bas
- 48epx des bords gauche et droit

Les sections suivantes décrivent comment étendre votre interface utilisateur aux bords de l’écran.

#### <a name="core-window-bounds"></a>Limites de fenêtre principale

Pour les applications UWP ciblant uniquement l’expérience «10-foot», les limites de fenêtre principale constituent une option plus simple.

Dans la méthode `OnLaunched` de `App.xaml.cs`, ajoutez le code suivant :

```csharp
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode
    (Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```

Avec cette ligne de code, la fenêtre d’application remplit l’écran. Vous devez donc déplacer toutes les UI interactives et essentielles dans la zone adaptée à l’écran de TV décrite précédemment. L’interface utilisateur temporaire, telle que les menus contextuels et les classes [ComboBox](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.combobox.aspx) ouvertes restent automatiquement à l’intérieur de la zone adaptée à l’écran de TV.

![Limites de fenêtre principale](images/designing-for-tv/core-window-bounds.png)

#### <a name="pane-backgrounds"></a>Arrière-plans de volet de navigation

Les volets de navigation sont généralement placés près du bord de l’écran. L’arrière-plan doit donc s’étendre dans la zone inadaptée à l’écran de TV pour ne pas qu’il y ait d’espaces vides. Vous pouvez le faire en modifiant simplement la couleur d’arrière-plan du volet de navigation pour la rendre identique à la couleur d’arrière-plan de l’application.

Les limites de fenêtre principale vous permettent d’étendre votre interface utilisateur aux bords de l’écran (comme décrit précédemment), mais vous devez ensuite utiliser des marges positives sur le contenu de [SplitView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.splitview.aspx) pour le conserver dans la zone adaptée à l’écran de TV.

![Volet de navigation étendu aux bords de l’écran](images/designing-for-tv/tv-safe-areas-2.png)

L’arrière-plan du volet de navigation a été étendu aux bords de l’écran, tandis que ses éléments de navigation sont conservés dans la zone adaptée à l’écran de TV.
Le contenu de `SplitView` (dans ce cas, une grille d’éléments) a été étendu vers le bas de l’écran pour ne pas avoir l’air tronqué. La partie supérieure de la grille reste toujours dans la zone adaptée à l’écran de TV. (Pour en savoir plus, voir [Extrémités de listes et de grilles défilantes](#scrolling-ends-of-lists-and-grids)).

L’extrait de code suivant permet de réaliser l’effet en question:

```xml
<SplitView x:Name="RootSplitView"
           Margin="48,0,48,0">
    <SplitView.Pane>
        <ListView x:Name="NavMenuList"
                  ContainerContentChanging="NavMenuItemContainerContentChanging"
                  ItemContainerStyle="{StaticResource NavMenuItemContainerStyle}"
                  ItemTemplate="{StaticResource NavMenuItemTemplate}"
                  ItemInvoked="NavMenuList_ItemInvoked"
                  ItemsSource="{Binding NavMenuListItems}"/>
    </SplitView.Pane>
    <Frame x:Name="frame"
           Navigating="OnNavigatingToPage"
           Navigated="OnNavigatedToPage"/>
</SplitView>
```

[CommandBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx) est un autre exemple de volet généralement positionné près d’un ou plusieurs bords de l’application. Son arrière-plan doit donc s’étendre aux bords des écrans de TV. Il contient généralement un bouton **Plus** (...) sur le côté droit qui doit rester dans la zone adaptée à l’écran de TV. Voici quelques stratégies différentes permettant d’obtenir les interactions et effets visuels souhaités.

**Option1**: modifiez la couleur d’arrière-plan de `CommandBar` pour la définir sur transparent ou sur la même couleur que l’arrière-plan de la page:

```xml
<CommandBar x:Name="topbar"
            Background="{ThemeResource SystemControlBackgroundAltHighBrush}">
            ...
</CommandBar>
```

`CommandBar` paraît ainsi avoir le même arrière-plan que le reste de la page; l’arrière-plan s’étend donc vers le bord de l’écran en toute fluidité.

**Option2**: ajoutez un rectangle en arrière-plan dont le remplissage est de la même couleur que l’arrière-plan de `CommandBar`, puis placez-le sous `CommandBar` et à travers le reste de la page:

```xml
<Rectangle VerticalAlignment="Top"
            HorizontalAlignment="Stretch"      
            Fill="{ThemeResource SystemControlBackgroundChromeMediumBrush}"/>
<CommandBar x:Name="topbar"
            VerticalAlignment="Top"
            HorizontalContentAlignment="Stretch">
            ...
</CommandBar>
```

> [!NOTE]
> Si vous optez pour cette approche, gardez à l’esprit que le bouton **Plus** modifie la hauteur de la classe `CommandBar` ouverte, si nécessaire, afin de montrer les étiquettes des `AppBarButton` sous leurs icônes. Nous vous recommandons de déplacer les étiquettes à *droite* de leurs icônes afin d’éviter ce redimensionnement. Pour plus d’informations, voir [Libellés CommandBar](#commandbar-labels).

Ces deux approches s’appliquent également aux autres types de contrôle répertoriés dans cette section.

#### <a name="scrolling-ends-of-lists-and-grids"></a>Extrémités de listes et de grilles défilantes

Il est courant que les listes et les grilles contiennent davantage d’éléments que l’écran puisse afficher simultanément. Lorsque c’est le cas, nous vous recommandons d’étendre la liste ou la grille au bord de l’écran. Les listes et les grilles à défilement horizontal doivent s’étendre vers le bord droit et celles à défilement vertical doivent s’étendre vers le bas.

![Grille contenue dans la zone adaptée à l’écran de TV tronquée](images/designing-for-tv/tv-safe-area-grid-cutoff.png)

Pendant qu’une liste ou une grille est étendue de la sorte, il est important de conserver le visuel du focus et son élément associé à l’intérieur de la zone adaptée à l’écran de TV.

![Le focus de la grille défilante doit être conservé à l’intérieur de la zone adaptée à l’écran de TV](images/designing-for-tv/scrolling-grid-focus.png)

La plateforme UWP comporte des fonctionnalités qui permettent de conserver le visuel du focus à l’intérieur des [VisibleBounds](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.applicationview.visiblebounds.aspx), mais vous devez ajouter du remplissage pour vous assurer que les éléments de liste/grille peuvent défiler à l’écran à l’intérieur de la zone adaptée à l’écran de TV. Plus précisément, vous ajoutez une marge positive à la classe [ItemsPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) des classes [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx) ou [GridView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemspresenter.aspx), comme l’illustre l’extrait de code suivant:

```xml
<Style x:Key="TitleSafeListViewStyle"
       TargetType="ListView">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="ListView">
                <Border BorderBrush="{TemplateBinding BorderBrush}"
                        Background="{TemplateBinding Background}"
                        BorderThickness="{TemplateBinding BorderThickness}">
                    <ScrollViewer x:Name="ScrollViewer"
                                  TabNavigation="{TemplateBinding TabNavigation}"
                                  HorizontalScrollMode="{TemplateBinding ScrollViewer.HorizontalScrollMode}"
                                  HorizontalScrollBarVisibility="{TemplateBinding ScrollViewer.HorizontalScrollBarVisibility}"
                                  IsHorizontalScrollChainingEnabled="{TemplateBinding ScrollViewer.IsHorizontalScrollChainingEnabled}"
                                  VerticalScrollMode="{TemplateBinding ScrollViewer.VerticalScrollMode}"
                                  VerticalScrollBarVisibility="{TemplateBinding ScrollViewer.VerticalScrollBarVisibility}"
                                  IsVerticalScrollChainingEnabled="{TemplateBinding ScrollViewer.IsVerticalScrollChainingEnabled}"
                                  IsHorizontalRailEnabled="{TemplateBinding ScrollViewer.IsHorizontalRailEnabled}"
                                  IsVerticalRailEnabled="{TemplateBinding ScrollViewer.IsVerticalRailEnabled}"
                                  ZoomMode="{TemplateBinding ScrollViewer.ZoomMode}"
                                  IsDeferredScrollingEnabled="{TemplateBinding ScrollViewer.IsDeferredScrollingEnabled}"
                                  BringIntoViewOnFocusChange="{TemplateBinding ScrollViewer.BringIntoViewOnFocusChange}"
                                  AutomationProperties.AccessibilityView="Raw">
                        <ItemsPresenter Header="{TemplateBinding Header}"
                                        HeaderTemplate="{TemplateBinding HeaderTemplate}"
                                        HeaderTransitions="{TemplateBinding HeaderTransitions}"
                                        Footer="{TemplateBinding Footer}"
                                        FooterTemplate="{TemplateBinding FooterTemplate}"
                                        FooterTransitions="{TemplateBinding FooterTransitions}"
                                        Padding="{TemplateBinding Padding}"
                                        Margin="0,27,0,27"/>
                    </ScrollViewer>
                </Border>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

Vous placez l’extrait de code précédent dans les ressources de la page ou de l’application, puis vous y accédez de la manière suivante:

```xml
<Page>
    <Grid>
        <ListView Style="{StaticResource TitleSafeListViewStyle}"
                  ... />
```

> [!NOTE]
> Cet extrait de code est spécifiquement conçu pour les contrôles `ListView`. Pour un style `GridView`, définissez l’attribut [TargetType](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.controltemplate.targettype.aspx) des éléments [ControlTemplate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.controltemplate.aspx) et [Style](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.style.aspx) sur `GridView`.

Pour un contrôle plus fin sur la procédure éléments sont affichés, si votre application cible la version 1803 ou version ultérieure, vous pouvez utiliser l' [événement UIElement.BringIntoViewRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested). Vous pouvez le placer sur l' [élément ItemsPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) pour le **contrôle ListView**/**GridView** pour intercepter avant que le **ScrollViewer** interne est le cas, comme dans les extraits de code suivants:

```xaml
<GridView x:Name="gridView">
    <GridView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid Orientation="Horizontal"
                           BringIntoViewRequested="ItemsWrapGrid_BringIntoViewRequested"/>
        </ItemsPanelTemplate>
    </GridView.ItemsPanel>
</GridView>
```

```cs
// The BringIntoViewRequested event is raised by the framework when items receive keyboard (or Narrator) focus or 
// someone triggers it with a call to UIElement.StartBringIntoView.
private void ItemsWrapGrid_BringIntoViewRequested(UIElement sender, BringIntoViewRequestedEventArgs args)
{
    if (args.VerticalAlignmentRatio != 0.5)  // Guard against our own request
    {
        args.Handled = true;
        // Swallow this request and restart it with a request to center the item.  We could instead have chosen
        // to adjust the TargetRect’s Y and Height values to add a specific amount of padding as it bubbles up, 
        // but if we just want to center it then this is easier.

        // (Optional) Account for sticky headers if they exist
        var headerOffset = 0.0;
        var itemsWrapGrid = sender as ItemsWrapGrid;
        if (gridView.IsGrouping && itemsWrapGrid.AreStickyGroupHeadersEnabled)
        {
            var header = gridView.GroupHeaderContainerFromItemContainer(args.TargetElement as GridViewItem);
            if (header != null)
            {
                headerOffset = ((FrameworkElement)header).ActualHeight;
            }
        }

        // Issue a new request
        args.TargetElement.StartBringIntoView(new BringIntoViewOptions()
        {
            AnimationDesired = true,
            VerticalAlignmentRatio = 0.5, // a normalized alignment position (0 for the top, 1 for the bottom)
            VerticalOffset = headerOffset, // applied after meeting the alignment ratio request
        });
    }
}
```

## <a name="colors"></a>Couleurs

Par défaut, la plateforme Windows universelle adapte les couleurs de votre application à la plage de couleurs adaptées aux écrans de TV (voir [Couleurs adaptées aux écrans de TV](#tv-safe-colors) pour plus d’informations) afin que votre application s’affiche correctement sur n’importe quel téléviseur. De plus, vous pouvez apporter des améliorations à la palette de couleurs de votre application pour optimiser l’expérience visuelle sur télévision.

### <a name="application-theme"></a>Thème d’application

Vous pouvez choisir un **thème d’application** (clair ou foncé) en fonction de ce qui est approprié pour votre application, ou vous pouvez ignorer le thème. Pour en savoir plus sur les recommandations générales pour les thèmes, voir [Thèmes de couleur](../style/color.md).

L’UWP permet également aux applications de définir le thème de manière dynamique selon les paramètres système fournis par les appareils sur lesquels elles s’exécutent.
Bien que l’UWP respecte toujours les paramètres de thème spécifiés par l’utilisateur, chaque appareil fournit également un thème par défaut approprié.
En raison de la nature de XboxOne, qui génère plus d’expériences *multimédias* que d’expériences de *productivité*, son thème de système est foncé par défaut.
Si le thème de votre application est basé sur les paramètres système, celui-ci devrait être foncé sur XboxOne par défaut.

### <a name="accent-color"></a>Couleur d’accentuation

La plateforme UWP fournit un moyen pratique pour afficher la **couleur d’accentuation** que l’utilisateur a sélectionnée dans ses paramètres système.

Sur XboxOne, l’utilisateur est en mesure de sélectionner une couleur utilisateur, comme il peut sélectionner une couleur d’accentuation sur un PC.
Dans la mesure où votre application appelle ces couleurs d’accentuation par le biais de pinceaux ou de ressources de couleur, la couleur sélectionnée par l’utilisateur dans les paramètres système sera utilisée. Notez que les couleurs d’accentuation sur XboxOne sont définies par l’utilisateur et non par le système.

Notez également que l’ensemble des couleurs utilisateur sur XboxOne n’est pas le même que sur les PC, téléphones et autres appareils.

Tant que votre application utilise une ressource de pinceau, telle que **SystemControlForegroundAccentBrush**, ou une ressource de couleur (**SystemAccentColor**), ou appelle les couleurs d’accentuation directement via l’API [UIColorType.Accent*](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx), ces couleurs sont remplacées par des couleurs d’accentuation disponibles sur XboxOne. Les couleurs de pinceau à contraste élevé sont également extraites à partir du système de la même manière que sur un PC et un téléphone.

Pour en savoir plus sur la couleur d’accentuation en général, voir [Couleur d’accentuation](../style/color.md#accent-color).

### <a name="color-variance-among-tvs"></a>Variantes de couleur entre les écrans de télévision

Lors de la conception d’applications pour la télévision, notez que les couleurs s’affichent de manière légèrement différente en fonction des types de télévision. Ne supposez pas que les couleurs ressembleront exactement à celles de votre écran. Si votre application repose sur des nuances de couleur subtiles pour différencier les diverses parties de l’interface utilisateur, les couleurs pourraient facilement se mélanger et ainsi dérouter les utilisateurs. Essayez d’utiliser des couleurs assez distinctes pour que les utilisateurs puissent bien les différentier, quelle que soit la télévision qu’ils utilisent.

### <a name="tv-safe-colors"></a>Couleurs adaptées aux écrans de TV

Les valeurs RVB d’une couleur représentent l’intensité du rouge, du vert et du bleu. Les écrans de télévision ne gèrent pas très bien les intensités extrêmes &mdash;ces couleurs peuvent produire un effet de «bandes» ou apparaître délavées sur certains écrans de télévision. En outre, les couleurs à haute intensité peuvent provoquer un effet Bloom (les pixels avoisinants émettent les mêmes couleurs). Bien que les avis divergent sur quelles couleurs sont adaptées aux écrans de TV, les couleurs dont les valeurs RVB sont comprises entre 16 et 235 (ou 10-EB en notation hexadécimale) sont généralement adaptées aux écrans de TV.

![Plage de couleurs adaptées aux écrans de TV](images/designing-for-tv/tv-safe-colors-2.png)

Historiquement, les applications sur Xbox devaient personnaliser leurs couleurs afin qu’elles soient comprises dans cette plage de couleurs «adaptées aux écrans de TV». Toutefois, depuis Fall Creators Update, XboxOne adapte automatiquement le contenu de la plage complète à la plage adaptée aux écrans de TV. Cela signifie que la plupart des développeurs d’applications n’ont plus à se préoccuper des couleurs adaptées aux écrans de TV.

> [!IMPORTANT]
> Cet effet d’adaptation des couleurs n’est pas appliqué au contenu vidéo qui se trouve déjà dans la plage de couleurs adaptées aux écrans de télévision lors de la lecture à l’aide de [Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197).

Si vous développez une application à l’aide de DirectX11 ou DirectX12, et créez votre propre chaîne d’échange pour effectuer le rendu de l’interface utilisateur ou de la vidéo, vous pouvez spécifier l’espace de couleur que vous utilisez en appelant [IDXGISwapChain3::SetColorSpace1](https://msdn.microsoft.com/library/windows/desktop/dn903676), qui indiquera au système s’il a besoin d’adapter les couleurs ou non.

## <a name="guidelines-for-ui-controls"></a>Recommandations en matière de contrôles d’interface utilisateur

Il existe plusieurs contrôles d’interface utilisateur qui fonctionnent correctement sur plusieurs appareils, mais pour lesquels certains éléments doivent être pris en compte s’ils sont utilisés sur un téléviseur. Découvrez certaines meilleures pratiques portant sur l’utilisation de ces contrôles lors de la conception pour l’expérience «10-foot».

### <a name="pivot-control"></a>Contrôle Pivot

Un contrôle [Pivot](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.pivot.aspx) permet une navigation rapide des affichages au sein d’une application en sélectionnant différents en-têtes ou onglets. Le contrôle souligne l’en-tête sur lequel se trouve le focus, ce qui rend plus visible l’en-tête sélectionné lorsque vous utilisez le boîtier de commande/la télécommande.

![Souligné par contrôle Pivot](images/designing-for-tv/pivot-underline.png)

Vous pouvez régler la propriété [Pivot.IsHeaderItemsCarouselEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.pivot.isheaderitemscarouselenabled.aspx) sur `true` pour que les pivots gardent toujours la même position, et éviter que l’en-tête de pivot sélectionné se place toujours en première position. Cette expérience est plus intéressante pour les grands écrans tels que les écrans de télévision, car le renvoi à la ligne des en-têtes peut gêner les utilisateurs. Si tous les en-têtes de pivot ne sont pas visibles à l’écran en même temps, une barre de défilement permet aux clients d’afficher les autres en-têtes. Toutefois, vous devez vous assurer qu’ils tiennent tous à l’écran pour offrir la meilleure expérience possible. Pour en savoir plus, consultez [Onglets et pivots](../controls-and-patterns/tabs-pivot.md).

### <a name="navigation-pane-a-namenavigation-pane"></a>Volet de navigation <a name="navigation-pane">

Un volet de navigation (également appelé *menu hamburger*) est un contrôle de navigation couramment utilisé dans les applications UWP. En règle générale, il s’agit d’un volet comportant plusieurs options dans un menu de style de liste qui dirigera l’utilisateur vers différentes pages. En général, ce volet démarre en mode réduit pour économiser l’espace; l’utilisateur peut l’ouvrir en cliquant sur un bouton.

Même si les volets de navigation sont très accessibles par souris et écran tactile, ce n’est pas le cas pour le boîtier de commande/la télécommande car l’utilisateur doit ouvrir le volet par le biais d’un bouton. Par conséquent, une bonne pratique consiste à rendre possible l’ouverture du panneau de navigation à l’aide de la touche **Affichage**, ainsi que son ouverture en naviguant tout à gauche de la page. Vous pouvez trouver un exemple de code montrant l’implémentation de ce modèle de conception dans le document [Navigation en mode focus programmé](../input/focus-navigation-programmatic.md#split-view-code-sample). L’accès aux contenus du volet est ainsi grandement facilité. Pour en savoir plus sur la façon dont les volets de navigation se comportent sur des écrans de tailles différentes et pour connaître les meilleures pratiques en matière de navigation pour le boîtier de commande/la télécommande, voir [Volets de navigation](../controls-and-patterns/navigationview.md).

### <a name="commandbar-labels"></a>Libellés CommandBar

Il est judicieux de placer les libellés à droite des icônes sur une classe [CommandBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx) afin de réduire sa hauteur et garantir la cohérence de cette dernière. Vous pouvez le faire en définissant la propriété [CommandBar.DefaultLabelPosition](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.defaultlabelposition.aspx) sur `CommandBarDefaultLabelPosition.Right`.

![CommandBar comportant des libellés à droite des icônes](images/designing-for-tv/commandbar.png)

La définition de cette propriété provoquera également l’affichage permanent des libellés, ce qui fonctionne bien pour l’expérience «10-foot», car elle réduit le nombre de clics requis pour l’utilisateur. C’est également un excellent modèle que les autres types d’appareil peuvent suivre.

### <a name="tooltip"></a>Tooltip

Le contrôle [Tooltip](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.tooltip.aspx) a été introduit pour fournir à l’utilisateur des informations supplémentaires dans l’interface utilisateur lorsqu’il pointe avec la souris ou maintient son doigt sur un élément. Pour le boîtier de commande et la télécommande, `Tooltip` s’affiche après un court instant lorsque l’élément obtient le focus, reste à l’écran pendant un court moment, puis disparaît. Ce comportement peut devenir gênant si trop de contrôles `Tooltip` sont utilisés. Essayez d’éviter `Tooltip` lors de la conception d’applications pour la télévision.

### <a name="button-styles"></a>Styles de bouton

Bien que les boutons UWP standard fonctionnent correctement sur les télévisions, certains styles visuels attirent mieux l’attention sur l’interface utilisateur. Vous devez prendre cela en compte pour l’ensemble des plateformes, en particulier pour l’expérience «10-foot»; elles bénéficient d’une communication claire sur l’emplacement du focus. Pour en savoir plus sur ces styles, voir [Boutons](../controls-and-patterns/buttons.md).

### <a name="nested-ui-elements"></a>Éléments d’interface utilisateur imbriquée

L’interface utilisateur imbriquée expose les éléments actionnables imbriqués inclus dans un élément d’interface utilisateur conteneur où l’élément imbriqué et l’élément conteneur peuvent prendre le focus indépendamment l’un de l’autre.

L’interface utilisateur imbriquée est parfaitement indiquée pour certains types d’entrée, mais pas toujours pour les manettes de jeu et les télécommandes, qui font appel à la navigation XY. Veillez à suivre les recommandations fournies dans cette rubrique pour vous assurer que votre interface utilisateur est optimisée pour l’environnement TV (visualisation à 3mètres) et que l’utilisateur peut facilement accéder à tous les éléments interactifs. Une solution courante consiste à placer des éléments de l’interface utilisateur imbriquée dans un `ContextFlyout` (voir [CommandBar et ContextFlyout](#commandbar-and-contextflyout)).

Pour plus d’informations sur l’interface utilisateur imbriquée, voir [Interface utilisateur imbriquée dans des éléments de liste](../controls-and-patterns/nested-ui.md).

### <a name="mediatransportcontrols"></a>MediaTransportControls

L’élément [MediaTransportControls](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediatransportcontrols.aspx) permet aux utilisateurs d’interagir avec leur média en fournissant une expérience de lecture par défaut grâce à laquelle ils peuvent lire le contenu, le mettre en pause, activer les sous-titres, etc. Ce contrôle est une propriété de l’objet [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.aspx) et prend en charge deux options de disposition: *sur une ligne* et *sur deux lignes*. Dans la disposition sur une ligne, le curseur et les boutons de lecture se trouvent tous sur une même ligne, le bouton lecture/pause étant situé à gauche du curseur. Dans la disposition sur deux lignes, le curseur occupe sa propre ligne, les boutons de lecture se trouvant sur une ligne distincte en dessous. Lors de la conception pour l’expérience «10-foot», la disposition sur deux lignes doit être utilisée, car elle assure une meilleure navigation avec une manette de jeu. Pour activer la disposition sur deux lignes, définissez `IsCompact="False"` pour l’élément `MediaTransportControls` dans la propriété [TransportControls](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.transportcontrols.aspx) de la `MediaPlayerElement`.

```xml
<MediaPlayerElement x:Name="mediaPlayerElement1"  
                    Source="Assets/video.mp4"
                    AreTransportControlsEnabled="True">
    <MediaPlayerElement.TransportControls>
        <MediaTransportControls IsCompact="False"/>
    </MediaPlayerElement.TransportControls>
</MediaPlayerElement>
```  

Pour en savoir plus sur l’ajout de médias à votre application, consultez l’article [Lecteur multimédia](../controls-and-patterns/media-playback.md).

> ![REMARQUE] L’objet `MediaPlayerElement` est uniquement disponible dans Windows10, version1607 et ultérieure. Si vous développez une application pour une version antérieure de Windows10, vous devez utiliser l’objet [MediaElement](https://msdn.microsoft.com/library/windows/apps/br242926) à la place. Les recommandations ci-dessus s’appliquent également à l’objet `MediaElement` et la propriété `TransportControls` est accessible de la même manière.

### <a name="search-experience"></a>Expérience de recherche

La recherche de contenu est l’une des fonctions les plus fréquemment exécutées dans le cadre d’une expérience «10-foot». Si votre application offre une fonction de recherche, l’utilisateur doit pouvoir y accéder rapidement en utilisant le bouton accélérateur **Y** sur le boîtier de commande.

La plupart des clients connaissent probablement cet accélérateur. Toutefois, si vous le souhaitez, vous pouvez ajouter un glyphe visuel **Y** dans l’interface utilisateur afin d’indiquer que le client peut utiliser le bouton pour accéder à la fonctionnalité de recherche. Si vous ajoutez cet indicateur, veillez à utiliser le symbole de la police **Segoe Xbox Symbol MDL2** (`&#xE3CC;` pour les applications XAML, `\E426` pour les applications HTML) pour fournir une expérience cohérente avec l’interpréteur de commandes Xbox et d’autres applications.

> [!NOTE]
> Comme la police **Segoe Xbox MDL2 Symbol** n’est disponible que sur Xbox, le symbole ne s’affiche pas correctement sur votre PC. Mais, il apparaîtra sur le téléviseur lorsque vous le déploierez sur Xbox.

Dans la mesure où le bouton **Y** n’est disponible que sur le boîtier de commande, veillez à fournir d’autres méthodes d’accès à la recherche, comme des boutons dans l’interface utilisateur. Sinon, certains clients risquent de ne pas pouvoir accéder à la fonctionnalité.

Dans l’expérience «10-foot», il est souvent plus facile pour les clients d’utiliser une fonction de recherche en mode plein écran, car l’espace disponible sur l’écran est limité. En mode plein écran ou écran partiel, il est préférable que le clavier visuel affiché soit déjà ouvert lorsque l’utilisateur sélectionne la fonction de recherche. Ainsi il peut immédiatement saisir les termes de sa recherche.

## <a name="custom-visual-state-trigger-for-xbox"></a>Déclencheur d’état visuel personnalisé pour Xbox

Pour personnaliser votre applicationUWP pour l’expérience «10-foot», nous vous recommandons d’effectuer des modifications de disposition lorsque l’application détecte son lancement sur une console Xbox. L’une des méthodes utilisées pour cette fin est le *déclencheur d’état visuel* personnalisé. Les déclencheurs d’état visuel sont particulièrement utiles lorsque vous souhaitez apporter des modifications dans **Blend pour Visual Studio**. L’extrait de code suivant montre comment créer un déclencheur d’état visuel pour Xbox:

```xml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState>
            <VisualState.StateTriggers>
                <triggers:DeviceFamilyTrigger DeviceFamily="Windows.Xbox"/>
            </VisualState.StateTriggers>
            <VisualState.Setters>
                <Setter Target="RootSplitView.OpenPaneLength"
                        Value="368"/>
                <Setter Target="RootSplitView.CompactPaneLength"
                        Value="96"/>
                <Setter Target="NavMenuList.Margin"
                        Value="0,75,0,27"/>
                <Setter Target="Frame.Margin"
                        Value="0,27,48,27"/>
                <Setter Target="NavMenuList.ItemContainerStyle"
                        Value="{StaticResource NavMenuItemContainerXboxStyle}"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

Pour créer le déclencheur, ajoutez la classe suivante à votre application. Il s’agit de la classe référencée par le codeXAML précédemment:

```csharp
class DeviceFamilyTrigger : StateTriggerBase
{
    private string _currentDeviceFamily, _queriedDeviceFamily;

    public string DeviceFamily
    {
        get
        {
            return _queriedDeviceFamily;
        }

        set
        {
            _queriedDeviceFamily = value;
            _currentDeviceFamily = AnalyticsInfo.VersionInfo.DeviceFamily;
            SetActive(_queriedDeviceFamily == _currentDeviceFamily);
        }
    }
}
```

Une fois que vous ajoutez votre déclencheur personnalisé, votre application effectuera automatiquement les modifications de disposition spécifiées dans votre code XAML à chaque fois qu’elle détecte son exécution sur une console XboxOne.

Vous pouvez également utiliser du code pour vérifier si votre application s’exécute sur Xbox, puis effectuer les ajustements appropriés. Vous pouvez utiliser la variable simple suivante pour vérifier si votre application s’exécute sur Xbox:

```csharp
bool IsTenFoot = (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily ==
                    "Windows.Xbox");
```

Après avoir effectué cette vérification, vous pouvez effectuer les réglages appropriés de votre interface utilisateur dans le bloc de code. Un exemple illustratif se trouve dans l’[exemple de couleur UWP](#uwp-color-sample).

## <a name="summary"></a>Résumé

La conception pour l’expérience «10-foot» implique de prendre en compte des points spéciaux qui la distinguent de la conception pour toute autre plateforme. Si vous pouvez certainement transférer directement votre application UWP vers XboxOne avec succès, cette application n’est pas nécessairement optimisée pour l’expérience «10-foot», ce qui peut générer de la frustration chez l’utilisateur. Suivez les recommandations contenues dans cet article pour vous assurer que votre application fonctionne aussi bien que possible sur TV.

## <a name="related-articles"></a>Articles connexes

- [Notions fondamentales sur les appareils pour les applications de plateforme Windows universelle (UWP)](index.md)
- [Interactions entre le boîtier de commande et la télécommande](../input/gamepad-and-remote-interactions.md)
- [Son dans les applications UWP](../style/sound.md)
