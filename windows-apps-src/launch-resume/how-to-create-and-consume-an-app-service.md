---
author: mcleblanc
title: Créer et utiliser un service d’application
description: Découvrez comment écrire une application de plateforme Windows universelle (UWP) capable de fournir des services à d’autres applications UWP, et comment utiliser ces services.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
---

# Créer et utiliser un service d’application


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Découvrez comment écrire une application de plateforme Windows universelle (UWP) capable de fournir des services à d’autres applications UWP et comment utiliser ces services.

## Créer un projet de fournisseur de service d’application


Dans la procédure décrite ici, nous allons créer tous les éléments dans une seule solution par souci de simplicité.

-   Dans Microsoft Visual Studio 2015, créez un projet d’application UWP et nommez-le AppServiceProvider. (Dans la boîte de dialogue **Nouveau projet**, sélectionnez **Modèles &gt; Autres langages &gt; Visual C# &gt; Windows &gt; Windows universel &gt; Application vide [Windows universel]**). Il s’agira de l’application qui fournit le service d’application.

## Ajouter une extension de service d’application à package.appxmanifest


Dans le fichier Package.appxmanifest du projet AppServiceProvider, ajoutez l’extension AppService suivante à l’élément **&lt;Application&gt;**. Cet exemple publie le service `com.Microsoft.Inventory`, qui identifie cette application en tant que fournisseur de service d’application. Le service proprement dit est implémenté sous forme de tâche en arrière-plan. L’application de service d’application expose le service aux autres applications. Nous vous recommandons d’utiliser un style de nom de domaine inverse pour le nom du service.

``` syntax
... 
<Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="AppServiceProvider.App">
      <Extensions>
        <uap:Extension Category="windows.appService" EntryPoint="MyAppService.Inventory">
          <uap:AppService Name="com.microsoft.inventory"/>
        </uap:Extension>
      </Extensions>
      ...
    </Application>
</Applications>
```

L’attribut **Category** identifie cette application en tant que fournisseur de service d’application.

L’attribut **EntryPoint** identifie la classe qui implémente le service, que nous allons implémenter à l’étape suivante.

## Créer le service d’application


1.  Un service d’application est implémenté sous forme de tâche en arrière-plan. Cela permet à une application au premier plan d’appeler un service d’application dans une autre application pour effectuer des tâches en arrière-plan. Ajoutez un nouveau projet de composant Windows Runtime nommé MyAppService à la solution (**Fichier&gt; Ajouter&gt; Nouveau projet**). (Dans la boîte de dialogue **Ajouter un nouveau projet**, choisissez **Installé &gt; Autres langages &gt; Visual C# &gt; Windows &gt; Windows universel &gt; Composant Windows Runtime (Windows universel)**.
2.  Dans le projet AppServiceProvider, ajoutez une référence au projet MyAppService.
3.  Dans le projet MyappService, ajoutez les instructions **using** suivantes au début du fichier Class1.cs :
    ```cs
    using Windows.ApplicationModel.AppService;
    using Windows.ApplicationModel.Background;
    using Windows.Foundation.Collections;
    ```

4.  Remplacez le code stub pour **Class1** par une nouvelle classe de tâche en arrière-plan nommée **Inventory** :

    ```cs
    public sealed class Inventory : IBackgroundTask
    {
        private BackgroundTaskDeferral backgroundTaskDeferral;
        private AppServiceConnection appServiceconnection;
        private String[] inventoryItems = new string[] { "Robot vacuum", "Chair" };
        private double[] inventoryPrices = new double[] { 129.99, 88.99 };

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            this.backgroundTaskDeferral = taskInstance.GetDeferral(); // Get a deferral so that the service isn't terminated.
            taskInstance.Canceled += OnTaskCanceled; // Associate a cancellation handler with the background task.

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
        }

        private void OnTaskCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            if (this.backgroundTaskDeferral != null)
            {
                // Complete the service deferral.
                this.backgroundTaskDeferral.Complete();
            }
        }
    }
    ```

    C’est dans cette classe que le service d’application effectuera son travail.

    **Run()** est appelé lorsque la tâche en arrière-plan est créée. Comme les tâches en arrière-plan sont arrêtées une fois **Run** terminé, le code effectue un report afin que la tâche en arrière-plan reste active pour traiter les demandes.

    **OnTaskCanceled()** est appelé lorsque la tâche est annulée. La tâche est annulée lorsque l’application cliente supprime la classe [**AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704), l’application cliente est suspendue, le système d’exploitation est arrêté ou mis en veille ou lorsque le système d’exploitation manque de ressources pour exécuter la tâche.

## Écrire le code du service d’application


**OnRequestedReceived()** est l’emplacement du code du service d’application. Remplacez le stub **OnRequestedReceived()** dans le fichier Class1.cs de MyAppService par le code de cet exemple. Ce code obtient un index pour un article en stock et le transmet au service avec une chaîne de commande pour récupérer le nom et le prix de l’article en stock spécifié. Le code de gestion des erreurs a été supprimé par souci de concision.

```cs
private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below to respond to the message
    // and we don't want this call to get cancelled while we are waiting.
    var messageDeferral = args.GetDeferral();

    ValueSet message = args.Request.Message;
    ValueSet returnData = new ValueSet();

    string command = message["Command"] as string;
    int? inventoryIndex = message["ID"] as int?;

    if ( inventoryIndex.HasValue &&
         inventoryIndex.Value >= 0 &&
         inventoryIndex.Value < inventoryItems.GetLength(0))
    {
        switch (command)
        {
            case "Price":
            {
                returnData.Add("Result", inventoryPrices[inventoryIndex.Value]);
                returnData.Add("Status", "OK");
                break;
            }

            case "Item":
            {
                returnData.Add("Result", inventoryItems[inventoryIndex.Value]);
                returnData.Add("Status", "OK");
                break;
            }

            default:
            {
                returnData.Add("Status", "Fail: unknown command");
                break;
            }
        }
    }
    else
    {
        returnData.Add("Status", "Fail: Index out of range");
    }

    await args.Request.SendResponseAsync(returnData); // Return the data to the caller.
    messageDeferral.Complete(); // Complete the deferral so that the platform knows that we're done responding to the app service call.
}
```

Notez que **OnRequestedReceived()** a pour valeur **async**, car nous effectuons un appel de méthode awaitable à [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) dans cet exemple.

Un report est effectué afin que le service puisse utiliser les méthodes **async** dans le gestionnaire OnRequestReceived. Cela permet de s’assurer que l’appel à OnRequestReceived ne se termine pas avant la fin du traitement du message. [
            **SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) est utilisé pour envoyer une réponse parallèlement à l’achèvement. **SendResponseAsync** n’indique pas l’achèvement de l’appel. C’est l’achèvement du report qui indique à [**SendMessageAsync**](https://msdn.microsoft.com/library/windows/apps/dn921712) que OnRequestReceived est terminé.

Les services d’application utilisent une classe [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) pour échanger des informations. La taille des données que vous pouvez transmettre est limitée uniquement par les ressources système. Il n’y a aucune clé prédéfinie utilisable dans votre classe **ValueSet**. Vous devez déterminer quelles valeurs de clé vous allez utiliser pour définir le protocole de votre service d’application. L’appelant doit être écrit avec ce protocole à l’esprit. Dans cet exemple, nous avons choisi une clé nommée « Command » dont la valeur indique si nous voulons que le service d’application fournisse le nom de l’article en stock ou son prix. L’index du nom d’inventaire est stocké sous la clé « ID ». La valeur renvoyée est stockée sous la clé « Result ».

Une énumération [**AppServiceClosedStatus**](https://msdn.microsoft.com/library/windows/apps/dn921703) est renvoyée à l’appelant pour indiquer si l’appel au service d’application a réussi ou échoué. L’appel au service d’application peut échouer par exemple si le système d’exploitation abandonne le point de terminaison du service ou si les ressources sont dépassées. Vous pouvez renvoyer des informations d’erreur supplémentaires via la classe [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131). Dans cet exemple, nous utilisons une clé nommée Status pour renvoyer des informations plus détaillées sur l’erreur à l’appelant.

L’appel à [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) renvoie la classe [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) à l’appelant.

## Déployer l’application de service et obtenir le nom de la famille de packages


L’application qui fournit le service d’application doit être déployée avant de pouvoir être appelée depuis un client. Vous avez également besoin du nom de famille du package de l’application de service d’application pour l’appeler.

-   Un moyen d’obtenir le nom de famille du package de l’application de service d’application consiste à appeler [**Windows.ApplicationModel.Package.Current.Id.FamilyName**](https://msdn.microsoft.com/library/windows/apps/br224670) à partir du projet **AppServiceProvider** (par exemple, depuis `public App()` dans App.xaml.cs) et à noter le résultat. Pour exécuter AppServiceProvider dans Microsoft Visual Studio, définissez-le en tant que projet de démarrage dans la fenêtre Explorateur de solutions, puis exécutez le projet.
-   Pour obtenir le nom de famille du package, vous pouvez également déployer la solution (**Générer &gt; Déployer la solution**) et noter le nom complet du package dans la fenêtre Sortie (**Affichage &gt; Sortie**). Vous devez supprimer les informations de plateforme de la chaîne dans la fenêtre Sortie pour obtenir le nom du package. Par exemple, si le nom complet du package indiqué dans la fenêtre de sortie était « 9fe3058b-3de0-4e05-bea7-84a06f0ee4f0\_1.0.0.0\_x86\_\_yd7nk54bq29ra », vous extrairiez « 1.0.0.0\_x86\_\_ » et laisseriez « 9fe3058b-3de0-4e05-bea7-84a06f0ee4f0\_yd7nk54bq29ra » comme nom de la famille de packages.

## Écrire un client pour appeler le service d’application


1.  Ajoutez un nouveau projet d’application Windows universel vide nommé ClientApp à la solution (**Fichier &gt; Ajouter &gt; Nouveau projet**). (Dans la boîte de dialogue **Ajouter un nouveau projet**, choisissez **Installé &gt; Autres langages &gt; Visual C# &gt; Windows &gt; Windows universel &gt; Application vide [Windows universel]**).
2.  Dans le projet ClientApp, ajoutez l’instruction **using** suivante au début du fichier MainPage.xaml.cs :
    ```cs
    >using Windows.ApplicationModel.AppService;
    ```

3.  Ajoutez une zone de texte et un bouton à MainPage.xaml.
4.  Ajoutez un gestionnaire d’événements de clic pour le bouton, puis le mot-clé **async** à la signature du gestionnaire de boutons.
5.  Remplacez le stub de votre gestionnaire d’événement de clic pour le bouton par le code suivant. Veillez à inclure la déclaration de champ `inventoryService`.

   ```cs
   private AppServiceConnection inventoryService;
    private async void button_Click(object sender, RoutedEventArgs e)
    {
        // Add the connection.
        if (this.inventoryService == null)
        {
            this.inventoryService = new AppServiceConnection();

            // Here, we use the app service name defined in the app service provider's Package.appxmanifest file in the <Extension> section. 
            this.inventoryService.AppServiceName = "com.microsoft.inventory";

            // Use Windows.ApplicationModel.Package.Current.Id.FamilyName within the app service provider to get this value.
            this.inventoryService.PackageFamilyName = "replace with the package family name";

            var status = await this.inventoryService.OpenAsync();
            if (status != AppServiceConnectionStatus.Success)
            {
                button.Content = "Failed to connect";
                return;
            }
        }

        // Call the service.
        int idx = int.Parse(textBox.Text);
        var message = new ValueSet();
        message.Add("Command", "Item");
        message.Add("ID", idx);
        AppServiceResponse response = await this.inventoryService.SendMessageAsync(message);
        string result = "";

        if (response.Status == AppServiceResponseStatus.Success)
        {
            // Get the data  that the service sent  to us.
            if (response.Message["Status"] as string == "OK")
            {
                result = response.Message["Result"] as string;
            }
        }

        message.Clear();
        message.Add("Command", "Price");
        message.Add("ID", idx);
        response = await this.inventoryService.SendMessageAsync(message);

        if (response.Status == AppServiceResponseStatus.Success)
        {
            // Get the data that the service sent to us.
            if (response.Message["Status"] as string == "OK")
            {
                result += " : Price = " + response.Message["Result"] as string;
            }
        }

        button.Content = result;
    }
    ```

    Remplacez le nom de la famille de packages de la ligne `this.inventoryService.PackageFamilyName = "replace with the package family name";` par le nom de la famille de packages du projet **AppServiceProvider** que vous avez obtenu à l’\[Étape 5 : Déployer l’application de service et obtenir le nom de la famille de packages].

    Le code établit tout d’abord une connexion avec le service d’application. La connexion reste alors ouverte jusqu’à ce que vous supprimiez **this.inventoryService**. Le nom du service d’application doit correspondre à l’attribut **AppService Name** que vous avez ajouté au fichier Package.appxmanifest du projet AppServiceProvider. Dans cet exemple, il s’agit de `<uap:AppService Name="com.microsoft.inventory"/>`.

    Un [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) nommé **message** est créé pour spécifier la commande que vous souhaitez envoyer au service d’application. L’exemple de service d’application attend une commande pour indiquer laquelle des deux actions effectuer. Nous obtenons l’index de l’élément textbox de ClientApp, puis appelons le service à l’aide de la commande Item pour obtenir la description de l’article. Ensuite, nous effectuons l’appel avec la commande Price afin d’obtenir le prix de l’article. Le texte du bouton est défini en fonction du résultat.

    Comme [**AppServiceResponseStatus**](https://msdn.microsoft.com/library/windows/apps/dn921724) indique uniquement si le système d’exploitation a été en mesure de connecter l’appel au service d’application, nous vérifions la clé Status dans le [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) que nous recevons à partir du service d’application pour nous assurer qu’il a pu répondre à la demande.

6.  Dans Visual Studio, définissez le projet ClientApp en tant que projet de démarrage dans la fenêtre Explorateur de solutions et exécutez la solution. Entrez le chiffre 1 dans la zone de texte et cliquez sur le bouton. Le service doit renvoyer « Chair : Price = 88.99 ».

    ![exemple d’application affichant chair price=88.99](images/appserviceclientapp.png)

Si l’appel au service d’application échoue, vérifiez les éléments suivants dans ClientApp :

1.  Vérifiez que le nom de la famille de packages affecté à la connexion au service d’inventaire correspond au nom de la famille de packages de l’application AppServiceProvider. Voir **button\_Click()**`this.inventoryService.PackageFamilyName = "...";`).
2.  Dans **button\_Click()**, vérifiez que le nom de service d’application affecté à la connexion au service d’inventaire correspond au nom du service d’application dans le fichier Package.appxmanifest du projet AppServiceProvider. Voir `this.inventoryService.AppServiceName = "com.microsoft.inventory";`.
3.  Assurez-vous que l’application AppServiceProvider a été déployée (dans l’Explorateur de solutions, cliquez avec le bouton droit sur la solution et choisissez **Déployer**).

## Déboguer le service d’application


1.  Assurez-vous que la solution est déployée dans son intégralité avant le débogage, car l’application qui fournit le service d’application doit être déployée avant que le service puisse être appelé. (Dans Visual Studio, **Générer &gt; Déployer la solution**).
2.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet AppServiceProvider et choisissez **Propriétés**. Dans l’onglet **Déboguer**, définissez **Action de démarrage** sur **Ne pas lancer, mais déboguer mon code au démarrage**.
3.  Dans le projet MyAppService, dans le fichier Class1.cs, définissez un point d’arrêt dans OnRequestReceived().
4.  Définissez le projet AppServiceProvider en tant que projet de démarrage et appuyez sur F5.
5.  Lancez ClientApp depuis le menu Démarrer (et non depuis Visual Studio).
6.  Entrez le chiffre 1 dans la zone de texte et appuyez sur le bouton. Le débogueur s’arrête dans l’appel au service d’application sur le point d’arrêt défini dans votre service d’application.

## Déboguer le client


1.  Pour déboguer le service d’application, suivez les instructions de l’étape précédente.
2.  Lancez ClientApp depuis le menu Démarrer.
3.  Attachez le débogueur au processus ClientApp.exe (et non au processus ApplicationFrameHost.exe). (Dans Visual Studio, choisissez **Déboguer &gt; Attacher au processus…**.)
4.  Dans le projet ClientApp, définissez un point d’arrêt dans **button\_Click()**.
5.  Les points d’arrêt dans le client et le service d’application sont maintenant atteints lorsque vous entrez le chiffre 1 dans la zone de texte de ClientApp et cliquez sur le bouton.

## Remarques


Cet exemple offre une introduction simple à la création d’un service d’application et à l’appel de ce service à partir d’une autre application. Les points importants sont la création d’une tâche en arrière-plan pour héberger le service d’application, l’ajout de l’extension windows.appservice au fichier Package.appxmanifest de l’application qui fournit le service d’application, l’obtention du nom de la famille de packages de l’application qui fournit le service d’application pour pouvoir se connecter à cette dernière à partir de l’application cliente, et l’utilisation de [**Windows.ApplicationModel.AppService.AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704) pour appeler le service.

## Code complet pour MyAppService


```cs
using System;
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
using Windows.Foundation.Collections;

namespace MyAppService
{
    public sealed class Inventory : IBackgroundTask
    {
        private BackgroundTaskDeferral backgroundTaskDeferral;
        private AppServiceConnection appServiceconnection;
        private String[] inventoryItems = new string[] { "Robot vacuum", "Chair" };
        private double[] inventoryPrices = new double[] { 129.99, 88.99 };

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            this.backgroundTaskDeferral = taskInstance.GetDeferral(); // Get a deferral so that the service isn't terminated.
            taskInstance.Canceled += OnTaskCanceled; // Associate a cancellation handler with the background task.

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // Get a deferral because we use an awaitable API below to respond to the message
            // and we don't want this call to get cancelled while we are waiting.
            var messageDeferral = args.GetDeferral();

            ValueSet message = args.Request.Message;
            ValueSet returnData = new ValueSet();

            string command = message["Command"] as string;
            int? inventoryIndex = message["ID"] as int?;

            if (inventoryIndex.HasValue &&
                 inventoryIndex.Value >= 0 &&
                 inventoryIndex.Value < inventoryItems.GetLength(0))
            {
                switch (command)
                {
                    case "Price":
                        {
                            returnData.Add("Result", inventoryPrices[inventoryIndex.Value]);
                            returnData.Add("Status", "OK");
                            break;
                        }

                    case "Item":
                        {
                            returnData.Add("Result", inventoryItems[inventoryIndex.Value]);
                            returnData.Add("Status", "OK");
                            break;
                        }

                    default:
                        {
                            returnData.Add("Status", "Fail: unknown command");
                            break;
                        }
                }
            }
            else
            {
                returnData.Add("Status", "Fail: Index out of range");
            }

            await args.Request.SendResponseAsync(returnData); // Return the data to the caller.
            messageDeferral.Complete(); // Complete the deferral so that the platform knows that we're done responding to the app service call.
        }


        private void OnTaskCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            if (this.backgroundTaskDeferral != null)
            {
                // Complete the service deferral.
                this.backgroundTaskDeferral.Complete();
            }
        }
    }
}
```

## Rubriques connexes


* [Prendre en charge votre application avec des tâches en arrière-plan](support-your-app-with-background-tasks.md)

 

 





<!--HONumber=May16_HO2-->


