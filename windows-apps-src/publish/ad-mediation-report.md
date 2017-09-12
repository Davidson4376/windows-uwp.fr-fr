---
author: jnHs
Description: "Le rapport de médiation publicitaire vous permet de voir votre taux de remplissage effectif, ainsi que les taux de remplissage respectifs des réseaux publicitaires que vous utilisez."
title: "Rapport de médiation publicitaire"
ms.assetid: 18A33928-B9F2-4F76-9A9C-F01FEE42FEA1
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 0c08c1061231b033dc5401c77085e16ea7001bb1
ms.sourcegitcommit: fadde8afee46238443ec1cb71846d36c91db9fb9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2017
---
# <a name="ad-mediation-report"></a>Rapport de médiation publicitaire

Ce rapport affiche les données de médiation publicitaire pour les applications Windows8.x ou Windows Phone8.x qui utilisent **AdMediatorControl** du [SDK Microsoft Advertising pour Windows et Windows Phone8.x](http://aka.ms/store-8-sdk) pour afficher des bannières publicitaires à partir de plusieurs réseaux publicitaires. Pour ces applications, ce rapport vous permet de voir votre taux de remplissage effectif, ainsi que les taux de remplissage respectifs des réseaux publicitaires que vous utilisez. Il montre également les taux d'adoption de chacune de vos configurations de médiation et vous offre une réelle visibilité sur les erreurs signalées par les réseaux publicitaires et le médiateur. Vous pouvez visualiser ces données dans votre tableau de bord ou [télécharger le rapport](download-analytic-reports.md) et le consulter hors connexion.

> [!NOTE]
> Le rapport **Médiation publicitaire** n’est disponible que si vous utilisez une classe **AdMediatorControl** dans votre application Windows8.x ou WindowsPhone8.x. Pour plus d’informations, consultez [cet article](https://msdn.microsoft.com/library/windows/apps/xaml/dn864359). Pour une application UWP qui utilise la [médiation publicitaire](monetize-with-ads.md#mediation) dans un contrôle **AdControl** ou **InterstitialAd**, utilisez le [rapport sur les performances publicitaires](advertising-performance-report.md) pour examiner les données de performances des réseaux publicitaires.

## <a name="page-filters"></a>Filtres de page

Dans la zone supérieure de la page, vous pouvez développer l'option **Filtres de page** pour filtrer toutes les données de cette page par plage de dates et/ou par marché.

-   **Date** : la valeur par défaut de ce filtre est **30 derniers jours**, mais vous pouvez étendre cette période aux **12 derniers mois**.
-   **Marché** : la valeur par défaut de ce filtre est **Tous les marchés**. Vous pouvez choisir un marché spécifique si vous souhaitez ne visualiser que les évaluations des clients sur ce marché.
-   **Plateforme** : la valeur par défaut est **Toutes les plateformes**. Si votre application cible plusieurs plateformes, vous pouvez en choisir une.

Les informations figurant dans tous les graphiques répertoriés ci-après correspondent à la période sélectionnée dans **Filtres de page**. Par défaut, ces données portent sur tous les marchés et toutes les plateformes sur lesquels votre application apparaît, sauf si vous avez utilisé **Filtres de page** pour spécifier un marché et/ou une plateforme spécifique(s).

## <a name="ad-mediation-performance"></a>Performances de la médiation publicitaire

Le graphique **Performances de la médiation publicitaire** montre le taux de remplissage total moyen sur la période sélectionnée. Il s'agit du taux de remplissage moyen sur toutes les sessions utilisateur, quelle que soit la configuration de votre médiation ou la fréquence selon laquelle vos différents réseaux publicitaires ont été appelés.

Vous pouvez cliquer sur l'en-tête **Demandes de médiation** pour voir le nombre moyen de demandes de médiation individuelles, ou cliquer sur **Affichage publicitaire** pour voir le nombre total moyen d'affichages publicitaires.

## <a name="ad-provider-fill-rates"></a>Taux de remplissage des fournisseurs de publicités

Le graphique **Taux de remplissage des fournisseurs de publicités** montre le taux de remplissage moyen de chacun de vos réseaux publicitaires sur la période sélectionnée.

Ces informations sont regroupées pour vous aider à comparer les performances de chacun de ces réseaux publicitaires.

## <a name="unique-users-per-mediation-configuration"></a>Utilisateurs uniques par configuration de médiation

Le graphique **Utilisateurs uniques par configuration de médiation** montre le nombre total d'utilisateurs uniques qui ont reçu chaque version de votre configuration de médiation sur la période sélectionnée.

## <a name="errors-by-ad-network"></a>Erreurs par réseau de publicité

Le graphique **Erreurs par réseau de publicité** montre le nombre total de demandes et d'erreurs pour chacun de vos réseaux publicitaires, ainsi que le pourcentage de demandes qui ont conduit à une erreur.

## <a name="errors-by-type"></a>Erreurs par type

Le graphique **Erreurs par type** montre les erreurs rencontrées par chaque réseau publicitaire. Il indique également le pourcentage total qu'une erreur spécifique représente sur un réseau, pour que vous puissiez vous faire une idée des erreurs récurrentes sur chacun des réseaux publicitaires.

 

 
