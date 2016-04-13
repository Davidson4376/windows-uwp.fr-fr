---
title: Créer et utiliser un service d’application
description: Découvrez comment écrire une application de plateforme Windows universelle (UWP) capable de fournir des services à d’autres applications UWP et comment utiliser ces services.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
---

# Créer et utiliser un service d’application


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Découvrez comment écrire une application de plateforme Windows universelle (UWP) capable de fournir des services à d’autres applications UWP et comment utiliser ces services.

## Créer un projet de fournisseur de service d’application


Dans la procédure décrite ici, nous allons créer tous les éléments dans une seule solution par souci de simplicité.

-   Dans Microsoft Visual Studio 2015, créez un projet d’application UWP et nommez-le AppServiceProvider. (Dans la boîte de dialogue **Nouveau projet**, sélectionnez **Modèles &gt; Autres langages &gt; Visual C# &gt; Windows &gt; Windows universel &gt; Application vide (Windows universel)**). Il s’agira de l’application qui fournit le service d’application.

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


1.  Un service d’application est implémenté sous forme de tâche en arrière-plan. Cela permet à une application au premier plan d’appeler un service d’application dans une autre application pour effectuer des tâches en arrière-plan. Ajoutez un nouveau projet de composant Windows Runtime nommé MyAppService à la solution (**Fichier &gt; Ajouter &gt; Nouveau projet**). (Dans la boîte de dialogue **Ajouter un nouveau projet**, choisissez **Installé &gt; Autres langages &gt; Visual C# &gt; Windows &gt; Windows universel &gt; Composant Windows Runtime (Windows universel)**
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
            this.backgroundTaskDeferral = taskInstance.GetDeferral(); // Get a deferral so that the service isn&#39;t terminated.
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

    **OnTaskCanceled()** est appelé lorsque la tâche est annulée. La tâche est annulée lorsque l’application cliente supprime la classe [**AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704), l’application cliente est suspendue, le système d’exploitation est arrêté ou mis en veille, ou le système d’exploitation manque de ressources pour exécuter la tâche.

## Écrire le code du service d’application


**OnRequestedReceived()** est l’emplacement du code du service d’application. Remplacez le stub **OnRequestedReceived()** dans le fichier Class1.cs de MyAppService par le code de cet exemple. Ce code obtient un index pour un article en stock et le transmet au service avec une chaîne de commande pour récupérer le nom et le prix de l’article en stock spécifié. Le code de gestion des erreurs a été supprimé par souci de concision.

```cs
private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below to respond to the message
    // and we don&#39;t want this call to get cancelled while we are waiting.
    var messageDeferral = args.GetDeferral();

    ValueSet message = args.Request.Message;
    ValueSet returnData = new ValueSet();

    string command = message["Command"] as string;
    int? inventoryIndex = message["ID"] as int?;

    if ( inventoryIndex.HasValue &amp;&amp;
         inventoryIndex.Value >= 0 &amp;&amp;
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
    messageDeferral.Complete(); // Complete the deferral so that the platform knows that we&#39;re done responding to the app service call.
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

-   Un moyen d’obtenir le nom de famille du package de l’application de service d’application consiste à appeler [**Windows.ApplicationModel.Package.Current.Id.FamilyName**](https://msdn.microsoft.com/library/windows/apps/br224670) à partir du projet **AppServiceProvider** (par exemple, depuis `public App()` dans App.xaml.cs) et à noter le résultat. Pour exécuter AppServiceProvider dans Microsoft Visual Studio, définissez-le en tant que projet de démarrage dans la fenêtre Explorateur de solutions et exécutez le projet.
-   Un autre moyen d’obtenir le nom de famille du package consiste à déployer la solution **Générer &gt; Déployer la solution**) et à noter le nom complet du package dans la fenêtre de sortie (**Afficher &gt; Sortie**). Vous devez supprimer les informations de plateforme de la chaîne dans la fenêtre de sortie pour obtenir le nom du package. Par exemple, si le nom complet du package indiqué dans la fenêtre de sortie était « 9fe3058b-3de0-4e05-bea7-84a06f0ee4f0\_1.0.0.0\_x86\_\_yd7nk54bq29ra », vous extrairiez « 1.0.0.0\_x86\_\_ » et laisseriez « 9fe3058b-3de0-4e05-bea7-84a06f0ee4f0\_yd7nk54bq29ra » comme nom de la famille de packages.

## Écrire un client pour appeler le service d’application


1.  Ajoutez un nouveau projet d’application universelle Windows vide nommé ClientApp à la solution (**Fichier &gt; Ajouter &gt; Nouveau projet**). (Dans la boîte de dialogue **Ajouter un nouveau projet**, choisissez **Installé &gt; Autres langages &gt; Visual C# &gt; Windows &gt; Windows universel &gt; Application vide (Windows universel)**).
2.  Dans le projet ClientApp, ajoutez l’instruction **using** suivante au début du fichier MainPage.xaml.cs :
    ```cs
    >using Windows.ApplicationModel.AppService;
    ```

3.  Ajoutez une zone de texte et un bouton à MainPage.xaml.
4.  Ajoutez un gestionnaire d’événements de clic pour le bouton et ajoutez le mot-clé **async** à la signature du gestionnaire du bouton.
5.  Remplacez le stub de votre gestionnaire d’événement de clic pour le bouton par le code suivant. Veillez à inclure la déclaration de champ `inventoryService`.

   ```cs
   private AppServiceConnection inventoryService;
    private async void button_Click(object sender, RoutedEventArgs e)
    {
        // Add the connection.
        if (this.inventoryService == null)
        {
            this.inventoryService = new AppServiceConnection();

            // Here, we use the app service name defined in the app service provider&#39;s Package.appxmanifest file in the &lt;Extension&gt; section. 
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

    Replace the package family name in the line `this.inventoryService.PackageFamilyName = "replace with the package family name";` with the package family name of the **AppServiceProvider** project that you obtained in \[Step 5: Deploy the service app and get the package family name\].

    The code first establishes a connection with the app service. The connection will remain open until you dispose **this.inventoryService**. The app service name must match the **AppService Name** attribute that you added to the AppServiceProvider project's Package.appxmanifest file. In this example, it is `<uap:AppService Name="com.microsoft.inventory"/>`.

    A [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) named **message** is created to specify the command that we want to send to the app service. The example app service expects a command to indicate which of two actions to take. We get the index from the textbox in the ClientApp, and then call the service with the "Item" command to get the description of the item. Then, we make the call with the "Price" command to get the item's price. The button text is set to the result.

    Because [**AppServiceResponseStatus**](https://msdn.microsoft.com/library/windows/apps/dn921724) only indicates whether the operating system was able to connect the call to the app service, we check the "Status" key in the [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) we receive from the app service to ensure that it was able to fulfill the request.

6.  In Visual Studio, set the ClientApp project to be the startup project in the Solution Explorer window and run the solution. Enter the number 1 into the text box and click the button. You should get "Chair : Price = 88.99" back from the service.

    ![sample app displaying chair price=88.99](images/appserviceclientapp.png)

If the app service call fails, check the following in the ClientApp:

1.  Verify that the package family name assigned to the inventory service connection matches the package family name of the AppServiceProvider app. See: **button\_Click()**`this.inventoryService.PackageFamilyName = "...";`).
2.  In **button\_Click()**, verify that the app service name that is assigned to the inventory service connection matches the app service name in the AppServiceProvider's Package.appxmanifest file. See: `this.inventoryService.AppServiceName = "com.microsoft.inventory";`.
3.  Ensure that the AppServiceProvider app has been deployed (In the Solution Explorer, right-click the solution and choose **Deploy**).

## Debug the app service


1.  Ensure that the entire solution is deployed before debugging because the app service provider app must be deployed before the service can be called. (In Visual Studio, **Build &gt; Deploy Solution**).
2.  In the Solution Explorer, right-click the AppServiceProvider project and choose **Properties**. From the **Debug** tab, change the **Start action** to **Do not launch, but debug my code when it starts**.
3.  In the MyAppService project, in the Class1.cs file, set a breakpoint in OnRequestReceived().
4.  Set the AppServiceProvider project to be the startup project and press F5.
5.  Start ClientApp from the Start menu (not from Visual Studio).
6.  Enter the number 1 into the text box and press the button. The debugger will stop in the app service call on the breakpoint in your app service.

## Debug the client


1.  Follow the instructions in the preceding step to debug the app service.
2.  Launch ClientApp from the Start menu.
3.  Attach the debugger to the ClientApp.exe process (not the ApplicationFrameHost.exe process). (In Visual Studio, choose **Debug &gt; Attach to Process...**.)
4.  In the ClientApp project, set a breakpoint in **button\_Click()**.
5.  The breakpoints in both the client and the app service will now be hit when you enter the number 1 into the text box of the ClientApp and click the button.

## Remarks


This example provides a simple introduction to creating an app service and calling it from another app. The key things to note are the creation of a background task to host the app service, the addition of the windows.appservice extension to the app service provider app's Package.appxmanifest file, obtaining the package family name of the app service provider app so that we can connect to it from the client app, and using [**Windows.ApplicationModel.AppService.AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704) to call the service.

## Full code for MyAppService


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
            this.backgroundTaskDeferral = taskInstance.GetDeferral(); // Get a deferral so that the service isn&#39;t terminated.
            taskInstance.Canceled += OnTaskCanceled; // Associate a cancellation handler with the background task.

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // Get a deferral because we use an awaitable API below to respond to the message
            // and we don&#39;t want this call to get cancelled while we are waiting.
            var messageDeferral = args.GetDeferral();

            ValueSet message = args.Request.Message;
            ValueSet returnData = new ValueSet();

            string command = message["Command"] as string;
            int? inventoryIndex = message["ID"] as int?;

            if (inventoryIndex.HasValue &amp;&amp;
                 inventoryIndex.Value >= 0 &amp;&amp;
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
            messageDeferral.Complete(); // Complete the deferral so that the platform knows that we&#39;re done responding to the app service call.
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

## Articles connexes


* [Prendre en charge votre application avec des tâches en arrière-plan](support-your-app-with-background-tasks.md)

 

 





<!--HONumber=Mar16_HO1-->


