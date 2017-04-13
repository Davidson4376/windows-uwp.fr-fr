---
author: jwmsft
description: "Fournit une valeur pour tout attribut XAML en évaluant une référence à une ressource qui provient de l’implémentation d’une recherche de ressource personnalisée. La recherche de ressource est effectuée via l’implémentation d’une classe CustomXamlResourceLoader."
title: Extension de balisage CustomResource
ms.assetid: 3A59A8DE-E805-4F04-B9D9-A91E053F3642
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: a14ae2e327805c5d123cc3b6232bfa18863f9da9
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="customresource-markup-extension"></a>Extension de balisage {CustomResource}

\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Fournit une valeur pour tout attribut XAML en évaluant une référence à une ressource qui provient de l’implémentation d’une recherche de ressource personnalisée. La recherche de ressource est effectuée via l’implémentation d’une classe [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327).

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object property="{CustomResource key}" .../>
```

## <a name="xaml-values"></a>Valeurs XAML

| Terme | Description |
|------|-------------|
| key | Clé de la ressource demandée. Le mode d’affectation initial est spécifique à l’implémentation de la classe [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) actuellement inscrite pour être utilisée. |

## <a name="remarks"></a>Notes

**CustomResource** est une technique permettant d’obtenir des valeurs définies ailleurs dans un référentiel de ressources personnalisées. Cette technique relativement avancée n’est pas utilisée dans la plupart des scénarios des applications Windows Runtime.

Le mode de résolution de **CustomResource** en dictionnaire de ressources n’est pas décrit dans cette rubrique, car il peut varier considérablement selon la façon dont [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) est implémenté.

La méthode [**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340) de l’implémentation de [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) est appelée par l’analyseur XAML Windows Runtime chaque fois qu’il rencontre l’utilisation de `{CustomResource}` dans le balisage. Le *resourceId* passé à **GetResource** vient de l’argument *key*. Par ailleurs, les autres paramètres d’entrée proviennent du contexte, par exemple la propriété à laquelle l’utilisation s’applique.

L’utilisation de `{CustomResource}` ne fonctionne pas par défaut (l’implémentation de base de [**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340) est incomplète). Pour créer une référence valide à `{CustomResource}`, vous devez effectuer chacune des étapes suivantes :

1.  Dérivez une classe personnalisée de [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) et substituez la méthode [**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340). N’appelez pas la base dans l’implémentation.
2.  Définissez [**CustomXamlResourceLoader.Current**](https://msdn.microsoft.com/library/windows/apps/br243328) de manière à référencer votre classe dans une logique d’initialisation. Cette opération doit intervenir avant le chargement de tout code XAML de niveau page comprenant l’utilisation d’une extension `{CustomResource}`. Vous pouvez définir **CustomXamlResourceLoader.Current** dans le constructeur de sous-classe [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324) qui est automatiquement généré dans les modèles code-behind App.xaml.
3.  Vous pouvez à présent utiliser des extensions `{CustomResource}` dans le XAML que votre application charge en tant que pages, ou à partir de dictionnaires de ressources XAML.

**CustomResource** est une extension de balisage. Les extensions de balisage sont généralement implémentées lorsqu’il est nécessaire de procéder à l’échappement de valeurs d’attribut pour en faire autre chose que des valeurs littérales ou des noms de gestionnaires. Il s’agit d’une mesure plus globale que celle qui consiste à placer simplement des convertisseurs de types au niveau de certains types ou propriétés. Toutes les extensions de balisage XAML utilisent les caractères «\{» et «\}» dans leur syntaxe d’attribut, ce qui correspond à la convention qui permet au processeur XAML de reconnaître qu’une extension de balisage doit traiter l’attribut.

## <a name="related-topics"></a>Rubriques connexes

* [Références aux ressources ResourceDictionary et XAML](https://msdn.microsoft.com/library/windows/apps/mt187273)
* [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327)
* [**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340)

