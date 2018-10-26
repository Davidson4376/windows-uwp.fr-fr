---
author: Jwmsft
Description: A text entry box that provides suggestions as the user types.
title: Recommandations concernant les zones de suggestion automatique
ms.assetid: 1F608477-F795-4F33-92FA-F200CC243B6B
dev.assetid: 54F8DB8A-120A-4D79-8B5A-9315A3764C2F
label: Auto-suggest box
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: efb6c8a9324419ea099041f34c341152351575f6
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5558910"
---
# <a name="auto-suggest-box"></a>Zone de suggestion automatique

Utilisez un objet AutoSuggestBox pour fournir une liste de suggestions afin que les utilisateurs puissent sélectionner des options au fil de leur saisie.

> **API importantes**: [classe AutoSuggestBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.aspx), [événement TextChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.textchanged.aspx), [événement SuggestionChose](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.suggestionchosen.aspx), [événement QuerySubmitted](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.querysubmitted.aspx)

![Zone de suggestion automatique](images/controls/auto-suggest-box-open.png)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

Si vous recherchez un contrôle simple et personnalisable permettant d’effectuer une recherche de texte avec une liste de suggestions, choisissez une zone de suggestion.

Pour plus d’informations sur le choix du contrôle de texte approprié, voir l’article [Contrôles de texte](text-controls.md).

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/AutoSuggestBox">ouvrir l’application et voir un objet AutoSuggestBox en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Zone de suggestion automatique dans l’application GrooveMusique.

![Zone de suggestion automatique dans l’application GrooveMusique](images/control-examples/auto-suggest-box-groove.png)

## <a name="anatomy"></a>Anatomie
Le point d’entrée de la zone de suggestion automatique se compose d’un en-tête facultatif et d’une zone de texte avec texte d’information facultatif:

![Exemple de point d’entrée pour le contrôle de suggestion automatique](images/controls_autosuggest_entrypoint.png)

La liste des résultats de suggestion automatique se remplit automatiquement dès que l’utilisateur commence à saisir du texte. La liste des résultats peut apparaître au-dessus ou en dessous de la zone de texte. Un bouton Effacer tout s’affiche :

![Exemple de contrôle de suggestion automatique étendu](images/controls_autosuggest_expanded01.png)

## <a name="create-an-auto-suggest-box"></a>Créer une zone de suggestion automatique

Pour utiliser un objet AutoSuggestBox, vous devez répondre à 3actions effectuées par les utilisateurs.

- Texte modifié: lorsque l’utilisateur entre du texte, mettre à jour la liste de suggestions.
- Suggestion choisie : lorsque l’utilisateur choisit une suggestion dans la liste de suggestions, mettre à jour la zone de texte.
- Requête envoyée : lorsque l’utilisateur envoie une requête, afficher les résultats de la requête.

### <a name="text-changed"></a>Texte modifié

L’événement [TextChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.textchanged.aspx) se produit chaque fois que le contenu de la zone de texte est mis à jour. Utilisez la propriété [Reason](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestboxtextchangedeventargs.reason.aspx) d’arguments d’événement pour déterminer si la modification est due à une entrée utilisateur. Si la raison de la modification est **UserInput**, filtrez vos données en fonction de l’entrée. Ensuite, définissez les données filtrées comme [ItemsSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) de l’objet AutoSuggestBox pour mettre à jour la liste de suggestions.

Pour contrôler la façon dont les éléments sont affichés dans la liste de suggestions, vous pouvez utiliser [DisplayMemberPath](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.displaymemberpath.aspx) ou [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx).

- Pour afficher le texte d’une seule propriété de votre élément de données, définissez la propriété DisplayMemberPath pour choisir la propriété de votre objet à afficher dans la liste de suggestions.
- Pour définir une apparence personnalisée pour chaque élément dans la liste, utilisez la propriété ItemTemplate.

### <a name="suggestion-chosen"></a>Suggestion choisie

Lorsqu’un utilisateur navigue dans la liste de suggestions à l’aide du clavier, vous devez mettre à jour le texte dans la zone de texte pour assurer la correspondance.

Vous pouvez définir la propriété [TextMemberPath](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.textmemberpath.aspx) pour choisir la propriété de votre objet de données à afficher dans la zone de texte. Si vous spécifiez un TextMemberPath, la zone de texte est automatiquement mise à jour. Vous devez généralement spécifier la même valeur pour DisplayMemberPath et TextMemberPath afin que le texte soit identique dans la liste de suggestions et dans la zone de texte.

Si vous avez besoin d’afficher plus qu’une simple propriété, gérez l’événement [SuggestionChosen](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.suggestionchosen.aspx) pour remplir la zone de texte avec du texte personnalisé basé sur l’élément sélectionné.

### <a name="query-submitted"></a>Requête envoyée

Gérez l’événement [QuerySubmitted](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.querysubmitted.aspx) pour effectuer une action de requête appropriée sur votre application et montrer les résultats à l’utilisateur.

L’événement QuerySubmitted se produit lorsqu’un utilisateur valide une chaîne de requête. L’utilisateur peut valider une requête de l’une de ces manières:
- Lorsque le focus est sur la zone de texte, appuyez sur Entrée ou cliquez sur l’icône de requête. La propriété [ChosenSuggestion](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestboxquerysubmittedeventargs.chosensuggestion.aspx) des arguments d’événement est **null**.
- Lorsque le focus est sur la liste de suggestions, appuyez sur Entrée, puis cliquez ou appuyez sur un élément. La propriété ChosenSuggestion des arguments d’événement contient l’élément sélectionné dans la liste.

Dans tous les cas, la propriété [QueryText](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestboxquerysubmittedeventargs.querytext.aspx) des arguments d’événement contient le texte de la zone de texte.

Voici un simple élément AutoSuggestBox avec les gestionnaires d’événements requis.

```xaml
<AutoSuggestBox PlaceholderText="Search" QueryIcon="Find" Width="200"
                TextChanged="AutoSuggestBox_TextChanged"
                QuerySubmitted="AutoSuggestBox_QuerySubmitted"
                SuggestionChosen="AutoSuggestBox_SuggestionChosen"/>
```

```csharp
private void AutoSuggestBox_TextChanged(AutoSuggestBox sender, AutoSuggestBoxTextChangedEventArgs args)
{
    // Only get results when it was a user typing,
    // otherwise assume the value got filled in by TextMemberPath
    // or the handler for SuggestionChosen.
    if (args.Reason == AutoSuggestionBoxTextChangeReason.UserInput)
    {
        //Set the ItemsSource to be your filtered dataset
        //sender.ItemsSource = dataset;
    }
}


private void AutoSuggestBox_SuggestionChosen(AutoSuggestBox sender, AutoSuggestBoxSuggestionChosenEventArgs args)
{
    // Set sender.Text. You can use args.SelectedItem to build your text string.
}


private void AutoSuggestBox_QuerySubmitted(AutoSuggestBox sender, AutoSuggestBoxQuerySubmittedEventArgs args)
{
    if (args.ChosenSuggestion != null)
    {
        // User selected an item from the suggestion list, take an action on it here.
    }
    else
    {
        // Use args.QueryText to determine what to do.
    }
}
```

## <a name="use-autosuggestbox-for-search"></a>Utiliser AutoSuggestBox pour la recherche

Utilisez un objet AutoSuggestBox pour fournir une liste de suggestions afin que les utilisateurs puissent sélectionner des options au fil de leur saisie.

Par défaut, la zone de texte n’affiche pas de bouton de requête. Vous pouvez définir la propriété [QueryIcon](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.queryicon.aspx) pour ajouter un bouton avec l’icône spécifiée sur le côté droit de la zone de texte. Par exemple, pour rendre AutoSuggestBox similaire à une zone de recherche classique, ajoutez une icône Rechercher comme suit.

```xaml
<AutoSuggestBox QueryIcon="Find"/>
```

Voici AutoSuggestBox avec une icône Rechercher.

![Exemple de point d’entrée pour le contrôle de suggestion automatique](images/controls_autosuggest_entrypoint.png)

## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées

-   Lorsque vous utilisez la zone de suggestion automatique pour effectuer des recherches et qu’il n’existe aucun résultat de recherche pour le texte entré, affichez le message d’une ligne «Aucun résultat» comme résultat afin que les utilisateurs sachent que leur requête de recherche a été exécutée:

    ![Exemple de zone de suggestion automatique sans résultat de recherche](images/controls_autosuggest_noresults.png)

<!--
<div class="microsoft-internal-note">
**Globalization and localization checklist**

<table>
<tr>
<th>Vertical spacing</th><td>Use non-Latin characters for vertical spacing to ensure non-Latin scripts will display properly, including numbers.</td>
</tr>
<tr>
<th>Scrolling</th><td>When auto suggest text is selected, user should be able to scroll to end of string.</td>
</tr>
</table>
</div>
-->

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.
- [Exemple AutoSuggestBox](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

## <a name="related-articles"></a>Articles associés

- [Contrôles de texte](text-controls.md)
- [Vérification de l’orthographe](text-controls.md)
- [Recherche](search.md)
- [Classe TextBox](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Classe PasswordBox Windows.UI.Xaml.Controls](https://msdn.microsoft.com/library/windows/apps/br227519)
- [Propriété String.Length](https://msdn.microsoft.com/library/system.string.length.aspx)
