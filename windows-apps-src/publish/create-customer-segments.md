---
author: shawjohn
Description: "Découvrez comment créer des segments de clients afin de cibler un sous-ensemble de votre clientèle à des fins de promotion ou d’engagement."
title: "Créer des segments de clients"
translationtype: Human Translation
ms.sourcegitcommit: c1b97d7ca331bdea101f16d3d59879db30173a0a
ms.openlocfilehash: 6448c366784344b649084a3aa746a2fc8f21d31d

---

# Créer des segments de clients

Parfois, vous avez besoin de cibler un sous-ensemble de votre clientèle à des fins de promotion et d’engagement. Vous pouvez accomplir cela dans le Centre de développement Windows en créant un type de [groupe de clients](create-customer-groups.md) appelé *segment* qui inclut les clients Windows10 correspondant aux critères démographiques ou de revenus de votre choix.

Par exemple, vous pouvez créer un segment incluant uniquement les clients âgés de 50ans ou plus, ou incluant les clients ayant dépensé plus de 10$ dans le Windows Store. Vous pouvez également combiner ces critères et créer un segment qui inclut tous les clients de plus de 50ans qui ont dépensé plus de 10$ dans le Windows Store. Nous fournissons quelques modèles de segment pour démarrer, mais vous pouvez définir et combiner les critères comme bon vous semble.

> **Conseil** Vous pouvez utiliser les segments pour [envoyer des notifications push ciblées](send-push-notifications-to-your-apps-customers.md) à un groupe de clients dans le cadre d’une campagne visant à susciter l’intérêt.

## Pour créer un segment de clients

1.  Dans le menu supérieur du [tableau de bord du Centre de développement Windows](https://developer.microsoft.com/dashboard/overview), sélectionnez **Clients**.
2.  Sur la page **Groupes de clients**, effectuez l’une des opérations suivantes:
 - Dans la section **Mes groupes de clients**, sélectionnez **Créer un groupe** pour définir entièrement un segment. Assurez-vous que **Segment** est sélectionné dans la liste déroulante **Type de groupe**.
 - Dans la section **Modèles de segment**, sélectionnez **Copier pour utiliser un segment prédéfini** que vous pouvez utiliser en l’état ou modifier selon vos besoins.
3.  Dans la liste **Inclure des clients à partir de cette application**, sélectionnez l’une de vos applications à cibler.
4.  Dans la zone **Nom du segment**, choisissez un nom pour votre segment.
5.  Dans la section **Définir des conditions d’inclusion**, choisissez les critères de filtre du segment.

    Par exemple, si vous voulez créer un segment qui inclut seulement les clients de votre application âgés de18à24ans, vous devez sélectionner les critères de filtre [**Démographie**] [**Groupe d’âge**] [**est**] [**18 à 24**] dans les listes déroulantes.

    Vous pouvez générer des segments plus complexes à l’aide des requêtes AND/OR pour inclure et exclure des clients en fonction de différents attributs. Pour ajouter une requête OR, sélectionnez **+ instruction OR**. Pour ajouter une requête ADD, sélectionnez **Ajouter un autre filtre**.

    Par conséquent, si vous souhaitez affiner ce segment pour inclure seulement des clients masculins se trouvant dans la tranche d’âge spécifiée, sélectionnez **Ajouter un autre filtre**, puis sélectionnez les critères de filtre supplémentaires [**Démographie**] [**Sexe**] [**est**] [**Masculin**]. Dans cet exemple, **Définition du segment** affiche **Groupe d’âge == 18 à 24 &amp;&amp; Sexe == Masculin**.

    ![Exemple de critères de filtre pour un segment](images/create-segment-inclusions.png)
6. Sélectionnez **Enregistrer**.

> **Important** Vous ne pouvez pas utiliser un segment comprenant un trop petit nombre de clients. Si votre définition de segment n’inclut pas suffisamment de clients, vous pouvez ajuster les critères du segment ou réessayer plus tard, si votre application a acquis plus de clients répondant à vos critères de segment.

Éléments à se rappeler concernant les segments de clients:
- Après avoir enregistré un segment, vous devez patienter 24heures avant de pouvoir l’utiliser pour les [notifications push ciblées](send-push-notifications-to-your-apps-customers.md).
- Les résultats du segment sont actualisés quotidiennement. Vous pouvez donc voir le nombre total de clients d’un segment changer d’un jour sur l’autre selon que les clients satisfont ou non les critères du segment.
- La plupart de ces attributs sont calculés à l’aide de toutes les données historiques, même s’il existe des exceptions. Par exemple, les champs **Date d’acquisition de l’application**, **ID de campagne**, **Stocker la date d’affichage de la page** et **Domaine URI référent** sont limités aux 90derniers jours de données.
- Le segment inclut seulement les clients qui ont acquis votre application sur Windows10. Si votre application prend en charge les anciennes versions du système d’exploitation, les clients utilisant ces anciennes versions du système d’exploitation ne sont pas inclus dans les segments que vous créez.
- Les clients excluent automatiquement tous les clients de moins de 17ans.


## Statistiques de l’application

La section **Statistiques de l’application** du segment fournit des informations sur votre application, ainsi que la taille du segment que vous venez de créer.

Notez que le champ **Clients de l’application disponibles** ne reflète pas le nombre réel de clients qui ont acquis votre application, mais uniquement le nombre de clients disponibles pouvant être inclus dans les segments (c’est-à-dire, les clients dont nous pouvons déterminer qu’ils satisfont aux conditions d’âge, ont acquis votre application sur Windows10 et qui sont associés à un compte Microsoft valide).

Si das les résultats, le champ **Clients dans ce segment** indique **Peu**, le segment n’inclut pas suffisamment de clients et est marqué comme inactif. Vous ne pouvez pas utiliser les segments inactifs pour les notifications ou d’autres fonctionnalités. Vous pouvez activer et utiliser un segment en effectuant l’une des opérations suivantes:

- Dans la section **Définir des conditions d’inclusion**, ajustez les critères de filtre pour que le segment inclut davantage de clients.
- Sur la page **Groupes de clients**, dans la section **Segments inactifs**, sélectionnez **Actualiser** pour voir si le segment contient actuellement suffisamment clients. Cette méthode peut fonctionner, par exemple, si les clients satisfaisant vos critères de segment qui téléchargent votre application sont plus nombreux depuis que vous avez créé le segment pour la première fois.



<!--HONumber=Nov16_HO1-->


