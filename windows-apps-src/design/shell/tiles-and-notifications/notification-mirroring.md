---
Description: Learn how to use notification mirroring on your toast notifications.
title: Mise en miroir des notifications
label: Notification mirroring
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: windows10, uwp, toast, centre de notifications dans le cloud, mise en miroir des notifications, notification, sur plusieurs appareils
ms.localizationpriority: medium
ms.openlocfilehash: dc870601159a80bc6d03a287fd19f082e968e09e
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8783273"
---
# <a name="notification-mirroring"></a>Mise en miroir des notifications

La mise en miroir des notifications, prise en charge par le centre de notifications dans le cloud, vous permet d’afficher les notifications de votre téléphone sur votre PC.

> [!IMPORTANT]
> **Nécessite la mise à jour anniversaire**: vous devez exécuter la build14393 ou une version supérieure pour que la mise en miroir des notifications fonctionne. Si vous souhaitez ne pas utiliser la mise en miroir des notifications, vous devez cibler le Kit de développement logiciel (SDK)14393 pour accéder aux API de mise en miroir.

Avec la mise en miroir des notifications et Cortana, les utilisateurs peuvent recevoir les notifications de leur téléphone (Windows Mobile et Android) sur leur PC et y répondre. En tant que développeur, vous n’avez rien à faire pour activer la mise en miroir des notifications, elle fonctionne automatiquement! Lorsque vous cliquez sur les boutons du toast mis en miroir, comme les réponses rapides aux messages, vous êtes redirigé vers le téléphone, ce qui appelle votre tâche en arrière-plan ou lance votre application au premier plan.

<img alt="Notification mirroring diagram" src="images/toast-mirroring.gif" width="350"/>

Les développeurs obtenir deux grands avantages à partir de la mise en miroir de notification: les notifications mises en miroir pour conséquence plus engagement des utilisateurs avec votre service, et ils permettent également aux utilisateurs de découvrir votre application de bureau Microsoft Store! Vos utilisateurs peuvent ne même pas savoir qu’une application UWP extraordinaire est disponible pour leur bureau Windows10. Les utilisateurs reçoivent la notification de mise en miroir sur leur téléphone, les utilisateurs peuvent cliquer sur la notification toast afin d’être redirigés vers le Microsoft Store, où ils peuvent installer votre application de bureau UWP.

La mise en miroir fonctionne avec Windows Phone et Android. Les utilisateurs doivent être connectés à Cortana sur leur téléphone et bureau pour que la mise en miroir des notifications puisse fonctionner.


## <a name="what-if-the-app-is-installed-on-both-devices"></a>Que se passe-t-il si l’application est installée sur les deux appareils?

Si l’utilisateur a déjà installé votre application sur son PC, nous désactivons automatiquement la notification mise en miroir sur son téléphone afin que les notifications ne s’affichent pas en double. Les notifications mises en miroir sont automatiquement désactivées selon les critères suivants...

1. Une application sur le PC existe avec le **même nom complet ou le même PFN** (Nom de la famille de packages)
2. L’application sur le PC a envoyé une notification toast

Si l’application sur le PC n’a pas encore envoyé de toast, les notifications sur le téléphone continuent de s’afficher, car il est possible que l’utilisateur n’ait pas encore lancé l’application sur le PC.


## <a name="how-to-opt-out-of-mirroring"></a>Désactivation de la mise en miroir

Les développeurs d’application UWP, les entreprises et les utilisateurs peuvent choisir de désactiver la mise en miroir des notifications.

> [!NOTE]
> La désactivation de la mise en miroir entraine également la désactivation du [masquage universel](universal-dismiss.md).


### <a name="as-a-developer-opt-out-an-individual-notification"></a>En tant que développeur, désactivez la mise en miroir d’une notification individuelle

Vous pouvez parfois avoir une notification spécifique à un appareil que vous ne voulez pas mettre en miroir sur d’autres appareils. Vous pouvez empêcher la mise en miroir d’une notification spécifique en définissant la propriété **Mirroring** de la notification toast. Pour le moment, cette propriété Mirroring peut être définie uniquement sur les notifications locales (elle ne peut pas être spécifiée lors de l’envoi d’une notification push WNS).

**Problème connu**: la récupération de la propriété Mirroring via les API `ToastNotificationHistory.GetHistory()` retourne toujours la valeur par défaut (**Allowed**) au lieu de l’option que vous avez spécifiée. Ne vous inquiétez pas, tout est normal: seule la valeur incorrecte est récupérée.

```csharp
var toast = new ToastNotification(xml)
{
    // Disable mirroring of this notification
    Mirroring = NotificationMirroring.Disabled
};
  
ToastNotificationManager.CreateToastNotifier().Show(toast);
```


### <a name="as-a-developer-opt-out-completely"></a>En tant que développeur, effectuez une désactivation complète de la mise en miroir

Certains développeurs peuvent choisir de désactiver complètement la mise en miroir des notifications. Même si nous pensons que toutes les applications pourraient bénéficier de la mise en miroir, nous facilitons également la procédure de désactivation. Il suffit d’appeler une fois la méthode suivante pour désactiver la mise en miroir des notifications sur votre application. Par exemple, vous pouvez placer cet appel dans le constructeur de votre application à l’intérieur de `App.xaml.cs`...

```csharp
public App()
{
    this.InitializeComponent();
    this.Suspending += OnSuspending;
 
    // Disable notification mirroring for entire app
    ToastNotificationManager.ConfigureNotificationMirroring(NotificationMirroring.Disabled);
}
```


### <a name="as-an-enterprise-how-do-i-opt-out"></a>En tant qu’entreprise, comment désactiver la mise en miroir?

Les entreprises peuvent choisir de désactiver complètement la mise en miroir des notifications. Pour ce faire, elles modifient simplement la stratégie de groupe pour désactiver la mise en miroir des notifications.


### <a name="as-a-user-how-do-i-opt-out"></a>En tant qu’utilisateur, comment désactiver la mise en miroir?

Les utilisateurs peuvent choisir de désactiver la mise en miroir des notifications sur des applications individuelles, ou de désactiver complètement la fonctionnalité. Si vous ne souhaitez pas que les notifications d’une application spécifique soient mises en miroir sur votre bureau, vous pouvez simplement désactiver cette application spécifique. Vous trouverez ces options dans les paramètres de Cortana sur votre téléphone et PC.