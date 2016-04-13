---
Description: Concevez votre application pour une esthétique et un fonctionnement optimaux sur votre télévision.
title: Conception pour Xbox et télévision
ms.assetid: 780209cb-3e8a-4cf7-8f80-8b8f449580bf
label: Designing for Xbox and TV
template: detail.hbs
isNew: true
---
<!--Note to Eliot from Linda: see my comments by searching on v-lcap; after your review, I recommend removing all commented out text unless you think you may need it later; it just gets in the way of an already long doc-->

> \[Cet article présente une fonctionnalité qui n’est pas encore disponible publiquement. Cette fonctionnalité est susceptible d’être considérablement modifiée avant d’être commercialisée. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.\]

# Conception pour Xbox et télévision

Concevez votre application de plateforme Windows universelle (UWP) pour une esthétique et un fonctionnement optimaux sur les écrans de télévision et Xbox One.

## Vue d’ensemble

La plateforme Windows universelle vous permet de créer des expériences agréables sur plusieurs types d’appareils Windows 10. 
La plupart des fonctionnalités fournies par l’infrastructure UWP permettent aux applications d’utiliser la même interface utilisateur sur ces appareils, sans travail supplémentaire. 
Toutefois, des éléments particuliers sont à prendre en compte afin d’optimiser votre application et l’adapter pour un fonctionnement parfait sur les écrans de télévision et Xbox One.

L’expérience qui consiste à se trouver assis sur son fauteuil en face de la télévision et à interagir avec celle-ci à l’aide d’un boîtier de commande ou d’une télécommande est appelée le « **10-foot experience** » 
Ce nom vient du fait que l’utilisateur se trouve généralement à 3 mètres (10 pieds) de l’écran. 
Cela soulève des défis propres à cette expérience, qui ne sont pas présents dans l’expérience « *2-foot experience* » ou lors d’interactions avec un PC. 
Si vous développez une application pour Xbox One ou tout autre appareil dont la sortie et l’entrée se font respectivement sur télévision et par boîtier de commande, vous devez toujours garder ceci à l’esprit.

Toutes les étapes décrites dans cet article ne sont pas nécessaires pour que votre application fonctionne pour les expériences « 10-foot experience », mais le fait de les comprendre et de prendre les décisions appropriées pour votre application créera une meilleure « 10-foot experience », mieux adaptée aux besoins spécifiques de votre application. 
Lorsque vous concevez une application pour un environnement de 3 mètres, prenez en compte les principes de conception suivants.

### Simplicité

La conception pour un environnement de 3 mètres présente des défis uniques. La résolution et la distance d’affichage peuvent rendre difficile pour les utilisateurs l’assimilation d’une trop grande quantité d’informations. 
Faites en sorte que votre conception soit épurée et réduite aux composants les plus simples. La quantité d’informations affichées sur une télévision doit être comparable à ce que vous pourriez voir sur un téléphone mobile, plutôt que sur un bureau d’ordinateur.

![Écran d’accueil de Xbox One](images/designing-for-tv/xbox-home-screen.png)

### Cohérence

Les applications UWP dans un environnement de 3 mètres doivent être intuitives et faciles d’utilisation. Le focus doit apparaître clairement. 
Organisez le contenu de manière à ce que la navigation soit prévisible et cohérente. Fournissez à vos utilisateurs le chemin d’accès le plus rapide au contenu souhaité.

![L’application Xbox One Movies](images/designing-for-tv/xbox-movies-app.png)

_**Tous les films présentés dans la capture d’écran sont disponibles sur Films et TV Microsoft.**_  

### Captivant

Les expériences les plus immersives et cinématographiques se passent sur grand écran. Des paysages de bord à bord et l’utilisation de couleurs et d’une typographie vives font passer vos applications au niveau supérieur. Osez. Imaginez.

![Application des avatars de Xbox One](images/designing-for-tv/xbox-avatar-app.png)

### Optimisations en matière d’expérience « 10-foot »

À présent que vous connaissez les principes d’une bonne conception d’application UWP pour une expérience « 10-foot », lisez les descriptions suivantes pour vous approprier les différentes façons d’optimiser votre application et créer une expérience utilisateur améliorée.
<!--[v-lcap] I recommend shortening the descriptions to the bare minimum, and focusing on the rest in the actual sections. I also rearranged the order of this table to map to the order you have in the document.-->


| Fonctionnalité        | Description           |
| -------------------------------------------------------------- |--------------------------------|
| [Redimensionnement des éléments de l’interface utilisateur](#ui-element-sizing)  | La plateforme Windows universelle utilise la [mise à l’échelle et les pixels effectifs](..\layout\design-and-ui-intro.md#effective-pixels-and-scaling) pour mettre à l’échelle l’interface utilisateur en fonction de la distance d’affichage. Lorsqu’une application UWP est exécutée sur Xbox One, cette dernière utilise un facteur d’échelle par défaut approprié afin de rendre tous les éléments de l’interface utilisateur, tels que le texte et les contrôles courants, facilement visibles depuis votre canapé de l’autre côté de la pièce. Le fait de comprendre le redimensionnement et de l’appliquer à votre interface utilisateur vous aide à optimiser votre environnement de 3 mètres.  |
|  [Zones adaptées à l’écran de TV](#tv-safe-area) | Le contenu n’occupe pas l’ensemble de l’écran de toutes les télévisions pour des raisons historiques, mais aussi technologiques. La plateforme UWP évite automatiquement et par défaut l’affichage de contenu dans les zones non adaptées (près des bords de l’écran). Cela crée cependant un effet d’« encadré » ; l’interface utilisateur semble alors s’afficher dans un cadre. Pour que votre application soit véritablement immersive sur les écrans de télévision, vous devez la modifier afin qu’elle s’étende jusqu’aux bords des écrans compatibles. |
| [Couleurs](#colors)  |  La plateforme UWP prend en charge les thèmes de couleur. Une application qui respecte le thème du système sera **foncée** par défaut sur Xbox One. Si votre application possède un thème de couleur spécifique, gardez à l’esprit que certaines couleurs ne fonctionnent pas correctement sur les écrans de télévision et doivent donc être évitées. Pour les meilleurs résultats possible, envisagez de créer une palette de couleurs spécialement adaptées aux écrans de TV pour votre application. |
| [Boîtier de commande et télécommande](#gamepad-and-remote-control)      | Le bon fonctionnement de votre application avec un boîtier de commande et une télécommande représente l’étape la plus importante de l’optimisation des expériences « 10-foot ». La prise en charge appropriée de la navigation et de l’interaction du clavier permet le bon fonctionnement des entrées du boîtier de commande et de la télécommande. Vous pouvez cependant apporter des améliorations relatives aux boîtiers de commande et aux télécommandes pour optimiser l’expérience d’interaction utilisateur sur un appareil où leurs actions sont relativement limitées. |
| [Mode souris](#mouse-mode)|Dans certaines interfaces utilisateur, telles que les cartes et les surfaces de dessin, l’utilisation de la navigation en mode focus XY est impossible ou peu pratique. Pour ces interfaces, la plateforme UWP propose le **mode souris** pour que l’utilisateur puisse naviguer librement avec le boîtier de commande/télécommande, comme avec une souris sur un bureau d’ordinateur.|
| [Visuel du focus](#focus-visual)  | Le visuel du focus est le contour de l’élément de l’interface utilisateur sur lequel se trouve actuellement le focus. Cela permet de guider l’utilisateur pour une navigation facile dans votre interface utilisateur et sans se perdre. Bien que le visuel du focus fonctionne sur plusieurs types d’appareil Windows 10, vous devrez vous assurer que celui-ci est facile à distinguer sur la TV et que son emplacement par défaut est souvent utilisé par les utilisateurs. Si le focus n’est pas clairement visible, l’utilisateur pourrait se perdre dans votre interface utilisateur et gardera ainsi une mauvaise impression de son expérience. Tirez également parti de la [superposition de l’abandon interactif](#light-dismiss-overlay) pour tirer l’attention sur l’interface utilisateur avec laquelle l’utilisateur interagit.  |
| [Interaction et navigation en mode focus XY](#xy-focus-navigation-and-interaction) | La plateforme UWP propose une **navigation en mode focus XY** qui permet à l’utilisateur de naviguer dans l’interface utilisateur de votre application. Toutefois, cela limite la navigation à quatre directions : haut, bas, gauche et droite. Cette section apporte des conseils pour y remédier ainsi que d’autres considérations. Vous devrez peut-être aussi [interagir avec le focus](#focus-engagement) (en appuyant sur le bouton **Entrée/Sélectionner** pour interagir avec un élément de l’interface utilisateur) afin d’utiliser certains contrôles, tels que les curseurs. Cela réduit le nombre de clics nécessaires pour passer d’un côté de l’écran à l’autre. | 
| [Recommandations en matière de contrôles d’interface utilisateur](#guidelines-for-ui-controls)  |  Il est possible d’apporter des améliorations à votre application qui sont adaptées à tous les appareils Windows 10, pas seulement à Xbox One ou aux expériences « 10-foot ». Plutôt que d’améliorer uniquement les expériences « 10-foot », envisagez d’appliquer ces meilleures pratiques sur tous les appareils pris en charge par la plateforme UWP ; ces dernières conviennent particulièrement aux télévisions.  |

<!--[v-lcap] "Focus engagement" is an H2 section that precedes "XY focus"; I recommend either putting it in its own row in the table ahead of XY focus, or changing it to an H3 section under "XY focus," whichever makes more sense; just as long as the overview table  matches what you do-->

<!--| [Sound](../style/sound.md)  |  Sounds play a key role in the 10-foot experience, helping to immerse and give feedback to the user. The UWP provides functionality that automatically turns on sounds for common controls when the app is running on Xbox One. Find out more about the sound support built into the UWP and learn how to take advantage of it. |-->


<!--[v-lcap] I had to put this table into markdown because the links weren't rendering in HTML.
<table>
    <tr>
        <td> [Gamepad and remote control](#gamepad-and-remote-control)</td>
        <td>Making sure that your app works well with gamepad and remote is the most important step in optimizing for 10-foot experiences. Properly supporting keyboard interaction and navigation helps get gamepad and remote control input working relatively well. However, there are gamepad and remote-specific improvements that you can make to optimize the user interaction experience on a device where their actions are somewhat limited.
        </td>
    </tr>
    <tr>
        <td>[XY focus navigation and interaction](#xy-focus-navigation-and-interaction)</td>
        <td>The UWP provides **XY focus navigation** that allows the user to navigate around your app's UI. However, this limits the user to navigating up, down, left, and right. Recommendations for dealing with this and other considerations are outlined in this section. You may also need to utilize [focus engagement](#focus-engagement) (by pressing the **Enter/Select** button to interact with a UI element) to use certain controls, such as sliders. This cuts down on the number of clicks required to get from one side of the screen to the other.
        </td>
    </tr>
    <tr>
        <td>[Mouse mode](#mouse-mode)</td>
        <td>In some user interfaces, such as maps and drawing surfaces, it is not possible or practical to use XY focus navigation. For these interfaces, the UWP provides **mouse mode** to let the gamepad/remote navigate freely, like a mouse on a desktop computer.
        </td>
    </tr>
    <tr>
        <td>[Focus visual](#focus-visual)</td>
        <td>The focus visual is the border around the UI element that currently has focus. This helps orient the user so that they can easily navigate your UI without getting lost. While focus visual works across multiple Windows 10 devices, you will want to make sure that it is easy to see on TV and that it defaults to a location that the user will interact with often. If the focus is not clearly visible, the user could get lost in your UI and not have a great experience. Also, take advantage of [Light dismiss overlay](#light-dismiss-overlay) to call attention to UI that a user is currently interacting with.
        </td>
    </tr>
    <tr>
        <td>[UI element sizing](#ui-element-sizing)</td>
        <td>
            The Universal Windows Platform uses [scaling and effective pixels](..\layout\design-and-ui-intro.md#effective-pixels-and-scaling) to scale the UI according to the viewing distance. 
            When a UWP app is running on Xbox One, it uses an appropriate default scale factor in order to make all the UI elements, such as text and common controls, easily visible from your couch across the room. 
            Understanding sizing and applying it across your UI will help optimize your app for the 10-foot environment.
        </td>
    </tr>
    <tr>
        <td>[TV-safe area](#tv-safe-area)</td>
        <td>
            Not all TVs display content all the way to the edge of the screen due to historical as well as technological reasons. 
            The UWP will automatically avoid displaying any UI in unsafe areas (areas close to the edges of the screen) by default. However, this creates a "boxed-in" effect in which the UI looks letterboxed. 
            For your app to be truly immersive on TV, you will want to modify it so that it extends to the edges of the screen on TVs that support it.
        </td>
    </tr>
    <tr>
        <td>[Colors](#colors)</td>
        <td>
            The UWP supports color themes, and an app that respects the system theme will default to **dark** on Xbox One. 
            If your app has a specific color theme, you should consider that some colors don't work well for TV and should be avoided. 
            For the best possible results, consider creating a TV-specific color palette for your app.
        </td>
    </tr>
    <tr>
        <td>[Sound](../style/sound.md)</td>
        <td>
            Sounds play a key role in the 10-foot experience, helping to immerse and give feedback to the user. The UWP provides functionality that automatically turns on sounds for common controls when the app is running on Xbox One. Find out more about the sound support built into the UWP and learn how to take advantage of it.
        </td>
    </tr>
    <tr>
        <td>[Guidelines for UI controls](#guidelines-for-ui-controls)</td>
        <td>
            There are certain improvements you can make to your app that make sense for all Windows 10 devices, not just Xbox One or other 10-foot experiences. 
            Rather than only improving the 10-foot experience, consider applying these best practices across all devices supported by the UWP, which are particularly beneficial for TV.
        </td>
    </tr>
</table>-->

<!--### Gamepad and remote interaction

Users of your TV app won't be interacting with the UI with a high-precision input device such as a mouse, and this needs to be taken into account when designing your app's interface. Visibility of the **focus visual** (the border highlighting the UI element that the user is currently navigated to) is also key to making sure that the user doesn't get lost.

#### [Gamepad and remote control](#gamepad-and-remote-control)

Making sure that your app works well with gamepad and remote is the most important step in optimizing for TV. As mentioned in [Running UWP apps on Xbox One](#running-uwp-apps-on-xbox-one), properly supporting keyboard interaction and navigation helps get gamepad and remote control input working relatively well. However, there are gamepad- and remote-specific improvements that you can make to optimize the user interaction experience on a device where their actions are somewhat limited.

#### [Focus visual](#focus-visual)

While focus visual works across multiple Windows 10 devices, you will want to make sure that it is easy to see on TV and that it defaults to a location that the user will interact with often. If the focus is not clearly visible, the user could get lost in your UI and not have a great experience.

#### [2D focus navigation and interaction](#2d-focus-navigation-and-interaction)

UWP provides 2D navigation that allows the user to navigate around your app's UI. However, this limits the user to navigating up, down, left, and right. Recommendations for dealing with this and other considerations are outlined in [2D navigation best practices](#2d-navigation-best-practices). You may also need to utilize [focus engagement](#focus-engagement) (pressing the **Enter/Accept** button to interact with a UI element) to use certain controls, such as sliders. This cuts down on the number of clicks required to get from one side of the screen to the other.

#### [Mouse mode](#mouse-mode)

In some user interfaces, such as maps and drawing surfaces, it is not possible or practical to use 2D navigation. For these interfaces, UWP provides **mouse mode** to let the gamepad/remote navigate freely, like a mouse on a desktop computer.

### Layout and content density

Unlike with desktop apps, in which the user is close to the screen, a user interacting with your app on TV will likely be sitting in a couch across the room. Because of this, reducing content density is important so that the user can easily navigate your app. Remember: simplicity is key.

#### [UI element sizing](#ui-element-sizing)

The Universal Windows Platform uses [scaling and effective pixels](../layout/grid.md) to scale the UI according to the viewing distance. When a UWP app is running on Xbox One, it uses an appropriate default scale factor in order to make all the UI elements, such as text and common controls, easily visible from your couch across the room. Understanding sizing and applying it across your UI will help optimize your app for TV.

#### [TV-safe area](#tv-safe-area)

Not all TVs display content all the way to the edge of the screen due to historical as well as technological reasons. UWP will automatically avoid displaying any UI in unsafe areas (areas close to the edges of the screen) by default. However, this creates a "boxed-in" effect in which the UI looks letterboxed. For your app to be truly immersive on TV, you will want to modify it so that it extends to the edges of the screen on TVs that support it.

### Styles for TV

When designing for TV, there are special considerations in regards to color and sound. The styles you use may work great for other devices, but might need some optimizing to make them shine on TV.

#### [Colors](#colors)

UWP supports color themes, and an app that respects the system theme will default to **dark** on Xbox One. If your app has a specific color theme, you should consider that some colors don't work well for TV and should be avoided. For the best possible results, consider creating a TV-specific color palette for your app.

#### [Sound](../style/sound.md)

Sounds play a key role in TV apps, helping immerse the user in the interactive experience. Find out more about the sound support built into UWP and learn how to take advantage of it.

### [Improvements for all platforms](#improvements-for-all-platforms)

There are certain improvements you can make to your app that make sense for all Windows 10 platforms, not just Xbox One or other TV experiences. Rather than only improving the TV experience, consider applying these best practices that apply across all platforms UWP supports, and are particularly beneficial for TV.
-->

<!--## Running UWP apps on Xbox One

Some features for UWP in the 10-foot environment are built-in, and you will get these without any additional work once you properly port to Xbox One. However, there are other optimizations you may want to consider making so that your app performs as well as it can on Xbox. Learn about these features in the following sections.

### Gamepad and remote control support

Arrow key and tab stop behavior (pressing **Tab** to get to the next UI element) on keyboard informs how 2D navigation moves focus through the **D-pad** on a game controller or remote. The **Enter** and **Space** keys are also automatically mapped to the **Enter/Accept** key (see [Gamepad and remote control](#gamepad-and-remote-control)).

The quality of gamepad and remote behavior that you get out of the box depends on how well keyboard is supported in your app, and thus may vary greatly from app to app. A good way to ensure that your app will work well with gamepad/remote is to make sure that it works well with keyboard on PC, and then test with gamepad/remote to find weak spots in your UI.

### TV-safe area

Using the [VisibleBounds](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.applicationview.visiblebounds.aspx) functionality of UWP (for more information, see [TV-safe area](#tv-safe-area)), the following will be done automatically for your app:

* All UI elements are drawn inside the TV-safe area
* The page's background colors are drawn all the way to the edges of the TV

![TV-safe area](images/designing-for-tv/tv-safe-area.png)

### UI scaling

UWP supports proper scaling of UI based on the settings of the system on which the app is currently running. On desktop, this setting can be found in **Settings > System > Display** as a sliding value. This same setting exists on phone as well if the device supports it.

![Change the size of text, apps, and other items](images/designing-for-tv/ui-scaling.png)

On Xbox One there is no such system setting, however in order for UWP UI elements to be sized appropriately for TV, they are scaled at a default of **200%**. As long as UI elements are appropriately sized for other platforms, they will be appropriately sized for TV as well. For more information, see [UI element sizing](#ui-element-sizing).


### Focus visual

The same focus visual can be used for keyboard as well as game controller input, and will always be highly visible on any platform that supports focus visual, including Xbox One and the 10-foot experience. For more information, see [Focus visual](#focus-visual).

### Sound

UWP provides functionality that automatically turns on sounds for common controls when the app is running on Xbox One. This helps provide feedback to the user that they have interacted with the app in some way. For more information, see [Sound](../style/sound.md).

### Accent color and high contrast colors

As long as your app uses a brush resource such as **SystemControlForegroundAccentBrush**, or a color resource (**SystemAccentColor**), or instead calls accent colors directly through the [UIColorType.Accent*](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx) API, those colors are replaced with accent colors appropriate for TV. High contrast brush colors are also pulled in from the system the same way as on PC and phone, but with TV-appropriate colors.

### Light dismiss overlay

In order to call the user's attention to the UI elements that the user is currently manipulating with the game controller or remote control, UWP automatically adds a "smoke" layer that covers areas outside of the popup UI when the app is running on Xbox One. This requires no extra work, but is something to keep in mind when designing your UI.
-->

## Redimensionnement des éléments de l’interface utilisateur

Étant donné que l’utilisateur d’une application dans un environnement de 3 mètres utilise un boîtier de commande ou une télécommande et se trouve à plusieurs mètres de l’écran, vous devez incorporer à votre conception certains éléments liés à l’interface utilisateur. 
Assurez-vous que le contenu sur l’interface utilisateur présente la densité appropriée et que l’interface n’est pas trop encombrée, afin que l’utilisateur puisse la parcourir et sélectionner des éléments en toute simplicité. N’oubliez pas que la simplicité est le maître mot.

### Facteur d’échelle et disposition adaptative

Le **facteur d’échelle** permet d’assurer que le dimensionnement des éléments de l’interface utilisateur est approprié pour l’appareil sur lequel s’exécute l’application. 
Sur le bureau, ce paramètre se trouve dans **Paramètres > Système > Affichage** sous forme de curseur. 
Ce paramètre existe également sur les appareils mobiles prenant en charge ce paramètre.

![Modifier la taille du texte, des applications et des autres éléments](images/designing-for-tv/ui-scaling.png) 

Xbox One ne présente pas ce paramètre ; cependant, pour avoir une taille appropriée, les éléments d’IU de plateforme UWP sont mis à l’échelle à une valeur de **200 %** par défaut. 
Du moment que les éléments d’IU sont d’une taille appropriée pour les autres appareils, ils le seront également pour la TV. 
Xbox One effectue le rendu de votre application à 1080p (1920 x 1080 pixels). Par conséquent, lorsque vous convertissez une application à partir d’autres appareils, tels que des PC, 
assurez-vous de l’aspect correct de l’interface utilisateur à 960 x 540 pixels sur une échelle de 100 % à l’aide des [techniques adaptatives](https://msdn.microsoft.com/en-us/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design).

La conception pour Xbox diffère de la conception pour PC car une seule résolution est concernée : 1920 x 1080. 
Peu importe si l’utilisateur possède une télévision dont la résolution est meilleure ; les applications UWP seront toujours mises à l’échelle à 1080p.

Les tailles de ressources appropriées seront également extraites pour votre application à partir de l’échelle de 200 % lorsque l’application est exécutée sur Xbox One, quelle que soit la résolution de la télévision.

### Densité de contenu

Lorsque vous concevez votre application, n’oubliez pas que l’utilisateur se trouve relativement loin de l’interface utilisateur et interagit avec cette dernière à l’aide d’un contrôleur de jeu ou d’une télécommande, qui rend la navigation plus chronophage que s’il utilisait une souris ou une entrée tactile.

#### Tailles des contrôles d’interface utilisateur

La taille des éléments interactifs de l’interface utilisateur doit être d’au moins 32 pixels (pixels effectifs). Ceci est la valeur par défaut pour les contrôles UWP courants. Lorsqu’elle est utilisée à une échelle de 200 %, elle permet d’assurer la visibilité à distance des éléments de l’interface utilisateur et de réduire la densité du contenu. 

<!--For more information about effective pixels, see [Effective pixels](../layout/grid.md#effective-pixels).-->

![Bouton UWP à une échelle de 100 % et 200 %](images/designing-for-tv/button-100-200.png)

#### Nombre de clics.

Lorsque l’utilisateur navigue d’un bord de l’écran de télévision à l’autre, la simplification de votre interface utilisateur doit se faire en **six clics** maximum. Là encore s’applique le principe de la **simplicité**. Pour en savoir plus, voir [Chemin nécessitant le moins de clics](#path-of-least-clicks).

![6 icônes pour traverser](images/designing-for-tv/six-clicks.png)

### Tailles de texte

Pour rendre votre interface utilisateur visible à distance, appuyez-vous sur les règles suivantes :

* Texte principal et contenu de lecture : 15 epx au minimum
* Texte non critique et contenu supplémentaire : 12 epx au minimum

Lorsque vous utilisez du texte supérieur à la normale dans votre interface utilisateur, choisissez une taille qui ne limite pas trop l’espace de l’écran (en occupant de l’espace que d’autres contenus pourraient remplir).

### Désactivation du facteur d’échelle

Nous recommandons que votre application tire parti de la prise en charge du facteur d’échelle, ce qui lui permettra de s’exécuter correctement sur tous les appareils, ayant été mise à l’échelle pour chaque type d’appareil. 
Il reste cependant possible de désactiver ce comportement et de concevoir toutes vos interfaces utilisateur à une échelle de 100 %. Notez que vous ne pouvez pas modifier la valeur du facteur d’échelle sur autre chose que 100 %.

Vous pouvez choisir d’annuler le facteur d’échelle à l’aide de l’extrait de code suivant :

```csharp
bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```

`result` vous indiquera si vous avez réussi l’annulation.

Veillez à calculer la taille appropriée des éléments d’interface utilisateur en doublant les valeurs de pixel *effectives* mentionnées dans cette rubrique pour des valeurs de pixel *réelles*.

## Zones adaptées à l’écran de TV

Le contenu n’occupe pas l’ensemble de l’écran de toutes les télévisions pour des raisons historiques, mais aussi technologiques. La plateforme UWP évite d’afficher du contenu d’interface utilisateur dans des zones inadaptées à l’écran de TV ; elle dessine uniquement l’arrière-plan de la page.

La zone inadaptée à l’écran de TV est représentée par la zone bleue dans l’image suivante.

![Zone inadaptée à l’écran de TV](images/designing-for-tv/tv-unsafe-area.png)

Vous pouvez définir l’arrière-plan sur une couleur statique ou une couleur de thème, voire sur une image, comme le démontrent les extraits de code suivants.

### Couleur de thème

```xml
<Page x:Class="Sample.MainPage"
      Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"/>
```

### Image

```xml
<Page x:Class="Sample.MainPage"
      Background="\Assets\Background.png"/>
```

Votre application ressemblera à ceci sans travail supplémentaire.

![Zones adaptées à l’écran de TV](images/designing-for-tv/tv-safe-area.png)

Ce n’est pas une solution optimale car elle crée un effet d’« encadré », donnant l’impression que des parties de l’interface utilisateur, telles que le volet et la grille de navigation, ont été tronquées. Vous pouvez cependant faire des optimisations pour étendre certaines parties de l’interface utilisateur à l’ensemble de l’écran. Cela donne à l’application un aspect plus cinématographique.

### Étendre l’IU à l’ensemble de l’écran

Nous vous recommandons d’étendre certains éléments d’interface utilisateur à l’ensemble de l’écran pour apporter à l’utilisateur une véritable expérience d’immersion. Cela comprend les classes [ScrollViewers](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) et [CommandBars](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx) ainsi que les [volets de navigation](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/nav-pane).

En revanche, il est important que le texte et les éléments interactifs ne soient jamais près des bords de l’écran ; cela garantit qu’ils ne seront pas tronqués sur certaines télévisions. Nous recommandons d’étendre seulement les éléments visuels non essentiels à une distance de 5 % des bords de l’écran. Comme mentionné dans [Redimensionnement des éléments de l’interface utilisateur](#ui-element-sizing), une application UWP suivant le facteur d’échelle par défaut de la console Xbox One (200 %) utilisera une zone de 960 x 540 epx. Vous devez donc éviter de placer l’interface utilisateur primordiale de votre application dans les zones suivantes :

- 27 epx à partir du haut et du bas
- 48 epx des bords gauche et droit

Il existe deux façons d’étendre l’interface utilisateur vers les bords de l’écran : les *limites de la fenêtre principale* et les *marges négatives*.

### Limites de fenêtre principale

Pour les applications UWP ciblant uniquement l’expérience « 10-foot », les limites de fenêtre principale constituent une option plus simple.

Dans la méthode `OnLaunched` de `App.xaml.cs`, ajoutez le code suivant :

```csharp
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode
    (Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```

Avec cette ligne de code, la fenêtre d’application remplit l’écran. Vous devez donc déplacer toutes les UI interactives et essentielles dans la zone adaptée à l’écran de TV décrite précédemment. L’interface utilisateur temporaire, telle que les menus contextuels et les classes [ComboBox](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.combobox.aspx) ouvertes resteront automatiquement à l’intérieur de la zone adaptée à l’écran de TV.

![Limites de fenêtre principale](images/designing-for-tv/core-window-bounds.png)

### Marges négatives

Pour les applications UWP ciblant un éventail d’appareils tels que les mobiles, les bureaux et Xbox One, les marges négatives peuvent constituer une méthode plus intuitive pour adapter les dispositions adaptatives. 
Nous vous recommandons de créer un [déclencheur personnalisé](#custom-visual-state-trigger-for-xbox-one) et modifier les marges pour les adapter aux dispositions des télévisions.

#### Arrière-plans de volet de navigation 

<!--[v-lcap] Do you mean "panel" or "pane" in this title and paragraph? 03-29-16 - updated to "pane" per Chigusa-->

Les volets de navigation sont généralement placés près du bord de l’écran. L’arrière-plan doit donc s’étendre dans la zone inadaptée à l’écran de TV pour ne pas qu’il y ait d’espaces vides. 
Vous pouvez faire ceci avec les marges négatives à l’aide du contrôle [SplitView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.splitview.aspx). 
Ce dernier est couramment utilisé en tant que bloc de construction. Vous pouvez également utiliser des marges positives sur le contenu de `SplitView` afin de le maintenir dans la zone adaptée à l’écran de TV.

![Volet de navigation étendu aux bords de l’écran](images/designing-for-tv/tv-safe-areas-2.png)

L’arrière-plan du volet de navigation a été étendu aux bords de l’écran, tandis que ses éléments de navigation sont conservés dans la zone adaptée à l’écran de TV. 
Le contenu de `SplitView` (dans ce cas, une grille d’éléments) a été étendu vers le bas de l’écran pour ne pas avoir l’air tronqué. La partie supérieure de la grille reste dans la zone adaptée à l’écran de TV. Plus loin dans cette section, vous apprendrez à conserver l’élément sélectionné dans la zone adaptée à l’écran de TV.

L’extrait de code suivant permet de réaliser l’effet en question :

```xml
<SplitView x:Name="RootSplitView"
           Margin="-48, -27">
            <SplitView.Pane>
                 <ListView x:Name="NavMenuList"
                           Margin="0,75,0,27"
                           ContainerContentChanging=
                                "NavMenuItemContainerContentChanging"
                           ItemContainerStyle="{StaticResource 
                                NavMenuItemContainerStyle}"
                           ItemTemplate="{StaticResource NavMenuItemTemplate}"
                           ItemInvoked="NavMenuList_ItemInvoked"/>
            </SplitView.Pane>
            <Frame x:Name="frame"
                   Margin="0,27,48,27"
                   Navigating="OnNavigatingToPage"
                   Navigated="OnNavigatedToPage"/>
    </SplitView>
```

[CommandBar](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx) est un autre exemple de volet généralement positionné près d’un ou plusieurs bords de l’application. Son arrière-plan devrait donc s’étendre aux bords des écrans de TV. Il contient aussi généralement un bouton **Plus**, représenté par « … » sur le côté droit, qui doit rester dans la zone adaptée à l’écran de TV. Voici quelques stratégies différentes permettant d’obtenir les interactions et effets visuels souhaités.

**Option 1**: modifie la couleur d’arrière-plan de `CommandBar` sur transparent ou sur la même couleur que l’arrière-plan de la page :

```xml
<CommandBar x:Name="topbar" 
            Background="{ThemeResource SystemControlBackgroundAltHighBrush}">
            ...
</CommandBar>
```

`CommandBar` paraît ainsi avoir le même arrière-plan que le reste de la page ; l’arrière-plan s’étend donc vers le bord de l’écran en toute fluidité.

**Option 2**: ajoute un rectangle en arrière-plan dont le remplissage est de la même couleur que l’arrière-plan de `CommandBar`, puis l’étend aux bords de l’écran avec des marges négatives :

```xml
<Rectangle VerticalAlignment="Top" 
            HorizontalAlignment="Stretch" 
            Margin="0,-27,-48,0"      
            Fill="{ThemeResource SystemControlBackgroundChromeMediumBrush}"/>
<CommandBar x:Name="topbar" 
            VerticalAlignment="Top" 
            HorizontalContentAlignment="Stretch">
            ...
</CommandBar>
```

> **Remarque** Si vous optez pour cette approche, gardez à l’esprit que le bouton **Plus** modifie la hauteur de la classe `CommandBar` ouverte, si nécessaire, afin de montrer les libellés des `AppBarButton` sous leurs icônes. Nous vous recommandons de déplacer les libellés à *droite* de leurs icônes afin d’éviter ce redimensionnement.

#### Éléments multimédias et images d’arrière-plan

La plupart des images et des autres éléments multimédias ne contiennent pas d’informations critiques à leurs extrémités. Ils peuvent donc être étendus aux bords de l’écran pour offrir une expérience utilisateur immersive. L’extrait de code suivant est un exemple de comment étendre une image aux bords de l’écran :

```xml
<Image Source="\Assets\HeaderBackground.png" 
       Stretch="Uniform" 
       Height="227" 
       VerticalAlignment="Top" 
       Margin="-48,-27,-48,0"/>
```

Vous pouvez bien sûr faire de même pour des contenus multimédias, tels que des vidéos.

#### Extrémités de listes et de grilles défilantes

Il est courant que les listes et les grilles contiennent davantage d’éléments que l’écran puisse afficher simultanément. Lorsque c’est le cas, nous vous recommandons d’étendre la liste ou la grille au bord de l’écran. Les listes et les grilles à défilement horizontal doivent s’étendre vers le bord droit et celles à défilement vertical doivent s’étendre vers le bas.

![Grille contenue dans la zone adaptée à l’écran de TV tronquée](images/designing-for-tv/tv-safe-area-grid-cutoff.png)

Pendant qu’une liste ou une grille est étendue de la sorte, il est important de conserver le visuel du focus et son élément associé à l’intérieur de la zone adaptée à l’écran de TV.

![Le focus de la grille défilante doit être conservé à l’intérieur de la zone adaptée à l’écran de TV](images/designing-for-tv/scrolling-grid-focus.png)

L’UWP comporte des fonctionnalités qui permettent de conserver le visuel du focus à l’intérieur de la propriété [VisibleBounds](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.applicationview.visiblebounds.aspx), mais vous devez ajouter du remplissage pour vous assurer que les éléments de liste/grille peuvent défiler à l’écran à l’intérieur de la zone adaptée à l’écran de TV. Plus précisément, vous ajoutez une marge positive à la classe [ItemsPresenter](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.itemspresenter.aspx) des classes [ListView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.listview.aspx) ou [GridView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.gridview.aspx), comme le démontre l’extrait de code suivant :

```xml
<Style x:Key="TitleSafeListViewStyle" 
       TargetType="ListView">
    <Setter Property="Margin" 
            Value="0,0,0,-27"/>
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

> **Remarque** Cet extrait de code est spécifiquement conçu pour un style `ListView` ou `GridView`. Définissez l’attribut [TargetType](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.controltemplate.targettype.aspx) du [ControlTemplate](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.controltemplate.aspx) et du [Style](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.style.aspx) sur `GridView`.


### Déclencheur d’état visuel personnalisé pour Xbox One <a name="custom-visual-state-trigger-for-xbox-one"></a>

Pour personnaliser votre application UWP pour l’expérience « 10-foot », nous vous recommandons d’effectuer des modifications de disposition lorsque l’application détecte son lancement sur une console Xbox. Cela peut être fait en utilisant un déclencheur d’état visuel personnalisé, comme dans l’extrait de code suivant :

```xml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState>
            <VisualState.StateTriggers>
                <triggers:DeviceFamilyTrigger DeviceFamily="Windows.Xbox"/>
            </VisualState.StateTriggers>
            <VisualState.Setters>
                <Setter Target="RootSplitView.Margin" 
                        Value="-48,-27"/>
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

## Couleurs

Par défaut, la plateforme Windows universelle ne modifie pas les couleurs de votre application. Vous pouvez cependant faire des améliorations à la palette de couleurs de votre application pour optimiser l’expérience visuelle sur télévision.

### Thème d’application

Vous pouvez choisir le **thème d’application** (clair ou foncé) en fonction de ce qui est approprié pour votre application, ou vous pouvez ignorer le thème. Pour en savoir plus concernant les recommandations générales pour les thèmes, consultez la section [Thèmes de couleur](../style/color.md#color-themes).

L’UWP permet également aux applications de définir le thème de manière dynamique selon les paramètres système fournis par les appareils sur lesquels elles s’exécutent. 
Bien que l’UWP respecte toujours les paramètres de thème spécifiés par l’utilisateur, chaque appareil fournit également un thème par défaut approprié. 
En raison de la nature de Xbox One, qui génère plus d’expériences de *média* que de *productivité*, son thème de système est foncé par défaut. 
Si le thème de votre application est basé sur les paramètres système, celui-ci devrait être foncé sur Xbox One par défaut.

### Couleur d’accentuation

La plateforme UWP fournit un moyen pratique pour afficher la **couleur d’accentuation** que l’utilisateur a sélectionnée à partir de ses paramètres système.

Sur Xbox One, l’utilisateur est en mesure de sélectionner une couleur utilisateur, comme il peut sélectionner une couleur d’accentuation sur un PC. 
Dans la mesure où votre application appelle ces couleurs d’accentuation par le biais de pinceaux ou de ressources de couleur, la couleur sélectionnée par l’utilisateur dans les paramètres système sera utilisée. Notez que les couleurs d’accentuation sur Xbox One sont définies par l’utilisateur et non par le système.

Notez également que l’ensemble des couleurs utilisateur sur Xbox One n’est pas le même que sur les PC, téléphones et autres appareils. 
C’est en partie dû au fait que ces couleurs sont triées sur le volet pour optimiser l’expérience « 10-foot » sur Xbox One, suivant les mêmes méthodologies et stratégies décrites dans cet article.

Tant que votre application utilise une ressource de pinceau, telle que **SystemControlForegroundAccentBrush**, ou une ressource de couleur (**SystemAccentColor**), ou appelle les couleurs d’accentuation directement via l’API [UIColorType.Accent*](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx), les couleurs sont remplacées par des couleurs d’accentuation qui conviennent pour la télévision. Les couleurs de pinceau à contraste élevé sont également extraites à partir du système de la même manière que sur un PC et téléphone, mais avec des couleurs qui conviennent pour la télévision.

Pour en savoir plus sur la couleur d’accentuation, consultez la section [Couleur d’accentuation](../style/color.md#accent-color).

### Variantes de couleur entre les écrans de télévision

Lors de la conception d’applications pour la télévision, notez que les couleurs s’affichent de manière légèrement différente en fonction des types de télévision. Ne supposez pas que les couleurs ressembleront exactement à celles de votre écran. Si votre application repose sur des nuances de couleur subtiles pour différencier les diverses parties de l’interface utilisateur, les couleurs pourraient facilement se mélanger et ainsi dérouter les utilisateurs. Essayez d’utiliser des couleurs assez distinctes pour que les utilisateurs puissent bien les différentier, quelle que soit la télévision qu’ils utilisent.

### Couleurs adaptées aux écrans de TV

Les valeurs RVB d’une couleur représentent l’intensité du rouge, du vert et du bleu. Les écrans de télévision prennent mal en charge des intensités extrêmes ; par conséquent, évitez d’utiliser ces couleurs lors de la conception pour l’expérience « 10-foot ». Ces couleurs peuvent produire un effet de « bandes » ou apparaître délavées sur certains écrans de télévision. En outre, les couleurs à haute intensité peuvent provoquer un effet Bloom (les pixels avoisinants émettent les mêmes couleurs). 

Bien que les avis divergent sur quelles couleurs sont adaptées aux écrans de TV, les couleurs dont les valeurs RVB sont comprises entre 16 et 235 (ou 10-EB en notation hexadécimale) sont généralement adaptées aux écrans de TV.

![Plage de couleurs adaptées aux écrans de TV](images/designing-for-tv/tv-safe-colors.png)

### Correction des couleurs inadaptées aux écrans de TV

La correction des couleurs inadaptées aux écrans de TV en ajustant leurs valeurs RVB afin qu’elles soient incluses dans la plage des couleurs adaptées est généralement appelée **déplacement de couleur**. Cette méthode peut être appropriée pour une application qui n’utilise pas une palette élargie de couleurs. Toutefois, cette manière de corriger les couleurs peut provoquer la collision des couleurs, ce qui n’assure pas une expérience « 10-foot » optimale.

Au lieu de cela, nous vous recommandons d’utiliser la **mise à l’échelle** une fois que vous avez vérifié que vos couleurs sont adaptées aux écrans de TV, à l’aide de méthodes telles que le déplacement de couleur afin d’optimiser votre palette de couleurs pour la télévision. 

<!--[v-lcap] This seems to contradict what you just said in the previous sentence-->

Cela implique la mise à l’échelle d’un certain facteur de toutes les valeurs RVB de vos couleurs pour qu’elles deviennent adaptées aux écrans de TV. 
La mise à l’échelle de toutes les couleurs de votre application permet d’éviter les collisions de couleur et améliore l’expérience « 10-foot ».

![Déplacement de couleur ou la mise à l’échelle](images/designing-for-tv/clamping-vs-scaling.png)

### Ressources

Lorsque vous modifiez des couleurs, veillez à mettre à jour les ressources en conséquence. Si votre application utilise une couleur dans le code XAML censée être identique à une couleur de ressource, la couleur de la ressource ne semblera pas identique si vous mettez seulement à jour le code XAML.

### Exemple de couleur UWP

Les [thèmes de couleurs UWP](../style/color.md#color-themes) sont conçus selon l’arrière-plan de l’application ; **noirs** pour le thème foncé ou **blancs** pour le thème clair. Ni le noir, ni le blanc étant adaptés aux écrans de TV, ces couleurs doivent être modifiées en utilisant le *déplacement de couleur*. Une fois modifiées, toutes les autres couleurs doivent être ajustées par le biais de la *mise à l’échelle* afin de conserver le contraste nécessaire.

<!--[v-lcap to eliot]why is the above paragraph in the past tense?-->

L’exemple de code suivant fournit un thème de couleurs optimisé pour une utilisation TV :

```xml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="ApplicationPageBackgroundThemeBrush" 
                                 Color="#FF101010"/>
                <Color x:Key="SystemAltHighColor">#FF101010</Color>
                <Color x:Key="SystemAltLowColor">#33101010</Color>
                <Color x:Key="SystemAltMediumColor">#99101010</Color>
                <Color x:Key="SystemAltMediumHighColor">#CC101010</Color>
                <Color x:Key="SystemAltMediumLowColor">#66101010</Color>
                <Color x:Key="SystemBaseHighColor">#FFEBEBEB</Color>
                <Color x:Key="SystemBaseLowColor">#33EBEBEB</Color>
                <Color x:Key="SystemBaseMediumColor">#99EBEBEB</Color>
                <Color x:Key="SystemBaseMediumHighColor">#CCEBEBEB</Color>
                <Color x:Key="SystemBaseMediumLowColor">#66EBEBEB</Color>
                <Color x:Key="SystemChromeAltLowColor">#FFDDDDDD</Color>
                <Color x:Key="SystemChromeBlackHighColor">#FF101010</Color>
                <Color x:Key="SystemChromeBlackLowColor">#33101010</Color>
                <Color x:Key="SystemChromeBlackMediumLowColor">#66101010</Color>
                <Color x:Key="SystemChromeBlackMediumColor">#CC101010</Color>
                <Color x:Key="SystemChromeDisabledHighColor">#FF333333</Color>
                <Color x:Key="SystemChromeDisabledLowColor">#FF858585</Color>
                <Color x:Key="SystemChromeHighColor">#FF767676</Color>
                <Color x:Key="SystemChromeLowColor">#FF1F1F1F</Color>
                <Color x:Key="SystemChromeMediumColor">#FF262626</Color>
                <Color x:Key="SystemChromeMediumLowColor">#FF2B2B2B</Color>
                <Color x:Key="SystemChromeWhiteColor">#FFEBEBEB</Color>
                <Color x:Key="SystemListLowColor">#19EBEBEB</Color>
                <Color x:Key="SystemListMediumColor">#33EBEBEB</Color>
            </ResourceDictionary>
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="ApplicationPageBackgroundThemeBrush" 
                                 Color="#FFEBEBEB" /> 
                <Color x:Key="SystemAltHighColor">#FFEBEBEB</Color>
                <Color x:Key="SystemAltLowColor">#33EBEBEB</Color>
                <Color x:Key="SystemAltMediumColor">#99EBEBEB</Color>
                <Color x:Key="SystemAltMediumHighColor">#CCEBEBEB</Color>
                <Color x:Key="SystemAltMediumLowColor">#66EBEBEB</Color>
                <Color x:Key="SystemBaseHighColor">#FF101010</Color>
                <Color x:Key="SystemBaseLowColor">#33101010</Color>
                <Color x:Key="SystemBaseMediumColor">#99101010</Color>
                <Color x:Key="SystemBaseMediumHighColor">#CC101010</Color>
                <Color x:Key="SystemBaseMediumLowColor">#66101010</Color>
                <Color x:Key="SystemChromeAltLowColor">#FF1F1F1F</Color>
                <Color x:Key="SystemChromeBlackHighColor">#FF101010</Color>
                <Color x:Key="SystemChromeBlackLowColor">#33101010</Color>
                <Color x:Key="SystemChromeBlackMediumLowColor">#66101010</Color>
                <Color x:Key="SystemChromeBlackMediumColor">#CC101010</Color>
                <Color x:Key="SystemChromeDisabledHighColor">#FFCCCCCC</Color>
                <Color x:Key="SystemChromeDisabledLowColor">#FF7A7A7A</Color>
                <Color x:Key="SystemChromeHighColor">#FFB2B2B2</Color>
                <Color x:Key="SystemChromeLowColor">#FFDDDDDD</Color>
                <Color x:Key="SystemChromeMediumColor">#FFCCCCCC</Color>
                <Color x:Key="SystemChromeMediumLowColor">#FFDDDDDD</Color>
                <Color x:Key="SystemChromeWhiteColor">#FFEBEBEB</Color>
                <Color x:Key="SystemListLowColor">#19101010</Color>
                <Color x:Key="SystemListMediumColor">#33101010</Color>
            </ResourceDictionary> 
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

> **Remarque** Le thème clair **SystemChromeMediumLowColor** et **SystemChromeMediumLowColor** sont délibérément de la même couleur et non suite à une opération de déplacement de couleur. 

<!--[v-lcap] Double check that you didn't mean to say something else in the sentence above-->

> **Remarque** Les couleurs hexadécimales s’écrivent sous la forme **ARVB** (Alpha, Rouge, Vert, Bleu).

Nous déconseillons l’utilisation de couleurs adaptées aux écrans de TV sur un écran pouvant afficher la palette complète des couleurs (sans opération de déplacement) car les couleurs sembleraient délavées. Au lieu de cela, chargez le dictionnaire de ressources (exemple précédent) lorsque votre application s’exécute sur Xbox, mais *pas* sur les autres plateformes. Dans la méthode `OnLaunched` de `App.xaml.cs`, ajoutez la vérification suivante :

```csharp
if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
{ 
    this.Resources.MergedDictionaries.Add(new ResourceDictionary 
    { 
        Source = new Uri("ms-appx:///TenFootStylesheet.xaml") 
    }); 
}
```

Cela permet de garantir que les couleurs appropriées s’affichent pour l’appareil sur lequel l’application est exécutée, fournissant à l’utilisateur une expérience plus agréable et plus esthétique.

<!--### Light dismiss overlay

In order to call the user's attention to the UI elements that the user is currently manipulating with the game controller or remote control, UWP automatically adds a "smoke" layer that covers areas outside of the popup UI when the app is running on Xbox One. This requires no extra work, but is something to keep in mind when designing your UI.
-->

## Boîtier de commande et télécommande

Comme le clavier et la souris pour PC et la fonction tactile pour les téléphones et tablettes, le boîtier de commande et la télécommande sont les principaux périphériques d’entrée pour l’expérience « 10-foot ». 
Cette section présente les boutons matériels et leur fonction. 
Dans les sections [Interaction et navigation en mode focus XY](#xy-focus-navigation-and-interaction) et [Mode souris](#mouse-mode), vous découvrirez comment optimiser votre application en cas d’utilisation de ces périphériques d’entrée.

La qualité du comportement du boîtier de commande et de la télécommande lors de la première utilisation dépend de la prise en charge correcte du clavier dans votre application. Un moyen efficace de vous assurer que votre application fonctionne correctement avec le boîtier de commande/télécommande est de voir si celle-ci fonctionne correctement avec le clavier sur PC, puis testez-la avec un boîtier de commande/télécommande pour déterminer les points faibles dans votre interface utilisateur.

### Boutons matériels

Tout au long de ce document, les boutons seront appelés par leurs noms fournis dans le schéma suivant.

![Schéma affichant les boutons d’un boîtier de commande et d’une télécommande](images/designing-for-tv/hardware-buttons-gamepad-remote.png)

Comme vous pouvez le constater sur le schéma, certains des boutons pris en charge sur le boîtier de commande ne le sont pas sur la télécommande, et vice versa. Bien que vous puissiez utiliser des boutons qui sont uniquement pris en charge sur un périphérique d’entrée (pour rendre plus rapide la navigation dans l’interface utilisateur), n’oubliez pas que leur utilisation pour des interactions critiques peut créer une situation dans laquelle l’utilisateur se trouve dans l’impossibilité d’interagir avec certaines parties de l’interface utilisateur.

Le tableau suivant répertorie tous les boutons matériels pris en charge par les applications UWP et les périphériques d’entrée qui les prennent en charge.

| Button                    | Boîtier de commande   | Télécommande    |
|---------------------------|-----------|-------------------|
| Bouton A/Sélectionner           | Oui       | Oui               |
| Bouton B/Précédent             | Oui       | Oui               |
| Bouton multidirectionnel   | Oui       | Oui               |
| Touche Menu               | Oui       | Oui               |
| Touche Affichage               | Oui       | Oui               |
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

### Prise en charge des boutons intégrés

La plateforme UWP mappe automatiquement le comportement d’entrée du clavier existant sur les entrées du boîtier de commande et de la télécommande. Le tableau suivant répertorie ces mappages intégrés.

| Clavier              | Boîtier de commande/Télécommande                        |
|-----------------------|---------------------------------------|
| Touches de direction            | Bouton multidirectionnel (également stick analogique gauche sur le boîtier de commande)    |
| Espace              | Bouton A/Sélectionner                       |
| Entrée                 | Bouton A/Sélectionner                       |
| Échap                | Bouton B/Précédent*                        |

\*Lorsque les événements [KeyDown](https://msdn.microsoft.com/library/windows/apps/br208941) et [KeyUp](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.uielement.keyup.aspx) pour le bouton B ne sont pas gérés par l’application, l’événement [SystemNavigationManager.BackRequested](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.core.systemnavigationmanager.backrequested.aspx) est déclenché, ce qui se traduit par une navigation vers l’arrière au sein de l’application.

### Prise en charge des boutons accélérateurs

Les boutons accélérateurs permettant d’accélérer la navigation dans une interface utilisateur. Cependant, ces boutons peuvent être propres à certains périphériques d’entrée ; certains utilisateurs ne seront donc pas en mesure d’utiliser ces fonctions. En réalité, le boîtier de commande est le seul périphérique d’entrée qui prend en charge les fonctions d’accélération pour les applications UWP sur Xbox One.

Le tableau suivant répertorie la prise en charge intégrée des accélérateurs dans l’UWP, en plus de ce que vous pouvez implémenter vous-même. Intégrez ces comportements à votre interface utilisateur personnalisée afin de proposer une expérience utilisateur cohérente et conviviale.

| Interaction   | Clavier   | Boîtier de commande      | Intégrée pour :  | Recommandée pour : |
|---------------|------------|--------------|----------------|------------------|
| Mouvement panoramique       | Aucun       | Stick analogique droit  | Aucun           |      [ScrollViewer](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) |
| Page vers le haut/bas  | Page vers le haut/bas | Gâchette gauche/droite | Aucun | `ScrollViewer` et la liste/grille
| Page vers la gauche/droite | Aucun | Gâchettes hautes gauche/droite | [Pivot](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.pivot.aspx) | `ScrollViewer`
| Zoom avant/arrière        | Ctrl +/- | Gâchette gauche/droite | `ScrollViewer` | Affichages qui prennent en charge le zoom avant et arrière

<!--[v-lcap] Had to move this table into markdown to get the links to work
<table>
    <tr>
        <th>Interaction</th>
        <th>Keyboard</th>
        <th>Gamepad</th>
        <th>Built-in for:</th>
        <th>Recommended for:</th>
    </tr>
    <tr>
        <td>Panning</td>
        <td>None</td>
        <td>Right stick</td>
        <td>None</td>
        <td>[ScrollViewer](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx)</td>
    </tr>
    <tr>
        <td>Page up/down</td>
        <td>Page up/down</td>
        <td>Left/right triggers</td>
        <td>None</td>
        <td>`ScrollViewer` and list/grid</td>
    </tr>
    <tr>
        <td>Page left/right</td>
        <td>None</td>
        <td>Left/right bumpers</td>
        <td>[Pivot](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.pivot.aspx).</td>
        <td>`ScrollViewer`</td>
    </tr>
    <tr>
        <td>Zoom in/out</td>
        <td>Ctrl +/-</td>
        <td>Left/right triggers</td>
        <td>`ScrollViewer`</td>
        <td>Views that support zooming in and out
    </tr>
</table>-->

## Mode souris

Comme décrit dans [Interaction et navigation en mode focus XY](#xy-focus-navigation-and-interaction), le focus est déplacé à l’aide du système de navigation XY sur Xbox One, ce qui permet à l’utilisateur de déplacer le focus entre les contrôles en effectuant des déplacements vers le haut, le bas, à gauche et à droite. 
Toutefois, certains contrôles, tels que [WebView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.webview.aspx) et 
[MapControl](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.maps.mapcontrol.aspx), 
nécessitent une interaction semblable à celle faite avec la souris, avec laquelle les utilisateurs peuvent déplacer le pointeur librement à l’intérieur des limites du contrôle. 
Certaines applications permettent également de déplacer le pointeur sur la page entière ; l’expérience boîtier de commande/télécommande est alors semblable à l’expérience souris sur PC.

Pour ces scénarios, demandez un pointeur (mode souris) pour la page entière, ou sur un contrôle à l’intérieur d’une page. 
Par exemple, votre application pourrait contenir une page comportant un contrôle `WebView` qui utilise le mode souris uniquement à l’intérieur du contrôle ; la navigation en mode focus XY serait utilisée partout ailleurs. 
Pour demander un pointeur, vous pouvez spécifier si vous le voulez **lorsqu’un contrôle ou une page sont actifs** ou **lorsqu’une page a le focus**.

> **Remarque** La demande de pointeur lorsqu’un contrôle a le focus n’est pas prise en charge.

Le schéma suivant montre les mappages de bouton pour le boîtier de commande/télécommande en mode souris.

![Mappages de bouton pour boîtier de commande/télécommande en mode souris](images/designing-for-tv/mouse-mode.png)

> **Remarque** Le mode souris est uniquement pris en charge sur Xbox One avec le boîtier de commande/télécommande. Il est ignoré sans avertissement sur d’autres familles d’appareils et types d’entrée.

Utilisez la propriété `RequiresPointer` dans un contrôle ou une page pour activer le mode souris sur ceux-ci. `RequiresPointer` a trois valeurs possibles : `Never` (valeur par défaut), `WhenEngaged` et `WhenFocused`.

> **Remarque** `RequiresPointer` est une nouvelle API qui n’a pas encore été documentée. 

<!--TODO: Link to doc-->

### Activation du mode souris dans un contrôle

Lorsque l’utilisateur active un contrôle avec `RequiresPointer="WhenEngaged"`, le mode souris est activé sur le contrôle en question jusqu’à ce que l’utilisateur le désactive. L’extrait de code suivant montre un `MapControl` simple qui, lorsqu’il est activé, active le mode souris :

```xml
<Page>
    <Grid>
        <MapControl IsEngagementRequired="true" 
                    RequiresPointer="WhenEngaged"/>
    </Grid>
</Page> 
```

> **Remarque** Si un contrôle active le mode souris lorsqu’il est employé, il doit également être employé avec `IsEngagementRequired="true"` ; sinon, le mode souris ne sera pas activé.

Lorsqu’un contrôle est en mode souris, ses contrôles imbriqués le seront également. Le mode demandé de ses enfants est ignoré ; un parent ne peut pas être en mode souris si ses enfants ne le sont pas également.

En outre, le mode demandé d’un contrôle est examiné uniquement lorsqu’il a le focus ; le mode n’est pas changé dynamiquement tant qu’il a le focus.

### Activation du mode souris dans une page

Lorsqu’une page a la propriété `RequiresPointer="WhenFocused"`, le mode souris sera activé pour la page entière lorsqu’elle a le focus. L’extrait de code suivant illustre l’obtention de cette propriété par une page :

```xml
<Page RequiresPointer="WhenFocused">
    ...
</Page> 
```

> **Remarque** La valeur `WhenFocused` est uniquement prise en charge sur les objets [Page](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.page.aspx). Une exception est levée si vous tentez de définir cette valeur sur un contrôle.

## Visuel du focus

Le visuel du focus est le contour de l’élément de l’interface utilisateur sur lequel se trouve actuellement le focus. Cela permet de guider l’utilisateur pour une navigation facile dans votre interface utilisateur et sans se perdre.

Avec l’ajout d’une mise à jour visuelle et de nombreuses options de personnalisation au visuel du focus, les développeurs peuvent être certains qu’un seul visuel du focus fonctionne correctement sur les PC et Xbox One, ainsi que sur tous les appareils Windows 10 prenant en charge le clavier et/ou le boîtier de commande/télécommande.

Bien que le même visuel du focus puisse être utilisé sur différentes plateformes, le contexte diffère légèrement pour l’expérience « 10-foot ». Vous devez supposer que l’utilisateur ne se concentre pas sur l’ensemble de l’écran de TV. C’est pourquoi il est important que l’élément ayant actuellement le focus s’affiche toujours clairement à l’utilisateur ; cela évite la frustration de devoir rechercher les éléments visuels.

Il est également important de garder à l’esprit que le visuel du focus s’affiche par défaut lorsqu’un boîtier de commande ou une télécommande sont utilisés, mais *pas* un clavier. Il s’affiche donc lorsque vous exécutez votre application sur Xbox One, même si vous ne l’avez pas implémenté.

### Placement initial du visuel du focus

Lors du lancement d’une application ou de la navigation vers une page, placez le focus sur un élément d’interface utilisateur approprié (le premier élément auquel l’utilisateur applique une action). Par exemple, une application de photos peut placer le focus sur le premier élément dans la galerie de photos, et une application de musique, dans laquelle l’utilisateur a ouvert une vue détaillée d’un morceau, peut placer le focus sur le bouton Lecture pour faciliter la lecture de la musique.

Essayez de placer le focus initial dans la zone supérieure gauche de votre application (ou en haut à droite pour un flux de droite à gauche). La plupart des utilisateurs ont tendance à se concentrer sur cet angle en premier car c’est à cet endroit que commence généralement le flux de contenu d’une application.

### Rendre le focus clairement visible

Un visuel du focus doit toujours être visible à l’écran pour que l’utilisateur puisse reprendre à l’endroit où il s’est arrêté sans avoir à chercher le focus. De même, des éléments pouvant être actifs doivent toujours se trouver à l’écran ; par exemple, n’utilisez pas de fenêtres contextuelles contenant uniquement du texte et pas d’éléments pouvant être actifs.

### Superposition de l’abandon interactif

Pour attirer l’attention de l’utilisateur sur les éléments d’interface utilisateur que ce dernier manipule avec le contrôleur de jeu ou la télécommande, l’UWP ajoute automatiquement une couche « de fumée » qui couvre les zones en dehors de l’interface utilisateur contextuelle lorsque l’application s’exécute sur Xbox One. Cela ne nécessite aucun travail supplémentaire, mais est un élément à prendre en compte lors de la conception de votre interface utilisateur.

## Activation du focus

L’activation du focus est conçue pour rendre plus facile l’utilisation d’un boîtier de commande ou d’une télécommande pour interagir avec une application. 

> **Remarque** La définition de l’activation du focus n’affecte pas le clavier ou d’autres périphériques d’entrée.

Lorsque la propriété `IsFocusEngagementEnabled` sur un objet [FrameworkElement](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.aspx) est définie sur `True`, elle signale le contrôle comme nécessitant une activation du focus. Cela signifie que l’utilisateur doit appuyer sur le bouton **A/sélectionner** pour « activer » le contrôle et interagir avec ce dernier. Lorsqu’il a terminé, l’utilisateur peut appuyer sur le bouton **B/Précédent** pour désactiver le contrôle et naviguer hors de dernier.

> **Remarque** `IsFocusEngagementEnabled` est une nouvelle API qui n’a pas encore été documentée.

### Interruption du focus

L’interruption du focus survient lorsqu’un utilisateur tente d’accéder à l’interface utilisateur d’une application, mais la navigation est « interrompue » au sein d’un contrôle, rendant difficile, voire impossible, le déplacement en dehors de ce contrôle.

L’exemple suivant montre une interface utilisateur provoquant l’interruption du focus.

![Boutons à gauche et à droite d’un curseur horizontal](images/designing-for-tv/focus-engagement-focus-trapping.png)

Si l’utilisateur veut accéder au bouton droit à partir du bouton gauche, il serait logique de supposer que la seule action requise serait d’appuyer deux fois sur le bouton multidirectionnel/stick analogique gauche. 
Toutefois, si le [Slider](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.slider.aspx) ne nécessite pas d’activation, le comportement suivant se produit : lorsque l’utilisateur appuie à droite pour la première fois, le focus est décalé au `Slider` ; lorsqu’il réappuie à droite, la poignée du `Slider` se déplace à droite. L’utilisateur continuerait de déplacer la poignée vers la droite sans toutefois réussir à atteindre le bouton.

Plusieurs approches existent pour contourner ce problème. L’une consiste à concevoir une disposition différente, comme dans l’exemple d’application pour le secteur immobilier dans [Interaction et navigation en mode focus XY](#xy-focus-navigation-and-interaction), où nous avons déplacé les boutons **Précédent** et **Suivant** au-dessus de `ListView`. L’empilement vertical (et non horizontal) des contrôles, comme le montre l’image suivante, peut résoudre le problème.

![Boutons au-dessus et en dessous d’un curseur horizontal](images/designing-for-tv/focus-engagement-focus-trapping-2.png)

À présent, l’utilisateur peut accéder à chaque contrôle en appuyant sur les boutons haut et bas sur le bouton multidirectionnel/stick analogique gauche. Lorsque `Slider` a le focus, l’utilisateur peut appuyer sur le bouton gauche/droite pour déplacer la poignée de `Slider`, comme prévu.

Une autre approche pour résoudre ce problème serait de demander une activation de `Slider`. Si vous définissez `IsFocusEngagementEnabled="True"`, cela se traduit par le comportement suivant.

![Nécessité d’activation du focus sur un curseur afin que l’utilisateur puisse accéder au bouton situé à droite](images/designing-for-tv/focus-engagement-slider.png)

Lorsque le `Slider` nécessite une activation du focus, l’utilisateur peut accéder au bouton situé à droite simplement en appuyant deux fois sur le bouton multidirectionnel/stick analogique gauche. Cette solution est excellente, car elle ne nécessite aucun ajustement de l’interface utilisateur et produit le comportement attendu.

### Contrôles d’éléments

Outre le contrôle [Slider](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.slider.aspx), il existe d’autres contrôles que vous souhaiterez peut-être activer, tels que :

- [Listbox](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.listbox.aspx)
- [ListView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.listview.aspx)
- [GridView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.gridview.aspx)
- [FlipView](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.flipview)

Contrairement au contrôle `Slider`, ces contrôles n’interrompent pas le focus en leur sein ; cependant, ils peuvent poser des problèmes en matière de facilité d’utilisation s’ils contiennent de grandes quantités de données. Voici un exemple d’un contrôle `ListView` qui contient une grande quantité de données.

![Contrôle ListView avec une grande quantité de données et des boutons au-dessus et en dessous](images/designing-for-tv/focus-engagement-list-and-grid-controls.png)

Comme pour l’exemple `Slider`, nous allons essayer de naviguer à partir du bouton du haut vers le bouton du bas avec un boîtier de commande/télécommande. 
En partant du bouton du haut, où se trouve le focus, le fait d’appuyer sur le bouton multidirectionnel/stick analogique gauche déplacera le focus sur le premier élément dans le contrôle `ListView` (« Élément 1 »). 
Lorsque l’utilisateur appuie à nouveau, le focus est déplacé sur l’élément suivant dans la liste, et non sur le bouton situé en bas. 
Pour accéder au bouton du bas, l’utilisateur doit d’abord parcourir chaque élément du contrôle `ListView`. 
Si le contrôle `ListView` contient une grande quantité de données, cela pourrait s’avérer peu pratique et dégrader l’expérience utilisateur.

Pour résoudre ce problème, définissez la propriété `IsFocusEngagementEnabled="True"` sur `ListView` pour demander son activation. 
Cela permet à l’utilisateur de traverser rapidement le contrôle `ListView` en appuyant simplement sur le bouton Bas. Toutefois, 
l’utilisateur ne sera pas en mesure de parcourir la liste ou de sélectionner un élément à partir de celle-ci, sauf s’il l’active en appuyant sur le bouton **A/Sélectionner** lorsqu’il a le focus, puis en appuyant sur le bouton **B/Précédent** pour le désactiver.

![Contrôle ListView avec activation requise](images/designing-for-tv/focus-engagement-list-and-grid-controls-2.png)

#### ScrollViewer

Le contrôle [ScrollViewer](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) diffère légèrement des contrôles précédents. 
Il possède des caractéristiques particulières. Si vous disposez d’un `ScrollViewer` avec du contenu pouvant être actif, la navigation vers `ScrollViewer` vous permet de parcourir ses éléments pouvant être actifs, par défaut . Comme pour un contrôle `ListView`, vous devez parcourir chaque élément pour naviguer en dehors du contrôle `ScrollViewer`. 

Si le contrôle `ScrollViewer` n’a *pas* de contenu pouvant être actif ; par exemple, s’il contient uniquement du texte, vous pouvez définir `IsFocusEngagementEnabled="True"` afin que l’utilisateur puisse activer `ScrollViewer` à l’aide du bouton **A/Sélectionner**. Une fois activé, l’utilisateur peut faire défiler le texte à l’aide du **bouton multidirectionnel/stick analogique gauche**, puis appuyer sur le bouton **B/Précédent** pour le désactiver.

Une autre approche consisterait à définir `IsTabStop="True"` sur `ScrollViewer` afin que l’utilisateur n’ait pas à activer le contrôle ; il pourrait simplement placer 
le focus sur ce dernier, puis effectuer un défilement à l’aide du **bouton multidirectionnel/stick analogique gauche** lorsqu’aucun élément pouvant être actif se trouve au sein du `ScrollViewer`

### Paramètres par défaut de l’activation du focus

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

Aucun autre contrôle UWP n’aura de modification comportementale ou visuelle lorsque `IsFocusEngagementEnabled="True"`.

## Interaction et navigation en mode focus XY

Si votre application prend en charge une navigation en mode focus appropriée pour le clavier, cela conviendra également pour le boîtier de commande et la télécommande. 
La navigation avec les touches de direction est mappée au **bouton multidirectionnel** (ainsi qu’au **stick analogique gauche** sur le boîtier de commande), et l’interaction avec les éléments d’interface utilisateur est mappée à la touche **Entrée/Sélectionner** 
(voir [Boîtier de commande et télécommande](#gamepad-and-remote-control)). Pour des recommandations en matière de conception de clavier, voir [Interactions avec le clavier](keyboard-interactions.md).

Si la prise en charge du clavier est correctement implémentée, votre application fonctionnera assez bien ; toutefois, du travail supplémentaire peut être nécessaire pour la prise en charge de chaque scénario. Réfléchissez aux besoins spécifiques de votre application pour proposer une expérience utilisateur optimale.

<!--### Focus placement

The focus visual should be initially placed on the UI element that makes sense as the first element on which the user would take action. For more information, see [Focus visual](#focus-visual).
-->

### Interface utilisateur inaccessible

Étant donné que la navigation en mode focus XY limite le déplacement vers le haut, le bas, à gauche et à droite, il existe des scénarios où certaines parties de l’interface utilisateur ne sont pas accessibles. 
Le schéma suivant illustre un exemple du type de disposition d’interface utilisateur que la navigation en mode focus XY ne prend pas en charge. 
Notez que l’élément au milieu n’est pas accessible à l’aide du boîtier de commande/télécommande dans la mesure où la navigation horizontale et la navigation verticale sont prioritaires ; l’élément du milieu ne sera jamais d’une priorité suffisamment élevée pour avoir le focus.

![Éléments dans les quatre coins avec un élément inaccessible au milieu](images/designing-for-tv/2d-navigation-best-practices-ui-layout-to-avoid.png)

Si, pour une raison quelconque, il n’est pas possible de réorganiser l’interface utilisateur, utilisez l’une des techniques décrites dans la section suivante pour remplacer le comportement de focus par défaut.

### Remplacement de la navigation par défaut <a name="overriding-the-default-navigation"></a>

Pendant que la plateforme UWP tente de garantir la convivialité de la navigation par bouton multidirectionnel/stick analogique gauche, elle ne peut cependant pas garantir un comportement optimisé pour les intentions de votre application. 
Pour vous assurer que la navigation est optimisée pour votre application, la meilleure solution consiste à la tester avec un boîtier de commande et de vérifier que tous les éléments d’interface utilisateur sont accessibles par l’utilisateur, d’une manière qui soit pertinente pour les scénarios de votre application. Si les scénarios de votre application nécessitent un comportement ne pouvant être obtenu par le biais de la navigation en mode focus XY, envisagez de suivre les conseils indiqués dans les sections suivantes et/ou de remplacer le comportement afin de placer le focus sur un élément logique.

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

Dans ce cas, lorsque le focus est sur le bouton `Home` et que l’utilisateur navigue vers la gauche, le focus se déplace sur le bouton `MyBtnLeft` ; si l’utilisateur navigue vers la droite, le focus se déplace sur le bouton `MyBtnRight` et ainsi de suite.

Pour empêcher le focus de se déplacer dans une certaine direction à partir d’un contrôle , utilisez la propriété `XYFocus*` pour pointer sur ce même contrôle :

```xml
<Button Name="HomeButton"  
        Content="Home"  
        XYFocusLeft ="{x:Bind HomeButton}" />
```

### Chemin nécessitant le moins de clics <a name="path-of-least-clicks"></a>

Permettez à l’utilisateur d’effectuer les tâches les plus courantes avec le moins de clics possible. Dans l’exemple suivant, la classe [TextBlock](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) est placée entre le bouton **Lecture** (qui a initialement le focus) et un élément couramment utilisé ; un élément inutile est ainsi placé entre les tâches prioritaires.

![Meilleures pratiques en matière de navigation nécessitant le moins de clics](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks.png)

Dans l’exemple suivant, la classe [TextBlock](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) est placée au-dessus du bouton **Lecture** à la place. 
Le simple fait de réorganiser l’interface utilisateur afin que les éléments inutiles ne soient pas placés entre les tâches prioritaires améliore considérablement la facilité d’utilisation de votre application.

![TextBlock déplacée au-dessus du bouton de lecture afin qu’elle ne soit plus entre les tâches prioritaires](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks-2.png)

<!--### Nested UI elements

When UI elements are nested inside other UI elements, the default behavior is that the user will not be able to access the nested UI elements.

One of the main scenarios is when there is UI that displays when the user hovers over a nested UI element with the mouse, but does not display otherwise.

![UI elements displaying when mouse hovers over them](images/designing-for-tv/2d-navigation-best-practices-ui-elements-display-on-mouse-hover.png)

The recommended way to handle this scenario for gamepad/remote input is to place these UI elements in a `ContextFlyout` that displays when the user presses the **Menu** button.
-->
<!--#### UI elements that always display

The second scenario is when there is UI that always displays. **TODO: Fill in this section when I get more info**
-->

### CommandBar et ContextFlyout

Lorsque vous utilisez une [CommandBar](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx), n’oubliez pas le problème de défilement dans une liste, comme mentionné dans [Problème : éléments d’interface utilisateur localisés suite à un long défilement dans une liste/grille](#problem-ui-elements-located-after-long-scrolling-list-grid). L’image suivante illustre une disposition d’interface utilisateur avec la `CommandBar` en bas d’une liste/grille. L’utilisateur devrait faire défiler toute la liste/grille pour atteindre la `CommandBar`.

![CommandBar en bas de la liste/grille](images/designing-for-tv/2d-navigation-best-practices-commandbar-and-contextflyout.png)

Que se passe-t-il si vous placez `CommandBar`*au-dessus* de la liste/grille ? Bien qu’un utilisateur ayant effectué un défilement vers le bas de la liste/grille doive en refaire un vers le haut pour atteindre la `CommandBar`, cette navigation est un plus courte que la configuration précédente. On suppose ici que le focus initial de votre application est placé à côté ou au-dessus de la `CommandBar` ; cette approche ne fonctionne pas aussi bien si le focus initial se trouve sous la liste/grille. Si ces éléments `CommandBar` sont des éléments d’action globale auxquels l’utilisateur n’accède que rarement (tels qu’un bouton **Synchronisation**), le fait de les disposer au-dessus de la liste/grille reste acceptable.

Si votre application dispose d’une `CommandBar` dont les éléments doivent être facilement accessibles par les utilisateurs, vous devrez peut-être placer ces éléments à l’intérieur d’un `ContextFlyout` et les supprimer de la `CommandBar`. 

<!--The `ContextFlyout` can be accessed by pressing the **Menu** button, providing a very convenient way for the user to access these actions quickly.-->

Bien qu’il soit impossible d’empiler les éléments d’une `CommandBar` verticalement, le fait de les placer en face du défilement (par exemple, à gauche ou à droite d’une liste avec défilement vertical, ou en haut ou en bas d’une liste avec défilement horizontal) constitue une autre option que vous devriez peut-être prendre en compte si elle fonctionne bien pour la disposition de votre interface utilisateur.

<!--### Navigation pane
A navigation pane (also known as a *hamburger menu*) is a navigation control commonly used in UWP apps. Typically it is a pane with several options to choose from in a list-style menu that will take the user to different pages. Generally this pane starts out collapsed to save space, and the user can open it by clicking on a button.
While nav panes are very accessible with mouse and touch, gamepad/remote makes them less accessible since the user has to navigate to a button to open the pane. Therefore, a good practice is to have the **Menu** button open the nav pane, as well as allow the user to open it by navigating all the way to the left of the page. This will provide the user with very easy access to the contents of the pane. See [Nav panes](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/nav-pane). TO DO: Is this the right link?--> 
<!--TODO: Image of nav pane-->

### Défis en matière de disposition de l’interface utilisateur

Certaines dispositions d’interface utilisateur posent plus de défis en raison de la navigation en mode focus XY et doivent être évaluées au cas par cas. Bien qu’il n’existe pas de méthode « absolue » et que la solution que vous choisissez dépend des besoins spécifiques de votre application, certaines techniques permettent de créer une excellente expérience TV.

Pour mieux comprendre ce point, examinons un exemple d’application qui illustre certains de ces problèmes et les techniques permettant de les résoudre.

> **Remarque** Cet exemple d’application sert à illustrer des problèmes d’interface utilisateur et leurs solutions potentielles et ne présente pas la meilleure expérience utilisateur possible de votre application.

Ce qui suit est un exemple d’application pour le secteur immobilier qui affiche une liste des maisons disponibles à la vente, une carte, la description des propriétés ainsi que d’autres informations. Cette application pose trois défis que vous pouvez surmonter à l’aide des techniques suivantes :

- [Réorganisation de l’interface utilisateur](#ui-rearrange)
- [Activation du focus](#engagement)
- [Mode souris](#mouse-mode)

![Exemple d’application pour le secteur immobilier](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app.png)

#### Problème : éléments d’interface utilisateur localisés suite à un long défilement dans une liste/grille <a name="problem-ui-elements-located-after-long-scrolling-list-grid"></a>

La [ListView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.listview.aspx) des propriétés montrées sur l’image suivante est une très longue liste à défilement. Si l’[activation](#focus-engagement) n’est *pas* requise pour la `ListView`, le focus sera placé sur le premier élément de la liste lorsque l’utilisateur navigue vers cette dernière. L’utilisateur doit parcourir tous les éléments de la liste pour atteindre le bouton **Précédent** ou **Suivant**. Dans ces cas peu pratiques où l’utilisateur doit parcourir toute la liste (c’est-à-dire, lorsque la liste est trop longue pour que cette expérience soit acceptable), vous devriez envisager d’autres options.

![Application pour le secteur immobilier : une liste comprenant 50 éléments nécessite 51 clics pour atteindre les boutons du bas.](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app-list.png)

#### Solutions

##### Réorganisation de l’interface utilisateur <a name="ui-rearrange"></a>

À moins que le focus initial soit placé au bas de la page, les éléments d’interface utilisateur placés au-dessus d’une liste à défilement longue sont en général plus facilement accessibles que s’ils étaient placés en bas. 
Si cette nouvelle disposition fonctionne pour d’autres appareils, il serait moins coûteux en ressources de modifier la disposition pour toutes les familles d’appareils au lieu d’apporter des modifications d’interface utilisateur spécifiques pour Xbox One. 
En outre, le fait de placer des éléments d’interface utilisateur à l’opposé du sens de défilement (autrement dit, horizontalement pour une liste à défilement vertical ou verticalement pour une liste à défilement horizontal) améliorera davantage l’accessibilité.

![Application pour le secteur immobilier : placement des boutons au-dessus d’une liste à défilement longue](images/designing-for-tv/2d-focus-navigation-and-interaction-ui-rearrange.png)

##### Activation du focus <a name="engagement"></a>

Lorsqu’une activation est *requise*, la totalité de la `ListView` devient une cible du focus unique. L’utilisateur sera en mesure d’ignorer le contenu de la liste afin d’accéder au prochain élément pouvant être actif. Pour en savoir plus sur les contrôles prenant en charge l’activation et comment les utiliser, consultez la section [Activation du focus](#focus-engagement).

![Application pour le secteur immobilier : définition de l’activation sur Requis pour atteindre les boutons Précédent/Suivant en un clic](images/designing-for-tv/2d-focus-navigation-and-interaction-engagement.png)

#### Problème : ScrollViewer sans élément pouvant être actif

Étant donné que la navigation en mode focus XY dépend de la navigation vers un seul élément d’interface utilisateur pouvant être actif à la fois, 
un [ScrollViewer](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) ne contenant aucun élément pouvant être actif (par exemple, contenant seulement du texte), peut provoquer un scénario dans lequel l’utilisateur n’est pas en mesure d’afficher l’ensemble du contenu dans `ScrollViewer`. 
Pour connaître les solutions de ce scénario et des scénarios connexes, consultez la section [Activation du focus](#focus-engagement).

![Application pour le secteur immobilier : ScrollViewer contenant uniquement du texte](images/designing-for-tv/2d-focus-navigation-and-interaction-scrollviewer.png)

#### Problème : interface utilisateur à défilement libre

Si votre application nécessite une interface utilisateur à défilement libre, telle qu’une surface de dessin ou, dans l’exemple présent, une carte, la navigation en mode focus XY ne fonctionne simplement pas. 
Dans ce cas, vous pouvez activer le [mode souris](#mouse-mode) pour permettre à l’utilisateur de naviguer librement à l’intérieur d’un élément d’interface utilisateur.

![Carte dans un élément d’interface utilisateur avec le mode souris](images/designing-for-tv/map-mouse-mode.png)

<!--## 2D navigation best practices

Since 2D navigation only lets the user navigate up, down, left, and right, but not diagonally, certain UI layouts work better than others. This section describes some common issues related to navigation and their recommended solutions. Please note that there is no single "right" way to solve these problems, and always think about how to solve them for your app's specific scenarios.

### UI layouts to avoid

The following diagram illustrates an example of the kind of UI layout that 2D navigation doesn't support. Note that the element in the middle is not accessible using gamepad/remote because the vertical and horizontal navigation will be prioritized and the middle element will never be high enough priority to get focus.

![Elements in four corners with inaccessible element in middle](images/designing-for-tv/2d-navigation-best-practices-ui-layout-to-avoid.png)

If for some reason rearranging the UI is not possible, use one of the techniques discussed in [Overriding the default navigation](#overriding-the-default-navigation) to override the default focus behavior.

### CommandBar and ContextFlyout

When using a [CommandBar](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx), keep in mind the issue of scrolling through a list as mentioned in [Problem: UI elements located after long scrolling list/grid](#problem-ui-elements-located-after-long-scrolling-list-grid). The following image shows a UI layout with the `CommandBar` on the bottom of a list/grid. The user would need to scroll all the way down through the list/grid to reach the `CommandBar`.

![CommandBar at bottom of list/grid](images/designing-for-tv/2d-navigation-best-practices-commandbar-and-contextflyout.png)

Now compare this to the following configuration, which puts the `CommandBar` *above* the list/grid. While a user who scrolled down the list/grid would have to scroll back up to reach the `CommandBar`, it is slightly less navigation than the previous configuration. Note that this is assuming that your app's initial focus is placed next to or above the `CommandBar`; this approach won't work as well if the initial focus is below the list/grid. If these `CommandBar` items are global action items that don't have to be accessed very often (such as a **Sync** button), it may be acceptable to have them above the list/grid as in this example.

![CommandBar above list/grid](images/designing-for-tv/2d-navigation-best-practices-commandbar-and-contextflyout-2.png)

If your app has a `CommandBar` whose items need to be readily accessible by users, you may want to consider placing these items inside a `ContextFlyout` and removing them from the `CommandBar`. The `ContextFlyout` can be accessed by pressing the **Menu** button, providing a very convenient way for the user to access these actions quickly.

While you can't stack a `CommandBar`'s items vertically, placing them against the scroll direction (for example, to the left or right of a vertically scrolling list, or the top or bottom of a horizontally scrolling list) is another option you may want to consider if it works well for your UI layout.

### Path of least clicks <a name="path-of-least-clicks"></a>

Try to allow the user to perform the most common tasks in the least number of clicks. In the following example, there is a [TextBlock](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) between the **Play** button and a commonly used element, adding an extra click to get from the initially focused element to the commonly used element.

![TextBlock adds extra click to get to commonly used element](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks.png)

Simply rearranging the UI so that unnecessary elements are not placed in between priority tasks will greatly improve your app's usability.

![TextBlock moved above Play button so that it is no longer between priority tasks](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks-2.png)

### Nested UI elements

When UI elements are nested inside other UI elements, the default behavior is that the user will not be able to access the nested UI elements. There are two main scenarios involving nested UI, and the solutions for them are slightly different.

#### UI elements that display on mouse hover

The first scenario is when there is UI that displays when the user hovers over an element with the mouse, but does not display otherwise.

![UI elements displaying when mouse hovers over them](images/designing-for-tv/2d-navigation-best-practices-ui-elements-display-on-mouse-hover.png)

The recommended way to handle this scenario for gamepad/remote input is to place these UI elements in a `ContextFlyout` that displays when the user presses the **Menu** button.

#### UI elements that always display

The second scenario is when there is UI that always displays. **TODO: Fill in this section when I get more info**

### Navigation pane

A navigation pane (also known as a *hamburger menu*) is a navigation control commonly used in UWP apps. Typically it is a pane with several options to choose from in a list-style menu that will take the user to different pages. Generally this pane starts out collapsed to save space, and the user can open it by clicking on a button.

While nav panes are very accessible with mouse and touch, gamepad/remote makes them less accessible since the user has to navigate to a button to open the pane. Therefore, a good practice is to have the **Menu** button open the nav pane, as well as allow the user to open it by navigating all the way to the left of the page. This will provide the user with very easy access to the contents of the pane. See [Nav panes](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/nav-pane) TODO: Is this the right link? for more information.

TODO: Image of nav pane
-->

## Recommandations en matière de contrôles d’interface utilisateur

La plateforme Windows universelle propose de nombreuses fonctionnalités permettant d’améliorer l’expérience utilisateur sur tous les appareils, dont certaines sont particulièrement bénéfiques à l’expérience « 10-foot ». La liste suivante décrit des exemples de ces améliorations que vous pouvez apporter à l’interface utilisateur de votre application.

### Contrôle Pivot

Le contrôle [Pivot](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.pivot.aspx) possède des propriétés que vous pouvez définir pour éviter le renvoi automatique à la ligne des en-têtes, comme sur téléphone et tablette. Cette expérience est plus intéressante pour les grands écrans tels que les écrans de télévision, car le renvoi à la ligne des en-têtes peut être gênant vis-à-vis des utilisateurs. Pour en savoir plus, voir [Onglets et sélecteurs de vue](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/tabs-pivot)

### Volet de navigation

L’UWP permet d’avoir une apparence cohérente sur tous les appareils. Pour en savoir plus sur la façon dont les volets de navigation se comportent sur des écrans de tailles différentes ainsi que pour connaître les meilleures pratiques en matière de navigation pour le boîtier de commande/télécommande, voir [Volets de navigation](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/nav-pane).

### Libellés CommandBar

Le contrôle [CommandBar](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx) possède une propriété qui affiche en permanence les libellés en regard des icônes. Cela convient pour l’expérience « 10-foot » car elle réduit le nombre de clics nécessaires pour découvrir la fonctionnalité des boutons. C’est également un excellent modèle que les autres types d’appareil peuvent suivre.

### Tooltip

Le contrôle d’info-bulle [Tooltip](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.tooltip.aspx) a été introduit pour fournir à l’utilisateur des informations supplémentaires dans l’interface utilisateur lorsqu’il pointe avec la souris ou maintient son doigt sur un élément. Pour le boîtier de commande et la télécommande, le `Tooltip` s’affiche après un court instant lorsque l’élément obtient le focus, reste à l’écran pendant un court moment, puis disparaît. Ce comportement pourrait devenir gênant si trop de `Tooltip` sont utilisés. Essayez d’éviter le contrôle d’info-bulle Tooltip lors de la conception d’applications pour la télévision.

### Styles de bouton

Bien que les boutons UWP standard fonctionnent correctement sur les télévisions, certains styles visuels attirent mieux l’attention sur l’interface utilisateur. Vous devez prendre cela en compte pour l’ensemble des plateformes, en particulier pour l’expérience « 10-foot » ; elles bénéficient d’une communication claire sur l’emplacement du focus. Pour en savoir plus sur ces styles, voir [Boutons](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/buttons).

<!--### Sort and filter lists and grids

The visual style for sorting and filtering list and grid items on UWP has been updated for 10-foot scenarios. Read more about how to use this UI functionality [here](TODO: Add link).
-->

### Éléments d’interface utilisateur imbriqués

Lorsque des éléments d’interface utilisateur sont imbriqués dans d’autres éléments d’interface utilisateur, le comportement par défaut est que l’utilisateur ne sera pas en mesure d’accéder aux éléments d’interface utilisateur imbriqués.

L’un des principaux scénarios consiste en l’affichage de l’interface utilisateur lorsque l’utilisateur pointe avec la souris sur un élément d’IU imbriqué, mais qui ne s’affiche pas autrement.

![Affichage des éléments d’IU lorsque la souris pointe sur eux](images/designing-for-tv/2d-navigation-best-practices-ui-elements-display-on-mouse-hover.png)

La méthode recommandée pour gérer ce scénario pour les entrées de boîtier de commande/télécommande consiste à placer ces éléments d’interface utilisateur dans un `ContextFlyout`.

<!--[v-lcap] This doc just ends abruptly. I recommended adding a short summary as well as links to related topics, or at least to the parent topic in this set or the next level up-->

<!--### Drag and drop

TODO: Are we including this?
-->


<!--HONumber=Mar16_HO5-->


