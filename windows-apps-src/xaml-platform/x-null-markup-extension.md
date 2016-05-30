---
author: jwmsft
description: En XAML, un balisage spécifie une valeur null pour une propriété.
title: Extension de balisage xNull
ms.assetid: E6A4038E-4ADA-4E82-9824-582FC16AB037
---

# Extension de balisage {x&#58;Null}

\[ Article mis à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

En XAML, un balisage spécifie une valeur **null** pour une propriété.

## Utilisation des attributs XAML

``` syntax
<object property="{x:Null}" .../>
```

## Remarques

**null** est le mot clé de référence Null pour C# et C++. Le mot clé Microsoft Visual Basic pour une référence Null est **Nothing**.

La valeur par défaut initiale peut varier selon les propriétés de dépendance, et n’est pas nécessairement **null**. De plus, de nombreuses propriétés de dépendance n’acceptent pas **null** en tant que valeur (via le balisage ou le code), en raison de leur implémentation interne. Le cas échéant, la définition d’une valeur d’attribut XAML avec **{x:Null}** peut engendrer une exception d’analyse.

Certains types Windows Runtime ont la valeur Nullable. Quand un type Nullable n’a pas déjà la valeur **null** comme valeur par défaut, vous pouvez utiliser **{x:Null}** pour définir la valeur **null** en XAML. Si vous utilisez des extensions de composant Visual C++ (C++/CX), les types Nullable sont représentés par [**Platform::IBox<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/jj606120.aspx). Si vous utilisez des langages Microsoft .NET, les types Nullable sont représentés par [**Nullable<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx).

## Rubriques connexes

* [**Nullable<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx)
* [**IReference<T>**](https://msdn.microsoft.com/library/windows/apps/br225864)
 



<!--HONumber=May16_HO2-->


