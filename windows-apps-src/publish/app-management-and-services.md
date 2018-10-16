---
author: jnHs
Description: Manage and view details related to each of your apps in the Windows Dev Center dashboard, and configure services such as A/B testing and maps.
title: Gestion des applications et services
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
ms.author: wdg-dev-content
ms.date: 07/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d0e4be450aa972ad8561f27a8d4749050458520a
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4682445"
---
# <a name="app-management-and-services"></a>Gestion des applications et services

Vous pouvez gérer chacune de vos applications dans le tableau de bord du Centre de développement Windows, en visualiser les détails et configurer des services comme les notifications, les tests A/B et les cartes.

Quand vous travaillez avec une application dans votre tableau de bord, vous visualisez les sections **Gestion des applications** et **Services** dans le menu de navigation de gauche. Développez ces sections pour accéder aux fonctionnalités décrites ci-après.

## <a name="services"></a>Services

La section **Services** vous permet de gérer les différents services pour vos applications.

## <a name="xbox-live"></a>XboxLive

Si vous publiez un jeu, vous pouvez activer le [Programme créateurs Xbox Live](http://xbox.com/developers/creators-program) sur cette page. Cela vous permet de démarrer la configuration et le tests des fonctionnalités Xbox Live et finalement publier votre jeu programme créateurs Xbox Live.

Pour plus d’informations, consultez [vous familiariser avec le programme créateurs Xbox Live](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md) et [créez un nouveau titre du programme créateurs Xbox Live et publier dans l’environnement de test](../xbox-live/get-started-with-creators/create-and-test-a-new-creators-title.md).

## <a name="experimentation"></a>Expérimentation

Utilisez la page **Expérimentation** pour créer et exécuter des expériences pour vos applications de plateforme Windows universelle (UWP) avec des tests A/B. Un test A/B vous permet d’évaluer l’efficacité des modifications apportées aux fonctionnalités de votre application (ou leurs variantes) sur certains clients avant d’implémenter les modifications pour le reste de vos clients.

Pour plus d’informations, voir [Exécuter des expériences d’application avec des tests A/B](../monetize/run-app-experiments-with-a-b-testing.md).

## <a name="maps"></a>Cartes

Pour utiliser les services cartographiques dans les applications pour Windows Phone 8.1 et versions antérieures, vous devez insérer un identifiant et un jeton d’application de service cartographique dans le code de votre application. Ce jeton est accessible sur la page **Cartes**, dans la section **Services**.

> [!NOTE]
> Pour utiliser les services cartographiques dans des applications ciblant Windows10 ou Windows8.x, visitez le [Centre de développement Bing Cartes](http://go.microsoft.com/fwlink/p/?LinkId=614880). Pour plus d’informations, voir [Demander une clé d’authentification de cartes](https://docs.microsoft.com/windows/uwp/maps-and-location/authentication-key).

Pour plus d’informations, voir [Utiliser les services cartographiques](use-map-services.md).

## <a name="product-collections-and-purchases"></a>Collections et achats de produits

Pour utiliser le Microsoft Store API de collection et l’API d’achat de Microsoft Store pour accéder aux informations de propriété pour les applications et modules complémentaires, vous devez entrer associé ID clients Azure AD ici. Notez que la prise en compte de ces modifications peut prendre jusqu’à 16heures.

Pour plus d’informations, consultez l’article [Gérer des droits sur les produits à partir d’un service](../monetize/view-and-grant-products-from-a-service.md).

## <a name="administrator-consent"></a>Consentement de l’administrateur

f votre produit s’intègre avec Azure AD et appelle des API qui requièrent des [autorisations de l’application ou les autorisations déléguées](https://developer.microsoft.com/graph/docs/concepts/permissions_reference) qui nécessite le consentement de l’administrateur, entrez votre ID d’Azure AD Client ici. Cela permet aux administrateurs qui achètent l’application leur organisation accorder à donner son consentement pour votre produit à agir pour le compte de tous les utilisateurs du client.

Pour plus d’informations, voir la [demande de consentement pour un client entière](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-scopes#requesting-consent-for-an-entire-tenant).

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

## <a name="wnsmpns"></a>WNS/MPNS

La section **WNS/MPNS** fournit des options pour vous aider à créer et envoyer des notifications aux clients de votre application. 

> [!TIP]
> Pour les applications UWP, nous vous conseillons d’à l’aide de l’option de **notification** dans le tableau de bord. Cette fonctionnalité vous permet d’envoyer des notifications à tous les clients de votre application, ou à un sous-ensemble ciblé de vos clients Windows 10 qui remplissent les critères que vous avez définis dans un [segment de clientèle](create-customer-segments.md). Pour plus d’informations, consultez l’article [Envoyer des notifications aux clients de votre application](send-push-notifications-to-your-apps-customers.md).

Selon le type de package de votre application et ses besoins spécifiques, vous pouvez également utiliser une des options suivantes: 

-   **Windows Push Notification Services (WNS)** vous permet d’envoyer un toast, une vignette, un badge et des mises à jour brutes depuis votre propre service cloud. Pour plus d’informations, voir l’article [Vue d’ensemble des services de notifications Push Windows (WNS)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md).

-   **Microsoft Azure Mobile Apps** vous permet d’envoyer des notifications Push, d’authentifier et de gérer les utilisateurs des applications et de stocker les données des applications dans le cloud. Pour plus d’informations, voir la [documentation sur les applications mobiles](http://go.microsoft.com/fwlink/p/?LinkId=221116).

-   **Microsoft Push Notifications Service (MPNS)** peut être utilisé avec vos packages .xap pour Windows Phone. Vous pouvez envoyer un nombre limité de notifications non authentifiées sans intervenir sur la configuration, bien que nous vous recommandons d’utiliser des notifications authentifiées pour éviter les seuils de limitation. Si vous utilisez MPNS, vous devez charger un certificat dans le champ fourni sur la page **WNS/MPNS** . Pour plus d’informations, voir [Configuration d’un service web authentifié pour envoyer des notifications Push pour Windows Phone 8](http://go.microsoft.com/fwlink/p/?LinkId=528736).
 

 
