---
author: andrewleader
Description: Learn how to create multi-step interactions in your notifications.
title: Toast avec activation des mises à jour en attente
label: Toast with pending update activation
template: detail.hbs
ms.author: mijacobs
ms.date: 12/14/2017
ms.topic: article
keywords: windows10, uwp, toast, en attente de mise à jour, pendingupdate, interactivité à plusieurs étapes, interactions à plusieurs étapes
ms.localizationpriority: medium
ms.openlocfilehash: 4d21c96676eec1b8b1a1f4397880af937dd8f4b6
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6036839"
---
# <a name="toast-with-pending-update-activation"></a>Toast avec activation des mises à jour en attente

Vous pouvez utiliser **PendingUpdate** pour créer des interactions à plusieurs étapes dans vos notifications toast. Par exemple, comme illustré ci-dessous, vous pouvez créer une série de toasts où les toasts suivants dépendent des réponses des toasts précédents.

![Toast avec mise à jour en attente](images/toast-pendingupdate.gif)

> [!IMPORTANT]
> **Nécessite Desktop Fall Creators Update et la version2.0.0 de la bibliothèque Notifications**: vous devez exécuter la build16299 de Desktop ou une version ultérieure pour que la mise à jour en attente fonctionne. Vous devez utiliser la version2.0.0 ou une version ultérieure de la [bibliothèque NuGet UWP Community Toolkit Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) pour affecter **PendingUpdate** à vos boutons. **PendingUpdate** est uniquement pris en charge sur les appareils de bureau et sera ignoré sur les autres appareils.


## <a name="prerequisites"></a>Éléments prérequis

Cet article suppose une connaissance pratique de...

- [Construction du contenu du toast](adaptive-interactive-toasts.md)
- [Envoi d’un toast et gestion de l’activation en arrière-plan](send-local-toast.md)


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


## <a name="related-topics"></a>Rubriquesassociées

- [Exemple de code complet sur GitHub](https://github.com/WindowsNotifications/quickstart-toast-pending-update)
- [Envoyer une notification toast locale et gérer l’activation](send-local-toast.md)
- [Documentation sur le contenu des toasts](adaptive-interactive-toasts.md)
- [Barre de progression du toast](toast-progress-bar.md)