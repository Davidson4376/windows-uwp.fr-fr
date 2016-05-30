---
author: jwmsft
description: Modifie le comportement de la compilation XAML pour que les champs des références d’objet nommé soient définis avec l’accès public au lieu de suivre le comportement private par défaut.
title: Attribut xFieldModifier
ms.assetid: 6FBCC00B-848D-4454-8B1F-287CA8406DDF
---

# Attribut x&#58;FieldModifier

\[ Article mis à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Modifie le comportement de la compilation XAML pour que les champs des références d’objet nommé soient définis avec l’accès **public** au lieu de suivre le comportement **private** par défaut.

## Utilisation des attributs XAML

``` syntax
<object x:FieldModifier="public".../>
```

## Dépendances

L’[attribut x:Phase](x-name-attribute.md) doit également être fourni sur le même élément.

## Remarques

La valeur de l’attribut **x:FieldModifier** dépend du langage de programmation. La chaîne à utiliser dépend de la façon dont chaque langage implémente son **CodeDomProvider** et des convertisseurs de type qu’il retourne pour définir les significations de **TypeAttributes.Public** et de **TypeAttributes.NotPublic**. Dans le cas de C#, de Microsoft Visual Basic ou des extensions de composant Visual C++ (C++/CX), vous pouvez attribuer la valeur de chaîne « public » ou « Public » ; l’analyseur n’impose pas de casse à cette valeur d’attribut.

Vous pouvez également spécifier **NonPublic** (**internal** en C# ou C++/CX, **Friend** en Visual Basic), mais cela n’est pas courant. L’accès interne n’a aucune application dans le modèle de génération de code XAML Windows Runtime. L’accès privé est l’accès par défaut.

**x:FieldModifier** n’est pertinent que pour les éléments comportant un [attribut x:Phase](x-name-attribute.md), car ce nom est utilisé pour référencer le champ une fois qu’il est public.

**Remarque** Le XAML Windows Runtime ne prend pas en charge **x:ClassModifier** ou **x:Subclass**.



<!--HONumber=May16_HO2-->


