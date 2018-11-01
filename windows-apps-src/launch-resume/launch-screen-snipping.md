---
author: QuinnRadich
title: Lancer la capture d’écran
description: Cette rubrique décrit les schémas d’URI ms-screenclip et ms-screensketch. Votre application peut utiliser ces schémas d’URI pour lancer l’application capture & croquis ou d’en ouvrir une nouvelle capture.
ms.author: quradic
ms.date: 8/1/2017
ms.topic: article
keywords: Windows 10, uwp, uri, capture, esquisse
ms.localizationpriority: medium
ms.openlocfilehash: 64df8d9768fa20a6d6819e93fe06904feede6223
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5874253"
---
# <a name="launch-screen-snipping"></a>Lancer la capture d’écran

La **ms-screenclip:** et **ms-screensketch:** schémas d’URI vous permet de lancer la capture d’écran ou de la modification des captures d’écran.

## <a name="open-a-new-snip-from-your-app"></a>Ouvrir une nouvelle capture à partir de votre application

La **ms-screenclip:** URI permet à votre application ouvrir et démarrer une nouvelle capture automatiquement. La capture qui en résulte est copiée dans le Presse-papiers de l’utilisateur, mais n’est pas automatiquement transmise à l’application d’ouverture.

**ms-screenclip:** accepte les paramètres suivants:

| Paramètre | Type | Requis | Description |
| --- | --- | --- | --- |
| source | chaîne | non | Une chaîne de forme libre pour indiquer la source qui a lancé l’URI. |
| delayInSeconds | entier | non | Une valeur entière comprise entre 1 et 30. Spécifie le délai, en secondes complètes, entre l’appel URI et le moment où commence la capture d’écran. |

## <a name="launching-the-snip--sketch-app"></a>Lancer la capture et application de croquis

La **ms-screensketch:** URI vous permet par programmation lancer l’application de capture et Sketch et ouvrez une image spécifique dans cette application pour annotation.

**ms-screensketch:** accepte les paramètres suivants:

| Paramètre | Type | Requis | Description |
| --- | --- | --- | --- |
| sharedAccessToken | chaîne | non | Un jeton qui identifie le fichier à ouvrir dans l’application capture & esquisse. Récupérée à partir de [SharedStorageAccessManager.AddFile](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile). Si ce paramètre est omis, l’application est lancée sans un fichier ouvert. |
| source | chaîne | non | Une chaîne de forme libre pour indiquer la source qui a lancé l’URI. |
| isTemporary | bool | non | Si définie sur True, écran Esquisse essaie de supprimer le fichier après l’avoir ouvert. |

L’exemple suivant appelle la méthode [LaunchUriAsync](https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_) pour envoyer une image à la capture et esquisse à partir de l’application de l’utilisateur.

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```