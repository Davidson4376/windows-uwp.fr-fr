---
author: JnHs
title: Rapport de campagne de publicité
ms.assetid: 8C5907A6-8059-4CAF-951F-C97301A5EEDF
description: Le rapport de campagne de publicité, accessible à partir du tableau de bord du Centre de développement Windows, vous permet d’évaluer les performances des campagnes de publicité de votre application.
ms.author: wdg-dev-content
ms.date: 11/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, promotion, application, campagne, rapport, installations
ms.localizationpriority: medium
ms.openlocfilehash: b5f63b53fca8a97c4f6e61aa38f45a749ca0621b
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/11/2018
ms.locfileid: "3849153"
---
# <a name="ad-campaign-report"></a>Rapport de campagne de publicité

Le rapport de **campagne de publicité** dans le tableau de bord du centre de développement Windows vous permet d’évaluer les performances des [campagnes](create-an-ad-campaign-for-your-app.md) de promotion votre application. Pour afficher le rapport, développez **Attract** dans le menu de navigation de gauche, puis sélectionnez **les campagnes de publicité**.

## <a name="definitions"></a>Définitions

Ce rapport affiche les données relatives aux éléments suivants:

-   **Expositions**: nombre de fois où votre publicité a été présentée à des clients.
-   **Clics**: nombre de fois où un client a cliqué sur votre publicité.
-   **Conversions**: si votre objectif de campagne consiste à augmenter le nombre d’installations de votre application, une conversion est le nombre de fois où quelqu’un a consulté votre publicité puis installé votre application dans les 24heures. Si votre objectif de campagne consiste à augmenter l’intérêt pour votre application, une conversion est le nombre de fois où quelqu’un a consulté votre publicité puis ouvert votre application dans les 24heures. Vous trouverez ci-dessous davantage d’informations sur le suivi d’installation et la méthode utilisée pour mesurer les conversions.
-   **Dépense**: somme d’argent que vous avez dépensée pour chaque campagne.

## <a name="apply-filters"></a>Appliquer des filtres

En haut du rapport, vous pouvez utiliser les **Filtres de section** pour ajuster l’étendue des données affichées dans le rapport:

-   **Date**: la sélection par défaut est **30derniers jours**, mais vous pouvez choisir d’afficher les données portant sur 3, 6 ou 12mois.
-   **Objectif de campagne**: vous pouvez afficher toutes les campagnes ou restreindre les données pour ne visualiser que celles dont l’objectif est **App installs** ou **App engagement**.
-   **Nom de l’application**: la sélection par défaut est **Tous**, mais vous pouvez choisir d’afficher les campagnes qui ne concernent qu’une application donnée.
-   **Type de campagne**: vous pouvez afficher tous les types de campagnes ou restreindre les données pour ne visualiser que les campagnes de publicité payantes, maison ou de la communauté.
-   **État**: la sélection par défaut est **Tous**, mais vous pouvez choisir d’afficher les données qui ne concernent que les campagnes présentant un état spécifique (**Actif**, **Brouillon**, **Suspendu**, **Terminé** ou **Requiert votre attention**).


## <a name="ad-campaign-metrics"></a>Métriques de campagne de publicité

La zone supérieure de la page présente une liste de vos campagnes de publicité avec le nombre d’expositions, de clics et de conversions pour chacune d’elles, ainsi que la dépense totale et toutes les actions disponibles. Pour modifier une campagne, vous pouvez cliquer sur son nom dans cette liste.

La zone inférieure de la page affiche ces métriques de performances sous forme de lignes tracées sur un graphique. Sélectionnez les en-têtes d’onglet pour visualiser les données **Expositions**, **Clics**, **Conversions** ou **Dépense** comme décrit ci-dessus.

Vous pouvez afficher des données de performances concernant jusqu’à six campagnes de publicité à la fois. Sélectionner **Plus de campagnes** pour choisir les campagnes à afficher. Vous pouvez également sélectionner le symbole moins en regard d’une campagne affichée pour la supprimer du graphique.


## <a name="install-tracking"></a>Suivi d’installation

L’exécution d’une campagne de publicité d’installation par le biais du Centre de développement offre une exposition dont vous avez grandement besoin pour promouvoir vos applications. Les expositions publicitaires sont proposées aux clients les plus susceptibles d’être intéressés par l’application, et ces clients peuvent cliquer sur la publicité et installer l’application à partir du WindowsStore. Auparavant, il était difficile de distinguer les installations provenant d’une campagne de publicité de celles issues d’autres sources.

Ce rapport affiche le nombre d’installations que vous avez obtenues en exécutant vos campagnes de publicité. Il s’agit uniquement des téléchargements qui sont une source directe de revenus dans le cadre de vos campagnes de publicité, et non des téléchargements provenant d’autres sources.

En surveillant les installations provenant de vos campagnes de publicité, vous pouvez mesurer le vrai retour sur investissement de l’argent consacré à promouvoir vos applications. Cela vous permet également de comparer le coût d’obtention d’un nouveau client avec la valeur de durée de vie de vos clients.


## <a name="measuring-conversions"></a>Mesure des conversions

Les campagnes de publicité offrent des expositions publicitaires au sein d’autres applications. Les clients qui reçoivent cette publicité sont susceptibles d’installer l’application de deux manières: en cliquant sur la publicité ou suite à l’exposition publicitaire.

Si un client reçoit la publicité et installe l’application dans les 24heures, soit en cliquant sur la publicité, soit en accédant directement à la page de description de l’application dans le WindowsStore, cette installation est attribuée à la campagne à l’origine de l’exposition.

Le suivi de l’installation s’effectue dans le WindowsStore sur un téléphone, une tablette, un PC et d’autres appareils Windows10 en fonction du client qui a installé l’application. Le moteur de publicité effectue le suivi des clients qui affichent la publicité, et nous utilisons ces informations pour mettre en corrélation les clients qui ont consulté la publicité avec ceux qui ont installé l’application. Pour que l’installation de l’application soit comptabilisée, plusieurs critères doivent être remplis:

1.  Le client n’a pas désactivé le ciblage.
2.  Le client est connecté à un compte Microsoft.
3.  Le client répond aux exigences [COPPA](http://go.microsoft.com/fwlink?LinkId=536558) (aucun suivi n’est possible pour les clients qui ne répondent pas aux exigences COPPA).

Par conséquent, il est possible que le suivi d’installation d’application sous-estime le nombre réel d’installations générées par une campagne de publicité. Notez que vous pouvez visualiser le nombre total d’installations d’une application (par le biais de campagnes ou autres) dans le rapport [Acquisitions](acquisitions-report.md) du Centre de développement.


## <a name="account-billing-history"></a>Historique de facturation du compte

Pour visualiser toutes les transactions de campagne de publicité associées à votre compte, sélectionnez **Historique de facturation** dans le menu de navigation de gauche.

Pour chaque transaction, nous affichons les informations suivantes: la **date de la transaction**, le **nom de la campagne** approprié, le **mode de paiement** utilisé pour la facturation, l’**identifiant de paiement**, la **date de début de facturation**, la **date de fin de facturation**, le **montant total** facturé et le **statut du paiement**.

Vous pouvez télécharger l’historique de facturation de votre compte sous la forme d’un document MicrosoftWord en cliquant sur le lien **Télécharger**.

## <a name="related-topics"></a>Rubriques connexes

* [Créer une campagne de publicité pour votre application](create-an-ad-campaign-for-your-app.md)

 

 
