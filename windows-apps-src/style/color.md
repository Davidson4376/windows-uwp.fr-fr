---
author: mijacobs
Description: "La couleur permet une orientation intuitive des différents niveaux d’information d’une application et joue un rôle crucial pour renforcer le modèle d’interaction."
title: Color
ms.assetid: 3ba7176f-ac47-498c-80ed-4448edade8ad
label: Color
template: detail.hbs
extraBodyClass: style-color
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 878470a7cbf44862c47a1428a1d25d332db32fdc

---

# Color

La couleur permet une orientation intuitive des différents niveaux d’information d’une application et joue un rôle crucial pour renforcer le modèle d’interaction.

Dans Windows, la couleur est également personnalisable. Les utilisateurs peuvent choisir une couleur et un thème clair ou foncé qui sera conservé tout au long de leur expérience.

## Couleur d’accentuation

L’utilisateur peut sélectionner une seule couleur appelée couleur d’accentuation à partir de *Paramètres&gt; Personnalisation&gt; Couleurs*. Il peut choisir parmi un ensemble de 48nuances de couleur, sauf sur Xbox où la palette inclut 21couleurs adaptées aux écrans de TV.

<!-- Alternate version for the dev center. Need to add hex values. -->
![Couleurs d’accentuation par défaut](images/accentcolorswatch.png) Couleurs d’accentuation par défaut

![Couleurs d’accentuation Xbox](images/accentcolorswatch_xbox.png) Couleurs d’accentuation Xbox



Lorsque les utilisateurs choisissent une couleur d’accentuation, celle-ci devient la couleur du thème du système. Les zones concernées sont l’écran de démarrage, la barre des tâches, le chrome de la fenêtre, les états de l’interaction sélectionnés et des liens hypertexte au sein des [contrôles communs](https://dev.windows.com/design/controls-patterns). Chaque application peut davantage intégrer la couleur d’accentuation par le biais de sa typographie, ses arrière-plans et ses interactions, voire la remplacer pour préserver sa personnalisation spécifique.

## Couleur sur couleur

Une fois qu’une couleur d’accentuation est sélectionnée, des nuances claires et foncées de la couleur en question sont créées en fonction des valeurs HSB de luminosité de la couleur. Les applications peuvent utiliser des variations de nuance pour créer une hiérarchie visuelle et fournir une indication concernant l’interaction.

Par défaut, les liens hypertexte utilisent la couleur d’accentuation de l’utilisateur. Si l’arrière-plan de la page est de couleur similaire, vous pouvez choisir d’affecter une nuance plus claire (ou plus foncée) pour l’accentuation des liens hypertexte, afin d’obtenir un meilleur contraste.

<figure class="figure-img" >
    <img src="images/shades.png" alt="A single accent color with its 6 shades"  />
        <figcaption><p>Les différentes nuances claires/foncées de la couleur d’accentuation par défaut.</p>
</figcaption>
</figure>

<figure class="figure-img" >
    <img src="images/action_center_redline_zoom.png" alt="Redlines for Colored Action Center"  />
        <figcaption><p>Exemple d’application de la logique de couleur à une spécification de conception.</p>
</figcaption>
</figure>

<aside class="aside-dev">
    <div class="aside-dev-title">
    </div>
    <div class="aside-dev-content">
En XAML, la couleur d’accentuation principale est exposée en tant que [ressource de thème](https://msdn.microsoft.com/library/windows/apps/Mt187274.aspx) nommée `SystemAccentColor`. Les nuances sont disponibles en tant que `SystemAccentColorLight3`, `SystemAccentColorLight2`, `SystemAccentColorLight1`, `SystemAccentColorDark1`, `SystemAccentColorDark2` et `SystemAccentColorDark3`. Également disponible par programme via les énumérations [UISettings.GetColorValue](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.uisettings.getcolorvalue.aspx) et [UIColorType](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx).
    </div>
</aside>

## Thèmes de couleur

Les utilisateurs peuvent également choisir entre un thème clair ou un thème foncé pour le système. Certaines applications modifient leur thème selon la préférence de l’utilisateur.

Les applications utilisant le thème clair sont conçues pour des scénarios impliquant des applications de productivité. Par exemple, la suite d’applications disponibles avec MicrosoftOffice. Le thème clair permet une lecture plus aisée de textes longs associée à de longues périodes de travail.

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
Chaque couleur est disponible en tant que [ressource de thème](https://msdn.microsoft.com/library/windows/apps/Mt187274.aspx#the_xaml_color_ramp_and_theme-dependent_brushes) XAML respectant la convention de dénomination `System*Color` (par ex. : `SystemChromeHighColor`). Vous pouvez gérer le thème de votre application, soit par le biais de [Application.RequestedTheme](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.requestedtheme.aspx), soit via [FrameworkElement.RequestedTheme](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.requestedtheme.aspx).
    </div>
</aside>

## Accessibilité

Notre palette est optimisée pour une utilisation sur écran. Pour le texte, nous vous recommandons de conserver un coefficient de contraste de 4,5 pour 1 par rapport à l’arrière-plan par souci de lisibilité optimale. Il existe de nombreux outils gratuits disponibles pour tester si vos choix de couleurs sont adaptés, par exemple [Coefficient de contraste](http://leaverou.github.io/contrast-ratio/).



<!--HONumber=Jun16_HO4-->


