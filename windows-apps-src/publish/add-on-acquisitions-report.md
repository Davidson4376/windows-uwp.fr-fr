---
author: jnHs
Description: The Add-on acquisitions report in the Windows Dev Center dashboard lets you see how many add-ons you've sold, along with demographic and platform details.
title: Rapport sur les acquisitions d’extensions
ms.assetid: F2DF9188-0A98-4AC3-81C0-3E2C37B15582
ms.author: wdg-dev-content
ms.date: 05/24/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, ventes d’extensions, acquisitions d’extensions, produits in-app, extensions
ms.localizationpriority: medium
ms.openlocfilehash: 019bb410e6ac65f9951f06052c78f40e9a5f32e2
ms.sourcegitcommit: 5dda01da4702cbc49c799c750efe0e430b699502
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/21/2018
ms.locfileid: "4119447"
---
# <a name="add-on-acquisitions-report"></a>Rapport sur les acquisitions d’extensions


Le rapport **acquisitions de modules complémentaires** dans le tableau de bord du centre de développement Windows vous permet de visualiser le nombre de modules complémentaires que vous avez vendus, ainsi que des données démographiques et des informations sur la plateforme et affiche les informations de conversion pour les clients sur Windows 10 (y compris Xbox). Vous pouvez également afficher les données d’acquisition en temps réel pour la dernière période ou 70-deux heures.

Vous pouvez visualiser ces données dans votre tableau de bord ou [télécharger le rapport](download-analytic-reports.md) et le consulter hors connexion. Vous pouvez également récupérer ces données par programme à l’aide de la méthode [obtenir des acquisitions d’extensions](../monetize/get-in-app-acquisitions.md) dans [l’API REST d’analyse du MicrosoftStore](../monetize/access-analytics-data-using-windows-store-services.md).

Dans ce rapport, une acquisition d’extension signifie qu’un client vous a acheté une extension (ou l’a acquise sans frais si vous l’avez offerte gratuitement). Plusieurs achats d’une même extension de type consommable effectués par le même client sont comptabilisés comme différentes acquisitions d’extension.

> [!IMPORTANT]
> Le rapport **Acquisitions de modules complémentaires** n’inclut aucune donnée sur les remboursements, reprises, rétrofacturations, etc. Pour évaluer les revenus de votre application, consultez l’article [Résumé du paiement](payout-summary.md). Dans la section **Réservé**, cliquez sur le lien **Télécharger les transactions réservées**.


## <a name="apply-filters"></a>Appliquer des filtres

Dans la zone supérieure de la page, vous pouvez sélectionner la période sur laquelle portent les données qui vous intéressent. La valeur par défaut est de **30D** (30jours), mais vous pouvez choisir d’afficher les données portant sur des périodes de 3, 6 ou 12mois, ou sur une plage de dates personnalisée que vous spécifiez. Vous pouvez également sélectionner **1 H** ou **72 H** pour afficher les données d’acquisition en temps quasi réel pour une heure ou de 70-deux heures; ces périodes de temps s’appliquent uniquement à l’onglet de **l’extension de tous les jours** du graphique **acquisitions de modules complémentaires** et à l’onglet **Acquisitions** du graphique **des marchés** . 

Vous pouvez également développer l’option **Filtres** pour filtrer toutes les données de cette page par extension, par marché et/ou par type d’appareil.

-   **Extension**: le filtre par défaut est **Tous les modules complémentaires**, mais vous pouvez restreindre les données à une ou plusieurs extensions de l’application.
-   **Marché**: le filtre par défaut est **Tous les marchés**, mais vous pouvez restreindre les données aux acquisitions relatives à un ou plusieurs marchés.
-   **Type d’appareil**: le paramétrage par défaut est **Tous les appareils**. Si vous souhaitez afficher les données d’acquisitions relatives à un certain type d’appareil uniquement (PC, console ou tablette, par exemple), vous pouvez en choisir un ici.

Les informations figurant dans tous les graphiques répertoriés ci-après correspondent à la plage de dates et à tous les filtres que vous avez sélectionnés. Certaines sections vous permettent également d’appliquer des filtres supplémentaires.


## <a name="add-on-acquisitions"></a>Acquisitions de modules complémentaires

Le graphique **Acquisitions de modules complémentaires** affiche le nombre total d’acquisitions quotidiennes ou hebdomadaires de vos modules complémentaires au cours de la période sélectionnée. (Lorsque vous utilisez l’option **Appliquer des filtres** afin d’afficher les données portant sur une période plus étendue, les données d’acquisition sont regroupées par semaine.)

Vous pouvez également visualiser le nombre total d’acquisitions sur toute la durée de vie de votre application en sélectionnant **Add-on cumulative**. Vous avez alors accès au total cumulé de l’ensemble des acquisitions effectuées depuis la première publication de votre application.

Si vous le souhaitez, vous pouvez filtrer les résultats par acquisition d’extension émanant du client ou du WindowsStore sur le web et/ou par version du système d’exploitation.


## <a name="customer-demographic"></a>Données démographiques sur les clients

Le graphique **Données démographiques sur les clients** présente des informations démographiques sur les personnes qui ont acquis vos extensions. Vous pouvez voir le nombre d’acquisitions (au cours de la période sélectionnée) effectuées par des personnes appartenant à un certain groupe d’âge et par sexe.

> [!NOTE]
> Certains clients ont choisi de ne pas partager ces informations. Si nous n’avons pas pu déterminer le groupe d’âge ni le sexe, l’acquisition entre dans la catégorie **Inconnu**.


## <a name="markets"></a>Marchés

Le graphique **Marchés** indique le nombre total d’acquisitions d’extension au cours de la période sélectionnée pour chaque marché dans lequel votre application est disponible. 

Vous pouvez visualiser ces données sous la forme visuelle **Carte** ou **Tableau**. La représentation sous forme de tableau affiche cinq marchés à la fois, soit dans l’ordre alphabétique, soit par nombre maximal ou minimal d’acquisitions ou d’installations. Vous pouvez également télécharger ces données afin de visualiser d’un seul coup d’œil les informations relatives à la totalité des marchés.


## <a name="add-on-page-views-and-conversions-by-campaign-id"></a>Vues et conversions de la page d’extension par ID de campagne

Le graphique **Vues et conversions de la page d’extension par ID de campagne** vous présente le nombre total de conversions d'extensions (acquisitions) par ID de campagne au cours de la période sélectionnée, ce qui vous permet d’effectuer le suivi des conversions et des vues de page par des clients qui utilisent Windows10 (y compris Xbox) pour chacune de vos [campagnes de promotion personnalisées](create-a-custom-app-promotion-campaign.md). Ce graphique affiche uniquement les conversions d’extensions.

> [!NOTE]
> Il est possible que des clients soient arrivés à la description de votre application en cliquant sur une campagne personnalisée qui n’a pas été créée par vos soins. Nous marquons chaque vue de page dans une session avec l’ID de campagne à partir duquel l’utilisateur a accédé au WindowsStore. Puis nous attribuons à cet ID de campagne les conversions appropriées parmi toutes les acquisitions effectuées dans les 24heures. Par conséquent, le nombre total de conversions peut être plus élevé que celui de vos ID de campagne, et certaines de vos conversions ou conversions d’extension peuvent présenter un nombre de vues de page nul. 


## <a name="conversions-breakdown-by-campaign-id"></a>Répartition des conversions par ID de campagne

Le graphique **Répartition des conversions par ID de campagne** vous permet d’effectuer le suivi des conversions et des vues de page par des clients qui utilisent Windows10 pour chacune de vos [campagnes de promotion personnalisées](create-a-custom-app-promotion-campaign.md). Ce graphique affiche les conversions d’application et d’extension par ID de campagne.

Dans ce graphique, une *vue de page* signifie qu’un client a consulté la description de l’application dans le WindowsStore. Une *conversion* signifie qu’un client a obtenu une licence pour l’application ou pour l’extension (qu’elle soit payante ou gratuite).

Gardez à l’esprit que ces nombres de vues de page et de conversions ne correspondent pas à des nombres de clients uniques. 


## <a name="top-add-ons"></a>Top des modules complémentaires

Le graphique **Top des modules complémentaires** présente le nombre total d’acquisitions de chacune de vos extensions au cours de la période sélectionnée, ce qui vous permet de déterminer les extensions les plus populaires. 



 

 
