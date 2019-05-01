---
author: knicholasa
description: Z en profondeur ou relative à la profondeur et clichés instantanés sont deux façons d’incorporer la profondeur à votre application pour aider les utilisateurs à se concentrer naturellement et efficacement.
title: Z en profondeur et les clichés instantanés pour les applications UWP
template: detail.hbs
ms.author: nichola
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
pm-contact: chigy
ms.localizationpriority: medium
ms.openlocfilehash: ab49f13d3938e55750ce523f9e0d4ae241304763
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63817706"
---
# <a name="z-depth-and-shadow"></a>Profondeur de tampon et ombre

![Format gif montrant quatre rectangles gris qui sont empilées en diagonale, les unes sur les autres. L’image gif est animé afin que les ombres apparaissent et disparaissent.](images/elevation-shadow/shadow.gif)

Création d’une hiérarchie visuelle des éléments dans votre interface utilisateur facilite l’analyse de l’interface utilisateur et transmet ce qui est important pour vous concentrer sur. Élévation, le fait de mettre la sélection d’éléments de votre interface utilisateur vers l’avant, est souvent utilisée pour obtenir une telle hiérarchie dans le logiciel. Cet article explique comment créer une élévation dans une application UWP à l’aide de la profondeur z et les clichés instantanés.

Profondeur de Z est un terme utilisé entre les créateurs d’application 3D pour désigner la distance entre deux surfaces le long de l’axe z. Il illustre comment fermer un objet est dans l’Observateur. Considérez-la comme un concept similaire à x / y coordonnées, mais dans la direction z.

## <a name="why-use-z-depth"></a>Pourquoi utiliser z en profondeur ?

Dans le monde physique, nous avons tendance à se concentrer sur les objets qui sont plus proches pour nous. Nous pouvons appliquer cet instinct spatial à l’interface utilisateur digital. Par exemple, si vous ajoutez un élément plus proche à l’utilisateur, puis l’utilisateur sera instinctivement vous concentrer sur l’élément. En déplacement éléments d’interface utilisateur plus proche de l’axe z, vous pouvez établir une hiérarchie visuelle entre les objets, aider les utilisateurs à effectuer des tâches naturellement et efficacement dans votre application.

## <a name="what-is-shadow"></a>Nouveautés de clichés instantanés ?

Clichés instantanés est une façon qu'un utilisateur perçoit une élévation. Pour créer une ombre sur l’aire sous lumière au-dessus d’un objet avec élévation de privilèges. Plus l’objet, le plus grand et plus doux l’ombre devient. Les objets avec élévation de privilèges dans votre interface utilisateur n’avez pas besoin d’avoir des ombres, mais ils permettent de créer l’apparence d’élévation.

Dans les applications UWP, ombres doivent être utilisés de façon délibérée, plutôt qu’esthétique. L’utilisation des ombres trop pour réduire ou éliminer la possibilité de vous concentrer l’utilisateur de l’ombre.

Si vous utilisez des contrôles standard, ThemeShadow ombres sont incorporées automatiquement dans votre interface utilisateur. Toutefois, vous pouvez manuellement inclure des ombres dans votre interface utilisateur à l’aide de la ThemeShadow ou les APIs DropShadow. 

## <a name="themeshadow"></a>ThemeShadow

Le type peut être appliqué à n’importe quel élément XAML pour dessiner de ThemeShadow occulte appropriée en fonction de x, y, les coordonnées z. ThemeShadow ajuste aussi automatiquement pour les autres spécifications de l’environnement :

- S’adapte aux modifications de l’éclairage, thème de l’utilisateur, l’environnement de l’application et shell.
- Applique des ombres à des éléments automatiquement en fonction de leur profondeur z. 
- Synchronise éléments lorsqu’ils déplacent et modifier une élévation.
- Conserve les ombres cohérent au sein et entre les applications.

Voici comment ThemeShadow a été implémenté sur un MenuFlyout. MenuFlyout intègre un expérience où la surface principale est élevée à 32 px et chaque menu en cascade supplémentaires est ouvert + 8px au-dessus du menu de que s’ouvre à partir.

![Capture d’écran ThemeShadow appliqué à un MenuFlyout avec trois menus open, imbriquées. Le premier menu est 32 px avec élévation de privilèges et chaque menu suivants qui s’ouvre dans le menu précédent est 8px avec élévation de privilèges plus telle qu’elle laisse une ombre distincte sur l’arrière-plan.](images/elevation-shadow/themeshadow-menuflyout.png)

### <a name="themeshadow-in-common-controls"></a>ThemeShadow contrôle en commun

Les contrôles communs suivants utilisent automatiquement ThemeShadow pour effectuer un cast des ombres à partir de la profondeur de 32 px, sauf indication contraire :

- [Menu contextuel](../controls-and-patterns/menus.md), [barre de commandes](../controls-and-patterns/app-bars.md), [menu volant de la barre de commandes](../controls-and-patterns/command-bar-flyout.md), [barre de menus](../controls-and-patterns/menus.md#create-a-menu-bar)
- [Boîtes de dialogue et menus volants](../controls-and-patterns/dialogs.md) (boîte de dialogue à 64px)
- [NavigationView](../controls-and-patterns/navigationview.md)
- [ComboBox](../controls-and-patterns/combo-box.md), [DropDownButton, SplitButton, ToggleSplitButton](../controls-and-patterns/buttons.md)
- [TeachingTip](../controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)
- [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) 
- [Sélecteurs/Date/heure de calendrier](../controls-and-patterns/date-and-time.md)
- [Info-bulle](../controls-and-patterns/tooltips.md) (16px)
- [Contrôle de transport de supports](../controls-and-patterns/media-playback.md#media-transport-controls), [InkToolbar](../controls-and-patterns/inking-controls.md)
- [Animation connectée](../motion/connected-animation.md)

Remarque: Menus volants s’applique uniquement ThemeShadow lors de la compilation par rapport à Windows 10 version 1903 ou un kit de développement logiciel plus récent.

### <a name="themeshadow-in-popups"></a>ThemeShadow dans les fenêtres contextuelles

Il est souvent le cas que l’interface utilisateur de votre application utilise une fenêtre contextuelle pour les scénarios où vous avez besoin d’attention et l’action rapide de l’utilisateur. Il s’agit d’exemples intéressants lorsque le cliché instantané doit être utilisé pour aider à créer la hiérarchie dans l’interface utilisateur de votre application.

ThemeShadow convertit automatiquement les ombres lorsqu’il est appliqué à un élément XAML dans un [contextuelle](/uwp/api/windows.ui.xaml.controls.primitives.popup). Elle sera convertie ombres sur le contenu de l’application en arrière-plan derrière elle et tous autres ouvrir les menus contextuels en dessous.

Pour utiliser ThemeShadow avec les menus contextuels, utilisez le `Shadow` propriété pour appliquer un ThemeShadow à un élément XAML. Puis, l’élever l’élément à partir d’autres éléments derrière lui, par exemple en utilisant le composant z de la `Translation` propriété.
Pour la plupart des interface utilisateur du menu contextuel, l’élévation par défaut recommandée par rapport à du contenu d’arrière-plan de l’application est 32 pixels efficaces.

Cet exemple montre un Rectangle dans une fenêtre contextuelle projette une ombre sur le contenu d’arrière-plan de l’application et tous les autres menus contextuels derrière lui :

```xaml
<Popup>
    <Rectangle x:Name="PopupRectangle" Fill="Lavender" Height="48" Width="96">
        <Rectangle.Shadow>
            <ThemeShadow />
        </Rectangle.Shadow>
    </Rectangle>
</Popup>
```

```csharp
// Elevate the rectangle by 32px
PopupRectangle.Translation += new Vector3(0, 0, 32);
```

![Une seule contextuelle rectangulaire avec une ombre.](images/elevation-shadow/PopupRectangle.png)

### <a name="disabling-default-themeshadow-on-custom-flyout-controls"></a>La désactivation par défaut le contrôle ThemeShadow sur le menu volant personnalisé

Les contrôles basés sur [Flyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.flyout), [DatePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepickerflyout), [MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.menuflyout) ou [TimePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepickerflyout) automatiquement utiliser ThemeShadow pour convertir un clichés instantanés.

Si les clichés instantanés par défaut ne semble pas correct sur votre contenu du contrôle, puis vous pouvez le désactiver en définissant le [IsDefaultShadowEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flyoutpresenter.isdefaultshadowenabled) propriété `false` sur le FlyoutPresenter associé :

```xaml
<Flyout>
    <Flyout.FlyoutPresenterStyle>
        <Style TargetType="FlyoutPresenter">
            <Setter Property="IsDefaultShadowEnabled" Value="False" />
        </Style>
    </Flyout.FlyoutPresenterStyle>
</Flyout>
```

### <a name="themeshadow-in-other-elements"></a>ThemeShadow dans d’autres éléments

En général nous vous encourageons à réfléchir à votre utilisation des clichés instantanés et limiter son utilisation au cas où il introduit significative hiérarchie d’objets visuel. Toutefois, nous fournissons un moyen de convertir une ombre à partir de n’importe quel élément d’interface utilisateur dans le cas où vous avez scénarios avancés qui nécessitent l’il.

Pour effectuer un cast d’un cliché instantané à partir d’un élément XAML qui n’est pas dans une fenêtre contextuelle, vous devez spécifier explicitement les autres éléments qui peuvent recevoir les clichés instantanés dans le `ThemeShadow.Receivers` collection. Les récepteurs ne peut pas être un ancêtre de la roulettes dans l’arborescence visuelle.

Cet exemple montre deux Rectangles qui ombres dans une grille derrière les portées :

```xaml
<Grid>
    <Grid.Resources>
        <ThemeShadow x:Name="SharedShadow" />
    </Grid.Resources>

    <Grid x:Name="BackgroundGrid" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" />

    <Rectangle x:Name="Rectangle1" Height="100" Width="100" Fill="Turquoise" Shadow="{StaticResource SharedShadow}" />

    <Rectangle x:Name="Rectangle2" Height="100" Width="100" Fill="Turquoise" Shadow="{StaticResource SharedShadow}" />
</Grid>
```

```csharp
/// Add BackgroundGrid as a shadow receiver and elevate the casting buttons above it
SharedShadow.Receivers.Add(BackgroundGrid);

Rectangle1.Translation += new Vector3(0, 0, 16);
Rectangle2.Translation += new Vector3(120, 0, 32);
```

![Deux rectangles turquoise en regard de chacun des autres, tous deux avec des ombres.](images/elevation-shadow/SharedShadow.png)

### <a name="performance-best-practices-for-themeshadow"></a>Meilleures pratiques relatives aux performances ThemeShadow

1. Le système définit une limite de 5 paires roulettes et le destinataire et s’arrête de clichés instantanés, si cette valeur est dépassée. Respecter la limite appliquée par le système de 5 paires roulettes et le destinataire.

2. Limiter le nombre d’éléments de récepteur personnalisée au minimum nécessaire.

3. Si plusieurs éléments receiver sont à l’élévation même, essayez de les combiner en ciblant un élément parent unique à la place.

4. Si plusieurs éléments chargé de convertir le même type d’ombre sur les mêmes éléments de récepteur ajouter l’ombre comme une ressource partagée, puis la réutiliser.

## <a name="drop-shadow"></a>Ombre portée

DropShadow n’est pas automatiquement réactif à son environnement et n’utilise pas de sources de lumière. Par exemple les implémentations, consultez le [DropShadow classe](https://docs.microsoft.com/uwp/api/windows.ui.composition.dropshadow).

## <a name="which-shadow-should-i-use"></a>Les clichés instantanés dois-je utiliser ?

| Propriété | ThemeShadow | DropShadow |
| - | - | - | - |
| **Min SDK** | Windows 10 version 1903 | 14393 |
| **Adaptability** | Oui | Non |
| **Personnalisation** | Non | Oui |
| **Source de lumière** | Automatique (global par défaut, mais vous pouvez remplacer par application) | Aucune |
| **Prise en charge dans les environnements 3D** | Oui | Non |

- N’oubliez pas que l’objectif de clichés instantanés est de fournir une hiérarchie explicite, non comme un simple traitement visual.
- En règle générale, nous vous recommandons d’utiliser ThemeShadow, qui s’adapte automatiquement à son environnement.
- Pour les problèmes sur les performances, limiter le nombre des ombres, utiliser tout autre traitement visual ou utiliser DropShadow.
- Si vous avez des scénarios plus avancés pour atteindre la hiérarchie d’objets visuel, envisagez d’utiliser tout autre traitement visual (par exemple, couleur). Si les clichés instantanés est nécessaire, utilisez DropShadow.
