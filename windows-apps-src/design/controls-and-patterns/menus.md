---
Description: Les menus et les menus contextuels affichent une liste de commandes ou d’options lorsque l’utilisateur les demande.
title: Menus et menus contextuels
label: Menus and context menus
template: detail.hbs
ms.date: 01/08/2019
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 0327d8c1-8329-4be2-84e3-66e1e9a0aa60
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: d3ea8e2bff2455340a1183dbe5c1840fdb599d46
ms.sourcegitcommit: 7a1d5198345d114c58287d8a047eadc4fe10f012
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59247187"
---
# <a name="menus-and-context-menus"></a>Menus et menus contextuels

Les menus et les menus contextuels affichent une liste de commandes ou d’options lorsque l’utilisateur les demande. Utiliser un menu volant pour afficher un menu de ligne unique. Utilisez une barre de menus pour afficher un ensemble de menus dans une ligne horizontale, généralement en haut d’une fenêtre d’application. Chaque menu peut avoir des sous-menus et des éléments de menu.

![Exemple de menu contextuel standard](images/contextmenu_rs2_icons.png)

| **Obtenir la bibliothèque d’interface utilisateur Windows** |
| - |
| Ce contrôle est inclus dans le cadre de la bibliothèque d’interface utilisateur de Windows, un package NuGet qui contient les nouveaux contrôles et fonctionnalités de l’interface utilisateur pour les applications UWP. Pour plus d’informations, y compris les instructions d’installation, consultez le [vue d’ensemble de la bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

| **API de plateforme** | **API de bibliothèque de l’interface utilisateur de Windows** |
| - | - |
| [Classe de MenuFlyout](/uwp/api/windows.ui.xaml.controls.menuflyout), [classe de barre de menus](/uwp/api/windows.ui.xaml.controls.menubar), [ContextFlyout propriété](/uwp/api/windows.ui.xaml.uielement.contextflyout), [FlyoutBase.AttachedFlyout propriété](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx) | [Classe de la barre de menus](/uwp/api/microsoft.ui.xaml.controls.menubar) |

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Les menus et les menus contextuels permettent de gagner de l’espace en organisant les commandes et en les masquant jusqu’à ce que l’utilisateur en ait besoin. Si une commande particulière doit être utilisée fréquemment et que vous disposez de l’espace disponible, envisagez de le placer directement dans son propre élément, plutôt que dans un menu, afin que les utilisateurs n’aient pas à passer par un menu afin d’y accéder.

Menus contextuels et les menus permettent d’organiser les commandes ; Pour afficher du contenu arbitraire, telle qu’une demande de notification ou une confirmation, utilisez un [boîte de dialogue ou un menu volant](dialogs.md).

### <a name="menubar-vs-menuflyout"></a>Visual Studio de barre de menus. MenuFlyout

Pour afficher un menu dans un menu volant attaché à un élément d’interface utilisateur sur le canevas, utilisez le contrôle MenuFlyout pour héberger vos éléments de menu. Vous pouvez appeler un menu volant sous la forme d’un menu standard ou un menu contextuel. Un menu volant héberge un menu de niveau supérieur unique (et les sous-menus facultatifs).

Pour afficher un ensemble de plusieurs menus de niveau supérieur dans une ligne horizontale, utilisez une barre de menus. En règle générale, vous positionnez la barre de menus en haut de la fenêtre d’application.

### <a name="menubar-vs-commandbar"></a>Visual Studio de barre de menus. CommandBar

Barre de menus et CommandBar tous deux représentent des surfaces qui vous permettent d’exposer des commandes à vos utilisateurs. La barre de menus fournit un moyen rapide et simple pour exposer un ensemble de commandes pour les applications nécessitant une organisation plus ou le regroupement que ne le permet un CommandBar.

Vous pouvez également utiliser une barre de menus conjointement avec un CommandBar. Utilisez la barre de menus pour fournir la majeure partie des commandes et le CommandBar pour mettre en surbrillance les commandes les plus utilisées.

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/MenuFlyout">ouvrir l’application et voir l'objet MenuFlyout en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="menus-vs-context-menus"></a>Menus par rapport aux menus contextuels

Menus et des menus contextuels sont similaires dans quoi ils ressemblent et ce qu’ils contiennent. En fait, vous pouvez utiliser le même contrôle, [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030), pour les créer. La différence est la façon dont vous laisser l’utilisateur y accéder.

Quand utiliser un menu ou un menu contextuel ?

- Si l’élément hôte est un bouton ou un autre élément de commande dont le rôle principal est de présenter des commandes supplémentaires, utilisez un menu.
- Si l’élément hôte est un autre type d’élément ayant un autre objectif principal (par exemple, la présentation de texte ou une image), utilisez un menu contextuel.

Par exemple, utiliser un menu sur un bouton pour fournir des options pour obtenir la liste de tri et le filtrage. Dans ce scénario, l’objectif principal du contrôle de bouton consiste à fournir un accès à un menu.

![Exemple de menu dans Mail](images/Mail_Menu.png)

Si vous voulez ajouter des commandes (telles que couper, copier et coller) à un élément de texte, utilisez un menu contextuel au lieu d’un menu. Dans ce scénario, le rôle principal de l’élément de texte est de présenter et de modifier du texte ; les commandes supplémentaires (par exemple, couper, copier et coller) sont secondaires et doivent figurer dans un menu contextuel.

![Exemple de menu contextuel dans la galerie de photos](images/ContextMenu_example.png)

### <a name="menus"></a>Les menus :

- Ont un point d’entrée unique (un menu Fichier en haut de l’écran, par exemple) qui est toujours affiché.
- Sont généralement attachés à un bouton ou un élément de menu parent.
- Sont appelés en cliquant avec le bouton gauche de la souris (ou par le biais d’une action équivalente, telles que l’appui avec votre doigt).
- Sont associés à un élément via sa [Flyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx) ou [FlyoutBase.AttachedFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx) propriétés, ou regroupés dans une barre de menus en haut de la fenêtre d’application.

### <a name="context-menus"></a>Menus contextuels

- Sont attachés à un élément unique et affichent les commandes secondaires.
- Sont appelés en cliquant avec le bouton droit de la souris (ou par le biais d’une action équivalente, telle que l’appui prolongé avec votre doigt).
- Sont associés à un élément via sa propriété [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx).

## <a name="icons"></a>Icônes

Envisagez de fournir des icônes d’élément de menu pour :

- Les éléments fréquemment utilisés.
- Éléments de menu dont l’icône est standard ou bien connu.
- Éléments de menu dont l’icône illustre bien ce que fait la commande.

Ne vous sentez pas obligé de fournir des icônes pour les commandes qui n’ont pas de visualisation standard. Les icônes incompréhensibles ne sont pas utiles : elles créent un encombrement visuel et empêchent les utilisateurs de se concentrer sur les éléments de menu importants.

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
> La taille de l’icône dans un MenuFlyoutItem est 16x16px. Si vous utilisez SymbolIcon, FontIcon ou PathIcon, l’icône s’ajuste automatiquement à la taille correcte sans perte de fidélité. Si vous utilisez BitmapIcon, assurez-vous que la taille de votre ressource est de 16 x 16 px.  

## <a name="create-a-menu-flyout-or-a-context-menu"></a>Créer un menu volant ou un menu contextuel

Pour créer un menu volant ou un menu contextuel, vous utilisez le [MenuFlyout classe](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout). Vous définissez le contenu du menu en ajoutant [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutitem), [MenuFlyoutSubItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutsubitem), [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem), [RadioMenuFlyoutItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem)et [MenuFlyoutSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutseparator) objets à le MenuFlyout.

Voici le rôle de ces objets :

- [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutitem) : effectuer une action immédiate.
- [MenuFlyoutSubItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutsubitem), contenant une liste d’éléments de menu en cascade.
- [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem) : activer ou désactiver une option.
- [RadioMenuFlyoutItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem): basculer entre les éléments de menu de mutuellement exclusives.
- [MenuFlyoutSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutseparator) : séparer visuellement des éléments de menu.

Cet exemple crée un [MenuFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout) et utilise le [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) propriété, une propriété disponible pour la plupart des contrôles pour afficher le MenuFlyout en tant qu’un menu contextuel.

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

L’exemple qui suit est presque identique, mais au lieu d’utiliser la propriété [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) pour afficher la [classe MenuFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout) sous forme de menu contextuel, l’exemple utilise la propriété [FlyoutBase.ShowAttachedFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout) pour l’afficher sous forme de menu.

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

### <a name="light-dismiss"></a>Ignorer légers

Légers ignorer les contrôles tels que les menus, les menus contextuels et les autres menus volants, intercepter à l’intérieur de l’interface utilisateur temporaire jusqu'à ce que le focus clavier et le boîtier de commande. Pour fournir une indication visuelle de ce comportement, les contrôles de la Xbox permettant de faire disparaître la luminosité dessinent une superposition qui assombrit l’interface utilisateur hors de portée. Ce comportement peut être modifié à l’aide de la propriété [LightDismissOverlayMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode.aspx). Par défaut, des interfaces utilisateur temporaires Dessine la superposition dismiss clair sur Xbox (**automatique**) mais pas d’autres familles de périphériques. Vous pouvez choisir de forcer la superposition d’être toujours **sur** ou toujours **hors**.

```xaml
<MenuFlyout LightDismissOverlayMode="Off" />
```

## <a name="create-a-menu-bar"></a>Créer une barre de menus

> [!IMPORTANT]
> Barre de menus nécessite Windows 10, version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou version ultérieure, ou le [bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Vous utilisez les mêmes éléments pour créer des menus dans une barre de menus, comme dans un menu volant. Toutefois, au lieu de le regroupement d’objets MenuFlyoutItem dans un MenuFlyout, vous les regrouper dans un élément MenuBarItem. Chaque MenuBarItem est ajouté à la barre de menus en tant qu’un menu de niveau supérieur.

![Exemple d’une barre de menus](images/menu-bar-submenu.png)

> [!NOTE]
> Cet exemple montre uniquement comment créer la structure de l’interface utilisateur, mais n’affiche pas d’implémentation d’une des commandes.

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

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) - Affichez tous les contrôles XAML dans un format interactif.
- [Exemple de menu contextuel XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu)

## <a name="related-articles"></a>Articles connexes

- [Classe MenuFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout)
- [Classe de la barre de menus](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubar)
