---
author: mijacobs
description: "Révéler est un effet visuel qui permet d'ajouter de la profondeur et une meilleure mise au point des éléments interactifs de votre application."
title: "Principales fonctionnalités de Révéler"
template: detail.hbs
ms.author: mijacobs
ms.date: 08/9/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: kisai
design-contact: conrwi
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: high
ms.openlocfilehash: 8ba0d9939d7ab1d9826ed2848e476499f09c628f
ms.sourcegitcommit: 4b522af988273946414a04fbbd1d7fde40f8ba5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2018
---
# <a name="reveal-highlight"></a>Principales fonctionnalités de Révéler

Révéler est un effet visuel qui permet d'ajouter de la profondeur et une meilleure mise au point des éléments interactifs de votre application.

> **API importantes**: [classe RevealBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush), [classe RevealBackgroundBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbackgroundbrush), [classe RevealBorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealborderbrush), [classe RevealBrushHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrushhelper), [classe VisualState](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.VisualState)

Révéler fait cela en révélant conteneur du contenu interactif lorsque le pointeur se trouve à proximité.

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

## <a name="reveal-and-the-fluent-design-system"></a>Révéler et Fluent Design System

 Fluent Design System vous aide à créer une interface utilisateur moderne et audacieuse qui incorpore la lumière, la profondeur, le mouvement, les matières et la notion d'échelle. Révéler est un composant de Fluent Design System qui ajoute de la lumière à votre application. Pour plus d’informations, voir [Présentation de Fluent Design pour UWP](../fluent-design-system/index.md).

## <a name="how-to-use-it"></a>Mode d’utilisation

Révéler fonctionne automatiquement pour certains contrôles. Pour d’autres contrôles, vous pouvez activer Révéler en leur attribuant un style spécial.

## <a name="controls-that-automatically-use-reveal"></a>Contrôles utilisant automatiquement Révéler

- [**ListView**](../controls-and-patterns/lists.md)
- [**GridView**](../controls-and-patterns/lists.md)
- [**TreeView**](../controls-and-patterns/tree-view.md)
- [**NavigationView**](../controls-and-patterns/navigationview.md)
- [**AutosuggestBox**](../controls-and-patterns/auto-suggest-box.md)
- [**MediaTransportControl**](../controls-and-patterns/media-playback.md)
- [**CommandBar**](../controls-and-patterns/app-bars.md)
- [**ComboBox**](../controls-and-patterns/lists.md)

Ces illustrations montrent l’effet de Révéler sur plusieurs contrôles différents:

![Exemples de Révéler](images/RevealExamples_Collage.png)

## <a name="enabling-reveal-on-other-common-controls"></a>Activer Révéler sur d’autres contrôles courants

Si vous avez besoin d'appliquer Révéler pour votre scénario (ces contrôles se trouvent dans le contenu principal et/ou sont utilisés dans des contrôles de liste ou de collecte), nous mettons à votre disposition des styles de ressource qui vous permettent d'activer Révéler dans ces situations.

Ces contrôles n’utilisent pas Révéler par défaut car il s'agit de contrôles mineurs, en général des contrôles annexes aux éléments essentiels de votre application; toutefois, chaque application est différente et si ces contrôles sont ceux qui sont le plus utilisés dans votre application, nous proposons certains styles qui vous seront utiles pour ce faire:

| Nom du contrôle   | Nom de la ressource |
|----------|:-------------:|
| Bouton |  ButtonRevealStyle |
| ToggleButton | ToggleButtonRevealStyle |
| RepeatButton | RepeatButtonRevealStyle |
| AppBarButton | AppBarButtonRevealStyle |
| SemanticZoom | SemanticZoomRevealStyle |

Pour appliquer ces styles, mettez simplement à jour la propriété Style comme suit:

```XAML
<Button Content="Button Content" Style="{StaticResource ButtonRevealStyle}"/>
```

## <a name="enabling-reveal-on-custom-controls"></a>Activer Révéler sur les contrôles personnalisés

Pour décider d'appliquer ou non Révéler à votre contrôle, la règle générale à suivre est d'avoir un groupement d'éléments interactifs tous associés à une fonctionnalité ou une action principale que vous souhaitez effectuer dans votre application.

Par exemple, les éléments NavigationView sont associés à la navigation dans la page. Les boutons de CommandBar se rapportent à des actions de menu ou à des actions de page. Les boutons de MediaTransportControl en dessous se rapportent tous au média en cours de lecture.

Les contrôles qui bénéficient de Révéler n'ont pas besoin d'être liés ensemble; ils doivent juste se trouver dans une zone de haute densité et servir un objectif plus important.

Pour activer Révéler sur les contrôles personnalisés ou remodélisés, vous pouvez accéder au style de ce contrôle dans les états visuels du modèle de ce contrôle, et définir Révéler sur la grille racine:

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

Il est important de noter que Révéler a besoin du pinceau et des méthodes setter dans son état visuel pour fonctionner correctement. Il ne suffit pas de définir le pinceau d'un contrôle sur l'une des quatre ressources de pinceau de Révéler pour que Révéler soit activé pour ce contrôle. À l’opposé, n'avoir que les cibles ou les paramètres sans les valeurs des pinceaux Révéler ne suffit pas non plus pour activer cet effet.

Nous avons créé un ensemble de pinceaux Révéler utilisables pour personnaliser votre interface utilisateur. Par exemple, vous pouvez utiliser le pinceau **ButtonRevealBackground** pour créer un arrière-plan du bouton personnalisé, ou le pinceau **ListViewItemRevealBackground** pour les listes personnalisées, etc.

(Pour en savoir plus sur le fonctionnement des ressources en XAML, consultez l'article [Dictionnaire des ressources XAML](../controls-and-patterns/resourcedictionary-and-xaml-resource-references.md).)

### <a name="reveal-on-listview-controls-with-nested-buttons"></a>Révéler sur les contrôles ListView avec des boutons imbriqués

Si vous avez une vue [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) ainsi que des boutons ou du contenu appelable imbriqué dans ses éléments [ListViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewitem) éléments, vous devez activer Révéler pour les éléments imbriqués.

Dans le cas des boutons ou des contrôles de type bouton dans un contrôle ListViewItem, définissez simplement la propriété du contrôle [Style](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_Style) sur la ressource statique **ButtonRevealStyle**. 

![Révéler pour des éléments imbriqués](images/NestedListContent.png)

Cet exemple illustre l'effet Révéler sur plusieurs boutons à l’intérieur d’un contrôle ListViewItem. 

```XAML
<ListViewItem>
    <StackPanel Orientation="Horizontal">
        <TextBlock Margin="5">Test Text: lorem ipsum.</TextBlock>
        <StackPanel Orientation="Horizontal">
            <Button Content="&#xE71B;" FontFamily="Segoe MDL2 Assets" Width="45" Height="45" Margin="5" Style="{StaticResource ButtonRevealStyle}"/>
            <Button Content="&#xE728;" FontFamily="Segoe MDL2 Assets" Width="45" Height="45" Margin="5" Style="{StaticResource ButtonRevealStyle}"/>
            <Button Content="&#xE74D;" FontFamily="Segoe MDL2 Assets" Width="45" Height="45" Margin="5" Style="{StaticResource ButtonRevealStyle}"/>
         </StackPanel>
    </StackPanel>
</ListViewItem>
```

### <a name="listviewitempresenter-with-reveal"></a>ListViewItemPresenter avec Révéler

En XAML, les listes sont un peu spéciales. Dans le cas de Révéler, nous devons définir le gestionnaire d'état visuel pour Révéler, uniquement dans le ListViewItemPresenter:

```XAML
<ListViewItemPresenter>
<!-- ContentTransitions, SelectedForeground, etc. properties -->
RevealBackground="{ThemeResource ListViewItemRevealBackground}"
RevealBorderThickness="{ThemeResource ListViewItemRevealBorderThemeThickness}"
RevealBorderBrush="{ThemeResource ListViewItemRevealBorderBrush}">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
        <VisualState x:Name="Normal" />
        <VisualState x:Name="Selected" />
        <VisualState x:Name="PointerOver">
            <VisualState.Setters>
                <Setter Target="Root.(RevealBrush.State)" Value="PointerOver" />
            </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="PointerOverSelected">
                <VisualState.Setters>
                    <Setter Target="Root.(RevealBrush.State)" Value="PointerOver" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="PointerOverPressed">
                <VisualState.Setters>
                    <Setter Target="Root.(RevealBrush.State)" Value="Pressed" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Pressed">
                <VisualState.Setters>
                    <Setter Target="Root.(RevealBrush.State)" Value="Pressed" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="PressedSelected">
                <VisualState.Setters>
                    <Setter Target="Root.(RevealBrush.State)" Value="Pressed" />
                </VisualState.Setters>
            </VisualState>
            </VisualStateGroup>
                <VisualStateGroup x:Name="EnabledGroup">
                    <VisualState x:Name="Enabled" />
                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                             <Setter Target="Root.RevealBorderThickness" Value="0"/>
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</ListViewItemPresenter>
```

Cela implique d'ajouter les méthodes setter de l'état visuel spécifique de Révéler à la fin de la collection de propriétés de ListViewItemPresenter.

### <a name="full-template-example"></a>Exemple de modèle complet

Voici un modèle complet illustrant l'aspect d'un bouton Révéler:

```XAML
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

## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées
- Utilisez Révéler sur des éléments sur lesquels l'utilisateur peut agir (boutons, sélections)
- Utilisez Révéler dans les regroupements d'éléments interactifs qui n’ont pas de séparateurs visuels par défaut (listes, barres de commandes)
- Utilisez Révéler dans les zones à densité élevée d'éléments interactifs
- N’utilisez pas Révéler sur le contenu statique (arrière-plans, texte)
- N’utilisez pas Révéler dans des situations d'isolement ou d'unicité
- N’utilisez pas Révéler sur les éléments de très grande taille (plus de 500epx)
- N’utilisez pas Révéler pour des décisions de sécurité, car cela pourrait distraire l'utilisateur tandis que vous souhaitez lui communiquer un message important

## <a name="how-we-designed-reveal"></a>Comment nous avons conçu Révéler

Révéler se compose de deux composants visuels principaux: le comportement **Révéler l'élément sélectionné** et le comportement **Révéler la bordure**.

![Révéler les couches](images/RevealLayers.png)

La fonction Révéler l'élément sélectionné est directement liée au contenu sélectionné (via le pointeur ou l’entrée de la mise au point) et applique un halo léger autour de l’élément pointé ou en focus, vous indiquant ainsi que vous pouvez interagir avec celui-ci.

La fonction Révéler la bordure est appliquée à l’élément sélectionné et les éléments à proximité. Cela vous indique que vous pouvez réaliser des actions avec les objets à proximité similaires à celles de l'élément sélectionné.

Voici le détail de la recette de la fonction Révéler:

- La fonction Révéler la bordure se trouve par-dessus tout le contenu, mais sur des bords désignés
- Le texte et le contenu s'affichent directement sous Révéler la bordure
- Révéler l'élément sélectionné se trouve sous le contenu et le texte
- La plaque de fond (qui active la fonction Révéler l'élément sélectionné)
- L’arrière-plan (l'arrière-plan de la commande)

<!--
<div class=”microsoft-internal-note”>
To create your own Reveal lighting effect for static comps or prototype purposes, see the full [uni design guidance](http://uni/DesignDepot.FrontEnd/#/ProductNav/3020/1/dv/?t=Resources%7CToolkit%7CReveal&f=Neon) for this effect in illustrator.
</div>
-->  

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles associés

- [Classe RevealBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)
- [Acrylique](acrylic.md)
- [Effets de composition](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
- [Fluent Design pour UWP](../fluent-design-system/index.md)
- [De la science dans le système: Fluent Design et la Profondeur](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [De la science dans le système: Fluent Design et la Lumière](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
