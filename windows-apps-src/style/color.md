---
Description: La couleur permet une orientation intuitive des différents niveaux d’information d’une application et joue un rôle crucial pour renforcer le modèle d’interaction.
title: Couleur
ms.assetid: 3ba7176f-ac47-498c-80ed-4448edade8ad
label: Color
template: detail.hbs
extraBodyClass: style-color
brief: Color provides intuitive wayfinding through an app's various levels of information and serves as a crucial tool for reinforcing the interaction model.<br /><br />In Windows, color is also personal. Users can choose a color and a light or dark theme to be reflected throughout their experience.
---

# Couleur pour les applications UWP
La couleur permet une orientation intuitive des différents niveaux d’information d’une application et joue un rôle crucial pour renforcer le modèle d’interaction.

## Couleur d’accentuation

L’utilisateur peut sélectionner une seule couleur, appelée couleur d’accentuation. Il peut choisir parmi 48 échantillons de couleur.


<!-- Alternate version for the dev center. Need to add hex values. -->
<figure>
![Accent colors](images/accentcolorswatch.png)
<figcaption>En règle générale, quand la couleur d’accentuation d’origine est utilisée en tant qu’arrière-plan, placez toujours du texte blanc par-dessus.</figcaption>
</figure>

Lorsque les utilisateurs choisissent une couleur d’accentuation, celle-ci devient la couleur du thème du système. Les zones concernées sont l’écran de démarrage, la barre des tâches, le chrome de la fenêtre, les états de l’interaction sélectionnés et des liens hypertexte au sein des [contrôles communs](https://dev.windows.com/design/controls-patterns). Chaque application peut davantage intégrer la couleur d’accentuation par le biais de sa typographie, ses arrière-plans et ses interactions, voire la remplacer pour préserver sa personnalisation spécifique.

## Sélection de couleur

Une fois qu’une couleur d’accentuation est sélectionnée, des nuances claires et foncées de la couleur en question sont créées en fonction des valeurs de liste de compatibilité matérielle (HCL) de luminosité de la couleur. Les applications peuvent utiliser des variations de nuance pour créer une hiérarchie visuelle et fournir une indication concernant l’interaction.

![Une couleur d’accentuation unique avec ses 6 nuances](images/shades.png)

<aside class="aside-dev">
    <div class="aside-dev-title">
    </div>
    <div class="aside-dev-content">
            In XAML, the accent color is exposed as a [theme resource](https://msdn.microsoft.com/en-us/library/windows/apps/Mt187274.aspx) named `SystemAccentColor`. It's also available programmatically from [UISettings.GetColorValue](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uisettings.getcolorvalue.aspx). You can programmatically access the different shades from [UISettings.GetColorValue](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uisettings.getcolorvalue.aspx), see the [UIColorType](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx) enum.
    </div>
</aside>

## Thèmes de couleur

L’utilisateur peut également choisir entre un thème clair ou foncé pour le système (sur téléphone ; les tablettes et le Bureau ne présentent pas cette option, mais peuvent fournir un paramètre dans l’application). Certaines applications modifient leur thème selon la préférence de l’utilisateur.

Les applications utilisant le thème clair sont conçues pour des scénarios impliquant des applications de productivité. Par exemple, la suite d’applications disponibles avec Microsoft Office. Le thème clair permet une lecture plus aisée de textes longs associée à de longues périodes de travail.

Un thème foncé rend plus visible le contraste des contenus pour les applications multimédias ou dans des scénarios où les utilisateurs ont accès à une quantité importante de vidéos et d’images. Dans ces scénarios, la lecture n’est pas forcément prépondérante, contrairement au visionnage d’un film, dans un environnement peu éclairé.

Si votre application ne correspond à aucune de ces définitions, envisagez de suivre le thème du système pour permettre à l’utilisateur de choisir ce qui lui convient.

Pour faciliter la conception des thèmes, Windows propose une palette de couleurs supplémentaires qui s’adapte automatiquement au thème.


<!-- OP version -->
### Thème clair
#### Base
![Thème clair Base](images/themes-light-base.png)
#### Alt
![Thème clair Alt](images/themes-light-alt.png)
#### List
![Thème clair List](images/themes-light-list.png)
#### Chrome
![Thème clair Chrome](images/themes-light-chrome.png)
### Thème foncé
#### Base
![Thème foncé Base](images/themes-dark-base.png)
#### Alt
![Thème foncé Alt](images/themes-dark-alt.png)
#### List
![Thème foncé List](images/themes-dark-list.png)
#### Chrome
![Thème foncé Chrome](images/themes-dark-chrome.png)


<aside class="aside-dev">
    <div class="aside-dev-title">
    </div>
    <div class="aside-dev-content">
            Each color is available as a XAML [theme resource](https://msdn.microsoft.com/en-us/library/windows/apps/Mt187274.aspx#the_xaml_color_ramp_and_theme-dependent_brushes) that follows the `System*Color` naming convention (ex: `SystemChromeHighColor`). You can control your app's theme through either [Application.RequestedTheme](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.application.requestedtheme.aspx) or [FrameworkElement.RequestedTheme](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.requestedtheme.aspx).
    </div>
</aside>

## Accessibilité

Notre palette est optimisée pour une utilisation sur écran. Nous vous recommandons de conserver un coefficient de contraste minimal de 4,5 pour 1 pour le texte par souci de lisibilité optimale.


<!--HONumber=Mar16_HO5-->


