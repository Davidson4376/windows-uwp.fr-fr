---
author: Karl-Bridge-Microsoft
Description: Build UWP apps and custom/templated controls that support platform text scaling.
title: Mise à l’échelle du texte
label: Text scaling
template: detail.hbs
keywords: UWP, texte, la mise à l’échelle, l’accessibilité, la «options d’ergonomie», afficher, «Ajout de texte plus large», l’interaction utilisateur, entrées
ms.author: kbridge
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3110615e43ffa4839d9be6ebf5113fc9d8b7c05f
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7571431"
---
# <a name="text-scaling"></a>Mise à l’échelle du texte

![Exemple de texte mise à l’échelle 100 % à 225 %](images/coretext/text-scaling-news-hero-small.png)  
*Exemple de texte mise à l’échelle dans Windows 10 (100 % à 225 %)*

## <a name="overview"></a>Vue d'ensemble

Lecture de texte sur un écran d’ordinateur (à partir d’un appareil mobile à un ordinateur portable à un moniteur de bureau à l’écran giant de Surface Hub) peut être difficile pour de nombreuses personnes. À l’inverse, certains utilisateurs de trouver les tailles de police utilisées dans les applications et sites web à être plus grande que nécessaire.

Pour vous assurer que le texte est aussi lisible que possible pour un large éventail d’utilisateurs, Windows offre la possibilité pour les utilisateurs de modifier la taille de police relative sur le système d’exploitation et les applications individuelles. Au lieu d’à l’aide d’une application de la Loupe (qui généralement simplement agrandit tous les éléments dans une zone de l’écran et présente ses propres problèmes de facilité d’utilisation), la modification de la résolution d’affichage ou la partie de confiance sur l’échelle en PPP (qui redimensionne tous les éléments en fonction de l’affichage et de l’affichage type distance), un utilisateur peut accéder rapidement à un paramètre de redimensionner uniquement du texte, cela peut aller de 100 % (la taille par défaut) jusqu'à 225 %.

## <a name="support"></a>Support

Les applications Windows universelles (standard et PWA), prend en charge de texte mise à l’échelle par défaut.

Si votre application UWP inclut des contrôles personnalisés, des surfaces de texte personnalisé, hauteurs de contrôle codées en dur, des infrastructures plus anciens ou 3e infrastructures tierces, vous avez probablement apporter certaines mises à jour pour garantir une expérience cohérente et utile pour vos utilisateurs.  

DirectWrite, GDI et XAML SwapChainPanels ne pas prennent en charge mise à l’échelle du texte, tandis que la prise en charge Win32 est limitée aux menus, des icônes et des barres d’outils.  

<!-- If you want to support text scaling in your application with these frameworks, you’ll need to support the text scaling change event outlined below and provide alternative sizes for your UI and content.   -->

## <a name="user-experience"></a>Expérience utilisateur

Les utilisateurs peuvent ajuster à l’échelle du texte avec l’ajout de texte plus grand curseur sur les paramètres -> d’ergonomie -> Vision/afficher l’écran.

![Exemple de texte mise à l’échelle 100 % à 225 %](images/coretext/text-scaling-settings-100-small.png)  
*Valeur à partir des paramètres d’échelle de texte -> d’ergonomie -> Vision/afficher l’écran*

## <a name="ux-guidance"></a>Recommandations en matière d’expérience utilisateur

Comme le texte est redimensionné, contrôles et conteneurs doivent également redimensionner et ajuster dynamiquement pour prendre en charge le texte et sa nouvelle disposition. Comme mentionné précédemment, en fonction de l’application, une infrastructure et une plateforme, une grande partie de ce travail est faite pour vous. Les recommandations d’expérience utilisateur suivante décrit les cas où il n’est pas.

### <a name="use-the-platform-controls"></a>Utilisez les contrôles de plateforme

Nous nous avons entendu dire cela déjà? Il est important de répétition: lorsque cela est possible, toujours utiliser les contrôles intégrés fournis avec les différentes infrastructures d’application Windows pour obtenir l’expérience utilisateur la plus exhaustive possible pour l’effort minimal.

Par exemple, tous les contrôles de texte UWP prennent en charge le texte complet expérience sans aucune personnalisation ou la création de modèles de mise à l’échelle.

Voici un extrait de code à partir d’une application UWP de base qui inclut plusieurs contrôles de texte standard:

``` xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    <TextBlock Grid.Row="0" 
                Style="{ThemeResource TitleTextBlockStyle}"
                Text="Text scaling test" 
                HorizontalTextAlignment="Center" />
    <Grid Grid.Row="1">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto"/>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="Auto"/>
        </Grid.ColumnDefinitions>
        <Image Grid.Column="0" 
                Source="Assets/StoreLogo.png" 
                Width="450" Height="450"/>
        <StackPanel Grid.Column="1" 
                    HorizontalAlignment="Center">
            <TextBlock TextWrapping="WrapWholeWords">
                The quick brown fox jumped over the lazy dog.
            </TextBlock>
            <TextBox PlaceholderText="Type something here" />
        </StackPanel>
        <Image Grid.Column="2" 
                Source="Assets/StoreLogo.png" 
                Width="450" Height="450"/>
    </Grid>
    <TextBlock Grid.Row="2" 
                Style="{ThemeResource TitleTextBlockStyle}"
                Text="Text scaling test footer" 
                HorizontalTextAlignment="Center" />
</Grid>
```

![Mise à l’échelle 100 % à 225 % de texte animé](images/coretext/text-scaling.gif)  
*Mise à l’échelle du texte animé*

### <a name="use-auto-sizing"></a>Utiliser le dimensionnement automatique

Ne spécifiez pas tailles absolues pour vos contrôles. Si possible, permettre à la plateforme de redimensionner vos contrôles automatiquement en fonction des paramètres utilisateur et d’appareil.  

Dans cet extrait de code de l’exemple précédent, nous utilisons la `Auto` et `*` les valeurs de largeur pour un ensemble de colonnes de la grille et permettent de la plateforme ajustent la disposition de l’application en fonction de la taille des éléments contenus dans la grille.

``` xaml
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto"/>
    <ColumnDefinition Width="*"/>
    <ColumnDefinition Width="Auto"/>
</Grid.ColumnDefinitions>
```

### <a name="use-text-wrapping"></a>Utilisez un saut de ligne

Pour vous assurer que la disposition de votre application est aussi flexible et adaptable que possible, activer l’habillage de texte dans n’importe quel contrôle contenant du texte (de nombreux contrôles ne pas prennent en charge un saut de ligne par défaut).

Si vous ne spécifiez pas un saut de ligne, la plateforme utilise les autres méthodes pour ajuster la disposition, notamment le découpage (voir l’exemple précédent).

Ici, nous utilisons la `AcceptsReturn` et `TextWrapping` des propriétés de zone de texte pour assurer notre disposition est plus souple possible.

``` xaml
<TextBox PlaceholderText="Type something here" 
          AcceptsReturn="True" TextWrapping="Wrap" />
```

![Mise à l’échelle de 100 % à 225 % avec habillage de texte animé](images/coretext/text-scaling-textwrap.gif)  
*Mise à l’échelle avec habillage de texte animé*

### <a name="specify-text-trimming-behavior"></a>Spécifier le comportement de découpage de texte

Si l’habillage de texte n’est pas le comportement par défaut, la plupart des contrôles de texte permettent de spécifier des points de suspension pour le comportement de découpage de texte ou images votre texte. Découpage est préférable pour les points de suspension ellipses occupent l’espace eux-mêmes.

> [!NOTE]
> Si vous avez besoin découper votre texte, la fin de la chaîne, et non au début de l’élément.

Dans cet exemple, nous montrons comment découper le texte dans un contrôle TextBlock à l’aide de la propriété [TextTrimming](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.texttrimming) .

``` xaml
<TextBlock TextTrimming="Clip">
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

![Texte mise à l’échelle 100 % à 225 % avec un découpage de texte](images/coretext/text-scaling-clipping-small.png)  
*Texte mise à l’échelle avec un découpage de texte*

### <a name="use-a-tooltip"></a>Utilisez une info-bulle

Si vous déroutez le texte, utilisez une info-bulle pour fournir le texte complète à vos utilisateurs.

Ici, nous ajoutons une info-bulle à un contrôle TextBlock qui ne prennent pas en charge d’habillage du texte:

``` xaml
<TextBlock TextTrimming="Clip">
    <ToolTipService.ToolTip>
        <ToolTip Content="The quick brown fox jumped over the lazy dog."/>
    </ToolTipService.ToolTip>
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

### <a name="dont-scale-font-based-icons-or-symbols"></a>Ne sont pas évolutifs icônes basées sur la police ou les symboles

Lorsque vous utilisez des icônes basées sur une police pour mettre en valeur ou ornement, désactivez la mise à l’échelle sur ces caractères.

Attribuez à la propriété [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled) `false` pour XAML de la plupart des contrôles.

### <a name="support-text-scaling-natively"></a>Texte de prise en charge en mode natif de mise à l’échelle

Gérez l’événement de système de UISettings [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) dans votre infrastructure personnalisé et les contrôles. Cet événement est déclenché chaque fois que l’utilisateur définit le facteur d’échelle de texte sur son système.

## <a name="summary"></a>Résumé

Cette rubrique fournit une vue d’ensemble du texte mise à l’échelle de prise en charge dans Windows et inclut des développeurs et expérience utilisateur des conseils sur la façon de personnaliser l’expérience utilisateur.

## <a name="related-articles"></a>Articles connexes

### <a name="api-reference"></a>Informations de référence sur les API

- [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)
- [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged)
