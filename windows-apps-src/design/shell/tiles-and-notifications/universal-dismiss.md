---
author: andrewleader
Description: Learn how to use Universal Dismiss on your toast notifications.
title: Masquage universel
label: Universal Dismiss
template: detail.hbs
ms.author: mijacobs
ms.date: 12/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, toast, centre de notifications dans le cloud, masquage universel, notification, sur plusieurs appareils, masquer une fois, masquer partout
ms.localizationpriority: medium
ms.openlocfilehash: 90ad60949504d4478341ff9455fe0f7da90d78a9
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4565101"
---
# <a name="universal-dismiss"></a>Masquage universel

Le masquage universel, pris en charge par le centre de notifications dans le cloud, signifie que lorsque vous masquez une notification sur un appareil, la même notification est également masquée sur les autres appareils.

> [!IMPORTANT]
> **Nécessite la mise à jour anniversaire**: vous devez cibler le Kit de développement logiciel (SDK)14393 et exécuter la Build14393 ou une version supérieure pour utiliser le masquage universel.

Les rappels du calendrier sont un exemple courant de ce scénario.... vous disposez d’une application de calendrier sur vos deux appareils... vous recevez un rappel sur votre téléphone et votre application de bureau... vous cliquez sur Masquer sur votre application de bureau... grâce au masquage universel, le rappel sur votre téléphone est également masqué! **L’activation du masquage universel ne nécessite qu’une seule ligne de code!**

<img alt="Diagram of Universal Dismiss" src="images/universal-dismiss.gif" width="406"/>

Dans ce scénario, l’important est que **la même application est installée sur plusieurs appareils**, ce qui signifie que **chaque appareil reçoit déjà des notifications**. Une application de calendrier est l’exemple typique, car la même application de calendrier est généralement installée sur votre PC Windows et votre téléphone, et chaque instance de l’application vous envoie déjà des rappels sur chaque appareil. En ajoutant la prise en charge du masquage universel, les instances des mêmes rappels peuvent être liées sur plusieurs appareils.


## <a name="how-to-enable-universal-dismiss"></a>Activation du masquage universel

En tant que développeur, l’activation du masquage universel est extrêmement facile. Vous devez simplement fournir un ID qui nous permet de lier chaque notification sur plusieurs appareils. Ainsi, lorsque l’utilisateur masque une notification sur un appareil, la notification liée correspondante est également masquée sur l’autre appareil.

![Diagramme RemoteId de masquage universel](images/universal-dismiss-remoteid.jpg)

> **RemoteId**: identificateur qui identifie de manière unique une notification *sur plusieurs appareils*.

Une seule ligne de code est utilisée pour ajouter RemoteId, ce qui active la prise en charge du masquage universel! C’est vous qui décidez comment générer votre RemoteId. Toutefois, vous devez veiller à ce qu’il identifie de manière unique votre notification sur plusieurs appareils, et que le même identificateur puisse être généré à partir de différentes instances de votre application s’exécutant sur plusieurs appareils.

Par exemple, dans mon application de planification de devoirs, je génère mon RemoteId en indiquant que son type est «rappel», puis j’inclus l’ID du compte en ligne et l’identificateur en ligne du devoir. Je peux générer de manière cohérente le même RemoteId, quel que soit l’appareil qui envoie la notification, car ces ID en ligne sont partagés sur les différents appareils.

```csharp
var toast = new ScheduledToastNotification(content.GetXml(), startTime);
 
// If the RemoteId property is present
if (ApiInformation.IsPropertyPresent(typeof(ScheduledToastNotification).FullName, nameof(ScheduledToastNotification.RemoteId)))
{
    // Assign the RemoteId to add support for Universal Dismiss
    toast.RemoteId = $"reminder_{account.AccountId}_{homework.Identifier}"
}
  
ToastNotificationManager.CreateToastNotifier().AddToSchedule(toast);
```

Le code suivant s’exécute sur mon téléphone et sur mon application de bureau, ce qui signifie que la notification sur les deux appareils aura le même RemoteId.

C’est tout ce que vous avez à faire! Lorsque l’utilisateur masque (ou clique sur) une notification, nous vérifions si elle a un RemoteId, auquel cas, nous envoyons une demande de masquage de ce RemoteId sur tous les appareils de l’utilisateur.

**Problème connu**: la récupération du **RemoteId** via l’API `ToastNotificationHistory.GetHistory()` retourne toujours une chaîne vide au lieu du **RemoteId** vous avez spécifié. Ne vous inquiétez pas, tout est normal: seule la valeur incorrecte est récupérée.

> [!NOTE]
> Si l’utilisateur ou l’entreprise désactive la [mise en miroir des notifications](notification-mirroring.md) pour votre application (ou désactive complètement la mise en miroir des notifications), le masquage universel ne fonctionne pas, car vos notifications ne sont pas disponibles dans le cloud.


## <a name="supported-devices"></a>Appareils pris en charge

Depuis la mise à jour anniversaire, le masquage universel est pris en charge sur Windows Mobile et Windows Desktop. Le masquage universel fonctionne dans les deux sens, entre un PC et un PC, un PC et un téléphone et un téléphone et un téléphone.