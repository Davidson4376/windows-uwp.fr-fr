---
title: Rapport de campagne publicitaire
ms.assetid: 8C5907A6-8059-4CAF-951F-C97301A5EEDF
description: Le rapport de campagne Ad dans Partner Center vous permet de voir le fonctionnement de vos campagnes de promotion application.
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, promotion, application, campagne, rapport, installations
ms.localizationpriority: medium
ms.openlocfilehash: 7b798abf88805ef3c693149149be958ba0cc1de9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653544"
---
# <a name="ad-campaign-report"></a>Rapport de campagne publicitaire

Le **campagne de publicité** signaler dans [partenaires](https://partner.microsoft.com/dashboard) vous permet de voir comment la promotion de votre application [campagnes de publicité](create-an-ad-campaign-for-your-app.md) effectuez. Pour afficher le rapport, développez **attraction** dans le menu de navigation gauche, puis sélectionnez **campagnes marketing**.

## <a name="definitions"></a>DÉFINITIONS

Ce rapport affiche les données relatives aux éléments suivants :

-   **Les expositions**: Le nombre de fois que votre annonce a été indiqué pour les clients.
-   **Clique sur**: Le nombre de fois où un utilisateur a cliqué sur votre annonce.
-   **Conversions**: Si votre objectif de la campagne est d’augmenter les installations de votre application, une conversion est le nombre de fois que quelqu'un a affiché votre annonce et ensuite installé votre application dans les 24 heures. Si votre objectif de campagne consiste à augmenter l’intérêt pour votre application, une conversion est le nombre de fois où quelqu’un a consulté votre publicité puis ouvert votre application dans les 24 heures. Vous trouverez ci-dessous davantage d’informations sur le suivi d’installation et la méthode utilisée pour mesurer les conversions.
-   **Dépenses**: La somme d’argent que vous avez passé sur chaque campagne.

## <a name="apply-filters"></a>Appliquer les filtres

En haut du rapport, vous pouvez utiliser les **Filtres de section** pour ajuster l’étendue des données affichées dans le rapport :

-   **Date** : La sélection par défaut est **30 derniers jours**, mais vous pouvez choisir d’afficher les données pour 3, 6 ou 12 mois.
-   **Objectif de la campagne**: Vous pouvez afficher toutes les campagnes marketing, ou limiter les données pour afficher uniquement celles dont l’objectif est **application installe** ou **engagement envers l’application**.
-   **Nom de l’application**: La sélection par défaut est **tous les**, mais vous pouvez choisir d’afficher les campagnes pour seulement une application spécifique.
-   **Type de campagne**: Vous pouvez afficher tous les types de campagne ou limiter les données à afficher uniquement payé, maison ou des campagnes de publicité Communauté.
-   **Statut** : La sélection par défaut est **tous les**, mais vous pouvez choisir d’afficher les données uniquement pour les campagnes qui appartiennent à un état particulier (**Active**, **Draft**, **suspendu**, **Terminé**, ou **nécessite une attention**).


## <a name="ad-campaign-metrics"></a>Métriques de campagne de publicité

La zone supérieure de la page présente une liste de vos campagnes de publicité avec le nombre d’expositions, de clics et de conversions pour chacune d’elles, ainsi que la dépense totale et toutes les actions disponibles. Pour modifier une campagne, vous pouvez cliquer sur son nom dans cette liste.

La zone inférieure de la page affiche ces métriques de performances sous forme de lignes tracées sur un graphique. Sélectionnez les en-têtes d’onglet pour visualiser les données **Expositions**, **Clics**, **Conversions** ou **Dépense** comme décrit ci-dessus.

Vous pouvez afficher des données de performances concernant jusqu’à six campagnes de publicité à la fois. Sélectionner **Plus de campagnes** pour choisir les campagnes à afficher. Vous pouvez également sélectionner le symbole moins en regard d’une campagne affichée pour la supprimer du graphique.


## <a name="install-tracking"></a>Suivi d’installation

Une campagne de publicité installation en cours d’exécution via des partenaires présente très utile pour publier vos applications. Les expositions publicitaires sont proposées aux clients les plus susceptibles d’être intéressés par l’application, et ces clients peuvent cliquer sur la publicité et installer l’application à partir du Windows Store. Auparavant, il était difficile de distinguer les installations provenant d’une campagne de publicité de celles issues d’autres sources.

Ce rapport affiche le nombre d’installations que vous avez obtenues en exécutant vos campagnes de publicité. Il s’agit uniquement des téléchargements qui sont une source directe de revenus dans le cadre de vos campagnes de publicité, et non des téléchargements provenant d’autres sources.

En surveillant les installations provenant de vos campagnes de publicité, vous pouvez mesurer le vrai retour sur investissement de l’argent consacré à promouvoir vos applications. Cela vous permet également de comparer le coût d’obtention d’un nouveau client avec la valeur de durée de vie de vos clients.


## <a name="measuring-conversions"></a>Mesure des conversions

Les campagnes de publicité offrent des expositions publicitaires au sein d’autres applications. Les clients qui reçoivent cette publicité sont susceptibles d’installer l’application de deux manières : en cliquant sur la publicité ou suite à l’exposition publicitaire.

Si un client reçoit la publicité et installe l’application dans les 24 heures, soit en cliquant sur la publicité, soit en accédant directement à la page de description de l’application dans le Windows Store, cette installation est attribuée à la campagne à l’origine de l’exposition.

Le suivi de l’installation s’effectue dans le Windows Store sur un téléphone, une tablette, un PC et d’autres appareils Windows 10 en fonction du client qui a installé l’application. Le moteur de publicité effectue le suivi des clients qui affichent la publicité, et nous utilisons ces informations pour mettre en corrélation les clients qui ont consulté la publicité avec ceux qui ont installé l’application. Pour que l’installation de l’application soit comptabilisée, plusieurs critères doivent être remplis :

1.  Le client n’a pas désactivé le ciblage.
2.  Le client est connecté à un compte Microsoft.
3.  Le client répond aux exigences [COPPA](https://go.microsoft.com/fwlink?LinkId=536558) (aucun suivi n’est possible pour les clients qui ne répondent pas aux exigences COPPA).

Par conséquent, il est possible que le suivi d’installation d’application sous-estime le nombre réel d’installations générées par une campagne de publicité. Veuillez noter que vous pouvez afficher le nombre total d’installations pour une application (par le biais des campagnes et sinon) dans le [Acquisitions](acquisitions-report.md) rapport dans l’espace partenaires.


## <a name="account-billing-history"></a>Historique de facturation du compte

Pour visualiser toutes les transactions de campagne de publicité associées à votre compte, sélectionnez **Historique de facturation** dans le menu de navigation de gauche.

Pour chaque transaction, nous affichons les informations suivantes : la **date de la transaction**, le **nom de la campagne** approprié, le **mode de paiement** utilisé pour la facturation, l’**identifiant de paiement**, la **date de début de facturation**, la **date de fin de facturation**, le **montant total** facturé et le **statut du paiement**.

Vous pouvez télécharger l’historique de facturation de votre compte sous la forme d’un document Microsoft Word en cliquant sur le lien **Télécharger**.

## <a name="related-topics"></a>Rubriques connexes

* [Créer une campagne de publicité pour votre application](create-an-ad-campaign-for-your-app.md)

 

 
