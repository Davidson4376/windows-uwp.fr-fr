---
author: jwmsft
description: Modifie le comportement de la compilation XAML pour que les champs des références d’objet nommé soient définis avec l’accès public au lieu de suivre le comportement private par défaut.
title: Attribut xFieldModifier
ms.assetid: 6FBCC00B-848D-4454-8B1F-287CA8406DDF
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: de1d7dedbd2bd3d51bd2e1c1a9652d18f2b78ef0
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7111728"
---
# <a name="xfieldmodifier-attribute"></a>Attribut x:FieldModifier


Modifie le comportement de la compilation XAML pour que les champs des références d’objet nommé soient définis avec l’accès **public** au lieu de suivre le comportement **private** par défaut.

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object x:FieldModifier="public".../>
```

## <a name="dependencies"></a>Dépendances

L’[attribut x:Phase](x-name-attribute.md) doit également être fourni sur le même élément.

## <a name="remarks"></a>Remarques

La valeur de l’attribut **x:FieldModifier** dépend du langage de programmation. Les valeurs valides sont **privé**, **public**, **protégé**, **interne** ou **ami**. Pour les extensions de composant c#, Microsoft Visual Basic ou Visual c++ (C++ / CX), vous pouvez donner à la chaîne de valeur «public» ou «Public»; l’analyseur n’impose pas de casse à cette valeur d’attribut.

L’accès **privé** est l’accès par défaut.

**x:FieldModifier** n’est pertinent que pour les éléments comportant un [attribut x:Phase](x-name-attribute.md), car ce nom est utilisé pour référencer le champ une fois qu’il est public.

**Remarque**XAML Windows Runtime ne prend pas en charge **x: ClassModifier** ou **x: Subclass**.

