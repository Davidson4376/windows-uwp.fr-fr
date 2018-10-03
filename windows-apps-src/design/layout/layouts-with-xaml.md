---
author: muhsinking
Description: XAML gives you a flexible layout system to create a responsive UI.
title: Dispositions dynamiques avec XAML
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0b45196a83edf45a69f6b79ab82542cef6817703
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/02/2018
ms.locfileid: "4260472"
---
# <a name="responsive-layouts-with-xaml"></a>Dispositions dynamiques avec XAML

Le système de disposition XAML fournit le dimensionnement automatique, des panneaux de disposition, des états visuels et même des définitions d’interface utilisateur séparées pour créer une interface utilisateur réactive. Grâce à une disposition dynamique, vous pouvez faire en sorte que votre application s’affiche parfaitement avec différentes tailles de fenêtres d’application, résolutions, densités de pixels et orientations. Vous pouvez également utiliser XAML pour repositionner, redimensionner, ajuster dynamiquement, afficher/masquer, remplacer ou remodéliser l’interface utilisateur de votre application, comme indiqué dans [Techniques de conception réactive](responsive-design.md). Ici, nous expliquons comment implémenter des dispositions dynamiques avec XAML.

## <a name="fluid-layouts-with-properties-and-panels"></a>Dispositions fluides avec propriétés et panneaux

Une disposition dynamique repose principalement sur l’utilisation appropriée de propriétés XAML et de panneaux de disposition pour repositionner, redimensionner et ajuster dynamiquement le contenu d’une manière fluide. 

Le système de disposition XAML prend en charge à la fois les dispositions statiques et fluides. Dans une disposition statique, vous affectez aux contrôles des positions et des tailles de pixels explicites. Lorsque l’utilisateur change la résolution ou l’orientation de son appareil, l’interface utilisateur n’est pas modifiée. Les dispositions statiques peuvent être tronquées selon les facteurs de formes et de tailles d’écran. D’un autre côté, les dispositions fluides rétrécissent, s’agrandissent et s’ajustent dynamiquement à l’espace visuel disponible sur un appareil. 

Dans la pratique, vous utilisez une combinaison d’éléments statiques et fluides pour créer votre interface utilisateur. Vous utilisez toujours des valeurs et des éléments statiques à certains endroits, mais assurez-vous que l’interface utilisateur globale s’adapte à différentes résolutions, tailles d’écran et vues.

Nous vous expliquons ici comment utiliser les propriétés XAML et les panneaux de disposition pour créer une disposition fluide.

### <a name="layout-properties"></a>Propriétés de disposition
Les propriétés de disposition contrôlent la taille et la position d’un élément. Pour créer une disposition fluide, utilisez le dimensionnement proportionnel ou automatique pour les éléments, et laissez les panneaux de disposition positionner eux-mêmes leurs enfants selon les besoins. 

Voici certaines propriétés de disposition courantes et comment les utiliser pour créer des dispositions fluides.

**Height et Width**

Les propriétés [**Height**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.height.aspx) et [**Width**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.width.aspx) spécifient la taille d’un élément. Vous pouvez utiliser des valeurs fixes mesurées en pixels effectifs, ou vous pouvez utiliser le dimensionnement automatique ou proportionnel. 

Le dimensionnement automatique redimensionne les éléments d’interface utilisateur pour qu’ils s’adaptent à leur contenu ou à leur conteneur parent. Vous pouvez également utiliser le dimensionnement automatique avec les lignes et les colonnes d’une grille. Pour utiliser le dimensionnement automatique, définissez la propriété Height et/ou Width des éléments d’interface utilisateur sur **Auto**.

> [!NOTE]
> Le redimensionnement d’un élément à son contenu ou à son conteneur dépend de la façon dont le conteneur parent gère le dimensionnement de ses enfants. Pour plus d’informations, voir [Panneaux de disposition](#layout-panels) plus loin dans cet article.

Le *dimensionnement proportionnel* répartit l’espace disponible entre les lignes et les colonnes d’une grille par proportions pondérées. En XAML, les valeurs proportionnelles sont exprimées par \* (ou *n*\* pour le dimensionnement proportionnel pondéré). À titre d’exemple, dans une disposition à deux colonnes, pour spécifier qu’une colonne est cinq fois plus large que l’autre colonne, utilisez « 5\* » et « \* » pour les propriétés [**Width**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.columndefinition.width.aspx) des éléments [**ColumnDefinition**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.columndefinition.aspx).

Cet exemple combine le dimensionnement fixe, automatique et proportionnel dans un élément [**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx) avec 4colonnes.

&nbsp;|&nbsp;|&nbsp;
------|------|------
Colonne_1 | **Auto** | La taille de la colonne s’adaptera à son contenu.
Colonne_2 | * | Une fois les colonnes Auto calculées, la colonne conserve une partie de la largeur restante. Colonne_2 sera deux fois moins large que Colonne_4.
Colonne_3 | **44** | La colonne aura une largeur de 44 pixels.
Colonne_4 | **2**\* | Une fois les colonnes Auto calculées, la colonne conserve une partie de la largeur restante. Colonne_4 sera deux fois plus large que Colonne_2.

La largeur par défaut de la colonne est «*», de sorte que vous n’avez pas besoin de définir explicitement cette valeur pour la deuxième colonne.

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition/>
        <ColumnDefinition Width="44"/>
        <ColumnDefinition Width="2*"/>
    </Grid.ColumnDefinitions>
    <TextBlock Text="Column 1 sizes to its conent." FontSize="24"/>
</Grid>
```

Dans le concepteur XAML de Visual Studio, le résultat se présente comme suit.

![Grille de 4colonnes dans le concepteur Visual Studio](images/xaml-layout-grid-in-designer.png)

Pour obtenir la taille d’un élément à l’exécution, utilisez les propriétés en lecture seule [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.actualheight.aspx) et [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.actualwidth.aspx) à la place de Height et Width.

**Contraintes de taille**

Lorsque vous utilisez le dimensionnement automatique dans votre interface utilisateur, vous devez parfois spécifier des limites pour la taille d’un élément. Vous pouvez définir les propriétés [**MinWidth**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.minwidth.aspx)/[**MaxWidth**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.maxwidth.aspx) et [**MinHeight**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.minheight.aspx)/[**MaxHeight**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.maxheight.aspx) pour spécifier des valeurs qui limitent la taille d’un élément tout en permettant un redimensionnement fluide.

Dans un élément Grid, vous pouvez utiliser MinWidth/MaxWidth avec des définitions de colonne, et MinHeight/MaxHeight avec des définitions de ligne.

**Alignement**

Utilisez les propriétés [**HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.horizontalalignment.aspx) et [**VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.verticalalignment.aspx) pour spécifier la manière dont un élément doit être positionné dans son conteneur parent.
- Les valeurs de **HorizontalAlignment** sont **Left**, **Center**, **Right** et **Stretch**.
- Les valeurs de **VerticalAlignment** sont **Top**, **Center**, **Bottom**, et **Stretch**

Avec l’alignement **Stretch**, les éléments remplissent tout l’espace qui leur est alloué dans le conteneur parent. La valeur par défaut des deux propriétés d’alignement est Stretch. Toutefois, certains contrôles, tels que [**Button**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.button.aspx), remplacent cette valeur dans leur style par défaut.
Tout élément qui peut avoir des éléments enfants peut traiter la valeur Stretch pour les propriétés HorizontalAlignment et VerticalAlignment de manière unique. Par exemple, un élément utilisant les valeurs par défaut Stretch placées dans un élément Grid s’étire pour remplir la cellule qui le contient. Le même élément placé dans un élément Canvas adapte sa taille à son contenu. Pour plus d’informations sur la façon dont chaque panneau gère la valeur Stretch, voir l’article [Panneaux de disposition](layout-panels.md).

Pour plus d’informations, voir l’article [Alignement, marge et remplissage](alignment-margin-padding.md), et les pages de référence [**HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.horizontalalignment.aspx) et [**VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.verticalalignment.aspx).

**Visibilité**

Vous pouvez afficher ou masquer un élément en définissant sa propriété [**Visibility**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.visibility.aspx) sur l’une des valeurs [d’énumération **Visibility**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visibility.aspx) : **Visible** ou **Collapsed**. Lorsqu’un élément a la valeur Collapsed, il ne prend aucun espace dans la disposition de l’interface utilisateur.

Vous pouvez modifier la propriété Visibility d’un élément dans le code ou dans un état visuel. Lorsque la valeur Visibility d’un élément est modifiée, tous ses éléments enfants sont également modifiés. Vous pouvez remplacer des sections de votre interface utilisateur en révélant un panneau et en en réduisant un autre.

> [!Tip]
> Lorsque vous disposez d’éléments dans votre interface utilisateur qui sont **Collapsed** par défaut, les objets sont toujours créés au démarrage, même si elles ne sont pas visibles. Vous pouvez différer le chargement de ces éléments jusqu’à ce qu’ils soient affichés en définissant **l’attribut x:DeferLoadStrategy** sur Lazy. Cela peut améliorer les performances de démarrage. Pour plus d’informations, voir [Attribut x:DeferLoadStrategy](../../xaml-platform/x-deferloadstrategy-attribute.md).

### <a name="style-resources"></a>Ressources de style

Vous n’êtes pas obligé de définir chaque valeur de propriété individuellement sur un contrôle. Il est généralement plus efficace de regrouper les valeurs de propriété au sein d’une ressource [**Style**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.style.aspx) et d’appliquer le Style à un contrôle. Cela est particulièrement vrai lorsque vous devez appliquer les mêmes valeurs de propriété à plusieurs contrôles. Pour plus d’informations sur les styles, voir [Application de styles aux contrôles](../controls-and-patterns/xaml-styles.md).

### <a name="layout-panels"></a>Panneaux de disposition

Pour positionner des objets visuels, vous devez les placer dans un objet Panel ou tout autre objet conteneur. L’infrastructure XAML fournit diverses classes Panel, notamment [**Canvas**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx), [**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx), [**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aspx) et [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.aspx), qui servent de conteneurs et vous permettent de positionner et d’organiser les éléments d’interface utilisateur.

L’élément principal à prendre en considération lors du choix d’un panneau de disposition est la façon dont ce dernier positionne et dimensionne ses éléments enfants. Vous devez également considérer de quelle manière les éléments enfants superposés s’empilent les uns sur les autres.

Voici une comparaison des principales fonctionnalités des contrôles de panneau fournis dans l’infrastructure XAML.

Contrôle de panneau | Description
--------------|------------
[**Canvas**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx) | **Canvas** ne prend pas en charge l’interface utilisateur fluide ; vous contrôlez tous les aspects du positionnement et du dimensionnement des éléments enfants. Il est généralement utilisé pour les cas particuliers, tels que la création de graphismes, ou pour définir des petites zones statiques d’une interface utilisateur adaptative plus grande. Vous pouvez utiliser le code ou les états visuels pour repositionner les éléments à l’exécution.<li>Les éléments sont positionnés de manière absolue à l’aide des propriétés jointes Canvas.Top et Canvas.Lef.</li><li>La disposition en couches peut être spécifiée de manière explicite en utilisant la propriété jointe Canvas.ZIndex.</li><li>Les valeurs Stretch sont ignorées pour les propriétés HorizontalAlignment/VerticalAlignment. Si la taille d’un élément n’est pas explicitement définie, il est dimensionné selon son contenu.</li><li>Le contenu enfant n’apparaît pas détouré s’il est plus grand que le panneau. </li><li>Le contenu enfant n’est pas limité par les bordures du panneau.</li>
[**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx) | **Grid** prend en charge le redimensionnement fluide des éléments enfants. Vous pouvez utiliser le code ou les états visuels pour repositionner et ajuster dynamiquement les éléments.<li>Les éléments sont organisés en lignes et en colonnes à l’aide des propriétés jointes Grid.Row et Grid.Column.</li><li>Les éléments peuvent s’étendre sur plusieurs lignes et colonnes via les propriétés jointes Grid.RowSpan et Grid.ColumnSpan.</li><li>Les valeurs Stretch sont respectées pour les propriétés HorizontalAlignment/VerticalAlignment. Si la taille d’un élément n’est pas explicitement définie, ce dernier s’étire pour remplir l’espace disponible dans la cellule de la grille.</li><li>Le contenu enfant apparaît rogné s’il est plus grand que le panneau.</li><li>La taille du contenu est limitée par les bordures du panneau, de sorte que le contenu de défilement affiche des barres de défilement si nécessaire.</li>
[**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aspx) | <li>Les éléments sont disposés en fonction du bord ou du centre du panneau et les uns par rapport aux autres.</li><li>Les éléments sont positionnés à l’aide d’une variété de propriétés jointes qui contrôlent l’alignement du panneau, l’alignement frère et la position sœur. </li><li>Les valeurs Stretch sont ignorées pour les propriétés HorizontalAlignment/VerticalAlignment sauf si les propriétés jointes RelativePanel de l’alignement provoquent un étirement (par exemple, un élément est aligné sur les bords gauche et droit du panneau). Si la taille d’un élément n’est pas explicitement définie, et si cet élément ne s’étire pas, il est dimensionné selon son contenu.</li><li>Le contenu enfant apparaît rogné s’il est plus grand que le panneau.</li><li>La taille du contenu est limitée par les bordures du panneau, de sorte que le contenu de défilement affiche des barres de défilement si nécessaire.</li>
[**StackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.aspx) |<li>Les éléments sont empilés sur une ligne unique, verticalement ou horizontalement.</li><li>Les valeurs Stretch sont respectées pour les propriétés HorizontalAlignment/VerticalAlignment dans la direction opposée de la propriété Orientation. Si la taille d’un élément n’est pas définie explicitement, ce dernier s’étire pour remplir la largeur disponible (ou la hauteur si la valeur de la propriété Orientation est Horizontal). Dans la direction spécifiée par la propriété Orientation, l’élément adapte sa taille à son contenu.</li><li>Le contenu enfant apparaît rogné s’il est plus grand que le panneau.</li><li>La taille du contenu n’est pas limitée par les bordures du panneau dans la direction spécifiée par la propriété Orientation, de sorte que le contenu de défilement s’étire au-delà des bordures du panneau et n’affiche pas de barres de défilement. Vous devez limiter explicitement la hauteur (ou la largeur) du contenu enfant pour que les barres de défilement s’affichent.</li>
[**VariableSizedWrapGrid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.aspx) |<li>Les éléments sont disposés en lignes ou en colonnes qui sont automatiquement renvoyés à la ligne ou dans une nouvelle colonne lorsque la valeur MaximumRowsOrColumns est atteinte.</li><li>La disposition des éléments en ligne ou en colonne est spécifiée par la propriété Orientation.</li><li>Les éléments peuvent s’étendre sur plusieurs lignes et colonnes via les propriétés jointes VariableSizedWrapGrid.RowSpan et VariableSizedWrapGrid.ColumnSpan.</li><li>Les valeurs Stretch sont ignorées pour les propriétés HorizontalAlignment/VerticalAlignment. Les éléments sont dimensionnés selon la spécification des propriétés ItemHeight et ItemWidth. Si ces propriétés ne sont pas définies, l’élément dans la première cellule adapte sa taille au contenu et toutes les autres cellules héritent de cette taille.</li><li>Le contenu enfant apparaît rogné s’il est plus grand que le panneau.</li><li>La taille du contenu est limitée par les bordures du panneau, de sorte que le contenu de défilement affiche des barres de défilement si nécessaire.</li>

Pour des informations détaillées et des exemples de ces panneaux, voir [Panneaux de disposition](layout-panels.md). Voir également [Exemple de techniques réactives](http://go.microsoft.com/fwlink/p/?LinkId=620024).

Les panneaux de disposition vous permettent d’organiser votre interface utilisateur dans des groupes logiques de contrôles. Lorsque vous les utilisez avec les paramètres de propriété appropriés, vous profitez d’une certaine prise en charge du redimensionnement automatique, du repositionnement et de l’ajustement dynamique des éléments de l’interface utilisateur. Toutefois, la plupart des dispositions d’interface utilisateur nécessitent des modifications supplémentaires lorsque la taille de la fenêtre est considérablement modifiée. Pour ce faire, vous pouvez utiliser les états visuels.

## <a name="adaptive-layouts-with-visual-states-and-state-triggers"></a>Dispositions adaptatives avec états visuels et déclencheurs d’état
Utilisez les états visuels pour apporter des changements significatifs à votre interface utilisateur en fonction de la taille de la fenêtre ou d’autres modifications.

Lorsque la fenêtre de votre application grandit ou rétrécit au-delà d’une certaine proportion, vous pouvez, si vous le souhaitez, changer les propriétés de disposition pour repositionner, redimensionner, ajuster dynamiquement, révéler ou remplacer des sections de votre interface utilisateur. Vous pouvez définir des états visuels différents pour votre interface utilisateur et les appliquer lorsque la largeur ou la hauteur de la fenêtre atteint un seuil spécifié. 

Un [**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.adaptivetrigger.aspx) offre un moyen facile de définir le seuil (également appelé «point d’arrêt») au niveau duquel un état est appliqué. Un [**VisualState**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstate.aspx) définit les valeurs de propriété qui sont appliquées à un élément lorsqu’il est dans un état particulier. Vous regroupez les états visuels dans un [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstatemanager.aspx) qui applique le VisualState approprié lorsque les conditions spécifiées sont remplies.

### <a name="set-visual-states-in-code"></a>Définir les états visuels dans le code

Pour appliquer un état visuel à partir du code, vous appelez la méthode [**VisualStateManager.GoToState**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstatemanager.gotostate.aspx). Par exemple, pour appliquer un état lorsque la fenêtre de l’application a une taille donnée, gérez l’événement [**SizeChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.window.sizechanged.aspx) et appelez **GoToState** pour appliquer l’état approprié.

Ici, un [**VisualStateGroup**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstategroup.aspx) contient deuxdéfinitions de VisualState. Le premier, `DefaultState`, est vide. Lorsqu’il est appliqué, les valeurs définies dans la page XAML sont appliquées. Le second, `WideState`, change la propriété [**DisplayMode**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.splitview.displaymode.aspx) de [**SplitView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.splitview.aspx) en **Inline** et ouvre le volet. Cet état est appliqué dans le gestionnaire d’événements SizeChanged si la largeur de la fenêtre est supérieure à 640pixels effectifs.

> [!NOTE]
> Windows ne permet pas à votre application de détecter l’appareil spécifique sur lequel elle s’exécute. Le système peut vous indiquer la famille d’appareils (mobile, ordinateur, etc.) sur laquelle l’application s’exécute, la résolution réelle et la quantité d’espace à l’écran disponible pour l’application (la taille de la fenêtre de l’application). Nous vous recommandons de définir des états visuels pour les [tailles d’écran et points d’arrêt](screen-sizes-and-breakpoints-for-responsive-design.md).


```xaml
<Page ...>
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState x:Name="DefaultState">
                        <Storyboard>
                        </Storyboard>
                    </VisualState>

                <VisualState x:Name="WideState">
                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames
                            Storyboard.TargetProperty="SplitView.DisplayMode"
                            Storyboard.TargetName="mySplitView">
                            <DiscreteObjectKeyFrame KeyTime="0">
                                <DiscreteObjectKeyFrame.Value>
                                    <SplitViewDisplayMode>Inline</SplitViewDisplayMode>
                                </DiscreteObjectKeyFrame.Value>
                            </DiscreteObjectKeyFrame>
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames
                            Storyboard.TargetProperty="SplitView.IsPaneOpen"
                            Storyboard.TargetName="mySplitView">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="True"/>
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <SplitView x:Name="mySplitView" DisplayMode="CompactInline"
                   IsPaneOpen="False" CompactPaneLength="20">
            <!-- SplitView content -->

            <SplitView.Pane>
                <!-- Pane content -->
            </SplitView.Pane>
        </SplitView>
    </Grid>
</Page>
```

```csharp
private void CurrentWindow_SizeChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
{
    if (e.Size.Width > 640)
        VisualStateManager.GoToState(this, "WideState", false);
    else
        VisualStateManager.GoToState(this, "DefaultState", false);
}
```

### <a name="set-visual-states-in-xaml-markup"></a>Définir les états visuels dans le balisage XAML

Avant Windows 10, les définitions de VisualState exigeaient des objets [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.storyboard.aspx) pour les modifications de propriété, et vous deviez appeler **GoToState** dans le code pour appliquer l’état. Cela est illustré dans l’exemple précédent. De nombreux exemples, ainsi que certains codes existants, utilisent encore cette syntaxe.

À compter de Windows 10, vous pouvez utiliser la syntaxe [**Setter**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.setter.aspx) simplifiée ci-dessous, et vous pouvez utiliser un [**StateTrigger**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.statetrigger.aspx) dans le balisage XAML pour appliquer l’état. Les déclencheurs d’état permettent de créer des règles simples qui déclenchent automatiquement des changements d’état visuel en réaction à un événement de l’application.

Cet exemple produit le même résultat que le précédent, mais utilise la syntaxe **Setter** simplifiée à la place d’un Storyboard pour définir les modifications de propriété. En outre, au lieu d’appeler GoToState, il utilise le déclencheur d’état intégré [**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.adaptivetrigger.aspx) pour appliquer l’état. Lorsque vous utilisez des déclencheurs d’état, vous n’avez pas besoin de définir un `DefaultState` vide. Les paramètres par défaut sont réappliqués automatiquement lorsque les conditions du déclencheur d’état ne sont plus satisfaites.

```xaml
<Page ...>
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState>
                    <VisualState.StateTriggers>
                        <!-- VisualState to be triggered when the
                             window width is >=720 effective pixels. -->
                        <AdaptiveTrigger MinWindowWidth="640" />
                    </VisualState.StateTriggers>

                    <VisualState.Setters>
                        <Setter Target="mySplitView.DisplayMode" Value="Inline"/>
                        <Setter Target="mySplitView.IsPaneOpen" Value="True"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <SplitView x:Name="mySplitView" DisplayMode="CompactInline"
                   IsPaneOpen="False" CompactPaneLength="20">
            <!-- SplitView content -->

            <SplitView.Pane>
                <!-- Pane content -->
            </SplitView.Pane>
        </SplitView>
    </Grid>
</Page>
```

> [!Important]
> Dans l’exemple précédent, la propriété jointe VisualStateManager.VisualStateGroups est définie sur l’élément **Grid** . Lorsque vous utilisez des éléments StateTrigger, vérifiez toujours que VisualStateGroups est jointe au premier enfant de la racine pour que les déclencheurs prennent effet automatiquement. (Ici, **Grid** est le premier enfant de l’élément racine **Page**.)

### <a name="attached-property-syntax"></a>Syntaxe de la propriété jointe

Dans un élément VisualState, vous définissez généralement une valeur pour une propriété de contrôle ou pour l’une des propriétés jointes du panneau qui contient le contrôle. Lorsque vous définissez une propriété jointe, placez le nom de propriété jointe entre parenthèses.

Cet exemple montre comment définir la propriété jointe [**RelativePanel.AlignHorizontalCenterWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignhorizontalcenterwithpanel.aspx) sur un TextBox nommé `myTextBox`. Le premier code XAML utilise la syntaxe [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.objectanimationusingkeyframes.aspx) et le deuxième la syntaxe **Setter**.

```xaml
<!-- Set an attached property using ObjectAnimationUsingKeyFrames. -->
<ObjectAnimationUsingKeyFrames
    Storyboard.TargetProperty="(RelativePanel.AlignHorizontalCenterWithPanel)"
    Storyboard.TargetName="myTextBox">
    <DiscreteObjectKeyFrame KeyTime="0" Value="True"/>
</ObjectAnimationUsingKeyFrames>

<!-- Set an attached property using Setter. -->
<Setter Target="myTextBox.(RelativePanel.AlignHorizontalCenterWithPanel)" Value="True"/>
```

### <a name="custom-state-triggers"></a>Déclencheurs d’état personnalisés

Vous pouvez étendre la classe [**StateTrigger**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.statetrigger.aspx) pour créer des déclencheurs personnalisés pour de multiples situations. Par exemple, vous pouvez créer un élément StateTrigger pour déclencher différents états selon le type d’entrée, puis augmenter les marges autour d’un contrôle lorsque le type d’entrée est tactile. Sinon, vous pouvez créer un élément StateTrigger pour appliquer différents états selon la famille d’appareils sur laquelle l’application est exécutée. Pour des exemples montrant comment créer des déclencheurs personnalisés et les utiliser pour créer des expériences d’interface utilisateur optimisées à partir d’une seule vue XAML, voir [Exemple de déclencheurs d’état](http://go.microsoft.com/fwlink/p/?LinkId=620025).

### <a name="visual-states-and-styles"></a>Styles et états visuels

Vous pouvez utiliser des ressources Style dans les états visuels pour appliquer un ensemble de modifications de propriété à plusieurs contrôles. Pour plus d’informations sur les styles, voir [Application de styles aux contrôles](../controls-and-patterns/xaml-styles.md).

Dans ce code XAML simplifié tiré de l’exemple de déclencheurs d’état, une ressource Style est appliquée à un contrôle Button afin d’ajuster la taille et les marges pour une entrée tactile ou à l’aide de la souris. Pour le code complet et la définition du déclencheur d’état personnalisé, voir [Exemple de déclencheurs d’état](http://go.microsoft.com/fwlink/p/?LinkId=620025).

```xaml
<Page ... >
    <Page.Resources>
        <!-- Styles to be used for mouse vs. touch/pen hit targets -->
        <Style x:Key="MouseStyle" TargetType="Rectangle">
            <Setter Property="Margin" Value="5" />
            <Setter Property="Height" Value="20" />
            <Setter Property="Width" Value="20" />
        </Style>
        <Style x:Key="TouchPenStyle" TargetType="Rectangle">
            <Setter Property="Margin" Value="15" />
            <Setter Property="Height" Value="40" />
            <Setter Property="Width" Value="40" />
        </Style>
    </Page.Resources>

    <RelativePanel>
        <!-- ... -->
        <Button Content="Color Palette Button" x:Name="MenuButton">
            <Button.Flyout>
                <Flyout Placement="Bottom">
                    <RelativePanel>
                        <Rectangle Name="BlueRect" Fill="Blue"/>
                        <Rectangle Name="GreenRect" Fill="Green" RelativePanel.RightOf="BlueRect" />
                        <!-- ... -->
                    </RelativePanel>
                </Flyout>
            </Button.Flyout>
        </Button>
        <!-- ... -->
    </RelativePanel>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="InputTypeStates">
            <!-- Second set of VisualStates for building responsive UI optimized for input type.
                 Take a look at InputTypeTrigger.cs class in CustomTriggers folder to see how this is implemented. -->
            <VisualState>
                <VisualState.StateTriggers>
                    <!-- This trigger indicates that this VisualState is to be applied when MenuButton is invoked using a mouse. -->
                    <triggers:InputTypeTrigger TargetElement="{x:Bind MenuButton}" PointerType="Mouse" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="BlueRect.Style" Value="{StaticResource MouseStyle}" />
                    <Setter Target="GreenRect.Style" Value="{StaticResource MouseStyle}" />
                    <!-- ... -->
                </VisualState.Setters>
            </VisualState>
            <VisualState>
                <VisualState.StateTriggers>
                    <!-- Multiple trigger statements can be declared in the following way to imply OR usage.
                         For example, the following statements indicate that this VisualState is to be applied when MenuButton is invoked using Touch OR Pen.-->
                    <triggers:InputTypeTrigger TargetElement="{x:Bind MenuButton}" PointerType="Touch" />
                    <triggers:InputTypeTrigger TargetElement="{x:Bind MenuButton}" PointerType="Pen" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="BlueRect.Style" Value="{StaticResource TouchPenStyle}" />
                    <Setter Target="GreenRect.Style" Value="{StaticResource TouchPenStyle}" />
                    <!-- ... -->
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Page>
```

## <a name="tailored-layouts"></a>Dispositions personnalisées

Lorsque vous apportez des modifications importantes à votre disposition d’interface utilisateur sur différents appareils, il peut s’avérer plus pratique de définir un fichier d’interface utilisateur distinct avec une disposition personnalisée pour l’appareil, que d’adapter une interface utilisateur unique. Si la fonctionnalité est identique sur tous les appareils, vous pouvez définir des vues XAML distinctes qui partagent le même fichier de code. Si la vue et les fonctionnalités diffèrent considérablement sur les appareils, vous pouvez définir des éléments Page distincts et l’élément Page auquel accéder lors du chargement de l’application.

### <a name="separate-xaml-views-per-device-family"></a>Séparer les vues XAML par famille d’appareils

Utilisez les vues XAML pour créer différentes définitions d’interface utilisateur partageant le même code-behind. Vous pouvez fournir une définition d’interface utilisateur unique pour chaque famille d’appareils. Suivez ces étapes pour ajouter une vue XAML à votre application.

**Pour ajouter une vue XAML à une application**
1. Sélectionnez Projet &gt; Ajouter un nouvel élément. La boîte de dialogue Ajouter un nouvel élément s’ouvre.
    > **Conseil**&nbsp;&nbsp;Vérifiez que le projet ou un dossier, et non la solution, est sélectionné dans l’Explorateur de solutions.
2. Sous Visual C# ou Visual Basic dans le volet gauche, choisissez le type de modèle XAML.
3. Dans le volet central, choisissez Vue XAML.
4. Entrez le nom de la vue. La vue doit être nommée correctement. Pour plus d’informations sur l’attribution de noms, consultez le reste de cette section.
5. Cliquez sur Ajouter. Le fichier est ajouté au projet.

Les étapes précédentes créent uniquement un fichier XAML, mais pas un fichier code-behind associé. Au lieu de cela, la vue XAML est associée à un fichier code-behind existant en utilisant un qualificateur DeviceName qui fait partie du nom du fichier ou du dossier. Ce nom de qualificateur peut être mappé sur une valeur de chaîne qui représente la famille d’appareils de l’appareil sur lequel votre application est en cours d’exécution, par exemple, «Bureau», «Tablette» et les noms des autres familles d’appareils (voir [**ResourceContext.QualifierValues**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues.aspx)).

Vous pouvez ajouter le qualificateur au nom de fichier, ou ajouter le fichier à un dossier portant le nom du qualificateur.

**Utiliser le nom de fichier**

Pour utiliser le nom du qualificateur avec le fichier, utilisez ce format : *[pageName]*.DeviceFamily-*[qualifierString]*.xaml.

Examinons un exemple de fichier nommé MainPage.xaml. Pour créer une vue pour les tablettes, nommez la vue XAML MainPage.DeviceFamily-Tablet.xaml. Pour créer une vue pour les PC, nommez la vue MainPage.DeviceFamily-Desktop.xaml. Voici à quoi ressemble la solution dans Microsoft Visual Studio.

![Vues XAML avec des noms de fichier qualifiés](images/xaml-layout-view-ex-1.png)

**Utiliser le nom de dossier**

Pour organiser les vues dans votre projet Visual Studio à l’aide de dossiers, vous pouvez utiliser le nom de qualificateur avec le dossier. Pour ce faire, nommez votre dossier comme suit : DeviceFamily-*[qualifierString]*. Dans ce cas, chaque fichier de la vue XAML a le même nom. N’incluez pas le qualificateur dans le nom de fichier.

Voici un exemple, pour un fichier nommé là encore MainPage.xaml. Pour créer une vue pour les tablettes, créez un dossier nommé DeviceFamily-Tablet et placez dedans une vue XAML nommée MainPage.xaml. Pour créer une vue pour les PC, créez un dossier nommé DeviceFamily-Desktop, et placez dedans une autre vue XAML nommée MainPage.xaml. Voici à quoi ressemble la solution dans Visual Studio.

![Vues XAML dans des dossiers](images/xaml-layout-view-ex-2.png)

Dans les deux cas, une vue unique est utilisée pour les tablettes et les PC. Le fichier par défaut MainPage.xaml est utilisé si l’appareil sur lequel il est exécuté ne correspond à aucune des vues spécifiques de la famille d’appareils.

### <a name="separate-xaml-pages-per-device-family"></a>Séparer les pages XAML par famille d’appareils

Pour fournir des fonctionnalités et des vues uniques, vous pouvez créer des fichiers Page distincts (XAML et code), puis accéder à la page appropriée quand la page est nécessaire.

**Pour ajouter une page XAML à une application**
1. Sélectionnez Projet &gt; Ajouter un nouvel élément. La boîte de dialogue Ajouter un nouvel élément s’ouvre.
    > **Conseil**&nbsp;&nbsp;Vérifiez que le projet, et non la solution, est sélectionné dans l’Explorateur de solutions.
2. Sous Visual C# ou Visual Basic dans le volet gauche, choisissez le type de modèle XAML.
3. Dans le volet central, choisissez Page vierge.
4. Entrez le nom de la page. Par exemple, MainPage_Tablet. Les fichiers de code MainPage_Tablet.xaml et MainPage_Tablet.xaml.cs/vb/cpp sont créés.
5. Cliquez sur Ajouter. Le fichier est ajouté au projet.

À l’exécution, vérifiez la famille d’appareils sur laquelle l’application est exécutée et accédez de cette manière à la page appropriée.

```csharp
if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Tablet")
{
    rootFrame.Navigate(typeof(MainPage_Tablet), e.Arguments);
}
else
{
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
}
```

Vous pouvez également utiliser des critères différents pour déterminer à quelle page accéder. Pour plus d’exemples, consultez l’exemple de [Vues multiples personnalisées](http://go.microsoft.com/fwlink/p/?LinkId=620636), qui utilise la fonction [**GetIntegratedDisplaySize**](https://msdn.microsoft.com/library/windows/apps/xaml/dn904185.aspx) pour vérifier la taille physique d’un affichage intégré.

## <a name="related-topics"></a>Rubriquesassociées
- [Didacticiel: Créer des dispositions adaptatives](../basics/xaml-basics-adaptive-layout.md)
- [Exemple de techniques réactives (GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlResponsiveTechniques)
- [Exemple de déclencheurs d’état (GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlStateTriggers)
- [Exemple de vues multiples personnalisées (GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlTailoredMultipleViews)
