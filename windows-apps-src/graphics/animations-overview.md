---
ms.assetid: 0C8DEE75-FB7B-4E59-81E3-55F8D65CD982
Vue d’ensemble des animations
Utilisez les animations de la bibliothèque d’animations du Windows Runtime pour intégrer l’apparence de Windows dans votre application.
---
# Vue d’ensemble des animations

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Dans Windows Runtime, les animations peuvent améliorer votre application en la rendant plus vivante et interactive. À l’aide des animations de la bibliothèque d’animations du Windows Runtime, vous pouvez intégrer l’apparence de Windows dans votre application. Cette rubrique présente les animations et des exemples de situations classiques où chacune d’elles est utilisée.

**Conseil** Les contrôles Windows Runtime pour XAML incluent certains types d’animations en tant que comportements intégrés qui proviennent d’une bibliothèque d’animations. En utilisant ces contrôles dans votre application, vous pouvez obtenir l’apparence des animations sans aucune programmation préalable.

Les animations de la bibliothèque d’animations Windows Runtime offrent les avantages suivants :

-   Mouvements conformes au [Recommandations en matière d’animations](https://msdn.microsoft.com/library/windows/apps/Dn611854)
-   transitions rapides et fluides entre les états de l’interface utilisateur qui informent l’utilisateur sans le déranger dans ses activités ;
-   comportement visuel qui indique à l’utilisateur les transitions dans une application.

Par exemple, quand l’utilisateur ajoute un élément à une liste, au lieu que ce nouvel élément apparaisse instantanément dans la liste, il se met en place par animation. Les autres éléments de la liste sont animés vers leur nouvelle position en peu de temps, faisant ainsi de la place pour le nouvel élément. Ce comportement de transition rend l’interaction du contrôle plus apparente à l’utilisateur.

La bibliothèque d’animations ne fournit pas des animations pour chaque scénario possible. Il peut arriver que vous souhaitiez créer une animation personnalisée en XAML. Pour plus d’informations, voir [Animations dans une table de montage séquentiel](storyboarded-animations.md).

## Types d’animations

La bibliothèque d’animations et le système d’animations Windows Runtime ont pour but majeur de permettre aux contrôles et autres parties de l’interface utilisateur d’avoir un comportement animé. Il existe plusieurs types distincts d’animations.

-   Les *transitions thématiques* sont appliquées automatiquement quand certaines conditions sont modifiées dans l’interface utilisateur, impliquant des éléments ou contrôles provenant des types d’interface utilisateur XAML Windows Runtime prédéfinis. Elles sont appelées *transitions thématiques*, car elles prennent en charge l’apparence Windows et définissent les actions de toutes les applications pour des scénarios d’interface utilisateur particuliers quand elles passent d’un mode d’interaction à un autre. Les transitions thématiques font partie de la bibliothèque d’animations.
-   Les *animations thématiques* sont des animations appliquées à une ou plusieurs propriétés de types d’interface utilisateur XAML Windows Runtime prédéfinis. Elles diffèrent des transitions thématiques car elles ciblent un élément spécifique et existent dans des états visuels spécifiques dans un contrôle, alors que les transitions thématiques sont affectées à des propriétés du contrôle qui existent en dehors des états visuels et influencent les transitions entre ces états. Une grande partie des contrôles XAML Windows Runtime comprennent des animations thématiques dans des storyboards qui font partie de leur modèle de contrôle, les animations étant déclenchées par les états visuels. Du moment que vous ne changez pas les modèles, vous disposez de ces animations thématiques intégrées pour les contrôles dans votre interface utilisateur. En revanche, si vous remplacez des modèles, vous supprimez également les animations thématiques de contrôle intégrées. Pour les récupérer, vous devez définir une table de montage séquentiel qui comprend des animations thématiques dans le jeu d’états visuels du contrôle. Vous pouvez aussi exécuter des animations thématiques à partir de storyboards qui ne sont pas dans des états visuels et les démarrer avec la méthode [**Begin**](https://msdn.microsoft.com/library/windows/apps/BR210491), mais ce scénario est moins courant. Les animations thématiques font partie de la bibliothèque d’animations.
-   Les *transitions visuelles* sont appliquées quand un contrôle passe de l’un de ses états visuels définis à un autre état. Il s’agit d’animations personnalisées que vous créez et elles sont généralement associées au modèle personnalisé que vous créez pour un contrôle et aux définitions d’état visuel de ce modèle. L’animation est exécutée uniquement pendant le délai entre les états et celui-ci est généralement court, quelques secondes au plus. Pour plus d’informations, voir la [section « VisualTransition » dans Animations dans une table de montage séquentiel pour les états visuels](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808#VisualTransition).
-   Les *tables de montage séquentiel* sont appliquées quand elles sont spécifiquement lancées par le code de votre application ou par le code qui accompagne un composant, par exemple un contrôle que vous avez inclus. Les animations dans une table de montage séquentiel peuvent changer la valeur de n’importe quelle propriété de dépendance Windows Runtime au fil du temps. Elles ne sont pas limitées aux scénarios d’interface utilisateur ; en fait, il peut être utile de les considérer comme une technique de machine à états, si vous connaissez ce concept. Une animation dans une table de montage séquentiel n’est pas limitée au délai de transition entre les états visuels ; elle peut être exécutée à tout moment indépendamment de la modification de l’état d’un contrôle et l’animation peut éventuellement être exécutée en continu. Pour plus d’informations, voir [Animations dans une table de montage séquentiel](storyboarded-animations.md). Pour plus d’informations sur les propriétés de dépendance et sur l’emplacement où elles existent, voir [Vue d’ensemble des propriétés de dépendance](https://msdn.microsoft.com/library/windows/apps/Mt185583).

## Animations disponibles dans la bibliothèque

Les animations suivantes sont fournies dans la bibliothèque d’animations. Cliquez sur le nom d’une animation pour en savoir plus sur ses principaux scénarios d’utilisation, comment la définir, ainsi que pour afficher un exemple de l’animation.

-   [Transition de contenu et d’entrée](./animations-overview.md#content-transition-and-entrance-transition) : anime des éléments de contenu à l’intérieur ou en dehors de la vue.
-   [Apparition/disparition en fondu et fondu enchaîné](./animations-overview.md#fade-in-out-and-crossfade) : affiche les éléments ou contrôles provisoires, ou actualise une zone de contenu.
-   [Pointeur vers le haut/bas](./animations-overview.md#pointer-up-down) : affiche un retour visuel d’un appui ou d’un clic sur une vignette.
-   [Repositionner](./animations-overview.md#reposition) : déplace un élément à un autre endroit.
-   [Afficher/Masquer le menu contextuel](./animations-overview.md#show-hide-popup) : affiche l’interface utilisateur contextuelle sur la vue.
-   [Afficher/Masquer l’interface utilisateur latérale](./animations-overview.md#show-hide-edge-ui) : fait glisser l’interface utilisateur latérale, y compris les grands éléments d’interface utilisateur tels que les panneaux, dans ou en dehors de la vue.
-   [Changements d’éléments de listes](./animations-overview.md#list-item-changes) : ajoute ou supprime un élément d’une liste, ou réorganise les éléments.
-   [Glisser-déplacer](./animations-overview.md#drag-drop) : fournit un retour visuel durant une opération glisser-déplacer.
-   [Mouvements de balayage](./animations-overview.md#swipe-gestures) : indique qu’une vignette prend en charge l’interaction de balayage ou indique l’état de sélection par balayage.

### Transition de contenu et transition d’entrée

Les animations de transition de contenu ([**ContentThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR243103)) permettent de déplacer des éléments de contenu dans ou en dehors de la vue actuelle. Par exemple, les animations de transition de contenu affichent du contenu qui n’était pas prêt à être affiché quand la page a été chargée pour la première fois, ou quand le contenu d’une section d’une page change.

La bibliothèque d’animations XAML n’offre aucun concept d’animation qui s’applique à la page entière quand celle-ci est chargée, mais elle propose une transition distincte ([**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210288)) qui peut s’appliquer au contenu quand la page contenant le contenu est chargée pour la première fois et que cette partie du contenu est affichée. Ainsi, la première apparence du contenu et un changement de contenu peuvent offrir un retour différent.

### Apparition/disparition en fondu et fondu enchaîné

Utilisez des animations d’apparition et de disparition en fondu pour afficher ou masquer une interface utilisateur ou des contrôles provisoires. En XAML, elles sont représentées comme [**FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210298) et [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302). Il peut s’agir par exemple d’une barre de l’application dans laquelle de nouveaux contrôles peuvent s’afficher suite à une interaction de l’utilisateur. Un autre exemple concerne un indicateur de mouvement panoramique ou une barre de défilement provisoire qui disparaît progressivement si aucune entrée utilisateur n’a été détectée pendant un certain temps. Les applications doivent également utiliser l’animation d’apparition en fondu pendant la transition d’un élément d’espace réservé à l’élément final durant le chargement dynamique du contenu.

Utilisez une animation de fondu enchaîné pour rendre moins abrupte la transition quand l’état d’un élément change, par exemple quand l’application actualise le contenu actuel d’une vue. La bibliothèque d’animations XAML ne fournit aucune animation de fondu enchaîné dédiée (aucun équivalent pour [**crossFade**](https://msdn.microsoft.com/library/windows/apps/BR212661)), mais vous pouvez obtenir le même résultat à l’aide de [**FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210298) et [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302) avec synchronisation superposée.

### Pointeur vers le haut/bas

Utilisez les animations [**PointerUpThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/Hh969168) et [**PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/Hh969164) pour donner à l’utilisateur un retour d’un appui ou d’un clic réussi sur une vignette. Par exemple, quand un utilisateur clique ou appuie sur une vignette, l’animation de pointeur vers le bas est lue. Quand l’utilisateur cesse de cliquer ou d’appuyer, l’animation de pointeur vers le haut est lue.

### Repositionner

Utilisez les animations de repositionnement ([**RepositionThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210421) ou [**RepositionThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210429)) pour déplacer un élément à un autre endroit. Par exemple, le déplacement des en-têtes dans un contrôle d’éléments d’en-tête a recours à l’animation de repositionnement.

### Afficher/Masquer le menu contextuel

Utilisez [**PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210383) et [**PopOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210391) quand vous affichez et masquez un objet [**Popup**](https://msdn.microsoft.com/library/windows/apps/BR227842) ou une interface utilisateur contextuelle similaire par-dessus la vue actuelle. [
            **PopupThemeTransition**](https://msdn.microsoft.com/library/windows/apps/Hh969172) est une transition thématique qui fournit un retour utile si vous souhaitez effectuer un abandon interactif d’une fenêtre contextuelle.

### Afficher/Masquer l’interface utilisateur latérale

Utilisez l’animation [**EdgeUIThemeTransition**](https://msdn.microsoft.com/library/windows/apps/Hh702324)pour faire glisser des éléments d’interface utilisateur latéraux de petite taille dans et en dehors de la vue. Par exemple, utilisez ces animations quand vous affichez une barre de l’application personnalisée en haut ou en bas de l’écran ou une surface d’interface utilisateur pour des erreurs et des avertissements en haut de l’écran.

Utilisez l’animation [**PaneThemeTransition**](https://msdn.microsoft.com/library/windows/apps/Hh969160) pour afficher ou masquer un volet ou un panneau. Cette animation convient à l’interface utilisateur latérale de grande taille, telle qu’un clavier personnalisé ou un volet de tâches.

### Changements d’éléments de listes

Utilisez l’animation [**AddDeleteThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR243047) pour ajouter un comportement animé quand vous ajoutez ou supprimez un élément dans une liste existante. Pour l’ajout, la transition commence par repositionner les éléments existants dans la liste pour faire de la place aux nouveaux éléments, puis elle ajoute les nouveaux éléments. Pour la suppression, la transition supprime les éléments d’une liste et, si nécessaire, repositionne les éléments de la liste restants quand les éléments supprimés ont été retirés.

Il existe aussi un objet [**ReorderThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210409) distinct que vous appliquez si un élément change de position dans une liste. Son animation diffère de la suppression d’un élément et de son ajout à un nouvel emplacement avec les animations d’ajout/suppression associées.

### Glisser-déplacer

Utilisez les animations de glissement ([**DragItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243173), [**DragOverThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243177)) et de déplacement ([**DropTargetItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243185)) pour donner un retour visuel quand l’utilisateur fait glisser ou dépose un élément.

Quand elles sont actives, les animations montrent à l’utilisateur que la liste peut être réorganisée autour d’un élément déposé. Il est utile pour les utilisateurs de savoir où l’élément va être placé dans une liste s’il est déposé à l’emplacement actuel. Les animations indiquent visuellement qu’un élément que l’utilisateur fait glisser peut être déposé entre deux autres éléments de la liste et que ces éléments vont disparaître.

### Mouvements de balayage

Utilisez l’animation [**SwipeHintThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/Hh702489) pour montrer qu’une vignette prend en charge l’interaction de balayage. Les utilisateurs peuvent effectuer un mouvement de balayage vers le bas pour sélectionner une vignette. Si un utilisateur ne sait pas s’il peut effectuer un mouvement de balayage sur une vignette, un appui prolongé sur la vignette va lire l’animation d’indicateur de balayage pour suggérer que l’utilisateur doit interagir avec la vignette par un balayage.

Utilisez l’animation [**SwipeBackThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/Hh702477) pour montrer que la vignette a été sélectionnée et pour la renvoyer à son emplacement de pause.

## Utilisation d’animations avec des contrôles personnalisés

Le tableau suivant résume nos recommandations en matière de choix d’animation lors de la création d’une version personnalisée de ces contrôles Windows Runtime :

| Type d’élément d’interface utilisateur | Animation recommandée |
|---------|-----------------------|
| Boîte de dialogue | [
            **FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210298) et [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302) |
| Menu volant | [
            **PopInThemeAnimation**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.animation.popinthemeanimation.popinthemeanimation.aspx) et [**PopOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.popoutthemeanimation.popoutthemeanimation) |
| Info-bulle | [
            **FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210298) et [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302) |
| Menu contextuel | [
            **PopInThemeAnimation**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.animation.popinthemeanimation.popinthemeanimation.aspx) et [**PopOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.popoutthemeanimation.popoutthemeanimation) |
| Barre de commandes | [**EdgeUIThemeTransition**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.edgeuithemetransition.edgeuithemetransition) |
| Volet de tâches ou panneau latéral | [**PaneThemeTransition**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.panethemetransition.panethemetransition) |
| Contenu d’un conteneur d’éléments d’interface utilisateur quelconque | [**ContentThemeTransition**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.contentthemetransition.contentthemetransition) |
| Pour les contrôles ou si aucune autre animation ne s’applique | [
            **FadeInThemeAnimation**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.animation.fadeinthemeanimation.fadeinthemeanimation.aspx) et [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302) |

 

## Exemples d’animation de transition

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

Dans ce code, nous ajoutons l’objet [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210288) à la collection de transitions du bouton. Au moment du premier rendu du bouton, celui-ci vient glisser sur la zone d’affichage plutôt que d’apparaître simplement. Vous pouvez définir quelques propriétés sur l’objet d’animation afin d’ajuster la profondeur et le sens de son glissement, mais le but recherché est une API simple pour un scénario spécifique, plus précisément, pour faire apparaître un contrôle de façon attrayante.

Vous pouvez aussi définir des thèmes d’animation de transition dans les ressources de style de votre application, vous permettant ainsi d’appliquer l’effet uniformément. Cet exemple équivaut au précédent, à la différence qu’il est appliqué par le biais d’un [**Style**](https://msdn.microsoft.com/library/windows/apps/BR208849) :

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

Les exemples précédents appliquent une transition de thème à un seul contrôle. Cependant, les transitions de thème s’avèrent plus intéressantes encore si vous les appliquez à un conteneur d’objets. En procédant ainsi, tous les objets enfants du conteneur participent à la transition. Dans l’exemple suivant, un objet [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210288) est appliqué à une grille (représentée par l’objet [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)) de rectangles.

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

Les rectangles enfants de l’objet [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) apparaissent par le biais d’une transition, l’un après l’autre et de façon harmonieuse, plutôt que tous ensemble comme ce serait le cas si vous appliquiez cette animation aux rectangles un à un.

Voici une vidéo qui illustre cette animation :

![animation montant un rectangle enfant apparaissant par le biais d’une transition](./images/animation-child-rectangles.gif)

<iframe src="https://videoplayercdn.osi.office.net/embed/bb48c68b-c15d-44e4-86e5-8a8065da7a2e?autoplay=true&mkt=en-us&csid=IA-en-us" width="640" height="360" allowFullScreen="true" frameBorder="0" scrolling="no" ></iframe>

Les objets enfants d’un conteneur peuvent également se réagencer si un ou plusieurs de ces enfants changent de position. Dans l’exemple suivant, nous appliquons un objet [**RepositionThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210429) à une grille de rectangles. Lorsque vous retirez l’un des rectangles, tous les autres se repositionnent en conséquence.

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

Cette vidéo illustre l’animation qui s’exécute pour les rectangles supprimés :

Vous pouvez appliquer plusieurs animations de transition à un même objet ou à un conteneur d’objets. Par exemple, si vous souhaitez que les rectangles de la liste s’affichent progressivement, mais aussi qu’ils s’animent quand ils changent de position, vous pouvez appliquer les objets [**RepositionThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210429) et [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210288) comme suit :

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
| [**AddDeleteThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR243047) | Fournit le comportement de la transition animée pour les cas où les contrôles déclenchent l’ajout ou la suppression d’enfants ou de contenu. Généralement, le contrôle correspond à un conteneur d’éléments. |
| [**ContentThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR243103) | Fournit le comportement de la transition animée pour les cas où le contenu d’un contrôle change. Vous pouvez l’appliquer en plus de l’objet [**AddDeleteThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR243047). |
| [**EdgeUIThemeTransition**](https://msdn.microsoft.com/library/windows/apps/Hh702324) | Fournit le comportement de la transition animée pour la transition d’une (petite) interface utilisateur latérale. |
| [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210288) | Fournit le comportement de la transition animée lorsque les contrôles apparaissent pour la première fois. |
| [**PaneThemeTransition**](https://msdn.microsoft.com/library/windows/apps/Hh969160) | Fournit le comportement de la transition animée pour la transition d’une interface utilisateur de panneau (grande interface utilisateur latérale). |
| [**PopupThemeTransition**](https://msdn.microsoft.com/library/windows/apps/Hh969172) | Fournit le comportement de la transition animée qui s’applique aux composants contextuels des contrôles (par exemple, une interface utilisateur semblable à une info-bulle sur un objet) à mesure qu’ils apparaissent. |
| [**ReorderThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210409) | Fournit le comportement de la transition animée pour les cas où les éléments des contrôles list-view changent d’ordre. Généralement, cela se produit suite à une opération de glissement. Les différents contrôles et thèmes peuvent présenter des caractéristiques d’animation diverses. |
| [**RepositionThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210429) | Fournit le comportement de la transition animée lorsque les contrôles changent de position. |

 

## Exemples d’animations de thème

Les animations de transition sont simples à appliquer. Vous pouvez cependant être amené à nécessiter un contrôle plus fin sur la synchronisation et sur l’ordre de vos effets d’animation. Vous pouvez ainsi faire appel à des animations de thème pour permettre cela tout en utilisant un thème Windows cohérent avec le comportement de votre animation. Les animations de thème requièrent également moins de balisage que les animations personnalisées. Nous utilisons ici l’objet [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302) pour faire disparaître progressivement un rectangle de la zone d’affichage.

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

Contrairement aux animations de transition, une animation de thème n’a pas de déclencheur intégré (la transition) qui l’exécute automatiquement. Vous devez utiliser un objet [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) pour contenir une animation thématique quand vous la définissez en XAML. Vous pouvez aussi changer le comportement par défaut de l’animation. Par exemple, il vous est possible de ralentir la disparition progressive en augmentant la valeur de temps [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) de l’objet [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302).

**Remarque** Pour illustrer les techniques d’animation de base, nous utilisons du code d’application pour lancer l’animation en appelant des méthodes de [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490). Vous pouvez contrôler la façon dont les animations d’objet **Storyboard** s’exécutent à l’aide des méthodes [**Begin**](https://msdn.microsoft.com/library/windows/apps/BR210491), [**Stop**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.stop), [**Pause**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.animation.storyboard.pause.aspx), et [**Resume**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.animation.storyboard.resume.aspx) **Storyboard**. Toutefois, ça n’est pas comme cela qu’on inclut normalement des animations de la bibliothèque dans des applications. En général, on intègre plutôt les animations de la bibliothèque dans les modèles et styles XAML appliqués aux contrôles ou aux éléments. L’apprentissage des modèles et des états visuels est un peu plus compliqué. Vous trouverez cependant une description de l’utilisation des animations de la bibliothèque dans les états visuels dans la rubrique [Animations dans une table de montage séquentiel pour les états visuels](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808).

 

Vous pouvez appliquer plusieurs autres animations de thème à vos éléments d’interface utilisateur pour créer des effets d’animation. Les noms de ces API contiennent tous « ThemeAnimation » :

| API | Description |
|-----|-------------|
| [**DragItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243173) | Représente l’animation préconfigurée qui s’applique aux éléments déplacés par glissement. |
| [**DragOverThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243177) | Représente l’animation préconfigurée qui s’applique aux éléments figurant sous un élément déplacé par glissement. |
| [**DropTargetItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243185) | Animation préconfigurée qui s’applique à des éléments pouvant représenter des cibles de dépôt. |
| [**FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210298) | Animation d’opacité préconfigurée qui s’applique aux contrôles au moment où ils apparaissent pour la première fois. |
| [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302) | Animation d’opacité préconfigurée qui s’applique aux contrôles quand ils sont supprimés ou masqués de l’interface utilisateur. |
| [**PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/Hh969164) | Animation préconfigurée pour l’action de l’utilisateur où il appuie ou clique sur un élément. |
| [**PointerUpThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/Hh969168) | Animation préconfigurée pour l’action de l’utilisateur qui s’exécute quand l’utilisateur appuie sur un élément et que l’action est déclenchée. |
| [**PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210383) | Animation préconfigurée qui s’applique à des composants contextuels de contrôles au fur et à mesure qu’ils apparaissent. Cette animation allie opacité et translation. |
| [**PopOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210391) | Animation préconfigurée qui s’applique à des composants contextuels de contrôles au fur et à mesure de leur fermeture ou de leur suppression. Cette animation allie opacité et translation. |
| [**RepositionThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210421) | Animation préconfigurée pour un objet au fur et à mesure de son repositionnement. |
| [**SplitCloseThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210454) | Animation préconfigurée qui masque un élément d’interface utilisateur cible par le biais d’une animation fractionnée. |
| [**SplitOpenThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210472) | Animation préconfigurée qui dévoile un élément d’interface utilisateur cible par le biais d’une animation fractionnée |
| [**SwipeBackThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/Hh702477) | Animation préconfigurée qui s’applique aux contrôles lorsqu’un élément revient en glissant à son emplacement dans la disposition après une interaction de balayage. |
| [**SwipeHintThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/Hh702489) | Animation préconfigurée qui indique qu’un mouvement de balayage est maintenant possible. |

 

## Créer vos propres animations

Au cas où les animations de thème ne répondent pas à vos besoins, vous pouvez créer vos propres animations. Vous animez des objets en animant les valeurs d’une ou de plusieurs de leurs propriétés. Par exemple, vous pouvez animer la largeur d’un rectangle, l’angle d’un [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932) ou la valeur de la couleur d’un bouton. Ce type d’animation personnalisée porte le nom d’animation de table de montage séquentiel, pour le distinguer des animations de la bibliothèque déjà fournies par Windows Runtime comme type d’animation préconfiguré. Pour les animations de table de montage séquentiel, vous utilisez une animation qui peut changer les valeurs d’un type particulier (par exemple [**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136) pour animer un **Double**) et placer cette animation dans un [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) pour la contrôler.

Afin d’être animée, la propriété que vous animez doit être une *propriété de dépendance*. Pour plus d’informations sur les propriétés de dépendance, voir [Vue d’ensemble des propriétés de dépendance](https://msdn.microsoft.com/library/windows/apps/Mt185583). Pour plus d’informations sur la création d’animations personnalisées dans une table de montage, notamment la façon de la cibler et de les contrôler, voir [Animations dans une table de montage](storyboarded-animations.md).

Le scénario de définition d’interface utilisateur d’application le plus courant où vous définirez des animations dans une table de montage séquentiel personnalisées concerne la définition d’états visuels pour des contrôles en XAML. Cette opération sera nécessaire si vous créez une classe de contrôle ou si vous remodélisez un contrôle existant qui possède des états visuels dans son modèle de contrôle. Pour plus d’informations, voir [Animations dans une table de montage séquentiel pour les états visuels](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808). Ces animations ne sont généralement pas du tout des transitions graduelles. Elles se produisent instantanément et constituent davantage une technique permettant de définir un ensemble de changements de propriétés pour un état. Elles n’appliquent pas nécessairement un comportement animé visuellement à une interface utilisateur bien que, comme vous le verrez, les états visuels des contrôles incluent souvent les animations de la bibliothèque proprement dites. Dans ce cas, les animations thématiques appliquent bien un changement au fil du temps, quoique généralement sur une courte durée.

 

 






<!--HONumber=Mar16_HO1-->


