---
Description: Le rapport d’acquisitions de module complémentaire dans Partner Center vous permet de voir combien de modules complémentaires que vous avez achetés, ainsi que des données démographiques et les détails de la plateforme.
title: Rapport sur les acquisitions des extensions
ms.assetid: F2DF9188-0A98-4AC3-81C0-3E2C37B15582
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, ventes d’extensions, acquisitions d’extensions, produits in-app, extensions
ms.localizationpriority: medium
ms.openlocfilehash: 8027276779dac59f0745dd8053ee73cf1615e630
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57625834"
---
# <a name="add-on-acquisitions-report"></a>Rapport sur les acquisitions des extensions


Le **acquisitions de module complémentaire** signaler dans [partenaires](https://partner.microsoft.com/dashboard) vous permet de voir combien de modules complémentaires que vous avez achetés, ainsi que des données démographiques et des détails de la plateforme et affiche les informations de conversion pour les clients sur Windows 10 ( notamment la Xbox). Vous pouvez également afficher près de données d’acquisition en temps réel pour la dernière période ou soixante-dix deux heures.

Vous pouvez afficher ces données dans le centre de partenaires, ou [télécharger le rapport](download-analytic-reports.md) à afficher en mode hors connexion. Vous pouvez également récupérer ces données par programme à l’aide de la méthode [obtenir des acquisitions d’extensions](../monetize/get-in-app-acquisitions.md) dans [l’API REST d’analyse du Microsoft Store](../monetize/access-analytics-data-using-windows-store-services.md).

Dans ce rapport, une acquisition d’extension signifie qu’un client vous a acheté une extension (ou l’a acquise sans frais si vous l’avez offerte gratuitement). Plusieurs achats d’un même module complémentaire de type consommable effectués par le même client sont comptabilisés comme différentes acquisitions de modules complémentaires.

> [!IMPORTANT]
> Le **acquisitions de module complémentaire** rapport n’inclut pas les données sur les remboursements, retournements, facturation, etc. Pour estimer le résultat de l’application, visitez [synthèse des paiements](payout-summary.md). Dans la section **Réservé**, cliquez sur le lien **Télécharger les transactions réservées**.


## <a name="apply-filters"></a>Appliquer les filtres

Dans la zone supérieure de la page, vous pouvez sélectionner la période sur laquelle portent les données qui vous intéressent. La valeur par défaut est de **30D** (30 jours), mais vous pouvez choisir d’afficher les données portant sur des périodes de 3, 6 ou 12 mois, ou sur une plage de dates personnalisée que vous spécifiez. Vous pouvez également sélectionner **1H** ou **72 heures** pour afficher les données d’acquisition en temps quasi réel soit une heure ou deux soixante-dix heures ; ces périodes de temps s’appliquent uniquement à la **module complémentaire quotidiennement** onglet de la **acquisitions de module complémentaire** graphique et en le **Acquisitions** onglet de la **marchés** graphique. 

Vous pouvez également développer l’option **Filtres** pour filtrer toutes les données de cette page par extension, par marché et/ou par type d’appareil.

-   **Module complémentaire**: Le filtre par défaut est **tous les modules complémentaires**, mais vous pouvez limiter les données à un ou plusieurs des modules complémentaires de l’application.
-   **Marché**: Le filtre par défaut est **tous les marchés**, mais vous pouvez limiter les données à des acquisitions dans un ou plusieurs marchés.
-   **Type d’appareil**: Le paramètre par défaut est **tous les appareils**. Si vous souhaitez afficher les données d’acquisitions relatives à un certain type d’appareil uniquement (PC, console ou tablette, par exemple), vous pouvez en choisir un ici.

Les informations figurant dans tous les graphiques répertoriés ci-après correspondent à la plage de dates et à tous les filtres que vous avez sélectionnés. Certaines sections vous permettent également d’appliquer des filtres supplémentaires.


## <a name="add-on-acquisitions"></a>Acquisitions de modules complémentaires

Le graphique **Acquisitions de modules complémentaires** affiche le nombre total d’acquisitions quotidiennes ou hebdomadaires de vos modules complémentaires au cours de la période sélectionnée. (Lorsque vous utilisez l’option **Appliquer des filtres** afin d’afficher les données portant sur une période plus étendue, les données d’acquisition sont regroupées par semaine.)

Vous pouvez également visualiser le nombre total d’acquisitions sur toute la durée de vie de votre application en sélectionnant **Add-on cumulative**. Vous avez alors accès au total cumulé de l'ensemble des acquisitions effectuées depuis la première publication de votre application.

Si vous le souhaitez, vous pouvez filtrer les résultats par acquisition d’extension émanant du client ou du Windows Store sur le web et/ou par version du système d’exploitation.


## <a name="customer-demographic"></a>Données démographiques sur les clients

Le graphique **Données démographiques sur les clients** présente des informations démographiques sur les personnes qui ont acquis vos extensions. Vous pouvez voir le nombre d’acquisitions (au cours de la période sélectionnée) effectuées par des personnes appartenant à un certain groupe d’âge et par sexe.

> [!NOTE]
> Certains clients ont choisi de ne pas partager ces informations. Si nous n’avons pas pu déterminer le groupe d’âge ni le sexe, l’acquisition entre dans la catégorie **Inconnu**.


## <a name="markets"></a>Marchés

Le graphique **Marchés** indique le nombre total d’acquisitions d’extension au cours de la période sélectionnée pour chaque marché dans lequel votre application est disponible. 

Vous pouvez visualiser ces données sous la forme visuelle **Carte** ou **Tableau**. La représentation sous forme de tableau affiche cinq marchés à la fois, soit dans l’ordre alphabétique, soit par nombre maximal ou minimal d’acquisitions ou d’installations. Vous pouvez également télécharger ces données afin de visualiser d’un seul coup d’œil les informations relatives à la totalité des marchés.


## <a name="add-on-page-views-and-conversions-by-campaign-id"></a>Vues et conversions de la page d’extension par ID de campagne

Le graphique **Vues et conversions de la page d’extension par ID de campagne** vous présente le nombre total de conversions d'extensions (acquisitions) par ID de campagne au cours de la période sélectionnée, ce qui vous permet d’effectuer le suivi des conversions et des vues de page par des clients qui utilisent Windows 10 (y compris Xbox) pour chacune de vos [campagnes de promotion personnalisées](create-a-custom-app-promotion-campaign.md). Ce graphique affiche uniquement les conversions d’extensions.

> [!NOTE]
> Il est possible que des clients soient arrivés à la description de votre application en cliquant sur une campagne personnalisée qui n’a pas été créée par vos soins. Nous marquons chaque vue de page dans une session avec l’ID de campagne à partir duquel l’utilisateur a accédé au Windows Store. Puis nous attribuons à cet ID de campagne les conversions appropriées parmi toutes les acquisitions effectuées dans les 24 heures. Par conséquent, le nombre total de conversions peut être plus élevé que celui de vos ID de campagne, et certaines de vos conversions ou conversions d’extension peuvent présenter un nombre de vues de page nul. 


## <a name="conversions-breakdown-by-campaign-id"></a>Répartition des conversions par ID de campagne

Le graphique **Répartition des conversions par ID de campagne** vous permet d’effectuer le suivi des conversions et des vues de page par des clients qui utilisent Windows 10 pour chacune de vos [campagnes de promotion personnalisées](create-a-custom-app-promotion-campaign.md). Ce graphique affiche les conversions d’application et d’extension par ID de campagne.

Dans ce graphique, une *vue de page* signifie qu’un client a consulté la description de l’application dans le Windows Store. Une *conversion* signifie qu’un client a obtenu une licence pour l’application ou pour l’extension (qu’elle soit payante ou gratuite).

Gardez à l’esprit que ces nombres de vues de page et de conversions ne correspondent pas à des nombres de clients uniques. 


## <a name="top-add-ons"></a>Top des modules complémentaires

Le graphique **Top des modules complémentaires** présente le nombre total d’acquisitions de chacune de vos extensions au cours de la période sélectionnée, ce qui vous permet de déterminer les extensions les plus populaires. 



 

 
