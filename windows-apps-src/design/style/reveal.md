---
description: Révéler est un effet visuel qui permet d'ajouter de la profondeur et une meilleure mise au point des éléments interactifs de votre application.
title: Principales fonctionnalités de révéler
template: detail.hbs
ms.date: 08/9/2017
ms.topic: article
keywords: windows10, uwp
pm-contact: kisai
design-contact: conrwi
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 8278b126ed209148a2e44ea464e04073dcefc829
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8711907"
---
# <a name="reveal-highlight"></a>Principales fonctionnalités de révéler

![image hero](images/header-reveal-highlight.svg)

Révéler qu'est un effet visuel qui met en évidence les éléments interactifs, tels que les barres de commandes, lorsque l’utilisateur déplace le pointeur à proximité. 

> **API importantes**: [classe RevealBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush), [classe RevealBackgroundBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbackgroundbrush), [classe RevealBorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealborderbrush), [classe RevealBrushHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrushhelper), [classe VisualState](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.VisualState)

## <a name="how-it-works"></a>Principe de fonctionnement
L’effet révéler attire l’attention sur les éléments interactifs en révélant conteneur de l’élément lorsque le pointeur se trouve à proximité, comme illustré dans l’illustration suivante:

![Visuel de l’effet Révéler](images/Nav_Reveal_Animation.gif)

L'outil Révéler expose les bordures masquées qui se trouvent autour des objets, pour permettre aux utilisateurs de mieux comprendre l'espace avec lequel ils interagissent et voir les actions à leur disposition. Cela est particulièrement important dans les contrôles de liste et les groupes de boutons.

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/Reveal">ouvrez l’application et voir Révéler en action </a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>Résumé de la vidéo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev013/player]

## <a name="how-to-use-it"></a>Mode d’utilisation

Révéler fonctionne automatiquement pour certains contrôles. Pour d’autres contrôles, vous pouvez activer révéler en attribuant un style spécial au contrôle, comme décrit dans les sections [Activer révéler sur d’autres contrôles](#enabling-reveal-on-other-controls) et [Activer révéler sur les contrôles personnalisés](#enabling-reveal-on-custom-controls) de cet article.

## <a name="controls-that-automatically-use-reveal"></a>Contrôles utilisant automatiquement Révéler

- [**ListView**](../controls-and-patterns/lists.md)
- [**GridView**](../controls-and-patterns/lists.md)
- [**TreeView**](../controls-and-patterns/tree-view.md)
- [**NavigationView**](../controls-and-patterns/navigationview.md)
- [**MediaTransportControl**](../controls-and-patterns/media-playback.md)
- [**CommandBar**](../controls-and-patterns/app-bars.md)

Ces illustrations montrent révéler mettre en surbrillance sur plusieurs contrôles différents:

![Exemples de Révéler](images/RevealExamples_Collage.png)


## <a name="enabling-reveal-on-other-controls"></a>Activer Révéler sur d’autres contrôles

Si vous avez besoin d'appliquer Révéler pour votre scénario (ces contrôles se trouvent dans le contenu principal et/ou sont utilisés dans des contrôles de liste ou de collecte), nous mettons à votre disposition des styles de ressource qui vous permettent d'activer Révéler dans ces situations.

Ces contrôles n’utilisent pas Révéler par défaut car il s'agit de contrôles mineurs, en général des contrôles annexes aux éléments essentiels de votre application; toutefois, chaque application est différente et si ces contrôles sont ceux qui sont le plus utilisés dans votre application, nous proposons certains styles qui vous seront utiles pour ce faire:

| Nom du contrôle   | Nom de la ressource |
|----------|:-------------:|
| Bouton |  ButtonRevealStyle |
| ToggleButton | ToggleButtonRevealStyle |
| RepeatButton | RepeatButtonRevealStyle |
| AppBarButton | AppBarButtonRevealStyle |
| AppBarToggleButton | AppBarToggleButtonRevealStyle |
| GridViewItem (Révéler le premier plan du contenu) | GridViewItemRevealBackgroundShowsAboveContentStyle |

Pour appliquer ces styles, définissez simplement la propriété [Style](/uwp/api/Windows.UI.Xaml.Style) du contrôle:

```xaml
<Button Content="Button Content" Style="{StaticResource ButtonRevealStyle}"/>
```

### <a name="reveal-in-themes"></a>Révéler dans les thèmes

Révéler change légèrement en fonction du thème demandé pour le contrôle, l’application ou le paramètre utilisateur. Dans le thème foncé, la bordure et les éléments sélectionnés apparaissent en blanc avec Révéler, tandis que dans le thème clair, seules les bordures sont obscurcies en gris clair.

![Révéler dans les thèmes clair et sombre](images/Dark_vs_LightReveal.png)

Pour activer les bordures blanches dans le thème clair, définissez simplement le thème demandé sur le contrôle sur «Dark».

```xaml
<Grid RequestedTheme="Dark">
    <Button Content="Button" Click="Button_Click" Style="{ThemeResource ButtonRevealStyle}"/>
</Grid>
```

Ou remplacez TargetTheme pour RevealBorderBrush par «Dark». Rappel: si TargetTheme est défini sur «Dark», alors l’effet Révéler sera blanc, tandis que s’il est défini sur «Light», les bordures avec Révéler seront grises.

```xaml
 <RevealBorderBrush x:Key="MyLightBorderBrush" TargetTheme="Dark" Color="{ThemeResource SystemAccentColor}" FallbackColor="{ThemeResource SystemAccentColor}" />
```

## <a name="enabling-reveal-on-custom-controls"></a>Activer Révéler sur les contrôles personnalisés

Vous pouvez ajouter l’effet Révéler sur des contrôles personnalisés. Avant de procéder, il est utile de connaître un peu plus sur le fonctionne de l’effet révéler. Révéler est constitué de deux effets distincts: **Révéler la bordure** et **Révéler l'élément sélectionné**.

- **Révéler la bordure** affiche les bordures des éléments interactifs lorsqu’un pointeur se trouve à proximité. Cet effet vous indique que vous pouvez réaliser des actions avec les objets à proximité, similaires à celles de l'élément sélectionné.
- **Révéler l’élément sélectionné** applique un halo léger autour de l’élément pointé ou en focus, et lit une animation par pression lancée d’un clic. 

![Couches de l’effet Révéler](images/RevealLayers.png)

<!-- The Reveal recipe breakdown is:

- Border reveal will be on top of all content but on the designated edges
- Text and content will be displayed directly under Border Reveal
- Hover reveal will be beneath content and text
- The backplate (that turns on and enables Hover Reveal)
- The background (background of control) -->


Ces effets sont définis par deux pinceaux: 
* Révéler la bordure est défini par **RevealBorderBrush**
* Révéler est défini par **RevealBackgroundBrush**

```xaml
<RevealBorderBrush x:Key="MyRevealBorderBrush" TargetTheme="Light" Color="{ThemeResource SystemAccentColor}" FallbackColor="{ThemeResource SystemAccentColor}"/>
<RevealBackgroundBrush x:Key="MyRevealBackgroundBrush" TargetTheme="Light" Color="{StaticResource SystemAccentColor}" FallbackColor="{StaticResource SystemAccentColor}" />
```
Dans la plupart des cas, nous gérons l’utilisation des deux effets en activant Révéler automatiquement pour certains contrôles. Toutefois, d’autres contrôles doivent être activés en appliquant un style ou en modifiant directement leurs modèles.

### <a name="when-to-add-reveal"></a>Quand ajouter Révéler
Vous pouvez ajouter Révéler sur vos contrôles personnalisés, mais avant de le faire, examinez le type de contrôle ainsi que son comportement. 
* Si votre contrôle personnalisé est un élément interactif unique et qu’aucun contrôle similaire ne partage son espace (comme des éléments de menu dans un menu), il est probable que votre contrôle personnalisé n’ait pas besoin de Révéler.  
* Si vous avez un regroupement de contenus ou d’éléments interactifs associés, il est alors probable que cette zone de votre application ait besoin de l’effet Révéler. Cette zone est communément appelée une surface [Commandes](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/collection-commanding).

Par exemple, vous ne devez pas utiliser Révéler sur un bouton seul, mais vous devez l’utiliser sur un ensemble de boutons d’une barre de commandes.

<!-- For example, NavigationView's items are related to page navigation. CommandBar's buttons relate to menu actions or page feature actions. MediaTransportControl's buttons beneath all relate to the media being played. -->

### <a name="using-the-control-template-to-add-reveal"></a>Utiliser le modèle de contrôle pour ajouter Révéler 
Pour activer Révéler sur des contrôles personnalisés ou des contrôles remodélisés, vous devez modifier le modèle de contrôle du contrôle. La plupart des modèles de contrôle ont une grille à la racine; mettez à jour le [VisualState](/uwp/api/windows.ui.xaml.visualstate) de cette grille racine pour utiliser Révéler:

```xaml
<VisualState x:Name="PointerOver">
    <VisualState.Setters>
        <Setter Target="RootGrid.(RevealBrush.State)" Value="PointerOver" />
        <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPointerOver}" />
        <Setter Target="ContentPresenter.BorderBrush" Value="Transparent"/>
        <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPointerOver}" />
    </VisualState.Setters>
</VisualState>
```

Il est important de noter que Révéler a besoin du pinceau et des méthodes setter dans son état visuel pour fonctionner correctement. Il ne suffit pas de définir le pinceau d'un contrôle sur l'une des ressources de pinceaux Révéler pour que Révéler soit activé pour ce contrôle. À l’opposé, n'avoir que les cibles ou les paramètres sans les valeurs des pinceaux Révéler ne suffit pas non plus pour activer cet effet.

Pour en savoir plus sur la modification des modèles de contrôle, voir l’article [Modèles de contrôles XAML](../controls-and-patterns/control-templates.md).

Nous avons créé un ensemble de pinceaux Révéler système que vous pouvez utiliser pour personnaliser votre modèle. Par exemple, vous pouvez utiliser le pinceau **ButtonRevealBackground** pour créer un arrière-plan du bouton personnalisé, ou le pinceau **ListViewItemRevealBackground** pour les listes personnalisées, etc. (Pour en savoir plus sur le fonctionnement des ressources en XAML, consultez l'article [Dictionnaire des ressources XAML](../controls-and-patterns/resourcedictionary-and-xaml-resource-references.md).)

### <a name="full-template-example"></a>Exemple de modèle complet

Voici un modèle complet illustrant l'aspect d'un bouton Révéler:

```xaml
<Style TargetType="Button" x:Key="ButtonStyle1">
    <Setter Property="Background" Value="{ThemeResource ButtonRevealBackground}" />
    <Setter Property="Foreground" Value="{ThemeResource ButtonForeground}" />
    <Setter Property="BorderBrush" Value="{ThemeResource ButtonRevealBorderBrush}" />
    <Setter Property="BorderThickness" Value="2" />
    <Setter Property="Padding" Value="8,4,8,4" />
    <Setter Property="HorizontalAlignment" Value="Left" />
    <Setter Property="VerticalAlignment" Value="Center" />
    <Setter Property="FontFamily" Value="{ThemeResource ContentControlThemeFontFamily}" />
    <Setter Property="FontWeight" Value="Normal" />
    <Setter Property="FontSize" Value="{ThemeResource ControlContentThemeFontSize}" />
    <Setter Property="UseSystemFocusVisuals" Value="True" />
    <Setter Property="FocusVisualMargin" Value="-3" />
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="Button">
                <Grid x:Name="RootGrid" Background="{TemplateBinding Background}">

                    <VisualStateManager.VisualStateGroups>
                        <VisualStateGroup x:Name="CommonStates">
                            <VisualState x:Name="Normal">

                                <Storyboard>
                                    <PointerUpThemeAnimation Storyboard.TargetName="RootGrid" />
                                </Storyboard>
                            </VisualState>

                            <VisualState x:Name="PointerOver">
                                <VisualState.Setters>
                                    <Setter Target="RootGrid.(RevealBrush.State)" Value="PointerOver" />
                                    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPointerOver}" />
                                    <Setter Target="ContentPresenter.BorderBrush" Value="Transparent"/>
                                    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPointerOver}" />
                                </VisualState.Setters>

                                <Storyboard>
                                    <PointerUpThemeAnimation Storyboard.TargetName="RootGrid" />
                                </Storyboard>
                            </VisualState>

                            <VisualState x:Name="Pressed">
                                <VisualState.Setters>
                                    <Setter Target="RootGrid.(RevealBrush.State)" Value="Pressed" />
                                    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPressed}" />
                                    <Setter Target="ContentPresenter.BorderBrush" Value="{ThemeResource ButtonRevealBackgroundPressed}" />
                                    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPressed}" />
                                </VisualState.Setters>

                                <Storyboard>
                                    <PointerDownThemeAnimation Storyboard.TargetName="RootGrid" />
                                </Storyboard>
                            </VisualState>

                            <VisualState x:Name="Disabled">
                                <VisualState.Setters>
                                    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundDisabled}" />
                                    <Setter Target="ContentPresenter.BorderBrush" Value="{ThemeResource ButtonRevealBorderBrushDisabled}" />
                                    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundDisabled}" />
                                </VisualState.Setters>
                            </VisualState>
                        </VisualStateGroup>

              </VisualStateManager.VisualStateGroups>
                    <ContentPresenter x:Name="ContentPresenter"
                    BorderBrush="{TemplateBinding BorderBrush}"
                    BorderThickness="{TemplateBinding BorderThickness}"
                    Content="{TemplateBinding Content}"
                    ContentTransitions="{TemplateBinding ContentTransitions}"
                    ContentTemplate="{TemplateBinding ContentTemplate}"
                    Padding="{TemplateBinding Padding}"
                    HorizontalContentAlignment="{TemplateBinding HorizontalContentAlignment}"
                    VerticalContentAlignment="{TemplateBinding VerticalContentAlignment}"
                    AutomationProperties.AccessibilityView="Raw" />
                </Grid>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

### <a name="fine-tuning-the-reveal-effect-on-a-custom-control"></a>Optimisation de l’effet Révéler sur un contrôle personnalisé 

Lorsque vous activez révéler sur un contrôle personnalisé ou remodélisé ou une surface de commandes personnalisées, les conseils suivants peuvent vous aider à optimiser l’effet:
 
* Pour les éléments adjacents avec des tailles qui ne sont pas alignées en hauteur ni en largeur (en particulier dans les listes): supprimez le comportement d’approche de bordure et laissez les bordures apparaître par pointage uniquement.
* Pour les éléments de commandes qui entrent et sortent fréquemment de l’état désactivé: placez le pinceau d’approche de bordure sur les plaques de fond des éléments ainsi que sur leurs bordures afin de mettre en avant leur état.
* Pour les éléments de commandes adjacents qui sont si proches qu’ils se touchent: ajoutez une marge de 1pixel entre les deuxéléments. 

## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées
### <a name="do"></a>Faire:
- Utilisez Révéler sur des éléments sur lesquels l'utilisateur peut agir (barres de commandes, menus de navigation)
- Utilisez Révéler dans les regroupements d’éléments interactifs qui n’ont pas de séparateurs visuels par défaut (listes, rubans)
- Utilisez Révéler dans les zones à densité élevée d'éléments interactifs (scénarios de commandes)
- Ajoutez une marge de 1pixel entre les éléments Révéler

### <a name="dont"></a>Pratiques déconseillées
- N’utilisez pas Révéler sur le contenu statique (arrière-plans, texte)
- N’utilisez pas Révéler sur les fenêtres contextuelles, les menus volants ou les listes déroulantes
- N’utilisez pas Révéler dans des situations d'isolement ou d'unicité
- N’utilisez pas Révéler sur les éléments de très grande taille (plus de 500epx)
- N’utilisez pas Révéler pour des décisions de sécurité, car cela pourrait distraire l'utilisateur tandis que vous souhaitez lui communiquer un message important


## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.

## <a name="reveal-and-the-fluent-design-system"></a>Révéler et Fluent Design System

 Fluent Design System vous aide à créer une interface utilisateur moderne et audacieuse qui incorpore la lumière, la profondeur, le mouvement, les matières et la notion d'échelle. Révéler est un composant de Fluent Design System qui ajoute de la lumière à votre application. Pour plus d’informations, voir [Présentation de Fluent Design pour UWP](../fluent-design-system/index.md).

## <a name="related-articles"></a>Articles associés

- [Classe RevealBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)
- [Acrylique](acrylic.md)
- [Effets de composition](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
- [Fluent Design pour UWP](../fluent-design-system/index.md)
- [De la science dans le système: Fluent Design et la Profondeur](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [De la science dans le système: Fluent Design et la Lumière](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
