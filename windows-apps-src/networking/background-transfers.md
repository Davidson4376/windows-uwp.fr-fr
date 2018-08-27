---
author: stevewhims
description: Utilisez l’API de transfert en arrière-plan pour copier des fichiers de manière fiable sur le réseau.
title: Transferts en arrière-plan
ms.assetid: 1207B089-BC16-4BF0-BBD4-FD99950C764B
ms.author: stwhi
ms.date: 03/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fb273b6a37cb2f6322b0c9e3842b69676f82c616
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "2867858"
---
# <a name="background-transfers"></a>Transferts en arrière-plan
Utilisez l’API de transfert en arrière-plan pour copier des fichiers de manière fiable sur le réseau. L’API de transfert en arrière-plan offre des fonctionnalités avancées de chargement et téléchargement, qui s’exécutent en arrière-plan pendant la suspension d’une application, et perdurent après l’arrêt de l’application. L’API surveille l’état du réseau. Elle suspend et reprend automatiquement les transferts en cas de perte de connexion. Les transferts sont par ailleurs régis par l’Assistant Données et l’Assistant batterie, ce qui signifie que l’activité de téléchargement s’ajuste en fonction de l’état actuel de la batterie de l’appareil et de la connexion. L’API est idéale pour le chargement et le téléchargement de fichiers volumineux à l’aide du protocole HTTP(S). Le protocole FTP est également pris en charge, mais uniquement pour les téléchargements.

Le transfert en arrière-plan s’exécute séparément de l’application appelante et est principalement conçu pour des opérations de transfert à long terme pour des ressources telles que des vidéos, de la musique et des images volumineuses. Dans ces cas, l’utilisation du transfert en arrière-plan est essentielle, car les téléchargements se poursuivent même quand l’application est suspendue.

Si vous téléchargez peu de ressources et si l’opération est censée se terminer rapidement, utilisez les API [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) à la place du transfert en arrière-plan.

## <a name="using-windowsnetworkingbackgroundtransfer"></a>Utilisation de Windows.Networking.BackgroundTransfer

### <a name="how-does-the-background-transfer-feature-work"></a>Comment la fonctionnalité de transfert en arrière-plan fonctionne-t-elle ?
Quand une application utilise la fonctionnalité de transfert en arrière-plan pour lancer un transfert, la requête est configurée et initialisée à l’aide d’objets de classe [**BackgroundDownloader**](https://msdn.microsoft.com/library/windows/apps/br207126) ou [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140). Chaque opération de transfert est gérée individuellement par le système et séparément de l’application appelante. Les informations sur la progression sont disponibles si vous voulez indiquer un état à l’utilisateur dans l’interface utilisateur de votre application. En outre, l’exécution de votre application peut être suspendue, reprise, annulée ou même lue à partir des données pendant le transfert. La manière dont les transferts sont gérés par le système favorise une consommation d’énergie intelligente et évite les problèmes qui peuvent se produire lorsqu’une application connectée se heurte à des événements tels qu’une interruption ou un arrêt de l’application, ou encore des modifications soudaines de l’état du réseau.

> [!NOTE]
> En raison des contraintes de ressource par application, une application ne doit pas effectuer plus de 200transferts (DownloadOperations + UploadOperations) à un moment donné. Si cette limite est dépassée, cela peut mettre la file d’attente de transfert de l’application dans un état irrécupérable.

Lorsqu’une application est lancée, il doit appeler [**AttachAsync**](/uwp/api/windows.networking.backgroundtransfer.downloadoperation.AttachAsync) sur tous les objets existants [**DownloadOperation**](/uwp/api/windows.networking.backgroundtransfer.downloadoperation?branch=live) et [**UploadOperation**](/uwp/api/windows.networking.backgroundtransfer.uploadperation?branch=live) . Ne pas cette opération entraîne la perte des transferts déjà effectué et finalement affichera votre utilisation de la fonctionnalité de transfert en arrière-plan inutiles.

### <a name="performing-authenticated-file-requests-with-background-transfer"></a>Exécution de demandes de fichiers authentifiées avec le transfert en arrière-plan
Le transfert en arrière-plan fournit des méthodes qui prennent en charge les informations d’authentification serveur et proxy de base, les cookies et l’utilisation d’en-têtes HTTP personnalisés (par l’intermédiaire de la méthode [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br207146)) pour chaque opération de transfert.

### <a name="how-does-this-feature-adapt-to-network-status-changes-or-unexpected-shutdowns"></a>Comment cette fonctionnalité s’adapte-t-elle aux modifications de l’état du réseau ou aux arrêts inattendus ?
La fonctionnalité de transfert en arrière-plan garantit une démarche cohérente pour chaque opération de transfert en cas de modification de l’état du réseau. Pour cela, elle exploite intelligemment les informations sur l’état de la connectivité et du forfait données de l’opérateur fournies par la fonctionnalité [Connectivité](https://msdn.microsoft.com/library/windows/apps/hh452990). Afin de définir un comportement pour des scénarios réseau différents, une application définit une stratégie de coût pour chaque opération de transfert à l’aide de valeurs définies par [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138).

Par exemple, la stratégie de coût définie pour une opération peut indiquer que l’opération doit être automatiquement suspendue lorsque l’appareil exploite une connexion réseau limitée. Le transfert reprend (ou redémarre) automatiquement ensuite dès qu’une connexion à un réseau « non restreint » est établie. Pour plus d’informations sur le mode de définition par coût des réseaux, voir [**NetworkCostType**](https://msdn.microsoft.com/library/windows/apps/br207292).

Bien que la fonctionnalité de transfert en arrière-plan possède ses propres mécanismes pour gérer les modifications de l’état du réseau, d’autres considérations générales ayant trait à la connectivité des applications connectées au réseau sont à prendre en compte. Pour plus d’informations, voir [Exploitation des informations de connexion réseau disponibles](https://msdn.microsoft.com/library/windows/apps/hh452983).

> **Remarque**  Les applications pour appareils mobiles disposent de fonctionnalités qui permettent à l’utilisateur de surveiller et de limiter la quantité de données transférées en fonction du type de connexion, de l’état de l’itinérance et du forfait de données. C’est pourquoi les transferts en arrière-plan peuvent être suspendus sur le téléphone même quand [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138) indique que le transfert doit s’effectuer.

Le tableau suivant indique les moments où les transferts en arrière-plan sont autorisés sur le téléphone pour chaque valeur [**BackgroundTransferCostPolicy**](https://msdn.microsoft.com/library/windows/apps/br207138), en fonction de l’état actuel du téléphone. Vous pouvez utiliser la classe [**ConnectionCost**](https://msdn.microsoft.com/library/windows/apps/br207244) pour déterminer l’état actuel du téléphone.

| État de l’appareil                                                                                                                      | UnrestrictedOnly | Default | Always |
|-----------------------------------------------------------------------------------------------------------------------------------|------------------|---------|--------|
| Connecté au Wi-Fi                                                                                                                 | Autoriser            | Autoriser   | Autoriser  |
| Connexion limitée, pas d’itinérance, sous la limite de données, en bonne voie pour rester sous la limite                                                   | Refuser             | Autoriser   | Autoriser  |
| Connexion limitée, pas d’itinérance, sous la limite de données, en bonne voie pour dépasser la limite                                                       | Refuser             | Refuser    | Autoriser  |
| Connexion limitée, itinérance, sous la limite de données                                                                                     | Refuser             | Refuser    | Autoriser  |
| Connexion limitée, au-dessus de la limite de données. Cet état se produit uniquement quand l’utilisateur active la restriction des données en arrière-plan dans l’interface utilisateur Data Sense. | Refuser             | Refuser    | Refuser   |

## <a name="uploading-files"></a>Téléchargement de fichiers sur le serveur
Lorsque vous faites appel à la fonctionnalité de transfert en arrière-plan, le chargement qui en résulte existe sous la forme d’une opération [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) qui expose de nombreuses méthodes de contrôle servant à redémarrer ou à annuler l’opération. Les événements d’application (par exemple, une suspension ou un arrêt) et les changements de connectivité sont gérés automatiquement par le système pour chaque opération **UploadOperation**. Les chargements se poursuivent lors des périodes de suspension des applications et perdurent après l’arrêt des applications. De plus, la propriété [**CostPolicy**](https://msdn.microsoft.com/library/windows/apps/hh701018) peut être définie pour indiquer si oui ou non votre application entamera des chargements tandis qu’une connexion réseau limitée est utilisée pour la connectivité Internet.

Les exemples qui suivent vous guident tout au long du processus de création et d’initialisation d’un chargement de base et décrivent comment énumérer des opérations de chargement issues d’une session d’application précédente.

### <a name="uploading-a-single-file"></a>Chargement d’un fichier unique
La création d’un chargement commence avec [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140). Cette classe sert à fournir les méthodes qui permettent à votre application de configurer le chargement avant de créer l’objet [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) qui en résulte. L’exemple qui suit montre comment y parvenir avec les objets [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) et [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) requis.

**Identifier le fichier et la destination du chargement**

Avant de commencer avec la création d’un objet [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224), nous devons avant tout identifier l’URI du lieu de chargement, ainsi que le fichier qui sera chargé. Dans l’exemple qui suit, la valeur *uriString* est remplie au moyen d’une chaîne issue d’entrées de l’interface utilisateur et la valeur *file* est remplie à l’aide de l’objet [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) renvoyé par une opération [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275).

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_B "Identify the file and destination for the upload")]

**Créer et initialiser l’opération de chargement**

Dans l’étape qui précède, les valeurs *uriString* et *file* sont transmises à une instance de notre exemple suivant, UploadOp, où elles sont utilisées pour configurer et lancer la nouvelle opération de chargement. Pour commencer, la valeur *uriString* est analysée afin de créer l’objet [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) requis.

Ensuite, les propriétés de l’objet [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) fourni (*file*) sont utilisées par la classe [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) pour remplir l’en-tête de la demande et définir la propriété *SourceFile* à l’aide de l’objet **StorageFile**. La méthode [**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br207146) est ensuite appelée pour insérer le nom de fichier, fourni comme chaîne, et la propriété [**StorageFile.Name**](https://msdn.microsoft.com/library/windows/apps/br227220).

Pour finir, [**BackgroundUploader**](https://msdn.microsoft.com/library/windows/apps/br207140) crée l’objet [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) (*chargement*).

[!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_A "Create and initialize the upload operation")]

Notez les appels de méthode asynchrone définis à l’aide de promesses JavaScript. Examinons une ligne du dernier exemple :

```javascript
promise = upload.startAsync().then(complete, error, progress);
```

L’appel de méthode asynchrone est suivi d’une instruction then indiquant les méthodes, définies par l’application, qui sont appelées lorsqu’un résultat de l’appel de méthode asynchrone est retourné. Pour plus d’informations sur ce modèle de programmation, voir [Programmation asynchrone en JavaScript à l’aide de promesses](http://msdn.microsoft.com/library/windows/apps/hh464930.aspx).

### <a name="uploading-multiple-files"></a>Chargement de plusieurs fichiers
**Identifier les fichiers et la destination du chargement**

Dans un scénario impliquant plusieurs fichiers transférés avec une seule opération [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224), le processus commence comme habituellement en fournissant tout d’abord l’URI de destination et les informations de fichier local requis. Comme dans l’exemple de la section précédente, l’URI est fourni sous forme de chaîne par l’utilisateur final et [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) peut être utilisé pour offrir la possibilité d’indiquer les fichiers par le biais de l’interface utilisateur également. Toutefois, dans ce scénario, l’application doit plutôt appeler la méthode [**PickMultipleFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br207851) pour permettre la sélection de plusieurs fichiers par le biais de l’interface utilisateur.

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

Les deux prochains exemples utilisent du code contenu dans un exemple de méthode unique, **startMultipart**, qui a été appelée à la fin de la dernière étape. Pour les besoins de l’instruction, le code présent dans la méthode qui crée un tableau d’objets [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) a été séparé du code qui crée l’opération [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) résultante.

D’abord, la chaîne d’URI fournie par l’utilisateur est initialisée en tant qu’[**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998). Ensuite, une itération est effectuée sur le tableau d’objets [**IStorageFile**](https://msdn.microsoft.com/library/windows/apps/br227102) (**files**) passés à cette méthode. Chaque objet est utilisé pour créer un nouvel objet [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) qui est ensuite placé dans le tableau de **contentParts**.

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

Avec notre tableau de contentParts rempli avec tous les objets [**BackgroundTransferContentPart**](https://msdn.microsoft.com/library/windows/apps/hh923029) représentant chaque [**IStorageFile**](https://msdn.microsoft.com/library/windows/apps/br227102) pour le chargement, nous sommes prêts à appeler [**CreateUploadAsync**](https://msdn.microsoft.com/library/windows/apps/hh923973) à l’aide de l’[**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) pour indiquer où la demande sera envoyée.

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

### <a name="restarting-interrupted-upload-operations"></a>Redémarrage d’opérations de chargement interrompues
Lors de l’achèvement ou de l’annulation d’une opération [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224), toutes les ressources système associées sont libérées. Toutefois, si votre application est arrêtée avant que l’une de ces situations puisse se produire, toutes les opérations en cours sont suspendues et les ressources associées à chacune d’entre elles persistent. Si ces opérations ne sont pas énumérées et réintroduites dans la session d’application suivante, elles s’arrêteront et resteront présentes dans les ressources d’appareil.

1.  Avant de définir la fonction chargée d’énumérer les opérations persistantes, nous devons créer un tableau pour y stocker les objets [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224) que cette fonction renverra:

    [!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_C "Restart interrupted upload operation")]

1.  Il nous faut ensuite définir la fonction qui énumère les opérations persistantes et les stocke dans notre tableau. Notez que la méthode **load** appelée pour réaffecter les rappels vers [**UploadOperation**](https://msdn.microsoft.com/library/windows/apps/br207224), si elle persiste après l’arrêt de l’application, se trouve dans la classe UploadOp définie plus loin dans cette section.

    [!code-js[uploadFile](./code/backgroundtransfer/upload_quickstart/js/main.js#Snippetupload_quickstart_D "Enumerate persisted operations")]

## <a name="downloading-files"></a>Téléchargement de fichiers
Lorsque vous faites appel à la fonctionnalité de transfert en arrière-plan, chaque téléchargement qui en résulte existe sous la forme d’une opération [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) qui dévoile de nombreuses méthodes de contrôle servant à suspendre, reprendre, redémarrer et annuler l’opération. Les événements d’application (par exemple, une suspension ou un arrêt) et les changements de connectivité sont gérés automatiquement par le système pour chaque opération **DownloadOperation**. Les téléchargements se poursuivent lors des périodes de suspension des applications et perdurent après l’arrêt des applications. Dans le cadre des scénarios de réseau mobile, la propriété [**CostPolicy**](https://msdn.microsoft.com/library/windows/apps/hh701018) peut être définie pour indiquer si oui ou non votre application entamera ou poursuivra un téléchargement tandis qu’une connexion réseau limitée est utilisée pour la connectivité Internet.

Si vous téléchargez peu de ressources et si l’opération est censée se terminer rapidement, utilisez les API [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) à la place du transfert en arrière-plan.

Les exemples qui suivent vous guident tout au long du processus de création et d’initialisation d’un téléchargement de base et décrivent comment énumérer des opérations de chargement issues d’une session d’application précédente.

### <a name="configure-and-start-a-background-transfer-file-download"></a>Configurer et démarrer un téléchargement de fichier à l’aide de la fonctionnalité de transfert en arrière-plan
L’exemple qui suit montre comment des chaînes représentant un URI et un nom de fichier peuvent être utilisées pour créer un objet [**Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) et le [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) qui contiendra la ressource demandée. Dans cet exemple, le nouveau fichier est automatiquement placé à un emplacement prédéfini. La classe [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) peut également servir aux utilisateurs pour indiquer où enregistrer le fichier sur l’appareil. Notez que la méthode **load** appelée pour réaffecter les rappels vers [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154), si elle persiste après l’arrêt de l’application, se trouve dans la classe DownloadOp définie plus loin dans cette section.

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_A)]

Notez les appels de méthode asynchrone définis à l’aide de promesses JavaScript. Examinons la ligne 17 du dernier échantillon de code :

```javascript
promise = download.startAsync().then(complete, error, progress);
```

L’appel de méthode asynchrone est suivi d’une instruction then indiquant les méthodes, définies par l’application, qui sont appelées lorsqu’un résultat de l’appel de méthode asynchrone est retourné. Pour plus d’informations sur ce modèle de programmation, voir [Programmation asynchrone en JavaScript à l’aide de promesses](http://msdn.microsoft.com/library/windows/apps/hh464930.aspx).

### <a name="adding-additional-operation-control-methods"></a>Ajout de méthodes de contrôle des opérations supplémentaires
Le niveau de contrôle peut être augmenté en implémentant des méthodes [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) supplémentaires. Par exemple, en intégrant le code suivant à l’exemple ci-dessus, nous pouvons inclure la possibilité d’annuler l’opération.

[!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_B)]

### <a name="enumerating-persisted-operations-at-start-up"></a>Énumération des opérations persistantes au démarrage
Lors de l’achèvement ou de l’annulation d’une opération [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154), toutes les ressources système associées sont libérées. Toutefois, si votre application est arrêtée avant que l’un de ces événements ne se produise, les téléchargements sont suspendus et persistent en arrière-plan. Les exemples qui suivent expliquent comment réintroduire des téléchargements persistants dans une nouvelle session d’application.

1.  Avant de définir la fonction chargée d’énumérer les opérations persistantes, nous devons créer un tableau pour y stocker les objets [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) que cette fonction renverra:

    [!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_D)]

1.  Il nous faut ensuite définir la fonction qui énumère les opérations persistantes et les stocke dans notre tableau. Notez que la méthode **load** appelée pour réaffecter les rappels pour une opération [**DownloadOperation**](https://msdn.microsoft.com/library/windows/apps/br207154) persistante se trouve dans l’exemple DownloadOp défini plus loin dans cette section.

    [!code-js[uploadFile](./code/backgroundtransfer/download_quickstart/js/main.js#Snippetdownload_quickstart_E)]

1.  Vous pouvez désormais utiliser la liste remplie pour redémarrer des opérations en attente.

## <a name="post-processing"></a>Post-traitement
Une nouvelle fonctionnalité dans Windows 10 est la possibilité d’exécuter un code d’application à la fin d’un transfert en arrière-plan, même lorsque l’application n’est pas en cours d’exécution. Par exemple, votre application pourrait mettre à jour une liste de films disponibles à l’issue du téléchargement d’un film, au lieu de rechercher la présence de nouveaux films à chaque démarrage. Elle pourrait également gérer un transfert de fichier ayant échoué en réessayant via un serveur ou un port différent. Le post-traitement est appelé pour tous les transferts, réussis ou non. Vous pouvez donc l’utiliser pour implémenter une logique personnalisée de gestion des erreurs et de nouvelle tentative.

Un post-traitement utilise l’infrastructure de tâche en arrière-plan existante. Vous créez une tâche en arrière-plan et l’associez à vos transferts avant de les démarrer. Les transferts sont ensuite exécutés en arrière-plan et, quand ils sont terminés, votre tâche en arrière-plan est appelée pour effectuer le post-traitement.

Un post-traitement utilise une nouvelle classe, [**BackgroundTransferCompletionGroup**](https://msdn.microsoft.com/library/windows/apps/dn804209). Cette classe est semblable à la classe [**BackgroundTransferGroup**](https://msdn.microsoft.com/library/windows/apps/dn279030) existante, dans la mesure où elle vous permet de regrouper des transferts en arrière-plan. Toutefois, la classe **BackgroundTransferCompletionGroup** ajoute la possibilité de désigner une tâche en arrière-plan pour s’exécuter une fois le transfert terminé.

Pour initier un transfert en arrière-plan avec post-traitement, procédez comme suit.

1.  Créez un objet [**BackgroundTransferCompletionGroup**](https://msdn.microsoft.com/library/windows/apps/dn804209). Créez ensuite un objet [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768). Définissez la propriété **Trigger** de l’objet générateur sur l’objet groupe d’achèvement, et la propriété **TaskEntryPoint** du générateur sur le point d’entrée de la tâche en arrière-plan qui doit s’exécuter à la fin du transfert. Pour finir, appelez la méthode [**BackgroundTaskBuilder.Register**](https://msdn.microsoft.com/library/windows/apps/br224772) pour inscrire votre tâche en arrière-plan. Notez que de nombreux groupes d’achèvement peuvent partager un point d’entrée de tâche en arrière-plan, mais que vous ne pouvez avoir qu’un seul groupe d’achèvement par inscription de tâche en arrière-plan.

```csharp
var completionGroup = new BackgroundTransferCompletionGroup();
BackgroundTaskBuilder builder = new BackgroundTaskBuilder();

builder.Name = "MyDownloadProcessingTask";
builder.SetTrigger(completionGroup.Trigger);
builder.TaskEntryPoint = "Tasks.BackgroundDownloadProcessingTask";

BackgroundTaskRegistration downloadProcessingTask = builder.Register();
```

2.  Vous associez ensuite les transferts en arrière-plan au groupe d’achèvement. Une fois tous les transferts créés, activez le groupe d’achèvement.

```csharp
BackgroundDownloader downloader = new BackgroundDownloader(completionGroup);
DownloadOperation download = downloader.CreateDownload(uri, file);
Task<DownloadOperation> startTask = download.StartAsync().AsTask();

// App still sees the normal completion path
startTask.ContinueWith(ForegroundCompletionHandler);

// Do not enable the CompletionGroup until after all downloads are created.
downloader.CompletinGroup.Enable();
```

3.  Le code de la tâche en arrière-plan extrait la liste d’opérations des détails du déclencheur. Votre code peut alors inspecter les détails de chaque opération et effectuer un post-traitement approprié pour chaque opération.

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

La tâche de post-traitement est une tâche en arrière-plan normale. Elle fait partie du pool de toutes les tâches en arrière-plan, et est soumise à la même stratégie de gestion des ressources que toutes les tâches en arrière-plan.

Notez également que le post-traitement ne remplace pas les gestionnaires d’achèvement au premier plan. Si votre application définit un gestionnaire d’achèvement au premier plan et s’exécute lorsque le transfert de fichiers se termine, votre gestionnaire d’achèvement au premier plan et votre gestionnaire d’achèvement en arrière-plan sont appelés. L’ordre dans lequel les tâches au premier plan et en arrière-plan sont appelées n’est pas garanti. Si vous définissez les deux, vous devez vous assurer que les deux tâches fonctionneront correctement et n’interféreront pas entre elles en cas d’exécution simultanée.

## <a name="request-timeouts"></a>Délais d’expiration des demandes
Il existe deux cas principaux de délai de connexion à prendre en considération.

-   Lorsque vous établissez une nouvelle connexion pour un transfert, la demande de connexion est annulée si la connexion n’est pas établie dans un délai de cinq minutes.

-   Une fois la connexion établie, un message de requêteHTTP qui n’a reçu aucune réponse au bout de deux minutes est annulé.

> **Réponse**  Quel que soit le scénario, la fonctionnalité de transfert en arrière-plan part du principe qu’aucune connectivité Internet n’existe et tente jusqu’à trois fois de soumettre automatiquement une demande. Si aucune connectivitéInternet n’est décelée, les demandes supplémentaires attendront jusqu’à ce qu’elle le soit.

## <a name="debugging-guidance"></a>Recommandations en matière de débogage
L’arrêt d’une session de débogage dans Microsoft Visual Studio est comparable à la fermeture de votre application; les chargements PUT sont mis en pause et les chargements POST sont arrêtés. Même pendant le débogage, votre application doit énumérer, puis redémarrer ou annuler les chargements persistants. Par exemple, votre application peut annuler l’énumération des opérations de chargement persistantes, au démarrage, si les opérations précédentes n’ont pas d’intérêt pour cette session de débogage.

Vous pouvez faire en sorte que votre application annule l’énumération des opérations de téléchargement/chargement au démarrage durant une session de débogage, si les opérations précédentes n’ont pas d’intérêt pour cette session de débogage. Notez que s’il existe des mises à jour du projet Visual Studio, par exemple des modifications du manifeste de l’application, et si l’application est désinstallée et redéployée, [**GetCurrentUploadsAsync**](https://msdn.microsoft.com/library/windows/apps/hh701149) ne peut pas énumérer les opérations créées à l’aide du déploiement d’application précédent.

Lors de l’utilisation de Transfert en arrière-plan pendant le développement, vous pouvez vous trouver une situation dans laquelle les caches internes des opérations de transfert actives et terminées peuvent être désynchronisés. Cela peut entraîner l’incapacité de démarrer de nouvelles opérations de transfert ou d’interagir avec les opérations existantes et les objets [**BackgroundTransferGroup**](https://msdn.microsoft.com/library/windows/apps/dn279030). Dans certains cas, toute tentative d’interaction avec des opérations existantes peut déclencher un blocage. Cela peut se produire si la propriété [**TransferBehavior**](https://msdn.microsoft.com/library/windows/apps/dn279033) a la valeur **Parallel**. Ce problème ne se produit que dans certains scénarios de développement et n’est pas applicable à l’utilisateur final de votre application.

Quatre scénarios d’utilisation de Visual Studio peuvent provoquer ce problème.

-   Vous créez un projet avec le même nom d’application qu’un projet existant, mais dans un autre langage (en passant du C++ au C#, par exemple).
-   Vous modifiez l’architecture cible (en passant de l’architecture x86 à x64, par exemple) dans un projet existant.
-   Vous modifiez la culture (en passant de la culture neutre à fr-FR, par exemple) dans un projet existant.
-   Vous ajoutez ou supprimez une fonctionnalité du manifeste du package (en ajoutant l’**authentification en entreprise**, par exemple) dans un projet existant.

La maintenance régulière de l’application, notamment les mises à jour du manifeste qui entraînent l’ajout ou la suppression de fonctionnalités, ne déclenche pas ce problème pour les déploiements de votre application destinés à l’utilisateur final.
Pour contourner ce problème, désinstallez complètement toutes les versions de l’application, puis redéployez-la en utilisant le nouveau langage, la nouvelle architecture, la nouvelle culture ou la nouvelle fonctionnalité. Vous pouvez l’effectuer via l’**écran de démarrage** ou à l’aide de PowerShell et de l’applet de commande **Remove-AppxPackage**.

## <a name="exceptions-in-windowsnetworkingbackgroundtransfer"></a>Exceptions dans Windows.Networking.BackgroundTransfer
Une exception est levée quand une chaîne d’URI non valide est transmise au constructeur pour l’objet [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998).

**.NET:** le type [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) apparaît en tant que [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) en C# et VB.

En C# et Visual Basic, cette erreur peut être évitée en utilisant la classe [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) dans .NET 4.5 et l’une des méthodes [**System.Uri.TryCreate**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.trycreate.aspx) pour tester la chaîne envoyée par l’utilisateur de l’application avant la construction de l’URI.

En C++, aucune méthode ne permet d’essayer et d’analyser une chaîne passée à un URI. Si une application obtient une entrée de l’utilisateur pour la classe [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998), le constructeur doit se trouver dans un bloc try/catch. Si une exception est levée, l’application peut notifier l’utilisateur et demander un nouveau nom d’hôte.

L’espace de noms [**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) propose des méthodes d’assistance pratiques et utilise des énumérations dans l’espace de noms [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) pour la gestion des erreurs. Ceci peut s’avérer utile pour gérer différemment certaines exceptions réseau dans votre application.

Une erreur rencontrée sur une méthode asynchrone dans l’espace de noms [**Windows.Networking.backgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) est retournée en tant que valeur **HRESULT**. La méthode [**BackgroundTransferError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701093) sert à convertir une erreur réseau résultant d’une opération de transfert en arrière-plan en valeur d’énumération [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh747818). La plupart des valeurs d’énumération **WebErrorStatus** correspondent à une erreur retournée par l’opération cliente HTTP ou FTP native. Une application peut filtrer sur des valeurs d’énumération **WebErrorStatus** spécifiques pour modifier son comportement en fonction de la cause de l’exception.

Pour les erreurs de validation de paramètre, une application peut également utiliser la valeur **HRESULT** à partir de l’exception pour obtenir des informations plus détaillées sur l’erreur à l’origine de l’exception. Les valeurs **HRESULT** possibles sont répertoriées dans le fichier d’en-tête *Winerror.h*. Pour la plupart des erreurs de validation de paramètre, la valeur **HRESULT** renvoyée est **E\_INVALIDARG**.

## <a name="important-apis"></a>API importantes
* [**Windows.Networking.BackgroundTransfer**](/uwp/api/windows.networking.backgroundtransfer)
* [**Windows.Foundation.Uri**](/uwp/api/Windows.Foundation.Uri)
* [**Windows.Networking.Sockets**](/uwp/api/Windows.Networking.Sockets)
