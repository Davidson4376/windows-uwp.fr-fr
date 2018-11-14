---
author: mijacobs
Description: Menus and context menus display a list of commands or options when the user requests them.
title: Menus et menus contextuels
label: Menus and context menus
template: detail.hbs
ms.author: mijacobs
ms.date: 10/02/2018
ms.topic: article
keywords: windows10, uwp
ms.assetid: 0327d8c1-8329-4be2-84e3-66e1e9a0aa60
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 7a2b58ef505c4b6d045197dee525c5264a7dd518
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/08/2018
ms.locfileid: "6159379"
---
# <a name="menus-and-context-menus"></a>Menus et menus contextuels

Les menus et les menus contextuels affichent une liste de commandes ou d’options lorsque l’utilisateur les demande. Utilisez un menu volant pour afficher un seul, menu inline. Utilisez une barre de menus pour afficher un ensemble de menus dans une ligne horizontale, généralement en haut d’une fenêtre d’application. Chaque menu peut avoir des sous-menus et des éléments de menu.

![Exemple de menu contextuel standard](images/contextmenu_rs2_icons.png)

| **Obtenir la bibliothèque d’interface utilisateur Windows** |
| - |
| Ce contrôle est inclus dans le cadre de la bibliothèque d’interface utilisateur de Windows, un package NuGet qui contient les nouveaux contrôles et les fonctionnalités de l’interface utilisateur pour les applications UWP. Pour plus d’informations, y compris les instructions d’installation, consultez la [vue d’ensemble de la bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

| **API de la plateforme** | **API de bibliothèque de l’interface utilisateur Windows** |
| - | - |
| [Classe MenuFlyout](/uwp/api/windows.ui.xaml.controls.menuflyout), [classe de barre de menus](/uwp/api/windows.ui.xaml.controls.menubar), de la [propriété ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout), [propriété FlyoutBase.AttachedFlyout](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout) | [Classe de barre de menus](/uwp/api/microsoft.ui.xaml.controls.menubar) |

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

Les menus et les menus contextuels permettent de gagner de l’espace en organisant les commandes et en les masquant jusqu’à ce que l’utilisateur en ait besoin. Si une commande particulière doit être utilisée fréquemment et que vous disposez de l’espace disponible, envisagez de le placer directement dans son propre élément, plutôt que dans un menu, afin que les utilisateurs n’aient pas à passer par un menu afin d’y accéder.

Menus et menus contextuels permettent d’organiser les commandes; Pour afficher du contenu arbitraire, par exemple, une demande de notification ou une confirmation, utilisez une [boîte de dialogue ou un menu volant](dialogs.md).

### <a name="menubar-vs-menuflyout"></a>Barre de menus ou MenuFlyout

Pour afficher un menu dans un menu volant joint à un élément d’interface utilisateur sur les canevas, utilisez le contrôle MenuFlyout pour héberger vos éléments de menu. Vous pouvez appeler un menu volant sous la forme d’un menu standard ou un menu contextuel. Un menu volant héberge un menu de niveau supérieur unique (et sous-menus facultatifs).

Pour afficher un ensemble de plusieurs menus de niveau supérieur dans une ligne horizontale, utilisez une barre de menus. En règle générale, vous placez la barre de menus en haut de la fenêtre d’application.

### <a name="menubar-vs-commandbar"></a>Barre de menus ou CommandBar

Barre de menus et CommandBar tous deux représentent des surfaces que vous pouvez utiliser pour exposer des commandes à vos utilisateurs. La barre de menus fournit un moyen simple et rapide pour exposer un ensemble de commandes pour les applications qui peuvent requérir plus organisation ou regroupement que permet un contrôle CommandBar.

Vous pouvez également utiliser une barre de menus en conjonction avec un contrôle CommandBar. Utilisez la barre de menus de fournir la majeure partie des commandes et le contrôle CommandBar pour mettre en évidence les commandes les plus utilisées.

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/MenuFlyout">ouvrir l’application et voir l'objet MenuFlyout en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="menus-vs-context-menus"></a>Menus par rapport aux menus contextuels

Menus et menus contextuels sont similaires dans leur apparence et ce qu’ils contiennent. En fait, vous pouvez utiliser le même contrôle [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030), pour les créer. La différence est la façon dont vous permettez à l’utilisateur y accéder.

Quand utiliser un menu ou un menu contextuel?

- Si l’élément hôte est un bouton ou un autre élément de commande dont le principal rôle est d’afficher des commandes supplémentaires, utilisez un menu.
- Si l’élément hôte est un autre type d’élément ayant un autre objectif principal (par exemple, la présentation de texte ou une image), utilisez un menu contextuel.

Par exemple, utilisez un menu sur un bouton pour fournir des options pour obtenir la liste de tri et de filtrage. Dans ce scénario, l’objectif principal du contrôle de bouton consiste à fournir un accès à un menu.

![Exemple de menu dans Mail](images/Mail_Menu.png)

Si vous voulez ajouter des commandes (telles que couper, copier et coller) à un élément de texte, utilisez un menu contextuel au lieu d’un menu. Dans ce scénario, le rôle principal de l’élément de texte est de présenter et de modifier du texte; les commandes supplémentaires (par exemple, couper, copier et coller) sont secondaires et doivent figurer dans un menu contextuel.

![Exemple de menu contextuel dans la galerie de photos](images/ContextMenu_example.png)

### <a name="menus"></a>Les menus:

- Ont un point d’entrée unique (un menu Fichier en haut de l’écran, par exemple) qui est toujours affiché.
- Sont généralement attachés à un bouton ou un élément de menu parent.
- Sont appelés en cliquant avec le bouton gauche de la souris (ou par le biais d’une action équivalente, telles que l’appui avec votre doigt).
- Sont associés à un élément via ses propriétés de [menu volant](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx) ou [FlyoutBase.AttachedFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx) , ou regroupés dans une barre de menus en haut de la fenêtre d’application.

### <a name="context-menus"></a>Les menus contextuels:

- Sont attachés à un élément unique et affichent les commandes secondaires.
- Sont appelés en cliquant avec le bouton droit de la souris (ou par le biais d’une action équivalente, telle que l’appui prolongé avec votre doigt).
- Sont associés à un élément via sa propriété [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx).

## <a name="icons"></a>Icônes

Envisagez de fournir des icônes d’élément de menu pour:

- Les éléments les plus couramment utilisés.
- Éléments de menu dont l’icône est standard ou bien connue.
- Éléments de menu dont l’icône illustre convenablement ce que fait la commande.

Ne vous sentez pas obligé de fournir des icônes pour les commandes qui n’ont pas de visualisation standard. Les icônes incompréhensibles ne sont pas utiles: elles créent un encombrement visuel et empêchent les utilisateurs de se concentrer sur les éléments de menu importants.

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
> La taille de l’icône dans un MenuFlyoutItem est de 16 x 16 px. Si vous utilisez SymbolIcon, FontIcon ou PathIcon, l’icône s’adapte automatiquement à la taille correcte sans perte de fidélité. Si vous utilisez BitmapIcon, assurez-vous que la taille de votre ressource est de 16x16px.  

## <a name="create-a-menu-flyout-or-a-context-menu"></a>Créer un menu volant ou un menu contextuel

Pour créer un menu volant ou un menu contextuel, vous utilisez la [classe MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030). Vous définissez le contenu du menu en ajoutant des objets [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx), [ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) et [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx) à MenuFlyout.

Voici le rôle de ces objets:

- [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx): effectuer une action immédiate.
- [ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx): activer ou désactiver une option.
- [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx): séparer visuellement des éléments de menu.

Cet exemple crée un [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030) et utilise la propriété [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) , une propriété disponible pour la plupart des contrôles, pour afficher le MenuFlyout en tant qu’un menu contextuel.

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

L’exemple qui suit est presque identique. Cependant, au lieu d’utiliser la propriété [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) pour afficher la [classeMenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030) sous forme de menu contextuel, l’exemple utilise la propriété [FlyoutBase.ShowAttachedFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout) pour l’afficher sous forme de menu.

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

Les contrôles permettant de faire disparaître les contrôles, tels que des menus, des menus contextuels et des menus volants, interceptent le focus du clavier et du boîtier de commande à l’intérieur de l’interface utilisateur temporaire jusqu’à le faire disparaître. Pour fournir une indication visuelle de ce comportement, les contrôles de la Xbox permettant de faire disparaître la luminosité dessinent une superposition qui assombrit l’interface utilisateur hors de portée. Ce comportement peut être modifié à l’aide de la propriété [LightDismissOverlayMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode.aspx). Par défaut, les interfaces utilisateur temporaires dessinent la superposition permettant de faire disparaître la luminosité sur laXbox (**Auto**), mais pas sur d’autres familles d’appareils. Toutefois, les applications peuvent choisir de forcer la superposition afin d’être toujours **activées** ou **désactivées**.

```xaml
<MenuFlyout LightDismissOverlayMode="Off" />
```

## <a name="create-a-menu-bar"></a>Créer une barre de menus

> **Version d’évaluation**: barre de menus nécessite [dernière build de Windows 10 Insider Preview et Kit de développement](https://insider.windows.com/for-developers/) ou la [Bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Vous utilisez les mêmes éléments pour créer des menus dans une barre de menus, comme dans un menu volant. Toutefois, au lieu de le regroupement d’objets MenuFlyoutItem dans un MenuFlyout, vous les regrouper dans un élément MenuBarItem. Chaque MenuBarItem est ajouté à la barre de menus comme un menu de niveau supérieur.

![Exemple d’une barre de menus](images/menu-bar-submenu.png)

> [!NOTE]
> Cet exemple indique uniquement comment créer la structure de l’interface utilisateur, mais n’affiche pas de mise en œuvre d’une des commandes.

```xaml
<MenuBar>
    <MenuBarItem Title="File">
        <MenuFlyoutSubItem Text="New">
            <MenuFlyoutItem Text="Plain Text Document"/>
            <MenuFlyoutItem Text="Rich Text Document"/>
            <MenuFlyoutItem Text="Other Formats..."/>
        </MenuFlyoutSubItem>
        <MenuFlyoutItem Text="Open..."/>
        <MenuFlyoutItem Text="Save">
        <MenuFlyoutSeparator />
        <MenuFlyoutItem Text="Exit"/>
    </MenuBarItem>

    <MenuBarItem Title="Edit">
        <MenuFlyoutItem Text="Undo"/>
        <MenuFlyoutItem Text="Cut"/>
        <MenuFlyoutItem Text="Copy"/>
        <MenuFlyoutItem Text="Paste"/>
    </MenuBarItem>

    <MenuBarItem Title="Help">
        <MenuFlyoutItem Text="About"/>
    </MenuBarItem>
</MenuBar>
```

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.
- [Exemple de menu contextuel XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu)

## <a name="related-articles"></a>Articles associés

- [ClasseMenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030)
- [Classe de barre de menus](/uwp/api/Windows.UI.Xaml.Controls.MenuBar)
