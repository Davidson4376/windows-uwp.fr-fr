---
Description: Personnalisez la vue de l’écriture manuscrite intégrée en entrée de texte qui est prise en charge par les contrôles de texte UWP comme TextBox, RichEditBox et les contrôles tels qu’AutoSuggestBox qui fournissent une expérience d’entrée de texte similaire.
title: Entrée de texte avec la vue de l’écriture manuscrite
label: Text input with the handwriting view
template: detail.hbs
ms.date: 10/13/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: sewen
design-contact: minah.kim
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f7b31898e6a90410e4edc73ee36f71a7e4d94155
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63774774"
---
# <a name="text-input-with-the-handwriting-view"></a>Entrée de texte avec la vue de l’écriture manuscrite

![La zone de texte s’agrandit quand l’utilisateur appuie dessus avec un stylet](images/handwritingview/handwritingview2.gif)

Personnalisez la vue de l’écriture manuscrite intégrée en entrée de texte qui est prise en charge par les contrôles de texte UWP comme [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox), [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox) et les contrôles qui en dérivent tels qu’[AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox).

## <a name="overview"></a>Vue d’ensemble

Les zones de saisie de texte XAML ont une prise en charge intégrée de la saisie effectuée à l’aide du stylet avec [Windows Ink](../input/pen-and-stylus-interactions.md). Quand un utilisateur appuie sur une zone de saisie de texte avec un stylet Windows, la zone de texte se transforme en une surface d’écriture manuscrite, au lieu d’ouvrir un panneau de saisie distinct.

Le texte est reconnu alors que l’utilisateur écrit n’importe où dans la zone de texte et une fenêtre candidate montre les résultats de la reconnaissance. L’utilisateur peut appuyer sur un résultat pour le choisir ou continuer d’écrire pour accepter le candidat proposé. Les résultats de la reconnaissance littérale (lettre par lettre) sont inclus dans la fenêtre candidate, donc la reconnaissance ne se limite pas aux mots d’un dictionnaire. À mesure que l’utilisateur écrit, la saisie de texte acceptée est convertie en police de script qui conserve l’apparence de l’écriture naturelle.

> [!NOTE]
> La vue de l’écriture manuscrite est activée par défaut, mais vous pouvez la désactiver pour chaque contrôle et rétablir le panneau de saisie de texte à la place.

![Zone de texte avec entrée manuscrite et suggestions](images/handwritingview/handwritingview-inksuggestion1.gif)

Un utilisateur peut modifier son texte à l’aide d’actions et de mouvements standard, tels que ceux-ci :

- _barrer_ ou _raturer_ : dessiner un trait pour supprimer un mot ou une partie d’un mot
- _joindre_ : dessiner un arc entre des mots pour supprimer l’espace entre eux
- _insérer_ : dessiner un accent circonflexe pour insérer un espace
- _remplacer_ : écrire par-dessus le texte existant pour le remplacer

![Zone de texte avec correction de l’écriture manuscrite](images/handwritingview/handwritingview-inkcorrection1.gif)

## <a name="disable-the-handwriting-view"></a>Désactiver la vue de l’écriture manuscrite

La vue de l’écriture manuscrite intégrée est activée par défaut.

Vous souhaiterez peut-être désactiver la vue de l’écriture manuscrite si vous fournissez déjà des fonctionnalités équivalentes de conversion de l’entrée manuscrite en texte dans votre application, ou si votre expérience d’entrée de texte s’appuie sur une sorte de mise en forme ou caractère spécial (par exemple, une tabulation) non disponible dans l’écriture manuscrite.

Dans cet exemple, nous désactivons la vue de l’écriture manuscrite en affectant à la propriété [IsHandwritingViewEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.ishandwritingviewenabled) du contrôle [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) la valeur false. Tous les contrôles de texte qui prennent en charge la vue de l’écriture manuscrite acceptent une propriété similaire.

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen" 
    IsHandwritingViewEnabled="False">
</TextBox>
```

## <a name="specify-the-alignment-of-the-handwriting-view"></a>Spécifier l’alignement de la vue de l’écriture manuscrite

La vue de l’écriture manuscrite est située au-dessus du contrôle de texte sous-jacent et dimensionnée en fonction des préférences d’écriture de l’utilisateur (consultez **Paramètres -> Appareils -> Stylet et Windows Ink -> Écriture manuscrite -> Size of font (Taille de police) quand vous écrivez directement dans un champ de texte**). La vue est également automatiquement alignée par rapport au contrôle de texte et son emplacement au sein de l’application.

L’interface utilisateur de l’application n’étant pas réorganisée pour prendre en compte le plus grand contrôle, le système peut amener la vue à masquer des éléments importants de l’interface utilisateur.

Ici, nous montrons comment utiliser la propriété [PlacementAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.placementalignment) d’un [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) pour spécifier le point d’ancrage sur le contrôle de texte sous-jacent qui est utilisé pour aligner la vue de l’écriture manuscrite.

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen">
        <TextBox.HandwritingView>
            <HandwritingView PlacementAlignment="TopLeft"/>
        </TextBox.HandwritingView>
</TextBox>
```

## <a name="disable-auto-completion-candidates"></a>Désactiver des candidats de saisie semi-automatique

La fenêtre contextuelle de suggestion de texte est activée par défaut pour fournir une liste des principaux candidats à la reconnaissance des entrées manuscrites à partir de laquelle l’utilisateur peut effectuer son choix si le premier candidat est incorrect.

Si votre application propose déjà une fonctionnalité de reconnaissance fiable et personnalisée, vous pouvez utiliser la propriété [AreCandidatesEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.arecandidatesenabled) pour désactiver les suggestions intégrées, comme illustré dans l’exemple suivant.

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen">
        <TextBox.HandwritingView>
            <HandwritingView AreCandidatesEnabled="False"/>
        </TextBox.HandwritingView>
</TextBox>
```

## <a name="use-handwriting-font-preferences"></a>Utiliser les préférences de police d’écriture manuscrite

Un utilisateur peut choisir parmi un ensemble prédéfini de polices d’écriture manuscrite à utiliser lors du rendu de texte basé sur la reconnaissance des entrées manuscrites (consultez **Paramètres -> Appareils -> Stylet et Windows Ink -> Écriture manuscrite -> Police lors de l’utilisation de l’écriture manuscrite**).

> [!NOTE]
> Les utilisateurs peuvent même créer une police en fonction de leur propre écriture manuscrite.
> [!VIDEO https://www.youtube.com/embed/YRR4qd4HCw8]

Votre application peut accéder à ce paramètre et utiliser la police sélectionnée pour le texte reconnu dans le contrôle de texte.

Dans cet exemple, nous écoutons l’événement [TextChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged) d’un [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) et appliquons la police sélectionnée de l’utilisateur si la modification du texte provient du HandwritingView (ou une police par défaut dans le cas contraire).

```csharp
private void SampleTextBox_TextChanged(object sender, TextChangedEventArgs e)
{
    ((TextBox)sender).FontFamily = 
        ((TextBox)sender).HandwritingView.IsOpen ?
            new FontFamily(PenAndInkSettings.GetDefault().FontFamilyName) : 
            new FontFamily("Segoe UI");
}
```

## <a name="access-the-handwritingview-in-composite-controls"></a>Accéder au HandwritingView dans des contrôles composites

Les contrôles composites qui utilisent les contrôles [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) ou [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox), comme [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox), prennent également en charge un [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview).

Pour accéder au [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) dans un contrôle composite, utilisez l’API [VisualTreeHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper).

L’extrait de code XAML suivant affiche un contrôle [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox).

```xaml
<AutoSuggestBox Name="SampleAutoSuggestBox" 
    Height="50" Width="500" 
    PlaceholderText="Auto Suggest Example" 
    FontSize="16" FontFamily="Segoe UI"  
    Loaded="SampleAutoSuggestBox_Loaded">
</AutoSuggestBox>
```

Dans le code-behind correspondant, nous montrons comment désactiver le [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) dans le contrôle [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox).

1. Tout d’abord, nous gérons l’événement Loaded de l’application où nous appelons une fonction FindInnerTextBox pour démarrer la traversée de l’arborescence d’éléments visuels.

    ```csharp
    private void SampleAutoSuggestBox_Loaded(object sender, RoutedEventArgs e)
    {
        if (FindInnerTextBox((AutoSuggestBox)sender))
            autoSuggestInnerTextBox.IsHandwritingViewEnabled = false;
    }
    ```

2. Ensuite, nous commençons l’itération au sein de l’arborescence d’éléments visuels (en commençant par un AutoSuggestBox) dans la fonction FindInnerTextBox avec un appel à FindVisualChildByName.

    ```csharp
    private bool FindInnerTextBox(AutoSuggestBox autoSuggestBox)
    {
        if (autoSuggestInnerTextBox == null)
        {
            // Cache textbox to avoid multiple tree traversals. 
            autoSuggestInnerTextBox = 
                (TextBox)FindVisualChildByName<TextBox>(autoSuggestBox);
        }
        return (autoSuggestInnerTextBox != null);
    }
    ```

3. Enfin, cette fonction parcourt l’arborescence d’éléments visuels jusqu’à la récupération de TextBox.

    ```csharp
    private FrameworkElement FindVisualChildByName<T>(DependencyObject obj)
    {
        FrameworkElement element = null;
        int childrenCount = 
            VisualTreeHelper.GetChildrenCount(obj);
        for (int i = 0; (i < childrenCount) && (element == null); i++)
        {
            FrameworkElement child = 
                (FrameworkElement)VisualTreeHelper.GetChild(obj, i);
            if ((child.GetType()).Equals(typeof(T)) || (child.GetType().GetTypeInfo().IsSubclassOf(typeof(T))))
            {
                element = child;
            }
            else
            {
                element = FindVisualChildByName<T>(child);
            }
        }
        return (element);
    }
    ```

## <a name="reposition-the-handwritingview"></a>Repositionner le HandwritingView

Dans certains cas, vous devez vous assurer que le [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) couvre les éléments d’interface utilisateur qui ne le seraient pas sinon.

Ici, nous créons un TextBox qui prend en charge la dictée (implémenté en plaçant un TextBox et un bouton de dictée dans un StackPanel).

![TextBox avec dictée](images/handwritingview/textbox-with-dictation.png)

Comme le StackPanel est désormais plus grand que le TextBox, le [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) peut ne pas masquer tous les contrôles composites.

![TextBox avec dictée](images/handwritingview/textbox-with-dictation-handwritingview.png)

Pour résoudre ce problème, affectez à la propriété PlacementTarget du [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) l’élément d’interface utilisateur sur lequel il doit être aligné.

```xaml
<StackPanel Name="DictationBox" 
    Orientation="Horizontal" 
    VerticalAlignment="Top" 
    HorizontalAlignment="Left" 
    BorderThickness="1" BorderBrush="DarkGray" 
    Height="55" Width="500" Margin="50">
    <TextBox Name="DictationTextBox" 
        Width="450" BorderThickness="0" 
        FontSize="24" VerticalAlignment="Center">
        <TextBox.HandwritingView>
            <HandwritingView PlacementTarget="{Binding ElementName=DictationBox}"/>
        </TextBox.HandwritingView>
    </TextBox>
    <Button Name="DictationButton" 
        Height="48" Width="48" 
        FontSize="24" 
        FontFamily="Segoe MDL2 Assets" 
        Content="&#xE720;" 
        Background="White" Foreground="DarkGray"     Tapped="DictationButton_Tapped" />
</StackPanel>
```

## <a name="resize-the-handwritingview"></a>Redimensionner le HandwritingView

Vous pouvez également définir la taille du [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview), ce qui peut être utile quand vous devez vous assurer que la vue ne masque pas des éléments importants de l’interface utilisateur.

Comme dans l’exemple précédent, nous créons un TextBox qui prend en charge la dictée (implémenté en plaçant un TextBox et un bouton de dictée dans un StackPanel).

![TextBox avec dictée](images/handwritingview/textbox-with-dictation.png)

Dans ce cas, nous voulons vérifier que le bouton de dictée est toujours visible.

![TextBox avec dictée](images/handwritingview/textbox-with-dictation-handwritingview-resize.png)

Pour ce faire, nous lions la propriété MaxWidth du [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) à la largeur de l’élément d’interface utilisateur qu’il doit masquer.

```xaml
<StackPanel Name="DictationBox" 
    Orientation="Horizontal" 
    VerticalAlignment="Top" 
    HorizontalAlignment="Left" 
    BorderThickness="1" 
    BorderBrush="DarkGray" 
    Height="55" Width="500" 
    Margin="50">
    <TextBox Name="DictationTextBox" 
        Width="450" 
        BorderThickness="0" 
        FontSize="24" 
        VerticalAlignment="Center">
        <TextBox.HandwritingView>
            <HandwritingView 
                PlacementTarget="{Binding ElementName=DictationBox}“
                MaxWidth="{Binding ElementName=DictationTextBox, Path=Width"/>
        </TextBox.HandwritingView>
    </TextBox>
    <Button Name="DictationButton" 
        Height="48" Width="48" 
        FontSize="24" 
        FontFamily="Segoe MDL2 Assets" 
        Content="&#xE720;" 
        Background="White" Foreground="DarkGray" 
        Tapped="DictationButton_Tapped" />
</StackPanel>
```

## <a name="reposition-custom-ui"></a>Repositionner l’interface utilisateur personnalisée

Si vous disposez d’une interface utilisateur personnalisée qui apparaît en réponse à une entrée de texte, par exemple une fenêtre contextuelle d’information, il est possible que vous deviez repositionner cette interface utilisateur pour qu’elle ne masque pas la vue de l’écriture manuscrite.

![TextBox avec interface utilisateur personnalisée](images/handwritingview/textbox-with-customui.png)

L’exemple suivant montre comment écouter les événements [Opened](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.opened), [Closed](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.closed
) et [SizeChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged) du [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) pour définir la position d’un [Popup](https://docs.microsoft.com/uwp/api/windows.ui.popups).

```csharp
private void Search_HandwritingViewOpened(
    HandwritingView sender, HandwritingPanelOpenedEventArgs args)
{
    UpdatePopupPositionForHandwritingView();
}

private void Search_HandwritingViewClosed(
    HandwritingView sender, HandwritingPanelClosedEventArgs args)
{
    UpdatePopupPositionForHandwritingView();
}

private void Search_HandwritingViewSizeChanged(
    object sender, SizeChangedEventArgs e)
{
    UpdatePopupPositionForHandwritingView();
}

private void UpdatePopupPositionForHandwritingView()
{
if (CustomSuggestionUI.IsOpen)
    CustomSuggestionUI.VerticalOffset = GetPopupVerticalOffset();
}

private double GetPopupVerticalOffset()
{
    if (SearchTextBox.HandwritingView.IsOpen)
        return (SearchTextBox.Margin.Top + SearchTextBox.HandwritingView.ActualHeight);
    else
        return (SearchTextBox.Margin.Top + SearchTextBox.ActualHeight);    
}
```

## <a name="retemplate-the-handwritingview-control"></a>Redéfinir le modèle du contrôle HandwritingView

Comme avec tous les contrôles de framework XAML, vous pouvez personnaliser la structure visuelle et le comportement visuel d’un [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) selon vos besoins spécifiques.

Pour voir un exemple complet de la création d’un modèle personnalisé, consultez la procédure [Créer des contrôles de transport personnalisés](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/custom-transport-controls) ou [Exemple de contrôle d’édition personnalisé](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl).








