---
Description: Découvrez comment épingler des vignettes secondaires à la barre des tâches.
title: Épingler des vignettes secondaires à la barre des tâches
label: Pin secondary tiles to taskbar
template: detail.hbs
ms.date: 11/28/2018
ms.topic: article
keywords: Windows 10, uwp, épingler à la barre des tâches, une vignette secondaire, épingler des vignettes secondaires à la barre des tâches, raccourci
ms.localizationpriority: medium
ms.openlocfilehash: 7ad322fe371b0e1f3605ffb4c29108a15bb28e0c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57591974"
---
# <a name="pin-secondary-tiles-to-taskbar"></a>Épingler des vignettes secondaires à la barre des tâches

Tout comme l’épinglage de vignettes secondaires à démarrer, vous pouvez épingler des vignettes secondaires à la barre des tâches, permettant à vos utilisateurs un accès rapide au contenu au sein de votre application.

<img alt="Taskbar pinning" src="../images/taskbar/pin-secondary-ui.png" width="972"/>

> [!IMPORTANT]
> **Un accès API limité**: Cette API est une fonctionnalité d’un accès limité. Pour utiliser cette API, veuillez contacter [ taskbarsecondarytile@microsoft.com ](mailto:taskbarsecondarytile@microsoft.com?Subject=Limited%20Access%20permission%20to%20use%20secondary%20tiles%20on%20taskbar).

> **Requiert la mise à jour d’octobre 2018**: Vous devez cibler le Kit de développement logiciel 17763 et être en cours d’exécution build 17763 ou une version ultérieure pour l’épingler à la barre des tâches.


## <a name="guidance"></a>Instructions

Une vignette secondaire offre un moyen cohérent et efficace pour les utilisateurs d’accéder directement à des zones spécifiques au sein d’une application. Bien qu’un utilisateur choisit de « épingler » une vignette secondaire pour la barre des tâches ou non, les zones de discussions dans une application sont déterminées par le développeur. Pour plus d’informations, consultez [des conseils de vignette secondaire](secondary-tiles-guidance.md).


## <a name="1-determine-if-api-exists-and-unlock-limited-access"></a>1. Vérifiez la présence d’API et déverrouiller l’accès limité

Appareils plus anciens n’ont pas la barre des tâches épinglage API (si vous ciblez des versions antérieures de Windows 10). Par conséquent, vous ne doit pas afficher un bouton épingler sur ces appareils qui ne sont pas capables d’épinglage.

En outre, cette fonctionnalité est verrouillée sous à accès limité. Pour obtenir l’accès, contactez Microsoft. Appels aux API  **[TaskbarManager.RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync#Windows_UI_Shell_TaskbarManager_RequestPinSecondaryTileAsync_Windows_UI_StartScreen_SecondaryTile_)**,  **[TaskbarManager.IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** et **[TaskbarManager.TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)** échoue avec une exception d’accès refusé. Applications ne sont pas autorisées à utiliser cette API sans autorisation, et la définition d’API peut changer à tout moment.

Utilisez le [ApiInformation.IsMethodPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.ismethodpresent#Windows_Foundation_Metadata_ApiInformation_IsMethodPresent_System_String_System_String_) méthode pour déterminer si les API sont présents. Puis utilisez le **[LimitedAccessFeatures](https://docs.microsoft.com/uwp/api/windows.applicationmodel.limitedaccessfeatures)** API pour essayer de déverrouiller l’API.

```csharp
if (ApiInformation.IsMethodPresent("Windows.UI.Shell.TaskbarManager", "RequestPinSecondaryTileAsync"))
{
    // API present!
    // Unlock the pin to taskbar feature
    var result = LimitedAccessFeatures.TryUnlockFeature(
        "com.microsoft.windows.secondarytilemanagement",
        "<tokenFromMicrosoft>",
        "<publisher> has registered their use of com.microsoft.windows.secondarytilemanagement with Microsoft and agrees to the terms of use.");

    // If unlock succeeded
    if ((result.Status == LimitedAccessFeatureStatus.Available) ||
        (result.Status == LimitedAccessFeatureStatus.AvailableWithoutToken))
    {
        // Continue
    }
    else
    {
        // Don't show pin to taskbar button or call any of the below APIs
    }
}

else
{
    // Don't show pin to taskbar button or call any of the below APIs
}
```


## <a name="2-get-the-taskbarmanager-instance"></a>2. Obtenir l’instance TaskbarManager

Les applications UWP peuvent s’exécuter sur un large éventail d’appareils, qui ne prennent pas tous en charge la barre des tâches. Actuellement, seuls les appareils de bureau prennent en charge la barre des tâches. En outre, la présence de la barre des tâches peut et viennent. Pour vérifier si la barre des tâches sont actuellement présent, appelez le **[TaskbarManager.GetDefault](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.getdefault)** (méthode) et vérifiez que l’instance retournée n’est pas null. Ne pas d’afficher un bouton épingler si la barre des tâches n’est pas présent.

Nous vous recommandons d’exploitation sur l’instance pour la durée d’une opération unique, comme l’épinglage, puis en saisissant une nouvelle instance à la prochaine fois que vous devez effectuer une autre opération.

```csharp
TaskbarManager taskbarManager = TaskbarManager.GetDefault();

if (taskbarManager != null)
{
    // Continue
}
else
{
    // Taskbar not present, don't display a pin button
}
```


## <a name="3-check-whether-your-tile-is-currently-pinned-to-the-taskbar"></a>3. Vérifiez si votre vignette est actuellement épinglé à la barre des tâches

Si votre vignette est épinglée déjà, vous devez afficher un bouton Supprimer à la place. Vous pouvez utiliser la **[IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** méthode permettant de vérifier si votre vignette est épinglée actuellement (les utilisateurs peuvent détacher à tout moment). Dans cette méthode, vous passez le **paramètre TileId** de la vignette que vous souhaitez savoir est épinglé.

```csharp
if (await taskbarManager.IsSecondaryTilePinnedAsync("myTileId"))
{
    // The tile is already pinned. Display the unpin button.
}

else 
{
    // The tile is not pinned. Display the pin button.
}
```


## <a name="4-check-whether-pinning-is-allowed"></a>4. Vérifiez si l’épinglage est autorisée.

Épingler à la barre des tâches peut être désactivée par la stratégie de groupe. Le [TaskbarManager.IsPinningAllowed](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.ispinningallowed) propriété vous permet de vérifier si l’épinglage est autorisé.

Lorsque l’utilisateur clique sur le bouton de votre code confidentiel, vous devez vérifier cette propriété, et si elle est false, une boîte de dialogue informe l’utilisateur que l’épinglage n’est pas autorisée sur cet ordinateur doit être affichée.

```csharp
TaskbarManager taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager == null)
{
    // Display message dialog informing user that taskbar is no longer present, and then hide the button
}

else if (taskbarManager.IsPinningAllowed == false)
{
    // Display message dialog informing user pinning is not allowed on this machine
}

else
{
    // Continue pinning
}
```


## <a name="5-construct-and-pin-your-tile"></a>5. Construire et épingler votre vignette

L’utilisateur a cliqué sur votre bouton épingler, et que vous avez déterminé que les API sont présents, la barre des tâches est présente, et l’épinglage est autorisé... le temps de code confidentiel !

Tout d’abord, construire votre vignette secondaire comme vous le feriez lors de l’épinglage à démarrer. Vous pouvez en savoir plus sur les propriétés de la vignette secondaire en lisant [épingler des vignettes secondaires à démarrer](secondary-tiles-pinning.md). Toutefois, lors de l’épinglage à la barre des tâches, outre les propriétés précédemment requises, Square44x44Logo (c’est le logo utilisé par la barre des tâches) est également nécessaire. Sinon, une exception sera levée.

Ensuite, transmettez la vignette pour le **[RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync)** (méthode). Dans la mesure où il est soumis à accès limité, cela n’affichera pas une boîte de dialogue de confirmation et ne nécessite pas d’un thread d’interface utilisateur. Mais, à l’avenir lorsque cela est ouvert au-delà à accès limité, les appelants non utilisant à accès limité recevra une boîte de dialogue et être obligé d’utiliser le thread d’interface utilisateur.

```csharp
// Initialize the tile (all properties below are required)
SecondaryTile tile = new SecondaryTile("myTileId");
tile.DisplayName = "PowerPoint 2016 (Remote)";
tile.Arguments = "app=powerpoint";
tile.VisualElements.Square44x44Logo = new Uri("ms-appdata:///AppIcons/PowerPoint_Square44x44Logo.png");
tile.VisualElements.Square150x150Logo = new Uri("ms-appdata:///AppIcons/PowerPoint_Square150x150Logo.png");

// Pin it to the taskbar
bool isPinned = await taskbarManager.RequestPinSecondaryTileAsync(tile);
```

Cette méthode retourne une valeur booléenne qui indique si votre vignette est désormais épinglé à la barre des tâches. Si votre vignette a été déjà épinglé, la méthode met à jour la vignette existante et retourne la valeur true. Si l’épinglage n’a pas été autorisé ou la barre des tâches n’est pas pris en charge, la méthode retourne la valeur false.


## <a name="enumerate-tiles"></a>Énumérer des vignettes

Pour voir toutes les vignettes que vous avez créé et sont toujours épinglé quelque part (début, la barre des tâches ou les deux), utilisez  **[FindAllAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.findallasync)**. Vous pouvez ensuite vérifier si ces vignettes sont épinglés à la barre des tâches et/ou le début. Si la surface n’est pas pris en charge, ces méthodes retournent false.

```csharp
var taskbarManager = TaskbarManager.GetDefault();
var startScreenManager = StartScreenManager.GetDefault();

// Look through all tiles
foreach (SecondaryTile tile in await SecondaryTile.FindAllAsync())
{
    if (taskbarManager != null && await taskbarManager.IsSecondaryTilePinnedAsync(tile.TileId))
    {
        // Tile is pinned to the taskbar
    }

    if (startScreenManager != null && await startScreenManager.ContainsSecondaryTileAsync(tile.TileId))
    {
        // Tile is pinned to Start
    }
}
```


## <a name="update-a-tile"></a>Mettre à jour d’une vignette

Pour mettre à jour une vignette épinglée déjà, vous pouvez utiliser la [ **SecondaryTile.UpdateAsync** ](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.updateasync) méthode comme décrit dans [mise à jour d’une vignette secondaire](secondary-tiles-pinning.md#updating-a-secondary-tile).


## <a name="unpin-a-tile"></a>Désépingler une vignette

Votre application doit fournir un bouton détacher si la vignette est épinglée actuellement. Pour désépingler la vignette, appelez simplement  **[TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)**, en passant le **paramètre TileId** de la vignette secondaire vous aimeriez non attachée.

Cette méthode retourne une valeur booléenne qui indique si votre vignette n’est plus épinglé à la barre des tâches. Si votre vignette n’a pas été épinglée en premier lieu, il retourne également true. Si désépinglage n’a pas été autorisée, cela retourne false.

Si votre vignette a été épinglée uniquement à la barre des tâches, cette opération supprimera la vignette dans la mesure où il n’est plus épinglé n’importe où.

```csharp
var taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager != null)
{
    bool isUnpinned = await taskbarManager.TryUnpinSecondaryTileAsync("myTileId");
}
```


## <a name="delete-a-tile"></a>Supprimer une vignette

Si vous souhaitez désépingler une vignette à partir de n’importe où (démarrage, la barre des tâches), utilisez le **[RequestDeleteAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.requestdeleteasync)** (méthode).

Cela est approprié pour les cas où le contenu de l’utilisateur épinglé n’est plus applicable. Par exemple, si votre application vous permet d’épingler un bloc-notes de démarrer et barre des tâches, et l’utilisateur supprime le bloc-notes, vous devez simplement supprimer la vignette associée avec le bloc-notes.

```csharp
// Initialize a secondary tile with the same tile ID you want removed.
// Or, locate it with FindAllAsync()
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then delete the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="unpin-only-from-start"></a>Uniquement détacher du menu Démarrer

Si vous souhaitez uniquement désépingler une vignette secondaire à partir du début tout en les conservant sur la barre des tâches, vous pouvez appeler la **[StartScreenManager.TryRemoveSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager.tryremovesecondarytileasync)** (méthode). Cette opération supprimera de même de la vignette si elle n’est plus épinglé à d’autres surfaces.

Cette méthode retourne une valeur booléenne qui indique si votre vignette n’est plus épinglé à démarrer. Si votre vignette n’a pas été épinglée en premier lieu, il retourne également true. Si désépinglage n’a pas été autorisée ou début n’est pas pris en charge, il retourne la valeur false.

```csharp
await StartScreenManager.GetDefault().TryRemoveSecondaryTileAsync("myTileId");
```


## <a name="resources"></a>Ressources

* [Classe de TaskbarManager](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [Épingler des vignettes secondaires à démarrer](secondary-tiles-pinning.md)