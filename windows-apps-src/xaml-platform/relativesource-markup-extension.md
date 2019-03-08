---
description: Fournit un moyen de spécifier la source d’une liaison en termes de relation relative dans le graphique d’objet au moment de l’exécution.
title: Extension de balisage RelativeSource
ms.assetid: B87DEF36-BE1F-4C16-B32E-7A896BD09272
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 26cde97f82e6962d530721f1e0230138e5917016
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617924"
---
# <a name="relativesource-markup-extension"></a>Extension de balisage {RelativeSource}


Fournit un moyen de spécifier la source d’une liaison en termes de relation relative dans le graphique d’objet au moment de l’exécution.

## <a name="xaml-attribute-usage-self-mode"></a>Utilisation des attributs XAML (mode Self)

``` syntax
<Binding RelativeSource="{RelativeSource Self}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource Self} ...}" .../>
```

## <a name="xaml-attribute-usage-templatedparent-mode"></a>Utilisation des attributs XAML (mode TemplatedParent)

``` syntax
<Binding RelativeSource="{RelativeSource TemplatedParent}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource TemplatedParent} ...}" .../>
```

## <a name="xaml-values"></a>Valeurs XAML

| Terme | Description |
|------|-------------|
| {RelativeSource Self} | Produit une valeur [<strong>Mode</strong>](https://msdn.microsoft.com/library/windows/apps/br209915) de <strong>Self</strong>. L’élément cible doit être utilisé en tant que source pour cette liaison. Cela s’avère utile pour lier une seule propriété d’un élément à une autre propriété du même élément. |
| {RelativeSource TemplatedParent} | Produit un élément [<strong>ControlTemplate</strong>](https://msdn.microsoft.com/library/windows/apps/br209391) qui est appliqué en tant que source de cette liaison. Cela s’avère utile pour appliquer des informations d’exécution aux liaisons au niveau du modèle. | 

## <a name="remarks"></a>Notes

Un objet [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) peut définir [**Binding.RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) soit en tant qu’attribut sur un élément objet **Binding**, soit en tant que composant dans une [extension de balisage {Binding}](binding-markup-extension.md). C’est pourquoi deux syntaxes XAML différentes sont présentées.

**RelativeSource** est similaire à [l’extension de balisage {Binding}](binding-markup-extension.md).  Il s’agit d’une extension de balisage capable de renvoyer des instances d’elle-même et prenant en charge une construction basée sur une chaîne qui transmet avant tout un argument au constructeur. Dans ce cas, l’argument transmis est la valeur [**Mode**](https://msdn.microsoft.com/library/windows/apps/br209915).

Le mode **Self** s’avère utile pour lier une propriété d’un élément à une autre propriété du même élément et cela constitue une variation sur la liaison [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828), mais ne nécessite pas l’attribution d’un nom à l’élément, puis un référencement de l’élément à lui-même. Si vous liez une propriété d’un élément à une autre propriété du même élément, soit les propriétés doivent utiliser le même type de propriété, soit vous devez également utiliser un élément [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826) sur la liaison pour convertir les valeurs. Par exemple, vous pouvez utiliser [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) comme source de [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) sans conversion, mais un convertisseur est nécessaire pour utiliser [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/br209419) comme source de [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br209006).

Voici un exemple : Ce [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) utilise une [extension de balisage {Binding}](binding-markup-extension.md) pour que ses éléments [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) et [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) soient toujours égaux. Il est donc restitué sous forme d’un carré. Seule la hauteur est définie en tant que valeur fixe. Pour ce **Rectangle**, sa propriété [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) par défaut a la valeur **null** et non **this**. Par conséquent, pour faire en sorte que la source du contexte de données soit l’objet lui-même (et permettre la liaison à ses autres propriétés), nous utilisons l’argument `RelativeSource={RelativeSource Self}` dans l’utilisation de l’extension de balisage {Binding}.

```XML
<Rectangle
  Fill="Orange" Width="200"
  Height="{Binding RelativeSource={RelativeSource Self}, Path=Width}"
/>
```

Une autre utilisation de `RelativeSource={RelativeSource Self}` sert à définir le [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) d’un objet sur lui-même.  Par exemple, vous pouvez voir cette technique dans certains des exemples de kit de développement logiciel où le [ **Page** ](https://msdn.microsoft.com/library/windows/apps/br227503) classe a été étendue avec une propriété personnalisée qui fournit déjà un modèle de vue de prêts à l’emploi pour sa propre liaison de données par exemple : `<common:LayoutAwarePage ... DataContext="{Binding DefaultViewModel, RelativeSource={RelativeSource Self}}">`

**Remarque**  utilisation le XAML pour **RelativeSource** illustre uniquement l’utilisation pour laquelle il est destiné : définition d’une valeur pour [ **Binding.RelativeSource** ](https://msdn.microsoft.com/library/windows/apps/br209831)dans XAML en tant que partie d’une expression de liaison. En théorie, d’autres utilisations sont possibles s’il s’agit de définir une propriété dont la valeur est [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209913).

## <a name="related-topics"></a>Rubriques connexes

* [Vue d’ensemble du langage XAML](xaml-overview.md)
* [Liaison de données en profondeur](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [Extension de balisage {binding}](binding-markup-extension.md)
* [**Liaison**](https://msdn.microsoft.com/library/windows/apps/br209820)
* [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209913)

