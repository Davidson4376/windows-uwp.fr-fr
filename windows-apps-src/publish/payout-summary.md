---
Description: La page Résumé du paiement affiche le détail des sommes rapportées par vos applications et modules complémentaires. Elle vous permet également de connaître les délais et les montants de vos paiements.
title: Résumé du paiement
ms.assetid: F0D070BE-8267-4CC9-B0D2-085EBA74AC98
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, résumé du paiement, instruction, paiements, bénéfices, revenus, paiement
ms.localizationpriority: medium
ms.openlocfilehash: 90238360ecc48beb974546dc5b49ac09c01407eb
ms.sourcegitcommit: 35a511c2b29ae3d5008612a5fc13d3eb6370d2d0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67495734"
---
# <a name="payout-summary"></a>Résumé du paiement

Le **synthèse des paiements** vous donne des détails sur l’argent que vous avez obtenu auprès de Microsoft. Elle vous permet également de connaître les délais et les montants de vos paiements.

Si vous vendez des produits dans Place de marché Microsoft Azure, la page **Résumé du paiement** vous présente également des informations sur les paiements qui vous ont été versés. Pour plus d’informations sur le processus de paiement d’Azure Marketplace, voir [Politiques concernant la participation à Microsoft Azure Marketplace](https://go.microsoft.com/fwlink/p/?LinkId=722436) et [Microsoft Azure Marketplace Publisher Agreement (Contrat d’éditeur Microsoft Azure Marketplace)](https://go.microsoft.com/fwlink/p/?LinkID=699560 ).

> [!NOTE]
> Pour être éligible pour le paiement, votre produit doit atteindre la [seuil de paiement](payment-thresholds-methods-and-timeframes.md) 50 $. Pour plus d’informations sur le seuil de paiement, consultez cette page et passez en revue le contrat du développeur de l’application.

## <a name="access-the-payout-summary-pages"></a>Accéder aux pages de résumé de paiement

Pour ouvrir une des pages de résumé de paiement :

1. Sélectionnez l’icône de l’argent dans le coin supérieur droit.
2. Sélectionnez les paiements, historique des transactions, ou exporter des données.

## <a name="payments-page"></a>Page de paiements

Les totaux sur cette page représentent tous les programmes que vous participez. Vous pouvez filtrer par ID de Participant, programme, ID de paiement et Earning type. Reçoivent des montants en dollars américains. La valeur payante s’affiche également dans pay pour devise.

| Domaine                   | Description                                                                                  |
|------------------------|----------------------------------------------------------------------------------------------|
| Total payé cette année   | Le total combiné payé pour vous cette année, en dollars américains, pour tous vos programmes.       |
| Prochain paiement estimé | Le seul paiement suivant chez vous (même s’il existe d’autres sera bientôt disponible), en dollars américains. |
| Dernier paiement           | Le montant (en dollars américains), le nom du programme et le programme de votre paiement le plus récent.           |
| Paiements par source     | Montant des paiements, en dollars américains, représenté par programme au cours des 12 derniers mois.           |
| Paiements               | Sélectionnez payant ou en attente et de tri puis que vous le souhaitez. Pour plus d’informations d’un paiement spécifique, sélectionnez Affichage. Pour télécharger une copie de l’instruction de remise de paiement, sélectionnez le téléchargement. Notez que les données d’historique de transaction peuvent prendre jusqu'à 24 heures pour apparaître, donc vous ne voyiez pas les bénéfices associés tout de suite. |

Pour exporter les données sur cette page, sélectionnez Exporter, puis suivez les instructions sur la page de données d’exportation.

## <a name="transaction-history-page"></a>Page Historique de transactions

Cette page affiche toutes vos revenus individuelles, y compris la date, type et l’obtention pour chacune. Vous pouvez sélectionner une période à afficher, et vous pouvez également filtrer par ID d’inscription, programme, ID de paiement, type Earning, levier et état. Données sont disponibles pour l’année fiscale en cours (1er juillet-30 juin) et les deux exercices fiscaux précédents.

Pour plus d’informations sur : un bénéfice, sélectionnez la flèche bas à droite de la page. Ceci affichera le levier, le montant du chiffre d’affaires et le produit. Si pour une raison quelconque, un de ces données ne sont pas disponibles, mais vous devez avoir accès à ce dernier, contactez [prennent en charge](https://developer.microsoft.com/en-us/windows/support)]. Si le gagnant est le résultat d’un ajustement et pas une transaction, les champs de produit ne seront pas affichés.

Pour exporter les données de transaction sur cette page, sélectionnez Exporter et puis suivez les instructions sur la page de données d’exportation. Fichiers exportés à partir de la page de l’historique des transactions affichent les données dans la devise de transaction, dans la devise de la transaction et le dollar américain, les bénéfices et la valeur payante dans payer une devise.

## <a name="payment-status"></a>Statut du paiement

| L’obtention d’état           | Reason                                                                                                                                      | Action de partenaire requise ?                                   |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| Non traité              | Le gagnant est éligible pour le paiement. Il reste dans cet état pendant une période de refroidissement tel que défini dans le guide de programme pour le programme d’encouragement. | Non                                                         |
| À venir                 | Ordre de paiement générée en attente de révisions internes nécessaires avant que le paiement est traité.                                                               | Non                                                         |
| Relevé de taxes en attente      | Votre facture de taxe est incomplète ou non valide.                                                                                                  | Vous devez mettre à jour votre relevé de taxes avant que vous pouvez être payé |
| Rejetés au cours de révision   | Le paiement a été rejeté pendant l’examen.                                                                                                     | Contact [prise en charge Microsoft](https://developer.microsoft.com/en-us/windows/support) pour plus d’informations                      |
| Failed                   | Le paiement a échoué en raison d’une erreur de système de Microsoft.                                                                                         | Contact [prise en charge Microsoft](https://developer.microsoft.com/en-us/windows/support) pour plus d’informations                      |
| En cours              | Le paiement est en cours.                                                                                                                 | Non                                                         |
| Paiement incorrect        | L’imputation de paiement est en cours.                                                                                                       | Non                                                         |
| envoyé                     | Le paiement a été envoyé à votre banque.                                                                                                     | Non                                                         |
| Retraitement             | Le paiement a rencontré une erreur de système de Microsoft et est en cours retraité.                                                                  | Non                                                         |
| Inversé                 | Le paiement a été annulé par votre banque et sera envoyé à nouveau lors du prochain cycle de paiement.                                                     | Non                                                         |
| Facture de taxe rejeté     | Votre relevé de taxes a été rejetée pendant l’examen. Tous les paiements en attente seront bloquées jusqu'à ce que la révision de facture de taxe est terminée.                 | Contact [prise en charge Microsoft](https://developer.microsoft.com/en-us/windows/support) pour plus d’informations                      |
| Facture de taxe en cours de validation | Votre facture de taxe est en cours de révision. Votre paiement sera disponible une fois que la facture a été approuvée.                                   | Non                                                         |
| Rejeté                 | Le paiement a été rejeté par votre banque.                                                                                                      | Pour plus d’informations, contactez votre banque.                             |

## <a name="export-data-page"></a>Page de données d’exportation

Suivez les instructions de cette page pour exporter les données souhaitées.

Remarques :

- Lorsque vous accédez à cette page à partir de deux paiements ou la page Historique de transactions, vos filtres ne mènent pas. Vous devez à nouveau la configuration de la page de données d’exportation.
- La page de données d’exportation n’actualise pas sur son propre. Vous devrez peut-être actualiser la page manuellement pour afficher des données les plus récentes.
- Votre filtre peut entraîner une erreur des données non disponible. Cela signifie probablement vous avez laissé la valeur par défaut période sélectionnée à trois mois et ensuite sélectionné un ID paiement gagne qui se trouve en dehors de cette période. Développez votre période de temps et réessayez.

## <a name="payment-download-export"></a>Exportation de téléchargement de paiement

Cette option fournit un téléchargement de paiements vous avez reçu dans votre banque pour un programme donné, la taxe associée et agrégées recevant quantité. Ce rapport est utilisé pour de nombreux programmes partenaires, certaines colonnes peuvent donc être s’appliquent pas à votre rapport. Ces colonnes sont indiqués ci-après.

| Nom de la colonne              | Description                                                                                                                             |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| participantID            | L’identité principale du partenaire obtention dans le cadre du programme                                                                           |
| participantIDType        | Généralement id de programme pour les programmes de motivation et l’ID de vendeur pour les programmes de Store                                                              |
| participantName          | Nom du partenaire recevant                                                                                                             |
| programName              | Nom du programme offre incitative/store                                                                                                            |
| des coûts                   | Montant des coûts dans la devise de payer à pour ce programme/participantID                                                                     |
| earnedUSD                | Montant des coûts pour l’ID/le participant au programme, en dollars américains                                                                                    |
| withheldTax              | Montant des taxes prélevées dans la devise de payer à pour le programme/participantID                                                             |
| salesTax                 | Quantité totale de taxe dans la devise de payer à pour le programme/participantID                                                          |
| totalPayment             | Total des paiements dans la devise locale à l’exclusion de la retenue et y compris les taxes sur les ventes (le cas échéant) pour le programme/participantID |
| currencyCode             | Payer pour le code de devise                                                                                                                    |
| PaymentMethod            | La méthode utilisée pour payer le partenaire (virement bancaire électronique, note de crédit)                                                              |
| paymentID                | Identificateur unique pour le paiement. Ce nombre est généralement visible dans votre relevé de compte.                                               |
| Statut de paiement            | Statut du paiement                                                                                                                          |
| paymentStatusDescription | Description conviviale du statut de paiement                                                                                                  |
| Paiement              | Date paiement a été envoyé à partir de Microsoft                                                                                                    |

## <a name="transaction-history-download-export"></a>Exportation de téléchargement de l’historique des transactions

Cette option fournit un téléchargement de chaque élément de ligne recevant, que vous voyez dans la page de l’historique des transactions, l’obtention de type, date, montant de la transaction associée, customer, product et autres détails transactionnelles applicables à vos programmes.

| Nom de la colonne                    | Description                                                                                                                              |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| earningId                      | Identificateur unique pour chaque gagnant                                                                                                       |
| participantId                  | L’identité principale du partenaire obtention dans le cadre du programme                                                                            |
| participantIdType              | ID de vendeur                                                                                                                                |
| participantName                | Nom du partenaire recevant                                                                                                              |
| partnerCountryCode             | Emplacement/pays du partenaire recevant                                                                                                  |
| programName                    | Nom du programme offre incitative/store                                                                                                             |
| transactionId                  | Identificateur unique pour la transaction                                                                                                    |
| transactionCurrency            | Devise dans laquelle la transaction du client d’origine s’est produite.                                                                             |
| transactionDate                | Date de la transaction. Utile pour les programmes où nombre de transactions contribue à l’obtention d’un                                           |
| transactionExchangeRate        | Date de taux de change permet d’afficher le montant USD correspondant                                                                             |
| transactionAmount              | Montant de la transaction dans la devise de transaction d’origine selon les obtenant est généré                                              |
| transactionAmountUSD           | Montant de la transaction en dollars américains                                                                                                                |
| Levier                          | Indique la règle d’entreprise pour l’obtention                                                                                                  |
| earningRate                    | Encouragement taux appliqué sur le montant de la transaction pour générer une obtention                                                                      |
| quantity                       | Varie en fonction du programme. Indique la quantité à payer pour les programmes transactionnels                                                            |
| earningType                    | Indique si elle est frais, remise, coop, etc. de vente.                                                                                          |
| earningAmount                  | L’obtention de la quantité dans la devise de transaction d’origine                                                                                      |
| earningAmountUSD               | L’obtention de montant en dollars américains                                                                                                                    |
| earningDate                    | Date de l’obtention                                                                                                                      |
| calculationDate                | Date de que l’obtention a été calculée dans le système                                                                                            |
| earningExchangeRate            | Taux de change permet d’afficher le montant USD correspondant                                                                                  |
| exchangeRateDate               | Date de taux de change utilisé pour calculer EarningAmount USD                                                                                   |
| claimId                        | Sera toujours vide                                                                                                                     |
| paymentId                      | Identificateur unique pour le paiement. Ce nombre est généralement visible dans votre relevé de compte                                                 |
| Statut de paiement                  | Statut du paiement                                                                                                                           |
| paymentStatusDescription       | Description conviviale du statut de paiement                                                                                                   |
| customerId                     | Sera toujours vide                                                                                                                     |
| customerName                   | Sera toujours vide                                                                                                                     |
| Numéro de référence                     | Sera toujours vide                                                                                                                     |
| productName                    | Nom de produit lié à la transaction                                                                                                       |
| productId                      | Identificateur de produit unique                                                                                                                |
| parentProductId                | Identificateur unique du produit parent. Notez également que si la transaction ne présente aucun produit parent, l'ID du produit parent est l'ID du produit. |
| parentProductName              | Nom du produit parent. Notez également que si la transaction ne présente aucun produit parent, le nom du produit parent est identique au nom du produit.   |
| productType                    | Type du produit (par exemple, Application, Module complémentaire, Jeu, etc.)                                                                                        |
| invoiceNumber                  | Sera toujours vide                                                                                                                     |
| subscriptionId                 | Sera toujours vide                                                                                                                     |
| subscriptionStartDate          | Sera toujours vide                                                                                                                     |
| subscriptionEndDate            | Sera toujours vide                                                                                                                     |
| resellerId                     | Sera toujours vide                                                                                                                     |
| resellerName                   | Sera toujours vide                                                                                                                     |
| distributorId                  | Sera toujours vide                                                                                                                     |
| distributorName                | Sera toujours vide                                                                                                                     |
| agreementNumber                | Sera toujours vide                                                                                                                     |
| agreementStartDate             | Sera toujours vide                                                                                                                     |
| agreementEndDate               | Sera toujours vide                                                                                                                     |
| Charge de travail                       | Sera toujours vide                                                                                                                     |
| transactionType                | Type de la transaction (par exemple, achat, remboursement, contrepassation, rétrofacturation, etc.)                                                               |
| localProviderSeller            | Fournisseur local/vendeur d’enregistrement                                                                                                          |
| taxRemitted                    | Montant des taxes versées (taxe de vente, taxe d’utilisation ou TVA/taxe sur les biens et services).                                                                                   |
| taxRemitModel                  | Tiers responsable du versement des taxes (taxe de vente, taxe d’utilisation ou TVA/taxe sur les biens et services).                                                                    |
| storeFee                       | La quantité est conservée par Microsoft sous la forme d’un tarif pour la mise à disposition l’application ou le module complémentaire dans le Store.                                           |
| transactionPaymentMethod       | Instrument de paiement client utilisé pour la transaction (par exemple, carte, facturation de l’opérateur mobile, PayPal, etc.)                                |
| tpan                           | Indique le réseau par des tiers ad                                                                                                     |
| purchaseTypeCode               | Sera toujours vide                                                                                                                     |
| purchaseOrderType              | Sera toujours vide                                                                                                                     |
| purchaseOrderCoverageStartDate | Sera toujours vide                                                                                                                     |
| purchaseOrderCoverageEndDate   | Sera toujours vide                                                                                                                     |
| externalReferenceId            | Sera toujours vide                                                                                                                     |
| externalReferenceIdLabel       | Sera toujours vide                                                                                                                     |

## <a name="payout-statement-download-export-legacy"></a>Exportation de téléchargement instruction de paiement (hérité)

Pendant une durée limitée dans l’ancienne page de résumé de paiement, les instructions de paiement sera disponibles au téléchargement. Ce rapport contient les champs suivants.

> [!NOTE]
> Historique des transactions hérité possède une colonne appelée « Réservé » correspond à la colonne « Bénéfices » dans l’historique moderne, à ceci près qu’elle exclut tous les gains avec l’état = « Paiement envoyé ».

| Nom du champ              | Description                                                                                                                                                             |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Source de revenu          | Votre source de revenu, en fonction de la transaction (par exemple, Microsoft Store, Windows Phone Store, Windows Store 8, publicités, etc.)                  |
| ID de commande                | Identificateur de commande unique. Cet ID permet d’identifier les transactions d’achat ainsi que les opérations sans achat (par exemple : remboursements, rétrofacturations, etc.). Les deux auront le même ID de commande. En outre, en cas de paiement fractionné, où plusieurs modes de paiement sont utilisés pour un achat unique, l’ID de commande vous permettra de lier les transactions d’achat. |
| ID de la transaction          | Identifiant unique de la transaction.                                                                                                                                          |
| Date et heure de la transaction   | Date et heure d’exécution de la transaction (UTC).                                                                                                                       |
| ID de produit parent       | Identificateur unique du produit parent. Notez également que si la transaction ne présente aucun produit parent, l'ID du produit parent est l'ID du produit.                                |
| ID de produit              | Identifiant unique du produit.                                                                                                                                              |
| Nom du produit parent     | Nom du produit parent. Notez également que si la transaction ne présente aucun produit parent, le nom du produit parent est identique au nom du produit.                                  |
| Nom du produit            | Désignation du produit.                                                                                                                                                    |
| Type de produit            | Type du produit (par exemple, Application, Module complémentaire, Jeu, etc.)                                                                                                                       |
| Quantité                | Lorsque la Source de revenu est Microsoft Store pour Entreprises, la Quantité correspond au nombre de licences achetées. Pour toutes les autres Sources de revenu, la Quantité sera toujours 1. Remarque : même si une transaction unique est scindée en deux articles en raison du recours à deux méthodes de paiement différentes, chaque article affiche une Quantité égale à 1. |
| Type de transaction        | Type de la transaction (par exemple, achat, remboursement, contrepassation, rétrofacturation, etc.)                                                                                              |
| Moyen de paiement          | Instrument de paiement client utilisé pour la transaction (par exemple, carte, facturation de l’opérateur mobile, PayPal, etc.)                                                               |
| Pays / région        | Pays/région d’exécution de la transaction.                                                                                                                          |
| Fournisseur / vendeur local | Fournisseur/vendeur local de l’enregistrement.                                                                                                                                        |
| Devise de la transaction    | Devise utilisée pour la transaction.                                                                                                                                            |
| Montant de transaction      | Montant de la transaction.                                                                                                                                              |
| Taxes versées            | Montant des taxes versées (taxe de vente, taxe d’utilisation ou TVA/taxe sur les biens et services).                                                                                                                  |
| Profits nets            | Montant de la transaction moins les taxes versées.                                                                                                                                   |
| Frais du Windows Store               | Pourcentage des recettes nettes retenues par Microsoft à titre de frais de mise à disposition de l’application ou du module complémentaire dans le Windows Store.                                                      |
| Revenu de l’application            | Recettes nettes moins les frais du Windows Store.                                                                                                                                       |
| Impôts retenus          | Montant de l’impôt sur le revenu retenu. (Non inclus dans le fichier .csv **Réservé**)                                                                                                |
| Paiement                 | Revenu de l’application moins toute retenue d’impôt sur le revenu applicable (montant indiqué dans le champ « Devise de la transaction »). (Non inclus dans le fichier .csv **Réservé**)                               |
| Taux de change                 | Taux de change utilisé pour convertir la devise de la transaction en devise du paiement.                                                                                         |
| Devise de paiement        | Devise dans laquelle votre paiement a été effectué.                                                                                                                                       |
| Paiement converti       | Montant du paiement converti en devise du paiement à l’aide du taux de change.                                                                                                         |
| Modèle de remise des taxes         | Tiers responsable du versement des taxes (taxe de vente, taxe d’utilisation ou TVA/taxe sur les biens et services).                                                                                                   |
| Date et heure d’admissibilité   | Date et heure auxquelles vos revenus de transaction peuvent vous être versés (UTC). Lorsqu’un paiement est créé, il inclut les revenus de transactions dont la date et l’heure d’admissibilité sont antérieures à la date de création du paiement. (Inclus uniquement dans le fichier .csv **Réservé**) |
| Frais                 | Ventilation détaillée de tous les frais agrégés dans la colonne Montant de transaction. (Uniquement pour Microsoft Azure Marketplace ; non inclus dans le fichier .csv **Réservé**) |
