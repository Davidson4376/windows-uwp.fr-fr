---
Fournit un moyen de spécifier la source d’une liaison en termes de relation relative dans le graphique d’objet au moment de l’exécution.
Extension de balisage RelativeSource
ms.assetid: B87DEF36-BE1F-4C16-B32E-7A896BD09272
---

# Extension de balisage {RelativeSource}

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Fournit un moyen de spécifier la source d’une liaison en termes de relation relative dans le graphique d’objet au moment de l’exécution.

## Utilisation des attributs XAML (mode Self)

``` syntax
<Binding RelativeSource="{RelativeSource Self}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource Self} ...}" .../>
```

## Utilisation des attributs XAML (mode TemplatedParent)

``` syntax
<Binding RelativeSource="{RelativeSource TemplatedParent}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource TemplatedParent} ...}" .../>
```

## Valeurs XAML

| Terme | Description |
| {RelativeSource Self} | La valeur [<strong>Mode</strong>](https://msdn.microsoft.com/library/windows/apps/br209915) est <strong>Self</strong>. L’élément cible doit être utilisé en tant que source pour cette liaison. Cela s’avère utile pour lier une propriété d’un élément à une autre propriété du même élément. |
| {RelativeSource TemplatedParent} | L’élément [<strong>ControlTemplate</strong>](https://msdn.microsoft.com/library/windows/apps/br209391) appliqué est la source de cette liaison. Cela s’avère utile pour appliquer des informations d’exécution aux liaisons au niveau du modèle. | 

## Remarques

Un objet [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) peut définir [**Binding.RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) soit en tant qu’attribut sur un élément objet **Binding**, soit en tant que composant dans une [extension de balisage {Binding}](binding-markup-extension.md). C’est pourquoi deux syntaxes XAML différentes sont présentées.

**RelativeSource** est similaire à l’[extension de balisage {Binding}](binding-markup-extension.md), dans le sens où il s’agit d’une extension de balisage capable de renvoyer des instances d’elle-même, prenant en charge une construction basée sur une chaîne qui transmet avant tout un argument au constructeur. Dans ce cas, l’argument transmis est la valeur [**Mode**](https://msdn.microsoft.com/library/windows/apps/br209915).

Le mode **Self** s’avère utile pour les cas où le même élément doit être utilisé en tant qu’objet source et objet cible d’une liaison, alors que des propriétés différentes constituent la source et la cible. Cela s’avère utile pour lier une propriété d’un élément à une autre propriété du même élément et cela constitue une variation sur la liaison [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828) qui ne nécessite pas l’attribution d’un nom à l’élément, puis un référencement de l’élément à lui-même. Si vous liez une propriété d’un élément à une autre propriété du même élément, soit les propriétés doivent utiliser le même type de propriété, soit vous devez également utiliser un élément [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826) sur la liaison pour convertir les valeurs. Par exemple, vous pouvez utiliser [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) comme source de [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) sans conversion, mais un convertisseur est nécessaire pour utiliser [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/br209419) comme source de [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br209006).

Voici un exemple : Ce [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/br243371) utilise une [extension de balisage {Binding}](binding-markup-extension.md) pour que ses éléments [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) et [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) soient toujours égaux. Il est donc restitué sous forme d’un carré. Seule la hauteur est définie en tant que valeur fixe. Pour ce **Rectangle**, sa propriété [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) par défaut a la valeur **null** et non **this**. Par conséquent, pour faire en sorte que la source du contexte de données soit l’objet lui-même (et permettre la liaison à ses autres propriétés), nous utilisons l’argument `RelativeSource={RelativeSource Self}` dans l’utilisation de l’extension de balisage {Binding}.

```XAML
<Rectangle
  Fill="Orange" Width="200"
  Height="{Binding RelativeSource={RelativeSource Self}, Path=Width}"
/>
```

Une autre technique qui peut s’avérer utile consiste à utiliser `RelativeSource={RelativeSource Self}` pour affecter à la propriété [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) d’un objet l’objet en question. Ici, la classe [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) est étendue avec une propriété personnalisée qui fournit déjà un modèle d’affichage prêt à l’emploi pour sa propre liaison de données. Cette technique est employée dans certains exemples du SDK : `<common:LayoutAwarePage ... DataContext="{Binding DefaultViewModel, RelativeSource={RelativeSource Self}}">`

**Remarque** L’utilisation XAML de l’élément **RelativeSource** indique uniquement l’utilisation pour laquelle il est prévu : l’affectation d’une valeur à [**Binding.RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) en XAML dans le cadre d’une expression de liaison. En théorie, d’autres utilisations sont possibles s’il s’agit de définir une propriété dont la valeur est [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209913).

## Rubriques connexes

* [Vue d’ensemble du langage XAML](xaml-overview.md)
* [Présentation détaillée de la liaison de données](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [Extension de balisage {Binding}](binding-markup-extension.md)
* [**Liaison**](https://msdn.microsoft.com/library/windows/apps/br209820)
* [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209913)



<!--HONumber=Mar16_HO1-->


