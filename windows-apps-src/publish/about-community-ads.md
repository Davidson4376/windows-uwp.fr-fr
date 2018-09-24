---
author: jnHs
Description: You can cross-promote your app with apps published by other developers. We call this feature community ads.
title: À propos des annonces de la communauté
ms.assetid: F55CE478-99AF-4B70-90D1-D16419562136
ms.author: wdg-dev-content
ms.date: 10/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ada2e0610e81e986deba3ddab5e87547e05fe108
ms.sourcegitcommit: 194ab5aa395226580753869c6b66fce88be83522
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2018
ms.locfileid: "4149912"
---
# <a name="about-community-ads"></a>À propos des annonces de la communauté

Si votre application [affiche des bannières ou des bannières spots publicitaires](../monetize/display-ads-in-your-app.md), vous pouvez promouvoir votre application avec d’autres développeurs avec des applications du Microsoft Store pour gratuitement. Nous appelons cette fonctionnalité *annonces de la communauté*.  

Voici comment fonctionne ce programme:

* Une fois que vous accepté de participer aux annonces de la Communauté, comme décrit ci-dessous, vous pouvez [créer une campagne de publicité de la Communauté gratuite](create-an-ad-campaign-for-your-app.md). Votre application partagera ensuite un espace publicitaire promotionnel avec d’autres développeurs ayant également accepté de participer aux annonces de la Communauté. Votre application affichera des publicités pour les applications publiées par d’autres développeurs participant aux annonces de la communauté et vice-versa.
* Vous gagnez des crédits pour l’espace publicitaire promotionnel dans d’autres applications en affichant des annonces de la communauté dans votre application. Les crédits sont calculés selon le processus suivant:
  * Pour chaque pays ou région où une application affichant des annonces de la communauté est disponible, la valeur du coût par impression électronique (eCPM) du marché concerné est multipliée par le nombre de demandes d’annonces de la communauté faites par votre application dans ce pays ou cette région. Cette valeur représente les crédits que vous avez gagnés pour votre application dans ce pays ou cette région.
  * Le total de vos crédits gagnés pour une période donnée est égal à la somme de tous les crédits obtenus dans chaque pays ou région pour chacune de vos applications affichant des annonces de la communauté.
* Vos crédits sont répartis équitablement entre toutes les campagnes d’annonces de la communauté actives et sont convertis en expositions publicitaires pour votre application, selon la valeur actuelle de l’eCPM des pays ciblés par votre campagne.
* Pour suivre les performances des publicités de la communauté dans votre application, consultez le [rapport sur les performances publicitaires](advertising-performance-report.md).

### <a name="opt-in-to-community-ads"></a>Accepter les publicités de la communauté

Avant de pouvoir créer une campagne de publicité de la Communauté pour l’une de vos applications, vous devez le sélectionner sur la **MONÉTISER** &gt; page **publicités dans l’application** dans le tableau de bord du centre de développement Windows.

Pour participer aux annonces de la Communauté pour une application UWP:

1. Dans la section **paramètres de médiation** sur la page **publicités dans l’application** , sélectionnez une unité publicitaire que vous utilisez dans l’application.
2. Si l’option de **laisser Microsoft choisir les meilleurs paramètres de médiation pour votre application** est activée, les annonces de la Communauté sont activées pour votre unité publicitaire automatiquement. Dans le cas contraire, sélectionnez la configuration de base ou une configuration spécifique au marché dans la liste déroulante des **cibles** , puis vérifiez la zone **d’annonces de la Communauté Microsoft** dans la liste **d’autres réseaux publicitaires** .

    > [!NOTE]
    > Vous pouvez utiliser les champs de **poids** pour spécifier le taux de publicités que vous souhaitez afficher à partir de réseaux payants et d’autres réseaux publicitaires, y compris les annonces de la Communauté.

Pour participer aux annonces de la Communauté pour un client Windows 8.x ou Windows Phone 8.x application,

1. Sur la page **publicités In-app** , cochez la case **afficher des annonces de la Communauté dans mon application** .

Vous n’avez pas besoin de republier votre application après avoir effectué vos sélections. Une fois que vous avez accepté cette fonctionnalité, vous êtes en mesure de sélectionner **Community ad (free)** comme type de campagne lorsque vous [créez une campagne de publicité](create-an-ad-campaign-for-your-app.md).

### <a name="related-topics"></a>Rubriques connexes

* [Publicités dans l’application](in-app-ads.md)
* [Créer une campagne de publicité pour votre application](create-an-ad-campaign-for-your-app.md)
