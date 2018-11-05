---
author: jwmsft
description: Cette rubrique décrit le système de propriétés de dépendance disponible quand vous écrivez une application Windows Runtime en C++, C# ou Visual Basic avec des définitions XAML pour l’interface utilisateur.
title: Vue d’ensemble des propriétés de dépendance
ms.assetid: AD649E66-F71C-4DAA-9994-617C886FDA7E
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5fbf6e7ee8a224a6957428fddd11a2922adbecf4
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6038519"
---
# <a name="dependency-properties-overview"></a>Vue d’ensemble des propriétés de dépendance

Cette rubrique décrit le système de propriétés de dépendance disponible quand vous écrivez une application Windows Runtime en C++, C# ou Visual Basic avec des définitions XAML pour l’interface utilisateur.

## <a name="what-is-a-dependency-property"></a>Qu’est-ce qu’une propriété de dépendance ?

Une propriété de dépendance est un type spécialisé de propriété Plus précisément, il s’agit d’une propriété dont la valeur est suivie et influencée par un système de propriétés dédié qui fait partie de Windows Runtime.

Afin de prendre en charge une propriété de dépendance, l’objet qui définit la propriété doit être un objet [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) (en d’autres termes, une classe ayant une classe de base **DependencyObject** quelque part dans son héritage). Bon nombre des types que vous utilisez pour définir l’interface utilisateur pour une application UWP avec XAML seront une sous-classe **DependencyObject** et prennent en charge les propriétés de dépendance. Toutefois, un type provenant d’un espace de noms Windows Runtime dont le nom ne comporte pas «XAML» ne prendra pas en charge les propriétés de dépendance. Ce sont des propriétés de type ordinaire qui ne présentent pas le comportement de dépendance du système de propriétés.

Le but des propriétés de dépendance est de fournir un moyen systémique pour calculer la valeur d’une propriété en fonction d’autres entrées (d’autres propriétés, événements et états qui interviennent dans une application en cours d’exécution). Il peut s’agir des entrées suivantes :

- entrée externe telle qu’une préférence utilisateur ;
- mécanismes de détermination de propriété juste-à-temps tels que la liaison de données, les animations et les tables de montage séquentiel ;
- modèles à utilisation multiples tels que ressources et styles ;
- valeurs connues par le biais de relations parent-enfant avec d’autres éléments dans l’arborescence d’objets.

Une propriété de dépendance représente ou prend en charge une fonctionnalité spécifique du modèle de programmation pour définir une application Windows Runtime avec XAML pour les extensions de composant de l’interface utilisateur et c#, Microsoft Visual Basic ou Visual c++ (C++ / CX) pour le code. Ces fonctionnalités incluent :

- Liaison de données
- Styles
- Animations dans une table de montage séquentiel
- Comportement de «PropertyChanged» (il est possible d’implémenter une propriété de dépendance afin de fournir des rappels capables de propager des modifications à d’autres propriétés de dépendance)
- Utilisation d’une valeur par défaut provenant de métadonnées de propriété
- Utilitaire système de propriétés générales tel que [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357) et recherche de métadonnées

## <a name="dependency-properties-and-windows-runtime-properties"></a>Propriétés de dépendance et propriétés Windows Runtime

Les propriétés de dépendance étendent les fonctionnalités des propriétés Windows Runtime de base en fournissant une banque de propriétés interne globale contenant toutes les propriétés de dépendance d’une application au moment de l’exécution. Il s’agit d’une solution différente du modèle standard de stockage d’une propriété avec un champ privé, qui est privé dans la classe de définition de la propriété. Vous pouvez considérer cette banque de propriétés interne comme un ensemble d’identificateurs de propriétés et de valeurs qui existent pour tout objet particulier (à condition qu’il s’agisse d’une classe [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)). Au lieu d’être identifiée par son nom, chaque propriété de la banque est identifiée par une instance de [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362). Cependant, le système de propriétés masque en grande partie ce détail d’implémentation: vous pouvez généralement accéder aux propriétés de dépendance en utilisant un nom simple (nom de propriété par programmation dans le langage du code que vous utilisez, ou un nom d’attribut quand vous écrivez du code XAML).

Le type de base qui fournit les fondations du système de propriétés de dépendance est [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356). **DependencyObject** définit des méthodes qui peuvent accéder à la propriété de dépendance, et les instances d’une classe dérivée de **DependencyObject** prennent en charge en interne le concept de banque de propriétés mentionné plus haut.

Voici un résumé de la terminologie que nous employons dans la présente documentation concernant les propriétés de dépendance:

| Terme | Description |
|------|-------------|
| Propriété de dépendance | Propriété existant sur un identificateur [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) (voir ci-dessous). Cet identificateur est généralement disponible en tant que membre statique de la classe dérivée **DependencyObject** de définition. |
| Identificateur de propriété de dépendance | Valeur de constante permettant d’identifier la propriété. Elle est généralement publique et en lecture seule. |
| Wrapper de propriété | Implémentations **get** et **set** pouvant être appelées pour une propriété Windows Runtime. Sinon, projection propre au langage de la définition d’origine. Une implémentation wrapper de propriété **get** appelle [**GetValue**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.dependencyobject.getvalue.aspx), en passant l’identificateur de propriété de dépendance approprié. |

Le wrapper de propriété n’est pas seulement pratique pour les appelants, il expose également la propriété de dépendance à tout processus, tout outil ou toute projection qui utilise des définitions Windows Runtime pour les propriétés.

L’exemple suivant définit une propriété de dépendance «IsSpinning» personnalisée telle qu’elle est définie pour C#, puis montre la relation entre l’identificateur de propriété de dépendance et le wrapper de propriété.

```csharp
// IsSpinningProperty is the dependency property identifier
// no need for info in the last PropertyMetadata parameter, so we pass null
public static readonly DependencyProperty IsSpinningProperty =
    DependencyProperty.Register(
        "IsSpinning", typeof(Boolean),
        typeof(ExampleClass), null
    );
// The property wrapper, so that callers can use this property through a simple ExampleClassInstance.IsSpinning usage rather than requiring property system APIs
public bool IsSpinning
{
    get { return (bool)GetValue(IsSpinningProperty); }
    set { SetValue(IsSpinningProperty, value); }
}
```

> [!NOTE]
> L’exemple précédent n’est pas destiné exemple pour savoir comment créer une propriété de dépendance personnalisée. Il vise à illustrer les concepts de propriété de dépendance pour toute personne qui assimile mieux des concepts d’apprentissage par le biais du code. Pour obtenir un exemple plus complet, voir [Propriétés de dépendance personnalisées](custom-dependency-properties.md).

## <a name="dependency-property-value-precedence"></a>Priorité de la valeur de la propriété de dépendance

Lorsque vous obtenez la valeur d’une propriété de dépendance, vous obtenez une valeur qui a été déterminée pour cette propriété via l’une des entrées qui participent au système de propriétés Windows Runtime. Il existe une priorité de la valeur de la propriété de dépendance selon laquelle le système de propriétés Windows Runtime peut calculer des valeurs de manière prévisible. Il est donc important que vous soyez également familiarisé avec l’ordre de priorité de base. Sinon, il peut vous arriver d’essayer de définir une propriété à un niveau de priorité tandis qu’un autre paramètre (le système, un appelant tiers, une partie de votre propre code) est en train de la définir à un autre niveau. Vous ne saurez expliquer quelle valeur de propriété est utilisée et d’où cette valeur provient.

Par exemple, les styles et modèles ont vocation à constituer un point de départ partagé pour établir des valeurs de propriété, et par conséquent les aspects d’un contrôle. Mais sur une instance de contrôle particulière, vous pouvez avoir envie de modifier sa valeur par rapport à la valeur du modèle courant, notamment en donnant à ce contrôle une couleur d’arrière-plan différent ou une chaîne de texte différente en tant que contenu. Le système de propriétés Windows Runtime considère les valeurs locales à un niveau de priorité supérieur à celui des valeurs fournies par les styles et modèles. Cela permet d’avoir un scénario dans lequel des valeurs spécifiques à l’application remplacent les modèles. Vous pouvez donc utiliser comme il vous semble les contrôles dans l’interface utilisateur de l’application.

### <a name="dependency-property-precedence-list"></a>Ordre de priorité des propriétés de dépendance

La liste suivante indique l’ordre définitif utilisé par le système de propriétés pour assigner les valeurs d’exécution d’une propriété de dépendance. La priorité la plus élevée est répertoriée en premier. Vous trouverez des explications détaillées au bas de cette liste.

1. **Valeurs animées :** animations actives, animations de l’état visuel ou animations avec un comportement [**HoldEnd**](https://msdn.microsoft.com/library/windows/apps/br210306). Pour avoir un effet pratique, l’animation d’une propriété doit être prioritaire par rapport à la valeur de base (inanimée), même si cette valeur a été définie localement.
1. **Valeur locale :** elle peut être affectée à partir du wrapper de propriété, ce qui est comparable à la définition d’un attribut ou d’un élément de propriété en XAML ou par un appel de la méthode [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) à l’aide d’une propriété d’une instance spécifique. Si vous définissez une valeur locale à l’aide d’une liaison ou d’une ressource statique, celle-ci fonctionne dans la priorité comme si une valeur locale avait été définie, et les liaisons ou références de ressources sont effacées si une nouvelle valeur locale est définie.
1. **Propriétés basées sur un modèle :** un élément en comporte s’il a été créé dans le cadre d’un modèle (à partir d’une classe [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) ou [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348)).
1. **Méthodes setter de style :** valeurs provenant d’une classe [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) au sein de styles à partir de ressources d’application ou de page.
1. **Valeur par défaut :** une propriété de dépendance peut avoir une valeur par défaut définie dans le cadre de ses métadonnées.

### <a name="templated-properties"></a>Propriétés basées sur un modèle

Les propriétés basées sur un modèle en tant qu’élément de priorité ne s’appliquent pas à toute propriété d’un élément que vous déclarez directement dans le balisage de page XAML. Le concept de propriété basée sur un modèle existe uniquement pour les objets qui sont créés quand Windows Runtime applique un modèle XAML à un élément d’interface utilisateur et définit donc ses visuels.

Toutes les propriétés qui sont définies à partir d’un modèle de contrôle ont des valeurs d’un certain type. Ces valeurs s’apparentent à un jeu étendu de valeurs par défaut pour le contrôle et sont souvent associées à des valeurs que vous pouvez réinitialiser plus tard en définissant directement les valeurs des propriétés. Ainsi, les valeurs définies par le modèle doivent pouvoir être distinguées d’une vraie valeur locale, de manière à ce que toute nouvelle valeur locale puisse la remplacer.

> [!NOTE]
> Dans certains cas, le modèle peut même remplacer des valeurs locales s’il ne parvient pas à exposer les références de l’[extension de balisage {TemplateBinding}](templatebinding-markup-extension.md) pour les propriétés définissables sur les instances. Généralement, cela se produit uniquement si la propriété n’a réellement pas vocation à être définie sur les instances, par exemple si cela est seulement pertinent dans le cas des visuels et du comportement du modèle plutôt que de la fonction visée ou logique du runtime du contrôle qui utilise le modèle.

### <a name="bindings-and-precedence"></a>Liaisons et priorité

Les opérations de liaison disposent de la priorité appropriée quelle que soit l’étendue pour laquelle elles sont utilisées. Par exemple, une [extension de balisage {Binding}](binding-markup-extension.md) appliquée à une valeur locale agit comme une valeur locale, et une [extension de balisage {TemplateBinding}](templatebinding-markup-extension.md) pour une méthode setter de propriété s’applique comme une méthode setter de style. Étant donné que les liaisons doivent patienter jusqu’au moment de l’exécution avant d’obtenir des valeurs à partir des sources de données, le processus de détermination de la priorité de la valeur d’une propriété, quelle qu’elle soit, s’étend également jusqu’au moment de l’exécution.

Non seulement les liaisons fonctionnent au même niveau de priorité qu’une valeur locale, mais elles correspondent vraiment à une valeur locale, où la liaison est l’espace réservé pour une valeur différée. Si vous avez une liaison en place pour une valeur de propriété, et si vous définissez une valeur locale sur celle-ci au moment de l’exécution, elle remplace entièrement la liaison. De même, si vous appelez la méthode [**SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244257) pour définir une liaison qui naît seulement au moment de l’exécution, vous remplacez n’importe quelle valeur locale que vous auriez plutôt appliquée en XAML ou par du code précédemment exécuté.

### <a name="storyboarded-animations-and-base-value"></a>Animations dans une table de montage séquentiel et valeur de base

Les animations de table de montage séquentiel agissent sur un concept de *valeur de base*. La valeur de base est déterminée par le système de propriétés en fonction de sa priorité, mais en omettant cette dernière étape de recherche d’animations. Par exemple, une valeur de base peut provenir d’un modèle de contrôle, ou de la définition d’une valeur locale sur une instance d’un contrôle. Dans les deux cas, l’application d’une animation remplacera cette valeur de base et appliquera la valeur animée aussi longtemps que votre animation poursuit son exécution.

Pour une propriété animée, la valeur de base peut encore avoir un impact sur le comportement de l’animation, si cette animation ne spécifie pas à la fois **From** et **To** de manière explicite, ou si l’animation rétablit la valeur de base une fois qu’elle est terminée. Dans ces cas, à la fin de l’exécution d’une animation, le reste de la priorité est à nouveau utilisée.

Toutefois, une animation qui spécifie un **To** avec un comportement [**HoldEnd**](https://msdn.microsoft.com/library/windows/apps/br210306) peut remplacer une valeur locale jusqu’à ce qu’elle soit supprimée, même lorsqu’elle apparaît visuellement arrêtée. Conceptuellement, c’est comme si cette animation était exécutée indéfiniment même si aucune animation visuelle n’est produite dans l’interface utilisateur.

Vous pouvez appliquer plusieurs animations à une seule propriété. Chacune de ces animations a peut-être été définie pour remplacer les valeurs de base provenant de différents points dans la priorité de la valeur. Cependant, toutes ces animations vont être exécutées simultanément au moment de l’exécution. Cela signifie souvent qu’elles doivent combiner leurs valeurs car chaque animation a la même influence sur la valeur. Cela dépend de la façon exacte dont sont définies les animations et du type de valeur qui est animé.

Pour plus d’informations, voir [Animations dans une table de montage séquentiel](https://msdn.microsoft.com/library/windows/apps/mt187354).

### <a name="default-values"></a>Valeurs par défaut

L’établissement de la valeur par défaut pour une propriété de dépendance dont la valeur est [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) est expliqué plus en détail dans la rubrique [Propriétés de dépendance personnalisées](custom-dependency-properties.md).

Les propriétés de dépendance ont encore des valeurs par défaut même si celles-ci n’ont pas été explicitement définies dans les métadonnées d’une propriété donnée. Tant qu’elles ne sont pas modifiées par les métadonnées, les valeurs par défaut des propriétés de dépendance Windows Runtime comportent généralement l’un des éléments suivants:

- Une propriété qui utilise un objet au moment de l’exécution ou le type **Object** (un *type de référence*) a une valeur par défaut égale à **null**. Par exemple, la propriété [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) a la valeur **null** jusqu’à ce qu’elle soit volontairement définie ou héritée.
- Une propriété qui utilise une valeur de base telle que des chiffres ou une valeur booléenne (un *type de valeur*) utilise une valeur par défaut attendue. À titre d’exemple, 0 pour les nombres entiers et à virgule flottante, **false** pour une valeur booléenne.
- Une propriété qui utilise une structure Windows Runtime est dotée d’une valeur par défaut qui est obtenue par l’appel du constructeur implicite par défaut de cette structure. Ce constructeur utilise les valeurs par défaut pour chaque champ de valeur de base de la structure. Par exemple, la valeur [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) est initialisée par défaut avec ses valeurs **X** et **Y** à 0.
- Une propriété qui utilise une énumération a une valeur par défaut du premier membre défini dans cette énumération. Vérifiez la référence des énumérations spécifiques pour connaître la valeur par défaut utilisée.
- Une propriété qui utilise une chaîne ([**System.String**](https://msdn.microsoft.com/library/windows/apps/xaml/system.string.aspx) pour .NET, [**Platform::String**](https://msdn.microsoft.com/library/windows/apps/xaml/hh755812.aspx) pour C++/CX) a la valeur par défaut d’une chaîne vide (**""**).
- En général, les propriétés de collection ne sont pas implémentées en tant que propriétés de dépendance pour les raisons exposées plus loin dans cette rubrique. Mais si vous implémentez une propriété de collection personnalisée et que vous souhaitez la définir en tant que propriété de dépendance, assurez-vous d’éviter un *singleton accidentel*, comme décrit à la fin de la rubrique [Propriétés de dépendance personnalisées](custom-dependency-properties.md).

## <a name="property-functionality-provided-by-a-dependency-property"></a>Fonctionnalités de propriété fournie par une propriété de dépendance

### <a name="data-binding"></a>Liaison de données

Vous pouvez définir la valeur d’une propriété de dépendance en appliquant une liaison de données. La liaison de données utilise la syntaxe de l’[extension de balisage {Binding}](binding-markup-extension.md) en XAML, l’[extension de balisage {x:Bind}](x-bind-markup-extension.md) ou la classe [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) dans le code. Pour une propriété liée directement aux données, la détermination de la valeur de la propriété finale est différée jusqu’au moment de l’exécution. La valeur est alors obtenue à partir d’une source de données. Le rôle joué ici par le système de propriétés de dépendance est l’activation d’un comportement d’espace réservé pour des opérations telles que le chargement de code XAML quand la valeur est encore inconnue, puis la fourniture de la valeur au moment de l’exécution par l’interaction avec le moteur de liaison de données Windows Runtime.

L’exemple suivant définit la valeur [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) d’un élément [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652), à l’aide d’une liaison en XAML. La liaison utilise un contexte de données hérité et une source de données d’objet. (Aucun des deux n’est présenté dans l’exemple réduit ; pour obtenir un exemple plus complet qui montre le contexte et la source, voir [Présentation détaillée de la liaison de données](https://msdn.microsoft.com/library/windows/apps/mt210946).)

```xaml
<Canvas>
  <TextBlock Text="{Binding Team.TeamName}"/>
</Canvas>
```

Vous pouvez également établir des liaisons à l’aide de code plutôt qu’en XAML. Voir [**SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244257).

> [!NOTE]
> Liaisons comme celle-ci sont traitées comme une valeur locale à des fins de priorité de valeur de propriété de dépendance. Si vous affectez une autre valeur locale à une propriété qui contenait à l’origine une valeur [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820), vous remplacez entièrement la liaison, pas seulement la valeur de la liaison au moment de l’exécution. Les liaisons {x: Bind} sont implémentées à l’aide du code généré qui définit une valeur locale pour la propriété. Si vous définissez une valeur locale pour une propriété qui utilise {x: Bind}, cette valeur est alors remplacée à la prochaine évaluation de la liaison, par exemple lorsqu’elle observe une modification de la propriété sur son objet source.

### <a name="binding-sources-binding-targets-the-role-of-frameworkelement"></a>Sources de liaison, cibles de liaison, le rôle de FrameworkElement

Pour constituer la source d’une liaison, une propriété n’a pas besoin d’être une propriété de dépendance. Vous pouvez généralement utiliser n’importe quelle propriété en tant que source de liaison, bien que cela dépende de votre langage de programmation et que chacune comporte certains cas extrêmes. Toutefois, pour être la cible d’une [extension de balisage {Binding} ](binding-markup-extension.md) ou [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820), cette propriété doit être une propriété de dépendance. Ce n’est pas le cas pour l’extension de balisage {x: Bind} car elle utilise le code généré pour appliquer ses valeurs de liaison.

Si vous créez une liaison dans le code, notez que l’API [**SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244257) est uniquement définie pour [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706). Toutefois, vous pouvez créer plutôt une définition de liaison à l’aide de la classe [**BindingOperations**](https://msdn.microsoft.com/library/windows/apps/br209823), et ainsi référencer toute propriété [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356).

Qu’il s’agisse de code ou de XAML, n’oubliez pas que la propriété [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) est une propriété [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706). En utilisant une forme d’héritage de propriétés entre parents et enfants (habituellement établi dans le balisage XAML), le système de liaison peut résoudre une propriété **DataContext** qui existe sur un élément parent. Cet héritage peut effectuer l’évaluation même si l’objet enfant (qui possède la propriété cible) n’est pas une classe **FrameworkElement** et ne contient donc pas sa propre valeur **DataContext**. En revanche, cet élément parent (étant hérité) doit être une classe **FrameworkElement** afin de définir et contenir la propriété **DataContext**. Autrement, vous devez définir la liaison de sorte qu’elle puisse fonctionner avec une valeur **null** pour la propriété **DataContext**.

Connecter la liaison n’est pas la seule chose nécessaire dans la plupart des scénarios de liaison de données. Pour qu’une liaison unidirectionnelle ou bidirectionnelle soit efficace, la propriété source doit prendre en charge les notifications de modifications qui se propagent au système de liaison et par conséquent à la cible. Pour les sources de liaison personnalisées, cela signifie que la propriété doit être une propriété de dépendance ou que l’objet doit prendre en charge [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.componentmodel.inotifypropertychanged.aspx). Les collections doivent prendre en charge [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx). Certaines classes prennent en charge ces interfaces dans leurs implémentations afin d’être utiles en tant que classes de base pour les scénarios de liaison de données. La classe [**ObservableCollection&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/xaml/ms668604.aspx) en est un exemple. Pour plus d’informations sur la liaison de données et sa relation avec le système de propriétés, voir [Présentation détaillée de la liaison de données](https://msdn.microsoft.com/library/windows/apps/mt210946).

> [!NOTE]
> Les sources de données Microsoft .NET de types répertoriés ici prennent en charge. Les sources de données C++/CX utilisent différentes interfaces pour la notification des modifications ou le comportement susceptible d’être observé. Voir [Présentation détaillée de la liaison de données](https://msdn.microsoft.com/library/windows/apps/mt210946).

### <a name="styles-and-templates"></a>Styles et modèles

Les styles et modèles constituent deux des scénarios pour la définition de propriétés en tant que propriétés de dépendance. Les styles s’avèrent utiles pour définir les propriétés qui déterminent l’interface utilisateur de l’application. Les styles sont définis en tant que ressources en XAML, soit comme entrée dans une collection [**Resources**](https://msdn.microsoft.com/library/windows/apps/br208740), soit dans des fichiers XAML distincts tels que des dictionnaires de ressources de thème. Les styles interagissent avec le système de propriétés car ils contiennent des méthodes setter pour les propriétés. La propriété la plus importante ici est la propriété [**Control.Template**](https://msdn.microsoft.com/library/windows/apps/br209465) d’une classe [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390) : elle définit la majeure partie de l’aspect visuel et de l’état visuel d’une classe **Control**. Pour plus d’informations sur les styles et pour obtenir un exemple XAML qui définit une classe [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849) et utilise des méthodes setter, voir [Application de styles aux contrôles](https://msdn.microsoft.com/library/windows/apps/mt210950).

Les valeurs qui proviennent des styles ou des modèles sont des valeurs différées, semblables aux liaisons. Il en est ainsi pour que les utilisateurs de contrôles puissent remodéliser les contrôles ou redéfinir les styles. Et c’est pourquoi les méthodes setter de propriété dans les styles peuvent uniquement agir sur les propriétés de dépendance, pas sur les propriétés ordinaires.

### <a name="storyboarded-animations"></a>Animations dans une table de montage séquentiel

Vous pouvez animer la valeur d’une propriété de dépendance à l’aide d’une animation dans une table de montage séquentiel. Dans Windows Runtime, les animations dans une table de montage séquentiel ne sont pas simplement des décorations visuelles. Il est plus utile de penser aux animations en termes de technique de machine à états qui peut définir les valeurs des propriétés individuelles ou de toutes les propriétés et de tous les visuels d’un contrôle, et modifier ces valeurs dans le temps.

Pour être animée, la propriété cible de l’animation doit être une propriété de dépendance. En outre, pour être animée, le type de valeur de la propriété cible doit être pris en charge par l’un des types d’animation dérivés de [**Timeline**](https://msdn.microsoft.com/library/windows/apps/br210517) existants. Vous pouvez animer les valeurs de [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723), [**Double**](https://msdn.microsoft.com/library/windows/apps/system.double.aspx) et [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) soit à l’aide de l’interpolation, soit de techniques d’image clé. Vous pouvez animer la plupart des autres valeurs à l’aide d’images clés **Object** discrètes.

Lorsqu’une animation est appliquée et exécutée, la valeur animée fonctionne à un niveau de priorité supérieur à toute valeur (telle qu’une valeur locale) autrement affectée à la propriété. Les animations ont également un comportement [**HoldEnd**](https://msdn.microsoft.com/library/windows/apps/br210306) optionnel pouvant entraîner leur application aux valeurs de propriété même si elles semblent visuellement arrêtées.

Le principe de machine à états est incarné par l’utilisation d’animations dans une table de montage séquentiel dans le cadre du modèle d’état [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/br209021) pour les contrôles. Pour plus d’informations sur les animations dans une table de montage séquentiel, voir [Animations dans une table de montage](https://msdn.microsoft.com/library/windows/apps/mt187354). Pour plus d’informations sur **VisualStateManager** et sur la définition des états visuels des contrôles, voir [Animations dans une table de montage séquentiel pour les états visuels](https://msdn.microsoft.com/library/windows/apps/xaml/jj819808) ou [Modèles de contrôles](../design/controls-and-patterns/control-templates.md).

### <a name="property-changed-behavior"></a>Comportement modifié par une propriété

Le comportement modifié par une propriété est l’origine du côté « dépendance » de la terminologie liée aux propriétés de dépendance. Le maintien de valeurs valides pour une propriété quand une autre propriété peut influencer la valeur de la première propriété constitue un problème de développement difficile dans de nombreuses infrastructures. Dans le système de propriétés Windows Runtime, chaque propriété de dépendance peut spécifier un rappel qui est invoqué dès lors que sa valeur change. Ce rappel peut servir à notifier ou modifier des valeurs de propriété associées, d’une manière généralement synchrone. De nombreuses propriétés de dépendance ont un comportement modifié par une propriété. Vous pouvez également ajouter un comportement de rappel similaire à des propriétés de dépendance personnalisées, puis implémenter vos propres rappels modifiés par une propriété. Pour obtenir un exemple, voir [Propriétés de dépendance personnalisées](custom-dependency-properties.md).

Windows10 introduit la méthode [**RegisterPropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.dependencyobject.registerpropertychangedcallback.aspx). Cette méthode permet au code d’application de s’inscrire aux notifications de modification lorsque la propriété de dépendance spécifiée est modifiée sur une instance de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.dependencyobject.aspx).

### <a name="default-value-and-clearvalue"></a>Valeur par défaut et **ClearValue**

Une propriété de dépendance peut avoir une valeur par défaut définie dans le cadre de ses métadonnées. Dans le cas d’une propriété de dépendance, sa valeur par défaut ne perd pas de sa pertinence après la définition de la valeur pour la première fois. La valeur par défaut peut s’appliquer à nouveau au moment de l’exécution dès lors qu’un autre déterminant de la priorité de la valeur disparaît. (La priorité de la valeur de la propriété de dépendance est expliquée dans la section suivante.) Par exemple, vous pouvez volontairement supprimer une valeur de style ou une animation qui s’applique à une propriété, tout en souhaitant que la valeur soit une valeur par défaut raisonnable par la suite. La valeur par défaut de la propriété de dépendance peut fournir cette valeur, sans qu’il soit nécessaire de définir spécifiquement la valeur de chaque propriété dans le cadre d’une étape supplémentaire.

Vous pouvez délibérément affecter à la propriété la valeur par défaut même si vous lui avez déjà affecté une valeur locale. Pour réinitialiser une valeur en valeur par défaut, mais aussi pour activer d’autres participants en priorité qui seraient susceptibles de remplacer la valeur par défaut mais pas une valeur locale, appelez la méthode [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357) (référencez la propriété à effacer en tant que paramètre de méthode). Il n’est pas toujours souhaitable que la propriété utilise littéralement la valeur par défaut, mais la suppression de la valeur locale et le rétablissement de la valeur par défaut peuvent activer un autre élément en priorité, comme la valeur provenant d’un Style Setter dans un modèle de contrôle.

## <a name="dependencyobject-and-threading"></a>**DependencyObject** et threads

Toutes les instances de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) doivent être créées sur le thread d’interface utilisateur associé au [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) actuel qui est affiché par une application Windows Runtime. Bien qu’il soit indispensable de créer chaque **DependencyObject** sur le thread d’interface utilisateur principal, les objets sont accessibles à l’aide d’une référence de répartiteur en provenance des autres threads, via l’accès à la propriété [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br230616). Vous pouvez ensuite appeler des méthodes telles que [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) sur l’objet [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) et exécuter votre code dans les règles des restrictions de thread sur le thread d’interface utilisateur.

Les aspects relatifs aux threads de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) sont importants, car en règle générale, seul le code qui s’exécute sur le thread d’interface utilisateur peut modifier ou même lire la valeur d’une propriété de dépendance. Les problèmes de threads peuvent généralement être évités dans le code d’interface utilisateur classique qui utilise correctement les modèles **async** et les threads de travail d’arrière-plan. En règle générale, vous rencontrez des problèmes de threads relatifs à **DependencyObject** uniquement si vous définissez vos propres types **DependencyObject** et tentez de les utiliser pour des sources de données ou d’autres scénarios avec lesquels **DependencyObject** n’est pas nécessairement approprié.

## <a name="related-topics"></a>Rubriques connexes

### <a name="conceptual-material"></a>Documentation conceptuelle

- [Propriétés de dépendance personnalisées](custom-dependency-properties.md)
- [Vue d’ensemble des propriétés jointes](attached-properties-overview.md)
- [Présentation détaillée de la liaison de données](https://msdn.microsoft.com/library/windows/apps/mt210946)
- [Animations dans une table de montage séquentiel](https://msdn.microsoft.com/library/windows/apps/mt187354)
- [Création de composants Windows Runtime](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx)
- [Exemple de contrôles personnalisés et utilisateur XAML](http://go.microsoft.com/fwlink/p/?linkid=238581)

## <a name="apis-related-to-dependency-properties"></a>API associées aux propriétés de dépendance

- [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)
- [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362)

