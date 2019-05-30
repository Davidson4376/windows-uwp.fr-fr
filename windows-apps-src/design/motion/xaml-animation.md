---
ms.assetid: 0C8DEE75-FB7B-4E59-81E3-55F8D65CD982
title: Vue d’ensemble des animations
description: Utilisez les animations de la bibliothèque d’animations du Windows Runtime pour intégrer l’apparence de Windows dans votre application.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 22e035639709005417084d564145d9de218009a8
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366685"
---
# <a name="animations-in-xaml"></a>Animations en XAML

Les animations UWP peuvent améliorer votre application en la rendant plus vivante et interactive. À l’aide des animations de la bibliothèque d’animations du Windows Runtime, vous pouvez intégrer l’apparence de Windows dans votre application. Cette rubrique présente les animations et des exemples de situations classiques où chacune d’elles est utilisée.

> [!TIP]
> Les contrôles Windows Runtime pour XAML incluent certains types d’animations en tant que comportements intégrés qui proviennent d’une bibliothèque d’animations. En utilisant ces contrôles dans votre application, vous pouvez obtenir l’apparence des animations sans aucune programmation préalable.

Les animations de la bibliothèque d’animations Windows Runtime offrent les avantages suivants :

-   Mouvements conformes au [Recommandations en matière d’animations](https://docs.microsoft.com/windows/uwp/style/motion)
-   transitions rapides et fluides entre les états de l’interface utilisateur qui informent l’utilisateur sans le déranger dans ses activités ;
-   comportement visuel qui indique à l’utilisateur les transitions dans une application.

Par exemple, quand l’utilisateur ajoute un élément à une liste, au lieu que ce nouvel élément apparaisse instantanément dans la liste, il se met en place par animation. Les autres éléments de la liste sont animés vers leur nouvelle position en peu de temps, faisant ainsi de la place pour le nouvel élément. Ce comportement de transition rend l’interaction du contrôle plus apparente à l’utilisateur.

Windows 10, version 1607 introduit une nouvelle API [**ConnectedAnimationService**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice) pour l’implémentation d’animations où un élément animé s’affiche entre les vues pendant une navigation. Cette API possède un modèle d’utilisation différente de celui des autres API de la bibliothèque d’animations. L’utilisation de **ConnectedAnimationService** est traitée dans la [page de référence](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice).

La bibliothèque d’animations ne fournit pas des animations pour chaque scénario possible. Il peut arriver que vous souhaitiez créer une animation personnalisée en XAML. Pour plus d’informations, voir [Animations dans une table de montage séquentiel](storyboarded-animations.md).

En outre, pour certains scénarios avancés, tels que l’animation d’un élément en fonction de la position de défilement d’une visionneuse à défilement, les développeurs peuvent utiliser l’interopération de couche visuelle afin d’implémenter des animations personnalisées. Pour plus d’informations, voir [Couche visuelle](https://docs.microsoft.com/windows/uwp/composition/visual-layer).

## <a name="types-of-animations"></a>Types d’animations

La bibliothèque d’animations et le système d’animations Windows Runtime ont pour but majeur de permettre aux contrôles et autres parties de l’interface utilisateur d’avoir un comportement animé. Il existe plusieurs types distincts d’animations.

-   Les *transitions thématiques* sont appliquées automatiquement quand certaines conditions sont modifiées dans l’interface utilisateur, impliquant des éléments ou contrôles provenant des types d’interface utilisateur XAML Windows Runtime prédéfinis. Elles sont appelées *transitions thématiques*, car elles prennent en charge l’apparence Windows et définissent les actions de toutes les applications pour des scénarios d’interface utilisateur particuliers quand elles passent d’un mode d’interaction à un autre. Les transitions thématiques font partie de la bibliothèque d’animations.
-   Les *animations thématiques* sont des animations appliquées à une ou plusieurs propriétés de types d’interface utilisateur XAML Windows Runtime prédéfinis. Elles diffèrent des transitions thématiques car elles ciblent un élément spécifique et existent dans des états visuels spécifiques dans un contrôle, alors que les transitions thématiques sont affectées à des propriétés du contrôle qui existent en dehors des états visuels et influencent les transitions entre ces états. Une grande partie des contrôles XAML Windows Runtime comprennent des animations thématiques dans des storyboards qui font partie de leur modèle de contrôle, les animations étant déclenchées par les états visuels. Du moment que vous ne changez pas les modèles, vous disposez de ces animations thématiques intégrées pour les contrôles dans votre interface utilisateur. En revanche, si vous remplacez des modèles, vous supprimez également les animations thématiques de contrôle intégrées. Pour les récupérer, vous devez définir une table de montage séquentiel qui comprend des animations thématiques dans le jeu d’états visuels du contrôle. Vous pouvez aussi exécuter des animations thématiques à partir de storyboards qui ne sont pas dans des états visuels et les démarrer avec la méthode [**Begin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.begin), mais ce scénario est moins courant. Les animations thématiques font partie de la bibliothèque d’animations.
-   Les *transitions visuelles* sont appliquées quand un contrôle passe de l’un de ses états visuels définis à un autre état. Il s’agit d’animations personnalisées que vous créez et elles sont généralement associées au modèle personnalisé que vous créez pour un contrôle et aux définitions d’état visuel de ce modèle. L’animation est exécutée uniquement pendant le délai entre les états et celui-ci est généralement court, quelques secondes au plus. Pour plus d’informations, voir la [section « VisualTransition » dans Animations dans une table de montage séquentiel pour les états visuels](https://docs.microsoft.com/previous-versions/windows/apps/jj819808(v=win.10)).
-   Les *animations dans une table de montage séquentiel* animent la valeur d’une propriété de dépendance Windows Runtime au fil du temps. Les tables de montage séquentiel peuvent être définies dans le cadre d’une transition visuelle ou déclenchées par l’application lors de l’exécution. Pour plus d’informations, voir [Animations dans une table de montage séquentiel](storyboarded-animations.md). Pour plus d’informations sur les propriétés de dépendance et sur l’emplacement où elles existent, voir [Vue d’ensemble des propriétés de dépendance](https://docs.microsoft.com/windows/uwp/xaml-platform/dependency-properties-overview).
-   Les *animations connectées* fournies par la nouvelle API [**ConnectedAnimationService**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice) permettent aux développeurs de créer facilement un effet où un élément animé s’affiche entre les vues pendant une navigation. Cette API est disponible à partir de Windows 10, version 1607. Pour plus d’informations, voir [**ConnectedAnimationService**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice).

## <a name="animations-available-in-the-library"></a>Animations disponibles dans la bibliothèque

Les animations suivantes sont fournies dans la bibliothèque d’animations. Cliquez sur le nom d’une animation pour en savoir plus sur ses principaux scénarios d’utilisation, comment la définir, ainsi que pour afficher un exemple de l’animation.

-   [Page de transition](#page-transition): Anime les transitions de page dans un [ **Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame).
-   [Transition de contenu et entrée](#content-transition-and-entrance-transition): Anime une partie ou l’ensemble du contenu dans ou hors de la vue.
-   [Atténuation en entrée/sortie et fondu enchaîné](#fade-in-out-and-crossfade): Affiche les éléments temporaires ou des contrôles, ou actualise une zone de contenu.
-   [Pointeur haut/bas](#pointer-up-down): Fournit des commentaires visuels d’un cliquez ou appuyez sur une vignette.
-   [Repositionner](#reposition): Déplace un élément dans une nouvelle position.
-   [Menu contextuel Afficher/masquer](#show-hide-popup): Affiche l’interface utilisateur contextuelle en haut de la vue.
-   [Afficher/masquer le bord de l’interface utilisateur](#show-hide-edge-ui): Les diapositives edge interface utilisateur, y compris les grandes tel qu’un panneau, dans ou hors de la vue.
-   [Modifications de l’élément de liste](#list-item-changes): Ajoute ou supprime un élément de liste, ou la réorganisation des éléments.
-   [Glisser-déplacer](#drag-drop): Fournit des commentaires visuels pendant une opération de glisser-déplacer.

### <a name="page-transition"></a>Transition de page

Utilisez les transitions de page pour animer la navigation au sein d’une application. Étant donné que la plupart des applications utilisent un type de navigation quelconque, les animations de transition de page constituent le type le plus courant d’animation de thème utilisé par les applications. Pour plus d’informations sur les API de transition de page, voir [**NavigationThemeTransition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition)



### <a name="content-transition-and-entrance-transition"></a>Transition de contenu et transition d’entrée

Les animations de transition de contenu ([**ContentThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ContentThemeTransition)) permettent de déplacer des éléments de contenu dans ou en dehors de la vue actuelle. Par exemple, les animations de transition de contenu affichent du contenu qui n’était pas prêt à être affiché quand la page a été chargée pour la première fois, ou quand le contenu d’une section d’une page change.

[**EntranceThemeTransition** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition) représente un mouvement qui permettre s’appliquent au contenu lorsqu’une page ou une grande section de l’interface utilisateur du premier chargement. Ainsi, la première apparence du contenu et un changement de contenu peuvent offrir un retour différent. [**EntranceThemeTransition** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition) équivaut à un [ **NavigationThemeTransition** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) avec les paramètres par défaut, mais peuvent être utilisées en dehors d’un [ **Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame).
 
 
<span id="fade-in-out-and-crossfade"/>

### <a name="fade-inout-and-crossfade"></a>Apparition/disparition en fondu et fondu enchaîné

Utilisez des animations d’apparition et de disparition en fondu pour afficher ou masquer une interface utilisateur ou des contrôles provisoires. En XAML, elles sont représentées comme [**FadeInThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation) et [**FadeOutThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation). Il peut s’agir par exemple d’une barre de l’application dans laquelle de nouveaux contrôles peuvent s’afficher suite à une interaction de l’utilisateur. Un autre exemple concerne un indicateur de mouvement panoramique ou une barre de défilement provisoire qui disparaît progressivement si aucune entrée utilisateur n’a été détectée pendant un certain temps. Les applications doivent également utiliser l’animation d’apparition en fondu pendant la transition d’un élément d’espace réservé à l’élément final durant le chargement dynamique du contenu.

Utilisez une animation de fondu enchaîné pour rendre moins abrupte la transition quand l’état d’un élément change, par exemple quand l’application actualise le contenu actuel d’une vue. La bibliothèque d’animations XAML ne fournit aucune animation de fondu enchaîné dédiée (aucun équivalent pour [**crossFade**](https://docs.microsoft.com/previous-versions/windows/apps/br212661(v=win.10))), mais vous pouvez obtenir le même résultat à l’aide de [**FadeInThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation) et [**FadeOutThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation) avec synchronisation superposée.

<span id="pointer-up-down"/>

### <a name="pointer-updown"></a>Pointeur vers le haut/bas

Utilisez les animations [**PointerUpThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation) et [**PointerDownThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation) pour donner à l’utilisateur un retour d’un appui ou d’un clic réussi sur une vignette. Par exemple, quand un utilisateur clique ou appuie sur une vignette, l’animation de pointeur vers le bas est lue. Quand l’utilisateur cesse de cliquer ou d’appuyer, l’animation de pointeur vers le haut est lue.

### <a name="reposition"></a>Repositionner

Utilisez les animations de repositionnement ([**RepositionThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.RepositionThemeAnimation) ou [**RepositionThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.RepositionThemeTransition)) pour déplacer un élément à un autre endroit. Par exemple, le déplacement des en-têtes dans un contrôle d’éléments a recours à l’animation de repositionnement.

<span id="show-hide-popup"/>

### <a name="showhide-popup"></a>Afficher/Masquer le menu contextuel

Utilisez [**PopInThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation) et [**PopOutThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopOutThemeAnimation) quand vous affichez et masquez un objet [**Popup**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.Popup) ou une interface utilisateur contextuelle similaire par-dessus la vue actuelle. [**PopupThemeTransition** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition) est une transition de thème qui est le retour d’expérience utile si vous souhaitez light faire disparaître une fenêtre contextuelle.

<span id="show-hide-edge-ui"/>

### <a name="showhide-edge-ui"></a>Afficher/Masquer l’interface utilisateur latérale

Utilisez l’animation [**EdgeUIThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EdgeUIThemeTransition)pour faire glisser des éléments d’interface utilisateur latéraux de petite taille dans et en dehors de la vue. Par exemple, utilisez ces animations quand vous affichez une barre de l’application personnalisée en haut ou en bas de l’écran ou une surface d’interface utilisateur pour des erreurs et des avertissements en haut de l’écran.

Utilisez l’animation [**PaneThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PaneThemeTransition) pour afficher ou masquer un volet ou un panneau. Cette animation convient à l’interface utilisateur latérale de grande taille, telle qu’un clavier personnalisé ou un volet de tâches.

### <a name="list-item-changes"></a>Changements d’éléments de listes

Utilisez l’animation [**AddDeleteThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.AddDeleteThemeTransition) pour ajouter un comportement animé quand vous ajoutez ou supprimez un élément dans une liste existante. Pour l’ajout, la transition commence par repositionner les éléments existants dans la liste pour faire de la place aux nouveaux éléments, puis elle ajoute les nouveaux éléments. Pour la suppression, la transition supprime les éléments d’une liste et, si nécessaire, repositionne les éléments de la liste restants quand les éléments supprimés ont été retirés.

Il existe aussi un objet [**ReorderThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ReorderThemeTransition) distinct que vous appliquez si un élément change de position dans une liste. Son animation diffère de la suppression d’un élément et de son ajout à un nouvel emplacement avec les animations d’ajout/suppression associées.

Notez que ces animations sont incluses dans les modèles [**ListView**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) et [**GridView**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview) par défaut. Vous n’avez donc pas besoin d’ajouter ces animations manuellement si vous utilisez déjà ces contrôles.

<span id="drag-drop"/>

### <a name="dragdrop"></a>Glisser-déplacer

Utilisez les animations de glissement ([**DragItemThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DragItemThemeAnimation), [**DragOverThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DragOverThemeAnimation)) et de déplacement ([**DropTargetItemThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DropTargetItemThemeAnimation)) pour donner un retour visuel quand l’utilisateur fait glisser ou dépose un élément.

Quand elles sont actives, les animations montrent à l’utilisateur que la liste peut être réorganisée autour d’un élément déposé. Il est utile pour les utilisateurs de savoir où l’élément va être placé dans une liste s’il est déposé à l’emplacement actuel. Les animations indiquent visuellement qu’un élément que l’utilisateur fait glisser peut être déposé entre deux autres éléments de la liste et que ces éléments vont disparaître.

## <a name="using-animations-with-custom-controls"></a>Utilisation d’animations avec des contrôles personnalisés

Le tableau suivant résume nos recommandations en matière de choix d’animation lors de la création d’une version personnalisée de ces contrôles Windows Runtime :

| Type d’élément d’interface utilisateur | Animation recommandée |
|---------|-----------------------|
| Boîte de dialogue | [**FadeInThemeAnimation** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation) et [ **FadeOutThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation) |
| Menu volant | [**PopInThemeAnimation** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.popinthemeanimation.popinthemeanimation) et [ **PopOutThemeAnimation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.popoutthemeanimation.popoutthemeanimation) |
| Info-bulle | [**FadeInThemeAnimation** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation) et [ **FadeOutThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation) |
| Menu contextuel | [**PopInThemeAnimation** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.popinthemeanimation.popinthemeanimation) et [ **PopOutThemeAnimation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.popoutthemeanimation.popoutthemeanimation) |
| Barre de commandes | [**EdgeUIThemeTransition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.edgeuithemetransition.edgeuithemetransition) |
| Volet de tâches ou panneau latéral | [**PaneThemeTransition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.panethemetransition.panethemetransition) |
| Contenu d’un conteneur d’éléments d’interface utilisateur quelconque | [**ContentThemeTransition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.contentthemetransition.contentthemetransition) |
| Pour les contrôles ou si aucune autre animation ne s’applique | [**FadeInThemeAnimation** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.fadeinthemeanimation.fadeinthemeanimation) et [ **FadeOutThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation) |

 

## <a name="transition-animation-examples"></a>Exemples d’animation de transition

Dans l’idéal, votre application doit faire appel aux animations pour améliorer l’interface utilisateur ou pour la rendre plus attrayante. Une façon de procéder consiste à appliquer des transitions animées à l’élément d’interface utilisateur pour que, lorsqu’un élément entre ou quitte la zone d’affichage ou pour tout autre changement visuel, l’animation attire l’attention de l’utilisateur sur ce changement. Par exemple, vos boutons peuvent apparaître ou disparaître de l’affichage en un fondu progressif et rapide plutôt qu’apparaître et disparaître de façon abrupte sans autre effet. Nous avons créé une série d’API vous permettant de créer des transitions d’animations cohérentes, qu’elles soient recommandées ou classiques. L’exemple présenté ici illustre comment appliquer une animation à un bouton pour le faire apparaître sur la zone d’affichage en le faisant glisser.

```xml
<Button Content="Transitioning Button">
     <Button.Transitions>
         <TransitionCollection> 
             <EntranceThemeTransition/>
         </TransitionCollection>
     </Button.Transitions>
 </Button>
 ```

Dans ce code, nous ajoutons l’objet [**EntranceThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition) à la collection de transitions du bouton. Au moment du premier rendu du bouton, celui-ci vient glisser sur la zone d’affichage plutôt que d’apparaître simplement. Vous pouvez définir quelques propriétés sur l’objet d’animation afin d’ajuster la profondeur et le sens de son glissement, mais le but recherché est une API simple pour un scénario spécifique, plus précisément, pour faire apparaître un contrôle de façon attrayante.

Vous pouvez aussi définir des thèmes d’animation de transition dans les ressources de style de votre application, vous permettant ainsi d’appliquer l’effet uniformément. Cet exemple équivaut au précédent, à la différence qu’il est appliqué par le biais d’un [**Style**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) :

```xml
<UserControl.Resources>
     <Style x:Key="DefaultButtonStyle" TargetType="Button">
         <Setter Property="Transitions">
             <Setter.Value>
                 <TransitionCollection>
                     <EntranceThemeTransition/>
                 </TransitionCollection>
             </Setter.Value>
        </Setter>
    </Style>
</UserControl.Resources>
      
<StackPanel x:Name="LayoutRoot">
    <Button Style="{StaticResource DefaultButtonStyle}" Content="Transitioning Button"/>
</StackPanel>
```

Les exemples précédents appliquent une transition de thème à un seul contrôle. Cependant, les transitions de thème s’avèrent plus intéressantes encore si vous les appliquez à un conteneur d’objets. En procédant ainsi, tous les objets enfants du conteneur participent à la transition. Dans l’exemple suivant, un objet [**EntranceThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition) est appliqué à une grille (représentée par l’objet [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid)) de rectangles.

```xml
<!-- If you set an EntranceThemeTransition animation on a panel, the
     children of the panel will automatically offset when they animate
     into view to create a visually appealing entrance. -->        
<ItemsControl Grid.Row="1" x:Name="rectangleItems">
    <ItemsControl.ItemContainerTransitions>
        <TransitionCollection>
            <EntranceThemeTransition/>
        </TransitionCollection>
    </ItemsControl.ItemContainerTransitions>
    <ItemsControl.ItemsPanel>
        <ItemsPanelTemplate>
            <WrapGrid Height="400"/>
        </ItemsPanelTemplate>
    </ItemsControl.ItemsPanel>
            
    <!-- The sequence children appear depends on their order in 
         the panel's children, not necessarily on where they render
         on the screen. Be sure to arrange your child elements in
         the order you want them to transition into view. -->
    <ItemsControl.Items>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
    </ItemsControl.Items>
</ItemsControl>
```

Les rectangles enfants de l’objet [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) apparaissent par le biais d’une transition, l’un après l’autre et de façon harmonieuse, plutôt que tous ensemble comme ce serait le cas si vous appliquiez cette animation aux rectangles un à un.

Voici une démonstration de cette animation :

![animation montrant un rectangle enfant apparaissant par le biais d’une transition](./images/animation-child-rectangles.gif)

Les objets enfants d’un conteneur peuvent également se réagencer si un ou plusieurs de ces enfants changent de position. Dans l’exemple suivant, nous appliquons un objet [**RepositionThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.RepositionThemeTransition) à une grille de rectangles. Lorsque vous retirez l’un des rectangles, tous les autres se repositionnent en fonction.

```xml
<Button Content="Remove Rectangle" Click="RemoveButton_Click"/>
        
<ItemsControl Grid.Row="1" x:Name="rectangleItems">
    <ItemsControl.ItemContainerTransitions>
        <TransitionCollection>
                    
            <!-- Without this, there would be no animation when items 
                 are removed. -->
            <RepositionThemeTransition/>
        </TransitionCollection>
    </ItemsControl.ItemContainerTransitions>
    <ItemsControl.ItemsPanel>
        <ItemsPanelTemplate>
            <WrapGrid Height="400"/>
        </ItemsPanelTemplate>
    </ItemsControl.ItemsPanel>
            
    <!-- All these rectangles are just to demonstrate how the items
         in the grid re-flow into position when one of the child items
         are removed. -->
    <ItemsControl.Items>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
    </ItemsControl.Items>
</ItemsControl>
```

```cs
private void RemoveButton_Click(object sender, RoutedEventArgs e)
{
    if (rectangleItems.Items.Count > 0)
    {    
        rectangleItems.Items.RemoveAt(0);
    }                         
}
```

```cpp
// .h
private:
void RemoveButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e);

//.cpp
void BlankPage::RemoveButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e)
{
    if (rectangleItems->Items->Size > 0)
    {    
        rectangleItems->Items->RemoveAt(0);
    }
}
```

Vous pouvez appliquer plusieurs animations de transition à un même objet ou conteneur d’objets. Par exemple, si vous souhaitez que les rectangles de la liste s’affichent progressivement, mais aussi qu’ils s’animent quand ils changent de position, vous pouvez appliquer les objets [**RepositionThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.RepositionThemeTransition) et [**EntranceThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition) comme suit :

```xml
...
<ItemsControl.ItemContainerTransitions>
    <TransitionCollection>
        <EntranceThemeTransition/>                    
        <RepositionThemeTransition/>
    </TransitionCollection>
</ItemsControl.ItemContainerTransitions>
...      
```

Plusieurs effets de transition permettent de créer des animations applicables à vos éléments d’interface utilisateur au fur et à mesure de leur ajout, de leur retrait, de leur réorganisation, etc. Les noms de ces API contiennent tous « ThemeTransition » :

| API | Description |
|-----|-------------|
| [**NavigationThemeTransition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) | Fournit une animation de personnalité Windows pour la navigation de page dans un [**cadre**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame). |
| [**AddDeleteThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.AddDeleteThemeTransition) | Fournit le comportement de la transition animée pour les cas où les contrôles déclenchent l’ajout ou la suppression d’enfants ou de contenu. Généralement, le contrôle correspond à un conteneur d’éléments. |
| [**ContentThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ContentThemeTransition) | Fournit le comportement de la transition animée pour les cas où le contenu d’un contrôle change. Vous pouvez l’appliquer en plus de l’objet [**AddDeleteThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.AddDeleteThemeTransition). |
| [**EdgeUIThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EdgeUIThemeTransition) | Fournit le comportement de la transition animée pour la transition d’une (petite) interface utilisateur latérale. |
| [**EntranceThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition) | Fournit le comportement de la transition animée lorsque les contrôles apparaissent pour la première fois. |
| [**PaneThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PaneThemeTransition) | Fournit le comportement de la transition animée pour la transition d’une interface utilisateur de panneau (grande interface utilisateur latérale). |
| [**PopupThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition) | Fournit le comportement de la transition animée qui s’applique aux composants contextuels des contrôles (par exemple, une interface utilisateur semblable à une info-bulle sur un objet) à mesure qu’ils apparaissent. |
| [**ReorderThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ReorderThemeTransition) | Fournit le comportement de la transition animée pour les cas où les éléments des contrôles list-view changent d’ordre. Généralement, cela se produit suite à une opération de glissement. Les différents contrôles et thèmes peuvent présenter des caractéristiques d’animation diverses. |
| [**RepositionThemeTransition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.RepositionThemeTransition) | Fournit le comportement de la transition animée lorsque les contrôles changent de position. |

 

## <a name="theme-animation-examples"></a>Exemples d’animations de thème

Les animations de transition sont simples à appliquer. Vous pouvez cependant être amené à nécessiter un contrôle plus fin sur la synchronisation et sur l’ordre de vos effets d’animation. Vous pouvez ainsi faire appel à des animations de thème pour permettre cela tout en utilisant un thème Windows cohérent avec le comportement de votre animation. Les animations de thème requièrent également moins de balisage que les animations personnalisées. Nous utilisons ici l’objet [**FadeOutThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation) pour faire disparaître progressivement un rectangle de la zone d’affichage.

```xml
<StackPanel>    
    <StackPanel.Resources>
        <Storyboard x:Name="myStoryboard">
            <FadeOutThemeAnimation TargetName="myRectangle" />
        </Storyboard>
    </StackPanel.Resources>
    <Rectangle PointerPressed="Rectangle_Tapped" x:Name="myRectangle"  
              Fill="Blue" Width="200" Height="300"/>
</StackPanel>
```

```cs
// When the user taps the rectangle, the animation begins.
private void Rectangle_Tapped(object sender, PointerRoutedEventArgs e)
{
    myStoryboard.Begin();
}
```

```vb
' When the user taps the rectangle, the animation begins.
Private Sub Rectangle_Tapped(sender As Object, e As PointerRoutedEventArgs)
    myStoryboard.Begin()
End Sub
```

```cpp
//.h
void Rectangle_Tapped(Platform::Object^ sender, Windows::UI::Xaml::Input::PointerRoutedEventArgs^ e);

//.cpp
void BlankPage::Rectangle_Tapped(Object^ sender, PointerRoutedEventArgs^ e)
{
    myStoryboard->Begin();
}
```

Contrairement aux animations de transition, une animation de thème n’a pas de déclencheur intégré (la transition) qui l’exécute automatiquement. Vous devez utiliser un objet [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) pour contenir une animation thématique quand vous la définissez en XAML. Vous pouvez aussi changer le comportement par défaut de l’animation. Par exemple, il vous est possible de ralentir la disparition progressive en augmentant la valeur de temps [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration) de l’objet [**FadeOutThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation).

**Remarque**  à des fins d’affichage des techniques d’animation de base, nous utilisons code d’application pour démarrer l’animation en appelant des méthodes de [ **Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard). Vous pouvez contrôler comment la **Storyboard** animations exécutées à l’aide la [ **commencer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.begin), [ **arrêter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.stop), [ **Pause**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.pause), et [ **Resume** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.resume) **Storyboard** méthodes. Toutefois, ça n’est pas comme cela qu’on inclut normalement des animations de la bibliothèque dans des applications. En général, on intègre plutôt les animations de la bibliothèque dans les modèles et styles XAML appliqués aux contrôles ou aux éléments. L’apprentissage des modèles et des états visuels est un peu plus compliqué. Vous trouverez cependant une description de l’utilisation des animations de la bibliothèque dans les états visuels dans la rubrique [Animations dans une table de montage séquentiel pour les états visuels](https://docs.microsoft.com/previous-versions/windows/apps/jj819808(v=win.10)).

 

Vous pouvez appliquer plusieurs autres animations de thème à vos éléments d’interface utilisateur pour créer des effets d’animation. Les noms de ces API contiennent tous « ThemeAnimation » :

| API | Description |
|-----|-------------|
| [**DragItemThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DragItemThemeAnimation) | Représente l’animation préconfigurée qui s’applique aux éléments déplacés par glissement. |
| [**DragOverThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DragOverThemeAnimation) | Représente l’animation préconfigurée qui s’applique aux éléments figurant sous un élément déplacé par glissement. |
| [**DropTargetItemThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DropTargetItemThemeAnimation) | Animation préconfigurée qui s’applique à des éléments pouvant représenter des cibles de dépôt. |
| [**FadeInThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation) | Animation d’opacité préconfigurée qui s’applique aux contrôles au moment où ils apparaissent pour la première fois. |
| [**FadeOutThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation) | Animation d’opacité préconfigurée qui s’applique aux contrôles quand ils sont supprimés ou masqués de l’interface utilisateur. |
| [**PointerDownThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation) | Animation préconfigurée pour l’action de l’utilisateur où il appuie ou clique sur un élément. |
| [**PointerUpThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation) | Animation préconfigurée pour l’action de l’utilisateur qui s’exécute quand l’utilisateur appuie sur un élément et que l’action est déclenchée. |
| [**PopInThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation) | Animation préconfigurée qui s’applique à des composants contextuels de contrôles au fur et à mesure qu’ils apparaissent. Cette animation allie opacité et translation. |
| [**PopOutThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopOutThemeAnimation) | Animation préconfigurée qui s’applique à des composants contextuels de contrôles au fur et à mesure de leur fermeture ou de leur suppression. Cette animation allie opacité et translation. |
| [**RepositionThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.RepositionThemeAnimation) | Animation préconfigurée pour un objet au fur et à mesure de son repositionnement. |
| [**SplitCloseThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.SplitCloseThemeAnimation) | Animation préconfigurée, telle qu’une ouverture et fermeture [**ComboBox**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox), qui masque une interface utilisateur cible. |
| [**SplitOpenThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.SplitOpenThemeAnimation) | Animation préconfigurée, telle qu’une ouverture et fermeture [**ComboBox**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox), qui affiche une interface utilisateur cible. |
| [**DrillInThemeAnimation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.drillinthemeanimation) | Représente une animation préconfigurée qui s’exécute lorsqu’un utilisateur navigue vers l’avant dans une hiérarchie logique, par exemple, d’une page maître vers une page de détails. |
| [**DrillOutThemeAnimation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.drilloutthemeanimation) | Représente une animation préconfigurée qui s’exécute lorsqu’un utilisateur navigue en arrière dans une hiérarchie logique, par exemple, d’une page de détails vers une page maître. |

 

## <a name="create-your-own-animations"></a>Créer vos propres animations

Au cas où les animations de thème ne répondent pas à vos besoins, vous pouvez créer vos propres animations. Vous animez des objets en animant les valeurs d’une ou de plusieurs de leurs propriétés. Par exemple, vous pouvez animer la largeur d’un rectangle, l’angle d’un [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform) ou la valeur de la couleur d’un bouton. Ce type d’animation personnalisée porte le nom d’animation de table de montage séquentiel, pour le distinguer des animations de la bibliothèque déjà fournies par Windows Runtime comme type d’animation préconfiguré. Pour les animations de table de montage séquentiel, vous utilisez une animation qui peut changer les valeurs d’un type particulier (par exemple [**DoubleAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DoubleAnimation) pour animer un **Double**) et placer cette animation dans un [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) pour la contrôler.

Afin d’être animée, la propriété que vous animez doit être une *propriété de dépendance*. Pour plus d’informations sur les propriétés de dépendance, voir [Vue d’ensemble des propriétés de dépendance](https://docs.microsoft.com/windows/uwp/xaml-platform/dependency-properties-overview). Pour plus d’informations sur la création d’animations personnalisées dans une table de montage, notamment la façon de la cibler et de les contrôler, voir [Animations dans une table de montage](storyboarded-animations.md).

Le scénario de définition d’interface utilisateur d’application le plus courant où vous définirez des animations dans une table de montage séquentiel personnalisées concerne la définition d’états visuels pour des contrôles en XAML. Cette opération sera nécessaire si vous créez une classe de contrôle ou si vous remodélisez un contrôle existant qui possède des états visuels dans son modèle de contrôle. Pour plus d’informations, voir [Animations dans une table de montage séquentiel pour les états visuels](https://docs.microsoft.com/previous-versions/windows/apps/jj819808(v=win.10)).

 

 




