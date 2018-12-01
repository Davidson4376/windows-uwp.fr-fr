---
Description: Learn how to create customer segments so you can target a subset of your customer base for promotional or engagement purposes.
title: Créer des segments de clients
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, segment, segments, groupe ciblé, clients
ms.assetid: 58185f6c-d61f-478b-ab24-753d8986cd5a
ms.localizationpriority: medium
ms.openlocfilehash: d0df23f0da4efe01877c45e5b2b6b5f4e2142a92
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8335001"
---
# <a name="create-customer-segments"></a>Créer des segments de clients

Parfois, vous avez besoin de cibler un sous-ensemble de votre clientèle à des fins de promotion et d’engagement. Vous pouvez accomplir cela dans [L’espace partenaires](https://partner.microsoft.com/dashboard) en créant un type de [groupe de clients](create-customer-groups.md) appelé un *segment* qui inclut les clients Windows 10 qui remplissent les données démographiques ou revenus que vous choisissez.

Par exemple, vous pouvez créer un segment incluant uniquement les clients âgés de 50ans ou plus, ou incluant les clients ayant dépensé plus de 10$ dans le Microsoft Store. Vous pouvez également combiner ces critères et créer un segment qui inclut tous les clients de plus de 50ans qui ont dépensé plus de 10$ dans le Windows Store. 

Nous fournissons quelques modèles de segment pour démarrer, mais vous pouvez définir et combiner les critères comme bon vous semble.

> [!TIP]
> Les segments permettent d’envoyer des [notifications ciblées](send-push-notifications-to-your-apps-customers.md) ou des [offres ciblées](use-targeted-offers-to-maximize-engagement-and-conversions.md) à un groupe de clients dans le cadre de vos campagnes visant à susciter l’intérêt.

Éléments à garder à l’esprit concernant les segments de clients:
- Après avoir enregistré un segment, vous devez patienter 24heures avant de pouvoir l’utiliser pour les [notifications push ciblées](send-push-notifications-to-your-apps-customers.md).
- Les résultats du segment sont actualisés quotidiennement. Vous pouvez donc voir le nombre total de clients d’un segment changer d’un jour sur l’autre selon que les clients satisfont ou non les critères du segment.
- La plupart des attributs de segment sont calculés à l’aide de toutes les données historiques, même s’il existe des exceptions. Par exemple, les champs **Date d’acquisition de l’application**, **ID de campagne**, **Stocker la date d’affichage de la page** et **Domaine URI référent** sont limités aux 90derniers jours de données.
- Les segments comprennent uniquement des clients qui ont acquis votre application sur Windows10 en étant connectés avec un compte Microsoft valide. 
- Les segments ne comprennent pas les clients de moins de 17ans.

## <a name="to-create-a-customer-segment"></a>Pour créer un segment de clients

1.  Dans [L’espace partenaires](https://partner.microsoft.com/dashboard), développez **engager** dans le menu de navigation de gauche, puis sélectionnez **les groupes de clients**.
2.  Sur la page **Groupes de clients**, effectuez l’une des opérations suivantes:
 - Dans la section **Mes groupes de clients**, sélectionnez **Créer un groupe** pour définir un segment à partir de zéro. Sur la page suivante, sélectionnez la case d’option **Segment**.
 - Dans la section **Modèles de segment**, sélectionnez **Copier** en regard de l’un des segments prédéfinis (que vous pouvez utiliser en l’état ou modifier selon vos besoins).
3.  Dans la zone **Nom du groupe**, entrez un nom pour votre segment.
4.  Dans la liste **Inclure des clients à partir de cette application**, sélectionnez l’une de vos applications à cibler.
5.  Dans la section **Définir des conditions d’inclusion**, spécifiez les critères de filtre du segment.

    Vous pouvez choisir parmi plusieurs critères de filtre, y compris les **Acquisitions**, **source d’Acquisition**, **Démographie**, **Rating**, **prévision de l’évolution**, **achats du Windows Store**, **acquisitions de Windows Store**et **du Store dépenser**.

    Par exemple, si vous voulez créer un segment ne comprenant que les clients de votre application âgés de18à24ans, vous devez sélectionner les critères de filtre [**Démographie**] [**Groupe d’âges**] [**est**] [**18 à 24**] dans les listes déroulantes.

    Vous pouvez générer des segments plus complexes à l’aide des requêtes AND/OR pour inclure et exclure des clients en fonction de différents attributs. Pour ajouter une requête OR, sélectionnez **+ instruction OR**. Pour ajouter une requête ADD, sélectionnez **Ajouter un autre filtre**.

    Par conséquent, si vous souhaitez affiner ce segment pour inclure seulement des clients masculins se trouvant dans la tranche d’âge spécifiée, sélectionnez **Ajouter un autre filtre**, puis sélectionnez les critères de filtre supplémentaires [**Démographie**] [**Sexe**] [**est**] [**Masculin**]. Dans cet exemple, **Définition du segment** affiche **Groupe d’âge == 18 à 24 &amp;&amp; Sexe == Masculin**.

    ![Exemple de critères de filtre pour un segment](images/create-segment-inclusions.png)
6. Sélectionnez **Enregistrer**.

> [!IMPORTANT]
> Vous ne pouvez pas utiliser un segment comprenant un trop petit nombre de clients. Si votre définition de segment n’inclut pas suffisamment de clients, vous pouvez ajuster les critères du segment ou réessayer plus tard, si votre application a acquis plus de clients répondant à vos critères de segment.


## <a name="app-statistics"></a>Statistiques de l’application

La section **Statistiques de l’application** du segment fournit des informations sur votre application, ainsi que la taille du segment que vous venez de créer.

Notez que le champ **Clients de l’application disponibles** ne reflète pas le nombre réel de clients qui ont acquis votre application, mais uniquement le nombre de clients disponibles pouvant être inclus dans les segments (c’est-à-dire, les clients dont nous pouvons déterminer qu’ils satisfont aux conditions d’âge, ont acquis votre application sur Windows10 et qui sont associés à un compte Microsoft valide).

Si, dans les résultats, le champ **Clients dans ce segment** indique **Peu**, le segment n’inclut pas suffisamment de clients et est marqué comme inactif. Vous ne pouvez pas utiliser les segments inactifs pour les notifications ou d’autres fonctionnalités. Vous pouvez activer et utiliser un segment en effectuant l’une des opérations suivantes:

- Dans la section **Définir des conditions d’inclusion**, ajustez les critères de filtre pour que le segment inclut davantage de clients.
- Sur la page **Groupes de clients**, dans la section **Segments inactifs**, sélectionnez **Actualiser** pour voir si le segment contient à présent suffisamment de clients (par exemple, si les clients satisfaisant à vos critères de segment qui téléchargent votre application sont plus nombreux depuis que vous avez créé le segment pour la première fois, ou si un plus grand nombre de clients existants répondent désormais à vos critères de segment).
