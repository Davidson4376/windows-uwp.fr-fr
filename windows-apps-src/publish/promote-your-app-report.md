---
author: shawjohn
title: "Rapport Promouvoir votre application - Développer des applications UWP"
ms.assetid: 8C5907A6-8059-4CAF-951F-C97301A5EEDF
description: "Le rapport « Promouvoir votre application », accessible depuis le tableau de bord du Centre de développement Windows, vous permet d’évaluer les performances des campagnes publicitaires de votre application."
ms.author: johnshaw
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, promotion, application, campagne, rapport, installations"
translationtype: Human Translation
ms.sourcegitcommit: b01924366a0bc2afabe2f381e72e45862f0dd682
ms.openlocfilehash: af719bbb4cc9def4e9b202c8815320698c6148d0
ms.lasthandoff: 02/08/2017

---

# <a name="promote-your-app-report"></a>Rapport Promouvoir votre application

Le rapport **Promouvoir votre application**, accessible depuis le tableau de bord du Centre de développement Windows, vous permet d’évaluer les performances des campagnes de publicité de votre application.

Pour afficher le rapport, sélectionnez **Promotions** dans le menu de navigation supérieur. En haut de la page **Promouvoir votre application**, vous verrez une liste de vos campagnes de publicité ainsi que vos métriques de performances sous forme de tableau.

En bas de la page, les métriques de performances sont affichées sous forme de lignes tracées sur un graphique, avec la métrique sélectionnée sur l’axe y et l’heure sur l’axe x. Sélectionnez les en-têtes d’onglet pour afficher les données de ces métriques de performances :

-   **Expositions** : nombre de fois où votre publicité a été proposée à des clients.
-   **Clics** : nombre de fois où un client a cliqué sur votre publicité.
-   **Conversions** : si votre objectif de campagne consiste à augmenter le nombre d’installations de votre application, une conversion est le nombre de fois où quelqu’un a consulté votre publicité puis installé votre application dans les 24 heures. Si votre objectif de campagne consiste à augmenter l’intérêt pour votre application, une conversion est le nombre de fois où quelqu’un a consulté votre publicité puis ouvert votre application dans les 24 heures. Vous trouverez ci-dessous davantage d’informations sur le suivi d’installation et la méthode utilisée pour mesurer les conversions.
-   **Argent dépensé** : la somme d’argent que vous avez dépensée pour chaque campagne.

Vous pouvez afficher des données de performances pour jusqu’à six campagnes de publicité à la fois. Sélectionner **Plus de campagnes** pour choisir les campagnes à afficher. Vous pouvez également sélectionner le symbole moins à côté d’une campagne affichée pour la supprimer.

### <a name="what-is-install-tracking"></a>Qu’est-ce que le suivi d’installation ?

L’exécution d’une campagne de publicité d’installation au moyen de **Promouvoir votre application** dans le Centre de développement offre une exposition dont vous avez grandement besoin pour promouvoir vos applications. Des expositions publicitaires sont proposées dans l’application aux clients les plus susceptibles d’être intéressés, et les clients cliquent sur la publicité et installent l’application à partir du Windows Store. Auparavant, il était difficile de distinguer les installations provenant d’une campagne de publicité et celles provenant d’autres sources.

Le rapport **Promouvoir votre application** affiche le nombre d’installations que vous avez obtenu en exécutant votre campagne de publicité. Il s’agit uniquement des téléchargements qui sont une source directe de revenus dans le cadre de votre campagne de publicité, et non des téléchargements provenant d’autres sources.

En surveillant les installations provenant de vos campagnes de publicité, vous pouvez mesurer le vrai retour sur investissement de l’argent consacré à promouvoir vos applications. Cela vous permet également de comparer le coût d’obtention d’un nouveau client avec la valeur à vie de vos clients.

### <a name="how-are-conversions-measured"></a>Comment mesurons-nous les conversions ?

Les campagnes de publicité **Promouvoir votre application** offrent des expositions publicitaires au sein d’autres applications. Les clients qui reçoivent cette publicité sont susceptibles d’installer l’application de deux manières : en cliquant sur la publicité ou suite à l’exposition publicitaire.

Si un client reçoit la publicité et installe l’application dans les 24 heures, soit en cliquant sur la publicité, soit en accédant directement à la page de l’application dans le Windows Store, cette installation est attribuée à la campagne à l’origine de l’exposition.

Le suivi de l’installation s’effectue dans le Windows Store (sur un téléphone, une tablette, un PC et autres appareils Windows 10) en fonction du client qui a installé l’application. Le moteur de publicité effectue le suivi des clients qui affichent la publicité, et nous utilisons ces informations pour mettre en corrélation les clients qui ont consulté la publicité avec ceux qui ont installé l’application. Pour que l’installation de l’application soit comptabilisée, plusieurs critères doivent être remplis :

1.  Le client n’a pas désactivé le ciblage.
2.  Le client est connecté à un compte Microsoft.
3.  Le client répond aux exigences [COPPA](http://go.microsoft.com/fwlink?LinkId=536558) (aucun suivi n’est possible pour les clients qui ne répondent pas aux exigences COPPA).

Par conséquent, il est possible que le suivi d’installation d’application *sous-estime* le nombre réel d’installations générées par une campagne **Promouvoir votre application**. Notez que le nombre total de téléchargements pour une application est visible dans le rapport **Acquisitions** du Centre de développement.

## <a name="account-billing-history"></a>Historique de facturation du compte

Pour afficher toutes les transactions associées à votre compte, sélectionnez **Historique de facturation** dans le menu de navigation gauche.

Pour chaque transaction, nous affichons les informations suivantes : la **date de la transaction**, le **nom de la campagne** approprié, le **mode de paiement** utilisé pour la facturation, l’**identifiant de paiement**, la **date de début de facturation**, la **date de fin de facturation**, le **montant total** facturé et le **statut du paiement**.

Vous pouvez également télécharger l’historique de facturation de votre compte sous forme de document Microsoft Word en cliquant sur le lien **Télécharger**.

## <a name="related-topics"></a>Rubriques connexes

* [Créer une campagne de publicité pour votre application](create-an-ad-campaign-for-your-app.md)
* [Gestion de votre campagne de publicité](managing-your-ad-campaign.md)
* [À propos des publicités maison](about-house-ads.md)
* [Questions courantes](common-questions.md)
 

 

