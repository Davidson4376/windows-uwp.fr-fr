---
author: jnHs
Description: Le rapport Utilisation disponible dans le tableau de bord du Centre de développement Windows vous permet de déterminer la façon dont les clients utilisent votre application.
title: Rapport sur l’utilisation
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
---

# Rapport sur l’utilisation


> **Important** Le rapport **Utilisation** ne fournit des données que si vous avez activé le [Kit de développement logiciel (SDK) Visual Studio Application Insights](http://go.microsoft.com/fwlink/?LinkId=615086) dans votre application (ou que vous l’avez activé en cochant la case « Afficher la télémétrie dans le Centre de développement Windows » lors de la construction de votre package). Pour que des données soient visibles dans ce rapport, vous devez également soumettre votre application dans le tableau de bord unifié du Centre de développement, et la fonction de télémétrie de l’utilisation des applications doit avoir été activée dans vos paramètres de compte.

Le rapport **Utilisation** disponible dans le tableau de bord du Centre de développement Windows vous permet de déterminer la façon dont les clients utilisent votre application. Vous pouvez afficher ces données dans votre tableau de bord ou [télécharger le rapport](download-analytic-reports.md) et le consulter hors connexion.

Ce rapport fournit un aperçu global de l’utilisation de votre application. Si vous avez associé un abonnement Azure avec le SDK Visual Studio Application Insights de votre application, vous pouvez obtenir des informations de télémétrie plus détaillées sur l’utilisation de l’application dans le portail Azure. Des liens vous permettant d’accéder aux rapports détaillés de vos applications dans le portail Azure apparaissent dans la zone supérieure de ce rapport.

Notez que le compte Microsoft associé à votre compte de développeur doit être lié à l’abonnement Azure utilisé pour Application Insights. Si vous n’utilisez pas le même compte Microsoft aux deux emplacements, vous devez ouvrir une session sur le portail de gestion Azure, puis ajouter le compte Microsoft associé à votre compte de développeur en tant qu’administrateur de service, coadministrateur ou propriétaire pour cet abonnement.

## Appliquer les filtres


Dans la zone supérieure de la page, vous pouvez développer la section **Appliquer les filtres** pour filtrer toutes les données de cette page par plage de dates et/ou par groupe de produits (versions de système d’exploitation associées).

-   **Date** : le filtre par défaut est **72 derniers jours**, mais vous pouvez choisir jusqu’à **12 derniers mois**.
-   **Groupes de produits** : la valeur par défaut de ce filtre est **Tous**. Si votre application comporte plusieurs groupes de produits, vous pouvez en choisir un à cet emplacement.

Les informations figurant dans tous les graphiques répertoriés ci-après correspondent à la période sélectionnée dans **Appliquer les filtres**. Par défaut, ces données porteront sur tous les groupes de produits pris en charge, sauf si vous avez utilisé la section **Appliquer les filtres** pour ne visualiser que les données d’un groupe spécifique.

## Nombre total de sessions utilisateur


Le graphique **Nombre total de sessions utilisateur** affiche le nombre de sessions utilisateur quotidiennes de votre application au cours de la période sélectionnée.

Chaque session utilisateur correspond au lancement et à l’utilisation de votre application par un client sur une période donnée. Ce graphique n’effectue pas le suivi des utilisateurs uniques (autrement dit, différentes sessions utilisateur peuvent avoir été initialisées par un même client).

## Utilisateurs actifs


Le graphique **Utilisateurs actifs** affiche le nombre de clients qui ont utilisé votre application un jour spécifique de la période sélectionnée.

Chaque utilisateur actif représente un client ayant utilisé votre application le jour en question. Ce graphique n'effectue pas le suivi des sessions utilisateur uniques (autrement dit, les clients qui figurent dans ce graphique peuvent avoir utilisé votre application une ou plusieurs fois ce jour-là).

## Durée moyenne de session utilisateur en secondes


Le graphique **Durée moyenne de session utilisateur en secondes** affiche la durée moyenne pendant laquelle un client a utilisé votre application un jour spécifique de la période sélectionnée.

Vous pouvez également cliquer sur **Durée moyenne entre les sessions en secondes** pour que ce graphique indique le laps de temps moyen entre deux sessions d'utilisation de votre application sur la période sélectionnée.

## Événements personnalisés des 30 derniers jours


Le graphique **Événements personnalisés des 30 derniers jours** affiche le nombre total d'occurrences des événements personnalisés que vous avez définis pour votre application. Ces informations peuvent concerner plusieurs occurrences pour un même client.

## Vues de page des 30 derniers jours


Le graphique **Vues de page des 30 derniers jours** affiche le nombre total de vues de pages spécifiques de votre application. Ces informations peuvent inclure plusieurs vues par un même client.

 

 






<!--HONumber=May16_HO2-->


