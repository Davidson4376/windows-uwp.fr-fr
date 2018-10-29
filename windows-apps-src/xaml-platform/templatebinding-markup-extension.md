---
author: jwmsft
description: Lie la valeur d’une propriété dans un modèle de contrôle à la valeur d’une autre propriété exposée sur le contrôle basé sur un modèle. TemplateBinding peut uniquement être utilisé dans une définition ControlTemplate en XAML.
title: Extension de balisage TemplateBinding
ms.assetid: FDE71086-9D42-4287-89ED-8FBFCDF169DC
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 842f1bf1642e79d4bd2651560fdf7208cfb1877d
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5739840"
---
# <a name="templatebinding-markup-extension"></a>Extension de balisage {TemplateBinding}


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

**Remarque**implémentation du processeur dans le XAML Windows Runtime, il n’existe aucune représentation de classe de stockage pour **TemplateBinding**. **TemplateBinding** est à utiliser exclusivement dans le balisage XAML. Il n’y a pas de moyen simple de reproduire le comportement dans du code.

### <a name="xbind-in-controltemplate"></a>x: Bind dans ControlTemplate

À partir de la prochaine mise à jour majeure vers Windows 10, vous pouvez utiliser l’extension de balisage **x: Bind** n’importe où vous avez utilisé **TemplateBinding** dans [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391). 

La propriété [TargetType](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate.targettype#Windows_UI_Xaml_Controls_ControlTemplate_TargetType) devra (pas facultatif) sur [ControlTemplate](https://msdn.microsoft.com/library/windows/apps/br209391) lors de l’utilisation de **x: Bind**.

**x: Bind** prenant en charge, vous pouvez désormais utiliser deux [liaisons de fonction](../data-binding/function-bindings.md) liaisons bidirectionnelles bien comme dans [ControlTemplate](https://msdn.microsoft.com/library/windows/apps/br209391)

Dans l’exemple suivant, le TextBlock.Text est évaluée à Button.Content.ToString(). La propriété TargetType sur le ControlTemplate agit comme la source de données et accomplit le même résultat qu’un TemplateBinding et parent.

```xaml
<ControlTemplate TargetType="Button">
    <Grid>
        <TextBlock Text="{x:Bind Content}" />
    </Grid>
</ControlTemplate>
```

## <a name="related-topics"></a>Rubriques connexes

* [Démarrage rapide: modèles de contrôles](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374)
* [Présentation détaillée de la liaison de données](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391)
* [Vue d’ensemble du langage XAML](xaml-overview.md)
* [Vue d’ensemble des propriétés de dépendance](dependency-properties-overview.md)
 

