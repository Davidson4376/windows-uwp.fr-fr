---
Description: Instructions de disposition pour les formulaires dans les applications UWP.
title: Formulaires
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: windows 10, uwp, fluent
ms.openlocfilehash: 8a57f13e168a248569bca1beeceed7b4f6c89f69
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658154"
---
# <a name="forms"></a>Formulaires
Un formulaire est un groupe de contrôles de collecter et envoyer des données provenant d’utilisateurs. Les formulaires sont généralement utilisés pour les pages de paramètres, enquêtes, création de comptes et bien plus encore. 

Cet article décrit les instructions de conception pour la création de dispositions de XAML pour les formulaires.

![Exemple de formulaire](images/PivotHeader.png)

## <a name="when-should-you-use-a-form"></a>Quand faut-il utiliser un formulaire ?
Un formulaire est une page dédiée pour la collecte des entrées de données qui sont clairement liées entre eux. Vous devez utiliser un formulaire lorsque vous avez besoin de collecter des données à partir d’un utilisateur de manière explicite. Vous pouvez créer un formulaire pour un utilisateur :
- Connectez-vous à un compte
- S’inscrire à un compte
- Modifier les paramètres de l’application, telles que confidentialité ou options d’affichage
- À un questionnaire
- Acheter un élément
- Envoyer vos commentaires

## <a name="types-of-forms"></a>Types de formulaires

Lorsque vous abordez comment l’entrée d’utilisateur est soumise et affichée, il existe deux types de formulaires :

### <a name="1-instantly-updating"></a>1. La mise à jour immédiatement
![Page Paramètres](images/control-examples/toggle-switch-news.png)

Utiliser un formulaire instantanément la mise à jour lorsque vous souhaitez que les utilisateurs puissent voir immédiatement les résultats de la modification des valeurs sous la forme. Par exemple, dans les pages de paramètres, les sélections actuelles sont affichées, et toutes les modifications apportées aux sélections sont appliquées immédiatement. Pour valider les modifications dans votre application, vous devrez [ajouter un gestionnaire d’événements](controls-and-events-intro.md) à chaque contrôle d’entrée. Si un utilisateur modifie un contrôle d’entrée, puis votre application peut réagir de façon appropriée.

### <a name="2-submitting-with-button"></a>2. Envoi de bouton
L’autre type de formulaire permet à l’utilisateur à choisir à quel moment envoyer des données en un clic sur un bouton.

![calendrier ajouter une nouvelle page d’événement](images/calendar-form.png)

Ce type de formulaire donne la flexibilité de l’utilisateur dans la réponse. En règle générale, ce type de formulaire contient plusieurs champs d’entrée de forme libre et reçoit donc une plus grande variété de réponses. Pour garantir l’entrée d’utilisateur valide et correctement mis en forme des données lors de la soumission, tenez compte des recommandations suivantes :

- Rendent impossible d’envoyer des informations non valides en utilisant le contrôle approprié (par exemple, utilisez un CalendarDatePicker plutôt qu’une zone de texte pour les dates du calendrier). Découvrez plus en sélectionnant les contrôles d’entrée appropriés dans votre formulaire dans la section de contrôles d’entrée plus tard.
- Lorsque vous utilisez des contrôles de zone de texte, fournir aux utilisateurs une indication du format d’entrée souhaitée avec la [PlaceholderText](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox.PlaceholderText) propriété.
- Fournir aux utilisateurs avec le bon clavier visuel en indiquant l’entrée attendue d’un contrôle avec le [InputScope](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.inputscope) propriété.
- Mark requis d’entrée avec un astérisque * sur l’étiquette.
- Désactiver le bouton envoyer jusqu'à ce que toutes les informations requises sont renseignées.
- S’il existe des données non valides lors de la soumission, marquer les contrôles avec une entrée non valide avec des champs en surbrillance ou de bordures et oblige l’utilisateur à soumettre à nouveau le formulaire.
- Pour les autres erreurs, telles que de l’échec de connexion réseau, veillez à afficher un message d’erreur à l’utilisateur. 


## <a name="layout"></a>Disposition

Pour faciliter l’expérience utilisateur et vous assurer que les utilisateurs sont en mesure d’entrer l’entrée correcte, tenez compte des recommandations suivantes pour la mise en page pour les formulaires. 

### <a name="labels"></a>Étiquettes
[Étiquettes](labels.md) doit être alignée à gauche et placé au-dessus du contrôle d’entrée. De nombreux contrôles ont une propriété d’en-tête intégrée pour afficher l’étiquette. Pour les contrôles dépourvus de propriété Header, ou pour étiqueter des groupes de contrôles, vous pouvez utiliser un élément [TextBlock](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.TextBlock).

Pour [conception pour l’accessibilité](../accessibility/accessibility.md), tous les individuels et les groupes de contrôles pour plus de clarté pour les deux humaines et les lecteurs d’écran de l’étiquette. 

Pour les styles de police, utilisez la valeur par défaut [rampe de type UWP](../style/typography.md). Utilisez `TitleTextBlockStyle` pour les titres des pages, `SubtitleTextBlockStyle` pour les en-têtes de groupe, et `BodyTextBlockStyle` pour les étiquettes de contrôle.

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
Pour séparer visuellement des groupes de contrôles de l’autre, utilisez [alignement, marges et remplissage](../layout/alignment-margin-padding.md). Les contrôles d’entrée individuels sont 80px de hauteur et doivent être espacés 24 de px les unes des autres. Groupes de contrôles d’entrée doivent être espacés 48 de px uns des autres.

![groupes de formulaires](images/forms-groups.png)

### <a name="columns"></a>Colonnes
Création de colonnes peut réduire l’espace blanc inutile dans les formulaires, surtout avec des tailles d’écran plus grands. Toutefois, si vous souhaitez créer un formulaire de plusieurs colonne, le nombre de colonnes doit s’appuyer sur le nombre de contrôles d’entrée sur la page et la taille d’écran de la fenêtre d’application. Plutôt que de surcharger l’écran avec de nombreux contrôles d’entrée, envisagez de créer plusieurs pages de votre formulaire.  

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
Formulaires doivent être redimensionné en tant que les modifications de taille écran ou fenêtre, donc les utilisateurs ne négligent pas tous les champs d’entrée. Pour plus d’informations, consultez [techniques de conception réactives](../layout/responsive-design.md). Par exemple, vous souhaiterez conserver toujours des régions spécifiques du formulaire dans la vue, quelle que soit la taille d’écran.

![focus de formulaires](images/forms-focus2.png)

### <a name="tab-stops"></a>Taquets de tabulation
Les utilisateurs peuvent utiliser le clavier pour naviguer de contrôles avec [des taquets de tabulation](../input/keyboard-interactions.md#tab-stops). Par défaut, l’ordre de tabulation des contrôles reflète l’ordre dans lequel ils sont créés dans XAML. Pour remplacer le comportement par défaut, modifiez le **IsTabStop** ou **TabIndex** les propriétés du contrôle. 

![focus de tabulation sur le contrôle de formulaire](images/forms-focus1.png)

## <a name="input-controls"></a>Contrôles d’entrée
Les contrôles d’entrée sont les éléments d’interface utilisateur qui permettent aux utilisateurs d’entrer des informations sous une forme. Certains contrôles communs qui peuvent être ajoutés aux formulaires sont mentionnées ci-dessous, ainsi que des informations sur la façon de les utiliser.

### <a name="text-input"></a>Saisie de texte
Commande | Utilisez | Exemple
 - | - | -
[Zone de texte](text-box.md) | Capturer une ou plusieurs lignes de texte | Les noms, les réponses de forme libre ou des commentaires
[PasswordBox](password-box.md) | Collecter des données privées en dissimuler les caractères | Mots de passe, les numéros de sécurité sociale (SSN), les codes confidentiels, les informations de carte de crédit 
[AutoSuggestBox](auto-suggest-box.md) | Afficher les utilisateurs à une liste de suggestions à partir d’un jeu de données correspondant en tapant | Recherche de base de données, de messagerie dans : ligne, les requêtes précédentes
[RichEditBox](rich-edit-box.md) | Modifier des fichiers texte avec texte mis en forme, liens hypertexte et images | Chargement du fichier, version préliminaire et modifier dans l’application

### <a name="selection"></a>d’un certificat SSTP
Commande | Utilisez | Exemple
- | - | - 
| [Case à cocher](checkbox.md) | Sélectionnez ou désélectionnez un ou plusieurs éléments d’action | Acceptez les termes et conditions, ajouter des éléments facultatifs, sélectionnez toutes les réponses applicables
[Case d’option](radio-button.md) | Sélectionnez une option à partir de deux ou plusieurs choix | Choisissez le type, méthode, etc. de livraison.
[ToggleSwitch](toggles.md) | Choisissez une des deux options qui s’excluent mutuellement | Activé/désactivé

> **Remarque** : S’il existe cinq ou plusieurs éléments de sélection, utilisez un contrôle de liste.

### <a name="lists"></a>Listes
Commande | Utilisez | Exemple
- | - | -
[Zone de liste déroulante](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists.md#drop-down-lists) | Démarrer dans un état compact et se développent pour afficher la liste d’éléments sélectionnables | Sélectionnez à partir d’une longue liste d’éléments, tels que des États ou des pays
[ListView](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists#list-views) | Classement des éléments affecter des en-têtes de groupe, faites glisser et déplacer des éléments, organiser le contenu et réorganiser les éléments | Options de classement
[GridView](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists#grid-views) | Organiser et parcourir des collections basées sur l’image | Choisissez une photo, couleur, afficher le thème

### <a name="numeric-input"></a>Entrées numériques
Commande | Utilisez | Exemple
- | - | -
[Curseur](slider.md) | Sélectionnez un nombre dans une plage de valeurs numériques contiguës | Pourcentages, volume, la vitesse de lecture
[Contrôle d’accès](rating.md) | Taux avec étoiles | Retour d'expérience du client

### <a name="date-and-time"></a>Date et heure

Commande | Utilisez 
- | - 
[CalendarView](calendar-view.md) | Sélectionner une date unique ou une plage de dates à partir d’un calendrier toujours visible 
[CalendarDatePicker](calendar-date-picker.md) | Choisir une date unique dans un calendrier contextuel 
[DatePicker](date-picker.md) | Choisir une date localisée unique lorsque les informations contextuelles n’est pas importante
[TimePicker](time-picker.md) | Choisir une seule valeur d’heure

### <a name="additional-controls"></a>Contrôles supplémentaires 
Pour obtenir une liste complète des contrôles UWP, consultez [index des contrôles par fonction](controls-by-function.md).

Pour les contrôles d’interface utilisateur plus complexes et personnalisés, examinez ressources UWP proposées par des entreprises telles que [Telerik](https://www.telerik.com/), [SyncFusion](https://www.syncfusion.com/products/uwp), [DevExpress](https://www.devexpress.com/Products/NET/Controls/Win10Apps/), [ Infragistics](https://www.infragistics.com/products/universal-windows-platform), [ComponentOne](https://www.componentone.com/Studio/Platform/UWP), et [ActiPro](https://www.actiprosoftware.com/products/controls/universal).

## <a name="one-column-form-example"></a>Exemple de formulaire d’une colonne
Cet exemple utilise un ACRYLIQUE [maître/détail](master-details.md) [mode liste](lists.md) et [NavigationView](navigationview.md) contrôle.
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

## <a name="two-column-form-example"></a>Exemple de formulaire de deux colonnes
Cet exemple utilise le [Pivot](pivot.md) contrôle, [ACRYLIQUE](../style/acrylic.md) en arrière-plan, et [CommandBar](app-bars.md) en plus des contrôles d’entrée.
![Capture d’écran de l’exemple de formulaire](images/FormExample.png)
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

## <a name="customer-orders-database-sample"></a>Exemple de base de données de commandes client
![capture d’écran de la base de données des commandes client](images/customerorderform.png) pour savoir comment connecter l’entrée de formulaire à un **Azure** de base de données et voir un formulaire totalement implémenté, consultez le [base de données des commandes clients](https://github.com/Microsoft/Windows-appsample-customers-orders-database) exemple d’application.

## <a name="related-topics"></a>Rubriques connexes
- [Contrôles d’entrée](controls-and-events-intro.md)
- [Typographie](../style/typography.md)
