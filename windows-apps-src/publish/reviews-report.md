---
Description: Le rapport de révisions dans Partner Center vous permet de voir les révisions (commentaires) que les clients entrés lors de l’évaluation de votre application dans le Store.
title: Rapport sur les révisions
ms.assetid: E50C3A4D-1D8A-4E5B-8182-3FAD049F2A2D
ms.date: 08/16/2018
ms.topic: article
keywords: Windows 10, uwp, révision, commentaire, réviseur
ms.localizationpriority: medium
ms.openlocfilehash: 7ec883e7bcb98d69673b520df918e085182d35ec
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621824"
---
# <a name="reviews-report"></a>Rapport sur les révisions


Le **passe en revue** signaler dans [partenaires](https://partner.microsoft.com/dashboard) vous permet de voir les révisions (commentaires) que les clients entrés lors de l’évaluation de votre application dans le Store.

Vous pouvez afficher ces données dans le centre de partenaires, ou [télécharger le rapport](download-analytic-reports.md) à afficher en mode hors connexion. Ou bien, vous pouvez récupérer ces données par programmation à l’aide de la [obtenir les révisions d’application](../monetize/get-app-reviews.md) méthode dans le [analytique Microsoft Store REST API](../monetize/access-analytics-data-using-windows-store-services.md).

Vous pouvez également répondre aux revues client [directement à partir de cette page](respond-to-customer-reviews.md) ou par programmation [via le Microsoft Store révisions API](../monetize/submit-responses-to-app-reviews.md).

> [!TIP]
> Pour obtenir un aperçu des avis, des évaluations et des commentaires des utilisateurs formulés pour l’ensemble de vos applications au cours des 30 derniers jours, développez **Engager** dans le menu de navigation de gauche, puis sélectionnez **Reviews and feedback**. 


## <a name="apply-filters"></a>Appliquer les filtres

Dans la zone supérieure de la page, vous pouvez sélectionner la période sur laquelle portent les avis qui vous intéressent. La valeur par défaut est **Durée de vie**, mais vous pouvez choisir d’afficher les avis portant sur des périodes de 30 jours ou de 3, 6 ou 12 mois, ou sur une plage de dates personnalisée que vous spécifiez.

Vous pouvez développer **Filtres** pour filtrer les avis affichés sur cette page en fonction des options suivantes. Ces filtres ne s’appliqueront pas aux graphiques **Répartition des classifications** et **Évaluation moyenne au fil du temps**.

-   **Évaluation**: Par défaut les révisions portant sur toutes les évaluations sont activées, mais vous pouvez vérifier et décochez la case propres évaluations (de 1 à 5 étoiles) si vous souhaitez afficher uniquement les avis associés à des évaluations particuliers.
- **Passez en revue le contenu**: Le paramètre par défaut est **des évaluations avec passez en revue le contenu**, ce qui signifie que seulement les évaluations avec passez en revue le contenu s’affichera. Vous pouvez sélectionner **tous les** pour afficher toutes les évaluations, y compris celles qui n’incluent pas les écrites Lisez le texte. Notez que le **répartition des évaluations** graphique s’affichent toujours toutes les révisions, quel que soit votre sélection.
-   **Version du système d’exploitation**: Le paramètre par défaut est **tous les**. Vous pouvez choisir une version de système d’exploitation spécifique si vous souhaitez que cette page affiche uniquement les avis laissés par les clients qui utilisent cette version.
-   **Version du package**: Le paramètre par défaut est **tous les**. Si votre application comprend plusieurs packages, vous pouvez choisir un package spécifique ici si vous souhaitez visualiser uniquement les avis laissés par les clients qui disposaient de ce package lorsqu’ils ont évalué votre application.
-   **Réponses**: Le paramètre par défaut est **tous les**. Vous pouvez choisir de ne visualiser que les avis pour lesquels vous avez [répondu aux clients](respond-to-customer-reviews.md), ou uniquement ceux auxquels vous n'avez pas encore répondu.
-   **Mises à jour** : Le paramètre par défaut est **tous les**. Vous pouvez choisir de ne visualiser que les avis qui ont été mis à jour par le client depuis que vous avez [répondu à ces avis](respond-to-customer-reviews.md), ou uniquement ceux qui n’ont pas encore été mis à jour par le client.
-   **Marché**: Le paramètre par défaut est **tous les marchés**. Vous pouvez choisir un marché spécifique si vous ne souhaitez visualiser que les avis de clients appartenant à ce marché.
-   **Type d’appareil**: Le filtre par défaut est **tous les appareils**. Vous pouvez choisir un type d’appareil spécifique si vous souhaitez que cette page affiche uniquement les avis laissés par les clients utilisant celui-ci.
-   **Nom de la catégorie**: Le filtre par défaut est **tous les**. Vous pouvez choisir un spécifique [passez en revue la catégorie d’insight](#review-insight-categories) pour afficher uniquement les révisions que nous avons associé à cette catégorie. 

> [!TIP]
> Si cette page ne contient aucun avis, assurez-vous que vos filtres n’ont pas exclu la totalité des avis concernant votre application. Par exemple, si vous filtrez les avis en fonction d’un système d’exploitation non pris en charge par votre application, aucun avis n’apparaîtra sur cette page.


## <a name="ratings-breakdown"></a>Répartition des classifications

Le **répartition des évaluations** graphique s’affiche en haut de ce rapport afin que vous pouvez obtenir un coup de œil rapide à ce qui suit : 
- Évaluation moyenne à l’aide d’étoiles concernant l’application
- Nombre total d’évaluations de votre application au cours des 12 derniers mois.
- Nombre total d’évaluations correspondant à chaque nombre d’étoiles.
- Nombre d’évaluations de chaque type (nouvelles ou révisées) par nombre d’étoiles au cours des 12 derniers mois.
 - La section **Nouvelles évaluations** spécifie les évaluations qui ont été soumises par les clients et qui n’ont absolument pas été modifiées.
 - La section **Évaluations révisées** spécifie les évaluations qui ont été modifiées d’une façon quelconque par le client, même si cette modification porte uniquement sur le texte de l’avis.

> [!TIP]
> L’évaluation moyenne mise à disposition d’un client dans le Windows Store tient compte du marché et du type d’appareil du client. Dès lors, elle peut différer du contenu présenté dans ce rapport.

Notez que ce graphique inclut toujours toutes vos révisions, même si vous avez sélectionné **des évaluations avec passez en revue le contenu** dans le **passez en revue le contenu** filtre de page.

Ce graphique peut également être affichée dans le [rapport de contrôle d’accès](ratings-report.md), ainsi que de plus en détail des évaluations de votre application.


<span id = "review-insight-categories" />

## <a name="insight-categories"></a>Catégories d’Insight

Le **catégories d’Insight** graphique regroupe vos révisions en fonction des catégories que nous avons déterminé peut être associée à la révision.

> [!NOTE]
> Lorsque vous visualisez les avis par catégorie, les avis datant de moins de 24 heures et/ou formulés dans une langue autre que l’anglais ne sont pas inclus.

Dans la zone supérieure de la page, des blocs colorés représentent les avis par catégorie. Sélectionnez l’une de ces catégories pour visualiser uniquement les avis que nous lui avons associés. Vous pouvez également utiliser les [filtres de page](#apply-filters) pour effectuer un filtrage par catégorie.

Pour visualiser une répartition du nombre d’avis par catégorie, sélectionnez **Afficher les détails**. 


## <a name="reviews"></a>Avis

Chaque avis de client contient les éléments suivants :

-   Le titre et le texte de l'avis rédigés par le client. (Les avis rédigés par les clients qui utilisent Windows Phone 8.1 et les versions antérieures sont dépourvus de titre.)
-   La date de l'avis.
-   Le nom du réviseur tel qu’il apparaît dans le Microsoft Store.
-   Le pays ou la région de l'auteur de l'avis.
-   La version du package de l'application installée sur l'appareil du client au moment de la rédaction de l'avis. (Ces informations ne sont pas disponible pour les révisions soumis en ligne ou soumis par les clients sur Windows 8.1 et versions antérieures).
-   La version du système d'exploitation installée sur l'appareil du client au moment de la rédaction de l'avis.
-   Le nom de l'appareil utilisé par le client au moment de la rédaction de l'avis. (Ces informations ne sont pas disponible pour les révisions soumis en ligne ou soumis par les clients sur Windows 8.1 et versions antérieures).
-   La « note d'utilité » de l'avis signalant le nombre de fois où l'avis a été considéré comme utile par d'autres clients. Cette information est indiquée sous la forme de deux valeurs : la première spécifie le nombre de clients ayant jugé l'avis utile, la seconde correspond au nombre total de clients ayant laissé une évaluation. Par exemple, une note d'utilité de 4/10 signifie que sur 10 personnes, 4 ont trouvé l'avis utile, et 6 non. (Si aucun utilisateur n’a évalué l’utilité d’un avis, aucune note d’utilité ne s'affiche.)

Notez que les clients peuvent évaluer votre application sans formuler de commentaires. Le nombre d'avis sera donc généralement inférieur au nombre d'évaluations.

Vous pouvez trier les avis sur la page par date et/ou par évaluation, dans l'ordre croissant ou décroissant. Cliquez sur le **trier par** lien pour afficher les options pour trier par **Date** et/ou **évaluation**.

Vous pouvez également utiliser la zone de recherche pour rechercher des mots ou expressions dans les révisions de votre application. Notez que seuls le texte d’origine révision écrit par le client est recherché, même si la révision a été écrit dans une autre langue. Texte de révision traduites n’est pas recherché.

> [!NOTE]
> Les avis peuvent disparaître ponctuellement de ce rapport. Cela peut se produire lorsque Microsoft supprime des avis du Windows Store qui ont été rédigés par des clients qui utilisent certaines versions préliminaires ou builds de Windows 10 réservées aux Insiders. Nous procédons ainsi afin de réduire le risque de publication d’avis négatifs liés à un problème d’une version préliminaire de Windows. Nous pouvons également supprimer les avis du Windows Store qui ont été identifiés comme indésirables, inappropriés, offensants ou contraires à notre politique. Nous pensons que cette action va améliorer l’expérience utilisateur.


## <a name="translating-reviews"></a>Traduction des avis

Les commentaires qui n'ont pas été rédigés dans votre langue sont traduits par défaut. Si vous le souhaitez, vous pouvez désactiver la traduction des avis en décochant la case **Traduire les avis** située en haut à droite au-dessus de la liste des avis.

Notez que les évaluations sont traduites par un système de traduction automatique et que le résultat de la traduction n’est pas toujours précis. Le texte d’origine est fourni si vous souhaitez comparer la traduction ou utiliser un autre moyen de traduction.

Comme indiqué ci-dessus, lorsque recherche vos révisions, seul le texte d’origine laissé par le client recherche est effectuée (et non le texte traduit), même si vous avez le **traduire des révisions** case cochée.


## <a name="responding-to-customer-reviews"></a>Réponse aux avis des clients

Vous pouvez utiliser [partenaires](https://partner.microsoft.com/dashboard) ou [Microsoft Store passe en revue les API](../monetize/submit-responses-to-app-reviews.md) d’envoyer des réponses à de nombreuses révisions de vos clients. Pour plus d'informations, voir l'article [Répondre aux avis des clients](respond-to-customer-reviews.md).

Vous découvrirez ci-après certaines actions supplémentaires à envisager en fonction des évaluations et des avis que vous voyez.

-   Si vous notez de nombreux avis suggérant une nouvelle fonctionnalité ou la modification d'une fonctionnalité existante, ou se plaignant d'un problème, envisagez de publier une nouvelle version répondant spécifiquement à ces commentaires. (Veillez alors à mettre à jour la [description](create-app-descriptions.md) de votre application pour indiquer que le problème a été résolu.)
-   Si l’évaluation moyenne est élevée, mais que le nombre de téléchargements est faible, vous pouvez rechercher des façons d’[élargir le public cible de votre application](attract-customers-and-promote-your-apps.md), puisque celle-ci a été bien accueillie par les utilisateurs qui l’ont essayée.


 

 

 
