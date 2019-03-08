---
Description: Générer des applications UWP et des contrôles personnalisés/basé sur un modèle qui prennent en charge la mise à l’échelle du texte de plateforme.
title: Mise à l’échelle du texte
label: Text scaling
template: detail.hbs
keywords: UWP, texte, la mise à l’échelle, l’accessibilité, la « options d’ergonomie », afficher, « Ajout de texte plus grand », l’interaction utilisateur, entrée
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 22ad7a1ac6160fd8b1cfb70c69f299c5d89192d3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600814"
---
# <a name="text-scaling"></a>Mise à l’échelle du texte

![Exemple de texte mise à l’échelle de 100 à 225 %](images/coretext/text-scaling-news-hero-small.png)  
*Exemple de texte mise à l’échelle dans Windows 10 (de 100 à 225 %)*

## <a name="overview"></a>Vue d’ensemble

Lire le texte sur un écran d’ordinateur (à partir de l’appareil mobile à l’ordinateur portable pour le Moniteur du Bureau à l’écran géant d’un appareil Surface Hub) peut être difficile pour de nombreuses personnes. En revanche, certains utilisateurs trouvent les tailles de police utilisés dans les applications et sites web pour être plus volumineux que nécessaire.

Pour garantir le texte est lisible que possible pour un large éventail d’utilisateurs, Windows offre la possibilité pour les utilisateurs à modifier la taille de police relative sur le système d’exploitation et les applications individuelles. Au lieu d’à l’aide d’une application de la Loupe (qui en général simplement tous les éléments dans une zone de l’écran agrandit et présente ses propres problèmes de facilité d’utilisation), la modification de résolution d’affichage ou la partie de confiance sur l’échelle en PPP (ce qui redimensionne tous les éléments en fonction de l’affichage et l’affichage classique distance), un utilisateur peut accéder rapidement un paramètre pour redimensionner uniquement le texte, allant de 100 % (la taille par défaut) jusqu'à 225 %.

## <a name="support"></a>Support

Les applications Windows universelles (standard et PWA), prennent en charge la mise à l’échelle par défaut de texte.

Si votre application UWP inclut des contrôles personnalisés, les surfaces de texte personnalisé, les hauteurs de contrôle codées en dur, frameworks plus anciens ou 3e frameworks tiers, vous devez probablement apporter certaines mises à jour pour garantir une expérience cohérente et utile pour vos utilisateurs.  

DirectWrite, GDI et XAML SwapChainPanels ne pas en mode natif prennent en charge mise à l’échelle du texte, tandis que la prise en charge Win32 est limité aux menus, les icônes et les barres d’outils.  

<!-- If you want to support text scaling in your application with these frameworks, you’ll need to support the text scaling change event outlined below and provide alternative sizes for your UI and content.   -->

## <a name="user-experience"></a>Expérience de l'utilisateur

Les utilisateurs peuvent ajuster l’échelle du texte avec l’ajout de texte plus grand curseur sur les paramètres -> facilité d’accès -> Vision/afficher l’écran.

![Exemple de texte mise à l’échelle de 100 à 225 %](images/coretext/text-scaling-settings-100-small.png)  
*Valeur à partir des paramètres d’échelle de texte -> facilité d’accès -> Vision/afficher l’écran*

## <a name="ux-guidance"></a>Recommandations en matière d’expérience utilisateur

Comme il est redimensionné, contrôles et des conteneurs doivent également redimensionner et de reformater pour prendre en charge le texte et sa nouvelle disposition. Comme mentionné précédemment, en fonction de l’application, le framework et la plateforme, cette opération est effectuée pour vous. Le Guide de l’expérience utilisateur suivant couvre les cas où il n’est pas.

### <a name="use-the-platform-controls"></a>Utilisez les contrôles de plateforme

Nous dire ce déjà ? Il est inutile de le répéter : Dans la mesure du possible, vous devez toujours utiliser les contrôles intégrés fournis avec les différentes infrastructures d’application Windows pour obtenir l’expérience utilisateur plus complète possible pour le moins d’effort.

Par exemple, tous les contrôles de texte UWP prend en charge la mise à l’échelle de l’expérience sans toute personnalisation ou la création de modèles de texte intégral.

Voici un extrait à partir d’une application UWP qui inclut deux contrôles de texte standard :

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

![Texte animé mise à l’échelle de 100 à 225 %](images/coretext/text-scaling.gif)  
*Mise à l’échelle du texte animé*

### <a name="use-auto-sizing"></a>Utilisez le dimensionnement automatique

Ne spécifiez pas les tailles absolues pour vos contrôles. Si possible, permettre à la plateforme de redimensionner vos contrôles automatiquement en fonction des paramètres utilisateur et périphérique.  

Dans cet extrait de code à partir de l’exemple précédent, nous utilisons le `Auto` et `*` les valeurs de la largeur pour un ensemble de colonnes de la grille et de laisser la plateforme ajuster la disposition de l’application basée sur la taille des éléments contenus dans la grille.

``` xaml
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto"/>
    <ColumnDefinition Width="*"/>
    <ColumnDefinition Width="Auto"/>
</Grid.ColumnDefinitions>
```

### <a name="use-text-wrapping"></a>Utiliser l’habillage du texte

Pour garantir que la mise en page de votre application est aussi flexibles et adaptables que possible, activez l’habillage du texte dans n’importe quel contrôle qui contient du texte (de nombreux contrôles ne gèrent pas habillage du texte par défaut).

Si vous ne spécifiez pas habillage du texte, la plateforme utilise d’autres méthodes pour ajuster la disposition, y compris de découpage (voir exemple précédent).

Ici, nous utilisons le `AcceptsReturn` et `TextWrapping` des propriétés de zone de texte pour assurer notre disposition est plus flexible possible.

``` xaml
<TextBox PlaceholderText="Type something here" 
          AcceptsReturn="True" TextWrapping="Wrap" />
```

![Texte de mise à l’échelle de 100 à 225 % avec un habillage du texte animé](images/coretext/text-scaling-textwrap.gif)  
*Texte animé mise à l’échelle avec un habillage du texte*

### <a name="specify-text-trimming-behavior"></a>Spécifier le comportement de découpage de texte

Si l’habillage du texte n’est pas le comportement par défaut, la plupart des contrôles de texte vous permettent de spécifier des points de suspension pour la conduite de rognage de texte ou image de votre texte Découpage est recommandée pour les points de suspension comme points de suspension prennent place eux-mêmes.

> [!NOTE]
> Si vous avez besoin découper votre texte, découper la fin de la chaîne, et non au début.

Dans cet exemple, nous montrons comment découper le texte dans un TextBlock à l’aide de la [TextTrimming](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.texttrimming) propriété.

``` xaml
<TextBlock TextTrimming="Clip">
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

![Texte de mise à l’échelle de 100 à 225 % avec un découpage de texte](images/coretext/text-scaling-clipping-small.png)  
*Texte de mise à l’échelle avec un découpage de texte*

### <a name="use-a-tooltip"></a>Utiliser une info-bulle

Si l’image de texte, utilisez une info-bulle pour fournir la recherche en texte intégral à vos utilisateurs.

Ici, nous ajoutons une info-bulle à un bloc de texte qui ne prennent pas en charge d’habillage du texte :

``` xaml
<TextBlock TextTrimming="Clip">
    <ToolTipService.ToolTip>
        <ToolTip Content="The quick brown fox jumped over the lazy dog."/>
    </ToolTipService.ToolTip>
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

### <a name="dont-scale-font-based-icons-or-symbols"></a>Ne pas mettre à l’échelle basée sur la police des icônes ou des symboles

Lorsque vous utilisez les icônes basée sur la police pour l’importance ou de la décoration, désactivez la mise à l’échelle sur ces caractères.

Définir le [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled) propriété `false` pour XAML de la plupart des contrôles.

### <a name="support-text-scaling-natively"></a>Texte de prise en charge en mode natif la mise à l’échelle

Gérer le [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) UISettings d’événement système dans votre framework personnalisé et les contrôles. Cet événement est déclenché chaque fois que l’utilisateur définit le facteur d’échelle de texte sur leur système.

## <a name="summary"></a>Résumé

Cette rubrique fournit une vue d’ensemble du texte de mise à l’échelle prise en charge dans Windows et inclut des instructions de l’expérience utilisateur et développeur sur la façon de personnaliser l’expérience utilisateur.

## <a name="related-articles"></a>Articles connexes

### <a name="api-reference"></a>Informations de référence sur les API

- [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)
- [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged)
