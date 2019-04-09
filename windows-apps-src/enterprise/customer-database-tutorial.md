---
title: Créer une application de base de données de clients
description: Créer une application de base de données client et découvrez comment implémenter des fonctions d’application de base d’entreprise.
keywords: entreprise, didacticiel, données clients, créer la lecture de mise à jour, suppression, REST, l’authentification
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: med
ms.openlocfilehash: 7bd3a180762c3ef06d7c24ae001fb2c7fb7fc55e
ms.sourcegitcommit: 6df46d7d5b5522805eab11a9c0e07754f28673c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58808297"
---
# <a name="tutorial-create-a-customer-database-application"></a>Tutoriel : Créer une application de base de données de clients

Ce didacticiel crée une application simple pour la gestion d’une liste de clients. Ce faisant, il introduit une sélection des concepts de base pour les applications d’entreprise dans UWP. Vous allez apprendre à effectuer les opérations suivantes :

* Mettre en œuvre de création, lecture, mise à jour et supprimer des opérations sur une base de données SQL locale.
* Ajouter une grille de données, pour afficher et modifier des données client dans votre interface utilisateur.
* Organiser les éléments d’interface utilisateur dans une disposition de formulaire de base.

Le point de départ pour ce didacticiel est une application à page unique avec minimale de l’interface utilisateur et de fonctionnalités, en fonction de la version simplifiée de la [exemple d’application de base de données des commandes client](https://github.com/Microsoft/Windows-appsample-customers-orders-database). Il est écrit C# et XAML et nous attendons que vous avez une connaissance de base de ces deux langages.

![La page principale de l’application de travail](images/customer-database-tutorial/customer-list.png)

### <a name="prerequisites"></a>Prérequis

* [Assurez-vous de qu'avoir la dernière version de Visual Studio et le SDK Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* [Cloner ou télécharger l’exemple de didacticiel de base de données client](https://aka.ms/customer-database-tutorial)

Une fois que vous avez cloné/téléchargé le référentiel, vous pouvez modifier le projet en ouvrant **CustomerDatabaseTutorial.sln** avec Visual Studio.

> [!NOTE]
> Découvrez le [exemple de base de données des commandes client complet](https://github.com/Microsoft/Windows-appsample-customers-orders-database) pour voir l’application de ce didacticiel est basé sur.

## <a name="part-1-code-of-interest"></a>Partie 1 : Code d’intérêt

Si vous exécutez votre application immédiatement après son ouverture, vous verrez quelques boutons en haut d’un écran vide. S’il n’est pas visible par vous, l’application comprend déjà une base de données SQLite local doté de quelques clients de test. À ce stade, vous commencez par implémenter un contrôle d’interface utilisateur pour afficher les clients et ensuite passer à l’ajout d’opérations par rapport à la base de données. Avant de commencer, voici où vous allez travailler.

### <a name="views"></a>Affichages

**CustomerListPage.xaml** est la vue de l’application, qui définit l’interface utilisateur de la page unique dans ce didacticiel. Chaque fois que vous avez besoin pour ajouter ou modifier un élément visuel dans l’interface utilisateur, vous effectuerez cela dans ce fichier. Ce didacticiel vous guidera d’ajout de ces éléments :

* Un **RadDataGrid** pour afficher et modifier vos clients. 
* Un **StackPanel** pour définir les valeurs initiales pour un nouveau client.

### <a name="viewmodels"></a>ViewModels

**ViewModels\CustomerListPageViewModel.cs** est où se trouve la logique fondamentale de l’application. Chacune des actions utilisateur effectuées dans la vue sont passées dans ce fichier pour le traitement. Dans ce didacticiel, vous allez ajouter un nouveau code et implémenter les méthodes suivantes :

* **CreateNewCustomerAsync**, ce qui initialise un nouvel objet CustomerViewModel.
* **DeleteNewCustomerAsync**, qui supprime un nouveau client avant qu’il est affiché dans l’interface utilisateur.
* **DeleteAndUpdateAsync**, qui gère la logique du bouton de suppression.
* **GetCustomerListAsync**, qui Récupère une liste de clients à partir de la base de données.
* **SaveInitialChangesAsync**, qui ajoute les informations d’un nouveau client à la base de données.
* **UpdateCustomersAsync**, ce qui signifie que l’interface utilisateur afin de refléter tous les clients ajoutés ou supprimés.

**CustomerViewModel** est un wrapper pour les informations d’un client, qui effectue le suivi qu’il a été récemment modifié ou non. Vous n’aurez rien à ajouter à cette classe, mais la partie du code, vous allez ajouter un autre emplacement est référencé par.

Pour plus d’informations sur la construction de l’exemple, consultez le [vue d’ensemble de la structure application](../enterprise/customer-database-app-structure.md).

## <a name="part-2-add-the-datagrid"></a>Partie 2 : Ajouter le contrôle DataGrid

Avant de commencer à fonctionner sur les données client, vous devez ajouter un contrôle d’interface utilisateur pour afficher les clients. Pour ce faire, nous allons utiliser un tiers prédéfini **RadDataGrid** contrôle. Le **Telerik.UI.for.UniversalWindowsPlatform** package NuGet a déjà été inclus dans ce projet. Nous allons ajouter la grille à notre projet.

1. Ouvrez **Views\CustomerListPage.xaml** à partir de l’Explorateur de solutions. Ajoutez la ligne suivante du code au sein du **Page** pour déclarer un mappage à l’espace de noms Telerik contenant la grille de données.

    ```xaml
        xmlns:telerikGrid="using:Telerik.UI.Xaml.Controls.Grid"
    ```

2. Ci-dessous le **CommandBar** au sein de la main **RelativePanel** de la vue, ajoutez un **RadDataGrid** contrôle, avec certaines options de configuration de base :

    ```xaml
    <Grid
        x:Name="CustomerListRoot"
        Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <RelativePanel>
            <CommandBar
                x:Name="mainCommandBar"
                HorizontalAlignment="Stretch"
                Background="AliceBlue">
                <!--CommandBar content-->
            </CommandBar>
            <telerikGrid:RadDataGrid
                x:Name="DataGrid"
                BorderThickness="0"
                ColumnDataOperationsMode="Flyout"
                GridLinesVisibility="None"
                GroupPanelPosition="Left"
                RelativePanel.AlignLeftWithPanel="True"
                RelativePanel.AlignRightWithPanel="True"
                RelativePanel.Below="mainCommandBar" />
        </RelativePanel>
    </Grid>
    ```

3. Vous avez ajouté à la grille de données, mais il a besoin de données à afficher. Ajoutez les lignes de code suivantes à celui-ci :

    ```xaml
    ItemsSource="{x:Bind ViewModel.Customers}"
    UserEditMode="Inline"
    ```
    Maintenant que vous avez défini une source de données à afficher, **RadDataGrid** gèrent la majeure partie de la logique de l’interface utilisateur pour vous. Toutefois, si vous exécutez votre projet, vous toujours ne voient aucune donnée sur l’affichage. C’est parce que le ViewModel n’est pas le chargement encore.

![Application vide, avec aucun client](images/customer-database-tutorial/blank-customer-list.png)

## <a name="part-3-read-customers"></a>Partie 3 : Clients en lecture

Lorsqu’il est initialisé, **ViewModels\CustomerListPageViewModel.cs** appelle le **GetCustomerListAsync** (méthode). Que les besoins de méthode pour récupérer le données client de test à partir de la SQLite de base de données qui est inclus dans le didacticiel.

1. Dans **ViewModels\CustomerListPageViewModel.cs**, mettre à jour votre **GetCustomerListAsync** méthode avec ce code :

    ```csharp
    public async Task GetCustomerListAsync()
    {
        var customers = await App.Repository.Customers.GetAsync(); 
        if (customers == null)
        {
            return;
        }
        await DispatcherHelper.ExecuteOnUIThreadAsync(() =>
        {
            Customers.Clear();
            foreach (var c in customers)
            {
                Customers.Add(new CustomerViewModel(c));
            }
        });
    }
    ```
    Le **GetCustomerListAsync** méthode est appelée lorsque le ViewModel est chargé, mais avant cette étape, rien ne. Ici, nous avons ajouté un appel à la **GetAsync** méthode dans **référentiel/SqlCustomerRepository**. Cela lui permet de contacter le référentiel pour récupérer une collection énumérable d’objets Customer. Il analyse ensuite les objets distincts, avant de les ajouter à sa liste interne **ObservableCollection** afin de pouvoir être affichées et modifiées.

2. Exécuter votre application - vous voyez désormais la grille de données affichant la liste des clients.

![Liste initiale des clients](images/customer-database-tutorial/initial-customers.png)

## <a name="part-4-edit-customers"></a>Partie 4 : Modifier les clients

Vous pouvez modifier les entrées dans la grille de données en double-cliquant dessus, mais vous devez vous assurer que toutes les modifications apportées dans l’interface utilisateur sont également apportées à votre collection de clients dans le code-behind. Cela signifie que vous devez implémenter la liaison de données bidirectionnelle. Si vous souhaitez plus d’informations, consultez notre [introduction à la liaison de données](../get-started/display-customers-in-list-learning-track.md).

1. Tout d’abord, déclarez que **ViewModels\CustomerListPageViewModel.cs** implémente le **INotifyPropertyChanged** interface :

    ```csharp
    public class CustomerListPageViewModel : INotifyPropertyChanged
    ```

2. Puis, dans le corps principal de la classe, ajoutez les événements et les méthodes suivantes :

    ```csharp
    public event PropertyChangedEventHandler PropertyChanged;

    public void OnPropertyChanged([CallerMemberName] string propertyName = null) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    ```

    Le **OnPropertyChanged** méthode facilite vos méthodes setter déclencher le **PropertyChanged** événement, qui est nécessaire pour la liaison de données bidirectionnelle.

3. Mettre à jour de la méthode setter pour **SelectedCustomer** avec cet appel de fonction :

    ```csharp
    public CustomerViewModel SelectedCustomer
    {
        get => _selectedCustomer;
        set
        {
            if (_selectedCustomer != value)
            {
                _selectedCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

4. Dans **Views\CustomerListPage.xaml**, ajoutez le **SelectedCustomer** propriété votre grille de données.

    ```xaml
    SelectedItem="{x:Bind ViewModel.SelectedCustomer, Mode=TwoWay}"
    ```

    Cela associe la sélection d’utilisateur dans la grille de données à l’objet client correspondant dans le code-behind. Le mode de liaison TwoWay (bidirectionnel) autorise les modifications apportées dans l’interface utilisateur se reflète sur cet objet.

5. Exécutez votre application. Vous pouvez désormais afficher les clients affichées dans la grille et apporter des modifications aux données sous-jacentes via votre interface utilisateur.

![Modification d’un client dans la grille de données](images/customer-database-tutorial/edit-customer-inline.png)

## <a name="part-5-update-customers"></a>Partie 5 : Mettre à jour des clients

Maintenant que vous pouvez voir et modifier vos clients, vous devez être en mesure d’envoyer vos modifications à la base de données et pour extraire les mises à jour qui ont été apportées par d’autres utilisateurs.

1. Retour à **ViewModels\CustomerListPageViewModel.cs**et accédez à la **UpdateCustomersAsync** (méthode). Mettre à jour avec ce code, pour transférer les modifications à la base de données et à récupérer toutes les nouvelles informations :

    ```csharp
    public async Task UpdateCustomersAsync()
    {
        foreach (var modifiedCustomer in Customers
            .Where(x => x.IsModified).Select(x => x.Model))
        {
            await App.Repository.Customers.UpsertAsync(modifiedCustomer);
        }
        await GetCustomerListAsync();
    }
    ```
    Ce code utilise le **IsModified** propriété du **ViewModels\CustomerViewModel.cs**, qui est automatiquement mis à jour chaque fois que le client est modifié. Cela vous permet d’éviter les appels inutiles et pour transférer uniquement les modifications à partir de clients mis à jour à la base de données.

## <a name="part-6-create-a-new-customer"></a>Partie 6 : Créer un nouveau client

Ajout d’un nouveau client un défi, car le client s’affiche comme une ligne vide si vous ajoutez à l’interface utilisateur avant de fournir des valeurs pour ses propriétés. Ce n’est pas un problème, mais ici nous en faisons plus facile de définir des valeurs initiales d’un client. Dans ce didacticiel, nous allons ajouter un panneau réductible simple, mais si vous aviez plus d’informations à ajouter, vous pouvez créer une page distincte à cet effet.

### <a name="update-the-code-behind"></a>Mettre à jour le code-behind

1. Ajouter un nouveau champ privé et une propriété publique à **ViewModels\CustomerListPageViewModel.cs**. Cela sera utilisé pour contrôler si le panneau est visible.

    ```csharp
    private bool _addingNewCustomer = false;

    public bool AddingNewCustomer
    {
        get => _addingNewCustomer;
        set
        {
            if (_addingNewCustomer != value)
            {
                _addingNewCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

2. Ajoutez une nouvelle propriété publique dans le ViewModel, inverse de la valeur de **AddingNewCustomer**. Cela permettra de désactiver les boutons de barre de commande standard lorsque le panneau est visible.

    ```csharp
    public bool EnableCommandBar => !AddingNewCustomer;
    ```
    Vous devez désormais un moyen pour afficher le Panneau réductible et de créer un client à modifier qu’il contient. 

3. Ajoutez un nouveau fiend privé et une propriété publique dans le ViewModel, pour contenir le client nouvellement créé.

    ```csharp
    private CustomerViewModel _newCustomer;

    public CustomerViewModel NewCustomer
    {
        get => _newCustomer;
        set
        {
            if (_newCustomer != value)
            {
                _newCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

2.  Mise à jour votre **CreateNewCustomerAsync** méthode pour créer un nouveau client, ajoutez-le au référentiel et définissez-le comme le client sélectionné :

    ```csharp
    public async Task CreateNewCustomerAsync()
    {
        CustomerViewModel newCustomer = new CustomerViewModel(new Models.Customer());
        NewCustomer = newCustomer;
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        AddingNewCustomer = true;
    }
    ```

3. Mettre à jour le **SaveInitialChangesAsync** méthode pour ajouter un client qui vient d’être créé dans le référentiel, mettre à jour de l’interface utilisateur et fermez le panneau.

    ```csharp
    public async Task SaveInitialChangesAsync()
    {
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        await UpdateCustomersAsync();
        AddingNewCustomer = false;
    }
    ```
4. Ajoutez la ligne suivante de code en tant que la dernière ligne dans l’accesseur Set pour **AddingNewCustomer**:

    ```csharp
    OnPropertyChanged(nameof(EnableCommandBar));
    ```

    Cela garantit que **EnableCommandBar** est automatiquement mis à jour chaque fois que **AddingNewCustomer** est modifiée.

### <a name="update-the-ui"></a>Mettre à jour de l’interface utilisateur

1. Revenez à **Views\CustomerListPage.xaml**et ajoutez un **StackPanel** avec les propriétés suivantes entre votre **CommandBar** et votre grille de données :

    ```xaml
    <StackPanel
        x:Name="newCustomerPanel"
        Orientation="Horizontal"
        x:Load="{x:Bind ViewModel.AddingNewCustomer, Mode=OneWay}"
        RelativePanel.Below="mainCommandBar">
    </StackPanel>
    ```
    Le **x : Load** attribut veille à ce que ce volet s’affiche uniquement lorsque vous ajoutez un nouveau client.

2. Apportez la modification suivante à la position de votre grille de données, pour vous assurer qu’il déplace vers le bas lorsque le nouveau panneau s’affiche :

    ```xaml
    RelativePanel.Below="newCustomerPanel"
    ```

3. Mettre à jour votre panneau d’empilement avec quatre **zone de texte** contrôles. Ils lier les propriétés individuelles du nouveau client et vous permettent de modifier ses valeurs avant de l’ajouter à la grille de données.

    ```xaml
    <StackPanel
        x:Name="newCustomerPanel"
        Orientation="Horizontal"
        x:Load="{x:Bind ViewModel.AddingNewCustomer, Mode=OneWay}"
        RelativePanel.Below="mainCommandBar">
        <TextBox
            Header="First name"
            PlaceholderText="First"
            Margin="8,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.FirstName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Last name"
            PlaceholderText="Last"
            Margin="0,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.LastName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Address"
            PlaceholderText="1234 Address St, Redmond WA 00000"
            Margin="0,8,16,8"
            MinWidth="280"
            Text="{x:Bind ViewModel.NewCustomer.Address, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Company"
            PlaceholderText="Company"
            Margin="0,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.Company, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
    </StackPanel>
    ```

4. Ajouter un bouton simple à votre nouveau panneau d’empilement à enregistrer le client nouvellement créé :

    ```xaml
    <StackPanel>
        <!--Text boxes from step 3-->
        <AppBarButton
            x:Name="SaveNewCustomer"
            Click="{x:Bind ViewModel.SaveInitialChangesAsync}"
            Icon="Save"/>
    </StackPanel>
    ```

5. Mettre à jour le **CommandBar**, de sorte que le régulière create, delete et les boutons de mise à jour sont désactivées lorsque le panneau d’empilement est visible :

    ```xaml
    <CommandBar
        x:Name="mainCommandBar"
        HorizontalAlignment="Stretch"
        IsEnabled="{x:Bind ViewModel.EnableCommandBar, Mode=OneWay}"
        Background="AliceBlue">
        <!--App bar buttons-->
    </CommandBar>
    ```

6. Exécutez votre application. Vous pouvez maintenant créer un client et ses données dans le panneau d’empilement d’entrée.

![Création d’un client](images/customer-database-tutorial/add-new-customer.png)

## <a name="part-7-delete-a-customer"></a>Partie 7 : Supprimer un client

Suppression d’un client est la dernière opération de base dont vous avez besoin d’implémenter. Lorsque vous supprimez un client que vous avez sélectionné dans la grille de données, vous voudrez immédiatement appeler **UpdateCustomersAsync** pour mettre à jour de l’interface utilisateur. Toutefois, vous n’avez pas besoin d’appeler cette méthode si vous supprimez un client que vous venez de créer.

1. Accédez à **ViewModels\CustomerListPageViewModel.cs**et mettre à jour le **DeleteAndUpdateAsync** méthode :

    ```csharp
    public async void DeleteAndUpdateAsync()
    {
        if (SelectedCustomer != null)
        {
            await App.Repository.Customers.DeleteAsync(_selectedCustomer.Model.Id);
        }
        await UpdateCustomersAsync();
    }
    ```

2. Dans **Views\CustomerListPage.xaml**, mettre à jour du panneau d’empilement pour l’ajout d’un nouveau client afin qu’il contient un deuxième bouton :

    ```xaml
    <StackPanel>
        <!--Text boxes for adding a new customer-->
        <AppBarButton
            x:Name="DeleteNewCustomer"
            Click="{x:Bind ViewModel.DeleteNewCustomerAsync}"
            Icon="Cancel"/>
        <AppBarButton
            x:Name="SaveNewCustomer"
            Click="{x:Bind ViewModel.SaveInitialChangesAsync}"
            Icon="Save"/>
    </StackPanel>
    ```

3. Dans **ViewModels\CustomerListPageViewModel.cs**, mettre à jour le **DeleteNewCustomerAsync** méthode pour supprimer le nouveau client :

    ```csharp
    public async Task DeleteNewCustomerAsync()
    {
        if (NewCustomer != null)
        {
            await App.Repository.Customers.DeleteAsync(_newCustomer.Model.Id);
            AddingNewCustomer = false;
        }
    }
    ```

4. Exécutez votre application. Vous pouvez maintenant supprimer des clients, dans la grille de données ou dans le panneau d’empilement.

![Supprimer un nouveau client](images/customer-database-tutorial/delete-new-customer.png)

## <a name="conclusion"></a>Conclusion

Félicitations ! Toutes les fois cette opération effectuée, votre application possède maintenant un large éventail d’opérations de base de données locale. Vous pouvez créer, lire, mettre à jour et supprimer des clients au sein de votre interface utilisateur, et ces modifications sont enregistrées dans votre base de données et sont conservées lors des lancements différents de votre application.

Maintenant que vous avez terminé, considérez les points suivants :
* Si vous n’avez pas déjà, consultez le [vue d’ensemble de la structure application](../enterprise/customer-database-app-structure.md) pour plus d’informations sur la raison pour laquelle l’application est générée de façon dont il est.
* Explorez le [exemple de base de données des commandes client complet](https://github.com/Microsoft/Windows-appsample-customers-orders-database) pour voir l’application de ce didacticiel est basé sur.

Ou, si vous êtes prêt à un défi, vous pouvez continuer et versions ultérieures...

## <a name="going-further-connect-to-a-remote-database"></a>Pour aller plus loin : Se connecter à une base de données distante

Nous vous avons fourni une procédure pas à pas montrant comment implémenter ces appels par rapport à une base de données SQLite locale. Mais que se passe-t-il si vous souhaitez utiliser une base de données à distance, à la place ?

Si vous souhaitez à présent un essai, vous aurez besoin de votre propre compte Azure Active Directory (AAD) et la capacité d’héberger votre propre source de données.

Vous devez ajouter l’authentification, des fonctions pour gérer les appels REST, puis créer une base de données distante pour interagir avec. Voici le code dans la version complète [exemple de base de données des commandes client](https://github.com/Microsoft/Windows-appsample-customers-orders-database) que vous pouvez référencer pour chaque opération nécessaire.

### <a name="settings-and-configuration"></a>Paramètres et la configuration

Les étapes nécessaires pour se connecter à votre propre base de données distante sont précisés dans le [fichier Lisez-moi de l’exemple](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/README.md). Vous devez effectuer les opérations suivantes :

* Fournir votre Id de client de compte Azure à [Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs).
* Indiquez l’url de la base de données à distance [Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs).
* Fournir la chaîne de connexion de la base de données [Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs).
* Associer votre application avec le Microsoft Store.
* Recopiez les [projet de Service](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoService) dans votre application et le déployer dans Azure.

### <a name="authentication"></a>Authentification

Vous aurez besoin créer un bouton pour démarrer une séquence d’authentification et une fenêtre contextuelle ou une page distincte pour collecter des informations d’un utilisateur. Une fois que vous avez créé, vous devez fournir du code qui demande des informations de l’utilisateur et l’utilise pour acquérir un jeton d’accès. L’exemple de base de données des commandes client encapsule les appels de Microsoft Graph avec le **Gestionnaire de compte Web** bibliothèque pour acquérir un jeton et de gérer l’authentification à un compte AAD.

* La logique d’authentification est implémentée dans [ **AuthenticationViewModel.cs**](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/ViewModels/AuthenticationViewModel.cs).
* Le processus d’authentification s’affiche dans personnalisé [ **AuthenticationControl.xaml** ](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/UserControls/AuthenticationControl.xaml) contrôle.

### <a name="rest-calls"></a>Appels REST

Vous n’aurez pas à les modifier du code nous avons ajouté dans ce didacticiel afin d’implémenter les appels REST. Au lieu de cela, vous devez effectuer les opérations suivantes :

* Créer de nouvelles implémentations de la **ICustomerRepository** et **ITutorialRepository** interfaces, implémentation le même ensemble de fonctions via REST au lieu de SQLite. Vous aurez besoin pour sérialiser et désérialiser JSON et pouvez encapsuler vos appels REST dans une fonction **HttpHelper** classe si vous avez besoin. Reportez-vous à [l’exemple complet](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoRepository/Rest) pour plus de détails.
* Dans **App.xaml.cs**, créez une fonction pour initialiser le référentiel REST et appeler à la place de **SqliteDatabase** lorsque l’application est initialisée. Là encore, reportez-vous à [l’exemple complet](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/App.xaml.cs).

Une fois les trois de ces étapes terminées, vous devez être en mesure d’authentifier votre compte AAD via votre application. Appels REST vers la base de données distante remplacera les appels de SQLite locales, mais l’expérience utilisateur doit être identiques. Si vous vous sentez encore plus ambitieux jusqu'à présent, vous pouvez ajouter une page de paramètres pour autoriser l’utilisateur de basculer dynamiquement entre les deux.
