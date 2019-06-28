---
description: Ce didacticiel montre comment ajouter des interfaces utilisateur UWP XAML, créer des packages MSIX et incorporer d’autres composants modernes dans votre application WPF.
title: Ajouter un contrôle UWP CalendarView à l’aide de XAML (îles)
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, uwp, WinForms, wpf, xaml (îles)
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: fce9135267461f61515c7dc04bbaf43b1e4c04ed
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420113"
---
# <a name="part-3-add-a-uwp-calendarview-control-using-xaml-islands"></a>Partie 3 : Ajouter un contrôle UWP CalendarView à l’aide de XAML (îles)

Il s’agit de la troisième partie d’un didacticiel qui montre comment moderniser une application de bureau WPF exemple nommée dépenses de Contoso. Pour une vue d’ensemble du didacticiel, configuration requise et des instructions pour télécharger l’exemple d’application, consultez [didacticiel : Moderniser une application WPF](modernize-wpf-tutorial.md). Cet article suppose que vous avez déjà effectué [partie 2](modernize-wpf-tutorial-2.md).

Dans le scénario fictif de ce didacticiel, l’équipe de développement Contoso souhaite rendre plus facile de choisir la date pour une note de frais sur un appareil tactile. Dans cette partie du didacticiel, vous allez ajouter une UWP [CalendarView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/calendar-view) contrôle à l’application. Il s’agit du même contrôle qui est utilisé dans les fonctionnalités de date et l’heure de Windows 10 dans la barre des tâches.

![Image de CalendarViewControl](images/wpf-modernize-tutorial/CalendarViewControl.png)

Contrairement à la **InkCanvas** contrôler que vous avez ajouté dans [partie 2](modernize-wpf-tutorial-2.md), le Kit de ressources de communauté de Windows ne fournit pas d’une version incluse dans un wrapper de la plateforme Windows universelle **CalendarView** qui peut être utilisé dans les applications WPF . Comme alternative, vous allez héberger un **InkCanvas** dans le modèle générique [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) contrôle. Vous pouvez utiliser ce contrôle pour héberger n’importe quel contrôle UWP fourni par le fournisseur tiers de plateforme ou un 3e. Le **WindowsXamlHost** contrôle est fourni par le `Microsoft.Toolkit.Wpf.UI.XamlHost` package NuGet de package. Ce package est inclus dans le `Microsoft.Toolkit.Wpf.UI.Controls` package NuGet que vous avez installé dans [partie 2](modernize-wpf-tutorial-2.md).

Pour pouvoir utiliser le **WindowsXamlHost** contrôle, vous devez appeler directement WinRT APIs à partir du code dans l’application WPF. Le `Microsoft.Windows.SDK.Contracts` package NuGet contient les références nécessaires pour vous permettre d’appeler WinRT APIs à partir de l’application. Ce package est également inclus dans le `Microsoft.Toolkit.Wpf.UI.Controls` package NuGet que vous avez installé dans [partie 2](modernize-wpf-tutorial-2.md).

## <a name="add-the-windowsxamlhost-control"></a>Ajouter le contrôle WindowsXamlHost

1. Dans **l’Explorateur de solutions**, développez le **vues** dossier dans le **ContosoExpenses.Core** de projet et double-cliquez sur le **AddNewExpense.xaml**fichier. Il s’agit de la forme utilisée pour ajouter une nouvelle dépense à la liste. Voici comment il apparaît dans la version actuelle de l’application.

    ![Ajouter une nouvelle dépense](images/wpf-modernize-tutorial/AddNewExpense.png)

    Le contrôle de sélecteur de dates inclus dans WPF est conçu pour les ordinateurs traditionnels avec la souris et clavier. Choisir une date avec un écran tactile n’est pas réellement réalisable, en raison de la petite taille du contrôle et de l’espace limité entre chaque jour dans le calendrier.

2. En haut de la **AddNewExpense.xaml** , ajoutez l’attribut suivant à la **fenêtre** élément.

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    Après l’ajout de cet attribut, le **fenêtre** élément doit maintenant ressembler à ceci.

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

3. Modifier le **hauteur** attribut de la **fenêtre** élément de 450 à 800. Cela est nécessaire car la plateforme Windows universelle **CalendarView** contrôle prend plus d’espace que le sélecteur de dates WPF.

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

4. Recherchez le `DatePicker` élément près du bas du fichier et remplacez cet élément par le XAML suivant.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  />
    ```

    Ce XAML ajoute le **WindowsXamlHost** contrôle. Le **InitialTypeName** propriété indique le nom complet du contrôle UWP que vous voulez héberger (dans ce cas, **Windows.UI.Xaml.Controls.CalendarView**).

5. Appuyez sur F5 pour générer et exécuter l’application dans le débogueur. Choisissez un employé dans la liste, puis appuyez sur la **ajouter nouvelle dépense** bouton. Vérifiez que la page suivante héberge la nouvelle plateforme Windows universelle **CalendarView** contrôle.

    ![CalendarView wrapper](images/wpf-modernize-tutorial/CalendarViewWrapper.png)

6. Fermez l’application.

## <a name="interact-with-the-windowsxamlhost-control"></a>Interagir avec le contrôle WindowsXamlHost

Ensuite, vous allez mettre à jour l’application pour traiter la date sélectionnée et affichez-la sur l’écran de remplir le **dépenses** objet à enregistrer dans la base de données.

La plateforme Windows universelle [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) contient deux membres qui sont pertinents pour ce scénario :

- Le **SelectedDates** propriété comporte la date sélectionnée par l’utilisateur.
- Le **SelectedDatesChanged** événement est déclenché lorsque l’utilisateur sélectionne une date.

Toutefois, le **WindowsXamlHost** contrôle est un contrôle hôte générique pour *n’importe quel* type de contrôle UWP. Par conséquent, il n’expose pas une propriété appelée **SelectedDates** ou un événement appelé **SelectedDatesChanged**, car elles sont spécifiques de la **CalendarView** contrôle. Pour accéder à ces membres, vous devez écrire du code qui effectue un cast de la **WindowsXamlHost** à la **CalendarView** type. Le meilleur endroit pour ce faire est en réponse à la **ChildChanged** événements de la **WindowsXamlHost** contrôle, qui est déclenché lorsque le contrôle hébergé a été restitué.

1. Dans le **AddNewExpense.xaml** de fichiers, ajoutez un gestionnaire d’événements pour le **ChildChanged** événements de la **WindowsXamlHost** contrôle que vous avez ajouté précédemment. Lorsque vous avez terminé, le **WindowsXamlHost** élément doit ressembler à ceci.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  ChildChanged="CalendarUwp_ChildChanged" />
    ```

2. Dans le même fichier, recherchez le **Grid.RowDefinitions** élément des principales **grille**. Ajouter un autre **RowDefinition** élément avec la **hauteur** égal à **automatique** à la fin de la liste des éléments enfants. Lorsque vous avez terminé, le **Grid.RowDefinitions** élément doit ressembler à ceci (il doit maintenant être 9 **RowDefinition** éléments).

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

4. Ajoutez le XAML suivant après la **WindowsXamlHost** élément et avant la **bouton** élément vers la fin du fichier.

    ```xml
    <TextBlock Text="Selected date:" FontSize="16" FontWeight="Bold" Grid.Row="7" Grid.Column="0" />
    <TextBlock Text="{Binding Path=Date}" FontSize="16" Grid.Row="7" Grid.Column="1" />
    ```

5. Localiser le **bouton** élément vers la fin du fichier et de modifier le **Grid.Row** propriété à partir de **7** à **8**. Cela déplace le bouton vers le bas d’une ligne dans la grille, car vous avez ajouté une nouvelle ligne.

    ```xml
    <Button Content="Save" Grid.Row="8" Grid.Column="0" Command="{Binding Path=SaveExpenseCommand}" Margin="5, 12, 0, 0" HorizontalAlignment="Left" Width="180" />
    ```

6. Ouvrez le **AddNewExpense.xaml.cs** fichier de code.

7. Ajoutez l’instruction suivante en haut du fichier.

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

8. Ajouter le Gestionnaire d’événements suivant à la `AddNewExpense` classe. Ce code implémente le **ChildChanged** événements de la **WindowsXamlHost** contrôle que vous avez déclaré précédemment dans le fichier XAML.

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

    Ce code utilise le **enfant** propriété de la **WindowsXamlHost** contrôle pour accéder à la plateforme Windows universelle **CalendarView** contrôle. Le code puis s’abonne à la **SelectedDatesChanged** événement, qui est déclenché lorsque l’utilisateur sélectionne une date dans le calendrier. Ce gestionnaire d’événements transmet la date sélectionnée au ViewModel où la nouvelle **dépenses** objet est créé et enregistré dans la base de données. Pour ce faire, le code utilise l’infrastructure de messagerie fournie par le package de MVVM Light NuGet. Le code envoie un message appelé **SelectedDateMessage** au ViewModel, qui reçoivent et définir le **Date** propriété avec la valeur sélectionnée. Dans ce scénario la **CalendarView** contrôle est configuré pour le mode de sélection unique, afin que la collection contiendra un seul élément.

    À ce stade, le projet toujours n’est pas compilé, car il manque l’implémentation pour le **SelectedDateMessage** classe. Les étapes suivantes implémentent cette classe.

9. Dans **l’Explorateur de solutions**, avec le bouton droit le **Messages** dossier et choisissez **Ajouter -> classe**. Nommez la nouvelle classe **SelectedDateMessage** et cliquez sur **ajouter**.

10. Remplacez le contenu de la **SelectedDateMessage.cs** fichier de code avec le code suivant. Ce code ajoute un à l’aide instruction pour le `GalaSoft.MvvmLight.Messaging` hérite de l’espace de noms (à partir du package de MVVM Light NuGet), le **MessageBase** classe et ajoute **DateTime** propriété qui est initialisée par le biais du constructeur public. Lorsque vous avez terminé, le fichier doit ressembler à ceci.

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

11. Ensuite, vous allez mettre à jour le ViewModel pour recevoir ce message et de remplir le **Date** propriété du ViewModel. Dans **l’Explorateur de solutions**, développez le **ViewModels** dossier et ouvrez le **AddNewExpenseViewModel.cs** fichier.

12. Recherchez le constructeur public pour le `AddNewExpenseViewModel` classe et ajoutez le code suivant à la fin du constructeur. Ce code s’inscrit pour recevoir le **SelectedDateMessage**, extraire la date sélectionnée par le biais du **SelectedDate** propriété et nous l’utiliser pour définir le **Date** propriété exposées par le ViewModel. Étant donné que cette propriété est liée à la **TextBlock** contrôle que vous avez ajouté précédemment, vous pouvez voir la date sélectionnée par l’utilisateur.

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
    > Le `SaveExpenseCommand` méthode dans le même fichier de code effectue le travail de l’enregistrement de dépenses dans la base de données. Cette méthode utilise la **Date** à créer la nouvelle propriété **dépenses** objet. Cette propriété est maintenant en cours remplie par le **CalendarView** contrôler via le `SelectedDateMessage` message que vous avez créé.

13. Appuyez sur F5 pour générer et exécuter l’application dans le débogueur. Choisissez un des employés disponibles, puis sur le **ajouter nouvelle dépense** bouton. Renseignez tous les champs sous la forme et choisissez une date à partir du nouveau **CalendarView** contrôle. Vérifiez que la date sélectionnée s’affiche sous forme de chaîne sous le contrôle.

14. Appuyez sur la **enregistrer** bouton. Le formulaire va être fermé et la nouvelle dépense est ajoutée à la fin de la liste des dépenses. La première colonne avec la date de la dépense doit être la date que vous avez sélectionné dans le **CalendarView** contrôle.

## <a name="next-steps"></a>Étapes suivantes

À ce stade du didacticiel, vous avez remplacé un contrôle WPF date et heure avec UWP **CalendarView** contrôle, qui prend en charge tactile et stylets numériques en plus de la souris et clavier. Bien que le Kit de ressources de communauté de Windows ne fournit pas une version encapsulée de la plateforme Windows universelle **CalendarView** contrôle qui peut être utilisé directement dans une application WPF, vous pouviez héberger le contrôle à l’aide du modèle générique [WindowsXamlHost ](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) contrôle.

Vous êtes maintenant prêt pour [partie 4 : Ajouter des activités des utilisateurs Windows 10 et des notifications](modernize-wpf-tutorial-4.md).
