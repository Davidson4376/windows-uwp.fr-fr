---
title: Statistiques et classements 2017
description: Découvrez comment configurer Xbox Live proposées statistiques et classements 2017 dans Partner Center
ms.assetid: e0f307d2-ea02-48ea-bcdf-828272a894d4
ms.date: 10/30/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live, Xbox, jeux, uwp, windows 10, Xbox une, proposées des statistiques et classements, classements, statistiques 2017, les partenaires
ms.openlocfilehash: e152ea8aa745536f0b7843f4f7d372a79dc3223f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655104"
---
# <a name="configuring-featured-stats-and-leaderboards-2017-in-partner-center"></a>Configuration proposées statistiques et classements 2017 dans Partner Center

Pour un jeu d’interagir avec le service de statistiques, une stat doit être défini dans [partenaires](https://partner.microsoft.com/dashboard). Toutes les statistiques proposées s’afficheront sur GameHub, ce qui le rend automatiquement agir en tant qu’un classement. Nous stockons la valeur brute, toutefois, le jeu sera propriétaire de la logique permettant de déterminer si une nouvelle valeur doit être fournie.

![Capture d’écran de la page de primes sur le Hub de jeu](../../images/dev-center/featured-stats-and-leaderboards/featured-stats-and-leaderboards-2.png) l’image ci-dessus montre l’aspect des statistiques proposées dans GameHub de votre titre. Les statistiques proposées sont affichées au sein de la zone rouge.

Avec 2017 de plateforme de données, il vous suffit de configurer un état qui est utilisé pour un classement de réseaux sociaux qui sera proposé sur la page de GameHub du lecteur.

Vous pouvez utiliser les partenaires pour configurer un stat proposée et le classement associé à votre jeu. Ajoutez la configuration en procédant comme suit :

1. Accédez à la **proposées des statistiques et classements** section pour votre titre, situé sous **Services** > **Xbox Live**  >  **Proposées statistiques et classements**.
2. Cliquez sur le **New** bouton qui ouvre un formulaire modal. Une fois rempli, cliquez sur **enregistrer**.

![Image de la nouvelle boîte de dialogue stat/classement proposée](../../images/dev-center/featured-stats-and-leaderboards/featured-stats.png)

Le **surnom** champ est ce que les utilisateurs voient dans le GameHub. Cette chaîne peut être localisée dans le **localiser les chaînes** de la configuration du service Xbox Live.

Le **ID** champ est le nom stat et est la façon dont vous ferez référence à votre stat lors de la mise à jour à partir de votre code. Consultez le [la mise à jour des statistiques](../../leaderboards-and-stats-2017/player-stats-updating.md) pour plus d’informations.

Le **Format** est le format de données de la statistique. Les options incluent entier, décimal, pourcentage, intervalle de temps court, Long intervalle de temps et chaîne.

Chaque **Format** option vous donne des informations sur les valeurs acceptables ou la mise en forme sous la liste déroulante vers le bas lorsque sélectionné.

* Statistiques de l’entier acceptent des nombres entiers tels que 1, 2 ou 100.
* Statistiques décimales acceptent des nombres fractionnaires avec deux décimales comme 1,05 ou 12.00
* Statistiques de pourcentage acceptent des nombres entiers compris entre 0 et 100. '%' sera ajouté à la fin du nombre entier. (par exemple, 0 %, 100 %)
* Statistiques de l’intervalle de temps court suivent le format hh : mm : comme 02:10:30 et vous demande de fournir une unité de temps pour la statistique.   Les unités de temps disponibles sont les millisecondes, secondes, minutes, heures et jours.
* Intervalle de temps long stat suivent le format de Xd Xh Xm tel que 1d 2h 10m, cette statistique vous demandera également pour fournir une unité de temps pour la statistique.

Le **tri** champ vous permet de modifier l’ordre de tri du classement à être croissant ou décroissant.

Lorsque vous configurez un stat proposée et le classement, notez les conditions suivantes :

| Type de développeur | Condition requise | Limite |
|----------------|-------------|-------|
| Programme Créateurs Xbox Live | Il n’est pas nécessaire pour indiquer que les statistiques des statistiques proposées | 20 |
| ID@Xbox et les partenaires Microsoft | Vous devez désigner au moins 3 des statistiques en vedette | 20 |

## <a name="next-steps"></a>Étapes suivantes

Ensuite, vous devez mettre à jour les statistiques à partir de votre code.  Consultez [la mise à jour des statistiques](../../leaderboards-and-stats-2017/player-stats-updating.md) pour plus d’informations.
