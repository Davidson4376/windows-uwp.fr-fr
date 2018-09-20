---
title: Gestionnaire de comptes web
description: Cet article explique comment utiliser la classe AccountsSettingsPane pour connecter votre application de plateforme Windows universelle (UWP) à des fournisseurs d’identité externes, tels que Microsoft ou Facebook, à l’aide des API du Gestionnaire de comptes web de Windows10.
author: PatrickFarley
ms.author: pafarley
ms.date: 12/6/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, sécurité
ms.assetid: ec9293a1-237d-47b4-bcde-18112586241a
ms.localizationpriority: medium
ms.openlocfilehash: 2de5c969610aa6b4fa1a3af01af565d35854b5f2
ms.sourcegitcommit: 4f6dc806229a8226894c55ceb6d6eab391ec8ab6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2018
ms.locfileid: "4087650"
---
# <a name="web-account-manager"></a>Gestionnaire de comptes web

Cet article explique comment utiliser la classe **[AccountsSettingsPane](https://docs.microsoft.com/uwp/api/Windows.UI.ApplicationSettings.AccountsSettingsPane)** pour connecter votre application de plateforme Windows universelle (UWP) à des fournisseurs d’identité externes, tels que Microsoft ou Facebook, à l’aide des API du Gestionnaire de comptes web de Windows10. Vous découvrirez comment demander l’autorisation d’un utilisateur pour utiliser son compte Microsoft, obtenir un jeton d’accès et l’utiliser pour effectuer des opérations de base (par exemple, obtenir des données de profil ou télécharger des fichiers sur leur compte OneDrive). Pour obtenir l’autorisation et l’accès utilisateur, les étapes sont similaires quel que soit le fournisseur d’identité, à condition qu’il prenne en charge le Gestionnaire de compte web.

> [!NOTE]
> Pour obtenir un exemple du code complet, voir l’[exemple WebAccountManagement sur GitHub](http://go.microsoft.com/fwlink/p/?LinkId=620621).

## <a name="get-set-up"></a>Préparation

Pour commencer, créez une nouvelle application vierge dans Visual Studio. 

Ensuite, pour mettre en place la connexion à des fournisseurs d’identité, vous devez associer votre application au Windows Store. Pour ce faire, cliquez avec le bouton droit de la souris sur votre projet, choisissez **Store** > **Associer l’application au Windows Store**, puis suivez les instructions de l’assistant. 

Enfin, créez une interface utilisateur très simple constituée d’un bouton XAML et de deux zones de texte.

```XML
<StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
    <Button x:Name="LoginButton" Content="Log in" Click="LoginButton_Click" />
    <TextBlock x:Name="UserIdTextBlock"/>
    <TextBlock x:Name="UserNameTextBlock"/>
</StackPanel>
```

Reliez un gestionnaire d’événements à votre bouton dans le code-behind:

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{   
}
```

Pour finir, ajoutez les espaces de noms suivants afin de ne pas avoir à vous soucier ultérieurement des problèmes de référence: 

```csharp
using System;
using Windows.Security.Authentication.Web.Core;
using Windows.System;
using Windows.UI.ApplicationSettings;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.Data.Json;
using Windows.UI.Xaml.Navigation;
using Windows.Web.Http;
```

## <a name="show-the-accounts-settings-pane"></a>Afficher le volet AccountsSettingsPane

Le système fournit une interface utilisateur intégrée permettant de gérer les fournisseurs d’identité et les comptes web, appelée **AccountsSettingsPane**. Vous pouvez l’afficher comme suit:

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{
    AccountsSettingsPane.Show(); 
}
```

Si vous exécutez votre application et cliquez sur le bouton «Se connecter», elle affiche une fenêtre vide. 

![Volet Paramètres du compte](images/tb-1.png)

Le volet est vide, car le système propose uniquement un interpréteur de commandes de l’interface utilisateur. Il revient au développeur de programmer le remplissage du volet avec les fournisseurs d’identité. 

## <a name="register-for-accountcommandsrequested"></a>S’inscrire à AccountCommandsRequested

Pour ajouter des commandes au volet, il faut tout d’abord s’inscrire au gestionnaire d’événements AccountCommandsRequested. Cela indique au système d’exécuter notre logique de build lorsque l’utilisateur demande à afficher le volet (par exemple, en cliquant sur notre bouton XAML). 

Dans votre code-behind, remplacez les événements OnNavigatedTo et OnNavigatedFrom et ajoutez-y le code suivant: 

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    AccountsSettingsPane.GetForCurrentView().AccountCommandsRequested += BuildPaneAsync; 
}
```

```csharp
protected override void OnNavigatedFrom(NavigationEventArgs e)
{
    AccountsSettingsPane.GetForCurrentView().AccountCommandsRequested -= BuildPaneAsync; 
}
```

Les utilisateurs n’interagissent pas très souvent avec les comptes, ce qui signifie qu’en procédant ainsi pour l’inscription et la désinscription de votre gestionnaire d’événements, vous évitez les fuites de mémoire. De cette façon, votre volet personnalisé est uniquement en mémoire lorsqu’il y a de grandes chances qu’un utilisateur le demande (parce qu’il est sur une page «Paramètres» ou «Connexion», par exemple). 

## <a name="build-the-account-settings-pane"></a>Conception du volet Paramètres du compte

La méthode BuildPaneAsync est appelée chaque fois que le volet **AccountsSettingsPane** s’affiche. C’est ici que nous allons enregistrer le code nécessaire à la personnalisation des commandes affichées dans le volet. 

Commencez par obtenir un report. Cela indique au système qu’il doit retarder l’affichage du volet **AccountsSettingsPane** jusqu'à ce que sa conception soit terminée.

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();
        
    deferral.Complete(); 
}
```

Ensuite, obtenez un fournisseur à l’aide de la méthode WebAuthenticationCoreManager.FindAccountProviderAsync. L’URL du fournisseur varie en fonction du fournisseur et figure dans la documentation correspondante. Pour les comptes Microsoft et AzureActiveDirectory, il s'agit de «https://login.microsoft.com». 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();
        
    var msaProvider = await WebAuthenticationCoreManager.FindAccountProviderAsync(
        "https://login.microsoft.com", "consumers"); 
        
    deferral.Complete(); 
}
```

Notez que nous passons également la chaîne «consumers» au paramètre facultatif *authority*. En effet, Microsoft fournit deux types d’authentification: les comptes Microsoft (MSA) pour les «consommateurs» et Azure Active Directory (AAD) pour les «entreprises». Le paramètre «consumers» indique que nous voulons l’option MSA. Si vous développez une application d’entreprise, utilisez la chaîne «organizations» à la place.

Enfin, ajoutez le fournisseur à la classe **AccountsSettingsPane** en créant une nouvelle commande **[WebAccountProviderCommand](https://docs.microsoft.com/uwp/api/windows.ui.applicationsettings.webaccountprovidercommand)** comme suit: 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();

    var msaProvider = await WebAuthenticationCoreManager.FindAccountProviderAsync(
        "https://login.microsoft.com", "consumers");

    var command = new WebAccountProviderCommand(msaProvider, GetMsaTokenAsync);  

    e.WebAccountProviderCommands.Add(command);

    deferral.Complete(); 
}
```

La méthode GetMsaToken que nous avons passée à notre nouvelle commande **WebAccountProviderCommand** n’existe pas encore (nous allons la créer à l’étape suivante). Par conséquent, n’hésitez pas à l’ajouter en tant que méthode vide pour le moment.

Exécutez le code ci-dessus pour que votre volet ressemble à ceci: 

![Volet Paramètres du compte](images/tb-2.png)

### <a name="request-a-token"></a>Demander un jeton

Une fois que l’option Compte Microsoft s’affiche dans le volet **AccountsSettingsPane**, nous devons gérer ce qui se produit lorsque l’utilisateur la sélectionne. Nous avons enregistré notre méthode GetMsaToken afin qu’elle se déclenche quand l’utilisateur choisit d’ouvrir une session avec son compte Microsoft. C’est donc ainsi que nous allons obtenir le jeton. 

Pour obtenir un jeton, utilisez la méthode RequestTokenAsync comme suit: 

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
}
```

Dans cet exemple, nous passons la chaîne «wl.basic» au paramètre d’_étendue_. L’étendue représente le type d’informations concernant un utilisateur spécifique que vous demandez au service fournisseur. Certaines étendues donnent accès uniquement aux informations de base d’un utilisateur, comme son nom et son adresse e-mail, tandis que d'autres étendues peuvent accorder un accès à des informations sensibles telles que des photos de l’utilisateur ou la boîte de réception de sa messagerie électronique. En général, votre application doit utiliser l’étendue nécessaire la moins permissive pour remplir sa fonction. Les prestataires de services fournissent une documentation indiquant les étendues nécessaires pour obtenir des jetons à utiliser avec leurs services. 

* Pour les étendues Office365 et Outlook.com, consultez [Authentification des API Office365 et Outlook.com à l’aide du point de terminaison d’authentificationv2.0](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2). 
* Pour les étendues OneDrive, consultez [Authentification et connexion OneDrive](https://dev.onedrive.com/auth/msa_oauth.htm#authentication-scopes). 

> [!TIP]
> Si vous le souhaitez, si votre application utilise une indication de connexion (afin de remplir le champ de l’utilisateur avec une adresse de messagerie par défaut) ou une autre propriété spéciale liés à l’expérience de connexion, répertorier dans la propriété **[WebTokenRequest.AppProperties](https://docs.microsoft.com/uwp/api/windows.security.authentication.web.core.webtokenrequest.appproperties#Windows_Security_Authentication_Web_Core_WebTokenRequest_AppProperties)** . Cela entraînera le système ignorer la propriété lors de la mise en cache du compte web, ce qui empêche les différences de compte dans le cache.

Si vous développez une application d’entreprise, vous souhaiterez probablement vous connecter à une instance Azure Active Directory (AAD) et utiliser l’API Microsoft Graph plutôt que les services MSA classiques. Dans ce cas, utilisez le code suivant: 

```csharp
private async void GetAadTokenAsync(WebAccountProviderCommand command)
{
    string clientId = "your_guid_here"; // Obtain your clientId from the Azure Portal
    WebTokenRequest request = new WebTokenRequest(provider, "User.Read", clientId);
    request.Properties.Add("resource", "https://graph.microsoft.com");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
}
```

Le reste de cet article décrit la suite du scénario MSA, mais le code pour AAD est très similaire. Pour plus d’informations sur AAD/Microsoft Graph, notamment un exemple complet sur GitHub, voir la [documentation Microsoft Graph](https://graph.microsoft.io/docs/platform/get-started).

## <a name="use-the-token"></a>Utiliser le jeton

La méthode RequestTokenAsync renvoie un objet WebTokenRequestResult, qui contient les résultats de votre demande. Si votre demande a abouti, elle contiendra un jeton.  

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        string token = result.ResponseData[0].Token; 
    }
}
```

> [!NOTE]
> Si une erreur est générée lors de la demande d’un jeton, vérifiez que vous avez bien associé votre application au Store, comme décrit à l’étape1. Votre application ne pourra pas obtenir de jeton si vous avez ignoré cette étape. 

Une fois le jeton en votre possession, vous pouvez l’utiliser pour appeler les API de votre fournisseur. Dans le code ci-dessous, nous allons appeler l'[API Microsoft Live information utilisateur](https://msdn.microsoft.com/library/hh826533.aspx) pour obtenir des informations de base relatives à l’utilisateur et les afficher dans notre interface utilisateur. Notez toutefois que dans la plupart des cas, il est recommandé de stocker le jeton obtenu, puis de l’utiliser dans une autre méthode.

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        string token = result.ResponseData[0].Token; 
        
        var restApi = new Uri(@"https://apis.live.net/v5.0/me?access_token=" + token);

        using (var client = new HttpClient())
        {
            var infoResult = await client.GetAsync(restApi);
            string content = await infoResult.Content.ReadAsStringAsync();

            var jsonObject = JsonObject.Parse(content);
            string id = jsonObject["id"].GetString();
            string name = jsonObject["name"].GetString();

            UserIdTextBlock.Text = "Id: " + id; 
            UserNameTextBlock.Text = "Name: " + name;
        }
    }
}
```

La méthode utilisée pour appeler les différentes API REST varie d’un fournisseur à l’autre; voir la documentation sur les API du fournisseur pour plus d’informations sur l’utilisation de votre jeton. 

## <a name="store-the-account-for-future-use"></a>Stocker le compte pour une utilisation ultérieure

Les jetons sont utiles pour obtenir immédiatement des informations relatives à un utilisateur, mais leur durée de validité est généralement très variable: les jetons MSA, par exemple, ne sont valides que pendant quelques heures. Heureusement, vous n’avez pas besoin d’afficher de nouveau le volet **AccountsSettingsPane** chaque fois qu’un jeton arrive à expiration. Lorsqu’un utilisateur a autorisé une fois votre application, vous pouvez stocker les informations de compte de l’utilisateur pour une utilisation future. 

Pour ce faire, utilisez la classe **[WebAccount](https://docs.microsoft.com/uwp/api/windows.security.credentials.webaccount)**. Une classe **WebAccount** est renvoyée par la même méthode que vous avez utilisée pour demander le jeton:

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        WebAccount account = result.ResponseData[0].WebAccount; 
    }
}
```

Une fois que vous disposez d’une instance **WebAccount**, vous pouvez la stocker facilement. Dans l’exemple suivant, nous utilisons LocalSettings. Pour en savoir plus sur l’utilisation de LocalSettings et d'autres méthodes pour stocker des données utilisateur, voir [Stocker et récupérer des paramètres et données d’application](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).

```csharp
private async void StoreWebAccount(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values["CurrentUserProviderId"] = account.WebAccountProvider.Id;
    ApplicationData.Current.LocalSettings.Values["CurrentUserId"] = account.Id; 
}
```

Ensuite, nous pouvons utiliser une méthode asynchrone comme la suivante pour tenter d’obtenir un jeton en arrière-plan avec la classe **WebAccount** stockée.

```csharp
private async Task<string> GetTokenSilentlyAsync()
{
    string providerId = ApplicationData.Current.LocalSettings.Values["CurrentUserProviderId"]?.ToString();
    string accountId = ApplicationData.Current.LocalSettings.Values["CurrentUserId"]?.ToString();

    if (null == providerId || null == accountId)
    {
        return null; 
    }

    WebAccountProvider provider = await WebAuthenticationCoreManager.FindAccountProviderAsync(providerId);
    WebAccount account = await WebAuthenticationCoreManager.FindAccountAsync(provider, accountId);

    WebTokenRequest request = new WebTokenRequest(provider, "wl.basic");

    WebTokenRequestResult result = await WebAuthenticationCoreManager.GetTokenSilentlyAsync(request, account);
    if (result.ResponseStatus == WebTokenRequestStatus.UserInteractionRequired)
    {
        // Unable to get a token silently - you'll need to show the UI
        return null; 
    }
    else if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        // Success
        return result.ResponseData[0].Token;
    }
    else
    {
        // Other error 
        return null; 
    }
}
```

Placez la méthode ci-dessus juste avant le code qui crée le volet **AccountsSettingsPane**. Si le jeton est obtenu en arrière-plan, il est inutile d'afficher le volet. 

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{
    string silentToken = await GetMsaTokenSilentlyAsync();

    if (silentToken != null)
    {
        // the token was obtained. store a reference to it or do something with it here.
    }
    else
    {
        // the token could not be obtained silently. Show the AccountsSettingsPane
        AccountsSettingsPane.Show();
    }
}
```

Dans la mesure où il est très simple d’obtenir un jeton silencieusement, nous vous recommandons d’utiliser ce processus pour actualiser votre jeton entre les sessions, plutôt que de mettre en cache un jeton existant (étant donné que ce jeton peut expirer à tout moment).

> [!NOTE]
> L’exemple ci-dessus couvre uniquement les cas d’échec et de réussite de base. Votre application doit également prendre en compte des scénarios plus inhabituels, comme un utilisateur qui révoque l’autorisation de votre application ou qui supprimer son compte de Windows, par exemple, et les gérer de manière fluide.  

## <a name="remove-a-stored-account"></a>Supprimer un compte stocké

Si vous conservez un compte web, vous pouvez permettre à vos utilisateurs de dissocier leur compte de votre application. Ainsi, ils peuvent effectivement «se déconnecter» de l’application: les informations de leur compte ne seront plus chargées automatiquement lors du lancement. Pour ce faire, commencez par supprimer du stockage les comptes enregistrés et les informations sur le fournisseur. Appelez ensuite **[SignOutAsync](https://docs.microsoft.com/uwp/api/windows.security.credentials.webaccount.SignOutAsync)** pour vider le cache et invalider des jetons existants dont votre application dispose. 

```csharp
private async Task SignOutAccountAsync(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserProviderId");
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserId"); 
    account.SignOutAsync(); 
}
```

## <a name="add-providers-that-dont-support-webaccountmanager"></a>Ajouter des fournisseurs qui ne prennent pas en charge WebAccountManager

Si vous souhaitez intégrer l’authentification à votre application à partir d’un service qui ne prend pas en charge WebAccountManager (Google+ ou Twitter, par exemple), vous pouvez ajouter manuellement ce fournisseur à la classe **AccountsSettingsPane**. Pour ce faire, créez un nouvel objet WebAccountProvider avec vos propres nom et icône .png, puis ajoutez-le à la liste WebAccountProviderCommands. Voici le code stub: 

 ```csharp
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 

    var twitterProvider = new WebAccountProvider("twitter", "Twitter", new Uri(@"ms-appx:///Assets/twitter-auth-icon.png")); 
    var twitterCmd = new WebAccountProviderCommand(twitterProvider, GetTwitterTokenAsync);
    e.WebAccountProviderCommands.Add(twitterCmd);   
    
    // other code here
}

private async void GetTwitterTokenAsync(WebAccountProviderCommand command)
{
    // Manually handle Twitter login here
}

```

> [!NOTE] 
> Cette action ajoute uniquement une icône au volet **AccountsSettingsPane** et exécute la méthode que vous indiquez en cas de clic sur l’icône (GetTwitterTokenAsync, dans le cas présent). Vous devez fournir le code qui gère l’authentification réelle. Pour plus d’informations, voir (Service Broker d’authentification Web)[web-authentication-broker], qui fournit des méthodes d’assistance pour l’authentification à l’aide des services REST. 

## <a name="add-a-custom-header"></a>Ajouter un en-tête personnalisé

Vous pouvez personnaliser le volet Paramètres du compte à l’aide de la propriété HeaderText, comme ceci: 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 
    
    args.HeaderText = "MyAwesomeApp works best if you're signed in.";   
    
    // other code here
}
```

![Volet Paramètres du compte](images/tb-3.png)

Ne développez pas trop le texte de l’en-tête; il doit rester bref et concis. Si votre processus de connexion est compliqué et que vous avez besoin d’afficher plus d’informations, renvoyez l’utilisateur à une page distincte à l’aide d’un lien personnalisé. 

## <a name="add-custom-links"></a>Ajouter des liens personnalisés

Vous pouvez ajouter des commandes personnalisées à la classe AccountsSettingsPane. Elles apparaissent sous forme de liens sous vos WebAccountProviders pris en charge. Les commandes personnalisées sont parfaites pour les tâches simples sur les comptes d’utilisateurs, telles que l’affichage d’une politique de confidentialité ou l’ouverture d’une page de support pour les utilisateurs rencontrant des difficultés. 

Voici un exemple: 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 
    
    var settingsCmd = new SettingsCommand(
        "settings_privacy", 
        "Privacy policy", 
        async (x) => await Launcher.LaunchUriAsync(new Uri(@"https://privacy.microsoft.com/en-US/"))); 

    e.Commands.Add(settingsCmd); 
    
    // other code here
}
```

![Volet Paramètres du compte](images/tb-4.png)

En théorie, vous pouvez utiliser les commandes de paramètres pour tout. Toutefois, nous vous recommandons d’en limiter l’utilisation aux scénarios intuitifs liés aux comptes, tels que ceux décrits ci-dessus. 

## <a name="see-also"></a>Voir également

[Espace de noms Windows.Security.Authentication.Web.Core](https://msdn.microsoft.com/library/windows/apps/windows.security.authentication.web.core.aspx)

[Espace de noms Windows.Security.Credentials](https://msdn.microsoft.com/library/windows/apps/windows.security.credentials.aspx)

[Classe AccountsSettingsPane](https://msdn.microsoft.com/library/windows/apps/windows.ui.applicationsettings.accountssettingspane)

[Service Broker d’authentification web](web-authentication-broker.md)

[Exemple de gestion de comptes web](http://go.microsoft.com/fwlink/p/?LinkId=620621)

[Application de planification de déjeuners](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
