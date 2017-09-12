---
author: JnHs
Description: "Ciblez des segments de clients spécifiques avec un contenu personnalisé pour augmenter l’engagement, la rétention et la monétisation."
title: "Utilisez des offres ciblées pour optimiser l’engagement et les conversions"
ms.author: wdg-dev-content
ms.date: 07/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows10, uwp, offres ciblées, offres, notifications"
ms.openlocfilehash: b30add7a1aa55ffd852bff57fcebb7a5f40e114b
ms.sourcegitcommit: eaacc472317eef343b764d17e57ef24389dd1cc3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/17/2017
---
# <a name="use-targeted-offers-to-maximize-engagement-and-conversions"></a>Utilisez des offres ciblées pour optimiser l’engagement et les conversions

Ciblez des segments de clients spécifiques avec un contenu attractif et personnalisé pour augmenter l’engagement, la rétention et la monétisation. Dans certains cas, les offres ciblées peuvent être associées à une promotion parrainée par Microsoft qui donne lieu à une prime aux développeurs pour chaque conversion réussie, ce qui vous permet de gagner encore plus.

> [!IMPORTANT]
> Les offres ciblées ne sont utilisables qu’avec les applicationsUWP qui comprennent des extensions.

## <a name="targeted-offer-overview"></a>Vue d’ensemble des offres ciblées

De façon générale, vous devez effectuer trois opérations pour utiliser des offres ciblées:

1. **Créez l’offre dans votre tableau de bord.** Accédez à la page **Engager > Offres ciblées** pour créer des offres. Vous trouverez d’autres informations sur ce processus ci-dessous.
2. **Mettre en place les offres intégrées aux applications.** Utilisez les *API d'offres ciblées du Windows Store* dans le code de votre application pour récupérer les offres disponibles pour un utilisateur donné et pour demander à en bénéficier (si elle sont associées à des promotions). Vous devez également créer une expérience interne à l’application pour l’offre ciblée. Pour plus d’informations, consultez l’article [Gérer les offres ciblées à l’aide des services du WindowsStore](../monetize/manage-targeted-offers-using-windows-store-services.md).
3. **Soumettez votre application au WindowsStore.** Votre application doit être publiée avec la mise en place des offres dans l’application afin que les offres soit mises à la disposition des clients.

Une fois que vous avez exécuté ces étapes, les clients qui utilisent votre application voient s’afficher les offres qui leur sont disponibles à ce stade, en fonction de leur appartenance aux segments associés à vos offres. Veuillez noter que, bien que nous nous efforcions d’afficher toutes les offres disponibles pour vos clients, certains problèmes susceptibles d’affecter la disponibilité des offres peuvent parfois survenir.


## <a name="to-create-and-send-a-targeted-offer"></a>Pour créer et envoyer une offre ciblée

Suivez la procédure ci-après pour créer une offre ciblée dans le tableau de bord. 

1.  Dans le tableau de bord du Centre de développement Windows, développez **Engager** dans le menu de navigation de gauche, puis sélectionnez **Offres ciblées**.
2.  Sur la page **Offres ciblées**, examinez les offres disponibles. Sélectionnez **Créer une nouvelle offre** pour toute offre que vous souhaitez implémenter. 

    > [!NOTE]
    > Les offres disponibles que vous verrez s’afficher peuvent varier au fil du temps et selon les critères du compte. Dans un premier temps, vous verrez s'afficher une ou plusieurs offres selon des critères de segment prédéfinis. Bientôt, nous vous permettrons de créer des offres ciblées basées sur des [segments de clients que vous définirez](create-customer-segments.md).

3.  Dans la nouvelle ligne qui s’affiche sous les offres disponibles, sélectionnez le produit (application) dans lequel l’offre sera disponible. Ensuite, sélectionnez l’extension que vous souhaitez associer à l’offre. 
4.  Si vous souhaitez créer des offres supplémentaires, répétez les étapes2 et 3. Vous pouvez implémenter le même type d’offre plusieurs fois pour la même application, tant que vous sélectionnez des extensions différentes pour chaque offre. En outre, vous pouvez associer la même extension à plusieurs types d’offres.
5.  Lorsque vous avez fini de créer les offres, cliquez sur **Enregistrer**.

Une fois que vous avez implémenté vos offres, vous pouvez revenir à la page **Offres ciblées** de votre tableau de bord pour visualiser le nombre total de conversions pour chaque offre. 

Si vous décidez de ne pas utiliser une offre (ou que vous ne voulez plus l’utiliser), cliquez sur **Supprimer**.

> [!IMPORTANT]
> Assurez-vous que vous avez publié le code pour récupérer les offres disponibles pour un utilisateur donné et pour créer l’expérience interne à l’application. Pour plus d’informations, consultez l’article [Gérer les offres ciblées à l’aide des services du WindowsStore](../monetize/manage-targeted-offers-using-windows-store-services.md).
> 
> Lorsque vous étudiez le contenu de vos offres ciblées, n’oubliez pas que, comme pour tous les contenus d’application, le contenu de vos offres doit respecter les [Politiques relatives au contenu](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#content_policies) du WindowsStore.
> 
> Notez également que si un client qui utilise votre application (et qui est connecté à son compte Microsoft au moment où son appartenance au segment est déterminée) confie ultérieurement son appareil à un tiers pour que celui-ci l’utilise, ce tiers peut voir les offres destinées au client initial. 

