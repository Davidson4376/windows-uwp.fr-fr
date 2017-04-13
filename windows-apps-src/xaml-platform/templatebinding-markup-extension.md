---
author: jwmsft
description: "Lie la valeur d’une propriété dans un modèle de contrôle à la valeur d’une autre propriété exposée sur le contrôle basé sur un modèle. TemplateBinding peut uniquement être utilisé dans une définition ControlTemplate en XAML."
title: Extension de balisage TemplateBinding
ms.assetid: FDE71086-9D42-4287-89ED-8FBFCDF169DC
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: ca551ff53a0a91b5bc60263b6e282b95c32bf976
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="templatebinding-markup-extension"></a>Extension de balisage {TemplateBinding}

\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Lie la valeur d’une propriété dans un modèle de contrôle à la valeur d’une autre propriété exposée sur le contrôle basé sur un modèle. **TemplateBinding** peut uniquement être utilisé dans une définition [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) en XAML.

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object propertyName="{TemplateBinding sourceProperty}" .../>
```

## <a name="xaml-attribute-usage-for-setter-property-in-template-or-style"></a>Utilisation des attributs XAML (pour la propriété Setter dans un modèle ou style)

``` syntax
<Setter Property="propertyName" Value="{TemplateBinding sourceProperty}" .../>
```

## <a name="xaml-values"></a>Valeurs XAML

| Terme | Description |
|------|-------------|
| propertyName | Nom de la propriété définie dans la syntaxe de la méthode setter. Il doit s’agir d’une propriété de dépendance. |
| sourceProperty | Nom d’une autre propriété de dépendance qui existe sur le type qui est basé sur un modèle. |

## <a name="remarks"></a>Remarques

L’utilisation de **TemplateBinding** est un élément fondamental de la définition d’un modèle de contrôle, si vous êtes l’auteur d’un contrôle personnalisé ou si vous remplacez un modèle de contrôle pour des contrôles existants. Pour plus d’informations, voir [Démarrage rapide : modèles de contrôles](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374).

Il est relativement courant pour *propertyName* et *targetProperty* d’utiliser le même nom de propriété. Dans ce cas, un contrôle peut définir une propriété sur elle-même et la transmettre à une propriété existante nommée de manière intuitive de l’une de ses parties de composants. Par exemple, un contrôle qui incorpore un [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) dans sa composition, lequel est utilisé pour afficher la propre propriété **Text** du contrôle, peut inclure ce code XAML en tant que partie dans le modèle du contrôle: `<TextBlock Text="{TemplateBinding Text}" .... />`

Les types utilisés comme valeur de la propriété de source et valeur de la propriété cible doivent correspondre. Il est impossible d’introduire un convertisseur lorsque vous utilisez **TemplateBinding**. Si les valeurs ne correspondent pas, une erreur se produit lors de l’analyse du code XAML. Si vous avez besoin d’un convertisseur, vous pouvez utiliser la syntaxe détaillée pour une liaison de modèle. Par exemple: `{Binding RelativeSource={RelativeSource TemplatedParent}, Converter="..." ...}`

Toute tentative d’utilisation d’un **TemplateBinding** en dehors d’une définition [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) en XAML engendre une erreur d’analyse.

Vous pouvez utiliser **TemplateBinding** lorsque la valeur du parent basé sur un modèle est également différée comme autre liaison. L’évaluation de **TemplateBinding** peut démarrer lorsque des valeurs sont affectées à des liaisons runtime requises.

**TemplateBinding** est toujours une liaison à sens unique. Les deux propriétés impliquées doivent être des propriétés de dépendance.

**TemplateBinding** est une extension de balisage. Les extensions de balisage sont généralement implémentées lorsqu’il est nécessaire de procéder à l’échappement de valeurs d’attribut pour en faire autre chose que des valeurs littérales ou des noms de gestionnaires. Il s’agit d’une mesure plus globale que celle qui consiste à placer simplement des convertisseurs de types au niveau de certains types ou propriétés. Toutes les extensions de balisage XAML utilisent les caractères «{» et «}» dans leur syntaxe d’attribut, ce qui correspond à la convention qui permet au processeur XAML de reconnaître qu’une extension de balisage doit traiter l’attribut.

**Remarque** Dans l’implémentation du processeur XAML Windows Runtime, il n’existe aucune représentation de classe de stockage pour **TemplateBinding**. **TemplateBinding** est à utiliser exclusivement dans le balisage XAML. Il n’y a pas de moyen simple de reproduire le comportement dans du code.

## <a name="related-topics"></a>Rubriques connexes

* [Démarrage rapide: modèles de contrôles](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374)
* [Présentation détaillée de la liaison de données](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391)
* [Vue d’ensemble du langage XAML](xaml-overview.md)
* [Vue d’ensemble des propriétés de dépendance](dependency-properties-overview.md)
 

