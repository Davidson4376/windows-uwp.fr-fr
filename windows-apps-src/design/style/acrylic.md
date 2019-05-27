---
description: Un type de pinceau qui crée une texture translucide.
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
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65984261"
---
# <a name="acrylic-material"></a>Matière acrylique

![image hero](images/header-acrylic.svg)

ACRYLIQUE est un type de [pinceau](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Media.Brush) qui crée une texture translucide. Vous pouvez appliquer l'acrylique à des surfaces d’application pour ajouter de la profondeur et créer une hiérarchie visuelle.  <!-- By allowing user-selected wallpaper or colors to shine through, acrylic keeps users in touch with the OS personalization they've chosen. -->

> **API importantes** : [Classe de AcrylicBrush](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.acrylicbrush), [Background, propriété](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control.Background)

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

## <a name="acrylic-and-the-fluent-design-system"></a>Acrylique et Fluent Design System

 Le système de conception Fluent vous aide à créer une interface utilisateur moderne et audacieuse qui incorpore la lumière, la profondeur, le mouvement, les matières et la notion d'échelle. Acrylique est un composant de Fluent Design System qui permet d'ajouter des textures physiques (matières) et de la profondeur à votre application. Pour plus d’informations, voir [Présentation de Fluent Design pour UWP](/windows/apps/fluent-design-system).

 ## <a name="video-summary"></a>Résumé de la vidéo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev002/player]

## <a name="examples"></a>Exemples

:::row:::
    :::column span:::
![Une image](images/XAML-controls-gallery-app-icon.png)
    :::column-end:::
    :::column span="2":::
**Galerie de contrôles XAML**<br>
Si vous avez installé l’application de galerie de contrôles XAML, cliquez sur <a href="xamlcontrolsgallery:/item/Acrylic">ici</a> pour ouvrir l’application et consultez ACRYLIQUE en action.

<a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a><br>
<a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a>
    :::column-end:::
:::row-end:::

## <a name="acrylic-blend-types"></a>Types de lissage acrylique
La caractéristique d’acrylique la plus remarquable est sa transparence. Il existe deux types de lissage acrylique qui modifient ce qui est visible via le support :
 - **Acrylique en arrière-plan** révèle le papier peint du bureau et d'autres fenêtres qui se trouvent derrière l’application active, en ajoutant de la profondeur entre les fenêtres de l’application tout en reflétant les préférences définies par l’utilisateur.
 - **Acrylique dans l'application** ajoute une idée de profondeur dans le cadre de l’application, en fournissant à la fois une mise au point et une hiérarchie.

 ![Acrylique en arrière-plan](images/BackgroundAcrylic_DarkTheme.png)

 ![Acrylique dans l'application](images/AppAcrylic_DarkTheme.png)

 Couche plusieurs surfaces ACRYLIQUES avec précaution : plusieurs couches d’ACRYLIQUE d’arrière-plan peuvent créer perturbante illusions optique.

## <a name="when-to-use-acrylic"></a>Quand utiliser l'acrylique

* Utiliser dans l’application ACRYLIQUE pour prendre en charge l’interface utilisateur, tels que sur les surfaces qui peuvent se chevaucher contenu si le défilement ou interagi avec.
* Utilisez ACRYLIQUE d’arrière-plan pour les éléments d’interface utilisateur temporaires, tels que des menus contextuels, menus volants et l’interface utilisateur dismissable de lumière.<br />À l’aide d’ACRYLIQUE dans les scénarios temporaires permet de maintenir une relation visuelle avec le contenu qui a déclenché l’interface utilisateur temporaire.

Si vous utilisez dans l’application ACRYLIQUE sur des surfaces de navigation, envisager d’étendre le contenu sous le volet ACRYLIQUE pour améliorer le flux sur votre application. À l’aide de NavigationView fait pour vous automatiquement. Toutefois, pour éviter de créer un effet d’entrelacement, essayer de ne pas placer plusieurs éléments de bord à bord ACRYLIQUE - cela peut créer une indésirables à la limite entre les deux surfaces floues. ACRYLIQUE est un outil de proposer visual harmonie à vos conceptions, mais lorsqu’il est utilisé de manière incorrecte, vous risquez bruit visuel.

Prenez en compte les modèles d’utilisation suivantes pour déterminer la meilleure façon d’incorporer ACRYLIQUE dans votre application :

### <a name="horizontal-navigation-or-commanding"></a>Navigation horizontale ou l’exécution de commandes

Si votre application n’est pas en mesure de tirer parti de NavigationView et que vous prévoyez d’ajouter ACRYLIQUE vous-même, nous vous recommandons d’utiliser des ACRYLIQUE relativement translucide avec une opacité de teinte de 60 %.
 - Lorsque le volet s’ouvre sous forme de superposition au-dessus de tout autre contenu d’application, il doit afficher [une acrylique dans l'application à 60 %](#acrylic-theme-resources)

![Application de mappages à l’aide des commandes horizontal dans l’application](images/Maps_In_App_Acrylic_1.png)

En outre, avoir votre contenu étendre ou défilement sous l’ACRYLIQUE en haut vous donne votre application une expérience plus immersif et transparente.

### <a name="vertical-panes"></a>Volets verticales

Pour les volets verticales ou les surfaces qui aident à la section contenu de votre application, nous vous recommandons de qu'utiliser un arrière-plan opaque au lieu d’ACRYLIQUE. Si votre verticales volets s’ouvrent sur le contenu, comme dans NavigationView **Compact** ou **minimale** modes, nous vous conseillons d’utiliser dans l’application ACRYLIQUE pour aider à maintenir le contexte de la page lorsque l’utilisateur a ce volet ouvert.

### <a name="transient-surfaces"></a>Surfaces temporaires

Pour les applications avec les menus volants, menus contextuels non modale, ou lumière-ignorer les volets, il est recommandé d’utiliser ACRYLIQUE d’arrière-plan.

![Modèle d’application de messagerie à l’aide d’un menu volant d’information](images/Mail_TransientContextMenu.png)

La plupart de nos contrôles utilisent ACRYLIQUE par défaut. [MenuFlyouts](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus), [AutoSuggestBox](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/auto-suggest-box), [ComboBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox) et contrôles similaires avec light-faire disparaître les fenêtres contextuelles utiliseront tous l’ACRYLIQUE temporaire lorsqu’elles sont appelées.

> [!Note]
> Rendu des surfaces ACRYLIQUES consomme des GPU, ce qui peut augmenter la consommation d’énergie appareil et raccourcir la durée de la batterie. Effets ACRYLIQUES sont automatiquement désactivés lorsque les appareils entrer en mode économiseur de batterie, et les utilisateurs peuvent désactiver les effets ACRYLIQUES pour toutes les applications, s’ils choisissent.

## <a name="usability-and-adaptability"></a>Convivialité et capacité d'adaptation
L'acrylique adapte automatiquement son apparence à une vaste gamme d’appareils et de contextes.

En mode Contraste élevé, les utilisateurs continuent de voir la couleur d’arrière-plan qu'ils ont choisie, au lieu de l'acrylique. En outre, ACRYLIQUE d’arrière-plan et dans l’application ACRYLIQUE s’affichent une couleur unie :
 - Lorsque l’utilisateur désactive la transparence dans Paramètres > Personnalisation > couleur
 - Lorsque le mode économiseur de batterie est activé
 - Lorsque l’application est exécutée sur un matériel bas de gamme

En outre, uniquement les ACRYLIQUE arrière-plan remplace sa translucidité et texture avec une couleur unie :
 - Lorsqu’une fenêtre d’application sur le bureau est désactivée
 - Lorsque l’application UWP s’exécute sur un téléphone, une Xbox, HoloLens ou dans le mode tablette

### <a name="legibility-considerations"></a>Considérations relatives à la lisibilité
Il est important de s’assurer que tout le texte présenté dans votre application pour les utilisateurs est conforme aux [coefficients de contraste](../accessibility/accessible-text-requirements.md). Nous avons optimisé la recette acrylique afin que le texte noir, blanc ou même gris moyen réponde aux coefficients de contraste par-dessus l'acrylique. Les ressources de thème fournies par la plateforme sont définies par défaut sur un contraste de couleur d'une opacité de 80 %. Lorsque vous placez un corps de texte hautement coloré sur l'acrylique, vous pouvez réduire l’opacité de la teinte tout en assurant la lisibilité. En mode sombre, l'opacité de la teinte peut être de 70 %, tandis que dans un mode plus clair, l'acrylique correspondra à des coefficients de contraste d'une opacité de 50 %.

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
        <td align="center"> 80 % </td>
        <td> ChromeMedium <br/> ChromeLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltHigh <br/><br/> AltLow </td>
    </tr>
    </tr>
        <td> <b>Utilisation recommandée :</b> Il s’agit des ressources acryliques à usage général qui fonctionnent correctement dans un large éventail d’utilisations. Si votre application utilise un texte secondaire de couleur AltMedium avec une taille du texte inférieure à 18px, placez une ressource acrylique à 80 % derrière le texte pour <a href="../accessibility/accessible-text-requirements.md">répondre aux exigences de coefficient de contraste </a>. </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowMediumHighBrush, SystemControlAcrylicElementMediumHighBrush <br/> SystemControlBaseHighAcrylicWindowMediumHighBrush, SystemControlBaseHighAcrylicElementMediumHighBrush </td>
        <td align="center"> 70% </td>
        <td> ChromeMedium <br/><br/> BaseHigh </td>
    </tr>
    <tr>
        <td> <b>Utilisation recommandée :</b> Si votre application utilise le texte secondaire de AltMedium couleur avec une taille de texte de 18px ou plus, vous pouvez placer ces ressources ACRYLIQUE translucides plus de 70 % derrière le texte. Nous vous recommandons d’utiliser ces ressources dans les éléments de navigation horizontale supérieure et dans les zones des éléments de contrôle de votre application.  </td>
    </tr>
    <tr>
        <td> SystemControlChromeHighAcrylicWindowMediumBrush, SystemControlChromeHighAcrylicElementMediumBrush <br/> SystemControlChromeMediumAcrylicWindowMediumBrush, SystemControlChromeMediumAcrylicElementMediumBrush <br/> SystemControlChromeMediumLowAcrylicWindowMediumBrush, SystemControlChromeMediumLowAcrylicElementMediumBrush <br/> SystemControlBaseHighAcrylicWindowMediumBrush, SystemControlBaseHighAcrylicElementMediumBrush <br/> SystemControlBaseMediumLowAcrylicWindowMediumBrush, SystemControlBaseMediumLowAcrylicElementMediumBrush <br/> SystemControlAltMediumLowAcrylicWindowMediumBrush, SystemControlAltMediumLowAcrylicElementMediumBrush  </td>
        <td align="center"> 60 % </td>
        <td> ChromeHigh <br/><br/> ChromeMedium <br/><br/> ChromeMediumLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltMediumLow </td>
    </tr>
    <tr>
        <td> <b>Utilisation recommandée :</b> Lorsque vous placez uniquement le texte principal de AltHigh couleur sur ACRYLIQUE, votre application peut utiliser ces ressources de 60 %. Nous vous recommandons de peindre le <a href="../controls-and-patterns/navigationview.md">volet de navigation verticale</a> de votre application, autrement dit, le menu de type Hamburger, avec une acrylique de 60 %. </td>
    </tr>
</table>

En plus d’une acrylique de couleur neutre, nous avons également ajouté des ressources qui colorent l'acrylique avec la couleur d'accentuation spécifiée par l’utilisateur. Nous vous recommandons d’utiliser l'acrylique colorée avec parcimonie. Pour les variantes dark1 et dark2 fournies, placez du texte blanc ou de couleur claire cohérent avec la couleur de texte de thème foncé placé sur ces ressources.
<table>
    <tr>
        <th align="center">Ressources clés</th>
        <th align="center">Opacité de la teinte</th>
        <th align="center"><a href="color.md">Couleurs de teinte et de secours</a> </th>
    </tr>
    <tr>
        <td> SystemControlAccentAcrylicWindowAccentMediumHighBrush, SystemControlAccentAcrylicElementAccentMediumHighBrush  </td>
        <td align="center"> 70% </td>
        <td> SystemAccentColor </td>
    </tr>
    <tr>
        <td> SystemControlAccentDark1AcrylicWindowAccentDark1Brush, SystemControlAccentDark1AcrylicElementAccentDark1Brush  </td>
        <td align="center"> 80 % </td>
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
 - **TintColor** : la couleur ou teinte de la couche de superposition. Pensez à spécifier la valeur de couleur RVB et l’opacité de canal alpha.
 - **TintOpacity** : l’opacité de la couche de teinte. Nous recommandons d’opacité de 80 % comme point de départ, bien que différentes couleurs peuvent sembler plus attrayants à d’autres translucencies.
 - **TintLuminosityOpacity**: contrôle la quantité de saturation qui est autorisée à travers l’aire ACRYLIQUE à partir de l’arrière-plan.
 - **BackgroundSource** : l’indicateur permettant de spécifier si vous souhaitez utiliser une acrylique en arrière-plan ou dans l’application.
 - **FallbackColor**: la couleur unie qui remplace ACRYLIQUE dans économiseur de batterie. Pour l'acrylique en arrière-plan, la couleur de secours remplace également l'acrylique lorsque votre application ne se trouve pas dans la fenêtre active du bureau ou lorsque l’application est en cours d’exécution sur le téléphone et sur Xbox.

![Nuances acryliques dans un thème clair](images/CustomAcrylic_Swatches_LightTheme.png)

![Nuances acryliques dans un thème foncé](images/CustomAcrylic_Swatches_DarkTheme.png)

![Opactity luminosité par rapport à l’opacité de la teinte](images/LuminosityVersusTint.png)

Pour ajouter un pinceau acrylique, définissez les trois ressources pour les thèmes à contraste élevé, clair et foncé. Notez qu’en contraste élevé, nous recommandons d’utiliser un SolidColorBrush avec la même x:Key que AcrylicBrush sombre/clair.

> [!Note] 
> Si vous ne spécifiez pas une valeur TintLuminosityOpacity, le système s’ajuste automatiquement sa valeur en fonction de vos TintColor et le TintOpacity.

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

## <a name="extend-acrylic-into-the-title-bar"></a>Étendre l'acrylique dans la barre de titre

Pour donner à la fenêtre de votre application un aspect épuré, vous pouvez utiliser l'acrylique dans la zone de la barre de titre. Cet exemple étend l'acrylique dans la barre de titre en définissant les propriétés [ButtonBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonBackgroundColor) et [ButtonInactiveBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonInactiveBackgroundColor) de l'objet [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar) sur [Colors.Transparent](https://docs.microsoft.com/uwp/api/Windows.UI.Colors.Transparent).

```csharp
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
* Ne placez pas de bureau ACRYLIQUE sur les surfaces d’arrière-plan volumineux de votre application, cela interrompt le modèle mental d’ACRYLIQUE utilisé principalement pour les surfaces temporaires.
* Ne placez pas des acryliques dans l’application et en arrière-plan directement côte à côte pour éviter toute fatigue visuelle au niveau des lignes.
* Ne placez pas plusieurs volets acryliques avec la même teinte et la même opacité côte à côte, car cela entraîne une jointure visible indésirable.
* Ne placez pas du texte coloré sur les surfaces acryliques.

## <a name="how-we-designed-acrylic"></a>Comment nous avons conçu l'acrylique

Nous avons affiné les composants clés de l'acrylique pour définir ses propriétés et son apparence uniques. Nous avons commencé avec translucidité, flou et le bruit pour ajouter profondeur visuelle et dimension à surfaces plats. Nous avons ajouté une couche de mode fusion d'exclusion pour assurer le contraste et la lisibilité de l’interface utilisateur placée sur un arrière-plan acrylique. Enfin, nous avons ajouté des teintes de couleur pour permettre la personnalisation. De concert, ces couches viennent compléter un support pratique et moderne.

![ACRYLIQUE Recipe (Recette)](images/AcrylicRecipe_Diagram.jpg)
<br/>La recette de l'acrylique : arrière-plan, flou, fusion d'exclusion, superposition couleur/teinte, bruit


## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) - Affichez tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles connexes

[**Révéler la mise en surbrillance**](reveal.md)
