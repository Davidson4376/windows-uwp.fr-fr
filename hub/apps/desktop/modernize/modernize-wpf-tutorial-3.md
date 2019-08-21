---
description: Ce didacticiel montre comment ajouter des interfaces utilisateur en XAML UWP, créer des packages MSIX et intégrer d’autres composants modernes à votre application WPF.
title: Ajouter un contrôle CalendarView UWP à l’aide d'îles XAML
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, îlots XAML
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 830c1cdf2e24e716d51642bc65b5b6783d0d784a
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643372"
---
# <a name="part-3-add-a-uwp-calendarview-control-using-xaml-islands"></a>Partie 3 : Ajouter un contrôle CalendarView UWP à l’aide d'îles XAML

Il s’agit de la troisième partie d’un didacticiel qui montre comment moderniser un exemple d’application de bureau WPF nommée Contoso depenses. Pour obtenir une vue d’ensemble du didacticiel, des conditions préalables et des instructions pour télécharger l’exemple [d’application, consultez le didacticiel: Moderniser une application](modernize-wpf-tutorial.md)WPF. Cet article suppose que vous avez déjà effectué la [partie 2](modernize-wpf-tutorial-2.md).

Dans le scénario fictif de ce didacticiel, l’équipe de développement de contoso souhaite faciliter la sélection de la date d’une note de frais sur un appareil tactile. Dans cette partie du didacticiel, vous allez ajouter un contrôle [CALENDARVIEW](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/calendar-view) UWP à l’application. Il s’agit du même contrôle que celui utilisé dans la fonctionnalité date et heure de Windows 10 dans la barre des tâches.

![Image CalendarViewControl](images/wpf-modernize-tutorial/CalendarViewControl.png)

Contrairement au contrôle **InkCanvas** que vous avez ajouté dans la [partie 2](modernize-wpf-tutorial-2.md), le kit de commandes de la communauté Windows ne fournit pas de version encapsulée du **CalendarView** UWP qui peut être utilisé dans les applications WPF. Vous pouvez également héberger un **InkCanvas** dans le contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) générique. Vous pouvez utiliser ce contrôle pour héberger n’importe quel contrôle UWP de première partie fourni par l’SDK Windows ou la bibliothèque WinUI ou tout contrôle UWP personnalisé créé par un tiers. Le contrôle **WindowsXamlHost** est fourni par le `Microsoft.Toolkit.Wpf.UI.XamlHost` package NuGet du package. Ce package est inclus dans le `Microsoft.Toolkit.Wpf.UI.Controls` package NuGet que vous avez installé dans la [partie 2](modernize-wpf-tutorial-2.md).

> [!NOTE]
> Ce didacticiel montre uniquement comment utiliser [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) pour héberger le contrôle **CalendarView** du premier tiers fourni par le SDK Windows. Pour obtenir une procédure pas à pas qui montre comment héberger un contrôle personnalisé, consultez [héberger un contrôle UWP personnalisé dans une application WPF à l’aide des îlots XAML](host-custom-control-with-xaml-islands.md).

Pour utiliser le contrôle **WindowsXamlHost** , vous devez appeler directement les API WinRT à partir du code de l’application WPF. Le `Microsoft.Windows.SDK.Contracts` package NuGet contient les références nécessaires pour vous permettre d’appeler des API WinRT à partir de l’application. Ce package est également inclus dans le `Microsoft.Toolkit.Wpf.UI.Controls` package NuGet que vous avez installé dans la [partie 2](modernize-wpf-tutorial-2.md).

## <a name="add-the-windowsxamlhost-control"></a>Ajouter le contrôle WindowsXamlHost

1. Dans **Explorateur de solutions**, développez le dossier **views** dans le projet **ContosoExpenses. Core** , puis double-cliquez sur le fichier **AddNewExpense. Xaml** . Il s’agit du formulaire utilisé pour ajouter une nouvelle dépense à la liste. Voici comment il apparaît dans la version actuelle de l’application.

    ![Ajouter une nouvelle dépense](images/wpf-modernize-tutorial/AddNewExpense.png)

    Le contrôle de sélecteur de dates inclus dans WPF est destiné aux ordinateurs traditionnels avec la souris et le clavier. Le choix d’une date avec un écran tactile n’est pas faisable, en raison de la petite taille du contrôle et de l’espace limité entre chaque jour dans le calendrier.

2. En haut du fichier **AddNewExpense. Xaml** , ajoutez l’attribut suivant à l’élément **Window** .

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    Après avoir ajouté cet attribut, l’élément de **fenêtre** doit maintenant ressembler à ceci.

    ```xml
    <Window x:Class="ContosoExpenses.Views.AddNewExpense"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            DataContext="{Binding Source={StaticResource ViewModelLocator},Path=AddNewExpenseViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Add new expense" Height="450" Width="800"
            Background="{StaticResource AddNewExpenseBackground}">
    ```

3. Remplacez l’attribut **Height** de l’élément **Window** de 450 par 800. Cela est nécessaire, car le contrôle UWP **CalendarView** prend plus d’espace que le sélecteur de dates WPF.

    ```xml
    <Window x:Class="ContosoExpenses.Views.AddNewExpense"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            DataContext="{Binding Source={StaticResource ViewModelLocator},Path=AddNewExpenseViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Add new expense" Height="800" Width="800"
            Background="{StaticResource AddNewExpenseBackground}">
    ```

4. Localisez `DatePicker` l’élément près du bas du fichier et remplacez cet élément par le code XAML suivant.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  />
    ```

    Ce code XAML ajoute le contrôle **WindowsXamlHost** . La propriété **InitialTypeName** indique le nom complet du contrôle UWP que vous souhaitez héberger (dans ce cas, **Windows. UI. Xaml. Controls. CalendarView**).

5. Appuyez sur F5 pour générer et exécuter l’application dans le débogueur. Choisissez un employé dans la liste, puis appuyez sur le bouton **Ajouter une nouvelle dépense** . Vérifiez que la page suivante héberge le nouveau contrôle UWP **CalendarView** .

    ![Wrapper CalendarView](images/wpf-modernize-tutorial/CalendarViewWrapper.png)

6. Fermez l’application.

## <a name="interact-with-the-windowsxamlhost-control"></a>Interagir avec le contrôle WindowsXamlHost

Ensuite, vous allez mettre à jour l’application pour traiter la date sélectionnée, l’afficher à l’écran et remplir l’objet **Expense** pour l’enregistrer dans la base de données.

Le [CALENDARVIEW](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) UWP contient deux membres qui sont pertinents pour ce scénario:

- La propriété **SelectedDates** contient la date sélectionnée par l’utilisateur.
- L’événement **SelectedDatesChanged** est déclenché lorsque l’utilisateur sélectionne une date.

Toutefois, le contrôle **WindowsXamlHost** est un contrôle hôte générique pour *tout* type de contrôle UWP. Par conséquent, elle n’expose pas de propriété nommée **SelectedDates** ni d’un événement appelé **SelectedDatesChanged**, car ils sont spécifiques au contrôle **CalendarView** . Pour accéder à ces membres, vous devez écrire du code qui convertit le **WindowsXamlHost** en type **CalendarView** . Le meilleur emplacement pour ce faire est en réponse à l’événement **ChildChanged** du contrôle **WindowsXamlHost** , qui est déclenché lorsque le contrôle hébergé a été rendu.

1. Dans le fichier **AddNewExpense. Xaml** , ajoutez un gestionnaire d’événements pour l’événement **ChildChanged** du contrôle **WindowsXamlHost** que vous avez ajouté précédemment. Lorsque vous avez terminé, l’élément **WindowsXamlHost** doit ressembler à ceci.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  ChildChanged="CalendarUwp_ChildChanged" />
    ```

2. Dans le même fichier, recherchez l’élément **Grid. RowDefinitions** de la **grille**principale. Ajoutez un élément **RowDefinition** plus avec la **hauteur** égale à **auto** à la fin de la liste d’éléments enfants. Lorsque vous avez terminé, l’élément **Grid. RowDefinitions** doit ressembler à ceci (il doit maintenant y avoir 9 éléments **RowDefinition** ).

    ```xml
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    ```

4. Ajoutez le code XAML suivant après l’élément **WindowsXamlHost** et avant l’élément **Button** près de la fin du fichier.

    ```xml
    <TextBlock Text="Selected date:" FontSize="16" FontWeight="Bold" Grid.Row="7" Grid.Column="0" />
    <TextBlock Text="{Binding Path=Date}" FontSize="16" Grid.Row="7" Grid.Column="1" />
    ```

5. Localisez l’élément **Button** près de la fin du fichier et modifiez la propriété **Grid. Row** de **7** à **8**. Cela décale le bouton d’une ligne vers le haut dans la grille, car vous avez ajouté une nouvelle ligne.

    ```xml
    <Button Content="Save" Grid.Row="8" Grid.Column="0" Command="{Binding Path=SaveExpenseCommand}" Margin="5, 12, 0, 0" HorizontalAlignment="Left" Width="180" />
    ```

6. Ouvrez le fichier de code **AddNewExpense.Xaml.cs** .

7. Ajoutez l’instruction suivante au début du fichier.

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

8. Ajoutez le gestionnaire d'événements suivant à la classe `AddNewExpense`. Ce code implémente l’événement **ChildChanged** du contrôle **WindowsXamlHost** que vous avez déclaré précédemment dans le fichier XAML.

    ```csharp
    private void CalendarUwp_ChildChanged(object sender, System.EventArgs e)
    {
        WindowsXamlHost windowsXamlHost = (WindowsXamlHost)sender;

        Windows.UI.Xaml.Controls.CalendarView calendarView =
            (Windows.UI.Xaml.Controls.CalendarView)windowsXamlHost.Child;

        if (calendarView != null)
        {
            calendarView.SelectedDatesChanged += (obj, args) =>
            {
                if (args.AddedDates.Count > 0)
                {
                    Messenger.Default.Send<SelectedDateMessage>(new SelectedDateMessage(args.AddedDates[0].DateTime));
                }
            };
        }
    }
    ```

    Ce code utilise la propriété **enfant** du contrôle **WindowsXamlHost** pour accéder au contrôle de l’aide UWP. Le code s’abonne ensuite à l’événement **SelectedDatesChanged** , qui est déclenché lorsque l’utilisateur sélectionne une date dans le calendrier. Ce gestionnaire d’événements transmet la date sélectionnée au ViewModel dans lequel le nouvel objet **Expense** est créé et enregistré dans la base de données. Pour ce faire, le code utilise l’infrastructure de messagerie fournie par le package MVVM Light NuGet. Le code envoie un message appelé **SelectedDateMessage** au ViewModel, qui le reçoit et définit la propriété **Date** avec la valeur sélectionnée. Dans ce scénario, le contrôle **CalendarView** est configuré pour le mode de sélection unique, de sorte que la collection ne contiendra qu’un seul élément.

    À ce stade, le projet ne se compile toujours pas, car l’implémentation de la classe **SelectedDateMessage** est manquante. Les étapes suivantes implémentent cette classe.

9. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier **messages** et choisissez **Ajouter-> classe**. Nommez la nouvelle classe **SelectedDateMessage** , puis cliquez sur **Ajouter**.

10. Remplacez le contenu du fichier de code **SelectedDateMessage.cs** par le code suivant. Ce code ajoute une instruction using pour l' `GalaSoft.MvvmLight.Messaging` espace de noms (à partir du package MVVM Light NuGet), hérite de la classe **MessageBase** et ajoute la propriété **DateTime** qui est initialisée par le biais du constructeur public. Lorsque vous avez terminé, le fichier doit ressembler à ceci.

    ```csharp
    using GalaSoft.MvvmLight.Messaging;
    using System;
    
    namespace ContosoExpenses.Messages
    {
        public class SelectedDateMessage: MessageBase
        {
            public DateTime SelectedDate { get; set; }
    
            public SelectedDateMessage(DateTime selectedDate)
            {
                this.SelectedDate = selectedDate;
            }
        }
    }
    ```

11. Ensuite, vous allez mettre à jour le ViewModel pour recevoir ce message et remplir la propriété **Date** du ViewModel. Dans **Explorateur de solutions**, développez le dossier **ViewModels** et ouvrez le fichier **AddNewExpenseViewModel.cs** .

12. Recherchez le constructeur public pour la `AddNewExpenseViewModel` classe et ajoutez le code suivant à la fin du constructeur. Ce code s’inscrit pour recevoir le **SelectedDateMessage**, extrayez la date sélectionnée à partir de celle-ci via la propriété **SelectedDate** , et nous l’utilisons pour définir la propriété de **Date** exposée par le ViewModel. Étant donné que cette propriété est liée au contrôle **TextBlock** que vous avez ajouté précédemment, vous pouvez voir la date sélectionnée par l’utilisateur.

    ```csharp
    Messenger.Default.Register<SelectedDateMessage>(this, message =>
    {
        Date = message.SelectedDate;
    });
    ```

    Lorsque vous avez terminé, le `AddNewExpenseViewModel` constructeur doit ressembler à ceci. 

    ```csharp
    public AddNewExpenseViewModel(IDatabaseService databaseService, IStorageService storageService)
    {
        this.databaseService = databaseService;
        this.storageService = storageService;

        Date = DateTime.Today;

        Messenger.Default.Register<SelectedDateMessage>(this, message =>
        {
            Date = message.SelectedDate;
        });
    }
    ```

    > [!NOTE]
    > La `SaveExpenseCommand` méthode dans le même fichier de code effectue le travail d’enregistrement des dépenses dans la base de données. Cette méthode utilise la propriété **Date** pour créer le nouvel objet **Expense** . Cette propriété est maintenant remplie par le contrôle **CalendarView** par le biais `SelectedDateMessage` du message que vous venez de créer.

13. Appuyez sur F5 pour générer et exécuter l’application dans le débogueur. Choisissez l’un des employés disponibles, puis cliquez sur le bouton **Ajouter une nouvelle dépense** . Complétez tous les champs du formulaire et choisissez une date dans le nouveau contrôle **CalendarView** . Confirmez que la date sélectionnée s’affiche sous la forme d’une chaîne sous le contrôle.

14. Appuyez sur le bouton **Enregistrer** . Le formulaire sera fermé et la nouvelle dépense sera ajoutée à la fin de la liste des dépenses. La première colonne avec la date de dépense doit être la date que vous avez sélectionnée dans le contrôle **CalendarView** .

## <a name="next-steps"></a>Étapes suivantes

À ce stade du didacticiel, vous avez remplacé un contrôle de date et d’heure WPF par le contrôle UWP **CalendarView** , qui prend en charge les stylets tactiles et numériques en plus de l’entrée au clavier et à la souris. Bien que le kit de commandes de la communauté Windows ne fournisse pas de version encapsulée du contrôle de la fenêtre de lancement UWP qui peut être utilisé directement dans une application WPF, vous avez pu héberger le contrôle à l’aide du contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) générique.

Vous êtes maintenant prêt pour [la partie 4: Ajoutez des activités et des notifications](modernize-wpf-tutorial-4.md)utilisateur Windows 10.
