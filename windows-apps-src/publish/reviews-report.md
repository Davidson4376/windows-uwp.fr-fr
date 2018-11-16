---
author: jnHs
Description: The Reviews report in Partner Center lets you see the reviews (comments) that customers entered when rating your app in the Store.
title: Rapport sur les révisions
ms.assetid: E50C3A4D-1D8A-4E5B-8182-3FAD049F2A2D
ms.author: wdg-dev-content
ms.date: 08/16/2018
ms.topic: article
keywords: Windows 10, uwp, passez en revue, commentaire, réviseur
ms.localizationpriority: medium
ms.openlocfilehash: 777decb9b060cc411d6150f199cd7a02f9ffc6cb
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6983824"
---
# <a name="reviews-report"></a>Rapport sur les révisions


Le rapport **avis** dans [L’espace partenaires](https://partner.microsoft.com/dashboard) vous permet de l’avis (commentaires) que les clients ont formulés pendant l’évaluation de votre application dans le Windows Store.

Vous pouvez afficher ces données dans l’espace partenaires, ou [Télécharger le rapport](download-analytic-reports.md) à consulter hors connexion. Par ailleurs, vous pouvez récupérer ces données par programme à l’aide de la méthode [get des avis concernant l’application](../monetize/get-app-reviews.md) dans [Microsoft Store analytique API REST](../monetize/access-analytics-data-using-windows-store-services.md).

Vous pouvez également répondre au client avis [directement à partir de cette page](respond-to-customer-reviews.md), par programmation [via l’API d’avis du Microsoft Store](../monetize/submit-responses-to-app-reviews.md), ou à l’aide de l' [application Centre de développement](https://www.microsoft.com/store/apps/dev-center/9nblggh4r5ws).

> [!TIP]
> Pour obtenir un aperçu des avis, des évaluations et des commentaires des utilisateurs formulés pour l’ensemble de vos applications au cours des 30derniers jours, développez **Engager** dans le menu de navigation de gauche, puis sélectionnez **Reviews and feedback**. 


## <a name="apply-filters"></a>Appliquer des filtres

Dans la zone supérieure de la page, vous pouvez sélectionner la période sur laquelle portent les avis qui vous intéressent. La valeur par défaut est **Durée de vie**, mais vous pouvez choisir d’afficher les avis portant sur des périodes de 30jours ou de 3, 6 ou 12mois, ou sur une plage de dates personnalisée que vous spécifiez.

Vous pouvez développer **Filtres** pour filtrer les avis affichés sur cette page en fonction des options suivantes. Ces filtres ne s’appliqueront pas aux graphiques **Répartition des classifications** et **Évaluation moyenne au fil du temps**.

-   **Évaluation**: les avis avec des évaluations incluant toutes les étoiles sont sélectionnés par défaut, mais vous pouvez sélectionner ou désélectionner des évaluations spécifiques (comprenant entre 1 et 5étoiles) si vous souhaitez ne visualiser que les avis associés à des évaluations données.
- **Passez en revue le contenu**: le paramètre par défaut est **que les évaluations avec avis**, ce qui signifie que les évaluations uniquement avec le contenu de l’avis seront affiche. Vous pouvez sélectionner **tous les** afficher toutes les évaluations, y compris ceux qui n’incluent pas n’importe quel texte d’avis écrite. Notez que le graphique de **répartition des classifications** s’affiche toujours tous les avis, quel que soit votre sélection.
-   **Version du système d’exploitation**: la valeur par défaut de ce filtre est **Toutes**. Vous pouvez choisir une version de système d’exploitation spécifique si vous souhaitez que cette page affiche uniquement les avis laissés par les clients qui utilisent cette version.
-   **Version du package**: la valeur par défaut de ce filtre est **Toutes**. Si votre application comprend plusieurs packages, vous pouvez choisir un package spécifique ici si vous souhaitez visualiser uniquement les avis laissés par les clients qui disposaient de ce package lorsqu’ils ont évalué votre application.
-   **Réponses**: la valeur par défaut de ce filtre est **Toutes**. Vous pouvez choisir de ne visualiser que les avis pour lesquels vous avez [répondu aux clients](respond-to-customer-reviews.md), ou uniquement ceux auxquels vous n'avez pas encore répondu.
-   **Mises à jour** : la valeur par défaut de ce filtre est **Tous**. Vous pouvez choisir de ne visualiser que les avis qui ont été mis à jour par le client depuis que vous avez [répondu à ces avis](respond-to-customer-reviews.md), ou uniquement ceux qui n’ont pas encore été mis à jour par le client.
-   **Marché** : la valeur par défaut de ce filtre est **Tous les marchés**. Vous pouvez choisir un marché spécifique si vous ne souhaitez visualiser que les avis de clients appartenant à ce marché.
-   **Type d’appareil** : le filtre par défaut est **Tous les appareils**. Vous pouvez choisir un type d’appareil spécifique si vous souhaitez que cette page affiche uniquement les avis laissés par les clients utilisant celui-ci.
-   **Nom de catégorie**: la valeur par défaut de ce filtre est **Tous**. Vous pouvez choisir un spécifiques de [passer en revue la catégorie d’insight](#review-insight-categories) affiche uniquement les avis que nous avons associées à cette catégorie. 

> [!TIP]
> Si cette page ne contient aucun avis, assurez-vous que vos filtres n’ont pas exclu la totalité des avis concernant votre application. Par exemple, si vous filtrez les avis en fonction d’un système d’exploitation non pris en charge par votre application, aucun avis n’apparaîtra sur cette page.


## <a name="ratings-breakdown"></a>Répartition des classifications

Le graphique de **répartition des classifications** s’affiche en haut de ce rapport, afin que vous pouvez obtenir un aperçu des éléments suivants: 
- Évaluation moyenne à l’aide d’étoiles concernant l’application
- Nombre total d’évaluations de votre application au cours des 12derniers mois.
- Nombre total d’évaluations correspondant à chaque nombre d’étoiles.
- Nombre d’évaluations de chaque type (nouvelles ou révisées) par nombre d’étoiles au cours des 12derniers mois.
 - La section **Nouvelles évaluations** spécifie les évaluations qui ont été soumises par les clients et qui n’ont absolument pas été modifiées.
 - La section **Évaluations révisées** spécifie les évaluations qui ont été modifiées d’une façon quelconque par le client, même si cette modification porte uniquement sur le texte de l’avis.

> [!TIP]
> L’évaluation moyenne mise à disposition d’un client dans le WindowsStore tient compte du marché et du type d’appareil du client. Dès lors, elle peut différer du contenu présenté dans ce rapport.

Notez que ce graphique inclut toujours toutes vos avis, même si vous avez sélectionné **que les évaluations avec avis** dans le filtre de page **passer en revue le contenu** .

Vous pouvez également visualiser ce graphique dans le [rapport sur les évaluations](ratings-report.md), ainsi que de plus d’informations sur les évaluations de votre application.


<span id = "review-insight-categories" />

## <a name="insight-categories"></a>Catégories d’analyse des

Les groupes de graphiques **Insight categories** votre avis catégories que nous avons déterminé peuvent être associés à l’avis.

> [!NOTE]
> Lorsque vous visualisez les avis par catégorie, les avis datant de moins de 24heures et/ou formulés dans une langue autre que l’anglais ne sont pas inclus.

Dans la zone supérieure de la page, des blocs colorés représentent les avis par catégorie. Sélectionnez l’une de ces catégories pour visualiser uniquement les avis que nous lui avons associés. Vous pouvez également utiliser les [filtres de page](#apply-filters) pour effectuer un filtrage par catégorie.

Pour visualiser une répartition du nombre d’avis par catégorie, sélectionnez **Afficher les détails**. 


## <a name="reviews"></a>Avis

Chaque avis de client contient les éléments suivants:

-   Le titre et le texte de l'avis rédigés par le client. (Les avis rédigés par les clients qui utilisent Windows Phone 8.1 et les versions antérieures sont dépourvus de titre.)
-   La date de l'avis.
-   Le nom de réviseur telle qu’elle s’affiche dans le Microsoft Store.
-   Le pays ou la région de l'auteur de l'avis.
-   La version du package de l'application installée sur l'appareil du client au moment de la rédaction de l'avis. (Ces informations n’est pas disponible pour les avis soumis par les clients Windows8.1 ou une version antérieure).
-   La version du système d'exploitation installée sur l'appareil du client au moment de la rédaction de l'avis.
-   Le nom de l'appareil utilisé par le client au moment de la rédaction de l'avis. (Ces informations n’est pas disponible pour les avis soumis par les clients Windows8.1 ou une version antérieure).
-   La « note d'utilité » de l'avis signalant le nombre de fois où l'avis a été considéré comme utile par d'autres clients. Cette information est indiquée sous la forme de deux valeurs : la première spécifie le nombre de clients ayant jugé l'avis utile, la seconde correspond au nombre total de clients ayant laissé une évaluation. Par exemple, une note d'utilité de 4/10 signifie que sur 10 personnes, 4 ont trouvé l'avis utile, et 6 non. (Si aucun utilisateur n’a évalué l’utilité d’un avis, aucune note d’utilité ne s’affiche.)

Notez que les clients peuvent évaluer votre application sans formuler de commentaires. Le nombre d’avis sera donc généralement inférieur au nombre d’évaluations.

Vous pouvez trier les avis sur la page par date et/ou par évaluation, dans l’ordre croissant ou décroissant. Cliquez sur le lien de **Trier par** pour afficher les options de tri par **Date** et/ou **évaluation**.

Vous pouvez également utiliser la zone de recherche pour rechercher des mots spécifiques ou des expressions dans les avis de votre application. Notez qu’uniquement le texte d’origine avis rédigé par le client recherche est effectuée, même si l’avis a été écrit dans une autre langue. Texte d’avis traduites ne porte pas.

> [!NOTE]
> Les avis peuvent disparaître ponctuellement de ce rapport. Cela peut se produire lorsque Microsoft supprime des avis du WindowsStore qui ont été rédigés par des clients qui utilisent certaines versions préliminaires ou builds de Windows10 réservées aux Insiders. Nous procédons ainsi afin de réduire le risque de publication d’avis négatifs liés à un problème d’une version préliminaire de Windows. Nous pouvons également supprimer les avis du Windows Store qui ont été identifiés comme indésirables, inappropriés, offensants ou contraires à notre politique. Nous pensons que cette action contribuera à améliorer l’expérience utilisateur.


## <a name="translating-reviews"></a>Traduction des avis

Les commentaires qui n'ont pas été rédigés dans votre langue sont traduits par défaut. Si vous le souhaitez, vous pouvez désactiver la traduction des avis en décochant la case **Traduire les avis** située en haut à droite au-dessus de la liste des avis.

Notez que les évaluations sont traduites par un système de traduction automatique et que le résultat de la traduction n’est pas toujours précis. Le texte d’origine est fourni si vous souhaitez le comparer à la traduction ou utiliser un autre moyen de traduction.

Comme indiqué plus haut, lorsque recherche votre avis, seul le texte d’origine à gauche par le client recherche est effectuée (et non une le texte traduit), même si vous avez la case à cocher **traduire avis** activée.


## <a name="responding-to-customer-reviews"></a>Réponse aux avis des clients

Vous pouvez utiliser [L’espace partenaires](https://partner.microsoft.com/dashboard), l' [API d’avis du Microsoft Store](../monetize/submit-responses-to-app-reviews.md)ou l' [application Centre de développement](https://www.microsoft.com/store/apps/dev-center/9nblggh4r5ws) d’envoyer des réponses à la plupart des avis de vos clients. Pour plus d’informations, consultez l’article [Répondre aux avis des clients](respond-to-customer-reviews.md).

Vous découvrirez ci-après certaines actions supplémentaires à envisager en fonction des évaluations et des avis que vous voyez.

-   Si vous notez de nombreux avis suggérant une nouvelle fonctionnalité ou la modification d'une fonctionnalité existante, ou se plaignant d'un problème, envisagez de publier une nouvelle version répondant spécifiquement à ces commentaires. (Veillez alors à mettre à jour la [description](create-app-descriptions.md) de votre application pour indiquer que le problème a été résolu.)
-   Si l’évaluation moyenne est élevée, mais que le nombre de téléchargements est faible, vous pouvez rechercher des façons d’[élargir le public cible de votre application](attract-customers-and-promote-your-apps.md), puisque celle-ci a été bien accueillie par les utilisateurs qui l’ont essayée.


 

 

 
