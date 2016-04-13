---
Description: 'Cette rubrique présente des exemples de tâches de codage nécessaires pour réaliser certains des scénarios de protection des données d’entreprise (EDP) relatifs aux fichiers les plus courants.'
MS-HAID: 'dev\_files.protect\_your\_enterprise\_data\_with\_edp'
MSHAttr: 'PreferredLib:/library/windows/apps'
Search.Product: eADQiWindows 10XVcnh
title: 'Utiliser la protection des données d’entreprise (EDP) pour protéger des fichiers'
---

# Utiliser la protection des données d’entreprise (EDP) pour protéger des fichiers

__Remarque__ La stratégie de protection des données d’entreprise (EDP) ne peut pas être appliquée sur Windows 10, version 1511 (build 10586) ou antérieure.

Cette rubrique présente des exemples de tâches de codage nécessaires pour réaliser certains des scénarios de protection des données d’entreprise (EDP) relatifs aux fichiers les plus courants. Pour un aperçu complet du point de vue des développeurs de la manière dont la fonctionnalité EDP est liée aux fichiers, aux flux, au Presse-papiers, à la mise en réseau, aux tâches en arrière-plan et à la protection des données verrouillées, voir [protection des données d’entreprise (EDP)](../enterprise/edp-hub.md).

**Remarque** L’[exemple de protection des données d’entreprise (EDP)](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409) couvre nombre des scénarios expliqués dans cette rubrique.

## Prérequis

-   **Préparation pour la configuration de la fonctionnalité EDP**

    Voir [Configurer votre ordinateur pour EDP](../enterprise/edp-hub.md#set-up-your-computer-for-EDP).

-   **Impliquez-vous dans la création d’une application spécifique aux entreprises**

    Une application qui assure de manière autonome que les données d’entreprise restent sous le contrôle des entreprises est véritablement spécifique aux entreprises. Une application spécifique aux entreprises est plus puissante, plus intelligente, plus flexible et plus fiable qu’une application ordinaire. Votre application indique au système qu’elle est spécifique en déclarant la fonctionnalité **enterpriseDataPolicy** restreinte. Mais configurer une fonctionnalité ne suffit pas à établir la spécificité. Pour en savoir plus, voir [Applications spécifiques aux entreprises](../enterprise/edp-hub.md#enterprise-enlightened-apps).

-   **Comprendre la programmation asynchrone pour les applications pour la plateforme Windows universelle (UWP)**

    Pour savoir comment écrire des applications asynchrones en C\# ou Visual Basic, voir [Appeler des API asynchrones en C\# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Pour savoir comment écrire des applications asynchrones en C++, voir [Programmation asynchrone en C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

## Chemin d’accès à vos dossiers locaux et affichage des fichiers protégés dans l’Explorateur de fichiers


Vous pouvez créer une application pour héberger les exemples de code dans cette rubrique afin de les essayer. Les exemples de code écrivent des fichiers sur le dossier local de votre application, et l’emplacement exact de ce dossier sur le disque est unique à votre application. Pour déterminer l’emplacement du dossier local de votre application, vous pouvez ajouter le code suivant.

```CSharp
// Put a breakpoint on the line after the line of code below. Use the value of localFolderPath
// in File Explorer to find the output file that the later code examples create.
string localFolderPath = ApplicationData.Current.LocalFolder.Path;
```

Une fois que vous avez le chemin d’accès, vous êtes en mesure d’utiliser l’Explorateur de fichiers pour trouver facilement les fichiers que votre application crée. De cette façon, vous êtes en mesure de confirmer qu’ils sont protégés, et qu’ils sont protégés sur l’identité correcte.

Dans l’Explorateur de fichiers, sélectionnez **Modifier les options des dossiers et de recherche** et sur l’onglet **Affichage**, cochez **Afficher les fichiers chiffrés en couleur**. Par ailleurs, utilisez la procédure  **Affichage** &gt; **Ajouter des colonnes** de l’Explorateur de fichiers pour ajouter la colonne **Chiffrement sur** afin de voir l’identité d’entreprise sur laquelle vous protégez vos fichiers.

## Protéger les données d’entreprise dans un nouveau fichier (pour une application interactive)


Il existe de nombreuses manières dont les données d’entreprise peuvent accéder à votre application, par exemple, à partir de certains points de terminaison réseau, de fichiers, du Presse-papiers ou du contrat de partage. Votre application peut également créer des données d’entreprise. Quel que soit le moyen par lequel votre application spécifique se procure des données d’entreprise, elle doit veiller à protéger les données sur l’identité d’entreprise gérée lorsqu’elle conserve les données dans un nouveau fichier.

Les étapes de base consistent à utiliser une API de stockage standard pour créer le fichier, une API EDP pour protéger le fichier sur l’identité d’entreprise, puis (là encore, à l’aide d’API de stockage standard) à écrire dans le fichier. Veillez à protéger le fichier avant d’écrire dessus (comme illustré dans l’exemple ci-dessous). Vous utilisez la méthode [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157) pour protéger le fichier. Et, comme toujours, il est judicieux de protéger sur une identité seulement si cette identité est gérée. Pour plus d’informations sur les raisons qui expliquent cela et sur la manière dont votre application peut déterminer l’identité de l’entreprise dans laquelle elle est exécutée, voir [Confirmer qu’une identité est gérée](../enterprise/edp-hub.md#confirming_an_identity_is_managed).

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void SaveEnterpriseDataToFile(string enterpriseData, string identity)
{
    // You should only protect to an identity that is managed.
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
    StorageFile storageFile = storageFolder.CreateFileAsync("sample.txt",
        CreationCollisionOption.ReplaceExisting);

    // It's important to protect a file *before* writing enterprise data to it.
    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.ProtectAsync(storageFile, identity);

    // If the file is now protected, to the intended identity, then we can write to it.
    if (fileProtectionInfo.Identity == identity &&
        fileProtectionInfo.Status == FileProtectionStatus.Protected)
        await Windows.Storage.FileIO.WriteTextAsync(storageFile, enterpriseData);
}
```

## Protéger les données d’entreprise dans un nouveau fichier (pour une tâche en arrière-plan)


L’API [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157) que nous avons utilisée dans la section précédente est uniquement appropriée pour les applications interactives. Pour une tâche en arrière-plan, votre code peut s’exécuter dans l’écran de verrouillage. L’organisation peut administrer une stratégie sécurisée de protection des données verrouillées, dans laquelle les clés de chiffrement nécessaires pour accéder aux ressources protégées sont temporairement supprimées de la mémoire d’un appareil lorsqu’il est verrouillé. Cela empêche toute fuite de données si l’appareil est perdu. Cette fonctionnalité supprime également les clés associées aux fichiers protégés lorsque leurs descripteurs sont fermés. Toutefois, il est possible de créer des fichiers protégés au cours de la fenêtre de verrouillage (temps écoulé entre le verrouillage et le déverrouillage de l’appareil) et d’y accéder tout en gardant le descripteur de fichier ouvert. Comme **StorageFolder.CreateFileAsync** ferme le descripteur lors de la création du fichier, cet algorithme ne peut pas être utilisé.

1.  Créez un fichier en utilisant **StorageFolder.CreateFileAsync**.
2.  Chiffrez à l’aide de **FileProtectionManager.ProtectAsync**.
3.  Écrivez dans le fichier après l’avoir ouvert.

Comme l’étape 1 implique de fermer le descripteur de fichier (même si l’étape 1 ne fermait pas le descripteur l’étape 2 le ferait), l’étape 3 n’est pas possible, car les clés de chiffrement permettant d’accéder à ce fichier ne sont plus disponibles.

Pour traiter ce scénario, vous pouvez utiliser [**FileProtectionManager.CreateProtectedAndOpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn705153) pour créer un fichier protégé et lui retourner un **IRandomAccessStream**. Ce faisant, les applications peuvent effectuer plusieurs écritures dans un fichier tout en conservant le descripteur de fichier ouvert.

L’exemple de code illustre une application de messagerie très simple qui écrit un nouveau fichier en cas de réception d’un nouveau message. Les applications de messagerie doivent synchroniser la messagerie lorsque l’appareil est verrouillé. Lorsque cette application reçoit un nouveau message, elle crée un fichier en utilisant [**CreateProtectedAndOpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn705153), récupère un flux de sortie et écrit le corps du message dans celui-ci.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;
using Windows.Storage.Streams;

...

private async void SaveEnterpriseDataToFile(string enterpriseData, string identity)
{
    // You should only protect to an identity that is managed.
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;

    ProtectedFileCreateResult protectedFileCreateResult =
        await FileProtectionManager.CreateProtectedAndOpenAsync(storageFolder,
            "sample.txt", identity, CreationCollisionOption.ReplaceExisting);

    // It&#39;s important to successfully protect a file *before* writing enterprise data to it.
    if (protectedFileCreateResult.ProtectionInfo.Identity == identity &&
        protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.Protected)
    {
        using (IOutputStream outputStream =
            protectedFileCreateResult.Stream.GetOutputStreamAt(0))
        {
            using (DataWriter writer = new DataWriter(outputStream))
            {
                writer.WriteString(enterpriseData);
                await writer.StoreAsync();
                await writer.FlushAsync();
            }
        }
    }
    else if (protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.AccessSuspended)
    {
        // Perform any special processing for the access suspended case.
    }
}
```

## Protéger un dossier dont l’objectif est de contenir des données d’entreprise


Si vous souhaitez que tous les éléments d’un dossier soient protégés, vous pouvez également effectuer cette opération. Vous pouvez utiliser [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157) pour protéger un dossier vide. Ensuite, tout fichier ou dossier créé ultérieurement dans le dossier sera également protégé. Vous pouvez protéger un dossier existant, ou en créer un à protéger (l’exemple ci-dessous crée un dossier). Mais dans les deux cas, pour que la protection aboutisse, le dossier doit être vide à ce moment-là. Si ce n’est pas le cas, alors [**FileProtectionInfo.Status**](https://msdn.microsoft.com/library/windows/apps/dn705150) contiendra une valeur de [**FileProtectionStatus.NotProtectable**](https://msdn.microsoft.com/library/windows/apps/dn279147).

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void CreateANewFolderAndProtectItAsync(string folderName, string identity)
{
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
    StorageFolder newStorageFolder =
        await storageFolder.CreateFolderAsync(folderName);

    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.ProtectAsync(newStorageFolder, identity);

    if (fileProtectionInfo.Identity != identity ||
        fileProtectionInfo.Status != FileProtectionStatus.Protected)
    {
        // Protection failed.
    }
}
```

## Protection contre la copie d’un fichier à l’autre


Dans ce scénario, un fichier dont vous savez qu’il est protégé sur l’identité d’entreprise appropriée existe déjà. Vous pouvez répliquer cette protection sur un autre fichier très commodément. Vous n’avez même pas besoin de savoir l’identité dont il s’agit : il vous suffit de savoir que c’est la bonne.

Pour créer une copie simple d’un fichier protégé, vous pouvez appeler [**StorageFile.CopyAsync**](https://msdn.microsoft.com/library/windows/apps/br227190). La copie du fichier obtenue disposera de la même protection que l’original.

Pour protéger un fichier non protégé existant avant d’y écrire des données d’entreprise, au lieu d’appeler [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157) (procédure vue dans un scénario précédent, et pour laquelle vous devez transmettre une identité gérée), vous pouvez appeler [**FileProtectionManager.CopyProtectionAsync**](https://msdn.microsoft.com/library/windows/apps/dn705152) comme illustré dans l’exemple de code.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void CopyProtectionFromOneFileToAnother
    (StorageFile sourceStorageFile, StorageFile targetStorageFile)
{
    bool copyResult = await
        FileProtectionManager.CopyProtectionAsync(sourceStorageFile, targetStorageFile);

    if (!copyResult)
    {
        // Copying failed. To diagnose, you could check the file's status.
        // (call FileProtectionManager.GetProtectionInfoAsync and
        // check FileProtectionInfo.Status).
    }
}
```

## Gérer le refus d’accès à un fichier que vous avez protégé


Dans ce scénario, votre application essaie d’accéder à un fichier (précédemment protégé par votre application) et se voit refuser l’accès. Vous devez vérifier l’état du fichier pour déterminer où se trouve l’erreur. Dans cet exemple de code, l’application appelle l’API [**FileProtectionManager.GetProtectionInfoAsync**](https://msdn.microsoft.com/library/windows/apps/dn705154) pour interroger l’état et déterminer si la raison est la suivante : l’accès au fichier a maintenant été révoqué à la suite de la gestion à distance.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async System.Threading.Tasks.Task<bool> IsFileProtectionStatusRevoked
    (StorageFile storageFile)
{
    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.GetProtectionInfoAsync(storageFile);

    if (fileProtectionInfo.Status == FileProtectionStatus.Revoked)
    {
        // Inform the user that their data has been revoked.
    }
    return fileProtectionInfo.Status == FileProtectionStatus.Revoked;
}
```

## Activer l’application de la stratégie d’interface utilisateur en fonction de l’identité de protection d’un fichier


Lorsque votre application est sur le point d’afficher le contenu d’un fichier protégé (par exemple, un fichier PDF) dans son interface utilisateur, elle doit activer l’application de la stratégie d’interface utilisateur en fonction de l’identité de protection du fichier. Vous devez interroger les informations de protection du fichier et activer l’application de stratégie d’interface utilisateur du système à partir de l’identité récupérée.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void EnableUIPolicyFromFile(StorageFile storageFile)
{
    FileProtectionInfo fileProtectionInfo = 
        await FileProtectionManager.GetProtectionInfoAsync(storageFile);

    if (fileProtectionInfo.Status != FileProtectionStatus.Protected)
    {
        // No policy enforcement required, unless the file is inaccessible
        // (Revoked, ProtectedToOtherIdentity) in which case that should
        // be handled in an app-specific way.
        return;
    }

    ProtectionPolicyManager.TryApplyProcessUIPolicy(fileProtectionInfo.Identity);
}
```

**Remarque** Cet article s’adresse aux développeurs de Windows 10 qui créent des applications pour la plateforme Windows universelle (UWP). Si vous développez une application pour Windows 8.x ou Windows Phone 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Rubriques connexes


[exemple de protection des données d’entreprise (EDP)](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

[**Espace de noms Windows.Security.EnterpriseData**](https://msdn.microsoft.com/library/windows/apps/dn279153)

 

 





<!--HONumber=Mar16_HO5-->


