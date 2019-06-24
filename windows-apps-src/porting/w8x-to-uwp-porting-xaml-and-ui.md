---
description: La pratique de définition de l’interface utilisateur sous la forme de balisage XAML déclaratif convertit extrêmement bien des applications 8.1 universelles en applications UWP.
title: Portage du balisage XAML et de la couche interface utilisateur de Windows Runtime 8.x vers UWP
ms.assetid: 78b86762-7359-474f-b1e3-c2d7cf9aa907
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5fcc4312cd238279e01e275d2525c9ac8df98190
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67322366"
---
# <a name="porting-windows-runtime-8x-xaml-and-ui-to-uwp"></a>Portage du balisage XAML et de la couche interface utilisateur de Windows Runtime 8.x vers UWP


Rubrique précédente : [Résolution des problèmes](w8x-to-uwp-troubleshooting.md).

La pratique de définition de l’interface utilisateur sous la forme de balisage XAML déclaratif convertit extrêmement bien des applications 8.1 universelles en applications UWP. Vous constaterez que la majeure partie de votre balisage est compatible, même si vous devez peut-être apporter quelques ajustements aux clés de ressources système ou aux modèles personnalisés que vous utilisez. Le code impératif de vos modèles d’affichage nécessitera peu ou pas de modifications. Une grande partie, voire la majeure partie, du code de votre couche de présentation qui manipule les éléments d’interface utilisateur devrait également être simple à porter.

## <a name="imperative-code"></a>Code impératif

Si vous souhaitez simplement accéder à l’étape de construction de votre projet, vous pouvez commenter ou remplacer tout code non essentiel. Puis effectuer une itération, un problème à la fois et consultez les rubriques suivantes dans cette section (et la rubrique précédente : [Résolution des problèmes](w8x-to-uwp-troubleshooting.md)), jusqu'à ce que les problèmes de génération et d’exécution sont repassée-out et votre port est terminée.

## <a name="adaptiveresponsive-ui"></a>Interface utilisateur adaptative/réactive

Étant donné que votre application peut s’exécuter sur une large gamme d’appareils—présentant chacun différentes tailles d’écran et résolutions—, vous pouvez compléter la procédure minimale de portage de votre application en adaptant votre interface utilisateur afin d’en optimiser l’aspect sur ces appareils. Vous pouvez utiliser la fonctionnalité adaptative Gestionnaire d’état visuel pour détecter dynamiquement la taille de la fenêtre et modifier la disposition en conséquence. Un exemple de procédure à suivre est décrit à la section [Interface utilisateur adaptative](w8x-to-uwp-case-study-bookstore2.md) de la rubrique d’étude de cas Bookstore2.

## <a name="back-button-handling"></a>Gestion du bouton Précédent

8\.1 universelle pour les applications, les applications Windows Runtime 8.x et Windows Phone Store ont des approches différentes pour l’interface utilisateur que vous affichez et les événements que vous gérez pour le bouton précédent. Mais, pour les applications Windows 10, vous pouvez utiliser une approche unique dans votre application. Sur les appareils mobiles, le bouton est fourni à votre intention sous la forme d’un bouton capacitif sur l’appareil ou d’un bouton dans l’interpréteur de commandes. Sur un appareil de bureau, vous ajoutez un bouton au chrome de votre application chaque fois que cette dernière permet la navigation vers l’arrière. Ceci est indiqué dans la barre de titre des applications avec fenêtres ou dans la barre des tâches en mode tablette. L’événement de bouton Précédent est un concept universel sur toutes les familles d’appareils, et les boutons implémentés dans le matériel ou dans le logiciel déclenchent le même événement [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.backrequested).

L’exemple ci-après fonctionne pour toutes les familles d’appareils et est adapté aux cas dans lesquels le même traitement s’applique à toutes les pages et où vous n’avez pas besoin de confirmer la navigation (par exemple, pour signaler les modifications non enregistrées).

```csharp
   // app.xaml.cs

    protected override void OnLaunched(LaunchActivatedEventArgs e)
    {
        [...]

        Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
        rootFrame.Navigated += RootFrame_Navigated;
    }

    private void RootFrame_Navigated(object sender, NavigationEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        // Note: On device families that have no title bar, setting AppViewBackButtonVisibility can safely execute 
        // but it will have no effect. Such device families provide back button UI for you.
        if (rootFrame.CanGoBack)
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Visible;
        }
        else
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Collapsed;
        }
    }

    private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        if (rootFrame.CanGoBack)
        {
            rootFrame.GoBack();
        }
    }
```

Il existe également une approche unique pour toutes les familles d’appareils concernant la fermeture de l’application par programme.

```csharp
   Windows.UI.Xaml.Application.Current.Exit();
```

## <a name="charms"></a>Icônes

Vous n’avez pas besoin de modifier votre code qui s’intègre avec les icônes, mais vous n’avez pas besoin de l’ajout d’une interface utilisateur à votre application à la place de la barre d’icônes, ce qui ne fait pas partie de l’interpréteur de commandes Windows 10. Une application de 8.1 universelle s’exécutant sur Windows 10 a sa propre interface utilisateur fournie par le rendu de système de chrome dans la barre de titre de l’application de remplacement.

## <a name="controls-and-control-styles-and-templates"></a>Contrôles et styles et modèles de contrôle

Une application de 8.1 universelle s’exécutant sur Windows 10 conserveront 8.1 apparence et son comportement en ce qui concerne les contrôles. Toutefois, lorsque vous migrez cette application à une application Windows 10, il existe des différences dans l’apparence et le comportement à connaître. L’architecture et la conception de contrôles étant essentiellement inchangé pour les applications Windows 10, les modifications concernent principalement les améliorations de langage, la simplification et la convivialité de conception.

**Remarque**    état visuel PointerOver le concerne dans les styles/des modèles personnalisés dans les applications Windows 10 et dans les applications Windows Runtime 8.x, mais pas dans les applications Windows Phone Store. Pour cette raison (et, en raison des clés de ressources système qui sont prises en charge pour les applications Windows 10), nous recommandons que vous réutiliser les styles/modèles personnalisés à partir de vos applications du Windows Runtime 8.x lorsque vous portez votre application vers Windows 10.
Si vous voulez être certain que vos styles/modèles personnalisés sont à l’aide de la dernière série d’états visuels et tirent parti des améliorations de performances apportées aux styles par défaut/modèles, puis modifier une copie du nouveau modèle de valeur par défaut de Windows 10 et ré-appliquer votre personnalisation à qui. Un exemple d’amélioration des performances correspond à la suppression de tous les éléments **Border** qui délimitaient précédemment un élément **ContentPresenter** ou Panel et au rendu de la bordure par un élément enfant.

Voici quelques exemples plus spécifiques de modifications apportées aux contrôles.

| Nom du contrôle | Modification |
|--------------|--------|
| **AppBar**   | Si vous utilisez le **AppBar** contrôle ([**CommandBar** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) est recommandé à la place), il n’est pas masquée par défaut dans une application Windows 10. Vous pouvez contrôler ce comportement avec la propriété [**AppBar.ClosedDisplayMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar.closeddisplaymode). |
| **AppBar**, [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) | Dans une application Windows 10, **AppBar** et [ **CommandBar** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) ont un **plus** bouton (les points de suspension). |
| [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) | Dans une application de 8.x Windows Runtime, les commandes secondaire d’un [ **CommandBar** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) sont toujours visibles. Dans une application Windows Phone Store et dans une application Windows 10, le n’apparaissent pas jusqu'à ce que la barre de commandes s’ouvre. |
| [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) | Pour une application du Windows Phone Store, la valeur de [**CommandBar.IsSticky**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar.issticky) n’a pas d’influence sur la possibilité d’abandon interactif de la barre. Pour une application Windows 10, si **IsSticky** est défini sur true, puis le **CommandBar** ignore un mouvement dismiss clair. |
| [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) | Dans une application Windows 10, [ **CommandBar** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) ne gère pas la [ **EdgeGesture.Completed** ](https://docs.microsoft.com/uwp/api/windows.ui.input.edgegesture.completed) ni [  **UIElement.RightTapped** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.righttapped) événements. Il ne réagit pas non plus à un appui ni à un balayage vers le haut. Vous avez toujours la possibilité de gérer ces événements et de définir [**IsOpen**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbar.isopen). |
| [**DatePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker), [**TimePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker) | Passez en revue l’apparence de votre application avec les changements visuels apportés à [**DatePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker) et [**TimePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker). Pour une application Windows 10 en cours d’exécution sur un appareil mobile, ces contrôles ne sont plus accéder à une page de sélection mais plutôt une fenêtre contextuelle révocables de lumière. |
| [**DatePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker), [**TimePicker**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker) | Dans une application Windows 10, vous ne pouvez pas mettre [ **DatePicker** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker) ou [ **TimePicker** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker) à l’intérieur d’un volant. Si vous souhaitez que ces contrôles s’affichent dans un contrôle de type fenêtre contextuelle, vous pouvez utiliser [**DatePickerFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePickerFlyout) et [**TimePickerFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePickerFlyout). |
| **GridView**, **ListView** | Pour **GridView**/**ListView**, voir la section [Modifications GridView et ListView](#gridview-and-listview-changes). |
| [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) | Dans une application du Windows Phone Store, un contrôle [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) exécute une boucle entre la dernière section et la première. Dans une application de 8.x Windows Runtime et dans une application Windows 10, les sections du hub n’imbriquez pas autour. |
| [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) | Dans une application du Windows Phone Store, l’image d’arrière-plan d’un contrôle [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) se déplace en parallaxe par rapport aux sections de hub. Dans une application de 8.x Windows Runtime et dans une application Windows 10, parallaxe n’est pas utilisé. |
| [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub)  | Dans une application 8.1 universelle, la propriété [**HubSection.IsHeaderInteractive**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.hubsection.isheaderinteractive) rend interactifs l’en-tête de section et le glyphe de chevron rendu en regard de ce dernier. Dans une application Windows 10, il existe un intuitif interactif « En savoir plus » en regard de l’en-tête, mais l’en-tête lui-même n’est pas interactif. **IsHeaderInteractive** détermine toujours si l’interaction déclenche l’événement [**Hub.SectionHeaderClick**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.hub.sectionheaderclick). |
| **MessageDialog** | Si vous utilisez **MessageDialog**, préférez [**ContentDialog**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog), plus flexible. Voir également l’[exemple d’éléments de base d’une interface utilisateur XAML](https://go.microsoft.com/fwlink/p/?linkid=619992) (en anglais). |
| **ListPickerFlyout**, **PickerFlyout**  | **ListPickerFlyout** et **PickerFlyout** sont déconseillées pour une application Windows 10. Dans le cas d’un menu volant à sélection unique, utilisez [**MenuFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout) ; pour des expériences plus complexes, préférez [**Flyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout). |
| [**PasswordBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox) | Le [ **PasswordBox.IsPasswordRevealButtonEnabled** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.ispasswordrevealbuttonenabled) propriété est déconseillée dans une application Windows 10 et définir n’a aucun effet. Utilisez [ **PasswordBox.PasswordRevealMode** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.passwordrevealmode) au lieu de cela, qui utilise par défaut **aperçu** (dans laquelle un glyphe yeux s’affiche, comme dans une application de 8.x Windows Runtime). Voir également l’article [Recommandations en matière de zones de mot de passe](https://docs.microsoft.com/windows/uwp/controls-and-patterns/password-box). |
| [**Pivot**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) | Le contrôle [**Pivot**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) est désormais universel et n’est plus limité aux appareils mobiles. |
| [**SearchBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SearchBox) | Bien que la méthode [**SearchBox**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.searchbox) soit implémentée dans la famille d’appareils universelle, elle n’est pas entièrement fonctionnelle sur des appareils mobiles. Voir [Remplacement de SearchBox par AutoSuggestBox](#searchbox-deprecated-in-favor-of-autosuggestbox). |
| **SemanticZoom** | Pour **SemanticZoom**, voir [Modifications SemanticZoom](#semanticzoom-changes). |
| [**ScrollViewer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)  | Certaines propriétés par défaut de [**ScrollViewer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) ont changé. [**HorizontalScrollMode** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode) est **automatique**, [ **VerticalScrollMode** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollmode) est **automatique**et [ **ZoomMode** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.zoommode) est **désactivé**. Si les nouvelles valeurs par défaut ne sont pas adaptées à votre application, vous pouvez les modifier dans un style ou sous forme de valeurs locales sur le contrôle proprement dit.  |
| [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) | Dans une application de 8.x Windows Runtime, vérification orthographique est désactivé par défaut pour un [ **zone de texte**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox). Dans une application Windows Phone Store et dans une application Windows 10, il est activé par défaut. |
| [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) | La taille de police par défaut d’un élément [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) est passée de 11 à 15. |
| [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) | La valeur par défaut de [**TextBox.TextReadingOrder**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.textreadingorder) est passée de **Default** à **DetectFromContent**. Si cette valeur ne convient pas, utilisez **UseFlowDirection**. La valeur **Default** est déconseillée. |
| Divers | Couleur d’accentuation s’applique à une application Windows Phone Store et pour les applications Windows 10, mais pas pour les applications Windows Runtime 8.x.  |

Pour plus d’informations sur les contrôles des applications UWP, voir [Contrôles par fonction](https://docs.microsoft.com/windows/uwp/controls-and-patterns/controls-by-function), [Liste des contrôles](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/) et [Recommandations relatives aux contrôles](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/index).

##  <a name="design-language-in-windows10"></a>Langage de conception dans Windows 10

Il existe certaines différences légère mais importantes dans le langage de conception entre les applications 8.1 universelles et applications Windows 10. Pour plus de détails, voir [Conception](https://developer.microsoft.com/en-us/windows/apps/design). Malgré les changements en matière de langage, nos principes de conception restent cohérents : être attentif aux détails, mais toujours viser la simplicité en se concentrant sur le contenu sans superflu, en réduisant à tout prix les éléments visuels et en restant authentique en matière de domaine numérique ; utiliser la hiérarchie visuelle, en particulier avec la typographie ; concevoir à l’aide d’une grille et donner vie à vos expériences grâce à des animations fluides.

## <a name="effective-pixels-viewing-distance-and-scale-factors"></a>Pixels effectifs, distance d’affichage et facteurs d’échelle

Auparavant, les pixels d’affichage permettaient d’extraire la taille et la disposition des éléments d’interface utilisateur de la taille physique et de la résolution réelles des appareils. Ces pixels ont évolué de manière à devenir des « pixels effectifs ». Voici ce que cette expression désigne, sa signification et la valeur ajoutée proposée.

Le terme « résolution » fait référence à la mesure de la densité des pixels et non, comme on le pense souvent, au nombre de pixels. La « résolution effective » est la façon dont les pixels physiques qui composent une image ou un glyphe apparaissent à l’œil, étant donné les différences liées à la distance de visualisation et à la taille des pixels physiques sur l’appareil (la densité de pixels étant l’inverse de la taille des pixels physiques). La résolution effective est une bonne unité de mesure pour créer une expérience, car elle est centrée sur l’utilisateur. La compréhension de tous ces facteurs et le contrôle de la taille des éléments d’interface utilisateur vous permettent d’optimiser l’expérience utilisateur.

Les différents appareils utilisés présentent une largeur variable (en pixels effectifs). Celle-ci est de 320 epx sur les plus petits d’entre eux, de 1 024 epx sur les écrans de taille modeste et nettement plus élevée sur d’autres. Il vous suffit de continuer à utiliser les éléments à dimensionnement automatique et les panneaux à disposition dynamique que vous utilisez depuis toujours. Dans certains cas, il se peut que vous définissiez une taille fixe pour les propriétés de vos éléments d’interface utilisateur dans le balisage XAML. Un facteur d’échelle est automatiquement affecté à votre application, en fonction de l’appareil sur lequel elle s’exécute et des paramètres d’affichage définis par l’utilisateur. Ce facteur permet aux éléments à taille fixe de l’interface utilisateur de conserver la même taille (approximativement) sur les écrans de différentes tailles de l’utilisateur, pour les opérations tactiles ou pour la lecture. Et avec la disposition dynamique, votre interface utilisateur ne sera pas seulement mise à l’échelle sur différents appareils. Elle s’efforcera également d’adapter la quantité de contenu appropriée à l’espace disponible.

Pour que votre application offre une expérience optimale sur tous les écrans, nous vous recommandons de créer chaque ressource bitmap dans différentes tailles, chacune étant adaptée à un facteur d’échelle spécifique. Fournir des ressources aux échelles 100 %, 200 % et 400 % (dans cet ordre de priorité) produit d’excellents résultats dans la plupart des cas à tous les facteurs d’échelle intermédiaires.

**Remarque**  If, pour une raison quelconque, vous ne peut pas créer des ressources dans plusieurs tailles, puis créez des éléments multimédias à l’échelle 100 %. Dans Microsoft Visual Studio, le modèle de projet par défaut pour les applications UWP fournit des ressources de personnalisation (vignettes et logos) dans une seule taille, mais elles ne sont pas à l’échelle 100 %. Lorsque vous créez des ressources pour votre propre application, suivez les recommandations de cette section, fournissez des tailles 100 %, 200 % et 400 %, et utilisez des packs de ressources.

Si vous disposez d’illustrations complexes, vous serez peut-être amené à fournir vos ressources dans un plus grand nombre de tailles. Si vous débutez avec une image vectorielle, il est relativement aisé de générer des ressources de haute qualité à n’importe quel facteur d’échelle.

Nous ne recommandons pas que vous essayez de prendre en charge tous les facteurs d’échelle, mais la liste complète des facteurs d’échelle pour les applications Windows 10 est 100 %, 125 %, 150 %, 200 %, 250 %, 300 % et 400 %. Si vous fournissez ces facteurs, le Windows Store sélectionne les ressources de taille appropriée pour chaque appareil, et seules ces ressources sont téléchargées. Le Windows Store sélectionne les ressources à télécharger en fonction de la résolution de l’appareil. Vous pouvez réutiliser actifs à partir de votre application de 8.x Windows Runtime à des facteurs d’échelle tels que 140 % et % de 220, mais votre application s’exécutera à un des nouveaux facteurs de mise à l’échelle, et par conséquent, une image bitmap mise à l’échelle sera inévitable. Testez votre application sur divers appareils pour voir si vous êtes satisfait des résultats dans votre cas.

Vous pourrez être nouveau à l’aide de balisage XAML à partir d’une application de 8.x Windows Runtime où les valeurs de dimension littéral sont utilisées dans le balisage (par exemple, pour les formes de taille ou d’autres éléments, par exemple pour typographie). Mais, dans certains cas, un plus grand facteur d’échelle est utilisé sur un appareil pour une application Windows 10 que pour une application universelle 8.1 (par exemple, 150 % sert où 140 % auparavant, et 200 % est utilisé dans lequel a été 180 %). Par conséquent, si vous trouvez que ces valeurs littérales sont désormais trop volumineux sur Windows 10, puis recommencez les multiplie par 0,8. Pour en savoir plus, voir [Conception réactive 101 pour les applications UWP](https://docs.microsoft.com/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design).

## <a name="gridview-and-listview-changes"></a>Modifications GridView et ListView

Plusieurs modifications ont été apportées aux méthodes Style Setters par défaut associées à l’élément [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView), afin de permettre le défilement vertical du contrôle (auparavant, le défilement était effectué horizontalement par défaut). Si vous avez modifié une copie du style par défaut dans votre projet, votre copie ne comportera pas ces modifications. Vous devrez donc les effectuer manuellement. Voici une liste des modifications apportées.

-   La méthode setter associée à l’élément [**ScrollViewer.HorizontalScrollBarVisibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility) est passée de la valeur **Auto** à la valeur **Disabled**.
-   La méthode setter associée à l’élément [**ScrollViewer.VerticalScrollBarVisibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility) est passée de la valeur **Disabled** à la valeur **Auto**.
-   La méthode setter associée à l’élément [**ScrollViewer.HorizontalScrollMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode) est passée de la valeur **Enabled** à la valeur **Disabled**.
-   La méthode setter associée à l’élément [**ScrollViewer.VerticalScrollMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollmode) est passée de la valeur **Disabled** à la valeur **Enabled**.
-   Dans la méthode setter associée à l’élément [**ItemsPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel), la valeur de [**ItemsWrapGrid.Orientation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemswrapgrid.orientation) est passée de **Vertical** à **Horizontal**.

Si cette dernière modification (vers **Orientation**) vous semble contradictoire, rappelez-vous que nous parlons d’une grille de renvoi à la ligne. Une grille de renvoi à la ligne avec orientation horizontale (nouvelle valeur) est comparable à un système d’écriture dans lequel le texte s’affiche à l’horizontale et passe à la ligne suivante à la fin d’une page. Une page de texte de ce type fait l’objet d’un défilement vertical. À l’inverse, une grille de renvoi à la ligne avec orientation verticale (valeur précédente) est similaire à un système d’écriture dans lequel le texte s’affiche à la verticale et, de ce fait, défile à l’horizontale.

Voici les aspects de [ **GridView** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) et [ **ListView** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) dont la modification ou qui ne sont pas pris en charge dans Windows 10.

-   Le [ **IsSwipeEnabled** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.isswipeenabled) propriété (applications Windows Runtime 8.x uniquement) n’est pas pris en charge pour les applications Windows 10. L’API est toujours présente, mais le paramètre n’a aucun effet. Tous les précédents mouvements de sélection sont pris en charge, à l’exception du balayage vers le bas (non pris en charge, car les données montrent qu’il n’est pas détectable) et du clic avec le bouton droit (qui est réservé à l’affichage d’un menu contextuel).
-   Le [ **ReorderMode** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.reordermode) propriété (applications Windows Phone Store uniquement) n’est pas pris en charge pour les applications Windows 10. L’API est toujours présente, mais le paramètre n’a aucun effet. En définissant [**AllowDrop**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.allowdrop) et [**CanReorderItems**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.canreorderitems) sur true dans votre élément **GridView** ou **ListView**, vous permettrez à l’utilisateur d’effectuer une réorganisation à l’aide d’un mouvement d’appui prolongé (ou de clic et de glissement).
-   Lorsque vous développez pour Windows 10, utilisez [ **ListViewItemPresenter** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ListViewItemPresenter) au lieu de [ **GridViewItemPresenter** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.GridViewItemPresenter) dans votre conteneur d’éléments appliquer un style, les deux, pour [ **ListView** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) et pour [ **GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView). Si vous modifiez une copie des styles de conteneur d’éléments par défaut, vous obtiendrez le type correct.
-   Les visuels de sélection ont changé pour une application Windows 10. Si vous définissez [**SelectionMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode) sur **Multiple**, une case à cocher est affichée par défaut pour chaque élément. Le paramétrage par défaut pour les éléments **ListView** signifie que la case à cocher est incluse en regard de l’élément, et par conséquent, l’espace occupé par le reste de l’élément sera légèrement réduit et déplacé. Pour les éléments **GridView**, la case à cocher est placée sur l’élément par défaut. Dans les deux cas, vous pouvez contrôler la disposition (incluse ou superposée) des cases à cocher (avec la propriété [**CheckMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkmode)) et leur affichage (avec la propriété [**SelectionCheckMarkVisualEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectioncheckmarkvisualenabled)) sur l’élément [**ListViewItemPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter) à l’intérieur de votre style de conteneur d’éléments, comme dans l’exemple ci-dessous.
-   Dans Windows 10, le [ **ContainerContentChanging** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.containercontentchanging) événement est déclenché deux fois par élément lors de la virtualisation de l’interface utilisateur : une fois pour la demande de récupération et de réutilisation. Si [**InRecycleQueue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.containercontentchangingeventargs.inrecyclequeue) est défini sur **true** et que vous n’avez aucun travail de récupération particulier à effectuer, vous pouvez quitter votre gestionnaire d’événements immédiatement avec la certitude qu’il se rouvrira quand ce même élément sera réutilisé (moment auquel **InRecycleQueue** prendra la valeur **false**).

```xml
<Style x:Key="CustomItemContainerStyle" TargetType="ListViewItem|GridViewItem">
    ...
    <Setter.Value>
        <ControlTemplate TargetType="ListViewItem|GridViewItem">
            <ListViewItemPresenter CheckMode="Inline|Overlay" ... />
        </ControlTemplate>
    </Setter.Value>
    ...
</Style>
```

![ListViewItemPresenter avec case à cocher incluse](images/w8x-to-uwp-case-studies/ui-listviewbase-cb-inline.jpg)

ListViewItemPresenter avec case à cocher incluse

![ListViewItemPresenter avec case à cocher superposée](images/w8x-to-uwp-case-studies/ui-listviewbase-cb-overlay.jpg)

ListViewItemPresenter avec case à cocher superposée

-   En raison de la suppression des mouvements de balayage vers le bas et de clic avec le bouton droit pour la sélection (pour les raisons exposées ci-dessus), le modèle d’interaction a changé. En conséquence, les événements [**ItemClick**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.itemclick) et [**SelectionChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) ne s’excluent plus mutuellement. Pour votre application Windows 10, passez en revue vos scénarios et décider d’adopter la « sélection » ou le modèle d’interaction « appeler ». Pour plus de détails, voir [Comment changer le mode d’interaction](https://docs.microsoft.com/previous-versions/windows/apps/hh780625(v=win.10)).
-   Les propriétés que vous utilisez pour styliser [**ListViewItemPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter) ont été modifiées. Nouvelles propriétés : [**CheckBoxBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkboxbrush), [**PressedBackground**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.pressedbackground), [**SelectedPressedBackground**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedpressedbackground) et [**FocusSecondaryBorderBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.focussecondaryborderbrush). Les propriétés qui sont ignorées pour une application Windows 10 sont [ **remplissage** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.padding) (utilisez [ **ContentMargin** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.contentmargin) à la place), [ **CheckHintBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkhintbrush), [ **CheckSelectingBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkselectingbrush), [ **PointerOverBackgroundMargin** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.pointeroverbackgroundmargin), [ **ReorderHintOffset**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.reorderhintoffset), [ **SelectedBorderThickness**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedborderthickness), et [  **SelectedPointerOverBorderBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedpointeroverborderbrush).

Le tableau suivant décrit les modifications apportées aux états visuels et aux groupes d’états visuels dans les modèles de contrôle [**ListViewItem**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewItem) et [**GridViewItem**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridViewItem).

| 8.1                 |                         | Windows 10        |                     |
|---------------------|-------------------------|-------------------|---------------------|
| CommonStates        |                         | CommonStates      |                     |
|                     | Normal                  |                   | Normal              |
|                     | PointerOver             |                   | PointerOver         |
|                     | Pressed                 |                   | Pressed             |
|                     | PointerOverPressed      |                   | [non disponible]       |
|                     | Désactivé                |                   | [non disponible]       |
|                     | [non disponible]           |                   | PointerOverSelected |
|                     | [non disponible]           |                   | Sélectionné            |
|                     | [non disponible]           |                   | PressedSelected     |
| [non disponible]       |                         | DisabledStates    |                     |
|                     | [non disponible]           |                   | Désactivé            |
|                     | [non disponible]           |                   | Enabled             |
| SelectionHintStates |                         | [non disponible]     |                     |
|                     | VerticalSelectionHint   |                   | [non disponible]       |
|                     | HorizontalSelectionHint |                   | [non disponible]       |
|                     | NoSelectionHint         |                   | [non disponible]       |
| [non disponible]       |                         | MultiSelectStates |                     |
|                     | [non disponible]           |                   | MultiSelectDisabled |
|                     | [non disponible]           |                   | MultiSelectEnabled  |
| SelectionStates     |                         | [non disponible]     |                     |
|                     | Désélection             |                   | [non disponible]       |
|                     | Désélectionné              |                   | [non disponible]       |
|                     | UnselectedPointerOver   |                   | [non disponible]       |
|                     | UnselectedSwiping       |                   | [non disponible]       |
|                     | Sélection               |                   | [non disponible]       |
|                     | Sélectionné                |                   | [non disponible]       |
|                     | SelectedSwiping         |                   | [non disponible]       |
|                     | SelectedUnfocused       |                   | [non disponible]       |

Si vous avez personnalisé un modèle de contrôle [**ListViewItem**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewItem) ou [**GridViewItem**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridViewItem), passez-le en revue en tenant compte des modifications ci-dessus. Nous vous recommandons de commencer par modifier une copie du nouveau modèle par défaut, puis de lui réappliquer votre personnalisation. Si pour une raison quelconque, vous ne pouvez pas effectuer cette opération, mais que vous devez modifier votre modèle existant, voici quelques recommandations générales sur la procédure à suivre.

-   Ajoutez le nouveau groupe d’états visuels MultiSelectStates.
-   Ajoutez le nouvel état visuel MultiSelectDisabled.
-   Ajoutez le nouvel état visuel MultiSelectEnabled.
-   Ajoutez le nouveau groupe d’états visuels DisabledStates.
-   Ajoutez le nouvel état visuel Enabled.
-   Dans le groupe d’états visuels CommonStates, supprimez l’état visuel PointerOverPressed.
-   Déplacez l’état visuel Disabled vers le groupe d’états visuels DisabledStates.
-   Ajoutez le nouvel état visuel PointerOverSelected.
-   Ajoutez le nouvel état visuel PressedSelected.
-   Supprimez le groupe d’états visuels SelectedHintStates.
-   Dans le groupe d’états visuels SelectionStates, déplacez l’état visuel Selected vers le groupe d’états visuels CommonStates.
-   Supprimez l’intégralité du groupe d’états visuels SelectionStates.

## <a name="localization-and-globalization"></a>Localisation et globalisation

Vous pouvez réutiliser les fichiers Resources.resw de votre projet 8.1 universel dans votre projet d’application UWP. Après avoir copié le fichier, ajoutez-le au projet et définissez **Action de génération** sur **PRIResource** et **Copier dans le répertoire de sortie** sur **Ne pas copier**. La rubrique [**ResourceContext.QualifierValues**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues) décrit comment charger des ressources propres à la famille d’appareils en fonction du facteur de sélection de ressources de cette dernière.

## <a name="play-to"></a>Lire sur

Les API dans le [ **Windows.Media.PlayTo** ](https://docs.microsoft.com/uwp/api/Windows.Media.PlayTo) espace de noms sont déconseillées pour les applications Windows 10 en faveur de la [ **Windows.Media.Casting** ](https://docs.microsoft.com/uwp/api/Windows.Media.Casting) API.

## <a name="resource-keys-and-textblock-style-sizes"></a>Clés de ressources et tailles de style TextBlock

Le langage de conception a évolué pour Windows 10 et, par conséquent, certains styles système ont changé. Dans certains cas, il vous sera utile de revoir les conceptions visuelles de vos affichages afin de les harmoniser avec les propriétés de style modifiées.

Dans d’autres cas, les clés de ressources ne sont plus prises en charge. L’éditeur de balisage XAML de Visual Studio met en surbrillance les références aux clés de ressources qui ne peuvent pas être résolues. Par exemple, il souligne une référence à la clé de style `ListViewItemTextBlockStyle` d’une ligne ondulée rouge. Si ce n’est pas corrigé, l’application s’arrête immédiatement lorsque vous essayez de la déployer vers l’émulateur ou l’appareil. Il est donc important de veiller à l’exactitude du balisage XAML. Et vous allez découvrir que Visual Studio est un formidable outil pour intercepter ces problèmes.

Pour les clés qui sont toujours prises en charge, les modifications apportées au langage de conception signifient que les propriétés définies par certains styles ont changé. Par exemple, `TitleTextBlockStyle` définit **FontSize** à 14.667px dans une application de 8.x Windows Runtime et 18.14px dans une application Windows Phone Store. Mais les mêmes jeux de style **FontSize** à une quantité 24 supérieure px dans une application Windows 10. Passez en revue vos conceptions et dispositions et utilisez les styles appropriés aux endroits adéquats. Pour plus d’informations, voir [Recommandations en matière de polices](https://docs.microsoft.com/windows/uwp/controls-and-patterns/fonts) et [Concevoir des applications UWP](https://developer.microsoft.com/en-us/windows/apps/design).

Voici la liste complète des clés qui ne sont plus prises en charge.

-   CheckBoxAndRadioButtonMinWidthSize
-   CheckBoxAndRadioButtonTextPaddingThickness
-   ComboBoxFlyoutListPlaceholderTextOpacity
-   ComboBoxFlyoutListPlaceholderTextThemeMargin
-   ComboBoxHighlightedBackgroundThemeBrush
-   ComboBoxHighlightedBorderThemeBrush
-   ComboBoxHighlightedForegroundThemeBrush
-   ComboBoxInlinePlaceholderTextForegroundThemeBrush
-   ComboBoxInlinePlaceholderTextThemeFontWeight
-   ComboBoxItemDisabledThemeOpacity
-   ComboBoxItemHighContrastBackgroundThemeMargin
-   ComboBoxItemMinHeightThemeSize
-   ComboBoxPlaceholderTextBlockStyle
-   ComboBoxPlaceholderTextThemeMargin
-   CommandBarBackgroundThemeBrush
-   CommandBarForegroundThemeBrush
-   ContentDialogButton1HostPadding
-   ContentDialogButton2HostPadding
-   ContentDialogButtonsMinHeight
-   ContentDialogContentLandscapeWidth
-   ContentDialogContentMinHeight
-   ContentDialogDimmingColor
-   ContentDialogTitleMinHeight
-   ControlContextualInfoTextBlockStyle
-   ControlHeaderContentPresenterStyle
-   ControlHeaderTextBlockStyle
-   FlyoutContentPanelLandscapeThemeMargin
-   FlyoutContentPanelPortraitThemeMargin
-   GrabberMargin
-   GridViewItemMargin
-   GridViewItemPlaceholderBackgroundThemeBrush
-   GroupHeaderTextBlockStyle
-   HeaderContentPresenterStyle
-   HighContrastBlack
-   HighContrastWhite
-   HubHeaderCharacterSpacing
-   HubHeaderFontSize
-   HubHeaderMarginThickness
-   HubSectionHeaderCharacterSpacing
-   HubSectionHeaderFontSize
-   HubSectionHeaderMarginThickness
-   HubSectionMarginThickness
-   InlineWindowPlayPauseMargin
-   ItemTemplate
-   LeftFullWindowMargin
-   LeftMargin
-   ListViewEmptyStaticTextBlockStyle
-   ListViewItemContentTextBlockStyle
-   ListViewItemContentTranslateX
-   ListViewItemMargin
-   ListViewItemMultiselectCheckBoxMargin
-   ListViewItemSubheaderTextBlockStyle
-   ListViewItemTextBlockStyle
-   MediaControlPanelAudioThemeBrush
-   MediaControlPanelPhoneVideoThemeBrush
-   MediaControlPanelVideoThemeBrush
-   MediaControlPanelVideoThemeColor
-   MediaControlPlayPauseThemeBrush
-   MediaControlTimeRowThemeBrush
-   MediaControlTimeRowThemeColor
-   MediaDownloadProgressIndicatorThemeBrush
-   MediaErrorBackgroundThemeBrush
-   MediaTextThemeBrush
-   MenuFlyoutBackgroundThemeBrush
-   MenuFlyoutBorderThemeBrush
-   MenuFlyoutLandscapeThemePadding
-   MenuFlyoutLeftLandscapeBorderThemeThickness
-   MenuFlyoutPortraitBorderThemeThickness
-   MenuFlyoutPortraitThemePadding
-   MenuFlyoutRightLandscapeBorderThemeThickness
-   MessageDialogContentStyle
-   MessageDialogTitleStyle
-   MinimalWindowMargin
-   PasswordBoxCheckBoxThemeMargin
-   PhoneAccentBrush
-   PhoneBackgroundBrush
-   PhoneBackgroundColor
-   PhoneBaseBlackColor
-   PhoneBaseHighColor
-   PhoneBaseLowColor
-   PhoneBaseLowSolidColor
-   PhoneBaseMediumHighColor
-   PhoneBaseMediumMidColor
-   PhoneBaseMediumMidSolidColor
-   PhoneBaseMidColor
-   PhoneBaseWhiteColor
-   PhoneBorderThickness
-   PhoneButtonBasePressedForegroundBrush
-   PhoneButtonContentPadding
-   PhoneButtonFontWeight
-   PhoneButtonMinHeight
-   PhoneButtonMinWidth
-   PhoneChromeBrush
-   PhoneChromeColor
-   PhoneControlBackgroundColor
-   PhoneControlDisabledColor
-   PhoneControlForegroundColor
-   PhoneDisabledBrush
-   PhoneDisabledColor
-   PhoneFontFamilyLight
-   PhoneFontFamilySemiBold
-   PhoneForegroundBrush
-   PhoneForegroundColor
-   PhoneHighContrastSelectedBackgroundThemeBrush
-   PhoneHighContrastSelectedForegroundThemeBrush
-   PhoneImagePlaceholderColor
-   PhoneLowBrush
-   PhoneMidBrush
-   PhonePageBackgroundColor
-   PhonePivotLockedTranslation
-   PhonePivotUnselectedItemOpacity
-   PhoneRadioCheckBoxBorderBrush
-   PhoneRadioCheckBoxBrush
-   PhoneRadioCheckBoxCheckBrush
-   PhoneRadioCheckBoxPressedBrush
-   PhoneStrokeThickness
-   PhoneTextHighColor
-   PhoneTextLowColor
-   PhoneTextMidColor
-   PhoneTextOverAccentColor
-   PhoneTouchTargetLargeOverhang
-   PhoneTouchTargetOverhang
-   PivotHeaderItemPadding
-   PlaceholderContentPresenterStyle
-   ProgressBarHighContrastAccentBarThemeBrush
-   ProgressBarIndeterminateRectagleThemeSize
-   ProgressBarRectangleStyle
-   ProgressRingActiveBackgroundOpacity
-   ProgressRingElipseThemeMargin
-   ProgressRingElipseThemeSize
-   ProgressRingTextForegroundThemeBrush
-   ProgressRingTextThemeMargin
-   ProgressRingThemeSize
-   RichEditBoxTextThemeMargin
-   RightFullWindowMargin
-   RightMargin
-   ScrollBarMinThemeHeight
-   ScrollBarMinThemeWidth
-   ScrollBarPanningThumbThemeHeight
-   ScrollBarPanningThumbThemeWidth
-   SliderThumbDisabledBorderThemeBrush
-   SliderTrackBorderThemeBrush
-   SliderTrackDisabledBorderThemeBrush
-   TextBoxBackgroundColor
-   TextBoxBorderColor
-   TextBoxDisabledHeaderForegroundThemeBrush
-   TextBoxFocusedBackgroundThemeBrush
-   TextBoxForegroundColor
-   TextBoxPlaceholderColor
-   TextControlHeaderMarginThemeThickness
-   TextControlHeaderMinHeightSize
-   TextStyleExtraExtraLargeFontSize
-   TextStyleExtraLargePlusFontSize
-   TextStyleMediumFontSize
-   TextStyleSmallFontSize
-   TimeRemainingElementMargin

## <a name="searchbox-deprecated-in-favor-of-autosuggestbox"></a>Remplacement de SearchBox par AutoSuggestBox

Bien que la méthode [**SearchBox**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.searchbox) soit implémentée dans la famille d’appareils universelle, elle n’est pas entièrement fonctionnelle sur des appareils mobiles. Utilisez [**AutoSuggestBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox) pour votre expérience de recherche universelle. Voici une méthode classique pour implémenter une expérience de recherche avec **AutoSuggestBox**.

Quand l’utilisateur commence à taper, l’événement **TextChanged** est déclenché avec la raison **UserInput**. Vous complétez ensuite la liste des suggestions et définissez l’élément **ItemsSource** d’[**AutoSuggestBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox). Lorsque l’utilisateur parcourt la liste, l’événement **SuggestionChosen** est déclenché (et si vous avez défini **TextMemberDisplayPath**, la zone de texte est complétée automatiquement avec la propriété spécifiée). Quand l’utilisateur soumet un choix avec la touche Entrée, l’événement **QuerySubmitted** est déclenché. À ce stade, vous pouvez agir sur cette suggestion (dans ce cas, très probablement naviguer vers une autre page avec plus de détails sur le contenu spécifié). Notez que les propriétés **LinguisticDetails** et **Language** de **SearchBoxQuerySubmittedEventArgs** ne sont plus prises en charge (il existe des API équivalentes pour prendre en charge ces fonctionnalités). **KeyModifiers** n’est plus pris en charge

[**AutoSuggestBox** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AutoSuggestBox) prend également en charge pour les éditeurs de méthode d’entrée (IME). Et si vous souhaitez afficher une icône « Rechercher », vous le pouvez également (l’interaction avec l’icône déclenchera l’événement **QuerySubmitted**).

```xml
   <AutoSuggestBox ... >
        <AutoSuggestBox.QueryIcon>
            <SymbolIcon Symbol="Find"/>
        </AutoSuggestBox.QueryIcon>
    </AutoSuggestBox>
```

Voir également l’[exemple de portage AutoSuggestBox](https://go.microsoft.com/fwlink/p/?linkid=619996) (en anglais).

## <a name="semanticzoom-changes"></a>Modifications SemanticZoom

Le mouvement de zoom arrière d’un paramètre [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) a fait l’objet d’une convergence sur le modèle Windows Phone, consistant en un appui ou un clic sur un en-tête de groupe (ainsi, sur les ordinateurs de bureau, l’affordance du bouton moins pour un zoom arrière n’est plus affichée). Ainsi, nous avons donc garanti un comportement cohérent sur tous les appareils et ce, sans effort. Il existe cependant une différence d’ordre cosmétique avec le modèle doté de Windows Phone : la vue avec zoom arrière (liste de raccourcis) remplace la vue avec zoom avant au lieu de se superposer à elle. Pour cette raison, vous pouvez supprimer tous les arrière-plans semi-opaques des vues avec zoom arrière.

Dans une application Windows Phone Store, la vue Zoom arrière s’étend à la taille de l’écran. Dans une application de 8.x Windows Runtime et dans une application Windows 10, la taille de la vue Zoom arrière est contraint aux limites de la **SemanticZoom** contrôle.

Dans une application du Windows Phone Store, le contenu derrière la vue avec zoom arrière (dans l’ordre de plan) transparaît si la vue avec zoom arrière comporte une transparence dans son arrière-plan. Dans une application de 8.x Windows Runtime et dans une application Windows 10, rien n’est visible derrière la vue Zoom arrière.

Dans une application Windows Runtime 8.x lorsque l’application est désactivée et réactivée, la vue Zoom arrière est fermée (si elle a été affiché) et la vue Zoom avant est affichée à la place. Dans une application Windows Phone Store et dans une application Windows 10, la vue Zoom arrière reste affiche s’il a été affiché.

Dans une application Windows Phone Store et dans une application Windows 10, la vue Zoom arrière disparaît lorsque vous appuyez sur le bouton précédent. Pour une application Windows Runtime 8.x n’est aucun traitement de bouton précédent intégrée, donc la question ne s’applique pas.

## <a name="settings"></a>Paramètres

Le Runtime Windows 8.x **SettingsPane** classe n’est pas appropriée pour Windows 10. À la place, vous devez non seulement créer une page Paramètres, mais également permettre aux utilisateurs d’y accéder à partir de votre application. Nous vous recommandons d’exposer cette page Paramètres d’application au niveau supérieur, en tant que dernier élément épinglé sur votre volet de navigation. Toutefois, voici l’ensemble complet des options dont vous disposez.

-   Volet de navigation : l’élément Paramètres doit être le dernier de la liste de choix de navigation et épinglé en bas.
-   Barre de l’application/barre d’outils (dans un affichage d’onglets ou dans une disposition de zone dynamique) : l’élément Paramètres doit être le dernier du menu volant de barre de l’application ou de barre d’outils. Il est déconseillé de placer Paramètres parmi les éléments de niveau supérieur de la navigation.
-   Hub : l’élément Paramètres doit figurer dans le menu volant (soit dans le menu de barre de l’application, soit dans le menu de barre d’outils de la disposition Hub).

Il est déconseillé de reléguer l’élément Paramètres dans un volet maître/détail.

Votre page Paramètres doit remplir la totalité de la fenêtre de votre application, et doit également contenir les options À propos de et Commentaires. Pour obtenir des conseils sur la conception de votre page Paramètres, voir [Recommandations en matière de paramètres d’application](https://docs.microsoft.com/windows/uwp/app-settings/guidelines-for-app-settings).

## <a name="text"></a>Text

Le texte (ou la typographie) constitue un aspect important d’une application UWP et, pendant le portage, il vous sera peut-être utile de revoir les conceptions visuelles de vos vues afin de les harmoniser avec le nouveau langage de conception. Utilisez ces illustrations pour trouver les styles  **TextBlock** système de plateforme Windows universelle (UWP) qui sont disponibles. Trouver celles qui correspondent aux styles Windows Phone Silverlight que vous avez utilisé. Vous pouvez également créer vos propres styles universelles et copier les propriétés des styles de système de Windows Phone Silverlight dans ceux.

![Styles TextBlock système pour les applications Windows 10](images/label-uwp10stylegallery.png) <br/>Styles de TextBlock système pour les applications Windows 10

Dans les applications du Windows Phone Store et les applications Windows Runtime 8.x, la famille de polices par défaut est Global d’Interface utilisateur. Dans une application Windows 10, la famille de polices par défaut est Segoe UI. Par conséquent, les métriques de police dans votre application peuvent être différentes. Si vous souhaitez reproduire l’aspect de votre texte 8.1, vous pouvez définir vos propres métriques à l’aide de propriétés telles que [**LineHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.lineheight) et [**LineStackingStrategy**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.linestackingstrategy).

Dans les applications du Windows Phone Store et les applications Windows Runtime 8.x, la langue par défaut pour le texte est définie sur la langue de la build, ou à en-us. Dans une application Windows 10, la langue par défaut est définie pour la langue d’application supérieure (police de secours). Vous pouvez définir explicitement la propriété [**FrameworkElement.Language**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.language), mais le comportement de substitution des polices se révélera plus efficace si vous ne définissez aucune valeur pour cette propriété.

Pour plus d’informations, voir [Recommandations en matière de polices](https://docs.microsoft.com/windows/uwp/controls-and-patterns/fonts) et [Concevoir des applications UWP](https://go.microsoft.com/fwlink/p/?LinkID=533896). Voir également la section [Contrôles](#controls-and-control-styles-and-templates) ci-dessus relative aux modifications apportées aux contrôles de texte.

## <a name="theme-changes"></a>Modifications de thème

Pour une application universelle 8.1, le thème par défaut est sombre. Pour les appareils Windows 10, le thème par défaut a changé, mais vous pouvez contrôler le thème utilisé en déclarant un thème demandé dans App.xaml. Par exemple, pour utiliser un thème sombre sur tous les appareils, ajoutez `RequestedTheme="Dark"` à l’élément racine Application.

## <a name="tiles-and-toasts"></a>Vignettes et toasts

Pour les vignettes et des toasts, les modèles que vous utilisez actuellement continue de fonctionner dans votre application Windows 10. Toutefois, de nouveaux modèles adaptatifs sont disponibles ; ils sont décrits dans [Notifications, vignettes, toasts et badges](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-badges-notifications).

Auparavant, sur les ordinateurs de bureau, une notification toast était un message transitoire. Elle disparaissait et n’était plus récupérable si vous l’aviez manquée ou ignorée. Sur Windows Phone, si une notification toast est ignorée ou se ferme temporairement, elle est placée dans le Centre de maintenance. À présent, le Centre de maintenance n'est plus limité à la famille d’appareils mobiles.

Pour envoyer une notification toast, il n'est plus nécessaire de déclarer une fonctionnalité.

## <a name="window-size"></a>Taille de la fenêtre

Pour une application universelle 8.1, l’élément de manifeste d’application [**ApplicationView**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema2013/element-applicationview) est utilisé pour déclarer une largeur minimale de fenêtre. Dans votre application UWP, vous pouvez spécifier une taille minimale (largeur et hauteur) avec le code impératif. La taille minimale par défaut est 500 x 320 epx (plus petite taille minimale acceptée). La plus grande taille minimale acceptée est de 500 x 500 epx.

```csharp
   Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetPreferredMinSize
        (new Size { Width = 500, Height = 500 });
```

Rubrique suivante : [Portage pour le modèle d’E/S, d’appareil et d’application](w8x-to-uwp-input-and-sensors.md).

