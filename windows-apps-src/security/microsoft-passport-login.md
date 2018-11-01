---
title: Créer une application de connexion WindowsHello
description: Voici la première partie de la procédure complète sur la création d’une application UWP Windows10 qui utilise Windows Hello comme alternative aux systèmes d’authentification par nom d’utilisateur et mot de passe traditionnels.
ms.assetid: A9E11694-A7F5-4E27-95EC-889307E0C0EF
author: PatrickFarley
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, sécurité
ms.localizationpriority: medium
ms.openlocfilehash: caa0063c0aa919e094abedd96b6205e83467e0e5
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5883689"
---
# <a name="create-a-windows-hello-login-app"></a>Créer une application de connexion WindowsHello

Voici la première partie de la procédure complète sur la création d’une application UWP Windows10 qui utilise Windows Hello comme alternative aux systèmes d’authentification par nom d’utilisateur et mot de passe traditionnels. L’application utilise un nom d’utilisateur pour la connexion et crée une clé Hello pour chaque compte. Ces comptes sont protégés par le code PIN configuré dans les paramètres Windows lors de la configuration de Windows Hello.

Cette procédure pas à pas est en deux parties: création de l’application et connexion au service principal. Lorsque vous avez terminé cet article, passez à la deuxième partie: [Service de connexion Windows Hello](microsoft-passport-login-auth-service.md).

Avant de commencer, consultez la vue d’ensemble [Windows Hello](microsoft-passport.md) pour bien comprendre le fonctionnement de Windows Hello.

## <a name="get-started"></a>Prise en main


Pour créer ce projet, il vous faut connaître C# et XAML. Vous devez également être à l’aide de Visual Studio 2015 (Community Edition ou une version ultérieure), ou une version ultérieure de Visual Studio, sur un ordinateur Windows 10. Alors que Visual Studio 2015 est la version minimale requise, nous vous recommandons d’utiliser la dernière version de Visual Studio pour les dernières mises à jour de sécurité et de développeur.

-   Ouvrez Visual Studio et sélectionnez Fichier > Nouveau > projet.
-   Une fenêtre Nouveau projet s’ouvre. Navigation vers Modèles &gt; Visual C#.
-   Choisissez une application vide (Windows universelle) et appelez-la «PassportLogin».
-   Générez et exécutez la nouvelle application (F5). Une fenêtre vide doit s’afficher sur l’écran. Fermez l’application.

![Nouveau projet Windows Hello](images/passport-login-1.png)

## <a name="exercise-1-login-with-microsoft-passport"></a>Exercice1: Connexion avec Microsoft Passport


Dans cet exercice, vous découvrirez comment vérifier que Windows Hello est bien installé sur l’ordinateur et comment vous connecter à un compte à l’aide de Windows Hello.

-   Dans le nouveau projet, créez un dossier dans la solution appelé «Vues». Ce dossier contient les pages que vous consulterez dans cet exemple. Cliquez avec le bouton droit sur le projet dans l’Explorateur de solutions, sélectionnez Ajouter> Nouveau dossier, puis renommez le dossier en Vues.

    ![Ajouter un dossier WindowsHello](images/passport-login-2.png)

-   Cliquez avec le bouton droit sur le nouveau dossier Vues, sélectionnez Ajouter > Nouvel élément, puis sélectionnez Page vierge. Nommez cette page «Login.xaml».

    ![Ajouter une page vierge WindowsHello](images/passport-login-3.png)

-   Pour définir l’interface utilisateur de la nouvelle page de connexion, ajoutez le code XAML suivant. Ce code XAML définit un élément StackPanel pour aligner les enfants suivants :

    -   TextBlock qui contient un titre.
    -   TextBlock pour les messages d’erreur.
    -   TextBox pour le nom d’utilisateur à saisir.
    -   Bouton pour naviguer vers une page d’inscription.
    -   TextBlock qui contient le statut de Windows Hello.
    -   TextBlock pour expliquer la page de connexion en l’absence de serveur principal ou d’utilisateurs configurés.

    ```xml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <StackPanel Orientation="Vertical">
        <TextBlock Text="Login" FontSize="36" Margin="4" TextAlignment="Center"/>
        <TextBlock x:Name="ErrorMessage" Text="" FontSize="20" Margin="4" Foreground="Red" TextAlignment="Center"/>
        <TextBlock Text="Enter your username below" Margin="0,0,0,20"
                   TextWrapping="Wrap" Width="300"
                   TextAlignment="Center" VerticalAlignment="Center" FontSize="16"/>
        <TextBox x:Name="UsernameTextBox" Margin="4" Width="250"/>
        <Button x:Name="PassportSignInButton" Content="Login" Background="DodgerBlue" Foreground="White"
            Click="PassportSignInButton_Click" Width="80" HorizontalAlignment="Center" Margin="0,20"/>
        <TextBlock Text="Don't have an account?"
                    TextAlignment="Center" VerticalAlignment="Center" FontSize="16"/>
        <TextBlock x:Name="RegisterButtonTextBlock" Text="Register now"
                   PointerPressed="RegisterButtonTextBlock_OnPointerPressed"
                   Foreground="DodgerBlue"
                   TextAlignment="Center" VerticalAlignment="Center" FontSize="16"/>
        <Border x:Name="PassportStatus" Background="#22B14C"
                   Margin="0,20" Height="100" >
          <TextBlock x:Name="PassportStatusText" Text="Microsoft Passport is ready to use!"
                 Margin="4" TextAlignment="Center" VerticalAlignment="Center" FontSize="20"/>
        </Border>
        <TextBlock x:Name="LoginExplaination" FontSize="24" TextAlignment="Center" TextWrapping="Wrap" 
            Text="Please Note: To demonstrate a login, validation will only occur using the default username 'sampleUsername'"/>
      </StackPanel>
    </Grid>
    ```

-   Certaines méthodes doivent être ajoutées au code-behind pour obtenir la génération de la solution. Appuyez sur F7 ou utilisez l’Explorateur de solutions pour accéder à Login.xaml.cs. Ajoutez deux méthodes d’événement pour gérer les événements de connexion et d’inscription. Pour le moment, ces méthodes configurent ErrorMessage.Text sur une chaîne vide.

    ```cs
    namespace PassportLogin.Views
    {
        public sealed partial class Login : Page
        {
            public Login()
            {
                this.InitializeComponent();
            }
     
            private void PassportSignInButton_Click(object sender, RoutedEventArgs e)
            {
                ErrorMessage.Text = "";
            }
            private void RegisterButtonTextBlock_OnPointerPressed(object sender, PointerRoutedEventArgs e)
            {
                ErrorMessage.Text = "";
            }
        }
    }
    ```

-   Afin d’afficher la page de connexion, modifiez le code MainPage de manière à accéder à la page de connexion lorsque la page MainPage est chargée. Ouvrez le fichier MainPage.xaml.cs. Dans l’Explorateur de solutions, double-cliquez sur MainPage.xaml.cs. Si vous ne trouvez pas cet élément, cliquez sur la petite flèche en regard de MainPage.xaml pour afficher le code-behind. Créez une méthode de gestionnaire d’événements chargé qui accède à la page de connexion. Vous devez ajouter une référence à l’espace de noms Vues.

    ```cs
    using PassportLogin.Views;
     
    namespace PassportLogin
    {
        public sealed partial class MainPage : Page
        {
            public MainPage()
            {
                this.InitializeComponent();
                Loaded += MainPage_Loaded;
            }
     
            private void MainPage_Loaded(object sender, RoutedEventArgs e)
            {
                Frame.Navigate(typeof(Login));
            }
        }
    }
    ```

-   Sur la page de connexion, vous devez gérer l’événement OnNavigatedTo pour valider la présence de Windows Hello sur cet ordinateur. Dans le fichier Login.xaml.cs, implémentez les éléments suivants. L’objet MicrosoftPassportHelper signale une erreur, car nous ne l’avons pas encore implémenté.

    ```cs
    public sealed partial class Login : Page
    {
        public Login()
        {
            this.InitializeComponent();
        }
     
        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            // Check Microsoft Passport is setup and available on this machine
            if (await MicrosoftPassportHelper.MicrosoftPassportAvailableCheckAsync())
            {
            }
            else
            {
                // Microsoft Passport is not setup so inform the user
                PassportStatus.Background = new SolidColorBrush(Windows.UI.Color.FromArgb(255, 50, 170, 207));
                PassportStatusText.Text = "Microsoft Passport is not setup!\n" + 
                    "Please go to Windows Settings and set up a PIN to use it.";
                PassportSignInButton.IsEnabled = false;
            }
        }
    }
    ```

-   Pour créer la classe MicrosoftPassportHelper, cliquez avec le bouton droit sur la solution PassportLogin (Windows universelle), puis cliquez sur Ajouter &gt; Nouveau dossier. Nommez ce dossier Utilitaires.

    ![création d’une classe d’assistance dans Passport](images/passport-login-5.png)

-   Cliquez avec le bouton droit sur le dossier Utilitaires, puis cliquez sur Ajouter&gt; Classe. Nommez cette classe «MicrosoftPassportHelper.cs».
-   Modifiez la définition de classe de MicrosoftPassportHelper sur statique publique, puis ajoutez la méthode suivante pour indiquer à l’utilisateur si Windows Hello est prêt à être utilisé. Vous devez ajouter les espaces de noms requis.

    ```cs
    using System;
    using System.Diagnostics;
    using System.Threading.Tasks;
    using Windows.Security.Credentials;
     
    namespace PassportLogin.Utils
    {
        public static class MicrosoftPassportHelper
        {
            /// <summary>
            /// Checks to see if Passport is ready to be used.
            /// 
            /// Passport has dependencies on:
            ///     1. Having a connected Microsoft Account
            ///     2. Having a Windows PIN set up for that _account on the local machine
            /// </summary>
            public static async Task<bool> MicrosoftPassportAvailableCheckAsync()
            {
                bool keyCredentialAvailable = await KeyCredentialManager.IsSupportedAsync();
                if (keyCredentialAvailable == false)
                {
                    // Key credential is not enabled yet as user 
                    // needs to connect to a Microsoft Account and select a PIN in the connecting flow.
                    Debug.WriteLine("Microsoft Passport is not setup!\nPlease go to Windows Settings and set up a PIN to use it.");
                    return false;
                }
     
                return true;
            }
        }
    }
    ```

-   Dans Login.xaml.cs, ajoutez une référence à l’espace de noms Utilitaires. Cela permet de résoudre l’erreur dans la méthode OnNavigatedTo.

    ```cs
    using PassportLogin.Utils;
    ```

-   Créez et exécutez l’application (F5). Vous accédez à la page de connexion et la bannière Windows Hello vous indique que Hello est prêt à être utilisé. Vous devez voir une bannière bleue ou verte indiquant le statut de Windows Hello sur votre ordinateur.

    ![Page de connexion WindowsHello prête](images/passport-login-6.png)

    ![Page de connexion WindowsHello non prête](images/passport-login-7.png)

-   L’étape suivante consiste à générer la logique de connexion. Créez un dossier appelé «Modèles».
-   Dans le dossier Modèles, créez une classe appelée «Account.cs». Cette classe fera office de modèle de compte. Comme il s’agit d’un modèle, il contient uniquement un nom d’utilisateur. Modifiez la définition de classe sur publique et ajoutez la propriété Nom d’utilisateur.
    
    ```cs
    namespace PassportLogin.Models
    {
        public class Account
        {
            public string Username { get; set; }
        }
    }
    ```

-   Il vous faut une solution pour gérer les comptes. Pour ces travaux pratiques, une liste d’utilisateurs sera enregistrée et chargée localement puisqu’il n’y a ni serveur ni base de données. Cliquez avec le bouton droit sur le dossier Utilitaires, puis ajoutez une nouvelle classe appelée «AccountHelper.cs». Modifiez la définition de classe sur publique statique. AccountHelper est une classe statique qui contient toutes les méthodes nécessaires pour enregistrer et charger la liste des comptes localement. L’enregistrement et le chargement fonctionnent à l’aide de XmlSerializer. Vous devez également vous souvenir du fichier que vous avez enregistré et de l’emplacement auquel vous l’avez enregistré. Les espaces de noms supplémentaires doivent être référencés.
    
    ```cs
    using System.IO;
    using System.Xml.Serialization;
    using Windows.Storage;
    using PassportLogin.Models;

    namespace PassportLogin.Utils
    {
        public static class AccountHelper
        {
            // In the real world this would not be needed as there would be a server implemented that would host a user account database.
            // For this tutorial we will just be storing accounts locally.
            private const string USER_ACCOUNT_LIST_FILE_NAME = "accountlist.txt";
            private static string _accountListPath = Path.Combine(ApplicationData.Current.LocalFolder.Path, USER_ACCOUNT_LIST_FILE_NAME);
            public static List<Account> AccountList = new List<Account>();
     
            /// <summary>
            /// Create and save a useraccount list file. (Updating the old one)
            /// </summary>
            private static async void SaveAccountListAsync()
            {
                string accountsXml = SerializeAccountListToXml();
     
                if (File.Exists(_accountListPath))
                {
                    StorageFile accountsFile = await StorageFile.GetFileFromPathAsync(_accountListPath);
                    await FileIO.WriteTextAsync(accountsFile, accountsXml);
                }
                else
                {
                    StorageFile accountsFile = await ApplicationData.Current.LocalFolder.CreateFileAsync(USER_ACCOUNT_LIST_FILE_NAME);
                    await FileIO.WriteTextAsync(accountsFile, accountsXml);
                }
            }
     
            /// <summary>
            /// Gets the useraccount list file and deserializes it from XML to a list of useraccount objects.
            /// </summary>
            /// <returns>List of useraccount objects</returns>
            public static async Task<List<Account>> LoadAccountListAsync()
            {
                if (File.Exists(_accountListPath))
                {
                    StorageFile accountsFile = await StorageFile.GetFileFromPathAsync(_accountListPath);
     
                    string accountsXml = await FileIO.ReadTextAsync(accountsFile);
                    DeserializeXmlToAccountList(accountsXml);
                }
     
                return AccountList;
            }
     
            /// <summary>
            /// Uses the local list of accounts and returns an XML formatted string representing the list
            /// </summary>
            /// <returns>XML formatted list of accounts</returns>
            public static string SerializeAccountListToXml()
            {
                XmlSerializer xmlizer = new XmlSerializer(typeof(List<Account>));
                StringWriter writer = new StringWriter();
                xmlizer.Serialize(writer, AccountList);
     
                return writer.ToString();
            }
     
            /// <summary>
            /// Takes an XML formatted string representing a list of accounts and returns a list object of accounts
            /// </summary>
            /// <param name="listAsXml">XML formatted list of accounts</param>
            /// <returns>List object of accounts</returns>
            public static List<Account> DeserializeXmlToAccountList(string listAsXml)
            {
                XmlSerializer xmlizer = new XmlSerializer(typeof(List<Account>));
                TextReader textreader = new StreamReader(new MemoryStream(Encoding.UTF8.GetBytes(listAsXml)));
     
                return AccountList = (xmlizer.Deserialize(textreader)) as List<Account>;
            }
        }
    }
    ```

-   Ensuite, implémentez un moyen d’ajouter et de supprimer un compte de la liste locale. Ces actions enregistrent chacune la liste. La méthode finale dont vous avez besoin pour ces travaux pratiques est une méthode de validation. Dans la mesure où il n’existe aucun serveur d’authentification ni base de données des utilisateurs, cette méthode valide par rapport à un utilisateur unique codé en dur. Ces méthodes doivent être ajoutées à la classe AccountHelper.
    
    ```cs
    public static Account AddAccount(string username)
            {
                // Create a new account with the username
                Account account = new Account() { Username = username };
                // Add it to the local list of accounts
                AccountList.Add(account);
                // SaveAccountList and return the account
                SaveAccountListAsync();
                return account;
            }
     
            public static void RemoveAccount(Account account)
            {
                // Remove the account from the accounts list
                AccountList.Remove(account);
                // Re save the updated list
                SaveAccountListAsync();
            }
     
            public static bool ValidateAccountCredentials(string username)
            {
                // In the real world, this method would call the server to authenticate that the account exists and is valid.
                // For this tutorial however we will just have a existing sample user that is just "sampleUsername"
                // If the username is null or does not match "sampleUsername" it will fail validation. In which case the user should register a new passport user
     
                if (string.IsNullOrEmpty(username))
                {
                    return false;
                }
     
                if (!string.Equals(username, "sampleUsername"))
                {
                    return false;
                }
     
                return true;
            }
    ```

-   La prochaine étape consiste à gérer une demande de connexion de l’utilisateur. Dans Login.xaml.cs, créez une variable privée qui contient la connexion du compte actif. Ajoutez ensuite un nouvel appel de méthode SignInPassport. Cela permet de valider les informations d’identification de compte à l’aide de la méthode AccountHelper.ValidateAccountCredentials. Cette méthode retourne une valeur booléenne si le nom d’utilisateur entré est identique à la valeur de chaîne codée en dur définie à l’étape précédente. La valeur codée en dur de cet exemple est «sampleUsername».

    ```cs
    using PassportLogin.Models;
    using PassportLogin.Utils;
    using System.Diagnostics;
     
    namespace PassportLogin.Views
    {
        public sealed partial class Login : Page
        {
            private Account _account;
     
            public Login()
            {
                this.InitializeComponent();
            }
     
            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                // Check Microsoft Passport is setup and available on this machine
                if (await MicrosoftPassportHelper.MicrosoftPassportAvailableCheckAsync())
                {
                }
                else
                {
                    // Microsoft Passport is not setup so inform the user
                    PassportStatus.Background = new SolidColorBrush(Windows.UI.Color.FromArgb(255, 50, 170, 207));
                    PassportStatusText.Text = "Microsoft Passport is not setup!\nPlease go to Windows Settings and set up a PIN to use it.";
                    PassportSignInButton.IsEnabled = false;
                }
            }
     
            private void PassportSignInButton_Click(object sender, RoutedEventArgs e)
            {
                ErrorMessage.Text = "";
                SignInPassport();
            }
     
            private void RegisterButtonTextBlock_OnPointerPressed(object sender, PointerRoutedEventArgs e)
            {
                ErrorMessage.Text = "";
            }
     
            private async void SignInPassport()
            {
                if (AccountHelper.ValidateAccountCredentials(UsernameTextBox.Text))
                {
                    // Create and add a new local account
                    _account = AccountHelper.AddAccount(UsernameTextBox.Text);
                    Debug.WriteLine("Successfully signed in with traditional credentials and created local account instance!");
     
                    //if (await MicrosoftPassportHelper.CreatePassportKeyAsync(UsernameTextBox.Text))
                    //{
                    //    Debug.WriteLine("Successfully signed in with Microsoft Passport!");
                    //}
                }
                else
                {
                    ErrorMessage.Text = "Invalid Credentials";
                }
            }
        }
    }
    ```

-   Vous avez peut-être remarqué le code commenté qui faisait référence à une méthode dans MicrosoftPassportHelper. Dans MicrosoftPassportHelper.cs, ajoutez une nouvelle méthode appelée CreatePassportKeyAsync. Cette méthode utilise l’API WindowsHello dans la classe [**KeyCredentialManager**](https://msdn.microsoft.com/library/windows/apps/dn973043). L’appel de [**RequestCreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn973048) crée une clé Passport propre au *accountId* et à l’ordinateur local. Notez les commentaires dans l’instruction switch si vous voulez implémenter ce scénario dans le monde réel.

    ```cs
    /// <summary>
    /// Creates a Passport key on the machine using the _account id passed.
    /// </summary>
    /// <param name="accountId">The _account id associated with the _account that we are enrolling into Passport</param>
    /// <returns>Boolean representing if creating the Passport key succeeded</returns>
    public static async Task<bool> CreatePassportKeyAsync(string accountId)
    {
        KeyCredentialRetrievalResult keyCreationResult = await KeyCredentialManager.RequestCreateAsync(accountId, KeyCredentialCreationOption.ReplaceExisting);

        switch (keyCreationResult.Status)
        {
            case KeyCredentialStatus.Success:
                Debug.WriteLine("Successfully made key");

                // In the real world authentication would take place on a server.
                // So every time a user migrates or creates a new Microsoft Passport account Passport details should be pushed to the server.
                // The details that would be pushed to the server include:
                // The public key, keyAttesation if available, 
                // certificate chain for attestation endorsement key if available,  
                // status code of key attestation result: keyAttestationIncluded or 
                // keyAttestationCanBeRetrievedLater and keyAttestationRetryType
                // As this sample has no concept of a server it will be skipped for now
                // for information on how to do this refer to the second Passport sample

                //For this sample just return true
                return true;
            case KeyCredentialStatus.UserCanceled:
                Debug.WriteLine("User cancelled sign-in process.");
                break;
            case KeyCredentialStatus.NotFound:
                // User needs to setup Microsoft Passport
                Debug.WriteLine("Microsoft Passport is not setup!\nPlease go to Windows Settings and set up a PIN to use it.");
                break;
            default:
                break;
        }

        return false;
    }
    ```

-   Maintenant que vous avez créé la méthode CreatePassportKeyAsync, revenez au fichier Login.xaml.cs et supprimez les marques de commentaire du code de la méthode SignInPassport.

    ```cs
    private async void SignInPassport()
    {
        if (AccountHelper.ValidateAccountCredentials(UsernameTextBox.Text))
        {
            //Create and add a new local account
            _account = AccountHelper.AddAccount(UsernameTextBox.Text);
            Debug.WriteLine("Successfully signed in with traditional credentials and created local account instance!");

            if (await MicrosoftPassportHelper.CreatePassportKeyAsync(UsernameTextBox.Text))
            {
                Debug.WriteLine("Successfully signed in with Microsoft Passport!");
            }
        }
        else
        {
            ErrorMessage.Text = "Invalid Credentials";
        }
    }
    ```

-   Créez et exécutez l’application. Vous arriverez sur le page de connexion. Tapez «sampleUsername», puis cliquez sur Connexion. Une invite Windows Hello vous demande d’entrer votre code PIN. Une fois le code PIN correct saisi, la méthode CreatePassportKeyAsync peut créer une clé Windows Hello. Contrôlez les fenêtres de sortie pour voir si un message indiquant que l’opération a réussi s’affiche.

    ![Invite de connexion WindowsHello](images/passport-login-8.png)

## <a name="exercise-2-welcome-and-user-selection-pages"></a>Exercice2: Pages d’accueil et de sélection d’utilisateur


Cet exercice est la suite de l’exercice précédent. Lorsqu’un utilisateur réussit à se connecter, il arrive sur une page d’accueil sur laquelle il peut se déconnecter ou supprimer son compte. Puisque Windows Hello crée une clé pour chaque ordinateur, un écran de sélection utilisateur affichant tous les utilisateurs qui se sont connectés à cet ordinateur peut être créé. Un utilisateur peut ensuite sélectionner l’un de ces comptes et accéder directement à l’écran d’accueil sans devoir saisir de nouveau un mot de passe, puisqu’il a déjà été authentifié pour accéder à l’ordinateur.

-   Dans le dossier Vues, ajoutez une nouvelle page vierge appelée «Welcome.xaml». Ajoutez le code XAML suivant pour terminer l’interface utilisateur. Cette dernière affiche un titre, le nom de l’utilisateur connecté et deux boutons. L’un permet de revenir à la liste des utilisateurs (que vous créerez plus tard) et l’autre permet de gérer l’oubli de l’utilisateur.

    ```xml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <StackPanel Orientation="Vertical">
        <TextBlock x:Name="Title" Text="Welcome" FontSize="40" TextAlignment="Center"/>
        <TextBlock x:Name="UserNameText" FontSize="28" TextAlignment="Center" Foreground="Black"/>

        <Button x:Name="BackToUserListButton" Content="Back to User List" Click="Button_Restart_Click"
                HorizontalAlignment="Center" Margin="0,20" Foreground="White" Background="DodgerBlue"/>

        <Button x:Name="ForgetButton" Content="Forget Me" Click="Button_Forget_User_Click"
                Foreground="White"
                Background="Gray"
                HorizontalAlignment="Center"/>
      </StackPanel>
    </Grid>
    ```

-   Dans le fichier code-behind Welcome.xaml.cs, ajoutez une nouvelle variable privée qui contient le compte connecté. Vous devez implémenter une méthode pour remplacer l’événement OnNavigateTo et stocker le compte transmis à la page d’accueil. Vous devez également implémenter l’événement click pour les deux boutons définis dans le code XAML. Vous aurez besoin d’une référence aux dossiers Modèles et Utilitaires.

    ```cs
    using PassportLogin.Models;
    using PassportLogin.Utils;
    using System.Diagnostics;
     
    namespace PassportLogin.Views
    {
        public sealed partial class Welcome : Page
        {
            private Account _activeAccount;
     
            public Welcome()
            {
                InitializeComponent();
            }
     
            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
                _activeAccount = (Account)e.Parameter;
                if (_activeAccount != null)
                {
                    UserNameText.Text = _activeAccount.Username;
                }
            }
     
            private void Button_Restart_Click(object sender, RoutedEventArgs e)
            {
            }
     
            private void Button_Forget_User_Click(object sender, RoutedEventArgs e)
            {
                // Remove it from Microsoft Passport
                // MicrosoftPassportHelper.RemovePassportAccountAsync(_activeAccount);
     
                // Remove it from the local accounts list and resave the updated list
                AccountHelper.RemoveAccount(_activeAccount);
     
                Debug.WriteLine("User " + _activeAccount.Username + " deleted.");
            }
        }
    }
    ```

-   Vous avez peut-être remarqué une ligne commentée dans l’événement click oublier l’utilisateur. Le compte est en cours de suppression de votre liste locale, mais il n’existe pour le moment aucun moyen de le supprimer de Windows Hello. Vous devez implémenter une nouvelle méthode dans MicrosoftPassportHelper.cs pour gérer la suppression d’un utilisateur Windows Hello. Cette méthode utilise d’autres APIWindowsHello pour ouvrir et supprimer le compte. Lorsque vous supprimez un compte dans le monde réel, la base de données ou le serveur doivent être notifiés afin que la base de données utilisateur reste valide. Vous aurez besoin d’une référence au dossier Modèles.

    ```cs
    using PassportLogin.Models;

    /// <summary>
    /// Function to be called when user requests deleting their account.
    /// Checks the KeyCredentialManager to see if there is a Passport for the current user
    /// Then deletes the local key associated with the Passport.
    /// </summary>
    public static async void RemovePassportAccountAsync(Account account)
    {
        // Open the account with Passport
        KeyCredentialRetrievalResult keyOpenResult = await KeyCredentialManager.OpenAsync(account.Username);

        if (keyOpenResult.Status == KeyCredentialStatus.Success)
        {
            // In the real world you would send key information to server to unregister
            //e.g. RemovePassportAccountOnServer(account);
        }

        // Then delete the account from the machines list of Passport Accounts
        await KeyCredentialManager.DeleteAsync(account.Username);
    }
    ```

-   Dans Welcome.xaml.cs, supprimez les marques de commentaire de la ligne qui appelle RemovePassportAccountAsync.

    ```cs
    private void Button_Forget_User_Click(object sender, RoutedEventArgs e)
    {
        // Remove it from Microsoft Passport
        MicrosoftPassportHelper.RemovePassportAccountAsync(_activeAccount);
     
        // Remove it from the local accounts list and resave the updated list
        AccountHelper.RemoveAccount(_activeAccount);
     
        Debug.WriteLine("User " + _activeAccount.Username + " deleted.");
    }
    ```

-   Dans la méthode SignInPassport (de Login.xaml.cs), lorsque CreatePassportKeyAsync réussit, elle accède à l’écran d’accueil et transmet le compte.

    ```cs
    private async void SignInPassport()
    {
        if (AccountHelper.ValidateAccountCredentials(UsernameTextBox.Text))
        {
            // Create and add a new local account
            _account = AccountHelper.AddAccount(UsernameTextBox.Text);
            Debug.WriteLine("Successfully signed in with traditional credentials and created local account instance!");

            if (await MicrosoftPassportHelper.CreatePassportKeyAsync(UsernameTextBox.Text))
            {
                Debug.WriteLine("Successfully signed in with Microsoft Passport!");
                Frame.Navigate(typeof(Welcome), _account);
            }
        }
        else
        {
            ErrorMessage.Text = "Invalid Credentials";
        }
    }
    ```

-   Créez et exécutez l’application. Connectez-vous avec «sampleUsername» et cliquez sur Connexion. Entrez votre code PIN. Si tout fonctionne correctement, vous accédez à l’écran d’accueil. Essayez de cliquer sur oublier l’utilisateur et contrôlez la fenêtre Sortie pour voir si l’utilisateur a été supprimé. Lorsque l’utilisateur est supprimé, vous restez sur la page d’accueil. Vous devez créer une page de sélection d’utilisateur à laquelle l’application peut accéder.

    ![Écran d’accueil WindowsHello](images/passport-login-9.png)

-   Dans le dossierVues, créez une page vierge appelée «UserSelection.xaml» et ajoutez le codeXAML suivant pour définir l’interface utilisateur. Cette page contient un [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) qui affiche tous les utilisateurs de la liste locale des comptes et un bouton qui accède à la page de connexion pour permettre à l’utilisateur d’ajouter un autre compte.

    ```xml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <StackPanel Orientation="Vertical">
        <TextBlock x:Name="Title" Text="Select a User" FontSize="36" Margin="4" TextAlignment="Center" HorizontalAlignment="Center"/>

        <ListView x:Name="UserListView" Margin="4" MaxHeight="200" MinWidth="250" Width="250" HorizontalAlignment="Center">
          <ListView.ItemTemplate>
            <DataTemplate>
              <Grid Background="DodgerBlue" Height="50" Width="250" HorizontalAlignment="Stretch" VerticalAlignment="Stretch">
                <TextBlock Text="{Binding Username}" HorizontalAlignment="Center" TextAlignment="Center" VerticalAlignment="Center" Foreground="White"/>
              </Grid>
            </DataTemplate>
          </ListView.ItemTemplate>
        </ListView>

        <Button x:Name="AddUserButton" Content="+" FontSize="36" Width="60" Click="AddUserButton_Click" HorizontalAlignment="Center"/>
      </StackPanel>
    </Grid>
    ```

-   Dans UserSelection.xaml.cs, implémentez la méthode chargée qui accède à la page de connexion lorsque la liste locale ne contient aucun compte. Implémentez également l’événement SelectionChanged pour le contrôle ListView et un événement click pour le bouton.

    ```cs
    using System.Diagnostics;
    using PassportLogin.Models;
    using PassportLogin.Utils;

    namespace PassportLogin.Views
    {
        public sealed partial class UserSelection : Page
        {
            public UserSelection()
            {
                InitializeComponent();
                Loaded += UserSelection_Loaded;
            }

            private void UserSelection_Loaded(object sender, RoutedEventArgs e)
            {
                if (AccountHelper.AccountList.Count == 0)
                {
                    //If there are no accounts navigate to the LoginPage
                    Frame.Navigate(typeof(Login));
                }


                UserListView.ItemsSource = AccountHelper.AccountList;
                UserListView.SelectionChanged += UserSelectionChanged;
            }

            /// <summary>
            /// Function called when an account is selected in the list of accounts
            /// Navigates to the Login page and passes the chosen account
            /// </summary>
            private void UserSelectionChanged(object sender, RoutedEventArgs e)
            {
                if (((ListView)sender).SelectedValue != null)
                {
                    Account account = (Account)((ListView)sender).SelectedValue;
                    if (account != null)
                    {
                        Debug.WriteLine("Account " + account.Username + " selected!");
                    }
                    Frame.Navigate(typeof(Login), account);
                }
            }

            /// <summary>
            /// Function called when the "+" button is clicked to add a new user.
            /// Navigates to the Login page with nothing filled out
            /// </summary>
            private void AddUserButton_Click(object sender, RoutedEventArgs e)
            {
                Frame.Navigate(typeof(Login));
            }
        }
    }
    ```

<!-- -->

-   Il existe certains emplacements dans l’application à partir desquels vous voulez naviguer vers la page UserSelection. Dans le fichier MainPage.xaml.cs, vous devez accéder à la page UserSelection plutôt qu’à la page de connexion. Pendant que vous vous trouvez dans l’événement chargé sur la page MainPage, chargez la liste des comptes pour que la page UserSelection puisse vérifier s’il existe des comptes. Cela nécessite la modification de la méthode chargée afin qu’elle soit asynchrone, ainsi que l’ajout d’une référence au dossier Utilitaires.

    ```cs
    using PassportLogin.Utils;

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        // Load the local Accounts List before navigating to the UserSelection page
        await AccountHelper.LoadAccountListAsync();
        Frame.Navigate(typeof(UserSelection));
    }
    ```

-   Ensuite, vous devrez accéder à la page UserSelection à partir de la page d’accueil. Dans les deux événements click, retournez à la page UserSelection.

    ```cs
    private void Button_Restart_Click(object sender, RoutedEventArgs e)
    {
        Frame.Navigate(typeof(UserSelection));
    }

    private void Button_Forget_User_Click(object sender, RoutedEventArgs e)
    {
        // Remove it from Microsoft Passport
        MicrosoftPassportHelper.RemovePassportAccountAsync(_activeAccount);

        // Remove it from the local accounts list and resave the updated list
        AccountHelper.RemoveAccount(_activeAccount);

        Debug.WriteLine("User " + _activeAccount.Username + " deleted.");

        // Navigate back to UserSelection page.
        Frame.Navigate(typeof(UserSelection));
    }
    ```

-   Dans la page de connexion, vous avez besoin du code pour vous connecter au compte sélectionné dans la liste de la page UserSelection. Dans l’événement OnNavigatedTo, stockez le compte transmis à la navigation. Commencez par ajouter une nouvelle variable privée qui identifie si le compte est un compte existant. Gérez l’événement OnNavigatedTo.

    ```cs
    namespace PassportLogin.Views
    {
        public sealed partial class Login : Page
        {
            private Account _account;
            private bool _isExistingAccount;

            public Login()
            {
                InitializeComponent();
            }

            /// <summary>
            /// Function called when this frame is navigated to.
            /// Checks to see if Microsoft Passport is available and if an account was passed in.
            /// If an account was passed in set the "_isExistingAccount" flag to true and set the _account
            /// </summary>
            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                // Check Microsoft Passport is setup and available on this machine
                if (await MicrosoftPassportHelper.MicrosoftPassportAvailableCheckAsync())
                {
                    if (e.Parameter != null)
                    {
                        _isExistingAccount = true;
                        // Set the account to the existing account being passed in
                        _account = (Account)e.Parameter;
                        UsernameTextBox.Text = _account.Username;
                        SignInPassport();
                    }
                }
                else
                {
                    // Microsoft Passport is not setup so inform the user
                    PassportStatus.Background = new SolidColorBrush(Windows.UI.Color.FromArgb(255, 50, 170, 207));
                    PassportStatusText.Text = "Microsoft Passport is not setup!\n" + 
                        "Please go to Windows Settings and set up a PIN to use it.";
                    PassportSignInButton.IsEnabled = false;
                }
            }
        }
    }
    ```

-   La méthode SignInPassport devra être mise à jour pour la connexion au compte sélectionné. MicrosoftPassportHelper nécessite une autre méthode pour ouvrir le compte avec Passport, car le compte dispose déjà d’une clé Passport. Implémentez la nouvelle méthode dans MicrosoftPassportHelper.cs pour vous connecter à un utilisateur existant avec Passport. Pour plus d’informations sur chaque partie du code, lisez les commentaires de code.

    ```cs
    /// <summary>
    /// Attempts to sign a message using the Passport key on the system for the accountId passed.
    /// </summary>
    /// <returns>Boolean representing if creating the Passport authentication message succeeded</returns>
    public static async Task<bool> GetPassportAuthenticationMessageAsync(Account account)
    {
        KeyCredentialRetrievalResult openKeyResult = await KeyCredentialManager.OpenAsync(account.Username);
        // Calling OpenAsync will allow the user access to what is available in the app and will not require user credentials again.
        // If you wanted to force the user to sign in again you can use the following:
        // var consentResult = await Windows.Security.Credentials.UI.UserConsentVerifier.RequestVerificationAsync(account.Username);
        // This will ask for the either the password of the currently signed in Microsoft Account or the PIN used for Microsoft Passport.

        if (openKeyResult.Status == KeyCredentialStatus.Success)
        {
            // If OpenAsync has succeeded, the next thing to think about is whether the client application requires access to backend services.
            // If it does here you would Request a challenge from the Server. The client would sign this challenge and the server
            // would check the signed challenge. If it is correct it would allow the user access to the backend.
            // You would likely make a new method called RequestSignAsync to handle all this
            // e.g. RequestSignAsync(openKeyResult);
            // Refer to the second Microsoft Passport sample for information on how to do this.

            // For this sample there is not concept of a server implemented so just return true.
            return true;
        }
        else if (openKeyResult.Status == KeyCredentialStatus.NotFound)
        {
            // If the _account is not found at this stage. It could be one of two errors. 
            // 1. Microsoft Passport has been disabled
            // 2. Microsoft Passport has been disabled and re-enabled cause the Microsoft Passport Key to change.
            // Calling CreatePassportKey and passing through the account will attempt to replace the existing Microsoft Passport Key for that account.
            // If the error really is that Microsoft Passport is disabled then the CreatePassportKey method will output that error.
            if (await CreatePassportKeyAsync(account.Username))
            {
                // If the Passport Key was again successfully created, Microsoft Passport has just been reset.
                // Now that the Passport Key has been reset for the _account retry sign in.
                return await GetPassportAuthenticationMessageAsync(account);
            }
        }

        // Can't use Passport right now, try again later
        return false;
    }
    ```

-   Mettez à jour la méthode SignInPassport dans Login.xaml.cs pour gérer le compte existant. La nouvelle méthode dans MicrosoftPassportHelper.cs. est utilisée. Si cela fonctionne, le compte est connecté et l’utilisateur accède à l’écran d’accueil.

    ```cs
    private async void SignInPassport()
    {
        if (_isExistingAccount)
        {
            if (await MicrosoftPassportHelper.GetPassportAuthenticationMessageAsync(_account))
            {
                Frame.Navigate(typeof(Welcome), _account);
            }
        }
        else if (AccountHelper.ValidateAccountCredentials(UsernameTextBox.Text))
        {
            //Create and add a new local account
            _account = AccountHelper.AddAccount(UsernameTextBox.Text);
            Debug.WriteLine("Successfully signed in with traditional credentials and created local account instance!");

            if (await MicrosoftPassportHelper.CreatePassportKeyAsync(UsernameTextBox.Text))
            {
                Debug.WriteLine("Successfully signed in with Microsoft Passport!");
                Frame.Navigate(typeof(Welcome), _account);
            }
        }
        else
        {
            ErrorMessage.Text = "Invalid Credentials";
        }
    }
    ```

-   Créez et exécutez l’application. Connectez-vous avec «sampleUsername». Entrez votre code PIN. Si tout fonctionne correctement, vous accédez à l’écran d’accueil. Cliquez pour revenir à la liste des utilisateurs. Vous devez maintenant voir un utilisateur dans la liste. Si vous cliquez sur cet utilisateur, Passport vous permet de vous reconnecter sans avoir à entrer à nouveau le mot de passe.

    ![Liste de sélection des utilisateurs Windows Hello](images/passport-login-10.png)

## <a name="exercise-3-registering-a-new-windows-hello-user"></a>Exercice 3: Inscription d’un nouvel utilisateur WindowsHello


Dans cet exercice, vous créerez une page pour créer un compte avec Windows Hello. Son fonctionnement sera identique à celui de la page de connexion. La page de connexion est implémentée pour un utilisateur existant qui migre pour utiliser Windows Hello. Une page PassportRegister crée l’inscription auprès de Windows Hello pour un nouvel utilisateur.

-   Dans le dossier Vues, créez une page vierge appelée «PassportRegister.xaml». Dans le code XAML, ajoutez le code ci-dessous pour configurer l’interface utilisateur. L’interface est similaire à la page de connexion.

    ```xml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <StackPanel Orientation="Vertical">
        <TextBlock x:Name="Title" Text="Register New Passport User" FontSize="24" Margin="4" TextAlignment="Center"/>

        <TextBlock x:Name="ErrorMessage" Text="" FontSize="20" Margin="4" Foreground="Red" TextAlignment="Center"/>

        <TextBlock Text="Enter your new username below" Margin="0,0,0,20"
                   TextWrapping="Wrap" Width="300"
                   TextAlignment="Center" VerticalAlignment="Center" FontSize="16"/>

        <TextBox x:Name="UsernameTextBox" Margin="4" Width="250"/>

        <Button x:Name="PassportRegisterButton" Content="Register" Background="DodgerBlue" Foreground="White"
            Click="RegisterButton_Click_Async" Width="80" HorizontalAlignment="Center" Margin="0,20"/>

        <Border x:Name="PassportStatus" Background="#22B14C"
                   Margin="4" Height="100">
          <TextBlock x:Name="PassportStatusText" Text="Microsoft Passport is ready to use!" FontSize="20"
                 Margin="4" TextAlignment="Center" VerticalAlignment="Center"/>
        </Border>
      </StackPanel>
    </Grid>
    ```

-   Dans le fichier code-behind PassportRegister.xaml.cs, implémentez une variable de compte privée et un événement click pour le bouton S’inscrire. Cela ajoute un nouveau compte local et crée une clé Passport.

    ```cs
    using PassportLogin.Models;
    using PassportLogin.Utils;

    namespace PassportLogin.Views
    {
        public sealed partial class PassportRegister : Page
        {
            private Account _account;

            public PassportRegister()
            {
                InitializeComponent();
            }

            private async void RegisterButton_Click_Async(object sender, RoutedEventArgs e)
            {
                ErrorMessage.Text = "";

                //In the real world you would normally validate the entered credentials and information before 
                //allowing a user to register a new account. 
                //For this sample though we will skip that step and just register an account if username is not null.

                if (!string.IsNullOrEmpty(UsernameTextBox.Text))
                {
                    //Register a new account
                    _account = AccountHelper.AddAccount(UsernameTextBox.Text);
                    //Register new account with Microsoft Passport
                    await MicrosoftPassportHelper.CreatePassportKeyAsync(_account.Username);
                    //Navigate to the Welcome Screen. 
                    Frame.Navigate(typeof(Welcome), _account);
                }
                else
                {
                    ErrorMessage.Text = "Please enter a username";
                }
            }
        }
    }
    ```

-   Vous devez accéder à cette page à partir de la page de connexion lorsque vous cliquez sur S’inscrire.

    ```cs
    private void RegisterButtonTextBlock_OnPointerPressed(object sender, PointerRoutedEventArgs e)
    {
        ErrorMessage.Text = "";
        Frame.Navigate(typeof(PassportRegister));
    }
    ```

-   Créez et exécutez l’application. Essayez d’inscrire un nouvel utilisateur. Revenez à la liste des utilisateurs et vérifiez que vous pouvez sélectionner cet utilisateur et vous connecter.

    ![Inscription d’un nouvel utilisateur WindowsHello](images/passport-login-11.png)

Dans cet exercice pratique, vous avez appris les bases pour utiliser la nouvelle API Windows Hello permettant d’authentifier les utilisateurs existants et de créer des comptes pour les nouveaux utilisateurs. Avec ces nouvelles connaissances, les utilisateurs n’ont plus besoin de mémoriser un mot de passe, mais n’ayez crainte, votre application reste protégée grâce à l’authentification utilisateur. Windows10 utilise la nouvelle technologie d’authentification Windows Hello pour prendre en charge ses options de connexion biométrique.

## <a name="related-topics"></a>Rubriques connexes

* [Windows Hello](microsoft-passport.md)
* [Service de connexion WindowsHello](microsoft-passport-login-auth-service.md)
