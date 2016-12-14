---
author: mcleblanc
description: "La pratique de définition de l’interface utilisateur sous la forme de balisage XAML déclaratif convertit extrêmement bien des applications 8.1 universelles en applications UWP."
title: Portage du balisage XAML et de la couche interface utilisateur de Windows Runtime 8.x vers UWP
ms.assetid: 78b86762-7359-474f-b1e3-c2d7cf9aa907
translationtype: Human Translation
ms.sourcegitcommit: 9dc441422637fe6984f0ab0f036b2dfba7d61ec7
ms.openlocfilehash: ea8844925cc227d9f082595b039dd68164ad1228

---

# <a name="porting-windows-runtime-8x-xaml-and-ui-to-uwp"></a>Portage du balisage XAML et de la couche interface utilisateur de Windows Runtime 8.x vers UWP

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Rubrique précédente : [Résolution des problèmes](w8x-to-uwp-troubleshooting.md).

La pratique de définition de l’interface utilisateur sous la forme de balisage XAML déclaratif convertit extrêmement bien des applications 8.1 universelles aux applications de plateforme Windows universelle (UWP). Vous constaterez que la majeure partie de votre balisage est compatible, même si vous devrez peut-être apporter quelques ajustements aux clés de ressources système ou aux modèles personnalisés que vous utilisez. Le code impératif de vos modèles d’affichage nécessitera peu ou pas de modifications. Une grande partie, voire la majeure partie, du code de votre couche de présentation qui manipule les éléments d’interface utilisateur devrait également être simple à porter.

## <a name="imperative-code"></a>Code impératif

Si vous souhaitez simplement accéder à l’étape de construction de votre projet, vous pouvez commenter ou remplacer tout code non essentiel. Vous pouvez ensuite itérer un problème à la fois et consulter les rubriques suivantes de cette section (ainsi que la rubrique précédente : [Résolution des problèmes](w8x-to-uwp-troubleshooting.md)), jusqu’à ce que les problèmes de génération et d’exécution soient supprimés et le portage terminé.

## <a name="adaptiveresponsive-ui"></a>Interface utilisateur adaptative/réactive

Étant donné que votre application peut s’exécuter sur une large gamme d’appareils—présentant chacun différentes tailles d’écran et résolutions—, vous pouvez compléter la procédure minimale de portage de votre application en adaptant votre interface utilisateur afin d’en optimiser l’aspect sur ces appareils. Vous pouvez utiliser la fonctionnalité adaptative Gestionnaire d’état visuel pour détecter dynamiquement la taille de la fenêtre et modifier la disposition en conséquence. Un exemple de procédure à suivre est décrit à la section [Interface utilisateur adaptative](w8x-to-uwp-case-study-bookstore2.md) de la rubrique d’étude de cas Bookstore2.

## <a name="back-button-handling"></a>Gestion du bouton Précédent

Dans le cas des applications 8.1 universelles, les approches concernant l’interface utilisateur que vous présentez et les événements que vous gérez pour le bouton Précédent diffèrent selon que les applications sont destinées au Windows Store ou au Windows Phone Store. Pour les applications Windows 10, en revanche, vous pouvez adopter une seule et même approche dans votre application. Sur les appareils mobiles, le bouton est fourni à votre intention sous la forme d’un bouton capacitif sur l’appareil ou d’un bouton dans l’interpréteur de commandes. Sur un appareil de bureau, vous ajoutez un bouton au chrome de votre application chaque fois que cette dernière permet la navigation vers l’arrière. Ceci est indiqué dans la barre de titre des applications avec fenêtres ou dans la barre des tâches en mode tablette. L’événement de bouton Précédent est un concept universel sur toutes les familles d’appareils, et les boutons implémentés dans le matériel ou dans le logiciel déclenchent le même événement [**BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893596).

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

Vous n’avez pas besoin de modifier les parties de votre code qui s’intègrent à des icônes, mais vous devez ajouter certains éléments d’interface utilisateur à votre application afin de remplacer la barre Icônes, absente de l’environnement de Windows 10. Une application 8.1 universelle s’exécutant sur Windows 10 possède sa propre interface utilisateur de remplacement, fournie par le chrome rendu par le système dans la barre de titre de l’application.

## <a name="controls-and-control-stylestemplates"></a>Contrôles et styles/modèles de contrôle

Une application universelle 8.1 s’exécutant sur Windows 10 conserve l’apparence et le comportement de la version 8.1 pour les contrôles. Toutefois, lorsque vous portez cette application vers une application Windows 10, certaines différences dans l’apparence et le comportement sont à prendre en compte. L’architecture et la conception des contrôles restant essentiellement inchangées pour les applications Windows 10, les modifications portent principalement sur des améliorations en matière de [langage de conception](#design-language), de simplification et d’utilisation.

**Remarque** L’état visuel PointerOver est adapté aux styles/modèles personnalisés dans les applications Windows 10 et les applications du Windows Store, mais non dans les applications du Windows Phone Store. Pour cette raison (et à cause des clés de ressources système qui sont prises en charge pour les applications Windows 10), nous vous recommandons de réutiliser les styles/modèles personnalisés de vos applications du Windows Store quand vous portez votre application vers Windows 10.
Si vous voulez avoir l’assurance que vos styles/modèles personnalisés utilisent le dernier jeu d’états visuels et bénéficient des améliorations de performances apportées aux styles/modèles par défaut, modifiez une copie du nouveau modèle par défaut de Windows 10 et réappliquez votre personnalisation à ce dernier. Un exemple d’amélioration des performances correspond à la suppression de tous les éléments **Border** qui délimitaient précédemment un élément **ContentPresenter** ou Panel et au rendu de la bordure par un élément enfant.

Voici quelques exemples plus spécifiques de modifications apportées aux contrôles.

| Nom du contrôle | Modification |
|--------------|--------|
| **AppBar**   | Si vous utilisez le contrôle **AppBar** (qu’il est préférable de remplacer par [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927)), il n’est pas masqué par défaut dans une application Windows 10. Vous pouvez contrôler ce comportement avec la propriété [**AppBar.ClosedDisplayMode**](https://msdn.microsoft.com/library/windows/apps/dn633872). |
| **AppBar**, [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) | Dans une application Windows 10, **AppBar** et [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) comportent un bouton **Voir plus** (ellipse). |
| [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) | Dans une application du Windows Store, les commandes secondaires d’un contrôle [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) sont toujours visibles. Dans une application du Windows Phone Store et dans une application Windows 10, ces commandes n’apparaissent pas tant que la barre de commandes n’est pas ouverte. |
| [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) | Pour une application du Windows Phone Store, la valeur de [**CommandBar.IsSticky**](https://msdn.microsoft.com/library/windows/apps/hh701944) n’a pas d’influence sur la possibilité d’abandon interactif de la barre. Pour une application Windows 10, si **IsSticky** est défini sur true, l’élément **CommandBar** ignorera tout mouvement d’abandon interactif. |
| [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) | Dans une application Windows 10, l’élément [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) ne gère pas les événements [**EdgeGesture.Completed**](https://msdn.microsoft.com/library/windows/apps/hh701622) ni [**UIElement.RightTapped**](https://msdn.microsoft.com/library/windows/apps/br208984). Il ne réagit pas non plus à un appui ni à un balayage vers le haut. Vous avez toujours la possibilité de gérer ces événements et de définir [**IsOpen**](https://msdn.microsoft.com/library/windows/apps/hh701939). |
| [**DatePicker**](https://msdn.microsoft.com/library/windows/apps/dn298584), [**TimePicker**](https://msdn.microsoft.com/library/windows/apps/dn299280) | Passez en revue l’apparence de votre application avec les changements visuels apportés à [**DatePicker**](https://msdn.microsoft.com/library/windows/apps/dn298584) et [**TimePicker**](https://msdn.microsoft.com/library/windows/apps/dn299280). Pour une application Windows 10 s’exécutant sur un appareil mobile, ces contrôles n’accèdent plus à une page de sélection, mais à une fenêtre contextuelle révocable à l’aide d’un léger mouvement. |
| [**DatePicker**](https://msdn.microsoft.com/library/windows/apps/dn298584),[**TimePicker**](https://msdn.microsoft.com/library/windows/apps/dn299280) | Dans une application Windows 10, vous ne pouvez pas placer [**DatePicker**](https://msdn.microsoft.com/library/windows/apps/dn298584) ni [**TimePicker**](https://msdn.microsoft.com/library/windows/apps/dn299280) à l’intérieur d’un menu volant. Si vous souhaitez que ces contrôles s’affichent dans un contrôle de type fenêtre contextuelle, vous pouvez utiliser [**DatePickerFlyout**](https://msdn.microsoft.com/library/windows/apps/dn625013) et [**TimePickerFlyout**](https://msdn.microsoft.com/library/windows/apps/dn608313). |
| **GridView**, **ListView** | Pour **GridView**/**ListView**, voir la section [Modifications GridView/ListView](#gridview). |
| [**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843) | Dans une application du Windows Phone Store, un contrôle [**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843) exécute une boucle entre la dernière section et la première. Dans une application du Windows Store et dans une application Windows 10, les sections de hub n’exécutent aucune boucle. |
| [**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843) | Dans une application du Windows Phone Store, l’image d’arrière-plan d’un contrôle [**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843) se déplace en parallaxe par rapport aux sections de hub. Dans une application du Windows Store et dans une application Windows 10, l’effet parallaxe n’est pas utilisé. |
| [**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843)  | Dans une application 8.1 universelle, la propriété [**HubSection.IsHeaderInteractive**](https://msdn.microsoft.com/library/windows/apps/dn251917) rend interactifs l’en-tête de section et le glyphe de chevron rendu en regard de ce dernier. Dans une application Windows 10, il existe une affordance interactive « Voir plus » à côté de l’en-tête, mais l’en-tête proprement dit n’est pas interactif. **IsHeaderInteractive** détermine toujours si l’interaction déclenche l’événement [**Hub.SectionHeaderClick**](https://msdn.microsoft.com/library/windows/apps/dn251953). |
| **MessageDialog** | Si vous utilisez **MessageDialog**, préférez [**ContentDialog**](https://msdn.microsoft.com/library/windows/apps/dn633972), plus flexible. Voir également l’[exemple d’éléments de base d’une interface utilisateur XAML](http://go.microsoft.com/fwlink/p/?linkid=619992) (en anglais). |
| **ListPickerFlyout**, **PickerFlyout**  | **ListPickerFlyout** et **PickerFlyout** sont déconseillés pour une application Windows 10. Dans le cas d’un menu volant à sélection unique, utilisez [**MenuFlyout**](https://msdn.microsoft.com/library/windows/apps/dn299030) ; pour des expériences plus complexes, préférez [**Flyout**](https://msdn.microsoft.com/library/windows/apps/dn279496). |
| [**PasswordBox**](https://msdn.microsoft.com/library/windows/apps/br227519) | La propriété [**PasswordBox.IsPasswordRevealButtonEnabled**](https://msdn.microsoft.com/library/windows/apps/hh702579) est déconseillée dans une application Windows 10, et sa configuration n’a aucun effet. Utilisez plutôt la propriété [**PasswordBox.PasswordRevealMode**](https://msdn.microsoft.com/library/windows/apps/dn890867), définie par défaut sur **Peek** (qui affiche un glyphe d’œil, comme dans une application du Windows Store). Voir également l’article [Recommandations en matière de zones de mot de passe](https://msdn.microsoft.com/library/windows/apps/dn596103). |
| [**Pivot**](https://msdn.microsoft.com/library/windows/apps/dn608241) | Le contrôle [**Pivot**](https://msdn.microsoft.com/library/windows/apps/dn608241) est désormais universel et n’est plus limité aux appareils mobiles. |
| [**SearchBox**](https://msdn.microsoft.com/library/windows/apps/dn252771) | Bien que la méthode [**SearchBox**](https://msdn.microsoft.com/library/windows/apps/dn252803) soit implémentée dans la famille d’appareils universelle, elle n’est pas entièrement fonctionnelle sur des appareils mobiles. Voir [Remplacement de SearchBox par AutoSuggestBox](#searchbox). |
| **SemanticZoom** | Pour **SemanticZoom**, voir [Modifications SemanticZoom](#semantic-zoom). |
| [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527)  | Certaines propriétés par défaut de [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527) ont changé. [**HorizontalScrollMode**](https://msdn.microsoft.com/library/windows/apps/br209549) présente la valeur **Auto**, [**VerticalScrollMode**](https://msdn.microsoft.com/library/windows/apps/br209589) la valeur **Auto**, et [**ZoomMode**](https://msdn.microsoft.com/library/windows/apps/br209601) la valeur **Disabled**. Si les nouvelles valeurs par défaut ne sont pas adaptées à votre application, vous pouvez les modifier dans un style ou sous forme de valeurs locales sur le contrôle proprement dit.  |
| [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) | Dans une application du Windows Store, la vérification de l’orthographe est désactivée par défaut pour un élément [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683). Dans une application du Windows Phone Store et dans une application Windows 10, cette vérification est activée par défaut. |
| [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) | La taille de police par défaut d’un élément [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) est passée de 11 à 15. |
| [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) | La valeur par défaut de [**TextBox.TextReadingOrder**](https://msdn.microsoft.com/library/windows/apps/dn252859) est passée de **Default** à **DetectFromContent**. Si cette valeur ne convient pas, utilisez **UseFlowDirection**. La valeur **Default** est déconseillée. |
| Divers | La couleur d’accentuation s’applique aux applications du Windows Phone Store et aux applications Windows 10, mais non aux applications du Windows Store.  |

Pour plus d’informations sur les contrôles des applications UWP, voir [Contrôles par fonction](https://msdn.microsoft.com/library/windows/apps/mt185405), [Liste des contrôles](https://msdn.microsoft.com/library/windows/apps/mt185406) et [Recommandations relatives aux contrôles](https://msdn.microsoft.com/library/windows/apps/dn611856).

##  <a name="design-language-in-windows-10"></a>Langage de conception dans Windows 10

Il existe certaines différences légères, mais importantes dans le langage de conception entre les applications universelles 8.1 et Windows 10. Pour plus de détails, voir [Conception](http://dev.windows.com/design). Malgré les changements en matière de langage, nos principes de conception restent cohérents : être attentif aux détails, mais toujours viser la simplicité en se concentrant sur le contenu sans superflu, en réduisant à tout prix les éléments visuels et en restant authentique en matière de domaine numérique ; utiliser la hiérarchie visuelle, en particulier avec la typographie ; concevoir à l’aide d’une grille et donner vie à vos expériences grâce à des animations fluides.

## <a name="effective-pixels-viewing-distance-and-scale-factors"></a>Pixels effectifs, distance d’affichage et facteurs d’échelle

Auparavant, les pixels d’affichage permettaient d’extraire la taille et la disposition des éléments d’interface utilisateur de la taille physique et de la résolution réelles des appareils. Ces pixels ont évolué de manière à devenir des « pixels effectifs ». Voici ce que cette expression désigne, sa signification et la valeur ajoutée proposée.

Le terme « résolution » fait référence à la mesure de la densité des pixels et non, comme on le pense souvent, au nombre de pixels. La « résolution effective » est la façon dont les pixels physiques qui composent une image ou un glyphe apparaissent à l’œil, étant donné les différences liées à la distance de visualisation et à la taille des pixels physiques sur l’appareil (la densité de pixels étant l’inverse de la taille des pixels physiques). La résolution effective est une bonne unité de mesure pour créer une expérience, car elle est centrée sur l’utilisateur. La compréhension de tous ces facteurs et le contrôle de la taille des éléments d’interface utilisateur vous permettent de tirer parti de l’expérience utilisateur.

Les différents appareils utilisés présentent une largeur variable (en pixels effectifs). Celle-ci est de 320 epx sur les plus petits d’entre eux, de 1 024 epx sur les écrans de taille modeste et nettement plus grande sur d’autres. Il vous suffit de continuer à utiliser les éléments à dimensionnement automatique et les panneaux à disposition dynamique que vous utilisez depuis toujours. Dans certains cas, il se peut que vous définissiez une taille fixe pour les propriétés de vos éléments d’interface utilisateur dans le balisage XAML. Un facteur d’échelle est automatiquement affecté à votre application, en fonction de l’appareil sur lequel elle s’exécute et des paramètres d’affichage définis par l’utilisateur. Ce facteur permet aux éléments à taille fixe de l’interface utilisateur de conserver approximativement la même taille sur les différents formats d’écran de l’utilisateur, pour les opérations tactiles ou pour la lecture. Et avec la disposition dynamique, votre interface utilisateur ne sera pas seulement mise à l’échelle sur différents appareils. Elle s’efforcera également d’adapter la quantité de contenu appropriée à l’espace disponible.

Pour que votre application offre une expérience optimale sur tous les écrans, nous vous recommandons de créer chaque ressource bitmap dans différentes tailles, chacune étant adaptée à un facteur d’échelle spécifique. Fournir des ressources aux échelles 100 %, 200 % et 400 % (dans cet ordre de priorité) produit d’excellents résultats dans la plupart des cas à tous les facteurs d’échelle intermédiaires.

**Remarque** Si, pour une raison quelconque, vous ne pouvez pas créer de ressources dans plusieurs tailles, créez des ressources à l’échelle 100 %. Dans Microsoft Visual Studio, le modèle de projet par défaut pour les applications UWP fournit des ressources de personnalisation (vignettes et logos) dans une seule taille, mais elles ne sont pas à l’échelle 100 %. Lorsque vous créez des ressources pour votre propre application, suivez les recommandations de cette section, fournissez des tailles 100 %, 200 % et 400 %, et utilisez des packs de ressources.

Si vous disposez d’illustrations complexes, vous serez peut-être amené à fournir vos ressources dans un plus grand nombre de tailles. Si vous débutez avec une image vectorielle, il est relativement aisé de générer des ressources de haute qualité à n’importe quel facteur d’échelle.

Nous ne vous recommandons pas d’essayer de prendre en charge tous les facteurs d’échelle, mais la liste complète des facteurs d’échelle pour les applications Windows 10 est la suivante : 100 %, 125 %, 150 %, 200 %, 250 %, 300 % et 400 %. Si vous fournissez ces facteurs, le Windows Store sélectionne les ressources de taille appropriée pour chaque appareil, et seules ces ressources sont téléchargées. Le Windows Store sélectionne les ressources à télécharger en fonction de la résolution de l’appareil. Vous pouvez réutiliser des ressources de votre application du Windows Store à des facteurs d’échelle tels que 140 % et 220 %, mais votre application s’exécutera dans l’un des nouveaux facteurs d’échelle et par conséquent, une mise à l’échelle des bitmaps sera inévitable. Testez votre application sur divers appareils pour voir si vous êtes satisfait des résultats dans votre cas.

Vous réutilisez peut-être le balisage XAML d’une application du Windows Store dans laquelle des valeurs de dimensions littérales sont exploitées dans le balisage (pour le dimensionnement de formes ou d’autres éléments, à des fins de typographie, par exemple). Mais dans certains cas, le facteur d’échelle utilisé sur un appareil pour une application Windows 10 est plus élevé que pour une application universelle 8.1 (par exemple, utilisation du facteur 150 % contre 140 % auparavant, et du facteur 200 % plutôt que 180 %). Si vous pensez que ces valeurs littérales sont trop élevées dans Windows 10, essayez de les multiplier par 0,8. Pour plus d’informations, voir [Conception réactive 101 pour les applications UWP](https://msdn.microsoft.com/library/windows/apps/dn958435).

## <a name="gridviewlistview-changes"></a>Modifications GridView/ListView

Plusieurs modifications ont été apportées aux méthodes Style Setters par défaut associées à l’élément [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705), afin de permettre le défilement vertical du contrôle (auparavant, le défilement était effectué horizontalement par défaut). Si vous avez modifié une copie du style par défaut dans votre projet, votre copie ne comportera pas ces modifications. Vous devrez donc les effectuer manuellement. Voici une liste des modifications apportées.

-   La méthode setter associée à l’élément [**ScrollViewer.HorizontalScrollBarVisibility**](https://msdn.microsoft.com/library/windows/apps/br209547) est passée de la valeur **Auto** à la valeur **Disabled**.
-   La méthode setter associée à l’élément [**ScrollViewer.VerticalScrollBarVisibility**](https://msdn.microsoft.com/library/windows/apps/br209587) est passée de la valeur **Disabled** à la valeur **Auto**.
-   La méthode setter associée à l’élément [**ScrollViewer.HorizontalScrollMode**](https://msdn.microsoft.com/library/windows/apps/br209549) est passée de la valeur **Enabled** à la valeur **Disabled**.
-   La méthode setter associée à l’élément [**ScrollViewer.VerticalScrollMode**](https://msdn.microsoft.com/library/windows/apps/br209589) est passée de la valeur **Disabled** à la valeur **Enabled**.
-   Dans la méthode setter associée à l’élément [**ItemsPanel**](https://msdn.microsoft.com/library/windows/apps/br242826), la valeur de [**ItemsWrapGrid.Orientation**](https://msdn.microsoft.com/library/windows/apps/dn298907) est passée de **Vertical** à **Horizontal**.

Si cette dernière modification (vers **Orientation**) vous semble contradictoire, rappelez-vous que nous parlons d’une grille de renvoi à la ligne. Une grille de renvoi à la ligne avec orientation horizontale (nouvelle valeur) est comparable à un système d’écriture dans lequel le texte s’affiche à l’horizontale et passe à la ligne suivante à la fin d’une page. Une page de texte de ce type fait l’objet d’un défilement vertical. À l’inverse, une grille de renvoi à la ligne avec orientation verticale (valeur précédente) est similaire à un système d’écriture dans lequel le texte s’affiche à la verticale et, de ce fait, défile à l’horizontale.

Voici les aspects de [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705) et [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) qui ont été modifiés ou qui ne sont pas pris en charge sous Windows 10.

-   La propriété [**IsSwipeEnabled**](https://msdn.microsoft.com/library/windows/apps/hh702518) (applications du Windows Store uniquement) n’est pas prise en charge pour les applications Windows 10. L’API est toujours présente, mais le paramètre n’a aucun effet. Tous les précédents mouvements de sélection sont pris en charge, à l’exception du balayage vers le bas (non pris en charge, car les données montrent qu’il n’est pas détectable) et du clic avec le bouton droit (qui est réservé à l’affichage d’un menu contextuel).
-   La propriété [**ReorderMode**](https://msdn.microsoft.com/library/windows/apps/dn625099) (applications du Windows Phone Store uniquement) n’est pas prise en charge pour les applications Windows 10. L’API est toujours présente, mais sa configuration n’a aucun effet. En définissant [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/br208912) et [**CanReorderItems**](https://msdn.microsoft.com/library/windows/apps/br242882) sur true dans votre élément **GridView** ou **ListView**, vous permettrez à l’utilisateur d’effectuer une réorganisation à l’aide d’un mouvement d’appui prolongé (ou de clic et de glissement).
-   Lorsque vous développez une application pour Windows 10, utilisez [**ListViewItemPresenter**](https://msdn.microsoft.com/library/windows/apps/dn298500) à la place de [**GridViewItemPresenter**](https://msdn.microsoft.com/library/windows/apps/dn279298) dans votre style de conteneur d’éléments, à la fois pour [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) et pour [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705). Si vous modifiez une copie des styles de conteneur d’éléments par défaut, vous obtiendrez le type correct.
-   Les éléments visuels de sélection ont été modifiés pour une application Windows 10. Si vous définissez [**SelectionMode**](https://msdn.microsoft.com/library/windows/apps/br242915) sur **Multiple**, une case à cocher est affichée par défaut pour chaque élément. Le paramétrage par défaut pour les éléments **ListView** signifie que la case à cocher est incluse en regard de l’élément, et par conséquent, l’espace occupé par le reste de l’élément sera légèrement réduit et déplacé. Pour les éléments **GridView**, la case à cocher est placée sur l’élément par défaut. Dans les deux cas, vous pouvez contrôler la disposition (incluse ou superposée) des cases à cocher (avec la propriété [**CheckMode**](https://msdn.microsoft.com/library/windows/apps/dn913923)) et leur affichage (avec la propriété [**SelectionCheckMarkVisualEnabled**](https://msdn.microsoft.com/library/windows/apps/dn298541)) sur l’élément [**ListViewItemPresenter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.aspx) à l’intérieur de votre style de conteneur d’éléments, comme dans l’exemple ci-dessous.
-   Dans Windows 10, l’événement [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/dn298914) est déclenché deux fois par élément pendant la virtualisation de l’interface utilisateur : une fois pour la récupération et une autre pour la réutilisation. Si [**InRecycleQueue**](https://msdn.microsoft.com/library/windows/apps/dn279443) est défini sur **true** et que vous n’avez aucun travail de récupération particulier à effectuer, vous pouvez quitter votre gestionnaire d’événements immédiatement avec la certitude qu’il se rouvrira quand ce même élément sera réutilisé (moment auquel **InRecycleQueue** prendra la valeur **false**).

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

-   En raison de la suppression des mouvements de balayage vers le bas et de clic avec le bouton droit pour la sélection (pour les raisons exposées ci-dessus), le modèle d’interaction a changé. En conséquence, les événements [**ItemClick**](https://msdn.microsoft.com/library/windows/apps/br242904) et [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) ne s’excluent plus mutuellement. Pour votre application Windows 10, passez en revue vos scénarios et décidez si vous souhaitez adopter le modèle d’interaction « sélection » ou « appel ». Pour plus de détails, voir [Comment changer le mode d’interaction](https://msdn.microsoft.com/library/windows/apps/xaml/hh780625).
-   Les propriétés que vous utilisez pour styliser [**ListViewItemPresenter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.aspx) ont été modifiées. Nouvelles propriétés : [**CheckBoxBrush**](https://msdn.microsoft.com/library/windows/apps/dn913905), [**PressedBackground**](https://msdn.microsoft.com/library/windows/apps/dn913931), [**SelectedPressedBackground**](https://msdn.microsoft.com/library/windows/apps/dn913937) et [**FocusSecondaryBorderBrush**](https://msdn.microsoft.com/library/windows/apps/dn898370). Propriétés ignorées pour une application Windows 10 : [**Padding**](https://msdn.microsoft.com/library/windows/apps/dn424775) (utilisez [**ContentMargin**](https://msdn.microsoft.com/library/windows/apps/dn424773) à la place), [**CheckHintBrush**](https://msdn.microsoft.com/library/windows/apps/dn298504), [**CheckSelectingBrush**](https://msdn.microsoft.com/library/windows/apps/dn298506), [**PointerOverBackgroundMargin**](https://msdn.microsoft.com/library/windows/apps/dn424778), [**ReorderHintOffset**](https://msdn.microsoft.com/library/windows/apps/dn298528), [**SelectedBorderThickness**](https://msdn.microsoft.com/library/windows/apps/dn298533) et [**SelectedPointerOverBorderBrush**](https://msdn.microsoft.com/library/windows/apps/dn298539).

Le tableau suivant décrit les modifications apportées aux états visuels et aux groupes d’états visuels dans les modèles de contrôle [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/br242919) et [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/hh738501).

| 8.1                 |                         | Windows 10        |                     |
|---------------------|-------------------------|-------------------|---------------------|
| CommonStates        |                         | CommonStates      |                     |
|                     | Normal                  |                   | Normal              |
|                     | PointerOver             |                   | PointerOver         |
|                     | Pressed                 |                   | Pressed             |
|                     | PointerOverPressed      |                   | [non disponible]       |
|                     | Désactivée                |                   | [non disponible]       |
|                     | [non disponible]           |                   | PointerOverSelected |
|                     | [non disponible]           |                   | Sélectionné            |
|                     | [non disponible]           |                   | PressedSelected     |
| [non disponible]       |                         | DisabledStates    |                     |
|                     | [non disponible]           |                   | Désactivée            |
|                     | [non disponible]           |                   | Activé             |
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

Si vous avez personnalisé un modèle de contrôle [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/br242919) ou [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/hh738501), passez-le en revue en tenant compte des modifications ci-dessus. Nous vous recommandons de commencer par modifier une copie du nouveau modèle par défaut, puis de lui réappliquer votre personnalisation. Si pour une raison quelconque, vous ne pouvez pas effectuer cette opération, mais que vous devez modifier votre modèle existant, voici quelques recommandations générales sur la procédure à suivre.

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

Vous pouvez réutiliser les fichiers Resources.resw de votre projet 8.1 universel dans votre projet d’application UWP. Après avoir copié le fichier, ajoutez-le au projet et définissez **Action de génération** sur **PRIResource** et **Copier dans le répertoire de sortie** sur **Ne pas copier**. La rubrique [**ResourceContext.QualifierValues**](https://msdn.microsoft.com/library/windows/apps/br206071) décrit comment charger des ressources propres à la famille d’appareils en fonction du facteur de sélection de ressources de cette dernière.

## <a name="play-to"></a>Lire sur

Les API de l’espace de noms [**Windows.Media.PlayTo**](https://msdn.microsoft.com/library/windows/apps/br207025) sont déconseillées pour les applications Windows 10 et doivent être remplacées par les API de l’espace de noms [**Windows.Media.Casting**](https://msdn.microsoft.com/library/windows/apps/dn972568).

## <a name="resource-keys-and-textblock-style-sizes"></a>Clés de ressources et tailles de style TextBlock

Le langage de conception a évolué pour Windows 10. Suite à cela, certains styles système ont été modifiés. Dans certains cas, il vous sera utile de revoir les conceptions visuelles de vos affichages afin de les harmoniser avec les propriétés de style modifiées.

Dans d’autres cas, les clés de ressources ne sont plus prises en charge. L’éditeur de balisage XAML dans Visual Studio met en surbrillance les références aux clés de ressources qui ne peuvent pas être résolues. Par exemple, il souligne une référence à la clé de style `ListViewItemTextBlockStyle` d’une ligne ondulée rouge. Si ce n’est pas corrigé, l’application s’arrête immédiatement lorsque vous essayez de la déployer vers l’émulateur ou l’appareil. Il est donc important de veiller à l’exactitude du balisage XAML. Et vous allez découvrir que Visual Studio est un formidable outil pour intercepter ces problèmes.

Pour les clés qui sont toujours prises en charge, les modifications apportées au langage de conception signifient que les propriétés définies par certains styles ont changé. Par exemple, `TitleTextBlockStyle` définit **FontSize** sur 14,667 px dans une application du Windows Store et sur 18,14 px dans une application du Windows Phone Store. Mais le même style définit **FontSize** sur la valeur plus élevée 24 px dans une application Windows 10. Passez en revue vos conceptions et dispositions et utilisez les styles appropriés aux endroits adéquats. Pour plus d’informations, voir [Recommandations en matière de polices](https://msdn.microsoft.com/library/windows/apps/hh700394.aspx) et [Concevoir des applications UWP](http://dev.windows.com/design).

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

Bien que la méthode [**SearchBox**](https://msdn.microsoft.com/library/windows/apps/dn252803) soit implémentée dans la famille d’appareils universelle, elle n’est pas entièrement fonctionnelle sur des appareils mobiles. Utilisez [**AutoSuggestBox**](https://msdn.microsoft.com/library/windows/apps/dn633874) pour votre expérience de recherche universelle. Voici une méthode classique pour implémenter une expérience de recherche avec **AutoSuggestBox**.

Quand l’utilisateur commence à taper, l’événement **TextChanged** est déclenché avec la raison **UserInput**. Vous complétez ensuite la liste des suggestions et définissez l’élément **ItemsSource** d’[**AutoSuggestBox**](https://msdn.microsoft.com/library/windows/apps/dn633874). Lorsque l’utilisateur parcourt la liste, l’événement **SuggestionChosen** est déclenché (et si vous avez défini **TextMemberDisplayPath**, la zone de texte est complétée automatiquement avec la propriété spécifiée). Quand l’utilisateur soumet un choix avec la touche Entrée, l’événement **QuerySubmitted** est déclenché. À ce stade, vous pouvez agir sur cette suggestion (dans ce cas, très probablement naviguer vers une autre page avec plus de détails sur le contenu spécifié). Notez que les propriétés **LinguisticDetails** et **Language** de **SearchBoxQuerySubmittedEventArgs** ne sont plus prises en charge (il existe des API équivalentes pour prendre en charge ces fonctionnalités). **KeyModifiers** n’est plus pris en charge

[**AutoSuggestBox**](https://msdn.microsoft.com/library/windows/apps/dn633874) prend également en charge les éditeurs de méthode d’entrée (IME). Et si vous souhaitez afficher une icône « Rechercher », vous le pouvez également (l’interaction avec l’icône déclenchera l’événement **QuerySubmitted**).

```xml
   <AutoSuggestBox ... >
        <AutoSuggestBox.QueryIcon>
            <SymbolIcon Symbol="Find"/>
        </AutoSuggestBox.QueryIcon>
    </AutoSuggestBox>
```

Voir également l’[exemple de portage AutoSuggestBox](http://go.microsoft.com/fwlink/p/?linkid=619996) (en anglais).

## <a name="semanticzoom-changes"></a>Modifications SemanticZoom

Le mouvement de zoom arrière d’un paramètre [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) a fait l’objet d’une convergence sur le modèle Windows Phone, consistant en un appui ou un clic sur un en-tête de groupe (ainsi, sur les ordinateurs de bureau, l’affordance du bouton moins pour un zoom arrière n’est plus affichée). Ainsi, nous avons donc garanti un comportement cohérent sur tous les appareils et ce, sans effort. Il existe cependant une différence d’ordre cosmétique avec le modèle doté de Windows Phone : la vue avec zoom arrière (liste de raccourcis) remplace la vue avec zoom avant au lieu de se superposer à elle. Pour cette raison, vous pouvez supprimer tous les arrière-plans semi-opaques des vues avec zoom arrière.

Dans une application du Windows Phone Store, la vue avec zoom arrière s’adapte à la taille de l’écran. Dans une application du Windows Store et dans une application Windows 10, la taille de la vue avec zoom arrière est contrainte par les limites du contrôle **SemanticZoom**.

Dans une application du Windows Phone Store, le contenu derrière la vue avec zoom arrière (dans l’ordre de plan) transparaît si la vue avec zoom arrière comporte une transparence dans son arrière-plan. Dans une application du Windows Store et dans une application Windows 10, aucun contenu n’est visible derrière la vue avec zoom arrière.

Dans le cas d’une application du Windows Store, lorsque l’application est désactivée puis réactivée, la vue avec zoom arrière est masquée (si elle était affichée) et est remplacée par la vue avec zoom avant. Dans une application du Windows Phone Store et dans une application Windows 10, la vue avec zoom arrière reste affichée si elle était visible précédemment.

Dans une application du Windows Phone Store et dans une application Windows 10, la vue avec zoom arrière est masquée lorsque l’utilisateur appuie sur le bouton Précédent. Une application du Windows Store n’intègre aucun traitement de bouton Précédent. Elle n’est donc pas concernée par ce comportement.

## <a name="settings"></a>Paramètres

La classe Windows Runtime 8.x **SettingsPane** n’est pas adaptée à Windows 10. À la place, vous devez non seulement créer une page Paramètres, mais également permettre aux utilisateurs d’y accéder à partir de votre application. Nous vous recommandons d’exposer cette page Paramètres d’application au niveau supérieur, en tant que dernier élément épinglé sur votre volet de navigation. Toutefois, voici l’ensemble complet des options dont vous disposez.

-   Volet de navigation : l’élément Paramètres doit être le dernier de la liste de choix de navigation et épinglé en bas.
-   Barre de l’application/barre d’outils (dans un affichage d’onglets ou dans une disposition de zone dynamique) : l’élément Paramètres doit être le dernier du menu volant de barre de l’application ou de barre d’outils. Il est déconseillé de placer Paramètres parmi les éléments de niveau supérieur de la navigation.
-   Hub : l’élément Paramètres doit figurer dans le menu volant (soit dans le menu de barre de l’application, soit dans le menu de barre d’outils de la disposition Hub).

Il est déconseillé de reléguer l’élément Paramètres dans un volet maître/détail.

Votre page Paramètres doit remplir la totalité de la fenêtre de votre application, et doit également contenir les options À propos de et Commentaires. Pour obtenir des conseils sur la conception de votre page Paramètres, voir [Recommandations en matière de paramètres d’application](https://msdn.microsoft.com/library/windows/apps/hh770544).

## <a name="text"></a>Texte

Le texte (ou la typographie) constitue un aspect important d’une application UWP et, pendant le portage, il vous sera peut-être utile de revoir les conceptions visuelles de vos vues afin de les harmoniser avec le nouveau langage de conception. Utilisez ces illustrations pour trouver les styles  **TextBlock** système de plateforme Windows universelle (UWP) qui sont disponibles. Recherchez ceux qui correspondent aux styles de Silverlight pour Windows Phone que vous avez utilisés. Vous pouvez également créer vos propres styles universels et y copier les propriétés des styles système de Silverlight pour Windows Phone.

![Styles TextBlock système pour les applications Windows 10](images/label-uwp10stylegallery.png) <br/>Styles TextBlock système pour les applications Windows 10

Dans les applications du Windows Store et du Windows Phone Store, la famille de polices par défaut est l’Interface utilisateur globale. Dans une application Windows 10, la famille de polices par défaut est Segoe UI. Par conséquent, les métriques de police dans votre application peuvent être différentes. Si vous souhaitez reproduire l’aspect de votre texte 8.1, vous pouvez définir vos propres métriques à l’aide de propriétés telles que [**LineHeight**](https://msdn.microsoft.com/library/windows/apps/br209671) et [**LineStackingStrategy**](https://msdn.microsoft.com/library/windows/apps/br244362).

Dans les applications du Windows Store et du Windows Phone Store, la langue par défaut du texte est définie sur la langue de la build, ou sur en-us. Dans une application Windows 10, la langue par défaut est définie sur la langue de l’application supérieure (substitution des polices). Vous pouvez définir explicitement la propriété [**FrameworkElement.Language**](https://msdn.microsoft.com/library/windows/apps/hh702066), mais le comportement de substitution des polices se révélera plus efficace si vous ne définissez aucune valeur pour cette propriété.

Pour plus d’informations, voir [Recommandations en matière de polices](https://msdn.microsoft.com/library/windows/apps/hh700394.aspx) et [Concevoir des applications UWP](http://go.microsoft.com/fwlink/p/?LinkID=533896). Voir également la section [Contrôles](#controls) ci-dessus relative aux modifications apportées aux contrôles de texte.

## <a name="theme-changes"></a>Modifications de thème

Pour une application universelle 8.1, le thème par défaut est sombre. Pour les appareils Windows 10, le thème par défaut a changé, mais vous pouvez contrôler le thème utilisé en déclarant un thème demandé dans App.xaml. Par exemple, pour utiliser un thème sombre sur tous les appareils, ajoutez `RequestedTheme="Dark"` à l’élément racine Application.

## <a name="tiles-and-toasts"></a>Vignettes et toasts

Pour les vignettes et toasts, les modèles que vous utilisez actuellement continueront à fonctionner dans votre application Windows 10. Toutefois, de nouveaux modèles adaptatifs sont disponibles ; ils sont décrits dans [Notifications, vignettes, toasts et badges](https://msdn.microsoft.com/library/windows/apps/mt185606).

Auparavant, sur les ordinateurs de bureau, une notification toast était un message transitoire. Elle disparaissait et n’était plus récupérable si vous l’aviez manquée ou ignorée. Sur Windows Phone, si une notification toast est ignorée ou se ferme temporairement, elle est placée dans le Centre de maintenance. À présent, le Centre de maintenance n'est plus limité à la famille d’appareils mobiles.

Pour envoyer une notification toast, il n'est plus nécessaire de déclarer une fonctionnalité.

## <a name="window-size"></a>Taille de la fenêtre

Pour une application universelle 8.1, l’élément de manifeste d’application [**ApplicationView**](https://msdn.microsoft.com/library/windows/apps/dn391667) est utilisé pour déclarer une largeur minimale de fenêtre. Dans votre application UWP, vous pouvez spécifier une taille minimale (largeur et hauteur) avec le code impératif. La taille minimale par défaut est 500 x 320 epx (plus petite taille minimale acceptée). La plus grande taille minimale acceptée est de 500 x 500 epx.

```csharp
   Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetPreferredMinSize
        (new Size { Width = 500, Height = 500 });
```

Rubrique suivante : [Portage pour le modèle d’E/S, d’appareil et d’application](w8x-to-uwp-input-and-sensors.md).




<!--HONumber=Dec16_HO1-->


