---
Description: Personnaliser l’affichage de l’écriture manuscrite intégrée pour l’encre à l’entrée de texte qui est pris en charge par les contrôles de texte UWP tels que la zone de texte, RichEditBox (et les contrôles tels que le AutoSuggestBox qui fournissent une expérience d’entrée de texte similaire).
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
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57634914"
---
# <a name="text-input-with-the-handwriting-view"></a>Entrée de texte avec la vue de l’écriture manuscrite

![La zone de texte s’agrandit lorsqu'un utilisateur appuie dessus avec un stylet](images/handwritingview/handwritingview2.gif)

Personnaliser l’affichage de l’écriture manuscrite intégrée pour l’encre à l’entrée de texte pris en charge par les contrôles de texte UWP tels que le [zone de texte](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox), [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox), et les contrôles dérivés de ces telles que le [ AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox).

## <a name="overview"></a>Vue d’ensemble

Prise en charge incorporée de stylet d’entrée à l’aide des fonctionnalités de zones de saisie de texte XAML [Windows Ink](../input/pen-and-stylus-interactions.md). Lorsqu’un utilisateur appuie dans une zone de texte d’entrée à l’aide d’un stylet Windows, la zone de texte transforme en une surface de l’écriture manuscrite plutôt que d’ouvrir un panneau d’entrée.

Le texte est reconnu comme l’utilisateur écrit n’importe où dans la zone de texte et d’un candidat fenêtre affiche les résultats de reconnaissance. L’utilisateur peut appuyer sur un résultat pour le choisir ou continuer à écrire pour accepter le candidat proposé. Les résultats de la reconnaissance littérale (lettre par lettre) sont inclus dans la fenêtre candidate, donc la reconnaissance ne se limite pas aux mots d'un dictionnaire. À mesure que l’utilisateur écrit, la saisie de texte acceptée est convertie en police de script qui conserve l’apparence de l’écriture naturelle.

> [!NOTE]
> La vue de l’écriture manuscrite est activée par défaut, mais vous pouvez le désactiver sur une fonction du contrôle et rétablir le panneau de saisie de texte à la place.

![Zone de texte avec l’encre et suggestions](images/handwritingview/handwritingview-inksuggestion1.gif)

Un utilisateur peut modifier son texte à l’aide d'actions et de mouvements standard, telles que ceux-ci :

- _barrer_ ou _raturer_ - dessiner une rayure pour supprimer un mot ou une partie d’un mot
- _joindre_ -dessiner un arc entre des mots pour supprimer l’espace entre eux
- _Insérer_ -dessiner un symbole d'accent circonflexe pour insérer un espace
- _remplacer_ -écrivez par-dessus le texte existant pour le remplacer

![Zone de texte avec une correction de l’encre](images/handwritingview/handwritingview-inkcorrection1.gif)

## <a name="disable-the-handwriting-view"></a>Désactiver l’affichage de l’écriture manuscrite

La vue de l’écriture manuscrite intégré est activée par défaut.

Vous souhaiterez peut-être désactiver l’affichage de l’écriture manuscrite si vous déjà fournissez les fonctionnalités de l’encre en texte équivalentes dans votre application ou votre expérience d’entrée de texte s’appuie sur une sorte de mise en forme caractère ou spécial (par exemple, un onglet) non disponible dans l’écriture manuscrite.

Dans cet exemple, nous désactivons la vue de l’écriture manuscrite en définissant le [IsHandwritingViewEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.ishandwritingviewenabled) propriété de la [zone de texte](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) contrôle sur false. Tous les contrôles de texte qui prennent en charge de la vue de l’écriture manuscrite prend en charge une propriété similaire.

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen" 
    IsHandwritingViewEnabled="False">
</TextBox>
```

## <a name="specify-the-alignment-of-the-handwriting-view"></a>Spécifier l’alignement de la vue de l’écriture manuscrite

La vue de l’écriture manuscrite est située au-dessus du contrôle de texte sous-jacent et dimensionnée en fonction des préférences de l’écriture de l’utilisateur (consultez **Paramètres -> appareils -> stylet & Windows Ink -> l’écriture manuscrite -> taille de police lorsque vous écrivez directement dans champ de texte**). La vue est également automatiquement alignés sur le contrôle de texte et son emplacement au sein de l’application.

L’interface utilisateur de l’application n’est pas redistribué pour prendre en compte le plus grand contrôle, donc le système peut entraîner l’affichage à occlude l’interface utilisateur important.

Ici, nous montrons comment utiliser le [PlacementAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.placementalignment) propriété d’un [zone de texte](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) pour spécifier le point d’ancrage sur le contrôle de texte sous-jacent est utilisé pour aligner le vue de l’écriture manuscrite.

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

## <a name="disable-auto-completion-candidates"></a>Désactiver des candidats de la saisie semi-automatique

La fenêtre contextuelle suggestion de texte est activée par défaut pour fournir une liste de l’encre supérieur candidats de reconnaissance à partir de laquelle l’utilisateur peut sélectionner au cas où le candidat supérieur est incorrect.

Si votre application a déjà fournit la fonctionnalité de reconnaissance robuste, personnalisé, vous pouvez utiliser la [AreCandidatesEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.arecandidatesenabled) propriété permettant de désactiver les suggestions intégrées, comme illustré dans l’exemple suivant.

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

## <a name="use-handwriting-font-preferences"></a>Utiliser les préférences de police de l’écriture manuscrite

Un utilisateur peut choisir à partir d’un ensemble prédéfini de l’écriture manuscrite de polices à utiliser lorsque le rendu de texte en fonction de la reconnaissance de l’encre (consultez **Paramètres -> appareils -> stylet & Windows Ink -> l’écriture manuscrite -> police lors de l’utilisation de l’écriture manuscrite**).

> [!NOTE]
> Les utilisateurs peuvent même créer une police en fonction de leur propres l’écriture manuscrite.
> [!VIDEO https://www.youtube.com/embed/YRR4qd4HCw8]

Votre application peut accéder à ce paramètre et utiliser la police sélectionnée pour le texte reconnu dans le contrôle de texte.

Dans cet exemple, nous écouter le [TextChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged) événement d’un [zone de texte](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) et appliquer la police sélectionnée de l’utilisateur si la modification du texte provenant d’une l’HandwritingView (ou une police par défaut, dans le cas contraire).

```csharp
private void SampleTextBox_TextChanged(object sender, TextChangedEventArgs e)
{
    ((TextBox)sender).FontFamily = 
        ((TextBox)sender).HandwritingView.IsOpen ?
            new FontFamily(PenAndInkSettings.GetDefault().FontFamilyName) : 
            new FontFamily("Segoe UI");
}
```

## <a name="access-the-handwritingview-in-composite-controls"></a>Accéder à la HandwritingView dans des contrôles composites

Les contrôles composites qui utilisent le [zone de texte](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) ou [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox) contrôles, tels que [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) prennent également en charge un [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview).

Pour accéder à la [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) dans un contrôle composite, utilisez le [VisualTreeHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper) API.

L’extrait de code XAML suivant affiche un [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) contrôle.

```xaml
<AutoSuggestBox Name="SampleAutoSuggestBox" 
    Height="50" Width="500" 
    PlaceholderText="Auto Suggest Example" 
    FontSize="16" FontFamily="Segoe UI"  
    Loaded="SampleAutoSuggestBox_Loaded">
</AutoSuggestBox>
```

Dans le code-behind correspondant, nous montrons comment désactiver le [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) sur le [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox).

1. Tout d’abord, nous gérons l’événement d’application chargé où nous appelons une fonction FindInnerTextBox pour démarrer le parcours d’arborescence d’éléments visuels.

    ```csharp
    private void SampleAutoSuggestBox_Loaded(object sender, RoutedEventArgs e)
    {
        if (FindInnerTextBox((AutoSuggestBox)sender))
            autoSuggestInnerTextBox.IsHandwritingViewEnabled = false;
    }
    ```

2. Ensuite, nous commençons itération au sein de l’arborescence d’éléments visuels (commençant à un AutoSuggestBox) dans la fonction FindInnerTextBox avec un appel à FindVisualChildByName.

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

3. Enfin, cette fonction effectue une itération dans l’arborescence visuelle jusqu'à ce que la zone de texte est récupéré.

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

Dans certains cas, vous devez vous assurer que le [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) couvre les éléments d’interface utilisateur il sinon ne peut-être pas.

Ici, nous créons une zone de texte qui prend en charge de dictée (implémentée en plaçant une zone de texte et un bouton de dictée dans un StackPanel).

![Zone de texte avec la dictée](images/handwritingview/textbox-with-dictation.png)

Comme le StackPanel est désormais supérieure à la zone de texte, le [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) ne peut pas occlude tous le composite OLE.

![Zone de texte avec la dictée](images/handwritingview/textbox-with-dictation-handwritingview.png)

Pour résoudre ce problème, définissez la propriété PlacementTarget de la [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) à l’élément d’interface à laquelle il doit être alignée.

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

Vous pouvez également définir la taille de la [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview), qui peut être utile lorsque vous devez vous assurer de la vue n’occlude UI important.

Comme dans l’exemple précédent, nous allons créer une zone de texte qui prend en charge de dictée (implémentée en plaçant une zone de texte et un bouton de dictée dans un StackPanel).

![Zone de texte avec la dictée](images/handwritingview/textbox-with-dictation.png)

Dans ce cas, nous voulons vérifier que le bouton de dictée est toujours visible.

![Zone de texte avec la dictée](images/handwritingview/textbox-with-dictation-handwritingview-resize.png)

Pour ce faire, nous lions la propriété MaxWidth de la [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) à la largeur de l’élément d’interface utilisateur qu’il doit occlude.

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

Si vous disposez d’interface utilisateur personnalisée qui apparaît dans la réponse à une entrée de texte, par exemple un message d’information, vous devrez peut-être repositionner cette interface utilisateur afin qu’il n’occlude la vue de l’écriture manuscrite.

![Zone de texte avec une interface utilisateur personnalisée](images/handwritingview/textbox-with-customui.png)

L’exemple suivant montre comment écouter le [Opened](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.opened), [fermé](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.closed
), et [SizeChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged) événements de la [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) pour définir le position d’un [contextuelle](https://docs.microsoft.com/uwp/api/windows.ui.popups).

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

## <a name="retemplate-the-handwritingview-control"></a>Retemplate le contrôle HandwritingView

Comme avec tous les contrôles de framework XAML, vous pouvez personnaliser la structure visuelle et le comportement visuel d’un [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) pour vos besoins spécifiques.

Pour voir un exemple complet de la création d’un modèle personnalisé d’extraction le [créer des contrôles de transport personnalisé](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/custom-transport-controls) procédures ou [exemple de contrôle personnalisé de modifier](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl).








