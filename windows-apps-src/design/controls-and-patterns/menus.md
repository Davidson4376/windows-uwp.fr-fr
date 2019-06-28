---
Description: Les menus et les menus contextuels affichent une liste de commandes ou d’options lorsque l’utilisateur les demande.
title: Menus et menus contextuels
label: Menus and context menus
template: detail.hbs
ms.date: 04/19/2019
ms.topic: article
ms.custom: RS5, 19H1
keywords: windows 10, uwp
ms.assetid: 0327d8c1-8329-4be2-84e3-66e1e9a0aa60
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 10e91e8098f232d2875c802567674c9feacb2af9
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66364617"
---
# <a name="menus-and-context-menus"></a>Menus et menus contextuels

Les menus et les menus contextuels affichent une liste de commandes ou d’options lorsque l’utilisateur les demande. Utilisez un menu volant pour afficher un menu inséré. Utilisez une barre de menus pour afficher un ensemble de menus sur une ligne horizontale, généralement en haut d’une fenêtre d’application. Chaque menu peut avoir des sous-menus et des éléments de menu.

![Exemple de menu contextuel standard](images/contextmenu_rs2_icons.png)

| **Obtenir la bibliothèque d’interface utilisateur Windows** |
| - |
| Ce contrôle est inclus dans la bibliothèque d’interface utilisateur Windows. Cette dernière est un package NuGet contenant de nouveaux contrôles et de nouvelles fonctionnalités d’interface utilisateur pour les applications UWP. Pour plus d’informations, notamment des instructions d’installation, consultez [Vue d’ensemble de la bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

| **API de plateforme** | **API de la bibliothèque d’interface utilisateur Windows** |
| - | - |
| [classe MenuFlyout](/uwp/api/windows.ui.xaml.controls.menuflyout), [classe MenuBar](/uwp/api/windows.ui.xaml.controls.menubar), [propriété ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout), [propriété FlyoutBase.AttachedFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout) | [MenuBar, classe](/uwp/api/microsoft.ui.xaml.controls.menubar) |

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Les menus et les menus contextuels permettent de gagner de l’espace en organisant les commandes et en les masquant jusqu’à ce que l’utilisateur en ait besoin. Si une commande particulière doit être utilisée fréquemment et que vous disposez de l’espace disponible, envisagez de le placer directement dans son propre élément, plutôt que dans un menu, afin que les utilisateurs n’aient pas à passer par un menu afin d’y accéder.

Les menus et les menus contextuels permettent d’organiser les commandes. Pour afficher du contenu arbitraire, comme une notification ou une demande de confirmation, utilisez une [boîte de dialogue ou un menu volant](dialogs.md).

### <a name="menubar-vs-menuflyout"></a>Différences entre MenuBar et MenuFlyout

Pour afficher un menu dans un menu volant attaché à un élément d’interface utilisateur sur canevas, utilisez le contrôle MenuFlyout pour héberger les éléments de menu. Vous pouvez appeler un menu volant comme un menu standard ou un menu contextuel. Un menu volant héberge un seul menu de niveau supérieur (et des sous-menus facultatifs).

Pour afficher un ensemble de menus de niveau supérieur sur une ligne horizontale, utilisez une barre de menus. En règle générale, les barres de menus sont placées en haut de la fenêtre d’application.

### <a name="menubar-vs-commandbar"></a>Différences entre MenuBar CommandBar

MenuBar et CommandBar sont des surfaces qui permettent d’afficher des commandes pour vos utilisateurs. MenuBar fournit un moyen rapide et simple d’afficher un ensemble de commandes dans les applications nécessitant un niveau d’organisation et de regroupement plus avancé que celui fourni par CommandBar.

Vous pouvez également utiliser un MenuBar conjointement à un CommandBar. Utilisez MenuBar pour fournir le groupe de commandes, et CommandBar pour mettre en surbrillance les commandes les plus utilisées.

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/MenuFlyout">ouvrir l’application et voir l’objet MenuFlyout en contexte</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="menus-vs-context-menus"></a>Menus par rapport aux menus contextuels

Les menus et les menus contextuels ont une apparence et un contenu similaires. D’ailleurs, vous pouvez utiliser le même contrôle ([MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout)) pour les créer. La seule différence réside dans la façon dont vous permettez à l’utilisateur d’y accéder.

Quand utiliser un menu ou un menu contextuel ?

- Si l’élément hôte est un bouton ou un autre élément de commande dont le principal rôle est d’afficher des commandes supplémentaires, utilisez un menu.
- Si l’élément hôte est un autre type d’élément ayant un autre objectif principal (par exemple, la présentation de texte ou une image), utilisez un menu contextuel.

Par exemple, vous pouvez utiliser un menu sur un bouton pour fournir des options de tri et de filtrage à une liste. Dans ce scénario, l’objectif principal du contrôle de bouton consiste à fournir un accès à un menu.

![Exemple de menu dans Mail](images/Mail_Menu.png)

Si vous voulez ajouter des commandes (telles que couper, copier et coller) à un élément de texte, utilisez un menu contextuel au lieu d’un menu. Dans ce scénario, le rôle principal de l’élément de texte est de présenter et de modifier du texte ; les commandes supplémentaires (par exemple, couper, copier et coller) sont secondaires et doivent figurer dans un menu contextuel.

![Exemple de menu contextuel dans la galerie de photos](images/ContextMenu_example.png)

### <a name="menus"></a>Les menus :

- Ont un point d’entrée unique (un menu Fichier en haut de l’écran, par exemple) qui est toujours affiché.
- Sont généralement attachés à un bouton ou un élément de menu parent.
- Sont appelés en cliquant avec le bouton gauche de la souris (ou par le biais d’une action équivalente, telles que l’appui avec votre doigt).
- Sont associés à un élément via ses propriétés [Flyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button.flyout) ou [FlyoutBase.AttachedFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout), ou sont regroupés dans une barre de menus en haut de la fenêtre d’application.

### <a name="context-menus"></a>Les menus contextuels :

- Sont attachés à un élément unique et affichent les commandes secondaires.
- Sont appelés en cliquant avec le bouton droit de la souris (ou par le biais d’une action équivalente, telle que l’appui prolongé avec votre doigt).
- Sont associés à un élément via sa propriété [ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout).

## <a name="icons"></a>Icônes

Fournissez des icônes d’élément de menu pour :

- Les éléments les plus fréquemment utilisés
- Les éléments de menu dont l’icône est standard ou bien connue
- Les éléments de menu dont l’icône montre clairement ce que permet de faire la commande

Ne vous sentez pas obligé de fournir des icônes pour les commandes qui n’ont pas de visualisation standard. Les icônes non explicites ne sont pas utiles : elles créent un encombrement visuel et empêchent les utilisateurs de se concentrer sur les éléments de menu importants.

![Exemple de menu contextuel avec des icônes](images/contextmenu_rs2_icons.png)

````xaml
<MenuFlyout>
  <MenuFlyoutItem Text="Share" >
    <MenuFlyoutItem.Icon>
      <FontIcon Glyph="&#xE72D;" />
    </MenuFlyoutItem.Icon>
  </MenuFlyoutItem>
  <MenuFlyoutItem Text="Copy" Icon="Copy" />
  <MenuFlyoutItem Text="Delete" Icon="Delete" />
  <MenuFlyoutSeparator />
  <MenuFlyoutItem Text="Rename" />
  <MenuFlyoutItem Text="Select" />
</MenuFlyout>
````

> [!TIP]
> La taille des icônes d’un MenuFlyoutItem est de 16 x 16 px. Si vous utilisez SymbolIcon, FontIcon ou PathIcon, la taille de l’icône s’ajustera automatiquement sans perte de fidélité. Si vous utilisez BitmapIcon, vérifiez que la taille de votre ressource est de 16 x 16 px.  

## <a name="create-a-menu-flyout-or-a-context-menu"></a>Créer un menu volant ou un menu contextuel

Pour créer un menu volant ou un menu contextuel, utilisez la [classe MenuFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout). Vous définissez le contenu du menu en ajoutant des objets [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutitem), [MenuFlyoutSubItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutsubitem), [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem), [RadioMenuFlyoutItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem) et [MenuFlyoutSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutseparator) à MenuFlyout.

Voici le rôle de ces objets :

- [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutitem) : effectuer une action immédiate.
- [MenuFlyoutSubItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutsubitem) : contient une liste d’éléments de menu en cascade.
- [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem) : activer ou désactiver une option.
- [RadioMenuFlyoutItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem) : basculer entre des éléments de menu qui s’excluent l’un l’autre.
- [MenuFlyoutSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutseparator) : séparer visuellement des éléments de menu.

Cet exemple crée un [MenuFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout) et utilise la propriété [ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout) (qui est disponible pour la plupart des contrôles) pour afficher le MenuFlyout sous la forme d’un menu contextuel.

````xaml
<Rectangle
  Height="100" Width="100">
  <Rectangle.ContextFlyout>
    <MenuFlyout>
      <MenuFlyoutItem Text="Change color" Click="ChangeColorItem_Click" />
    </MenuFlyout>
  </Rectangle.ContextFlyout>
  <Rectangle.Fill>
    <SolidColorBrush x:Name="rectangleFill" Color="Red" />
  </Rectangle.Fill>
</Rectangle>
````

````csharp
private void ChangeColorItem_Click(object sender, RoutedEventArgs e)
{
    // Change the color from red to blue or blue to red.
    if (rectangleFill.Color == Windows.UI.Colors.Red)
    {
        rectangleFill.Color = Windows.UI.Colors.Blue;
    }
    else
    {
        rectangleFill.Color = Windows.UI.Colors.Red;
    }
}
````

L’exemple qui suit est presque identique, mais au lieu d’utiliser la propriété [ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout) pour afficher la [classe MenuFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout) sous forme de menu contextuel, l’exemple utilise la propriété [FlyoutBase.ShowAttachedFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout) pour l’afficher sous forme de menu.

````xaml
<Rectangle
  Height="100" Width="100"
  Tapped="Rectangle_Tapped">
  <FlyoutBase.AttachedFlyout>
    <MenuFlyout>
      <MenuFlyoutItem Text="Change color" Click="ChangeColorItem_Click" />
    </MenuFlyout>
  </FlyoutBase.AttachedFlyout>
  <Rectangle.Fill>
    <SolidColorBrush x:Name="rectangleFill" Color="Red" />
  </Rectangle.Fill>
</Rectangle>
````

````csharp
private void Rectangle_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);
}

private void ChangeColorItem_Click(object sender, RoutedEventArgs e)
{
    // Change the color from red to blue or blue to red.
    if (rectangleFill.Color == Windows.UI.Colors.Red)
    {
        rectangleFill.Color = Windows.UI.Colors.Blue;
    }
    else
    {
        rectangleFill.Color = Windows.UI.Colors.Red;
    }
}
````

### <a name="light-dismiss"></a>Abandon interactif

Les contrôles d’abandon interactif, tels que les menus, les menus contextuels et les menus volants, interceptent le focus du clavier et du boîtier de commande à l’intérieur de l’interface utilisateur temporaire, jusqu’à le faire disparaître. Pour fournir une indication visuelle de ce comportement, les contrôles de la Xbox permettant de faire disparaître la luminosité dessinent une superposition qui assombrit l’interface utilisateur hors de portée. Ce comportement peut être modifié à l’aide de la propriété [LightDismissOverlayMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode). Par défaut, les interfaces utilisateur temporaires dessinent la superposition de l’abandon interactif sur Xbox (**Automatique**), mais pas sur les autres familles d’appareils. Vous pouvez forcer la superposition à être toujours **Activée** ou **Désactivée**.

```xaml
<MenuFlyout LightDismissOverlayMode="Off" />
```

## <a name="create-a-menu-bar"></a>Créer une barre de menus

> [!IMPORTANT]
> MenuBar nécessite Windows 10 version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou ultérieure, ou la [bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Vous utilisez les mêmes éléments pour créer les menus d’une barre de menus que pour créer un menu volant. Toutefois, au lieu de regrouper les objets MenuFlyoutItem dans un MenuFlyout, vous les regroupez dans un élément MenuBarItem. Chaque MenuBarItem est ajouté au MenuBar en tant que menu de niveau supérieur.

![Exemple de barre de menus](images/menu-bar-submenu.png)

> [!NOTE]
> Cet exemple montre uniquement comment créer la structure de l’interface utilisateur, mais ne montre pas comment implémenter les commandes.

```xaml
<muxc:MenuBar>
    <muxc:MenuBarItem Title="File">
        <MenuFlyoutSubItem Text="New">
            <MenuFlyoutItem Text="Plain Text Document"/>
            <MenuFlyoutItem Text="Rich Text Document"/>
            <MenuFlyoutItem Text="Other Formats..."/>
        </MenuFlyoutSubItem>
        <MenuFlyoutItem Text="Open..."/>
        <MenuFlyoutItem Text="Save"/>
        <MenuFlyoutSeparator />
        <MenuFlyoutItem Text="Exit"/>
    </muxc:MenuBarItem>

    <muxc:MenuBarItem Title="Edit">
        <MenuFlyoutItem Text="Undo"/>
        <MenuFlyoutItem Text="Cut"/>
        <MenuFlyoutItem Text="Copy"/>
        <MenuFlyoutItem Text="Paste"/>
    </muxc:MenuBarItem>

    <muxc:MenuBarItem Title="View">
        <MenuFlyoutItem Text="Output"/>
        <MenuFlyoutSeparator/>
        <muxc:RadioMenuFlyoutItem Text="Landscape" GroupName="OrientationGroup"/>
        <muxc:RadioMenuFlyoutItem Text="Portrait" GroupName="OrientationGroup" IsChecked="True"/>
        <MenuFlyoutSeparator/>
        <muxc:RadioMenuFlyoutItem Text="Small icons" GroupName="SizeGroup"/>
        <muxc:RadioMenuFlyoutItem Text="Medium icons" IsChecked="True" GroupName="SizeGroup"/>
        <muxc:RadioMenuFlyoutItem Text="Large icons" GroupName="SizeGroup"/>
    </muxc:MenuBarItem>

    <muxc:MenuBarItem Title="Help">
        <MenuFlyoutItem Text="About"/>
    </muxc:MenuBarItem>
</muxc:MenuBar>
```

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemple de galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) : affichez tous les contrôles XAML dans un format interactif.
- [Exemple de menu contextuel XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu)

## <a name="related-articles"></a>Articles connexes

- [MenuFlyout, classe](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout)
- [MenuBar, classe](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubar)
