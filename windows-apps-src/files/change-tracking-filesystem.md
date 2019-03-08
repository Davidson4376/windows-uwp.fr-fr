---
title: Suivre les modifications de système de fichiers en arrière-plan
description: Décrit comment effectuer le suivi des modifications apportées aux fichiers et dossiers en arrière-plan comme les utilisateurs les déplacement dans le système.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b0ec7762fd64f0f0b8de65faa1aaf079bdaba3a3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621574"
---
# <a name="track-file-system-changes-in-the-background"></a>Suivre les modifications de système de fichiers en arrière-plan

**API importantes**

-   [**StorageLibraryChangeTracker**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker)
-   [**StorageLibraryChangeReader**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader)
-   [**StorageLibraryChangedTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger)
-   [**StorageLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary)

Le [ **StorageLibraryChangeTracker** ](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker) classe permet aux applications de suivre les modifications dans les fichiers et dossiers utilisateurs les déplacement dans le système. À l’aide de la **StorageLibraryChangeTracker** (classe), une application peut suivre :

- Fichier des opérations, notamment pour ajouter, supprimer, modifier.
- Opérations de dossier telles que les changements de noms et les suppressions.
- Les fichiers et dossiers déplacement sur le lecteur.

Utilisez ce guide pour apprendre le modèle de programmation pour travailler avec le traceur de modifications, d’afficher un exemple de code et comprendre les différents types d’opérations de fichiers qui sont suivies par **StorageLibraryChangeTracker**.

**StorageLibraryChangeTracker** fonctionne pour les bibliothèques de l’utilisateur, ou pour n’importe quel dossier sur l’ordinateur local. Cela inclut les disques secondaires ou des lecteurs amovibles mais n’inclut pas les lecteurs NAS ou des lecteurs réseau.

## <a name="using-the-change-tracker"></a>À l’aide au traceur de modifications

Le traceur de modifications est implémenté sur le système comme un tampon circulaire stockant la dernière *N* les opérations de système de fichiers. Les applications sont en mesure de lire les modifications hors de la mémoire tampon et à les traiter dans leurs propres expériences. Une fois que l’application est terminée avec les modifications, il marque les modifications comme traité et ne peut pas les afficher à nouveau.

Pour utiliser le traceur de modifications sur un dossier, procédez comme suit :

1. Activer le suivi des modifications pour le dossier.
2. Attendre des modifications.
3. Lire les modifications.
4. Accepter les modifications.

Les sections suivantes décrivent chacune des étapes avec certains exemples de code. L’exemple de code complet est fourni à la fin de l’article.

### <a name="enable-the-change-tracker"></a>Activer le traceur de modifications

La première chose que l’application doit faire est d’informer le système s’il est intéressant dans une bibliothèque donnée de suivi des modifications. Pour ce faire appel à la [ **activer** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) méthode sur le traceur de modifications pour la bibliothèque d’intérêt.

```csharp
StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
videoTracker.Enable();
```

Quelques remarques importantes :

- Assurez-vous que votre application a l’autorisation à la bibliothèque appropriée dans le manifeste avant de créer le [ **StorageLibrary** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) objet. Consultez [autorisations d’accès de fichier](https://docs.microsoft.com/en-us/windows/uwp/files/file-access-permissions) pour plus d’informations.
- [**Activer** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) est thread-safe, ne réinitialise pas le pointeur et peut être appelée autant de fois que vous le souhaitez (plus loin dans cette rubrique).

![L’activation d’un traceur de modifications vide](images/changetracker-enable.png)

### <a name="wait-for-changes"></a>Attendre des modifications

Une fois le traceur de modifications est initialisé, il commence à enregistrer toutes les opérations qui se produisent dans une bibliothèque, même si l’application n’est pas en cours d’exécution. Les applications peuvent s’inscrire pour être activé à tout moment une modification est en vous inscrivant pour le [ **StorageLibraryChangedTrigger** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger) événement.

![Modifications ajoutées pour le traceur de modifications sans l’application les lire](images/changetracker-waiting.png)

### <a name="read-the-changes"></a>Lire les modifications

L’application peut ensuite interroger la présence de modifications depuis le traceur de modifications et recevoir une liste des modifications depuis la dernière fois qu’elle est activée. Le code ci-dessous montre comment obtenir la liste des modifications apportées au traceur de modifications.

```csharp
StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
videosLibrary.ChangeTracker.Enable();
StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
IReadOnlyList changeSet = await changeReader.ReadBatchAsync();
```

L’application est alors chargée de traiter les modifications dans sa propre expérience ou d’une base de données en fonction des besoins.

![Lire les modifications au traceur de modifications dans une base de données d’application](images/changetracker-reading.png)

> [!TIP]
> Le deuxième appel à activer consiste à se défendre contre une condition de concurrence, si l’utilisateur ajoute un autre dossier à la bibliothèque pendant que votre application lit les modifications. Sans l’appel supplémentaire à **activer** le code échoue avec ecSearchFolderScopeViolation (0 x 80070490) si l’utilisateur modifie les dossiers dans leur bibliothèque

### <a name="accept-the-changes"></a>Accepter les modifications

Une fois que l’application est terminée les modifications de traitement, il doit indiquer au système d’ignorer ces modifications en appelant le [ **AcceptChangesAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync) (méthode).

```csharp
await changeReader.AcceptChangesAsync();
```

![Marquage des modifications comme lus afin qu’ils ne seront affichera à nouveau](images/changetracker-accepting.png)

L’application maintenant reçoit les nouvelles modifications lors de la lecture au traceur de modifications à l’avenir.

- Si les modifications ont eu lieu entre l’appel [ **ReadBatchAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.readbatchasync) et [AcceptChangesAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync), le pointeur sera uniquement être avancée à l’application a vu la modification la plus récente. Ces autres modifications seront toujours disponibles la prochaine fois qu’elle appelle **ReadBatchAsync**.
- Ne pas accepter les modifications entraîne le système retourner le même ensemble de modifications à la prochaine fois que l’appelle application **ReadBatchAsync**.

## <a name="important-things-to-remember"></a>Points importants à retenir

Lorsque vous utilisez le traceur de modifications, il existe quelques points que vous devez prendre en compte pour vous assurer que tout fonctionne correctement.

### <a name="buffer-overruns"></a>Dépassements de mémoire tampon

Bien que nous essayons de réserver suffisamment d’espace dans le traceur de modifications pour contenir toutes les opérations qui se produisent sur le système jusqu'à ce que votre application peut les lire, il est très facile d’imaginer un scénario dans lequel l’application ne lit pas les modifications avant que la mémoire tampon circulaire remplace lui-même. En particulier si la restauration des données à partir d’une sauvegarde ou la synchronisation d’une grande collection d’images à partir de leur téléphone de l’appareil photo de l’utilisateur.

Dans ce cas, **ReadBatchAsync** renvoie le code d’erreur [ **StorageLibraryChangeType.ChangeTrackingLost**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetype). Si votre application reçoit ce code d’erreur, cela signifie deux choses :

* La mémoire tampon a remplacé lui-même depuis la dernière fois que vous avez examiné. Le meilleur plan d’action consiste à analyser à nouveau la bibliothèque, étant donné que toutes les informations du dispositif de suivi sera incomplets.
* Le traceur de modifications ne retournera pas plus de modifications jusqu'à ce que vous appeliez [ **réinitialiser**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.reset). Une fois que l’application appelle la réinitialisation, le pointeur est déplacé vers la dernière modification et suivi reprend normalement.

Il est rare d’obtenir ces cas, mais nous ne voulons pas au traceur de modifications à la bulle et n’occupent trop de stockage dans les scénarios où l’utilisateur déplace un grand nombre de fichiers autour de son disque. Cela devrait permettre des applications de réagir aux opérations de système de fichiers volumineux lors ne pas endommager l’expérience utilisateur dans Windows.

### <a name="changes-to-a-storagelibrary"></a>Modifications apportées à un StorageLibrary

Le [ **StorageLibrary** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) classe existe sous la forme d’un groupe virtuel des dossiers racine qui contiennent d’autres dossiers. Pour rapprocher cela avec un traceur de modifications de système de fichiers, nous avons apporté les choix suivants :

- Les modifications apportées aux descendants des dossiers racine bibliothèque seront représentées dans le traceur de modifications. Les dossiers de bibliothèque racine est accessible à l’aide de la [ **dossiers** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders) propriété.
- Ajout ou suppression de dossiers racine à partir d’un **StorageLibrary** (via [ **RequestAddFolderAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestaddfolderasync) et [ **RequestRemoveFolderAsync**  ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestremovefolderasync)) ne crée pas une entrée dans le traceur de modifications. Ces modifications peuvent être suivies par le biais du [ **DefinitionChanged** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.definitionchanged) événement ou en énumérant les dossiers racine dans la bibliothèque à l’aide de la [ **dossiers** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders) propriété.
- Si un dossier avec le contenu déjà est ajouté à la bibliothèque, il sera pas une modification de notification ou la modification des entrées dispositif de suivi générées. Les modifications suivantes aux descendants de ce dossier génère des notifications et modifier les entrées de suivi.

### <a name="calling-the-enable-method"></a>Appel de la méthode d’activation

Les applications doivent appeler [ **activer** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) dès qu’ils démarrent le système de fichiers de suivi et avant chaque énumération des modifications. Cela garantit que toutes les modifications sont capturées par le traceur de modifications.  

## <a name="putting-it-together"></a>Assemblage

Voici tout le code qui est utilisé pour enregistrer les modifications de la vidéothèque et commencer à extraire les modifications à partir d’au traceur de modifications.

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
