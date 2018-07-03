---
author: mijacobs
description: Découvrez comment utiliser la typographie de votre application pour aider les utilisateurs à comprendre facilement le contenu.
title: Typographie des applications UWP
ms.author: mijacobs
ms.date: 04/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 505167775b61908be7f47068dbf3221c293f6112
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843369"
---
# <a name="typography"></a>Typographie

![image hero](images/header-typography.svg)

En tant que représentation visuelle du langage, la typographie a pour mission principale de communiquer des informations. Son style doit toujours être aligné sur cet objectif. Dans cet article, nous décrirons comment appliquer un style à la typographie de votre application UWP pour aider les utilisateurs à comprendre facilement et efficacement le contenu.

## <a name="font"></a>Police

Vous devez utiliser une police dans toute l’interface utilisateur de votre application, et nous vous recommandons d’utiliser la police par défaut pour les applications UWP, **Segoe UI**. Elle est conçue pour conserver une lisibilité optimale, quelles que soient les tailles et les densités en pixels. Elle se caractérise par une esthétique nette, légère et aérée en parfaite harmonie avec le contenu du système.

![Exemple de texte dans la police Segoe UI](images/type/segoe-sample.svg)

Pour afficher les langues autres que l’anglais ou pour sélectionner une autre police pour votre application, consultez [Langues](#Languages) et [Polices](#Fonts) pour connaître nos polices recommandées pour les applications UWP.

:::row::: :::column::: ![à faire](images/do.svg) Sélectionnez une police pour votre interface utilisateur.
:::column-end::: :::column::: ![à ne pas faire](images/dont.svg) Ne mélangez pas plusieurs polices.
:::column-end::: :::row-end:::

## <a name="size-and-scaling"></a>Taille et mise à l’échelle

Les tailles de police utilisées dans les applications UWP sont automatiquement mises à l’échelle sur tous les appareils. L’algorithme de mise à l’échelle garantit qu’une police de 24pixels sur un appareil Surface Hub placé à une distance de 3mètres est aussi lisible qu’une police de 24pixels sur un téléphone doté d’un écran 5pouces distant de quelques centimètres.

![distances d’affichage des différents appareils](images/type/scaling-chart.svg)

En raison du mode de fonctionnement du système de mise à l’échelle, la conception est effectuée en pixels effectifs, et non en pixels physiques réels. En outre, il n’est pas nécessaire de modifier les tailles de police pour différentes tailles ou résolutions d’écran.

:::row::: :::column::: ![à faire](images/do.svg) Suivez le redimensionnement UWP [gamme de caractères](#type-ramp).
:::column-end::: :::column::: ![à ne pas faire](images/dont.svg) Utilisez une taille de police inférieure à12px.
:::column-end::: :::row-end:::

## <a name="hierarchy"></a>Hiérarchie

:::row::: :::column::: Les utilisateurs se basent sur la hiérarchie visuelle lors de l’analyse d’une page: les en-têtes résument le contenu et le texte du corps fournit d’autres détails. Pour créer une hiérarchie visual précise dans votre application, suivez la gamme de caractères UWP.
:::column-end::: :::column::: ![styles de bloc de texte](images/type/type-hierarchy.svg) :::column-end::: :::row-end:::

### <a name="type-ramp"></a>Gamme de caractères

La gamme de caractères UWP établit des relations cruciales entre les styles de caractère sur une page, afin d’aider les utilisateurs à lire facilement le contenu. Toutes les tailles sont exprimées en pixels effectifs et sont optimisées pour les applications UWP s’exécutant sur tous les appareils.

![Gamme de caractères](images/type/type-ramp.svg)

### <a name="using-the-type-ramp"></a>Utilisation de la gamme de caractères

:::row::: :::column::: Vous pouvez accéder aux niveaux de la gamme de caractères en tant que [ressources statiques](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp) XAML. Les styles suivent la convention d’affectation de noms `*TextBlockStyle`.
:::column-end::: :::column::: ![styles de bloc de texte](images/type/text-block-type-ramp.svg) :::column-end::: :::row-end:::

```XAML
<TextBlock Text="Header" Style="{StaticResource HeaderTextBlockStyle}"/>
<TextBlock Text="SubHeader" Style="{StaticResource SubheaderTextBlockStyle}"/>
<TextBlock Text="Title" Style="{StaticResource TitleTextBlockStyle}"/>
<TextBlock Text="SubTitle" Style="{StaticResource SubtitleTextBlockStyle}"/>
<TextBlock Text="Base" Style="{StaticResource BaseTextBlockStyle}"/>
<TextBlock Text="Body" Style="{StaticResource BodyTextBlockStyle}"/>
<TextBlock Text="Caption" Style="{StaticResource CaptionTextBlockStyle}"/>
```

:::row::: :::column::: ![à faire](images/do.svg) Utilisez «Body» pour la plus grande partie du texte.

        Use "Base" for titles when space is constrained.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        Use "Caption" for primary action or any long strings.

        Use "Header" or "Subheader" if text needs to wrap.
    :::column-end:::
:::row-end:::

## <a name="alignment"></a>Alignement

La valeur [TextAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.textalignment) par défaut est Left, et dans la plupart des cas, cette approche d’alignement à gauche et de non-alignement à droite garantit un ancrage cohérent du contenu et une disposition uniforme. Pour les langues RTL, voir [Ajuster la disposition et les polices pour prendre en charge la globalisation](../globalizing/adjust-layout-and-fonts--and-support-rtl.md).

![Montre un texte aligné à gauche.](images/type/alignment.svg)

```xaml
<TextBlock TextAlignment="Left">
```

## <a name="character-count"></a>Nombre de caractères

:::row::: :::column::: ![à faire](images/do.svg) Utilisez des lignes de 50 à 60caractères afin de faciliter la lecture.
:::column-end::: :::column::: ![à ne pas faire](images/dont.svg) Une ligne de moins de 20caractères ou de plus de 60caractères est difficile à lire.
:::column-end::: :::row-end:::

## <a name="clipping-and-ellipses"></a>Détourage et ellipses

Lorsque la quantité de texte s’étend au-delà de l’espace disponible, nous vous recommandons de dérouter le texte, ce qui correspond au comportement par défaut de la plupart des [contrôles de texte UWP](../controls-and-patterns/text-controls.md).

![Cadre d’appareil avec détourage de texte](images/type/clipping.svg)

```xaml
<TextBlock TextWrapping="WrapWholeWords" TextTrimming="Clip"/>
```

:::row::: :::column::: ![à faire](images/do.svg) Déroutez le texte et renvoyez-le automatiquement à la ligne si plusieurs lignes sont activées.
:::column-end::: :::column::: ![à ne pas faire](images/dont.svg) Utilisez des ellipses pour éviter tout encombrement visuel.
:::column-end::: :::row-end:::

**Remarque**: si les conteneurs ne sont pas clairement définis (par exemple, sans couleur d’arrière-plan de différenciation), ou s’il existe un lien pour afficher plus de texte, utilisez des ellipses.

## <a name="languages"></a>Langues 

Segoe UI est notre police pour l’anglais, les langues européennes, le grec, l’hébreu, l’arménien, le géorgien et l’arabe. Pour les autres langues, consultez les recommandations suivantes.

### <a name="globalizinglocalizing-fonts"></a>Polices de globalisation/localisation

Utilisez les API de [mappage de police LanguageFont](https://docs.microsoft.com/uwp/api/Windows.Globalization.Fonts.LanguageFont) pour l’accès par programmation à la gamme de polices, à la taille, à l’épaisseur et au style recommandés pour une langue particulière. L’objet LanguageFont assure l’accès aux informations de police appropriées pour diverses catégories de contenu, notamment les en-têtes d’interface utilisateur, les notifications, le texte de corps et les polices de corps de document modifiables par l’utilisateur. Pour plus d’informations, voir [Ajuster la disposition et les polices pour prendre en charge la globalisation](../globalizing/adjust-layout-and-fonts--and-support-rtl.md).

### <a name="fonts-for-non-latin-languages"></a>Polices pour les langues non latines

<table>
<thead>
<tr class="header">
<th align="left">Famille de polices</th>
<th align="left">Styles</th>
<th align="left">Remarques</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="font-family: Embrima;">Ebrima</td>
<td align="left">Normal, Gras</td>
<td align="left">Police d’interface utilisateur pour les scripts d’Afrique (éthiopien, n’ko, osmanya, tifinagh, vaï).</td>
</tr>
<tr class="even">
<td style="font-family: Gadugi;">Gadugi</td>
<td align="left">Normal, Gras</td>
<td align="left">Police d’interface utilisateur pour les scripts d’Amérique du Nord (syllabiques canadiens, cherokee).</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Leelawadee UI;">Leelawadee UI</td>
<td align="left">Normal, Semi-fin, Gras</td>
<td align="left">Police d’interface utilisateur pour les scripts d’Asie du Sud-Est (bugi, lao, khmer, thaï).</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Malgun Gothic;">Malgun Gothic</td>
<td align="left">Normal</td>
<td align="left">Police d’interface utilisateur pour le coréen.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft JhengHei UI;">Microsoft JhengHei UI</td>
<td align="left">Normal, Gras, Fin</td>
<td align="left">Police d’interface utilisateur pour le chinois traditionnel.</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Microsoft YaHei UI;">Microsoft YaHei UI</td>
<td align="left">Normal, Gras, Fin</td>
<td align="left">Police d’interface utilisateur pour le chinois simplifié.</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Myanmar Text;">Myanmar Text</td>
<td align="left">Normal</td>
<td align="left">Police de substitution pour le script du Myanmar.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Nirmala UI;">Nirmala UI</td>
<td align="left">Normal, Semilight, Gras</td>
<td align="left">Police d’interface utilisateur pour les scripts d’Asie du Sud (bangla, dévanâgarî, goudjrati, gurmukhi, kannada, malayalam, odia, ol tchiki, cinghalais, sora sompeng, tamoul et telugu)</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: SimSun;">SimSun</td>
<td align="left">Normal</td>
<td align="left">Police Chinese UI héritée. </td>
</tr>
<tr class="even">
<td align="left" style="font-family: Yu Gothic UI;">Yu Gothic UI</td>
<td align="left">Clair, Semi-clair, Normal, Semi-gras, Gras</td>
<td align="left">Police d’interface utilisateur pour le japonais.</td>
</tr>
</tbody>
</table>

## <a name="fonts"></a>Polices

### <a name="sans-serif-fonts"></a>Polices sans-serif

Les polices sans-serif sont un excellent choix pour les titres et les éléments d’interface utilisateur. 

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Famille de polices</th>
<th align="left">Styles</th>
<th align="left">Remarques</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left" style="font-family: Arial;">Arial</td>
<td align="left">Normal, Italique, Gras, Italique gras, Noir</td>
<td align="left">Prend en charge les scripts d’Europe et du Moyen-Orient (latin, grec, cyrillique, arabe, arménien et hébreu). L’épaisseur de police Noir prend uniquement en charge les scripts d’Europe.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Calibri;">Calibri</td>
<td align="left">Normal, Italique, Gras, Italique gras, Fin, Italique fin</td>
<td align="left">Prend en charge les scripts d’Europe et du Moyen-Orient (latin, grec, cyrillique, arabe et hébreu). Arabe disponible uniquement en écriture verticale.</td>
</tr>
<td style="font-family: Consolas;">Consolas</td>
<td>Normal, Italique, Gras, Italique gras</td>
<td>Police de largeur fixe qui prend en charge les scripts d’Europe (latin, grec et cyrillique).</td>
</tr>

<tr>
<td style="font-family: Segoe UI;">Segoe UI</td>
<td>Normal, Italique, Italique fin, Italique noir, Gras, Italique gras, Fin, Semi-fin, Semi-gras, Noir</td>
<td>Police d’interface utilisateur pour les scripts d’Europe et du Moyen-Orient (arabe, arménien, cyrillique, géorgien, grec, hébreu, latin) et le script lisu.</td>
</tr>

<tr class="even">
<td style="font-family: Selawik;">Selawik</td>
<td align="left">Normal, Semi-fin, Fin, Gras, Semi-gras</td>
<td align="left">Police open source métriquement compatible avec Segoe UI destinée aux applications sur d’autres plateformes qui ne veulent pas intégrer Segoe UI. <a href="https://github.com/Microsoft/Selawik">Obtenir Selawik sur GitHub.</a></td>
</tr>

</tbody>
</table>

### <a name="serif-fonts"></a>Polices serif

Les polices serif sont parfaites pour présenter de grandes quantités de texte. 

<table>
<thead>
<tr class="header">
<th align="left">Famille de polices</th>
<th align="left">Styles</th>
<th align="left">Remarques</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="font-family: Cambria;">Cambria</td>
<td align="left">Normal</td>
<td align="left">Police serif qui prend en charge les scripts d’Europe (latin, grec et cyrillique).</td>
</tr>
<tr class="even">
<td style="font-family: Courier New;">Courier New</td>
<td align="left">Normal, Italique, Gras, Italique gras</td>
<td align="left">Police serif à largeur fixe qui prend en charge les scripts d’Europe et du Moyen-Orient (latin, grec, cyrillique, arabe, arménien et hébreu).</td>
</tr>
<tr class="odd">
<td style="font-family: Georgia;">Georgia</td>
<td align="left">Normal, Italique, Gras, Italique gras</td>
<td align="left">Prend en charge les scripts d’Europe (latin, grec et cyrillique).</td>
</tr>

<tr class="even">
<td style="font-family: Times New Roman;">Times New Roman</td>
<td align="left">Normal, Italique, Gras, Italique gras</td>
<td align="left">Police héritée qui prend en charge les scripts d’Europe (latin, grec, cyrillique, arabe, arménien, hébreu).</td>
</tr>

</tbody>
</table>

### <a name="symbols-and-icons"></a>Symboles et icônes

<table>
<thead>
<tr class="header">
<th align="left">Famille de polices</th>
<th align="left">Styles</th>
<th align="left">Remarques</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Segoe MDL2 Assets</td>
<td align="left">Normal</td>
<td align="left">Police d’interface utilisateur pour les icônes d’application. Pour plus d’informations, voir l’article <a href="segoe-ui-symbol-font.md">Recommandations en matière d’icônes Segoe MDL2</a>.</td>
</tr>
<tr class="even">
<td align="left">Segoe UI Emoji</td>
<td align="left">Normal</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Segoe UI Symbol</td>
<td align="left">Normal</td>
<td align="left">Police de substitution pour les symboles</td>
</tr>
</tbody>
</table>

## <a name="related-articles"></a>Articles associés

* [Contrôles de texte](../controls-and-patterns/text-controls.md)
* [Ressources de thème XAML](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp)
* [Styles XAML](../controls-and-patterns/xaml-styles.md)
* [Typographie Microsoft](https://docs.microsoft.com/typography/)