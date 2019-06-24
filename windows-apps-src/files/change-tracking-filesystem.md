---
title: Suivre les modifications du système de fichiers en arrière-plan
description: Explique comment suivre les modifications apportées aux fichiers et aux dossiers en arrière-plan au fur et à mesure que les utilisateurs les déplacent dans le système.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b0ec7762fd64f0f0b8de65faa1aaf079bdaba3a3
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63807033"
---
# <a name="track-file-system-changes-in-the-background"></a>Suivre les modifications du système de fichiers en arrière-plan

**API importantes**

-   [**StorageLibraryChangeTracker**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker)
-   [**StorageLibraryChangeReader**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader)
-   [**StorageLibraryChangedTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger)
-   [**StorageLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary)

La classe [**StorageLibraryChangeTracker**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker) permet aux applications de suivre les modifications apportées aux fichiers et aux dossiers au fur et à mesure que les utilisateurs les déplacent dans le système. À l'aide de la classe **StorageLibraryChangeTracker**, une application peut suivre :

- les opérations effectuées sur les fichiers, comme les ajouts, les suppressions et les modifications ;
- les opérations effectuées sur les dossiers, comme les changements de nom et les suppressions ;
- les déplacements de fichiers et de dossiers sur le lecteur.

Utilisez ce guide pour vous familiariser avec le modèle de programmation permettant d'utiliser le traceur de modifications, consulter des exemples de code et identifier les différents types d'opérations effectuées sur les fichiers suivis par **StorageLibraryChangeTracker**.

**StorageLibraryChangeTracker** fonctionne pour les bibliothèques utilisateur ou pour tout dossier de l'ordinateur local, comme les lecteurs secondaires ou amovibles, mais pas pour les lecteurs NAS ou les lecteurs réseau.

## <a name="using-the-change-tracker"></a>Utiliser le traceur de modifications

Le traceur de modifications est implémenté sur le système en tant que mémoire tampon circulaire stockant les *N* dernières opérations du système de fichiers. Les applications sont capables de lire les modifications en dehors de la mémoire tampon, puis de les transformer en leurs propres expériences. Lorsque l'application en a fini avec les modifications, elle les marque comme traitées et ne les revoit plus jamais.

Pour utiliser le traceur de modifications sur un dossier, procédez comme suit :

1. Activez le suivi des modifications pour le dossier.
2. Attendez les modifications.
3. Lisez les modifications.
4. Acceptez les modifications.

Les sections suivantes illustrent chacune des étapes avec des exemples de code. L'exemple de code complet est fourni à la fin de l'article.

### <a name="enable-the-change-tracker"></a>Activer le traceur de modifications

L'application doit commencer par informer le système qu'elle est intéressée par le suivi des modifications d'une bibliothèque donnée. Pour ce faire, elle appelle la méthode [**Enable**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) pour la bibliothèque en question sur le traceur de modifications.

```csharp
StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
videoTracker.Enable();
```

Remarques importantes :

- Assurez-vous que votre application dispose de l'autorisation d'accès à la bibliothèque appropriée dans le manifeste avant de créer l'objet [**StorageLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary). Pour plus d'informations, consultez [Autorisations d'accès aux fichiers](https://docs.microsoft.com/en-us/windows/uwp/files/file-access-permissions).
- La méthode [**Enable**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) est thread-safe, elle ne réinitialise pas votre pointeur et elle peut être appelée autant de fois que vous le souhaitez (nous y reviendrons ultérieurement).

![Activation d'un traceur de modifications vide](images/changetracker-enable.png)

### <a name="wait-for-changes"></a>Attendre des modifications

Une fois initialisé, le traceur de modifications commence à enregistrer toutes les opérations effectuées dans une bibliothèque, même lorsque l'application n'est pas en cours d'exécution. Les applications peuvent s'inscrire à l'événement [**StorageLibraryChangedTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger) pour déclencher une activation chaque fois qu'une modification intervient.

![Modifications ajoutées au traceur de modifications sans que l'application ne les lise](images/changetracker-waiting.png)

### <a name="read-the-changes"></a>Lire les modifications

L'application peut ensuite interroger le traceur de modifications et recevoir la liste des modifications apportées depuis la dernière vérification. Le code ci-dessous montre comment obtenir une liste de modifications à partir du traceur de modifications.

```csharp
StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
videosLibrary.ChangeTracker.Enable();
StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
IReadOnlyList changeSet = await changeReader.ReadBatchAsync();
```

L'application est ensuite chargée du traitement des modifications dans sa propre expérience ou base de données, selon les besoins.

![Lecture des modifications à partir du traceur de modifications dans une base de données d'applications](images/changetracker-reading.png)

> [!TIP]
> Le deuxième appel à activer a pour but de se défendre d'une condition de concurrence si l'utilisateur ajoute un autre dossier à la bibliothèque pendant que l'application lit les modifications. Sans appel supplémentaire de la méthode **Enable**, le code échouera (code d'erreur ecSearchFolderScopeViolation - 0x80070490) si l'utilisateur modifie les dossiers de sa bibliothèque.

### <a name="accept-the-changes"></a>Accepter les modifications

Au terme du traitement des modifications, l'application doit indiquer au système de ne plus jamais afficher ces modifications en appelant la méthode [**AcceptChangesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync).

```csharp
await changeReader.AcceptChangesAsync();
```

![Marquage des modifications comme lues afin qu'elles ne s'affichent plus jamais](images/changetracker-accepting.png)

À l'avenir, l'application ne recevra que les nouvelles modifications lors de la lecture du traceur de modifications.

- Si des modifications sont intervenues entre les appels des méthodes [**ReadBatchAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.readbatchasync) et [AcceptChangesAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync), le pointeur sera uniquement avancé vers la modification la plus récente que l'application a vue. Ces autres modifications seront toujours disponibles lors du prochain appel de la méthode **ReadBatchAsync**.
- Si vous n'acceptez pas les modifications, le système renverra le même ensemble de modifications la prochaine fois que l'application appellera la méthode **ReadBatchAsync**.

## <a name="important-things-to-remember"></a>Points importants à retenir

Lorsque vous utilisez le traceur de modifications, vous devez garder certaines choses à l'esprit pour veiller à ce que tout fonctionne bien.

### <a name="buffer-overruns"></a>Dépassements de mémoire tampon

Bien que nous nous efforcions de réserver suffisamment d'espace dans le traceur de modifications pour conserver toutes les opérations qui interviennent sur le système jusqu'à ce que votre application puisse les lire, il est très facile d'imaginer un scénario dans lequel l'application n'a pas le temps de lire les modifications avant que le contenu de la mémoire tampon circulaire ne soit écrasé, en particulier si l'utilisateur restaure des données à partir d'une sauvegarde ou synchronise une grande collection de photos à partir de l'appareil photo de son téléphone.

Dans ce cas, **ReadBatchAsync** renvoie le code d'erreur [**StorageLibraryChangeType.ChangeTrackingLost**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetype). Si votre application reçoit ce code d'erreur, cela signifie deux choses :

* Le contenu de la mémoire tampon a été écrasé depuis la dernière fois que vous l'avez consultée. La meilleure solution consiste à réanalyser la bibliothèque, car toute information provenant du traceur sera incomplète.
* Le traceur de modifications ne renverra plus de modifications tant que vous n'aurez pas appelé [**Reset**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.reset). Une fois Reset appelé par l'application, le pointeur sera déplacé vers la modification la plus récente et le suivi reprendra normalement.

Ces cas devraient être rares, mais dans les scénarios où l'utilisateur déplace un grand nombre de fichiers sur son disque, nous préférons éviter que le traceur de modifications ne gonfle et ne prenne trop d'espace de stockage. Cela devrait permettre aux applications de réagir aux opérations massives du système de fichiers sans nuire à l'expérience client sous Windows.

### <a name="changes-to-a-storagelibrary"></a>Modifications apportées à un objet StorageLibrary

La classe [**StorageLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) est un groupe virtuel de dossiers racine contenant d'autres dossiers. Pour la rapprocher d'un traceur de modifications de système de fichiers, nous avons fait les choix suivants :

- Toute modification apportée aux descendants des dossiers de la bibliothèque racine sera représentée dans le traceur de modifications. Les dossiers de la bibliothèque racine sont accessibles à l'aide de la propriété [**Folders**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders).
- L'ajout ou la suppression de dossiers racine à partir d'un objet **StorageLibrary** (via [**RequestAddFolderAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestaddfolderasync) et [**RequestRemoveFolderAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestremovefolderasync)) ne crée aucune entrée dans le traceur de modifications. Ces modifications peuvent être suivies via l'événement [**DefinitionChanged**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.definitionchanged) ou en énumérant les dossiers racine de la bibliothèque à l'aide de la propriété [**Folders**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders).
- Si un dossier disposant déjà d'un contenu est ajouté à la bibliothèque, il n'y aura aucune notification de modification et aucune entrée ne sera générée dans le traceur de modifications. Toute modification ultérieure apportée aux descendants de ce dossier générera des notifications et modifiera les entrées du traceur.

### <a name="calling-the-enable-method"></a>Appel de la méthode Enable

Les applications doivent appeler [**Enable**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) dès qu'elles entament le suivi du système de fichiers et avant chaque énumération des modifications. Toutes les modifications pourront ainsi être capturées par le traceur de modifications.  

## <a name="putting-it-together"></a>Synthèse

Voici l'intégralité du code utilisé pour l'inscription aux modifications de la bibliothèque vidéo et pour commencer à extraire les modifications à partir du traceur de modifications.

```csharp
private async void EnableChangeTracker()
{
    StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
    StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
    videoTracker.Enable();
}

private async void GetChanges()
{
    StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
    videosLibrary.ChangeTracker.Enable();
    StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
    IReadOnlyList changeSet = await changeReader.ReadBatchAsync();


    //Below this line is for the blog post. Above the line is for the magazine
    foreach (StorageLibraryChange change in changeSet)
    {
        if (change.ChangeType == StorageLibraryChangeType.ChangeTrackingLost)
        {
            //We are in trouble. Nothing else is going to be valid.
            log("Resetting the change tracker");
            videosLibrary.ChangeTracker.Reset();
            return;
        }
        if (change.IsOfType(StorageItemTypes.Folder))
        {
            await HandleFileChange(change);
        }
        else if (change.IsOfType(StorageItemTypes.File))
        {
            await HandleFolderChange(change);
        }
        else if (change.IsOfType(StorageItemTypes.None))
        {
            if (change.ChangeType == StorageLibraryChangeType.Deleted)
            {
                RemoveItemFromDB(change.Path);
            }
        }
    }
    await changeReader.AcceptChangesAsync();
}
```
