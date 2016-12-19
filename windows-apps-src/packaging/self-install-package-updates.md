---
author: mcleanbyron
ms.assetid: 414ACC73-2A72-465C-BD15-1B51CB2334F2
title: "Télécharger et installer des mises à jour de package pour votre application"
description: "Apprenez à marquer des packages comme obligatoires dans le tableau de bord du Centre de développement et à écrire du code dans votre application pour télécharger et installer des mises à jour de packages."
translationtype: Human Translation
ms.sourcegitcommit: b96d4074a8960db314313c612955900c6a05dc48
ms.openlocfilehash: 4da8ffe72435501876a1e859d10a16cf19eb11fd

---
# Télécharger et installer des mises à jour de package pour votre application

\[ Article mis à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

À compter de Windows 10, version 1607, vous pouvez utiliser une API de l’espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) pour rechercher par programmation les mises à jour de packages pour l’application actuelle, puis télécharger et installer les packages mis à jour. Vous pouvez également rechercher les packages qui ont été [marqués comme obligatoires sur le tableau de bord du Centre de développement Windows](#mandatory-dashboard) et désactiver les fonctionnalités dans votre application jusqu’à ce que la mise à jour obligatoire soit installée.

Ces fonctionnalités vous permettent de maintenir à jour votre base d’utilisateurs avec la dernière version de votre application et des services associés.

## Vue d’ensemble des API

Les applications ciblant Windows 10, version 1607 ou ultérieure, peuvent utiliser les méthodes suivantes de la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) pour télécharger et installer les mises à jour de packages.

|  Méthode  |  Description  |
|----------|---------------|
| [GetAppAndOptionalStorePackageUpdatesAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getappandoptionalstorepackageupdatesasync.aspx) | Appelez cette méthode pour obtenir la liste des mises à jour de package disponibles.<br/><br/>**Important**  Il y a un temps de latence pouvant aller jusqu’à un jour entre le moment où un package réussit le processus de certification et le moment où la méthode [GetAppAndOptionalStorePackageUpdatesAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getappandoptionalstorepackageupdatesasync.aspx) reconnaît que la mise à jour du package est disponible pour l’application. |
| [RequestDownloadStorePackageUpdatesAsync](https://msdn.microsoft.com/library/windows/apps/mt706586.aspx) | Appelez cette méthode pour télécharger (mais pas installer) les mises à jour de package disponibles. Ce système d’exploitation affiche une boîte de dialogue qui demande à l’utilisateur l’autorisation de télécharger les mises à jour. |
| [RequestDownloadAndInstallStorePackageUpdatesAsync](https://msdn.microsoft.com/library/windows/apps/mt706585.aspx) | Appelez cette méthode pour télécharger et installer les mises à jour de package disponibles. Le système d’exploitation affiche une boîte de dialogue qui demande à l’utilisateur l’autorisation de télécharger et d’installer les mises à jour. Si vous avez déjà téléchargé les mises à jour de packages en appelant la méthode [RequestDownloadStorePackageUpdatesAsync](https://msdn.microsoft.com/library/windows/apps/mt706586.aspx), cette méthode ignore le processus de téléchargement et ne fait qu’installer les mises à jour.  |

<span/>

Ces méthodes utilisent des objets [StorePackageUpdate](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storepackageupdate.aspx) pour représenter les packages de mise à jour disponibles. Utilisez les propriétés [StorePackageUpdate](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storepackageupdate.aspx) suivantes pour obtenir des informations sur un package de mise à jour.

|  Propriété  |  Description  |
|----------|---------------|
| [Mandatory](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storepackageupdate.mandatory.aspx) | Utilisez cette propriété pour déterminer si le package est marqué comme obligatoire dans le tableau de bord du Centre de développement. |
| [Package](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storepackageupdate.package.aspx) | Utilisez cette propriété pour accéder aux données sous-jacentes relatives au package. |

<span/>

## Exemples de code

Les exemples de code suivants montrent comment télécharger et installer des mises à jour de packages dans votre application. Ces exemples supposent les point suivants :
* Le code s’exécute dans le contexte d’une [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx).
* La **Page** contient un élément [ProgressBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressbar.aspx) nommé ```downloadProgressBar``` pour fournir l’état de l’opération de téléchargement.
* Le fichier de code contient une instruction **using** pour l’espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx).
* Cette application mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour une [application multi-utilisateur](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications), utilisez la méthode [GetForUser](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getforuser.aspx) plutôt que la méthode [GetDefault](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getdefault.aspx) pour obtenir un objet [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx).

<span/>

### Télécharger et installer toutes les mises à jour de package

L’exemple de code suivant montre comment télécharger et installer toutes les mises à jour de packages disponibles.  

```csharp
private StoreContext context = null;

public async Task DownloadAndInstallAllUpdatesAsync()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // Get the updates that are available.
    IReadOnlyList<StorePackageUpdate> updates =
        await context.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count > 0)
    {
        // Alert the user that updates are available and ask for their consent
        // to start the updates.
        MessageDialog dialog = new MessageDialog(
            "Download and install updates now? This may cause the application to exit.", "Download and Install?");
        dialog.Commands.Add(new UICommand("Yes"));
        dialog.Commands.Add(new UICommand("No"));
        IUICommand command = await dialog.ShowAsync();

        if (command.Label.Equals("Yes", StringComparison.CurrentCultureIgnoreCase))
        {
            // Download and install the updates.
            IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> downloadOperation =
                context.RequestDownloadAndInstallStorePackageUpdatesAsync(updates);

            // The Progress async method is called one time for each step in the download
            // and installation process for each package in this request.
            downloadOperation.Progress = async (asyncInfo, progress) =>
            {
                await this.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                () =>
                {
                    downloadProgressBar.Value = progress.PackageDownloadProgress;
                });
            };

            StorePackageUpdateResult result = await downloadOperation.AsTask();
        }
    }
}
```

### Gérer les mises à jour de packages obligatoires

L’exemple de code suivant dérive de l’exemple précédent et montre comment déterminer si des packages de mise à jour ont été [marqués comme obligatoires dans le tableau de bord du Centre de développement Windows](#mandatory-dashboard). En général, vous devez rétrograder l’expérience d’utilisation de votre application normalement pour l’utilisateur si le téléchargement ou l’installation d’une mise à jour de package obligatoire échoue.

```csharp
private StoreContext context = null;

// Downloads and installs package updates in separate steps.
public async Task DownloadAndInstallAllUpdatesAsync()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }  
    // Get the updates that are available.
    IReadOnlyList<StorePackageUpdate> updates =
        await context.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count != 0)
    {
        // Download the packages.
        bool downloaded = await DownloadPackageUpdatesAsync(updates);

        if (downloaded)
        {
            // Install the packages.
            await InstallPackageUpdatesAsync(updates);
        }
    }
}

// Helper method for downloading package updates.
private async Task<bool> DownloadPackageUpdatesAsync(IEnumerable<StorePackageUpdate> updates)
{
    bool downloadedSuccessfully = false;

    IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> downloadOperation =
        this.context.RequestDownloadStorePackageUpdatesAsync(updates);

    // The Progress async method is called one time for each step in the download process for each
    // package in this request.
    downloadOperation.Progress = async (asyncInfo, progress) =>
    {
        await this.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
        () =>
        {
            downloadProgressBar.Value = progress.PackageDownloadProgress;
        });
    };

    StorePackageUpdateResult result = await downloadOperation.AsTask();

    switch (result.OverallState)
    {
        case StorePackageUpdateState.Completed:
            downloadedSuccessfully = true;
            break;
        default:
            // Get the failed updates.
            var failedUpdates = result.StorePackageUpdateStatuses.Where(
                status => status.PackageUpdateState != StorePackageUpdateState.Completed);

            // See if any failed updates were mandatory
            if (updates.Any(u => u.Mandatory && failedUpdates.Any(
                failed => failed.PackageFamilyName == u.Package.Id.FamilyName)))
            {
                // At least one of the updates is mandatory. Perform whatever actions you
                // want to take for your app: for example, notify the user and disable
                // features in your app.
                HandleMandatoryPackageError();
            }
            break;
    }

    return downloadedSuccessfully;
}

// Helper method for installing package updates.
private async Task InstallPackageUpdatesAsync(IEnumerable<StorePackageUpdate> updates)
{
    IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> installOperation =
        this.context.RequestDownloadAndInstallStorePackageUpdatesAsync(updates);

    // The package updates were already downloaded separately, so this method skips the download
    // operatation and only installs the updates; no download progress notifications are provided.
    StorePackageUpdateResult result = await installOperation.AsTask();

    switch (result.OverallState)
    {
        case StorePackageUpdateState.Completed:
            break;
        default:
            // Get the failed updates.
            var failedUpdates = result.StorePackageUpdateStatuses.Where(
                status => status.PackageUpdateState != StorePackageUpdateState.Completed);

            // See if any failed updates were mandatory
            if (updates.Any(u => u.Mandatory && failedUpdates.Any(failed => failed.PackageFamilyName == u.Package.Id.FamilyName)))
            {
                // At least one of the updates is mandatory, so tell the user.
                HandleMandatoryPackageError();
            }
            break;
    }
}

// Helper method for handling the scenario where a mandatory package update fails to
// download or install. Add code to this method to perform whatever actions you want
// to take, such as notifying the user and disabling features in your app.
private void HandleMandatoryPackageError()
{
}
```

<span id="mandatory-dashboard" />
## Rendre une soumission de package obligatoire dans le tableau de bord du Centre de développement

Quand vous créez une soumission de package pour une application qui cible Windows 10, version 1607 ou ultérieure, vous pouvez marquer le package comme obligatoire, ainsi que la date et l’heure auxquelles il devient obligatoire. Lorsque cette propriété est définie et que votre application détecte que la mise à jour du package est disponible à l’aide de l’API décrite précédemment dans cet article, votre application peut déterminer si le package de mise à jour est obligatoire et modifier son comportement jusqu’à ce que la mise à jour soit installée (par exemple, votre application peut désactiver certaines fonctionnalités).

>**Remarque**  L’état obligatoire d’une mise à jour de package n’est pas appliqué par Microsoft et le système d’exploitation ne fournit pas d’interface utilisateur pour indiquer aux utilisateurs qu’une mise à jour d’application obligatoire doit être installée. Les développeurs doivent utiliser le paramètre obligatoire pour appliquer des mises à jour d’application obligatoires dans leur propre code.  

Pour marquer une soumission de package comme obligatoire :

1. Connectez-vous au [tableau de bord du Centre de développement](https://dev.windows.com/overview) et accédez à la page de présentation de votre application.
2. Cliquez sur le nom de la soumission contenant la mise à jour de package que vous voulez rendre obligatoire.
3. Accédez à la page **Packages** de la soumission. Au bas de cette page, sélectionnez **Rendre obligatoire cette mise à jour**, puis choisissez le jour et l’heure auxquels la mise à jour du package devient obligatoire. Cette option s’applique à tous les packages UWP de la soumission.

Pour plus d’informations sur la configuration des packages dans le tableau de bord du Centre de développement, voir [Chargement des packages d’application](https://msdn.microsoft.com/windows/uwp/publish/upload-app-packages).

  >**Remarque**  Si vous créez une [version d’évaluation de package](https://msdn.microsoft.com/windows/uwp/publish/package-flights), vous pouvez marquer les packages comme obligatoires à l’aide d’une interface utilisateur similaire dans la page **Packages** de la version d’évaluation. Dans ce cas, la mise à jour de package obligatoire s’applique uniquement aux clients qui font partie du groupe de versions d’évaluation.



<!--HONumber=Nov16_HO1-->


