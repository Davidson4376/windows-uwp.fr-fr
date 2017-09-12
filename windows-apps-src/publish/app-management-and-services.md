---
author: jnHs
Description: "Gérez chacune de vos applications dans le tableau de bord du Centre de développement Windows, affichez-en les détails et configurez des services comme les notifications, les tests A/B et les cartes."
title: Gestion des applications et services
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 2ff75a5e85a37f4f47b6b32f3e8338524752bb2f
ms.sourcegitcommit: fadde8afee46238443ec1cb71846d36c91db9fb9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2017
---
# <a name="app-management-and-services"></a>Gestion des applications et services

Vous pouvez gérer chacune de vos applications dans le tableau de bord du Centre de développement Windows, en visualiser les détails et configurer des services comme les notifications, les tests A/B et les cartes.

Quand vous travaillez avec une application dans votre tableau de bord, vous visualisez les sections **Gestion des applications** et **Services** dans le menu de navigation de gauche. Développez ces sections pour accéder aux fonctionnalités décrites ci-après.

## <a name="services"></a>Services

La section **Services** vous permet de gérer les différents services pour vos applications.

## <a name="push-notifications"></a>Notifications Push

La section **Notifications Push** vous permet de créer et d’envoyer des notifications aux clients de votre application. Vous pouvez envoyer ces notifications à tous les clients de votre application, ou vous pouvez cibler un sous-ensemble de vos clients Windows10 qui remplissent les critères que vous avez définis dans un [segment de clients](create-customer-segments.md). Pour plus d’informations, consultez l’article [Envoyer des notifications aux clients de votre application](send-push-notifications-to-your-apps-customers.md).

En fonction du type de package de votre application et de sa configuration requise spécifique, vous pouvez utiliser l’une des options ci-après pour les notifications Push en cliquant sur **WNS/MPNS** dans le menu de navigation de gauche: 

-   **Windows Push Notification Services (WNS)** vous permet d’envoyer un toast, une vignette, un badge et des mises à jour brutes depuis votre propre service cloud. Pour plus d’informations, voir l’article [Vue d’ensemble des services de notifications Push Windows (WNS)](https://msdn.microsoft.com/library/windows/apps/mt187203).

-   **Microsoft Azure Mobile Apps** vous permet d’envoyer des notifications Push, d’authentifier et de gérer les utilisateurs des applications et de stocker les données des applications dans le cloud. Pour plus d’informations, voir la [documentation sur les applications mobiles](http://go.microsoft.com/fwlink/p/?LinkId=221116).

-   **Microsoft Push Notifications Service (MPNS)** peut être utilisé avec vos packages .xap pour Windows Phone. Vous pouvez envoyer un nombre limité de notifications non authentifiées sans intervenir sur la configuration, bien que nous vous recommandons d’utiliser des notifications authentifiées pour éviter les seuils de limitation. Si vous utilisez MPNS, vous devez charger un certificat sur le terrain tel que fourni sur la page des **notifications Push**. Pour plus d’informations, voir [Configuration d’un service web authentifié pour envoyer des notifications Push pour Windows Phone 8](http://go.microsoft.com/fwlink/p/?LinkId=528736).

## <a name="experimentation"></a>Expérimentation

Utilisez la page **Expérimentation** pour créer et exécuter des expériences pour vos applications de plateforme Windows universelle (UWP) avec des tests A/B. Un test A/B vous permet d’évaluer l’efficacité des modifications apportées aux fonctionnalités de votre application (ou leurs variantes) sur certains clients avant d’implémenter les modifications pour le reste de vos clients.

Pour plus d’informations, voir [Exécuter des expériences d’application avec des tests A/B](../monetize/run-app-experiments-with-a-b-testing.md).

## <a name="maps"></a>Cartes

Pour utiliser les services cartographiques dans les applications pour Windows Phone 8.1 et versions antérieures, vous devez insérer un identifiant et un jeton d’application de service cartographique dans le code de votre application. Ce jeton est accessible sur la page **Cartes**, dans la section **Services**.

> [!NOTE]
> Pour utiliser les services cartographiques dans des applications ciblant Windows10 ou Windows8.x, visitez le [Centre de développement Bing Cartes](http://go.microsoft.com/fwlink/p/?LinkId=614880). Pour plus d’informations, voir [Demander une clé d’authentification de cartes](https://msdn.microsoft.com/library/windows/apps/mt219694).

Pour plus d’informations, voir [Utiliser les services cartographiques](use-map-services.md).

## <a name="product-collections-and-purchases"></a>Collections et achats de produits

Pour utiliser l’API de collection du Windows Store et l’API d’achat du Windows Store afin d’accéder aux informations de propriété relatives aux applications et aux modules complémentaires, vous devez entrer les ID clients Azure AD associés ici. Notez que la prise en compte de ces modifications peut prendre jusqu’à 16heures.

Pour plus d’informations, consultez l’article [Gérer des droits sur les produits à partir d’un service](../monetize/view-and-grant-products-from-a-service.md).

## <a name="app-management"></a>Gestion des applications

La section **Gestion des applications** vous permet d’afficher les détails d’identité et de package, ainsi que de gérer les noms pour votre application.

## <a name="app-identity"></a>Identité des applications

Cette page propose des informations détaillées sur l’identité unique de votre application dans le Windows Store, notamment la ou les URL à associer à la description de l’application.

Pour plus d’informations, voir [Visualiser les détails d’identité d’application](view-app-identity-details.md).

## <a name="manage-app-names"></a>Gestion des noms d’application

C’est là que vous visualisez tous les noms que vous avez réservés pour votre application. Vous pouvez réserver d’autres noms ici ou supprimer des noms que vous n’utilisez plus.

Pour plus d’informations, voir [Gestion des noms d’application](manage-app-names.md).

## <a name="current-packages"></a>Packages actuels

Cette page propose des informations détaillées sur tous vos packages publiés.

> [!NOTE]
> Aucune information ne figure sur cette page tant que votre application n’a pas été publiée.

Nous affichons le nom, la version et l’architecture de chaque package. Cliquez sur **Détails** pour accéder à des informations complémentaires comme la langue prise en charge, les fonctionnalités de l’application et les tailles de fichier. Les informations affichées pour chaque package peuvent varier en fonction du système d’exploitation ciblé et d’autres facteurs. 

Les développeurs ayant des autorisations OEM peuvent également [générer des packages de préinstallation](generate-preinstall-packages-for-oems.md) depuis la page **Packages actuels**.

 

 
