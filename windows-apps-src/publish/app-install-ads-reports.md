---
author: jnHs
title: "Rapport de publicité sur l’installation d’applications"
ms.assetid: 8C5907A6-8059-4CAF-951F-C97301A5EEDF
description: 
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 45dd0b2cb34e724313fe431a5b8b7a6df9b5a5cf

---

# Rapport de publicité sur l’installation d’applications

Il existe deux façons d’accéder à la page des **Rapports de publicité sur l’installation d’applications** dans le Centre de développement:

-   Sur la page de votre application, dans le menu de navigation de gauche, cliquez sur **Analyse** &gt; **Publicités d’installation d’applications**.
-   Sur la page de votre application, dans le menu de navigation de gauche, cliquez sur **Monétisation** &gt;**Promouvoir votre application**, puis sur le lien **Rapport** d’une campagne.

Vous accédez à deux sections : **Performance de l’annonce** et **Historique de facturation du compte**.

## Performance de l’annonce

Ici, les métriques de performances sont affichées sous forme de lignes tracées sur un graphique, avec la métrique sélectionnée sur l’axe y et l’heure de l’axe x. Le rapport par défaut affiche les expositions. Cliquez sur les en-têtes d’onglet pour afficher les données des autres mesures de performances. Les métriques suivies ici sont les suivantes :

-   **Expositions** : nombre de fois où votre publicité a été vue.
-   **Clics** : nombre de fois où quelqu’un a cliqué sur votre publicité.
-   **Installations** : nombre de fois où quelqu’un a cliqué sur votre publicité, puis installé votre application. Pour plus d’informations sur le suivi des installations, voir ci-dessous.

Vous pouvez afficher des données de performances pour quatre campagnes de publicité différentes à la fois. Cliquez sur **Plus de campagnes** pour choisir les campagnes à afficher. Vous pouvez également cliquer sur le symbole moins à côté d’une campagne affichée pour la supprimer.

### Qu’est-ce que le suivi d’installation ?

L’exécution d’une campagne de publicité d’installation au moyen de **Promouvoir votre application** dans le Centre de développement offre aux développeurs qui tentent de promouvoir leurs applications une exposition dont ils ont grandement besoin. Des expositions publicitaires sont proposées dans l’application aux utilisateurs les plus susceptibles d’être intéressés, et les utilisateurs cliquent sur la publicité et installent l’application à partir du Windows Store. Jusqu’à aujourd’hui, il était difficile de distinguer les installations provenant d’une campagne de publicité et celles provenant d’autres sources.

Nous avons maintenant activé les rapports d’installation pour les campagnes d’installation d’application qui indiquent aux développeurs le nombre d’installations généré par la campagne de publicité. Il s’agit uniquement des téléchargements qui sont une source directe de revenus dans le cadre de la campagne de publicité, et non des téléchargements provenant d’autres sources.

En surveillant les installations provenant de leurs campagnes de publicité, les développeurs peuvent mesurer le vrai retour sur investissement de l’argent consacré à promouvoir leurs applications. Cela leur permet également de comparer le coût d’obtention d’un nouvel utilisateur et la durée de vie des utilisateurs.

### Comment mesurons-nous les installations ?

Les campagnes de publicité **Promouvoir votre application** offrent des expositions publicitaires au sein d’autres applications. Les utilisateurs qui reçoivent cette publicité sont susceptibles d’installer l’application de deux manières : en cliquant sur la publicité ou suite à l’exposition publicitaire.

Si un utilisateur reçoit la publicité et qu’il installe l’application dans les 24 heures, soit en cliquant sur la publicité, soit en accédant directement à la page de l’application dans le Windows Store, cette installation est attribuée à la campagne à l’origine de l’exposition.

Le suivi de l’installation s’effectue dans le Windows Store (sur un téléphone, une tablette et un PC) en fonction de l’utilisateur qui a installé l’application. Le moteur de publicité effectue le suivi des utilisateurs qui affichent la publicité, et nous utilisons ces informations pour mettre en corrélation les utilisateurs qui ont consulté la publicité avec les utilisateurs qui ont installé l’application. Pour que l’installation de l’application soit comptabilisée, plusieurs critères doivent être remplis :

1.  L’utilisateur n’a pas désactivé le ciblage.
2.  L’utilisateur est connecté à un compte Microsoft.
3.  L’utilisateur répond aux exigences [COPPA](http://go.microsoft.com/fwlink?LinkId=536558) (aucun suivi n’est possible pour les utilisateurs qui ne répondent pas aux exigences COPPA).

Par conséquent, il est possible que le suivi d’installation d’application *sous-estime* le nombre réel d’installations générées par une campagne **Promouvoir votre application**. Notez que le nombre total de téléchargements pour une application est visible dans le rapport **Acquisitions** du Centre de développement.

## Historique de facturation du compte

Ici, vous pouvez voir toutes les transactions associées à votre compte. Pour chaque transaction, nous affichons la **Date de la transaction**, le **Nom de la campagne** concernée, l’**Instrument de paiement** utilisé et le **Montant total** des frais.

## Rubriques connexes

* [Création d’une campagne de publicité pour votre application](create-an-ad-campaign-for-your-app.md)
* [Gestion de votre campagne de publicité](managing-your-ad-campaign.md)
* [À propos des publicités maison](about-house-ads.md)
* [Questions courantes](common-questions.md)
 

 







<!--HONumber=Aug16_HO3-->


