---
author: TylerMSFT
title: Créer et utiliser un service d’application
description: Apprenez à écrire une application de plateforme Windows universelle (UWP) pouvant fournir des services à d’autres applications UWP et à utiliser ces derniers.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: une application à la communication, communication interprocessus, IPC, messagerie, service d’application pour une application, en arrière-plan, la communication d’arrière-plan
ms.author: twhitney
ms.date: 09/18/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7475ae8db964b23de89488d883c135158ea20e74
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2018
ms.locfileid: "4461715"
---
# <a name="create-and-consume-an-app-service"></a>Créer et utiliser un service d’application

Les services d’application sont des applications UWP qui fournissent des services à d’autres applications UWP. Ils sont semblables aux services web, sur un appareil. Un service d’application s’exécute sous forme de tâche en arrière-plan dans l’application hôte et peut fournir son service à d’autres applications. Un service d’application peut par exemple fournir un service de scanneur de code-barres que d’autres applications peuvent utiliser. Il peut également s’agir d’une suite d’applications d’entreprise partageant un service d’application de vérification orthographique, accessible aux autres applications de la suite.  Les services d’application vous permettent de créer des services sans interface utilisateur, que les applications peuvent appeler sur le même appareil et, à partir de Windows10 version1607, sur des appareils distants. 

À partir de Windows10 version1607, vous pouvez créer des services d’application qui s’exécutent dans le même processus que l’application hôte. Cet article porte sur la création et la consommation d'un service d’application qui s’exécute dans un processus en arrière-plan distinct. Voir [Convertir un service d’application pour qu’il s’exécute dans le même processus que son application hôte](convert-app-service-in-process.md) pour plus d’informations sur l’exécution d’un service d’application qui s’exécute dans le même processus que le fournisseur.

Pour obtenir un exemple de code de service d’application, voir [Exemples d’applications de plateforme Windows universelle (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices).

## <a name="create-a-new-app-service-provider-project"></a>Créer un projet de fournisseur de service d’application

Dans la procédure décrite ici, nous allons créer tous les éléments dans une seule solution par souci de simplicité.

-   Dans Microsoft Visual Studio, créez un projet d’application UWP et nommez-le **AppServiceProvider**. (Dans la boîte de dialogue **Nouveau projet**, sélectionnez **Modèles &gt; Autres langages &gt; Visual C# &gt; Windows &gt; Windows universel &gt; Application vide [Windows universel]**). Il s’agit de l’application qui fournit le service d’application à d’autres applications UWP.
-   Lorsque vous êtes invité à sélectionner un **Version cible** pour le projet, sélectionnez au moins **10.0.14393**. Si vous souhaitez utiliser le nouvel attribut `SupportsMultipleInstances`, vous devez utiliser Visual Studio2017 et la cible **10.0.15063** (**Windows 10 Creators Update**) ou une version ultérieure.

<span id="appxmanifest"/>

## <a name="add-an-app-service-extension-to-packageappxmanifest"></a>Ajouter une extension de service d’application à package.appxmanifest

Dans le fichier Package.appxmanifest du projet AppServiceProvider, ajoutez l’extension AppService suivante à l’élément `&lt;Application&gt;`. Cet exemple publie le service `com.Microsoft.Inventory`, qui identifie cette application en tant que fournisseur de services d’application. Le service proprement dit est implémenté sous forme de tâche en arrière-plan. Le projet de service d’application expose le service aux autres applications. Nous vous recommandons d’utiliser un style de nom de domaine inverse pour le nom du service.

Notez que le préfixe d’espace de noms `xmlns:uap4` et l’attribut `uap4:SupportsMultipleInstances` sont uniquement valides si vous ciblez SDK Windows version10.0.15063 ou une version ultérieure. Vous pouvez les supprimer en toute sécurité si vous ciblez des versions du SDK antérieures.

``` xml
<Package
    ...
    xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    ...
    <Applications>
        <Application Id="AppServicesProvider.App"
          Executable="$targetnametoken$.exe"
          EntryPoint="AppServicesProvider.App">
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

L’attribut **Category** identifie cette application en tant que fournisseur de services d’application.

L’attribut **EntryPoint** identifie la classe qualifiée par un espace de noms qui implémente le service, que nous allons implémenter à l’étape suivante.

L’attribut **SupportsMultipleInstances** indique qu’à chaque fois que le service d’application est appelé, il doit s’exécuter dans un nouveau processus. Cela n’est pas obligatoire, mais est disponible si vous avez besoin de cette fonctionnalité et que vous ciblez le SDK `10.0.15063` (**Windows10 Creators Update**) ou une version ultérieure. Il doit également être précédé par l’espace de noms `uap4`.

## <a name="create-the-app-service"></a>Créer le service d’application

1.  Un service d’application peut être implémenté sous forme de tâche en arrière-plan. Cela permet à une application au premier plan d’appeler un service d’application dans une autre application. Pour créer un service d’application sous forme de tâche en arrière-plan, ajoutez un nouveau projet de composant Windows Runtime à la solution (**Fichier &gt;Ajouter &gt;Nouveau projet**) nommé MyAppService. (Dans la boîte de dialogue **Ajouter un nouveau projet**, choisissez **Installé &gt; Autres langages &gt; Visual C# &gt; Windows &gt; Windows universel &gt; Composant Windows Runtime (Windows universel)**.
2.  Dans le projet **AppServiceProvider**, ajoutez une référence de projet à projet au nouveau projet **MyAppService** (dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet **AppServiceProvider** > **Ajouter** > **Référence** > **Projets** > **Solution**, puis sélectionnez **MyAppService** > **OK**). Cette étape est critique, car si vous n’ajoutez pas la référence, le service d’application ne se connecte pas lors de l’exécution.
3.  Dans le projet MyappService, ajoutez les instructions **using** suivantes au début du fichier Class1.cs:
    ```cs
    using Windows.ApplicationModel.AppService;
    using Windows.ApplicationModel.Background;
    using Windows.Foundation.Collections;
    ```

4.  Remplacez le code stub pour **Class1** par une nouvelle classe de tâche en arrière-plan nommée **Inventory**:

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
            // This function is called when the app service receives a request
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

    **Run()** est appelé lorsque la tâche en arrière-plan est créée. Comme les tâches en arrière-plan sont arrêtées une fois **Run** terminé, le code effectue un report afin que la tâche en arrière-plan reste active pour traiter les demandes. Un service d’application implémenté sous forme de tâche en arrière-plan reste actif pendant environ 30secondes après avoir reçu un appel, sauf s’il est appelé à nouveau au cours de cette période ou si un rapport est effectué. Si le service d’application est implémenté dans le même processus que l’appelant, sa durée de vie est liée à celle de l’appelant.

    La durée de vie du service d’application dépend de l’appelant:

    1. Si l’appelant est au premier plan, la durée de vie du service d’application est identique à celle de l’appelant.
    2. Si l’appelant est en arrière-plan, le service d’application obtient 30secondes pour s’exécuter. Un report offre une fois 5secondes supplémentaires.

    **OnTaskCanceled()** est appelé lorsque la tâche est annulée. La tâche est annulée lorsque l’application cliente supprime la classe [**AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704), l’application cliente est suspendue, le système d’exploitation est arrêté ou mis en veille ou lorsque le système d’exploitation manque de ressources pour exécuter la tâche.

## <a name="write-the-code-for-the-app-service"></a>Écrire le code du service d’application

**OnRequestReceived()** est l’emplacement du code du service d’application. Remplacez le stub **OnRequestReceived()** dans le fichier Class1.cs de MyAppService par le code de cet exemple. Ce code obtient un index pour un article en stock et le transmet au service avec une chaîne de commande pour récupérer le nom et le prix de l’article en stock spécifié. Pour vos propres projets, ajoutez le code de gestion des erreurs.

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

    try
    {
        await args.Request.SendResponseAsync(returnData); // Return the data to the caller.
    }
    catch (Exception e)
    {
        // your exception handling code here
    }
    finally
    {
        // Complete the deferral so that the platform knows that we're done responding to the app service call.
        // Note for error handling: this must be called even if SendResponseAsync() throws an exception.
        messageDeferral.Complete();
    }
}
```

Notez que **OnRequestReceived()** a pour valeur **async**, car nous effectuons un appel de méthode awaitable à [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) dans cet exemple.

Un report est effectué afin que le service puisse utiliser les méthodes **async** dans le gestionnaire OnRequestReceived. Cela permet de s’assurer que l’appel à **OnRequestReceived** ne se termine pas avant la fin du traitement du message.  [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) envoie le résultat à l’appelant. **SendResponseAsync** n’indique pas l'achèvement de l’appel. C’est l’achèvement du report qui indique à [**SendMessageAsync**](https://msdn.microsoft.com/library/windows/apps/dn921712) que **OnRequestReceived** est terminé. L’appel à **SendResponseAsync()** est encapsulé dans un bloc Try/Finally, car vous devez achever le report même si **SendResponseAsync()** lève une exception.

Les services d’application utilisent une classe [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) pour échanger des informations. La taille des données que vous pouvez transmettre est limitée uniquement par les ressources système. Il n’y a aucune clé prédéfinie utilisable dans votre classe **ValueSet**. Vous devez déterminer quelles valeurs de clé vous allez utiliser pour définir le protocole de votre service d’application. L’appelant doit être écrit avec ce protocole à l’esprit. Dans cet exemple, nous avons choisi une clé nommée `Command` dont la valeur indique si nous voulons que le service d’application fournisse le nom de l’article en stock ou son prix. L’index du nom d’inventaire est stocké sous la clé `ID`. La valeur renvoyée est stockée sous la clé `Result`.

Une énumération [**AppServiceClosedStatus**](https://msdn.microsoft.com/library/windows/apps/dn921703) est renvoyée à l’appelant pour indiquer si l’appel au service d’application a réussi ou échoué. L’appel au service d’application peut échouer par exemple si le système d’exploitation abandonne le point de terminaison du service ou si les ressources sont dépassées. Vous pouvez renvoyer des informations d’erreur supplémentaires via la classe [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131). Dans cet exemple, nous utilisons une clé nommée `Status` pour renvoyer des informations plus détaillées sur l’erreur à l’appelant.

L’appel à [**SendResponseAsync**](https://msdn.microsoft.com/library/windows/apps/dn921722) renvoie la classe [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) à l’appelant.

## <a name="deploy-the-service-app-and-get-the-package-family-name"></a>Déployer l’application de service et obtenir le nom de la famille de packages

L’application qui fournit le service d’application doit être déployée avant de pouvoir être appelée depuis un client. Vous avez également besoin du nom de famille du package de l’application de service d’application pour l’appeler.

Un moyen d’obtenir le nom de famille du package de l’application de service d’application consiste à appeler [**Windows.ApplicationModel.Package.Current.Id.FamilyName**](https://msdn.microsoft.com/library/windows/apps/br224670) à partir du projet **AppServiceProvider** (par exemple, depuis `public App()` dans App.xaml.cs) et à noter le résultat. Pour exécuter AppServiceProvider dans Microsoft VisualStudio, définissez-le en tant que projet de démarrage dans la fenêtre Explorateur de solutions, puis exécutez le projet.

Pour obtenir le nom de famille du package, vous pouvez également déployer la solution (**Générer &gt; Déployer la solution**) et noter le nom complet du package dans la fenêtre Sortie (**Affichage &gt; Sortie**). Vous devez supprimer les informations de plateforme de la chaîne dans la fenêtre Sortie pour obtenir le nom du package. Par exemple, si le nom complet du package indiqué dans la fenêtre de sortie était `Microsoft.SDKSamples.AppServicesProvider.CPP_1.0.0.0_x86__8wekyb3d8bbwe`, vous devez extraire `1.0.0.0\_x86\_\_" leaving "Microsoft.SDKSamples.AppServicesProvider.CPP_8wekyb3d8bbwe` en tant que nom de la famille de packages.

## <a name="write-a-client-to-call-the-app-service"></a>Écrire un client pour appeler le service d’application

1.  Ajoutez un nouveau projet d’application universelle Windows vide à la solution à l’aide de **Fichier &gt; Ajouter &gt; Nouveau projet**. Dans la boîte de dialogue **Ajouter un nouveau projet**, choisissez **Installé &gt; Autres langages &gt; Visual C# &gt; Windows &gt; Windows universel &gt; Application vide (Windows universel)** et nommez-le **ClientApp**.
2.  Dans le projet ClientApp, ajoutez l’instruction **using** suivante au début du fichier MainPage.xaml.cs:
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
               textBox.Text= "Failed to connect";
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

       textBox.Text = result;
   }
   ```
Remplacez le nom de la famille de packages de la ligne `this.inventoryService.PackageFamilyName = "replace with the package family name";` par le nom de la famille de packages du projet **AppServiceProvider** que vous avez obtenu dans [Déployer l’application de service et obtenir le nom de la famille de packages](#deploy-the-service-app-and-get-the-package-family-name).

Le code établit tout d’abord une connexion avec le service d’application. La connexion reste alors ouverte jusqu’à ce que vous supprimiez `this.inventoryService`. Le nom du service d’application doit correspondre à l’attribut **AppService Name** que vous avez ajouté au fichier Package.appxmanifest du projet AppServiceProvider. Dans cet exemple, il s’agit de `<uap:AppService Name="com.microsoft.inventory"/>`.

Un [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) nommé **message** est créé pour spécifier la commande que vous souhaitez envoyer au service d’application. L’exemple de service d’application attend une commande pour indiquer laquelle des deuxactions effectuer. Nous obtenons l’index de l’élément textbox de l’application cliente, puis appelons le service à l’aide de la commande `Item` pour obtenir la description de l’article. Ensuite, nous effectuons l’appel avec la commande `Price` afin d’obtenir le prix de l’article. Le texte du bouton est défini en fonction du résultat.

Comme [**AppServiceResponseStatus**](https://msdn.microsoft.com/library/windows/apps/dn921724) indique uniquement si le système d’exploitation a été en mesure de connecter l’appel au service d’application, nous vérifions la clé `Status` dans le [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131) que nous recevons à partir du service d’application pour nous assurer qu’il a pu répondre à la demande.

6.  Dans VisualStudio, définissez le projet ClientApp en tant que projet de démarrage dans la fenêtre Explorateur de solutions et exécutez la solution. Entrez le chiffre1 dans la zone de texte et cliquez sur le bouton. Le service doit renvoyer «Chair : Price = 88.99».

    ![exemple d’application affichant chair price=88.99](images/appserviceclientapp.png)

Si l’appel au service d’application échoue, vérifiez les éléments suivants dans ClientApp:

1.  Vérifiez que le nom de la famille de packages affecté à la connexion au service d’inventaire correspond au nom de la famille de packages de l’application AppServiceProvider. Voir **button\_Click()**`this.inventoryService.PackageFamilyName = "...";`).
2.  Dans **button\_Click()**, vérifiez que le nom de service d’application affecté à la connexion au service d’inventaire correspond au nom du service d’application dans le fichier Package.appxmanifest du projet AppServiceProvider. Voir `this.inventoryService.AppServiceName = "com.microsoft.inventory";`.
3.  Assurez-vous que l’application AppServiceProvider a été déployée (dans l’Explorateur de solutions, cliquez avec le bouton droit sur la solution et choisissez **Déployer**).

## <a name="debug-the-app-service"></a>Déboguer le service d’application

1.  Assurez-vous que la solution est déployée dans son intégralité avant le débogage, car l’application qui fournit le service d’application doit être déployée avant que le service puisse être appelé. (Dans Visual Studio, **Générer &gt; Déployer la solution**).
2.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet **AppServiceProvider** et choisissez **Properties**. Dans l’onglet **Déboguer**, définissez **Action de démarrage** sur **Ne pas lancer, mais déboguer mon code au démarrage**. (Notez que si vous utilisez C++ pour implémenter votre fournisseur de services d’application, dans l’onglet **Débogage**, vous devez modifier **Lancer l’application** sur **Non**).
3.  Dans le projet MyAppService, dans le fichier Class1.cs, définissez un point d’arrêt dans `OnRequestReceived()`.
4.  Définissez le projet AppServiceProvider en tant que projet de démarrage et appuyez sur F5.
5.  Lancez ClientApp depuis le menu Démarrer (et non depuis VisualStudio).
6.  Entrez le chiffre1 dans la zone de texte et appuyez sur le bouton. Le débogueur s’arrête dans l’appel au service d’application sur le point d’arrêt défini dans votre service d’application.

## <a name="debug-the-client"></a>Déboguer le client

1.  Pour déboguer le client qui appelle le service d’application, suivez les instructions de l’étape précédente.
2.  Lancez ClientApp depuis le menu Démarrer.
3.  Attachez le débogueur au processus ClientApp.exe (et non au processus ApplicationFrameHost.exe). (Dans Visual Studio, choisissez **Déboguer &gt; Attacher au processus…**.)
4.  Dans le projet ClientApp, définissez un point d’arrêt dans **button\_Click()**.
5.  Les points d’arrêt dans le client et le service d’application sont maintenant atteints lorsque vous entrez le chiffre1 dans la zone de texte de ClientApp et cliquez sur le bouton.

## <a name="general-app-service-troubleshooting"></a>Résolution des problèmes du service d’application général ##

Si vous rencontrez un état **AppUnavailable** après une tentative de connexion à un service d’application, vérifiez les éléments suivants:

- Assurez-vous que le projet de fournisseur de services d’application et le projet de service d’application sont déployés. Les deux doivent être déployés avant d’exécuter le client sans quoi ce dernier ne pourra se connecter à aucun élément. Vous pouvez effectuer un déploiement à partir de Visual Studio à l’aide de **Générer** > **Déployer la solution**.
- Dans l’Explorateur de solutions, assurez-vous que le projet de fournisseur de services d’application a une référence de projet à projet au projet qui implémente le service d’application.
- Vérifiez que l’entrée `<Extensions>` et ses éléments enfants ont été ajoutés au fichier Package.appxmanifest appartenant au projet de fournisseur de services d’application, comme indiqué ci-dessus dans [Ajouter une extension de service d’application à package.appxmanifest](#appxmanifest).
- Vérifiez que la chaîne `AppServiceConnection.AppServiceName` du client qui appelle le fournisseur de services d’application correspond au `<uap3:AppService Name="..." />`spécifié dans le fichier Package.appxmanifest du projet de fournisseur de services d’application.
- Vérifiez que le `AppServiceConnection.PackageFamilyName`correspond au nom de la famille de packages du composant de fournisseur de services d’application, comme indiqué ci-dessus dans [Ajouter une extension de service d’application à package.appxmanifest](#appxmanifest)
- Pour les services d’application hors processus tels que celui de cet exemple, vérifiez que le `EntryPoint` spécifié dans l’élément `<uap:Extension ...>` du fichier Package.appxmanifest du projet de fournisseur de services d’application correspond à l’espace de noms et au nom de la classe publique qui implémente `IBackgroundTask` dans le projet de service d’application.

### <a name="troubleshoot-debugging"></a>Résolution des problèmes de débogage

Si le débogueur ne s’arrête pas aux points d’arrêt de votre fournisseur de services d’application ou de vos projets de service d’application, vérifiez les éléments suivants:

- Assurez-vous que le projet de fournisseur de services d’application et le projet de service d’application sont déployés. Les deux doivent être déployées avant d’exécuter le client. Vous pouvez les déployer à partir de Visual Studio à l’aide de **Générer** > **Déployer la solution**.
- Vérifiez que le projet que vous souhaitez déboguer est défini en tant que projet de démarrage et que les propriétés de débogage de ce projet sont définies de manière à ne pas exécuter le projet lorsque vous appuyez sur F5. Cliquez avec le bouton droit sur le projet, puis cliquez sur **Propriétés**, puis sur **Déboguer** (ou **Débogage** en langage C++). En C#, définissez **Action de démarrage** sur **Ne pas lancer, mais déboguer mon code au démarrage**. En langage C++, définissez **Lancer l’application** sur **Non**.

## <a name="remarks"></a>Notes

Cet exemple offre une introduction simple à la création d’un service d’application qui s’exécute sous forme de tâche en arrière-plan et à l’appel de ce service à partir d’une autre application. Les points importants sont la création d’une tâche en arrière-plan pour héberger le service d’application, l’ajout de l’extension windows.appservice au fichier Package.appxmanifest de l’application qui fournit le service d’application, l’obtention du nom de la famille de packages de l’application qui fournit le service d’application pour pouvoir se connecter à cette dernière à partir de l’application cliente, l’ajout d’une référence de projet à projet à partir du projet de fournisseur de services d’application au projet de service d’application et l’utilisation de [**Windows.ApplicationModel.AppService.AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn921704) pour appeler le service.

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

* [Convertir un service d’application pour qu’il s’exécute dans le même processus que son application hôte](convert-app-service-in-process.md)
* [Définir des tâches en arrière-plan pour les besoins de votre application](support-your-app-with-background-tasks.md)
* [Exemple de code de service d'application (C#, C++ et VB)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)
