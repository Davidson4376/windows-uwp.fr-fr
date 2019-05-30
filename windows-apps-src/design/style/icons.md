---
Description: Des icônes efficaces s’harmonisent efficacement avec la typographie et avec le reste du langage de conception. Elles ne sont pas ambiguës, et communiquent un message sans superflu, aussi rapidement et simplement que possible.
title: Icônes
ms.assetid: b90ac02d-5467-4304-99bd-292d6272a014
label: Icons
template: detail.hbs
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5e464251200812e79474d05d9d0a680b49167871
ms.sourcegitcommit: 7da28cf4f4e8390bc9a21a9488b03af39271cbbe
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64564537"
---
# <a name="icons-for-uwp-apps"></a>Icônes pour les applications UWP

![image d'en-tête d'icônes](images/icons/header-icons.png)

Les icônes fournissent un raccourci visuel pour une action, un concept ou un produit. En condensant une signification dans une image symbolique, les icônes peuvent franchir les barrières des langues et préserver une ressource extrêmement utile : l'espace à l’écran. 

Les icônes peuvent apparaître dans les applications, ainsi qu'à l’extérieur : 

:::row:::
    :::column:::
        **Icons inside the app**

        ![icons inside the app](images/icons/inside-icons.png)
À l’intérieur de votre application, vous utilisez les icônes pour représenter une action, telles que copier du texte ou en accédant à la page Paramètres.
    :::column-end:::
    :::column:::
**Icônes en dehors de l’application**

        ![icons outside the app](images/icons/outside-icons.jpg)
En dehors de votre application, Windows utilise une icône pour représenter votre application dans le menu Démarrer et dans la barre des tâches. Si l’utilisateur choisit d’épingler votre application dans le menu Démarrer, la vignette de votre application fonctionnalité icône de votre application. Icône de votre application s’affiche dans la barre de titre, et vous pouvez choisir de créer un écran de démarrage avec le logo de votre application.
    :::column-end:::
:::row-end:::

Cet article décrit les icônes au sein de votre application. Pour en savoir plus sur les icônes en dehors de votre application (icônes d’application), voir l'[article Icônes d'application et de vignette](/windows/uwp/design/shell/tiles-and-notifications/app-assets).

## <a name="when-to-use-icons"></a>Quand utiliser les icônes

Les icônes permettent d'économiser de l’espace, mais quand faut-il les utiliser ? 

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icons standard image](images/icons/icons-standard.svg)<br>

Utiliser une icône pour les actions, telles que couper, copier, coller et enregistrer ou pour les éléments de navigation dans un menu de navigation.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![icons concept image](images/icons/icons-concept.svg)<br>

Utiliser une icône s’il en existe déjà pour le concept que vous souhaitez représenter. (Pour voir si une icône existe, vérifiez la liste d’icône Segoe.)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icon shopping cart](images/icons/icon-shopping-cart.svg)<br>

Utiliser une icône s’il est facile de l’utilisateur à comprendre ce que signifie l’icône et elle est suffisamment simple être clair de petite taille.
    :::column-end:::
    :::column:::
        ![dont](images/dont.svg)
        ![icons concept image](images/icons/icon-bad-example.png)<br>

N’utilisez pas une icône si sa signification n’est pas évident, ou si rend effacer requiert une forme complexe.
    :::column-end:::
:::row-end:::



## <a name="using-the-right-type-of-icon"></a>Utiliser le type approprié d’icône

Il existe de nombreuses façons de créer une icône. Vous pouvez utiliser une police de symboles telle que la police Segoe MDL2 Assets. Vous pouvez créer votre propre image vectorielle. Vous pouvez même utiliser une image bitmap, bien que nous ne le conseillions pas. Voici un résumé de différentes façons d'ajouter une icône à votre application. 

### <a name="use-a-predefined-icon"></a>Utilisez une icône prédéfinie.
:::row:::
    :::column:::
Microsoft fournit des icônes plus de 1000 sous la forme de la police Segoe MDL2 actifs. Il peut ne pas être intuitif d'obtenir une icône à partir d’une police, mais notre technologie d’affichage de police donne à ces icônes un aspect net et précis sur n’importe quel écran, à n'importe quelle résolution et à n’importe quelle taille. Pour obtenir des instructions, consultez [Segoe MDL2 icônes](segoe-ui-symbol-font.md).
    :::column-end:::
    :::column:::
        ![pre-defined icon image](images/icons/predefined-icon.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-font"></a>Utilisez une police.
:::row:::
    :::column:::
Vous n’êtes pas obligé d’utiliser la police Segoe MDL2 actifs--vous pouvez utiliser n’importe quelle police que l’utilisateur a installé sur leur système, telles que Wingdings ou Webdings.
    :::column-end:::
    :::column:::
        ![wingdings image](images/icons/wingdings.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-scalable-vector-graphics-svg-file"></a>Utilisez un fichier SVG (Scalable Vector Graphics).
:::row:::
    :::column:::
Ressources SVG sont idéales pour les icônes, car ils ne cherchent toujours nets à toute taille ou la résolution. La plupart des applications de dessin peuvent s'exporter au format SVG. Pour obtenir des instructions, consultez [SVGImageSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.svgimagesource).
    :::column-end:::
    :::column:::
        ![SVG image](images/icons/icon-scale.gif)
    :::column-end:::
:::row-end:::

### <a name="use-geometry-objects"></a>Utilisez des objets de géométrie.
:::row:::
    :::column:::
Comme les fichiers SVG, les géométries sont une ressource basée sur un vecteur, afin qu’ils ne cherchent toujours aigus. Toutefois, la création d’une géométrie est complexe, car vous devez spécifier individuellement chaque point et chaque courbe. En réalité, ce n'est un bon choix que si vous devez modifier l’icône pendant que votre application est en cours d’exécution (pour l’animer, par exemple). Pour obtenir des instructions, voir [Commandes de déplacement et de dessin pour les géométries](../../xaml-platform/move-draw-commands-syntax.md). 
    :::column-end:::
    :::column:::
        ![Geometry objects image](images/icons/geometry-objects.png)
    :::column-end:::
:::row-end:::

### <a name="you-can-also-use-a-bitmap-image-such-as-png-gif-or-jpeg-although-we-dont-recommend-it"></a>Vous pouvez également utiliser une image bitmap, par exemple une image PNG, GIF ou JPEG, bien que nous ne le conseillions pas.
:::row:::
    :::column:::
Images bitmap sont créés dans une taille spécifique, afin qu’ils aient à être mis à l’échelle vers le haut ou vers le bas en fonction de la taille que vous souhaitez l’icône pour être et la résolution de l’écran. Lorsque l’image est réduite, elle peut paraître floue ; lorsqu’elle est agrandie, elle peut paraître pixelisée. Si vous devez utiliser une image bitmap, nous vous recommandons d’utiliser le format PNG ou GIF plutôt qu'une image JPEG. 
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![Bitmap image](images/icons/bitmap-image.png)
    :::column-end:::
:::row-end:::

## <a name="make-the-icon-do-something"></a>Faites faire une action à l’icône

Une fois que vous avez une icône, l’étape suivante consiste à lui faire faire quelque chose en l’associant à la commande ou une action de navigation. La meilleure façon de procéder consiste à ajouter l’icône sur un bouton ou une barre de commandes. 

![Image d'une barre de commandes](images/icons/app-bar-desktop.svg)

## <a name="create-an-icon-button"></a>Créer un bouton d’icône

Vous pouvez placer une icône dans un bouton standard. Dans la mesure où vous pouvez utiliser les boutons dans une plus grande variété de lieux, cela vous donne un peu plus de souplesse pour l'emplacement d'affichage de votre icône d’action. 

Il existe plusieurs méthodes pour ajouter une icône à un bouton :

:::row:::
    :::column span="2":::
        <b>Step 1</b><br>
La valeur de la famille de polices du bouton `Segoe MDL2 Assets` et sa propriété de contenu à la valeur unicode du glyphe que vous souhaitez utiliser :
    :::column-end:::
    :::column:::
        ![Create an icon button step 1](images/icons/create-icon-step-1.svg)
    :::column-end:::
:::row-end:::

```xaml 
<Button FontFamily="Segoe MDL2 Assets" Content="&#xE102;" />
```

:::row:::
    :::column span="2":::
        <b>Step 2</b><br>
Vous pouvez utiliser un des objets d’élément de l’icône : [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon), [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon), [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon), ou [SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon). Cela vous donne davantage de types d’icônes sélectionnables et vous permet de combiner des icônes et autres types de contenu, telles que du texte, si vous souhaitez :
    :::column-end:::
    :::column:::
        ![Create an icon button step 2](images/icons/icon-text-step-2.svg)
    :::column-end:::
:::row-end:::

```xaml 
<Button>
    <StackPanel>
        <SymbolIcon Symbol="Play" />
        <TextBlock>Play the movie</TextBlock>
    </StackPanel>
</Button>
```

## <a name="create-a-series-of-icons-in-a-command-bar"></a>Créer une série d’icônes dans une barre de commandes

:::row:::
    :::column span:::
Lorsque vous avez une série de commandes qui vont de pair, telles que couper/copier/coller ou un ensemble de commandes à un programme de retouche photo de dessin les réunir un [barre de commandes](../controls-and-patterns/app-bars.md). Une barre de commandes contient un ou plusieurs boutons ou boutons bascule de la barre de l’application, chacun d'entre eux représentant une action. Chaque bouton a une propriété [Icône](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.appbarbutton#Windows_UI_Xaml_Controls_AppBarButton_Icon) qui vous permet de contrôler l’icône à afficher. Il existe plusieurs façons de spécifier l'icône. 
    :::column-end:::
    :::column:::
        ![Example of a command bar with icons](images/icons/create-icon-command-bar.svg)
    :::column-end:::
:::row-end:::

Le moyen le plus simple consiste à utiliser la liste des icônes prédéfinies que nous fournissons : il suffit de spécifier le nom de l’icône, par exemple, « Précédent » ou « Arrêter », et le système la dessine : 

``` xaml
<CommandBar>
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle" Click="AppBarButton_Click" />
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat" Click="AppBarButton_Click"/>
    <AppBarSeparator/>
    <AppBarButton Icon="Back" Label="Back" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Stop" Label="Stop" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Play" Label="Play" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Forward" Label="Forward" Click="AppBarButton_Click"/>
</CommandBar>

```
Pour obtenir la liste complète des noms d’icônes, voir [Énumération Symbol](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbol). 

Il existe d'autres façons de fournir des icônes pour un bouton dans une barre de commandes :

+ [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon) : l’icône est basée sur un glyphe à partir de la famille de polices spécifiée.
+ [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon) : l’icône est basée sur un fichier image bitmap avec l'**Uri** spécifié.
+ [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon) : l’icône est basée sur des données de [Path](/uwp/api/windows.ui.xaml.shapes.path).

Pour plus d’informations sur les barres de commandes, voir l'[article Barre de commandes](../controls-and-patterns/app-bars.md). 



## <a name="related-articles"></a>Articles connexes

* [Instructions pour les ressources en mosaïque et icône](../shell/tiles-and-notifications/app-assets.md)
