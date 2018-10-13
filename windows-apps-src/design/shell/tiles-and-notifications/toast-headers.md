---
author: andrewleader
Description: Learn how to use headers to visually group your toast notifications in Action Center.
title: En-têtes de toast
label: Toast headers
template: detail.hbs
ms.author: mijacobs
ms.date: 12/7/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, toast, en-tête, en-têtes de toast, notification, regrouper les toasts, centre de notifications
ms.localizationpriority: medium
ms.openlocfilehash: 0b3c92a41832729b5a60411308d010c3cbb4470a
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4575128"
---
# <a name="toast-headers"></a>En-têtes de toast

Vous pouvez regrouper visuellement un ensemble de notifications associées dans le centre de notifications en utilisant un en-tête de toast dans vos notifications.

> [!IMPORTANT]
> **Nécessite Desktop Creators Update et la version1.4.0 de la bibliothèque Notifications**: vous devez exécuter la build15063 de Desktop ou une version ultérieure pour afficher les en-têtes de toast. Vous devez utiliser la version1.4.0 ou une version ultérieure de la [bibliothèque NuGet UWP Community Toolkit Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) pour construire l’en-tête dans le contenu de votre toast. Les en-têtes sont uniquement pris en charge sur les appareils de bureau.

Comme indiqué ci-dessous, cette conversation de groupe est unifiée sous un en-tête unique, «Camping!!». Chaque message individuel dans la conversation est une notification toast distincte qui partage le même en-tête de toast.

<img alt="Toasts with header" src="images/toast-headers-action-center.png" width="396"/>

Vous pouvez également choisir de regrouper visuellement vos notifications par catégorie, comme les rappels de vol, le suivi de packages, etc.

## <a name="add-a-header-to-a-toast"></a>Ajouter un en-tête à un toast

Procédez comme suit pour ajouter un en-tête à une notification toast.

> [!NOTE]
> Les en-têtes sont uniquement pris en charge sur les appareils de bureau. Les appareils qui ne prennent pas en charge les en-têtes ignorent simplement l’en-tête.

```csharp
ToastContent toastContent = new ToastContent()
{
    Header = new ToastHeader()
    {
        Id = "6289",
        Title = "Camping!!",
        Arguments = "action=openConversation&id=6289",
    },

    Visual = new ToastVisual() { ... }
};
```

```xml
<toast>

    <header
        id="6289"
        title="Camping!!"
        arguments="action=openConversation&amp;id=6289"/>

    <visual>
        ...
    </visual>

</toast>
```

En résumé...

1. Ajoutez l’objet **Header** à votre **ToastContent**
2. Affectez les propriétés **Id**, **Title** et **Arguments** requises
3. Envoyez votre notification ([en savoir plus](send-local-toast.md))
4. Dans un autre notification, utilisez le même **Id** d’en-tête pour les unifier sous l’en-tête. **Id** est la seule propriété utilisée pour déterminer si les notifications doivent être regroupées, ce qui signifie que les propriétés **Title** et **Arguments** peuvent être différentes. Les propriétés **Title** et **Arguments** de la notification la plus récente dans un groupe sont utilisées. Si cette notification est supprimée, les propriétés **Title** et **Arguments** sont utilisées dans la notification suivante la plus récente.


## <a name="handle-activation-from-a-header"></a>Gérer l’activation à partir d’un en-tête

Les utilisateurs peuvent cliquer sur les en-têtes pour obtenir des informations supplémentaires de votre application.

Par conséquent, les applications peuvent fournir des **Arguments** dans l’en-tête, comparables aux arguments de lancement du toast proprement dit.

L’activation est gérée comme une [activation de toast normale](send-local-toast.md#handling-activation-1), ce qui signifie que vous pouvez récupérer ces arguments dans la méthode **OnActivated** de `App.xaml.cs`, comme vous le faites lorsque l’utilisateur clique sur le corps de votre toast ou sur un bouton de votre toast.

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Handle toast activation
    if (e is ToastNotificationActivatedEventArgs)
    {
        // Arguments specified from the header
        string arguments = (e as ToastNotificationActivatedEventArgs).Argument;
    }
}
```


## <a name="additional-info"></a>Informations supplémentaires

L’en-tête sépare et regroupe visuellement les notifications. Il ne modifie aucune autre logistique concernant le nombre maximal de notifications qu’une application peut avoir (20) et le comportement «premier entré, premier sorti» de la liste des notifications.

L’ordre des notifications dans les en-têtes est le suivant... Pour une application donnée, la notification la plus récente de l’application (et le groupe d’en-têtes s’il fait partie d’un en-tête) s’affiche en premier.

L’**Id** peut être n’importe quelle chaîne de votre choix. Aucune restriction de longueur ou de caractère n’est appliquée aux propriétés de **ToastHeader**. La seule contrainte est que l’intégralité du contenu de votre toast XML ne peut pas dépasser 5Ko.

La création d’en-têtes ne modifie pas le nombre de notifications affichées dans le centre de notifications avant l’affichage du bouton «En savoir plus» (ce nombre est égal à 3 par défaut et peut être configuré par l’utilisateur pour chaque application dans les paramètres système des notifications).

Cliquer sur l’en-tête, tout comme cliquer sur le titre de l’application, n’efface pas les notifications appartenant à cet en-tête (votre application doit utiliser les API de toast pour effacer les notifications appropriées).


## <a name="related-topics"></a>Rubriquesassociées

- [Envoyer une notification toast locale et gérer l’activation](send-local-toast.md)
- [Documentation sur le contenu des toasts](adaptive-interactive-toasts.md)