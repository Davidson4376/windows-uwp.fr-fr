---
author: mcleanbyron
ms.assetid: 9FCBAF2E-5419-4169-A17C-9C4058DCF909
description: "Le WindowsStore expose plusieurs services que vous pouvez appeler par le biais d’API REST afin d’accéder par programmation à certains types de données pour les applications qui sont inscrites sur votre compte personnel du Centre de développement Windows ou sur celui de votre d’organisation."
title: Services du WindowsStore
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, uwp, services du Windows Store
ms.openlocfilehash: 27e4084cc3ab168fcb68969f973ad21f040cec4f
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="windows-store-services"></a>Services du WindowsStore

Le WindowsStore expose plusieurs services que vous pouvez appeler par le biais d’API REST afin d’accéder par programmation à certains types de données pour les applications qui sont inscrites sur votre compte personnel du Centre de développement Windows ou sur celui de votre d’organisation.

## <a name="in-this-section"></a>Dans cette section


| Rubrique            | Description                 |
|------------------|-----------------------------|
| [Accéder aux données d’analyse](access-analytics-data-using-windows-store-services.md) | Utilisez *l’API d’analyse du Windows Store* pour récupérer par programme les données d’analyse de vos applications. Cette API permet de récupérer les données pour les acquisitions d'applications et de modules complémentaires (également appelés produits in-app ou PIA), les échecs des applications, les classifications et les vérifications d'applications, ainsi que les données de performances pour les publicités in-app et les campagnes publicitaires. |
| [Répondre aux avis](respond-to-reviews-using-windows-store-services.md) | Utilisez cette méthode dans l'*API d'avis du WindowsStore* pour répondre par programmation aux avis concernant votre application dans le WindowsStore. Cette API est particulièrement utile pour les développeurs qui souhaitent répondre en bloc à de nombreux avis sans utiliser le tableau de bord du Centre de développement Windows.  |
| [Exécuter des campagnes publicitaires](run-ad-campaigns-using-windows-store-services.md) | Utilisez l'*API des promotions du WindowsStore* pour gérer par programmation des campagnes publicitaires pour promouvoir vos applications. Cette API permet de créer, de mettre à jour et de surveiller vos campagnes et d'autres ressources connexes, telles que des options créatives et de ciblage. Cette API est particulièrement utile pour les développeurs qui créent des volumes importants de campagnes publicitaires, et qui souhaitent le faire sans utiliser le tableau de bord du Centre de développement Windows. |
| [Créer et gérer des soumissions](create-and-manage-submissions-using-windows-store-services.md) | L’*API de soumission du Windows Store* vous permet d’interroger et de créer par programmation des soumissions pour des applications, des extensions et des versions d’évaluation de package pour votre compte personnel du Centre de développement Windows ou celui de votre d’organisation. Cette API est utile si votre compte gère beaucoup d’applications ou d’extensions, et que vous voulez automatiser et optimiser le processus de soumission de ces ressources. |
| [Gérer les droits sur les produits à partir d’un service](view-and-grant-products-from-a-service.md)  | Si vous disposez d’un catalogue d’applications et de modules complémentaires dans le WindowsStore, vous pouvez utiliser l'*API de collection du WindowsStore* et l'*API d’achat du WindowsStore* pour accéder aux informations de propriété de ces produits à partir de vos services, signaler le traitement de la commande d’un produit consommable pour un utilisateur et accorder des droits pour un produit gratuit à un utilisateur.  |
| [API de métadonnées d’application pour les réseaux publicitaires](app-metadata-api-for-advertising-networks.md)  | Les réseaux publicitaires peuvent utiliser cette API pour récupérer par programme des métadonnées sur les applications dans le Windows Store, notamment des détails tels que la description et la catégorie pour les descriptions dans le Windows Store de l’application, ainsi que des informations sur le fait que l’application cible ou non des enfants de moins de 13ans. L’accès à cette API est actuellement limité aux développeurs bénéficiant d’une autorisation accordée par Microsoft.  |
