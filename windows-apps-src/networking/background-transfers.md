---
description: Utilisez l’API de transfert en arrière-plan pour copier des fichiers de manière fiable sur le réseau.
title: Transferts en arrière-plan
ms.assetid: 1207B089-BC16-4BF0-BBD4-FD99950C764B
---

# Transferts en arrière-plan

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**API importantes**

-   [**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242)
-   [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998)
-   [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960)

Utilisez l’API de transfert en arrière-plan pour copier des fichiers de manière fiable sur le réseau. L’API de transfert en arrière-plan offre des fonctionnalités avancées de chargement et téléchargement, qui s’exécutent en arrière-plan pendant la suspension d’une application, et perdurent après l’arrêt de l’application. L’API surveille l’état du réseau. Elle suspend et reprend automatiquement les transferts en cas de perte de connexion. Les transferts sont par ailleurs régis par l’Assistant Données et l’Assistant batterie, ce qui signifie que l’activité de téléchargement s’ajuste en fonction de l’état actuel de la batterie de l’appareil et de la connexion. L’API est idéale pour le chargement et le téléchargement de fichiers volumineux à l’aide du protocole HTTP(S). Le protocole FTP est également pris en charge, mais uniquement pour les téléchargements.

Le transfert en arrière-plan s’exécute séparément de l’application appelante et est principalement conçu pour des opérations de transfert à long terme pour des ressources telles que des vidéos, de la musique et des images volumineuses. Dans ces cas, l’utilisation du transfert en arrière-plan est essentielle, car les téléchargements se poursuivent même quand l’application est suspendue.

Si vous téléchargez peu de ressources et si l’opération est censée se terminer rapidement, utilisez les API [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) à la place du transfert en arrière-plan.

## Utilisation de Windows.Networking.BackgroundTransfer


### Comment la fonctionnalité de transfert en arrière-plan fonctionne-t-elle ?

Quand une application utilise la fonctionnalité de transfert en arrière-plan pour lancer un transfert, la requête est configurée et initialisée à l’aide d’objets de classe [**BackgroundDownloader**](https://msdn.microsoft.com/library/windows/apps/br207126) ou [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140). Chaque opération de transfert est gérée individuellement par le système et séparément de l’application appelante. Les informations sur la progression sont disponibles si vous voulez indiquer un état à l’utilisateur dans l’interface utilisateur de votre application. En outre, l’exécution de votre application peut être suspendue, reprise, annulée ou même lue à partir des données pendant le transfert. La manière dont les transferts sont gérés par le système favorise une consommation d’énergie intelligente et évite les problèmes qui peuvent se produire lorsqu’une application connectée se heurte à des événements tels qu’une interruption ou un arrêt de l’application, ou encore des modifications soudaines de l’état du réseau.

### Exécution de demandes de fichiers authentifiées avec le transfert en arrière-plan

Le transfert en arrière-plan fournit des méthodes qui prennent en charge les informations d’authentification serveur et proxy de base, les cookies et l’utilisation d’en-têtes HTTP personnalisés (par l’intermédiaire de la méthode [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br207146)) pour chaque opération de transfert.

### Comment cette fonctionnalité s’adapte-t-elle aux modifications de l’état du réseau ou aux arrêts inattendus ?

La fonctionnalité de transfert en arrière-plan garantit une démarche cohérente pour chaque opération de transfert en cas de modification de l’état du réseau. Pour cela, elle exploite intelligemment les informations sur l’état de la connectivité et du forfait données de l’opérateur fournies par la fonctionnalité [Connectivité](https://msdn.microsoft.com/library/windows/apps/hh452990). Afin de définir un comportement pour des scénarios réseau différents, une application définit une stratégie de coût pour chaque opération de transfert à l’aide de valeurs définies par [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138).

Par exemple, la stratégie de coût définie pour une opération peut indiquer que l’opération doit être automatiquement suspendue lorsque l’appareil exploite une connexion réseau limitée. Le transfert reprend (ou redémarre) automatiquement ensuite dès qu’une connexion à un réseau « non restreint » est établie. Pour plus d’informations sur le mode de définition par coût des réseaux, voir [**NetworkCostType**](https://msdn.microsoft.com/library/windows/apps/br207292).

Bien que la fonctionnalité de transfert en arrière-plan possède ses propres mécanismes pour gérer les modifications de l’état du réseau, d’autres considérations générales ayant trait à la connectivité des applications connectées au réseau sont à prendre en compte. Pour plus d’informations, voir [Exploitation des informations de connexion réseau disponibles](https://msdn.microsoft.com/library/windows/apps/hh452983).

> **Remarque** Les applications pour appareils mobiles disposent de fonctionnalités qui permettent à l’utilisateur de surveiller et de limiter la quantité de données transférées en fonction du type de connexion, de l’état de l’itinérance et du forfait de données. C’est pourquoi les transferts en arrière-plan peuvent être suspendus sur le téléphone même quand [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138) indique que le transfert doit s’effectuer.

Le tableau suivant indique les moments où les transferts en arrière-plan sont autorisés sur le téléphone pour chaque valeur [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138), en fonction de l’état actuel du téléphone. Vous pouvez utiliser la classe [**ConnectionCost**](https://msdn.microsoft.com/library/windows/apps/br207244) pour déterminer l’état actuel du téléphone.

| État de l’appareil                                                                                                                      | UnrestrictedOnly | Default | Always |
|-----------------------------------------------------------------------------------------------------------------------------------|------------------|---------|--------|
| Connecté au Wi-Fi                                                                                                                 | Autoriser            | Autoriser   | Autoriser  |
| Connexion limitée, pas d’itinérance, sous la limite de données, en bonne voie pour rester sous la limite                                                   | Refuser             | Autoriser   | Autoriser  |
| Connexion limitée, pas d’itinérance, sous la limite de données, en bonne voie pour dépasser la limite                                                       | Refuser             | Refuser    | Autoriser  |
| Connexion limitée, itinérance, sous la limite de données                                                                                     | Refuser             | Refuser    | Autoriser  |
| Connexion limitée, au-dessus de la limite de données. Cet état se produit uniquement quand l’utilisateur active la restriction des données en arrière-plan dans l’interface utilisateur Data Sense. | Refuser             | Refuser    | Refuser   |

 

## Téléchargement de fichiers sur le serveur


Lorsque vous faites appel à la fonctionnalité de transfert en arrière-plan, le chargement qui en résulte existe sous la forme d’une opération [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) qui expose de nombreuses méthodes de contrôle servant à redémarrer ou à annuler l’opération. Les événements d’application (par exemple, une suspension ou un arrêt) et les changements de connectivité sont gérés automatiquement par le système pour chaque opération **UploadOperation**. Les chargements se poursuivent lors des périodes de suspension des applications et perdurent après l’arrêt des applications. De plus, la propriété [**CostPolicy**](https://msdn.microsoft.com/library/windows/apps/hh701018) peut être définie pour indiquer si oui ou non votre application entamera des chargements tandis qu’une connexion réseau limitée est utilisée pour la connectivité Internet.

Les exemples qui suivent vous guident tout au long du processus de création et d’initialisation d’un chargement de base et décrivent comment énumérer des opérations de chargement issues d’une session d’application précédente.

### Chargement d’un fichier unique

La création d’un chargement commence avec [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140). Cette classe sert à fournir les méthodes qui permettent à votre application de configurer le chargement avant de créer l’objet [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) qui en résulte. L’exemple qui suit montre comment y parvenir avec les objets [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) et [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) requis.

**Identifier le fichier et la destination du chargement**

Avant de commencer avec la création d’un objet [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224), nous devons avant tout identifier l’URI du lieu de chargement, ainsi que le fichier qui sera chargé. Dans l’exemple qui suit, la valeur *uriString* est remplie au moyen d’une chaîne issue d’entrées de l’interface utilisateur et la valeur *file* est remplie à l’aide de l’objet  [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) renvoyé par une opération [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275).

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_B "Identify the file and destination for the upload")]

**Créer et initialiser l’opération de chargement**

Dans l’étape qui précède, les valeurs *uriString* et *file* sont transmises à une instance de notre exemple suivant, UploadOp, où elles sont utilisées pour configurer et lancer la nouvelle opération de chargement. Pour commencer, la valeur *uriString* est analysée afin de créer l’objet [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) requis.

Ensuite, les propriétés de l’objet [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) fourni (*file*) sont utilisées par la classe [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) pour remplir l’en-tête de la demande et définir la propriété *SourceFile* à l’aide de l’objet **StorageFile**. La méthode [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br207146) est ensuite appelée pour insérer le nom de fichier, fourni comme chaîne, et la propriété [**StorageFile.Name**](https://msdn.microsoft.com/library/windows/apps/br227220).

Pour finir, [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) crée l’objet [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) (*upload*).

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_A "Create and initialize the upload operation")]

Notez les appels de méthode asynchrone définis à l’aide de promesses JavaScript. Examinons une ligne du dernier exemple :

```javascript
promise = upload.startAsync().then(complete, error, progress);
```

    The async method call is followed by a then statement which indicates methods, defined by the app, that are called when a result from the async method call is returned. For more information on this programming pattern, see [Asynchronous programming in JavaScript using promises](http://msdn.microsoft.com/library/windows/apps/hh464930.aspx).

### Chargement de plusieurs fichiers

**Identifier les fichiers et la destination du chargement**

    In a scenario involving multiple files transferred with a single [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224), the process begins as it usually does by first providing the required destination URI and local file information. Similar to the example in the previous section, the URI is provided as a string by the end-user and [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) can be used to provide the ability to indicate files through the user interface as well. However, in this scenario the app should instead call the [**PickMultipleFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br207851) method to enable the selection of multiple files through the UI.

```javascript
function uploadFiles() {
       var filePicker = new Windows.Storage.Pickers.FileOpenPicker();
       filePicker.fileTypeFilter.replaceAll(["*"]);

       filePicker.pickMultipleFilesAsync().then(function (files) {
          if (files === 0) {
             printLog("No file selected");
                return;
          }

          var upload = new UploadOperation();
          var uriString = document.getElementById("serverAddressField").value;
          upload.startMultipart(uriString, files);

          // Persist the upload operation in the global array.
          uploadOperations.push(upload);
       });
    }
```

**Créer des objets pour les paramètres fournis**

    The next two examples use code contained in a single example method, **startMultipart**, which was called at the end of the last step. For the purpose of instruction the code in the method that creates an array of [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) objects has been split from the code that creates the resultant [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224).

    First, the URI string provided by the user is initialized as a [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998). Next, the array of [**IStorageFile**](https://msdn.microsoft.com/library/windows/apps/br227102) objects (**files**) passed to this method is iterated through, each object is used to create a new [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) object which is then placed in the **contentParts** array.

```javascript
upload.startMultipart = function (uriString, files) {
        try {
            var uri = new Windows.Foundation.Uri(uriString);
            var uploader = new Windows.Networking.BackgroundTransfer.BackgroundUploader();

            var contentParts = [];
            files.forEach(function (file, index) {
                var part = new Windows.Networking.BackgroundTransfer.BackgroundTransferContentPart("File" + index, file.name);
                part.setFile(file);
                contentParts.push(part);
            });
```

**Créer et initialiser l’opération de chargement à parties multiples**

    With our contentParts array populated with all of the [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) objects representing each [**IStorageFile**](https://msdn.microsoft.com/library/windows/apps/br227102) for upload, we are ready to call [**CreateUploadAsync**](https://msdn.microsoft.com/library/windows/apps/hh923973) using the [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) to indicate where the request will be sent.

```javascript
        // Create a new upload operation.
            uploader.createUploadAsync(uri, contentParts).then(function (uploadOperation) {

               // Start the upload and persist the promise to be able to cancel the upload.
               upload = uploadOperation;
               promise = uploadOperation.startAsync().then(complete, error, progress);
            });

         } catch (err) {
             displayError(err);
         }
     };
```

### Redémarrage d’opérations de chargement interrompues

Lors de l’achèvement ou de l’annulation d’une opération [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224), toutes les ressources système associées sont libérées. Toutefois, si votre application est arrêtée avant que l’une de ces situations puisse se produire, toutes les opérations en cours sont suspendues et les ressources associées à chacune d’entre elles persistent. Si ces opérations ne sont pas énumérées et réintroduites dans la session d’application suivante, elles s’arrêteront et resteront présentes dans les ressources d’appareil.

1.  Avant de définir la fonction chargée d’énumérer les opérations persistantes, nous devons créer un tableau pour y stocker les objets [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) que cette fonction renverra :

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_C "Restart interrupted upload operation")]

2.  Il nous faut ensuite définir la fonction qui énumère les opérations persistantes et les stocke dans notre tableau. Notez que la méthode **load** appelée pour réaffecter les rappels vers [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224), si elle persiste après l’arrêt de l’application, se trouve dans la classe UploadOp définie plus loin dans cette section.

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_D "Enumerate persisted operations")]

## Téléchargement de fichiers

Lorsque vous faites appel à la fonctionnalité de transfert en arrière-plan, chaque téléchargement qui en résulte existe sous la forme d’une opération [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) qui dévoile de nombreuses méthodes de contrôle servant à suspendre, reprendre, redémarrer et annuler l’opération. Les événements d’application (par exemple, une suspension ou un arrêt) et les changements de connectivité sont gérés automatiquement par le système pour chaque opération **DownloadOperation**. Les téléchargements se poursuivent lors des périodes de suspension des applications et perdurent après l’arrêt des applications. Dans le cadre des scénarios de réseau mobile, la propriété [**CostPolicy**](https://msdn.microsoft.com/library/windows/apps/hh701018) peut être définie pour indiquer si oui ou non votre application entamera ou poursuivra un téléchargement tandis qu’une connexion réseau limitée est utilisée pour la connectivité Internet.

Si vous téléchargez peu de ressources et si l’opération est censée se terminer rapidement, utilisez les API [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) à la place du transfert en arrière-plan.

Les exemples qui suivent vous guident tout au long du processus de création et d’initialisation d’un téléchargement de base et décrivent comment énumérer des opérations de chargement issues d’une session d’application précédente.

### Configurer et démarrer un téléchargement de fichier à l’aide de la fonctionnalité de transfert en arrière-plan

L’exemple qui suit montre comment des chaînes représentant un URI et un nom de fichier peuvent être utilisées pour créer un objet [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) et le [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) qui contiendra la ressource demandée. Dans cet exemple, le nouveau fichier est automatiquement placé à un emplacement prédéfini. La classe [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) peut également servir aux utilisateurs pour indiquer où enregistrer le fichier sur l’appareil. Notez que la méthode **load** appelée pour réaffecter les rappels vers [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154), si elle persiste après l’arrêt de l’application, se trouve dans la classe DownloadOp définie plus loin dans cette section.

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_A)]

Notez les appels de méthode asynchrone définis à l’aide de promesses JavaScript. Examinons la ligne 17 du dernier échantillon de code :

```javascript
promise = download.startAsync().then(complete, error, progress);
```

L’appel de méthode asynchrone est suivi d’une instruction then indiquant les méthodes, définies par l’application, qui sont appelées lorsqu’un résultat de l’appel de méthode asynchrone est retourné. Pour plus d’informations sur ce modèle de programmation, voir [Programmation asynchrone en JavaScript à l’aide de promesses](http://msdn.microsoft.com/library/windows/apps/hh464930.aspx).

### Ajout de méthodes de contrôle des opérations supplémentaires

Le niveau de contrôle peut être augmenté en implémentant des méthodes [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) supplémentaires. Par exemple, en intégrant le code suivant à l’exemple ci-dessus, nous pouvons inclure la possibilité d’annuler l’opération.

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_B)]

### Énumération des opérations persistantes au démarrage

Lors de l’achèvement ou de l’annulation d’une opération [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154), toutes les ressources système associées sont libérées. Toutefois, si votre application est arrêtée avant que l’un de ces événements ne se produise, les téléchargements sont suspendus et persistent en arrière-plan. Les exemples qui suivent expliquent comment réintroduire des téléchargements persistants dans une nouvelle session d’application.

1.  Avant de définir la fonction chargée d’énumérer les opérations persistantes, nous devons créer un tableau pour y stocker les objets [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) que cette fonction renverra :

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_D)]

2.  Il nous faut ensuite définir la fonction qui énumère les opérations persistantes et les stocke dans notre tableau. Notez que la méthode **load** appelée pour réaffecter les rappels pour une opération [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) persistante se trouve dans l’exemple DownloadOp défini plus loin dans cette section.

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_E)]

3.  Vous pouvez désormais utiliser la liste remplie pour redémarrer des opérations en attente.

## Post-traitement

Une nouvelle fonctionnalité dans Windows 10 est la possibilité d’exécuter un code d’application à la fin d’un transfert en arrière-plan, même lorsque l’application n’est pas en cours d’exécution. Par exemple, votre application pourrait mettre à jour une liste de films disponibles à l’issue du téléchargement d’un film, au lieu de rechercher la présence de nouveaux films à chaque démarrage. Elle pourrait également gérer un transfert de fichier ayant échoué en réessayant via un serveur ou un port différent. Le post-traitement est appelé pour tous les transferts, réussis ou non. Vous pouvez donc l’utiliser pour implémenter une logique personnalisée de gestion des erreurs et de nouvelle tentative.

Un post-traitement utilise l’infrastructure de tâche en arrière-plan existante. Vous créez une tâche en arrière-plan et l’associez à vos transferts avant de les démarrer. Les transferts sont ensuite exécutés en arrière-plan et, quand ils sont terminés, votre tâche en arrière-plan est appelée pour effectuer le post-traitement.

Un post-traitement utilise une nouvelle classe, [**BackgroundTransferCompletionGroup**](https://msdn.microsoft.com/library/windows/apps/dn804209). Cette classe est semblable à la classe [**BackgroundTransferGroup**](https://msdn.microsoft.com/library/windows/apps/dn279030) existante, dans la mesure où elle vous permet de regrouper des transferts en arrière-plan. Toutefois, la classe **BackgroundTransferCompletionGroup** ajoute la possibilité de désigner une tâche en arrière-plan pour s’exécuter une fois le transfert terminé.

Pour initier un transfert en arrière-plan avec post-traitement, procédez comme suit.

1.  Créez un objet [**BackgroundTransferCompletionGroup**](https://msdn.microsoft.com/library/windows/apps/dn804209). Créez ensuite un objet [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768). Définissez la propriété **Trigger** de l’objet générateur sur l’objet groupe d’achèvement, et la propriété **TaskEngtyPoint** du générateur sur le point d’entrée de la tâche en arrière-plan qui doit s’exécuter à la fin du transfert. Pour finir, appelez la méthode [**BackgroundTaskBuilder.Register**](https://msdn.microsoft.com/library/windows/apps/br224772) pour inscrire votre tâche en arrière-plan. Notez que de nombreux groupes d’achèvement peuvent partager un point d’entrée de tâche en arrière-plan, mais que vous ne pouvez avoir qu’un seul groupe d’achèvement par inscription de tâche en arrière-plan.

   ```csharp
    var completionGroup = new BackgroundTransferCompletionGroup();
    BackgroundTaskBuilder builder = new BackgroundTaskBuilder();

    builder.Name = "MyDownloadProcessingTask";
    builder.SetTrigger(completionGroup.Trigger);
    builder.TaskEntryPoint = "Tasks.BackgroundDownloadProcessingTask";

    BackgroundTaskRegistration downloadProcessingTask = builder.Register();
    ```

2.  Next you associate background transfers with the completion group. Once all transfers are created, enable the completion group.

   ```csharp
    BackgroundDownloader downloader = new BackgroundDownloader(completionGroup);
    DownloadOperation download = downloader.CreateDownload(uri, file);
    Task<DownloadOperation> startTask = download.StartAsync().AsTask();

    // App still sees the normal completion path
    startTask.ContinueWith(ForegroundCompletionHandler);

    // Do not enable the CompletionGroup until after all downloads are created.
    downloader.CompletinGroup.Enable();
    ```

3.  The code in the background task extracts the list of operations from the trigger details, and your code can then inspect the details for each operation and perform appropriate post-processing for each operation.

   ```csharp
    public class BackgroundDownloadProcessingTask : IBackgroundTask
    {
      public async void Run(IBackgroundTaskInstance taskInstance)
      {
        var details = (BackgroundTransferCompletionGroupTriggerDetails)taskInstance.TriggerDetails;
        IReadOnlyList<DownloadOperation> downloads = details.Downloads;

        // Do post-processing on each finished operation in the list of downloads
      }
    }
    ```

The post-processing task is a regular background task. It is part of the pool of all background tasks, and it is subject to the same resource management policy as all background tasks.

Also, note that post-processing does not replace foreground completion handlers. If your app defines a foreground completion handler, and your app is running when the file transfer completes, then both your foreground completion handler and your background completion handler will be called. The order in which foreground and background tasks are called is not guaranteed. If you define both, you should ensure that the two tasks will work properly and not interfere with each other if they are running concurrently.

## Request timeouts

There are two primary connection timeout scenarios to take into consideration:

-   When establishing a new connection for a transfer, the connection request is aborted if it is not established within five minutes.

-   After a connection has been established, an HTTP request message that has not received a response within two minutes is aborted.

> **Note**  In either scenario, assuming there is Internet connectivity, Background Transfer will retry a request up to three times automatically. In the event Internet connectivity is not detected, additional requests will wait until it is.

## Debugging guidance

Stopping a debugging session in Microsoft Visual Studio is comparable to closing your app; PUT uploads are paused and POST uploads are terminated. Even while debugging, your app should enumerate and then restart or cancel any persisted uploads. For example, you can have your app cancel enumerated persisted upload operations at app startup if there is no interest in previous operations for that debug session.

While enumerating downloads/uploads on app startup during a debug session, you can have your app cancel them if there is no interest in previous operations for that debug session. Note that if there are Visual Studio project updates, like changes to the app manifest, and the app is uninstalled and re-deployed, [**GetCurrentUploadsAsync**](https://msdn.microsoft.com/library/windows/apps/hh701149) cannot enumerate operations created using the previous app deployment.

When using Background Transfer during development, you may get into a situation where the internal caches of active and completed transfer operations can get out of sync. This may result in the inability to start new transfer operations or interact with existing operations and [**BackgroundTransferGroup**](https://msdn.microsoft.com/library/windows/apps/dn279030) objects. In some cases, attempting to interact with existing operations may trigger a crash. This result can occur if the [**TransferBehavior**](https://msdn.microsoft.com/library/windows/apps/dn279033) property is set to **Parallel**. This issue occurs only in certain scenarios during development and is not applicable to end users of your app.

Four scenarios using Visual Studio can cause this issue.

-   You create a new project with the same app name as an existing project, but a different language (from C++ to C#, for example).
-   You change the target architecture (from x86 to x64, for example) in an existing project.
-   You change the culture (from neutral to en-US, for example) in an existing project.
-   You add or remove a capability in the package manifest (adding **Enterprise Authentication**, for example) in an existing project.

Regular app servicing, including manifest updates which add or remove capabilities, do not trigger this issue on end user deployments of your app.
To work around this issue, completely uninstall all versions of the app and re-deploy with the new language, architecture, culture, or capability. This can be done via the **Start** screen or using PowerShell and the **Remove-AppxPackage** cmdlet.

## Exceptions in Windows.Networking.BackgroundTransfer

An exception is thrown when an invalid string for a the Uniform Resource Identifier (URI) is passed to the constructor for the [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) object.

**.NET:  **The [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) type appears as [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) in C# and VB.

In C# and Visual Basic, this error can be avoided by using the [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) class in the .NET 4.5 and one of the [**System.Uri.TryCreate**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.trycreate.aspx) methods to test the string received from the app user before the URI is constructed.

In C++, there is no method to try and parse a string to a URI. If an app gets input from the user for the [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998), the constructor should be in a try/catch block. If an exception is thrown, the app can notify the user and request a new hostname.

The [**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) namespace has convenient helper methods and uses enumerations in the [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) namespace for handling errors. This can be useful for handling specific network exceptions differently in your app.

An error encountered on an asynchronous method in the [**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) namespace is returned as an **HRESULT** value. The [**BackgroundTransferError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701093) method is used to convert a network error from a background transfer operation to a [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh747818) enumeration value. Most of the **WebErrorStatus** enumeration values correspond to an error returned by the native HTTP or FTP client operation. An app can filter on specific **WebErrorStatus** enumeration values to modify app behavior depending on the cause of the exception.

For parameter validation errors, an app can also use the **HRESULT** from the exception to learn more detailed information on the error that caused the exception. Possible **HRESULT** values are listed in the *Winerror.h* header file. For most parameter validation errors, the **HRESULT** returned is **E\_INVALIDARG**.



<!--HONumber=Mar16_HO1-->


