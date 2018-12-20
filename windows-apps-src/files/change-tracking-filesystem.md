---
title: Suivre les modifications de système de fichiers en arrière-plan
description: Décrit comment effectuer le suivi des modifications dans les fichiers et dossiers en arrière-plan aux utilisateurs les déplacement dans le système.
ms.date: 12/19/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b0ec7762fd64f0f0b8de65faa1aaf079bdaba3a3
ms.sourcegitcommit: 1cf708443d132306e6c99027662de8ec99177de6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/20/2018
ms.locfileid: "8980287"
---
# <a name="track-file-system-changes-in-the-background"></a>Suivre les modifications de système de fichiers en arrière-plan

**API importantes**

-   [**StorageLibraryChangeTracker**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker)
-   [**StorageLibraryChangeReader**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader)
-   [**StorageLibraryChangedTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger)
-   [**StorageLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary)

La classe [**StorageLibraryChangeTracker**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker) permet aux applications de suivre les modifications dans les fichiers et dossiers que les utilisateurs les déplacement dans le système. À l’aide de la classe **StorageLibraryChangeTracker** , une application peut suivre:

- Notamment ajouter des opérations de fichier, supprimer, modifier.
- Opérations de dossier telles que des déplacements et des suppressions.
- Les fichiers et dossiers déplacement sur le lecteur.

Utilisez ce guide pour apprendre le modèle de programmation pour travailler avec la modification de la mise hors tension, d’afficher un exemple de code et comprendre les différents types d’opérations sur les fichiers qui sont suivies par **StorageLibraryChangeTracker**.

**StorageLibraryChangeTracker** fonctionne pour les bibliothèques de l’utilisateur, ou pour n’importe quel dossier sur l’ordinateur local. Cela inclut des lecteurs secondaires ou des lecteurs amovibles, mais n’inclut pas de lecteurs NAS ou des lecteurs réseau.

## <a name="using-the-change-tracker"></a>À l’aide de la modification de la mise hors tension

Le dispositif de suivi de modification est implémentée sur le système comme un tampon circulaire stockant les opérations de système de fichiers *N* dernières. Les applications sont en mesure de lire les modifications sur la mémoire tampon et de les traiter ensuite dans leurs propres expériences. Une fois que l’application a terminé avec les modifications marque les modifications comme traité, elle ne sera jamais les afficher à nouveau.

Pour utiliser le dispositif de suivi des modifications sur un dossier, procédez comme suit:

1. Activer le suivi des modifications pour le dossier.
2. Attendez que les modifications.
3. Lire les modifications.
4. Accepter les modifications.

Les sections suivantes en revue chacune des étapes avec quelques exemples de code. L’exemple de code complet est fourni à la fin de l’article.

### <a name="enable-the-change-tracker"></a>Activer la mise hors tension de modification

La première chose que l’application doit faire consiste à indiquer au système qu’il souhaite dans une bibliothèque donnée de suivi des modifications. Il fait cela en appelant la méthode [**Activer**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) sur le dispositif de suivi des modifications pour la bibliothèque d’intérêt.

```csharp
StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
videoTracker.Enable();
```

Quelques remarques importantes:

- Assurez-vous que votre application est autorisée à la bibliothèque appropriée dans le manifeste avant de créer l’objet [**StorageLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) . Pour plus d’informations, consultez [Les autorisations d’accès aux fichiers](https://docs.microsoft.com/en-us/windows/uwp/files/file-access-permissions) .
- [**Activer**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) est thread-safe, ne rétablit pas le pointeur et peut être appelée autant de fois que vous le souhaitez (plus loin dans cette rubrique).

![L’activation d’un dispositif de suivi des modifications vide](images/changetracker-enable.png)

### <a name="wait-for-changes"></a>Attendre des modifications

Une fois que le dispositif de suivi de modification est initialisée, elle commence à enregistrer toutes les opérations qui se produisent au sein d’une bibliothèque, même lorsque l’application n’est pas en cours d’exécution. Les applications peuvent s’inscrire pour être activée chaque fois qu’il est une modification en inscrivant pour l’événement [**StorageLibraryChangedTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger) .

![Modifications ajoutées à la modification de la mise hors tension sans que l’application leur lecture](images/changetracker-waiting.png)

### <a name="read-the-changes"></a>Lire les modifications

L’application peut ensuite rechercher d’éventuelles modifications à partir de la modification de la mise hors tension et recevoir une liste des modifications apportées depuis la dernière fois, qu'il extrait. Le code ci-dessous montre comment obtenir une liste des modifications à partir de la modification de la mise hors tension.

```csharp
StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
videosLibrary.ChangeTracker.Enable();
StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
IReadOnlyList changeSet = await changeReader.ReadBatchAsync();
```

L’application est alors chargée de traiter les modifications dans sa propre expérience ou d’une base de données en fonction des besoins.

![Les modifications de lecture à partir de la modification de la mise hors tension dans une base de données d’application](images/changetracker-reading.png)

> [!TIP]
> Le deuxième appel à activer consiste à vous défendre contre une condition de concurrence si l’utilisateur ajoute un autre dossier à la bibliothèque pendant que votre application lit modifications. Sans l’appel supplémentaire pour **Activer** le code échoue avec ecSearchFolderScopeViolation (0 x 80070490) si l’utilisateur modifie les dossiers dans leur bibliothèque

### <a name="accept-the-changes"></a>Accepter les modifications

Une fois que l’application a terminé le traitement les modifications, il doit indiquer au système pour ne jamais afficher ces modifications à nouveau en appelant la méthode [**AcceptChangesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync) .

```csharp
await changeReader.AcceptChangesAsync();
```

![Marquage des modifications en tant que lus afin qu’ils ne jamais être affichées à nouveau](images/changetracker-accepting.png)

L’application recevrez désormais nouvelles modifications lors de la lecture de la modification de la mise hors tension à l’avenir.

- Si vous avez apportée entre appelant [**ReadBatchAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.readbatchasync) et [AcceptChangesAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync), le pointeur sera uniquement être avancée de la dernière modification de l’application a vu. Ces autres modifications toujours sera disponibles la prochaine fois qu’elle appelle **ReadBatchAsync**.
- N’accepte ne pas les modifications entraînera le système renvoyer le même ensemble de modifications à la prochaine fois que l’application appelle **ReadBatchAsync**.

## <a name="important-things-to-remember"></a>Points importants à retenir

Lorsque vous utilisez le dispositif de suivi des modifications, il y a plusieurs choses que vous devez garder à l’esprit pour vous assurer que tout fonctionne correctement.

### <a name="buffer-overruns"></a>Dépassements de mémoire tampon

Bien que nous essayez de réserver suffisamment d’espace dans le dispositif de suivi de modification pour contenir toutes les opérations survenant sur le système jusqu'à ce que votre application puisse les lire, il est très facile à Imaginez un scénario dans lequel l’application ne lit pas les modifications avant que la mémoire tampon circulaire remplace proprement dit. En particulier si l’utilisateur est de restauration des données à partir d’une sauvegarde ou de la synchronisation d’une grande collection de photos à partir de leur téléphone de l’appareil photo.

Dans ce cas, **ReadBatchAsync** retourne le code d’erreur [**StorageLibraryChangeType.ChangeTrackingLost**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetype). Si votre application reçoit ce code d’erreur, cela signifie deux choses:

* La mémoire tampon a lui-même remplacé depuis la dernière fois que vous avez examiné il. La meilleure façon d’action consiste à analyser à nouveau la bibliothèque, car toutes les informations du dispositif de suivi sera incomplètes.
* Le dispositif de suivi des modifications ne rétablit pas toutes les modifications plus jusqu'à ce que vous appeliez [**Réinitialiser**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.reset). Une fois que l’application appelle la réinitialisation, le pointeur est déplacé vers la dernière modification et suivi reprend normalement.

Il est rare d’obtenir ces cas, mais nous ne voulons pas le dispositif de suivi de modification à la bulle et occupent trop de stockage dans les scénarios où l’utilisateur déplace un grand nombre de fichiers autour de son disque. Cela devrait permettre des applications réagir aux opérations de système de fichiers volumineux tout en ne pas endommager l’expérience utilisateur dans Windows.

### <a name="changes-to-a-storagelibrary"></a>Modifications apportées à un StorageLibrary

La classe [**StorageLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) existe en tant qu’un groupe virtuel des dossiers racine qui contiennent d’autres dossiers. Pour rapprocher cela avec un dispositif de suivi du changement du système de fichier, nous avons effectué des options suivantes:

- Les modifications apportées aux descendants des dossiers de bibliothèque racine seront représentées dans le dispositif de suivi des modifications. Les dossiers de bibliothèque racine est disponible à l’aide de la propriété de [**dossiers**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders) .
- Ajout ou la suppression des dossiers racine à partir d’un **StorageLibrary** (via [**RequestAddFolderAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestaddfolderasync) et [**RequestRemoveFolderAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestremovefolderasync)) ne crée pas une entrée dans le dispositif de suivi des modifications. Ces modifications peuvent être suivies par le biais de l’événement [**DefinitionChanged**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.definitionchanged) ou en énumérant les dossiers racine dans la bibliothèque à l’aide de la propriété de [**dossiers**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders) .
- Si un dossier dont le contenu déjà qu’il est ajouté à la bibliothèque, il sera pas un changement de notification ou modifier des entrées de mise hors tension générées. Les modifications suivantes aux descendants de ce dossier seront générer des notifications et modifier des entrées de la mise hors tension.

### <a name="calling-the-enable-method"></a>Appeler la méthode d’activation

Les applications doivent appeler [**Activer**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) dès qu’ils démarrent le système de fichiers de suivi et avant chaque énumération des modifications. Cela permet de garantir que toutes les modifications seront capturées par le dispositif de suivi des modifications.  

## <a name="putting-it-together"></a>Synthèse éléments

Voici tout le code qui est utilisé pour inscrire les modifications apportées à partir de la bibliothèque de vidéos et commencer à extraire les modifications apportées à partir de la modification de la mise hors tension.

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
