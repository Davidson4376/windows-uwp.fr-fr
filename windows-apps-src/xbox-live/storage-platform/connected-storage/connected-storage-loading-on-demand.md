---
title: Connecté lors du chargement de stockage à la demande
description: Découvrez comment charger des données de stockage connecté à la demande, au lieu de tous en même temps.
ms.assetid: a0797a14-c972-4017-864c-c6ba0d5a3363
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox, stockage connecté
ms.localizationpriority: medium
ms.openlocfilehash: 31c1893f09e6d56682b4e718ee8b905ce72c7ad8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631714"
---
# <a name="connected-storage-loading-on-demand"></a>Connecté lors du chargement de stockage à la demande

`GetSyncOnDemandForUserAsync` vous permet de charger les données de reposant sur le cloud à partir d’un espace de stockage connectés « à la demande » au lieu de tous en même temps. Cela peut améliorer les performances sur `GetForUserAsync` pour les cas où le fichier enregistre sont particulièrement volumineux.

## <a name="to-load-data-from-a-connected-storage-space-on-demand"></a>Pour charger des données à partir d’un espace de stockage connectés à la demande

### <a name="1--call-getsyncondemandforuserasync"></a>1 :  Appel `GetSyncOnDemandForUserAsync`

Cela déclenche une synchronisation partielle qui télécharge une liste de conteneurs et leurs métadonnées à partir du cloud, mais pas leur contenu. Cette opération est rapide et, dans des conditions réseau en bon état, l’utilisateur ne doit pas voir tout chargement de l’interface utilisateur.

```cpp
auto op = ConnectedStorageSpace::GetSyncOnDemandForUserAsync(user);
op->Completed = ref new AsyncOperationCompletedHandler<ConnectedStorageSpace^>(
    [=](IAsyncOperation<ConnectedStorageSpace^>^ operation, Windows::Foundation::AsyncStatus status)
{
    switch (status)
    {
    case Windows::Foundation::AsyncStatus::Completed:
        auto storageSpace = operation->GetResults();
        break;
    case Windows::Foundation::AsyncStatus::Error:
    case Windows::Foundation::AsyncStatus::Canceled:
        // Present user option: ?Would you like to continue without saving progress??
        if (GetUserInputYesOrNo())
            SetGameState(LoadSaveState::NO_SAVE_MODE);
        else
            SetGameState(LoadSaveState::RETRY_LOAD);
        break;
    }
});
```

```csharp
var users = await Windows.System.User.FindAllAsync();

GameSaveProvider gameSaveProvider;

GameSaveProviderGetResult gameSaveTask = await GameSaveProvider.GetSyncOnDemandForUserAsync(users[0], context.AppConfig.ServiceConfigurationId); 
//Paramaters
//Windows.System.User user
//string SCID

if(gameSaveTask.Status == GameSaveErrorStatus.Ok)
{
    gameSaveProvider = gameSaveTask.Value;
}
```


### <a name="2--perform-a-container-query-using-getcontainerinfo2async"></a>2 :  Effectuer une requête de conteneur à l’aide `GetContainerInfo2Async`

Ceci renverra une collection de `ContainerInfo2`, qui contient 3 nouveaux champs de métadonnées :

    -   `DisplayName` : N’importe quel nom d’affichage que vous avez écrit à l’aide de la surcharge de `SubmitUpdatesAsync` qui accepte une chaîne de nom d’affichage en tant que paramètre. Nous vous suggérons toujours à l’aide de ce champ pour stocker un nom convivial.
    -   `LastModifiedTime` : La dernière fois ce conteneur a été modifié. Notez que, dans le cas des horodatages locaux et distants en conflit, ceux à distance est utilisé.
    -   `NeedsSync` : Une valeur booléenne indiquant si ce conteneur comporte des données qui doit être téléchargé à partir du cloud.

    À l’aide de ces métadonnées supplémentaires, vous pouvez présenter à l’utilisateur avec des informations fondamentales sur leurs jeux enregistre (y compris le nom, heure de la dernière utilisée et si la sélection d’un nécessitent un téléchargement) sans effectuer un téléchargement complet à partir du cloud.

```cpp
auto containerQuery = storageSpace->CreateContainerInfoQuery(nullptr); //return list of containers in ConnectedStorageSpace
auto queryOperation = containerQuery->GetContainerInfo2Async();

queryOperation->Completed = ref new AsyncOperationCompletedHandler<IVectorView<ContainerInfo2>^ >( 
    [=] (IAsyncOperation<IVectorView<ContainerInfo2>^ >^ operation, Windows::Foundation::AsyncStatus status)
    {
        switch (status)
        {
        case Windows::Foundation::AsyncStatus::Completed:
            // get the resulting vector of container info
            auto infoVector = operation->GetResults();
            break;
        case Windows::Foundation::AsyncStatus::Error:
        case Windows::Foundation::AsyncStatus::Canceled:
            // handle error cases
            break;
        }
    });
```

```csharp
GameSaveContainerInfoQuery infoQuery = gameSaveProvider.createContainerInfoQuery();
GameSaveContainerInfoGetResult containerInfoResult = await infoQuery.GetContainerInfoAsync();
var containerInfoList;

if(containerInfoResult.Status == GameSaveErrorStatus.Ok)
{
    containerInfoList = containerInfoResult.value;
}

// Use the containerInfoList to inform further actions or display container data to user. 
```

### <a name="3--trigger-a-sync"></a>3 :  Déclencher une synchronisation

Un synce de stockage connecté est déclenchée en appelant une de l’API de stockage connectés existants suivants :

**C++**

    -   BlobInfoQueryResult::GetBlobInfoAsync
    -   BlobInfoQueryResult::GetItemCountAsync
    -   ConnectedStorageContainer::GetAsync
    -   ConnectedStorageContainer::ReadAsync
    -   ConnectedStorageSpace::DeleteContainerAsync

**C#**

    -   GameSaveBlobInfoQuery.GetBlobInfoAsync
    -   GameSaveBlobInfoQuery.GetItemCountAsync
    -   GameSaveContainer.GetAsync
    -   GameSaveContainer.ReadAsync
    -   GameSaveProvider.DeleteContainerAsync

Cela entraîne l’utilisateur afficher la barre de l’interface utilisateur et la progression de synchronisation habituelles, comme des données à partir de leur conteneur sélectionné sont téléchargées. Notez que seules les données à partir du conteneur sélectionné sont synchronisées ; données à partir d’autres conteneurs ne sont pas téléchargées.

Lors de l’appel de ces API dans le contexte d’une synchronisation à la demande sur, ces opérations peuvent tous produire les nouveaux codes d’erreur suivant :

-   `ConnectedStorageErrorStatus::ContainerSyncFailed`(`GameSaveErrorStatus.ContainerSyncFailed` dans UWP C# API) : Cette erreur indique que l’opération a échoué et que le conteneur n’a pas pu être synchronisé avec le cloud. La cause la plus probable est conditions de réseau de l’utilisateur a provoqué la synchronisation échoue. Il peut souhaiter essayer à nouveau après leur avez résolu leur réseau, ou ils peuvent choisir d’utiliser un conteneur, ils n’ont pas à la synchronisation. Votre interface utilisateur doit autoriser une de ces options. Aucune boîte de dialogue de nouvelle tentative n’est nécessaire, car ils seront déjà connaissent la boîte de dialogue Nouvelle tentative de l’interface utilisateur système.

-   `ConnectedStorageErrorStatus::ContainerNotInSync`(`GameSaveErrorStatus.ContainerNotInSync` dans UWP C# API) : Cette erreur indique que votre titre incorrectement a tenté d’écrire dans un conteneur non synchronisée. Appel `ConnectedStorageContainer::SubmitUpdatesAsync`(`GameSaveContainer.SubmitUpdatesAsync` dans UWP C# API) sur un conteneur avec l’indicateur NeedsSync défini sur true n’est pas autorisée. Vous devez commencer par lire un conteneur pour déclencher une synchronisation avant d’écrire sur elle. Si vous écrivez dans un conteneur sans effectuer de lecture à partir de celui-ci, il est probable que votre titre comporte un bogue dans lequel vous risquez de perdre progression de l’utilisateur.

Ce comportement est différent de quand un utilisateur lit un hors connexion. Bien que hors connexion, il n’existe aucune indication d’indique si les conteneurs sont synchronisés, donc il revient à l’utilisateur à résoudre les éventuels conflits ultérieurement. Dans ce cas, toutefois, le système sait que l’utilisateur doit se synchroniser, donc il ne permet pas à obtenir dans un état incorrect à l’aide d’un conteneur obsolète (bien que s’ils le souhaitent, ils peuvent toujours redémarrer le titre et lisez-le en mode hors connexion).

### <a name="4--use-the-rest-of-the-connected-storage-api-as-normal"></a>4:  Utilisez le reste de l’API de stockage connecté comme d’habitude

Comportement de stockage connecté reste inchangé lors de la synchronisation à la demande.
