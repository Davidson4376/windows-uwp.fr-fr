---
author: Karl-Bridge-Microsoft
Description: Learn how accelerator keys can improve the usability and accessibility of UWP apps.
title: Raccourcis clavier
label: Keyboard accelerators
template: detail.hbs
keywords: clavier, raccourci, touche de raccourci, raccourcis clavier, accessibilité, navigation, focus, texte, entrée, interactions utilisateur, boîtier de commande, distant
ms.author: kbridge
ms.date: 10/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
pm-contact: chigy
design-contact: miguelrb
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: ce84debc3422f923c7c88aae1fa216665ef1ef0f
ms.sourcegitcommit: 53ba430930ecec8ea10c95b390fe6e654fe363e1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/06/2018
ms.locfileid: "3408995"
---
# <a name="keyboard-accelerators"></a>Raccourcis clavier

![Clavier Surface](images/accelerators/accelerators_hero2.png)

Les touches de raccourci (ou raccourcis clavier) sont des combinaisons de touches qui améliorent l’utilisation et l’accessibilité de vos applications Windows, en offrant une méthode intuitive permettant aux utilisateurs d’appeler des actions ou des commandes courantes sans naviguer dans l’interface utilisateur de l’application.

Consultez la rubrique [Touches d'accès rapide](access-keys.md) pour plus d’informations sur la navigation dans l’interface utilisateur d’une application Windows à l’aide de raccourcis clavier.

> [!NOTE]
> Un clavier est indispensable pour les utilisateurs qui souffrent d’un handicap (voir [Accessibilité du clavier](https://docs.microsoft.com/windows/uwp/accessibility/keyboard-accessibility)). C’est aussi un outil important pour les utilisateurs qui préfèrent se servir d’un clavier, voyant cet outil comme un moyen d’interaction avec une application efficace.

## <a name="overview"></a>Vue d'ensemble

Les raccourcis incluent généralement les touches de fonction F1 à F12 ou une combinaison d’une touche standard avec une plusieurs touches de modification (CTRL, Maj).

> [!NOTE]
> Les contrôles de plateforme UWP intègrent des raccourcis clavier. Par exemple, ListView prend en charge Ctrl+A pour sélectionner tous les éléments d’une liste et RichEditBox prend en charge Ctrl+Tab pour insérer une tabulation dans la zone de texte. Ces raccourcis clavier intégrés sont appelés **raccourcis de contrôle** et sont exécutés uniquement si le focus est positionné sur l’élément ou l’un de ses enfants. Les raccourcis que vous définissez à l’aide des API de raccourcis clavier présentées ici sont appelés **raccourcis d’application**.

Les raccourcis clavier ne sont pas disponibles pour chaque action, mais sont souvent associés aux commandes affichées dans les menus (et doivent être spécifiés avec le contenu de l’élément de menu). Les raccourcis peuvent également être associés à des actions qui n’ont pas d’éléments de menu équivalents. Toutefois, étant donné que les utilisateurs utilisent les menus d’une application pour découvrir et connaître le jeu de commandes disponibles, vous devez essayer de faciliter la découverte des raccourcis (l’utilisation d’intitulés ou de modèles établis peut s’avérer utile).

![Raccourcis clavier décrits dans un intitulé d’élément de menu](images/accelerators/accelerators_menuitemlabel.png)  
*Raccourcis clavier décrits dans un intitulé d’élément de menu*

## <a name="when-to-use-keyboard-accelerators"></a>Quand utiliser les raccourcis clavier

Nous vous recommandons de spécifier les raccourcis clavier chaque fois que cela est approprié dans votre interface utilisateur, et de prendre en charge les raccourcis dans tous les contrôles personnalisés.

- Les raccourcis clavier rendent votre application plus accessible aux utilisateurs souffrant de troubles psychomoteurs, y compris ceux qui ne peuvent pas appuyer simultanément sur les touches ou qui ont des difficultés à utiliser une souris.**

  Une interface utilisateur de clavier bien conçue représente un aspect important de l’accessibilité logicielle. Elle permet aux utilisateurs malvoyants ou souffrant d’un handicap moteur de naviguer dans une application et d’interagir avec ses fonctionnalités. Les utilisateurs qui ne sont pas en mesure d’utiliser une souris peuvent avoir recours à diverses technologies d’assistance, telles que les outils de clavier amélioré, les claviers visuels, les écrans élargis, les lecteurs d’écran et les utilitaires d’entrée vocale. Pour ces utilisateurs, une couverture complète de la commande est essentielle.

- Les raccourcis clavier rendent votre application plus facile d’utilisation pour les utilisateurs avancés qui préfèrent interagir par le biais d’un clavier.

  Les utilisateurs expérimentés ont souvent une préférence marquée pour l’utilisation du clavier, car les commandes clavier peuvent être entrées plus rapidement et ne nécessitent pas de retirer les mains du clavier. Pour ces utilisateurs, l’efficacité et la cohérence sont essentielles. L’exhaustivité n’est importante que pour les commandes les plus fréquemment utilisées.

## <a name="specify-a-keyboard-accelerator"></a>Spécifier un raccourci clavier

Utilisez les API [KeyboardAccelerator](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.keyboardaccelerator.-ctor) pour créer des raccourcis clavier dans les applicationsUWP. Avec ces API, vous n’êtes pas obligé de gérer plusieurs événements KeyDown pour détecter la combinaison de touches enfoncées et vous pouvez localiser les raccourcis dans les ressources d’application.

Nous vous recommandons de définir les raccourcis clavier pour les actions les plus courantes dans votre application, et de les documenter à l’aide de l’intitulé ou de l’info-bulle de l’élément de menu. Dans cet exemple, nous déclarons les raccourcis clavier uniquement pour les commandes Renommer et Copier.

``` xaml
<CommandBar Margin="0,200" AccessKey="M">
  <AppBarButton 
    Icon="Share" 
    Label="Share" 
    Click="OnShare" 
    AccessKey="S" />
  <AppBarButton 
    Icon="Copy" 
    Label="Copy" 
    ToolTipService.ToolTip="Copy (Ctrl+C)" 
    Click="OnCopy" 
    AccessKey="C">
    <AppBarButton.KeyboardAccelerators>
      <KeyboardAccelerator 
        Modifiers="Control" 
        Key="C" />
    </AppBarButton.KeyboardAccelerators>
  </AppBarButton>

  <AppBarButton 
    Icon="Delete" 
    Label="Delete" 
    Click="OnDelete" 
    AccessKey="D" />
  <AppBarSeparator/>
  <AppBarButton 
    Icon="Rename" 
    Label="Rename" 
    ToolTipService.ToolTip="Rename (F2)" 
    Click="OnRename" 
    AccessKey="R">
    <AppBarButton.KeyboardAccelerators>
      <KeyboardAccelerator 
        Modifiers="None" Key="F2" />
    </AppBarButton.KeyboardAccelerators>
  </AppBarButton>

  <AppBarButton 
    Icon="SelectAll" 
    Label="Select" 
    Click="OnSelect" 
    AccessKey="A" />
  
  <CommandBar.SecondaryCommands>
    <AppBarButton 
      Icon="OpenWith" 
      Label="Sources" 
      AccessKey="S">
      <AppBarButton.Flyout>
        <MenuFlyout>
          <ToggleMenuFlyoutItem Text="OneDrive" />
          <ToggleMenuFlyoutItem Text="Contacts" />
          <ToggleMenuFlyoutItem Text="Photos"/>
          <ToggleMenuFlyoutItem Text="Videos"/>
        </MenuFlyout>
      </AppBarButton.Flyout>
    </AppBarButton>
    <AppBarToggleButton 
      Icon="Save" 
      Label="Auto Save" 
      IsChecked="True" 
      AccessKey="A"/>
  </CommandBar.SecondaryCommands>

</CommandBar>
```

![Raccourci clavier décrit dans une info-bulle](images/accelerators/accelerators_tooltip.png)  
***Raccourci clavier décrit dans une info-bulle***

L’objet [UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) possède une collection [KeyboardAccelerator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator), [KeyboardAccelerators](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.KeyboardAccelerators), dans laquelle vous spécifiez vos objets KeyboardAccelerator personnalisés et définissez les combinaisons de touches pour le raccourci clavier:

-   **[Key](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Key)**: [VirtualKey](https://docs.microsoft.com/uwp/api/windows.system.virtualkey) utilisée pour le raccourci clavier.

-   **[Modifiers](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Modifiers)**: [VirtualKeyModifiers](https://docs.microsoft.com/uwp/api/windows.system.virtualkeymodifiers) utilisées pour le raccourci clavier. Si Modifiers n’est pas défini, la valeur par défaut est None.

> [!NOTE]
> Les raccourcis à touche unique (A, Suppr, F2, barre d’espace, Échap, touche multimédia) et les raccourcis à touches multiples (Ctrl+Maj+M) sont pris en charge. Toutefois, les touches virtuelles de boîtier de commande ne sont pas prises en charge.

## <a name="scoped-accelerators"></a>Raccourcis dans une étendue

Certains raccourcis fonctionnent uniquement dans des étendues spécifiques, tandis que d’autres fonctionnent dans toute l’application.

Par exemple, MicrosoftOutlook inclut les raccourcis suivants:
-   Ctrl+B, Ctrl+I et Échap fonctionnent uniquement sur l’étendue relative à l’envoi d’un formulaire de courrier électronique
-   Ctrl+1 et Ctrl+2 fonctionnent dans toute l’application

### <a name="context-menus"></a>Menus contextuels

Les actions des menus contextuels affectent uniquement des zones ou des éléments spécifiques, comme les caractères sélectionnés dans un éditeur de texte ou un morceau de musique dans une playlist. Pour cette raison, nous vous recommandons de définir l’étendue des raccourcis clavier pour les éléments de menu contextuel sur le parent du menu contextuel.

Utilisez la propriété [ScopeOwner](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.ScopeOwner) pour spécifier l’étendue du raccourci clavier. Ce code montre comment implémenter un menu contextuel sur un contrôle ListView avec des raccourcis clavier dans une étendue:

``` xaml
<ListView x:Name="MyList">
  <ListView.ContextFlyout>
    <MenuFlyout>
      <MenuFlyoutItem Text="Share" Icon="Share"/>
      <MenuFlyoutItem Text="Copy" Icon="Copy">
        <MenuFlyoutItem.KeyboardAccelerators>
          <KeyboardAccelerator 
            Modifiers="Control" 
            Key="C" 
            ScopeOwner="{x:Bind MyList }" />
        </MenuFlyoutItem.KeyboardAccelerators>
      </MenuFlyoutItem>
      
      <MenuFlyoutItem Text="Delete" Icon="Delete" />
      <MenuFlyoutSeparator />
      
      <MenuFlyoutItem Text="Rename">
        <MenuFlyoutItem.KeyboardAccelerators>
          <KeyboardAccelerator 
            Modifiers="None" 
            Key="F2" 
            ScopeOwner="{x:Bind MyList}" />
        </MenuFlyoutItem.KeyboardAccelerators>
      </MenuFlyoutItem>
      
      <MenuFlyoutItem Text="Select" />
    </MenuFlyout>
    
  </ListView.ContextFlyout>
    
  <ListViewItem>Track 1</ListViewItem>
  <ListViewItem>Alternative Track 1</ListViewItem>

</ListView>
```

L’attribut ScopeOwner de l’élément MenuFlyoutItem.KeyboardAccelerators indique qu’il s’agit d’un raccourci dans une étendue, et non pas global (la valeur par défaut est null ou global). Pour plus d’informations, voir la section **Résolution des raccourcis** plus loin dans cette rubrique.

## <a name="invoke-a-keyboard-accelerator"></a>Appeler un raccourci clavier 

L’objet [KeyboardAccelerator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator) utilise le [modèle de contrôle UI Automation (UIA)](https://msdn.microsoft.com/library/windows/desktop/ee671194(v=vs.85).aspx) pour exécuter une action lorsqu’un raccourci est appelé.

Les UIA [modèles de contrôle] exposent les fonctionnalités des contrôles courants. Par exemple, le contrôle Button implémente le modèle de contrôle [Invoke](https://msdn.microsoft.com/library/windows/desktop/ee671279(v=vs.85).aspx) pour prendre en charge l’événement Click (un contrôle est généralement appelé par clic, double-clic ou en appuyant sur Entrée, un raccourci clavier prédéfini ou une autre combinaison de touches). Lorsqu’un raccourci clavier est utilisé pour appeler un contrôle, l’infrastructure XAML recherche si le contrôle implémente le modèle de contrôle Invoke et, si tel est le cas, l’active (il n’est pas nécessaire d’écouter l’événement KeyboardAcceleratorInvoked).

Dans l’exemple suivant, Ctrl+S déclenche l’événement Click, car le bouton implémente le modèle Invoke.

``` xaml 
<Button Content="Save" Click="OnSave">
  <Button.KeyboardAccelerators>
    <KeyboardAccelerator Key="S" Modifiers="Control" />
  </Button.KeyboardAccelerators>
</Button>
```

Si un élément implémente plusieurs modèles de contrôle, un seul peut être activé par le biais d’un raccourci. Les modèles de contrôle sont classés dans l’ordre de priorité suivant:
1.  Invoke (Button)
2.  Toggle (Checkbox)
3.  Selection (ListView)
4.  Expand/Collapse (ComboBox) 

Si aucune correspondance n’est identifiée, le raccourci n’est pas valide et un message de débogage s’affiche («Aucun modèle d’automation pour ce composant n’a été trouvé. Implémentez tout comportement souhaité dans l’événement Invoked. Définir la propriété Handled sur true dans votre gestionnaire d’événements supprime ce message.»)

## <a name="custom-keyboard-accelerator-behavior"></a>Comportement des raccourcis clavier personnalisés

L’événement Invoked de l’objet [KeyboardAccelerator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator) est déclenché lorsque le raccourci est exécuté. L’objet d’événement [KeyboardAcceleratorInvokedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs) inclut les propriétés suivantes:
- **Handled** (Boolean): définir cette propriété sur true empêche l'événement de déclencher le modèle de contrôle et arrête la propagation d'événements du raccourci. La valeur par défaut est false.
- **Element** (DependencyObject): objet qui contient le raccourci.

Nous montrons ici comment définir une collection de raccourcis clavier et comment gérer l’événement Invoked.

``` xaml
<ListView x:Name="MyListView">
  <ListView.KeyboardAccelerators>
    <KeyboardAccelerator Key="A" Modifiers="Control,Shift" Invoked="SelectAllInvoked" />
    <KeyboardAccelerator Key="F5" Invoked="RefreshInvoked"  />
  </ListView.KeyboardAccelerators>
</ListView>   
```

``` csharp
void SelectAllInvoked (KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
  CustomSelectAll(MyListView);
  args.Handled = true;
}

void RefreshInvoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
  Refresh(MyListView);
  args.Handled = true;
}
```

## <a name="disable-a-keyboard-accelerator"></a>Désactiver un raccourci clavier 

Si un contrôle est désactivé, le raccourci associé est également désactivé. Dans l’exemple suivant, comme la propriété IsEnabled de ListView est définie sur false, le raccourci Ctrl+A associé ne peut pas être appelé.

``` xaml
<ListView >
  <ListView.KeyboardAccelerators>
    <KeyboardAccelerator Key="A" 
      Modifiers="Control"
      Invoked="CustomListViewSelecAllInvoked" />
  </ListView.KeyboardAccelerators>
  
  <TextBox>
    <TextBox.KeyboardAccelerators>
      <KeyboardAccelerator 
        Key="A" 
        Modifiers="Control" 
        Invoked="CustomTextSelecAllInvoked" 
        IsEnabled="False" />
    </TextBox.KeyboardAccelerators>
  </TextBox>

<ListView>
```

Les contrôles parents et enfants peuvent partager le même raccourci. Dans ce cas, le contrôle parent peut être appelé même si le focus est positionné sur l’enfant et que son raccourci est désactivé.

## <a name="screen-readers-and-keyboard-accelerators"></a>Lecteurs d’écran et raccourcis clavier 

Les lecteurs d’écran tels que Narrateur peuvent annoncer la combinaison de touches du raccourci clavier aux utilisateurs. Par défaut, il s’agit de chaque touche de modification (dans l’ordre d’énumération VirtualModifiers) suivie par la touche (et séparées par des signes «+»). Vous pouvez personnaliser cela par le biais de la propriété AutomationProperties associée [AcceleratorKey](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.AcceleratorKeyProperty). Si plusieurs raccourcis sont spécifiés, seul le premier est annoncé.

Dans cet exemple, AutomationProperty.AcceleratorKey renvoie la chaîne «CTRL+Maj+A»:

``` xaml
<ListView x:Name="MyListView">
  <ListView.KeyboardAccelerators>

    <KeyboardAccelerator 
      Key="A" 
      Modifiers="Control,Shift" 
      Invoked="CustomSelectAllInvoked" />
      
    <KeyboardAccelerator 
      Key="F5" 
      Modifiers="None" 
      Invoked="RefreshInvoked" />

  </ListView.KeyboardAccelerators>

</ListView>   
```

> [!NOTE] 
> Le fait de définir AutomationProperties.AcceleratorKey n’active pas la fonctionnalité clavier, cela indique uniquement à l’infrastructure UIA les touches utilisées.

## <a name="common-keyboard-accelerators"></a>Raccourcis clavier courants

Nous vous conseillons de créer des raccourcis clavier cohérents entre les applicationsUWP. Les utilisateurs doivent mémoriser les raccourcis clavier et s'attendent à obtenir des résultats identiques (ou similaires).

Cela n’est pas toujours possible en raison des différences de fonctionnalités entre les applications.

| **Édition** | **Raccourci clavier courant** |
| ------------- | ----------------------------------- |
| Commencer le mode édition | Ctrl+E |
| Sélectionner tous les éléments dans un contrôle ou une fenêtre sur lequel ou laquelle se trouve le focus | Ctrl+A |
| Rechercher et remplacer | Ctrl+H |
| Annuler | Ctrl+Z |
| Rétablir | Ctrl+Y |
| Supprimer la sélection et la copier dans le Presse-papiers | Ctrl+X |
| Copier la sélection dans le Presse-papiers | Ctrl+C, Ctrl+Insertion |
| Coller le contenu du Presse-papiers | Ctrl+V, Maj+Insertion |
| Coller le contenu du Presse-papiers (avec options) | Ctrl+Alt+V |
| Renommer un élément | F2 |
| Ajouter un nouvel élément | Ctrl+N |
| Ajouter un nouvel élément secondaire | Ctrl+Maj+N |
| Supprimer l’élément sélectionné (avec annulation) | Suppr, Ctrl+D |
| Supprimer l’élément sélectionné (sans annulation) | Maj+Suppr |
| Mettre en gras | Ctrl+B |
| Souligner | Ctrl+U |
| Mettre en italiques | Ctrl+I |

| **Navigation** | |
| ------------- | ----------------------------------- |
| Rechercher du contenu dans un contrôle ou une fenêtre sur lequel ou laquelle se trouve le focus | Ctrl+F |
| Atteindre le résultat suivant de la recherche | F3 |

| **Autres actions** | |
| ------------- | ----------------------------------- |
| Ajouter aux Favoris | Ctrl+D | 
| Actualiser | F5 ou Ctrl+R | 
| Zoom avant | Ctrl++ | 
| Zoom arrière | Ctrl+- | 
| Zoom vers l’affichage par défaut | Ctrl+0 | 
| Enregistrer | Ctrl+S | 
| Fermer | Ctrl+W | 
| Imprimer | Ctrl+P | 

Notez que certaines des combinaisons ne sont pas valides pour les versions localisées de Windows. Par exemple, dans la version espagnole de Windows, Ctrl+N est utilisé pour la mise en gras au lieu de Ctrl+B. Nous vous recommandons de fournir des raccourcis clavier localisés si l’application est localisée.

## <a name="usability-affordances-for-keyboard-accelerators"></a>Affordances de facilité d’utilisation pour les raccourcis clavier

### <a name="tooltips"></a>Info-bulles

Comme les raccourcis clavier ne sont généralement pas décrit directement dans l’interface utilisateur de votre application UWP, vous pouvez améliorer la détectabilité via des [info-bulles](../controls-and-patterns/tooltips.md), qui s’affichent automatiquement lorsque l’utilisateur déplace le focus sur, maintient l’appui sur ou pointe le pointeur de la souris sur un contrôle. L’info-bulle peut identifier si un contrôle est associé à un raccourci clavier et, si tel est le cas, quelle est la combinaison de touches de raccourci.

**Windows 10, Version 1803 (mise à jour du mois d’avril 2018) et versions ultérieures**

Par défaut, lorsque les raccourcis clavier sont déclarés, tous les contrôles (à l’exception [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem) et [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)) présentent les combinaisons de touches correspondants dans une info-bulle.

> [!NOTE] 
> Si plusieurs raccourcis sont définis pour un contrôle, seul le premier est présenté.

![Info-bulle de touche de raccourci](images/accelerators/accelerators_tooltip_savebutton_small.png)

*Combinaison de touches de raccourci dans une info-bulle*

Pour les objets [AppBarToggleButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbartogglebutton) , [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton)et [bouton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button), le raccourci clavier est ajouté à info-bulle de la valeur par défaut du contrôle. Pour [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton) et [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)) des objets, le raccourci clavier s’affiche avec le texte de menu volant.

> [!NOTE]
> En spécifiant une info-bulle (voir Button1 dans l’exemple suivant) permet de remplacer ce comportement.

```xaml
<StackPanel x:Name="Container" Grid.Row="0" Background="AliceBlue">
    <Button Content="Button1" Margin="20"
            Click="OnSave" 
            KeyboardAcceleratorPlacementMode="Auto" 
            ToolTipService.ToolTip="Tooltip">
        <Button.KeyboardAccelerators>
            <KeyboardAccelerator  Key="A" Modifiers="Windows"/>
        </Button.KeyboardAccelerators>
    </Button>
    <Button Content="Button2"  Margin="20"
            Click="OnSave" 
            KeyboardAcceleratorPlacementMode="Auto">
        <Button.KeyboardAccelerators>
            <KeyboardAccelerator  Key="B" Modifiers="Windows"/>
        </Button.KeyboardAccelerators>
    </Button>
    <Button Content="Button3"  Margin="20"
            Click="OnSave" 
            KeyboardAcceleratorPlacementMode="Auto">
        <Button.KeyboardAccelerators>
            <KeyboardAccelerator  Key="C" Modifiers="Windows"/>
        </Button.KeyboardAccelerators>
    </Button>
</StackPanel>
```

![Info-bulle de touche de raccourci](images/accelerators/accelerators-button-small.png)

*Touches de raccourci ajouté à l’info-bulle de la valeur par défaut du bouton*

```xaml
<AppBarButton Icon="Save" Label="Save">
    <AppBarButton.KeyboardAccelerators>
        <KeyboardAccelerator Key="S" Modifiers="Control"/>
    </AppBarButton.KeyboardAccelerators>
</AppBarButton>
```

![Info-bulle de touche de raccourci](images/accelerators/accelerators-appbarbutton-small.png)

*Touches de raccourci ajouté à l’info-bulle de la valeur par défaut de AppBarButton*

```xaml
<AppBarButton AccessKey="R" Icon="Refresh" Label="Refresh" IsAccessKeyScope="True">
    <AppBarButton.Flyout>
        <MenuFlyout>
            <MenuFlyoutItem AccessKey="A" Icon="Refresh" Text="Refresh A">
                <MenuFlyoutItem.KeyboardAccelerators>
                    <KeyboardAccelerator Key="R" Modifiers="Control"/>
                </MenuFlyoutItem.KeyboardAccelerators>
            </MenuFlyoutItem>
            <MenuFlyoutItem AccessKey="B" Icon="Globe" Text="Refresh B" />
            <MenuFlyoutItem AccessKey="C" Icon="Globe" Text="Refresh C" />
            <MenuFlyoutItem AccessKey="D" Icon="Globe" Text="Refresh D" />
            <ToggleMenuFlyoutItem AccessKey="E" Icon="Globe" Text="ToggleMe">
                <MenuFlyoutItem.KeyboardAccelerators>
                    <KeyboardAccelerator Key="Q" Modifiers="Control"/>
                </MenuFlyoutItem.KeyboardAccelerators>
            </ToggleMenuFlyoutItem>
        </MenuFlyout>
    </AppBarButton.Flyout>
</AppBarButton>
```

![Info-bulle de touche de raccourci](images/accelerators/accelerators-appbar-menuflyoutitem-small.png)

*Touches de raccourci ajouté au texte de MenuFlyoutItem*

Contrôlez le comportement de présentation à l’aide de la propriété [KeyboardAcceleratorPlacementMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.KeyboardAcceleratorPlacementMode), qui accepte deux valeurs: [Auto](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardacceleratorplacementmode) ou [Hidden](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardacceleratorplacementmode).    

```xaml
<Button Content="Save" Click="OnSave" KeyboardAcceleratorPlacementMode="Auto">
    <Button.KeyboardAccelerators>
        <KeyboardAccelerator Key="S" Modifiers="Control" />
    </Button.KeyboardAccelerators>
</Button>
```

Dans certains cas, vous devrez peut-être présenter une info-bulle relative à un autre élément (généralement un objet conteneur). Par exemple, un contrôle Pivot qui affiche l’info-bulle pour un PivotItem avec l’en-tête Pivot. 

Voici comment utiliser la propriété KeyboardAcceleratorPlacementTarget pour afficher la combinaison de touches de raccourcis clavier pour un bouton Enregistrer avec le conteneur Grid au lieu du bouton.

```xaml
<Grid x:Name="Container" Padding="30">
  <Button Content="Save"
    Click="OnSave"
    KeyboardAcceleratorPlacementMode="Auto"
    KeyboardAcceleratorPlacementTarget="{x:Bind Container}">
    <Button.KeyboardAccelerators>
      <KeyboardAccelerator  Key="S" Modifiers="Control" />
    </Button.KeyboardAccelerators>
  </Button>
</Grid>
```

### <a name="labels"></a>Intitulés

Dans certains cas, nous recommandons d’utiliser l’intitulé d'un contrôle pour identifier si le contrôle est associé à un raccourci clavier et, si tel est le cas, quelle est la combinaison de touches de raccourcis. 

Certains contrôles de plateforme le font par défaut, en particulier les objets [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem) et [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem), tandis que [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton) et [AppBarToggleButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbartogglebutton) le font lorsqu’ils apparaissent dans le menu de dépassement de la [CommandBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar).

![Raccourcis clavier décrits dans un intitulé d’élément de menu](images/accelerators/accelerators_menuitemlabel.png)  
*Raccourcis clavier décrits dans un intitulé d’élément de menu*

Vous pouvez remplacer le texte de l’intitulé par défaut d’un raccourci par le biais de la propriété [KeyboardAcceleratorTextOverride](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton.KeyboardAcceleratorTextOverride) des contrôles [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem), [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem), [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton) et [AppBarToggleButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbartogglebutton) (utilisez un seul espace pour aucun texte). 

> [!NOTE] 
> Le texte de remplacement n’est pas présenté si le système ne peut pas détecter un clavier connecté (vous pouvez le vérifier vous-même par le biais de la propriété [KeyboardPresent](https://docs.microsoft.com/uwp/api/windows.devices.input.keyboardcapabilities.KeyboardPresent)).

## <a name="advanced-concepts"></a>Concepts avancés

Nous allons examiner ici certains aspects de bas niveau des raccourcis clavier.

### <a name="when-an-accelerator-is-invoked"></a>Quand un raccourci est appelé

Les raccourcis sont constitués de deux types de touches: les touches de modification et les touches de non-modification. Les touches de modification incluent les touches Maj, Menu, Ctrl et Windows, qui sont exposées via [VirtualKeyModifiers](http://msdn.microsoft.com/library/windows/apps/xaml/Windows.System.VirtualKeyModifiers). Les touches de non-modification sont n’importe quelle touche virtuelle, comme Suppr, F3, barre d’espace, Échap et toutes les touches alphanumériques et touches de ponctuation. Un raccourci clavier est appelé lorsque l’utilisateur appuie sur une touche de non-modification tout en maintenant enfoncée(s) une ou plusieurs touches de modification. Par exemple, si l’utilisateur appuie sur Ctrl+Maj+M, lorsque la touche M est enfoncée l’infrastructure vérifie les touches de modification (Ctrl et Maj) et déclenche le raccourci, s’il existe.

> [!NOTE]
> Par conception, le raccourci est à répétition automatique (par exemple, lorsque l’utilisateur appuie sur Ctrl+Maj, puis maintient enfoncée la touche M, le raccourci est appelé de façon répétée jusqu'à ce que la touche M soit relâchée). Ce comportement ne peut pas être modifié.

### <a name="input-event-priority"></a>Priorité des événements d’entrée
Les événements d’entrée se produisent dans un ordre spécifique que vous pouvez intercepter et gérer en fonction des exigences de votre application. 

#### <a name="the-keydownkeyup-bubbling-event"></a>Événement de propagation KeyDown/KeyUp 

En XAML, une frappe est traitée comme s’il existe uniquement un pipeline de propagation d’entrée. Ce pipeline d’entrée est utilisé par les événements KeyDown/KeyUp et les entrées de caractères. Par exemple, si le focus est positionné sur un élément et que l’utilisateur appuie sur une touche, un événement KeyDown est déclenché sur l’élément, suivi par le parent de l’élément, et ainsi de suite jusqu'au haut de l’arborescence, jusqu'à ce que la propriété args.Handled soit définie sur true.

L’événement KeyDown est également utilisé par certains contrôles pour implémenter les raccourcis de contrôle intégrés. Lorsqu’un contrôle est associé à un raccourci clavier, il gère l’événement KeyDown, ce qui signifie qu’il n'y aura pas de propagation d'événements KeyDown. Par exemple, RichEditBox prend en charge la copie avec Ctrl+C. Lorsque vous appuyez sur Ctrl, l’événement KeyDown est déclenché et se propage, mais lorsque l’utilisateur appuie sur C en même temps, l’événement KeyDown est marqué Handled et n’est pas déclenché (sauf si le paramètre handledEventsToo de [UIElement.AddHandler](http://msdn.microsoft.com/library/windows/apps/xaml/Windows.UI.Xaml.UIElement.AddHandler) est défini sur true).

#### <a name="the-characterreceived-event"></a>Événement CharacterReceived

Comme l’événement [CharacterReceived](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.CharacterReceived) est déclenché après l’événement [KeyDown](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.KeyDown) pour les contrôles de texte tels que TextBox, vous pouvez annuler les entrées de caractères dans le gestionnaire d’événements KeyDown.

#### <a name="the-previewkeydown-and-previewkeyup-events"></a>Événements PreviewKeyDown et PreviewKeyUp

Les événements d’entrée d'aperçu sont déclenchés avant tout autre événement. Si vous ne gérez pas ces événements, le raccourci pour l’élément sur lequel est positionné le focus est déclenché, suivi de l’événement KeyDown. Les deux événements se propagent jusqu'à ce qu’ils soient gérés.


![Séquence d’événements de touches](images/accelerators/accelerators_keyevents.png)
***Séquence d’événements de touches***

Ordre des événements:

Événements Preview KeyDown...
raccourci d’application méthode OnKeyDown événement KeyDown raccourcis d’application sur le parent méthode OnKeyDown sur le parent événement KeyDown sur le parent (se propage à la racine)...
événement CharacterReceived événements PreviewKeyUp KeyUpEvents

Lorsque l’événement de raccourci est géré, l’événement KeyDown est également marqué comme géré. L’événement KeyUp reste non géré.

### <a name="resolving-accelerators"></a>Résolution des raccourcis

Un événement de raccourci clavier se propage à partir de l’élément sur lequel est positionné le focus jusqu'à la racine. Si l’événement n’est pas géré, l’infrastructure XAML recherche d'autres raccourcis d’application hors étendue en dehors du chemin de propagation.

Lorsque deux raccourcis clavier sont définis avec la même combinaison de touches, le premier raccourci clavier trouvé sur l’arborescence visuelle est appelé.

Les raccourcis clavier dans une étendue sont appelés uniquement quand le focus est positionné à l’intérieur d’une étendue spécifique. Par exemple, dans une grille qui contient des dizaines de contrôles, un raccourci clavier peut être appelé pour un contrôle uniquement quand le focus se trouve dans la grille (le propriétaire d’étendue).

### <a name="scoping-accelerators-programmatically"></a>Contrôle de l’étendue des raccourcis par programmation

La méthode [UIElement.TryInvokeKeyboardAccelerator](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.uielement.tryinvokekeyboardaccelerator) appelle tous les raccourcis correspondants dans la sous-arborescence de l’élément.

La méthode [UIElement.OnProcessKeyboardAccelerators](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.uielement.onprocesskeyboardaccelerators) est exécutée avant le raccourci clavier. Cette méthode transmet un objet [ProcessKeyboardAcceleratorArgs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.processkeyboardacceleratoreventargs) qui contient la touche, la touche de modification et une valeur booléenne indiquant si le raccourci clavier est géré. S’il est marqué comme géré, le raccourci clavier se propage (de sorte que le raccourci clavier extérieur n’est jamais appelé).

> [!NOTE]
> OnProcessKeyboardAccelerators se déclenche toujours, qu’il soit géré ou non (similaire à l’événement OnKeyDown). Vous devez vérifier si l’événement a été marqué comme géré.

Dans cet exemple, nous utilisons OnProcessKeyboardAccelerators et TryInvokeKeyboardAccelerator pour contrôler l’étendue des raccourcis clavier à l’objet Page:

``` csharp
protected override void OnProcessKeyboardAccelerators(
  ProcessKeyboardAcceleratorArgs args)
{
  if(args.Handled != true)
  {
    this.TryInvokeKeyboardAccelerator(args);
    args.Handled = true;
  }
}
```

### <a name="localize-the-accelerators"></a>Localiser les raccourcis

Nous vous recommandons de localiser tous les raccourcis clavier. Vous pouvez effectuer cette opération avec le fichier de ressources UWP standard (.resw) et l’attribut x:Uid dans vos déclarations XAML. Dans cet exemple, WindowsRuntime charge automatiquement les ressources.

![Localisation des raccourcis clavier avec un fichier de ressources UWP](images/accelerators/accelerators_localization.png)
***Localisation des raccourcis clavier avec un fichier de ressources UWP***

``` xaml
<Button x:Uid="myButton" Click="OnSave">
  <Button.KeyboardAccelerators>
    <KeyboardAccelerator x:Uid="myKeyAccelerator" Modifiers="Control"/>
  </Button.KeyboardAccelerators>
</Button>
```

### <a name="setup-an-accelerator-programmatically"></a>Définir un raccourci par programmation

Voici un exemple de définition d’un raccourci par programmation:

``` csharp
void AddAccelerator(
  VirtualKeyModifiers keyModifiers, 
  VirtualKey key, 
  TypedEventHandler<KeyboardAccelerator, KeyboardAcceleratorInvokedEventArgs> handler )
  {
    var accelerator = 
      new KeyboardAccelerator() 
      { 
        Modifiers = keyModifiers, Key = key
      };
    accelerator.Invoked = handler;
    this.KeyAccelerators.Add(accelerator);
  }
```

> [!NOTE]
> KeyboardAccelerator n’est pas partageable, le même KeyboardAccelerator ne peut pas être ajouté à plusieurs éléments.

### <a name="override-keyboard-accelerator-behavior"></a>Comportement de remplacement des raccourcis clavier

Vous pouvez gérer l’événement [KeyboardAccelerator.Invoked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Invoked) pour remplacer le comportement du KeyboardAccelerator par défaut.

Cet exemple montre comment remplacer la commande «Tout sélectionner» (raccourci clavier Ctrl+A) dans un contrôle ListView personnalisé. Nous avons également défini la propriété Handled sur true pour arrêter la propagation de l’événement.

```csharp
public class MyListView : ListView
{
  …
  protected override void OnKeyboardAcceleratorInvoked(KeyboardAcceleratorInvokedEventArgs args) 
  {
    if(args.Accelerator.Key == VirtualKey.A 
      && args.Accelerator.Modifiers == KeyboardModifiers.Control)
    {
      CustomSelectAll(TypeOfSelection.OnlyNumbers); 
      args.Handled = true;
    }
  }
  …
}
```

## <a name="related-articles"></a>Articles associés

* [Interactions avec le clavier](keyboard-interactions.md)
* [Touches d’accès rapide](access-keys.md)

**Exemples**
* [Galerie de contrôles XAML (c'est-à-dire XamlUiBasics)](https://github.com/Microsoft/Windows-universal-samples/tree/c2aeaa588d9b134466bbd2cc387c8ff4018f151e/Samples/XamlUIBasics)


 

