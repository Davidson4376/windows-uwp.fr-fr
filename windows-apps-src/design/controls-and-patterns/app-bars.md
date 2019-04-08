---
Description: Les barres de commandes permettent aux utilisateurs d’accéder facilement aux tâches les plus courantes de votre application.
title: Barre de commandes
label: App bars/command bars
template: detail.hbs
op-migration-status: ready
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 868b4145-319b-4a97-82bd-c98d966144db
pm-contact: yulikl
design-contact: ksulliv
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 3d2a7d34f00d40429863f08ffe6a9c34222daa32
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649304"
---
# <a name="command-bar"></a>Barre de commandes

Les barres de commandes permettent aux utilisateurs d'accéder facilement aux tâches les plus courantes de votre application. Les barres de commandes donnent accès à des commandes au niveau de l’application ou spécifiques à la page et peuvent être utilisées avec n’importe quel modèle de navigation.

> **API importantes** : [Classe de CommandBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx), [AppBarButton classe](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarbutton.aspx), [AppBarToggleButton classe](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbartogglebutton.aspx), [AppBarSeparator classe](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarseparator.aspx)

![Exemple d’une barre de commandes contenant des icônes](images/controls_appbar_icons.png)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Le contrôle CommandBar est un contrôle à usage général, flexible et léger qui permet d’afficher du contenu complexe, comme des images, des barres de progression ou des blocs de texte, ainsi que des commandes simples, comme des contrôles [AppBarButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarbutton.aspx), [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbartogglebutton.aspx) et [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.appbarseparator.aspx).

> [!NOTE]
> XAML fournit les contrôles [AppBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar) et [CommandBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar). Utilisez le contrôle AppBar uniquement lorsque vous mettez à niveau une application Windows 8 universelle qui utilise ce contrôle et si vous avez besoin de réduire les modifications. Pour les nouvelles applications dans Windows 10, nous vous recommandons plutôt d’utiliser le contrôle CommandBar. Ce document part du principe que vous utilisez le contrôle CommandBar.

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/CommandBar">ouvrir l’application et voir le contrôle CommandBar en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Une barre de commandes développée dans l’application Photos Microsoft.

![Barre de commandes dans l’application Photos Microsoft](images/control-examples/command-bar-photos.png)

Une barre de commandes dans le Calendrier Outlook sur Windows Phone.

![Barre de commandes dans l’application Calendrier Outlook](images/control-examples/command-bar-calendar-phone.png)

## <a name="anatomy"></a>Anatomie

Par défaut, la barre de commandes affiche une ligne de boutons d’icônes et un bouton de « voir plus » facultatif, qui est représenté par des points de suspension \[•••\]. Voici la barre de commandes créée par l’exemple de code présenté plus loin. Elle est présentée à l’état compact et fermé.

![Barre de commandes fermée](images/command-bar-compact.png)

La barre de commandes peut également être affichée à l’état fermé minimal comme ce qui suit : Voir la section [États ouvert et fermé](#open-and-closed-states) pour plus d’informations.

![Barre de commandes fermée](images/command-bar-minimal.png)

Voici la même barre de commandes à l’état ouvert. Les étiquettes identifient les principales parties du contrôle.

![Barre de commandes fermée](images/commandbar_anatomy_open.png)

La barre de commandes est divisée en 4 zones principales :
- La zone de contenu est alignée sur le côté gauche de la barre. Elle s’affiche si la propriété [Content](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx) est remplie.
- La zone de commande principale est alignée à droite de la barre. Elle s’affiche si la propriété [PrimaryCommands](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.primarycommands.aspx) est remplie.  
- Le « voir » \[•••\] est affiché sur la droite de la barre. En appuyant sur la « voir » \[•••\] bouton révèle la commande principale des étiquettes et ouvre le menu de dépassement de capacité s’il existe des commandes secondaire. Le bouton ne sera pas visible si aucun libellé de commande principale ou secondaire n'est présent. Pour modifier le comportement par défaut, utilisez la propriété [OverflowButtonVisibility](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.overflowbuttonvisibility.aspx).
- Le menu de dépassement est visible uniquement quand la barre de commandes est ouverte et si la propriété [SecondaryCommands](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.secondarycommands.aspx) est remplie. Lorsque l’espace est limité, les commandes principales se déplacent dans la zone SecondaryCommands. Pour modifier le comportement par défaut, utilisez la propriété [IsDynamicOverflowEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.isdynamicoverflowenabled.aspx).

La disposition est inversée lorsque [FlowDirection](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.flowdirection.aspx) est défini sur **RightToLeft**.

## <a name="create-a-command-bar"></a>Créer une barre de commandes
Cet exemple permet de créer la barre de commandes illustrée précédemment.

```xaml
<CommandBar>
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle" Click="AppBarButton_Click" />
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat" Click="AppBarButton_Click"/>
    <AppBarSeparator/>
    <AppBarButton Icon="Back" Label="Back" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Stop" Label="Stop" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Play" Label="Play" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Forward" Label="Forward" Click="AppBarButton_Click"/>

    <CommandBar.SecondaryCommands>
        <AppBarButton Label="Like" Click="AppBarButton_Click"/>
        <AppBarButton Label="Dislike" Click="AppBarButton_Click"/>
    </CommandBar.SecondaryCommands>

    <CommandBar.Content>
        <TextBlock Text="Now playing..." Margin="12,14"/>
    </CommandBar.Content>
</CommandBar>
```

## <a name="commands-and-content"></a>Commandes et contenu
Le contrôle CommandBar a 3 propriétés que vous pouvez utiliser pour ajouter des commandes et contenu : [PrimaryCommands](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.primarycommands.aspx), [SecondaryCommands](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.secondarycommands.aspx), et [contenu](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx).


### <a name="commands"></a>Commandes

Par défaut, les éléments de la barre de commandes sont ajoutés à la collection **PrimaryCommands**. Vous devez ajouter les commandes par ordre d'importance afin que les commandes principales soient toujours visibles. Quand la barre de commandes change de largeur, par exemple quand les utilisateurs redimensionnent la fenêtre de leur application, les commandes principales se déplacent dynamiquement entre la barre de commandes et le menu de dépassement aux points d’arrêt. Pour modifier ce comportement par défaut, utilisez la propriété [IsDynamicOverflowEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.isdynamicoverflowenabled.aspx). 

Sur les écrans plus petits (largeur de 320 epx), la barre de commandes peut contenir un maximum de 4 commandes principales. 

Vous pouvez également ajouter des commandes à la collection **SecondaryCommands**. Celles-ci s'afficheront dans le menu de dépassement.

![Exemple d’une barre de commandes comportant une zone et des icônes « Plus »](images/appbar_rs2_overflow_icons.png)

Vous pouvez déplacer par programme les commandes entre les propriétés PrimaryCommands et SecondaryCommands en fonction des besoins.

- *S’il existe une commande qui apparaîtrait de manière cohérente sur plusieurs pages, il est préférable de conserver cette commande dans un emplacement similaire.*
- *Nous vous recommandons de placer les accepter, Oui, et les commandes OK à gauche de refuser, non et annulent. Cohérence offre aux utilisateurs la confiance nécessaire pour déplacer le système et leur permet de transférer leurs connaissances de navigation de l’application à partir d’une application à l’application.*

### <a name="app-bar-buttons"></a>Boutons de la barre de l’application

Les propriétés PrimaryCommands et SecondaryCommands peuvent être remplies uniquement avec les éléments de commande [AppBarButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx), [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx), et [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx). 

Les contrôles des boutons de la barre de l’application sont caractérisés par une icône et une étiquette avec un libellé. Ces contrôles sont optimisés pour une utilisation dans une barre de commandes, et leur apparence change selon que le contrôle est utilisé dans la barre de commandes ou le menu de dépassement.

La taille des icônes du menu de dépassement est de 16 x 16 px, soit une taille inférieure à celles des icônes de la zone de commande principale (20 x 20 px). Si vous utilisez SymbolIcon, FontIcon ou PathIcon, la taille de l’icône s’ajustera automatiquement sans perte de fidélité lorsque la commande entrera dans la zone de commande secondaire. 

### <a name="button-labels"></a>Libellé des boutons
La propriété [IsCompact](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton.IsCompact) d'AppBarButton détermine si l’étiquette s’affiche. Dans un contrôle CommandBar, la barre de commandes remplace automatiquement la propriété IsCompact du bouton lorsque la barre de commandes est ouverte et fermée.

Pour positionner les étiquettes des boutons de la barre d’application, utilisez la propriété [DefaultLabelPosition](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.defaultlabelposition.aspx) du contrôle CommandBar.

```xaml
<CommandBar DefaultLabelPosition="Right">
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle"/>
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat"/>
</CommandBar>
```

![Barre de commandes avec étiquettes sur la droite](images/app-bar-labels-on-right.png)

Sur les grandes fenêtres, envisagez de déplacer les étiquettes à droite des icônes de bouton de la barre d’application pour améliorer la lisibilité. Les étiquettes du bas obligent les utilisateurs à ouvrir la barre de commandes pour révéler les étiquettes, tandis que les étiquettes sur la droite sont visibles même lorsque la barre de commandes est fermée.

Dans les menus de dépassement, les étiquettes sont positionnées à droite des icônes par défaut, et **LabelPosition** est ignorée. Vous pouvez ajuster les styles en définissant la propriété [CommandBarOverflowPresenterStyle](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar.CommandBarOverflowPresenterStyle) sur un Style qui cible la propriété [CommandBarOverflowPresenter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbaroverflowpresenter). 

Les étiquettes des boutons doivent être courtes, de préférence un seul mot. Les libellés longs sous une icône s’étaleront sur plusieurs lignes, ce qui augmente la hauteur totale de la barre de commandes ouverte. Vous pouvez inclure un trait d’union conditionnel (0x00AD) dans le libellé pour indiquer l’endroit où une césure doit intervenir. En XAML, cela s’exprime par une séquence d’échappement, telle que celle qui suit :

```xaml
<AppBarButton Icon="Back" Label="Areally&#x00AD;longlabel"/>
```

Lorsque le libellé est renvoyé à la ligne à l’emplacement indiqué, cela se présente comme suit.

![Bouton de la barre de l’application avec renvoi à la ligne du libellé](images/app-bar-button-label-wrap.png)

### <a name="command-bar-flyouts"></a>Menus volants de barre de commandes

Envisagez le regroupement logique des commandes ; placez par exemple les commandes Répondre, Répondre à tous et Transférer dans un menu Répondre. Un bouton de barre de l’application active en règle générale une seule commande ; il peut cependant être utilisé pour afficher des classes [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.menuflyout.aspx) ou [Flyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flyout.aspx) avec du contenu personnalisé.

![Exemple de menus volants d’une barre de commandes](images/AppbarGuidelines_Flyouts.png)

### <a name="other-content"></a>Autre contenu

Vous pouvez ajouter n’importe quel élément XAML à la zone de contenu en définissant la propriété **Content**. Si vous voulez ajouter plusieurs éléments, vous devez les placer dans un conteneur de panneaux et faire en sorte que le panneau soit le seul enfant de la propriété Content.

Quand le dépassement dynamique est activé, le contenu n’est pas tronqué car les commandes principales peuvent se déplacer dans le menu de dépassement. Dans le cas contraire, les commandes principales sont prioritaires et peuvent provoquer la troncature du contenu.

Lorsque [ClosedDisplayMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closeddisplaymode.aspx) est défini sur **Compact**, le contenu peut être tronqué s’il est plus grand que la taille compacte de la barre de commandes. Vous devez gérer les événements [Opening](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opening.aspx) et [Closed](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closed.aspx) pour afficher ou masquer des parties de l’interface utilisateur dans la zone de contenu afin qu’elles ne soient pas tronquées. Voir la section [États ouvert et fermé](#open-and-closed-states) pour plus d’informations.


## <a name="open-and-closed-states"></a>États ouvert et fermé

La barre de commandes peut être ouverte ou fermée. Lorsque la barre de commandes est ouverte, les boutons de commandes principales s’affichent avec les libellés et le menu de dépassement s'ouvre en présence de commandes secondaires.

Un utilisateur peut basculer entre ces États en appuyant sur la « voir » \[•••\] bouton. Vous pouvez basculer d’un état à l’autre par programme en définissant la propriété [IsOpen](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.isopen.aspx). 

Vous pouvez utiliser les événements [Opening](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opening.aspx), [Opened](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.opened.aspx), [Closing](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closing.aspx), et [Closed](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closed.aspx) pour répondre à l’ouverture et à la fermeture de la barre de commandes.  
- Les événements Opening et Closing se produisent avant le début de l’animation de transition.
- Les événements Opened et Closed se produisent après la transition.

Dans cet exemple, les événements Opening et Closing permettent de modifier l’opacité de la barre de commandes. Lorsque la barre de commandes est fermée, celle-ci devient semi-transparente révélant l’arrière-plan de l’application. Lorsque la barre de commandes est ouverte, celle-ci devient opaque de sorte que l’utilisateur puisse se concentrer sur les commandes.

```xaml
<CommandBar Opening="CommandBar_Opening"
            Closing="CommandBar_Closing">
    <AppBarButton Icon="Accept" Label="Accept"/>
    <AppBarButton Icon="Edit" Label="Edit"/>
    <AppBarButton Icon="Save" Label="Save"/>
    <AppBarButton Icon="Cancel" Label="Cancel"/>
</CommandBar>
```

```csharp
private void CommandBar_Opening(object sender, object e)
{
    CommandBar cb = sender as CommandBar;
    if (cb != null) cb.Background.Opacity = 1.0;
}

private void CommandBar_Closing(object sender, object e)
{
    CommandBar cb = sender as CommandBar;
    if (cb != null) cb.Background.Opacity = 0.5;
}

```

### <a name="issticky"></a>IsSticky

Si un utilisateur interagit avec d’autres parties d’une application lorsqu’une barre de commandes est ouverte, cette dernière se ferme automatiquement. Cela s'appelle un *abandon interactif*. Vous pouvez contrôler le comportement d’abandon interactif en définissant la propriété [IsSticky](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.issticky.aspx). Lorsque `IsSticky="true"`, la barre reste ouverte jusqu'à ce que l’utilisateur appuie sur le « voir » \[•••\] bouton ou sélectionne un élément dans le menu de dépassement de capacité. 

Nous vous recommandons d’éviter les barres de commande rémanentes, car elles ne répondent pas aux attentes des utilisateurs en matière d’abandon interactif.

### <a name="display-mode"></a>Mode d’affichage

Vous pouvez contrôler la façon dont la barre de commandes est affichée à l’état fermé en définissant la propriété [ClosedDisplayMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.closeddisplaymode.aspx). Vous avez le choix entre 3 modes d’affichage lorsque la barre est fermée :
- **Compact**: Le mode par défaut. Affiche le contenu, les icônes de commande principal sans les étiquettes et le « voir » plus \[•••\] bouton.
- **Minimale**: Affiche uniquement une fine barre qui joue le rôle du « voir plus » \[•••\] bouton. L’utilisateur peut appuyer n’importe où sur la barre pour l’ouvrir.
- **Masqué** : La barre de commandes n’est pas affichée quand il est fermé. Cela peut être utile pour afficher des commandes contextuelles avec une barre de commandes en ligne. Dans ce cas, vous devez ouvrir la barre de commandes par programme en définissant la propriété **IsOpen** ou en modifiant la propriété ClosedDisplayMode pour la définir sur **Minimal** ou **Compact**.

Ici, une barre de commandes est utilisée pour contenir des commandes de mise en forme simples pour un élément [RichEditBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx). Lorsque la zone d’édition n’a pas le focus, les commandes de mise en forme peuvent devenir gênantes, c’est pourquoi elles sont masquées. Lorsque la zone d’édition est utilisée, la propriété ClosedDisplayMode de la barre de commandes est définie sur Compact afin que les commandes de mise en forme soient visibles.

```xaml
<StackPanel Width="300"
            GotFocus="EditStackPanel_GotFocus"
            LostFocus="EditStackPanel_LostFocus">
    <CommandBar x:Name="FormattingCommandBar" ClosedDisplayMode="Hidden">
        <AppBarButton Icon="Bold" Label="Bold" ToolTipService.ToolTip="Bold"/>
        <AppBarButton Icon="Italic" Label="Italic" ToolTipService.ToolTip="Italic"/>
        <AppBarButton Icon="Underline" Label="Underline" ToolTipService.ToolTip="Underline"/>
    </CommandBar>
    <RichEditBox Height="200"/>
</StackPanel>
```

```csharp
private void EditStackPanel_GotFocus(object sender, RoutedEventArgs e)
{
    FormattingCommandBar.ClosedDisplayMode = AppBarClosedDisplayMode.Compact;
}

private void EditStackPanel_LostFocus(object sender, RoutedEventArgs e)
{
    FormattingCommandBar.ClosedDisplayMode = AppBarClosedDisplayMode.Hidden;
}
```

>**Remarque**&nbsp;&nbsp; Cet exemple ne porte pas sur l’implémentation des commandes d’édition. Pour plus d’informations, voir l’article [RichEditBox](rich-edit-box.md).

Bien que les modes Minimal et Hidden soient utiles dans certaines situations, n’oubliez pas que le masquage de toutes les actions peut prêter à confusion.

La modification de la propriété ClosedDisplayMode pour fournir plus ou moins d’indications à l’utilisateur affecte la disposition des éléments environnants. En revanche, lorsque CommandBar passe de l’état fermé à ouvert ou inversement, cela n’affecte pas la disposition des autres éléments.

## <a name="placement"></a>Sélection élective
Les barres de commandes peuvent être placées en haut et en bas de la fenêtre d’application et en ligne.

![Exemple 1 de placement de la barre d’application](images/AppbarGuidelines_Placement1.png)

-   Pour les petits appareils de poche, nous vous recommandons de positionner les barres de commande en bas de l’écran pour faciliter leur accessibilité.
-   Pour les écrans plus grands, le fait de placer les barres de commande près du haut de la fenêtre les rend plus détectables et plus accessibles.

Utilisez l’API [DiagonalSizeInInches](https://msdn.microsoft.com/library/windows/apps/windows.graphics.display.displayinformation.diagonalsizeininches.aspx) pour déterminer la taille physique de l’écran.

Les barres de commandes peuvent être placées dans les zones d’écran suivantes sur les écrans à vue unique (exemple à gauche) et sur les écrans à vues multiples (exemple à droite). Les barres de commandes incorporées peuvent être placées n’importe où dans la zone d’action.

![Exemple 2 de placement de la barre de l’application](images/AppbarGuidelines_Placement2.png)

>**Appareils tactiles**: Si la barre de commandes doit rester visible pour l’utilisateur lorsque le clavier tactile ou réversible entrée du panneau SIP, qui s’affiche vous pouvez affecter à la barre de commandes pour le [BottomAppBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.bottomappbar.aspx) propriété d’une Page et il passera à restent visibles lorsque le panneau SIP est présent . Dans le cas contraire, vous devez placer la barre de commandes de sorte qu’elle soit alignée et positionnée par rapport au contenu de votre application.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) - Affichez tous les contrôles XAML dans un format interactif.
- [Exemples de commandes de XAML](https://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="related-articles"></a>Articles connexes

* [Principes fondamentaux de conception de commande pour les applications UWP](../basics/commanding-basics.md)
* [Classe de CommandBar](https://msdn.microsoft.com/library/windows/apps/dn279427)
