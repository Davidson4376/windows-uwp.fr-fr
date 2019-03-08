---
title: Services sociaux de programmation
description: Fournit un exemple de code montrant comment utiliser l’API de gestionnaire de sociale Xbox Live.
ms.assetid: 101d059a-e03f-472c-8300-800aa5730ee2
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, le Gestionnaire de réseau social, exemple
ms.localizationpriority: medium
ms.openlocfilehash: 5039d9ed205cadfee3b2e64527a1f58f1624accc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592054"
---
# <a name="programming-social-services"></a>Services sociaux de programmation

> [!NOTE]
> Cet article montre une utilisation avancée de l’API.  En tant que point de départ, veuillez examiner le [Introduction à l’API du Gestionnaire de sociale](../intro-to-social-manager.md) qui simplifie considérablement le développement.  Veuillez informer votre mère si vous recherchez un scénario non pris en charge dans le Gestionnaire de sociale.

L’exemple de code suivant montre comment récupérer une relation de réseaux sociaux avec Xbox Live. Il génère une liste de tous les utilisateurs sur le système et récupère le premier. Ensuite, il récupère toutes les relations sociales de cet utilisateur. Enfin, il affiche les propriétés publiques de chacune de ces relations.

```cpp
XboxLiveContext^ xboxLiveContext = NULL;

// An XboxLiveContext for a user should be created only once, or you may encounter unpredictable behavior.
void OneTimeInit()
{
  // Get the XboxLiveContext.  This should only be done once per user after signing in.
  xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext(User::Users->GetAt(0));
}

void Example_SocialService_GetSocialRelationshipsAsync()
{
    // Generate a list of users on the system.
    // Create an XboxLiveContext from user 0 (the first one returned).
    // This user's authentication token will be used for service requests.
    XboxLiveContext^ xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext(User::Users->GetAt(0));

    // Asynchronously retrieve all social relationships from that context.
    auto asyncOp = xboxLiveContext->SocialService->GetSocialRelationshipsAsync();

    create_task( asyncOp )
    .then([](XboxSocialRelationshipResult^ result)
    {
        // For each social relationship of the specified user...
        for( XboxSocialRelationship^ xboxSocialRelationship : result->Items )
        {
            // ...display the public properties of the relationship.
            LogComment( xboxSocialRelationship->XboxUserId );
            LogComment( xboxSocialRelationship->IsFavorite.ToString() );
        }

    })
    .wait();
}
```
