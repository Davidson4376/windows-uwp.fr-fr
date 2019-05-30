---
description: Lie la valeur d’une propriété dans un modèle de contrôle à la valeur d’une autre propriété exposée sur le contrôle basé sur un modèle. TemplateBinding peut uniquement être utilisé dans une définition ControlTemplate en XAML.
title: Extension de balisage TemplateBinding
ms.assetid: FDE71086-9D42-4287-89ED-8FBFCDF169DC
ms.date: 10/29/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e784b14c30222a28a0e10f8ba0bcf96e6c7f9afd
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372314"
---
# <a name="templatebinding-markup-extension"></a>Extension de balisage {TemplateBinding}

Lie la valeur d’une propriété dans un modèle de contrôle à la valeur d’une autre propriété exposée sur le contrôle basé sur un modèle. **TemplateBinding** peut uniquement être utilisé dans une définition [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) en XAML.

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

## <a name="remarks"></a>Notes

L’utilisation de **TemplateBinding** est un élément fondamental de la définition d’un modèle de contrôle, si vous êtes l’auteur d’un contrôle personnalisé ou si vous remplacez un modèle de contrôle pour des contrôles existants. Pour plus d’informations, consultez [Guide de démarrage rapide : Modèles de contrôle](https://docs.microsoft.com/previous-versions/windows/apps/hh465374(v=win.10)).

Il est relativement courant pour *propertyName* et *targetProperty* d’utiliser le même nom de propriété. Dans ce cas, un contrôle peut définir une propriété sur elle-même et la transmettre à une propriété existante nommée de manière intuitive de l’une de ses parties de composants. Par exemple, un contrôle qui incorpore un [ **TextBlock** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) dans sa composition, qui est utilisé pour afficher le contrôle propre **texte** propriété, peut inclure ce XAML en tant que partie dans le modèle de contrôle : `<TextBlock Text="{TemplateBinding Text}" .... />`

Les types utilisés comme valeur de la propriété de source et valeur de la propriété cible doivent correspondre. Il est impossible d’introduire un convertisseur lorsque vous utilisez **TemplateBinding**. Si les valeurs ne correspondent pas, une erreur se produit lors de l’analyse du code XAML. Si vous avez besoin d’un convertisseur, vous pouvez utiliser la syntaxe détaillée pour une liaison de modèle tels que : `{Binding RelativeSource={RelativeSource TemplatedParent}, Converter="..." ...}`

Toute tentative d’utilisation d’un **TemplateBinding** en dehors d’une définition [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) en XAML engendre une erreur d’analyse.

Vous pouvez utiliser **TemplateBinding** lorsque la valeur du parent basé sur un modèle est également différée comme autre liaison. L’évaluation de **TemplateBinding** peut démarrer lorsque des valeurs sont affectées à des liaisons runtime requises.

**TemplateBinding** est toujours une liaison à sens unique. Les deux propriétés impliquées doivent être des propriétés de dépendance.

**TemplateBinding** est une extension de balisage. Les extensions de balisage sont généralement implémentées lorsqu’il est nécessaire de procéder à l’échappement de valeurs d’attribut pour en faire autre chose que des valeurs littérales ou des noms de gestionnaires. Il s’agit d’une mesure plus globale que celle qui consiste à placer simplement des convertisseurs de types au niveau de certains types ou propriétés. Toutes les extensions de balisage XAML utilisent les caractères « { » et « } » dans leur syntaxe d’attribut, ce qui correspond à la convention qui permet au processeur XAML de reconnaître qu’une extension de balisage doit traiter l’attribut.

**Remarque**  implémentation du processeur dans le XAML de Runtime de Windows, il n’existe aucune représentation de classe de stockage pour **TemplateBinding**. **TemplateBinding** est à utiliser exclusivement dans le balisage XAML. Il n’y a pas de moyen simple de reproduire le comportement dans du code.

### <a name="xbind-in-controltemplate"></a>x : Bind dans un ControlTemplate

> [!NOTE]
> À l’aide de x : Bind dans un ControlTemplate nécessite Windows 10, version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou version ultérieure. Pour plus d’informations sur les versions cibles, voir [Code adaptatif de version](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

À partir de Windows 10, version 1809, vous pouvez utiliser la **x : Bind** extension de balisage n’importe où vous utilisez **TemplateBinding** dans un [ **ControlTemplate** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate). 

Le [TargetType](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate.targettype) propriété est requise (non facultatifs) sur [ControlTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) lors de l’utilisation **x : Bind**.

Avec **x : Bind** prennent en charge, vous pouvez utiliser deux [fonction liaisons](../data-binding/function-bindings.md) , ainsi que des liaisons bidirectionnelles dans un [ControlTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate).

Dans cet exemple, le **TextBlock.Text** prend la valeur de propriété **Button.Content.ToString**. TargetType sur le ControlTemplate agit comme la source de données et effectue le même résultat qu’un TemplateBinding au parent.

```xaml
<ControlTemplate TargetType="Button">
    <Grid>
        <TextBlock Text="{x:Bind Content, Mode=OneWay}"/>
    </Grid>
</ControlTemplate>
```

## <a name="related-topics"></a>Rubriques connexes

* [Démarrage rapide : Modèles de contrôle](https://docs.microsoft.com/previous-versions/windows/apps/hh465374(v=win.10))
* [Présentation détaillée de la liaison de données](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)
* [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)
* [Vue d’ensemble du langage XAML](xaml-overview.md)
* [Vue d’ensemble des propriétés de dépendance](dependency-properties-overview.md)
 

