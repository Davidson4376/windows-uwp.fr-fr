---
author: andrewleader
Description: Learn how to use custom audio on your toast notifications.
title: Contenu audio personnalisé des toast
label: Custom audio on toasts
template: detail.hbs
ms.author: mijacobs
ms.date: 12/15/2017
ms.topic: article
keywords: windows10, uwp, toast, contenu audio personnalisé, notification, audio, son
ms.localizationpriority: medium
ms.openlocfilehash: 8ef27dfed400715256d1d9cfa51f383a9b72c90d
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7295051"
---
# <a name="custom-audio-on-toasts"></a>Contenu audio personnalisé des toasts

Les notifications toast peuvent utiliser du contenu audio personnalisé, qui permet à votre application d’exprimer les effets sonores uniques de votre marque. Par exemple, une application de messagerie peut utiliser le propre son de message de ses notifications toast, afin que l’utilisateur puisse être instantanément informé de la réception d’une notification de l’application, au lieu d’entendre le son de notification générique.

## <a name="install-uwp-community-toolkit-nuget-package"></a>Installer le package NuGet UWP Community Toolkit

Pour créer des notifications par le biais de code, nous vous recommandons vivement d’utiliser la bibliothèque UWP Community Toolkit Notifications, qui fournit un modèle d’objet pour le contenu XML de la notification. Vous pouvez construire manuellement le contenu XML de la notification, mais cette méthode est sujette aux erreurs et compliquée. La bibliothèque Notifications dans UWP Community Toolkit est créée et gérée par l’équipe propriétaire des notifications chez Microsoft.

Installez [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) à partir de NuGet (nous utilisons la version1.0.0 dans cette documentation).


## <a name="add-namespace-declarations"></a>Ajouter des déclarations d’espace de noms

`Windows.UI.Notifications` inclut les API de vignette et de toast. `Microsoft.Toolkit.Uwp.Notifications` inclut la bibliothèque Notifications.

```csharp
using Microsoft.Toolkit.Uwp.Notifications;
using Windows.UI.Notifications;
```


## <a name="construct-the-notification"></a>Construire la notification

Le contenu de la notification toast inclut du texte et des images, ainsi que des boutons et des entrées. Pour afficher un extrait de code complet, voir [Envoyer un toast local](send-local-toast.md) .

```csharp
ToastContent toastContent = new ToastContent()
{
    Visual = new ToastVisual()
    {
        ... (omitted)
    }
};
```


## <a name="add-the-custom-audio"></a>Ajouter le contenu audio personnalisé

Windows Mobile a toujours pris en charge le contenu audio personnalisé des notifications toast. Toutefois, Desktop a uniquement ajouté la prise en charge du contenu audio personnalisé dans la version1511 (build10586). Si vous envoyez un toast contenant des données audio personnalisées à un appareil de bureau avant la version1511, le toast est en mode silencieux. Par conséquent, pour la version préliminaire1511 de Desktop, vous Ne devez PAS inclure le contenu audio personnalisé de votre notification toast, afin que la notification utilise au moins le son de notification par défaut.

**Problème connu**: si vous utilisez la version1511 de Desktop, le contenu audio personnalisé fonctionne uniquement si votre application est installée via le Store. Cela signifie que vous ne pouvez pas tester localement votre contenu audio personnalisé sur l'appareil de bureau avant de l’envoyer au Store, mais le contenu audio fonctionne correctement une fois installé à partir du Store. Nous avons résolu ce problème dans la mise à jour anniversaire, afin que le contenu audio personnalisé de votre application déployée localement fonctionne correctement.

```csharp
?
bool supportsCustomAudio = true;
 
// If we're running on Desktop before Version 1511, do NOT include custom audio
// since it was not supported until Version 1511, and would result in a silent toast.
if (AnalyticsInfo.VersionInfo.DeviceFamily.Equals("Windows.Desktop")
    && !ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 2))
{
    supportsCustomAudio = false;
}
 
if (supportsCustomAudio)
{
    toastContent.Audio = new ToastAudio()
    {
        Src = new Uri("ms-appx:///Assets/Audio/CustomToastAudio.m4a")
    };
}
```

Les types de fichier audio pris en charge sont...

- .aac
- .flac
- .m4a
- .mp3
- .wav
- .wma


## <a name="send-the-notification"></a>Envoyer la notification

Maintenant que le contenu de votre toast est terminé, l’envoi de la notification est très simple.

```csharp
// Create the toast notification from the previous toast content
ToastNotification notification = new ToastNotification(toastContent.GetXml());
             
// And then send the toast
ToastNotificationManager.CreateToastNotifier().Show(notification);
```


## <a name="related-topics"></a>Rubriquesassociées

- [Exemple de code complet sur GitHub](https://github.com/WindowsNotifications/quickstart-toast-with-custom-audio)
- [Envoyer un toast local](send-local-toast.md)
- [Documentation sur le contenu des toasts](adaptive-interactive-toasts.md)