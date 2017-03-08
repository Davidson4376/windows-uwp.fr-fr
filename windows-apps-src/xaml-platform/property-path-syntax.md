---
author: jwmsft
description: "Vous pouvez utiliser la classe PropertyPath et la syntaxe de chaîne pour instancier une valeur PropertyPath en XAML ou dans le code."
title: Syntaxe de Property-path
ms.assetid: FF3ECF47-D81F-46E3-BE01-C839E0398025
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 301f411f1911e12163ba123b93f99f5f15b5e479
ms.lasthandoff: 02/07/2017

---

# <a name="property-path-syntax"></a>Syntaxe de Property-path

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Vous pouvez utiliser la classe [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259) et la syntaxe de chaîne pour instancier une valeur **PropertyPath** en XAML ou dans le code. Les valeurs **PropertyPath** sont utilisées par la liaison de données. Une syntaxe similaire est utilisée pour cibler les animations dans une table de montage séquentiel. Dans les deux scénarios, un chemin de propriété décrit la traversée d’une ou de plusieurs relations de propriétés d’objet qui finissent par devenir une seule propriété.

Vous pouvez affecter une chaîne de chemin de propriété directement à un attribut en XAML. Vous pouvez utiliser la même syntaxe de chaîne pour construire un [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259) qui définit [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) dans du code, ou pour définir un ciblage d’animation dans du code via [**SetTargetProperty**](https://msdn.microsoft.com/library/windows/apps/br210503). Il existe deux zones de fonctionnalités distinctes dans Windows Runtime qui utilisent un chemin de propriété : la liaison de données et le ciblage d’animation. Le ciblage d’animation ne crée pas de valeurs de syntaxe Property-path sous-jacentes dans l’implémentation Windows Runtime ; il conserve l’information sous forme de chaîne. Toutefois, les concepts de traversée de propriété d’objet sont très similaires. La liaison de données et le ciblage d’animation évaluent chacun un chemin de propriété de façon légèrement différente. C’est la raison pour laquelle nous décrivons séparément la syntaxe du chemin de propriété dans chaque cas.

## <a name="property-path-for-objects-in-data-binding"></a>Chemin de propriété pour les objets de la liaison de données

Dans Windows Runtime, vous pouvez lier la valeur cible de n’importe quelle propriété de dépendance. La valeur de la propriété source d’une liaison de données n’a pas besoin d’être une propriété de dépendance ; il peut s’agir d’une propriété d’un objet métier (par exemple une classe écrite dans un langage Microsoft .NET ou en C++). L’objet source de la valeur de liaison peut également être un objet de dépendance existant déjà défini par l’application. La source peut être référencée soit par un simple nom de propriété, soit par une traversée des relations de propriétés d’objet dans le graphique d’objet de l’objet métier.

Vous pouvez lier soit une valeur de propriété individuelle, soit une propriété cible qui contient des listes ou des collections. Si votre source est une collection ou si le chemin d’accès spécifie une propriété de collection, le moteur de liaison de données fait correspondre les éléments de collection de la source à la cible de liaison, ce qui se traduit par un remplissage de [**ListBox**](https://msdn.microsoft.com/library/windows/apps/br242868) à l’aide d’une liste d’éléments provenant d’une collection de données sources, sans qu’il soit nécessaire d’anticiper les éléments spécifiques de cette collection.

### <a name="traversing-an-object-graph"></a>Traversée d’un graphique d’objet

L’élément de la syntaxe qui désigne la traversée d’une relation de propriété d’objet dans un graphique d’objet est le point (**.**). Chaque point d’une chaîne de chemin de propriété indique une division entre un objet (côté gauche du point) et une propriété de cet objet (côté droit du point). La chaîne est évaluée de gauche à droite, ce qui permet d’exécuter le code pas à pas dans plusieurs relations de propriétés d’objet. Voyons un exemple :

``` syntax
"{Binding Path=Customer.Address.StreetAddress1}"
```

Voici comment ce chemin d’accès est évalué :

1.  L’objet de contexte de données (ou un [**Source**](https://msdn.microsoft.com/library/windows/apps/br209832) spécifié par le même [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820)) donne lieu à la recherche de la propriété nommée « Customer ».
2.  L’objet qui a la valeur de la propriété « Customer » donne lieu à la recherche de la propriété nommée « Address ».
3.  L’objet qui a la valeur de la propriété « Address » donne lieu à la recherche de la propriété nommée « StreetAddress1 ».

À chacune de ces étapes, la valeur est traitée comme un objet. Le type du résultat est vérifié uniquement lorsque la liaison est appliquée à une propriété spécifique. Cet exemple échoue si « Address » est juste une valeur de chaîne qui n’indique pas quelle est la partie de la chaîne correspondant à l’adresse. En règle générale, la liaison pointe vers les valeurs de propriétés imbriquées spécifiques d’un objet métier ayant une structure d’information connue et intentionnelle.

### <a name="rules-for-the-properties-in-a-data-binding-property-path"></a>Règles des propriétés dans un chemin de propriété de liaison de données

-   Toutes les propriétés référencées par un chemin de propriété doivent être publiques dans l’objet métier source.
-   La propriété de fin (dernière propriété nommée dans le chemin d’accès) doit être publique et modifiable. Vous ne pouvez pas lier des valeurs statiques.
-   La propriété de fin doit être accessible en lecture/écriture si ce chemin d’accès est utilisé en tant qu’information de [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) pour une liaison bidirectionnelle.

### <a name="indexers"></a>Indexeurs

Un chemin de propriété pour la liaison de données peut inclure des références à des propriétés indexées. Cela permet de lier des listes/vecteurs ordonnés ou des dictionnaires/mappages. Utilisez des crochets « \[\] » pour indiquer une propriété indexée. Le contenu de ces crochets peut être soit un entier (pour la liste ordonnée), soit une chaîne sans guillemets (pour les dictionnaires). Vous pouvez également lier un dictionnaire où la clé est un entier. Vous pouvez utiliser différentes propriétés indexées dans le même chemin d’accès en séparant l’objet de la propriété à l’aide d’un point.

Prenons par exemple un objet métier qui contient une liste de « Teams » (liste ordonnée). Chacune des équipes (teams) a un dictionnaire de « Players » où chaque joueur est indexé par le nom de famille. Voici un exemple de chemin de propriété pour un joueur spécifique de la deuxième équipe : « Teams\[1\].Players\[Smith\] ». (Vous utilisez 1 pour indiquer le deuxième élément de « Teams », car la liste est indexée à partir de zéro.)

**Remarque** La prise en charge de l’indexation pour les sources de données C++ est limitée ; voir [Présentation détaillée de la liaison de données](https://msdn.microsoft.com/library/windows/apps/mt210946).

### <a name="attached-properties"></a>Propriétés jointes

Les chemins de propriété peuvent inclure des références à des propriétés jointes. Comme le nom permettant d’identifier une propriété jointe comprend déjà un point, vous devez mettre entre parenthèses le nom de la propriété jointe afin que le point ne soit pas considéré comme une étape de propriété d’objet. Par exemple, la chaîne qui permet de spécifier l’utilisation de [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/hh759773) en tant que chemin de liaison est « (Canvas.ZIndex) ». Pour plus d’informations sur les propriétés jointes, voir [Vue d’ensemble des propriétés jointes](attached-properties-overview.md).

### <a name="combining-property-path-syntax"></a>Combinaison d’éléments de la syntaxe du chemin de propriété

Vous pouvez combiner divers éléments de la syntaxe du chemin de propriété en une seule chaîne. Par exemple, vous pouvez définir un chemin de propriété qui fait référence à une propriété jointe indexée, si votre source de données contient une telle propriété.

### <a name="debugging-a-binding-property-path"></a>Débogage d’un chemin de propriété de liaison

Comme un chemin de propriété est interprété par un moteur de liaison et qu’il s’appuie sur des informations présentes uniquement au moment de l’exécution, vous devez souvent déboguer un chemin de propriété de liaison sans pouvoir compter sur la prise en charge classique au moment du design ou au moment de la compilation dans les outils de développement. Bien souvent, l’échec de la résolution d’un chemin de propriété se traduit au moment de l’exécution par une valeur vide et l’absence d’erreurs, car il s’agit du comportement de secours défini volontairement pour la résolution de la liaison. Heureusement, Microsoft Visual Studio fournit un mode de sortie du débogage qui peut isoler la partie d’un chemin de propriété spécifiant une source de liaison dont la résolution a échoué. Pour plus d’informations sur l’utilisation de cette fonctionnalité d’outil de développement, voir la section « Débogage » de la rubrique [Présentation détaillée de la liaison de données](../data-binding/data-binding-in-depth.md#debugging).

## <a name="property-path-for-animation-targeting"></a>Chemin de propriété pour le ciblage d’animation

Une animation s’appuie sur le ciblage d’une propriété de dépendance où des valeurs de table de montage séquentiel sont appliquées durant l’animation. Pour identifier l’objet dans lequel une propriété d’animation existe, l’animation cible un élément par son nom ([attribut x:Name](x-name-attribute.md)). Il est souvent nécessaire de définir un chemin de propriété qui commence par l’objet identifié en tant que [**Storyboard.TargetName**](https://msdn.microsoft.com/library/windows/apps/hh759823) et se termine par la valeur de propriété de dépendance particulière à laquelle l’animation doit s’appliquer. Ce chemin de propriété est utilisé en tant que valeur de [**Storyboard.TargetProperty**](https://msdn.microsoft.com/library/windows/apps/hh759824).

Pour plus d’informations sur la définition d’animations en XAML, voir [Animations dans une table de montage séquentiel](https://msdn.microsoft.com/library/windows/apps/mt187354).

## <a name="simple-targeting"></a>Ciblage simple

Si vous animez une propriété qui existe sur l’objet ciblé lui-même et si une animation peut être appliquée directement au type de cette propriété (au lieu d’une sous-propriété de la valeur d’une propriété), il vous suffit de nommer la propriété animée sans qualification supplémentaire. Par exemple, si vous ciblez une sous-classe de [**Shape**](https://msdn.microsoft.com/library/windows/apps/br243377) telle que [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/br243371), et si vous appliquez un [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723) animé à la propriété [**Fill**](https://msdn.microsoft.com/library/windows/apps/br243378), votre chemin de propriété peut être « Fill ».

## <a name="indirect-property-targeting"></a>Ciblage de propriété indirect

Vous pouvez animer une propriété qui représente une sous-propriété de l’objet cible. En d’autres termes, s’il existe une propriété de l’objet cible qui est un objet lui-même et si cet objet a des propriétés, vous devez définir un chemin de propriété qui explique comment exécuter le code pas à pas de cette relation de propriété d’objet. Chaque fois que vous spécifiez un objet pour lequel vous souhaitez animer une sous-propriété, mettez entre parenthèses le nom de la propriété et spécifiez cette dernière en respectant le format *typename*.*propertyname*. Par exemple, pour spécifier la valeur d’objet de la propriété [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/br208980) d’un objet cible, spécifiez « (UIElement.RenderTransform) » en tant que première étape du chemin de propriété. Il ne s’agit pas d’un chemin d’accès complet, car aucune animation ne peut s’appliquer directement à une valeur [**Transform**](https://msdn.microsoft.com/library/windows/apps/br243006). Par conséquent, pour cet exemple, vous complétez à présent le chemin de propriété afin que la propriété de fin soit une propriété de la sous-classe **Transform** pouvant être animée par une valeur **Double** : « (UIElement.RenderTransform).(CompositeTransform.TranslateX) »

## <a name="specifying-a-particular-child-in-a-collection"></a>Spécification d’un enfant particulier dans une collection

Pour spécifier un élément enfant dans une propriété de collection, vous pouvez utiliser un indexeur numérique. Utilisez des crochets « \[\] » autour de la valeur d’index de l’entier. Vous ne pouvez référencer que des listes ordonnées, pas des dictionnaires. Dans la mesure où une collection n’est pas une valeur qui peut être animée, l’utilisation d’un indexeur ne peut jamais représenter la propriété de fin d’un chemin de propriété.

Par exemple, pour indiquer que vous voulez animer la première couleur d’interruption de couleur dans un [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/br210108) appliqué à la propriété [**Background**](https://msdn.microsoft.com/library/windows/apps/br209395) d’un contrôle, voici le chemin de propriété correspondant : « (Control.Background).(GradientBrush.GradientStops)\[0\].(GradientStop.Color) ». Notez que l’indexeur n’est pas la dernière étape du chemin d’accès et que la dernière étape, en particulier, doit faire référence à la propriété [**GradientStop.Color**](https://msdn.microsoft.com/library/windows/apps/br210094) de l’élément 0 de la collection pour lui appliquer une valeur animée [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723).

## <a name="animating-an-attached-property"></a>Animation d’une propriété jointe

Il ne s’agit pas d’un scénario répandu. Toutefois, il est possible d’animer une propriété jointe, du moment que cette dernière possède une valeur de propriété qui correspond à un type d’animation. Comme le nom permettant d’identifier une propriété jointe comprend déjà un point, vous devez mettre entre parenthèses le nom de la propriété jointe afin que le point ne soit pas considéré comme une étape de propriété d’objet. Par exemple, pour spécifier l’animation de la propriété jointe [**Grid.Row**](https://msdn.microsoft.com/library/windows/apps/hh759795) d’un objet, utilisez le chemin de propriété « (Grid.Row) ».

**Remarque** Pour cet exemple, la valeur de [**Grid.Row**](https://msdn.microsoft.com/library/windows/apps/hh759795) correspond au type de propriété **Int32**. Par conséquent, vous ne pouvez pas l’animer avec une animation **Double**. À la place, définissez un [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/br210320) qui a des composants [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/br243132), où [**ObjectKeyFrame.Value**](https://msdn.microsoft.com/library/windows/apps/br210344) a la valeur d’un entier tel que « 0 » ou « 1 ».

## <a name="rules-for-the-properties-in-an-animation-targeting-property-path"></a>Règles des propriétés dans un chemin de propriété de ciblage d’animation

-   Le point de départ supposé du chemin de propriété est l’objet identifié par [**Storyboard.TargetName**](https://msdn.microsoft.com/library/windows/apps/hh759823).
-   Tous les objets et propriétés référencés dans le chemin de propriété doivent être publics.
-   La propriété de fin (dernière propriété nommée dans le chemin d’accès) doit être publique, être accessible en lecture/écriture et représenter une propriété de dépendance.
-   La propriété de fin doit avoir un type de propriété pouvant être animé par l’une des classes générales des types d’animation (animations [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723), animations **Double**, animations [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870), [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/br210320)).

## <a name="the-propertypath-class"></a>Classe PropertyPath

La classe [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259) est le type de propriété sous-jacent de [**Binding.Path**](https://msdn.microsoft.com/library/windows/apps/br209830) pour le scénario de liaison.

La plupart du temps, vous pouvez appliquer [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259) en XAML, sans recourir à la moindre ligne de code. Mais dans certains cas, vous pouvez être amené à définir un objet **PropertyPath** avec du code et l’affecter à une propriété au moment de l’exécution.

[**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259) a un constructeur [**PropertyPath(String)**](https://msdn.microsoft.com/library/windows/apps/br244261) et n’a pas de constructeur par défaut. La chaîne que vous passez à ce constructeur est une chaîne définie à l’aide de la syntaxe du chemin de propriété, comme nous l’avons expliqué précédemment. Vous utilisez également la même chaîne pour affecter [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) en tant qu’attribut XAML. La seule autre API de la classe **PropertyPath** est la propriété [**Path**](https://msdn.microsoft.com/library/windows/apps/br244260), qui est accessible en lecture seule. Vous pouvez utiliser cette propriété comme chaîne de construction pour une autre instance de **PropertyPath**.

## <a name="related-topics"></a>Rubriques connexes

* [Présentation détaillée de la liaison de données](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [Animations dans une table de montage séquentiel](https://msdn.microsoft.com/library/windows/apps/mt187354)
* [Extension de balisage {Binding}](binding-markup-extension.md)
* [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259)
* [**Liaison**](https://msdn.microsoft.com/library/windows/apps/br209820)
* [**Constructeur de liaison**](https://msdn.microsoft.com/library/windows/apps/br209825)
* [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713)


