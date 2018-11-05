---
author: jnHs
Description: The Payout summary shows you details about the money you’ve earned with your apps and add-ons. It also lets you know when you’ll receive payments and how much you'll be paid.
title: Résumé du paiement
ms.assetid: F0D070BE-8267-4CC9-B0D2-085EBA74AC98
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: windows10, uwp, résumé du paiement, instruction, paiements, bénéfices, revenus, paiement
ms.localizationpriority: medium
ms.openlocfilehash: 5f6369247f0e287ec2698213b7f0b7be7e1f21d4
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6030786"
---
# <a name="payout-summary"></a>Résumé du paiement


La page **Résumé du paiement** affiche le détail des sommes rapportées par vos applications et modules complémentaires. Elle vous permet également de connaître les délais et les montants de vos paiements.

Si vous utilisez la publicité pour générer des revenus, les informations de paiement concernant vos revenus publicitaires sont présentées sur la page **Résumé du paiement**. Nous allons montrer l’application dans laquelle ces revenus ont été gagnés ou «non mappés» pour les unités publicitaires utilisées dans plusieurs applications, ou qui ne peuvent pas être mappés à une application spécifique. 

Si vous vendez des produits dans Place de marché MicrosoftAzure, la page **Résumé du paiement** vous présente également des informations sur les paiements qui vous ont été versés. Pour plus d’informations sur le processus de paiement d’Azure Marketplace, voir [Politiques concernant la participation à Microsoft Azure Marketplace](http://go.microsoft.com/fwlink/p/?LinkId=722436) et [Microsoft Azure Marketplace Publisher Agreement (Contrat d’éditeur Microsoft Azure Marketplace)](http://go.microsoft.com/fwlink/p/?LinkID=699560 ). Des informations supplémentaires concernant les rapports sur les revenus liés à Place de marché MicrosoftAzure sont disponibles [ici](http://go.microsoft.com/fwlink/p/?LinkID=722439).

> [!NOTE]
> Pour que vous soyez éligible à la perception de revenus, vos revenus doivent atteindre le [seuil de paiement](payment-thresholds-methods-and-timeframes.md) applicable. Si vos revenus sont inférieurs au seuil fixé, le montant restera dans la catégorie **Réservé** jusqu’à ce que ce seuil soit atteint. Pour plus de détails sur le seuil de paiement applicable aux revenus de l’application, consultez le [Contrat du développeur de l’application](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement). Pour les revenus publicitaires, le seuil de paiement est fixé à 50USD (ou son équivalent dans la devise locale). 
>
> Les paiements sont effectués mensuellement (à condition que le seuil de paiement ait été atteint). Généralement, nous transférons les paiements dus pour un mois donné vers le 15 de ce mois. Notez qu’un délai de 3 à 10jours ouvrés supplémentaires est nécessaire pour l’enregistrement des montants sur votre compte de paiement. Pour plus d’informations, consultez la page [Seuils de paiement, méthodes et délais](payment-thresholds-methods-and-timeframes.md).

Pour afficher le **Résumé du paiement**, cliquez sur l’icône de **paiement** qui s’affiche près du coin supérieur droit dans l’espace partenaires, puis sélectionnez **Résumé du paiement**.

## <a name="current-proceeds-and-payments"></a>Revenus et paiements actuels

La zone **Revenus et paiements actuels**, qui se situe en haut de la page, contient trois sections : **Réservé**, **Paiement à venir** et **Dernier paiement**.

-   La vignette **Réservé** affiche le montant accumulé sur votre compte (y compris les revenus publicitaires), mais dont le versement n’a pas encore été planifié. (Les revenus liés à Place de marché MicrosoftAzure n’apparaissent pas dans la section **Réservé**; si vous prenez uniquement part à Place de marché MicrosoftAzure, un montant nul s’affichera dans cette section.) Les revenus associés à vos dernières ventes d’applications restent en suspens pendant environ 30jours avant de pouvoir vous être versés. Au terme de cette période, le versement de vos revenus est planifié pour le mois suivant (dans la mesure où vous avez atteint le [seuil de paiement](payment-thresholds-methods-and-timeframes.md)). Lors d’une tentative de paiement, votre solde réservé est amputé du montant du paiement. Le montant obtenu s’affiche alors dans **Paiement à venir**. Notez que le montant affiché dans **Réservé** est une estimation, les ventes dans d’autres devises étant soumises aux variations de taux de change avant la création du paiement. Vous pouvez remarquer un léger changement de votre solde réservé au début de chaque mois. Votre solde réservé est mis à jour tous les mois afin de refléter les taux de change mensuels et de représenter ainsi une estimation plus précise. Vous pouvez cliquer sur **Afficher les détails** pour consulter des informations supplémentaires ou cliquer sur le lien **Télécharger vos transactions réservées** pour afficher un fichier .csv de l'ensemble de vos transactions figurant dans la section **Réservé**.
-   La section **Paiement à venir** indique le nombre de paiements à venir, le montant de vos prochains paiements et les dates de création des paiements. Si vos revenus éligibles n’ont pas encore atteint le [seuil de paiement](payment-thresholds-methods-and-timeframes.md), aucun paiement à venir n’apparaît dans cette section. Sélectionnez **Afficher les détails** pour consulter des informations supplémentaires, notamment le montant des paiements et leurs sources de revenu respectives. Lorsqu’un montant est indiqué dans la section **Paiement à venir**, un lien temporaire **Télécharger les transactions** s’affiche.  En cliquant sur ce lien, vous pouvez afficher un fichier .csv regroupant toutes les transactions qui constituent vos paiements à venir.  Remarque: lorsque le montant des **Paiements à venir** passe dans la section **Dernier paiement**, le lien **Télécharger les transactions** n’apparaît plus.
-   La section **Dernier paiement** indique le montant de la dernière tentative de paiement. Si le paiement a été effectué avec succès, le lien **Afficher les détails** est bleu, et vous pouvez cliquer sur ce lien pour visualiser les détails de chaque paiement. Notez que si nous avons tenté d’effectuer plusieurs paiements et que seul l’un d’eux a réussi, seul le montant du paiement effectué avec succès s’affiche ici. Si un ou plusieurs paiements ont échoué, le lien **Afficher les détails** est rouge et affiche le nombre de paiements en échec. Vous pouvez cliquer sur **Afficher les détails** pour visualiser plus d’informations sur le problème et ainsi remédier à la situation.

## <a name="proceeds-by-app-and-adjustments"></a>Revenus par application et ajustements


Cette section subdivise les informations du résumé afin que vous puissiez voir les spécificités de chaque application. Si vous avez généré des revenus par le biais de publicités, le montant total de vos revenus publicitaires s’affiche ici sur une seule ligne.

Cette section vous permet d’identifier les applications ayant généré le plus de revenus dans les catégories **Réservé** ou **Dernier paiement**. Vous pouvez également afficher le montant total que vous avez perçu pour chaque application. Si le solde de votre compte a subi des [modifications](#proceeds-by-app-and-adjustments), ces derniers figurent également dans cette section. (Notez que les modifications apportées aux revenus publicitaires n’y sont pas affichées pour le moment).

## <a name="payment-statements"></a>Relevés de paiement


Dans cette section, vous pouvez afficher les relevés de l’ensemble des paiements mensuels effectués ainsi que le montant total qui vous a été versé.

La section **Montant cumulé des paiements** indique le montant total perçu pour l’ensemble de vos ventes. Cliquez sur **Afficher les détails** pour visualiser les montants provenant de chaque source de revenus.

Au-dessous de la section **Montant cumulé des paiements**, vous pouvez visualiser vos trois derniers relevés par défaut. Pour afficher le relevé complet (dans le cas de paiements réussis), cliquez sur **Afficher**. La zone de liste déroulante vous permet d’accéder à vos relevés de paiement historiques.

En haut de chaque relevé figure le montant total de votre paiement mensuel. La section **Paiements émis**, située juste en dessous, résume le mode de calcul utilisé pour déterminer le montant du paiement.

Au-dessous, dans la section **Répartition des revenus**, vous pouvez voir en détail le montant des revenus générés par marché, par source de revenus (par exemple, Microsoft Store, Windows Store8, Windows Phone Store, etc.) et par application. Vous bénéficiez également d’informations sur les [ajustements](#proceeds-by-app-and-adjustments) effectués, comme la date, le montant et le motif de ces modifications.

Notez que les sections mentionnées ci-dessus affichent uniquement des informations sur votre revenu (et sur les ajustements) compte tenu des ventes recensées pour votre application. Si vous avez généré des revenus grâce à la publicité, une autre section Microsoft Advertising regroupera les détails relatifs aux paiements et aux conversions de devises.

## <a name="adjustments"></a>Ajustements


| Catégorie d’ajustement     | Description                                                                                                |
|-------------------------|------------------------------------------------------------------------------------------------------------|
| Ajustement compensatoire | Ajustement appliqué à votre solde de revenu n’entrant dans aucune des autres catégories d’ajustement répertoriées |
| Solde historique        | Soldes de revenu dérivés d’un ancien système de paiement                                                             |
| Transfert de taxe              | Ajustement fiscal sur les ventes réalisées en Corée                                                                   |

 

## <a name="downloading-payment-transactions"></a>Téléchargement des transactions de paiement


En haut de chaque relevé figure un lien **Télécharger les transactions**. Cliquez sur ce lien pour créer un fichier .csv comprenant des informations détaillées sur chacune des transactions de votre paiement.

Le tableau ci-après décrit les champs qui figurent dans le fichier .csv. Notez que les champs que vous voyez peuvent différer, car nous mettons régulièrement à jour notre reporting.

| Nom du champ              | Description                                                                                                                              |
|-------------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| Source de revenu          | Votre source de revenu, en fonction de la transaction (par exemple, Microsoft Store, Windows Phone Store, Windows Store8, publicités, etc.) |
| ID de commande                |  Identificateur de commande unique. Cet ID permet d’identifier les transactions d’achat ainsi que les opérations sans achat (par exemple: remboursements, rétrofacturations, etc.). Les deux auront le même ID de commande. En outre, en cas de paiement fractionné, où plusieurs modes de paiement sont utilisés pour un achat unique, l’ID de commande vous permettra de lier les transactions d’achat.                                                                                                          |
| ID de la transaction          |       Identifiant unique de la transaction.  |
| Date et heure de la transaction   | Date et heure d’exécution de la transaction (UTC).                                                                                        |
| ID de produit parent       | Identificateur unique du produit parent. Notez également que si la transaction ne présente aucun produit parent, l'ID du produit parent est l'ID du produit. |
| ID de produit              | Identifiant unique du produit.                                                                                                               |
| Nom du produit parent     | Nom du produit parent. Notez également que si la transaction ne présente aucun produit parent, le nom du produit parent est identique au nom du produit.   |
| Nom du produit            | Désignation du produit.                                                                                                                     |
| Type de produit            | Type du produit (par exemple, Application, Module complémentaire, Jeu, etc.)                                                                                        |
| Quantité                | Lorsque la Source de revenu est Microsoft Store pour Entreprises, la Quantité correspond au nombre de licences achetées. Pour toutes les autres Sources de revenu, la Quantité sera toujours 1. Remarque: même si une transaction unique est scindée en deux articles en raison du recours à deux méthodes de paiement différentes, chaque article affiche une Quantité égale à 1.    |
| Type de transaction        | Type de la transaction (par exemple, achat, remboursement, contrepassation, rétrofacturation, etc.)                                                                |
| Moyen de paiement          | Instrument de paiement client utilisé pour la transaction (par exemple, carte, facturation de l’opérateur mobile, PayPal, etc.)                                 |
| Pays / région        | Pays/région d’exécution de la transaction.                                                                                            |
| Fournisseur / vendeur local | Fournisseur/vendeur local de l’enregistrement.                                                                                                          |
| Devise de la transaction    | Devise utilisée pour la transaction.                                                                                                              |
| Montant de transaction      | Montant de la transaction.                                                                                                                |
| Taxes versées            | Montant des taxes versées (taxe de vente, taxe d’utilisation ou TVA/taxe sur les biens et services).                                                                                    |
| Profits nets            | Montant de la transaction moins les taxes versées.                                                                                                     |
| Frais du Windows Store               | Pourcentage des recettes nettes retenues par Microsoft à titre de frais de mise à disposition de l’application ou du module complémentaire dans le Windows Store.                        |
| Revenu de l’application            | Recettes nettes moins les frais du Windows Store.                                                                                                         |
| Impôts retenus          | Montant de l’impôt sur le revenu retenu. (Non inclus dans le fichier .csv **Réservé**)                                                                  |
| Paiement                 | Revenu de l’application moins toute retenue d’impôt sur le revenu applicable (montant indiqué dans le champ «Devise de la transaction»). (Non inclus dans le fichier .csv **Réservé**) |
| Taux de change                 | Taux de change utilisé pour convertir la devise de la transaction en devise du paiement.                                                           |
| Devise de paiement        | Devise dans laquelle votre paiement a été effectué.                                                                                                         |
| Paiement converti       | Montant du paiement converti en devise du paiement à l’aide du taux de change.                                                                           |
| Modèle de remise des taxes         | Tiers responsable du versement des taxes (taxe de vente, taxe d’utilisation ou TVA/taxe sur les biens et services).                                                                     |
| Date et heure d’admissibilité   | Date et heure auxquelles vos revenus de transaction peuvent vous être versés (UTC). Lorsqu’un paiement est créé, il inclut les revenus de transactions dont la date et l’heure d’admissibilité sont antérieures à la date de création du paiement. (Inclus uniquement dans le fichier .csv **Réservé**)                                                                     |
| Frais                 | Ventilation détaillée de tous les frais agrégés dans la colonne Montant de transaction. (Uniquement pour Microsoft Azure Marketplace; non inclus dans le fichier .csv **Réservé**)          |

 

 

 




