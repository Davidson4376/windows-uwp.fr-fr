---
Description: Recommandations de disposition pour les formulaires dans les applications UWP
title: Formulaires
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: windows 10, uwp, fluent
ms.openlocfilehash: 8a57f13e168a248569bca1beeceed7b4f6c89f69
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63794216"
---
# <a name="forms"></a>Formulaires
Un formulaire est un groupe de contrôles qui collectent des données auprès de l’utilisateur et les envoient. Les formulaires sont généralement utilisés pour les pages de paramètres, les enquêtes, la création de comptes et bien plus encore. 

Cet article aborde les recommandations pour la conception de dispositions XAML pour les formulaires.

![Exemple de formulaire](images/PivotHeader.png)

## <a name="when-should-you-use-a-form"></a>Quand utiliser un formulaire ?
Un formulaire est une page dédiée à la collecte d’entrées de données qui sont clairement liées les unes aux autres. Vous devez utiliser un formulaire quand vous avez besoin de collecter des données auprès d’un utilisateur de manière explicite. Vous pouvez créer un formulaire pour un utilisateur dans le cadre des scénarios suivants :
- Se connecter à un compte
- S’inscrire pour obtenir un compte
- Changer les paramètres de l’application, tels que les options de confidentialité ou d’affichage
- Répondre à un questionnaire
- Acheter un article
- Envoyer vos commentaires

## <a name="types-of-forms"></a>Types de formulaires

L’envoi et l’affichage des entrées utilisateur font appel à deux types de formulaires :

### <a name="1-instantly-updating"></a>1. Mise à jour instantanée
![Page de paramètres](images/control-examples/toggle-switch-news.png)

Utilisez un formulaire avec mise à jour instantanée si vous souhaitez que les utilisateurs voient immédiatement les résultats de la modification des valeurs du formulaire. Par exemple, dans les pages de paramètres, les sélections actuelles sont affichées, et toutes les modifications apportées à celles-ci sont appliquées immédiatement. Pour valider les modifications dans votre application, vous devez [ajouter un gestionnaire d’événements](controls-and-events-intro.md) à chaque contrôle d’entrée. Si un utilisateur change un contrôle d’entrée, votre application peut réagir de façon appropriée.

### <a name="2-submitting-with-button"></a>2. Envoi avec un bouton
L’autre type de formulaire permet à l’utilisateur de choisir à quel moment envoyer les données au moyen d’un clic sur un bouton.

![Page d’ajout de nouvel événement dans un calendrier](images/calendar-form.png)

Ce type de formulaire permet à l’utilisateur de répondre avec une certaine souplesse. En règle générale, ce type de formulaire contient plusieurs champs d’entrée libre et reçoit donc une plus grande variété de réponses. Pour vous assurer que l’entrée utilisateur est valide et que les données sont correctement mises en forme au moment de l’envoi, tenez compte des recommandations suivantes :

- Rendez impossible l’envoi d’informations non valides en utilisant le contrôle approprié (par exemple, utilisez un CalendarDatePicker plutôt qu’un TextBox pour les dates du calendrier). Pour plus d’informations sur la sélection des contrôles d’entrée appropriés dans votre formulaire, consultez la section Contrôles d’entrée plus loin.
- Quand vous utilisez des contrôles TextBox, fournissez aux utilisateurs une indication du format d’entrée souhaité avec la propriété [PlaceholderText](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox.PlaceholderText).
- Fournissez aux utilisateurs le bon clavier visuel en indiquant l’entrée attendue d’un contrôle avec la propriété [InputScope](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.inputscope).
- Marquez une entrée obligatoire d’un astérisque * sur l’étiquette.
- Désactivez le bouton d’envoi jusqu’à ce que toutes les informations requises soient renseignées.
- S’il existe des données non valides au moment de l’envoi, marquez les contrôles ayant une entrée non valide en mettant en valeur les champs ou bordures correspondants et obligez l’utilisateur à renvoyer le formulaire.
- Pour les autres erreurs, telles que l’échec de la connexion réseau, veillez à afficher un message d’erreur approprié à l’utilisateur. 


## <a name="layout"></a>Disposition

Pour faciliter l’expérience utilisateur et vous assurer que les utilisateurs peuvent fournir l’entrée correcte, tenez compte des recommandations suivantes sur la conception des dispositions des formulaires. 

### <a name="labels"></a>Étiquettes
Les [étiquettes](labels.md) doivent être alignées à gauche et placées au-dessus du contrôle d’entrée. De nombreux contrôles disposent d’une propriété Header intégrée pour afficher l’étiquette. Pour les contrôles dépourvus de propriété Header, ou pour étiqueter des groupes de contrôles, vous pouvez utiliser un élément [TextBlock](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.TextBlock).

Pour [faciliter l’accessibilité](../accessibility/accessibility.md), étiquetez chaque contrôle et groupe de contrôles afin qu’il soit clairement identifiable par les lecteurs humains et les lecteurs d’écran. 

Pour les styles de police, utilisez la [gamme de caractères UWP](../style/typography.md) par défaut. Utilisez `TitleTextBlockStyle` pour les titres de page, `SubtitleTextBlockStyle` pour les en-têtes de groupe et `BodyTextBlockStyle` pour les étiquettes de contrôle.

<div class="mx-responsive-img">
<table>
<tr><th>Pratiques conseillées</th><th>Pratiques déconseillées</th></tr>
<tr>
<td><img src="../controls-and-patterns/images/forms-shortform1col.png" alt="form with top labels"></td>
<td><img src="../controls-and-patterns/images/forms-leftlabel-donot1.png" alt="form with left labels don't"></td>
</tr>
</table>
</div>

### <a name="spacing"></a>Espacement
Pour séparer visuellement les groupes de contrôles les uns des autres, utilisez [l’alignement, les marges et l’espacement](../layout/alignment-margin-padding.md). Les contrôles d’entrée individuels ont une hauteur de 80 px et doivent être espacés de 24 px les uns des autres. Les groupes de contrôles d’entrée doivent être espacés de 48 px les uns des autres.

![groupes de formulaires](images/forms-groups.png)

### <a name="columns"></a>Colonnes
La création de colonnes peut réduire l’espace blanc inutile dans les formulaires, surtout avec des tailles d’écran plus grandes. Toutefois, si vous souhaitez créer un formulaire multicolonne, le nombre de colonnes doit dépendre du nombre de contrôles d’entrée dans la page et de la taille d’écran de la fenêtre d’application. Plutôt que de surcharger l’écran avec de nombreux contrôles d’entrée, envisagez de créer plusieurs pages pour votre formulaire.  

<div class="mx-responsive-img">
<table>
<tr><th>Pratiques conseillées</th><th>Pratiques déconseillées</th></tr>
<tr>
<td><img src="../controls-and-patterns/images/forms-2cols.png" alt="form with 2 columns"></td>
<td><img src="../controls-and-patterns/images/forms-2cols-bad.png" alt="form with 2 bad columns"></td>
</tr>
<tr><td><img src="../controls-and-patterns/images/forms-3cols.png" alt="form with 3 columns"></td></tr>
</table>

</div>

### <a name="responsive-layout"></a>Disposition dynamique
Les formulaires doivent être redimensionnés quand la taille de l’écran ou de la fenêtre change, afin qu’aucun champ d’entrée n’échappe à l’utilisateur. Pour plus d’informations, consultez [Techniques de conception dynamique](../layout/responsive-design.md). Par exemple, vous pouvez souhaiter que des régions spécifiques du formulaire soient toujours visibles quelle que soit la taille de l’écran.

![focus des formulaires](images/forms-focus2.png)

### <a name="tab-stops"></a>Taquets de tabulation
Les utilisateurs peuvent utiliser le clavier pour parcourir les contrôles avec des [taquets de tabulation](../input/keyboard-interactions.md#tab-stops). Par défaut, l’ordre de tabulation des contrôles reflète l’ordre dans lequel ils sont créés en XAML. Pour remplacer le comportement par défaut, changez les propriétés **IsTabStop** ou **TabIndex** du contrôle. 

![focus de tabulation sur le contrôle dans le formulaire](images/forms-focus1.png)

## <a name="input-controls"></a>Contrôles d’entrée
Les contrôles d’entrée sont les éléments d’interface utilisateur qui permettent aux utilisateurs d’entrer des informations dans les formulaires. Certains contrôles courants qui peuvent être ajoutés aux formulaires sont listés ci-dessous ainsi que les situations dans lesquelles les utiliser.

### <a name="text-input"></a>Saisie de texte
Commande | Utilisez | Exemple
 - | - | -
[TextBox](text-box.md) | Capturer une ou plusieurs lignes de texte | Noms, réponses libres ou commentaires
[PasswordBox](password-box.md) | Collecter des données privées en masquant les caractères | Mots de passe, numéros de sécurité sociale (SSN), codes PIN, informations de carte de crédit 
[AutoSuggestBox](auto-suggest-box.md) | Présenter à l’utilisateur une liste de suggestions à partir d’un jeu de données correspondant à mesure qu’il tape des caractères | Recherche dans une base de données, ligne du destinataire d’un e-mail, requêtes précédentes
[RichEditBox](rich-edit-box.md) | Modifier des fichiers texte contenant du texte mis en forme, des liens hypertexte et des images | Chargement d’un fichier, aperçu et modification dans l’application

### <a name="selection"></a>d’un certificat SSTP
Commande | Utilisez | Exemple
- | - | - 
| [CheckBox](checkbox.md) | Sélectionner ou désélectionner un ou plusieurs éléments d’action | Accepter les conditions, ajouter des éléments facultatifs, sélectionner toutes les réponses applicables
[RadioButton](radio-button.md) | Sélectionner une option parmi deux ou plusieurs choix | Choisir un type, un mode de livraison, etc.
[ToggleSwitch](toggles.md) | Choisir une option parmi deux options qui s’excluent mutuellement | Activé/désactivé

> **Remarque** : S’il existe cinq éléments de sélection ou plus, utilisez un contrôle de liste.

### <a name="lists"></a>Listes
Commande | Utilisez | Exemple
- | - | -
[ComboBox](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists.md#drop-down-lists) | Démarrer dans un état compact et développer pour afficher la liste des éléments sélectionnables | Sélectionner dans une longue liste d’éléments, tels que des États ou des pays
[ListView](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists#list-views) | Catégoriser des éléments et affecter des en-têtes de groupe, glisser-déplacer des éléments, traiter du contenu et réorganiser des éléments | Options de classement
[GridView](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists#grid-views) | Organiser et parcourir des collections basées sur des images | Choisir une photo, une couleur, un thème d’affichage

### <a name="numeric-input"></a>Entrée numérique
Commande | Utilisez | Exemple
- | - | -
[Curseur](slider.md) | Sélectionner un nombre dans une plage de valeurs numériques contiguës | Pourcentages, volume, vitesse de lecture
[Évaluation](rating.md) | Évaluation avec étoiles | Retour d'expérience du client

### <a name="date-and-time"></a>Date et heure

Commande | Utilisez 
- | - 
[CalendarView](calendar-view.md) | Sélectionner une date unique ou une plage de dates à partir d’un calendrier toujours visible 
[CalendarDatePicker](calendar-date-picker.md) | Sélectionner une date unique à partir d’un calendrier contextuel 
[DatePicker](date-picker.md) | Sélectionner une seule date localisée quand les informations contextuelles importent peu
[TimePicker](time-picker.md) | Sélectionner une seule valeur d’heure

### <a name="additional-controls"></a>Contrôles supplémentaires 
Pour obtenir la liste complète des contrôles UWP, consultez [Index des contrôles par fonction](controls-by-function.md).

Pour les contrôles d’interface utilisateur personnalisés et plus complexes, consultez les ressources UWP disponibles auprès de sociétés comme [Telerik](https://www.telerik.com/), [SyncFusion](https://www.syncfusion.com/products/uwp), [DevExpress](https://www.devexpress.com/Products/NET/Controls/Win10Apps/), [Infragistics](https://www.infragistics.com/products/universal-windows-platform), [ComponentOne](https://www.componentone.com/Studio/Platform/UWP) et [ActiPro](https://www.actiprosoftware.com/products/controls/universal).

## <a name="one-column-form-example"></a>Exemple de formulaire monocolonne
Cet exemple utilise un [mode liste](lists.md) [maître/détail](master-details.md) acrylique et un contrôle [NavigationView](navigationview.md).
![Capture d’écran d’un autre exemple de formulaire](images/FormExample2.png)
```xaml
<StackPanel>
    <TextBlock Text="New Customer" Style="{StaticResource TitleTextBlockStyle}"/>
    <Button Height="100" Width="100" Background="LightGray" Content="Add photo" Margin="0,24" Click="AddPhotoButton_Click"/>
    <TextBox x:Name="Name" Header= "Name" Margin="0,24,0,0" MaxLength="32" Width="400" HorizontalAlignment="Left" InputScope="PersonalFullName"/>
    <TextBox x:Name="PhoneNumber" Header="Phone Number" Margin="0,24,0,0" MaxLength="15" Width="400" HorizontalAlignment="Left" InputScope="TelephoneNumber" />
    <TextBox x:Name="Email" Header="Email" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
    <TextBox x:Name="Address" Header="Address" PlaceholderText="Address" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
    <TextBox x:Name="Address2" Margin="0,24,0,0" PlaceholderText="Address 2" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
    <RelativePanel>
        <TextBox x:Name="City" PlaceholderText="City" Margin="0,24,0,0" MaxLength="50" Width="200" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
        <ComboBox x:Name="State" PlaceholderText="State" Margin="24,24,0,0"  Width="100" RelativePanel.RightOf="City">
             <x:String>WA</x:String>
        </ComboBox>
    </RelativePanel>
    <TextBox x:Name="ZipCode" PlaceholderText="Zip Code" Margin="0,24,0,0" MaxLength="6" Width="100" HorizontalAlignment="Left" InputScope="Number" />
    <StackPanel Orientation="Horizontal">
        <Button Content="Save" Margin="0,24" Click="SaveButton_Click"/>
        <Button Content="Cancel" Margin="24" Click="CancelButton_Click"/>
    </StackPanel>  
</StackPanel>
```

## <a name="two-column-form-example"></a>Exemple de formulaire à deux colonnes
Cet exemple utilise le contrôle [Pivot](pivot.md), l’arrière-plan [acrylique](../style/acrylic.md) et un [CommandBar](app-bars.md) en plus de contrôles d’entrée.
![Capture d’écran d’un exemple de formulaire](images/FormExample.png)
```xaml
<Grid>
    <Pivot Background="{ThemeResource SystemControlAccentAcrylicWindowAccentMediumHighBrush}" >
        <Pivot.TitleTemplate>
            <DataTemplate>
                <Grid>
                    <TextBlock Text="Company Name" Style="{ThemeResource HeaderTextBlockStyle}"/>
                </Grid>
            </DataTemplate>
        </Pivot.TitleTemplate>
        <PivotItem Header="Orders" Margin="0"/>    
        <PivotItem Header="Customers" Margin="0">
            <!--Form Example-->
            <Grid Background="White">
                <RelativePanel>
                    <StackPanel x:Name="Customer" Margin="20">
                        <TextBox x:Name="CustomerName" Header= "Customer Name" Margin="0,24,0,0" MaxLength="320" Width="400" HorizontalAlignment="Left" InputScope="PersonalFullName"/>
                        <TextBox x:Name="CustomerPhoneNumber" Header="Phone Number" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="TelephoneNumber" />
                        <TextBox x:Name="Address" Header="Address" PlaceholderText="Address" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="AlphanumericFullWidth" />
                        <TextBox x:Name="Address2" Margin="0,24,0,0" PlaceholderText="Address 2" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="AlphanumericFullWidth" />
                        <RelativePanel>
                            <TextBox x:Name="City" PlaceholderText="City" Margin="0,24,0,0" MaxLength="50" Width="200" HorizontalAlignment="Left" InputScope="Text" />
                            <ComboBox x:Name="State" PlaceholderText="State" Margin="24,24,0,0"  Width="100" RelativePanel.RightOf="City">
                                <x:String>WA</x:String>
                            </ComboBox>
                        </RelativePanel>
                        <TextBox x:Name="ZipCode" PlaceholderText="Zip Code" Margin="0,24,0,0" MaxLength="6" Width="100" HorizontalAlignment="Left" InputScope="Number" />
                    </StackPanel>
                    <StackPanel x:Name="Associate" Margin="20" RelativePanel.RightOf="Customer">
                        <TextBox x:Name="AssociateName" Header= "Associate" Margin="0,24,0,0" MaxLength="320" Width="400" HorizontalAlignment="Left" InputScope="PersonalFullName"/>
                        <TextBox x:Name="AssociatePhoneNumber" Header="Phone Number" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="TelephoneNumber" />
                        <DatePicker x:Name="TargetInstallDate" Header="Target install Date" HorizontalAlignment="Left" Margin="0,24,0,0"></DatePicker>
                        <TimePicker x:Name="InstallTime" Header="Install Time" HorizontalAlignment="Left" Margin="0,24,0,0"></TimePicker>
                    </StackPanel>
                </RelativePanel>
            </Grid>
        </PivotItem>
        <PivotItem Header="Invoices"/>
        <PivotItem Header="Stock"/>
        <Pivot.RightHeader>
            <CommandBar OverflowButtonVisibility="Collapsed" Background="Transparent">
                <AppBarButton Icon="Add"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Edit" />
                <AppBarButton Icon="Delete"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Save"/>
            </CommandBar>
        </Pivot.RightHeader>
    </Pivot>
</Grid>
```

## <a name="customer-orders-database-sample"></a>Exemple de base de données de commandes de clients
![capture d’écran d’une base de données de commandes de clients](images/customerorderform.png) Pour savoir comment connecter une entrée de formulaire à une base de données **Azure** et voir un formulaire totalement implémenté, consultez l’exemple d’application [Base de données de commandes de clients](https://github.com/Microsoft/Windows-appsample-customers-orders-database).

## <a name="related-topics"></a>Rubriques connexes
- [Contrôles d’entrée](controls-and-events-intro.md)
- [Typographie](../style/typography.md)
