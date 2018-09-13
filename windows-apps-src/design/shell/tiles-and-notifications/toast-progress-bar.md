---
author: andrewleader
Description: Learn how to use a progress bar within your toast notification.
title: Barre de progression du toast et liaison des données
label: Toast progress bar and data binding
template: detail.hbs
ms.author: mijacobs
ms.date: 12/7/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, toast, barre de progression, barre de progression du toast, notification, liaison des données du toast
ms.localizationpriority: medium
ms.openlocfilehash: b99c2479bef3c10ecc82707e475f49fd2b9014ec
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "3963255"
---
# <a name="toast-progress-bar-and-data-binding"></a>Barre de progression du toast et liaison des données

L’utilisation d’une barre de progression dans votre notification toast vous permet de communiquer l’état des opérations longues à l’utilisateur, comme les téléchargements, le rendu vidéo, les objectifs des exercices, etc.

> [!IMPORTANT]
> **Nécessite Creators Update et la version1.4.0 de la bibliothèque Notifications**: vous devez cibler le Kit de développement logiciel (SDK)15063 et exécuter la version15063 ou une version supérieure pour utiliser les barres de progression dans les toasts. Vous devez utiliser la version1.4.0 ou une version ultérieure de la [bibliothèque NuGet UWP Community Toolkit Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) pour construire la barre de progression dans le contenu de votre toast.

Une barre de progression dans un toast peut être «indéterminée» (aucune valeur spécifique, les points animés indiquent qu’une opération est en cours) ou «déterminée» (un pourcentage spécifique de la barre est rempli, par exemple 60%).

> **API importantes**: [classe NotificationData](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notificationdata), [méthode ToastNotifier.Update](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotifier.Update), [classe ToastNotification](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification)

> [!NOTE]
> Seuls les appareils de bureau prennent en charge les barres de progression dans les notifications toast. Sur les autres appareils, la barre de progression est supprimée de votre notification.

L’image ci-dessous illustre une barre de progression déterminée avec toutes ses propriétés correspondantes indiquées.

<img alt="Toast with progress bar properties labeled" src="images/toast-progressbar-annotated.png" width="626"/>

| Propriété | Type | Requis | Description |
|---|---|---|---|
| **Title** | chaîne ou [BindableString](toast-schema.md#bindablestring) | false | Obtient ou définit une chaîne de titre facultative. Prend en charge la liaison des données. |
| **Value** | double ou [AdaptiveProgressBarValue](toast-schema.md#adaptiveprogressbarvalue) ou [BindableProgressBarValue](toast-schema.md#bindableprogressbarvalue) | false | Obtient ou définit la valeur de la barre de progression. Prend en charge la liaison des données. La valeur par défaut est 0. Peut être un double compris entre 0.0 et 1.0, `AdaptiveProgressBarValue.Indeterminate` ou `new BindableProgressBarValue("myProgressValue")`. |
| **ValueStringOverride** | chaîne ou [BindableString](toast-schema.md#bindablestring) | false | Obtient ou définit une chaîne facultative à afficher à la place de la chaîne de pourcentage par défaut. Si elle n’est pas fournie, la valeur «70%» s’affiche. |
| **Status** | chaîne ou [BindableString](toast-schema.md#bindablestring) | true | Obtient ou définit une chaîne d’état (obligatoire), qui est affichée sous la barre de progression à gauche. Cette chaîne doit refléter l’état de l’opération, par exemple «Téléchargement en cours...» ou «Installation en cours...» |


Voici la procédure à suivre pour générer la notification illustrée ci-dessus...

```csharp
ToastContent content = new ToastContent()
{
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Downloading your weekly playlist..."
                },
 
                new AdaptiveProgressBar()
                {
                    Title = "Weekly playlist",
                    Value = 0.6,
                    ValueStringOverride = "15/26 songs",
                    Status = "Downloading..."
                }
            }
        }
    }
};
```

```xml
<toast>
    <visual>
        <binding template="ToastGeneric">
            <text>Downloading your weekly playlist...</text>
            <progress
                title="Weekly playlist"
                value="0.6"
                valueStringOverride="15/26 songs"
                status="Downloading..."/>
        </binding>
    </visual>
</toast>
```

Toutefois, vous devrez mettre à jour dynamiquement les valeurs de la barre de progression pour la rendre dynamique. Pour ce faire, utilisez la liaison des données pour mettre à jour le toast.


## <a name="using-data-binding-to-update-a-toast"></a>Utilisation de la liaison des données pour mettre à jour un toast

L’utilisation de la liaison des données implique les étapes suivantes...

1. Construisez le contenu du toast qui utilise des champs liés aux données
2. Affectez un objet **Tag** (et éventuellement **Group**) à votre **ToastNotification**
3. Définissez vos valeurs **Data** initiales sur votre **ToastNotification**
4. Envoyez le toast
5. Utilisez les objets **Tag** et **Group** pour mettre à jour les valeurs **Data** avec de nouvelles valeurs

L’extrait de code suivant illustre les étapes1 à 4. L’extrait de code suivant explique comment mettre à jour les valeurs **Data** du toast.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications;
 
public void SendUpdatableToastWithProgress()
{
    // Define a tag (and optionally a group) to uniquely identify the notification, in order update the notification data later;
    string tag = "weekly-playlist";
    string group = "downloads";
 
    // Construct the toast content with data bound fields
    var content = new ToastContent()
    {
        Visual = new ToastVisual()
        {
            BindingGeneric = new ToastBindingGeneric()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = "Downloading your weekly playlist..."
                    },
    
                    new AdaptiveProgressBar()
                    {
                        Title = "Weekly playlist",
                        Value = new BindableProgressBarValue("progressValue"),
                        ValueStringOverride = new BindableString("progressValueString"),
                        Status = new BindableString("progressStatus")
                    }
                }
            }
        }
    };
 
    // Generate the toast notification
    var toast = new ToastNotification(content.GetXml());
 
    // Assign the tag and group
    toast.Tag = tag;
    toast.Group = group;
 
    // Assign initial NotificationData values
    // Values must be of type string
    toast.Data = new NotificationData();
    toast.Data.Values["progressValue"] = "0.6";
    toast.Data.Values["progressValueString"] = "15/26 songs";
    toast.Data.Values["progressStatus"] = "Downloading...";
 
    // Provide sequence number to prevent out-of-order updates, or assign 0 to indicate "always update"
    toast.Data.SequenceNumber = 1;
 
    // Show the toast notification to the user
    ToastNotificationManager.CreateToastNotifier().Show(toast);
}
```

Ensuite, lorsque vous souhaitez modifier vos valeurs **Data**, utilisez la méthode [**Update**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotifier.Update) pour fournir les nouvelles données sans reconstruire toute la charge utile du toast.

```csharp
using Windows.UI.Notifications;
 
public void UpdateProgress()
{
    // Construct a NotificationData object;
    string tag = "weekly-playlist";
    string group = "downloads";
 
    // Create NotificationData and make sure the sequence number is incremented
    // since last update, or assign 0 for updating regardless of order
    var data = new NotificationData
    {
        SequenceNumber = 2
    };

    // Assign new values
    // Note that you only need to assign values that changed. In this example
    // we don't assign progressStatus since we don't need to change it
    data.Values["progressValue"] = "0.7";
    data.Values["progressValueString"] = "18/26 songs";

    // Update the existing notification's data by using tag/group
    ToastNotificationManager.CreateToastNotifier().Update(data, tag, group);
}
```

L’utilisation de la méthode **Update** au lieu de remplacer la totalité du toast garantit également que la notification toast reste à la même position dans le centre de notifications et ne se déplace pas vers le haut ou vers le bas. Il serait assez déconcertant pour l’utilisateur si le toast ne cessait de se déplacer vers le haut du centre de notifications toutes les secondes alors que la barre de progression est remplie!

La méthode **Update** renvoie une énumération, [**NotificationUpdateResult**](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notificationupdateresult), qui vous permet de savoir si la mise à jour a réussi ou si la notification est introuvable (ce qui signifie que l’utilisateur a probablement ignoré votre notification et vous devez cesser de lui envoyer des mises à jour). Il est déconseillé d’afficher un autre toast jusqu’à ce que l’opération de progression soit terminée (par exemple, lorsque le téléchargement est terminé).


## <a name="elements-that-support-data-binding"></a>Éléments prenant en charge la liaison des données
Les éléments suivants des notifications toast prennent en charge la liaison des données

- Toutes les propriétés de **AdaptiveProgress**
- La propriété **Text** des éléments **AdaptiveText** de niveau supérieur


## <a name="update-or-replace-a-notification"></a>Mettre à jour ou remplacer une notification

Depuis Windows10, vous pouviez toujours **remplacer** une notification en envoyant un nouveau toast avec les mêmes objets **Tag** et **Group**. Quelle est donc la différence entre **remplacer** le toast et **mettre à jour** les données du toast?

| | Remplacement | Mise à jour |
| -- | -- | --
| **Position dans le centre de notifications** | Déplace la notification vers le haut du centre de notifications. | Conserve la notification à sa place dans le centre de notifications. |
| **Modification du contenu** | Peut modifier complètement tout le contenu ou toute la disposition du toast | Peut uniquement modifier les propriétés qui prennent en charge la liaison des données (barre de progression et texte de niveau supérieur) |
| **Réaffichage comme fenêtre contextuelle** | Peut réapparaître comme fenêtre contextuelle de toast si vous laissez **SuppressPopup** défini sur `false` (ou définissez-le sur true pour l’envoyer en mode silencieux au centre de notifications) | Ne réapparaît pas comme fenêtre contextuelle; les données du toast sont mises à jour en mode silencieux dans le centre de notifications |
| **Ignoré par l’utilisateur** | Que l’utilisateur ait ignoré ou non votre précédente notification, votre toast de remplacement est toujours envoyé | Si l’utilisateur a ignoré votre toast, la mise à jour du toast échoue |

En général, la **mise à jour est utile pour...**

- Informations qui sont souvent modifiées sur une courte période et ne nécessitent pas d’être portées à l’attention de l’utilisateur
- Modifications subtiles du contenu de votre toast, par exemple modifications de 50 à 65%

Souvent, une fois que votre séquence de mises à jour est terminée (par exemple, le fichier a été téléchargé), nous vous recommandons le remplacement pour la dernière étape, car...

- Votre notification finale comporte probablement des modifications de disposition radicales, comme la suppression de la barre de progression, l’ajout de nouveaux boutons, etc.
- L’utilisateur a peut-être ignoré votre notification de progression en attente, car il ne se souhaite pas suivre son téléchargement, mais souhaite néanmoins être notifié par un toast lorsque l’opération est terminée.


## <a name="related-topics"></a>Rubriquesassociées

- [Exemple de code complet sur GitHub](https://github.com/WindowsNotifications/quickstart-toast-progress-bar)
- [Documentation sur le contenu des toasts](adaptive-interactive-toasts.md)