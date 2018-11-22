---
author: jwmsft
description: Fournit une valeur pour un attribut XAML en évaluant une référence à une ressource, avec une logique système supplémentaire qui récupère différentes ressources en fonction du thème actuellement actif.
title: Extension de balisage ThemeResource
ms.assetid: 8A1C79D2-9566-44AA-B8E1-CC7ADAD1BCC5
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 024e48380941c0d79eef65780396ec9b89edc3c7
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7568437"
---
# <a name="themeresource-markup-extension"></a>Extension de balisage {ThemeResource}

Fournit une valeur pour un attribut XAML en évaluant une référence à une ressource, avec une logique système supplémentaire qui récupère différentes ressources en fonction du thème actuellement actif. À l’image de l’[extension de balisage {StaticResource}](staticresource-markup-extension.md), les ressources sont définies dans un élément [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794), et une utilisation **ThemeResource** référence la clé de cette ressource dans l’élément **ResourceDictionary**.

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object property="{ThemeResource key}" .../>
```

## <a name="xaml-values"></a>Valeurs XAML

| Terme | Description |
|------|-------------|
| key | Clé de la ressource demandée. Cette clé est initialement affectée par l’élément [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794). Une clé de ressource peut correspondre à toute chaîne définie dans la grammaire XamlName. |

## <a name="remarks"></a>Remarques

Un élément **ThemeResource** est une technique permettant d’obtenir les valeurs d’un attribut XAML définies ailleurs dans un dictionnaire de ressources XAML. L’extension de balisage remplit la même fonction de base que l’[extension de balisage {StaticResource}](staticresource-markup-extension.md). La différence comportementale par rapport à l’extension de balisage {StaticResource} tient au fait qu’une **référence ThemeResource** peut utiliser dynamiquement différents dictionnaires comme emplacement de recherche principal, en fonction du thème en cours d’utilisation par le système.

Au démarrage de l’application, toute référence aux ressources effectuée par une référence **ThemeResource** est évaluée en fonction du thème utilisé à ce moment-là. Toutefois, si l’utilisateur change ultérieurement de thème actif au moment de l’exécution, le système réévalue chaque référence **ThemeResource**, récupère une ressource propre au thème éventuellement différente et réaffiche l’application avec les nouvelles valeurs de ressource à tous les emplacements appropriés dans l’arborescence visuelle. Un élément **StaticResource** est déterminé au chargement du XAML ou au démarrage de l’application et n’est pas réévalué au moment de l’exécution. (D’autres techniques telles que les états visuels rechargent le XAML dynamiquement, mais ces techniques opèrent à un niveau supérieur à celui de l’évaluation de ressource de base autorisé par l’[extension de balisage {StaticResource}](staticresource-markup-extension.md)).

**ThemeResource** utilise un argument, lequel spécifie la clé de la ressource demandée. Une clé de ressource est toujours une chaîne dans le code XAML Windows Runtime. Pour plus d’informations sur la façon dont la clé de ressource est initialement spécifiée, voir [Attribut x:Key](x-key-attribute.md).

Pour plus d’informations sur la façon de définir des ressources et d’utiliser correctement un élément [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794), et pour obtenir un exemple de code, voir l’article [Références aux ressources ResourceDictionary et XAML](https://msdn.microsoft.com/library/windows/apps/mt187273).

**Important** Comme dans le cas de **StaticResource**, un élément **ThemeResource** ne doit pas essayer de faire une référence anticipée à une ressource définie lexicalement plus loin dans le fichier XAML. Une telle tentative n’est pas prise en charge. Même si la référence anticipée n’échoue pas, toute tentative en ce sens pénalise les performances. Pour obtenir de meilleurs résultats, ajustez la composition de vos dictionnaires de ressources afin d’éviter les références anticipées.

Une tentative de définition d’un élément **ThemeResource** sur une clé impossible à résoudre lève une exception d’analyse XAML au moment de l’exécution. Les outils de conception peuvent également générer des avertissements ou des erreurs.

Dans l’implémentation du processeur XAML Windows Runtime, il n’existe aucune représentation de classe de stockage pour **ThemeResource**. L’équivalent le plus proche dans le code consiste à utiliser l’API de collection d’un élément [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794), par exemple l’appel à [**Contains**](https://msdn.microsoft.com/library/windows/apps/jj635925) ou à [**TryGetValue**](https://msdn.microsoft.com/library/windows/apps/jj603139).

**ThemeResource** est une extension de balisage. Les extensions de balisage sont généralement implémentées lorsqu’il est nécessaire de procéder à l’échappement de valeurs d’attribut pour en faire autre chose que des valeurs littérales ou des noms de gestionnaire. Il s’agit d’une mesure plus globale que celle qui consiste à placer simplement des convertisseurs de type au niveau de certains types ou propriétés. Toutes les extensions de balisage XAML utilisent les caractères «{» et «}» dans leur syntaxe d’attribut, ce qui correspond à la convention qui permet au processeur XAML de reconnaître qu’une extension de balisage doit traiter l’attribut.

### <a name="when-and-how-to-use-themeresource-rather-than-staticresource"></a>Quand et comment utiliser {ThemeResource} plutôt que {StaticResource}

Les règles selon lesquelles un élément **ThemeResource** est résolu en un élément d’un dictionnaire de ressources sont généralement les mêmes que celles pour **StaticResource**. Une recherche **ThemeResource** peut s’étendre aux fichiers [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) référencés dans une collection [**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/br208807), mais un élément **StaticResource** peut également effectuer cette opération. La différence tient au fait qu’un élément **ThemeResource** peut faire l’objet d’une réévaluation au moment de l’exécution, contrairement à un élément **StaticResource**.

L’ensemble de clés dans chaque dictionnaire de thème doit fournir le même ensemble de ressources à clé, quel que soit le thème actif. Si une ressource à clé existe dans le dictionnaire de thème **HighContrast**, une autre ressource portant le même nom doit également exister dans **Light** et **Default**. Si ce n’est pas le cas, la recherche de ressource risque d’échouer quand l’utilisateur change de thème, et l’aspect de votre application en sera affecté. Il est cependant possible qu’un dictionnaire de thème contienne des ressources à clé qui ne sont référencées qu’à partir de la même étendue pour fournir des sous-valeurs; il n’est pas nécessaire que celles-ci soient équivalentes dans tous les thèmes.

En général, vous ne devez placer les ressources dans les dictionnaires de thème et établir des références à ces ressources à l’aide de **ThemeResource** que quand ces valeurs peuvent changer d’un thème à l’autre ou qu’elles sont prises en charge par des valeurs qui évoluent. Cette approche est appropriée pour les types de ressources suivants:

-   Pinceaux, notamment les couleurs pour [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962). Ces derniers représentent environ 80 % des utilisations de **ThemeResource** dans les modèles de contrôle XAML par défaut (generic.xaml).
-   Valeurs de pixel pour les bordures, les décalages, les marges et les remplissages, etc.
-   Propriétés de police telles que **FontFamily** ou **FontSize**.
-   Modèles complets pour un nombre limité de contrôles dont le style est généralement défini par le système et qui sont couramment utilisés pour une présentation dynamique, comme [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/hh738501) et [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/br242919).
-   Styles d’affichage de texte (généralement pour modifier la couleur, l’arrière-plan et éventuellement la taille de la police).

Windows Runtime offre un ensemble de ressources qui sont spécifiquement conçues pour être référencées par **ThemeResource**. Ces ressources sont toutes répertoriées dans le fichier XAML themeresources.xaml figurant dans le dossier include/winrt/xaml/design du Kit de développement logiciel (SDK) Windows. Pour obtenir des informations sur les pinceaux de thème et les styles supplémentaires définis dans themeresources.xaml, voir l’article [Ressources de thème XAML](https://msdn.microsoft.com/library/windows/apps/mt187274). Les pinceaux sont documentés dans un tableau qui indique la valeur de couleur de chaque pinceau pour chacun des trois thèmes actifs possibles.

Les définitions XAML des états visuels dans un modèle de contrôle doivent utiliser des références **ThemeResource** chaque fois qu’une ressource sous-jacente est susceptible d’évoluer en raison de la modification d’un thème. En règle générale, la modification d’un thème système n’entraîne pas de changement d’état visuel. Dans ce cas, les ressources doivent utiliser des références **ThemeResource** afin que les valeurs puissent être réévaluées pour l’état visuel toujours actif. Par exemple, si un état visuel change une couleur de pinceau pour une partie spécifique de l’interface utilisateur et l’une de ses propriétés, et que cette couleur de pinceau diffère d’un thème à l’autre, vous devez utiliser une référence **ThemeResource** pour fournir la valeur de cette propriété dans le modèle par défaut, ainsi que toute modification d’état visuel dans ce modèle par défaut.

Les utilisations de **ThemeResource** peuvent être considérées comme une série de valeurs dépendantes. Par exemple, une valeur [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723) utilisée par un élément [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962) qui est également une ressource à clé peut utiliser une référence **ThemeResource**. Toutefois, toutes les propriétés d’interface utilisateur qui utilisent la ressource **SolidColorBrush** à clé utilisent également une référence **ThemeResource**, de sorte que chaque propriété de type [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) autorise spécifiquement une modification de valeur dynamique quand le thème change.

**Remarque** `{ThemeResource}` et d’évaluation de ressource d’exécution lors du changement du thème est pris en charge dans le code XAML Windows8.1, mais non dans le code XAML pour les applications ciblant package Windows8.

### <a name="system-resources"></a>Ressources système

Certaines ressources de thème référencent des valeurs de ressource système comme sous-valeur sous-jacente. Une ressource système est une valeur de ressource spéciale qui ne se trouve dans aucun dictionnaire de ressource XAML. Ces valeurs dépendent du XAML Windows Runtime, qui prend en charge le transfert de valeurs à partir du système proprement dit, ainsi que leur représentation sous une forme qu’une ressource XAML peut référencer. Par exemple, il existe une ressource système nommée «SystemColorButtonFaceColor» qui représente une couleur RVB. Cette couleur est tributaire des différents aspects des thèmes et couleurs système qui ne sont pas uniquement propres à Windows Runtime et aux applications Windows Runtime.

Les ressources système sont souvent les valeurs sous-jacentes d’un thème à contraste élevé. L’utilisateur contrôle les choix de couleur de son thème à contraste élevé et effectue ces choix à l’aide de fonctionnalités système qui, elles non plus, ne sont pas propres aux applications Windows Runtime. Grâce au référencement des ressources système en tant que références **ThemeResource**, le comportement par défaut des thèmes à contraste élevé pour les applications Windows Runtime peut utiliser les valeurs propres au thème contrôlées par l’utilisateur et exposées par le système. En outre, les références sont désormais marquées pour la réévaluation si le système détecte un changement de thème au moment de l’exécution.

### <a name="an-example-themeresource-usage"></a>Exemple d’utilisation de {ThemeResource}

Voici un exemple de XAML tiré des fichiers generic.xaml et themeresources.xaml par défaut et illustrant l’utilisation de **ThemeResource**. Nous n’allons examiner qu’un seul modèle (l’élément [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) par défaut) et analyser comment deux propriétés ([**Background**](https://msdn.microsoft.com/library/windows/apps/br209395) et [**Foreground**](https://msdn.microsoft.com/library/windows/apps/br209414)) sont déclarées pour réagir aux changements de thème.

```xml
    <!-- Default style for Windows.UI.Xaml.Controls.Button -->
    <Style TargetType="Button">
        <Setter Property="Background" Value="{ThemeResource ButtonBackgroundThemeBrush}" />
        <Setter Property="Foreground" Value="{ThemeResource ButtonForegroundThemeBrush}"/>
...
```

Ici, les propriétés prennent une valeur [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush), et la référence aux ressources [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962) nommées `ButtonBackgroundThemeBrush` et `ButtonForegroundThemeBrush` est effectuée à l’aide de **ThemeResource**.

Ces mêmes propriétés sont également ajustées par certains états visuels pour un élément [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265). En particulier, la couleur d’arrière-plan change quand l’utilisateur clique sur un bouton. Ici également, les animations [**Background**](https://msdn.microsoft.com/library/windows/apps/br209395) et [**Foreground**](https://msdn.microsoft.com/library/windows/apps/br209414) dans la table de montage séquentiel des états visuels utilisent des objets [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/br243132) et des références aux pinceaux avec **ThemeResource** comme valeur d’image clé.

```xml
<VisualState x:Name="Pressed">
  <Storyboard>
    <ObjectAnimationUsingKeyFrames Storyboard.TargetName="Border"
        Storyboard.TargetProperty="Background">
      <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonPressedBackgroundThemeBrush}" />
    </ObjectAnimationUsingKeyFrames>
    <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ContentPresenter"
         Storyboard.TargetProperty="Foreground">
       <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource ButtonPressedForegroundThemeBrush}" />
    </ObjectAnimationUsingKeyFrames>
  </Storyboard>
</VisualState>
```

Chacun de ces pinceaux est défini précédemment dans generic.xaml: le fait qu’ils soient définis avant tout modèle qui les utilise évite des références XAML anticipées. Voici ces définitions pour le dictionnaire de thème «Default».

```xml
    <ResourceDictionary.ThemeDictionaries>
        <ResourceDictionary x:Key="Default">
...
            <SolidColorBrush x:Key="ButtonBackgroundThemeBrush" Color="Transparent" />
            <SolidColorBrush x:Key="ButtonForegroundThemeBrush" Color="#FFFFFFFF" />
...
            <SolidColorBrush x:Key="ButtonPressedBackgroundThemeBrush" Color="#FFFFFFFF" />
            <SolidColorBrush x:Key="ButtonPressedForegroundThemeBrush" Color="#FF000000" />
...
```

Ensuite, ces pinceaux sont également définis pour chacun des autres dictionnaires de thème, par exemple:

```xml
        <ResourceDictionary x:Key="HighContrast">
            <!-- High Contrast theme resources -->
...
            <SolidColorBrush x:Key="ButtonBackgroundThemeBrush" Color="{ThemeResource SystemColorButtonFaceColor}" />
            <SolidColorBrush x:Key="ButtonForegroundThemeBrush" Color="{ThemeResource SystemColorButtonTextColor}" />

...
            <SolidColorBrush x:Key="ButtonPressedBackgroundThemeBrush" Color="{ThemeResource SystemColorButtonTextColor}" />
            <SolidColorBrush x:Key="ButtonPressedForegroundThemeBrush" Color="{ThemeResource SystemColorButtonFaceColor}" />
```

Ici, la valeur [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) est une autre référence **ThemeResource** à une ressource système. Si vous référencez une ressource système et que vous souhaitez qu’elle évolue à chaque changement de thème, vous devez utiliser **ThemeResource** pour établir la référence.

## <a name="windows8-behavior"></a>Comportement de package Windows8

Package Windows8 ne prenaient pas en charge l’extension de balisage **ThemeResource** , il est disponible à partir de Windows8.1. En outre, package Windows8 ne gérait pas changement dynamique des ressources liées aux thèmes pour une application Windows Runtime. Vous deviez redémarrer l’application pour que le changement de thème soit activé pour les styles et les modèles XAML. Cela n’est pas une bonne expérience utilisateur, afin que les applications sont vivement recommandées de recompile et cible Windows8.1 afin qu’ils peuvent utiliser des styles avec des utilisations **ThemeResource** et que vous pouvant basculer dynamiquement les thèmes lorsque l’utilisateur effectue. Applications qui ont été compilées pour le package Windows8 mais s’exécutant sur Windows8.1 continue à utiliser le comportement de package Windows8.

## <a name="design-time-tools-support-for-the-themeresource-markup-extension"></a>Prise en charge d’outils au moment de la conception pour l’extension de balisage **{ThemeResource}**

Studio2013 Visual Microsoft peut inclure les valeurs de clés possibles dans les listes déroulantes Microsoft IntelliSense lorsque vous utilisez l’extension de balisage **{ThemeResource}** dans une page XAML. Par exemple, dès que vous tapez « {ThemeResource », toute clé de ressource provenant des [ressources de thème XAML](https://msdn.microsoft.com/library/windows/apps/mt187274) s’affiche.

Lorsqu’une clé de ressource fait partie intégrante d’une utilisation quelconque de **{ThemeResource}**, la fonctionnalité **Atteindre la définition**(F12) peut résoudre cette ressource et vous présenter le fichier generic.xaml, dans lequel la ressource de thème est définie, à utiliser au moment de la conception. Les ressources de thème étant définies plus d’une fois (par thème), la fonctionnalité **Atteindre la définition** vous conduit à la première définition trouvée dans le fichier, c’est-à-dire la définition de **Default**. Si vous souhaitez obtenir les autres définitions, vous pouvez rechercher le nom de la clé dans le fichier et accéder aux définitions des autres thèmes.

## <a name="related-topics"></a>Rubriques connexes

* [Références aux ressources ResourceDictionary et XAML](https://msdn.microsoft.com/library/windows/apps/mt187273)
* [Ressources de thème XAML](https://msdn.microsoft.com/library/windows/apps/mt187274)
* [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)
* [Attribut x:Key](x-key-attribute.md)
 

