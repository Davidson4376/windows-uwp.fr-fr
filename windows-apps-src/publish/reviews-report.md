---
author: jnHs
Description: "Le rapport Avis disponible dans le tableau de bord du Centre de développement Windows vous permet de consulter les commentaires que les clients ont formulés pendant l’évaluation de votre application dans le Windows Store."
title: Rapport Avis
ms.assetid: E50C3A4D-1D8A-4E5B-8182-3FAD049F2A2D
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 7d1a768ce558718b43a4d124f7c88868e999fb93

---

# Rapport Avis


Le rapport **Avis** disponible dans le tableau de bord du Centre de développement Windows vous permet de consulter les commentaires que les clients ont formulés pendant l'évaluation de votre application dans le Windows Store. Vous pouvez afficher ces données dans votre tableau de bord ou [télécharger le rapport](download-analytic-reports.md) et le consulter hors connexion. Vous pouvez également récupérer ces données par programme à l’aide de l’[API REST d’analyse du Windows Store](../monetize/access-analytics-data-using-windows-store-services.md).

> **Remarque** Vous pouvez également [répondre aux avis de clients](respond-to-customer-reviews.md) à partir de cette page.

Ce rapport affiche le nombre d’étoiles qu’un client a attribué à votre application en rédigeant un avis, mais n’analyse pas l’ensemble des statistiques d’évaluation de votre application. Pour consulter ces statistiques, consultez l’article [Rapport sur les évaluations](ratings-report.md).

Notez que les clients peuvent évaluer votre application sans formuler de commentaires. Le nombre d'avis sera donc généralement inférieur au nombre d'évaluations. Par défaut, cette page affiche également les évaluations qui ne comportent aucun commentaire. Toutefois, si vous préférez ne visualiser que les évaluations qui intègrent des commentaires, vous pouvez utiliser l'option **Filtres de page**, comme décrit ci-après.

Chaque avis de client contient les éléments suivants :

-   Le titre et le texte de l'avis rédigés par le client. (Les avis rédigés par les clients qui utilisent Windows Phone 8.1 et les versions antérieures sont dépourvus de titre.)
-   La date de l'avis.
-   Le nom de l'auteur de l'avis tel qu'il apparaît dans le Windows Store.
-   Le pays ou la région de l'auteur de l'avis.
-   La version du package de l'application installée sur l'appareil du client au moment de la rédaction de l'avis. (Cette information n'est pas disponible pour les avis soumis par les clients qui utilisent Windows 8.1 ou une version antérieure.)
-   La version du système d'exploitation installée sur l'appareil du client au moment de la rédaction de l'avis.
-   Le nom de l'appareil utilisé par le client au moment de la rédaction de l'avis. (Cette information n'est pas disponible pour les avis soumis par les clients qui utilisent Windows 8.1 ou une version antérieure.)
-   La « note d'utilité » de l'avis signalant le nombre de fois où l'avis a été considéré comme utile par d'autres clients. Cette information est indiquée sous la forme de deux valeurs : la première spécifie le nombre de clients ayant jugé l'avis utile, la seconde correspond au nombre total de clients ayant laissé une évaluation. Par exemple, une note d'utilité de 4/10 signifie que sur 10 personnes, 4 ont trouvé l'avis utile, et 6 non. (Si aucun utilisateur n'a évalué l'utilité d'un avis, aucune note d'utilité ne s'affiche.)

## Appliquer les filtres


Dans la zone supérieure de la page, vous pouvez développer l’option **Appliquer les filtres** pour filtrer toutes les données de cette page.

>**Conseil** Si cette page ne contient aucun avis, assurez-vous que vos filtres n’ont pas exclu la totalité des avis concernant votre application. Par exemple, si vous filtrez les avis en fonction d’un système d’exploitation non pris en charge par votre application, aucun avis n’apparaîtra sur cette page.

-   **Évaluation** : les évaluations Toutes les étoiles sont contrôlées par défaut, mais vous pouvez choisir une ou plusieurs évaluations spécifiques (entre 1 et 5 étoiles) si vous voulez ne visualiser que les avis associés à une évaluation donnée.
-   **Date** : la valeur par défaut de ce filtre est **30 derniers jours**, mais vous pouvez étendre cette période aux **12 derniers mois**.
-   **Contenu d’avis** : par défaut, ce filtre est défini sur la valeur **Tous**, qui demande d’inclure les évaluations ne comportant aucun avis. Vous pouvez sélectionner l'option **Évaluations avec avis** pour ne visualiser que les évaluations qui comportent un avis.
-   **Système d’exploitation cible** : la valeur par défaut de ce filtre est **Tous**. Vous pouvez choisir un système d'exploitation ciblé spécifique pour ne visualiser que les évaluations émanant de clients qui utilisent votre ou vos package(s) sur ce système d'exploitation.
-   **Réponses** : la valeur par défaut de ce filtre est **Tous**. Vous pouvez choisir de ne visualiser que les avis pour lesquels vous avez [répondu aux clients](respond-to-customer-reviews.md), ou uniquement ceux auxquels vous n'avez pas encore répondu.
-   **Mises à jour** : la valeur par défaut de ce filtre est **Tous**. Vous pouvez choisir de ne visualiser que les avis qui ont été mis à jour par le client depuis que vous avez [répondu à ces avis](respond-to-customer-reviews.md), ou uniquement ceux qui n'ont pas encore été mis à jour par le client.
-   **Marché** : la valeur par défaut de ce filtre est **Tous les marchés**. Vous pouvez choisir un marché spécifique si vous ne souhaitez visualiser que les avis de clients appartenant à ce marché.
-   **Type d’appareil** : le filtre par défaut est **Tous les appareils**. Vous pouvez choisir un type d’appareil spécifique si vous souhaitez que cette page affiche uniquement les avis laissés par les clients utilisant celui-ci.
-   **Version du package** : le filtre par défaut est **Tous les packages**. Vous pouvez choisir un package spécifique si vous souhaitez que cette page affiche uniquement les avis laissés par les clients possédant ce package lorsqu’ils on évalué l’application.

Les informations figurant dans tous les graphiques répertoriés ci-après refléteront la période sélectionnée dans la section **Appliquer des filtres**, ainsi que d’autres filtres que vous avez choisis ici.

> **Remarque** L’évaluation moyenne mise à disposition des clients du Windows Store tient compte du marché et du type d’appareil du client, ainsi que de l’évaluation de l’année passée. Dès lors, elle peut différer du contenu affiché dans le rapport. Pour afficher un aperçu de l’évaluation moyenne associée à un client donné dans le Windows Store, vous devez appliquer des filtres afin de sélectionner un marché et un type d’appareil spécifiques et définir la **Date** sur **12 derniers mois**.

## Traduction des avis


Les commentaires qui n'ont pas été rédigés dans votre langue sont traduits par défaut. Si vous le souhaitez, vous pouvez désactiver la traduction des avis en décochant la case **Traduire les avis** située en haut à droite au-dessus de la liste des avis.

Notez que les évaluations sont traduites par un système de traduction automatique et que le résultat de la traduction n’est pas toujours précis. Le texte d'origine est fourni si vous souhaitez comparer la traduction ou utiliser un autre moyen de traduction.

## Tri des avis


Vous pouvez trier les avis sur la page par date et/ou par évaluation, dans l'ordre croissant ou décroissant. Cliquez sur le lien **Trier par** pour afficher les options de tri par date et/ou évaluation. Lorsque vous cliquez sur une case d’option dans la section Date ou Évaluation, les critères de tri sont appliqués et l’étiquette de tri s’affiche en regard de la case **Trier par**. Vous pouvez supprimer les critères de tri en cliquant sur l’icône **X** qui apparaît sur chaque étiquette.

## Réponse aux avis des clients


Le tableau de bord du Centre de développement Windows Store vous permet de répondre à la plupart des avis rédigés par vos clients. Pour plus d'informations, voir l'article [Répondre aux avis des clients](respond-to-customer-reviews.md).

Vous découvrirez ci-après certaines actions supplémentaires à envisager en fonction des évaluations et des avis que vous voyez.

-   Si vous notez de nombreux avis suggérant une nouvelle fonctionnalité ou la modification d'une fonctionnalité existante, ou se plaignant d'un problème, envisagez de publier une nouvelle version répondant spécifiquement à ces commentaires. (Veillez alors à mettre à jour la [description](create-app-descriptions.md) de votre application pour indiquer que le problème a été résolu.)
-   Si l’évaluation moyenne est élevée, mais que le nombre de téléchargements est faible, vous pouvez rechercher des façons d’[élargir le public cible de votre application](app-promotion-and-customer-engagement.md), puisque celle-ci a été bien accueillie par les utilisateurs qui l’ont essayée.

> **Remarque** Vous constaterez probablement une différence dans le nombre d’avis si vous comparez le rapport **Évaluations** dans le Centre de développement Windows avec le rapport Avis dans l’application mobile de l’ancien Centre de développement. Cela s’explique par le fait que l’application affiche uniquement les avis laissés par les clients utilisant Windows Phone 8.1 et antérieur. Ce peut être également la conséquence du travail effectué par Microsoft pour supprimer les avis du Windows Store, qui ont été identifiés comme indésirables, inappropriés, offensants ou comme violant la politique. Nous pensons que cette action va améliorer l’expérience utilisateur.

 

 

 



<!--HONumber=Jun16_HO4-->


