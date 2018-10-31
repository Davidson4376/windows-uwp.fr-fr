---
author: jwmsft
Description: Use content links to embed rich data in your text controls.
title: Liens de contenu dans les contrôles de texte
label: Content links
template: detail.hbs
ms.author: jimwalk
ms.date: 03/07/2018
ms.topic: article
keywords: windows10, uwp
pm-contact: miguelrb
design-contact: ''
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 5f8a63e91bd5415f33118294a03567bb5e670ae2
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5839820"
---
# <a name="content-links-in-text-controls"></a>Liens de contenu dans les contrôles de texte

Les liens de contenu offrent un moyen d'incorporer des données enrichies dans vos contrôles de texte, ce qui permet à l’utilisateur de trouver et d’utiliser plus d’informations sur une personne ou un lieu, sans quitter le contexte de votre application.

Lorsque l’utilisateur préfixe une entrée avec un symbole esperluette (@) dans un contrôle RichEditBox, une liste de suggestions de personnes et/ou de lieux correspondant à l’entrée s'affiche. Si, par exemple, l’utilisateur sélectionne un lieu, un lien de type ContentLink vers ce lieu est inséré dans le texte. Si l’utilisateur appelle le lien de contenu dans le contrôle RichEditBox, un menu volant s’affiche avec une carte et des informations supplémentaires sur le lieu en question.

> **API importantes**: [classe ContentLink](/uwp/api/windows.ui.xaml.documents.contentlink), [classe ContentLinkInfo](/uwp/api/windows.ui.text.contentlinkinfo), [classe RichEditTextRange](/uwp/api/windows.ui.text.richedittextrange)

> [!NOTE]
> Les API de liens de contenu sont réparties dans les espaces de noms suivants: Windows.UI.Xaml.Controls, Windows.UI.Xaml.Documents et Windows.UI.Text.



## <a name="content-links-in-rich-edit-vs-text-block-controls"></a>Liens de contenu dans une édition enrichie par opposition aux contrôles de bloc de texte

Vous pouvez utiliser les liens de contenu de deux façons:

1. Dans un [RichEditBox](/uwp/api/windows.ui.xaml.controls.richeditbox), l’utilisateur peut ouvrir un sélecteur pour ajouter un lien de contenu en préfixant le texte avec un symbole @. Le lien de contenu est stocké dans le cadre du contenu texte enrichi.
1. Dans un contrôle [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) ou [RichTextBlock](/uwp/api/windows.ui.xaml.controls.richtextblock), le lien de contenu est un élément de texte dont l'utilisation et le comportement sont très similaires à ceux d'un [Lien hypertexte](/uwp/api/windows.ui.xaml.documents.hyperlink).

Voici à quoi ressemblent les liens de contenu par défaut dans un contrôle RichEditBox et dans un contrôle TextBlock.

![lien de contenu dans une zone d’édition enrichie](images/content-link-default-richedit.png)
![lien de contenu dans un bloc de texte](images/content-link-default-textblock.png)

Les différences d’utilisation, de rendu et de comportement sont traitées en détail dans les sections suivantes. Ce tableau vous donne une comparaison rapide des principales différences entre un lien de contenu dans un contrôle RichEditBox et dans un bloc de texte.

| Fonctionnalité   | RichEditBox | bloc de texte |
| --------- | ----------- | ---------- |
| Utilisation | Instance ContentLinkInfo | Élément de texte ContentLink |
| Curseur | Déterminé par type de lien de contenu, ne peut pas être modifié. | Déterminé par la propriété Cursor, **null** par défaut |
| Info-bulle | Pas de rendu | Affiche un texte secondaire |

## <a name="enable-content-links-in-a-richeditbox"></a>Activer les liens de contenu dans un contrôle RichEditBox

L’utilisation d’un lien de contenu la plus courante consiste à permettre à un utilisateur d’ajouter rapidement des informations en préfixant le nom de la personne ou du lieu avec un symbole esperluette (@) dans son texte. Lors de l'activation dans un contrôle [RichEditBox](/uwp/api/windows.ui.xaml.controls.richeditbox), cela ouvre un sélecteur et permet à l’utilisateur d’insérer une personne à partir de sa liste de contacts, ou un lieu à proximité, selon ce que vous avez activé.

Le lien de contenu peut être enregistré avec le contenu de texte enrichi, et vous pouvez l'extraire pour l'utiliser dans d’autres parties de votre application. Par exemple, dans une application de messagerie, vous pouvez extraire les informations de la personne et les utiliser pour remplir la zone À avec une adresse e-mail.

> [!NOTE]
> Le sélecteur de lien de contenu est une application qui fait partie de Windows. Il s'exécute donc dans un processus distinct de votre application.

### <a name="content-link-providers"></a>Fournisseurs de liens de contenu

Vous activez des liens de contenu dans un contrôle RichEditBox en ajoutant un ou plusieurs fournisseurs de liens de contenu à la collection [RichEditBox.ContentLinkProviders](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkProviders). Deuxfournisseurs de liens de contenu sont intégrés à l’infrastructure XAML.

- [ContactContentLinkProvider](/uwp/api/windows.ui.xaml.documents.contactcontentlinkprovider): recherche des contacts à l’aide de l'application **Contacts**.
- [PlaceContentLinkProvider](/uwp/api/windows.ui.xaml.documents.placecontentlinkprovider): recherche des lieux à l’aide de l'application **Cartes**.

> [!IMPORTANT]
> La valeur par défaut de la propriété RichEditBox.ContentLinkProviders est **null**, non une collection vide. Vous avez besoin de créer explicitement la [ContentLinkProviderCollection](/uwp/api/windows.ui.xaml.documents.contentlinkprovidercollection) avant d’ajouter des fournisseurs de liens de contenu.

Voici comment ajouter les fournisseurs de liens de contenu en XAML.

```xaml
<RichEditBox>
    <RichEditBox.ContentLinkProviders>
        <ContentLinkProviderCollection>
            <ContactContentLinkProvider/>
            <PlaceContentLinkProvider/>
        </ContentLinkProviderCollection>
    </RichEditBox.ContentLinkProviders>
</RichEditBox>
```

Vous pouvez également ajouter des fournisseurs de liens de contenu dans un Style et l’appliquer à plusieurs RichEditBoxes comme suit.

```xaml
<Page.Resources>
    <Style TargetType="RichEditBox" x:Key="ContentLinkStyle">
        <Setter Property="ContentLinkProviders">
            <Setter.Value>
                <ContentLinkProviderCollection>
                    <PlaceContentLinkProvider/>
                    <ContactContentLinkProvider/>
                </ContentLinkProviderCollection>
            </Setter.Value>
        </Setter>
    </Style>
</Page.Resources>

<RichEditBox x:Name="RichEditBox01" Style="{StaticResource ContentLinkStyle}" />
<RichEditBox x:Name="RichEditBox02" Style="{StaticResource ContentLinkStyle}" />
```

Voici comment ajouter des fournisseurs de liens de contenu dans le code.

```csharp
RichEditBox editor = new RichEditBox();
editor.ContentLinkProviders = new ContentLinkProviderCollection
{
    new ContactContentLinkProvider(),
    new PlaceContentLinkProvider()
};
```

### <a name="content-link-colors"></a>Couleurs de lien de contenu

L’apparence d’un lien de contenu est déterminée par son premier plan, son arrière-plan et son icône. Dans un contrôle RichEditBox, vous pouvez définir les propriétés [ContentLinkForegroundColor](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkForegroundColor) et [ContentLinkBackgroundColor](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkBackgroundColor) pour modifier les couleurs par défaut. 

Vous ne pouvez pas définir le curseur. Le curseur est restitué par le contrôle RichEditbox en fonction du type de lien de contenu - un curseur [Person](/uwp/api/windows.ui.core.corecursortype) pour un lien vers une personne, ou un curseur [Pin](/uwp/api/windows.ui.core.corecursortype) pour un lien vers un lieu.

### <a name="the-contentlinkinfo-object"></a>L’objet ContentLinkInfo

Lorsque l’utilisateur effectue une sélection dans le sélecteur de personnes ou de lieux, le système crée un objet [ContentLinkInfo](/uwp/api/windows.ui.text.contentlinkinfo) et l’ajoute à la propriété **ContentLinkInfo** de l'actuel objet [RichEditTextRange](/uwp/api/windows.ui.text.richedittextrange).

L’objet ContentLinkInfo contient les informations qui permettent d'afficher, d'appeler et de gérer le lien de contenu.

- **DisplayText**: il s’agit de la chaîne qui s’affiche lorsque le lien de contenu est restitué. Dans un contrôle RichEditBox, l’utilisateur peut modifier le texte d’un lien de contenu après sa création, ce qui modifie la valeur de cette propriété.
- **SecondaryText**: cette chaîne est affichée dans l’info-bulle d’un lien de contenu rendu.
  - Dans un lien de contenu de type Place créé par le sélecteur, elle contient l’adresse du lieu, si disponible.
- **Uri**: lien vers plus d’informations sur l’objet de la liaison de contenu. Cet Uri peut ouvrir une application installée ou un site Web.
- **Id**: il s’agit d'un compteur par contrôle en lecture seule, créé par le contrôle RichEditBox. Il sert à suivre cette propriété ContentLinkInfo au cours d'actions telles que la suppression ou la modification. Si la propriété ContentLinkInfo est coupée et recollée dans le contrôle, elle obtiendra de nouvelles valeurs d’Id. Les valeurs d'ID sont incrémentielles.
- **LinkContentKind**: chaîne qui décrit le type de liaison de contenu. Les types de contenu intégrés sont _Places_ et _Contacts_. La valeur est sensible à la casse.

#### <a name="link-content-kind"></a>Type de contenu de lien

Il existe plusieurs situations où le LinkContentKind est important.

- Lorsqu’un utilisateur copie un lien de contenu dans un contrôle RichEditBox et le colle dans un autre contrôle RichEditBox, les deux contrôles doivent avoir un ContentLinkProvider pour ce type de contenu. Si ce n’est pas le cas, le lien est collé sous forme de texte.
- Vous pouvez utiliser LinkContentKind dans un gestionnaire d’événements [ContentLinkChanged](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkChanged) pour déterminer comment traiter un lien de contenu lorsque vous l’utilisez dans d’autres parties de votre application. Voir la section Exemple pour plus d'informations.
- Le LinkContentKind influence la façon dont le système ouvre l’Uri lorsque le lien est appelé. Nous allons le voir ensuite dans la discussion sur le lancement d'Uri.

#### <a name="uri-launching"></a>Lancement d’URI

La propriété Uri fonctionne de façon similaire à la propriété NavigateUri d’un lien hypertexte. Lorsqu’un utilisateur clique dessus, elle lance l’Uri dans le navigateur par défaut, ou dans l’application enregistrée pour le protocole particulier spécifié dans la valeur de l’Uri.

Le comportement spécifique des 2types de lien de contenu intégrés sont décrits ici.

##### <a name="places"></a>Places

Le sélecteur Places crée une propriété ContentLinkInfo avec une racine d'Uri de https://maps.windows.com/. Ce lien peut être ouvert de 3manières différentes:

- Si LinkContentKind = «Places», il ouvre une fiche d’informations dans un menu volant. Le menu volant est similaire à celui du sélecteur de lien de contenu. Il fait partie de Windows et s’exécute dans un processus distinct de votre application.
- Si LinkContentKind n’est pas «Places», il tente d’ouvrir l'application **Cartes** à l’endroit spécifié. Par exemple, cela peut se produire si vous avez modifié le LinkContentKind dans le Gestionnaire d’événements ContentLinkChanged.
- Si l’Uri ne peut pas être ouvert dans l’application Cartes, la carte est ouverte dans le navigateur par défaut. Cela se produit généralement lorsque les paramètres _Applications pour les sites web_ de l’utilisateur n'autorisent pas l’ouverture de l’Uri avec l'application **Cartes**.

##### <a name="people"></a>People

Le sélecteur People crée un ContentLinkInfo avec un Uri qui utilise le protocole **ms-people**.

- Si LinkContentKind = «People», il ouvre une fiche d’informations dans un menu volant. Le menu volant est similaire à celui du sélecteur de lien de contenu. Il fait partie de Windows et s’exécute dans un processus distinct de votre application.
- Si LinkContentKind n’est pas «People», il ouvre l'application **Contacts**. Par exemple, cela peut se produire si vous avez modifié le LinkContentKind dans le Gestionnaire d’événements ContentLinkChanged.

> [!TIP]
> Pour plus d’informations sur l’ouverture d’autres applications et sites Web à partir de votre application, consultez les rubriques sous [lancer une application avec un Uri] (/windows/uwp/launch-resume/launch-app-with-uri).

#### <a name="invoked"></a>Invoked

Lorsque l’utilisateur appelle un lien de contenu, l'événement [ContentLinkInvoked](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkInvoked) se déclenche avant que le comportement par défaut du lancement de l’Uri ne se produise. Vous pouvez gérer cet événement pour remplacer ou annuler le comportement par défaut.

Cet exemple montre comment vous pouvez remplacer le comportement de lancement par défaut d’un lien de contenu de type Place. Au lieu d’ouvrir la carte dans une fiche d’informations Place, l'application Cartes ou le navigateur web par défaut, vous marquez l’événement comme Géré et ouvrez la carte dans un contrôle [WebView](/uwp/api/windows.ui.xaml.controls.webview) dans l’application.

```xaml
<RichEditBox x:Name="editor"
             ContentLinkInvoked="editor_ContentLinkInvoked">
    <RichEditBox.ContentLinkProviders>
        <ContentLinkProviderCollection>
            <PlaceContentLinkProvider/>
        </ContentLinkProviderCollection>
    </RichEditBox.ContentLinkProviders>
</RichEditBox>

<WebView x:Name="webView1"/>
```

```csharp
private void editor_ContentLinkInvoked(RichEditBox sender, ContentLinkInvokedEventArgs args)
{
    if (args.ContentLinkInfo.LinkContentKind == "Places")
    {
        args.Handled = true;
        webView1.Navigate(args.ContentLinkInfo.Uri);
    }
}
```

#### <a name="contentlinkchanged"></a>ContentLinkChanged

Vous pouvez utiliser l'événement [ContentLinkChanged](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkChanged) pour être informé lorsqu’un lien de contenu est ajouté, modifié ou supprimé. Cela vous permet d’extraire le lien de contenu du texte et de l’utiliser à d’autres endroits dans votre application. Cela est illustré plus loin dans la section Exemples.

Les propriétés de l'objet [ContentLinkChangedEventArgs](/uwp/api/windows.ui.xaml.controls.contentlinkchangedeventargs) sont:

- **ChangedKind**: cette énumération ContentLinkChangeKind est l’action contenue dans le ContentLink. Par exemple, si le ContentLink est inséré, supprimé ou modifié.
- **Info**: le ContentLinkInfo qui était la cible de l’action.

Cet événement est déclenché pour chaque action ContentLinkInfo. Par exemple, si l’utilisateur copie et colle plusieurs liens de contenu dans le contrôle RichEditBox à la fois, cet événement se déclenche pour chaque élément ajouté. Ou, si l’utilisateur sélectionne et supprime plusieurs liens de contenu en même temps, cet événement se déclenche pour chaque élément supprimé.

Cet événement se déclenche sur le contrôle RichEditBox après l'événement **TextChanging** et avant l'événement **TextChanged**.

#### <a name="enumerate-content-links-in-a-richeditbox"></a>Énumère les liens de contenu dans un contrôle RichEditBox

Vous pouvez utiliser la propriété RichEditTextRange.ContentLink pour obtenir un lien de contenu à partir d’un document d’édition enrichie. L’énumération TextRangeUnit a la valeur ContentLink pour spécifier le lien de contenu comme unité à utiliser lors de la navigation dans une plage de texte.

Cet exemple montre comment énumérer tous les liens de contenu dans un contrôle RichEditBox et extraire les contacts dans une liste.

```xaml
<StackPanel Width="300">
    <RichEditBox x:Name="Editor" Height="200">
        <RichEditBox.ContentLinkProviders>
            <ContentLinkProviderCollection>
                <ContactContentLinkProvider/>
                <PlaceContentLinkProvider/>
            </ContentLinkProviderCollection>
        </RichEditBox.ContentLinkProviders>
    </RichEditBox>

    <Button Content="Get people" Click="Button_Click"/>

    <ListView x:Name="PeopleList" Header="People">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="ContentLinkInfo">
                <TextBlock>
                    <ContentLink Info="{x:Bind}" Background="Transparent"/>
                </TextBlock>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</StackPanel>
```

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    PeopleList.Items.Clear();
    RichEditTextRange textRange = Editor.Document.GetRange(0, 0) as RichEditTextRange;

    do
    {
        // The Expand method expands the Range EndPosition 
        // until it finds a ContentLink range. 
        textRange.Expand(TextRangeUnit.ContentLink);
        if (textRange.ContentLinkInfo != null
            && textRange.ContentLinkInfo.LinkContentKind == "People")
        {
            PeopleList.Items.Add(textRange.ContentLinkInfo);
        }
    } while (textRange.MoveStart(TextRangeUnit.ContentLink, 1) > 0);
}
```

## <a name="contentlink-text-element"></a>Élément de texte ContentLink

Pour utiliser un lien de contenu dans un contrôle TextBlock ou RichTextBlock, vous utilisez l'élément de texte [ContentLink](/uwp/api/windows.ui.xaml.documents.contentlink) (à partir de l'espace de noms Windows.UI.Xaml.Documents).

Les sources courantes d'un ContentLink dans un bloc de texte sont:

- Un lien de contenu créé par un sélecteur que vous avez extrait à partir d’un contrôle RichTextBlock.
- Un lien de contenu vers un lieu que vous créez dans votre code.

Pour les autres cas, un élément de texte Hyperlink est généralement approprié.

### <a name="contentlink-appearance"></a>Apparence du ContentLink

L’apparence d’un lien de contenu est déterminée par son premier plan, son arrière-plan et son curseur. Dans un bloc de texte, vous pouvez définir les propriétés Foreground (à partir de TextElement) et [Background](/uwp/api/windows.ui.xaml.documents.contentlink.background) pour modifier les couleurs par défaut.

Par défaut, le curseur [Hand](/uwp/api/windows.ui.core.corecursortype) s'affiche lorsque l’utilisateur pointe sur le lien de contenu. À la différence du contrôle RichEditBlock, les contrôles de bloc de texte ne modifient pas le curseur automatiquement en fonction du type de lien. Vous pouvez définir la propriété [Cursor](/uwp/api/windows.ui.xaml.documents.contentlink.Cursor) pour modifier le curseur en fonction du type de lien ou d’autres facteurs.

Voici un exemple de ContentLink utilisé dans un contrôle TextBlock. Le ContentLinkInfo est créé dans le code et affecté à la propriété Info de l’élément de texte ContentLink qui est créé en XAML.

```xaml
<StackPanel>
    <TextBlock>
        <Span xml:space="preserve">
            <Run>This valcano erupted in 1980: </Run><ContentLink x:Name="placeContentLink" Cursor="Pin"/>
            <LineBreak/>
        </Span>
    </TextBlock>

    <Button Content="Show" Click="Button_Click"/>
</StackPanel>
```

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    var info = new Windows.UI.Text.ContentLinkInfo();
    info.DisplayText = "Mount St. Helens";
    info.SecondaryText = "Washington State";
    info.LinkContentKind = "Places";
    info.Uri = new Uri("https://maps.windows.com?cp=46.1912~-122.1944");
    placeContentLink.Info = info;
}
```

> [!TIP]
> Quand vous utilisez un ContentLink dans un contrôle de texte avec d’autres éléments de texte en XAML, placez le contenu dans un conteneur [Span](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.span.aspx) et appliquez l’attribut `xml:space="preserve"` au conteneur Span pour conserver l’espace entre le ContentLink et d'autres éléments.

## <a name="examples"></a>Exemples

Dans cet exemple, un utilisateur peut entrer un lien de contenu vers une personne ou un lieu dans un contrôle RickTextBlock. Vous gérez l’événement ContentLinkChanged pour extraire les liens de contenu et les garder à jour dans une liste de contacts ou dans une liste de lieux.

Dans les modèles d’élément des listes, vous utilisez un contrôle TextBlock avec un élément de texte ContentLink pour afficher les informations de lien de contenu. Le contrôle ListView fournit son propre arrière-plan pour chaque élément, donc l’arrière-plan du ContentLink est défini sur Transparent.

```xaml
<StackPanel Orientation="Horizontal" Grid.Row="1">
    <RichEditBox x:Name="Editor"
                 ContentLinkChanged="Editor_ContentLinkChanged"
                 Margin="20" Width="300" Height="200"
                 VerticalAlignment="Top">
        <RichEditBox.ContentLinkProviders>
            <ContentLinkProviderCollection>
                <ContactContentLinkProvider/>
                <PlaceContentLinkProvider/>
            </ContentLinkProviderCollection>
        </RichEditBox.ContentLinkProviders>
    </RichEditBox>

    <ListView x:Name="PeopleList" Header="People">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="ContentLinkInfo">
                <TextBlock>
                    <ContentLink Info="{x:Bind}"
                                 Background="Transparent"
                                 Cursor="Person"/>
                </TextBlock>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>

    <ListView x:Name="PlacesList" Header="Places">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="ContentLinkInfo">
                <TextBlock>
                    <ContentLink Info="{x:Bind}"
                                 Background="Transparent"
                                 Cursor="Pin"/>
                </TextBlock>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</StackPanel>
```

```csharp
private void Editor_ContentLinkChanged(RichEditBox sender, ContentLinkChangedEventArgs args)
{
    var info = args.ContentLinkInfo;
    var change = args.ChangeKind;
    ListViewBase list = null;

    // Determine whether to update the people or places list.
    if (info.LinkContentKind == "People")
    {
        list = PeopleList;
    }
    else if (info.LinkContentKind == "Places")
    {
        list = PlacesList;
    }

    // Determine whether to add, delete, or edit.
    if (change == ContentLinkChangeKind.Inserted)
    {
        Add();
    }
    else if (args.ChangeKind == ContentLinkChangeKind.Removed)
    {
        Remove();
    }
    else if (change == ContentLinkChangeKind.Edited)
    {
        Remove();
        Add();
    }

    // Add content link info to the list. It's bound to the
    // Info property of a ContentLink in XAML.
    void Add()
    {
        list.Items.Add(info);
    }

    // Use ContentLinkInfo.Id to find the item,
    // then remove it from the list using its index.
    void Remove()
    {
        var items = list.Items.Where(i => ((ContentLinkInfo)i).Id == info.Id);
        var item = items.FirstOrDefault();
        var idx = list.Items.IndexOf(item);

        list.Items.RemoveAt(idx);
    }
}
```