---
title: Utilisation du stockage connecté pour supprimer des données
description: Découvrez comment utiliser le stockage connecté pour supprimer les données blob et du conteneur.
ms.assetid: ccf7488c-5d55-480e-b3aa-412220d03104
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox, stockage connecté
ms.localizationpriority: medium
ms.openlocfilehash: 756de46d05cdbf64d85491b4e8c6f783122f2356
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601314"
---
# <a name="use-connected-storage-to-delete-data"></a>Utilisation du stockage connecté pour supprimer des données

Des objets BLOB sont supprimés de façon asynchrone en créant un `ConnectedStorageContainer` dans un `ConnectedStorageSpace` pour un utilisateur et en appelant le `SubmitUpdatesAsync` méthode sur le conteneur qui fournit une liste de chaînes représentant les objets BLOB nommés doit être supprimé pour le paramètre blobsToDelete.

Conteneurs de données sont supprimées de façon asynchrone en créant un `ConnectedStorageContainer` et en appelant son `DeleteContainerAsync` (méthode).

## <a name="to-delete-blob-data-from-connected-storage"></a>Pour supprimer des données blob à partir du stockage connecté

1.  Récupérer un `ConnectedStorageSpace` objet pour l’utilisateur en appelant `GetForUserAsync`.

    Dans l’exemple XDK retourné `ConnectedStorageSpace` objet est ajouté à un mappage permettant de faciliter la gestion de `ConnectedStorageSpace` objets pour plusieurs utilisateurs.

2.  Créer un `ConnectedStorageContainer` objet en appelant `CreateContainer` sur la `ConnectedStorageSpace` objet.
3.  Appelez `SubmitUpdatesAsync` sur la `ConnectedStorageContainer` objet.

## <a name="to-delete-a-container-from-connected-storage"></a>Pour supprimer un conteneur de stockage connecté

1.  Récupérer un `ConnectedStorageSpace` objet pour l’utilisateur en appelant `GetForUserAsync`.

    Dans l’exemple XDK retourné `ConnectedStorageSpace` objet est ajouté à un mappage permettant de faciliter la gestion de `ConnectedStorageSpace` objets pour plusieurs utilisateurs.

2.  Appelez le `DeleteContainerAsync` méthode des méthodes ConnectedStorageSpace.

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

// Delete data from a fixed container/blob name into an IBuffer
void DeleteBlob(User^ user)
{

    //Create a list of blob names to delete
    std::vector<Platform::String^> blobsToDelete;

    //Add the blob name to your Generic Iterable
    blobsToDelete.push_back(L"someBlobName");

    auto blobNames = ref new Platform::Collections::VectorView<Platform::String^>(blobsToDelete);

    auto storageSpace = gConnectedStorageSpaceForUsers->Lookup(user->Id);
    auto container = storageSpace->CreateContainer("Saves/Checkpoint");

    //Delete is occurring asynchronously at this point.

    auto op = container->SubmitUpdatesAsync(nullptr, blobNames);

    op->Completed = ref new AsyncActionCompletedHandler(
        [=](IAsyncAction^ a, Windows::Foundation::AsyncStatus status){
            switch(status)
            {
                case Windows::Foundation::AsyncStatus::Completed:
                //Delete was successful no need for action
                break;
                case Windows::Foundation::AsyncStatus::Error:
                case Windows::Foundation::AsyncStatus::Canceled:
                // Handle failed deletion
            }
     });
}

void DeleteContainer(User^ user)
{
    auto storageSpace = gConnectedStorageSpaceForUsers->Lookup(user->Id);

    auto deleteOperation = storageSpace->DeleteContainerAsync("Saves/Checkpoint");

    deleteOperation->Completed = ref new AsyncActionCompletedHandler(
        [=](IAsyncAction^ a, Windows::Foundation::AsyncStatus status){
            switch(status)
            {
                case Windows::Foundation::AsyncStatus::Completed:
                //Delete was successful no need for action
                break;
                case Windows::Foundation::AsyncStatus::Error:
                case Windows::Foundation::AsyncStatus::Canceled:
                // Handle failed deletion
            }
     });

}
```

Vous pouvez trouver que les API de stockage connecté XDK documentées dans le fichier .chm XDK sous le chemin d’accès : **Xbox un XDK >> référence de l’API >> référence des API de plateforme >> référence de l’API système >> Windows.Xbox.Storage**.
Les API XDK sont également documentées sur le [developer.microsoft.com site](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n).
Le lien vers les API XDK nécessite que vous avez un Account(MSA) Microsoft qui a été activée pour un accès Kit(XDK) de développeur Xbox.
Windows.Xbox.Storage est le nom de l’espace de noms de stockage connecté pour les consoles Xbox One.


## <a name="c-uwp-sample"></a>C#Exemple d’UWP

Tandis que les jeux XDK et les applications UWP peuvent utiliser différentes API, l’API UWP est modélisée d’après l’API XDK très étroitement. Pour supprimer des données, vous devrez toujours suivre les mêmes étapes de base tout en rendant la note de certaines modifications de nom d’espace de noms et classe. Au lieu d’utiliser l’espace de noms `Windows::Xbox::Storage` vous utiliserez `Windows.Gaming.XboxLive.Storage`. La classe `ConnectedStorageSpace`, équivaut à `GameSaveProvider`. La classe `ConnectedStorageContainer` équivaut à `GameSaveContainer`. Ces modifications sont plus détaillées dans la Section de stockage connecté de [portage Xbox Live Code à partir de XDK vers UWP](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md).

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
//string name (name of container to be created) 

//DELETE A BLOB
//form an array of strings containing the blob names you would like to delete.
string[] blobsToDelete = new string[] { c_saveBlobName };

//Call SubmitUpdatesAsync with the dictionary of the names of blobs to delete as the second parameter.
GameSaveOperationResult gameSaveOperationResult = await gameSaveContainer.SubmitUpdatesAsync(null, blobsToDelete, c_saveContainerDisplayName);
//Parameters
//IReadOnlyDictionary<String, IBuffer> blobsToWrite
//IEnumerable<string> blobsToDelete
//string displayName

//Check status of the operation to ensure successful delete.
if(gameSaveOperationResult.Status == GameSaveErrorStatus.Ok)
{
    //Successful Deletion Logic
}
else
{
    //handle failed deletion logic. 
}

//DELETE A SAVE CONTAINER
GameSaveOperationResult deleteContainerResult = await gameSaveProvider.DeleteContainerAsync(c_saveContainerName);
//Parameter
//string name (name of container to be deleted)

//Check status of the operation to ensure successful delete.
if(deleteContainerResult.Status == GameSaveErrorStatus.Ok)
{
    //Successful Deletion Logic
}
else
{
    //handle failed deletion logic. 
}
```

Connecté les API de stockage pour les applications UWP sont documentées dans le [référence d’API de Xbox Live](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage).
Pour voir un autre exemple qui utilise l’extraction de stockage connecté le [Xbox Live API exemples jeu enregistrer le projet](https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK/GameSave).
