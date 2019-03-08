---
Description: Le rapport de contrôle d’accès dans Partner Center vous permet de voir la façon dont les clients évalués à votre application dans le Store.
title: Rapport sur les évaluations
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, rating, étoiles en étoile, taux, évalués
ms.localizationpriority: medium
ms.openlocfilehash: 9b507212cd660a025bc3d858fc2938cf59d4219f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640054"
---
# <a name="ratings-report"></a>Rapport sur les évaluations


Le **évaluations** signaler dans [partenaires](https://partner.microsoft.com/dashboard) vous permet de voir la façon dont les clients évalués à votre application dans le Store. 

Vous pouvez afficher ces données dans le centre de partenaires, ou [télécharger le rapport](download-analytic-reports.md) à afficher en mode hors connexion. Ou bien, vous pouvez récupérer ces données par programmation à l’aide de la [obtenir les révisions d’application](../monetize/get-app-reviews.md) méthode dans le [analytique Microsoft Store REST API](../monetize/access-analytics-data-using-windows-store-services.md).

> [!TIP]
> Pour obtenir un aperçu des avis, des évaluations et des commentaires des utilisateurs formulés pour l’ensemble de vos applications au cours des 30 derniers jours, développez **Engager** dans le menu de navigation de gauche, puis sélectionnez **Reviews and feedback**. 

## <a name="apply-filters"></a>Appliquer les filtres

Dans la zone supérieure de la page, vous pouvez sélectionner la période sur laquelle portent les avis qui vous intéressent. La valeur par défaut est **Durée de vie**, mais vous pouvez choisir d’afficher les avis portant sur des périodes de 30 jours ou de 3, 6 ou 12 mois, ou sur une plage de dates personnalisée que vous spécifiez. Notez que les graphiques **Répartition des classifications** et **Évaluation moyenne au fil du temps** affichent toujours les données des 12 derniers mois ; ces options de période n’auront donc aucune incidence sur ces graphiques.

Vous pouvez développer **Filtres** pour filtrer les avis affichés sur cette page en fonction des options suivantes. Ces filtres ne s’appliqueront pas aux graphiques **Répartition des classifications** et **Évaluation moyenne au fil du temps**.

-   **Marché**: Le paramètre par défaut est **tous les marchés**. Vous pouvez choisir un marché spécifique si vous souhaitez ne visualiser que les évaluations des clients sur ce marché.
-   **Type d’appareil**: Le filtre par défaut est **tous les appareils**. Vous pouvez choisir un type d’appareil spécifique si vous souhaitez que cette page affiche uniquement l’évaluation laissée par les clients utilisant celui-ci.
-   **Version du système d’exploitation**: Le paramètre par défaut est **tous les**. Vous pouvez choisir une version du système d’exploitation spécifique si vous souhaitez que cette page pour afficher uniquement la gauche des évaluations de clients sur cette version du système d’exploitation.
-   **Nom de la catégorie**: Le filtre par défaut est **tous les**. Vous pouvez choisir d’afficher uniquement les évaluations associées avec les révisions que nous avons identifiés comme appartenant à un spécifique [passez en revue la catégorie d’insight](reviews-report.md#insight-categories) pour afficher uniquement les révisions que nous avons associé à cette catégorie. 

> [!TIP]
> Si vous ne voyez pas n’importe quel contrôle d’accès sur la page, vérifiez que vos filtres n’est pas exclu toutes vos évaluations. Par exemple, si vous filtrez par un système d’exploitation cible que votre application ne prend pas en charge, vous ne verrez n’importe quel contrôle d’accès.


## <a name="rating-breakdown"></a>Répartition de l’évaluation

Le **évaluation répartition** graphique montre : 
- Évaluation moyenne à l’aide d’étoiles concernant l’application
- Nombre total d’évaluations de votre application au cours des 12 derniers mois.
- Nombre total d’évaluations correspondant à chaque nombre d’étoiles.
- Nombre d’évaluations de chaque type (nouvelles ou révisées) par nombre d’étoiles au cours des 12 derniers mois.
 - La section **Nouvelles évaluations** spécifie les évaluations qui ont été soumises par les clients et qui n’ont absolument pas été modifiées.
 - La section **Évaluations révisées** spécifie les évaluations qui ont été modifiées d’une façon quelconque par le client, même si cette modification porte uniquement sur le texte de l’avis.

> [!TIP]
> L’évaluation moyenne mise à disposition d’un client dans le Windows Store tient compte du marché et du type d’appareil du client. Dès lors, elle peut différer du contenu présenté dans ce rapport.


## <a name="average-rating"></a>Évaluation moyenne

Le **évaluation moyenne** graphique montre la manière dont l’évaluation moyenne de l’application a changé au cours des 12 mois.

Au lieu de calculer la moyenne de toutes les évaluations gauche pendant les 12 derniers mois (comme dans le **répartition des évaluations** graphique), la **évaluation moyenne** graphique vous montre comment les clients a évalué l’application sur une semaine donnée. Ceci peut vous aider à identifier les tendances ou à déterminer la mesure dans laquelle les mises à jour ou d’autres facteurs ont affecté ou non les évaluations.

## <a name="rating-by-market"></a>Évaluation par marché

Le **évaluation par marché** graphique montre une répartition de l’évaluation moyenne sur chaque marché au fil de la période sélectionnée. Par défaut, nous affichons ces données dans un élément visuel **carte** formulaire, mais vous pouvez également activer le contrôle dans le coin supérieur droit pour l’afficher en tant qu’un **Table**.

Le **Table** affiche cinq marchés à la fois, triés par ordre alphabétique ou par évaluation moyenne la plus élevée/la plus basse. Vous pouvez également télécharger des données de ce graphique pour afficher simultanément les informations pour tous les marchés.

Vous pouvez également filtrer ce graphique par **évaluation**. Par défaut les révisions portant sur toutes les évaluations sont activées, mais vous pouvez vérifier et décochez la case propres évaluations (de 1 à 5 étoiles) si vous souhaitez afficher uniquement les avis associés à des évaluations particuliers.