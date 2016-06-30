---
author: jnHs
Description: "Le rapport Canaux et conversions dans le tableau de bord du Centre de développement Windows vous permet de voir comment les clients de Windows 10 sont arrivés à votre application."
title: Rapport sur les canaux et conversions
ms.assetid: C359B9FB-A17B-4A8E-B8EE-19F2F98AA4FF
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: e82299f156a7e4f82e9219dc9b0ef6171e72c74e

---

# Rapport sur les canaux et conversions


Le rapport **Canaux et conversions** dans le tableau de bord du Centre de développement Windows vous permet de voir comment les clients de Windows 10 sont arrivés à votre application. Il vous permet de suivre les [campagnes de promotion personnalisées](create-a-custom-app-promotion-campaign.md) pour votre application ou ses produits in-app, et de voir combien de ces visites ont généré des nouvelles acquisitions. Vous pouvez afficher ces données dans votre tableau de bord ou [télécharger le rapport](download-analytic-reports.md) et le consulter hors connexion.

> **Important** Ce rapport n’affiche que les données de conversion et les vues de page des clients sur Windows 10.

 

Dans ce rapport, un *canal* fait référence à la méthode par laquelle un client est arrivé à la page de description de votre application (par exemple, navigation et recherche dans le Windows Store, lien depuis un site web externe, lien depuis l’une de vos campagnes personnalisés, etc.). Les canaux suivants sont inclus :

-   **Trafic du magasin :** le client naviguait ou effectuait une recherche dans le Windows Store lorsqu'ils a consulté la description de votre application.
-   **Site web externe :** le client suivait un lien (sans ID de campagne personnalisée) vers la description de votre application à partir d'un site web.
-   **Moteur de recherche :** le client suivait un lien vers la description de votre application qui a été renvoyée par un moteur de recherche en ligne.
-   **Campagne personnalisée :** le client suivait un lien qui a utilisé un [ID de campagne personnalisée](create-a-custom-app-promotion-campaign.md).

Une *vue de page* signifie qu'un client a consulté la description de votre application dans le Store, soit via le Store sur le web, soit depuis l’application du Store sur Windows 10.

Une *conversion* signifie qu’un client a obtenu une licence pour votre application (qu’elle soit payante ou gratuite) ou un produit in-app.

> **Important** Les données de conversion ne sont disponibles que pour vos campagnes personnalisées. Pour les autres types de canal, seules les données sur les vues de page sont incluses dans ce rapport.

 

## Appliquer les filtres


Dans la zone supérieure de la page, vous pouvez développer l'option **Appliquer les filtres** pour filtrer toutes les données de cette page par plage de dates et/ou par marché.

-   **Date** : la valeur par défaut de ce filtre est **30 derniers jours**, mais vous pouvez étendre cette période aux **12 derniers mois**.
-   **Marché** : la valeur par défaut de ce filtre est **Tous les marchés**. Vous pouvez choisir un marché spécifique si vous souhaitez n’afficher les détails des clients sur ce marché.
-   **Type d’appareil** : le filtre par défaut est **Tous les appareils**. Vous pouvez choisir un type d’appareil spécifique si vous souhaitez que cette page affiche uniquement les données des clients utilisant celui-ci.

Les informations figurant dans tous les graphiques répertoriés ci-après refléteront la période sélectionnée dans la section **Appliquer des filtres**, ainsi que d’autres filtres que vous avez choisi ici.

## Vues et conversions de la page de l’application par canal


Le graphique **Vues et conversions de la page de l’application par canal** indique la fréquence selon laquelle la page de description de votre application a été consultée et comment les clients y sont arrivés. Il indique également le nombre de conversions des campagnes personnalisées sur la période sélectionnée.

L’onglet **Vues de page** de ce graphique indique le nombre d’affichages de la page de description de votre application sur la période sélectionnée. Les vues sont regroupées selon le type de canal par lequel le client a trouvé la description de votre application.

L’onglet **Conversions** de ce graphique indique le nombre de conversions (nouvelles acquisitions) sur la période sélectionnée pour les clients qui sont arrivés à la description de votre application via une campagne personnalisée.

> **Important** Pour plus d’informations sur toutes les acquisitions de votre application, y compris celles qui ne sont pas survenues par un lien de campagne personnalisée et celles de clients utilisant d’autres versions du système d’exploitation, consultez le [rapport Acquisitions](acquisitions-report.md).

 

## Performances de campagne par canal


Le graphique **Performances de campagne par canal** affiche le nombre de vues de page pour chaque type de canal. Il indique également le nombre de conversions d’application et de produit in-app depuis vos campagnes personnalisées sur la période sélectionnée.

## Vues et conversions de la page de l’application par ID de campagne


Le graphique **Vues et conversions de la page de l’application par ID de campagne** affiche le nombre de vues de page et les conversions pour chacun de vos [ID de campagne](create-a-custom-app-promotion-campaign.md) pendant la période sélectionnée.

##  Conversions de produit in-app par ID de campagne


Le graphique **Conversions de produit in-app par ID de campagne** indique le nombre de conversions de produit in-app par ID de campagne personnalisée.

Lorsqu’une installation d’application est comptabilisée comme une conversion pour une campagne personnalisée, tout achat de produit in-app dans cette application est également comptabilisé comme une conversion pour la même campagne personnalisée.

Par défaut, ce rapport inclut les produits in-app pour lesquels une conversion provient d’un lien utilisant un ID de campagne personnalisée pendant la période sélectionnée. Pour afficher les données d'un produit in-app spécifique, sélectionnez-le dans **Filtres de section**.

## Répartition des conversions


Le graphique **Répartition des conversions** affiche des détails supplémentaires sur les vues de page résultant de chacun de vos types de canal. Cliquez sur chaque type de canal pour obtenir plus d'informations sur les conversions à partir de ce canal :

-   **Campagne personnalisée :** affiche les ID de campagne spécifiques.
-   **Site web externe :** affiche le domaine du site web lié à l'application.
-   **Trafic du magasin :** indique si le client utilisait l’application cliente du magasin ou le magasin en ligne.
-   **Moteur de recherche :** affiche les termes de recherche spécifiques utilisés par le client.

Pour les campagnes personnalisées, vous pouvez également voir le nombre de conversions d’application et de conversions de produit in-app résultant de chaque ID de campagne spécifique.

 

 







<!--HONumber=Jun16_HO4-->


