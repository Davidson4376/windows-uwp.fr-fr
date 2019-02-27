---
description: L’extension de balisage est une alternative hautes performances à la liaison. xBind - nouveauté de Windows 10 - s’exécute en moins de temps et en utilisant moins de mémoire que Binding et prend en charge une meilleure le débogage.
title: Extension de balisage xBind
ms.assetid: 529FBEB5-E589-486F-A204-B310ACDC5C06
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 625c48e2f0fc57a4e9fd3a98acc505e01e2eb42c
ms.sourcegitcommit: ff131135248c85a8a2542fc55437099d549cfaa5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2019
ms.locfileid: "9117829"
---
# <a name="xbind-markup-extension"></a>Extension de balisage {x:Bind}

**Remarque**pour des informations générales sur l’utilisation des données de liaison dans votre application avec **{x: Bind}** (et pour une comparaison entre **{x: Bind}** et **{Binding}**), voir [liaison de données en profondeur](https://msdn.microsoft.com/library/windows/apps/mt210946).

L’extension de balisage **{x: Bind}** — nouveauté de Windows 10, est une alternative aux **{Binding}**. **{x: Bind}** s’exécute en moins de temps et en utilisant moins de mémoire que **{Binding}** et prend en charge le débogage de mieux.

Lors de la compilation du XAML, l’extension de balisage **{x: Bind}** est convertie en code qui récupère une valeur à partir d’une propriété sur une source de données et la définit sur la propriété spécifiée dans le balisage. L’objet de liaison peut éventuellement être configuré pour observer les modifications de la valeur de la propriété de source de données, et s’actualiser en fonction de ces modifications (`Mode="OneWay"`). Il peut également être configuré pour renvoyer les modifications dans sa propre valeur à la propriété source (`Mode="TwoWay"`).

Les objets de liaison créés par **{x:Bind}** et **{Binding}** sont en grande partie équivalents du point de vue fonctionnel. Toutefois, **{x:Bind}** exécute un code spécial qu’il génère au moment de la compilation, et **{Binding}** utilise une inspection d’objet runtime à usage général. Par conséquent, les liaisons **{x:Bind}** (souvent appelées des liaisons compilées) offrent des performances remarquables, valident vos expressions de liaison lors de la compilation, et prennent en charge le débogage en vous permettant de définir des points d’arrêt dans les fichiers de code générés en tant que la classe partielle pour votre page. Ces fichiers se trouvent dans votre dossier `obj`, et portent des noms tels que `<view name>.g.cs` (pour C#).

> [!TIP]
> **{x:Bind}** présente un mode par défaut de **OneTime**, à l'inverse de **{Binding}**, qui présente un mode par défaut de **OneWay**. Ce mode a été choisi pour des raisons de performances, étant donné que l'utilisation de **OneWay** génère davantage de code pour effectuer la liaison et gérer la détection de modification. Vous spécifiez explicitement un mode d'utilisation pour la liaison OneWay ou TwoWay. Vous pouvez également utiliser [x:DefaultBindMode](x-defaultbindmode-attribute.md) pour modifier le mode par défaut défini pour **{x:Bind}** pour un segment spécifique de l’arborescence du balisage. Le mode spécifié applique sur cet élément et ses enfants toute expression **{x:Bind}** qui ne spécifie pas explicitement un mode dans le cadre de la liaison.

**Exemples d’applications illustrant {x:Bind}**

-   [Exemple avec {x:Bind}](https://go.microsoft.com/fwlink/p/?linkid=619989)
-   [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame)
-   [Exemple d’éléments de base d’une interface utilisateur XAML](https://go.microsoft.com/fwlink/p/?linkid=619992)

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object property="{x:Bind}" .../>
-or-
<object property="{x:Bind propertyPath}" .../>
-or-
<object property="{x:Bind bindingProperties}" .../>
-or-
<object property="{x:Bind propertyPath, bindingProperties}" .../>
-or-
<object property="{x:Bind pathToFunction.functionName(functionParameter1, functionParameter2, ...), bindingProperties}" .../>
```

| Terme | Description |
|------|-------------|
| _propertyPath_ | Chaîne qui spécifie le chemin de propriété pour la liaison. Pour plus d’informations, voir la section [Chemin de propriété](#property-path) ci-dessous. |
| _bindingProperties_ |
| _propName_=_value_\[, _propName_=_value_\]* | Une ou plusieurs propriétés de liaison spécifiées à l’aide d’une syntaxe constituée d’une ou plusieurs paires nom/valeur. |
| _propName_ | Nom de chaîne de la propriété à définir sur l’objet de liaison. Par exemple, «Converter». |
| _value_ | Valeur à attribuer à la propriété. La syntaxe de l’argument dépend de la propriété définie. Voici un exemple d’utilisation de _propName_=_value_ dans lequel la valeur est elle-même une extension de balisage : `Converter={StaticResource myConverterClass}`. Pour plus d’informations, voir la section [Propriétés que vous pouvez définir avec {x:Bind}](#properties-that-you-can-set-with-xbind) ci-dessous. |

## <a name="examples"></a>Exemples

```XAML
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

Cet exemple de XAML utilise **{x:Bind}** avec une propriété **ListView.ItemTemplate**. Notez la déclaration d’une valeur **x:DataType**.

```XAML
  <DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

## <a name="property-path"></a>Chemin de propriété

*PropertyPath* définit **Path** pour une expression **{x:Bind}**. **Path** est un chemin de propriété spécifiant la valeur de la propriété, de la sous-propriété, du champ ou de la méthode avec lesquels vous établissez la liaison (la source). Vous pouvez mentionner explicitement le nom de propriété **Path** : `{Binding Path=...}`. Ou vous pouvez l’omettre : `{Binding ...}`.

### <a name="property-path-resolution"></a>Résolution de chemin de propriété

**{x:Bind}** n’utilise pas le **DataContext** comme source par défaut. Au lieu de cela, elle utilise le contrôle de page ou d’utilisateur proprement dit. Par conséquent, elle apparaît dans le code-behind de votre contrôle de page ou d’utilisateur pour les propriétés, champs et méthodes. Pour exposer votre modèle d’affichage à **{x:Bind}**, vous devez généralement ajouter des champs ou propriétés au code-behind de votre contrôle de page ou d’utilisateur. Les étapes dans un chemin de propriété sont délimitées par des points (.), et vous pouvez inclure plusieurs délimiteurs pour parcourir des sous-propriétés successives. Utilisez le point délimiteur quel que soit le langage de programmation utilisé pour implémenter l’objet cible de la liaison.

Par exemple : dans une page, **Text="{x:Bind Employee.FirstName}"** recherche un membre **Employee** sur la page, puis un membre **FirstName** sur l’objet renvoyé par **Employee**. Si vous liez un contrôle d’éléments à une propriété contenant des dépendances d’un employé, votre chemin de propriété pourrait être «Employee.Dependents», et le modèle d’élément du contrôle d’éléments se chargerait de l’affichage des éléments dans «Dependents».

Pour C++ / CX, **{x:Bind}** ne peut pas effectuer de liaison à des champs et propriétés privés dans la page ou le modèle de données. Vous devez avoir une propriété publique pour que la liaison soit possible. La surface d’exposition pour la liaison doit être exposée en tant que classes/interfaces CX pour que nous puissions obtenir les métadonnées pertinentes. L’attribut **\[Bindable\]** ne doit pas être nécessaire.

Avec **x:Bind**, vous n’avez pas besoin d’utiliser **ElementName=xxx** dans l’expression de liaison. Au lieu de cela, vous pouvez utiliser le nom de l’élément en tant que la première partie du chemin d’accès de la liaison, car les éléments nommés deviennent des champs à l’intérieur du contrôle de page ou d’utilisateur qui représente la source de liaison racine. 


### <a name="collections"></a>Collections

Si la source de données est une collection, un chemin de propriété peut spécifier les éléments de la collection selon leur position ou index. Par exemple, «Teams[0].Players», où le littéral «\[\]» encadre le «0» qui demande le premier élément d’une collection ayant un index de base zéro.

Pour utiliser un indexeur, le modèle doit implémenter **IList&lt;T&gt;** or **IVector&lt;T&gt;** sur le type de la propriété à indexer. Si le type de la propriété indexée prend en charge **INotifyCollectionChanged** ou **IObservableVector**, et si la liaison est OneWay ou TwoWay, il s’inscrit pour écouter les notifications de modification sur ces interfaces. La logique de détection des modifications met à jour en fonction de tous les changements de collection, même si cela n’affecte pas la valeur indexée spécifique. En effet, la logique d’écoute est commune dans toutes les instances de la collection.

Si la source de données est un dictionnaire ou une carte, un chemin de propriété peut spécifier les éléments de la collection par leur nom de chaîne. Par exemple, **&lt;TextBlock Text="{x:Bind Players\['John Smith'\]" /&gt;** recherchera dans le dictionnaire un élément nommé «John Smith». Le nom doit être entouré de guillemets simples ou doubles. Utilisez l’accent circonflexe (^) comme caractère d’échappement des guillemets dans les chaînes. Il est généralement plus simple d’utiliser d’autres guillemets que ceux utilisés dans l’attribut XAML.

Pour utiliser un indexeur de chaîne, le modèle doit implémenter **IDictionary&lt;string, T&gt;** ou **IMap&lt;string, T&gt;** sur le type de la propriété à indexer. Si le type de la propriété indexée prend en charge **IObservableMap** et que la liaison est OneWay ou TwoWay, il s’inscrit pour écouter les notifications de modification sur ces interfaces. La logique de détection des modifications met à jour en fonction de tous les changements de collection, même si cela n’affecte pas la valeur indexée spécifique. En effet, la logique d’écoute est commune dans toutes les instances de la collection.

### <a name="attached-properties"></a>Propriétés jointes

Pour effectuer une liaison à des propriétés jointes, vous devez placer les noms de classe et de propriété entre parenthèses après le point. Par exemple, **Text="{x:Bind Button22.(Grid.Row)}"**. Si la propriété n’est pas déclarée dans un espace de noms Xaml, vous devez la faire précéder d’un espace de noms xml que vous devez mapper à un espace de noms de code au début du document.

### <a name="casting"></a>Transtypage

Les liaisons compilées sont fortement typées et correspondent au type de chaque étape dans un chemin. Si le type retourné ne comprend pas le membre, il échoue lors de la compilation. Vous pouvez spécifier une conversion pour indiquer à la liaison le type réel de l’objet. Dans le cas suivant, **obj** est une propriété d’objet type, mais contient une zone de texte, de sorte que nous pouvons utiliser **Text="{x:Bind ((TextBox)obj).Text}"** ou **Text="{x:Bind obj.(TextBox.Text)}"**.
Le champ **groups3** dans **Text="{x:Bind ((data:SampleDataGroup)groups3\[0\]).Title}"** étant un dictionnaire d’objets, vous devez le convertir en **data:SampleDataGroup**. Notez l’utilisation du préfixe d’espace de noms xml **data:** pour mapper l’objet type à un espace de noms du code qui ne fait pas partie de l’espace de noms XAML par défaut.

_Remarque: la syntaxe de cast de type C# est plus souple que la syntaxe de propriété jointe. Elle est désormais recommandée._

## <a name="functions-in-binding-paths"></a>Fonctions dans les chemins de liaison

À compter de Windows10, version1607, **{x: Bind}** prend en charge l’utilisation d’une fonction comme niveau feuille du chemin de liaison. Il s’agit d’une fonctionnalité puissante pour la liaison de données qui permet à plusieurs scénarios dans le balisage. [Liaisons de fonction](../data-binding/function-bindings.md) pour plus d’informations, voir.

## <a name="event-binding"></a>Liaison d’événement

La liaison d’événement est une fonctionnalité unique pour une liaison compilée. Elle vous permet de spécifier le gestionnaire pour un événement à l’aide d’une liaison, au lieu d’une méthode dans le code-behind. Par exemple :**Click="{x:Bind rootFrame.GoForward}"**.

Pour des événements, la méthode cible ne doit pas être surchargée et doit:

- correspondre à la signature de l’événement ;
- OU ne pas avoir de paramètres ;
- OU avoir le même nombre de paramètres des types qui peuvent être assignés à partir des types des paramètres d’événement.

Dans le code-behind généré, la liaison compilés gère l’événement et le route vers la méthode sur le modèle, en évaluant le chemin de l’expression de liaison quand l’événement se produit. Cela signifie que, contrairement aux liaisons de propriété, elle ne suit pas les modifications du modèle.

Pour plus d’informations sur la syntaxe de chaîne pour un chemin de propriété, consultez la [Syntaxe de PropertyPath](property-path-syntax.md), en gardant à l’esprit les différences décrites ici pour **{x:Bind}**.

## <a name="properties-that-you-can-set-with-xbind"></a>Propriétés que vous pouvez définir avec {x:Bind}

**{x:Bind}** est illustrée avec la syntaxe de l’espace réservé *bindingProperties*, car plusieurs propriétés en lecture/écriture peuvent être définies dans l’extension de balisage. Les propriétés peuvent être définies dans n’importe quel ordre à l’aide de paires *propName*=*value* séparées par des virgules. Notez que vous ne pouvez pas inclure de saut de ligne dans l’expression de liaison. Certaines propriétés requièrent des types sans conversion de type. Elles nécessitent donc leurs propres extensions de balisage imbriquées dans l’extension de balisage **{x:Bind}**.

Ces propriétés fonctionnent essentiellement de la même manière que les propriétés de la classe [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820).

| Propriété | Description |
|----------|-------------|
| **Path** | Voir la section [Chemin de propriété](#property-path) ci-dessus. |
| **Converter** | Spécifie l’objet convertisseur appelé par le moteur de liaison. Le convertisseur peut être défini en XAML, mais uniquement si vous faites référence à une instance d’objet que vous avez assignée dans une référence d’[extension de balisage {StaticResource}](staticresource-markup-extension.md) à cet objet dans le dictionnaire de ressources. |
| **ConverterLanguage** | Spécifie la culture que doit utiliser le convertisseur. (Si vous définissez **ConverterLanguage**, vous devez également définir **Converter**.) La culture est définie comme un identificateur basé sur des normes. Pour plus d’informations, voir [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880). |
| **ConverterParameter** | Spécifie le paramètre de convertisseur qui peut être utilisé dans la logique du convertisseur. (Si vous définissez **ConverterParameter**, vous devez également définir **Converter**.) La plupart des convertisseurs utilisent une logique simple qui obtient toutes les informations de la valeur transmise à convertir et ne nécessitent pas de valeur **ConverterParameter**. Le paramètre **ConverterParameter** est destiné aux implémentations de convertisseur moyennement avancées qui comprennent plusieurs logiques basées sur ce qui est transmis dans **ConverterParameter**. Vous pouvez écrire un convertisseur qui utilise des valeurs qui ne sont pas des chaînes, mais il s’agit d’un scénario peu courant. Pour plus d’informations, voir la section Remarques dans [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827). |
| **FallbackValue** | Spécifie une valeur à afficher quand la source ou le chemin ne peuvent pas être résolus. |
| **Mode** | Spécifie le mode de liaison, sous la forme de l’une des chaînes suivantes : «OneTime», «OneWay» ou «TwoWay». La valeur par défaut est «OneTime». Notez qu’elle diffère de la valeur par défaut **{Binding}**, qui est « OneWay » dans la plupart des cas. |
| **TargetNullValue** | Spécifie une valeur à afficher quand la valeur de la source est résolue, mais est explicitement **null**. |
| **BindBack** | Spécifie une fonction à utiliser pour le sens inverse d’une liaison bidirectionnelle. |
| **UpdateSourceTrigger** | Spécifie à quel moment renvoyer les modifications du contrôle vers le modèle dans les liaisons TwoWay. La valeur par défaut pour toutes les propriétés, sauf pour TextBox.Text est PropertyChanged; TextBox.Text est LostFocus.|

> [!NOTE]
> Si vous convertissez un balisage de **{Binding}** en **{x:Bind}**, soyez attentif aux différences de valeurs par défaut de la propriété **Mode**.
 
> [**x:DefaultBindMode**](https://docs.microsoft.com/windows/uwp/xaml-platform/x-defaultbindmode-attribute) peut servir à modifier le mode par défaut défini pour x:Bind pour un segment spécifique de l’arborescence du balisage. Le mode sélectionné appliquera sur cet élément et ses enfants toute expression x:Bind qui ne spécifie pas explicitement un mode dans le cadre de la liaison. OneTime est plus performante que OneWay. En effet, l'utilisation de OneWay provoque plus de code à générer pour le raccordement et la gestion de la détection des modifications.

## <a name="remarks"></a>Notes

Dans la mesure où l’extension de balisage **{x:Bind}** utilise un code généré pour obtenir ses avantages, elle nécessite des informations de type au moment de la compilation. Cela signifie que vous ne pouvez pas effectuer de liaison à des propriétés quand vous ne connaissez pas le type à l’avance. Pour cette raison, vous ne pouvez pas utiliser **{x:Bind}** avec la propriété **DataContext**, qui est du type **Object**, et est également sujette à modification au moment de l’exécution.

Lorsque vous utilisez **{x: Bind}** avec des modèles de données, vous devez indiquer le type cible de la liaison en définissant une valeur **x: DataType** , comme illustré dans la section [exemples](#examples) . Vous pouvez également définir le type sur une interface ou un type de classe de base, puis utiliser des conversions si nécessaire pour formuler une expression complète.

Les liaisons compilées dépendent de la génération du code. Par conséquent, si vous utilisez **{x:Bind}** dans un dictionnaire de ressources, ce dernier doit comporter une classe code-behind. Pour un exemple de code, voir [Dictionnaires de ressources avec {x:Bind}](../data-binding/data-binding-in-depth.md#resource-dictionaries-with-x-bind).

Le code généré des pages et des contrôles utilisateur incluant des liaisons compilées contiendra une propriété «Bindings». Celle-ci comprend les méthodes suivantes:

- **Update()** - Met à jour les valeurs de toutes les liaisons compilées. Toutes les liaisons unidirectionnelles/bidirectionnelles contiennent des écouteurs afin de détecter les modifications.
- **Initialize()** - Si les liaisons n’ont pas encore été initialisées, appelle Update() pour initialiser les liaisons.
- **StopTracking()** - Déconnecte tous les écouteurs créés pour les liaisons uni- et bidirectionnelles. Elles peuvent être réinitialisées à l’aide de la méthode Update().

> [!NOTE]
> Depuis Windows10, version1607, l’infrastructure XAML fournit un convertisseur intégré permettant de convertir un booléen en Visibility. Le convertisseur mappe **true** à la valeur d’énumération **Visible**et **false** à la valeur d’énumération **Collapsed**. Vous pouvez ainsi lier une propriété Visibility à un booléen sans avoir à créer un convertisseur. Notez qu'il ne s'agit pas d'une fonctionnalité de liaison des fonctions, mais uniquement de liaison des propriétés. Pour utiliser le convertisseur intégré, la version du SDK cible de votre application doit être 14393 ou une version ultérieure. Vous ne pouvez pas l’utiliser si votre application cible des versions antérieures de Windows10. Pour plus d’informations sur les versions cibles, voir [Code adaptatif de version](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

**Conseil**  si vous avez besoin de spécifier une accolade simple pour une valeur, comme dans le [**chemin d’accès**](https://msdn.microsoft.com/library/windows/apps/br209830) ou [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827), faites-la précéder d’une barre oblique inverse: `\{`. Vous pouvez également placer l’ensemble de la chaîne qui contient les accolades à échapper dans une paire de guillemets secondaire. Par exemple : `ConverterParameter='{Mix}'`.

[**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826), [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) et **ConverterLanguage** sont tous liés au scénario de conversion d’une valeur ou d’un type de la source de liaison en type ou valeur compatible avec la propriété cible de liaison. Pour obtenir plus d’informations et des exemples, voir la section « Conversions de données » de la rubrique [Présentation détaillée de la liaison de données](https://msdn.microsoft.com/library/windows/apps/mt210946).

**{x:Bind}** est uniquement une extension de balisage. Il n’existe aucun moyen de créer ou manipuler de telles liaisons par programmation. Pour plus d’informations sur les extensions de balisage, voir [Vue d’ensemble du langage XAML](xaml-overview.md).

