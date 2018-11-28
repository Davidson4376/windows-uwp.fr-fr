---
title: Schéma ms-tonepicker
description: Cette rubrique décrit le schéma ms-tonepicker et comment il affiche un sélecteur pour sélectionner/enregistrer une tonalité et lui attribuer un nom convivial.
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.assetid: 0c17e4fb-7241-4da9-b457-d6d3a7aefccb
ms.localizationpriority: medium
ms.openlocfilehash: 293c755ecaf81ce80fab148a8aca92a7e3a8fa48
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7841429"
---
# <a name="choose-and-save-tones-using-the-ms-tonepicker-uri-scheme"></a>Sélectionner et enregistrer des tonalités à l’aide du schéma d’URI ms-tonepicker

Cette rubrique décrit comment utiliser le schéma d’URI **ms-tonepicker:**. Ce schéma d’URI peut être utilisé pour:
- Vérifier si le sélecteur de tonalités est disponible sur l’appareil.
- Afficher le sélecteur de tonalités pour répertorier les sonneries d’appel, sons système, sonneries de SMS et sonneries d’alarme disponibles; et obtenir un jeton de tonalité qui représente la tonalité sélectionnée par l’utilisateur.
- Afficher l’enregistreur de tonalités, qui utilise un jeton de fichier audio comme entrée et l’enregistre sur l’appareil. Les tonalités enregistrées sont alors accessibles dans le sélecteur de tonalités. Les utilisateurs peuvent également attribuer un nom convivial à la tonalité.
- Convertir un jeton de tonalité en nom convivial de la tonalité.

## <a name="ms-tonepicker-uri-scheme-reference"></a>Référence de schéma d’URI ms-tonepicker:

Ce schéma d’URI ne transmet pas les arguments via la chaîne de schéma d’URI, mais via une classe [ValueSet](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset.aspx). Toutes les chaînes sont sensibles à la casse.

Les sections ci-dessous indiquent quels arguments doivent être transmis pour effectuer la tâche spécifiée.

## <a name="task-determine-if-the-tone-picker-is-available-on-the-device"></a>Tâche: vérifier si le sélecteur de tonalités est disponible sur l’appareil
```cs
var status = await Launcher.QueryUriSupportAsync(new Uri("ms-tonepicker:"),     
                                     LaunchQuerySupportType.UriForResults,
                                     "Microsoft.Tonepicker_8wekyb3d8bbwe");

if (status != LaunchQuerySupportStatus.Available)
{
    // the tone picker is not available
}
```

## <a name="task-display-the-tone-picker"></a>Tâche: afficher le sélecteur de tonalités

Les arguments que vous pouvez transmettre pour afficher le sélecteur de tonalités sont les suivants:

| Paramètre | Type | Obligatoire | Valeurs possibles | Description |
|-----------|------|----------|-------|-------------|
| Action | chaîne | oui | «PickRingtone» | Ouvre le sélecteur de tonalités. |
| CurrentToneFilePath | chaîne | non | Jeton de tonalité existant. | Tonalité à afficher en tant que tonalité actuelle dans le sélecteur de tonalités. Si cette valeur n’est pas définie, la première tonalité de la liste est sélectionnée par défaut.<br>Cela n’est pas réellement un chemin d’accès au fichier. Vous pouvez obtenir une valeur adaptée pour `CurrenttoneFilePath` à partir de la valeur `ToneToken` renvoyée par le sélecteur de tonalités.  |
| TypeFilter | chaîne | non | «Sonneries», «Notifications», «Alarmes», «Aucun» | Sélectionne les tonalités à ajouter au sélecteur. Si aucun filtre n’est spécifié, toutes les tonalités s’affichent. |

Valeurs renvoyées dans [LaunchUriResults.Result](https://msdn.microsoft.com/library/windows/apps/windows.system.launchuriresult.result.aspx):

| Valeurs renvoyées | Type | Valeurs possibles | Description |
|--------------|------|-------|-------------|
| Résultat | Int32 | 0 - succès. <br>1 - opération annulée. <br>7 - paramètres non valides. <br>8 - aucune tonalité correspondant aux critères de filtre. <br>255 - l’action spécifiée n’est pas implémentée. | Résultat de l’opération de sélection. |
| ToneToken | chaîne | Jeton de la tonalité sélectionnée. <br>La chaîne est vide si l’utilisateur sélectionne **par défaut** dans le sélecteur. | Ce jeton peut être utilisé dans une charge utile de notification toast, ou attribué en tant que sonnerie d’appel ou de SMS d’un contact. Le paramètre est renvoyé dans le ValueSet uniquement si la valeur **Résultat** est 0. |
| DisplayName | chaîne | Nom convivial de la tonalité spécifiée. | Une chaîne, visible par l’utilisateur, qui représente la tonalité sélectionnée. Le paramètre est renvoyé dans le ValueSet uniquement si la valeur **Résultat** est 0. |


**Exemple: Ouvrir le sélecteur de tonalités afin que l’utilisateur puisse sélectionner une tonalité**

``` cs
LauncherOptions options = new LauncherOptions();
options.TargetApplicationPackageFamilyName = "Microsoft.Tonepicker_8wekyb3d8bbwe";

ValueSet inputData = new ValueSet() {
    { "Action", "PickRingtone" },
    { "TypeFilter", "Ringtones" } // Show only ringtones
};

LaunchUriResult result = await Launcher.LaunchUriForResultsAsync(new Uri("ms-tonepicker:"), options, inputData);

if (result.Status == LaunchUriStatus.Success)
{
     Int32 resultCode =  (Int32)result.Result["Result"];
     if (resultCode == 0)
     {
         string token = result.Result["ToneToken"] as string;
         string name = result.Result["DisplayName"] as string;
     }
     else
     {
           // handle failure
     }
}
```

## <a name="task-display-the-tone-saver"></a>Tâche: afficher l’enregistreur de tonalités

Les arguments que vous pouvez transmettre pour afficher l’enregistreur de tonalités sont les suivants:

| Paramètre | Type | Obligatoire | Valeurs possibles | Description |
|-----------|------|----------|-------|-------------|
| Action | chaîne | oui | «SaveRingtone» | Ouvre le sélecteur pour enregistrer une sonnerie. |
| ToneFileSharingToken | chaîne | oui | Jeton de partage de fichier [SharedStorageAccessManager](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.aspx) pour l’enregistrement du fichier de la sonnerie. | Enregistre un fichier audio spécifique en tant que sonnerie. Le fichier audio doit être au format mpeg ou x-ms-wma. |
| DisplayName | chaîne | non | Nom convivial de la tonalité spécifiée. | Définit le nom d’affichage à utiliser lors de l’enregistrement de la sonnerie spécifiée. |

Valeurs renvoyées dans [LaunchUriResults.Result](https://msdn.microsoft.com/library/windows/apps/windows.system.launchuriresult.result.aspx):

| Valeurs renvoyées | Type | Valeurs possibles | Description |
|--------------|------|-------|-------------|
| Résultat | Int32 | 0 - succès.<br>1 - opération annulée par l’utilisateur.<br>2 - fichier non valide.<br>3 - type de contenu non valide.<br>4 - le fichier dépasse la taille de sonnerie maximale autorisée (1Mo dans Windows10).<br>5 - le fichier dépasse la longueur maximale autorisée (40secondes).<br>6 - le fichier est protégé par la gestion des droits numériques (DRM).<br>7 - paramètres non valides. | Résultat de l’opération de sélection. |

**Exemple: Enregistrer un fichier de musique local en tant que sonnerie**

``` cs
LauncherOptions options = new LauncherOptions();
options.TargetApplicationPackageFamilyName = "Microsoft.Tonepicker_8wekyb3d8bbwe";

ValueSet inputData = new ValueSet() {
    { "Action", "SaveRingtone" },
    { "ToneFileSharingToken", SharedStorageAccessManager.AddFile(myLocalFile) }
};

LaunchUriResult result = await Launcher.LaunchUriForResultsAsync(new Uri("ms-tonepicker:"), options, inputData);

if (result.Status == LaunchUriStatus.Success)
{
     Int32 resultCode = (Int32)result.Result["Result"];

     if (resultCode == 0)
     {
         // no issues
     }
     else
     {
          switch (resultCode)
          {
             case 2:
              // The specified file was invalid
              break;
              case 3:
              // The specified file's content type is invalid
              break;
              case 4:
              // The specified file was too big
              break;
              case 5:
              // The specified file was too long
              break;
              case 6:
              // The file was protected by DRM
              break;
              case 7:
              // The specified parameter was incorrect
              break;
          }
      }
 }
```

## <a name="task-convert-a-tone-token-to-its-friendly-name"></a>Tâche: convertir un jeton de tonalité en nom convivial de la tonalité

Les arguments que vous pouvez transmettre pour obtenir le nom convivial d’une tonalité sont les suivants:

| Paramètre | Type | Obligatoire | Valeurs possibles | Description |
|-----------|------|----------|-------|-------------|
| Action | chaîne | oui | «GetToneName» | Indique que vous souhaitez obtenir le nom convivial d’une tonalité. |
| ToneToken | chaîne | oui | Jeton de tonalité | Jeton de tonalité à partir duquel vous souhaitez obtenir un nom d’affichage. |

Valeurs renvoyées dans [LaunchUriResults.Result](https://msdn.microsoft.com/library/windows/apps/windows.system.launchuriresult.result.aspx):

| Valeur renvoyée | Type | Valeurs possibles | Description |
|--------------|------|-------|-------------|
| Résultat | Int32 | 0 - opération de sélection réussie.<br>7 - paramètre incorrect (par exemple, aucune valeur ToneToken fournie).<br>9 - erreur lors de la lecture du nom du jeton spécifié.<br>10 - impossible de trouver le jeton de tonalité spécifié. | Résultat de l’opération de sélection.
| DisplayName | chaîne | Nom convivial de la tonalité. | Renvoie le nom d’affichage de la tonalité sélectionnée. Ce paramètre est renvoyé dans le ValueSet uniquement si la valeur **Résultat** est 0. |

**Exemple: Récupérer un jeton de tonalité à partir de Contact.RingToneToken et afficher son nom convivial sur la carte de visite.**

```cs
using (var connection = new AppServiceConnection())
{
    connection.AppServiceName = "ms-tonepicker-nameprovider";
    connection.PackageFamilyName = "Microsoft.Tonepicker_8wekyb3d8bbwe";
    AppServiceConnectionStatus connectionStatus = await connection.OpenAsync();
    if (connectionStatus == AppServiceConnectionStatus.Success)
    {
        var message = new ValueSet() {
            { "Action", "GetToneName" },
            { "ToneToken", token)
        };
        AppServiceResponse response = await connection.SendMessageAsync(message);
        if (response.Status == AppServiceResponseStatus.Success)
        {
            Int32 resultCode = (Int32)response.Message["Result"];
            if (resultCode == 0)
            {
                string name = response.Message["DisplayName"] as string;
            }
            else
            {
                // handle failure
            }
        }
        else
        {
            // handle failure
        }
    }
}
```
