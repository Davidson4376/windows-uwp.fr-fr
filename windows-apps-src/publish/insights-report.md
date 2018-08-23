---
author: jnHs
Description: The Insights report in the Windows Dev Center dashboard
title: Rapport d’analyses
ms.author: wdg-dev-content
ms.date: 06/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, série uwp, insight, tendance, anomalies, des anomalies, les modifications de données
ms.localizationpriority: medium
ms.openlocfilehash: be70dccbb7a12b65b9e7bbd07f27ae7ea3a578ff
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2018
ms.locfileid: "2816129"
---
# <a name="insights-report"></a>Rapport d’analyses


Le rapport **d’analyses** dans le tableau de bord du centre de développement Windows met en évidence les modifications importantes (augmente ou diminue pour une mesure spécifique) qui nous détectés au cours des 30 derniers jours dans vos acquisitions, la santé et/ou des données d’utilisation. Cela vous permet d’obtenir un aperçu des modifications potentiellement importantes sans avoir à afficher tous les graphiques dans chacun de ces rapports.

> [!NOTE]
> Les données de ce rapport couvrent au cours des 30 derniers jours. Vous ne pouvez pas sélectionner une autre période de temps pour ce rapport.

L’état trie les données dans trois onglets: **Acquisitions**, de **santé**et de **l’utilisation**. Pour afficher les perspectives pour l’une de ces zones, sélectionnez l’onglet.

Perspectives sont affichés lorsque nous détecte un changement important dans vos données. Pour chaque analyse, nous allons montrer les éléments suivants:
- **Type d’aperçu**: la zone dans laquelle l’analyse a été détectée.
- **Valeur**: indicateur spécifique qui a changé de manière significative (ou **tous les** si la modification s’applique à l’ensemble du **type de Insight**).
- **Date**: date à laquelle nous avons identifié la modification. Cette date correspond à la fin de la semaine dans laquelle nous avons détecté une augmentation significative du nombre ou la baisse par rapport à la semaine avant que.
- **Incidence globale**: le pourcentage de la valeur augmentée ou réduite dans votre base de clients ensemble. Cela vous aide à comprendre l’étendue l’impact d’une modification particulière peut être, en particulier lors de la comparaison avec des informations sur le pourcentage indiquée dans **principaux collaborateurs.**
- **Contributeurs principaux**: le cas échéant, le segment spécifique, package ou autre facteur identification pour aider à comprendre quels clients le changement étant lié à. Par exemple, une modification peut être détectée principalement avec les clients à partir d’un marché spécifique ou sur un certain type de périphérique. Pour les données de **l’état de santé** , cela peut inclure des hachages de défaillance spécifique ou versions du lot. Le cas échéant, nous allons également le pourcentage que la valeur a augmenté ou diminué pour ce facteur.
- **Action**:
   - Sélectionnez **Afficher 14 jours tendance** pour afficher un graphique montrant comment la métrique modifiée sur 14 jours complet menant à la date d’un aperçu.
   - Sélectionnez à **nous dire si c’est le cas** de faites-nous part de vos commentaires et dites-nous si nous proposons répercuter les informations semblent précises. Vos commentaires nous aideront à continuer d’améliorer les données que nous fournissons ici. 

