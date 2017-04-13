---
author: jwmsft
description: "Modifie le comportement de la compilation XAML pour que les champs des références d’objet nommé soient définis avec l’accès public au lieu de suivre le comportement private par défaut."
title: Attribut xFieldModifier
ms.assetid: 6FBCC00B-848D-4454-8B1F-287CA8406DDF
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: cad84be24836bc6a33a4ab08f1ca4fa2d9e97512
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="xfieldmodifier-attribute"></a>Attribut x:FieldModifier

\[ Article mis à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Modifie le comportement de la compilation XAML pour que les champs des références d’objet nommé soient définis avec l’accès **public** au lieu de suivre le comportement **private** par défaut.

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object x:FieldModifier="public".../>
```

## <a name="dependencies"></a>Dépendances

L’[attribut x:Phase](x-name-attribute.md) doit également être fourni sur le même élément.

## <a name="remarks"></a>Remarques

La valeur de l’attribut **x:FieldModifier** dépend du langage de programmation. Les valeurs valides sont **privé**, **public**, **protégé**, **interne** ou **ami**. Dans le cas de C#, de Microsoft Visual Basic ou des extensions de composant Visual C++ (C++/CX), vous pouvez attribuer la valeur de chaîne «public» ou «Public»; l’analyseur n’impose pas de casse à cette valeur d’attribut.

L’accès **privé** est l’accès par défaut.

**x:FieldModifier** n’est pertinent que pour les éléments comportant un [attribut x:Phase](x-name-attribute.md), car ce nom est utilisé pour référencer le champ une fois qu’il est public.

**Remarque**  Le XAML Windows Runtime ne prend pas en charge **x:ClassModifier** ou **x:Subclass**.

