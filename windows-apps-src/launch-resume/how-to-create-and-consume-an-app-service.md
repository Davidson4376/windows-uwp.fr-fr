---
title: Créer et utiliser un service d’application
description: Apprenez à écrire une application de plateforme Windows universelle (UWP) pouvant fournir des services à d’autres applications UWP et à utiliser ces derniers.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: communication de l’application à l’application, la communication entre processus, IPC, arrière-plan arrière-plan communication, une application vers, app service, de messagerie
ms.date: 01/16/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 96ecad8494ad82dc4927b3675ad322f07a8ca7f3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643574"
---
# <a name="create-and-consume-an-app-service"></a>Créer et utiliser un service d’application

Les services d’application sont des applications UWP qui fournissent des services à d’autres applications UWP. Ils sont semblables aux services web, sur un appareil. Un service d’application s’exécute sous forme de tâche en arrière-plan dans l’application hôte et peut fournir son service à d’autres applications. Un service d’application peut par exemple fournir un service de scanneur de code-barres que d’autres applications peuvent utiliser. Il peut également s’agir d’une suite d’applications d’entreprise partageant un service d’application de vérification orthographique, accessible aux autres applications de la suite.  Les services d’application vous permettent de créer des services sans interface utilisateur, que les applications peuvent appeler sur le même appareil et, à partir de Windows 10 version 1607, sur des appareils distants.

À partir de Windows 10 version 1607, vous pouvez créer des services d’application qui s’exécutent dans le même processus que l’application hôte. Cet article porte sur la création et la consommation d'un service d’application qui s’exécute dans un processus en arrière-plan distinct. Voir [Convertir un service d’application pour qu’il s’exécute dans le même processus que son application hôte](convert-app-service-in-process.md) pour plus d’informations sur l’exécution d’un service d’application qui s’exécute dans le même processus que le fournisseur.

Pour obtenir un exemple de code de service d’application, voir [Exemples d’applications de plateforme Windows universelle (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices).

## <a name="create-a-new-app-service-provider-project"></a>Créer un projet de fournisseur de service d’application

Dans la procédure décrite ici, nous allons créer tous les éléments dans une seule solution par souci de simplicité.

1. Dans Visual Studio 2015 ou version ultérieure, créez un projet d’application UWP et nommez-le **AppServiceProvider**.
    1. Sélectionnez **fichier > Nouveau > projet...** 
    2. Dans le **nouveau projet** boîte de dialogue, sélectionnez **installé > Visual C# > application vide (Windows universel)**. Il s’agit de l’application qui fournit le service d’application à d’autres applications UWP.
    3. Nommez le projet **AppServiceProvider**, choisissez un emplacement pour celui-ci, puis cliquez sur **OK**.

2. Lorsque vous êtes invité à sélectionner un **cible** et **version minimale** pour le projet, sélectionnez au moins **10.0.14393**. Si vous souhaitez utiliser le nouveau **SupportsMultipleInstances** attribut, vous devez être à l’aide de Visual Studio 2017 et cible **10.0.15063** (**Windows 10 Creators Update**) ou une version ultérieure.

<span id="appxmanifest"/>

## <a name="add-an-app-service-extension-to-packageappxmanifest"></a>Ajouter une extension de service d’application Package.appxmanifest

Dans le **AppServiceProvider** projet, ouvrez le **Package.appxmanifest** fichier dans un éditeur de texte : 

1. Faites un clic droit dans le **l’Explorateur de solutions**. 
2. Sélectionnez **ouvrir avec**. 
3. Sélectionnez **éditeur XML (texte)**. 

Ajoutez le code suivant `AppService` extension à l’intérieur du `<Application>` élément. Cet exemple publie le service `com.microsoft.inventory`, qui identifie cette application en tant que fournisseur de service d’application. Le service proprement dit est implémenté sous forme de tâche en arrière-plan. Le projet de service d’application expose le service aux autres applications. Nous vous recommandons d’utiliser un style de nom de domaine inverse pour le nom du service.

Notez que le préfixe d’espace de noms `xmlns:uap4` et l’attribut `uap4:SupportsMultipleInstances` sont uniquement valides si vous ciblez SDK Windows version 10.0.15063 ou une version ultérieure. Vous pouvez les supprimer en toute sécurité si vous ciblez des versions du SDK antérieures.

``` xml
<Package
    ...
    xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    ...
    <Applications>
        <Application Id="AppServiceProvider.App"
          Executable="$targetnametoken$.exe"
          EntryPoint="AppServiceProvider.App">
          ...
          <Extensions>
            <uap:Extension Category="windows.appService" EntryPoint="MyAppService.Inventory">
              <uap3:AppService Name="com.microsoft.inventory" uap4:SupportsMultipleInstances="true"/>
            </uap:Extension>
          </Extensions>
          ...
        </Application>
    </Applications>
```

Le `Category` attribut identifie cette application comme fournisseur de service d’application.

Le `EntryPoint` attribut identifie la classe d’espace de noms qualifié qui implémente le service, nous allons implémenter ensuite.

Le `SupportsMultipleInstances` attribut indique que chaque fois que le service d’application est appelé et il doit s’exécuter dans un nouveau processus. Cela n’est pas obligatoire, mais est disponible pour vous si vous avez besoin de cette fonctionnalité et que vous ciblez le 10.0.15063 SDK (**Windows 10 Creators Update**) ou une version ultérieure. Il doit également être précédé par l’espace de noms `uap4`.

## <a name="create-the-app-service"></a>Créer le service d’application

1.  Un service d’application peut être implémenté sous forme de tâche en arrière-plan. Cela permet à une application au premier plan d’appeler un service d’application dans une autre application. Pour créer un service d’application comme une tâche en arrière-plan, ajoutez un nouveau projet de composant d’exécution de Windows à la solution (**fichier &gt; ajouter &gt; nouveau projet**) nommé **MyAppService**. Dans le **ajouter un nouveau projet** boîte de dialogue, sélectionnez **installé > Visual C# > composant d’exécution de Windows (Windows universel)**.
2.  Dans le **AppServiceProvider** de projet, ajoutez une référence de projet à projet vers le nouveau **MyAppService** projet (dans le **l’Explorateur de solutions**, avec le bouton droit sur le  **AppServiceProvider** projet > **ajouter** > **référence** > **projets**  >   **Solution**, sélectionnez **MyAppService** > **OK**). Cette étape est critique, car si vous n’ajoutez pas la référence, le service d’application ne se connecte pas lors de l’exécution.
3.  Dans le **MyAppService** de projet, ajoutez le code suivant **à l’aide de** instructions au début de **Class1.cs**:
    ```cs
    using Windows.ApplicationModel.AppService;
    using Windows.ApplicationModel.Background;
    using Windows.Foundation.Collections;
    ```

4.  Renommer **Class1.cs** à **Inventory.cs**et remplacez le code stub pour **Class1** avec une nouvelle classe de tâche en arrière-plan nommé **inventaire**:

    ```cs
    public sealed class Inventory : IBackgroundTask
    {
        private BackgroundTaskDeferral backgroundTaskDeferral;
        private AppServiceConnection appServiceconnection;
        private String[] inventoryItems = new string[] { "Robot vacuum", "Chair" };
        private double[] inventoryPrices = new double[] { 129.99, 88.99 };

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Get a deferral so that the service isn't terminated.
            this.backgroundTaskDeferral = taskInstance.GetDeferral();

            // Associate a cancellation handler with the background task.
            taskInstance.Canceled += OnTaskCanceled;

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // This function is called when the app service receives a request.
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

    **Exécutez** est appelée lorsque la tâche d’arrière-plan est créée. Comme les tâches en arrière-plan sont arrêtées une fois **Run** terminé, le code effectue un report afin que la tâche en arrière-plan reste active pour traiter les demandes. Un service d’application qui est implémenté comme une tâche en arrière-plan reste actif pendant environ 30 secondes après avoir reçu un appel, sauf si elle est appelée à nouveau dans cette fenêtre de temps ou d’un report est mis hors service. Si le service d’application est implémenté dans le même processus que l’appelant, la durée de vie de l’app service est liée à la durée de vie de l’appelant.

    La durée de vie du service d’application dépend de l’appelant :

    * Si l’appelant est au premier plan, la durée de vie de l’application de service est identique à l’appelant.
    * Si l’appelant est en arrière-plan, le service de l’application obtient 30 secondes à s’exécuter. Un report offre une fois 5 secondes supplémentaires.

    **OnTaskCanceled** est appelée lorsque la tâche est annulée. La tâche est annulée lorsque l’application cliente supprime le [AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/dn921704), l’application cliente est suspendue, le système d’exploitation est arrêté ou est en veille ou le système d’exploitation manque de ressources pour exécuter la tâche.

## <a name="write-the-code-for-the-app-service"></a>Écrire le code du service d’application

**OnRequestReceived** ici le code pour le service d’application. Remplacer le stub **OnRequestReceived** dans **MyAppService**de **Inventory.cs** avec le code de cet exemple. Ce code obtient un index pour un article en stock et le transmet au service avec une chaîne de commande pour récupérer le nom et le prix de l’article en stock spécifié. Pour vos propres projets, ajoutez le code de gestion des erreurs.

```cs
private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below to respond to the message
    // and we don't want this call to get canceled while we are waiting.
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

    try
    {
        // Return the data to the caller.
        await args.Request.SendResponseAsync(returnData);
    }
    catch (Exception e)
    {
        // Your exception handling code here.
    }
    finally
    {
        // Complete the deferral so that the platform knows that we're done responding to the app service call.
        // Note for error handling: this must be called even if SendResponseAsync() throws an exception.
        messageDeferral.Complete();
    }
}
```

Notez que **OnRequestReceived** est **async** , car nous faites appel à un élément pouvant être attendu de la méthode [SendResponseAsync](https://msdn.microsoft.com/library/windows/apps/dn921722) dans cet exemple.

Un report est nécessaire afin que le service peut utiliser **async** méthodes dans le **OnRequestReceived** gestionnaire. Cela permet de s’assurer que l’appel à **OnRequestReceived** ne se termine pas avant la fin du traitement du message.  [SendResponseAsync](https://msdn.microsoft.com/library/windows/apps/dn921722) envoie le résultat à l’appelant. **SendResponseAsync** n’indique pas l’achèvement de l’appel. Il s’agit de l’achèvement de report qui signale à [SendMessageAsync](https://msdn.microsoft.com/library/windows/apps/dn921712) qui **OnRequestReceived** est terminée. L’appel à **SendResponseAsync** est encapsulée dans un bloc try/finally, car vous devez effectuer au report, même si **SendResponseAsync** lève une exception.

Utilisation d’App services [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131) objets pour échanger des informations. La taille des données que vous pouvez transmettre est limitée uniquement par les ressources système. Il n’y a aucune clé prédéfinie utilisable dans votre classe **ValueSet**. Vous devez déterminer quelles valeurs de clé vous allez utiliser pour définir le protocole de votre service d’application. L’appelant doit être écrit avec ce protocole à l’esprit. Dans cet exemple, nous avons choisi une clé nommée `Command` dont la valeur indique si nous voulons que le service d’application fournisse le nom de l’article en stock ou son prix. L’index du nom d’inventaire est stocké sous la clé `ID`. La valeur renvoyée est stockée sous la clé `Result`.

Une énumération [AppServiceClosedStatus](https://msdn.microsoft.com/library/windows/apps/dn921703) est renvoyée à l’appelant pour indiquer si l’appel au service d’application a réussi ou échoué. L’appel au service d’application peut échouer par exemple si le système d’exploitation abandonne le point de terminaison du service ou si les ressources sont dépassées. Vous pouvez renvoyer des informations d’erreur supplémentaires via la classe [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131). Dans cet exemple, nous utilisons une clé nommée `Status` pour renvoyer des informations plus détaillées sur l’erreur à l’appelant.

L’appel à [SendResponseAsync](https://msdn.microsoft.com/library/windows/apps/dn921722) renvoie la classe [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131) à l’appelant.

## <a name="deploy-the-service-app-and-get-the-package-family-name"></a>Déployer l’application de service et obtenir le nom de la famille de packages

Le fournisseur de services d’application doit être déployé avant de pouvoir l’appeler à partir d’un client. Vous pouvez le déployer en sélectionnant **Générer > déployer la Solution** dans Visual Studio.

Vous devez également le nom de famille de packages du fournisseur de services d’application pour pouvoir pour l’appeler. Vous pouvez l’obtenir en ouvrant le **AppServiceProvider** du projet **Package.appxmanifest** fichier en mode concepteur (double-cliquez dessus dans le **l’Explorateur de solutions**). Sélectionnez le **empaquetage** onglet, copiez la valeur en regard **nom de famille de Package**et le coller quelque part tel que le bloc-notes pour l’instant.

## <a name="write-a-client-to-call-the-app-service"></a>Écrire un client pour appeler le service d’application

1.  Ajoutez un nouveau projet d’application universelle Windows vide à la solution à l’aide de **Fichier &gt; Ajouter &gt; Nouveau projet**. Dans le **ajouter un nouveau projet** boîte de dialogue, sélectionnez **installé > Visual C# > application vide (Windows universel)** et nommez-le **ClientApp**.

2.  Dans le **ClientApp** de projet, ajoutez le code suivant **à l’aide de** instruction au début du **MainPage.xaml.cs**:
    ```cs
    using Windows.ApplicationModel.AppService;
    ```

3.  Ajouter une zone de texte appelée **zone de texte** et un bouton pour **MainPage.xaml**.

4.  Ajoutez un bouton Gestionnaire click pour le bouton appelé **button_Click**et ajoutez le mot clé **async** à la signature du Gestionnaire de bouton.

5. Remplacez le stub de votre gestionnaire d’événement de clic pour le bouton par le code suivant. Veillez à inclure la déclaration de champ `inventoryService`.
    ```cs
   private AppServiceConnection inventoryService;

   private async void button_Click(object sender, RoutedEventArgs e)
   {
       // Add the connection.
       if (this.inventoryService == null)
       {
           this.inventoryService = new AppServiceConnection();

           // Here, we use the app service name defined in the app service 
           // provider's Package.appxmanifest file in the <Extension> section.
           this.inventoryService.AppServiceName = "com.microsoft.inventory";

           // Use Windows.ApplicationModel.Package.Current.Id.FamilyName 
           // within the app service provider to get this value.
           this.inventoryService.PackageFamilyName = "Replace with the package family name";

           var status = await this.inventoryService.OpenAsync();

           if (status != AppServiceConnectionStatus.Success)
           {
               textBox.Text= "Failed to connect";
               this.inventoryService = null;
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
           // Get the data  that the service sent to us.
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

       textBox.Text = result;
   }
   ```
    
    Remplacez le nom de la famille de packages de la ligne `this.inventoryService.PackageFamilyName = "Replace with the package family name";` par le nom de la famille de packages du projet **AppServiceProvider** que vous avez obtenu dans [Déployer l’application de service et obtenir le nom de la famille de packages](#deploy-the-service-app-and-get-the-package-family-name).

    > [!NOTE]
    > Veillez à coller dans le littéral de chaîne, plutôt que dans une variable. Il ne fonctionnera pas si vous utilisez une variable.

    Le code établit tout d’abord une connexion avec le service d’application. La connexion reste alors ouverte jusqu’à ce que vous supprimiez `this.inventoryService`. Le nom de service d’application doit correspondre à la `AppService` l’élément `Name` attribut que vous avez ajouté à la **AppServiceProvider** du projet **Package.appxmanifest** fichier. Dans cet exemple, il s’agit de `<uap3:AppService Name="com.microsoft.inventory"/>`.

    Un [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131) nommé `message` est créé pour spécifier la commande que nous souhaitons envoyer au service de l’application. L’exemple de service d’application attend une commande pour indiquer laquelle des deux actions effectuer. Nous obtenir l’index de la zone de texte dans l’application cliente, puis appelez le service avec le `Item` commande pour obtenir la description de l’élément. Ensuite, nous effectuons l’appel avec la commande `Price` afin d’obtenir le prix de l’article. Le texte du bouton est défini en fonction du résultat.

    Étant donné que [AppServiceResponseStatus](https://msdn.microsoft.com/library/windows/apps/dn921724) indique uniquement si le système d’exploitation a pu se connecter à l’appel au service d’application, nous vérifions le `Status` clé dans le [ValueSet](https://msdn.microsoft.com/library/windows/apps/dn636131) que nous recevons de l’application service pour vous assurer qu’il a été en mesure de répondre à la demande.

6. Définir le **ClientApp** projet comme projet de démarrage (faites un clic droit dans le **l’Explorateur de solutions** > **définir comme projet de démarrage**) et exécuter la solution. Entrez le chiffre 1 dans la zone de texte et cliquez sur le bouton. Vous devez obtenir « CHAISE : Prix = 88.99" à partir du service.

    ![exemple d’application affichant chair price=88.99](images/appserviceclientapp.png)

Si l’appel de service d’application échoue, vérifiez les éléments suivants le **ClientApp** projet :

1.  Vérifiez que le nom de famille de packages affecté à la connexion de service d’inventaire correspond au nom de famille de packages de la **AppServiceProvider** application. Voir la ligne de **bouton\_cliquez sur** avec `this.inventoryService.PackageFamilyName = "...";`.
2.  Dans **bouton\_cliquez sur**, vérifiez que le nom de service d’application qui est affecté à la connexion de service d’inventaire correspond le nom de service d’application dans le **AppServiceProvider**de  **Package.appxmanifest** fichier. Voir `this.inventoryService.AppServiceName = "com.microsoft.inventory";`.
3.  Vérifiez que le **AppServiceProvider** application a été déployée. (Dans le **l’Explorateur de solutions**, avec le bouton droit de la solution et choisissez **déployer la Solution**).

## <a name="debug-the-app-service"></a>Déboguer le service d’application

1.  Assurez-vous que la solution est déployée dans son intégralité avant le débogage, car l’application qui fournit le service d’application doit être déployée avant que le service puisse être appelé. (Dans Visual Studio, **Générer &gt; Déployer la solution**).
2.  Dans le **l’Explorateur de solutions**, avec le bouton droit le **AppServiceProvider** de projet et choisissez **propriétés**. Dans l’onglet **Déboguer**, définissez **Action de démarrage** sur **Ne pas lancer, mais déboguer mon code au démarrage**. (Notez que si vous utilisez C++ pour implémenter votre fournisseur de services d’application, dans l’onglet **Débogage**, vous devez modifier **Lancer l’application** sur **Non**).
3.  Dans le **MyAppService** de projet, dans le **Inventory.cs** de fichier, définir un point d’arrêt **OnRequestReceived**.
4.  Définir le **AppServiceProvider** projet qui sera le projet de démarrage et appuyez sur **F5**.
5.  Démarrer **ClientApp** dans le menu Démarrer (pas à partir de Visual Studio).
6.  Entrez le chiffre 1 dans la zone de texte et appuyez sur le bouton. Le débogueur s’arrête dans l’appel au service d’application sur le point d’arrêt défini dans votre service d’application.

## <a name="debug-the-client"></a>Déboguer le client

1.  Pour déboguer le client qui appelle le service d’application, suivez les instructions de l’étape précédente.
2.  Lancez **ClientApp** dans le menu Démarrer.
3.  Attacher le débogueur à la **ClientApp.exe** processus (pas le **ApplicationFrameHost.exe** processus). (Dans Visual Studio, choisissez **Déboguer &gt; Attacher au processus…**.)
4.  Dans le **ClientApp** de projet, définissez un point d’arrêt dans **bouton\_cliquez sur**.
5.  Les points d’arrêt dans le client et le service d’application seront désormais atteints lorsque vous entrez le numéro 1 dans la zone de texte de **ClientApp** et cliquez sur le bouton.

## <a name="general-app-service-troubleshooting"></a>Résolution des problèmes du service d’application général

Si vous rencontrez un **AppUnavailable** état après avoir essayé de vous connecter à un service d’application, vérifiez les éléments suivants :

- Assurez-vous que le projet de fournisseur de services d’application et le projet de service d’application sont déployés. Les deux doivent être déployés avant d’exécuter le client sans quoi ce dernier ne pourra se connecter à aucun élément. Vous pouvez effectuer un déploiement à partir de Visual Studio à l’aide de **Générer** > **Déployer la solution**.
- Dans le **l’Explorateur de solutions**, assurez-vous que votre projet de fournisseur de service application dispose d’une référence de projet à projet au projet qui implémente le service d’application.
- Vérifiez que le `<Extensions>` entrée et ses éléments enfants, ont été ajoutés à la **Package.appxmanifest** fichier appartenant au projet du fournisseur d’application de service comme indiqué ci-dessus dans [ajouter une extension de service d’application pour Package.appxmanifest](#appxmanifest).
- Vérifiez que le [AppServiceConnection.AppServiceName](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.appservicename) chaîne dans votre client qui appelle le fournisseur de services d’application correspond à la `<uap3:AppService Name="..." />` spécifié dans le service fournisseur du projet d’application **Package.appxmanifest**  fichier.
- Vérifiez que le [AppServiceConnection.PackageFamilyName](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.packagefamilyname) correspond au nom de famille de package l’application fournisseur du composant de service comme indiqué ci-dessus dans [ajouter une extension de service d’application Package.appxmanifest](#appxmanifest)
- Pour les services d’application d’out-of-proc tel que celui dans cet exemple, vérifier que le `EntryPoint` spécifié dans le `<uap:Extension ...>` élément de votre service fournisseur du projet d’application **Package.appxmanifest** fichier correspond à l’espace de noms et nom de la classe de la classe publique qui implémente [IBackgroundTask](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask) dans votre projet de service d’application.

### <a name="troubleshoot-debugging"></a>Résolution des problèmes de débogage

Si le débogueur ne s’arrête pas aux points d’arrêt de votre fournisseur de services d’application ou de vos projets de service d’application, vérifiez les éléments suivants :

- Assurez-vous que le projet de fournisseur de services d’application et le projet de service d’application sont déployés. Les deux doivent être déployées avant d’exécuter le client. Vous pouvez les déployer à partir de Visual Studio à l’aide de **Générer** > **Déployer la solution**.
- Assurez-vous que le projet que vous souhaitez déboguer est défini comme projet de démarrage et que les propriétés de débogage pour ce projet sont définies de ne pas exécuter le projet lorsque **F5** est enfoncé. Cliquez avec le bouton droit sur le projet, puis cliquez sur **Propriétés**, puis sur **Déboguer** (ou **Débogage** en langage C++). En C#, définissez **Action de démarrage** sur **Ne pas lancer, mais déboguer mon code au démarrage**. En langage C++, définissez **Lancer l’application** sur **Non**.

## <a name="remarks"></a>Notes

Cet exemple offre une introduction simple à la création d’un service d’application qui s’exécute sous forme de tâche en arrière-plan et à l’appel de ce service à partir d’une autre application. Les points clés à noter sont :

* Créer une tâche en arrière-plan pour héberger le service d’application.
* Ajouter le `windows.appService` extension pour le fournisseur de services application **Package.appxmanifest** fichier.
* Obtenir le nom de famille de packages du fournisseur de services d’application afin que nous pouvons nous connecter à celui-ci à partir de l’application cliente.
* Ajoutez une référence de projet à projet à partir du projet de fournisseur de service application au projet de service d’application.
* Utilisez [Windows.ApplicationModel.AppService.AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/dn921704) pour appeler le service.

## <a name="full-code-for-myappservice"></a>Code complet pour MyAppService

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
            // Get a deferral so that the service isn't terminated.
            this.backgroundTaskDeferral = taskInstance.GetDeferral();

            // Associate a cancellation handler with the background task.
            taskInstance.Canceled += OnTaskCanceled;

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // Get a deferral because we use an awaitable API below to respond to the message
            // and we don't want this call to get canceled while we are waiting.
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

            // Return the data to the caller.
            await args.Request.SendResponseAsync(returnData);

            // Complete the deferral so that the platform knows that we're done responding to the app service call.
            // Note for error handling: this must be called even if SendResponseAsync() throws an exception.
            messageDeferral.Complete();
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

## <a name="related-topics"></a>Rubriques connexes

* [Convertir un service d’application à exécuter dans le même processus que son application hôte](convert-app-service-in-process.md)
* [Prendre en charge votre application avec des tâches en arrière-plan](support-your-app-with-background-tasks.md)
* [Exemple de code d’application service (C#, C++ et VB)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)
