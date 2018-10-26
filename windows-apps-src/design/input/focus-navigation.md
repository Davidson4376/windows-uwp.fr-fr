---
author: Karl-Bridge-Microsoft
title: Navigation en mode focus pour clavier, boîtier de commande, télécommande et outils d’accessibilité
Description: ''
label: ''
template: detail.hbs
keywords: clavier, contrôleur de jeu, télécommande, navigation, navigation directionnelle interne, zone directionnelle, stratégie de navigation, entrée, interaction utilisateur, accessibilité, facilité d’utilisation
ms.date: 03/02/2018
ms.author: kbridge
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f4c86847e02c4ba987f8b1dcc42016145b175193
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5568803"
---
# <a name="focus-navigation-for-keyboard-gamepad-remote-control-and-accessibility-tools"></a>Navigation en mode focus pour clavier, boîtier de commande, télécommande et outils d’accessibilité

![Clavier, télécommande et bouton multidirectionnel](images/dpad-remote/dpad-remote-keyboard.png)

Utilisez la navigation en mode focus pour fournir des expériences d’interaction complètes et cohérentes dans vos applications UWP, des contrôles personnalisés pour les experts en utilisation du clavier, les utilisateurs en situation de handicap ou présentant d’autres exigences d’accessibilité, ainsi que l’expérience «10-foot» pour les écrans de télévisions et la XboxOne.

## <a name="overview"></a>Vue d'ensemble

La navigation en mode focus désigne le mécanisme sous-jacent qui permet aux utilisateurs de naviguer et d’interagir avec l’interface utilisateur d’une application UWP à l’aide d’un clavier, d’un boîtier de commande ou d’une télécommande.

> [!NOTE] 
> Les dispositifs d’entrée sont généralement classés en tant que dispositifs de pointage, tels que l’interaction tactile, le pavé tactile, le stylet et la souris, et dispositifs qui n’effectuent pas de pointage, tels que clavier, le boîtier de commande et la télécommande.

Cette rubrique décrit comment optimiser une application UWP et générer des expériences d’interaction personnalisées pour les utilisateurs qui s’appuient sur des types d’entrée qui n’effectuent pas de pointage. 

Même si nous nous concentrons sur les entrées au clavier pour les contrôles personnalisés dans les applications UWP sur PC, une expérience de clavier bien conçue est également importante pour les claviers logiciels, tels que le clavier tactile et le clavier visuel, prenant en charge les outils d’accessibilité tels que le Narrateur de Windows et l’expérience «10-foot».

Voir [Gérer les entrées du pointeur](handle-pointer-input.md) pour obtenir des conseils sur la création d’expériences personnalisées dans les applications UWP pour les dispositifs de pointage.

Pour plus d’informations sur la création d’applications et d’expériences pour le clavier, voir [Interactions avec le clavier](keyboard-interactions.md).

## <a name="general-guidance"></a>Recommandations d’ordre général
Seuls les éléments d’interface utilisateur qui nécessitent l’interaction de l’utilisateur doivent prendre en charge la navigation en mode focus; les éléments qui ne nécessitent aucune action, comme les images statiques, n’ont pas besoin du focus pour le clavier. Les lecteurs d’écran et les outils d’accessibilité similaires annoncent cependant ces éléments statiques, même lorsqu’ils ne sont pas inclus dans la navigation en mode focus. 

Il est important de se souvenir que, contrairement à la navigation avec un dispositif de pointage, tel que la souris ou la fonctionnalité tactile, la navigation en mode focus est linéaire. Au moment d’implémenter la navigation en mode focus, envisagez la manière dont un utilisateur interagira avec votre application et comment devrait se dérouler la navigation logique. Dans la plupart des cas, nous recommandons que le comportement de navigation en mode focus personnalisé suive le modèle de lecture préféré inhérente à la culture de l’utilisateur.

Certaines autres considérations de navigation en mode focus incluent les éléments suivants:
- Les contrôles sont-ils regroupés logiquement?
- Existe-t-il des groupes de contrôles avec une plus grande importance? 
   - Si oui, ces groupes contiennent-ils des sous-groupes?
- La disposition nécessite-t-elle une navigation directionnelle personnalisée (touches de direction) et un ordre de tabulation?

Le livre électronique [Engineering Software for Accessibility](https://www.microsoft.com/download/details.aspx?id=19262) (Conception de logiciels accessibles) comporte un excellent chapitre sur la *conception de la hiérarchie logique*.

## <a name="2d-directional-navigation-for-keyboard"></a>Navigation directionnelle2D pour le clavier

La région de navigation interne2D d’un contrôle, ou d’un groupe de contrôles, est appelée «zone directionnelle». Lorsque le focus se déplace sur cet objet, les touches de direction du clavier (gauche, droite, haut et bas) peuvent être utilisées pour naviguer entre les éléments enfants au sein de la zone directionnelle.

![zone directionnelle](images/keyboard/directional-area-small.png)

*Région de navigation interne2D, ou zone directionnelle, d’un groupe de contrôles*

Vous pouvez utiliser la propriété [XYFocusKeyboardNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_XYFocusKeyboardNavigation) (qui peut avoir les valeurs [Auto](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode), [Enabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode), ou [Disabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)) pour gérer la navigation interne2D avec les touches de direction du clavier.

> [!NOTE]  
> L’ordre de tabulation n’est pas affecté par cette propriété. Pour éviter une expérience de navigation confuse, nous recommandons que les éléments enfants d’une zone directionnelle ne soient *pas* explicitement spécifiés dans l’ordre de navigation par tabulations de votre application. Consultez les propriétés [UIElement.TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) et [TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) pour plus d’informations sur le comportement de la tabulation pour un élément.  

### <a name="autohttpsdocsmicrosoftcomuwpapiwindowsuixamlinputxyfocuskeyboardnavigationmode-default-behavior"></a>[Auto](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode) (comportement par défaut)

Lorsqu’il est défini sur Auto, le comportement de navigation directionnelle est déterminé par l’ascendance de l’élément ou par la hiérarchie d’héritage. Si tous les ancêtres sont en mode par défaut (définis sur **Auto**), le comportement de navigation directionnelle avec le clavier n’est *pas* pris en charge.

### [<a name="disabled"></a>Disabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)

Définissez **XYFocusKeyboardNavigation** sur **Disabled** pour bloquer la navigation directionnelle sur le contrôle et ses éléments enfants.

![Comportement désactivé de XYFocusKeyboardNavigation](images/keyboard/xyfocuskeyboardnav-disabled.gif)

*Comportement désactivé de XYFocusKeyboardNavigation*

Dans cet exemple, le [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) principal (ContainerPrimary) a sa propriété **XYFocusKeyboardNavigation** définie sur **Enabled**. Tous les éléments enfants héritent de ce paramètre et il est possible d’y naviguer avec les touches de direction. Toutefois, les éléments B3 et B4 sont dans un [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) secondaire (ContainerSecondary), avec **XYFocusKeyboardNavigation** défini sur **Disabled**, ce qui remplace le conteneur principal et désactive la navigation avec les touches de direction vers lui-même et entre ses éléments enfants.

```XAML
<Grid 
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" 
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="75"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
                Grid.Row="0" 
                FontWeight="ExtraBold" 
                HorizontalTextAlignment="Center"
                TextWrapping="Wrap" 
                Padding="10" />
    <StackPanel Name="ContainerPrimary" 
                XYFocusKeyboardNavigation="Enabled" 
                KeyDown="ContainerPrimary_KeyDown" 
                Orientation="Horizontal" 
                BorderBrush="Green" 
                BorderThickness="2" 
                Grid.Row="1" 
                Padding="10" 
                MaxWidth="200">
        <Button Name="B1" 
                Content="B1" 
                GettingFocus="Btn_GettingFocus" />
        <Button Name="B2" 
                Content="B2" 
                GettingFocus="Btn_GettingFocus" />
        <StackPanel Name="ContainerSecondary" 
                    XYFocusKeyboardNavigation="Disabled" 
                    Orientation="Horizontal" 
                    BorderBrush="Red" 
                    BorderThickness="2">
            <Button Name="B3" 
                    Content="B3" 
                    GettingFocus="Btn_GettingFocus" />
            <Button Name="B4" 
                    Content="B4" 
                    GettingFocus="Btn_GettingFocus" />
        </StackPanel>
    </StackPanel>
</Grid>
```

### [<a name="enabled"></a>Enabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)

Définissez **XYFocusKeyboardNavigation** sur **Enabled** pour prendre en charge la navigation directionnelle2D pour un contrôle et chacun de ses objets enfants [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement).

Lorsqu’elle est définie, la navigation avec les touches de direction est limitée aux éléments situés dans la zone directionnelle. La navigation par tabulation n’est pas affectée, comme tous les contrôles restent accessibles via la hiérarchie de leur ordre de tabulation.

![Comportement activé de XYFocusKeyboardNavigation](images/keyboard/xyfocuskeyboardnav-enabled.gif)

*Comportement activé de XYFocusKeyboardNavigation*

Dans cet exemple, le [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) principal (ContainerPrimary) a sa propriété **XYFocusKeyboardNavigation** définie sur **Enabled**. Tous les éléments enfants héritent de ce paramètre et il est possible d’y accéder avec les touches de direction. Les éléments B3 et B4 sont dans un [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) secondaire(ContainerSecondary) où **XYFocusKeyboardNavigation** n’est pas défini, qui hérite du paramètre de conteneur principal. L’élément B5 n’est pas dans une zone directionnelle déclarée et ne prend pas en charge la navigation avec les touches de direction, mais il prend pas en charge le comportement de navigation par tabulation standard.

```xaml
<Grid
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="100"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
               Grid.Row="0"
               FontWeight="ExtraBold"
               HorizontalTextAlignment="Center"
               TextWrapping="Wrap"
               Padding="10" />
    <StackPanel Grid.Row="1"
                Orientation="Horizontal"
                HorizontalAlignment="Center">
        <StackPanel Name="ContainerPrimary"
                    XYFocusKeyboardNavigation="Enabled"
                    KeyDown="ContainerPrimary_KeyDown"
                    Orientation="Horizontal"
                    BorderBrush="Green"
                    BorderThickness="2"
                    Padding="5" Margin="5">
            <Button Name="B1"
                    Content="B1"
                    GettingFocus="Btn_GettingFocus" Margin="5" />
            <Button Name="B2"
                    Content="B2"
                    GettingFocus="Btn_GettingFocus" />
            <StackPanel Name="ContainerSecondary"
                        Orientation="Horizontal"
                        BorderBrush="Red"
                        BorderThickness="2"
                        Margin="5">
                <Button Name="B3"
                        Content="B3"
                        GettingFocus="Btn_GettingFocus"
                        Margin="5" />
                <Button Name="B4"
                        Content="B4"
                        GettingFocus="Btn_GettingFocus"
                        Margin="5" />
            </StackPanel>
        </StackPanel>
        <Button Name="B5"
                Content="B5"
                GettingFocus="Btn_GettingFocus"
                Margin="5" />
    </StackPanel>
</Grid>
```

Vous pouvez avoir plusieurs niveaux de zones directionnelles imbriquées. Si tous les éléments parents ont le paramètre XYFocusKeyboardNavigation défini sur Enabled, les limites de la région de navigation interne sont ignorées.

Voici un exemple de deux zones directionnelles imbriquées dans un élément qui ne prend pas explicitement en charge la navigation directionnelle2D. Dans ce cas, la navigation directionnelle n’est pas prise en charge entre les deux zones imbriquées.

![Comportement activé et imbriqué de XYFocusKeyboardNavigation](images/keyboard/xyfocuskeyboardnav-enabled-nested1.gif)

*Comportement activé et imbriqué de XYFocusKeyboardNavigation*

Voici un exemple plus complexe de troiszones directionnelles imbriquées dans lesquelles:

-   Lorsque B1 a le focus, il est possible de naviguer uniquement vers B5 (et vice versa), car une limite de zone directionnelle où XYFocusKeyboardNavigation est défini sur Disabled rend B2, B3 et B4 inaccessibles avec les touches de direction.
-   Quand B2 a le focus, il est possible de naviguer uniquement vers B3 (et vice versa), car la limite de zone directionnelle empêche la navigation avec les touches de direction vers B1, B4 et B5
-   Quand B4 a le focus, la touche Tab doit être utilisée pour naviguer entre les contrôles

![Comportement activé et imbriqué de façon complexe de XYFocusKeyboardNavigation](images/keyboard/xyfocuskeyboardnav-enabled-nested2.gif)

*Comportement activé et imbriqué de façon complexe de XYFocusKeyboardNavigation*

## <a name="tab-navigation"></a>Navigation par tabulation

Si les touches de direction peuvent être utilisées pour la navigation directionnelle2D dans un contrôle, ou un groupe de contrôles, la touche Tab permet de naviguer entre tous les contrôles dans une application UWP. 

Tous les contrôles interactifs prennent en charge la navigation par défaut avec la touche Tab (les propriétés [IsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_IsEnabled) et [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) sont définies sur **true**), l’ordre de tabulation logique étant dérivé de la disposition du contrôle dans votre application. Toutefois, l’ordre par défaut ne correspond pas nécessairement à l’ordre visuel. La position d’affichage réelle peut dépendre du conteneur de disposition parent et de certaines propriétés que vous pouvez définir sur les éléments enfants pour influencer la disposition.

Évitez un ordre de tabulation personnalisé qui fait sauter le focus dans toute votre application. Par exemple, une liste des contrôles placée dans un formulaire doit présenter un ordre de tabulation de gauche à droite et de haut en bas (selon les paramètres régionaux).

Dans cette section, nous décrivons comment cet ordre de tabulation peut être entièrement personnalisé en fonction de votre application.

### <a name="set-the-tab-navigation-behavior"></a>Définir le comportement de navigation par tabulation

La propriété [TabFocusNavigation](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_TabFocusNavigation) de [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) spécifie le comportement de navigation par tabulation pour la totalité de son arborescence d’objets (ou de sa zone directionnelle).

> [!NOTE]
> Utilisez cette propriété au lieu de la propriété [Control.TabNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation) pour les objets qui n’utilisent pas un [ControlTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate) pour définir leur apparence.

Comme nous l’avons mentionné dans la section précédente, pour éviter une expérience de navigation confuse, nous recommandons que les éléments enfants d’une zone directionnelle ne soient *pas* explicitement spécifiés dans l’ordre de navigation par tabulations de votre application. Consultez les propriétés [UIElement.TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) et [TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) pour plus d’informations sur le comportement de la tabulation pour un élément.   
> Pour les versions antérieures à Windows10Creators Update (build 10.0.15063), les paramètres de tabulation étaient limités aux objets [ControlTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate). Pour plus d’informations, voir [Control.TabNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation).

[TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) a une valeur de type [KeyboardNavigationMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardnavigationmode) avec les valeurs possibles suivantes (notez que ces exemples ne sont pas des groupes de contrôles personnalisés et ne nécessitent pas de navigation interne avec les touches de direction):

- **Local** (par défaut)   
  Les index de tabulation sont reconnus sur la sous-arborescence locale à l’intérieur du conteneur. Dans cet exemple, l’ordre de tabulation est B1, B2, B3, B4, B5, B6, B7, B1.

   ![Comportement de navigation par tabulation «Local»](images/keyboard/tabnav-local.gif)

   *Comportement de navigation par tabulation «Local»*

- **Once**  
  Le conteneur et tous les éléments enfants reçoivent une fois le focus. Dans cet exemple, l’ordre de tabulation est B1, B2, B7, B1 (la navigation interne avec la touche de direction est également illustrée).

   ![Comportement de navigation par tabulation «Once»](images/keyboard/tabnav-once.gif)

   *Comportement de navigation par tabulation «Once»*

- **Cycle**   
  Les cycles de focus reviennent à l’élément susceptible de recevoir le focus initial à l’intérieur d’un conteneur. Dans cet exemple, l’ordre de tabulation est B1, B2, B3, B4, B5, B6, B2...

   ![Comportement de navigation par tabulation «Cycle»](images/keyboard/tabnav-cycle.gif)

   *Comportement de navigation par tabulation «Cycle»*

Voici le code pour les exemples précédents (avec TabFocusNavigation = "Cycle").

```XAML
<Grid 
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" 
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="300"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
               Grid.Row="0" 
               FontWeight="ExtraBold" 
               HorizontalTextAlignment="Center"
               TextWrapping="Wrap" 
               Padding="10" />
    <StackPanel Name="ContainerPrimary"
                KeyDown="Container_KeyDown" 
                Orientation="Horizontal" 
                HorizontalAlignment="Center"
                BorderBrush="Green" 
                BorderThickness="2" 
                Grid.Row="1" 
                Padding="10" 
                MaxWidth="200">
        <Button Name="B1" 
                Content="B1" 
                GettingFocus="Btn_GettingFocus" 
                Margin="5"/>
        <StackPanel Name="ContainerSecondary" 
                    KeyDown="Container_KeyDown"
                    XYFocusKeyboardNavigation="Enabled" 
                    TabFocusNavigation ="Cycle"
                    Orientation="Vertical" 
                    VerticalAlignment="Center"
                    BorderBrush="Red" 
                    BorderThickness="2"
                    Padding="5" Margin="5">
            <Button Name="B2" 
                    Content="B2" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B3" 
                    Content="B3" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B4" 
                    Content="B4" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B5" 
                    Content="B5" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B6" 
                    Content="B6" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
        </StackPanel>
        <Button Name="B7" 
                Content="B7" 
                GettingFocus="Btn_GettingFocus" 
                Margin="5"/>
    </StackPanel>
</Grid>
```

### [<a name="tabindex"></a>TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex)

Utilisez [TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) pour spécifier l’ordre dans lequel les éléments reçoivent le focus lorsque l’utilisateur navigue dans les contrôles à l’aide de la touche Tab. Un contrôle avec un index de tabulation inférieur reçoit le focus avant un contrôle avec un index supérieur.

Lorsqu’un contrôle n’a aucun [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) spécifié, il se voit attribuer une valeur d’index supérieure à la valeur d’index actuelle la plus élevée (et la priorité la plus basse) de tous les contrôles interactifs de l’arborescence visuelle, basée sur l’étendue. 

Tous les éléments enfants d’un contrôle sont considérés comme une étendue. Si l’un de ces éléments a également des éléments enfants, ceux-ci sont considérés comme faisant partie d’une autre étendue. Toute ambiguïté est résolue en choisissant le premier élément dans l’arborescence visuelle de l’étendue. 

Pour exclure spécifiquement un contrôle de l’ordre de tabulation, attribuez à la propriété [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) la valeur **false**.

Remplacez l’ordre de tabulation par défaut en définissant la propriété [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex).

> [!NOTE] 
> [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) fonctionne de la même manière avec [UIElement.TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) et [Control.TabNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation).


Nous montrons ici comment la navigation en mode focus peut être affectée par la propriété [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) sur des éléments spécifiques. 

![Navigation par tabulation «Local» avec comportement TabIndex](images/keyboard/tabnav-tabindex.gif)

*Navigation par tabulation «Local» avec comportement TabIndex*

Dans l’exemple précédent, il existe deux étendues: 
- B1, zone directionnelle (B2 - B6) et B7
- zone directionnelle (B2 - B6)

Quand B3 (dans la zone directionnelle) reçoit le focus, l’étendue change et la navigation par tabulation est transférée vers la zone directionnelle dans laquelle le meilleur candidat pour le focus ultérieur est identifié. Dans ce cas, B2 suivi par B4, B5 et B6. Puis, l’étendue change de nouveau, et le focus se déplace sur B1.

Voici le code pour cet exemple.

```xaml
<Grid
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="300"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
               Grid.Row="0"
               FontWeight="ExtraBold"
               HorizontalTextAlignment="Center"
               TextWrapping="Wrap"
               Padding="10" />
    <StackPanel Name="ContainerPrimary"
                KeyDown="Container_KeyDown"
                Orientation="Horizontal"
                HorizontalAlignment="Center"
                BorderBrush="Green"
                BorderThickness="2"
                Grid.Row="1"
                Padding="10"
                MaxWidth="200">
        <Button Name="B1"
                Content="B1"
                TabIndex="1"
                ToolTipService.ToolTip="TabIndex = 1"
                GettingFocus="Btn_GettingFocus"
                Margin="5"/>
        <StackPanel Name="ContainerSecondary"
                    KeyDown="Container_KeyDown"
                    TabFocusNavigation ="Local"
                    Orientation="Vertical"
                    VerticalAlignment="Center"
                    BorderBrush="Red"
                    BorderThickness="2"
                    Padding="5" Margin="5">
            <Button Name="B2"
                    Content="B2"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B3"
                    Content="B3"
                    TabIndex="3"
                    ToolTipService.ToolTip="TabIndex = 3"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B4"
                    Content="B4"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B5"
                    Content="B5"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B6"
                    Content="B6"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
        </StackPanel>
        <Button Name="B7"
                Content="B7"
                TabIndex="2"
                ToolTipService.ToolTip="TabIndex = 2"
                GettingFocus="Btn_GettingFocus"
                Margin="5"/>
    </StackPanel>
</Grid>
```

## <a name="2d-directional-navigation-for-keyboard-gamepad-and-remote-control"></a>Navigation directionnelle2D pour clavier, boîtier de commande et télécommande

Les types d’entrée qui n’effectuent pas de pointage, tels qu’un clavier, un boîtier de commande, une télécommande, et des outils d’accessibilité tels que le Narrateur de Windows, partagent un mécanisme sous-jacent commun pour la navigation et l’interaction avec l’interface utilisateur de votre application UWP.

Dans cette section, nous abordons comment spécifier une stratégie de navigation préférée et optimiser la navigation en mode focus au sein de votre application par le biais d’un ensemble de propriétés de stratégie de navigation qui prennent en charge tous les types d’entrée basés sur le focus qui n’effectuent pas de pointage.

Pour obtenir des informations plus générales sur la création d’applications et d’expériences pour Xbox/TV, voir [Interactions avec le clavier](keyboard-interactions.md) et [Conception pour Xbox et TV](../devices/designing-for-tv.md#xy-focus-navigation-and-interaction).

### <a name="navigation-strategies"></a>Stratégies de navigation

> Les stratégies de navigation sont applicables au clavier, au boîtier de commande, à la télécommande et à divers outils d’accessibilité.

Les propriétés de stratégie de navigation suivantes vous permettent de déterminer quel contrôle reçoit le focus basé sur la touche de direction, le bouton multidirectionnel, ou toute pression similaire. 

-   XYFocusUpNavigationStrategy
-   XYFocusDownNavigationStrategy
-   XYFocusLeftNavigationStrategy
-   XYFocusRightNavigationStrategy

Ces propriétés peuvent avoir les valeurs [Auto](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy) (par défaut), [NavigationDirectionDistance](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy), [Projection](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy) ou [RectilinearDistance ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy).

Si la valeur est **Auto**, le comportement de l’élément est basé sur ses ancêtres. Si tous les éléments sont définis sur **Auto**, **Projection** est utilisé.

> [!NOTE]
> Il existe d’autres facteurs qui influencent le résultat, tels que l’élément ciblé précédemment ou la proximité de l’axe du sens de la navigation.

### <a name="projection"></a>Projection

La stratégie Projection indique que le focus se déplace vers le premier élément rencontré lorsque le bord de l’élément ayant actuellement le focus est *projeté* dans la direction de navigation.

Dans cet exemple, chaque direction de navigation en mode focus est définie sur Projection. Notez comment le focus se déplace vers le bas de B1 à B4, en contournant B3. Cela est dû au fait que B3 ne se trouve pas dans la zone de projection. Notez également comment un candidat au focus n’est pas identifié lors du déplacement vers la gauche à partir de B1. Cela est dû au fait que la position de B2 par rapport à B1 élimine B3 comme candidat. Si B3 se trouvait dans la même ligne que B2, il serait un candidat viable pour la navigation vers la gauche. B2 est un candidat viable en raison de sa proximité dégagée à l’axe du sens de navigation.

![Stratégie de navigation Projection](images/keyboard/xyfocusnavigationstrategy-projection.gif)

*Stratégie de navigation Projection*

### <a name="navigationdirectiondistance"></a>NavigationDirectionDistance

La stratégie NavigationDirectionDistance déplace le focus vers l’élément le plus proche de l’axe principal du sens de la navigation.

Le bord du rectangle englobant correspondant à la direction de navigation est *étendu* et *projeté* pour identifier les cibles candidates. Le premier élément rencontré est identifié comme étant la cible. Dans le cas de plusieurs candidats, l’élément le plus proche est identifié comme étant la cible. S’il existe encore plusieurs candidats, l’élément le plus haut/le plus à gauche est identifié comme candidat.

![Stratégie de navigation NavigationDirectionDistance](images/keyboard/xyfocusnavigationstrategy-navigationdirectiondistance.gif)

*Stratégie de navigation NavigationDirectionDistance*

### <a name="rectilineardistance"></a>RectilinearDistance

La stratégie RectilinearDistance déplace le focus vers l’élément le plus proche en fonction de la distance2D rectiligne ([métrique Taxicab](https://en.wikipedia.org/wiki/Taxicab_geometry)).

La somme de la distance principale et de la distance secondaire à chaque candidat potentiel est utilisée pour identifier le meilleur candidat. En cas d’égalité, le premier élément vers la gauche est sélectionné si la direction demandée est vers le haut ou le bas, et le premier élément vers le haut est sélectionné si la direction demandée est vers la gauche ou la droite.

![Stratégie de navigation RectilinearDistance](images/keyboard/xyfocusnavigationstrategy-rectilineardistance.gif)

*Stratégie de navigation RectilinearDistance*

Cette image montre comment, quand B1 a le focus et que la direction demandée est vers le bas, B3 est le candidat au focus RectilinearDistance. Ce résultat est basé sur les calculs suivants pour cet exemple:
-   Distance (B1, B3, bas) est égale à 10+0=10
-   Distance (B1, B2, bas) est égale à 0+40=30
-   Distance (B1, D, bas) est égale à 30+0=30


## <a name="related-articles"></a>Articles associés
- [Navigation en mode focus programmé](focus-navigation-programmatic.md)
- [Interactions avec le clavier](keyboard-interactions.md)
- [Accessibilité du clavier](../accessibility/keyboard-accessibility.md) 



