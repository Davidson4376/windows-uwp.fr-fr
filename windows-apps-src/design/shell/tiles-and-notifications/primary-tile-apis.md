---
author: andrewleader
Description: You can programmatically pin your own app's primary tile to Start, just like you can pin secondary tiles. And you can check whether it's currently pinned.
title: API de vignette principale
label: Primary tile API's
template: detail.hbs
ms.author: wdg-dev-content
ms.date: 05/19/2017
ms.topic: article
keywords: Windows10, uwp, StartScreenManager, épingler la vignette principale, API de vignette principale, vérifier si la vignette est épinglée, vignette dynamique
ms.localizationpriority: medium
ms.openlocfilehash: 8d5c65881552199fce6f90bbf15e4bb2bac950ce
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5833289"
---
# <a name="primary-tile-apis"></a>API de vignette principale
 

Les API de vignette principale vous permettent de vérifier si votre application est actuellement épinglée au menu Démarrer et de demander d’épingler la vignette principale de votre application.

> [!IMPORTANT]
> **Nécessite Creators Update**: vous devez cibler le Kit de développement logiciel (SDK)15063 et exécuter la Build15063 ou une version plus récente pour utiliser les API de vignette principale.

> **API importantes**: [**classe StartScreenManager**](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager), [ContainsAppListEntryAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_ContainsAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_), [RequestAddAppListEntryAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_RequestAddAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_)


## <a name="when-to-use-primary-tile-apis"></a>Quand utiliser les API de vignette principale

Vous attachez beaucoup d’importance à la conception d’une expérience optimale pour la vignette principale de votre application, et vous avez désormais la possibilité de demander à l’utilisateur de l’épingler au menu Démarrer. Néanmoins, avant de nous pencher sur le code, voici quelques éléments à garder à l’esprit lorsque vous concevez votre expérience:

* **Concevez** une expérience utilisateur non perturbatrice et facilement révocable dans votre application avec un appel à l’action «Épingler une vignette dynamique» clair.
* **Expliquez** clairement la valeur de la vignette dynamique de votre application avant de demander à l’utilisateur de l’épingler.
* **Ne demandez pas** à un utilisateur d’épingler la vignette de votre application si la vignette est déjà épinglée ou si l’appareil ne la prend pas en charge (plus d’informations à suivre).
* **Ne demandez pas** plusieurs fois à l’utilisateur d’épingler la vignette de votre application (cela risque de l’agacer).
* **N’appelez pas** l’API Pin sans interaction explicite de l’utilisateur ou lorsque votre application est réduite ou fermée.


## <a name="checking-whether-the-apis-exist"></a>Vérification de l’existence de l’API

Si votre application prend en charge des versions antérieures de Windows10, vous devez vérifier si ces API de vignette principale sont disponibles. Pour ce faire, utilisez ApiInformation. Si les API de vignette principale ne sont pas disponibles, évitez d’exécuter des appels vers celles-ci.

```csharp
if (ApiInformation.IsTypePresent("Windows.UI.StartScreen.StartScreenManager"))
{
    // Primary tile API's supported!
}
else
{
    // Older version of Windows, no primary tile API's
}
```


## <a name="check-if-start-supports-your-app"></a>Vérifier si Démarrer prend en charge votre application

En fonction du menu Démarrer actuel et de votre type d’application, l’épinglage de votre application à l’écran de démarrage actuel peut ne pas être pris en charge. Seuls les ordinateurs et les mobiles prennent en charge l’épinglage de la vignette principale au menu Démarrer. Par conséquent, avant d’afficher une interface utilisateur avec épinglage ou d’exécuter un code confidentiel, vous devez vérifier si votre application est prise en charge par l’écran de démarrage actuel. Si elle ne l’est pas, n’invitez pas l’utilisateur à épingler la vignette.

```csharp
// Get your own app list entry
// (which is always the first app list entry assuming you are not a multi-app package)
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// Check if Start supports your app
bool isSupported = StartScreenManager.GetDefault().SupportsAppListEntry(entry);
```


## <a name="check-whether-youre-currently-pinned"></a>Vérifier si la vignette est actuellement épinglée

Pour savoir si votre vignette principale est actuellement épinglée au menu Démarrer, utilisez la méthode [ContainsAppListEntryAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_ContainsAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_).

```csharp
// Get your own app list entry
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// Check if your app is currently pinned
bool isPinned = await StartScreenManager.GetDefault().ContainsAppListEntryAsync(entry);
```


##  <a name="pin-your-primary-tile"></a>Épingler votre vignette principale

Si votre vignette principale n’est pas épinglée pour le moment et si elle est prise en charge par Démarrer, vous souhaiterez peut-être afficher un conseil aux utilisateurs indiquant qu’ils peuvent épingler votre vignette principale.

> [!NOTE]
> Vous devez appeler cette API à partir d’un thread d’interface utilisateur pendant que votre application est au premier plan, et vous devez appeler uniquement cette APIafterthe utilisateur a demandé intentionnellement le bepinned vignette principale (par exemple, lorsque l’utilisateur a cliqué sur Oui pour votre Conseil sur l’épinglage de la vignette).

Si l’utilisateur clique sur votre bouton pour épingler la vignette principale, vous appelez ensuite la méthode [RequestAddAppListEntryAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_RequestAddAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_) pour demander que votre vignette soit épinglée au menu Démarrer. Cette action affiche une boîte de dialogue demandant à l’utilisateur de confirmer qu’il souhaite épingler votre vignette au menu Démarrer.

Cela retourne une valeur booléenne indiquant si votre vignette est à présent épinglée au menu Démarrer. Si votre vignette a déjà été épinglée, cela retourne immédiatement la valeur true sans afficher la boîte de dialogue à l’utilisateur. Si l’utilisateur clique sur Non dans la boîte de dialogue ou si l’épinglage de votre vignette au menu Démarrer n’est pas pris en charge, la valeur false est retournée. Dans le cas contraire, l’utilisateur a cliqué sur Oui, la vignette a été épinglée, et l’API retourne la valeur true.

```csharp
// Get your own app list entry
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// And pin it to Start
bool isPinned = await StartScreenManager.GetDefault().RequestAddAppListEntryAsync(entry);
```


## <a name="resources"></a>Ressources

* [Exemple de code complet sur GitHub](https://github.com/WindowsNotifications/quickstart-pin-primary-tile)
* [Épingler à la barre des tâches](../pin-to-taskbar.md)
* [Vignettes, badges et notifications](index.md)
* [Documentation sur les vignettes adaptatives](create-adaptive-tiles.md)
