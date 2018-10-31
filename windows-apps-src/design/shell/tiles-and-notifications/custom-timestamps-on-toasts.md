---
author: andrewleader
Description: Learn how to use custom timestamps on your toast notifications.
title: Horodatages personnalisés des toasts
label: Custom timestamps on toasts
template: detail.hbs
ms.author: mijacobs
ms.date: 12/15/2017
ms.topic: article
keywords: windows10, uwp, toast, horodatage personnalisé, horodatage, notification, centre de notifications
ms.localizationpriority: medium
ms.openlocfilehash: 290825fa079052b79fb2feaec8af928f8da93f95
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5865062"
---
# <a name="custom-timestamps-on-toasts"></a>Horodatages personnalisés des toasts

Par défaut, l’horodatage des notifications toast (visible dans le centre de notifications) est défini sur l’heure d’envoi de la notification.

<img alt="Toast with custom timestamp" src="images/toast-customtimestamp.jpg" width="396"/>

Vous pouvez éventuellement remplacer l’horodatage par vos propres date et heure personnalisées, afin que l’horodatage représente l’heure de création réelle du message, des informations ou du contenu, au lieu de l’heure d’envoi de la notification. Cela garantit également que vos notifications s’affichent dans l’ordre correct dans le centre de notifications (triées par heure). Nous recommandons que la plupart des applications spécifient un horodatage personnalisé.

> [!IMPORTANT]
> **Nécessite Creators Update et la version1.4.0 de la bibliothèque Notifications**: vous devez exécuter la build15063 ou une version ultérieure pour afficher les horodatages personnalisés. Vous devez utiliser la version1.4.0 ou une version ultérieure de la [bibliothèque NuGet UWP Community Toolkit Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) pour affecter l’horodatage du contenu de votre toast.

Pour utiliser un horodatage personnalisé, affectez simplement la propriété **DisplayTimestamp** de votre **ToastContent**.

```csharp
ToastContent toastContent = new ToastContent()
{
    DisplayTimestamp = new DateTime(2017, 04, 15, 19, 45, 00, DateTimeKind.Utc),
    ...
};
```

```xml
<toast displayTimestamp="2017-04-15T19:45:00Z">
  ...
</toast>
```

Si vous utilisez du contenu XML, la date doit être au format [ISO8601](https://en.wikipedia.org/wiki/ISO_8601).

> [!NOTE]
> Vous pouvez uniquement utiliser 3décimales au plus pour les secondes (bien qu’il n'y ait en réalité aucun intérêt à fournir une valeur aussi précise). Si vous indiquez plus de décimales, la charge utile ne sera pas valide et vous recevrez la notification «Nouvelle notification».


## <a name="usage-guidance"></a>Conseils d’utilisation

En général, nous recommandons que la plupart des applications spécifient un horodatage personnalisé. Cela garantit que l’horodatage de la notification représente avec précision l’heure de génération du message, des informations et du contenu, indépendamment des retards du réseau, du mode avion ou de l’intervalle fixe des tâches en arrière-plan périodiques.

Par exemple, une application d’informations peut exécuter toutes les 15minutes une tâche en arrière-plan qui recherche de nouveaux articles et affiche les notifications. Avant les horodatages personnalisés, l’horodatage correspondait à l’heure de génération de la notification toast (toujours dans des intervalles de 15minutes). Toutefois, l’application peut maintenant définir l’horodatage sur l’heure de publication réelle de l’article. De même, les applications de messagerie et les applications de réseau social peuvent bénéficier de cette fonctionnalité si un modèle similaire d’extraction périodique est utilisé pour leurs notifications.

En outre, l’utilisation d’un horodatage personnalisé permet de garantir que l’horodatage est correct, même si l’utilisateur a été déconnecté d’Internet. Par exemple, lorsque l’utilisateur met sous tension son ordinateur et que votre tâche en arrière-plan s’exécute, vous pouvez veiller à ce que l’horodatage de vos notifications représente l’heure d’envoi des messages, au lieu de l’heure à laquelle l’utilisateur a mis sous tension son ordinateur.


## <a name="default-timestamp"></a>Horodatage par défaut

Si vous ne fournissez pas d’horodatage personnalisé, nous utilisons l’heure d’envoi de votre notification.

Si vous avez envoyé une notification push par le biais de WNS, nous utilisons l’heure de réception de la notification par le serveur WNS (la latence de remise de la notification à l’appareil n’affecte donc pas l’horodatage).

Si vous avez envoyé une notification locale, nous utilisons l’heure de réception de la notification par la plateforme de notification (qui doit être immédiatement).


## <a name="related-topics"></a>Rubriquesassociées

- [Envoyer un toast local](send-local-toast.md)
- [Documentation sur le contenu des toasts](adaptive-interactive-toasts.md)