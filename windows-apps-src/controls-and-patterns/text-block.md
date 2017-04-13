---
author: Jwmsft
ms.assetid: DA562509-D893-425A-AAE6-B2AE9E9F8A19
Description: "Le bloc de texte est le contrôle principalement utilisé pour afficher du texte en lecture seule dans les applications."
title: Bloc de texte
label: Text block
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 904f0982deb596783ae886c26fee03c180d51987
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="text-block"></a>Bloc de texte

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

 Le bloc de texte est le contrôle principalement utilisé pour afficher du texte en lecture seule dans les applications. Ce contrôle vous permet d’afficher une ou plusieurs lignes de texte, des liens hypertexte inclus et du texte avec mise en forme de type gras, italique ou souligné.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**Classe TextBlock**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.aspx)</li>
<li>[**Propriété Text**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx)</li>
<li>[**Propriété Inlines**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.inlines.aspx)</li>
</ul>
</div>


## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

Un bloc de texte est généralement plus facile à utiliser et offre de meilleures performances en termes de rendu de texte qu’un bloc de texte enrichi. C’est pourquoi il est recommandé pour la plupart des textes d’interface utilisateur d’application. Vous pouvez facilement visualiser et utiliser le texte d’un bloc de texte dans votre application en obtenant la valeur de la propriété [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx). Il propose également de nombreuses options de mise en forme similaires pour personnaliser la manière dont votre texte est restitué.

Même si vous pouvez placer des sauts de ligne dans le texte, un bloc de texte est conçu pour afficher un seul paragraphe et ne prend pas en charge le retrait du texte. Utilisez un élément **RichTextBlock** si vous devez prendre en charge plusieurs paragraphes, du texte sur plusieurs colonnes, d’autres dispositions de texte complexes ou des éléments d’interface utilisateur inline, tels que des images.

Pour plus d’informations sur le choix du contrôle de texte approprié, voir l’article [Contrôles de texte](text-controls.md).

## <a name="create-a-text-block"></a>Créer un bloc de texte

Voici comment définir un contrôle TextBlock simple et sa propriété Text sur une chaîne.

```xaml
<TextBlock Text="Hello, world!" />
```

```csharp
TextBlock textBlock1 = new TextBlock();
textBlock1.Text = "Hello, world!";
```

    <TextBlock Text="Hello, world!" />

    TextBlock textBlock1 = new TextBlock();
    textBlock1.Text = "Hello, world!";

### <a name="content-model"></a>Modèle de contenu

Les deux propriétés utilisables pour ajouter du contenu à un élément TextBlock sont : [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx) et [Inlines](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.inlines.aspx).

La méthode la plus courante pour afficher du texte est de définir la propriété Text sur une valeur de chaîne, comme illustré dans l’exemple précédent.

Vous pouvez également ajouter du contenu en plaçant des éléments de contenu de flux inline dans la propriété TextBox.Inlines, comme suit.
```xaml
<TextBlock><Run>Text can be <Bold>bold</Bold>, <Italic>italic</Italic>, or <Bold><Italic>both</Italic></Bold>.</Run></TextBlock>
```

Les éléments dérivés de la classe Inline, tels que Bold, Italic, Run, Span et LineBreak activent une mise en forme différente pour les diverses parties du texte. Pour en savoir plus, voir la section [Mise en forme du texte](). L’élément Hyperlink inline vous permet d’ajouter un lien hypertexte à votre texte. Toutefois, l’utilisation de Inlines désactive également le rendu du texte du chemin rapide, qui est abordé dans la section suivante.


## <a name="performance-considerations"></a>Considérations relatives aux performances

Dans la mesure du possible, XAML utilise un chemin de code plus efficace pour la disposition du texte. Ce chemin rapide permet à la fois de diminuer l’utilisation de mémoire globale et de réduire considérablement le temps processeur requis pour mesurer et organiser le texte. Ce chemin rapide s’applique uniquement au contrôle TextBlock. Celui-ci doit donc être préféré au contrôle RichTextBlock dans la mesure du possible.

Certaines conditions exigent un contrôle TextBlock afin de revenir à un chemin de code plus riche en fonctionnalités et exigeant en ressources de processeur pour le rendu du texte. Pour conserver le rendu du texte sur le chemin rapide, veillez à suivre ces recommandations lors de la définition des propriétés répertoriées ici.
- [**Text**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx): la condition la plus importante est que le chemin rapide soit utilisé uniquement lorsque vous définissez le texte en définissant explicitement la propriété Text en XAML ou dans le code (comme illustré dans les exemples précédents). La définition du texte via la collection Inlines du contrôle TextBlock (telle que `<TextBlock>Inline text</TextBlock>`) désactive le chemin rapide en raison de la complexité potentielle liée à de multiples formats.
- [**CharacterSpacing**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.characterspacing.aspx): seule la valeur par défaut0 permet d’utiliser le chemin rapide.
- [**TextTrimming**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.texttrimming.aspx): seules les valeurs **None**, **CharacterEllipsis** et **WordEllipsis** permettent d’utiliser le chemin rapide. La valeur **Clip** désactive le chemin rapide.

> **Remarque**&nbsp;&nbsp;Dans les versions antérieures à Windows10, version1607, des propriétés supplémentaires ont également un impact sur le chemin rapide. Si votre application est exécutée dans une version antérieure de Windows, ces conditions entraînent également l’affichage de votre texte sur le chemin lent. Pour plus d’informations sur les versions, voir Code adaptatif de version.
- [**Typography**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx): seules les valeurs par défaut des différentes propriétés Typography permettent d’utiliser le chemin rapide.
- [**LineStackingStrategy**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.linestackingstrategy.aspx): si [LineHeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.lineheight.aspx) est différent de0, les valeurs **BaselineToBaseline** et **MaxHeight** désactivent le chemin rapide.
- [**IsTextSelectionEnabled**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.istextselectionenabled.aspx): seule la valeur **false** permet d’utiliser le chemin rapide. La définition de cette propriété sur **true** désactive le chemin rapide.

Vous pouvez définir la propriété [DebugSettings.IsTextPerformanceVisualizationEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.debugsettings.istextperformancevisualizationenabled.aspx) sur **true** pendant le débogage pour déterminer si le texte utilise le chemin rapide pour le rendu. Quand cette propriété est définie sur true, le texte qui utilise le chemin rapide s’affiche en vert clair.

>**Conseil**&nbsp;&nbsp;Cette fonctionnalité est expliquée en détail dans cette session à partir de la build2015 - [Performances XAML: techniques d’optimisation des expériences d’application Windows universelle générée en XAML](https://channel9.msdn.com/Events/Build/2015/3-698).



Vous définissez généralement les paramètres de débogage dans la substitution de méthode [OnLaunched](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.application.onlaunched.aspx) dans la page code-behind pour App.xaml, comme ceci:
```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
#if DEBUG
    if (System.Diagnostics.Debugger.IsAttached)
    {
        this.DebugSettings.IsTextPerformanceVisualizationEnabled = true;
    }
#endif

// ...

}
```

Dans cet exemple, le premier contrôle TextBlock est affiché à l’aide du chemin rapide, mais pas le second.
```xaml
<StackPanel>
    <TextBlock Text="This text is on the fast path."/>
    <TextBlock>This text is NOT on the fast path.</TextBlock>
<StackPanel/>
```

Lorsque vous exécutez ce code XAML en mode débogage avec la propriété IsTextPerformanceVisualizationEnabled définie sur true, le résultat se présente ainsi:

![Texte affiché en mode débogage](images/text-block-rendering-performance.png)

>**Attention**&nbsp;&nbsp;La couleur du texte qui n’est pas sur le chemin rapide n’est pas modifiée. Si vous avez du texte dans votre application dont la couleur spécifiée est vert clair, cette couleur reste inchangée quand il est sur le chemin de rendu plus lent. Ne pas confondre le texte défini comme étant vert dans l’application avec le texte du chemin d’accès rapide apparaissant en vert en raison des paramètres de débogage.

## <a name="formatting-text"></a>Mise en forme du texte

Bien que la propriété Text stocke du texte brut, vous pouvez appliquer différentes options de mise en forme au contrôle TextBlock afin de personnaliser la manière dont le texte est restitué dans votre application. Vous pouvez définir des propriétés de contrôle standard comme FontFamily, FontSize, FontStyle, Foreground et CharacterSpacing pour modifier l’apparence du texte. Vous pouvez également utiliser des éléments de texte inline et des propriétés Typography associées pour mettre en forme votre texte. Ces options affectent uniquement la manière dont TextBlock affiche le texte localement. Par conséquent, si vous copiez et collez le texte dans un contrôle de texte enrichi, par exemple, aucune mise en forme n’est appliquée.

>**Remarque**&nbsp;&nbsp;N’oubliez pas, comme indiqué dans la section précédente, que les éléments de texte insérés et les valeurs typographiques par défaut ne sont pas restitués sur le chemin rapide.


### <a name="inline-elements"></a>Éléments insérés

L’espace de noms [Windows.UI.Xaml.Documents](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.aspx) propose une variété d’éléments de texte inline utilisables pour mettre en forme votre texte, par exemple Bold, Italic, Run, Span, et LineBreak.

Vous pouvez afficher, dans un contrôle TextBlock, une série de chaînes présentant chacune une mise en forme différente. Pour ce faire, utilisez un élément Run pour afficher chaque chaîne avec sa mise en forme et séparez chaque élément Run par un élément LineBreak.

Voici comment définir, dans un contrôle TextBlock, plusieurs chaînes de texte dont la mise en forme diffère à l’aide d’objets Run séparés par un élément LineBreak.
```xaml
<TextBlock FontFamily="Arial" Width="400" Text="Sample text formatting runs">
    <LineBreak/>
    <Run Foreground="Gray" FontFamily="Courier New" FontSize="24">
        Courier New 24
    </Run>
    <LineBreak/>
    <Run Foreground="Teal" FontFamily="Times New Roman" FontSize="18" FontStyle="Italic">
        Times New Roman Italic 18
    </Run>
    <LineBreak/>
    <Run Foreground="SteelBlue" FontFamily="Verdana" FontSize="14" FontWeight="Bold">
        Verdana Bold 14
    </Run>
</TextBlock>
```

Résultat:

![Texte mis en forme avec des éléments Run](images/text-block-run-examples.png)

### <a name="typography"></a>Typographie

Les propriétés associées de la classe [Typography](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx) donnent accès à un ensemble de propriétés typographiques Microsoft OpenType. Vous pouvez définir ces propriétés associées sur TextBlock ou sur des éléments de texte inline individuels. Ces exemples portent sur les deux types.
```xaml
<TextBlock Text="Hello, world!"
           Typography.Capitals="SmallCaps"
           Typography.StylisticSet4="True"/>
```

```csharp
TextBlock textBlock1 = new TextBlock();
textBlock1.Text = "Hello, world!";
Windows.UI.Xaml.Documents.Typography.SetCapitals(textBlock1, FontCapitals.SmallCaps);
Windows.UI.Xaml.Documents.Typography.SetStylisticSet4(textBlock1, true);
```

```xaml
<TextBlock>12 x <Run Typography.Fraction="Slashed">1/3</Run> = 4.</TextBlock>
```


## <a name="related-articles"></a>Articles connexes

- [Contrôles de texte](text-controls.md)
- [Recommandations en matière de vérification orthographique](spell-checking-and-prediction.md)
- [Ajout de la fonctionnalité de recherche](search.md)
- [Recommandations en matière de saisie de texte](text-controls.md)
- [**Classe TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)
- [**Classe PasswordBox Windows.UI.Xaml.Controls**](https://msdn.microsoft.com/library/windows/apps/br227519)
- [Propriété String.Length](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)
