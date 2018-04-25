---
author: mijacobs
description: Un type de pinceau qui crée une texture partiellement transparente.
title: Support acrylique
template: detail.hbs
ms.author: mijacobs
ms.date: 08/9/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: yulikl
design-contact: rybick
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: high
ms.openlocfilehash: 3f42a839f29b13d7245770675b1f3eea84ffa5d5
ms.sourcegitcommit: 2470c6596d67e1f5ca26b44fad56a2f89773e9cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="acrylic-material"></a>Support acrylique

Acrylique est un type de [pinceau](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Media.Brush) qui crée une texture partiellement transparente. Vous pouvez appliquer l'acrylique à des surfaces d’application pour ajouter de la profondeur et créer une hiérarchie visuelle.  <!-- By allowing user-selected wallpaper or colors to shine through, Acrylic keeps users in touch with the OS personalization they've chosen. -->

> **API importantes**: [classe AcrylicBrush](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.acrylicbrush), [propriété Background](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control.Background)


![Acrylique en thème clair](images/Acrylic_DarkTheme_Base.png)

![Acrylique en thème foncé](images/Acrylic_LightTheme_Base.png)

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/Acrylic">ouvrez l’application et voir Acrylique en action </a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>Résumé de la vidéo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev002/player]

## <a name="acrylic-and-the-fluent-design-system"></a>Acrylique et Fluent Design System

 Fluent Design System vous aide à créer une interface utilisateur moderne et audacieuse qui incorpore la lumière, la profondeur, le mouvement, les matières et la notion d'échelle. Acrylique est un composant de Fluent Design System qui permet d'ajouter des textures physiques (matières) et de la profondeur à votre application. Pour plus d’informations, voir [Présentation de Fluent Design pour UWP](../fluent-design-system/index.md).

## <a name="when-to-use-acrylic"></a>Quand utiliser l'acrylique

Nous vous conseillons de placer une interface utilisateur compatible, comme des éléments d'exécution de commandes ou de navigation dans l’application, sur une surface acrylique. Ce support est également utile pour des éléments d’interface utilisateur temporaires, telles que les boîtes de dialogue et les menus volants, car il permet de maintenir une relation visuelle avec le contenu qui déclenche l'apparition de l’interface utilisateur temporaire. Nous avons conçu l'acrylique pour être utilisé en tant que support d'arrière-plan et pour les volets visuellement discrets, par conséquent, ce support ne s'applique pas aux éléments détaillés du premier plan.

Les surfaces qui se trouvent derrière le contenu principal de l’application doivent utiliser des arrière-plans de couleur unie et opaque.

Envisagez d'utiliser l'acrylique pour l'étendre à un ou plusieurs bords de votre application, y compris la barre de titre d'une fenêtre, et ce afin d'améliorer l'aspect visuel. Évitez de créer un effet de répartition en empilant des acryliques de types différents. Acrylique est un outil qui vous permet d’harmoniser la présentation visuelle de vos conceptions. Néanmoins, il peut entraîner un bruit visuel s'il est utilisé de manière incorrecte.

Tenez compte des modèles d’utilisation suivants pour déterminer la meilleure façon d’incorporer l'acrylique dans votre application.

### <a name="vertical-acrylic-pane"></a>Volet acrylique vertical

En ce qui concerne les applications avec navigation verticale, nous vous recommandons d’appliquer l'acrylique au volet secondaire contenant des éléments de navigation.

![Modèle d’application utilisant un seul volet acrylique vertical](images/acrylic_app-pattern_vertical.png)

[NavigationView](../controls-and-patterns/navigationview.md) est une nouvelle commande commune que vous pouvez ajouter à la navigation de votre application et qui inclut l'acrylique dans sa conception visuelle. Le volet de NavigationView affiche l'acrylique en arrière-plan lorsque le volet est ouvert côte à côte avec le contenu principal, et passe automatiquement à l'acrylique dans l’application lorsque le volet est ouvert sous forme de superposition.

Si votre application n’est pas en mesure d'utiliser NavigationView et que vous prévoyez d’ajouter l'acrylique vous-même, nous vous recommandons d’utiliser une acrylique relativement transparente, avec une opacité de 60%.
 - Lorsque le volet s’ouvre sous forme de superposition au-dessus de tout autre contenu d’application, il doit afficher [une acrylique dans l'application à 60%](#acrylic-theme-resources)
 - Lorsque le volet s’ouvre côte à côte avec le contenu de l’application principale, il doit afficher [une acrylique en arrière-plan de 60%](#acrylic-theme-resources)

### <a name="multiple-acrylic-panes"></a>Volets acryliques multiples

Pour les applications contenant trois volets verticaux distincts, nous vous recommandons d’ajouter des acryliques au contenu non principal.
 - Pour le volet secondaire le plus proche du contenu principal, utilisez une [acrylique en arrière-plan de 80%](#acrylic-theme-resources)
 - Pour le troisième volet plus éloigné du contenu principal, utilisez une [acrylique en arrière-plan de 60%](#acrylic-theme-resources)

![Modèle d’application à deux volets verticaux acryliques](images/acrylic_app-pattern_double-vertical.png)

### <a name="horizontal-acrylic-pane"></a>Volet acrylique horizontal

Pour les applications dotées d'une navigation horizontale, ou pour les commandes ou d’autres éléments horizontaux situés sur la partie supérieure de l’application, nous vous recommandons d’appliquer une [acrylique à 70%](#acrylic-theme-resources) à cet élément visuel.

![Modèle d’application utilisant un volet acrylique horizontal](images/acrylic_app-pattern_horizontal.png)

Les applications avec canevas, qui fournissent du contenu en continu sur lequel il est possible de zoomer, doivent utiliser l'acrylique dans l’application dans la barre supérieure pour permettre aux utilisateurs d'interagir avec ce contenu. Les applications avec canevas sont par exemple les applications de plans, de peintures et de dessin.

En ce qui concerne les applications n'utilisant pas de canevas en continu, nous recommandons d’utiliser un arrière-plan acrylique afin de connecter les utilisateurs avec leur environnement de bureau général.

### <a name="acrylic-in-utility-apps"></a>Acrylique dans les applications utilitaires

Il est possible de renforcer l'utilisation en tant qu'applications utilitaires des widgets ou des applications légères en dessinant un bord à bord acrylique à l’intérieur de la fenêtre de l’application. Les applications appartenant à cette catégorie sont généralement associées à de courtes durées d'implication de la part des utilisateurs et il est peu probable qu'elles occupent la totalité de l'écran du bureau. La calculatrice et le centre de notification en sont des exemples.

![L'application utilitaire Calculatrice avec l'arrière-plan totalement en acrylique](images/acrylic_app-pattern_full.png)

> [!Note]
> Le rendu des surfaces acryliques sollicite les ressources du processeur graphique de manière intensive, ce qui peut accroître la consommation d’énergie de l'appareil et réduire l’autonomie de sa batterie. Les effets acryliques sont automatiquement désactivés lorsque les périphériques entrent en mode Économiseur de batterie et les utilisateurs peuvent désactiver les effets acryliques pour toutes les applications s’ils le souhaitent.


## <a name="acrylic-blend-types"></a>Types de lissage acrylique
La caractéristique d’acrylique la plus remarquable est sa transparence. Il existe deux types de lissage acrylique qui modifient ce qui est visible via le support:
 - **Acrylique en arrière-plan** révèle le papier peint du bureau et d'autres fenêtres qui se trouvent derrière l’application active, en ajoutant de la profondeur entre les fenêtres de l’application tout en reflétant les préférences définies par l’utilisateur.
 - **Acrylique dans l'application** ajoute une idée de profondeur dans le cadre de l’application, en fournissant à la fois une mise au point et une hiérarchie.

 ![Acrylique en arrière-plan](images/BackgroundAcrylic_DarkTheme.png)

 ![Acrylique dans l'application](images/AppAcrylic_DarkTheme.png)

 Superposez les surfaces d'acrylique avec précaution. L'acrylique en arrière-plan, comme son nom l’indique, ne doit pas être la couche la plus proche de l’utilisateur en ordre de plan. De multiples couches acryliques en arrière-plan risquent de provoquer des illusions optiques inattendues et doivent également être évitées. Si vous choisissez de superposer les couches d'acrylique, faites-le avec l'acrylique dans l'application et pensez à utiliser une teinte plus claire en termes de valeur afin de ramener visuellement les couches en avant pour l'utilisateur.


## <a name="usability-and-adaptability"></a>Convivialité et capacité d'adaptation
L'acrylique adapte automatiquement son apparence à une vaste gamme d’appareils et de contextes.

En mode Contraste élevé, les utilisateurs continuent de voir la couleur d’arrière-plan qu'ils ont choisie, au lieu de l'acrylique. En outre, l'acrylique en arrière-plan et l'acrylique dans l’application apparaissent toutes deux comme une couleur unie
 - Lorsque l’utilisateur désactive la transparence dans les paramètres de personnalisation
 - Lorsque le mode économiseur de batterie est activé
 - Lorsque l’application est exécutée sur un matériel bas de gamme

En outre, seule l'acrylique en arrière-plan remplacera sa transparence et sa texture par une couleur unie
 - Lorsqu’une fenêtre d’application sur le bureau est désactivée
 - Lorsque l’application UWP s’exécute sur un téléphone, une Xbox, HoloLens ou dans le mode tablette

### <a name="legibility-considerations"></a>Considérations relatives à la lisibilité
Il est important de s’assurer que tout le texte présenté dans votre application pour les utilisateurs est conforme aux [coefficients de contraste](../accessibility/accessible-text-requirements.md). Nous avons optimisé la recette acrylique afin que le texte noir, blanc ou même gris moyen réponde aux coefficients de contraste par-dessus l'acrylique. Les ressources de thème fournies par la plateforme sont définies par défaut sur un contraste de couleur d'une opacité de 80%. Lorsque vous placez un corps de texte hautement coloré sur l'acrylique, vous pouvez réduire l’opacité de la teinte tout en assurant la lisibilité. En mode sombre, l'opacité de la teinte peut être de 70%, tandis que dans un mode plus clair, l'acrylique correspondra à des coefficients de contraste d'une opacité de 50%.

Nous vous déconseillons de placer du texte d'une couleur distinctive sur les surfaces acryliques, car ces combinaisons suivantes sont susceptibles de ne pas satisfaire aux exigences minimales de coefficient de contraste, avec une taille de texte 15 px. Essayez d’éviter de placer des [liens hypertexte](../controls-and-patterns/hyperlinks.md) sur les éléments acryliques. En outre, si vous choisissez de personnaliser le niveau de couleur ou l’opacité de teinte de l'acrylique en dehors de la plateforme par défaut fournie par la ressource de thème, gardez à l'esprit l’impact possible sur la lisibilité.

## <a name="acrylic-theme-resources"></a>Ressources de thèmes acryliques
Vous pouvez aisément appliquer l'acrylique sur les surfaces de votre application à l’aide de la nouvelle AcrylicBrush XAML ou des ressources de thème AcrylicBrush prédéfinies. Tout d’abord, vous devez décider si vous souhaitez utiliser l'acrylique dans l’application ou en arrière-plan. Veillez à passer en revue les modèles d’application courants décrits précédemment dans cet article pour obtenir des recommandations.

Nous avons créé une collection de ressources de thème de pinceaux pour les types acryliques en arrière-plan et dans l’application qui respectent le thème de l’application et reviennent à des couleurs unies selon les besoins. Les ressources nommées *AcrylicWindow* représentent une acrylique en arrière-plan, tandis que *AcrylicElement* fait référence à une acrylique dans l’application.

<table>
    <tr>
        <th align="center">Ressources clés</th>
        <th align="center">Opacité de la teinte</th>
        <th align="center"><a href="color.md">Couleur de secours</a> </th>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowBrush, SystemControlAcrylicElementBrush <br/> SystemControlChromeLowAcrylicWindowBrush, SystemControlChromeLowAcrylicElementBrush <br/> SystemControlBaseHighAcrylicWindowBrush, SystemControlBaseHighAcrylicElementBrush <br/> SystemControlBaseLowAcrylicWindowBrush, SystemControlBaseLowAcrylicElementBrush <br/> SystemControlAltHighAcrylicWindowBrush, SystemControlAltHighAcrylicElementBrush <br/> SystemControlAltLowAcrylicWindowBrush, SystemControlAltLowAcrylicElementBrush </td>
        <td align="center"> 80% </td>
        <td> ChromeMedium <br/> ChromeLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltHigh <br/><br/> AltLow </td>
    </tr>
    </tr>
        <td> <b>Utilisation recommandée:</b> il s’agit des ressources acryliques à usage général qui fonctionnent correctement pour de nombreuses utilisations. Si votre application utilise un texte secondaire de couleur AltMedium avec une taille du texte inférieure à 18px, placez une ressource acrylique à 80% derrière le texte pour <a href="../accessibility/accessible-text-requirements.md">répondre aux exigences de coefficient de contraste </a>. </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowMediumHighBrush, SystemControlAcrylicElementMediumHighBrush <br/> SystemControlBaseHighAcrylicWindowMediumHighBrush, SystemControlBaseHighAcrylicElementMediumHighBrush </td>
        <td align="center"> 70% </td>
        <td> ChromeMedium <br/><br/> BaseHigh </td>
    </tr>
    <tr>
        <td> <b>Utilisation recommandée:</b> si votre application utilise un texte secondaire de couleur AltMedium avec une taille de texte de 18px ou plus, vous pouvez placer des ressources acryliques à 70% plus transparentes derrière le texte. Nous vous recommandons d’utiliser ces ressources dans les éléments de navigation horizontale supérieure et dans les zones des éléments de contrôle de votre application.  </td>
    </tr>
    <tr>
        <td> SystemControlChromeHighAcrylicWindowMediumBrush, SystemControlChromeHighAcrylicElementMediumBrush <br/> SystemControlChromeMediumAcrylicWindowMediumBrush, SystemControlChromeMediumAcrylicElementMediumBrush <br/> SystemControlChromeMediumLowAcrylicWindowMediumBrush, SystemControlChromeMediumLowAcrylicElementMediumBrush <br/> SystemControlBaseHighAcrylicWindowMediumBrush, SystemControlBaseHighAcrylicElementMediumBrush <br/> SystemControlBaseMediumLowAcrylicWindowMediumBrush, SystemControlBaseMediumLowAcrylicElementMediumBrush <br/> SystemControlAltMediumLowAcrylicWindowMediumBrush, SystemControlAltMediumLowAcrylicElementMediumBrush  </td>
        <td align="center"> 60% </td>
        <td> ChromeHigh <br/><br/> ChromeMedium <br/><br/> ChromeMediumLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltMediumLow </td>
    </tr>
    <tr>
        <td> <b>Utilisation recommandée:</b> lorsque vous placez uniquement le texte principal de couleur AltHigh sur l'acrylique, votre application peut utiliser ces ressources à 60%. Nous vous recommandons de peindre le <a href="../controls-and-patterns/navigationview.md">volet de navigation verticale</a> de votre application, autrement dit, le menu de type Hamburger, avec une acrylique de 60%. </td>
    </tr>
</table>

En plus d’une acrylique de couleur neutre, nous avons également ajouté des ressources qui colorent l'acrylique avec la couleur d'accentuation spécifiée par l’utilisateur. Nous vous recommandons d’utiliser l'acrylique colorée avec parcimonie. Pour les variantes dark1 et dark2 fournies, placez du texte blanc ou de couleur claire cohérent avec la couleur de texte de thème foncé placé sur ces ressources.
<table>
    <tr>
        <th align="center">Ressources clés</th>
        <th align="center">Opacité de la teinte</th>
        <th align="center"><a href="color.md">Couleurs et teinte de secours</a> </th>
    </tr>
    <tr>
        <td> SystemControlAccentAcrylicWindowAccentMediumHighBrush, SystemControlAccentAcrylicElementAccentMediumHighBrush  </td>
        <td align="center"> 70% </td>
        <td> SystemAccentColor </td>
    </tr>
    <tr>
        <td> SystemControlAccentDark1AcrylicWindowAccentDark1Brush, SystemControlAccentDark1AcrylicElementAccentDark1Brush  </td>
        <td align="center"> 80% </td>
        <td> SystemAccentColorDark1 </td>
    </tr>
    <tr>
        <td> SystemControlAccentDark2AcrylicWindowAccentDark2MediumHighBrush, SystemControlAccentDark2AcrylicElementAccentDark2MediumHighBrush  </td>
        <td align="center"> 70% </td>
        <td> SystemAccentColorDark2 </td>
    </tr>
</table>


Pour peindre une surface spécifique, appliquez les ressources de thème ci-dessus aux éléments en arrière-plan comme vous le feriez avec n'importe quelle ressource de pinceau.

```xaml
<Grid Background="{ThemeResource SystemControlAcrylicElementBrush}">
```

## <a name="custom-acrylic-brush"></a>Pinceau acrylique personnalisé
Vous pouvez choisir d'ajouter une teinte à l'acrylique de votre application pour afficher une marque ou assurer un équilibre visuel avec les autres éléments de la page. Pour afficher la couleur plutôt que des nuances de gris, vous devez définir vos propres pinceaux acryliques en utilisant les propriétés suivantes.
 - **TintColor**: la couleur ou teinte de la couche de superposition. Pensez à spécifier la valeur de couleur RVB et l’opacité de canal alpha.
 - **TintOpacity**: l’opacité de la couche de teinte. Nous recommandons de commencer avec une opacité à 80%, bien que des variations de couleur puissent sembler plus attrayantes que d'autres niveaux de transparence.
 - **BackgroundSource**: l’indicateur permettant de spécifier si vous souhaitez utiliser une acrylique en arrière-plan ou dans l’application.
 - **FallbackColor**: la couleur unie qui remplace l'acrylique lorsque la batterie est faible. Pour l'acrylique en arrière-plan, la couleur de secours remplace également l'acrylique lorsque votre application ne se trouve pas dans la fenêtre active du bureau ou lorsque l’application est en cours d’exécution sur le téléphone et sur Xbox.


![Nuances acryliques dans un thème clair](images/CustomAcrylic_Swatches_LightTheme.png)

![Nuances acryliques dans un thème foncé](images/CustomAcrylic_Swatches_DarkTheme.png)

Pour ajouter un pinceau acrylique, définissez les trois ressources pour les thèmes à contraste élevé, clair et foncé. Notez qu’en contraste élevé, nous recommandons d’utiliser un SolidColorBrush avec la même x:Key que AcrylicBrush sombre/clair.

```xaml
<ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
        <AcrylicBrush x:Key="MyAcrylicBrush"
            BackgroundSource="HostBackdrop"
            TintColor="#FFFF0000"
            TintOpacity="0.8"
            FallbackColor="#FF7F0000"/>
    </ResourceDictionary>

    <ResourceDictionary x:Key="HighContrast">
        <SolidColorBrush x:Key="MyAcrylicBrush"
            Color="{ThemeResource SystemColorWindowColor}"/>
    </ResourceDictionary>

    <ResourceDictionary x:Key="Light">
        <AcrylicBrush x:Key="MyAcrylicBrush"
            BackgroundSource="HostBackdrop"
            TintColor="#FFFF0000"
            TintOpacity="0.8"
            FallbackColor="#FFFF7F7F"/>
    </ResourceDictionary>
</ResourceDictionary.ThemeDictionaries>
```

L’exemple suivant montre comment déclarer AcrylicBrush dans le code. Si votre application prend en charge plusieurs cibles du système d’exploitation, veillez à vérifier que cette API est disponible sur l’ordinateur de l’utilisateur.

```csharp
if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.UI.Xaml.Media.XamlCompositionBrushBase"))
{
    Windows.UI.Xaml.Media.AcrylicBrush myBrush = new Windows.UI.Xaml.Media.AcrylicBrush();
    myBrush.BackgroundSource = Windows.UI.Xaml.Media.AcrylicBackgroundSource.HostBackdrop;
    myBrush.TintColor = Color.FromArgb(255, 202, 24, 37);
    myBrush.FallbackColor = Color.FromArgb(255, 202, 24, 37);
    myBrush.TintOpacity = 0.6;

    grid.Fill = myBrush;
}
else
{
    SolidColorBrush myBrush = new SolidColorBrush(Color.FromArgb(255, 202, 24, 37));

    grid.Fill = myBrush;
}
```

## <a name="extend-acrylic-into-the-title-bar"></a>Étendre l'acrylique dans la barre de titre

Pour donner à la fenêtre de votre application un aspect épuré, vous pouvez utiliser l'acrylique dans la zone de la barre de titre. Cet exemple étend l'acrylique dans la barre de titre en définissant les propriétés [ButtonBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonBackgroundColor) et [ButtonInactiveBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonInactiveBackgroundColor) de l'objet [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar) sur [Colors.Transparent](https://docs.microsoft.com/uwp/api/Windows.UI.Colors.Transparent). 

```csharp
/// Extend acrylic into the title bar. 
private void ExtendAcrylicIntoTitleBar()
{
    CoreApplication.GetCurrentView().TitleBar.ExtendViewIntoTitleBar = true;
    ApplicationViewTitleBar titleBar = ApplicationView.GetForCurrentView().TitleBar;
    titleBar.ButtonBackgroundColor = Colors.Transparent;
    titleBar.ButtonInactiveBackgroundColor = Colors.Transparent;
}
```

Ce code peut être placé dans la méthode [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) de votre application (_App.xaml.cs_), après l’appel à [Window.Activate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.Activate), comme illustré ici, ou dans la première page de votre application. 


```csharp
// Call your extend acrylic code in the OnLaunched event, after 
// calling Window.Current.Activate.
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();

        rootFrame.NavigationFailed += OnNavigationFailed;

        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }

    if (e.PrelaunchActivated == false)
    {
        if (rootFrame.Content == null)
        {
            // When the navigation stack isn't restored navigate to the first page,
            // configuring the new page by passing required information as a navigation
            // parameter
            rootFrame.Navigate(typeof(MainPage), e.Arguments);
        }
        // Ensure the current window is active
        Window.Current.Activate();

        // Extend acrylic
        ExtendAcrylicIntoTitleBar();
    }
}
```

En outre, vous devrez dessiner le titre de votre application, lequel apparaît normalement automatiquement dans la barre de titres, avec un contrôle TextBlock utilisant `CaptionTextBlockStyle`. Pour plus d’informations, voir [Personnalisation de la barre de titre](../shell/title-bar.md).

## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées
* Utilisez l'acrylique en tant que support d'arrière-plan des surfaces des applications non principales, comme les volets de navigation.
* Étendez l'acrylique vers au moins un bord de votre application pour fournir une expérience fluide, en la mélangeant de manière subtile à l’environnement de l’application.
* Ne placez pas des acryliques dans l’application et en arrière-plan directement côte à côte pour éviter toute fatigue visuelle au niveau des lignes.
* Ne placez pas plusieurs volets acryliques avec la même teinte et la même opacité côte à côte, car cela entraîne une jointure visible indésirable.
* Ne placez pas du texte coloré sur les surfaces acryliques.

## <a name="how-we-designed-acrylic"></a>Comment nous avons conçu l'acrylique

Nous avons affiné les composants clés de l'acrylique pour définir ses propriétés et son apparence uniques. Nous avons commencé par travailler sur la transparence, le flou et le bruit pour ajouter une profondeur visuelle et une dimension aux surfaces planes. Nous avons ajouté une couche de mode fusion d'exclusion pour assurer le contraste et la lisibilité de l’interface utilisateur placée sur un arrière-plan acrylique. Enfin, nous avons ajouté des teintes de couleur pour permettre la personnalisation. De concert, ces couches viennent compléter un support pratique et moderne.

![Recette de l'acrylique](images/AcrylicRecipe_Diagram.png)
<br/>La recette de l'acrylique: arrière-plan, flou, fusion d'exclusion, superposition couleur/teinte, bruit

<!--
<div class="microsoft-internal-note">
When designing your app, please utilize these [design resources](http://uni/DesignDepot.FrontEnd/#/Search?t=Resources%7CNeon%7CToolkit&f=Acrylic%20Material) to show acrylic in comps. The linked templates are the most accurate way to represent acrylic material in Photoshop and Illustrator. The ordering, as noted in the recipe diagram above, should start from the top: <br/>
 - Noise asset (tiled) at 2% opacity <br/>
 - Base color/tint/alpha layer <br/>
 - Exclusion blend (white @ 10% opacity) <br/>
 - Gaussian blur (30px radius) <br/>
</div>
-->

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles associés

[**Principales fonctionnalités de Révéler**](reveal.md)
