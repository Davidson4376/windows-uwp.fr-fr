---
Description: Good icons harmonize with typography and with the rest of the design language. They don’t mix metaphors, and they communicate only what’s needed, as speedily and simply as possible.
title: Icônes
ms.assetid: b90ac02d-5467-4304-99bd-292d6272a014
label: Icons
template: detail.hbs
ms.date: 05/02/2018
ms.topic: article
keywords: windows10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: fb97f69ccdffcec86dfdaf5fa6c5f817644ebd61
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8947572"
---
# <a name="icons-for-uwp-apps"></a>Icônes pour les applications UWP

![image d'en-tête d'icônes](images/icons/header-icons.png)

Les icônes fournissent un raccourci visuel pour une action, un concept ou un produit. En condensant une signification dans une image symbolique, les icônes peuvent franchir les barrières des langues et préserver une ressource extrêmement utile: l'espace à l’écran. 

Les icônes peuvent apparaître dans les applications, ainsi qu'à l’extérieur: 

:::row:::
    :::column:::
        **Icons inside the app**

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

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icons standard image](images/icons/icons-standard.svg)<br>

        Use an icon for actions, like cut, copy, paste, and save, or for navigation items in a navigation menu.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![icons concept image](images/icons/icons-concept.svg)<br>

        Use an icon if one already exists for the concept you want to represent. (To see whether an icon exists, check the Segoe icon list.)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icon shopping cart](images/icons/icon-shopping-cart.svg)<br>

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
:::row:::
    :::column:::
        Microsoft provides over 1000 icons in the form of the Segoe MDL2 Assets font. It might not be intuitive to get an icon from a font, but our font display technology means these icons will look crisp and sharp on any display, at any resolution, and at any size. 
    :::column-end:::
    :::column:::
        ![pre-defined icon image](images/icons/predefined-icon.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-font"></a>Utilisez une police.
:::row:::
    :::column:::
        You don't have to use the Segoe MDL2 Assets font--you can use any font the user has installed on their system, such as Wingdings or Webdings.
    :::column-end:::
    :::column:::
        ![wingdings image](images/icons/wingdings.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-scalable-vector-graphics-svg-file"></a>Utilisez un fichier SVG (Scalable Vector Graphics).
:::row:::
    :::column:::
        SVG resources are ideal for icons, because they always look sharp at any size or resolution. Most drawing applications can export to SVG. 
    :::column-end:::
    :::column:::
        ![SVG image](images/icons/icon-scale.gif)
    :::column-end:::
:::row-end:::

### <a name="use-geometry-objects"></a>Utilisez des objets de géométrie.
:::row:::
    :::column:::
        Like SVG files, geometries are a vector-based resource, so they always look sharp. However, creating a geometry is complicated because you have to individually specify each point and curve. It's really only a good choice if you need to modify the icon while your app is running (to animate it, for example). For instructions, see [Move and draw commands for geometries](../../xaml-platform/move-draw-commands-syntax.md). 
    :::column-end:::
    :::column:::
        ![Geometry objects image](images/icons/geometry-objects.png)
    :::column-end:::
:::row-end:::

### <a name="you-can-also-use-a-bitmap-image-such-as-png-gif-or-jpeg-although-we-dont-recommend-it"></a>Vous pouvez également utiliser une image bitmap, par exemple une image PNG, GIF ou JPEG, bien que nous ne le conseillions pas.
:::row:::
    :::column:::
        Bitmap images are created at a specific size, so they have to be scaled up or down depending on how large you want the icon to be and the resolution of the screen. When the image is scaled down (shrunk), it can appear blurry; when it's scaled up, it can appear blocky and pixelated. If you have to use a bitmap image we recommend using a PNG or GIF over a JPEG. 
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![Bitmap image](images/icons/bitmap-image.png)
    :::column-end:::
:::row-end:::

## <a name="make-the-icon-do-something"></a>Faites faire une action à l’icône

Une fois que vous avez une icône, l’étape suivante consiste à lui faire faire quelque chose en l’associant à une commande ou une action de navigation. La meilleure façon de procéder consiste à ajouter l’icône à un bouton ou une barre de commandes. 

![Image d'une barre de commandes](images/icons/app-bar-desktop.svg)

## <a name="create-an-icon-button"></a>Créer un bouton d’icône

Vous pouvez placer une icône dans un bouton standard. Dans la mesure où vous pouvez utiliser les boutons dans une plus grande variété de lieux, cela vous donne un peu plus de souplesse pour l'emplacement d'affichage de votre icône d’action. 

Il existe plusieurs méthodes pour ajouter une icône à un bouton:

:::row:::
    :::column span="2":::
        <b>Step 1</b><br>
        Set the button's font family to `Segoe MDL2 Assets` and its content property to the unicode value of the glyph you want to use:
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
        You can use one of the icon element objects: [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon),
        [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon), 
        [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon), or
        [SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon). This gives you more types of icons to choose from, and enables you to combine icons and other types of content, such as text, if you want:
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
        When you have a series of commands that go together, such as cut/copy/paste or a set of drawing commands for a photo-editing program, put them together in a [command bar](../controls-and-patterns/app-bars.md). A command bar takes one or more app bar buttons or app bar toggle buttons, each of which represents an action. Each button has an [Icon](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.appbarbutton#Windows_UI_Xaml_Controls_AppBarButton_Icon) property you use to control which icon it displays. There are a variety of ways to specify the icon. 
    :::column-end:::
    :::column:::
        ![Example of a command bar with icons](images/icons/create-icon-command-bar.svg)
    :::column-end:::
:::row-end:::

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
