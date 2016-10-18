---
author: Jwmsft
Description: "Utilisez un contrôle RichTextBlock avec des éléments RichTextBlockOverflow pour créer des dispositions avancées de texte."
title: RichTextBlock
ms.assetid: E4BE4B1B-418E-4075-88F1-22C09DDF8E45
label: Rich text block
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 82c7e80afde143d7d12bbf4fe49aa2c52f244f6f

---
# Bloc de texte enrichi

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Les blocs de texte enrichi fournissent plusieurs fonctionnalités de disposition de texte avancée, que vous pouvez utiliser lorsque vous devez prendre en charge des paragraphes, des éléments d’interface utilisateur inline ou des dispositions de texte complexes.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.aspx"><strong>Classe RichTextBlock</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx"><strong>Classe RichTextBlockOverflow</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx"><strong>Classe Paragraph</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx"><strong>Classe Typography</strong></a></li>
</ul>

</div>
</div>







## Est-ce le contrôle approprié?

Utilisez un élément **RichTextBlock** si vous devez prendre en charge plusieurs paragraphes, du texte sur plusieurs colonnes, d’autres dispositions de texte complexes ou des éléments d’interface utilisateur inline, tels que des images.

Utilisez un **TextBlock** pour afficher la plupart de votre texte en lecture seule dans votre application. Vous pouvez l’utiliser pour afficher une ou plusieurs lignes de texte, des liens hypertexte inline et du texte mis en forme comme le gras, l’italique ou le souligné. TextBlock fournit un modèle de contenu plus simple, donc généralement plus facile à utiliser, et peut offrir de meilleures performances de rendu de texte que RichTextBlock. Il est souvent privilégié pour le texte de l’interface utilisateur des applications. Même si vous pouvez placer des sauts de ligne dans le texte, TextBlock est conçu pour afficher un seul paragraphe et ne prend pas en charge le retrait du texte.

Pour plus d’informations sur le choix du contrôle de texte approprié, voir l’article [Contrôles de texte](text-controls.md).

## Exemples


## Créer un bloc de texte enrichi

La propriété de contenu de RichTextBlock est la propriété [Blocks](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.blocks.aspx), qui prend en charge du texte basé sur un paragraphe via l’élément [Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx). Elle ne présente pas de propriété **Text** permettant d’accéder facilement au contenu de texte du contrôle dans votre application. Toutefois, RichTextBlock propose plusieurs fonctionnalités uniques que TextBlock ne fournit pas. 

RichTextBlock prend en charge les éléments suivants:
- Paragraphes multiples. Définissez le retrait de paragraphes en définissant la propriété [TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.textindent.aspx).
- Éléments d’interface utilisateur inline. Utilisez un élément [InlineUIContainer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.inlineuicontainer.aspx) pour afficher des éléments d’interface utilisateur, tels que des images et des éléments inline avec votre texte.
- Conteneurs de débordement. Utilisez des éléments [RichTextBlockOverflow](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx) pour créer des dispositions de texte sur plusieurs colonnes.

### Paragraphes

Vous utilisez des éléments [**Paragraph**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx) pour définir les blocs de texte à afficher dans un contrôle RichTextBlock. Chaque RichTextBlock doit inclure au moins un Paragraph. 

Vous pouvez définir la quantité de retraits pour tous les paragraphes d’un RichTextBlock en définissant la propriété [RichTextBlock.TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.textindent.aspx). Vous pouvez ignorer ce paramètre pour certains paragraphes d’un RichTextBlock en définissant la propriété [Paragraph.TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.textindent.aspx) sur une valeur différente.

```xaml
<RichTextBlock TextIndent="12">
  <Paragraph TextIndent="24">First paragraph.</Paragraph>
  <Paragraph>Second paragraph.</Paragraph>
  <Paragraph>Third paragraph. <Bold>With an inline.</Bold></Paragraph>
</RichTextBlock>
```

### Éléments d’interface utilisateur inline

La classe [**InlineUIContainer**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.inlineuicontainer.aspx) vous permet d’intégrer des éléments UIElement inline dans votre texte. Un scénario courant consiste à placer un élément Image inline dans votre texte. Vous pouvez également utiliser des éléments interactifs, tels que Button ou CheckBox.

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

## Conteneurs de débordement

Vous pouvez utiliser un contrôle RichTextBlock avec des éléments [**RichTextBlockOverflow**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx) pour créer des mises en forme sur plusieurs colonnes ou de page avancées. Le contenu d’un élément RichTextBlockOverflow est toujours issu d’un élément RichTextBlock. Vous liez des éléments RichTextBlockOverflow en les définissant comme les composants OverflowContentTarget d’un élément RichTextBlock ou d’un autre élément RichTextBlockOverflow.

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

## Mise en forme du texte

Bien que RichTextBlock stocke du texte brut, vous pouvez appliquer différentes options de mise en forme afin de personnaliser la manière dont le texte est restitué dans votre application. Vous pouvez définir des propriétés de contrôle standard comme FontFamily, FontSize, FontStyle, Foreground et CharacterSpacing pour modifier l’apparence du texte. Vous pouvez également utiliser des éléments de texte inline et des propriétés Typography associées pour mettre en forme votre texte. Ces options affectent uniquement la manière dont RichTextBlock affiche le texte localement, afin qu’aucune mise en forme ne soit appliquée si vous copiez et collez le texte dans un contrôle de texte enrichi, par exemple.

### Éléments inline

L’espace de noms [Windows.UI.Xaml.Documents](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.aspx) propose une variété d’éléments de texte inline utilisables pour mettre en forme votre texte, par exemple Bold, Italic, Run, Span, et LineBreak. Un moyen standard d’appliquer une mise en forme aux sections d’un texte consiste à placer le texte dans un élément Run ou Span, puis à définir les propriétés de cet élément.

Voici un Paragraph dont le texte de la première phrase est affiché en gras, bleu et 16points.

```xaml
<Paragraph>
    <Bold><Span Foreground="DarkSlateBlue" FontSize="16">Lorem ipsum dolor sit amet</Span></Bold>
    , consectetur adipiscing elit.
</Paragraph>
```

### Typographie

Les propriétés associées de la classe [Typography](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx) donnent accès à un ensemble de propriétés typographiques Microsoft OpenType. Vous pouvez définir ces propriétés associées sur RichTextBlock ou sur des éléments de texte inline individuels, comme indiqué ici.

```xaml
<RichTextBlock Typography.StylisticSet4="True">
    <Paragraph>
        <Span Typography.Capitals="SmallCaps">Lorem ipsum dolor sit amet</Span>
        , consectetur adipiscing elit.
    </Paragraph>
</RichTextBlock>
```

## Recommandations

Voir les articles Typographie et Recommandations concernant les polices.



## Articles connexes

[Contrôles de texte](text-controls.md)

**Pour les concepteurs**
- [Recommandations en matière de vérification orthographique](spell-checking-and-prediction.md)
- [Ajout de la fonctionnalité de recherche](https://msdn.microsoft.com/library/windows/apps/hh465231)
- [Recommandations en matière de saisie de texte](text-controls.md)

**Pour les développeurs (XAML)**
- [**Classe TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)
- [**Classe PasswordBox Windows.UI.Xaml.Controls**](https://msdn.microsoft.com/library/windows/apps/br227519)


**Pour les développeurs (autres)**
- [Propriété String.Length](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)



<!--HONumber=Aug16_HO3-->


