---
author: jnHs
Description: Quand vous travaillez sur une application dans le tableau de bord du Centre de développement Windows, vous pouvez visualiser les informations concernant l’identité unique qui lui est affectée par le Windows Store et obtenir un lien d’accès à la description de votre application dans le Windows Store.
title: Visualiser les informations d’identité des applications
ms.assetid: 86F05A79-EFBC-4705-9A71-3A056323AC65
---

# Visualiser les informations d’identité des applications


Quand vous travaillez sur une application dans le tableau de bord du Centre de développement Windows, vous pouvez visualiser les informations concernant l&#39;identité unique qui lui est affectée par le Windows Store et obtenir un lien d&#39;accès à la description de votre application dans le Windows Store.

Pour consulter ces informations, accédez à l’une de vos applications, puis développez l’option **Gestion des applications** dans le menu de navigation gauche. Cliquez sur **Identité de l’application** pour afficher les détails correspondants.

> **Remarque** Pour visualiser la plupart de ces informations d’identité, vous devez disposer d’un [nom réservé](create-your-app-by-reserving-a-name.md) pour votre application.

## Valeurs à inclure dans votre manifeste appx


Votre manifeste appx doit contenir les valeurs ci-après. Si vous utilisez Microsoft Visual Studio pour générer vos packages, et que vous êtes connecté avec le même compte Microsoft que celui que vous avez associé à votre compte de développeur, ces informations sont incluses automatiquement. Si vous générez votre package manuellement, vous devrez y ajouter vous-même ces valeurs.

-   **Package/Identité/Nom**
-   **Package/Identité/Serveur de publication**

Pour plus d'informations, voir [**Identity**](https://msdn.microsoft.com/library/windows/apps/br211441) dans la [référence du schéma de manifeste de package](https://msdn.microsoft.com/library/windows/apps/br211473).

Utilisés conjointement, ces éléments déclarent l'identité de votre application en établissant la « famille de packages » à laquelle appartiennent tous les packages de l'application. Les différents packages comporteront des détails supplémentaires, comme l'architecture et la version.

## Valeurs supplémentaires concernant la famille de packages


Les valeurs supplémentaires ci-après font référence à la famille de packages de votre application, mais ne figurent pas dans votre manifeste.

-   **Nom de la famille de packages (PFN)** : cette valeur est utilisée avec certaines API Windows.
-   **SID du package** : vous aurez besoin de cette valeur pour envoyer des notifications WNS à votre application. Pour plus d'informations, voir l'article [Vue d'ensemble des services de notifications Push Windows (WNS)](https://msdn.microsoft.com/library/windows/apps/mt187203).

## Lien d’accès à la description de votre application

Vous pouvez partager le lien d’accès à la page de votre application afin d’aider vos clients à trouver cette dernière dans le Windows Store. Ce lien est au format **`https://www.microsoft.com/store/apps/<your app's Store ID>`**.

> **Remarque** Cette URL fonctionne pour n’importe quelle version du système d’exploitation, à condition que votre application soit disponible dessus. Vous pouvez également voir des liens supplémentaires pour Windows 8.1 et versions antérieures et/ou Windows Phone 8.1 et versions antérieures, qui ne fonctionnent que sur les versions de système d’exploitation spécifiées.

Lorsqu’un client clique sur ce lien, il accède automatiquement à la page de description Web de votre application. Si l’appareil Windows du client prend en charge votre application, l’application du Windows Store lancera et affichera également la description de votre application.

L’**ID Windows Store** de votre application figure également dans cette section. Cet ID Windows Store peut être utilisé pour [générer des badges Windows Store](http://go.microsoft.com/fwlink/p/?LinkId=534236) ou pour identifier votre application.

 

 






<!--HONumber=May16_HO2-->


