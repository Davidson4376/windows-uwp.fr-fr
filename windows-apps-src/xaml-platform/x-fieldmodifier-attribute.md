---
description: Modifie le comportement de la compilation XAML pour que les champs des références d’objet nommé soient définis avec l’accès public au lieu de suivre le comportement private par défaut.
title: Attribut xFieldModifier
ms.assetid: 6FBCC00B-848D-4454-8B1F-287CA8406DDF
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 751cda36fc58d0e6add9204327a74ec947c9fc53
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660914"
---
# <a name="xfieldmodifier-attribute"></a>Attribut x:FieldModifier


Modifie le comportement de la compilation XAML pour que les champs des références d’objet nommé soient définis avec l’accès **public** au lieu de suivre le comportement **private** par défaut.

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object x:FieldModifier="public".../>
```

## <a name="dependencies"></a>Dépendances

L’[attribut x:Phase](x-name-attribute.md) doit également être fourni sur le même élément.

## <a name="remarks"></a>Notes

La valeur de l’attribut **x:FieldModifier** dépend du langage de programmation. Les valeurs valides sont **privé**, **public**, **protégé**, **interne** ou **ami**. Pour C#, extensions du composant Microsoft Visual Basic ou Visual C++ (C++ / c++ / CX), vous pouvez donner à la chaîne de valeur « public » ou « Public » ; l’analyseur n’impose pas le cas sur cette valeur d’attribut.

L’accès **privé** est l’accès par défaut.

**x:FieldModifier** n’est pertinent que pour les éléments comportant un [attribut x:Phase](x-name-attribute.md), car ce nom est utilisé pour référencer le champ une fois qu’il est public.

**Remarque**  Windows Runtime XAML ne prend pas en charge **x : ClassModifier** ou **x : Subclass**.

