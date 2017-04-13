---
author: shawjohn
Description: "Le rapport Canaux et conversions dans le tableau de bord du Centre de développement Windows vous permet de voir comment les clients de Windows 10 sont arrivés à votre application."
title: Rapport sur les canaux et conversions
ms.assetid: C359B9FB-A17B-4A8E-B8EE-19F2F98AA4FF
ms.author: johnshaw
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, uwp, canaux, conversions, rapport, analyse
ms.openlocfilehash: cbdd2e530594b97847196941580e2837d71698d0
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="channels-and-conversions-report"></a>Rapport sur les canaux et conversions


Le rapport **Canaux et conversions** dans le tableau de bord du Centre de développement Windows vous permet de voir comment les clients de Windows10 sont arrivés à votre application. Il vous permet de suivre les [campagnes de promotion personnalisées](create-a-custom-app-promotion-campaign.md) pour votre application ou ses modules complémentaires, et de voir combien de ces visites ont généré des nouvelles acquisitions. Vous pouvez afficher ces données dans votre tableau de bord ou [télécharger le rapport](download-analytic-reports.md) et le consulter hors connexion.

> **Important**   Ce rapport n’affiche que les données de conversion et les vues de page des clients sur Windows10.

 

Dans ce rapport, un *canal* fait référence à la méthode par laquelle un client est arrivé à la page de description de votre application (par exemple, navigation et recherche dans le Windows Store, lien depuis un site web externe, lien depuis l’une de vos campagnes personnalisés, etc.). Les canaux suivants sont inclus :

-   **Trafic du magasin :** le client naviguait ou effectuait une recherche dans le Windows Store lorsqu'ils a consulté la description de votre application.
-   **Campagne personnalisée :** le client suivait un lien qui a utilisé un [ID de campagne personnalisée](create-a-custom-app-promotion-campaign.md).
-   **Autres:** le client suivait un lien externe (sans ID de campagne personnalisée) d’un site web vers la description de votre application ou d’un moteur de recherche vers la description de votre application.

Une *vue de page* signifie qu’un client a consulté la description de votre application dans le Windows Store, soit via le Windows Store sur le web, soit depuis l’application du Windows Store sur Windows10.

Une *conversion* signifie qu’un client a obtenu une licence pour votre application (qu’elle soit payante ou gratuite) ou un module complémentaire.

Nous n’affichons pas de taux de conversion dans ce rapport, car nos vues de page et les numéros de conversion ne correspondent pas à des nombres de clients uniques.

Les données de conversion ne sont disponibles que pour vos campagnes personnalisées. Pour les autres types de canal, seules les données sur les vues de page sont incluses dans ce rapport.

> **Remarque**  Les clients peuvent arriver à la description de votre application en cliquant sur une campagne personnalisée qui n'a pas été créée par vos soins. Pour prendre en compte cette possibilité, nous marquons chaque vue de page dans une session avec l’ID de campagne à partir duquel l’utilisateur arrive dans le Windows Store. Nous attribuons ensuite une conversion de votre application à cet ID de campagne si une acquisition de votre application se produit dans les 24heures. Lorsque vous affichez votre rapport, c’est la raison pour laquelle vous risquez de voir des conversions attribuées à des campagnes qui ne vous sont pas familières, de voir un plus grand nombre de conversions totales que celui indiqué dans la répartition des conversions, ou encore d'obtenir des conversions d'applications ou des conversions de module complémentaire qui ne comportent aucune vue de page. Vous pouvez examiner la répartition des conversions par ID de campagne pour afficher uniquement les conversions attribuées à des campagnes que vous avez créées afin d’en évaluer l'efficacité.


## <a name="apply-filters"></a>Appliquer des filtres


Dans la zone supérieure de la page, vous pouvez développer l'option **Appliquer les filtres** pour filtrer toutes les données de cette page par plage de dates et/ou par marché.

-   **Date** : la valeur par défaut de ce filtre est **30 derniers jours**, mais vous pouvez étendre cette période aux **12 derniers mois**.
-   **Marché** : la valeur par défaut de ce filtre est **Tous les marchés**. Vous pouvez choisir un marché spécifique si vous souhaitez n’afficher les détails des clients sur ce marché.
-   **Type d’appareil** : le filtre par défaut est **Tous les appareils**. Vous pouvez choisir un type d’appareil spécifique si vous souhaitez que cette page affiche uniquement les données des clients utilisant celui-ci.

Les informations figurant dans tous les graphiques répertoriés ci-après refléteront la période sélectionnée dans la section **Appliquer des filtres**, ainsi que d’autres filtres que vous avez choisi ici.

## <a name="app-page-views-and-conversions-by-channel"></a>Vues et conversions de la page de l’application par canal


Le graphique **Vues et conversions de la page de l’application par canal** indique la fréquence selon laquelle la page de description de votre application a été consultée et comment les clients y sont arrivés. Il indique également le nombre de conversions des campagnes personnalisées sur la période sélectionnée.

L’onglet **Vues de page** de ce graphique indique le nombre d’affichages de la page de description de votre application sur la période sélectionnée. Les vues sont regroupées selon le type de canal par lequel le client a trouvé la description de votre application.

L’onglet **Conversions** de ce graphique indique le nombre de conversions (nouvelles acquisitions) sur la période sélectionnée pour les clients qui sont arrivés à la description de votre application via une campagne personnalisée.

Pour plus d’informations sur toutes les acquisitions de votre application, y compris celles qui ne sont pas survenues par un lien de campagne personnalisée et celles de clients utilisant d’autres versions du système d’exploitation, consultez le [rapport Acquisitions](acquisitions-report.md).

 

## <a name="total-campaign-conversions"></a>Conversions totale de campagnes


Le graphique **Conversions totales de campagnes** indique le nombre total de conversions d’applications et de modules complémentaires à partir de vos campagnes personnalisées sur la période sélectionnée.

## <a name="app-page-views-and-conversions-by-campaign-id"></a>Vues et conversions de la page de l’application par ID de campagne


Le graphique **Vues et conversions de la page de l’application par ID de campagne** affiche le nombre de vues de page et de conversions pour tous les [ID de campagne](create-a-custom-app-promotion-campaign.md) pendant la période sélectionnée.

##  <a name="add-on-conversions-by-campaign-id"></a>Conversions de modules complémentaires par ID de campagne


Le graphique **Conversions de module complémentaire par ID de campagne** indique le nombre de conversions de module complémentaire par ID de campagne personnalisée.

Lorsqu’une installation d’application est comptabilisée comme une conversion pour une campagne personnalisée, tout achat de module complémentaire dans cette application est également comptabilisé comme une conversion pour la même campagne personnalisée.

Par défaut, ce rapport inclut les modules complémentaires pour lesquels une conversion provient d’un lien utilisant un ID de campagne personnalisée pendant la période sélectionnée. Pour afficher les données d’un module complémentaire spécifique, sélectionnez-le dans **Filtres de section**.

## <a name="conversions-breakdown-by-campaign-id"></a>Répartition des conversions par ID de campagne


Le graphique **Répartition des conversions** affiche les informations suivantes sur les vues de page et les conversions obtenues à partir des campagnes personnalisées.

-   **ID:** affiche les ID de campagne spécifiques.
-   **Vues de page:** affiche le nombre de vues de page marquées avec l’ID de campagne de la première entrée du client dans le Windows Store.
-   **Conversions d’applications:** indique le nombre de conversions d’applications résultant de la campagne personnalisée.
-   **Conversions de modules complémentaires:** indique le nombre de conversions de modules complémentaires résultant de la campagne personnalisée.


 

 
