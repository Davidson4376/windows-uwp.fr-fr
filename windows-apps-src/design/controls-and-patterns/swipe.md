---
pm-contact: kisai
design-contact: ksulliv
dev-contact: Shmazlou
doc-status: Published
Description: Swipe commanding is a touch accelerator for context menus.
title: Balayage
label: Swipe
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a75723177e697d3fe4cdae270aba29eabcabf470
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8921533"
---
# <a name="swipe"></a>Balayage

Les commandes de balayage sont des accélérateurs de menus contextuels qui permettent aux utilisateurs d'accéder facilement aux actions courantes des menus sans avoir à modifier les états au sein de l’application.

> **API importantes**: [SwipeControl](/uwp/api/windows.ui.xaml.controls.swipecontrol), [SwipeItem](/uwp/api/windows.ui.xaml.controls.swipeitem), [classe ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView)

![Exécuter et afficher le thème de luminosité](images/LightThemeSwipe.png)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

Les commandes de balayage économisent de l’espace. C'est utile dans les situations où l’utilisateur peut répéter la même opération sur plusieurs éléments en succession rapide. Fournit des «actions rapides» sur les éléments qui ne nécessitent pas un changement complet d'état ou de fenêtre contextuelle sur la page.

Vous devez utiliser les commandes de balayage lorsque vous disposez d’un groupe d’éléments potentiellement élevé et que chacun d'eux a entre 1et 3actions qu'un utilisateur peut vouloir effectuer plusieurs fois. Ces actions peuvent comprendre les éléments suivants:

- Suppression
- Marquage ou archivage
- Enregistrement ou téléchargement
- Réponse

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/SwipeControl">ouvrir l’application et voir l'objet SwipeControl en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>Résumé de la vidéo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev015/player]

## <a name="how-does-swipe-work"></a>Comment le balayage fonctionne-t-il?

Les commandes de balayage UWP possèdent deux modes: [Révéler](/uwp/api/windows.ui.xaml.controls.swipemode) et [Exécuter](/uwp/api/windows.ui.xaml.controls.swipemode). Elles prennent également en charge quatre directions de balayage différentes: haut, bas, gauche et droite.

### <a name="reveal-mode"></a>Mode Révéler

Dans le mode Révéler, l’utilisateur balaie un élément pour ouvrir un menu comportant une ou plusieurs commandes et doit appuyez explicitement sur une commande pour l'exécuter. Lorsque l’utilisateur balaie et relâche un élément, le menu reste ouvert jusqu'à ce qu’une commande soit sélectionnée ou que le menu soit refermé par un nouveau balayage, en appuyant pour le désactiver ou en faisant défiler l’élément balayé hors de l’écran.

![Effectuer un balayage pour révéler](images/SwipeCommand-Reveal_v2.gif)

Révéler est un mode de balayage plus sûr et plus souple, et peut être utilisé pour la plupart des types d’actions de menu, même des actions potentiellement destructrices, telles que la suppression.

Lorsque l'utilisateur sélectionne une des options de menu affichées dans l'état ouvert et au repos du mode révéler, la commande de cet élément est appelée et le contrôle de balayage est fermé.

### <a name="execute-mode"></a>Mode Exécuter

Dans le mode Exécuter, l’utilisateur balaie un élément ouvert pour révéler et exécuter une commande unique à l'aide de ce balayage. Si l’utilisateur relâche l’élément balayé avant de l'avoir balayé au-delà d’un certain seuil, le menu se referme et la commande n’est pas exécutée. Si l’utilisateur fait glisser l’élément au-delà du seuil et le relâche ensuite, la commande est exécutée immédiatement.

![Effectuer un balayage pour exécuter](images/SwipeCommand_Delete_v2.gif)

Si l'utilisateur ne relâche pas le doigt une fois que le seuil est atteint et referme l’élément balayé, la commande n'est pas exécutée et l'action est effectuée sur l’élément.

Le mode d'exécution fournit une information plus visuelle grâce à la couleur et l’orientation de l’étiquette pendant qu'un élément est balayé.

Il est préférable d’utiliser le mode d'exécution lorsque l’action exécutée par l’utilisateur est très courante.

Elles peuvent également être utilisées pour des actions plus destructrices, telles que la suppression d’un élément. Toutefois, n’oubliez pas qu'Exécuter ne nécessite qu’une seule action de balayage dans une direction, par opposition à Révéler, ce qui nécessite que l’utilisateur clique explicitement sur un bouton.

### <a name="swipe-directions"></a>Directions de balayage

Le balayage fonctionne dans toutes les directions cardinales: haut, bas, gauche et droite. Chaque direction de balayage peut détenir ses propres éléments de balayage ou son contenu, mais une seule instance d’une direction peut être définie à la fois sur un même élément compatible avec le balayage.

Par exemple, vous ne pouvez pas avoir deux définitions de [LeftItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.LeftItems) pour le même SwipeControl.

## <a name="how-to-create-a-swipe-command"></a>Procédure de création d’une commande de balayage

Les commandes de balayage ont deux composants que vous devez définir:

- Le [SwipeControl](/uwp/api/windows.ui.xaml.controls.swipecontrol), qui s'enroule autour de votre contenu. Dans une collection, par exemple, un contrôle ListView, il se trouve au sein de votre DataTemplate.
- Les éléments du menu balayage, qui consistent en un ou plusieurs objets [SwipeItem](/uwp/api/windows.ui.xaml.controls.swipeitem) placés dans les conteneurs directionnels du contrôle balayage: [LeftItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.LeftItems), [RightItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.RightItems), [TopItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.TopItems) ou [BottomItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.BottomItems)

Le contenu de balayage peut être placé en ligne ou défini dans la section Ressources de votre application ou page.

Voici un exemple simple de SwipeControl enroulé autour d'un texte. Il montre la hiérarchie des éléments XAML requis pour créer une commande de balayage.

```xaml
<SwipeControl HorizontalAlignment="Center" VerticalAlignment="Center">
    <SwipeControl.LeftItems>
        <SwipeItems>
            <SwipeItem Text="Pin">
                <SwipeItem.IconSource>
                    <SymbolIconSource Symbol="Pin"/>
                </SwipeItem.IconSource>
            </SwipeItem>
        </SwipeItems>
    </SwipeControl.LeftItems>

     <!-- Swipeable content -->
    <Border Width="180" Height="44" BorderBrush="Black" BorderThickness="2">
        <TextBlock Text="Swipe to Pin" Margin="4,8,0,0"/>
    </Border>
</SwipeControl>
```

Maintenant, nous allons examiner un exemple plus complet de la façon d'utiliser généralement les commandes de balayage dans une liste. Dans cet exemple, vous allez configurer une commande de suppression qui utilise le mode Exécuter, et un menu d'autres commandes qui utilise le mode Révéler. Les deux ensembles de commandes sont définis dans la section Ressources de la page. Vous allez appliquer les commandes de balayage aux éléments dans un contrôle ListView.

Tout d’abord, créez les éléments de balayage, qui représentent les commandes, en tant que ressources au niveau page. SwipeItem utilise un [IconSource](/uwp/api/windows.ui.xaml.controls.iconsource) comme icône. Créez les icônes en tant que ressources, également.

```xaml
<Page.Resources>
    <SymbolIconSource x:Key="ReplyIcon" Symbol="MailReply"/>
    <SymbolIconSource x:Key="DeleteIcon" Symbol="Delete"/>
    <SymbolIconSource x:Key="PinIcon" Symbol="Pin"/>

    <SwipeItems x:Key="RevealOptions" Mode="Reveal">
        <SwipeItem Text="Reply" IconSource="{StaticResource ReplyIcon}"/>
        <SwipeItem Text="Pin" IconSource="{StaticResource PinIcon}"/>
    </SwipeItems>

    <SwipeItems x:Key="ExecuteDelete" Mode="Execute">
        <SwipeItem Text="Delete" IconSource="{StaticResource DeleteIcon}"
                   Background="Red"/>
    </SwipeItems>
</Page.Resources>
```

N’oubliez pas d'utiliser des libellés courts et concis pour les éléments de menu de votre contenu de balayage. Ces actions doivent être les actions principales qu’un utilisateur peut vouloir effectuer plusieurs fois sur une courte période.

Configurer une commande de balayage pour qu'elle fonctionne dans une collection ou un contrôle ListView se fait exactement de la même manière que pour définir une commande de balayage unique (expliqué précédemment), sauf que vous définissez votre SwipeControl dans un DataTemplate pour qu'il s'applique à chaque élément de la collection.

Voici un contrôle ListView avec le SwipeControl appliqué dans son DataTemplate d'éléments. Les propriétés LeftItems et RightItems référencent les éléments de balayage que vous avez créés en tant que ressources.

```xaml
<ListView x:Name="sampleList" Width="300">
    <ListView.ItemContainerStyle>
        <Style TargetType="ListViewItem">
            <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
            <Setter Property="VerticalContentAlignment" Value="Stretch"/>
        </Style>
    </ListView.ItemContainerStyle>
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="x:String">
            <SwipeControl x:Name="ListViewSwipeContainer"
                          LeftItems="{StaticResource RevealOptions}"
                          RightItems="{StaticResource ExecuteDelete}"
                          Height="60">
                <StackPanel Orientation="Vertical">
                    <TextBlock Text="{x:Bind}" FontSize="18"/>
                    <StackPanel Orientation="Horizontal">
                        <TextBlock Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit..." FontSize="12"/>
                    </StackPanel>
                </StackPanel>
            </SwipeControl>
        </DataTemplate>
    </ListView.ItemTemplate>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```

## <a name="handle-an-invoked-swipe-command"></a>Gérer une commande de balayage appelée

Pour agir sur une commande de balayage, vous gérez son événement [Appelé](/uwp/api/windows.ui.xaml.controls.swipeitem.Invoked). (Pour plus d’informations sur la façon dont un utilisateur peut appeler une commande, consultez la section _Comment le balayage fonctionne-t-il?_ plus haut dans cet article.) En règle générale, la commande de balayage est placée dans un contrôle ListView ou un scénario de liste. Dans ce cas, lorsqu’une commande est appelée, vous devrez effectuer une action sur cet élément balayé.

Voici comment gérer l’événement Appelé sur l'élément balayage _supprimer_ que vous avez créé précédemment.

```xaml
<SwipeItems x:Key="ExecuteDelete" Mode="Execute">
    <SwipeItem Text="Delete" IconSource="{StaticResource DeleteIcon}"
               Background="Red" Invoked="Delete_Invoked"/>
</SwipeItems>
```

L’élément de données est le DataContext du SwipeControl. Dans votre code, vous pouvez accéder à l’élément qui a été balayé en obtenant la propriété SwipeControl.DataContext à partir des arguments d’événement, comme illustré ici.

```csharp
 private void Delete_Invoked(SwipeItem sender, SwipeItemInvokedEventArgs args)
 {
     sampleList.Items.Remove(args.SwipeControl.DataContext);
 }
```

> [!NOTE]
> Ici, les éléments ont été ajoutés directement à la collection ListView.Items par souci de simplicité, donc l’élément est également supprimé de la même façon. Si vous définissez à la place la ListView.ItemsSource sur une collection, ce qui est plus courant, vous devez supprimer l’élément de la collection source.

Dans cette instance particulière, vous avez supprimé l’élément de la liste, donc l’état visuel final de l’élément balayé n’est pas important. Toutefois, dans les situations où vous souhaitez simplement exécuter une action, puis réduire à nouveau le balayage, vous pouvez régler la propriété [BehaviorOnInvoked](/uwp/api/windows.ui.xaml.controls.swipeitem.BehaviorOnInvoked) sur une des valeurs de l'enum [SwipeBehaviorOnInvoked](/uwp/api/windows.ui.xaml.controls.swipebehavioroninvoked).

- **Automatique**
  - Dans le mode Exécuter, l’élément de balayage ouvert reste ouvert lorsqu’il est appelé.
  - Dans le mode Révéler, l’élément de balayage ouvert est réduit lorsqu’il est appelé.
- **Fermer**
  - Lorsque l’élément est appelé, le contrôle de balayage est toujours réduit et revient à la normale, quel que soit le mode.
- **RemainOpen**
  - Lorsque l’élément est appelé, le contrôle de balayage reste toujours ouvert, quel que soit le mode.

Ici, un élément de balayage _réponse_ est configuré pour se fermer une fois qu’il est appelé.

```xaml
<SwipeItem Text="Reply" IconSource="{StaticResource ReplyIcon}"
           Invoked="Reply_Invoked"
           BehaviorOnInvoked = "Close"/>
```

## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées

- N’utilisez pas de balayage dans les FlipViews, les concentrateurs ou les sélecteurs de vue. La combinaison pourrait prêter à confusion pour l’utilisateur en raison de conflits de directions de balayage.
- Ne combinez pas le balayage horizontal et la navigation horizontale ou le balayage vertical avec la navigation verticale.
- Veillez bien à ce que l'action de balayage effectuée par l’utilisateur soit la même et reste cohérente sur tous les éléments qui peuvent être balayés.
- Utilisez le balayage pour les actions principales qu'un utilisateur souhaitera effectuer.
- Utilisez le balayage sur les éléments pour lesquels la même action est répétée plusieurs fois.
- Utilisez le balayage horizontal sur les éléments les plus larges et le balayage vertical sur les plus grands.
- Utilisez des libellés courts et concis.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles associés

- [Affichage Liste et affichage Grille](listview-and-gridview.md)
- [Modèles et conteneurs d’éléments](item-containers-templates.md)
- [Tirer pour actualiser](pull-to-refresh.md)
