---
author: mcleanbyron
ms.assetid: 414ACC73-2A72-465C-BD15-1B51CB2334F2
title: "Télécharger et installer des mises à jour de package pour votre application"
description: "Apprenez à marquer des packages comme obligatoires dans le tableau de bord du Centre de développement et à écrire du code dans votre application pour télécharger et installer des mises à jour de packages."
ms.author: mcleans
ms.date: 03/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 07b8b769cbcaf86bfa70a562de568cab65c91a77
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="download-and-install-package-updates-for-your-app"></a>Télécharger et installer des mises à jour de package pour votre application

\[ Article mis à jour pour les applications UWP sur Windows10. Pour les articles sur Windows8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

À compter de Windows10, version1607, vous pouvez utiliser une API de l’espace de noms [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) pour rechercher par programmation les mises à jour de packages pour l’application actuelle, puis télécharger et installer les packages mis à jour. Vous pouvez également rechercher les packages qui ont été [marqués comme obligatoires sur le tableau de bord du Centre de développement Windows](#mandatory-dashboard) et désactiver les fonctionnalités dans votre application jusqu’à ce que la mise à jour obligatoire soit installée.

Ces fonctionnalités vous permettent de maintenir à jour votre base d’utilisateurs avec la dernière version de votre application et des services associés.

## <a name="api-overview"></a>Vue d’ensemble des API

Les applications ciblant Windows10, version1607 ou ultérieure, peuvent utiliser les méthodes suivantes de la classe [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) pour télécharger et installer les mises à jour de packages.

|  Méthode  |  Description  |
|----------|---------------|
| [GetAppAndOptionalStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext#Windows_Services_Store_StoreContext_GetAppAndOptionalStorePackageUpdatesAsync) | Appelez cette méthode pour obtenir la liste des mises à jour de package disponibles. Notez qu’il y a un temps de latence pouvant aller jusqu’à un jour entre le moment où un package réussit le processus de certification et le moment où la méthode **GetAppAndOptionalStorePackageUpdatesAsync** reconnaît que la mise à jour du package est disponible pour l’application. |
| [RequestDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext#Windows_Services_Store_StoreContext_RequestDownloadStorePackageUpdatesAsync_Windows_Foundation_Collections_IIterable_Windows_Services_Store_StorePackageUpdate__) | Appelez cette méthode pour télécharger (mais pas installer) les mises à jour de package disponibles. Ce système d’exploitation affiche une boîte de dialogue qui demande à l’utilisateur l’autorisation de télécharger les mises à jour. |
| [RequestDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext#Windows_Services_Store_StoreContext_RequestDownloadAndInstallStorePackageUpdatesAsync_Windows_Foundation_Collections_IIterable_Windows_Services_Store_StorePackageUpdate__) | Appelez cette méthode pour télécharger et installer les mises à jour de package disponibles. Le système d’exploitation affiche une boîte de dialogue qui demande à l’utilisateur l’autorisation de télécharger et d’installer les mises à jour. Si vous avez déjà téléchargé les mises à jour de packages en appelant la méthode **RequestDownloadStorePackageUpdatesAsync**, cette méthode ignore le processus de téléchargement et ne fait qu’installer les mises à jour.  |

<span/>

Ces méthodes utilisent des objets [StorePackageUpdate](https://docs.microsoft.com/uwp/api/windows.services.store.storepackageupdate) pour représenter les packages de mise à jour disponibles. Utilisez les propriétés **StorePackageUpdate** suivantes pour obtenir des informations sur un package de mise à jour.

|  Propriété  |  Description  |
|----------|---------------|
| [Mandatory](https://docs.microsoft.com/uwp/api/windows.services.store.storepackageupdate#Windows_Services_Store_StorePackageUpdate_Mandatory) | Utilisez cette propriété pour déterminer si le package est marqué comme obligatoire dans le tableau de bord du Centre de développement. |
| [Package](https://docs.microsoft.com/uwp/api/windows.services.store.storepackageupdate#Windows_Services_Store_StorePackageUpdate_Package) | Utilisez cette propriété pour accéder aux données sous-jacentes relatives au package. |

<span/>

## <a name="code-examples"></a>Exemples de code

Les exemples de code suivants montrent comment télécharger et installer des mises à jour de packages dans votre application. Ces exemples supposent les point suivants:
* Le code s’exécute dans le contexte d’une [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx).
* La **Page** contient un élément [ProgressBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressbar.aspx) nommé ```downloadProgressBar``` pour fournir l’état de l’opération de téléchargement.
* Le fichier de code présente une instruction **using** pour les espaces de noms **Windows.Services.Store**, **Windows.Threading.Tasks** et **Windows.UI.Popups**.
* Cette application mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour une [application multi-utilisateur](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications), utilisez la méthode [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext#Windows_Services_Store_StoreContext_GetForUser_Windows_System_User_) plutôt que la méthode [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext#Windows_Services_Store_StoreContext_GetDefault) pour obtenir un objet **StoreContext**.

<span/>

### <a name="download-and-install-all-package-updates"></a>Télécharger et installer toutes les mises à jour de package

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

### <a name="handle-mandatory-package-updates"></a>Gérer les mises à jour de packages obligatoires

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

### <a name="display-progress-info-for-the-download-and-install"></a>Afficher les informations de progression pour le téléchargement et l’installation

Lorsque vous appelez **RequestDownloadStorePackageUpdatesAsync** ou **RequestDownloadAndInstallStorePackageUpdatesAsync**, vous pouvez affecter un gestionnaire [Progress](https://docs.microsoft.com/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_#Windows_Foundation_IAsyncOperationWithProgress_2_Progress) qui est appelé à chaque étape du téléchargement (ou du processus de téléchargement et d’installation) pour chacun des packages de cette demande. Le gestionnaire reçoit un objet [StorePackageUpdateStatus](https://docs.microsoft.com/uwp/api/windows.services.store.storepackageupdatestatus) qui fournit des informations sur le package de mise à jour qui a déclenché la notification de progression. Les exemples précédents utilisent le champ **PackageDownloadProgress** de l’objet **StorePackageUpdateStatus** pour afficher la progression du processus de téléchargement et d’installation.

Notez que lorsque vous appelez **RequestDownloadAndInstallStorePackageUpdatesAsync** pour télécharger et installer les mises à jour de package dans une opération unique, la valeur du champ **PackageDownloadProgress** passe de 0,0 à 0,8 durant le processus de téléchargement d’un package, puis de 0,8 à 1,0 durant l’installation. Par conséquent, si vous mappez le pourcentage affiché dans votre interface de progression personnalisée directement sur la valeur du champ **PackageDownloadProgress**, votre interface affichera une progression de 80% à l’issue du téléchargement, tandis que le système d’exploitation affiche la boîte de dialogue d’installation. Si vous souhaitez que votre interface de progression personnalisée affiche une valeur de 100% quand le package est téléchargé et prêt à être installé, vous pouvez modifier votre code afin d’affecter la valeur de 100% à votre interface quand le champ **PackageDownloadProgress** atteint la valeur de 0,8.

<span id="mandatory-dashboard" />
## <a name="make-a-package-submission-mandatory-in-the-dev-center-dashboard"></a>Rendre une soumission de package obligatoire dans le tableau de bord du Centre de développement

Quand vous créez une soumission de package pour une application qui cible Windows10, version1607 ou ultérieure, vous pouvez marquer le package comme obligatoire, ainsi que la date et l’heure auxquelles il devient obligatoire. Lorsque cette propriété est définie et que votre application détecte que la mise à jour du package est disponible à l’aide de l’API décrite précédemment dans cet article, votre application peut déterminer si le package de mise à jour est obligatoire et modifier son comportement jusqu’à ce que la mise à jour soit installée (par exemple, votre application peut désactiver certaines fonctionnalités).

> [!NOTE]
> L’état obligatoire d’une mise à jour de package n’est pas appliqué par Microsoft et le système d’exploitation ne fournit pas d’interface utilisateur pour indiquer aux utilisateurs qu’une mise à jour d’application obligatoire doit être installée. Les développeurs doivent utiliser le paramètre obligatoire pour appliquer des mises à jour d’application obligatoires dans leur propre code.  

Pour marquer une soumission de package comme obligatoire:

1. Connectez-vous au [tableau de bord du Centre de développement](https://dev.windows.com/overview) et accédez à la page de présentation de votre application.
2. Cliquez sur le nom de la soumission contenant la mise à jour de package que vous voulez rendre obligatoire.
3. Accédez à la page **Packages** de la soumission. Au bas de cette page, sélectionnez **Rendre obligatoire cette mise à jour**, puis choisissez le jour et l’heure auxquels la mise à jour du package devient obligatoire. Cette option s’applique à tous les packages UWP de la soumission.

Pour plus d’informations sur la configuration des packages dans le tableau de bord du Centre de développement, voir [Charger des packages d’application](../publish/upload-app-packages.md).

  > [!NOTE]
  > Si vous créez une [version d’évaluation de package](../publish/package-flights.md), vous pouvez marquer les packages comme obligatoires à l’aide d’une interface utilisateur similaire dans la page **Packages** de la version d’évaluation. Dans ce cas, la mise à jour de package obligatoire s’applique uniquement aux clients qui font partie du groupe de versions d’évaluation.
