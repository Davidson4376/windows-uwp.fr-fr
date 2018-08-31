---
author: mijacobs
Description: A flyout is a lightweight popup that is used to temporarily show UI that is related to what the user is currently doing.
title: Menus et menus contextuels
label: Menus and context menus
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: 0327d8c1-8329-4be2-84e3-66e1e9a0aa60
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e38e9d61e8546d412cc30bad26680243f3a188e4
ms.sourcegitcommit: 1e5590dd10d606a910da6deb67b6a98f33235959
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2018
ms.locfileid: "3241747"
---
# <a name="menus-and-context-menus"></a>Menus et menus contextuels



Les menus et les menus contextuels affichent une liste de commandes ou d’options lorsque l’utilisateur les demande.

> **API importantes**: [classe MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout), [propriété ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx), [propriété FlyoutBase.AttachedFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx)

![Exemple de menu contextuel standard](images/contextmenu_rs2_icons.png)


## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?
Les menus et les menus contextuels permettent de gagner de l’espace en organisant les commandes et en les masquant jusqu’à ce que l’utilisateur en ait besoin. Si une commande particulière doit être utilisée fréquemment et que vous disposez de l’espace disponible, envisagez de le placer directement dans son propre élément, plutôt que dans un menu, afin que les utilisateurs n’aient pas à passer par un menu afin d’y accéder.

Les menus et menus contextuels permettent d’organiser les commandes. Pour afficher du contenu arbitraire, comme une notification ou demander confirmation, utilisez une [boîte de dialogue ou un menu volant](dialogs.md).  

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

Les menus et menus contextuels sont identiques en termes d’affichage et de contenu. En fait, vous utilisez le même contrôle nommé [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030) pour les créer. La seule différence est la façon dont vous permettez à l’utilisateur d’y accéder.

Quand utiliser un menu ou un menu contextuel?
* Si l’élément hôte est un bouton ou un autre élément de commande dont le principal rôle est d’afficher des commandes supplémentaires, utilisez un menu.
* Si l’élément hôte est un autre type d’élément ayant un autre objectif principal (par exemple, la présentation de texte ou une image), utilisez un menu contextuel.

Par exemple, utilisez un menu sur un bouton dans un volet de navigation pour fournir des options de navigation supplémentaires. Dans ce scénario, l’objectif principal du contrôle de bouton consiste à fournir un accès à un menu.

![Exemple de menu dans Mail](images/Mail_Menu.png)

Si vous voulez ajouter des commandes (telles que couper, copier et coller) à un élément de texte, utilisez un menu contextuel au lieu d’un menu. Dans ce scénario, le rôle principal de l’élément de texte est de présenter et de modifier du texte; les commandes supplémentaires (par exemple, couper, copier et coller) sont secondaires et doivent figurer dans un menu contextuel.

![Exemple de menu contextuel dans la galerie de photos](images/ContextMenu_example.png) 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>Les menus:</b></p>
<ul>
<li>Ont un point d’entrée unique (un menu Fichier en haut de l’écran, par exemple) qui est toujours affiché.</li>
<li>Sont généralement attachés à un bouton ou un élément de menu parent.</li>
<li>Sont appelés en cliquant avec le bouton gauche de la souris (ou par le biais d’une action équivalente, telles que l’appui avec votre doigt).</li><li>Sont associés à un élément via ses propriétés <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx">Flyout</a> ou <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx">FlyoutBase.AttachedFlyout</a>.</li>
</ul>
</div>
  <div class="side-by-side-content-right">
   <p><b>Les menus contextuels:</b></p>

<ul>
<li>Sont attachés à un élément unique et affichent les commandes secondaires.</li>
<li>Sont appelés en cliquant avec le bouton droit de la souris (ou par le biais d’une action équivalente, telle que l’appui prolongé avec votre doigt).</li><li>Sont associés à un élément via sa propriété <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx">ContextFlyout</a>.</li>
</ul>
  </div>
</div>
</div>

## <a name="icons"></a>Icônes

Envisagez de fournir des icônes d’élément de menu pour:

<ul>
<li> Les éléments les plus fréquemment utilisés </li>
<li> Les éléments de menu dont l’icône est standard ou bien connue </li>
<li> Les éléments de menu dont l’icône illustre convenablement ce que fait la commande </li>
</ul>

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
> La taille des icônes de MenuFlyoutItems est de 16x16px. Si vous utilisez SymbolIcon, FontIcon ou PathIcon, la taille de l’icône s’ajustera automatiquement sans perte de fidélité. Si vous utilisez BitmapIcon, assurez-vous que la taille de votre ressource est de 16x16px.  

## <a name="create-a-menu-or-a-context-menu"></a>Créer un menu ou un menu contextuel

Pour créer un menu ou un menu contextuel, vous utilisez la [classe MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030). Vous définissez le contenu du menu en ajoutant des objets [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx), [ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) et [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx) à MenuFlyout. Voici le rôle de ces objets:
* [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx): effectuer une action immédiate.
* [ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx): activer ou désactiver une option.
* [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx): séparer visuellement des éléments de menu.


Cet exemple crée une [classe MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030) et utilise la propriété [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx), disponible pour la plupart des contrôles, pour afficher la [classe MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030) sous la forme d’un menu contextuel.

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


> Les contrôles permettant de faire disparaître les contrôles, tels que des menus, des menus contextuels et des menus volants, interceptent le focus du clavier et du boîtier de commande à l’intérieur de l’interface utilisateur temporaire jusqu’à le faire disparaître. Pour fournir une indication visuelle de ce comportement, les contrôles de la Xbox permettant de faire disparaître la luminosité dessinent une superposition qui assombrit l’interface utilisateur hors de portée. Ce comportement peut être modifié à l’aide de la propriété [LightDismissOverlayMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode.aspx). Par défaut, les interfaces utilisateur temporaires dessinent la superposition permettant de faire disparaître la luminosité sur laXbox (**Auto**), mais pas sur d’autres familles d’appareils. Toutefois, les applications peuvent choisir de forcer la superposition afin d’être toujours **activées** ou **désactivées**.

> ```xaml
> <MenuFlyout LightDismissOverlayMode="Off" />
> ```

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.
- [Exemple de menu contextuel XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu)

## <a name="related-articles"></a>Articles associés

- [ClasseMenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030)
