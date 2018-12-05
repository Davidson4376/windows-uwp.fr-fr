---
Description: View details related to the unique identity assigned to your app by the Microsoft Store, and get a link to your app's Store listing.
title: Visualiser les informations d’identité des applications
ms.assetid: 86F05A79-EFBC-4705-9A71-3A056323AC65
ms.date: 10/02/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 114524294a5b77165167601c1479b6c3894bb29b
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8738088"
---
# <a name="view-app-identity-details"></a>Visualiser les informations d’identité des applications


Vous pouvez afficher les détails concernant l’identité unique attribuée à votre application par le Microsoft Store sur ses pages de **l’identité de l’application** . Vous pouvez également obtenir un lien vers le Windows Store de votre application sur cette page de description.

Pour consulter ces informations, accédez à l’une de vos applications, puis développez **Gestion des applications** dans le menu de navigation de gauche. Sélectionnez **Identité de l’application** pour afficher les détails correspondants.


## <a name="values-to-include-in-your-app-package-manifest"></a>Valeurs à inclure dans le manifeste du package de votre application

Les valeurs suivantes doivent être inclus dans votre manifeste du package. Si vous [utilisez Microsoft VisualStudio pour générer vos packages](../packaging/packaging-uwp-apps.md), et que vous êtes connecté avec le même compte Microsoft que celui que vous avez associé à votre compte de développeur, ces informations sont incluses automatiquement. Si vous générez votre package manuellement, vous devrez y ajouter vous-même ces éléments:

-   **Package/Identité/Nom**
-   **Package/Identité/Serveur de publication**
-   **Package/Propriétés/NomCompletÉditeur**

Pour plus d’informations, consultez l’article [**Identity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) dans la [documentation de référence du schéma de manifeste du package](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root).

Utilisés conjointement, ces éléments déclarent l'identité de votre application en établissant la « famille de packages » à laquelle appartiennent tous les packages de l'application. Les différents packages comporteront des détails supplémentaires, comme l'architecture et la version.


## <a name="additional-values-for-package-family"></a>Valeurs supplémentaires concernant la famille de packages

Les valeurs supplémentaires ci-après font référence à la famille de packages de votre application, mais ne figurent pas dans votre manifeste.

-   **Nom de la famille de packages (PFN)** : cette valeur est utilisée avec certaines API Windows.
-   **SID du package** : vous aurez besoin de cette valeur pour envoyer des notifications WNS à votre application. Pour plus d'informations, voir l'article [Vue d'ensemble des services de notifications Push Windows (WNS)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md).


## <a name="link-to-your-apps-listing"></a>Lien d’accès à la description de votre application

Vous pouvez partager le lien d’accès direct à la page de votre application afin d’aider vos clients à trouver cette dernière dans le WindowsStore. Ce lien présente le format **`https://www.microsoft.com/store/apps/<your app's Store ID>`**. Lorsqu’un client clique sur ce lien, la page de description web de votre application s’affiche. Sur les appareils Windows, l’application du WindowsStore lancera et affichera également la description de votre application.

L’**ID Windows Store** de votre application figure également dans cette section. Cet IDStore peut être utilisé pour [générer des badges WindowsStore](http://go.microsoft.com/fwlink/p/?LinkId=534236) ou pour identifier votre application.

Le **lien du protocole WindowsStore** vous permet de créer un lien d’accès direct à votre application dans le WindowsStore sans ouvrir un navigateur, par exemple lorsque vous établissez ce lien à partir d’une application. Pour plus d’informations, consultez l’article [Créer un lien vers votre application](link-to-your-app.md).



 

 




