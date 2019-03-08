---
title: Utilisation du stockage connecté pour enregistrer les données
description: Découvrez comment utiliser le stockage connecté pour enregistrer les données.
ms.assetid: ccf7488c-5d55-480e-b3aa-412220d03104
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox, stockage connecté
ms.localizationpriority: medium
ms.openlocfilehash: 4140e3bbe0f0ab229e3637008e01892f4179292e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617614"
---
# <a name="use-connected-storage-to-save-data"></a>Utilisation du stockage connecté pour enregistrer les données


Les données sont enregistrées de manière asynchrone en créant un `ConnectedStorageContainer` dans un `ConnectedStorageSpace` pour un utilisateur et en appelant le `SubmitUpdatesAsync` méthode sur le conteneur.

> [!IMPORTANT]
> Dépendances de données au sein de conteneurs de stockage connectés sont dangereuses. Par exemple, téléchargement d’un conteneur dans le cloud peut-être effectuer, tandis que l’autre peut rester en file d’attente pour le chargement. Si l’utilisateur est déplacé vers une autre console, l’opération de synchronisation permettrait le premier conteneur synchronisés et d’y accéder sur la deuxième console, sans le premier conteneur en cours présents.

## <a name="to-save-data-to-connected-storage"></a>Pour enregistrer les données dans le stockage connecté

1.  Récupérer un `ConnectedStorageSpace` objet pour l’utilisateur en appelant `GetForUserAsync`.

    Dans l’exemple XDK retourné `ConnectedStorageSpace` objet est ajouté à un mappage permettant de faciliter la gestion de `ConnectedStorageSpace` objets pour plusieurs utilisateurs.

2.  Créer un `ConnectedStorageContainer` objet en appelant `CreateContainer` sur la `ConnectedStorageSpace` objet.
3.  Appelez `SubmitUpdatesAsync` sur le `ConnectedStorageContainer` avec vous de jeux d’enregistrement objet blob de données en tant que le `blobsToWrite` paramètre.

## <a name="c-xdk-sample"></a>Exemple de C++ XDK

```cpp
auto gConnectedStorageSpaceForUsers = ref new Platform::Collections::Map<unsigned int, Windows::Xbox::Storage::ConnectedStorageSpace^>();

bool GetUserInputYesOrNo() {return true;};

User^ gCurrentUser;
IBuffer^ WrapRawBuffer( void* ptr, size_t size );

// Acquire a Connected Storage space for a user. A Connected Storage space is required to manipulate Connected Storage Data.
void PrepareConnectedStorage(User^ user)
{
  auto op = ConnectedStorageSpace::GetForUserAsync(user);
  op->Completed = ref new AsyncOperationCompletedHandler<ConnectedStorageSpace^>(
    [=](IAsyncOperation<ConnectedStorageSpace^>^ operation, Windows::Foundation::AsyncStatus status)
    {
      switch(status)
      {
        case Windows::Foundation::AsyncStatus::Completed:
          gConnectedStorageSpaceForUsers->Insert(user->Id, operation->GetResults());
          break;
        case Windows::Foundation::AsyncStatus::Error:
        case Windows::Foundation::AsyncStatus::Canceled:
          // Present user option: ?Would you like to continue without saving progress??
          if( GetUserInputYesOrNo() )
            //If the users opts yes, continue in offline mode
          else
            //If the users opts no, retry.
          break;
      }
    });
}

enum Color { RED, BLUE };
enum EngineSize { BIG, SMALL };

struct CarData
{
    Color color;
    bool hasWheels;
    bool hasFancyRims;
    EngineSize engineSize;
};


const int MAX_CARS = 10;

struct SaveData
{
    CarData cars[MAX_CARS];
    int numCars;
    int currentCar;
    int cash;
};

SaveData gMySaveData;

void SaveCheckpoint(Windows::Storage::Streams::IBuffer^ buffer, User^ user);

bool ItIsTimeToSaveACheckpoint() {return true;};

void Update()
{
    if (ItIsTimeToSaveACheckpoint())
        SaveCheckpoint(WrapRawBuffer(&gMySaveData,sizeof(SaveData)),gCurrentUser);
}


void SaveCheckpoint(Windows::Storage::Streams::IBuffer^ buffer, User^ user)
{
     auto storageSpace = gConnectedStorageSpaceForUsers->Lookup( user->Id );

     auto container = storageSpace->CreateContainer("Saves/Checkpoint");

     auto updates = ref new Platform::Collections::Map<Platform::String^, Windows::Storage::Streams::IBuffer^>();
     updates->Insert("data", buffer);

     auto op = container->SubmitUpdatesAsync(updates->GetView(), nullptr);

     //Save is happening here asynchronously

     op->Completed = ref new AsyncActionCompletedHandler(
               [=](IAsyncAction^ a, Windows::Foundation::AsyncStatus status){
                   //Save function has completed
                   //This area can be filled with further post save logic.
     });
}
```

Vous pouvez trouver que les API de stockage connecté XDK documentées dans le fichier .chm XDK sous le chemin d’accès : **Xbox un XDK >> référence de l’API >> référence des API de plateforme >> référence de l’API système >> Windows.Xbox.Storage**.
Les API XDK sont également documentées sur le [developer.microsoft.com site](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n).
Le lien vers les API XDK nécessite que vous avez un Account(MSA) Microsoft qui a été activée pour un accès Kit(XDK) de développeur Xbox.
Windows.Xbox.Storage est le nom de l’espace de noms de stockage connecté pour les consoles Xbox One.

## <a name="c-uwp-sample"></a>C#Exemple d’UWP

Tandis que les jeux XDK et les applications UWP peuvent utiliser différentes API, l’API UWP est modélisée d’après l’API XDK très étroitement. Pour enregistrer les données, vous devrez toujours suivre les mêmes étapes de base tout en rendant la note de certaines modifications de nom d’espace de noms et classe. Au lieu d’utiliser l’espace de noms `Windows::Xbox::Storage` vous utiliserez `Windows.Gaming.XboxLive.Storage`. La classe `ConnectedStorageSpace`, équivaut à `GameSaveProvider`. La classe `ConnectedStorageContainer` équivaut à `GameSaveContainer`. Ces modifications sont plus détaillées dans la Section de stockage connecté de [portage Xbox Live Code à partir de XDK vers UWP](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md).

```csharp
//Namespace Required
Windows.Gaming.XboxLive.Storage

//Get The User
var users = await Windows.System.User.FindAllAsync();

int intData = 23;
const string c_saveBlobName = "Jersey";
const string c_saveContainerDisplayName = "GameSave";
const string c_saveContainerName = "GameSaveContainer";
GameSaveProvider gameSaveProvider;

GameSaveProviderGetResult gameSaveTask = await GameSaveProvider.GetForUserAsync(users[0], context.AppConfig.ServiceConfigurationId); 
//Parameters
//Windows.System.User user
//string SCID

if(gameSaveTask.Status == GameSaveErrorStatus.Ok)
{
    gameSaveProvider = gameSaveTask.Value;
}
else
{
    return;
    //throw new Exception("Game Save Provider Initialization failed");
}

//Now you have a GameSaveProvider (formerly ConnectedStorageSpace)
//Next you need to call CreateContainer to get a GameSaveContainer (formerly ConnectedStorageContainer)

GameSaveContainer gameSaveContainer = gameSaveProvider.CreateContainer(c_saveContainerName); // this will create a new named game save container with the name = to the input name
//Parameter
//string name

// To store a value in the container, it needs to be written into a buffer, then stored with
// a blob name in a Dictionary.

DataWriter writer = new DataWriter();

writer.WriteInt32(intData); //some number you want to save, in this case 23.

IBuffer dataBuffer = writer.DetachBuffer();

var blobsToWrite = new Dictionary<string, IBuffer>();

blobsToWrite.Add(c_saveBlobName, dataBuffer);

GameSaveOperationResult gameSaveOperationResult = await gameSaveContainer.SubmitUpdatesAsync(blobsToWrite, null, c_saveContainerDisplayName);
//IReadOnlyDictionary<String, IBuffer> blobsToWrite
//IEnumerable<string> blobsToDelete
//string displayName
```

Connecté les API de stockage pour les applications UWP sont documentées dans le [référence d’API de Xbox Live](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage).
Pour voir un autre exemple qui utilise l’extraction de stockage connecté le [Xbox Live API exemples jeu enregistrer le projet](https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK/GameSave).