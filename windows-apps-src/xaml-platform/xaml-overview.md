---
author: jwmsft
description: Nous présentons le langage XAML et les concepts XAML aux développeurs d’applications Windows Runtime, puis nous décrivons les différentes manières de déclarer des objets et de définir des attributs en langage XAML tel qu’il est utilisé pour créer une application du Windows Store.
title: Vue d’ensemble du langage XAML
ms.assetid: 48041B37-F1A8-44A4-BB8E-1D4DE30E7823
ms.author: jimwalk
ms.date: 07/18/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: b2576c11e50092b5d8e70189e0e8a88cfab716f9
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6274138"
---
# <a name="xaml-overview"></a>Vue d’ensemble du langage XAML

Nous présentons le langage XAML et les concepts XAML aux développeurs d’applications Windows Runtime, puis nous décrivons les différentes manières de déclarer des objets et de définir des attributs en langage XAML tel qu’il est utilisé pour créer une application du Windows Store.

## <a name="what-is-xaml"></a>Qu’est-ce que le XAML ?

Le langage XAML (Extensible Application Markup Language) est un langage déclaratif. Plus précisément, le langage XAML permet d’initialiser des objets et de définir des propriétés d’objets, à l’aide d’une structure de langage qui montre les relations hiérarchiques entre plusieurs objets et en utilisant une convention des types de stockage qui prend en charge l’extension des types. Vous pouvez créer des éléments d’interface utilisateur visibles dans le balisage XAML déclaratif. Vous pouvez alors associer un fichier code-behind distinct pour chaque fichier XAML pouvant répondre à des événements et manipuler les objets que vous déclarez à l’origine en XAML.

Le langage XAML prend en charge l’échange de sources entre différents outils et rôles dans le processus de développement, notamment l’échange de sources XAML entre outils de conception et IDE, ou entre développeurs généraux et développeurs en localisation. En utilisant XAML comme format d’échange, les rôles de concepteur et de développeur peuvent être isolés ou rassemblés, et les concepteurs et développeurs peuvent effectuer une itération pendant la production d’une application.

Les fichiers XAML que vous découvrez dans le cadre de vos projets d’applications Windows Runtime sont en réalité des fichiers XML avec l’extension de nom de fichier .xaml.

## <a name="basic-xaml-syntax"></a>Syntaxe XAML de base

La syntaxe de base du langage XAML s’appuie sur le langage XML. Par définition, un code XAML valide doit également être un code XML valide. Toutefois, le code XAML possède également des concepts de syntaxe auxquels une autre signification plus complète est attribuée, tout en restant valides dans XML selon la spécification XML 1.0. Par exemple, le code XAML prend en charge la *syntaxe de l’élément de propriété*, qui implique la possibilité de définir des valeurs de propriété au sein d’éléments plutôt qu’en tant que valeurs de chaîne dans les attributs ou en tant que contenu. Pour le langage XML ordinaire, l’élément de propriété XAML constitue un élément dont le nom contient un point. Il est donc valide en langage XML brut, mais n’a pas la même signification.

## <a name="xaml-and-microsoft-visual-studio"></a>Langage XAML et Microsoft Visual Studio

Microsoft Visual Studio vous aide à produire une syntaxe XAML valide, à la fois dans l’éditeur de texte XAML et dans l’aire de conception XAML, plus orientée vers les graphiques. Lorsque vous écrivez du code XAML pour votre application à l’aide de Visual Studio, ne vous préoccupez donc pas trop de la syntaxe à chaque séquence de touches. L’IDE vous aide souvent à écrire une syntaxe XAML valide en fournissant des conseils de saisie semi-automatique, en proposant des suggestions dans les listes et menus déroulants Microsoft IntelliSense, en affichant des bibliothèques d’éléments d’interface utilisateur dans la boîte à outils, ou par d’autres techniques. S’il s’agit de votre première expérience avec le langage XAML, il peut quand même s’avérer utile de connaître les règles de syntaxe et plus particulièrement la terminologie parfois utilisée pour décrire les restrictions ou les choix lorsque nous décrivons la syntaxe XAML dans les rubriques de référence ou dans d’autres rubriques. Nous abordons ces points détaillés de la syntaxe XAML dans une autre rubrique intitulée [Guide de la syntaxe XAML](xaml-syntax-guide.md).

## <a name="xaml-namespaces"></a>Espaces de noms XAML

En programmation au sens large, un espace de noms est un concept d’organisation qui détermine la façon dont les identificateurs pour les entités de programmation sont interprétés. Grâce aux espaces de noms, une infrastructure de programmation peut séparer les identificateurs déclarés par l’utilisateur de ceux déclarés par l’infrastructure, éliminer l’ambiguïté des identificateurs par le biais des qualifications d’espaces de noms, appliquer des règles de création d’étendue de noms, etc. XAML a son propre concept d’espace de noms XAML qui assume cette fonction pour le langage XAML. Voici comment XAML applique et étend les concepts d’espaces de noms du langage XML:

-   XAML utilise l’attribut XML **xmlns** réservé pour les déclarations d’espaces de noms. La valeur de l’attribut est généralement un URI (Uniform Resource Identifier), qui est une convention héritée de XML.
-   Les déclarations de préfixes en XAML servent à déclarer les espaces de noms différents de l’espace de noms par défaut, et les utilisations de préfixe dans des éléments et attributs font référence à ces espaces de noms.
-   Le langage XAML possède un concept d’espace de noms par défaut, qui est l’espace de noms utilisé lorsqu’une utilisation ou déclaration ne comprend aucun préfixe. L’espace de noms par défaut peut être défini de façon différente pour chaque infrastructure de programmation XAML.
-   Dans une construction ou un fichier XAML, les définitions d’espaces de noms héritent d’élément parent à élément enfant. Par exemple, si vous définissez un espace de noms dans l’élément racine d’un fichier XAML, tous les éléments de ce fichier héritent de cette définition d’espace de noms. Si un élément plus loin dans la page redéfinit l’espace de noms, les descendants de cet élément héritent de la nouvelle définition.
-   Les attributs d’un élément héritent des espaces de noms de l’élément. Il est relativement peu courant de voir des préfixes sur des attributs XAML.

Un fichier XAML déclare presque toujours un espace de noms XAML par défaut dans son élément racine. L’espace de noms XAML par défaut définit les éléments que vous pouvez déclarer sans les qualifier à l’aide d’un préfixe. Pour les projets d’application Windows Runtime standard, cet espace de noms par défaut contient tout le vocabulaire XAML intégré pour Windows Runtime qui est utilisé pour les définitions d’interface utilisateur : contrôles par défaut, éléments de texte, animations et graphiques XAML, types de prise en charge d’application de style et de liaison de données, etc. La plupart du code XAML que vous écrivez pour les applications Windows Runtime est donc capable d’éviter les préfixes et les espaces de noms XAML dans les références aux éléments d’interface utilisateur courants.

Voici un extrait de code illustrant une racine [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503), créée à l’aide d’un modèle, de la page initiale d’une application (montrant uniquement la balise d’ouverture simplifiée) : il déclare l’espace de noms par défaut et également l’espace de noms **x** (que nous allons ensuite expliquer).

```xml
<Page
    x:Class="Application1.BlankPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
>
```

## <a name="the-xaml-language-xaml-namespace"></a>Espace de noms XAML de langage XAML

Un espace de noms XAML particulier qui est déclaré dans presque tous les fichiers XAML Windows Runtime est l’espace de noms de langage XAML. Cet espace de noms inclut des éléments et des concepts définis par le langage XAML, par sa spécification du langage. Par convention, l’espace de noms XAML de langage XAML est mappé au préfixe «x». Les modèles de projet et de fichier par défaut des projets d’applications Windows Runtime définissent toujours à la fois l’espace de noms XAML par défaut (sans préfixe, juste `xmlns=`) et l’espace de noms XAML de langage XAML (préfixe « x ») dans le cadre de l’élément racine.

Le préfixe «x»/l’espace de noms XAML de langage XAML contient plusieurs constructions de programmation que vous utilisez souvent dans votre code XAML. Voici les plus couramment employées:

| Terme | Description |
|------|-------------|
| [x:Key](x-key-attribute.md) | Définit une clé unique définie par l’utilisateur pour chaque ressource dans un [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) XAML. La chaîne du jeton de la clé est l’argument de l’extension de balisage **StaticResource**. Vous utiliserez cette clé ultérieurement pour récupérer la ressource XAML d’une autre utilisation XAML à un autre endroit du code XAML de votre application. |
| [x:Class](x-class-attribute.md) | Indique l’espace de noms de code et le nom de la classe de code qui fournit le code-behind pour une page XAML. Nomme ainsi la classe créée ou jointe par des actions de génération lorsque vous générez votre application. Ces actions de génération prennent en charge le compilateur de balisage XAML, et combinent le balisage et le code-behind lorsque l’application est compilée. Vous devez avoir une telle classe pour prendre en charge le code-behind pour une page XAML. [**Window.Content**](https://msdn.microsoft.com/library/windows/apps/br209051) dans le modèle d’activation Windows Runtime par défaut. |
| [x:Name](x-name-attribute.md) | Spécifie un nom d’objet au moment de l’exécution pour l’instance qui existe dans le code d’exécution après le traitement d’un élément objet défini en XAML. La définition de **x:Name** en XAML s’apparente à la déclaration d’une variable nommée dans le code. Comme vous l’apprendrez plus tard, c’est exactement ce qui se produit lorsque votre code XAML est chargé en tant que composant d’une application Windows Runtime. <br/><div class="alert">**Remarque** [**FrameworkElement.Name**](https://msdn.microsoft.com/library/windows/apps/br208735) est une propriété similaire dans l’infrastructure, mais pas tous les éléments de la prise en charge. Vous devez donc utiliser **x:Name** pour l’identification d’élément chaque fois que **FrameworkElement.Name** n’est pas pris en charge sur ce type d’élément. |
| [x:Uid](x-uid-directive.md) | Identifie les éléments qui doivent utiliser des ressources localisées pour certaines de leurs valeurs de propriétés. Pour plus d’informations sur l’utilisation de **x:Uid**, voir [Démarrage rapide : traduction des ressources de l’interface utilisateur](https://msdn.microsoft.com/library/windows/apps/xaml/hh965329). |
| [Types de données intrinsèques XAML](xaml-intrinsic-data-types.md) | Ces types peuvent spécifier des valeurs pour des types à valeur simples lorsqu’un attribut ou une ressource l’exige. Ces types intrinsèques correspondent aux types à valeur simples habituellement définies dans le cadre des définitions intrinsèques de chaque langage de programmation. Par exemple, vous pouvez avoir besoin d’un objet représentant une valeur booléenne **true** afin de l’utiliser dans un état visuel dans une table de montage séquentiel [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/br210320). Pour cette valeur en XAML, vous allez utiliser le type intrinsèque **x:Boolean** en tant qu’élément objet, comme ceci: <code>&lt;x:Boolean&gt;True&lt;/x:Boolean&gt;</code> | 

Il existe d’autres constructions de programmation dans l’espace de noms XAML de langage XAML, mais elles ne sont pas aussi courantes.

## <a name="mapping-custom-types-to-xaml-namespaces"></a>Mappage de types personnalisés à des espaces de noms XAML

La simplicité avec laquelle vous pouvez étendre le vocabulaire XAML des applications Windows Runtime représente l’un des aspects les plus puissants du langage XAML. Vous pouvez définir vos propres types personnalisés dans le langage de programmation de votre application, puis référencer vos types personnalisés dans le balisage XAML. La prise en charge de l’extension via des types personnalisés est fondamentalement intégrée au mode de fonctionnement du langage XAML. Les infrastructures ou les développeurs d’applications sont chargés de créer les objets de stockage référencés par le langage XAML. Ni les infrastructures ni le développeur de l’application ne sont liés par des spécifications sur la représentation de ces objets dans leur vocabulaire ou leur action au-delà des règles de syntaxe XAML de base (il est attendu que les types d’espace de noms XAML en langage XAML accomplissent certaines actions, mais Windows Runtime assure toute la prise en charge nécessaire).

Si vous utilisez XAML pour des types qui proviennent d’autres bibliothèques que les bibliothèques et métadonnées principales Windows Runtime, vous devez déclarer et mapper un espace de noms XAML avec un préfixe. Utilisez ce préfixe dans les utilisations d’élément pour référencer les types définis dans votre bibliothèque. Vous devez déclarer les mappages de préfixes en tant qu’attributs **xmlns**, généralement dans un élément racine avec d’autres définitions d’espace de noms XAML.

Pour créer votre propre définition d’espace de noms faisant référence aux types personnalisés, vous devez d’abord spécifier le mot clé **xmlns:**, puis le préfixe souhaité. La valeur de cet attribut doit contenir le mot clé **using:** en tant que première partie de la valeur. Le reste de la valeur est un jeton de chaîne qui fait nominativement référence à l’espace de noms de stockage de code spécifique contenant vos types personnalisés.

Le préfixe définit le jeton de balisage utilisé pour faire référence à cet espace de noms XAML dans le reste du balisage dans ce fichier XAML. Un signe deux-points (:) est placé entre le préfixe et l’entité à référencer dans l’espace de noms XAML.

Par exemple, la syntaxe d’attribut pour mapper un préfixe `myTypes` à l’espace de noms `myCompany.myTypes` est `    xmlns:myTypes="using:myCompany.myTypes"`, et une utilisation d’élément représentative est `<myTypes:CustomButton/>`

Pour plus d’informations sur le mappage des espaces de noms XAML pour les types personnalisés, y compris les considérations spéciales pour les extensions de composant Visual c++ (C++ / CX), voir les [espaces de noms XAML et mappage d’espace de noms](xaml-namespaces-and-namespace-mapping.md).

## <a name="other-xaml-namespaces"></a>Autres espaces de noms XAML

On voit souvent des fichiers XAML qui définissent les préfixes «d» (pour l’espace de noms du concepteur) et «mc» (pour la compatibilité du balisage). En général, ils sont destinés à la prise en charge de l’infrastructure ou de scénarios dans un outil au moment de la conception. Pour plus d’informations, voir la [section « Autres espaces de noms XAML » de la rubrique sur les espaces de noms XAML](xaml-namespaces-and-namespace-mapping.md#other-XAML-namespaces).

## <a name="markup-extensions"></a>Extensions de balisage

Les extensions de balisage constituent un concept du langage XAML qui est souvent utilisé dans l’implémentation XAML Windows Runtime. Les extensions de balisage constituent souvent un type de raccourci permettant à un fichier XAML d’accéder à une valeur ou à un comportement qui ne se contente pas de déclarer les éléments en fonction des types de stockage. Certaines extensions de balisage peuvent définir des propriétés avec des chaînes brutes ou avec d’autres éléments imbriqués, le but étant de simplifier la syntaxe ou la refactorisation entre différents fichiers XAML.

Dans la syntaxe d’attribut XAML, les accolades «{» et «}» indiquent l’utilisation d’une extension de balisage XAML. Cette utilisation ordonne au traitement XAML d’éviter le traitement général consistant à traiter les valeurs d’attributs en tant que chaîne littérale ou valeur directement convertible en chaîne. À la place, un analyseur XAML appelle le code qui fournit le comportement pour cette extension de balisage particulière, et ce code fournit un autre objet ou résultat de comportement exigé par l’analyseur XAML. Les extensions de balisage peuvent posséder des arguments qui suivent le nom de l’extension de balisage et qui sont également contenus dans les accolades. En général, une extension de balisage évaluée fournit une valeur de retour de type objet. Pendant l’analyse, cette valeur de retour est insérée à l’emplacement de l’arborescence d’objets où l’utilisation de l’extension de balisage se situait dans le code XAML source.

Le langage XAML Windows Runtime prend en charge ces extensions de balisage qui sont définies sous l’espace de noms XAML par défaut et comprises par son analyseur XAML :

-   [{xBind}](x-bind-markup-extension.md) : prend en charge la liaison de données, qui diffère l'évaluation de propriété jusqu'au moment de l'exécution par l'exécution de code à usage spécial, qu’il génère au moment de la compilation. Cette extension de balisage prend en charge une large gamme d’arguments.
-   [{Binding}](binding-markup-extension.md) : prend en charge la liaison de données, qui diffère l'évaluation de propriété jusqu'au moment de l'exécution par l'exécution d’une inspection d’objet runtime à usage général. Cette extension de balisage prend en charge une large gamme d’arguments.
-   [{StaticResource}](staticresource-markup-extension.md) : prend en charge le référencement des valeurs de ressources qui sont définies dans un [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794). Ces ressources peuvent se trouver dans un autre fichier XAML, mais doivent en fin de compte être localisables par l’analyseur XAML au moment du chargement. L’argument de l’utilisation d’un `{StaticResource}` identifie la clé (le nom) pour une ressource indexée dans un [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794).
-   [{ThemeResource}](themeresource-markup-extension.md) : similaire à [{StaticResource}](staticresource-markup-extension.md), mais peut répondre à des modifications de thème au moment de l’exécution. {ThemeResource} apparaît assez souvent dans les modèles XAML Windows Runtime par défaut, car la plupart de ces modèles sont conçus afin d’assurer la compatibilité lorsque l’utilisateur change de thème pendant l’exécution de l’application.
-   [{TemplateBinding}](templatebinding-markup-extension.md) : cas spécial de [{Binding}](binding-markup-extension.md) qui prend en charge les modèles de contrôle en XAML et leur utilisation éventuelle au moment de l’exécution.
-   [{RelativeSource}](relativesource-markup-extension.md) : autorise une forme particulière de liaison de modèles où les valeurs proviennent du parent basé sur un modèle.
-   [{CustomResource}](customresource-markup-extension.md) : pour les scénarios de recherche de ressource avancés.

Windows Runtime prend également en charge l’[extension de balisage {x:Null}](x-null-markup-extension.md). Celle-ci est utilisée pour définir les valeurs [**Nullable**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx) sur **null** en XAML. Par exemple, vous pouvez l’utiliser dans un modèle de contrôle pour un élément [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316), qui interprète **null** comme un état de case à cocher indéterminé (déclenchant l’état visuel « Indeterminate »).

Une extension de balisage retourne généralement une instance existante provenant d’une autre partie du graphique d’objet de l’application, ou diffère une valeur au moment de l’exécution. Dans la mesure où vous pouvez utiliser une extension de balisage comme valeur d’attribut, et il s’agit là de son utilisation classique, vous voyez souvent des extensions de balisage fournissant des valeurs pour des propriétés de type référence qui auraient pu sinon requérir une syntaxe d’élément de propriété.

Par exemple, voici la syntaxe pour référencer un élément [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849) réutilisable à partir d’un [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) : `<Button Style="{StaticResource SearchButtonStyle}"/>`. Un élément [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849) étant un type référence et non une valeur simple, sans l’utilisation de `{StaticResource}`, vous auriez eu besoin d’un élément de propriété `<Button.Style>` et d’une définition `<Style>` dans cet élément pour définir la propriété [**FrameworkElement.Style**](https://msdn.microsoft.com/library/windows/apps/br208743).

En utilisant des extensions de balisage, chaque propriété qui peut être définie en XAML peut éventuellement être définie dans la syntaxe d’attribut. Vous pouvez utiliser la syntaxe d’attribut pour fournir des valeurs de référence pour une propriété même si elle ne prend sinon pas en charge une syntaxe d’attribut pour l’instanciation directe des objets. Vous pouvez aussi activer un comportement spécifique qui diffère l’exigence générale selon laquelle les propriétés XAML sont renseignées par des types valeur ou des types référence nouvellement créés.

En guise d’illustration, l’exemple de code XAML suivant définit la valeur de la propriété [**Style**](https://msdn.microsoft.com/library/windows/apps/br208743) d’un élément [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) à l’aide de la syntaxe d’attribut. La propriété [**Style**](https://msdn.microsoft.com/library/windows/apps/br208743) prend une instance de la classe [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849), un type de référence qui par défaut n’a pas pu être créé à l’aide d’une chaîne de syntaxe d’attribut. Mais dans ce cas, l’attribut référence une extension de balisage particulière, [StaticResource](staticresource-markup-extension.md). Quand cette extension de balisage est traitée, elle renvoie une référence à un élément **Style** précédemment défini sous forme de ressource à clé dans un dictionnaire de ressources.

```xml
<Canvas.Resources>
  <Style TargetType="Border" x:Key="PageBackground">
    <Setter Property="BorderBrush" Value="Blue"/>
    <Setter Property="BorderThickness" Value="5"/>
  </Style>
</Canvas.Resources>
...
<Border Style="{StaticResource PageBackground}">
  ...
</Border>
```

Vous pouvez imbriquer des extensions de balisage. L’extension de balisage la plus profonde est évaluée en premier.

En raison des extensions de balisage, vous devez utiliser une syntaxe spéciale pour une valeur «{» littérale dans un attribut. Pour plus d’informations, voir [Guide de la syntaxe XAML](xaml-syntax-guide.md).

## <a name="events"></a>Événements

Le langage XAML est un langage déclaratif pour les objets et leurs propriétés, mais il inclut également une syntaxe pour attacher des gestionnaires d’événements aux objets dans le balisage. La syntaxe d’événement XAML peut alors intégrer les événements déclarés en XAML via le modèle de programmation Windows Runtime. Vous spécifiez le nom de l’événement sous forme de nom d’attribut sur l’objet dans lequel l’événement est géré. Pour la valeur de l’attribut, vous spécifiez le nom d’une fonction de gestionnaire d’événements que vous définissez dans le code. Le processeur XAML utilise ce nom pour créer une représentation déléguée dans l’arborescence d’objets chargée, puis ajoute le gestionnaire spécifié à une liste de gestionnaires internes. Presque toutes les applications Windows Runtime sont définies à la fois par un balisage et des sources code-behind.

Voici un exemple simple. La classe [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) prend en charge un événement nommé [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737). Vous pouvez écrire un gestionnaire pour **Click** qui exécute du code appelé dès que l’utilisateur clique sur l’objet **Button**. En XAML, vous spécifiez **Click** comme attribut de **Button**. Pour la valeur d’attribut, indiquez une chaîne représentant le nom de méthode de votre gestionnaire.

```xml
<Button Click="showUpdatesButton-Click">Show updates</Button>
```

Au moment de la compilation, le compilateur suppose qu’une méthode nommée `showUpdatesButton-Click` est définie dans le fichier code-behind, dans l’espace de noms déclaré dans la valeur [x:Class](x-class-attribute.md) de la page XAML. En outre, cette méthode doit satisfaire au contrat de délégation lié à l’événement [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737). Par exemple :

```csharp
namespace App1
{
    public sealed partial class MainPage: Page {
        ...
        private void showUpdatesButton_Click (object sender, RoutedEventArgs e) {
            //your code
        }
    }
}
```

```vb
' Namespace included at project level
Public NotInheritable Class MainPage
    Inherits Page
        ...
        Private Sub showUpdatesButton_Click (sender As Object, e As RoutedEventArgs e)
            ' your code
        End Sub
    ...
End Class
```

```cppwinrt
namespace winrt::App1::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        ...
        void showUpdatesButton_Click(Windows::Foundation::IInspectable const&, Windows::UI::Xaml::RoutedEventArgs const&);
    };
}
```

```cpp
// .h
namespace App1
{
    public ref class MainPage sealed {
        ...
    private:
        void showUpdatesButton_Click(Object^ sender, RoutedEventArgs^ e);
    };
}
```

Au sein d’un projet, le code XAML est écrit sous forme de fichier .xaml et vous utilisez le langage de votre choix (C#, Visual Basic, C++/CX) pour écrire un fichier code-behind. Lorsque le balisage d’un fichier XAML est compilé dans le cadre d’une action de génération du projet, l’emplacement du fichier XAML code-behind de chaque page XAML est identifié en spécifiant un espace de noms et une classe sous forme d’attribut [x:Class](x-class-attribute.md) de l’élément racine de la page XAML. Pour plus d’informations sur le fonctionnement de ces mécanismes en XAML et sur leur relation avec les modèles de programmation et d’application, voir [Vue d’ensemble des événements et des événements routés](events-and-routed-events-overview.md).

**Remarque**pour les langages c++ / CX, il existe deux fichiers code-behind, un correspond à l’en-tête (. xaml.h) et l’autre implémentation (. xaml.cpp). L’implémentation fait référence à l’en-tête. D’un point de vue technique, c’est l’en-tête qui représente le point d’entrée de la connexion code-behind.

## <a name="resource-dictionaries"></a>Dictionnaires de ressources

La création d’un [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) est une tâche courante généralement accomplie en créant un dictionnaire de ressources en tant que zone d’une page XAML ou d’un fichier XAML distinct. Les dictionnaires de ressources et leur mode d’utilisation représentent un domaine conceptuel plus important qui sort du cadre de cette rubrique. Pour plus d’informations, voir [Références aux ressources ResourceDictionary et XAML](https://msdn.microsoft.com/library/windows/apps/mt187273).

## <a name="xaml-and-xml"></a>XAML et XML

Le langage XAML se base fondamentalement sur le langage XML. Mais il étend considérablement le langage XML. Plus précisément, il traite le concept de schéma légèrement différemment en raison de sa relation au concept de type de stockage et il ajoute des éléments de langage tels que les membres attachés et les extensions de balisage. L’attribut **xml:lang** est valide en langage XAML, mais il influence l’exécution au lieu du comportement d’analyse et dispose d’un alias qui est une propriété au niveau de l’infrastructure. Pour plus d’informations, voir [**FrameworkElement.Language**](https://msdn.microsoft.com/library/windows/apps/hh702066). **xml:base** est valide dans le balisage, mais les analyseurs l’ignorent. L’attribut **xml:space** est valide, mais il est uniquement pertinent pour les scénarios décrits dans la rubrique [XAML et espace blanc](xaml-and-whitespace.md). L’attribut **encoding** est valide en langage XAML. Seuls les codages UTF-8 et UTF-16 sont pris en charge. Le codage UTF-32 n’est pas pris en charge.

###  <a name="case-sensitivity-in-xaml"></a>Respect de la casse en langage XAML

Le langage XAML respecte la casse. Il s’agit d’une autre conséquence du fait que le langage XAML soit basé sur le langage XML, lequel respecte la casse. Les noms des éléments et attributs XAML respectent la casse. La valeur d’un attribut respecte potentiellement la casse ; cela dépend de la manière dont la valeur d’attribut est gérée pour des propriétés particulières. Par exemple, si la valeur d’attribut déclare un nom de membre d’une énumération, le comportement intégré qui convertit le type d’une chaîne de nom de membre afin de renvoyer la valeur du membre de l’énumération ne tient pas compte de la casse. Par opposition, la valeur de la propriété **Name** et les méthodes utilitaires permettant d’utiliser des objets en fonction du nom que la propriété **Name** déclare traitent la chaîne du nom en tenant compte de la casse.

## <a name="xaml-namescopes"></a>Namescopes XAML

Le langage XAML définit un concept de namescope XAML. Le concept de namescope XAML influence la manière dont les processeurs XAML doivent traiter la valeur de **x:Name** ou **Name** appliquée aux éléments XAML, en particulier les étendues dans lesquelles les noms doivent être des identificateurs uniques fiables. Pour plus d’informations sur les namescopes XAML, voir [Namescopes XAML](xaml-namescopes.md).

## <a name="the-role-of-xaml-in-the-development-process"></a>Rôle du langage XAML dans le processus de développement

Le langage XAML assume plusieurs rôles importants dans le processus de développement d’application.

-   Il s’agit du principal format pour la déclaration de l’interface utilisateur d’une application et des éléments de cette interface, si vous programmez en C#, Visual Basic ou C++/CX. En général, au moins un fichier XAML dans votre projet représente une métaphore de page dans votre application pour l’interface utilisateur affichée initialement. D’autres fichiers XAML peuvent déclarer des pages supplémentaires pour l’interface utilisateur de navigation. D’autres encore peuvent déclarer des ressources telles que des modèles ou des styles.
-   On utilise le format XAML pour déclarer des styles et des modèles appliqués aux contrôles et à l’interface utilisateur d’une application.
-   Vous pouvez utiliser des styles et des modèles pour écrire des modèles de contrôles existants ou si vous utilisez un contrôle qui fournit un modèle par défaut dans le cadre d’un package de contrôle. Lorsque vous l’utilisez pour définir des styles et des modèles, le code XAML pertinent est souvent déclaré sous forme de fichier XAML discret avec une racine [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794).
-   XAML est le format courant pour la prise en charge de la création d’interface utilisateur d’application et l’échange de la conception d’interface utilisateur entre différentes applications de conception. Plus spécifiquement, le code XAML de l’application peut être partagé par différents outils de conception XAML (ou fenêtres de conception au sein d’outils).
-   Plusieurs autres technologies définissent également l’interface utilisateur de base en XAML. En ce qui concerne le code XAML Windows Presentation Foundation (WPF) et le code XAML Microsoft Silverlight, le code XAML pour Windows Runtime utilise le même URI pour son espace de noms XAML par défaut partagé. Le vocabulaire XAML pour Windows Runtime recoupe considérablement le vocabulaire XAML pour interface utilisateur également utilisé par Silverlight et, dans une moindre mesure, par WPF. Ainsi, XAML assure la promotion d’un itinéraire de migration efficace pour une interface utilisateur initialement définie pour des technologies d’avant-garde qui utilisaient également XAML.
-   XAML définit l’apparence visuelle d’une interface utilisateur et un fichier code-behind associé définit la logique. Vous pouvez ajuster la conception de l’interface utilisateur sans modifier la logique dans le code-behind. XAML simplifie le flux de travail entre les concepteurs et les développeurs.
-   Grâce à la richesse de la prise en charge de l’aire de conception et du concepteur visuel pour le langage XAML, XAML prend en charge un prototypage d’interface utilisateur rapide durant les phases de développement initiales.

Votre degré d’interaction avec les fichiers XAML dépendra de votre propre rôle dans le processus de développement Le degré de votre interaction avec les fichiers XAML dépend également de votre environnement de développement, de l’utilisation éventuelle de fonctionnalités d’environnement de conception interactives telles que les boîtes à outils et les éditeurs de propriétés, ainsi que de l’étendue et de la finalité de votre application Windows Runtime. Néanmoins, il est probable que durant le développement de l’application vous modifierez un fichier XAML au niveau élément à l’aide d’un éditeur de texte ou d’un éditeur XAML. Grâce à ces informations, vous pourrez en toute confiance modifier du code XAML dans une représentation texte ou XML et conserver la validité et la finalité des déclarations de ce fichier XAML quand il sera consommé par des outils, des opérations de compilation de balisage ou la phase d’exécution de votre application Windows Runtime.

## <a name="optimize-your-xaml-for-load-performance"></a>Optimiser le chargement du code XAML

Voici certains conseils pour définir des éléments d’interface utilisateur en XAML dans un souci de performance. Nombreux de ces conseils concernent l’utilisation de ressources XAML, mais sont répertoriés ici dans la vue d’ensemble du langage XAML pour des raisons pratiques. Pour plus d’informations sur les ressources XAML, voir [Références aux ressources ResourceDictionary et XAML](https://msdn.microsoft.com/library/windows/apps/mt187273). Pour obtenir d’autres conseils relatifs aux performances, notamment sur XAML qui affiche délibérément certaines mauvaises pratiques en termes de performance que vous devrez éviter dans votre code XAML, voir [Optimiser votre balisage XAML](https://msdn.microsoft.com/library/windows/apps/mt204779).

-   Si vous utilisez souvent le même pinceau de couleur dans votre XAML, définissez un [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962) comme ressource au lieu de recourir systématiquement à une couleur nommée comme valeur d’attribut.
-   Si vous utilisez la même ressource dans plusieurs pages d’interface utilisateur, pensez à la définir dans [**Application.Resources**](https://msdn.microsoft.com/library/windows/apps/br242338) plutôt que dans chaque page. À l’inverse, si une seule page utilise une ressource spécifique, ne définissez pas celle-ci dans **Application.Resources**, mais uniquement pour la page qui en a besoin. Cette opération s’avère bénéfique à la fois pour la factorisation XAML pendant la conception de votre application et pour les performances pendant l’analyse XAML.
-   Pour les ressources empaquetées par votre application, déterminez si certaines sont inutilisées (ressource possédant une clé, mais qui n’est utilisée par aucune référence [StaticResource](staticresource-markup-extension.md) dans votre application). Supprimez-les complètement de votre code XAML avant de publier votre application.
-   Si vous utilisez des fichiers XAML distincts qui fournissent des ressources de conception ([**MergedDictionaries**](https://msdn.microsoft.com/library/windows/apps/br208801)), pensez à placer en commentaire les ressources inutilisées ou à les supprimer de ces fichiers. Même si votre processus de développement comprend un point de départ XAML commun à différentes applications ou fournit les ressources communes à toute votre application, votre application est responsable chaque fois du package des ressources XAML et potentiellement de leur chargement.
-   Ne définissez pas d’éléments d’interface utilisateur superflus pour la composition, mais utilisez les modèles de contrôle par défaut chaque fois que cela est possible (la performance du chargement de ces modèles a déjà été testée et vérifiée).
-   Utilisez des conteneurs tels que [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) afin d’éviter une présence excessive d’éléments d’interface utilisateur. En fait, ne dessinez pas le même pixel plusieurs fois. Pour plus d’informations sur les excès et savoir comment les détecter, voir [**DebugSettings.IsOverdrawHeatMapEnabled**](https://msdn.microsoft.com/library/windows/apps/hh701823).
-   Utilisez les modèles d’éléments par défaut pour [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) ou [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705) ; ceux-ci présentent une logique **Presenter** spéciale qui permet de résoudre les problèmes de performances lors de la construction de l’arborescence visuelle pour un grand nombre d’éléments de liste.

## <a name="debugging-xaml"></a>Débogage de XAML

XAML étant un langage de balisage, certaines des stratégies ordinairement utilisées à des fins de débogage dans Microsoft Visual Studio ne sont pas disponibles. Par exemple, il est impossible de définir un point d’arrêt dans un fichier XAML. Toutefois, vous pouvez recourir à d’autres techniques pour résoudre les problèmes liés aux définitions de l’interface utilisateur ou à un autre balisage XAML pendant la phase de développement de votre application.

En règle générale, quand un fichier XAML présente des problèmes, un système ou votre application déclenche une exception d’analyse XAML. Chaque fois qu’une exception d’analyse XAML se produit, le XAML chargé par l’analyseur XAML ne parvient pas à créer d’arborescence d’objet valide. Dans certains cas, l’exception d’analyse XAML n’est pas récupérable, comme, par exemple, quand le XAML représente la première « page » de votre application qui est chargée en tant que Visual racine.

Le XAML est souvent modifié dans un IDE tel que Visual Studio et l’une de ses aires de conception XAML. Il est fréquent que Visual Studio valide le contenu et l’intégrité d’une source XAML en cours de conception. Par exemple, il peut afficher des lignes ondulées dans l’éditeur de texte XAML dès que vous tapez une mauvaise valeur d’attribut, si bien que vous n’avez pas besoin de passer par une phase de compilation du XAML pour constater que quelque chose ne va pas avec votre définition de l’interface utilisateur.

Une fois que l’application s’exécute pour de bon, les erreurs d’analyse XAML éventuelles qui n’ont pas été détectées au moment de la conception sont signalées par le Common Language Runtime (CLR) en tant qu’objet [**XamlParseException**](https://msdn.microsoft.com/library/windows/apps/hh673774). Pour plus d’informations sur ce que vous pouvez effectuer avec **XamlParseException** au moment de l’exécution, voir [Gestion des exceptions pour les applications Windows Runtime en C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/dn532194).

**Remarque**les applications qui utilisent C++ / CX pour le code n’obtiennent pas l' spécifiques [**XamlParseException**](https://msdn.microsoft.com/library/windows/apps/hh673774). Toutefois, le message dans l’exception est explicite quant au fait que la source de l’erreur est liée à XAML et comprend des informations contextuelles telles que les numéros de ligne d’un fichier XAML, à l’image de l’objet **XamlParseException**.

Pour plus d’informations sur le débogage d’une application Windows Runtime, voir [Démarrer une session de débogage](https://msdn.microsoft.com/library/windows/apps/xaml/hh781607.aspx).