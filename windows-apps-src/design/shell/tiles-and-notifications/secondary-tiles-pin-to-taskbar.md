---
Description: Learn how to pin secondary tiles to taskbar.
title: Épingler les vignettes secondaires à la barre des tâches
label: Pin secondary tiles to taskbar
template: detail.hbs
ms.date: 11/28/2018
ms.topic: article
keywords: Windows 10, uwp, épingler à la barre des tâches, vignette secondaire, épingler les vignettes secondaires à la barre des tâches, raccourci
ms.localizationpriority: medium
ms.openlocfilehash: 7ad322fe371b0e1f3605ffb4c29108a15bb28e0c
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8933173"
---
# <a name="pin-secondary-tiles-to-taskbar"></a>Épingler les vignettes secondaires à la barre des tâches

Tout comme les épingler des vignettes secondaires au menu Démarrer, vous pouvez épingler les vignettes secondaires à la barre des tâches, ce qui donne aux utilisateurs un accès rapide au contenu au sein de votre application.

<img alt="Taskbar pinning" src="../images/taskbar/pin-secondary-ui.png" width="972"/>

> [!IMPORTANT]
> **API d’accès limité**: cette API est une fonctionnalité d’un accès limité. Pour utiliser cette API, veuillez contacter [taskbarsecondarytile@microsoft.com](mailto:taskbarsecondarytile@microsoft.com?Subject=Limited%20Access%20permission%20to%20use%20secondary%20tiles%20on%20taskbar).

> **Nécessite octobre 2018 mise à jour**: vous devez cibler le Kit de développement logiciel 17763 et exécuter la build 17763 ou une version ultérieure pour épingler à la barre des tâches.


## <a name="guidance"></a>Indications

Une vignette secondaire offre un moyen cohérent et efficace permettant aux utilisateurs d’accéder directement à des zones spécifiques au sein d’une application. Même si un utilisateur choisit de «épingler» une vignette secondaire à la barre des tâches ou non, les zones de regroupement d’une application sont déterminées par le développeur. Pour plus d’informations, consultez les [instructions relatives aux vignettes secondaires](secondary-tiles-guidance.md).


## <a name="1-determine-if-api-exists-and-unlock-limited-access"></a>1. Vérifiez la présence d’API et à accès limité de déverrouillage

Les appareils plus anciens n’ont pas la barre des tâches épinglage API (si vous ciblez des versions antérieures de Windows 10). Par conséquent, vous ne devez pas afficher un bouton de code confidentiel sur ces appareils qui ne sont pas en mesure d’épinglage.

En outre, cette fonctionnalité est verrouillée sous accès limité. Pour pouvoir accéder, contactez Microsoft. Appels d’API **[TaskbarManager.RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync#Windows_UI_Shell_TaskbarManager_RequestPinSecondaryTileAsync_Windows_UI_StartScreen_SecondaryTile_)** **[TaskbarManager.IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** et **[TaskbarManager.TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)** échoue avec une exception d’accès refusé. Les applications ne sont pas autorisées à utiliser cette API sans autorisation, et la définition de l’API susceptibles de changer à tout moment.

Utilisez la méthode [ApiInformation.IsMethodPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.ismethodpresent#Windows_Foundation_Metadata_ApiInformation_IsMethodPresent_System_String_System_String_) pour déterminer si les API sont présents. Puis utilisez l’API **[LimitedAccessFeatures](https://docs.microsoft.com/uwp/api/windows.applicationmodel.limitedaccessfeatures)** pour essayer de déverrouillage de l’API.

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


## <a name="2-get-the-taskbarmanager-instance"></a>2. obtenir l’instance TaskbarManager

Les applications UWP peuvent s’exécuter sur un large éventail d’appareils, qui ne prennent pas tous en charge la barre des tâches. Actuellement, seuls les appareils de bureau prennent en charge la barre des tâches. En outre, la présence de la barre des tâches peut provenir et accédez. Pour vérifier si la barre des tâches sont actuellement présent, appelez la méthode **[TaskbarManager.GetDefault](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.getdefault)** et vérifiez que l’instance renvoyée n’est pas null. N’affichez pas un bouton de code confidentiel si la barre des tâches n’est pas présent.

Nous vous recommandons d’exploitation sur l’instance pendant toute la durée d’une seule opération, comme l’épinglage et puis saisissant une nouvelle instance de la prochaine fois que vous devez effectuer une autre opération.

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


## <a name="3-check-whether-your-tile-is-currently-pinned-to-the-taskbar"></a>3. Vérifiez si votre vignette est actuellement épinglée à la barre des tâches

Si votre vignette est déjà épinglé, vous devez afficher un bouton désépingler à la place. Vous pouvez utiliser la méthode **[IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** pour vérifier si votre vignette est actuellement épinglée (les utilisateurs peuvent supprimez-la à tout moment). Dans cette méthode, vous transmettez le **TileId** de la vignette que vous souhaitez connaître est épinglée.

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

L’épinglage à la barre des tâches peut être désactivé par une stratégie de groupe. La propriété [TaskbarManager.IsPinningAllowed](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.ispinningallowed) vous permet de vérifier si l’épinglage est autorisé.

Lorsque l’utilisateur clique sur le bouton de code confidentiel, vous devez vérifier cette propriété, et si elle est false, vous devez afficher une boîte de message indiquant à l’utilisateur qui l’épinglage n’est pas autorisée sur cet ordinateur.

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


## <a name="5-construct-and-pin-your-tile"></a>5. construction et épingler votre vignette

L’utilisateur a cliqué sur votre bouton de code confidentiel, et que vous avez déterminé que les API sont présents, la barre des tâches est présente et épinglage autorisé … temps d’épingler!

Tout d’abord, construire votre vignette secondaire comme vous le feriez lors de l’épinglage au menu Démarrer. Pour en savoir plus sur les propriétés de la vignette secondaire en lisant [épingler les vignettes secondaires au menu Démarrer](secondary-tiles-pinning.md). Toutefois, lors de l’épinglage à la barre des tâches, outre les propriétés requises précédemment Square44x44Logo (c’est le logo utilisé par la barre des tâches) est également requis. Dans le cas contraire, une exception sera levée.

Ensuite, transmettez la vignette à la méthode **[RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync)** . Dans la mesure où il s’agit sous accès limité, cela n’affiche pas une boîte de dialogue de confirmation et ne nécessite pas d’un thread d’interface utilisateur. Toutefois, à l’avenir lorsque cela est ouverte au-delà des accès limité, les appelants n’utilisent ne pas accès limité recevra une boîte de dialogue et être tenus d’utiliser le thread d’interface utilisateur.

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

Cette méthode retourne une valeur booléenne qui indique si votre vignette est désormais épinglée à la barre des tâches. Si votre vignette a déjà été épinglée, la méthode met à jour la vignette existante et retourne la valeur true. Si l’épinglage n’a pas été autorisée ou la barre des tâches n’est pas pris en charge, la méthode retourne la valeur false.


## <a name="enumerate-tiles"></a>Énumérer les vignettes

Pour afficher toutes les tuiles que vous avez créée sont épinglées toujours quelque part (démarrage, la barre des tâches ou les deux), utilisent **[FindAllAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.findallasync)**. Vous pouvez ensuite vérifier si ces vignettes sont épinglées à la barre des tâches et/ou l’écran de démarrage. Si la surface n’est pas pris en charge, ces méthodes retournent la valeur false.

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


## <a name="update-a-tile"></a>Mettre à jour une vignette

Pour mettre à jour une vignette déjà épinglée, vous pouvez utiliser la méthode [**SecondaryTile.UpdateAsync**](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.updateasync) comme décrit dans la [mise à jour d’une vignette secondaire](secondary-tiles-pinning.md#updating-a-secondary-tile).


## <a name="unpin-a-tile"></a>Désépingler une vignette

Votre application doit fournir un bouton désépingler si la vignette est actuellement épinglée. Pour supprimer la vignette, appelez simplement **[TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)**, en passant le **TileId** de la vignette secondaire que vous souhaitez obtenir provisoires.

Cette méthode retourne une valeur booléenne qui indique si votre vignette est épinglée n’est plus à la barre des tâches. Si votre vignette n’a pas été épinglée en premier lieu, cela retourne également la valeur true. Si désépingler n’a pas été autorisée, il renvoie false.

Si votre vignette a été uniquement épinglé à la barre des tâches, cela supprime la vignette dans la mesure où il n’est plus épinglé n’importe quel endroit.

```csharp
var taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager != null)
{
    bool isUnpinned = await taskbarManager.TryUnpinSecondaryTileAsync("myTileId");
}
```


## <a name="delete-a-tile"></a>Supprimer une vignette

Si vous souhaitez désépingler une vignette depuis n’importe où (début, la barre des tâches), utilisez la méthode **[RequestDeleteAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.requestdeleteasync)** .

Cela est approprié pour les cas où le contenu épinglée par l’utilisateur n’est plus applicable. Par exemple, si votre application vous permet d’épingler un ordinateur portable à démarrer et barre des tâches, et l’utilisateur supprime le carnet de notes, vous devez simplement supprimer la vignette associée à l’ordinateur portable.

```csharp
// Initialize a secondary tile with the same tile ID you want removed.
// Or, locate it with FindAllAsync()
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then delete the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="unpin-only-from-start"></a>Désépingler uniquement à partir de l’écran de démarrage

Si vous souhaitez uniquement désépingler une vignette secondaire à partir de l’écran de démarrage en le laissant sur la barre des tâches, vous pouvez appeler la méthode **[StartScreenManager.TryRemoveSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager.tryremovesecondarytileasync)** . La vignette de la même façon seront supprimés si elle n’est plus épinglé à toutes les autres surfaces.

Cette méthode retourne une valeur booléenne qui indique si votre vignette est épinglée n’est plus au menu Démarrer. Si votre vignette n’a pas été épinglée en premier lieu, cela retourne également la valeur true. Si désépingler n’a pas été autorisée ou écran de démarrage n’est pas pris en charge, il retourne la valeur false.

```csharp
await StartScreenManager.GetDefault().TryRemoveSecondaryTileAsync("myTileId");
```


## <a name="resources"></a>Ressources

* [Classe TaskbarManager](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [Épingler les vignettes secondaires au menu Démarrer](secondary-tiles-pinning.md)