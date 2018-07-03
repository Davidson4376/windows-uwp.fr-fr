---
author: mijacobs
Description: Good icons harmonize with typography and with the rest of the design language. They don’t mix metaphors, and they communicate only what’s needed, as speedily and simply as possible.
title: Icônes
ms.assetid: b90ac02d-5467-4304-99bd-292d6272a014
label: Icons
template: detail.hbs
ms.author: mijacobs
ms.date: 05/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 077967c37f76c8f1d0942f365344de65db13b041
ms.sourcegitcommit: ce45a2bc5ca6794e97d188166172f58590e2e434
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/06/2018
ms.locfileid: "1983572"
---
# <a name="icons-for-uwp-apps"></a>Icônes pour les applications UWP

![image d'en-tête d'icônes](images/icons/header-icons.png)

Les icônes fournissent un raccourci visuel pour une action, un concept ou un produit. En condensant une signification dans une image symbolique, les icônes peuvent franchir les barrières des langues et préserver une ressource extrêmement utile: l'espace à l’écran. 

Les icônes peuvent apparaître dans les applications, ainsi qu'à l’extérieur: 

:::row::: :::column::: **Icônes à l’intérieur de l’application**

        ![icons inside the app](images/icons/inside-icons.png)
        Inside your app, you use icons to represent an action, such as copying text or navigating to the settings page.
    :::column-end:::
    :::column:::
        **Icons outside the app**

        ![icons outside the app](images/icons/outside-icons.jpg)
         Outside your app, Windows uses an icon to represent your app in the start menu and in the taskbar. If the user chooses to pin your app to the start menu, your app's start tile can feature your app's icon. Your app's icon appears in the title bar and you can choose to create a splash screen with your app's logo.
    :::column-end:::
:::row-end:::

Cet article décrit les icônes au sein de votre application. Pour en savoir plus sur les icônes en dehors de votre application (icônes d’application), voir l'[article Icônes d'application et de vignette](/windows/uwp/design/shell/tiles-and-notifications/app-assets).

## <a name="when-to-use-icons"></a>Quand utiliser les icônes

Les icônes permettent d'économiser de l’espace, mais quand faut-il les utiliser? 

:::row::: :::column::: ![faire](images/do.svg) ![image standard d’icônes](images/icons/icons-standard.svg)<br>

        Use an icon for actions, like cut, copy, paste, and save, or for navigation items in a navigation menu.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![icons concept image](images/icons/icons-concept.svg)<br>

        Use an icon if one already exists for the concept you want to represent. (To see whether an icon exists, check the Segoe icon list.)
    :::column-end:::
:::row-end:::

:::row::: :::column::: ![faire](images/do.svg) ![icône de panier d’achat](images/icons/icon-shopping-cart.svg)<br>

        Use an icon if it's easy for the user to understand what the icon means and it's simple enough to be clear at small sizes.
    :::column-end:::
    :::column:::
        ![dont](images/dont.svg)
        ![icons concept image](images/icons/icon-bad-example.png)<br>

        Don't use an icon if its meaning isn't clear, or if making it clear requires a complex shape.
    :::column-end:::
:::row-end:::



## <a name="using-the-right-type-of-icon"></a>Utiliser le type approprié d’icône

Il existe de nombreuses façons de créer une icône. Vous pouvez utiliser une police de symboles telle que la police Segoe MDL2 Assets. Vous pouvez créer votre propre image vectorielle. Vous pouvez même utiliser une image bitmap, bien que nous ne le conseillions pas. Voici un résumé de différentes façons d'ajouter une icône à votre application. 

### <a name="use-a-predefined-icon"></a>Utilisez une icône prédéfinie.
:::row::: :::column::: Microsoft fournit plus de 1000icônes dans la police Segoe MDL2 Assets. Il peut ne pas être intuitif d'obtenir une icône à partir d’une police, mais notre technologie d’affichage de police donne à ces icônes un aspect net et précis sur n’importe quel écran, à n'importe quelle résolution et à n’importe quelle taille. :::column-end::: :::column::: ![image d’icône prédéfinie](images/icons/predefined-icon.png) :::column-end::: :::row-end:::

### <a name="use-a-font"></a>Utilisez une police.
:::row::: :::column::: Vous n’êtes pas obligé d’utiliser la police Segoe MDL2 Assets: vous pouvez utiliser n’importe quelle police installée par l’utilisateur sur son système, telle que Wingdings ou Webdings.
:::column-end::: :::column::: ![image wingdings](images/icons/wingdings.png) :::column-end::: :::row-end:::

### <a name="use-a-scalable-vector-graphics-svg-file"></a>Utilisez un fichier SVG (Scalable Vector Graphics).
:::row::: :::column::: Les ressources SVG sont idéales pour les icônes, car elles paraissent toujours nettes à n’importe quelle taille ou résolution. La plupart des applications de dessin peuvent s'exporter au format SVG. :::column-end::: :::column::: ![image SVG](images/icons/icon-scale.gif) :::column-end::: :::row-end:::

### <a name="use-geometry-objects"></a>Utilisez des objets de géométrie.
:::row::: :::column::: Comme les fichiers SVG, les géométries sont une ressource vectorielle; elles paraissent toujours nettes. Toutefois, la création d’une géométrie est complexe, car vous devez spécifier individuellement chaque point et chaque courbe. En réalité, ce n'est un bon choix que si vous devez modifier l’icône pendant que votre application est en cours d’exécution (pour l’animer, par exemple). Pour obtenir des instructions, voir [Commandes de déplacement et de dessin pour les géométries](../../xaml-platform/move-draw-commands-syntax.md). :::column-end::: :::column::: ![Image d’objets de géométrie](images/icons/geometry-objects.png) :::column-end::: :::row-end:::

### <a name="you-can-also-use-a-bitmap-image-such-as-png-gif-or-jpeg-although-we-dont-recommend-it"></a>Vous pouvez également utiliser une image bitmap, par exemple une image PNG, GIF ou JPEG, bien que nous ne le conseillions pas.
:::row::: :::column::: Les images bitmap sont créées dans une taille spécifique, donc elles doivent être mises à l’échelle pour les agrandir ou les réduire en fonction de la taille d'icône souhaitée et de la résolution de l’écran. Lorsque l’image est réduite, elle peut paraître floue; lorsqu’elle est agrandie, elle peut paraître pixelisée. Si vous devez utiliser une image bitmap, nous vous recommandons d’utiliser le format PNG ou GIF plutôt qu'une image JPEG. :::column-end::: :::column::: ![ne pas faire](images/dont.svg) ![Image bitmap](images/icons/bitmap-image.png) :::column-end::: :::row-end:::

## <a name="make-the-icon-do-something"></a>Faites faire une action à l’icône

Une fois que vous vous disposez d’une icône, l’étape suivante consiste à lui faire faire quelque chose en l’associant à une commande ou à une action de navigation. La meilleure façon de procéder consiste à ajouter l’icône à un bouton ou à une barre de commandes. 

![Image d'une barre de commandes](images/icons/app-bar-desktop.svg)

## <a name="create-an-icon-button"></a>Créer un bouton d’icône

Vous pouvez placer une icône dans un bouton standard. Dans la mesure où vous pouvez utiliser les boutons dans une plus grande variété de lieux, cela vous donne un peu plus de souplesse pour l'emplacement d'affichage de votre icône d’action. 

Il existe plusieurs méthodes pour ajouter une icône à un bouton:

:::row::: :::column span="2"::: <b>Étape1</b><br>
        Définissez la famille de polices du bouton sur la valeur `Segoe MDL2 Assets` et sa propriété de contenu sur la valeur unicode du glyphe à utiliser: :::column-end::: :::column::: ![Créer un bouton d'icône - Étape1](images/icons/create-icon-step-1.svg) :::column-end::: :::row-end:::

```xaml 
<Button FontFamily="Segoe MDL2 Assets" Content="&#xE102;" />
```

:::row::: :::column span="2"::: <b>Étape2</b><br>
        Vous pouvez utiliser l'un des objets d’élément de l’icône: [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon), [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon), [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon) ou [SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon). Cela vous donne davantage de choix de types d'icônes et vous permet de combiner les icônes et d'autres types de contenu, tels que le texte, si vous le souhaitez: :::column-end::: :::column::: ![Créer un bouton d'icône - Étape2](images/icons/icon-text-step-2.svg) :::column-end::: :::row-end:::

```xaml 
<Button>
    <StackPanel>
        <SymbolIcon Symbol="Play" />
        <TextBlock>Play the movie</TextBlock>
    </StackPanel>
</Button>
```

## <a name="create-a-series-of-icons-in-a-command-bar"></a>Créer une série d’icônes dans une barre de commandes

:::row::: :::column span::: Lorsque vous avez une série de commandes qui vont ensemble, telles que couper/copier/coller ou un ensemble de commandes de dessin pour un programme de retouche photo, placez-les ensemble dans une [barre de commandes](../controls-and-patterns/app-bars.md). Une barre de commandes contient un ou plusieurs boutons ou boutons bascule de la barre de l’application, chacun d'entre eux représentant une action. Chaque bouton a une propriété [Icône](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.appbarbutton#Windows_UI_Xaml_Controls_AppBarButton_Icon) qui vous permet de contrôler l’icône à afficher. Il existe plusieurs façons de spécifier l'icône. :::column-end::: :::column::: ![Exemple d’une barre de commandes avec des icônes](images/icons/create-icon-command-bar.svg) :::column-end::: :::row-end:::

Le moyen le plus simple consiste à utiliser la liste des icônes prédéfinies que nous fournissons: il suffit de spécifier le nom de l’icône, par exemple, «Précédent» ou «Arrêter», et le système la dessine: 

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

Il existe d'autres façons de fournir des icônes pour un bouton dans une barre de commandes:

+ [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon): l’icône est basée sur un glyphe à partir de la famille de polices spécifiée.
+ [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon): l’icône est basée sur un fichier image bitmap avec l'**Uri** spécifié.
+ [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon): l’icône est basée sur des données de [Path](/uwp/api/windows.ui.xaml.shapes.path).

Pour plus d’informations sur les barres de commandes, voir l'[article Barre de commandes](../controls-and-patterns/app-bars.md). 



## <a name="related-articles"></a>Articles associés

* [Recommandations en matière de ressources de vignette et d’icône](../shell/tiles-and-notifications/app-assets.md)
