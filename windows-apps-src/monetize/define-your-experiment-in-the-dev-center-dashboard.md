---
Description: Pour exécuter une expérience sur votre application de plateforme Windows universelle (UWP) avec des tests A/B, vous devez définir votre expérience dans le tableau de bord du Centre de développement.
title: Définissez votre expérience dans le tableau de bord du Centre de développement
ms.assetid: 675F2ADE-0D4B-41EB-AA4E-56B9C8F32C41
---

# Définissez votre expérience dans le tableau de bord du Centre de développement

Pour exécuter une expérience sur votre application de plateforme Windows universelle (UWP) avec des tests A/B, définissez votre expérience dans le tableau de bord du Centre de développement.

Les sections suivantes décrivent le processus général de définition d’une expérience dans le tableau de bord. Pour une procédure pas à pas illustrant le processus de création et d’exécution d’une expérience de bout en bout, voir [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## Obtenir une clé API

Pour commencer, accédez à la page **Expérimentation** du tableau de bord du Centre de développement et obtenez une *clé API* pour votre expérience.

Une clé API est un ID unique qui associe votre application à une expérience de votre compte du Centre de développement. Chaque expérience est associée à une clé API. Toutefois, une application peut avoir plusieurs clés API, et chaque clé API peut être associée à plusieurs expériences. Vous pouvez utiliser des clés API pour distinguer les différents ensembles d’expériences. Par exemple, vous pouvez publier un ensemble d’expériences à l’intention des testeurs de votre organisation, et un autre ensemble uniquement à l’intention des utilisateurs externes de votre application.

Vous devez utiliser cette clé API pour vous connecter au service des tests A/B dans le code de votre application. Pour en savoir plus, voir [Coder votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md).

1. Connectez-vous au [tableau de bord du Centre de développement](https://dev.windows.com/overview).
2. Sous **Vos applications**, sélectionnez l’application pour laquelle vous voulez créer une expérience.
3. Dans le volet de navigation, sélectionnez **Services** puis **Expérimentation**.
4. La section **clés API** fournit une clé API par défaut pour votre application nommée **clé API 1**. Si vous voulez utiliser cette clé, saisissez un nom convivial pour cette clé (facultatif) et copiez-le dans votre code d’application. Pour générer une nouvelle clé API, sélectionnez **Nouvelle clé API** et entrez un nom convivial pour celle-ci.

## Créer une expérience

Ensuite, créez une expérience et définissez les objectifs de cette dernière.

1. Dans la section **Expériences**, cliquez sur le bouton **Nouvelle expérience**.
2. Dans la section **Nom de clé API**, sélectionnez la clé API que vous voulez associer à cette expérience. Si vous n’avez qu’une clé d’API, celle-ci sera sélectionnée par défaut.
3. Dans le champ **Nom d’expérience**, tapez un nom qui vous permet d’identifier facilement l’expérience. Une fois que vous avez créé une expérience, ce nom apparaît dans la liste des expériences brouillonnes, actives et terminées sur la page **Expérimentation**.
4. Si vous souhaitez créer une expérience de test, cochez la case **Expérience de test**. La différence entre les expériences de test et les expériences normales réside dans le fait qu’une expérience de test peut être modifiée après avoir été activée.

  Les expériences de test sont faites pour vous aider à tester toutes les variantes sur un appareil client avant la publication de votre expérience aux clients. Pour vous assurer qu’une variante est correctement publiée aux clients, vous pouvez activer une expérience de test avec une distribution à 100 % allouée à une seule variante et une distribution à 0 % allouée à d’autres variantes. Après avoir vérifié cette variante, vous pouvez répéter ce processus pour d’autres variantes.
  > **Remarque** cochez uniquement cette case si vous créez une expérience de test pour valider des paramètres par le biais de tests internes. Ne cochez pas cette case si vous créez une expérience qui sera publiée aux clients.

5. Dans le champ **Nom d’événement d’affichage**, tapez le nom de l’*événement d’affichage* pour votre expérience. L’événement d’affichage est une chaîne arbitraire qui représente une activité lorsque l’utilisateur affiche une variante faisant partie de votre expérience. Le code de votre application enverra cette chaîne d’événement d’affichage au Centre de développement lorsque l’utilisateur affiche une variante. Pour en savoir plus, voir [Coder votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md).
6. Dans la section **Objectifs et événements de conversion**, définissez au moins un objectif pour votre expérience :
  * Dans le champ **Nom d’objectif**, tapez un nom descriptif pour votre objectif. Après avoir exécuté une expérience, ce nom apparaît dans le résumé des résultats pour l’expérience.
  * Dans le champ **Nom de l’événement de conversion**, tapez le nom de l’*événement de conversion* pour cet objectif. Un événement de conversion est une chaîne arbitraire qui représente un objectif pour cet objectif. Le code de votre application enverra cette chaîne d’événement de conversion au Centre de développement lorsque l’utilisateur atteint un objectif. Pour en savoir plus, voir [Coder votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md).
  * Dans le champ **Objectif**, sélectionnez **Augmenter** ou **Réduire** si vous souhaitez augmenter ou réduire le nombre d’occurrences de l’événement de conversion. Ces informations sont utilisées dans le résumé des résultats de l’expérience.

  >**Remarque** Le Centre de développement signale uniquement le premier événement de conversion pour chaque vue utilisateur sur une période de 24 heures. Si un utilisateur déclenche plusieurs événements de conversion dans votre application sur une période de 24 heures, seul le premier événement de conversion est signalé. Ceci est conçu pour empêcher le fait qu’un utilisateur unique fausse les résultats de l’expérience pour un groupe représentatif d’utilisateurs alors que l’objectif est d’optimiser le nombre d’utilisateurs qui effectuent une conversion.

## Définir les variantes et les paramètres pour l’expérience

Définissez ensuite les variantes et les paramètres pour l’expérience

* Une *variante* est une collection d’un ou plusieurs paramètres que vous testez dans votre expérience. Chaque expérience doit contenir au moins un paramètre et deux variantes (y compris le contrôle). Une expérience peut avoir jusqu’à cinq variantes.
* Un *paramètre* est une valeur que votre application utilise pour initialiser une variable de programme. Au cours de l’expérience, la valeur du paramètre change d’une variante à une autre. Une fois l’expérience terminée, le paramètre se voit affecter la même valeur que la variante que vous choisissez de publier à tous les utilisateurs de votre application. Les paramètres peuvent avoir les types suivants : string, Boolean, double, et integer.

Pour définir les variantes et les paramètres pour l’expérience :
1. Dans la section **Variantes et paramètres**, vous devez voir deux variantes par défaut, **Variante A (contrôle)** et **Variante B**. Si vous voulez davantage de variantes, cliquez sur **Ajouter une variante**. Si vous le souhaitez, vous pouvez renommer chaque variation.
2. Ensuite, créez les paramètres de vos variantes. Cliquez sur **Ajouter un paramètre** pour créer chaque nouveau paramètre, puis tapez le nom du paramètre et la valeur du paramètre dans chaque variante.
3. Par défaut, les variantes sont réparties en valeurs égales entre les utilisateurs de l’application. Si vous souhaitez définir un pourcentage de distribution spécifique, décochez la case **Répartir en valeurs égales** et tapez les pourcentages dans la ligne **Distribution (%)**.

## Enregistrez votre expérience

Lorsque vous avez terminé la saisie des champs obligatoires pour votre expérience, cliquez sur **Enregistrer** pour enregistrer votre expérience.

> **Important** Lorsque vous enregistrez une expérience, vous ne pouvez plus modifier la clé API pour cette dernière, même si vous n’avez pas encore activé l’expérience.

Si vous êtes satisfait des paramètres de votre expérience et êtes prêt à l’activer afin de pouvoir commencer la collecte des données de l’expérience depuis votre application, cliquez sur **Activer**. Lorsque l’expérience est active, votre application peut récupérer les paramètres des variantes et signaler les événements d’affichage et de conversion au Centre de développement.

> **Important** Lorsque vous activez une expérience, vous ne pouvez plus modifier les paramètres de l’expérience, sauf s’il s’agit d’une expérience de test (vous avez coché la case **Expérience de test** lorsque vous avez créé l’expérience). Nous vous recommandons de coder l’expérience dans votre application avant d’activer votre expérience.

## Étapes suivantes

Après avoir défini votre expérience dans le tableau de bord du Centre de développement, vous êtes prêt pour les étapes suivantes :
1. [Coder votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md). Utilisez une API dans le Kit de développement logiciel (SDK) d’engagement et de monétisation pour obtenir les paramètres de variante pour l’expérimentation, utiliser ces données pour modifier le comportement de la fonctionnalité que vous testez et envoyer des événements d’affichage et de conversion au Centre de développement.
2. [Exécutez et gérez votre expérience dans le tableau de bord du Centre de développement](manage-your-experiment.md) Utilisez le tableau de bord pour examiner les résultats de l’expérience et pour la terminer.

## Rubriques connexes

  * [Coder votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md)
  * [Gérer votre expérience dans le tableau de bord du Centre de développement](manage-your-experiment.md)
  * [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
  * [Exécuter des expériences d’application avec des tests A/B](run-app-experiments-with-a-b-testing.md)


<!--HONumber=Mar16_HO5-->


