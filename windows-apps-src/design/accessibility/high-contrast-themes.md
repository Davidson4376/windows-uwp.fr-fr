---
author: Xansky
description: Décrit les étapes requises pour garantir que votre application UWP est utilisable lorsqu’un thème à contraste élevé est actif.
ms.assetid: FD7CA6F6-A8F1-47D8-AA6C-3F2EC3168C45
title: Thèmes à contraste élevé
template: detail.hbs
ms.author: mhopkins
ms.date: 09/28/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7cf8b634cfc7ba66cde107150b54ecec76b2861d
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5837051"
---
# <a name="high-contrast-themes"></a>Thèmes à contraste élevé  

Windows prend en charge les thèmes à contraste élevé pour les systèmes d’exploitation et les applications que les utilisateurs peuvent être amenés à activer. Les thèmes à contraste élevé utilisent une petite palette de couleurs contrastées qui facilitent l’affichage de l’interface.

![Calculatrice représentée en thème clair et en thème noir à contraste élevé](images/high-contrast-calculators.png)

*Calculatrice représentée en thème clair et en thème noir à contraste élevé*

Pour basculer sur un thème à contraste élevé, accédez à *Paramètres &gt; Options d’ergonomie &gt; Contraste élevé*.

> [!NOTE]
> Ne confondez pas les thèmes à contraste élevé avec les thèmes Ombre et lumière, qui prennent en charge une palette de couleurs plus large, sans contraste élevé. Pour plus d’informations sur les thèmes Ombre et lumière, consultez l’article sur la [couleur](../style/color.md).

Les contrôles communs sont pourvus d’une prise en charge gratuite complète du contraste élevé, mais nous vous recommandons de faire preuve de prudence lors de la personnalisation de votre interface utilisateur. Le bogue le plus courant lié au contraste élevé est provoqué par le codage en dur d’une couleur sur un contrôle inclus.

```xaml
<!-- Don't do this! -->
<Grid Background="#E6E6E6">

<!-- Instead, create BrandedPageBackgroundBrush and do this. -->
<Grid Background="{ThemeResource BrandedPageBackgroundBrush}">
```

Lorsque la couleur `#E6E6E6` est incluse dans le premier exemple, la Grille conserve la couleur d’arrière-plan dans l’ensemble des thèmes. Si l’utilisateur bascule sur le thème noir à contraste élevé, il souhaitera disposer d’un arrière-plan noir sur votre application. Dans la mesure où `#E6E6E6` est presque blanc, certains utilisateurs peuvent ne pas pouvoir interagir avec votre application.

Dans le second exemple, l’[**extension de balisage {ThemeResource}**](../../xaml-platform/themeresource-markup-extension.md) est utilisée pour référencer une couleur dans la collection [**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.resourcedictionary.themedictionaries.aspx), une propriété dédiée d’un élément [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794). **ThemeDictionaries** permet au code XAML de modifier automatiquement les couleurs en fonction du thème courant de l’utilisateur.

## <a name="theme-dictionaries"></a>Dictionnaires de thème

Lorsque vous devez modifier la couleur par défaut du système, créez une collection ThemeDictionaries pour votre application.

1. Commencez par créer le système approprié, le cas échéant. Dans App.xaml, créez une collection **ThemeDictionaries**, en incluant **Default** et **HighContrast** à une valeur minimum.
2. Dans **Default**, créez le type de [Brush](http://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.brush.aspx) dont vous avez besoin, généralement **SolidColorBrush**. Attribuez-lui un nom *x: Key* spécifiant son utilisation.
3. Attribuez la **couleur** souhaitée.
4. Copiez cette classe **Brush** dans la propriété **HighContrast**.

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called
            out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- Optional, Light is used in light theme.
            If included, Default will be used for Dark theme -->
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- HighContrast is used in all high contrast themes -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

La dernière étape, décrite dans la section suivante, consiste à identifier la couleur à utiliser dans le contraste élevé.

> [!NOTE]
> **HighContrast** n’est pas le seul nom de clé disponible. Vous pouvez aussi utiliser **HighContrastBlack**, **HighContrastWhite** et **HighContrastCustom**. Dans la plupart des cas, vous avez seulement besoin de **HighContrast**.

## <a name="high-contrast-colors"></a>Couleurs à contraste élevé

La page *Paramètres > Options d’ergonomie > Contraste élevé* comporte 4thèmes à contraste élevé par défaut. 


![Paramètres de contraste élevé](images/high-contrast-settings.png)  

*Lorsque l’utilisateur sélectionne une option, la page affiche un aperçu.*  

![Ressources de contraste élevé](images/high-contrast-resources.png)  

*Pour changer la valeur des échantillons de couleur de l’aperçu, cliquez dessus. Par ailleurs, chaque échantillon est mappé directement sur une ressource de couleur XAML.*  

Chaque ressource **SystemColor*Color** est une variable qui met automatiquement à jour la couleur lorsque l’utilisateur bascule entre les thèmes à contraste élevé. Vous trouverez ci-après des recommandations vous indiquant quand et où utiliser chaque ressource.

Ressource | Utilisation |
|--------|-------|
**SystemColorWindowTextColor** | Copie de texte de corps, titres, listes; tout texte ne pouvant faire l’objet d’aucune interaction |
| **SystemColorHotlightColor** | Liens hypertexte |
| **SystemColorGrayTextColor** | Interface utilisateur désactivée |
| **SystemColorHighlightTextColor** | Couleur de premier plan pour le texte ou l’interface utilisateur activés, sélectionnés ou faisant l’objet d’une interaction |
| **SystemColorHighlightColor** | Couleur d’arrière-plan pour le texte ou l’interface utilisateur activés, sélectionnés ou faisant l’objet d’une interaction |
| **SystemColorButtonTextColor** | Couleur de premier plan des boutons; toute interface utilisateur ne pouvant faire l’objet d’aucune interaction |
| **SystemColorButtonFaceColor** | Couleur d’arrière-plan des boutons; toute interface utilisateur ne pouvant faire l’objet d’aucune interaction |
| **SystemColorWindowColor** | Arrière-plan des pages, des volets, des fenêtres contextuelles et des barres |

Il est souvent utile d’examiner les applications, menus Démarrer ou contrôles communs existants afin de savoir comment d’autres utilisateurs ont résolu des problèmes de conception de contraste élevé similaire au vôtre.

**Pratiques conseillées**

* Respectez les paires premier plan/arrière-plan autant que possible.
* Testez l’ensemble des 4thèmes à contraste élevé pendant l’exécution de votre application. Lorsqu’il change de thème, l’utilisateur ne devrait pas avoir à redémarrer votre application.
* Soyez cohérent.

**Pratiques déconseillées**

* Codez en dur une couleur dans le thème **HighContrast**; utilisez les ressources **SystemColor*Color**.
* Choisissez une ressource de couleur à des fins esthétiques. N’oubliez pas que la couleur change avec le thème!
* N’utilisez pas **SystemColorGrayTextColor** pour la copie du corps de texte secondaire ou agissant comme indication.


Pour poursuivre l’exemple précédent, vous devez sélectionner une ressource pour **BrandedPageBackgroundBrush**. Son nom indiquant qu’elle sera utilisée en arrière-plan, **SystemColorWindowColor** est un choix pertinent.

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called
            out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- Optional, Light is used in light theme.
            If included, Default will be used for Dark theme -->
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- HighContrast is used in all high contrast themes -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="{ThemeResource SystemColorWindowColor}" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

Plus loin dans votre application,vous pouvez désormais définir l’arrière-plan.

```xaml
<Grid Background="{ThemeResource BrandedPageBackgroundBrush}">
```

Notez comment **\{ThemeResource\}** est utilisée deux fois, une pour référencer **SystemColorWindowColor** et une autre pour référencer **BrandedPageBackgroundBrush **. Les deuxinstances sont nécessaires pour la bonne définition du thème lors de l’exécution. Il s’agit d’un bon moment pour tester la fonctionnalité dans votre application. L’arrière-plan de la grille est automatiquement mis à jour lorsque vous définissez un thème à contraste élevé. Il est également mis à jour lorsque vous basculez entre deuxthèmes à contraste élevé différents.

## <a name="when-to-use-borders"></a>Quand utiliser les bordures

L’ensemble des pages, des volets, des fenêtres contextuelles et des barres doivent utiliser **SystemColorWindowColor** pour leur arrière-plan en contraste élevé. Si nécessaire, ajoutez une bordure à contraste élevé uniquement afin de préserver les bordures importantes au sein de votre interface utilisateur.

![Volet de navigation séparé du reste de la page](images/high-contrast-actions-content.png)  

*Le volet de navigation et la page partagent la même couleur d’arrière-plan en contraste élevé. Il est nécessaire de les séparer avec une bordure en contraste élevé uniquement.*


## <a name="list-items"></a>Éléments de liste

En mode de contraste élevé, les éléments de [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) présentent un arrière-plan défini sur **SystemColorHighlightColor** lorsqu’ils sont survolés, activés ou sélectionnés. Les éléments de liste complexes présentent régulièrement un bogue, durant lequel la couleur du contenu de l’élément de liste ne peut pas être inversée lorsque ce dernier est survolé, activé ou sélectionné. Cela rend impossible sa lecture.

![Liste simple en thème clair et en thème noir à contraste élevé](images/high-contrast-list1.png)

*Liste simple en thème clair (à gauche) et en thème noir à contraste élevé (à droite). Le deuxième élément est sélectionné; notez comment la couleur du texte est inversée en contraste élevé.*


### <a name="list-items-with-colored-text"></a>Éléments de liste avec du texte coloré

Un coupable définit TextBlock.Foreground dans l’instance [DataTemplate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx).de ListView. Cette procédure est généralement effectuée dans le cadre de l’établissement de la hiérarchie visuelle. La propriété Foreground est définie sur [ListViewItem](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewitem.aspx), tandis que TextBlocks dans DataTemplate hérite de la couleur appropriée d’arrière-plan lorsque l’élément est survolé, activé ou sélectionné. Toutefois, la définition de Foreground rompt l’héritage.

![Liste complexe en thème clair et en thème noir à contraste élevé](images/high-contrast-list2.png)

*Liste complexe en thème clair (à gauche) et en thème noir à contraste élevé (à droite). En contraste élevé, l’inversion de la seconde ligne de l’élément sélectionné est mise en échec.*  

Vous pouvez contourner ce problème en définissant Foreground de manière conditionnelle, via un Style d’une collection **ThemeDictionaries**. Dans la mesure où **Foreground** n’est pas défini par **SecondaryBodyTextBlockStyle** dans **HighContrast**, sa couleur sera correctement inversée.

```xaml
<!-- In App.xaml... -->
<ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
        <Style
            x:Key="SecondaryBodyTextBlockStyle"
            TargetType="TextBlock"
            BasedOn="{StaticResource BodyTextBlockStyle}">
            <Setter Property="Foreground" Value="{StaticResource SystemControlForegroundBaseMediumBrush}" />
        </Style>
    </ResourceDictionary>

    <ResourceDictionary x:Key="Light">
        <Style
            x:Key="SecondaryBodyTextBlockStyle"
            TargetType="TextBlock"
            BasedOn="{StaticResource BodyTextBlockStyle}">
            <Setter Property="Foreground" Value="{StaticResource SystemControlForegroundBaseMediumBrush}" />
        </Style>
    </ResourceDictionary>

    <ResourceDictionary x:Key="HighContrast">
        <!-- The Foreground Setter is omitted in HighContrast -->
        <Style
            x:Key="SecondaryBodyTextBlockStyle"
            TargetType="TextBlock"
            BasedOn="{StaticResource BodyTextBlockStyle}" />
    </ResourceDictionary>
</ResourceDictionary.ThemeDictionaries>

<!-- Usage in your DataTemplate... -->
<DataTemplate>
    <StackPanel>
        <TextBlock Style="{StaticResource BodyTextBlockStyle}" Text="Double line list item" />

        <!-- Note how ThemeResource is used to reference the Style -->
        <TextBlock Style="{ThemeResource SecondaryBodyTextBlockStyle}" Text="Second line of text" />
    </StackPanel>
</DataTemplate>
```


## <a name="detecting-high-contrast"></a>Détection du contraste élevé

Vous pouvez vérifier par programme si le thème actuel est un thème à contraste élevé à l’aide des membres de la classe [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237).

> [!NOTE]
> Prenez soin d’appeler le constructeur **AccessibilitySettings** à partir d’une étendue dans laquelle l’application est initialisée et affiche déjà du contenu.

## <a name="related-topics"></a>Rubriques connexes  
* [Accessibilité](accessibility.md)
* [Exemple de paramètres et de contraste d’interface utilisateur](http://go.microsoft.com/fwlink/p/?linkid=231539)
* [Exemple d’accessibilité XAML](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [Exemple de contraste élevé XAML](http://go.microsoft.com/fwlink/p/?linkid=254993)
* [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237)
