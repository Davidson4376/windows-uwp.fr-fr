---
author: Jwmsft
Description: Use a RichTextBlock with RichTextBlockOverflow elements to create advanced text layouts.
title: RichTextBlock
ms.assetid: E4BE4B1B-418E-4075-88F1-22C09DDF8E45
label: Rich text block
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 16ebc375a72984af8bbc40823121d2d94689fcf2
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6666897"
---
# <a name="rich-text-block"></a>Bloc de texte enrichi

 

Les blocs de texte enrichi fournissent plusieurs fonctionnalités de disposition de texte avancée, que vous pouvez utiliser lorsque vous devez prendre en charge des paragraphes, des éléments d’interface utilisateur inline ou des dispositions de texte complexes.

> **API importantes**: [classe RichTextBlock](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.aspx), [classe RichTextBlockOverflow](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx), [classe Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx), [classe Typography](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

Utilisez un élément **RichTextBlock** si vous devez prendre en charge plusieurs paragraphes, du texte sur plusieurs colonnes, d’autres dispositions de texte complexes ou des éléments d’interface utilisateur inline, tels que des images.

Utilisez un **TextBlock** pour afficher la plupart de votre texte en lecture seule dans votre application. Vous pouvez l’utiliser pour afficher une ou plusieurs lignes de texte, des liens hypertexte inline et du texte mis en forme comme le gras, l’italique ou le souligné. TextBlock fournit un modèle de contenu plus simple, donc généralement plus facile à utiliser, et peut offrir de meilleures performances de rendu de texte que RichTextBlock. Il est souvent privilégié pour le texte de l’interface utilisateur des applications. Même si vous pouvez placer des sauts de ligne dans le texte, TextBlock est conçu pour afficher un seul paragraphe et ne prend pas en charge le retrait du texte.

Pour plus d’informations sur le choix du contrôle de texte approprié, voir l’article [Contrôles de texte](text-controls.md).

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/RichTextBlock">ouvrir l’application et voir l'objet RichTextBlock en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-rich-text-block"></a>Créer un bloc de texte enrichi

La propriété de contenu de RichTextBlock est [Blocks](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.blocks.aspx). Elle prend en charge le texte basé sur un paragraphe via l’élément [Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx). Elle ne présente pas de propriété **Text** permettant d’accéder facilement au contenu de texte du contrôle dans votre application. Toutefois, RichTextBlock propose plusieurs fonctionnalités uniques que TextBlock ne fournit pas. 

RichTextBlock prend en charge les éléments suivants:
- Paragraphes multiples. Définissez le retrait de paragraphes en définissant la propriété [TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.textindent.aspx).
- Éléments d’interface utilisateur inline. Utilisez un élément [InlineUIContainer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.inlineuicontainer.aspx) pour afficher des éléments d’interface utilisateur, tels que des images et des éléments inline avec votre texte.
- Conteneurs de débordement. Utilisez des éléments [RichTextBlockOverflow](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx) pour créer des dispositions de texte sur plusieurs colonnes.

### <a name="paragraphs"></a>Paragraphes

Vous utilisez des éléments [Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx) pour définir les blocs de texte à afficher dans un contrôle RichTextBlock. Chaque RichTextBlock doit inclure au moins un Paragraph. 

Vous pouvez définir la quantité de retraits pour tous les paragraphes d’un RichTextBlock en définissant la propriété [RichTextBlock.TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.textindent.aspx). Vous pouvez ignorer ce paramètre pour certains paragraphes d’un RichTextBlock en définissant la propriété [Paragraph.TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.textindent.aspx) sur une valeur différente.

```xaml
<RichTextBlock TextIndent="12">
  <Paragraph TextIndent="24">First paragraph.</Paragraph>
  <Paragraph>Second paragraph.</Paragraph>
  <Paragraph>Third paragraph. <Bold>With an inline.</Bold></Paragraph>
</RichTextBlock>
```

### <a name="inline-ui-elements"></a>Éléments d’interface utilisateur inline

La classe [InlineUIContainer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.inlineuicontainer.aspx) vous permet d’intégrer des éléments UIElement inline dans votre texte. Un scénario courant consiste à placer un élément Image inline dans votre texte. Vous pouvez également utiliser des éléments interactifs, tels que Button ou CheckBox.

Si vous voulez intégrer plusieurs éléments inline dans la même position, envisagez d’utiliser un panneau comme enfant InlineUIContainer unique, puis placez plusieurs éléments dans ce panneau.

Cet exemple montre comment utiliser un InlineUIContainer pour insérer une image dans un RichTextBlock. 

```xaml
<RichTextBlock>
    <Paragraph>
        <Italic>This is an inline image.</Italic>
        <InlineUIContainer>
            <Image Source="Assets/Square44x44Logo.png" Height="30" Width="30"/>
        </InlineUIContainer>
        Mauris auctor tincidunt auctor.
    </Paragraph>
</RichTextBlock>
```

## <a name="overflow-containers"></a>Conteneurs de débordement

Vous pouvez utiliser un contrôle RichTextBlock avec des éléments [RichTextBlockOverflow](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx) pour créer des mises en forme sur plusieurs colonnes ou de page avancées. Le contenu d’un élément RichTextBlockOverflow est toujours issu d’un élément RichTextBlock. Vous liez des éléments RichTextBlockOverflow en les définissant comme les composants OverflowContentTarget d’un élément RichTextBlock ou d’un autre élément RichTextBlockOverflow.

Voici un exemple simple qui crée une disposition à deux colonnes. Consultez la section Exemples pour obtenir un exemple plus complexe.

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <RichTextBlock Grid.Column="0" 
                   OverflowContentTarget="{Binding ElementName=overflowContainer}" >
        <Paragraph>
            Proin ac metus at quam luctus ultricies.
        </Paragraph>
    </RichTextBlock>
    <RichTextBlockOverflow x:Name="overflowContainer" Grid.Column="1"/>
</Grid>
```

## <a name="formatting-text"></a>Mise en forme du texte

Bien que RichTextBlock stocke du texte brut, vous pouvez appliquer différentes options de mise en forme afin de personnaliser la manière dont le texte est restitué dans votre application. Vous pouvez définir des propriétés de contrôle standard comme FontFamily, FontSize, FontStyle, Foreground et CharacterSpacing pour modifier l’apparence du texte. Vous pouvez également utiliser des éléments de texte inline et des propriétés Typography associées pour mettre en forme votre texte. Ces options affectent uniquement la manière dont RichTextBlock affiche le texte localement, afin qu’aucune mise en forme ne soit appliquée si vous copiez et collez le texte dans un contrôle de texte enrichi, par exemple.

### <a name="inline-elements"></a>Éléments inline

L’espace de noms [Windows.UI.Xaml.Documents](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.aspx) propose une variété d’éléments de texte inline utilisables pour mettre en forme votre texte, par exemple Bold, Italic, Run, Span, et LineBreak. Un moyen standard d’appliquer une mise en forme aux sections d’un texte consiste à placer le texte dans un élément Run ou Span, puis à définir les propriétés de cet élément.

Voici un Paragraph dont le texte de la première phrase est affiché en gras, bleu et 16points.

```xaml
<Paragraph>
    <Bold><Span Foreground="DarkSlateBlue" FontSize="16">Lorem ipsum dolor sit amet</Span></Bold>
    , consectetur adipiscing elit.
</Paragraph>
```

### <a name="typography"></a>Typographie

Les propriétés associées de la classe [Typography](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx) donnent accès à un ensemble de propriétés typographiques Microsoft OpenType. Vous pouvez définir ces propriétés associées sur RichTextBlock ou sur des éléments de texte inline individuels, comme indiqué ici.

```xaml
<RichTextBlock Typography.StylisticSet4="True">
    <Paragraph>
        <Span Typography.Capitals="SmallCaps">Lorem ipsum dolor sit amet</Span>
        , consectetur adipiscing elit.
    </Paragraph>
</RichTextBlock>
```

## <a name="recommendations"></a>Recommandations

Voir les articles Typographie et Recommandations concernant les polices.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles connexes

[Contrôles de texte](text-controls.md)

**Pour les concepteurs**
- [Recommandations en matière de vérification orthographique](text-controls.md)
- [Ajout de la fonctionnalité de recherche](https://msdn.microsoft.com/library/windows/apps/hh465231)
- [Recommandations en matière de saisie de texte](text-controls.md)

**Pour les développeurs (XAML)**
- [Classe TextBox](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Classe PasswordBox Windows.UI.Xaml.Controls](https://msdn.microsoft.com/library/windows/apps/br227519)


**Pour les développeurs (autres)**
- [Propriété String.Length](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)
