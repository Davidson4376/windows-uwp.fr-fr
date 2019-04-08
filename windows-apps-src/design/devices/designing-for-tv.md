---
Description: Concevez votre application pour une esthétique et un fonctionnement optimaux sur votre télévision.
title: Conception pour Xbox et télévision
ms.assetid: 780209cb-3e8a-4cf7-8f80-8b8f449580bf
label: Designing for Xbox and TV
template: detail.hbs
isNew: true
keywords: Xbox, télévision, expérience « 10-foot », boîtier de commande, télécommande, entrées, interactions
ms.date: 11/13/2018
ms.topic: article
pm-contact: chigy
design-contact: jeffarn
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 431b8912e43647bc2678aaab7efc9ec68b866d10
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616654"
---
# <a name="designing-for-xbox-and-tv"></a>Conception pour Xbox et télévision

Concevez votre application de plateforme Windows universelle (UWP) pour une esthétique et un fonctionnement optimaux sur les écrans de télévision et Xbox One.

Consultez [Gamepad et contrôle à distance des interactions](../input/gamepad-and-remote-interactions.md) pour obtenir des conseils sur l’interaction des expériences dans les applications UWP dans le *10-fièvre* rencontrer.

## <a name="overview"></a>Vue d’ensemble

La plateforme Windows universelle vous permet de créer des expériences agréables sur plusieurs types d’appareils Windows 10.
La plupart des fonctionnalités fournies par l’infrastructure UWP permettent aux applications d’utiliser la même interface utilisateur sur ces appareils, sans travail supplémentaire.
Toutefois, des éléments particuliers sont à prendre en compte afin d’optimiser votre application et l’adapter pour un fonctionnement parfait sur les écrans de télévision et Xbox One.

L’expérience qui consiste à se trouver assis sur son fauteuil en face de la télévision et à interagir avec celle-ci à l’aide d’un boîtier de commande ou d’une télécommande est appelée le « **10-foot experience** »
Ce nom vient du fait que l’utilisateur se trouve généralement à 3 mètres (10 pieds) de l’écran.
Cela soulève des défis propres à cette expérience, qui ne sont pas présents dans l’expérience « *2-foot experience* » ou lors d’interactions avec un PC.
Si vous développez une application pour Xbox One ou tout autre appareil dont la sortie et l’entrée se font respectivement sur télévision et par boîtier de commande, vous devez toujours garder ceci à l’esprit.

Toutes les étapes décrites dans cet article ne sont pas nécessaires pour que votre application fonctionne pour les expériences « 10-foot experience », mais le fait de les comprendre et de prendre les décisions appropriées pour votre application créera une meilleure « 10-foot experience », mieux adaptée aux besoins spécifiques de votre application.
Lorsque vous concevez une application pour un environnement de 3 mètres, prenez en compte les principes de conception suivants.

### <a name="simple"></a>Simple

La conception pour un environnement de 3 mètres présente des défis uniques. La résolution et la distance d’affichage peuvent rendre difficile pour les utilisateurs l’assimilation d’une trop grande quantité d’informations.
Faites en sorte que votre conception soit épurée et réduite aux composants les plus simples. La quantité d’informations affichées sur une télévision doit être comparable à ce que vous pourriez voir sur un téléphone mobile, plutôt que sur un bureau d’ordinateur.

![Écran d’accueil de Xbox One](images/designing-for-tv/xbox-home-screen.png)

### <a name="coherent"></a>Cohérence

Les applications UWP dans un environnement de 3 mètres doivent être intuitives et faciles d’utilisation. Le focus doit apparaître clairement.
Organisez le contenu de manière à ce que la navigation soit prévisible et cohérente. Fournissez à vos utilisateurs le chemin d’accès le plus rapide au contenu souhaité.

![L’application Xbox One Movies](images/designing-for-tv/xbox-movies-app.png)

_**Tous les films, illustrées à la capture d’écran sont disponibles sur Microsoft Movies & TV.**_  

### <a name="captivating"></a>Captivant

Les expériences les plus immersives et cinématographiques se passent sur grand écran. Des paysages de bord à bord et l’utilisation de couleurs et d’une typographie vives font passer vos applications au niveau supérieur. Osez. Imaginez.

![Application des avatars de Xbox One](images/designing-for-tv/xbox-avatar-app.png)

### <a name="optimizations-for-the-10-foot-experience"></a>Optimisations en matière d’expérience « 10-foot »

À présent que vous connaissez les principes d’une bonne conception d’application UWP pour une expérience « 10-foot », lisez les descriptions suivantes pour vous approprier les différentes façons d’optimiser votre application et créer une expérience utilisateur améliorée.

| Fonctionnalité        | Description           |
| -------------------------------------------------------------- |--------------------------------|
| [Dimensionnement d’éléments de l’interface utilisateur](#ui-element-sizing)  | La plateforme Windows universelle utilise la [mise à l’échelle et les pixels effectifs](../basics/design-and-ui-intro.md#effective-pixels-and-scaling) pour mettre à l’échelle l’interface utilisateur en fonction de la distance d’affichage. Le fait de comprendre le redimensionnement et de l’appliquer à votre interface utilisateur vous aide à optimiser votre environnement de 3 mètres.  |
|  [Zone de TV-safe](#tv-safe-area) | La plateforme UWP évite automatiquement et par défaut l’affichage de contenu dans les zones non adaptées à l’écran de TV (près des bords de l’écran). Cela crée cependant un effet « d’encadré » ; l’interface utilisateur semble alors s’afficher dans un cadre. Pour que votre application soit véritablement immersive sur les écrans de télévision, vous devez la modifier afin qu’elle s’étende jusqu’aux bords des écrans compatibles. |
| [Couleurs](#colors)  |  La plateforme UWP prend en charge les thèmes de couleur. Une application qui respecte le thème du système sera **foncée** par défaut sur Xbox One. Si votre application possède un thème de couleur spécifique, gardez à l’esprit que certaines couleurs ne fonctionnent pas correctement sur les écrans de télévision et doivent donc être évitées. |
| [Signal sonore](../style/sound.md)    | Les sons jouent un rôle clé dans l’expérience « 10-foot », contribuant ainsi à l’envoi de commentaires à l’utilisateur. La plateforme UWP fournit des fonctionnalités qui activent automatiquement les sons des contrôles courants lorsque l’application s’exécute sur Xbox One. Découvrez la prise en charge des sons intégrée à la plateforme UWP et comment en tirer partie.    |
| [Instructions pour les contrôles d’interface utilisateur](#guidelines-for-ui-controls)  |  Il existe plusieurs contrôles d’interface utilisateur qui fonctionnent correctement sur plusieurs appareils, mais pour lesquels certains éléments doivent être pris en compte s’ils sont utilisés sur un téléviseur. Découvrez certaines meilleures pratiques portant sur l’utilisation de ces contrôles lors de la conception pour l’expérience « 10-foot ». |
| [Déclencheur d’état visuel personnalisé pour Xbox](#custom-visual-state-trigger-for-xbox) | Pour personnaliser votre application UWP pour l’expérience « 10-foot », nous vous recommandons d’utiliser un *déclencheur d’état visuel* personnalisé pour modifier la disposition lorsque l’application détecte son lancement sur une console Xbox. |

En plus des considérations sur la disposition et conception précédente, il existe un nombre de [interaction gamepad et de contrôle à distance](../input/gamepad-and-remote-interactions.md) optimisations, vous devez envisager lors de la création de votre application.

| Fonctionnalité        | Description           |
| -------------------------------------------------------------- |--------------------------------|
| [Interaction et navigation du focus XY](../input/gamepad-and-remote-interactions.md#xy-focus-navigation-and-interaction) | **Navigation du focus XY** permet à l’utilisateur à naviguer dans l’interface utilisateur de votre application. Toutefois, cela limite la navigation à quatre directions : haut, bas, gauche et droite. Cette section apporte des recommandations pour y remédier ainsi que d’autres considérations. |
| [Mode de la souris](../input/gamepad-and-remote-interactions.md#mouse-mode)|Navigation du focus XY n’est pas pratique, ou même possible, pour certains types d’applications, telles que les cartes ou de dessin et de peinture des applications. Dans ce cas, **mode de la souris** permet aux utilisateurs naviguent librement avec un boîtier de commande ou le contrôle à distance, tout comme une souris sur un PC.|
| [Visuel de focus](../input/gamepad-and-remote-interactions.md#focus-visual)  | Focus visuel est une bordure qui met en surbrillance l’élément d’interface utilisateur ayant actuellement le focus. Cela permet à l’utilisateur de rapidement identifier l’interface utilisateur de parcourir ou de l’interaction avec.  |
| [Engagement de focus](../input/gamepad-and-remote-interactions.md#focus-engagement) | Engagement de focus, l’utilisateur doit appuyer sur la **A/sélectionner** bouton sur un boîtier de commande ou le contrôle à distance quand un élément d’interface utilisateur a le focus pour interagir avec lui. |
| [Boutons matériels](../input/gamepad-and-remote-interactions.md#hardware-buttons) | Le boîtier de commande et le contrôle à distance fournissent des configurations et des boutons très différents. |

> [!NOTE]
> La plupart des extraits de code dans cette rubrique sont en langage XAML/C#. Mais, les principes et les concepts s’appliquent à toutes les applications UWP. Si vous développez une application UWP en HTML/JavaScript pour Xbox, consultez l’excellente bibliothèque [TVHelpers](https://github.com/Microsoft/TVHelpers/wiki) sur GitHub.

## <a name="ui-element-sizing"></a>Redimensionnement des éléments de l’interface utilisateur

Étant donné que l’utilisateur d’une application dans un environnement de 3 mètres utilise un boîtier de commande ou une télécommande et se trouve à plusieurs mètres de l’écran, vous devez incorporer à votre conception certains éléments liés à l’interface utilisateur.
Assurez-vous que le contenu sur l’interface utilisateur présente la densité appropriée et que l’interface n’est pas trop encombrée, afin que l’utilisateur puisse la parcourir et sélectionner des éléments en toute simplicité. N’oubliez pas que la simplicité est le maître mot.

### <a name="scale-factor-and-adaptive-layout"></a>Facteur d’échelle et disposition adaptative

Le **facteur d’échelle** permet de s’assurer que le dimensionnement des éléments de l’interface utilisateur est approprié pour l’appareil sur lequel s’exécute l’application.
Sur le Bureau, ce paramètre se trouve dans **Paramètres &gt; Système &gt; Affichage** sous forme de curseur.
Ce paramètre existe également sur les appareils mobiles prenant en charge ce paramètre.

![Modifier la taille du texte, des applications et des autres éléments](images/designing-for-tv/ui-scaling.png)

Sur Xbox One, aucun paramètre système ne permet d’effectuer ces opérations. Cependant, pour que les éléments d’interface utilisateur UWP aient une taille appropriée pour la TV, ils sont dimensionnés par défaut à **200 %** pour les applications XAML et à **150 %** pour les applications HTML.
Tant que les éléments de l’interface utilisateur ont une taille appropriée pour les autres appareils, ce sera également le cas pour la TV.
Xbox One effectue le rendu de votre application à 1080 p (1920 x 1080 pixels). Par conséquent, lorsque vous intégrez une application à partir d’autres appareils, comme un PC, assurez-vous de l’aspect correct de l’interface utilisateur à 960 x 540 pixels à une échelle de 100 % (ou à 1280 x 720 pixels à une échelle de 100 % pour les applications HTML) à l’aide de [techniques adaptatives](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

La conception pour Xbox diffère quelque peu de la conception pour PC, car une seule résolution est concernée : 1920 x 1080.
Peu importe si l’utilisateur possède une télévision dont la résolution est supérieure &mdash; les applications UWP sont toujours mises à l’échelle à 1080 p.

Les tailles de ressource appropriées sont également extraites pour votre application à partir de l’échelle 200 % (ou 150 % pour les applications HTML) lorsque l’application est exécutée sur Xbox One, quelle que soit la résolution de la télévision.

### <a name="content-density"></a>Densité de contenu

Lorsque vous concevez votre application, n’oubliez pas que l’utilisateur se trouve relativement loin de l’interface utilisateur et interagit avec cette dernière à l’aide d’un contrôleur de jeu ou d’une télécommande, qui rend la navigation plus chronophage que s’il utilisait une souris ou une entrée tactile.

#### <a name="sizes-of-ui-controls"></a>Tailles des contrôles d’interface utilisateur

La taille des éléments interactifs de l’interface utilisateur doit être d’au moins 32 pixels (pixels effectifs). Ceci est la valeur par défaut pour les contrôles UWP courants. Lorsqu’elle est utilisée à une échelle de 200 %, elle permet d’assurer la visibilité à distance des éléments de l’interface utilisateur et de réduire la densité du contenu.

![Bouton UWP à une échelle de 100 % et 200 %](images/designing-for-tv/button-100-200.png)

#### <a name="number-of-clicks"></a>Nombre de clics.

Lorsque l’utilisateur navigue d’un bord de l’écran de télévision à l’autre, la simplification de votre interface utilisateur doit se faire en **six clics** maximum. Là encore s’applique le principe de la **simplicité**. 

![6 icônes pour traverser](images/designing-for-tv/six-clicks.png)

### <a name="text-sizes"></a>Tailles de texte

Pour rendre votre interface utilisateur visible à distance, appuyez-vous sur les règles suivantes :

* Texte principal et lecture du contenu : minimum de 15 epx
* Texte non critique et contenu supplémentaire : au moins 12 epx

Lorsque vous utilisez du texte supérieur à la normale dans votre interface utilisateur, choisissez une taille qui ne limite pas trop l’espace de l’écran (en occupant de l’espace que d’autres contenus pourraient remplir).

### <a name="opting-out-of-scale-factor"></a>Désactivation du facteur d’échelle

Nous recommandons que votre application tire parti de la prise en charge du facteur d’échelle, ce qui lui permettra de s’exécuter correctement sur tous les appareils, ayant été mise à l’échelle pour chaque type d’appareil.
Il reste cependant possible de désactiver ce comportement et de concevoir toutes vos interfaces utilisateur à une échelle de 100 %. Notez que vous ne pouvez pas remplacer le facteur d’échelle par une valeur autre que 100 %.

Dans les applications XAML, vous pouvez annuler le facteur d’échelle en utilisant l’extrait de code suivant :

```csharp
bool result =
    Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```

`result` vous informe si vous a été désinscrit.

Pour plus d’informations et des exemples de code HTML/JavaScript, consultez [Comment désactiver la mise à l’échelle](../../xbox-apps/disable-scaling.md).

Veillez à calculer la taille appropriée des éléments d’interface utilisateur en doublant le nombre *effectif* de pixels *réels* mentionnés dans cette rubrique (ou en multipliant la valeur par 1,5 pour les applications HTML).

## <a name="tv-safe-area"></a>Zones adaptées à l’écran de TV

Le contenu n’occupe pas l’ensemble de l’écran de toutes les télévisions pour des raisons historiques, mais aussi technologiques. La plateforme UWP évite d’afficher du contenu d’interface utilisateur dans des zones inadaptées à l’écran de TV ; elle dessine uniquement l’arrière-plan de la page.

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

Ce n’est pas une solution optimale car elle crée un effet d’« encadré », donnant l’impression que des parties de l’interface utilisateur, telles que le volet et la grille de navigation, ont été tronquées. Vous pouvez cependant faire des optimisations pour étendre certaines parties de l’interface utilisateur à l’ensemble de l’écran. Cela donne à l’application un aspect plus cinématographique.

### <a name="drawing-ui-to-the-edge"></a>Étendre l’IU à l’ensemble de l’écran

Nous vous recommandons d’étendre certains éléments d’interface utilisateur à l’ensemble de l’écran pour apporter à l’utilisateur une véritable expérience d’immersion. Cela comprend les classes [ScrollViewers](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) et [CommandBars](../controls-and-patterns/navigationview.md) ainsi que les [volets de navigation](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar).

En revanche, il est important que le texte et les éléments interactifs ne soient jamais près des bords de l’écran ; cela garantit qu’ils ne seront pas tronqués sur certaines télévisions. Nous recommandons d’étendre seulement les éléments visuels non essentiels à une distance de 5 % des bords de l’écran. Comme mentionné dans [Redimensionnement des éléments de l’interface utilisateur](#ui-element-sizing), une application UWP suivant le facteur d’échelle par défaut de la console Xbox One (200 %) utilise une zone de 960 x 540 epx. Vous devez donc éviter de placer l’interface utilisateur primordiale de votre application dans les zones suivantes :

- 27 epx à partir du haut et du bas
- 48 epx des bords gauche et droit

Les sections suivantes décrivent comment étendre votre interface utilisateur aux bords de l’écran.

#### <a name="core-window-bounds"></a>Limites de fenêtre principale

Pour les applications UWP ciblant uniquement l’expérience « 10-foot », les limites de fenêtre principale constituent une option plus simple.

Dans la méthode `OnLaunched` de `App.xaml.cs`, ajoutez le code suivant :

```csharp
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode
    (Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```

Avec cette ligne de code, la fenêtre d’application remplit l’écran. Vous devez donc déplacer toutes les UI interactives et essentielles dans la zone adaptée à l’écran de TV décrite précédemment. L’interface utilisateur temporaire, telle que les menus contextuels et les classes [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) ouvertes restent automatiquement à l’intérieur de la zone adaptée à l’écran de TV.

![Limites de fenêtre principale](images/designing-for-tv/core-window-bounds.png)

#### <a name="pane-backgrounds"></a>Arrière-plans de volet de navigation

Les volets de navigation sont généralement placés près du bord de l’écran. L’arrière-plan doit donc s’étendre dans la zone inadaptée à l’écran de TV pour ne pas qu’il y ait d’espaces vides. Vous pouvez le faire en modifiant simplement la couleur d’arrière-plan du volet de navigation pour la rendre identique à la couleur d’arrière-plan de l’application.

Les limites de fenêtre principale vous permettent d’étendre votre interface utilisateur aux bords de l’écran (comme décrit précédemment), mais vous devez ensuite utiliser des marges positives sur le contenu de [SplitView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SplitView) pour le conserver dans la zone adaptée à l’écran de TV.

![Volet de navigation étendu aux bords de l’écran](images/designing-for-tv/tv-safe-areas-2.png)

L’arrière-plan du volet de navigation a été étendu aux bords de l’écran, tandis que ses éléments de navigation sont conservés dans la zone adaptée à l’écran de TV.
Le contenu de `SplitView` (dans ce cas, une grille d’éléments) a été étendu vers le bas de l’écran pour ne pas avoir l’air tronqué. La partie supérieure de la grille reste toujours dans la zone adaptée à l’écran de TV. (Pour en savoir plus, voir [Extrémités de listes et de grilles défilantes](#scrolling-ends-of-lists-and-grids)).

L’extrait de code suivant permet de réaliser l’effet en question :

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

[CommandBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar) est un autre exemple de volet généralement positionné près d’un ou plusieurs bords de l’application. Son arrière-plan doit donc s’étendre aux bords des écrans de TV. Il contient généralement un bouton **Plus** (...) sur le côté droit qui doit rester dans la zone adaptée à l’écran de TV. Voici quelques stratégies différentes permettant d’obtenir les interactions et effets visuels souhaités.

**Option 1**: Modification la `CommandBar` la couleur d’un arrière-plan transparent ou la même couleur que l’arrière-plan de la page :

```xml
<CommandBar x:Name="topbar"
            Background="{ThemeResource SystemControlBackgroundAltHighBrush}">
            ...
</CommandBar>
```

`CommandBar` paraît ainsi avoir le même arrière-plan que le reste de la page ; l’arrière-plan s’étend donc vers le bord de l’écran en toute fluidité.

**Option 2**: Ajouter un rectangle dont le remplissage est la même couleur d’arrière-plan en tant que le `CommandBar` en arrière-plan, et qu’il se trouve sous le `CommandBar` et sur le reste de la page :

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

La plateforme UWP comporte des fonctionnalités qui permettent de conserver le visuel du focus à l’intérieur des [VisibleBounds](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.visiblebounds), mais vous devez ajouter du remplissage pour vous assurer que les éléments de liste/grille peuvent défiler à l’écran à l’intérieur de la zone adaptée à l’écran de TV. Plus précisément, vous ajoutez une marge positive à la classe [ItemsPresenter](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsPresenter) des classes [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) ou [GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView), comme l’illustre l’extrait de code suivant :

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

Vous placez l’extrait de code précédent dans les ressources de la page ou de l’application, puis vous y accédez de la manière suivante :

```xml
<Page>
    <Grid>
        <ListView Style="{StaticResource TitleSafeListViewStyle}"
                  ... />
```

> [!NOTE]
> Cet extrait de code est spécifiquement conçu pour les contrôles `ListView`. Pour un style `GridView`, définissez l’attribut [TargetType](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate.targettype) des éléments [ControlTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) et [Style](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) sur `GridView`.

Pour un contrôle plus affiné sur la façon dont les éléments sont mises dans une vue, si votre application cible la version 1803 ou ultérieure, vous pouvez utiliser la [UIElement.BringIntoViewRequested événement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested). Vous pouvez le placer sur le [ItemsPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) pour le **ListView**/**GridView** de l’intercepter avant interne **ScrollViewer** est le cas, comme dans les extraits de code suivant :

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
En raison de la nature de Xbox One, qui génère plus d’expériences *multimédias* que d’expériences de *productivité*, son thème de système est foncé par défaut.
Si le thème de votre application est basé sur les paramètres système, celui-ci devrait être foncé sur Xbox One par défaut.

### <a name="accent-color"></a>Couleur d’accentuation

La plateforme UWP fournit un moyen pratique pour afficher la **couleur d’accentuation** que l’utilisateur a sélectionnée dans ses paramètres système.

Sur Xbox One, l’utilisateur est en mesure de sélectionner une couleur utilisateur, comme il peut sélectionner une couleur d’accentuation sur un PC.
Dans la mesure où votre application appelle ces couleurs d’accentuation par le biais de pinceaux ou de ressources de couleur, la couleur sélectionnée par l’utilisateur dans les paramètres système sera utilisée. Notez que les couleurs d’accentuation sur Xbox One sont définies par l’utilisateur et non par le système.

Notez également que l’ensemble des couleurs utilisateur sur Xbox One n’est pas le même que sur les PC, téléphones et autres appareils.

Tant que votre application utilise une ressource de pinceau, telle que **SystemControlForegroundAccentBrush**, ou une ressource de couleur (**SystemAccentColor**), ou appelle les couleurs d’accentuation directement via l’API [UIColorType.Accent*](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UIColorType), ces couleurs sont remplacées par des couleurs d’accentuation disponibles sur Xbox One. Les couleurs de pinceau à contraste élevé sont également extraites à partir du système de la même manière que sur un PC et un téléphone.

Pour en savoir plus sur la couleur d’accentuation en général, voir [Couleur d’accentuation](../style/color.md#accent-color).

### <a name="color-variance-among-tvs"></a>Variantes de couleur entre les écrans de télévision

Lors de la conception d’applications pour la télévision, notez que les couleurs s’affichent de manière légèrement différente en fonction des types de télévision. Ne supposez pas que les couleurs ressembleront exactement à celles de votre écran. Si votre application repose sur des nuances de couleur subtiles pour différencier les diverses parties de l’interface utilisateur, les couleurs pourraient facilement se mélanger et ainsi dérouter les utilisateurs. Essayez d’utiliser des couleurs assez distinctes pour que les utilisateurs puissent bien les différentier, quelle que soit la télévision qu’ils utilisent.

### <a name="tv-safe-colors"></a>Couleurs adaptées aux écrans de TV

Les valeurs RVB d’une couleur représentent l’intensité du rouge, du vert et du bleu. Les écrans de télévision ne gèrent pas très bien les intensités extrêmes &mdash; ces couleurs peuvent produire un effet de « bandes » ou apparaître délavées sur certains écrans de télévision. En outre, les couleurs à haute intensité peuvent provoquer un effet Bloom (les pixels avoisinants émettent les mêmes couleurs). Bien que les avis divergent sur quelles couleurs sont adaptées aux écrans de TV, les couleurs dont les valeurs RVB sont comprises entre 16 et 235 (ou 10-EB en notation hexadécimale) sont généralement adaptées aux écrans de TV.

![Plage de couleurs adaptées aux écrans de TV](images/designing-for-tv/tv-safe-colors-2.png)

Historiquement, les applications sur Xbox devaient personnaliser leurs couleurs afin qu’elles soient comprises dans cette plage de couleurs « adaptées aux écrans de TV ». Toutefois, depuis Fall Creators Update, Xbox One adapte automatiquement le contenu de la plage complète à la plage adaptée aux écrans de TV. Cela signifie que la plupart des développeurs d’applications n’ont plus à se préoccuper des couleurs adaptées aux écrans de TV.

> [!IMPORTANT]
> Cet effet d’adaptation des couleurs n’est pas appliqué au contenu vidéo qui se trouve déjà dans la plage de couleurs adaptées aux écrans de télévision lors de la lecture à l’aide de [Media Foundation](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk).

Si vous développez une application à l’aide de DirectX 11 ou DirectX 12, et créez votre propre chaîne d’échange pour effectuer le rendu de l’interface utilisateur ou de la vidéo, vous pouvez spécifier l’espace de couleur que vous utilisez en appelant [IDXGISwapChain3::SetColorSpace1](https://docs.microsoft.com/windows/desktop/api/dxgi1_4/nf-dxgi1_4-idxgiswapchain3-setcolorspace1), qui indiquera au système s’il a besoin d’adapter les couleurs ou non.

## <a name="guidelines-for-ui-controls"></a>Recommandations en matière de contrôles d’interface utilisateur

Il existe plusieurs contrôles d’interface utilisateur qui fonctionnent correctement sur plusieurs appareils, mais pour lesquels certains éléments doivent être pris en compte s’ils sont utilisés sur un téléviseur. Découvrez certaines meilleures pratiques portant sur l’utilisation de ces contrôles lors de la conception pour l’expérience « 10-foot ».

### <a name="pivot-control"></a>Contrôle Pivot

Un contrôle [Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) permet une navigation rapide des affichages au sein d’une application en sélectionnant différents en-têtes ou onglets. Le contrôle souligne l’en-tête sur lequel se trouve le focus, ce qui rend plus visible l’en-tête sélectionné lorsque vous utilisez le boîtier de commande/la télécommande.

![Souligné par contrôle Pivot](images/designing-for-tv/pivot-underline.png)

Vous pouvez régler la propriété [Pivot.IsHeaderItemsCarouselEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.isheaderitemscarouselenabledproperty) sur `true` pour que les pivots gardent toujours la même position, et éviter que l’en-tête de pivot sélectionné se place toujours en première position. Cette expérience est plus intéressante pour les grands écrans tels que les écrans de télévision, car le renvoi à la ligne des en-têtes peut gêner les utilisateurs. Si tous les en-têtes de pivot ne sont pas visibles à l’écran en même temps, une barre de défilement permet aux clients d’afficher les autres en-têtes. Toutefois, vous devez vous assurer qu’ils tiennent tous à l’écran pour offrir la meilleure expérience possible. Pour en savoir plus, consultez [Onglets et pivots](/windows/uwp/design/controls-and-patterns/pivot).

### <a name="navigation-pane-a-namenavigation-pane-"></a>Volet de navigation <a name="navigation-pane" />

Un volet de navigation (également appelé *menu hamburger*) est un contrôle de navigation couramment utilisé dans les applications UWP. En règle générale, il s’agit d’un volet comportant plusieurs options dans un menu de style de liste qui dirigera l’utilisateur vers différentes pages. En général, ce volet démarre en mode réduit pour économiser l’espace ; l’utilisateur peut l’ouvrir en cliquant sur un bouton.

Même si les volets de navigation sont très accessibles par souris et écran tactile, ce n’est pas le cas pour le boîtier de commande/la télécommande car l’utilisateur doit ouvrir le volet par le biais d’un bouton. Par conséquent, une bonne pratique consiste à rendre possible l’ouverture du panneau de navigation à l’aide de la touche **Affichage**, ainsi que son ouverture en naviguant tout à gauche de la page. Vous pouvez trouver un exemple de code montrant l’implémentation de ce modèle de conception dans le document [Navigation en mode focus programmé](../input/focus-navigation-programmatic.md#split-view-code-sample). L’accès aux contenus du volet est ainsi grandement facilité. Pour en savoir plus sur la façon dont les volets de navigation se comportent sur des écrans de tailles différentes et pour connaître les meilleures pratiques en matière de navigation pour le boîtier de commande/la télécommande, voir [Volets de navigation](../controls-and-patterns/navigationview.md).

### <a name="commandbar-labels"></a>Libellés CommandBar

Il est judicieux de placer les libellés à droite des icônes sur une classe [CommandBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar) afin de réduire sa hauteur et garantir la cohérence de cette dernière. Vous pouvez le faire en définissant la propriété [CommandBar.DefaultLabelPosition](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.defaultlabelpositionproperty) sur `CommandBarDefaultLabelPosition.Right`.

![CommandBar comportant des libellés à droite des icônes](images/designing-for-tv/commandbar.png)

La définition de cette propriété provoquera également l’affichage permanent des libellés, ce qui fonctionne bien pour l’expérience « 10-foot », car elle réduit le nombre de clics requis pour l’utilisateur. C’est également un excellent modèle que les autres types d’appareil peuvent suivre.

### <a name="tooltip"></a>Info-bulle

Le contrôle [Tooltip](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToolTip) a été introduit pour fournir à l’utilisateur des informations supplémentaires dans l’interface utilisateur lorsqu’il pointe avec la souris ou maintient son doigt sur un élément. Pour le boîtier de commande et la télécommande, `Tooltip` s’affiche après un court instant lorsque l’élément obtient le focus, reste à l’écran pendant un court moment, puis disparaît. Ce comportement peut devenir gênant si trop de contrôles `Tooltip` sont utilisés. Essayez d’éviter `Tooltip` lors de la conception d’applications pour la télévision.

### <a name="button-styles"></a>Styles de bouton

Bien que les boutons UWP standard fonctionnent correctement sur les télévisions, certains styles visuels attirent mieux l’attention sur l’interface utilisateur. Vous devez prendre cela en compte pour l’ensemble des plateformes, en particulier pour l’expérience « 10-foot » ; elles bénéficient d’une communication claire sur l’emplacement du focus. Pour en savoir plus sur ces styles, voir [Boutons](../controls-and-patterns/buttons.md).

### <a name="nested-ui-elements"></a>Éléments d’interface utilisateur imbriquée

L’interface utilisateur imbriquée expose les éléments actionnables imbriqués inclus dans un élément d’interface utilisateur conteneur où l’élément imbriqué et l’élément conteneur peuvent prendre le focus indépendamment l’un de l’autre.

L’interface utilisateur imbriquée est parfaitement indiquée pour certains types d’entrée, mais pas toujours pour les manettes de jeu et les télécommandes, qui font appel à la navigation XY. Veillez à suivre les recommandations fournies dans cette rubrique pour vous assurer que votre interface utilisateur est optimisée pour l’environnement TV (visualisation à 3 mètres) et que l’utilisateur peut facilement accéder à tous les éléments interactifs. Une solution courante consiste à placer des éléments d’interface utilisateur imbriqués dans un `ContextFlyout`.

Pour plus d’informations sur l’interface utilisateur imbriquée, voir [Interface utilisateur imbriquée dans des éléments de liste](../controls-and-patterns/nested-ui.md).

### <a name="mediatransportcontrols"></a>MediaTransportControls

L’élément [MediaTransportControls](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls) permet aux utilisateurs d’interagir avec leur média en fournissant une expérience de lecture par défaut grâce à laquelle ils peuvent lire le contenu, le mettre en pause, activer les sous-titres, etc. Ce contrôle est une propriété de l’objet [MediaPlayerElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) et prend en charge deux options de disposition : *sur une ligne* et *sur deux lignes*. Dans la disposition sur une ligne, le curseur et les boutons de lecture se trouvent tous sur une même ligne, le bouton lecture/pause étant situé à gauche du curseur. Dans la disposition sur deux lignes, le curseur occupe sa propre ligne, les boutons de lecture se trouvant sur une ligne distincte en dessous. Lors de la conception pour l’expérience « 10-foot », la disposition sur deux lignes doit être utilisée, car elle assure une meilleure navigation avec une manette de jeu. Pour activer la disposition sur deux lignes, définissez `IsCompact="False"` pour l’élément `MediaTransportControls` dans la propriété [TransportControls](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.transportcontrols) de la `MediaPlayerElement`.

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

> ![REMARQUE] L’objet `MediaPlayerElement` est uniquement disponible dans Windows 10, version 1607 et ultérieure. Si vous développez une application pour une version antérieure de Windows 10, vous devez utiliser l’objet [MediaElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) à la place. Les recommandations ci-dessus s’appliquent également à l’objet `MediaElement` et la propriété `TransportControls` est accessible de la même manière.

### <a name="search-experience"></a>Expérience de recherche

La recherche de contenu est l’une des fonctions les plus fréquemment exécutées dans le cadre d’une expérience « 10-foot ». Si votre application offre une fonction de recherche, l’utilisateur doit pouvoir y accéder rapidement en utilisant le bouton accélérateur **Y** sur le boîtier de commande.

La plupart des clients connaissent probablement cet accélérateur. Toutefois, si vous le souhaitez, vous pouvez ajouter un glyphe visuel **Y** dans l’interface utilisateur afin d’indiquer que le client peut utiliser le bouton pour accéder à la fonctionnalité de recherche. Si vous ajoutez cet indicateur, veillez à utiliser le symbole de la police **Segoe Xbox Symbol MDL2** (`&#xE3CC;` pour les applications XAML, `\E426` pour les applications HTML) pour fournir une expérience cohérente avec l’interpréteur de commandes Xbox et d’autres applications.

> [!NOTE]
> Comme la police **Segoe Xbox MDL2 Symbol** n’est disponible que sur Xbox, le symbole ne s’affiche pas correctement sur votre PC. Mais, il apparaîtra sur le téléviseur lorsque vous le déploierez sur Xbox.

Dans la mesure où le bouton **Y** n’est disponible que sur le boîtier de commande, veillez à fournir d’autres méthodes d’accès à la recherche, comme des boutons dans l’interface utilisateur. Sinon, certains clients risquent de ne pas pouvoir accéder à la fonctionnalité.

Dans l’expérience « 10-foot », il est souvent plus facile pour les clients d’utiliser une fonction de recherche en mode plein écran, car l’espace disponible sur l’écran est limité. En mode plein écran ou écran partiel, il est préférable que le clavier visuel affiché soit déjà ouvert lorsque l’utilisateur sélectionne la fonction de recherche. Ainsi il peut immédiatement saisir les termes de sa recherche.

## <a name="custom-visual-state-trigger-for-xbox"></a>Déclencheur d’état visuel personnalisé pour Xbox

Pour personnaliser votre application UWP pour l’expérience « 10-foot », nous vous recommandons d’effectuer des modifications de disposition lorsque l’application détecte son lancement sur une console Xbox. L’une des méthodes utilisées pour cette fin est le *déclencheur d’état visuel* personnalisé. Les déclencheurs d’état visuel sont particulièrement utiles lorsque vous souhaitez apporter des modifications dans **Blend pour Visual Studio**. L’extrait de code suivant montre comment créer un déclencheur d’état visuel pour Xbox :

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

Pour créer le déclencheur, ajoutez la classe suivante à votre application. Il s’agit de la classe référencée par le code XAML précédemment :

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

Une fois que vous ajoutez votre déclencheur personnalisé, votre application effectuera automatiquement les modifications de disposition spécifiées dans votre code XAML à chaque fois qu’elle détecte son exécution sur une console Xbox One.

Vous pouvez également utiliser du code pour vérifier si votre application s’exécute sur Xbox, puis effectuer les ajustements appropriés. Vous pouvez utiliser la variable simple suivante pour vérifier si votre application s’exécute sur Xbox :

```csharp
bool IsTenFoot = (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily ==
                    "Windows.Xbox");
```

Après avoir effectué cette vérification, vous pouvez effectuer les réglages appropriés de votre interface utilisateur dans le bloc de code. 

## <a name="summary"></a>Résumé

La conception pour l’expérience « 10-foot » implique de prendre en compte des points spéciaux qui la distinguent de la conception pour toute autre plateforme. Si vous pouvez certainement transférer directement votre application UWP vers Xbox One avec succès, cette application n’est pas nécessairement optimisée pour l’expérience « 10-foot », ce qui peut générer de la frustration chez l’utilisateur. Suivez les recommandations contenues dans cet article pour vous assurer que votre application fonctionne aussi bien que possible sur TV.

## <a name="related-articles"></a>Articles connexes

- [Notions fondamentales de périphérique pour les applications de plateforme universelle Windows (UWP)](index.md)
- [Interactions GamePad et de contrôle à distance](../input/gamepad-and-remote-interactions.md)
- [Audio dans les applications UWP](../style/sound.md)
