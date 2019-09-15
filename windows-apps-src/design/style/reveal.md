---
description: Révéler est un effet lumineux qui permet de donner de la profondeur et le focus à des éléments interactifs de votre application.
title: Effet Révéler
template: detail.hbs
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: conrwi
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 0810365eeb0023a31862d31213862e2b3bce8db8
ms.sourcegitcommit: 5687e5340f8d78da95c3ac28304d1c9b8960c47d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70930349"
---
# <a name="reveal-highlight"></a>Effet Révéler

![Image Hero](images/header-reveal-highlight.svg)

Révéler est un effet lumineux qui met en évidence les éléments interactifs, comme les barres de commandes, quand l’utilisateur place le pointeur à leur proximité. 

> **API importantes** : [classe RevealBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush), [classe RevealBackgroundBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbackgroundbrush), [classe RevealBorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealborderbrush), [classe RevealBrushHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrushhelper), [classe VisualState](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.VisualState)

## <a name="how-it-works"></a>Fonctionnement
L’effet Révéler attire l’attention sur des éléments interactifs en révélant le conteneur de l’élément quand le pointeur se trouve à proximité, comme présenté dans l’illustration suivante :

![Visuel de l’effet Révéler](images/Nav_Reveal_Animation.gif)

L’effet Révéler expose les bordures masquées qui se trouvent autour des objets, pour permettre aux utilisateurs de mieux comprendre l’espace avec lequel ils interagissent et voir les actions à leur disposition. Ceci est spécialement important dans les contrôles de liste et les groupes de boutons.

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/Reveal">ouvrez l’application et voir l’effet Révéler en action </a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>Résumé de la vidéo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev013/player]

## <a name="how-to-use-it"></a>Comment l’utiliser

L’effet Révéler fonctionne automatiquement pour certains contrôles. Pour d’autres contrôles, vous pouvez activer l’effet Révéler en affectant un style spécial au contrôle, comme décrit dans les sections [Activer Révéler sur d’autres contrôles](#enabling-reveal-on-other-controls) et [Activer Révéler sur les contrôles personnalisés](#enabling-reveal-on-custom-controls) de cet article.

## <a name="controls-that-automatically-use-reveal"></a>Contrôles utilisant automatiquement Révéler

- [**ListView**](../controls-and-patterns/lists.md)
- [**GridView**](../controls-and-patterns/lists.md)
- [**TreeView**](../controls-and-patterns/tree-view.md)
- [**NavigationView**](../controls-and-patterns/navigationview.md)
- [**MediaTransportControl**](../controls-and-patterns/media-playback.md)
- [**CommandBar**](../controls-and-patterns/app-bars.md)

Ces illustrations montrent l’effet Révéler sur plusieurs contrôles différents :

![Exemples d’effets Révéler](images/RevealExamples_Collage.png)


## <a name="enabling-reveal-on-other-controls"></a>Activer Révéler sur d’autres contrôles

Si vous avez besoin d’appliquer Révéler pour votre scénario (ces contrôles se trouvent dans le contenu principal et/ou sont utilisés dans des contrôles de liste ou de collecte), nous mettons à votre disposition des styles de ressource qui vous permettent d’activer Révéler dans ces situations.

Ces contrôles n’utilisent pas Révéler par défaut, car il s’agit de contrôles mineurs, en général des contrôles annexes aux éléments essentiels de votre application ; cependant, chaque application est différente, et si ces contrôles sont ceux qui sont le plus utilisés dans votre application, nous proposons certains styles qui vous seront utiles pour ce faire :

| Nom du contrôle   | Nom de ressource |
|----------|:-------------:|
| Button |  ButtonRevealStyle |
| ToggleButton | ToggleButtonRevealStyle |
| RepeatButton | RepeatButtonRevealStyle |
| AppBarButton | AppBarButtonRevealStyle |
| AppBarToggleButton | AppBarToggleButtonRevealStyle |
| GridViewItem (Révéler le premier plan du contenu) | GridViewItemRevealBackgroundShowsAboveContentStyle |

Pour appliquer ces styles, définissez simplement la propriété [Style](/uwp/api/Windows.UI.Xaml.Style) du contrôle :

```xaml
<Button Content="Button Content" Style="{ThemeResource ButtonRevealStyle}"/>
```

### <a name="reveal-in-themes"></a>Révéler dans les thèmes

Révéler change légèrement en fonction du thème demandé pour le contrôle, l’application ou le paramètre utilisateur. Dans le thème Dark, la bordure et les éléments sélectionnés apparaissent en blanc avec Révéler, tandis que dans le thème Light, seules les bordures sont obscurcies en gris clair.

![Révéler dans les thèmes Dark et Light](images/Dark_vs_LightReveal.png)

Pour activer les bordures blanches dans le thème Light, définissez simplement le thème demandé pour le contrôle sur « Dark ».

```xaml
<Grid RequestedTheme="Dark">
    <Button Content="Button" Click="Button_Click" Style="{ThemeResource ButtonRevealStyle}"/>
</Grid>
```

Vous pouvez aussi changer le TargetTheme pour RevealBorderBrush en « Dark ». Rappelez-vous de ceci : si TargetTheme est défini sur « Dark », l’effet Révéler sera blanc, tandis que s’il est défini sur « Light », les bordures avec Révéler seront grises.

```xaml
 <RevealBorderBrush x:Key="MyLightBorderBrush" TargetTheme="Dark" Color="{ThemeResource SystemAccentColor}" FallbackColor="{ThemeResource SystemAccentColor}" />
```

## <a name="enabling-reveal-on-custom-controls"></a>Activer Révéler sur les contrôles personnalisés

Vous pouvez ajouter l’effet Révéler à des contrôles personnalisés. Avant de le faire, il est utile d’en savoir un peu plus sur le fonctionnement de l’effet Révéler. Révéler est composé de deux effets distincts : **Révéler la bordure** et **Révéler le pointage**.

- **Révéler la bordure** montre les bordures des éléments interactifs quand un pointeur se trouve à proximité. Cet effet vous indique que vous pouvez réaliser des actions avec les objets à proximité, similaires à celles de l’élément sélectionné.
- **Révéler le pointage** applique un halo léger autour de l’élément pointé ou qui a le focus, et produit une animation par pression lancée d’un clic. 

![Couches de l’effet Révéler](images/RevealLayers.png)

<!-- The Reveal recipe breakdown is:

- Border reveal will be on top of all content but on the designated edges
- Text and content will be displayed directly under Border Reveal
- Hover reveal will be beneath content and text
- The backplate (that turns on and enables Hover Reveal)
- The background (background of control) -->


Ces effets sont définis par deux pinceaux : 
* Révéler la bordure est défini par **RevealBorderBrush**
* Révéler le pointage est défini par **RevealBackgroundBrush**

```xaml
<RevealBorderBrush x:Key="MyRevealBorderBrush" TargetTheme="Light" Color="{ThemeResource SystemAccentColor}" FallbackColor="{ThemeResource SystemAccentColor}"/>
<RevealBackgroundBrush x:Key="MyRevealBackgroundBrush" TargetTheme="Light" Color="{StaticResource SystemAccentColor}" FallbackColor="{StaticResource SystemAccentColor}" />
```
Dans la plupart des cas, nous gérons l’utilisation des deux effets en activant Révéler automatiquement pour certains contrôles. Cependant, d’autres contrôles doivent être activés en appliquant un style ou en modifiant directement leurs modèles.

### <a name="when-to-add-reveal"></a>Quand ajouter l’effet Révéler
Vous pouvez ajouter Révéler à vos contrôles personnalisés, mais avant de le faire, examinez le type de contrôle ainsi que son comportement. 
* Si votre contrôle personnalisé est un élément interactif unique et qu’aucun contrôle similaire ne partage son espace (comme des éléments de menu dans un menu), il est probable que votre contrôle personnalisé n’ait pas besoin de Révéler.  
* Si vous avez un regroupement de contenus ou d’éléments interactifs associés, il est alors probable que cette zone de votre application ait besoin de l’effet Révéler. Cette zone est communément appelée une surface [Commandes](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/collection-commanding).

Par exemple, vous ne devez pas utiliser Révéler sur un bouton seul, mais vous devez l’utiliser sur un ensemble de boutons d’une barre de commandes.

<!-- For example, NavigationView's items are related to page navigation. CommandBar's buttons relate to menu actions or page feature actions. MediaTransportControl's buttons beneath all relate to the media being played. -->

### <a name="using-the-control-template-to-add-reveal"></a>Utilisation du modèle de contrôle pour ajouter Révéler 
Pour activer Révéler sur des contrôles personnalisés ou des contrôles remodélisés, vous devez modifier le modèle de contrôle du contrôle. La plupart des modèles de contrôle ont une grille à la racine ; mettez à jour le [VisualState](/uwp/api/windows.ui.xaml.visualstate) de cette grille racine pour utiliser Révéler :

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

Il est important de noter que Révéler a besoin du pinceau et des méthodes setter dans son état visuel pour fonctionner correctement. Il ne suffit pas de définir le pinceau d’un contrôle sur une des ressources de pinceaux Révéler pour que Révéler soit activé pour ce contrôle. À l’opposé, avoir seulement les cibles ou les paramètres sans les valeurs des pinceaux Révéler ne suffit pas non plus pour activer cet effet.

Pour plus d’informations sur la modification des modèles de contrôle, consultez l’article [Modèles de contrôles XAML](../controls-and-patterns/control-templates.md).

Nous avons créé un ensemble de pinceaux Révéler système que vous pouvez utiliser pour personnaliser votre modèle. Par exemple, vous pouvez utiliser le pinceau **ButtonRevealBackground** pour créer un arrière-plan de bouton personnalisé, ou le pinceau **ListViewItemRevealBackground** pour les listes personnalisées, etc. (Pour plus d’informations sur le fonctionnement des ressources en XAML, consultez l’article [Dictionnaire des ressources XAML](../controls-and-patterns/resourcedictionary-and-xaml-resource-references.md).)

### <a name="full-template-example"></a>Exemple de modèle complet

Voici un modèle complet illustrant l’aspect d’un bouton Révéler :

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

Quand vous activez Révéler sur un contrôle personnalisé, un contrôle remodélisé ou une surface de commandes personnalisées, les conseils suivants peuvent vous aider à optimiser l’effet :
 
* Sur les éléments adjacents avec des tailles qui ne s’alignent pas en hauteur ou en largeur (en particulier dans les listes) : Supprimez le comportement lié à l’approche des bordures et conservez les bordures affichées seulement en cas de pointage.
* Pour les éléments de commandes dont l’état Désactivé change fréquemment : Placez le pinceau d’approche des bordures sur les fonds des éléments et sur leurs bordures pour mettre en évidence leur état.
* Pour les éléments de commandes adjacents qui sont si près qu’ils se touchent : Ajoutez une marge de 1 px entre les deux éléments. 

## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées
### <a name="do"></a>Pratiques conseillées :
- Utilisez Révéler sur des éléments où l’utilisateur peut effectuer de nombreuses actions (barres de commandes, menus de navigation)
- Utilisez Révéler dans les regroupements d’éléments interactifs qui n’ont pas de séparateurs visuels par défaut (listes, rubans)
- Utilisez Révéler dans les zones où il y a une forte densité d’éléments interactifs (scénarios de commandes)
- Ajoutez une marge de 1 pixel entre les éléments auxquels vous appliquez l’effet Révéler

### <a name="dont"></a>Pratiques déconseillées
- N’utilisez pas Révéler sur du contenu statique (arrière-plans, texte)
- N’utilisez pas Révéler sur les fenêtres contextuelles, les menus volants ou les listes déroulantes
- N’utilisez pas Révéler dans des situations d’isolement ou d’unicité
- N’utilisez pas Révéler sur les éléments de très grande taille (plus de 500 px)
- N’utilisez pas Révéler pour des décisions concernant la sécurité, car cela pourrait distraire l’attention de l’utilisateur auquel vous devez délivrer le message


## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemple de Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) : tous les contrôles XAML dans un format interactif.

## <a name="reveal-and-the-fluent-design-system"></a>Révéler et le système Fluent Design

 Le système Fluent Design vous aide à créer une interface utilisateur moderne et claire, qui incorpore de la lumière, de la profondeur, du mouvement, des matériaux et une mise à l’échelle. Révéler est un composant du système Fluent Design qui ajoute de la lumière à votre application. Pour plus d’informations, consultez [Vue d’ensemble de Fluent Design pour UWP](/windows/apps/fluent-design-system).

## <a name="related-articles"></a>Articles connexes

- [RevealBrush, classe](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)
- [Acrylique](acrylic.md)
- [Effets de composition](https://docs.microsoft.com/windows/uwp/graphics/composition-effects)
- [Fluent Design pour UWP](/windows/apps/fluent-design-system)
- [Science in the System: Fluent Design and Depth](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [Science in the System: Fluent Design and Light](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
