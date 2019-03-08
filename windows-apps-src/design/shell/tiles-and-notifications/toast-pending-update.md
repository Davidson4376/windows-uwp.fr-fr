---
Description: Découvrez comment créer des interactions de plusieurs étapes dans vos notifications.
title: Toast avec activation des mises à jour en attente
label: Toast with pending update activation
template: detail.hbs
ms.date: 12/14/2017
ms.topic: article
keywords: windows 10, uwp, toast, en attente de mise à jour, pendingupdate, interactivité à plusieurs étapes, interactions à plusieurs étapes
ms.localizationpriority: medium
ms.openlocfilehash: b1574ee2913bd2889af204aae1089dc170df95b8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57648554"
---
# <a name="toast-with-pending-update-activation"></a>Toast avec activation des mises à jour en attente

Vous pouvez utiliser **PendingUpdate** pour créer des interactions à plusieurs étapes dans vos notifications toast. Par exemple, comme illustré ci-dessous, vous pouvez créer une série de toasts où les toasts suivants dépendent des réponses des toasts précédents.

![Toast avec mise à jour en attente](images/toast-pendingupdate.gif)

> [!IMPORTANT]
> **Requiert le bureau Fall Creators Update et 2.0.0 de bibliothèque de Notifications**: Vous devez exécuter Bureau build 16299 ou une version ultérieure pour afficher le travail de mise à jour en attente. Vous devez utiliser la version 2.0.0 ou une version ultérieure de la [bibliothèque NuGet UWP Community Toolkit Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) pour affecter **PendingUpdate** à vos boutons. **PendingUpdate** est uniquement pris en charge sur les appareils de bureau et sera ignoré sur les autres appareils.


## <a name="prerequisites"></a>Conditions préalables

Cet article suppose une connaissance pratique de...

- [Créant du contenu de toast](adaptive-interactive-toasts.md)
- [Envoyer un toast et gestion de l’activation en arrière-plan](send-local-toast.md)


## <a name="overview"></a>Vue d’ensemble

Pour implémenter un toast qui utilise la mise à jour en attente comme comportement après activation...

1. Sur les boutons d’activation en arrière-plan de votre toast, définissez un objet **AfterActivationBehavior** sur **PendingUpdate**

2. Affectez un objet **Tag** (et éventuellement **Group**) lors de l’envoi de votre toast

3. Lorsque l’utilisateur clique sur le bouton, votre tâche en arrière-plan est activée, et le toast est conservé à l’écran dans un état de mise à jour en attente

4. Dans votre tâche en arrière-plan, envoyez un nouveau toast avec votre nouveau contenu en utilisant les mêmes objets **Tag** et **Group**


## <a name="assign-pendingupdate"></a>Affecter PendingUpdate

Sur les boutons d’activation en arrière-plan, définissez **AfterActivationBehavior** sur **PendingUpdate**. Notez que cela fonctionne uniquement pour les boutons dont l’objet **ActivationType** est défini sur **Background**.

```csharp
new ToastButton("Yes", "action=orderLunch")
{
    ActivationType = ToastActivationType.Background,

    ActivationOptions = new ToastActivationOptions()
    {
        AfterActivationBehavior = ToastAfterActivationBehavior.PendingUpdate
    }
}
```

```xml
<action
    content='Yes'
    arguments='action=orderLunch'
    activationType='background'
    afterActivationBehavior='pendingUpdate' />
```


## <a name="use-a-tag-on-the-notification"></a>Utiliser un objet Tag dans la notification

Afin de remplacer ultérieurement la notification, nous devons affecter l’objet **Tag** (et éventuellement **Group**) à la notification.

```csharp
// Create the notification
var notif = new ToastNotification(content.GetXml())
{
    Tag = "lunch"
};

// And show it
ToastNotificationManager.CreateToastNotifier().Show(notif);
```


## <a name="replace-the-toast-with-new-content"></a>Remplacer le toast par un nouveau contenu

En réponse au clic de l’utilisateur sur le bouton, votre tâche en arrière-plan est déclenchée et vous devez remplacer le toast par un nouveau contenu. Vous remplacez le toast en envoyant simplement un nouveau toast avec les mêmes objets **Tag** et **Group**.

Nous vous recommandons vivement de **définir le son sur le mode silencieux** lorsque vous effectuez des remplacements en réponse à un clic sur un bouton, car l’utilisateur interagit déjà avec votre toast.

```csharp
// Generate new content
ToastContent content = new ToastContent()
{
    ...

    // We disable audio on all subsequent toasts since they appear right after the user
    // clicked something, so the user's attention is already captured
    Audio = new ToastAudio() { Silent = true }
};

// Create the new notification
var notif = new ToastNotification(content.GetXml())
{
    Tag = "lunch"
};

// And replace the old one with this one
ToastNotificationManager.CreateToastNotifier().Show(notif);
```


## <a name="related-topics"></a>Rubriques connexes

- [Exemple de code complet sur GitHub](https://github.com/WindowsNotifications/quickstart-toast-pending-update)
- [Envoyer une activation toast et handle locale](send-local-toast.md)
- [Documentation de contenu de toast](adaptive-interactive-toasts.md)
- [Barre de progression de toast](toast-progress-bar.md)