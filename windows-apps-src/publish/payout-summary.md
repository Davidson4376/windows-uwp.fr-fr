---
Description: La page Résumé du paiement affiche le détail des sommes rapportées par vos applications et modules complémentaires. Elle vous permet également de connaître les délais et les montants de vos paiements.
title: Résumé du paiement
ms.assetid: F0D070BE-8267-4CC9-B0D2-085EBA74AC98
ms.date: 08/02/2019
ms.topic: article
keywords: windows 10, uwp, résumé du paiement, instruction, paiements, bénéfices, revenus, paiement
ms.localizationpriority: medium
ms.openlocfilehash: 777ee4201b435f17cdc4fc3650a2d33645ff56b9
ms.sourcegitcommit: 769ec7811aaaa79fe521e3e984a2e1a2a9671caf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70057823"
---
# <a name="payout-summary"></a>Résumé du paiement

Le **Résumé du paiement** vous donne des détails sur l’argent que vous avez obtenu auprès de Microsoft. Elle vous permet également de connaître les délais et les montants de vos paiements.

Si vous vendez des produits dans Place de marché Microsoft Azure, la page **Résumé du paiement** vous présente également des informations sur les paiements qui vous ont été versés. Pour plus d’informations sur le processus de paiement d’Azure Marketplace, voir [Politiques concernant la participation à Microsoft Azure Marketplace](https://go.microsoft.com/fwlink/p/?LinkId=722436) et [Microsoft Azure Marketplace Publisher Agreement (Contrat d’éditeur Microsoft Azure Marketplace)](https://go.microsoft.com/fwlink/p/?LinkID=699560 ).

> [!NOTE]
> Pour être éligible au paiement, votre produit doit atteindre le [seuil de paiement](payment-thresholds-methods-and-timeframes.md) de $50. Pour plus d’informations sur le seuil de paiement, consultez cette page et passez en revue le contrat de développeur d’applications.

## <a name="access-the-payout-summary-pages"></a>Accéder aux pages de résumé de paiement

Pour ouvrir l’une des pages de résumé de paiement:

1. Sélectionnez l’icône Money dans le coin supérieur droit.
2. Sélectionnez paiements, historique des transactions ou exporter des données.

## <a name="payments-page"></a>Page paiements

Les totaux de cette page représentent tous les programmes auxquels vous participez. Vous pouvez filtrer par ID participant, programme, ID paiement et type acquis. Les montants sont indiqués en dollars américains. La valeur payante est également affichée dans payer à la devise.

| Domaine                   | Description                                                                                  |
|------------------------|----------------------------------------------------------------------------------------------|
| Total payé cette année   | Le total cumulé payé pour vous cette année, en dollars américains, pour tous vos programmes.       |
| Prochain paiement estimé | Seul le paiement suivant vous vient (même s’il y en a d’autres bientôt disponibles), en dollars américains. |
| Dernier paiement           | Le montant (en dollars américains), le nom du programme et le programme de votre paiement le plus récent.           |
| Paiements par source     | Montant des paiements, en dollars américains, représentés par le programme au cours des 12 derniers mois.           |
| Paiements               | Sélectionnez payant ou en attente, puis triez comme vous le souhaitez. Pour plus d’informations sur un paiement spécifique, sélectionnez affichage. Pour télécharger une copie de l’instruction de remise de paiement, sélectionnez Télécharger. Notez que les données de l’historique des transactions peuvent prendre jusqu’à 24 heures pour apparaître. vous risquez donc de ne pas voir les bénéfices associés immédiatement. |

Pour exporter les données de cette page, sélectionnez Exporter, puis suivez les instructions de la page exporter des données.

## <a name="transaction-history-page"></a>Page historique des transactions

Cette page affiche tous vos revenus individuels, y compris la date, le type et le résultat pour chacun d’entre eux. Vous pouvez sélectionner une période à afficher, et vous pouvez également filtrer par ID d’inscription, programme, ID de paiement, type en cours d’exécution, levier et état. Les données sont disponibles pour l’exercice fiscal actuel (du 1er juillet au 30 juin) et dans les deux années fiscales précédentes.

Pour obtenir plus de détails sur un gain, sélectionnez la flèche vers le bas à droite de la page. Le levier, le chiffre d’affaires et le produit s’affichent. Si, pour une raison quelconque, ces données ne sont pas disponibles, mais que vous avez besoin d’y accéder, contactez le [support technique](https://developer.microsoft.com/en-us/windows/support). Si le résultat est le résultat d’un ajustement et non d’une transaction, les champs de produit ne sont pas affichés.

Pour exporter les données de transaction sur cette page, sélectionnez Exporter, puis suivez les instructions de la page exporter des données. Les fichiers exportés à partir de la page historique des transactions affichent les données dans la devise de la transaction, les bénéfices dans la devise de la transaction et le dollar des États-Unis, et la valeur payée dans paiement à la devise

## <a name="payment-status"></a>Statut du paiement

| État de l’obtention           | Reason                                                                                                                                      | Action de partenaire requise?                                   |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| Non traité              | Le bénéfice est éligible au paiement. Il reste dans cet État pour une période de refroidissement telle que définie dans le Guide du programme d’incentives. | Non                                                         |
| À venir                 | Commande de paiement générée en attente de révisions internes avant le traitement du paiement.                                                               | Non                                                         |
| Facture d’impôt en attente      | Votre facture fiscale est incomplète ou non valide.                                                                                                  | Vous devez mettre à jour votre facture fiscale avant de pouvoir payer |
| Rejeté pendant la révision   | Le paiement a été rejeté pendant la révision.                                                                                                     | Contacter le [support Microsoft](https://developer.microsoft.com/en-us/windows/support) pour plus d’informations                      |
| Échec                   | Le paiement a échoué en raison d’une erreur système Microsoft.                                                                                         | Contacter le [support Microsoft](https://developer.microsoft.com/en-us/windows/support) pour plus d’informations                      |
| En cours              | Le paiement est en cours.                                                                                                                 | Non                                                         |
| Paiement incorrect        | Le remboursement est en cours.                                                                                                       | Non                                                         |
| Échangé                     | Le paiement a été envoyé à votre banque.                                                                                                     | Non                                                         |
| Retraitement             | Le paiement a rencontré une erreur système Microsoft et est en cours de retraitement.                                                                  | Non                                                         |
| Inversé                 | Le paiement a été inversé par votre banque et sera renvoyé dans le prochain cycle de paiement.                                                     | Non                                                         |
| Facture de taxe rejetée     | Votre facture fiscale a été rejetée pendant la révision. Tous les paiements en attente seront en attente jusqu’à ce que la révision de la facture fiscale soit terminée.                 | Contacter le [support Microsoft](https://developer.microsoft.com/en-us/windows/support) pour plus d’informations                      |
| Facture fiscale en cours de révision | Votre relevé de taxes est en cours de révision. Votre paiement est lancé une fois que la facture d’impôt a été approuvée.                                   | Non                                                         |
| Rejeté                 | Le paiement a été rejeté par votre banque.                                                                                                      | Pour plus d’informations, contactez votre banque.                             |

## <a name="export-data-page"></a>Page exporter des données

Suivez les instructions de cette page pour exporter les données souhaitées.

Remarques :

- Lorsque vous accédez à cette page à partir de la page paiements ou historique des transactions, vos filtres ne s’effectuent pas. Vous devez les refaire dans la page exporter des données.
- La page exporter des données n’est pas actualisée automatiquement. Vous devrez peut-être actualiser la page manuellement pour afficher les données les plus récentes.
- Votre filtre peut entraîner une erreur aucune donnée n’est disponible. Cela signifie probablement que vous avez laissé la période de temps par défaut sélectionnée à trois mois, puis que vous avez sélectionné un ID de paiement d’un gain en dehors de cette période. Développez votre période, puis réessayez.

## <a name="payment-download-export"></a>Exportation du téléchargement de paiement

Cette option permet de télécharger les paiements que vous avez reçus dans votre banque pour un programme donné, la taxe associée et le montant agrégé. Ce rapport est utilisé pour de nombreux programmes de l’espace partenaires. par conséquent, certaines colonnes peuvent être inapplicables à votre rapport. Ces colonnes sont indiquées ci-dessous.

| Nom de la colonne              | Description                                                                                                                             |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| participantID            | Identité principale du partenaire qui accumule le programme                                                                           |
| participantIDType        | En général, ID de programme pour les programmes de motivation et ID de vendeur pour les programmes de boutique                                                              |
| participantName          | Nom du partenaire gagnant                                                                                                             |
| nom_programme              | Nom du programme d’incentives/boutiques                                                                                                            |
| tiré                   | Montant gagné dans le paiement à la devise pour ce programme/participantID                                                                     |
| earnedUSD                | Montant gagné pour le programme/ID participant, en USD                                                                                    |
| withheldTax              | Montant des taxes retenues dans le paiement à la devise pour le programme/participantID                                                             |
| salesTax                 | Montant total des taxes dans le paiement à la devise pour le programme/participantID                                                          |
| totalPayment             | Paiement total en devise locale, à l’exclusion de la taxe à retenir et des taxes de vente (le cas échéant) pour le programme/participantID |
| currencyCode             | Paiement à code devise                                                                                                                    |
| paymentMethod            | Méthode utilisée pour payer le partenaire (virement bancaire électronique, note de crédit)                                                              |
| paymentID                | Identificateur unique du paiement. Ce nombre est généralement visible dans votre relevé bancaire.                                               |
| paymentStatus            | Statut du paiement                                                                                                                          |
| paymentStatusDescription | Description conviviale de l’état du paiement                                                                                                  |
| paymentDate              | Date à laquelle le paiement a été envoyé par Microsoft                                                                                                    |

## <a name="transaction-history-download-export"></a>Exportation du téléchargement de l’historique des transactions

Cette option fournit un téléchargement de chaque élément de ligne en cours d’obtention que vous voyez dans la page historique des transactions, le type en cours, la date, le montant de la transaction associée, le client, le produit et d’autres détails transactionnels applicables à vos programmes.

| Nom de la colonne                    | Description                                                                                                                              |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| earningId                      | Identificateur unique pour chaque gain                                                                                                       |
| participantId                  | Identité principale du partenaire qui accumule le programme                                                                            |
| participantIdType              | IDENTIFIant du vendeur                                                                                                                                |
| participantName                | Nom du partenaire gagnant                                                                                                              |
| partnerCountryCode             | Emplacement/pays du partenaire gagnant                                                                                                  |
| nom_programme                    | Nom du programme d’incentives/boutiques                                                                                                             |
| transactionId                  | Identificateur unique de la transaction                                                                                                    |
| transactionCurrency            | Devise dans laquelle la transaction client d’origine s’est produite                                                                             |
| transactionDate                | Date de la transaction. Utile pour les programmes où de nombreuses transactions contribuent à un gain                                           |
| transactionExchangeRate        | Date du taux de change utilisée pour afficher le montant correspondant USD                                                                             |
| transactionAmount              | Montant de la transaction dans la devise de transaction d’origine en fonction de la quantité de données générées                                              |
| transactionAmountUSD           | Montant de la transaction en USD                                                                                                                |
| situés                          | Indique une règle d’entreprise pour le bénéfice                                                                                                  |
| earningRate                    | Taux d’incentives appliqué au montant de la transaction pour générer un gain                                                                      |
| quantity                       | Varie selon le programme. Indique la quantité facturée pour les programmes transactionnels                                                            |
| earningType                    | Indique s’il s’agit de frais, de remise, de Coop, de vente, etc.                                                                                          |
| earningAmount                  | Montant de l’obtention dans la devise de la transaction d’origine                                                                                      |
| earningAmountUSD               | Montant de gain en USD                                                                                                                    |
| earningDate                    | Date du gain                                                                                                                      |
| calculationDate                | Date à laquelle l’obtention a été calculée dans le système                                                                                            |
| earningExchangeRate            | Taux de change utilisé pour afficher la quantité USD correspondante                                                                                  |
| exchangeRateDate               | Date du taux de change utilisée pour calculer EarningAmount USD                                                                                   |
| claimId                        | Est toujours vide                                                                                                                     |
| paymentId                      | Identificateur unique du paiement. Ce nombre est généralement visible dans votre relevé bancaire                                                 |
| paymentStatus                  | Statut du paiement                                                                                                                           |
| paymentStatusDescription       | Description conviviale de l’état du paiement                                                                                                   |
| customerId                     | Est toujours vide                                                                                                                     |
| Souhaite                   | Est toujours vide                                                                                                                     |
| partNumber                     | Est toujours vide                                                                                                                     |
| productName                    | Nom de produit lié à la transaction                                                                                                       |
| productId                      | Identificateur de produit unique                                                                                                                |
| parentProductId                | Identificateur unique du produit parent. Notez également que si la transaction ne présente aucun produit parent, l'ID du produit parent est l'ID du produit. |
| parentProductName              | Nom du produit parent. Notez également que si la transaction ne présente aucun produit parent, le nom du produit parent est identique au nom du produit.   |
| productType                    | Type du produit (par exemple, Application, Module complémentaire, Jeu, etc.)                                                                                        |
| invoiceNumber                  | Est toujours vide                                                                                                                     |
| subscriptionId                 | Est toujours vide                                                                                                                     |
| subscriptionStartDate          | Est toujours vide                                                                                                                     |
| subscriptionEndDate            | Est toujours vide                                                                                                                     |
| resellerId                     | Est toujours vide                                                                                                                     |
| Nom du revendeur                   | Est toujours vide                                                                                                                     |
| distributorId                  | Est toujours vide                                                                                                                     |
| distributorName                | Est toujours vide                                                                                                                     |
| agreementNumber                | Est toujours vide                                                                                                                     |
| agreementStartDate             | Est toujours vide                                                                                                                     |
| agreementEndDate               | Est toujours vide                                                                                                                     |
| utile                       | Est toujours vide                                                                                                                     |
| transactionType                | Type de la transaction (par exemple, achat, remboursement, contrepassation, rétrofacturation, etc.)                                                               |
| localProviderSeller            | Fournisseur local/vendeur de l’enregistrement                                                                                                          |
| taxRemitted                    | Montant des taxes versées (taxe de vente, taxe d’utilisation ou TVA/taxe sur les biens et services).                                                                                   |
| taxRemitModel                  | Tiers responsable du versement des taxes (taxe de vente, taxe d’utilisation ou TVA/taxe sur les biens et services).                                                                    |
| storeFee                       | Montant retenu par Microsoft en tant que frais pour rendre l’application ou le module complémentaire disponible dans le Store.                                           |
| transactionPaymentMethod       | Instrument de paiement client utilisé pour la transaction (par exemple, carte, facturation de l’opérateur mobile, PayPal, etc.)                                |
| tpan                           | Indique le réseau ad tiers                                                                                                     |
| purchaseTypeCode               | Est toujours vide                                                                                                                     |
| purchaseOrderType              | Est toujours vide                                                                                                                     |
| purchaseOrderCoverageStartDate | Est toujours vide                                                                                                                     |
| purchaseOrderCoverageEndDate   | Est toujours vide                                                                                                                     |
| externalReferenceId            | Est toujours vide                                                                                                                     |
| externalReferenceIdLabel       | Est toujours vide                                                                                                                     |

## <a name="payout-statement-download-export-legacy"></a>Déchargement de l’instruction de paiement Export (hérité)

Pour une durée limitée dans l’ancienne page de résumé de paiement, les instructions de paiement sont disponibles au téléchargement. Ce rapport contient les champs suivants.

> [!NOTE]
> L’historique des transactions héritées comporte une colonne appelée «réservé» qui correspond à la colonne «bénéfices» dans l’historique moderne, sauf qu’elle exclut tous les bénéfices avec l’État «paiement envoyé».

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
