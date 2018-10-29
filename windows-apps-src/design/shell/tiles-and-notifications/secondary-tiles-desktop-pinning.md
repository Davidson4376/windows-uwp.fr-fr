---
author: vladimp
Description: Windows desktop applications can pin secondary tiles thanks to the Desktop Bridge!
title: Épingler des vignettes secondaires à partir d’une application de bureau
label: Pin secondary tiles from desktop application
template: detail.hbs
ms.author: mijacobs
ms.date: 05/25/2017
ms.topic: article
keywords: Windows10, pont de bureau, vignettes secondaires, épingler, code confidentiel, épinglage, démarrage rapide, exemple de code, exemple, secondarytile, application de bureau, win32, winforms, wpf
ms.localizationpriority: medium
ms.openlocfilehash: 44e37b47e22d10f509afd5d7503fa8f7a43ab365
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5740193"
---
# <a name="pin-secondary-tiles-from-desktop-application"></a>Épingler des vignettes secondaires à partir d’une application de bureau


Grâce au [Pont du bureau](https://developer.microsoft.com/windows/bridges/desktop), les applications de bureau Windows (telles que Win32, Windows Forms et WPF) peuvent épingler des vignettes secondaires!

![Capture d’écran de vignettes secondaires](images/secondarytiles.png)

> [!IMPORTANT]
> **Nécessite Fall Creators Update**: vous devez cibler le Kit de développement logiciel (SDK)16299 et exécuter la Build16299 ou une version ultérieure pour épingler les vignettes secondaires à partir des applications Pont du bureau.

L’ajout d’une vignette secondaire à partir de votre application WinForms ou WPF est très similaire à une application UWP pure. La seule différence est que vous devez spécifier votre handle de fenêtre principale (HWND). Cela est dû au fait que lors de l’épinglage d’une vignette, Windows affiche une boîte de dialogue modale demandant à l’utilisateur de confirmer s’il souhaite épingler la vignette. Si l’application de bureau ne configure pas l’objet SecondaryTile avec la fenêtre du propriétaire, Windows ne sait pas où dessiner la boîte de dialogue et l’opération échoue.


## <a name="package-your-app-with-desktop-bridge"></a>Créer un package de votre application avec le Pont du bureau

Si vous n’avez pas créé de package de votre application avec le Pont du bureau, [vous devez commencez par le faire](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-root) avant de pouvoir utiliser toutes les API UWP.


## <a name="enable-access-to-iinitializewithwindow-interface"></a>Permettre l’accès à l’interface IInitializeWithWindow

Si votre application est écrite dans un langage géré comme C# ou VisualBasic, déclarez l’interface IInitializeWithWindow dans le code de votre application avec l’attribut [ComImport](https://msdn.microsoft.com/library/system.runtime.interopservices.comimportattribute.aspx) et Guid comme indiqué dans l’exemple suivant en C#. Cet exemple suppose que votre fichier de code présente une instruction using pour l’espace de noms System.Runtime.InteropServices.

```csharp
[ComImport]
[Guid("3E68D4BD-7135-4D10-8018-9FB6D9F33FA1")]
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IInitializeWithWindow
{
    void Initialize(IntPtr hwnd);
}
```

Sinon, si vous utilisez C++, ajoutez une référence au fichier d’en-tête **shobjidl.h** dans votre code. Ce fichier d’en-tête contient la déclaration de l’interface *IInitializeWithWindow*.


## <a name="initialize-the-secondary-tile"></a>Initialiser la vignette secondaire

Initialiser un nouvel objet de vignette secondaire exactement comme vous le feriez avec une application UWP normale. Pour en savoir plus sur la création et l’épinglage des vignettes secondaires, voir [Épingler les vignettes secondaires](secondary-tiles-pinning.md).

```csharp
// Initialize the tile with required arguments
SecondaryTile tile = new SecondaryTile(
    "myTileId5391",
    "Display name",
    "myActivationArgs",
    new Uri("ms-appx:///Assets/Square150x150Logo.png"),
    TileSize.Default);
```


## <a name="assign-the-window-handle"></a>Affecter le handle de fenêtre

Il s’agit d’une étape essentielle pour les applications de bureau. Effectuez un cast de l’objet sur un objet [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx). Ensuite, appelez la méthode [IInitializeWithWindow.Initialize](https://msdn.microsoft.com/library/windows/desktop/hh706982.aspx), puis transmettez le handle de la fenêtre que vous souhaitez configurer comme propriétaire pour la boîte de dialogue modale. L’exemple suivant en C# vous explique comment transmettre le handle de la fenêtre principale de votre application à la méthode.

```csharp
// Assign the window handle
IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)tile;
initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
```


## <a name="pin-the-tile"></a>Épingler la vignette secondaire

Enfin, demandez à épingler la vignette comme vous le feriez pour une application UWP normale.

```csharp
// Pin the tile
bool isPinned = await tile.RequestCreateAsync();

// TODO: Update UI to reflect whether user can now either unpin or pin
```


## <a name="send-tile-notifications"></a>Envoyer des notifications par vignette

> [!IMPORTANT]
> **Requiert la Mise à jour d'avril2018 version17134.81 ou ultérieure**: vous devez exécuter la version17134.81 ou version ultérieure pour envoyer des notifications par vignette ou par badge à des vignettes secondaires à partir d’applications Pont du bureau. Avant cette mise à jour de maintenance.81, une exception0x80070490 *Élément introuvable* se produisait lors de l’envoi de notifications par vignette ou par badge aux vignettes secondaires à partir d’applications Pont du bureau.

L'envoi de notifications par vignette ou par badge est identique à celui des applications UWP. Voir [Envoyer une notification par vignette locale](sending-a-local-tile-notification.md) pour commencer.


## <a name="resources"></a>Ressources

* [Exemple de code complet](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Vue d’ensemble des vignettes secondaires](secondary-tiles.md)
* [Épingler les vignettes secondaires (UWP)](secondary-tiles-pinning.md)
* [Pont du bureau](https://developer.microsoft.com/windows/bridges/desktop)
* [Exemples de code du Pont du bureau](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)