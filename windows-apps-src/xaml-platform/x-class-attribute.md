---
author: jwmsft
description: Configure la compilation XAML afin de joindre des classes partielles entre le balisage et le code-behind. La classe partielle du code est définie dans un fichier de code distinct et la classe partielle de balisage est créée par la génération du code lors de la compilation XAML.
title: Attribut x:Class
ms.assetid: 40A7C036-133A-44DF-9D11-0D39232C948F
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6746969b1b717183894d6b941be41c9aca452960
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5932496"
---
# <a name="xclass-attribute"></a>Attribut x:Class


Configure la compilation XAML afin de joindre des classes partielles entre le balisage et le code-behind. La classe partielle du code est définie dans un fichier de code distinct et la classe partielle de balisage est créée par la génération du code lors de la compilation XAML.

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML


``` syntax
<object x:Class="namespace.classname"...>
  ...
</object>
```

## <a name="xaml-values"></a>Valeurs XAML

| Terme | Description |
|------|-------------|
| namespace | Facultatif. Spécifie un espace de noms qui contient la classe partielle identifiée par _classname_. Si _namespace_ est spécifié, un point (.) sépare _namespace_ et _classname_. Si _namespace_ est omis, _classname_ est considéré comme n’ayant pas d’espace de noms. |
| classname | Obligatoire. Spécifie le nom de la classe partielle qui connecte le code XAML chargé et votre code-behind pour ce code XAML. | 

## <a name="remarks"></a>Remarques

Il est possible de déclarer **x:Class** en tant qu’attribut pour un élément qui est la racine d’une arborescence d’objets/d’un fichier XAML et qui est compilé par des actions de génération, ou pour la racine de la classe [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324) dans la définition d’application d’une application compilée. La déclaration de **x:Class** sur un élément autre qu’un nœud racine, et dans toutes les circonstances pour un fichier XAML qui n’est pas compilé avec l’action de génération **Page**, engendre une erreur au moment de la compilation.

La classe utilisée en tant que **x:Class** ne peut pas être une classe imbriquée.

La valeur de l’attribut **x:Class** doit être une chaîne qui spécifie le nom qualifié complet d’une classe. Vous pouvez omettre les informations relatives à l’espace de noms tant qu’il s’agit de la manière dont le code-behind est également structuré (votre définition de classe démarre au niveau de la classe). Le fichier code-behind d’une définition de page ou d’application doit se trouver dans un fichier de code inclus dans le cadre du projet. La classe code-behind doit être publique. La classe code-behind doit être partielle.

## <a name="clr-language-rules"></a>Règle du langage CLR

Bien que votre fichier code-behind puisse être un fichier C++, il existe certaines conventions qui suivent quand même la forme du langage CLR, afin qu’il n’y ait aucune différence dans la syntaxe XAML. En particulier, le séparateur entre l’espace de noms et les composants de nom de classe d’une valeur **x:Class** est toujours un point (« . »), même si le séparateur entre l’espace de noms et le nom de classe dans le fichier de code C++ associé au XAML est « :: ». Si vous déclarez des espaces de noms imbriqués en C++, le séparateur entre les chaînes d’espaces de noms imbriquées successives doit être également « . » plutôt que « :: » quand vous spécifiez la partie *namespace* de la valeur **x:Class**.

