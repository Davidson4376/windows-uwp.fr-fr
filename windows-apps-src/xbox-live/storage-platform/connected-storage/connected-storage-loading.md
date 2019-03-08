---
title: Utilisation du stockage connecté pour charger des données
description: Découvrez comment utiliser le stockage connecté pour charger des données.
ms.assetid: c660a456-fe7d-453a-ae7b-9ecaa2ba0a15
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox, stockage connecté
ms.localizationpriority: medium
ms.openlocfilehash: a2f7498e8063e290dc506de72b34d2c77d29b26e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637134"
---
# <a name="use-connected-storage-to-load-data"></a>Utilisation du stockage connecté pour charger des données

Les données sont lues en mode asynchrone à l’aide de la `ReadAsync` ou `GetAsync` connecté la méthode de stockage.

### <a name="to-load-data-from-connected-storage"></a>Pour charger des données à partir du stockage connecté

1.  Récupérer un `ConnectedStorageSpace` pour l’utilisateur en appelant `GetForUserAsync`.

    Dans l’exemple XDK retourné `ConnectedStorageSpace` est ajouté à un mappage permettant de faciliter la gestion de `ConnectedStorageSpace` objets pour plusieurs utilisateurs.

2.  Créer un `ConnectedStorageContainer` en appelant `CreateContainer` sur le `ConnectedStorageSpace`.
3.  Récupérer les données en appelant `ReadAsync` ou `GetAsync` sur le `ConnectedStorageContainer`. `ReadAsync` vous obligent à transmettre dans une mémoire tampon lors de la `GetAsync` alloue les nouvelles mémoires tampon pour stocker les données qui sont en lecture.

## <a name="c-xdk-sample"></a>Exemple de C++ XDK

```cpp
auto gConnectedStorageSpaceForUsers = ref new Platform::Collections::Map<unsigned int, Windows::Xbox::Storage::ConnectedStorageSpace^>();

bool GetUserInputYesOrNo() { return true; };

User^ gCurrentUser;
IBuffer^ WrapRawBuffer(void* ptr, size_t size);

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

void OnLoadCompleted(IAsyncAction^ action, Windows::Foundation::AsyncStatus status);

// Load data from a fixed container/blob name into an IBuffer
void Load(Windows::Storage::Streams::IBuffer^ buffer)
{

    auto reads = ref new Platform::Collections::Map<Platform::String^, Windows::Storage::Streams::IBuffer^>();
    reads->Insert("data", buffer);

    auto storageSpace = gConnectedStorageSpaceForUsers->Lookup(gCurrentUser->Id);
    auto container = storageSpace->CreateContainer("Saves/Checkpoint");

    //Save Data is Loading

    auto op = container->ReadAsync(reads->GetView());

    op->Completed = ref new AsyncActionCompletedHandler(OnLoadCompleted);
}

void OnLoadCompleted(IAsyncAction^ action, Windows::Foundation::AsyncStatus status)
{
    switch (action->Status)
    {
    case Windows::Foundation::AsyncStatus::Completed:
        //Successful load logic here.
        break;

    case Windows::Foundation::AsyncStatus::Error:
    case Windows::Foundation::AsyncStatus::Canceled:
        //Fail logic here
        break;

    default:
        //all other possible values of action->status are also failures, alternate fail logic here. 
        break;
    }
}
```

Vous pouvez trouver que les API de stockage connecté XDK documentées dans le fichier .chm XDK sous le chemin d’accès : **Xbox un XDK >> référence de l’API >> référence des API de plateforme >> référence de l’API système >> Windows.Xbox.Storage**.
Les API XDK sont également documentées sur le [developer.microsoft.com site](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n).
Le lien vers les API XDK nécessite que vous avez un Account(MSA) Microsoft qui a été activée pour un accès Kit(XDK) de développeur Xbox.
Windows.Xbox.Storage est le nom de l’espace de noms de stockage connecté pour les consoles Xbox One.

## <a name="c-uwp-sample"></a>C#Exemple d’UWP

Tandis que les jeux XDK et les applications UWP peuvent utiliser différentes API, l’API UWP est modélisée d’après l’API XDK très étroitement. Pour charger des données, vous devrez toujours suivre les mêmes étapes de base tout en rendant la note de certaines modifications de nom d’espace de noms et classe. Au lieu d’utiliser l’espace de noms `Windows::Xbox::Storage` vous utiliserez `Windows.Gaming.XboxLive.Storage`. La classe `ConnectedStorageSpace`, équivaut à `GameSaveProvider`. La classe `ConnectedStorageContainer` équivaut à `GameSaveContainer`. Ces modifications sont plus détaillées dans la Section de stockage connecté de [portage Xbox Live Code à partir de XDK vers UWP](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md).

```csharp
//Namespace Required
Windows.Gaming.XboxLive.Storage

//Get The User
var users = await Windows.System.User.FindAllAsync();

int intData = 0;
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
    //throw new Exception("Game Save Provider Initialization failed");;
}

//Now you have a GameSaveProvider
//Next you need to call CreateContainer to get a GameSaveContainer

GameSaveContainer gameSaveContainer = gameSaveProvider.CreateContainer(c_saveContainerName);
//Parameter
//string name (name of the GameSaveContainer Created)

//form an array of strings containing the blob names you would like to read.
string[] blobsToRead = new string[] { c_saveBlobName };

// GetAsync allocates a new Dictionary to hold the retrieved data. You can also use ReadAsync
// to provide your own preallocated Dictionary.
GameSaveBlobGetResult result = await gameSaveContainer.GetAsync(blobsToRead);

int loadedData = 0;

//Check status to make sure data was read from the container
if(result.Status == GameSaveErrorStatus.Ok)
{
    //prepare a buffer to receive blob
    IBuffer loadedBuffer;

    //retrieve the named blob from the GetAsync result, place it in loaded buffer.
    result.Value.TryGetValue(c_saveBlobName, out loadedBuffer);

    if(loadedBuffer == null)
    {

        throw new Exception(String.Format("Didn't find expected blob \"{0}\" in the loaded data.", c_saveBlobName));

    }
    DataReader reader = DataReader.FromBuffer(loadedBuffer);
    loadedData = reader.ReadInt32();
}
```

Connecté les API de stockage pour les applications UWP sont documentées dans le [référence d’API de Xbox Live](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage).
Pour voir un autre exemple qui utilise l’extraction de stockage connecté le [Xbox Live API exemples jeu enregistrer le projet](https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK/GameSave).