---
Description: Utilisez un contrôle RichTextBlock avec des éléments RichTextBlockOverflow pour créer des dispositions avancées de texte.
title: RichTextBlock
ms.assetid: E4BE4B1B-418E-4075-88F1-22C09DDF8E45
label: Rich text block
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 71f8298456b3c297994d6aa11d815a6b46ba7ff4
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340363"
---
# <a name="rich-text-block"></a>Bloc de texte enrichi

 

Les blocs de texte enrichi fournissent plusieurs fonctionnalités de disposition de texte avancée, que vous pouvez utiliser lorsque vous devez prendre en charge des paragraphes, des éléments d’interface utilisateur inline ou des dispositions de texte complexes.

> **API importantes** : [classe RichTextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock), [classe RichTextBlockOverflow](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlockOverflow), [classe Paragraph](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.Paragraph), [classe Typography](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.Typography)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Utilisez un élément **RichTextBlock** si vous devez prendre en charge plusieurs paragraphes, du texte sur plusieurs colonnes, d’autres dispositions de texte complexes ou des éléments d’interface utilisateur inline, tels que des images.

Pour afficher la majeure partie de votre texte en lecture seule dans votre application, utilisez un contrôle **TextBlock**. Ce contrôle vous permet d’afficher une ou plusieurs lignes de texte, des liens hypertexte inclus et du texte avec mise en forme de type gras, italique ou souligné. TextBlock fournit un modèle de contenu plus simple, donc généralement plus facile à utiliser, et peut offrir de meilleures performances de rendu de texte que RichTextBlock. Il est souvent privilégié pour le texte de l’interface utilisateur des applications. Même si vous pouvez insérer des sauts de ligne dans le texte, TextBlock est conçu pour n’afficher qu’un seul paragraphe et ne prend pas en charge le retrait du texte.

Pour plus d’informations sur le choix du contrôle de texte approprié, voir l’article [Contrôles de texte](text-controls.md).

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/RichTextBlock">ouvrir l’application et voir l’objet RichTextBlock en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-rich-text-block"></a>Créer un bloc de texte enrichi

La propriété de contenu de RichTextBlock est [Blocks](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richtextblock.blocks). Elle prend en charge le texte basé sur un paragraphe via l’élément [Paragraph](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.Paragraph). Elle ne présente pas de propriété **Text** vous permettant d’accéder facilement au contenu de texte du contrôle dans votre application. Toutefois, RichTextBlock propose plusieurs fonctionnalités uniques que TextBlock ne fournit pas. 

RichTextBlock prend en charge les éléments suivants :
- Paragraphes multiples. Définissez le retrait de paragraphes en définissant la propriété [TextIndent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richtextblock.textindent).
- Éléments d’interface utilisateur inline. Utilisez un élément [InlineUIContainer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.InlineUIContainer) pour afficher des éléments d’interface utilisateur, tels que des images et des éléments inline avec votre texte.
- Conteneurs de débordement. Utilisez des éléments [RichTextBlockOverflow](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlockOverflow) pour créer des dispositions de texte sur plusieurs colonnes.

### <a name="paragraphs"></a>Paragraphes

Vous utilisez des éléments [Paragraph](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.Paragraph) pour définir les blocs de texte à afficher dans un contrôle RichTextBlock. Chaque RichTextBlock doit inclure au moins un Paragraph. 

Vous pouvez définir la quantité de retraits pour tous les paragraphes d’un RichTextBlock en définissant la propriété [RichTextBlock.TextIndent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richtextblock.textindent). Vous pouvez ignorer ce paramètre pour certains paragraphes d’un RichTextBlock en définissant la propriété [Paragraph.TextIndent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.paragraph.textindent) sur une valeur différente.

```xaml
<RichTextBlock TextIndent="12">
  <Paragraph TextIndent="24">First paragraph.</Paragraph>
  <Paragraph>Second paragraph.</Paragraph>
  <Paragraph>Third paragraph. <Bold>With an inline.</Bold></Paragraph>
</RichTextBlock>
```

### <a name="inline-ui-elements"></a>Éléments d’interface utilisateur inline

La classe [InlineUIContainer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.InlineUIContainer) vous permet d’intégrer des éléments UIElement inline dans votre texte. Un scénario courant consiste à placer un élément Image inline dans votre texte. Vous pouvez également utiliser des éléments interactifs, tels que Button ou CheckBox.

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

Vous pouvez utiliser un contrôle RichTextBlock avec des éléments [RichTextBlockOverflow](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlockOverflow) pour créer des mises en forme sur plusieurs colonnes ou de page avancées. Le contenu d’un élément RichTextBlockOverflow est toujours issu d’un élément RichTextBlock. Vous liez des éléments RichTextBlockOverflow en les définissant comme les composants OverflowContentTarget d’un élément RichTextBlock ou d’un autre élément RichTextBlockOverflow.

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

L’espace de noms [Windows.UI.Xaml.Documents](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents) propose une variété d’éléments de texte inline utilisables pour mettre en forme votre texte, par exemple Bold, Italic, Run, Span, et LineBreak. Un moyen standard d’appliquer une mise en forme aux sections d’un texte consiste à placer le texte dans un élément Run ou Span, puis à définir les propriétés de cet élément.

Voici un Paragraph dont le texte de la première phrase est affiché en gras, bleu et 16 points.

```xaml
<Paragraph>
    <Bold><Span Foreground="DarkSlateBlue" FontSize="16">Lorem ipsum dolor sit amet</Span></Bold>
    , consectetur adipiscing elit.
</Paragraph>
```

### <a name="typography"></a>Typographie

Les propriétés associées de la classe [Typography](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.Typography) donnent accès à un ensemble de propriétés typographiques Microsoft OpenType. Vous pouvez définir ces propriétés associées sur RichTextBlock ou sur des éléments de texte inline individuels, comme indiqué ici.

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

- [Exemple de Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) : tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles connexes

[Contrôles de texte](text-controls.md)

**Pour les concepteurs**
- [Recommandations sur la vérification orthographique](text-controls.md)
- [Ajout de la fonctionnalité de recherche](https://docs.microsoft.com/previous-versions/windows/apps/hh465231(v=win.10))
- [Recommandations sur la saisie de texte](text-controls.md)

**Pour les développeurs (XAML)**
- [TextBox, classe](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)
- [PasswordBox Windows.UI.Xaml.Controls, classe](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)


**Pour les développeurs (autres)**
- [String.Length, propriété](https://docs.microsoft.com/dotnet/api/system.string.length)
