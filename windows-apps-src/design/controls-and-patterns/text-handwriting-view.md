---
author: Karl-Bridge-Microsoft
Description: Customize the built-in handwriting view for ink to text input that is supported by UWP text controls such as the TextBox, RichEditBox (and controls like the AutoSuggestBox that provide a similar text input experience).
title: Saisie de texte avec l’affichage de l’écriture manuscrite
label: Text input with the handwriting view
template: detail.hbs
ms.author: kbridge
ms.date: 10/13/18
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: sewen
design-contact: minah.kim
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 3117cde7b8b00973c135fbc759fa99b6a48ec6ac
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5481751"
---
# <a name="text-input-with-the-handwriting-view"></a>Saisie de texte avec l’affichage de l’écriture manuscrite

![La zone de texte s’agrandit lorsqu'un utilisateur appuie dessus avec un stylet](images/handwritingview/handwritingview2.gif)

Personnaliser l’affichage de l’écriture manuscrite intégrée de l’encre à l’entrée de texte est pris en charge par les contrôles de texte UWP tels que le [contrôle TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox), [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox)et autres contrôles qui fournissent une expérience d’entrée de texte similaire (comme [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)).

## <a name="overview"></a>Vue d’ensemble

Zones de saisie de texte XAML présentent prise en charge intégrée du stylet à l’aide de [Windows Ink](../input/pen-and-stylus-interactions.md). Lorsqu’un utilisateur appuie sur une zone de saisie de texte à l’aide d’un stylet Windows, la zone de texte se transforme en une surface de l’écriture manuscrite, au lieu d’ouvrir un panneau de saisie distinct.

Le texte est reconnu à mesure que l’utilisateur écrit n’importe où dans la zone de texte et un candidat fenêtre affiche les résultats de reconnaissance. L’utilisateur peut appuyer sur un résultat pour le choisir ou continuer à écrire pour accepter le candidat proposé. Les résultats de la reconnaissance littérale (lettre par lettre) sont inclus dans la fenêtre candidate, donc la reconnaissance ne se limite pas aux mots d'un dictionnaire. À mesure que l’utilisateur écrit, la saisie de texte acceptée est convertie en police de script qui conserve l’apparence de l’écriture naturelle.

> [!NOTE]
> La vue de l’écriture manuscrite est activée par défaut, mais vous pouvez la désactiver sur la base de chaque contrôle et rétablir le panneau de saisie de texte à la place.

![Zone de texte avec entrée manuscrite et suggestions](images/handwritingview/handwritingview-inksuggestion1.gif)

Un utilisateur peut modifier son texte à l’aide d'actions et de mouvements standard, telles que ceux-ci:

- _barrer_ ou _raturer_ - dessiner une rayure pour supprimer un mot ou une partie d’un mot
- _joindre_ -dessiner un arc entre des mots pour supprimer l’espace entre eux
- _Insérer_ -dessiner un symbole d'accent circonflexe pour insérer un espace
- _remplacer_ -écrivez par-dessus le texte existant pour le remplacer

![Zone de texte avec correction d’entrées manuscrites](images/handwritingview/handwritingview-inkcorrection1.gif)

## <a name="disable-the-handwriting-view"></a>Désactiver l’affichage de l’écriture manuscrite

La vue de l’écriture manuscrite intégrée est activée par défaut.

Vous souhaiterez peut-être désactiver l’affichage de l’écriture manuscrite si vous fournissez une fonctionnalité d’encre en texte équivalente dans votre application ou votre expérience de saisie de texte s’appuie sur un certain type de mise en forme caractère ou spécial (par exemple, un onglet) non disponible par le biais de l’écriture manuscrite.

Dans cet exemple, nous désactivons l’affichage de l’écriture manuscrite en définissant la propriété [IsHandwritingViewEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.ishandwritingviewenabled) du contrôle [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) sur false. Tous les contrôles de texte qui prennent en charge l’affichage de l’écriture manuscrite prend en charge une propriété similaire.

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen" 
    IsHandwritingViewEnabled="False">
</TextBox>
```

## <a name="specify-the-alignment-of-the-handwriting-view"></a>Spécifier l’alignement de la vue de l’écriture manuscrite

La vue de l’écriture manuscrite est située au-dessus du contrôle de texte sous-jacent et dimensionnée en fonction des préférences de l’écriture manuscrite de l’utilisateur (voir **Paramètres-& gt; appareils-& gt; stylet et Windows Ink-& gt; l’écriture manuscrite -> taille de police lors de l’écriture directement dans le champ de texte **). L’affichage est automatiquement aligné par rapport au contrôle de texte et son emplacement dans l’application.

L’interface utilisateur de l’application n’est pas réorganisé pour prendre en charge le plus grand contrôle, afin que le système peut provoquer l’affichage pour masquer l’interface utilisateur important.

Voici comment utiliser la propriété [PlacementAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.placementalignment) d’une [zone de texte](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) pour spécifier le point d’ancrage sur le contrôle de texte sous-jacent est utilisée pour aligner la vue de l’écriture manuscrite.

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

## <a name="disable-auto-completion-candidates"></a>Désactiver les candidats de la saisie semi-automatique

La fenêtre contextuelle suggestion de texte est activée par défaut pour fournir une liste de l’entrée manuscrite supérieur candidats de reconnaissance à partir de laquelle l’utilisateur peut sélectionner au cas où le candidat supérieur est incorrect.

Si votre application déjà fournit robuste, la fonctionnalité de reconnaissance personnalisée, vous pouvez utiliser la propriété [AreCandidatesEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.arecandidatesenabled) pour désactiver les suggestions intégrées, comme illustré dans l’exemple suivant.

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

Un utilisateur peut choisir à partir d’une collection prédéfinie de polices basées sur l’écriture manuscrite à utiliser lorsque le rendu du texte basé sur reconnaissance d’entrée manuscrite (voir **Paramètres-& gt; appareils-& gt; stylet et Windows Ink-& gt; l’écriture manuscrite -> police lors de l’utilisation de l’écriture manuscrite**).

> [!NOTE]
> Les utilisateurs peuvent encore créer une police en fonction de leur propre l’écriture manuscrite.
> [!VIDEO https://www.youtube.com/embed/YRR4qd4HCw8]

Votre application peut accéder à ce paramètre et utiliser la police sélectionnée pour le texte reconnu dans le contrôle de texte.

Dans cet exemple, nous écouter l’événement [TextChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged) d’un [contrôle TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) et appliquer la police sélectionnée de l’utilisateur si la modification de texte provenance de la HandwritingView (ou une police par défaut, dans le cas contraire).

```csharp
private void SampleTextBox_TextChanged(object sender, TextChangedEventArgs e)
{
    ((TextBox)sender).FontFamily = 
        ((TextBox)sender).HandwritingView.IsOpen ?
            new FontFamily(PenAndInkSettings.GetDefault().FontFamilyName) : 
            new FontFamily("Segoe UI");
}
```

## <a name="access-the-handwritingview-in-composite-controls"></a>Accéder à la HandwritingView dans les contrôles composites

Contrôles composites qui utilisent les contrôles [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) ou [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox) , par exemple [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) également prendre en charge un [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview).

Pour accéder à la [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) dans un contrôle composite, utilisez l’API [VisualTreeHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper) .

L’extrait de code XAML suivant affiche un contrôle [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) .

```xaml
<AutoSuggestBox Name="SampleAutoSuggestBox" 
    Height="50" Width="500" 
    PlaceholderText="Auto Suggest Example" 
    FontSize="16" FontFamily="Segoe UI"  
    Loaded="SampleAutoSuggestBox_Loaded">
</AutoSuggestBox>
```

Dans le code-behind, correspondant, nous montrons comment désactiver le [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) sur l' [objet AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox).

1. Tout d’abord, nous gérons l’événement d’application chargé où nous appelons une fonction FindInnerTextBox pour démarrer la traversée d’arborescence d’éléments visuels.

    ```csharp
    private void SampleAutoSuggestBox_Loaded(object sender, RoutedEventArgs e)
    {
        if (FindInnerTextBox((AutoSuggestBox)sender))
            autoSuggestInnerTextBox.IsHandwritingViewEnabled = false;
    }
    ```

2. Ensuite, nous allons commencer itération au sein de l’arborescence visuelle (qui commencent à un objet AutoSuggestBox) dans la fonction FindInnerTextBox avec un appel à FindVisualChildByName.

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

3. Enfin, cette fonction parcourt l’arborescence d’éléments visuel jusqu'à ce que la zone de texte est récupérée.

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

## <a name="reposition-the-handwritingview"></a>Repositionner l’HandwritingView

Dans certains cas, vous devrez peut-être vous assurer que le [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) couvre les éléments d’interface utilisateur qu’il peut sinon pas.

Ici, nous créons une zone de texte qui prend en charge de la dictée (implémentée en plaçant une zone de texte et un bouton de la dictée dans un élément StackPanel).

![Élément TextBox avec la dictée](images/handwritingview/textbox-with-dictation.png)

Comme l’objet StackPanel est plus grande que la zone de texte, le [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) ne risquent pas de masquer toutes l’OLE composite.

![Élément TextBox avec la dictée](images/handwritingview/textbox-with-dictation-handwritingview.png)

Pour contourner ce problème, définissez la propriété de PlacementTarget de la [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) à l’élément d’interface utilisateur sur lequel il doit être alignée.

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

## <a name="resize-the-handwritingview"></a>Redimensionner la HandwritingView

Vous pouvez également définir la taille de la [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview), qui peut être utile lorsque vous devez vous assurer que l’affichage ne prend pas masquer l’interface utilisateur important.

Comme dans l’exemple précédent, nous créons une zone de texte qui prend en charge de la dictée (implémentée en plaçant une zone de texte et un bouton de la dictée dans un élément StackPanel).

![Élément TextBox avec la dictée](images/handwritingview/textbox-with-dictation.png)

Dans ce cas, nous souhaitons vous assurer que le bouton de la dictée est toujours visible.

![Élément TextBox avec la dictée](images/handwritingview/textbox-with-dictation-handwritingview-resize.png)

Pour ce faire, nous lions la propriété MaxWidth de la [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) à la largeur de l’élément d’interface utilisateur qu’il doit masquer.

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

Si vous disposez d’interface utilisateur personnalisée qui s’affiche en réponse à la saisie de texte, par exemple, un menu contextuel d’information, vous devrez peut-être repositionner cette interface utilisateur afin qu’il n’a pas masquer l’affichage de l’écriture manuscrite.

![Élément TextBox avec l’interface utilisateur personnalisée](images/handwritingview/textbox-with-customui.png)

L’exemple suivant montre comment écouter les événements [Opened](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.opened)et [Closed](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.closed
) [SizeChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged) de la [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) pour définir la position d’un [menu contextuel](https://docs.microsoft.com/uwp/api/windows.ui.popups).

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

## <a name="retemplate-the-handwritingview-control"></a>Redéfinir le contrôle HandwritingView

Comme avec tous les contrôles d’infrastructure XAML, vous pouvez personnaliser la structure visuelle et le comportement visuel d’un [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) à vos besoins spécifiques.

Pour voir un exemple complet de création d’un modèle personnalisé d’extraction la procédure de [créer des contrôles de transport personnalisés](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/custom-transport-controls) ou si l' [exemple de contrôle d’édition personnalisé](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl).







