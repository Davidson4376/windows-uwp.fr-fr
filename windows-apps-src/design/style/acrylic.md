---
description: Type de pinceau qui crée une texture translucide.
title: Matière acrylique
template: detail.hbs
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: rybick
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4731ab089189a8a03656281d1a9a6da6e4d24e89
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65984261"
---
# <a name="acrylic-material"></a>Matière acrylique

![Image Hero](images/header-acrylic.svg)

Acrylique est un type de [pinceau](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Media.Brush) qui produit une texture translucide. Vous pouvez appliquer l’acrylique à des surfaces d’application pour ajouter de la profondeur et établir une hiérarchie visuelle.  <!-- By allowing user-selected wallpaper or colors to shine through, acrylic keeps users in touch with the OS personalization they've chosen. -->

> **API importantes** : [classe AcrylicBrush](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.acrylicbrush), [propriété Background](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control.Background)

:::row:::
    :::column:::
        Acrylic in light theme
        ![Acrylic in light theme](images/Acrylic_LightTheme_Base.png)
    :::column-end:::
    :::column:::
        Acrylic in dark theme
        ![Acrylic in dark theme](images/Acrylic_DarkTheme_Base.png)
    :::column-end:::
:::row-end:::

## <a name="acrylic-and-the-fluent-design-system"></a>Acrylique et le système Fluent Design

 Le système Fluent Design vous aide à créer une interface utilisateur moderne et claire qui incorpore de la lumière, de la profondeur, du mouvement, des matières et une mise à l’échelle. Acrylique est un composant du système Fluent Design qui permet d’ajouter des textures physiques (matières) et de la profondeur à votre application. Pour en savoir plus, voir [Présentation de Fluent Design pour UWP](/windows/apps/fluent-design-system).

 ## <a name="video-summary"></a>Résumé de la vidéo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev002/player]

## <a name="examples"></a>Exemples

:::row:::
    :::column span:::
![Image](images/XAML-controls-gallery-app-icon.png)
    :::column-end:::
    :::column span="2":::
**Galerie de contrôles XAML**<br>
Si vous disposez de l’application Galerie de contrôles XAML, cliquez <a href="xamlcontrolsgallery:/item/Acrylic">ici</a> pour ouvrir l’application et voir Acrylique en action.

<a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a><br>
<a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a>
    :::column-end:::
:::row-end:::

## <a name="acrylic-blend-types"></a>Types de fusions d’acrylique
La caractéristique la plus remarquable de l’acrylique est sa transparence. Il existe deux types de fusions d’acrylique qui modifient ce qui est visible au travers de la matière :
 - **Acrylique en arrière-plan** révèle le papier peint du bureau et d’autres fenêtres qui se trouvent derrière l’application active, en ajoutant de la profondeur entre les fenêtres d’application tout en reflétant les préférences définies par l’utilisateur.
 - **Acrylique dans l’application** ajoute de la profondeur dans le cadre de l’application, en fournissant à la fois une mise au point et une hiérarchie.

 ![Acrylique en arrière-plan](images/BackgroundAcrylic_DarkTheme.png)

 ![Acrylique dans l’application](images/AppAcrylic_DarkTheme.png)

 Lorsque vous appliquez plusieurs couches d’acrylique, agissez avec précaution : plusieurs couches d’acrylique en arrière-plan peuvent créer des illusions d’optique perturbantes.

## <a name="when-to-use-acrylic"></a>Quand utiliser l’acrylique

* Utilisez l’acrylique dans l’application pour traiter l’interface utilisateur, par exemple quand des surfaces qui peuvent chevaucher du contenu lors de l’interaction avec celui-ci.
* Utilisez l’acrylique en arrière-plan pour des éléments d’interface utilisateur temporaires, tels que des menus contextuels, menus volants et autres éléments qui disparaissent par abandon interactif.<br />L’utilisation de l’acrylique dans des scénarios temporaires permet de maintenir une relation visuelle avec le contenu qui a déclenché l’interface utilisateur temporaire.

Si vous utilisez l’acrylique dans l’application sur des surfaces de navigation, envisagez d’étendre le contenu sous le volet acrylique pour améliorer le flux sur votre application. NavigationView fait cela pour vous automatiquement. Toutefois, pour éviter de créer un effet de rayures, évitez de placer plusieurs éléments en acrylique bord à bord. Cela peut créer une sorte de rayure indésirable à la jointure entre deux surfaces floues. L’acrylique vous permet d’harmoniser la présentation visuelle de vos conceptions mais, utilisée de manière incorrecte, il peut générer un bruit visuel.

Pour déterminer la meilleure façon d’incorporer de l’acrylique dans votre application, envisagez les modèles d’utilisation suivants :

### <a name="horizontal-navigation-or-commanding"></a>Navigation ou exécution de commandes horizontale

Si votre application n’est pas en mesure d’utiliser NavigationView et que vous prévoyez d’ajouter de l’acrylique vous-même, nous vous recommandons d’utiliser une acrylique relativement translucide, d’une opacité de 60 %.
 - Lorsque le volet qui s’ouvre se superpose à un autre contenu d’application, il doit afficher [une acrylique dans l’application à 60 %](#acrylic-theme-resources)

![Application Cartes utilisant des commandes horizontales dans l’application](images/Maps_In_App_Acrylic_1.png)

En outre, le fait que votre contenu s’étende ou défile sous l’acrylique permet à votre application d’offrir une expérience plus immersive et transparente.

### <a name="vertical-panes"></a>Volets verticaux

Pour les volets verticaux ou les surfaces qui aident à compartimenter le contenu de votre application, nous vous recommandons d’utiliser un arrière-plan opaque plutôt que l’acrylique. Si vos volets verticaux s’ouvrent par-dessus le contenu, comme dans les modes **Compact** ou **Minimal** de NavigationView, nous vous conseillons d’utiliser l’acrylique dans l’application pour conserver le contexte de la page lorsque ce type de volet est ouvert.

### <a name="transient-surfaces"></a>Surfaces temporaires

Pour les applications utilisant des menus volants, des menus contextuels non modaux, ou des volets qui disparaissent par abandon interactif, il est recommandé d’utiliser l’acrylique en arrière-plan.

![Modèle d’application de courrier utilisant un menu volant informatif](images/Mail_TransientContextMenu.png)

La plupart de nos contrôles utilisent l’acrylique par défaut. Des contrôles tels que [MenuFlyouts](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus), [AutoSuggestBox](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/auto-suggest-box), [ComboBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox) et autres contrôles similaires avec des fenêtres contextuelles disparaissant par abandon interactif utilisent tous l’acrylique temporaire.

> [!Note]
> Le rendu des surfaces acryliques sollicite les ressources du processeur graphique de manière intensive, ce qui peut accroître la consommation d’énergie de l’appareil et réduire l’autonomie de sa batterie. Les effets acryliques sont automatiquement désactivés lorsque les appareils entrent en mode Économiseur de batterie, et les utilisateurs peuvent désactiver les effets acryliques pour toutes les applications s’ils le souhaitent.

## <a name="usability-and-adaptability"></a>Convivialité et capacité d’adaptation
L’acrylique adapte automatiquement son apparence à un vaste éventail d’appareils et de contextes.

En mode Contraste élevé, les utilisateurs continuent de voir la couleur d’arrière-plan qu’ils ont choisie, au lieu de l’acrylique. En outre, l’acrylique en arrière-plan et l’acrylique dans l’application apparaissent toutes deux comme une couleur unie :
 - Lorsque l’utilisateur désactive la transparence dans Paramètres > Personnalisation > Couleur
 - Lorsque le mode Économiseur de batterie est activé
 - Lorsque l’application est exécutée sur du matériel bas de gamme

En outre, seule l’acrylique en arrière-plan remplace sa translucidité et sa texture par une couleur unie :
 - Quand une fenêtre d’application sur le bureau se désactive
 - Lorsque l’application UWP s’exécute sur un téléphone, une Xbox, HoloLens ou en mode tablette

### <a name="legibility-considerations"></a>Considérations relatives à la lisibilité
Il est important de s’assurer que tout le texte présenté aux utilisateurs dans votre application est conforme à des [taux de contraste](../accessibility/accessible-text-requirements.md). Nous avons optimisé la recette acrylique afin que le texte noir, blanc ou même gris moyen respecte les taux de contraste par-dessus l’acrylique. Les ressources de thème fournies par la plateforme sont définies par défaut sur un contraste de couleur d’une opacité de 80 %. Lorsque vous placez un corps de texte hautement coloré sur l’acrylique, vous pouvez réduire l’opacité tout en préservant la lisibilité. En mode sombre, l’opacité de la teinte peut être de 70 %, tandis qu’en mode clair, l’acrylique présente des taux de contraste d’une opacité de 50 %.

Nous vous déconseillons de placer du texte de couleur accentuée sur des surfaces acryliques, car de telles combinaisons sont susceptibles de ne pas satisfaire aux exigences minimales de taux de contraste, avec une taille de texte 15 px. Essayez d’éviter de placer des [liens hypertexte](../controls-and-patterns/hyperlinks.md) sur des éléments acryliques. Par ailleurs, si vous choisissez de personnaliser le niveau de couleur ou d’opacité de l’acrylique en dehors des réglages par défaut de la plateforme fournis par la ressource de thème, gardez à l’esprit l’impact possible sur la lisibilité.

## <a name="acrylic-theme-resources"></a>Ressources de thèmes acryliques
Vous pouvez aisément appliquer l’acrylique sur les surfaces de votre application à l’aide de la nouvelle AcrylicBrush XAML ou des ressources de thème AcrylicBrush prédéfinies. Tout d’abord, vous devez décider si vous souhaitez utiliser l’acrylique dans l’application ou en arrière-plan. Veillez à passer en revue les modèles d’application courants décrits plus haut dans cet article pour obtenir des recommandations.

Nous avons créé une collection de ressources de thème de pinceaux pour les types d’acryliques en arrière-plan et dans l’application qui respectent le thème de l’application et reviennent à des couleurs unies selon les besoins. Les ressources nommées *AcrylicWindow* correspondent à une acrylique en arrière-plan, tandis que les ressources nommées *AcrylicElement* correspondent à une acrylique dans l’application.

<table>
    <tr>
        <th align="center">Ressources clés</th>
        <th align="center">Opacité de la teinte</th>
        <th align="center"><a href="color.md">Couleur de secours</a> </th>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowBrush, SystemControlAcrylicElementBrush <br/> SystemControlChromeLowAcrylicWindowBrush, SystemControlChromeLowAcrylicElementBrush <br/> SystemControlBaseHighAcrylicWindowBrush, SystemControlBaseHighAcrylicElementBrush <br/> SystemControlBaseLowAcrylicWindowBrush, SystemControlBaseLowAcrylicElementBrush <br/> SystemControlAltHighAcrylicWindowBrush, SystemControlAltHighAcrylicElementBrush <br/> SystemControlAltLowAcrylicWindowBrush, SystemControlAltLowAcrylicElementBrush </td>
        <td align="center"> 80 % </td>
        <td> ChromeMedium <br/> ChromeLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltHigh <br/><br/> AltLow </td>
    </tr>
    </tr>
        <td> <b>Utilisation recommandée :</b> il s’agit des ressources acryliques à usage général qui fonctionnent correctement pour de nombreuses utilisations. Si votre application utilise un texte secondaire de couleur AltMedium avec une taille de texte inférieure à 18px, placez une ressource acrylique à 80 % derrière le texte pour <a href="../accessibility/accessible-text-requirements.md">respecter les exigences de taux de contraste</a>. </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowMediumHighBrush, SystemControlAcrylicElementMediumHighBrush <br/> SystemControlBaseHighAcrylicWindowMediumHighBrush, SystemControlBaseHighAcrylicElementMediumHighBrush </td>
        <td align="center"> 70 % </td>
        <td> ChromeMedium <br/><br/> BaseHigh </td>
    </tr>
    <tr>
        <td> <b>Utilisation recommandée :</b> si votre application utilise un texte secondaire de couleur AltMedium avec une taille de texte d’au moins 18px, vous pouvez placer des ressources acryliques à 70 % plus translucides derrière le texte. Nous vous recommandons d’utiliser ces ressources dans les zones de navigation et de commandes horizontales supérieures de votre application.  </td>
    </tr>
    <tr>
        <td> SystemControlChromeHighAcrylicWindowMediumBrush, SystemControlChromeHighAcrylicElementMediumBrush <br/> SystemControlChromeMediumAcrylicWindowMediumBrush, SystemControlChromeMediumAcrylicElementMediumBrush <br/> SystemControlChromeMediumLowAcrylicWindowMediumBrush, SystemControlChromeMediumLowAcrylicElementMediumBrush <br/> SystemControlBaseHighAcrylicWindowMediumBrush, SystemControlBaseHighAcrylicElementMediumBrush <br/> SystemControlBaseMediumLowAcrylicWindowMediumBrush, SystemControlBaseMediumLowAcrylicElementMediumBrush <br/> SystemControlAltMediumLowAcrylicWindowMediumBrush, SystemControlAltMediumLowAcrylicElementMediumBrush  </td>
        <td align="center"> 60 % </td>
        <td> ChromeHigh <br/><br/> ChromeMedium <br/><br/> ChromeMediumLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltMediumLow </td>
    </tr>
    <tr>
        <td> <b>Utilisation recommandée :</b> lorsque vous placez uniquement le texte principal de couleur AltHigh sur l’acrylique, votre application peut utiliser ces ressources à 60 %. Nous vous recommandons de peindre le <a href="../controls-and-patterns/navigationview.md">volet de navigation verticale</a> de votre application, autrement dit, le menu de type Hamburger, avec une acrylique de 60 %. </td>
    </tr>
</table>

En plus d’une acrylique de couleur neutre, nous avons ajouté des ressources qui colorent l’acrylique avec la couleur d’accentuation spécifiée par l’utilisateur. Nous vous recommandons d’utiliser l’acrylique colorée avec parcimonie. Pour les variantes dark1 et dark2 fournies, placez du texte blanc ou de couleur claire cohérent avec la couleur de texte de thème foncé placé sur ces ressources.
<table>
    <tr>
        <th align="center">Ressources clés</th>
        <th align="center">Opacité de la teinte</th>
        <th align="center"><a href="color.md">Teinte et couleurs de secours</a> </th>
    </tr>
    <tr>
        <td> SystemControlAccentAcrylicWindowAccentMediumHighBrush, SystemControlAccentAcrylicElementAccentMediumHighBrush  </td>
        <td align="center"> 70 % </td>
        <td> SystemAccentColor </td>
    </tr>
    <tr>
        <td> SystemControlAccentDark1AcrylicWindowAccentDark1Brush, SystemControlAccentDark1AcrylicElementAccentDark1Brush  </td>
        <td align="center"> 80 % </td>
        <td> SystemAccentColorDark1 </td>
    </tr>
    <tr>
        <td> SystemControlAccentDark2AcrylicWindowAccentDark2MediumHighBrush, SystemControlAccentDark2AcrylicElementAccentDark2MediumHighBrush  </td>
        <td align="center"> 70 % </td>
        <td> SystemAccentColorDark2 </td>
    </tr>
</table>


Pour peindre une surface spécifique, appliquez les ressources de thème ci-dessus aux éléments en arrière-plan comme vous le feriez avec n’importe quelle ressource de pinceau.

```xaml
<Grid Background="{ThemeResource SystemControlAcrylicElementBrush}">
```

## <a name="custom-acrylic-brush"></a>Pinceau acrylique personnalisé
Vous pouvez choisir d’ajouter une teinte à l’acrylique de votre application pour afficher une marque ou assurer un équilibre visuel avec les autres éléments de la page. Pour afficher la couleur plutôt que des nuances de gris, vous devez définir vos propres pinceaux acryliques en utilisant les propriétés suivantes.
 - **TintColor** : couleur ou teinte de la couche de superposition. Pensez à spécifier la valeur de couleur RVB et l’opacité du canal alpha.
 - **TintOpacity** : opacité de la couche de couleur. Nous recommandons de commencer avec une opacité à 80 %, même si des variations de couleur peuvent sembler plus attrayantes que d’autres translucidités.
 - **TintLuminosityOpacity** : contrôle la quantité de saturation de l’arrière-plan autorisée filtrer au travers de la surface acrylique.
 - **BackgroundSource** : indicateur permettant de spécifier si vous souhaitez utiliser l’acrylique en arrière-plan ou dans l’application.
 - **FallbackColor** : couleur unie qui remplace l’acrylique en mode Économiseur de batterie. Pour l’acrylique en arrière-plan, la couleur de secours remplace également l’acrylique lorsque l’application ne se trouve pas dans la fenêtre active du bureau ou quand elle s’exécute sur smartphone ou sur Xbox.

![Nuances d’acrylique dans un thème clair](images/CustomAcrylic_Swatches_LightTheme.png)

![Nuances acrylique dans un thème foncé](images/CustomAcrylic_Swatches_DarkTheme.png)

![Opacité de la luminosité comparée à l’opacité de la teinte](images/LuminosityVersusTint.png)

Pour ajouter un pinceau acrylique, définissez les trois ressources pour les thèmes à contraste élevé, clair et foncé. Notez qu’en contraste élevé, nous recommandons d’utiliser un SolidColorBrush avec la même x:Key que l’AcrylicBrush sombre/clair.

> [!Note] 
> Si vous ne spécifiez pas de valeur pour TintLuminosityOpacity, le système ajuste automatiquement sa valeur en fonction de vos valeurs TintColor et TintOpacity.

```xaml
<ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
        <AcrylicBrush x:Key="MyAcrylicBrush"
            BackgroundSource="HostBackdrop"
            TintColor="#FFFF0000"
            TintOpacity="0.8"
            TintLuminosityOpacity="0.5"
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
            TintLuminosityOpacity="0.5"
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

## <a name="extend-acrylic-into-the-title-bar"></a>Étendre l’acrylique dans la barre de titre

Pour donner à la fenêtre de votre application un aspect épuré, vous pouvez utiliser l’acrylique dans la zone de la barre de titre. Cet exemple étend l’acrylique dans la barre de titre en définissant les propriétés [ButtonBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonBackgroundColor) et [ButtonInactiveBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonInactiveBackgroundColor) de l’objet [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar) sur [Colors.Transparent](https://docs.microsoft.com/uwp/api/Windows.UI.Colors.Transparent).

```csharp
private void ExtendAcrylicIntoTitleBar()
{
    CoreApplication.GetCurrentView().TitleBar.ExtendViewIntoTitleBar = true;
    ApplicationViewTitleBar titleBar = ApplicationView.GetForCurrentView().TitleBar;
    titleBar.ButtonBackgroundColor = Colors.Transparent;
    titleBar.ButtonInactiveBackgroundColor = Colors.Transparent;
}
```

Ce code peut être placé dans la méthode [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) de votre application (_App.xaml.cs_), après l’appel de [Window.Activate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.Activate), comme illustré ici, ou dans la première page de votre application.

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

En outre, vous devez dessiner le titre de votre application, lequel apparaît normalement automatiquement dans la barre de titres, avec un contrôle TextBlock utilisant `CaptionTextBlockStyle`. Pour plus d’informations, voir [Personnalisation de la barre de titre](../shell/title-bar.md).

## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées
* Utilisez l’acrylique en tant que support d’arrière-plan des surfaces des applications non principales, comme les volets de navigation.
* Étendez l’acrylique vers au moins un bord de votre application pour fournir une expérience fluide, en la mélangeant de manière subtile à l’environnement de l’application.
* N’appliquez pas d’acrylique sur de grandes surfaces d’arrière-plan de votre application, car cela irait à l’encontre du modèle mental de l’acrylique utilisée principalement pour des surfaces temporaires.
* Ne placez pas d’acryliques dans l’application et en arrière-plan côte à côte pour éviter toute fatigue visuelle due aux jointures entre ceux-ci.
* Ne placez pas davantage plusieurs volets acryliques de même teinte ou opacité côte à côte, car cela entraîne une jointure visible indésirable.
* Ne placez pas du texte de couleur accentuée sur des surfaces acryliques.

## <a name="how-we-designed-acrylic"></a>Comment nous avons conçu l’acrylique

Nous avons affiné les composants clés de l’acrylique pour obtenir ses propriétés et son apparence uniques. Nous avons commencé par travailler sur la translucidité, le flou et le bruit pour ajouter une profondeur visuelle et une dimension aux surfaces planes. Nous avons ajouté au mode de fusion une couche d’exclusion pour préserver le contraste et la lisibilité de l’interface utilisateur placée sur un arrière-plan acrylique. Enfin, nous avons ajouté des teintes de couleur pour permettre la personnalisation. Ces couches s’additionnent pour constituer un support pratique et moderne.

![Recette de l’acrylique](images/AcrylicRecipe_Diagram.jpg)
<br/>La recette de l’acrylique est la suivante : arrière-plan, flou, fusion d’exclusion, superposition de couleurs/teintes, bruit


## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemple de Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) : tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles connexes

[**Effet Révéler**](reveal.md)
