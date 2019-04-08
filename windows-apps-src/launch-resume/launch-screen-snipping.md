---
title: Lancer la capture d’écran
description: Cette rubrique décrit les schémas d’URI ms-screenclip et ms-screensketch. Votre application peut utiliser ces schémas d’URI pour lancer l’application capture & ébauche de projet ou pour ouvrir une nouvelle capture.
ms.date: 08/09/2017
ms.topic: article
keywords: Windows 10, uwp, uri, la capture, ébauche de projet
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 06e988387f574b74d511b14a2ebca24b0a149158
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595384"
---
# <a name="launch-screen-snipping"></a>Lancer la capture d’écran

Le **ms-screenclip :** et **ms-screensketch :** Schémas d’URI permet d’initier la capture d’écran ou la modification des captures d’écran.

## <a name="open-a-new-snip-from-your-app"></a>Ouvrez une nouvelle capture à partir de votre application

Le **ms-screenclip :** URI permet à votre application ouvrir et démarrer une nouvelle capture automatiquement. La capture qui en résulte est copiée dans le Presse-papiers de l’utilisateur, mais n’est pas automatiquement passée à l’application d’ouverture.

**MS-screenclip :** accepte les paramètres suivants :

| Paramètre | Type | Obligatoire | Description |
| --- | --- | --- | --- |
| Source | chaîne | non | Une chaîne de forme libre pour indiquer la source qui a lancé l’URI. |
| delayInSeconds | entier | non | Une valeur entière comprise entre 1 et 30. Spécifie le délai, en secondes, entre l’appel de l’URI et lorsque la capture d’écran commence. |
| callbackformat | chaîne | non | Ce paramètre n’est pas disponible. |

## <a name="launching-the-snip--sketch-app"></a>Lancement de la capture & application de l’ébauche de projet

Le **ms-screensketch :** URI permet de lancer l’application capture & ébauche de projet par programmation et ouvrir une image spécifique dans cette application pour l’annotation.

**MS-screensketch :** accepte les paramètres suivants :

| Paramètre | Type | Obligatoire | Description |
| --- | --- | --- | --- |
| sharedAccessToken | chaîne | non | Un jeton qui identifie le fichier à ouvrir dans l’application capture & ébauche de projet. Récupéré à partir de [SharedStorageAccessManager.AddFile](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile). Si ce paramètre est omis, l’application est lancée sans avoir ouvert un fichier. |
| secondarySharedAccessToken | chaîne | non | Chaîne identifiant un fichier JSON avec métadonnées relatives à la capture. Les métadonnées peuvent inclure un **clipPoints** champ avec un tableau de coordonnées x, y, et/ou un [userActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity). |
| Source | chaîne | non | Une chaîne de forme libre pour indiquer la source qui a lancé l’URI. |
| IsTemporary | bool | non | Si la valeur est True, écran croquis va tenter de supprimer le fichier après son ouverture. |

L’exemple suivant appelle la [LaunchUriAsync](https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_) méthode pour envoyer une image de capture & ébauche de projet à partir de l’application de l’utilisateur.

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```

L’exemple suivant illustre l’un fichier spécifié par le **secondarySharedAccessToken** paramètre de **ms-screensketch** peut contenir :

```json
{
  "clipPoints": [
    {
      "x": 0,
      "y": 0
    },
    {
      "x": 2080,
      "y": 0
    },
    {
      "x": 2080,
      "y": 780
    },
    {
      "x": 0,
      "y": 780
    }
  ],
  "userActivity": "{\"$schema\":\"http://activity.windows.com/user-activity.json\",\"UserActivity\":\"type\",\"1.0\":\"version\",\"cross-platform-identifiers\":[{\"platform\":\"windows_universal\",\"application\":\"Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge\"},{\"platform\":\"host\",\"application\":\"edge.activity.windows.com\"}],\"activationUrl\":\"microsoft-edge:https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"contentUrl\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"visualElements\":{\"attribution\":{\"iconUrl\":\"https://www.microsoft.com/favicon.ico?v2\",\"alternateText\":\"microsoft.com\"},\"description\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"backgroundColor\":\"#FF0078D7\",\"displayText\":\"Use snipping tool to capture screenshots - Windows Help\",\"content\":{\"$schema\":\"http://adaptivecards.io/schemas/adaptive-card.json\",\"type\":\"AdaptiveCard\",\"version\":\"1.0\",\"body\":[{\"type\":\"Container\",\"items\":[{\"type\":\"TextBlock\",\"text\":\"Use snipping tool to capture screenshots - Windows Help\",\"weight\":\"bolder\",\"size\":\"large\",\"wrap\":true,\"maxLines\":3},{\"type\":\"TextBlock\",\"text\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"size\":\"normal\",\"wrap\":true,\"maxLines\":3}]}]}},\"isRoamable\":true,\"appActivityId\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\"}"
}

```
