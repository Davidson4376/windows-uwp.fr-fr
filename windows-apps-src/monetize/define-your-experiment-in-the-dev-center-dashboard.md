---
author: mcleanbyron
Description: Before you can run an experiment in your Universal Windows Platform (UWP) app with A/B testing, you must define your experiment in the Dev Center dashboard.
title: Définir votre expérience dans le tableau de bord
ms.assetid: 675F2ADE-0D4B-41EB-AA4E-56B9C8F32C41
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, MicrosoftStore Services SDK, tests A/B, expériences
ms.localizationpriority: medium
ms.openlocfilehash: f690aaede800f76b2eb006355e982ea6b3509b78
ms.sourcegitcommit: 1e5590dd10d606a910da6deb67b6a98f33235959
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2018
ms.locfileid: "3234734"
---
# <a name="define-your-experiment-in-the-dashboard"></a>Définir votre expérience dans le tableau de bord

Après avoir [créé un projet et défini des variables distantes dans le tableau de bord du Centre de développement](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md), puis [codé votre application pour l’expérimentation](code-your-experiment-in-your-app.md), vous êtes prêt à créer une expérience dans le projet. Quand vous créez l’expérience, vous définissez les objectifs et les variantes que vos utilisateurs reçoivent.

Pour découvrir une procédure pas à pas illustrant le processus de création et d’exécution d’une expérience de bout en bout, voir [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

<span id="get-an-api-key" />
<span id="create-an-experiment" />

## <a name="create-your-experiment"></a>Créer votre expérience

1. Connectez-vous au [tableau de bord du Centre de développement](https://dev.windows.com/overview).
2. Sous **Vos applications**, sélectionnez l’application pour laquelle vous voulez créer une expérience.
3. Dans le volet de navigation, sélectionnez **Services**, puis **Expérimentation**.
4. Dans la page **Expérimentation**, identifiez le projet dans lequel vous voulez ajouter une expérience dans la table de projets, puis cliquez sur le lien **Add experiment (Ajouter une expérience)** correspondant à ce projet.
5. Dans le champ **Nom d’expérience**, tapez un nom qui vous permet d’identifier facilement l’expérience. Une fois que vous avez créé une expérience, son nom apparaît dans la liste des expériences existantes dans la page **Expérimentation** de votre application et dans la page du projet.
6. Si vous voulez modifier l’expérience alors qu’elle est active, cochez la case **Editable experiment (Expérience modifiable)**. Cochez uniquement cette case si vous créez une expérience pour valider toutes les variantes par le biais de tests internes. Pour plus d’informations, voir [Créer une expérience pour des tests internes](define-your-experiment-in-the-dev-center-dashboard.md#test_experiments).
    > [!NOTE]
    > Ne cochez pas cette case si vous créez une expérience que vous publierez pour vos clients (autrement dit, une expérience associée à un ID de projet utilisé dans une version de votre application accessible aux clients). La modification d’une expérience alors qu’elle est active rend non valides ses résultats.

7. Dans le menu déroulant **Nom du projet**, le projet actuel est automatiquement sélectionné. Si vous voulez ajouter la nouvelle expérience à un autre projet, vous pouvez le sélectionner ici. Dans le cas contraire, ne touchez pas à cette sélection.
8.   Notez la valeur [ID de projet](run-app-experiments-with-a-b-testing.md#terms). Quand vous [codez votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md), vous devez indiquer cet ID dans votre code pour pouvoir recevoir des données de variation et signaler des événements d’affichage et de conversion au Centre de développement.
9. Dans la section **Événement d’affichage**, tapez le nom de l’[événement d’affichage](run-app-experiments-with-a-b-testing.md#terms) pour votre expérience dans le champ **Nom d’événement d’affichage**.
10. Dans la section **Objectifs et événements de conversion**, définissez au moins un objectif pour votre expérience:
  * Dans le champ **Nom d’objectif**, tapez un nom descriptif pour votre objectif. Après avoir exécuté une expérience, ce nom apparaît dans le résumé des résultats pour l’expérience.
  * Dans le champ **Nom de l’événement de conversion**, tapez le nom de l’[événement de conversion](run-app-experiments-with-a-b-testing.md#terms) pour cet objectif.
  * Dans le champ **Objectif**, sélectionnez **Augmenter** ou **Réduire** si vous souhaitez augmenter ou réduire le nombre d’occurrences de l’événement de conversion. Ces informations sont utilisées dans le résumé des résultats de l’expérience.

> [!NOTE]
> Le Centre de développement signale uniquement le premier événement de conversion pour chaque vue utilisateur sur une période de 24heures. Si un utilisateur déclenche plusieurs événements de conversion dans votre application au cours d’une période de 24heures, seul le premier événement de conversion est signalé. Ceci est conçu pour empêcher le fait qu’un utilisateur unique fausse les résultats de l’expérience pour un groupe représentatif d’utilisateurs alors que l’objectif est d’optimiser le nombre d’utilisateurs qui effectuent une conversion.

<span id="define-the-variations-and-settings-for-the-experiment" />

### <a name="define-the-remote-variables-and-variations-for-your-experiment"></a>Définir les variables distantes et les variantes de votre expérience

Ensuite, définissez les [variables](run-app-experiments-with-a-b-testing.md#terms) distantes et les [variantes](run-app-experiments-with-a-b-testing.md#terms) de votre expérience.

1. Dans la section **Remote variables and variations( Variables distantes et variantes)**, vous devez voir deux variantes par défaut, **VarianteA (contrôle)** et **VarianteB**. Si vous voulez plus de variantes, cliquez sur **Ajouter une variante**. Si vous le souhaitez, vous pouvez renommer chaque variation.
2. Par défaut, les variantes sont réparties en valeurs égales entre les utilisateurs de l’application. Si vous souhaitez définir un pourcentage de distribution spécifique, décochez la case **Répartir en valeurs égales** et tapez les pourcentages dans la ligne **Distribution (%)**.
3. Ajoutez les variables distantes à vos variantes. Dans le contrôle de liste déroulante situé au bas de cette section, choisissez chaque variable à ajouter et cliquez sur **Ajouter une variable**.
    > [!NOTE]
    > Les variables répertoriées dans ce contrôle sont héritées du projet de l’expérience. La valeur par défaut de la variable (telle qu’elle est définie dans le projet) est automatiquement affectée à la variante du contrôle. Si vous voulez créer des variables qui ne sont pas répertoriées ici, accédez à la page de projet associée et ajoutez-y les variables.

4. Modifiez les valeurs des variables pour chaque variante unique dans l’expérience (autrement dit, les variantes autres que les variantes du contrôle).

<span id="save-and-activate-your-experiment" />

### <a name="save-and-activate-your-experiment"></a>Enregistrer et activer votre expérience

Lorsque vous avez terminé la saisie des champs obligatoires pour votre expérience, cliquez sur **Enregistrer** pour enregistrer votre expérience.

Si vous êtes satisfait des paramètres de votre expérience et êtes prêt à l’activer afin de pouvoir commencer la collecte des données de l’expérience depuis votre application, cliquez sur **Activer**. Quand l’expérience est active, votre application peut récupérer les variables de variante et signaler les événements d’affichage et de conversion au Centre de développement. Pour plus d’informations, voir [Exécuter et gérer votre expérience dans le tableau de bord du Centre de développement](manage-your-experiment.md).

> [!IMPORTANT]
> Un projet peut uniquement comprendre une expérience active à la fois. Une fois que vous avez activé une expérience, vous ne pouvez plus en modifier les paramètres, sauf si vous avez coché la case **Editable experiment (Expérience modifiable)** quand vous avez créé l’expérience. Nous vous recommandons de coder l’expérience dans votre application avant de l’activer.

<span id="test_experiments"/>

## <a name="create-an-experiment-for-internal-testing"></a>Créer une expérience pour des tests internes

Vous pouvez tester votre expérience auprès d’un public contrôlé (par exemple, un ensemble de testeurs internes) pour vérifier que toutes les variantes fonctionnent comme prévu avant d’activer l’expérience pour vos clients. Pour ce faire, créez une expérience avec l’option **Editable experiment (Expérience modifiable)** sélectionnée.

Pour tester votre expérience avant de la publier pour les clients, suivez ce processus:

1. Créez deux projets: un pour la build publique de votre application et un pour une build privée de votre application disponible uniquement pour vos testeurs. Les instructions suivantes font référence à ces projets en les désignant par projet public et projet test, respectivement.
2. Quand vous [codez votre application pour l’expérimentation](code-your-experiment-in-your-app.md), faites référence à l’ID du projet public dans la build public de votre application. Dans la build privée de votre application, faites référence à l’ID du projet test.
3. Créez une expérience dans le projet test, puis sélectionnez l’option **Editable experiment (Expérience modifiable)** pour l’expérience.
4. Activez l’expérience dans le projet test. Allouez une distribution de 100% à une seule variante et vérifiez que cette variante fonctionne comme prévu pour les testeurs. Répétez le processus pour d’autres variantes.
5. Une fois que vous avez vérifié que les variantes fonctionnent comme prévu, apportez les modifications finales à l’expérience dans le projet test. Quand vous êtes prêt à publier l’expérience pour vos clients, clonez-la dans le projet public. Dans cette expérience, ne sélectionnez pas l’option **Editable experiment (Expérience modifiable)**.
4. Assurez-vous que la distribution de la variante cible est correcte dans l’expérience clonée.
5. Activez l’expérience clonée pour la publier pour vos clients.

## <a name="next-steps"></a>Étapes suivantes

Après avoir défini votre expérience dans le tableau de bord du Centre de développement et codé l’expérience dans votre application, vous êtes prêt à [exécuter et à gérer votre expérience dans le tableau de bord du Centre de développement](manage-your-experiment.md).

## <a name="related-topics"></a>Rubriques connexes

* [Créer un projet et définir des variables distantes dans le tableau de bord du Centre de développement](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Coder votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md)
* [Gérer votre expérience dans le tableau de bord du Centre de développement](manage-your-experiment.md)
* [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Exécuter des expériences d’application avec des tests A/B](run-app-experiments-with-a-b-testing.md)
