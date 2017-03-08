---
author: jwmsft
description: "Au lieu de recourir à la méthode Binding, vous pouvez utiliser l’extension de balisage xBind. Celle-ci n’offre pas certaines des fonctionnalités de Binding, mais elle s’exécute en moins de temps et en utilisant moins de mémoire que Binding, et prend mieux en charge le débogage."
title: Extension de balisage xBind
ms.assetid: 529FBEB5-E589-486F-A204-B310ACDC5C06
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: a82cb66c66b593c0241a651e4df34e3998a106c6
ms.lasthandoff: 02/07/2017

---

# <a name="xbind-markup-extension"></a>Extension de balisage {x:Bind}

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

**Remarque**  Pour plus d’informations sur l’utilisation de la liaison de données dans votre application avec **{x:Bind}** (et pour une comparaison entre **{x:Bind}** et **{Binding}**), voir [Présentation détaillée de la liaison de données](https://msdn.microsoft.com/library/windows/apps/mt210946).

L’extension de balisage **{x:Bind}**, nouveauté de Windows 10, peut être utilisée en remplacement de la méthode **{Binding}**. L’extension de balisage **{x:Bind}** n’offre pas certaines des fonctionnalités de **{Binding}**, mais elle s’exécute en moins de temps et en utilisant moins de mémoire que **{Binding}**, et prend mieux en charge le débogage.

Lors de la compilation du XAML, l’extension de balisage **{x : Bind}** est convertie en code qui récupère une valeur à partir d’une propriété sur une source de données et la définit sur la propriété spécifiée dans le balisage. L’objet de liaison peut éventuellement être configuré pour observer les modifications de la valeur de la propriété de source de données, et s’actualiser en fonction de ces modifications. Il peut également être configuré pour renvoyer les modifications dans sa propre valeur à la propriété source. Les objets de liaison créés par **{x:Bind}** et **{Binding}** sont en grande partie équivalents du point de vue fonctionnel. Toutefois, **{x:Bind}** exécute un code spécial qu’il génère au moment de la compilation, et **{Binding}** utilise une inspection d’objet runtime à usage général. Par conséquent, les liaisons **{x:Bind}** (souvent appelées des liaisons compilées) offrent des performances remarquables, valident vos expressions de liaison lors de la compilation, et prennent en charge le débogage en vous permettant de définir des points d’arrêt dans les fichiers de code générés en tant que la classe partielle pour votre page. Ces fichiers se trouvent dans votre dossier `obj`, et portent des noms tels que `<view name>.g.cs` (pour C#).

**Exemples d’applications illustrant {x:Bind}**

-   [Exemple avec {x:Bind}](http://go.microsoft.com/fwlink/p/?linkid=619989)
-   [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame)
-   [Exemple d’éléments de base d’une interface utilisateur XAML](http://go.microsoft.com/fwlink/p/?linkid=619992)

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object property="{x:Bind}" .../>
-or-
<object property="{x:Bind propertyPath}" .../>
-or-
<object property="{x:Bind bindingProperties}" .../>
-or-
<object property="{x:Bind propertyPath, bindingProperties}" .../>
```

| Terme | Description |
|------|-------------|
| _propertyPath_ | Chaîne qui spécifie le chemin de propriété pour la liaison. Pour plus d’informations, voir la section [Chemin de propriété](#property-path) ci-dessous. |
| _bindingProperties_ |
| _propName_=_value_\[, _propName_=_value_\]* | Une ou plusieurs propriétés de liaison spécifiées à l’aide d’une syntaxe constituée d’une ou plusieurs paires nom/valeur. |
| _propName_ | Nom de chaîne de la propriété à définir sur l’objet de liaison. Par exemple, « Converter ». |
| _value_ | Valeur à attribuer à la propriété. La syntaxe de l’argument dépend de la propriété définie. Voici un exemple d’utilisation de _propName_=_value_ dans lequel la valeur est elle-même une extension de balisage : `Converter={StaticResource myConverterClass}`. Pour plus d’informations, voir la section [Propriétés que vous pouvez définir avec {x:Bind}](#properties-you-can-set) ci-dessous. | 

## <a name="property-path"></a>Chemin de propriété

*PropertyPath* définit **Path** pour une expression **{x:Bind}**. **Path** est un chemin de propriété spécifiant la valeur de la propriété, de la sous-propriété, du champ ou de la méthode avec lesquels vous établissez la liaison (la source). Vous pouvez mentionner explicitement le nom de propriété **Path** : `{Binding Path=...}`. Ou vous pouvez l’omettre : `{Binding ...}`.

### <a name="property-path-resolution"></a>Résolution de chemin de propriété

**{x:Bind}** n’utilise pas le **DataContext** comme source par défaut. Au lieu de cela, elle utilise le contrôle de page ou d’utilisateur proprement dit. Par conséquent, elle apparaît dans le code-behind de votre contrôle de page ou d’utilisateur pour les propriétés, champs et méthodes. Pour exposer votre modèle d’affichage à **{x:Bind}**, vous devez généralement ajouter des champs ou propriétés au code-behind de votre contrôle de page ou d’utilisateur. Les étapes dans un chemin de propriété sont délimitées par des points (.), et vous pouvez inclure plusieurs délimiteurs pour parcourir des sous-propriétés successives. Utilisez le point délimiteur quel que soit le langage de programmation utilisé pour implémenter l’objet cible de la liaison.

Par exemple : dans une page, **Text="{x:Bind Employee.FirstName}"** recherche un membre **Employee** sur la page, puis un membre **FirstName** sur l’objet renvoyé par **Employee**. Si vous liez un contrôle d’éléments à une propriété contenant des dépendances d’un employé, votre chemin de propriété pourrait être « Employee.Dependents », et le modèle d’élément du contrôle d’éléments se chargerait de l’affichage des éléments dans « Dependents ».

Pour C++ / CX, **{x:Bind}** ne peut pas effectuer de liaison à des champs et propriétés privés dans la page ou le modèle de données. Vous devez avoir une propriété publique pour que la liaison soit possible. La surface d’exposition pour la liaison doit être exposée en tant que classes/interfaces CX pour que nous puissions obtenir les métadonnées pertinentes. L’attribut **\[Bindable\]** ne doit pas être nécessaire.

Avec **x:Bind**, vous n’avez pas besoin d’utiliser **ElementName=xxx** dans l’expression de liaison. Avec **x:Bind**, vous pouvez utiliser le nom de l’élément comme première partie du chemin pour la liaison, car les éléments nommés deviennent des champs à l’intérieur du contrôle de page ou d’utilisateur qui représente la source de liaison racine.

### <a name="collections"></a>Collections

Si la source de données est une collection, un chemin de propriété peut spécifier les éléments de la collection selon leur position ou index. Par exemple, « Teams[0].Players », où le littéral « \[\] » encadre le « 0 » qui demande le premier élément d’une collection ayant un index de base zéro.

Pour utiliser un indexeur, le modèle doit implémenter **IList&lt;T&gt;** or **IVector&lt;T&gt;** sur le type de la propriété à indexer. Si le type de la propriété indexée prend en charge **INotifyCollectionChanged** ou **IObservableVector**, et si la liaison est OneWay ou TwoWay, il s’inscrit pour écouter les notifications de modification sur ces interfaces. La logique de détection des modifications met à jour en fonction de tous les changements de collection, même si cela n’affecte pas la valeur indexée spécifique. En effet, la logique d’écoute est commune dans toutes les instances de la collection.

Si la source de données est un dictionnaire ou une carte, un chemin de propriété peut spécifier les éléments de la collection par leur nom de chaîne. Par exemple, **&lt;TextBlock Text="{x:Bind Players\['John Smith'\]" /&gt;** recherchera dans le dictionnaire un élément nommé « John Smith ». Le nom doit être entouré de guillemets simples ou doubles. Utilisez l’accent circonflexe (^) comme caractère d’échappement des guillemets dans les chaînes. Il est généralement plus simple d’utiliser d’autres guillemets que ceux utilisés dans l’attribut XAML.

Pour utiliser un indexeur de chaîne, le modèle doit implémenter **IDictionary&lt;string, T&gt;** ou **IMap&lt;string, T&gt;** sur le type de la propriété à indexer. Si le type de la propriété indexée prend en charge **IObservableMap** et que la liaison est OneWay ou TwoWay, il s’inscrit pour écouter les notifications de modification sur ces interfaces. La logique de détection des modifications met à jour en fonction de tous les changements de collection, même si cela n’affecte pas la valeur indexée spécifique. En effet, la logique d’écoute est commune dans toutes les instances de la collection.

### <a name="attached-properties"></a>Propriétés jointes

Pour effectuer une liaison à des propriétés jointes, vous devez placer les noms de classe et de propriété entre parenthèses après le point. Par exemple, **Text="{x:Bind Button22.(Grid.Row)}"**. Si la propriété n’est pas déclarée dans un espace de noms Xaml, vous devez la faire précéder d’un espace de noms xml que vous devez mapper à un espace de noms de code au début du document.

### <a name="casting"></a>Transtypage

Les liaisons compilées sont fortement typées et correspondent au type de chaque étape dans un chemin. Si le type retourné ne comprend pas le membre, il échoue lors de la compilation. Vous pouvez spécifier une conversion pour indiquer à la liaison le type réel de l’objet. Dans le cas suivant, **obj** est une propriété d’objet type, mais contient une zone de texte, de sorte que nous pouvons utiliser **Text="{x:Bind ((TextBox)obj).Text}"** ou **Text="{x:Bind obj.(TextBox.Text)}"**.
Le champ **groups3** dans **Text="{x:Bind ((data:SampleDataGroup)groups3\[0\]).Title}"** étant un dictionnaire d’objets, vous devez le convertir en **data:SampleDataGroup**. Notez l’utilisation du préfixe d’espace de noms xml **data:** pour mapper l’objet type à un espace de noms du code qui ne fait pas partie de l’espace de noms XAML par défaut.

_Remarque : la syntaxe de cast de type C# est plus souple que la syntaxe de propriété jointe. Elle est désormais recommandée._

## <a name="functions-in-binding-paths"></a>Fonctions dans les chemins de liaison

À compter de Windows 10, version 1607, **{x : Bind}** prend en charge l’utilisation d’une fonction comme niveau feuille du chemin de liaison. Cela offre les avantages suivants :
- Facilite la conversion de valeur
- Permet aux liaisons de dépendre de plus d’un paramètre

> [!NOTE]
> Pour utiliser des fonctions avec **{x:Bind}**, la version du SDK cible de votre application doit être 14393 ou une version ultérieure. Vous ne pouvez pas utiliser des fonctions si votre application cible des versions antérieures de Windows 10. Pour plus d’informations sur les versions cibles, voir [Code adaptatif de version](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

Dans l’exemple suivant, l’arrière-plan et le premier plan de l’élément sont liés à des fonctions de conversion reposant sur le paramètre de couleur :

``` Xamlmarkup
<DataTemplate x:DataType="local:ColorEntry">
    <Grid Background="{x:Bind Brushify(Color)}" Width="240">
        <TextBlock Text="{x:Bind ColorName}" Foreground="{x:Bind TextColor(Color)}" Margin="10,5" />
    </Grid>
</DataTemplate>
```
``` C#
class ColorEntry
{
    public string ColorName { get; set; }
    public Color Color { get; set; }

    public static SolidColorBrush Brushify(Color c)
    {
        return new SolidColorBrush(c);
    }

    public static SolidColorBrush TextColor(Color c)
    {
        return new SolidColorBrush(((c.R * 0.299 + c.G * 0.587 + c.B * 0.114) > 150) ? Colors.Black : Colors.White);
    }
}

```
### <a name="function-syntax"></a>Syntaxe de la fonction
``` Syntax
Text="{x:Bind MyModel.Order.CalculateShipping(MyModel.Order.Weight, MyModel.Order.ShipAddr.Zip, 'Contoso'), Mode=OneTime}"
             |      Path to function         |    Path argument   |       Path argument       | Const arg |  Bind Props
```

### <a name="path-to-the-function"></a>Chemin de la fonction
Le chemin de la fonction est spécifié comme tout autre chemin de propriété et peut inclure des points (.), des indexeurs ou des casts pour localiser la fonction.

Des fonctions statiques peuvent être spécifiées en utilisant la syntaxe XMLNamespace:ClassName.MethodName. Par exemple, **&lt;CalendarDatePicker Date="\{x:Bind sys:DateTime.Parse(TextBlock1.Text)\}" /&gt;** sera mappé à la fonction DateTime.Parse, en supposant que **xmlns:sys="using:System"** soit spécifié en haut de la page.

Si le mode est OneWay/TwoWay, la détection de modification est réalisée sur le chemin de la fonction et la liaison est réévaluée si des modifications sont apportées à ces objets.

La fonction en cours de liaison doit :
- Être accessible par le code et les métadonnées (donc travail interne/privée dans C#), mais C++/CX aura besoin de méthodes qui soient des méthodes WinRT publiques.
- La surcharge repose sur le nombre d’arguments, pas sur le type ; la fonction essaiera de trouver une correspondance avec la première surcharge présentant ces nombreux arguments.
- Les types d’arguments doivent correspondre aux données transmises. Nous ne faisons pas de conversions restrictives.
- Le type de retour de la fonction doit correspondre au type de la propriété qui utilise la liaison.


### <a name="function-arguments"></a>Arguments de la fonction
Plusieurs arguments peuvent être spécifiés dans la fonction. Ils sont séparés par une virgule (,).
- Chemin de liaison. Même syntaxe que si vous établissiez une liaison directement à cet objet.
  - Si le mode est OneWay/TwoWay, la détection de modification sera réalisée et la liaison réévaluée lors des modifications de l’objet.
- Chaîne de constante entourée de guillemets (les guillemets sont nécessaires pour la désigner comme chaîne). L’accent circonflexe (^) peut être utilisé comme caractère d’échappement des guillemets dans les chaînes.
- Numéro de constante. Par exemple, 123.456
- Valeur booléenne. Sous la forme « x : True » ou « x : False »

### <a name="two-way-function-bindings"></a>Liaisons de fonctions bidirectionnelles
Dans un scénario de liaison bidirectionnelle, une deuxième fonction doit être spécifiée pour la direction inverse de la liaison. Cette opération est réalisée à l’aide de la propriété de liaison **BindBack**, par exemple **Text="\{x:Bind a.MaFonc(b), BindBack=a.MaFonc2\}"**. La fonction doit prendre un seul argument, qui est la valeur qui doit être transmises en retour au modèle.

## <a name="event-binding"></a>Liaison d’événement

La liaison d’événement est une fonctionnalité unique pour une liaison compilée. Elle vous permet de spécifier le gestionnaire pour un événement à l’aide d’une liaison, au lieu d’une méthode dans le code-behind. Par exemple :**Click="{x:Bind rootFrame.GoForward}"**.

Pour des événements, la méthode cible ne doit pas être surchargée et doit :

-   correspondre à la signature de l’événement ;
-   OU ne pas avoir de paramètres ;
-   OU avoir le même nombre de paramètres des types qui peuvent être assignés à partir des types des paramètres d’événement.

Dans le code-behind généré, la liaison compilés gère l’événement et le route vers la méthode sur le modèle, en évaluant le chemin de l’expression de liaison quand l’événement se produit. Cela signifie que, contrairement aux liaisons de propriété, elle ne suit pas les modifications du modèle.

Pour plus d’informations sur la syntaxe de chaîne pour un chemin de propriété, consultez la [Syntaxe de PropertyPath](property-path-syntax.md), en gardant à l’esprit les différences décrites ici pour **{x:Bind}**.

##  <a name="properties-that-you-can-set-with-xbind"></a>Propriétés que vous pouvez définir avec {x:Bind}


**{x:Bind}** est illustrée avec la syntaxe de l’espace réservé *bindingProperties*, car plusieurs propriétés en lecture/écriture peuvent être définies dans l’extension de balisage. Les propriétés peuvent être définies dans n’importe quel ordre à l’aide de paires *propName*=*value* séparées par des virgules. Notez que vous ne pouvez pas inclure de saut de ligne dans l’expression de liaison. Certaines propriétés requièrent des types sans conversion de type. Elles nécessitent donc leurs propres extensions de balisage imbriquées dans l’extension de balisage **{x:Bind}**.

Ces propriétés fonctionnent essentiellement de la même manière que les propriétés de la classe [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820).

| Propriété | Description |
|----------|-------------|
| **Path** | Voir la section [Chemin de propriété](#property-path) ci-dessus. |
| **Converter** | Spécifie l’objet convertisseur appelé par le moteur de liaison. Le convertisseur peut être défini en XAML, mais uniquement si vous faites référence à une instance d’objet que vous avez assignée dans une référence d’[extension de balisage {StaticResource}](staticresource-markup-extension.md) à cet objet dans le dictionnaire de ressources. |
| **ConverterLanguage** | Spécifie la culture que doit utiliser le convertisseur. (Si vous définissez **ConverterLanguage**, vous devez également définir **Converter**.) La culture est définie comme un identificateur basé sur des normes. Pour plus d’informations, voir [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880). |
| **ConverterParameter** | Spécifie le paramètre de convertisseur qui peut être utilisé dans la logique du convertisseur. (Si vous définissez **ConverterParameter**, vous devez également définir **Converter**.) La plupart des convertisseurs utilisent une logique simple qui obtient toutes les informations de la valeur transmise à convertir et ne nécessitent pas de valeur **ConverterParameter**. Le paramètre **ConverterParameter** est destiné aux implémentations de convertisseur moyennement avancées qui comprennent plusieurs logiques basées sur ce qui est transmis dans **ConverterParameter**. Vous pouvez écrire un convertisseur qui utilise des valeurs qui ne sont pas des chaînes, mais il s’agit d’un scénario peu courant. Pour plus d’informations, voir la section Remarques dans [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827). |
| **FallbackValue** | Spécifie une valeur à afficher quand la source ou le chemin ne peuvent pas être résolus. |
| **Mode** | Spécifie le mode de liaison, sous la forme de l’une des chaînes suivantes : « OneTime », « OneWay » ou « TwoWay ». La valeur par défaut est « OneTime ». Notez qu’elle diffère de la valeur par défaut **{Binding}**, qui est « OneWay » dans la plupart des cas. |
| **TargetNullValue** | Spécifie une valeur à afficher quand la valeur de la source est résolue, mais est explicitement **null**. |
| **BindBack** | Spécifie une fonction à utiliser pour le sens inverse d’une liaison bidirectionnelle. | 

**Remarque**  Si vous convertissez un balisage de **{Binding}** en **{x:Bind}**, soyez attentif aux différences de valeur par défaut de la propriété **Mode**.
 
## <a name="remarks"></a>Remarques

Dans la mesure où l’extension de balisage **{x:Bind}** utilise un code généré pour obtenir ses avantages, elle nécessite des informations de type au moment de la compilation. Cela signifie que vous ne pouvez pas effectuer de liaison à des propriétés quand vous ne connaissez pas le type à l’avance. Pour cette raison, vous ne pouvez pas utiliser **{x:Bind}** avec la propriété **DataContext**, qui est du type **Object**, et est également sujette à modification au moment de l’exécution.

Lorsque vous utilisez **{x:Bind}** avec des modèles de données, vous devez indiquer le type cible de la liaison en définissant une valeur **x:DataType**, comme illustré dans l’exemple ci-dessous. Vous pouvez également définir le type sur une interface ou un type de classe de base, puis utiliser des conversions si nécessaire pour formuler une expression complète.

Les liaisons compilées dépendent de la génération du code. Par conséquent, si vous utilisez **{x:Bind}** dans un dictionnaire de ressources, ce dernier doit comporter une classe code-behind. Pour un exemple de code, voir [Dictionnaires de ressources avec {x:Bind}](../data-binding/data-binding-in-depth.md#resource-dictionaries-with-x-bind).

Le code généré des pages et des contrôles utilisateur incluant des liaisons compilées contiendra une propriété « Bindings ». Celle-ci comprend les méthodes suivantes :
- **Update()** - Met à jour les valeurs de toutes les liaisons compilées. Toutes les liaisons unidirectionnelles/bidirectionnelles contiennent des écouteurs afin de détecter les modifications.
- **Initialize()** - Si les liaisons n’ont pas encore été initialisées, appelle Update() pour initialiser les liaisons.
- **StopTracking()** - Déconnecte tous les écouteurs créés pour les liaisons uni- et bidirectionnelles. Elles peuvent être réinitialisées à l’aide de la méthode Update().

> [!NOTE]
> Depuis Windows 10, version 1607, l’infrastructure XAML fournit un convertisseur intégré permettant de convertir un booléen en Visibility. Le convertisseur mappe **true** à la valeur d’énumération **Visible**et **false** à la valeur d’énumération **Collapsed**. Vous pouvez ainsi lier une propriété Visibility à un booléen sans avoir à créer un convertisseur. Pour utiliser le convertisseur intégré, la version du SDK cible de votre application doit être 14393 ou une version ultérieure. Vous ne pouvez pas l’utiliser si votre application cible des versions antérieures de Windows 10. Pour plus d’informations sur les versions cibles, voir [Code adaptatif de version](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

**Conseil**   Si vous avez besoin de spécifier une accolade simple pour une valeur, comme dans [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) ou [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827), faites-la précéder d’une barre oblique inverse : `\{`. Vous pouvez également placer l’ensemble de la chaîne qui contient les accolades à échapper dans une paire de guillemets secondaire. Par exemple : `ConverterParameter='{Mix}'`.

[**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826), [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) et **ConverterLanguage** sont tous liés au scénario de conversion d’une valeur ou d’un type de la source de liaison en type ou valeur compatible avec la propriété cible de liaison. Pour obtenir plus d’informations et des exemples, voir la section « Conversions de données » de la rubrique [Présentation détaillée de la liaison de données](https://msdn.microsoft.com/library/windows/apps/mt210946).

**{x:Bind}** est uniquement une extension de balisage. Il n’existe aucun moyen de créer ou manipuler de telles liaisons par programmation. Pour plus d’informations sur les extensions de balisage, voir [Vue d’ensemble du langage XAML](xaml-overview.md).

## <a name="examples"></a>Exemples

```XML
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

Cet exemple de XAML utilise **{x:Bind}** avec une propriété **ListView.ItemTemplate**. Notez la déclaration d’une valeur **x:DataType**.

```XML
  <DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

